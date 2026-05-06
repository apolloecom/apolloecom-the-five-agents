# agent-publisher

## מה הסוכן עושה
**מתוכנן:** תזמון ופרסום של תוכן מאושר ל-channels (Instagram, Twitter, Substack, LinkedIn וכו'). אינטגרציה עם MCP servers של הפלטפורמות. מתעד כל פרסום ב-`Vault/Publishing Log/` עם metrics לאחר פרסום, ומפיק post-mortems.

## status
**stub** — הקובץ קיים כ-placeholder, ה-CEO לא יידפצ' אותו עד שיתמלא בהגדרת תפקיד מלאה **וגם** עד שיוגדרו MCP integrations לפלטפורמות.

## למי הוא משויך
- **תפקיד:** sub-agent של [[agent-ceo]].
- **מופעל ע"י:** ה-CEO בלבד.
- **קוראים:** עוד-לא-יישומי.

## מיקום
`/.claude/agents/publisher.md`

## קבצים קשורים
- [[agent-ceo]] — הסוכן הקורא לו.
- [[agent-editor]] — מקור הקלט (תוכן מאושר).
- [[skill-obsidian-vault-workflow]] — לעדכון Publishing Log.
- MCP servers (טרם הוגדרו) — אינטגרציות חיצוניות.

## הערות
תלות חיצונית: MCP integrations עם פלטפורמות הפרסום. עד שיוגדרו, הסוכן לא יוכל לפרסם בפועל אפילו אם הוגדר תפקידו.
