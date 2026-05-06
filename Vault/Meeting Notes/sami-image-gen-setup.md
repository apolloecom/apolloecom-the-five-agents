# Sami Image-Gen Setup

## Overview
הקמת שכבת ייצור התמונות של הפרויקט ב-2026-05-06: סקיל טכני [[skill-gpt-image-gen]] שעוטף את OpenAI Images API (`gpt-image-2`), וסוכן קריאייטיב [[agent-sami-image-gen]] ("Sami") שאחראי על **עקביות ויזואלית** — סורק `sami-image-gen/reference/`, מחלץ סגנון, ומלחין prompt משולב לפני קריאה לסקיל. ה-active sub-agent הראשון של [[agent-sasson]] (פרט לעצמו). routing אוטומטי דרך trigger keywords (HE/EN) שנוספו ל-Sub-Agents Registry.

## Open Questions
- `gpt-image-2` לא קיים ב-OpenAI API — בקריאה החיה הראשונה נעשה שימוש ב-`gpt-image-1` (fallback עובד כצפוי, תיעוד בסקיל נכון).
- האם להוסיף trigger keywords נוספים (למשל "תייצר לי", "צריך תמונה", "make me a photo")? נמתין לשימוש בפועל ונרחיב לפי דפוסים שנראה.
- האם להוסיף trigger keywords נוספים (למשל "תייצר לי", "צריך תמונה", "make me a photo")? נמתין לשימוש בפועל ונרחיב לפי דפוסים שנראה.
- האם יוצרים MCP integration עתידי בין Sami ל-[[agent-publisher]] כדי שתמונות יזרמו אוטומטית ל-Publishing Log עם metadata?

## Session Log

### 2026-05-06 — Initial setup: gpt-image-gen skill + sami-image-gen agent [shipped]
- **What was done:**
  - יצירת `.claude/skills/gpt-image-gen/SKILL.md` — מעטפת טכנית ל-OpenAI Images API. 5 סעיפים (When/Inputs/How/Output contract/Notes), curl primary + python fallback ל-base64 decode, JSON-escape של ה-prompt דרך `python3 -c json.dumps`, gate `test -s` לפני דיווח success.
  - יצירת `.claude/agents/sami-image-gen.md` — סוכן קריאייטיב, `model: opus`, `tools: Read/Write/Bash/Skill/Glob`. Description כולל trigger keywords HE+EN. System prompt עם 7-step workflow (scan→analyze→map→compose→generate→sidecar→verify) + output contract JSON + edge cases.
  - יצירת תיקיית `sami-image-gen/` בשורש: `reference/.gitkeep`, `outputs/.gitkeep`, `agent.md` ו-`skill.md` (pointer docs לבני אדם — Claude Code לא קורא מהם).
  - עדכון `.env.example` עם `OPENAI_API_KEY=` placeholder (ב-`.env` הערך כבר היה).
  - עדכון `.gitignore` עם `sami-image-gen/outputs/*` + `!sami-image-gen/outputs/.gitkeep` (artifacts gitignored, dir נשמר).
  - עדכון `.claude/agents/sasson.md`: הוספת קטגוריית `image` ל-Phase 1, חוק trigger-based routing, ו-Sub-Agents Registry מורחב עם שדה `triggers: {he: [...], en: [...]}` ועם entry של `sami-image-gen` כ-active. גם ה-stubs קיבלו `triggers: []` ריקים לעקביות סכמה.
  - עדכון `CLAUDE.md` שורה 38 — `sami-image-gen` (active) + 4 stubs.
  - יצירת תיעוד ב-`Vault/Project Files/`: `agent-sami-image-gen.md` ו-`skill-gpt-image-gen.md`. עדכון `Vault/Project Files/_index.md` עם section "Skills — Project-specific (custom)" חדש.
- **Decisions:**
  - **שם הסוכן: `sami-image-gen`** — נבחר אחרי שהתבהר ש-"semi" בבקשת המשתמש הוא תמלול של "סמי" + תפקיד. שם השם הפרטי משולב בתפקיד נותן זהות ויזואלית ספציפית, לא generic "image-gen".
  - **מודל: `gpt-image-2` literal** — המשתמש בחר במפורש למרות שה-API הציבורי כיום הוא `gpt-image-1`. בסקיל יש section Notes שמסביר את ה-fallback אם API יחזיר `model_not_found`.
  - **מבנה היברידי (canonical-flat + working-dir-bundle):** הקובץ הקנוני של הסוכן ב-`.claude/agents/sami-image-gen.md` (כי Claude Code מגלה רק קבצים flat). תיקיית `sami-image-gen/` בשורש מכילה reference/outputs לעבודה + pointer docs לבני אדם — לא הגדרות לקלוד. זה מבדיל בין "מה קלוד קורא" לבין "מה האדם רואה".
  - **Trigger keywords כשדה ב-registry, לא כסעיף נפרד:** הוסף `triggers:` ל-YAML של כל sub-agent (גם stubs עם `[]`). נותן ל-Sasson יכולת auto-routing מובנית, וסכמה אחידה גם כש-stubs יהפכו ל-active.
  - **`outputs/` ב-gitignore חוץ מ-`.gitkeep`:** artifacts מתחדשים, ה-`.txt` עם ה-prompt לצידם הוא ה-traceability (קל לחזור ולחזור על generation אם צריך).
  - **`test -s` כ-gate חובה:** הסקיל לא מדווח success בלי לוודא קובץ לא ריק. הסוכן לא מחזיר `status: ok` בלי שהקובץ והגודל אומתו. עיקרון verification-before-completion הוטמע פה במקום אחד.
- **Notes / Caveats:**
  - הסקיל **לא נבדק עם API חי** בסשן זה — ייתכן שהקריאה הראשונה תחזיר `model_not_found`. ה-fallback מתועד וברור.
  - תיקיית `reference/` נוצרה ריקה. עד שיצטברו תמונות שם, Sami יחזור ל-neutral baseline ויודיע `references_used: []` ב-output contract.
  - ה-stubs הקודמים (researcher/writer/editor/publisher) לא הופכו ל-active במשימה זו — נשאר scope של Sami בלבד.
  - ה-`triggers` הם רשימה התחלתית קצרה (~6 ב-HE, ~6 ב-EN). יורחבו לפי שימוש בפועל.
- **Related:** [[agent-sasson]], [[agent-sami-image-gen]], [[skill-gpt-image-gen]], [[root-claude-md]], [[root-env-files]], [[skill-verification-before-completion]], [[skill-writing-plans]]

### 2026-05-06 — First live image generation: user with bull [shipped]
- **What was done:**
  - הרצה ראשונה של ה-pipeline כולו: Sasson CEO → general-purpose (as Sami) → gpt-image-gen skill → PNG output.
  - Sami קרא את תמונת ה-reference (`WhatsApp Image 2025-09-27 at 21.41.29.jpeg`), חילץ מאפייני זהות (גבר ממוצא מזרח-תיכוני, שנות 30 מוקדמות, ראש מגולח, זקן כהה, עור זית, חולצת פשתן חרדלית), וחיבר prompt אנגלי המשלב את הסובייקט עם שור בנוף דרמטי בשעת זהב.
  - הקובץ `sami-image-gen/outputs/2026-05-06-me-with-bull.png` (1.3 MB, 1024×1024 PNG) נוצר ואומת בהצלחה עם `test -s`.
  - `.txt` sidecar נשמר לאיטרציה עתידית.
- **Decisions:**
  - **`gpt-image-2` → `gpt-image-1` fallback אומת:** ה-API החזיר `model_not_found` ל-`gpt-image-2`; ה-fallback הלך ל-`gpt-image-1` בלי בעיה. סקיל עובד כצפוי — אין שינוי נדרש בקוד, הcaveat מתועד.
  - **Dispatch path בפועל:** `Agent(subagent_type='sasson')` לא עובד (custom agents לא נחשפים דרך ה-Agent tool של Superpowers). מסלול שעובד: `ceo` built-in → brief extraction → `general-purpose` עם brief מלא של Sami. ייתכן שנרצה לתעד routing pattern זה בצורה יציבה יותר בעתיד.
- **Notes / Caveats:**
  - Reference set עדיין קטן (תמונה אחת) — Sami מחלץ סגנון בהצלחה אבל עם יותר תמונות ה-style brief יתחזק.
  - ה-prompt מבוסס על תיאור מילולי של הסובייקט (לא image-to-image) — ה-API לא מקבל reference images. הפרטים הוחלפו ל-text description, התוצאה אמינה ויזואלית.
- **Related:** [[agent-sasson]], [[agent-sami-image-gen]], [[skill-gpt-image-gen]], [[skill-verification-before-completion]]
