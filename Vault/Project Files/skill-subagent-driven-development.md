# skill-subagent-driven-development

## מה הסקיל עושה
ביצוע תוכניות יישום עם משימות עצמאיות בתוך הסשן הנוכחי, באמצעות תת-סוכנים ייעודיים: ספק (implementer), reviewer של מפרט, ו-reviewer של איכות קוד.

## מתי להפעיל
בעת ביצוע תוכנית עם משימות עצמאיות שניתנות להאצלה.

## קבצים בתת-התיקייה
- **`SKILL.md`** — המסמך הראשי.
- **`implementer-prompt.md`** — פרומפט לתת-סוכן המיישם.
- **`spec-reviewer-prompt.md`** — פרומפט לתת-סוכן הבודק את המפרט.
- **`code-quality-reviewer-prompt.md`** — פרומפט לתת-סוכן שעובר על איכות הקוד.

## למי הוא משויך
- **תפקיד:** orchestration בתוך-סשן.
- **קוראים:** Claude Code בעת ביצוע.

## מיקום
`/.claude/skills/subagent-driven-development/`

## קבצים קשורים
- [[skill-dispatching-parallel-agents]] — חלופה: סוכנים מקבילים בלי שלבי review.
- [[skill-executing-plans]] — מקביל אך כסשנים נפרדים.
- [[skill-writing-plans]] — מספק את התוכנית לביצוע.
