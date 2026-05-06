# skill-systematic-debugging

## מה הסקיל עושה
איתור באגים שיטתי המתמקד בשורש הבעיה — לא בתסמינים. מחייב מתודולוגיה לפני הצעת תיקון. סקיל "rigid".

## מתי להפעיל
בכל באג, כשל בדיקה או התנהגות לא צפויה — **לפני** הצעת תיקון.

## קבצים בתת-התיקייה
- **`SKILL.md`** — המסמך הראשי.
- **`CREATION-LOG.md`** — יומן יצירת הסקיל ושיקולי עיצוב.
- **`root-cause-tracing.md`** — מתודולוגיה לאיתור שורש בעיה.
- **`defense-in-depth.md`** — שכבות הגנה ומניעת חזרת באגים.
- **`condition-based-waiting.md`** + **`condition-based-waiting-example.ts`** — דוגמת המתנה מבוססת תנאי (לעומת polling/sleep).
- **`find-polluter.sh`** — סקריפט לאיתור בדיקה ש"מזהמת" סביבה.
- **`test-academic.md`**, **`test-pressure-1.md`**, **`test-pressure-2.md`**, **`test-pressure-3.md`** — תרחישי תרגול ולחץ.

## למי הוא משויך
- **תפקיד:** איכות והדפסה של תקלות.
- **קוראים:** Claude Code בעת באג.

## מיקום
`/.claude/skills/systematic-debugging/`

## קבצים קשורים
- [[skill-test-driven-development]] — לאחר שמצאנו את הבאג, נכתוב בדיקה שתופסת אותו.
- [[skill-verification-before-completion]] — לאחר התיקון, חובה לוודא.
- [[skill-receiving-code-review]] — אם הבאג עלה ב-review.

## הערות
זה הסקיל עם הכי הרבה קבצי לוויין — מתאר תהליך עם תרגולים מובנים.
