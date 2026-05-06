# claude-settings

## מה הקובץ עושה
`.claude/settings.json` הוא קובץ ההגדרות של Claude Code ברמת הפרויקט. כרגע מפעיל פלאגין יחיד: `superpowers@claude-plugins-official`.

## תוכן הנוכחי
```json
{
  "enabledPlugins": {
    "superpowers@claude-plugins-official": true
  }
}
```

## למי הוא משויך
- **תפקיד:** תצורת harness של Claude Code.
- **בעלים:** מפתח הפרויקט.
- **קוראים:** harness של Claude Code (אוטומטי בתחילת סשן).

## מיקום
`/.claude/settings.json`

## מצב git
הקובץ untracked כרגע ב-git (ראה `git status` הראשוני). יישלח ב-commit קרוב.

## קבצים קשורים
- [[skill-using-superpowers]] — הסקיל שמופעל דרך הפלאגין הזה.
- [[claude-folder-structure]] — מבנה התיקייה `.claude/` שבתוכה הקובץ.
- [[root-claude-md]] — `CLAUDE.md` שמתאר את הפרויקט שבו ההגדרות פעילות.

## הערות
- ניתן להרחיב את הקובץ עם `permissions`, `env`, `hooks` באמצעות הסקיל [[skill-update-config]] (זמין ברמת המשתמש).
- אם המשתמש רוצה אכיפה אוטומטית של obsidian-vault-workflow בכל סשן (ולא רק בקשה ב-CLAUDE.md), נדרש hook ב-settings.json — דורש בקשה מפורשת.
