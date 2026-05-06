# claude-folder-structure

## מה התיקייה עושה
`.claude/` היא תיקיית ההתאמות של Claude Code ברמת הפרויקט. מתחתיה שלוש תיקיות מתועדות + קובץ `settings.json`.

## תת-תיקיות
- **`.claude/agents/`** — סוכנים מותאמים אישית. כרגע ריקה (רק `.gitkeep`). פה יוגדרו 5 הסוכנים של "The Five Agents" כשיוקמו.
- **`.claude/commands/`** — slash commands מותאמים. כרגע ריקה (רק `.gitkeep`).
- **`.claude/skills/`** — 17 skills מותאמים שהותקנו ידנית מ-`obra/superpowers` + 3 skills של Obsidian. כל skill בתת-תיקייה משלו עם `SKILL.md`.

## למי היא משויכת
- **תפקיד:** תצורת harness של Claude Code לפרויקט.
- **בעלים:** מפתח הפרויקט.
- **קוראים:** harness של Claude Code (אוטומטי).

## מיקום
`/.claude/`

## קבצים קשורים
- [[claude-settings]] — `settings.json` בתוך התיקייה.
- [[root-claude-md]] — `CLAUDE.md` שמתאר את `.claude/` בפרויקט.
- [[skill-using-superpowers]] ועוד 16 skills — תיעודם בתיקיית [[Project Files/_index]].

## הערות
- תיקיות `agents/` ו-`commands/` ריקות וממתינות להגדרת הצוות (ה-CEO + 5 סוכני התוכן).
- הסקילים נוספו ב-commit `8da7b3f`.
