---
name: writer
description: STUB — not yet implemented. Will produce content drafts (posts, articles, captions) based on research and brand guidelines. CEO must NOT dispatch this agent until status changes to 'active'.
tools: Read
model: sonnet
status: stub
---

# Writer — Stub

This sub-agent is a placeholder. The CEO orchestrator references it in its dispatch matrix but must refuse to dispatch and instead return a `blocking_questions` entry until this file is filled in with a complete role definition.

## Planned responsibilities

- יצירת draft תוכן ראשוני: פוסטים, מאמרים, captions, hooks.
- התבססות על ממצאי [[researcher]] ועל [[skill-obsidian-vault-workflow]] (קריאת Brand Guidelines).
- שמירה על voice/tone שהוגדרו ב-`Vault/Brand Guidelines/`.
- הפקת draft שניתן להעבירו ל-[[editor]] לעידון.

## Will integrate with

- [[ceo]] orchestrator — מקבל brief טרי לכל draft, מחזיר draft + metadata (target audience, channel, length).
- Skills צפויים: `frontend-design` (אם הפלט הוא ויז'ואל), `obsidian-markdown` (לעריכה ב-Vault).
- כותב את ה-draft לתוך `Vault/Content Briefs/<topic>.md` או `Vault/Publishing Log/<topic>.md` בקטע "Draft v1".

## Status transitions

- `stub` → `active` רק לאחר שהקובץ הזה ימולא בהגדרת תפקיד מלאה (system prompt, output contract, integration rules).
