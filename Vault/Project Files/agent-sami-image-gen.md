# agent-sami-image-gen

## מה הסוכן עושה
**Sami** — סוכן הקריאייטיב הוויזואלי של The Five Agents. מקבל בקשה מילולית לתמונה ומפיק קובץ PNG עם עקביות סגנונית מול שאר התמונות בפרויקט. סורק את `sami-image-gen/reference/`, מחלץ סגנון (פלטה, קומפוזיציה, mood), מלחין prompt באנגלית, וקורא לסקיל [[skill-gpt-image-gen]] לייצור התמונה. שומר את הפלט ב-`sami-image-gen/outputs/<YYYY-MM-DD>-<slug>.png` עם sibling `.txt` של ה-prompt לאיטרציה עתידית.

## status
**active** — הסוכן מוכן ל-dispatch מ-[[agent-sasson]] לפי trigger keywords.

## למי הוא משויך
- **תפקיד:** sub-agent של [[agent-sasson]] (קריאייטיב/ויזואל).
- **מופעל ע"י:** Sasson כשהבקשה מכילה trigger keyword ("תמונה של", "ציור של", "generate image" וכו'). אפשר גם dispatch ידני: `Agent(subagent_type='sami-image-gen', ...)`.
- **תלוי ב:** [[skill-gpt-image-gen]] (מעטפת ה-API), `OPENAI_API_KEY` ב-`.env`.

## מיקום
- **קובץ קנוני:** `/.claude/agents/sami-image-gen.md` (זה מה ש-Claude Code מגלה).
- **תיקיית עבודה:** `/sami-image-gen/` בשורש הפרויקט — `reference/` (input), `outputs/` (gitignored), `agent.md` + `skill.md` (pointers לבני אדם).

## מבנה הקובץ
- Frontmatter: `name`, `description` (כולל trigger keywords HE/EN), `tools` (Read/Write/Bash/Skill/Glob), `model: opus`, `status: active`.
- System prompt: 6 סעיפים — Identity → 7-step Workflow (scan→analyze→map→compose→generate→sidecar→verify) → Output Contract → Edge cases → Style consistency principle → Communication style.

## קבצים קשורים
- [[agent-sasson]] — ה-CEO שמדפצ' את Sami.
- [[skill-gpt-image-gen]] — הסקיל שעוטף את OpenAI Images API.
- [[root-claude-md]] — מתעד את Sami ברשימת sub-agents (סעיף "Sub-agents קיימים").
- [[skill-verification-before-completion]] — חוק "test -s לפני 'תמונה נוצרה'" שווה ל-Phase 7 שלו.
- [[skill-obsidian-vault-workflow]] — לעדכון Publishing Log/Content Briefs כשתמונות נכנסות ל-content.

## הערות
- ה-7-step workflow הוא **חובה** — בעיקר step 1 (scan reference) ו-step 7 (test -s gate). דילוג מהווה הפרת חוזה.
- כשתיקיית `reference/` ריקה — Sami משתמש ב-neutral baseline ("clean, modern, well-composed, neutral palette") ומציין `references_used: []` ב-output.
- collision policy ל-output filename: סופית `-2`/`-3`/... אם slug קיים. אסור לדרוס.
- ה-`.txt` sidecar של ה-prompt הוא ל-iteration: כשמבקשים "וריאציה", קוראים את ה-`.txt`, עורכים, ויוצרים חדשה — בלי לאבד ניסוח.
