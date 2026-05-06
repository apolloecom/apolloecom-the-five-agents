# Sami — pointer

הקובץ הזה הוא pointer לבני אדם. **Claude Code לא קורא ממנו** — הוא מגלה סוכנים רק מ-`.claude/agents/*.md` flat.

## הקובץ הקנוני של הסוכן

```
../.claude/agents/sami-image-gen.md
```

שם זה ה-system prompt, ה-tools, וה-frontmatter שמפעילים את הסוכן בפועל.

## מבנה התיקייה הזו

```
sami-image-gen/
├── reference/   # זרוק לכאן תמונות השראה (סגנון, פלטה, קומפוזיציה)
├── outputs/     # תמונות שהסוכן ייצר (gitignored — רק ה-.gitkeep ב-tracked)
├── agent.md     # אתה כאן
└── skill.md     # pointer לסקיל הטכני (gpt-image-gen)
```

## איך להפעיל את Sami

מתוך Sasson (ה-CEO), כשבקשה מכילה trigger keyword (למשל "תמונה של" / "generate image"). אפשר גם לקרוא לו ידנית:

```
Agent(subagent_type='sami-image-gen', prompt='<תיאור התמונה>')
```

Sami יסרוק את `reference/`, יחלץ סגנון, ויקרא לסקיל `gpt-image-gen` כדי לייצר את הקובץ ב-`outputs/`.
