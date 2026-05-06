---
name: researcher
description: STUB — not yet implemented. Will handle topic research, source gathering, and factual grounding for content sub-agents. CEO must NOT dispatch this agent until status changes to 'active'.
tools: Read
model: sonnet
status: stub
---

# Researcher — Stub

This sub-agent is a placeholder. The CEO orchestrator references it in its dispatch matrix but must refuse to dispatch and instead return a `blocking_questions` entry until this file is filled in with a complete role definition.

## Planned responsibilities

- חקר נושא לעומק לפני יצירת תוכן (writer / editor יבוססו על העבודה שלו).
- איסוף מקורות, ציטוטים ועובדות מאומתות.
- ביסוס עובדתי (fact-checking) של טענות לפני פרסום.
- מיפוי טרנדים, מתחרים ושיח קיים בנושא.

## Will integrate with

- [[sasson]] orchestrator — מקבל ממנו brief ממוקד, מחזיר ממצאים מובנים.
- Skills צפויים: `deep-research` (אם זמין), `dispatching-parallel-agents` (לחיפוש במקורות מרובים במקביל).
- כותב את הממצאים לתוך `Vault/Content Briefs/<topic>.md` (בקטע מובנה — Sources / Facts / Trends).

## Status transitions

- `stub` → `active` רק לאחר שהקובץ הזה ימולא בהגדרת תפקיד מלאה (system prompt, output contract, integration rules).
