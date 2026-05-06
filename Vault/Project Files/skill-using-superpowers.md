# skill-using-superpowers

## מה הסקיל עושה
מסביר ל-Claude איך למצוא ולהשתמש בסקילים. נחשב לאבן הפינה של מערכת ה-Superpowers — נטען בתחילת כל סשן ומחייב הפעלת `Skill` לפני כל תגובה (גם שאלות הבהרה).

## מתי להפעיל
תמיד בתחילת סשן. אוטומטי — מטופל ע"י SessionStart hook.

## קבצים בתת-התיקייה
- **`SKILL.md`** — המסמך הראשי.
- **`references/copilot-tools.md`** — מיפוי שמות כלים ל-Copilot CLI.
- **`references/codex-tools.md`** — מיפוי שמות כלים ל-Codex.
- **`references/gemini-tools.md`** — מיפוי שמות כלים ל-Gemini CLI.

## למי הוא משויך
- **תפקיד:** Bootstrap של מערכת ה-skills.
- **קוראים:** Claude Code בכל סשן.

## מיקום
`/.claude/skills/using-superpowers/`

## קבצים קשורים
- [[skill-brainstorming]], [[skill-writing-plans]] — סקילים שלפי הזרימה הזו צריך להפעיל.
- [[claude-settings]] — מפעיל את plug-in ה-Superpowers.

## הערות
ה-references מאפשרים למסגרות שאינן Claude Code (Copilot, Codex, Gemini) להשתמש בסקילים תוך מיפוי שמות כלים.
