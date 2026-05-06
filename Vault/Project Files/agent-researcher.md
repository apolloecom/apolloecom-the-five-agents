# agent-researcher

## מה הסוכן עושה
**מתוכנן:** חקר נושא, איסוף מקורות, ביסוס עובדתי (fact-checking), מיפוי טרנדים ושיח קיים. הבסיס שעליו [[agent-writer]] יבסס drafts.

## status
**stub** — הקובץ קיים כ-placeholder, ה-CEO לא יידפצ' אותו עד שיתמלא בהגדרת תפקיד מלאה.

## למי הוא משויך
- **תפקיד:** sub-agent של [[agent-ceo]].
- **מופעל ע"י:** ה-CEO בלבד (דרך Skill('subagent-driven-development') או Skill('dispatching-parallel-agents')).
- **קוראים:** עוד-לא-יישומי.

## מיקום
`/.claude/agents/researcher.md`

## קבצים קשורים
- [[agent-ceo]] — הסוכן הקורא לו.
- [[agent-writer]] — הצרכן הראשי של ממצאיו.
- [[skill-dispatching-parallel-agents]] — לחיפוש במקורות מרובים במקביל.

## הערות
ה-tools כרגע: `Read` בלבד (כי stub). כשהסוכן יהפוך ל-active, יקבל `WebSearch`, `WebFetch`, אולי `Bash` למקרה של APIs.
