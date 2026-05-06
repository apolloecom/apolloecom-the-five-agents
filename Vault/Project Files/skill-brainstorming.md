# skill-brainstorming

## מה הסקיל עושה
מחייב תהליך סיעור-מוחות לפני **כל** עבודה יצירתית: יצירת פיצ'רים, רכיבים, פונקציונליות חדשה או שינוי התנהגות. מבקש מ-Claude לחקור כוונות משתמש, דרישות ועיצוב לפני שמתחילים לקודד.

## מתי להפעיל
לפני כל יצירה. גם אם המשימה נראית פשוטה.

## קבצים בתת-התיקייה
- **`SKILL.md`** — המסמך הראשי.
- **`spec-document-reviewer-prompt.md`** — פרומפט לסוכן שעובר על מסמך מפרט.
- **`visual-companion.md`** — מלווה ויזואלי לתהליך הסיעור.
- **`scripts/start-server.sh`**, **`scripts/stop-server.sh`** — הפעלה/עצירה של שרת מקומי.
- **`scripts/server.cjs`** — שרת Node.js לתצוגת הסיעור.
- **`scripts/helper.js`** — עוזר client-side.
- **`scripts/frame-template.html`** — תבנית HTML למסגרת.

## למי הוא משויך
- **תפקיד:** שלב חקר לפני יישום.
- **קוראים:** Claude Code לפני יצירה/שינוי.

## מיקום
`/.claude/skills/brainstorming/`

## קבצים קשורים
- [[skill-writing-plans]] — שלב הבא לאחר סיעור-מוחות.
- [[skill-using-superpowers]] — מציין שזהו סקיל "process".

## הערות
הסקילים בסדר עדיפות: brainstorming → writing-plans → executing-plans/test-driven-development.
