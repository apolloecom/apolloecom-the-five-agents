# agent-yael

## מה הסוכן עושה
**Yael** (יעל) — סוכנת הכתיבה של The Five Agents. מקבלת מאמר גלם מ-`Content/`, קוראת את `yael/style-guide.md` ואת תיקיית `yael/reference/` (דוגמאות לטקסטים בסגנון הפרויקט), ומשכתבת את המאמר בסגנון שלנו. כשהיא מזהה מקום שצריך תמונה, היא משאירה placeholder בפורמט `{{IMAGE_NEEDED: "<תיאור מפורט ל-Sami>"}}` במקום הנכון בטקסט. שומרת את התוצר ב-`Output/<original-stem>.md` ומחזירה JSON עם רשימת ה-placeholders ל-Sasson, שמטפל בהם דרך Sami ועושה את ה-`git mv` של המקור ל-`Content/Ready/`.

## status
**active** — הסוכנת מוכנה ל-dispatch מ-[[agent-sasson]] לפי trigger keywords (HE: "שכתב", "ערוך", "תרגם", "סכם", "מאמר", "תוכן", "פוסט"; EN: "rewrite", "edit", "translate", "summarize", "article", "content", "post").

## למי הוא משויך
- **תפקיד:** sub-agent של [[agent-sasson]] (כתיבה/תוכן). מחליפה את [[agent-writer]] ה-stub שהוסר.
- **מופעל ע"י:** Sasson כשהבקשה מכילה trigger keyword. אפשר גם dispatch ידני: `Agent(subagent_type='yael', ...)`.
- **תלוי ב:** `yael/style-guide.md` (מדריך סגנון, יוצר ע"י המשתמש), `yael/reference/` (דוגמאות), [[agent-sasson]] (post-processing של placeholders).

## מיקום
- **קובץ קנוני:** `/.claude/agents/yael.md` (זה מה ש-Claude Code מגלה).
- **תיקיית עבודה:** `/yael/` בשורש הפרויקט — `style-guide.md`, `reference/` (input), `agent.md` (pointer לבני אדם).
- **תיקיות קלט/פלט (בשורש):** `/Content/` (מאמרי גלם), `/Content/Ready/` (מקור אחרי טיפול), `/Output/` (תוצרים משוכתבים, tracked בגיט).

## מבנה הקובץ
- Frontmatter: `name`, `description` (כולל trigger keywords HE/EN), `tools: Read, Write, Edit, Glob, Grep` (LLM-only — אין Bash, אין Agent, אין WebSearch), `model: opus`, `status: active`.
- System prompt: 6 סעיפים — Identity & Hard Constraints → 6-step Workflow (load style → read source → rewrite → flag image needs → save → verify) → Output Contract JSON → Edge cases → Style consistency principle → Communication style.

## קבצים קשורים
- [[agent-sasson]] — ה-CEO שמדפצ' את Yael ומבצע post-processing של ה-placeholders שלה.
- [[agent-sami-image-gen]] — Sasson מפעיל אותו לכל `{{IMAGE_NEEDED}}` placeholder של Yael.
- [[root-claude-md]] — מתעד את Yael ברשימת sub-agents (סעיף "Sub-agents קיימים").
- [[skill-obsidian-vault-workflow]] — לעדכון Publishing Log/Meeting Notes כשתוצרים נכנסים ל-content.
- [[skill-verification-before-completion]] — Yael עושה Read אחרי Write לאמת שהפלט נכתב לפני דיווח success.

## הערות
- ה-6-step workflow הוא **חובה** — בעיקר Step 1 (load style-guide + reference) ו-Step 6 (Read-verify לפני דיווח). דילוג מהווה הפרת חוזה.
- כש-`yael/style-guide.md` הוא רק stub (TODO header) — Yael ממשיכה עם neutral baseline ומציינת `style_guide_status: "stub_only"` ב-output.
- collision policy ל-output filename: סופית `-2`/`-3`/... אם slug קיים. אסור לדרוס.
- Yael **לא מזיזה את המקור** מ-`Content/` ל-`Content/Ready/` — Sasson עושה את זה דרך `git mv` אחרי שהוא מאמת את ה-output. הסיבה: Yael היא LLM-only ואין לה Bash, וגם — הזזת המקור לפני אימות = סיכון לאיבוד.
- ה-trigger keywords "ערוך"/"edit" עלולים להתנגש עם [[agent-editor]] בעתיד. כשעורך יהפוך ל-active, יש להעביר את המילים האלה אליו.
