# Yael — pointer

הקובץ הזה הוא pointer לבני אדם. **Claude Code לא קורא ממנו** — הוא מגלה סוכנים רק מ-`.claude/agents/*.md` flat.

## הקובץ הקנוני של הסוכן

```
../.claude/agents/yael.md
```

שם זה ה-system prompt, ה-tools, וה-frontmatter שמפעילים את הסוכן בפועל.

## מבנה התיקייה הזו

```
yael/
├── style-guide.md   # מדריך הסגנון של הפרויקט. Yael קוראת אותו בתחילת כל משימה.
├── reference/       # דוגמאות לטקסטים בסגנון שלנו. Yael קוראת את כולם.
└── agent.md         # אתה כאן
```

## תיקיות שיעל עובדת מולן (בשורש הפרויקט)

```
Content/        # קלט: מאמרי גלם שממתינים לשכתוב
Content/Ready/  # סיים: מקור מועבר לכאן אחרי ש-Yael סיימה (Sasson עושה את ה-mv בפועל)
Output/         # פלט: התוצרים המשוכתבים (tracked בגיט)
```

## מה Yael יודעת

- לקרוא, לכתוב, לערוך, לסכם, לתרגם, לזהות צורך בתמונה.
- LLM-only: tools = `Read, Write, Edit, Glob, Grep`.

## מה Yael לא יודעת

- לחפש באינטרנט (אין WebSearch / WebFetch).
- ליצור תמונות (אין גישה ל-API; משאירה `{{IMAGE_NEEDED: "..."}}` placeholder ש-Sasson מטפל בו דרך Sami).
- להפעיל סוכנים אחרים (אין Agent tool — סאב-אייג'נטים ב-Claude Code לא יכולים לדפצ' סאב-אייג'נטים אחרים).
- להריץ shell (אין Bash — את ה-mv של המקור Sasson עושה אחריה).

## איך להפעיל את Yael

מתוך Sasson (ה-CEO), כשבקשה מכילה trigger keyword (למשל "שכתב", "ערוך", "rewrite", "rephrase"). אפשר גם לקרוא לה ידנית:

```
Agent(subagent_type='yael', prompt='<תיאור משימה + נתיב קובץ ב-Content/>')
```

Yael תקרא את `style-guide.md` ואת `reference/`, תשכתב את הקובץ שצוין מ-`Content/`, תשמור ל-`Output/`, ותחזיר JSON עם רשימת `image_placeholders` (אם השאירה כאלה).

## הזרימה השלמה (Yael + Sasson + Sami)

1. משתמש: "שכתבי את `Content/article.md`"
2. Sasson מזהה trigger → דוחה brief ל-Yael עם נתיב המקור
3. Yael קוראת style-guide + reference, משכתבת, שומרת ל-`Output/article.md`, מציינת `{{IMAGE_NEEDED: "..."}}` היכן שצריך תמונה
4. Yael מחזירה JSON: `output_path`, `image_placeholders[]`, `summary`
5. Sasson עושה `git mv Content/article.md Content/Ready/article.md`
6. Sasson עובר על כל placeholder ומפעיל את Sami (דרך main-Claude) להפיק תמונה
7. Sasson מחליף כל `{{IMAGE_NEEDED}}` ב-`![alt](path)` מתוך התוצר של Sami
8. הקובץ הסופי ב-`Output/article.md` מוכן לפרסום
