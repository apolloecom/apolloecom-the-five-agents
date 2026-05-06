# obsidian-config

## מה התיקייה עושה
`.obsidian/` היא תיקיית התצורה של Obsidian בה הקובץ root הופך ל-Vault של Obsidian. היא מתעדת אילו core plugins פעילים, מראה (appearance), workspace, ו-sync.

## הקבצים בה
- **`app.json`** — הגדרות כלליות של האפליקציה (כרגע ריק `{}` — ברירת מחדל).
- **`appearance.json`** — מראה: theme, צבעים, גופנים (כרגע ריק `{}`).
- **`core-plugins.json`** — רשימת ה-core plugins שהופעלו: file-explorer, global-search, switcher, graph, backlink, canvas, outgoing-link, tag-pane, properties, page-preview, daily-notes, templates, note-composer, command-palette, editor-status, bookmarks, outline, word-count, file-recovery, sync, **bases**.
- **`workspace.json`** — מצב חלונות פתוחים בעת הסגירה האחרונה.

## תכונות בולטות שהופעלו
- **bases** (true) — תוסף Bases של Obsidian פעיל. הסקיל [[skill-obsidian-bases]] רלוונטי.
- **sync** (true) — Obsidian Sync פעיל.
- **slash-command** (false), **publish** (false), **webviewer** (false) — מכובים.

## למי היא משויכת
- **תפקיד:** תצורת ה-vault של Obsidian.
- **בעלים:** מפתח הפרויקט (משתמש Obsidian).
- **קוראים:** אפליקציית Obsidian.

## מיקום
`/.obsidian/` (שורש)

## קבצים קשורים
- [[skill-obsidian-vault-workflow]] — הזרימה הפרויקטלית שעובדת על תיקיית `Vault/`.
- [[skill-obsidian-markdown]] — תחביר ה-Markdown שיוזן בתוך ה-vault.
- [[skill-obsidian-bases]] — תוסף Bases שהופעל ב-core-plugins.json.

## הערות
- שם התיקייה הראשית של ה-vault הוא `Vault/` (V גדולה), בעוד שהסקיל מציין `vault/` (קטנה) — אנחנו עובדים בתיקייה הקיימת.
- `app.json` ו-`appearance.json` ריקים — אין קונפיגורציה מותאמת מעבר לברירת מחדל.
