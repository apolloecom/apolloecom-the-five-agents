# agent-ceo

## מה הסוכן עושה
סוכן-המנכ"ל המתזמר של The Five Agents. point of entry לכל בקשת משתמש. מנתח, מסווג, מתכנן (brainstorm → write-plan), מדפצ' לסוכני-בן (subagent-driven או parallel-agents), אוכף checkpoints ב-git, ומפעיל verification-before-completion לפני כל הצהרת done. **לא** מבצע עריכות בעצמו (אין לו `Edit`/`Write` ב-`tools`).

## status
**active** — הסוכן הראשי הפעיל בפרויקט.

## למי הוא משויך
- **תפקיד:** orchestrator יחיד.
- **מופעל ע"י:** main-Claude (לפי הוראת `CLAUDE.md`).
- **מנהל:** [[agent-researcher]], [[agent-writer]], [[agent-editor]], [[agent-publisher]].

## מיקום
`/.claude/agents/ceo.md`

## מבנה הקובץ
- Frontmatter: `name`, `description`, `tools` (Read/Grep/Glob/Bash/Agent/TodoWrite/Skill/ExitPlanMode), `model: opus`.
- System prompt בן 15 סעיפים: Identity → Phase 0-8 → Memory Layers → Output Contract → Sub-Agents Registry → Anti-Patterns → Communication Style.

## קבצים קשורים
- [[root-claude-md]] — מחייב את הפעלת ה-CEO בכל בקשה (סעיף "Mandatory Workflow — CEO Auto-Invocation").
- [[skill-obsidian-vault-workflow]] — Phase 0 ו-Phase 8 של ה-CEO.
- [[skill-brainstorming]], [[skill-writing-plans]] — Phases 2-3.
- [[skill-dispatching-parallel-agents]], [[skill-subagent-driven-development]], [[skill-executing-plans]] — Phase 5 dispatch matrix.
- [[skill-verification-before-completion]] — Phase 7 (gate ל-done).
- [[agent-researcher]], [[agent-writer]], [[agent-editor]], [[agent-publisher]] — סוכני-בן (כולם stub כרגע).

## הערות
- ה-CEO לעולם לא יריץ `git reset` — אם נדרשת חזרה, מחזיר `blocking_questions` למשתמש.
- אם המשתמש מבקש משימה שדורשת sub-agent ב-`status: stub` — ה-CEO מחזיר `blocking_questions` ולא מדפצ'.
