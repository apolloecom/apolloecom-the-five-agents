# Yael Setup

## Overview
הקמת סוכנת הכתיבה של הפרויקט ב-2026-05-06: [[agent-yael]] (Yael, יעל) — סוכנת LLM-only (tools: Read/Write/Edit/Glob/Grep) שמשכתבת מאמרי גלם מ-`Content/` ל-`Output/` בסגנון הפרויקט. הסגנון מוגדר ב-`yael/style-guide.md` + דוגמאות ב-`yael/reference/` (שניהם נקראים בתחילת כל dispatch). Yael לא מייצרת תמונות בעצמה — היא משאירה placeholders בפורמט `{{IMAGE_NEEDED: "<תיאור ל-Sami>"}}` במקומות הנכונים בטקסט, ו-[[agent-sasson]] מטפל בהם בפוסט-פרוסס: עושה `git mv` למקור ל-`Content/Ready/`, ומפעיל את [[agent-sami-image-gen]] לכל placeholder. החליפה את ה-stub `[[agent-writer]]` שהוסר. שלישית active sub-agent (אחרי Sasson ו-Sami).

## Open Questions
- `yael/style-guide.md` נשאר stub עם TODO header — נדרש מילוי בפועל לפני שימוש בפרודקשן. המשתמש יצור את התוכן בנפרד.
- `yael/reference/` ריק — Yael תשתמש ב-neutral baseline עד שיצטברו דוגמאות.
- Trigger overlap: "ערוך"/"edit" משויכים ל-Yael כעת. כש-[[agent-editor]] יהפוך ל-active, יש להעביר את המילים האלה אליו (decision pending).
- האם Yael צריכה תיעוד ב-`Vault/Publishing Log/<topic>.md` כשהיא רצה על production article, או שזה תפקיד של Sasson ב-Phase 8? ננסה ב-smoke test הראשון ונחליט.
- מה קורה אם Sami מחזיר error באמצע פוסט-פרוסס של 5 placeholders? Sasson אמור לעצור ולהחזיר `blocking_questions`, אבל זה לא נבדק בפועל.

## Session Log

### 2026-05-06 — Initial setup: yael agent + Sasson post-processing protocol [shipped]
- **What was done:**
  - יצירת `.claude/agents/yael.md` — סוכנת LLM-only, `model: opus`, `tools: Read, Write, Edit, Glob, Grep` (אין Bash/Agent/WebSearch). Description כולל trigger keywords HE+EN. System prompt עם 6-step workflow (load style → read source → rewrite → flag image needs → save → verify) + Output Contract JSON עם `image_placeholders[]` + edge cases + style consistency principle.
  - יצירת תיקיית `yael/` בשורש: `style-guide.md` (stub עם TODO header), `reference/.gitkeep`, `agent.md` (pointer לבני אדם — Claude Code לא קורא ממנו).
  - יצירת תיקיות `Content/`, `Content/Ready/`, `Output/` בשורש עם `.gitkeep` לכל אחת. שלוש התיקיות tracked בגיט (לא gitignored כמו `sami-image-gen/outputs/` — תוצרי טקסט שווים שמירה היסטורית).
  - עדכון `.claude/agents/sasson.md`: (a) החלפת ה-`writer` stub registry entry ב-`yael` עם `status: active` ו-trigger keywords מלאים. (b) הוספת סעיף חדש "**7.5. Phase 5b — Yael Post-Processing Protocol**" אחרי Phase 5 (Dispatch) ולפני Phase 6 (Context Flush) — מסביר את ה-4 צעדים: `git mv` למקור, לולאה על placeholders עם dispatch ל-Sami, החלפה דרך Edit, verify ב-grep, vault append.
  - עדכון `CLAUDE.md` שורה 38 — `yael` כ-active הצטרפה ל-`sami-image-gen`, נשארו 3 stubs (researcher/editor/publisher במקום 4). הוספת פסקה קצרה שמסבירה את זרימת Yael+Sasson עם הפניה לסעיף 7.5.
  - מחיקת `.claude/agents/writer.md` ו-`Vault/Project Files/agent-writer.md` (Yael היא ה-writer; ה-stub הוחלף).
  - יצירת `Vault/Project Files/agent-yael.md` (mirror format של [[agent-sami-image-gen]]). עדכון `Vault/Project Files/_index.md` — הסרת `[[agent-writer]]`, הוספת `[[agent-yael]]`.
  - יצירת קובץ זה (`Vault/Meeting Notes/yael-setup.md`). עדכון `Vault/Meeting Notes/_index.md` עם entry חדש.
- **Decisions:**
  - **שם הסוכנת: `yael`** — המשתמש בחר השם העברי "יעל" כדי לתת זהות אישית לתפקיד. עוקב אחרי דפוס Sasson/Sami (שמות פרטיים בעברית) במקום generic "writer".
  - **LLM-only tools (אין Bash/Agent/WebSearch):** הגבלה מודעת. סאב-אייג'נטים ב-Claude Code לא יכולים להפעיל סאב-אייג'נטים אחרים, אז Yael לא יכולה להפעיל את Sami בעצמה. במקום זה — placeholder pattern + Sasson כ-orchestrator. בנוסף: בלי Bash היא לא יכולה לטעות עם file system ops; היא רק קוראת/כותבת.
  - **Source-file move ע"י Sasson, לא Yael:** Yael היא LLM-only (אין לה Bash). הזזת המקור = `git mv` שזה תפקיד אורקסטרציה ממילא. גם בטיחות: המקור לא זז עד ש-Sasson מאמת שה-output נכתב.
  - **`{{IMAGE_NEEDED: "..."}}` placeholder inline בטקסט:** שמירת המיקום הנרטיבי. Sasson מחליף עם Edit `replace_all=false` בסדר עליון-תחתון, בלי IDs. פשוט ויעיל.
  - **Sasson דרך main-Claude מפעיל את Sami:** סאב-אייג'נטים לא דופצ'ים סאב-אייג'נטים. Sasson מוציא הוראה ב-output contract; main-Claude מבצע את `Agent(subagent_type='sami-image-gen', ...)`. עקבי עם ה-pattern שהתגלה ב-[[sami-image-gen-setup]].
  - **`Output/` tracked (לא gitignored):** שלא כמו `sami-image-gen/outputs/` (PNGs כבדים, מתחדשים), תוצרי הטקסט של Yael שווים history בגיט — קל לראות diff בין גרסאות, להחזיר אם נדרש.
  - **Trigger keywords משותפות:** "ערוך"/"edit" משוייכות כעת ל-Yael, אבל זה overlap פוטנציאלי עם editor. תועד כ-Open Question.
  - **`style-guide.md` כ-stub עם TODO header:** המשתמש יצור את התוכן בנפרד. Yael ממשיכה לעבוד גם בלי תוכן בפועל (`style_guide_status: "stub_only"` ב-output) — neutral baseline.
- **Notes / Caveats:**
  - הסוכנת **לא נבדקה בריצה חיה** בסשן זה — ייתכנו תקלות edge case שייחשפו רק ב-smoke test. הצעדים הבאים: drop article ל-`Content/sample.md`, dispatch מ-Sasson, וודא שה-output contract תקין וש-Sasson יודע לעבד את ה-placeholders.
  - תיקיית `yael/reference/` ריקה. Yael תחזור ל-neutral baseline עד שיצטברו דוגמאות. כשמוסיפים דוגמאות, ה-style brief שלה יזוז לכיוון שלהן בהדרגה.
  - הזרימה Yael→Sasson→Sami תלויה ב-output contract של Yael להיות JSON תקני. אם Yael מחזירה JSON שבור, Sasson לא יידע לעבד. לא הוספנו JSON schema validation — נסתמך על ההוראה הברורה ב-system prompt.
  - ה-trigger "ערוך" בעברית עלול להפעיל את Yael על משימות שלא קשורות לתוכן (למשל "ערוך את הקובץ X" טכנית). Sasson אמור לסווג נכון לפי הקשר, אבל זה edge case שצריך לעקוב אחריו.
- **Related:** [[agent-sasson]], [[agent-yael]], [[agent-sami-image-gen]], [[sami-image-gen-setup]], [[root-claude-md]], [[skill-verification-before-completion]], [[skill-obsidian-vault-workflow]]
