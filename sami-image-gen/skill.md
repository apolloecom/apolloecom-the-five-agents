# gpt-image-gen — pointer

הקובץ הזה הוא pointer לבני אדם. **Claude Code לא קורא ממנו** — הוא מגלה skills רק מ-`.claude/skills/*/SKILL.md`.

## הקובץ הקנוני של הסקיל

```
../.claude/skills/gpt-image-gen/SKILL.md
```

## מה הסקיל עושה

מעטפת טכנית טהורה לקריאת OpenAI Images API. מקבל `prompt` + `output_path`, מחזיר קובץ PNG על דיסק. **לא** מקבל החלטות יצירתיות — זה התפקיד של Sami (`./agent.md`).

## דרישות סביבה

- `OPENAI_API_KEY` ב-`.env` (כבר קיים)
- `python3` ב-PATH (תמיד; משמש ל-JSON-escape של ה-prompt וכ-fallback ל-base64 decode)
- `jq` אופציונלי — נעדיף אותו אם קיים, אחרת python מטפל בכל

## מודל

הסקיל קורא ל-`gpt-image-2`. אם API מחזיר `model_not_found`, ה-fallback המוסכם הוא `gpt-image-1`.
