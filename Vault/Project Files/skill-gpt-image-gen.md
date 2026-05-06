# skill-gpt-image-gen

## מה הסקיל עושה
מעטפת טכנית טהורה ל-OpenAI Images API. מקבל `prompt` + `output_path` (חובה), `size`/`quality`/`model` (אופציונלי), ומחזיר קובץ PNG על דיסק. **לא** עושה decisions יצירתיים — זה תפקיד של [[agent-sami-image-gen]] (או של ה-caller). הסקיל מטפל ב-3 דברים בלבד: בניית הבקשה, decode של `b64_json` ל-bytes, וידוא שהקובץ לא ריק.

## למי הוא משויך
- **קורא ראשי:** [[agent-sami-image-gen]] — הסוכן הקנוני שמשתמש בסקיל.
- **קוראים אפשריים:** סוכנים אחרים שצריכים תמונה אד-הוק בלי curation. לא מומלץ — תיאלץ להמציא את ה-prompt בעצמך.
- **תלות סביבה:** `OPENAI_API_KEY` ב-`.env` (קיים), `python3` (תמיד), `jq` (אופציונלי).

## מיקום
`/.claude/skills/gpt-image-gen/SKILL.md`

## מבנה הקובץ
- Frontmatter: `name: gpt-image-gen`, description.
- 5 סעיפים: When to invoke → Inputs → How to call (5 צעדים) → Output contract → Notes.
- ה-**5 צעדים** של How to call:
  1. ודא `OPENAI_API_KEY` קיים (אחרת `set -a; source .env; set +a`).
  2. POST ל-`https://api.openai.com/v1/images/generations`. ה-prompt עובר JSON-escape דרך python כדי לעמוד בפני quotes/newlines/unicode.
  3. Decode `b64_json` — `jq` ראשון, python fallback (תמיד עובד גם ב-Git Bash).
  4. `test -s "$OUTPUT"` — gate חובה לפני דיווח success.
  5. ניקוי `$RESPONSE_FILE` הזמני.

## מודל
- Default ב-spec: `gpt-image-2` (literal — בחירת המשתמש).
- אם API מחזיר `model_not_found` (סביר ב-2026): fallback מוסכם הוא `gpt-image-1`. עדכון בקובץ הסקיל בלבד, לא ב-callers.

## Exit codes
- `0` — הצליח, הקובץ קיים ו-`size > 0`.
- `1` — `OPENAI_API_KEY` לא set.
- `2` — API החזיר error object (אין `data` field). ה-error JSON מודפס ל-stderr.
- `3` — קובץ נכתב אבל ריק/חסר.

## קבצים קשורים
- [[agent-sami-image-gen]] — הקורא הראשי.
- [[agent-sasson]] — מתזמר את Sami (לא קורא ישירות לסקיל).
- [[root-env-files]] — `.env` ו-`.env.example` (`OPENAI_API_KEY`).
- [[skill-verification-before-completion]] — אותו עיקרון של "evidence לפני success" (`test -s` כאן הוא ה-evidence).

## הערות
- **JSON-escape ב-python:** הסיבה שלא משתמשים ב-`-d "{...}"` heredoc פשוט: prompt עם apostrophe או quote (`It's a "test"`) שובר את ה-JSON. `python3 -c 'json.dumps(...)'` מטפל בזה ללא תלויות חיצוניות.
- **jq ראשון, python fallback:** jq מהיר ונקי ב-happy path, אבל לא מותקן ב-Git Bash על Windows by default. python3 הוא dependency של ה-stack ממילא.
- **עלות:** כל קריאה עולה כסף. אין caching מובנה — callers שמחזרים על prompts צריכים להחזיק את ה-`.txt` sidecar (כפי ש-Sami עושה).
