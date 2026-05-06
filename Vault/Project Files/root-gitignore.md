# root-gitignore

## מה הקובץ עושה
`.gitignore` מגדיר אילו קבצים ותיקיות לא נכנסים ל-git. מסתיר סודות (`.env`), תלויות (`node_modules/`, `__pycache__/`), build artifacts, קבצי IDE, וקבצי OS.

## הקטגוריות שמוסתרות
1. **Environment** — `.env`, `.env.local`, `.env.*.local`.
2. **Dependencies** — `node_modules/`, `__pycache__/`, `*.pyc`, `.venv/`, `venv/`.
3. **Build** — `dist/`, `build/`, `*.egg-info/`.
4. **IDE** — `.vscode/`, `.idea/`, `*.swp`.
5. **OS** — `.DS_Store`, `Thumbs.db`.
6. **Logs** — `*.log`, `logs/`.

## למי הוא משויך
- **תפקיד:** היגיינת ריפו של git.
- **בעלים:** מפתח הפרויקט.
- **קוראים:** git (אוטומטי).

## מיקום
`/.gitignore` (שורש)

## קבצים קשורים
- [[root-env-files]] — `.env` שמוסתר ע"י הקובץ.
- [[root-claude-md]] — מתועד תחת אותה תיקיית פרויקט.

## הערות
- הקובץ תומך גם ב-Node (npm) וגם ב-Python (venv, pyc) — מצב נפוץ עבור פרויקט עם Anthropic SDK שמסופק לשתי השפות.
