# skill-using-git-worktrees

## מה הסקיל עושה
מבטיח שעבודה על פיצ'ר חדש מתבצעת בסביבה מבודדת — דרך כלי native של ה-harness או fallback ל-`git worktree`. מונע ערבוב עבודה עם ה-workspace הנוכחי.

## מתי להפעיל
בתחילת עבודה על פיצ'ר שצריך בידוד, או לפני ביצוע תוכנית יישום.

## קבצים בתת-התיקייה
- **`SKILL.md`** — המסמך הראשי (קובץ יחיד).

## למי הוא משויך
- **תפקיד:** ניהול workspace.
- **קוראים:** Claude Code בתחילת פיצ'ר.

## מיקום
`/.claude/skills/using-git-worktrees/`

## קבצים קשורים
- [[skill-executing-plans]] — מיועד לרוץ ב-worktree מבודד.
- [[skill-finishing-a-development-branch]] — בסוף נסגור גם את ה-worktree.
