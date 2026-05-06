# skill-verification-before-completion

## מה הסקיל עושה
מחייב הרצת אימותים לפני הצהרה שמשימה הושלמה, נסגרה או עוברת. דורש evidence (פלט פקודה אמיתי) לפני assertions. אוסר commits/PRs ללא אימות מאומת.

## מתי להפעיל
לפני סגירת משימה, commit, יצירת PR, או הצהרה כללית של "זה עובד".

## קבצים בתת-התיקייה
- **`SKILL.md`** — המסמך הראשי (קובץ יחיד).

## למי הוא משויך
- **תפקיד:** שכבת מנעול אחרון על איכות.
- **קוראים:** Claude Code לפני declaration.

## מיקום
`/.claude/skills/verification-before-completion/`

## קבצים קשורים
- [[skill-test-driven-development]] — אילו בדיקות אמורות לעבור.
- [[skill-finishing-a-development-branch]] — סגירת ענף לאחר verify.
- [[skill-requesting-code-review]] — review לאחר verify.

## הערות
חוק ברזל: "evidence before assertions, always".
