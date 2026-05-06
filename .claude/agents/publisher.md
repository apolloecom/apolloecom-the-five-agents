---
name: publisher
description: STUB — not yet implemented. Will schedule and publish approved content to channels (Instagram, Twitter, Substack, etc.) via MCP integrations. CEO must NOT dispatch this agent until status changes to 'active'.
tools: Read
model: sonnet
status: stub
---

# Publisher — Stub

This sub-agent is a placeholder. The CEO orchestrator references it in its dispatch matrix but must refuse to dispatch and instead return a `blocking_questions` entry until this file is filled in with a complete role definition.

## Planned responsibilities

- תזמון ופרסום של תוכן מאושר ל-channels (Instagram, Twitter, Substack, LinkedIn, YouTube וכו').
- אינטגרציה עם MCP servers של הפלטפורמות (טרם הוגדרו).
- עדכון `Vault/Publishing Log/` עם רשומה לכל פרסום (זמן, channel, performance metrics לאחר פרסום).
- post-mortems אחרי פרסומים: מה עבד, מה לא, מה לעדכן ב-Brand Guidelines.

## Will integrate with

- [[sasson]] orchestrator — מקבל תוכן מאושר + מטא-דאטה (channel, scheduled time), מחזיר אישור פרסום או שגיאה.
- Skills צפויים: `schedule` (לתזמון), `obsidian-vault-workflow` (לעדכון Publishing Log).
- MCP servers: יוגדרו לפי הפלטפורמות הנתמכות.
- כותב לתוך `Vault/Publishing Log/<channel>-<date>.md`.

## Status transitions

- `stub` → `active` רק לאחר שהקובץ הזה ימולא בהגדרת תפקיד מלאה, וגם לאחר שהוגדרו MCP integrations לפלטפורמות הנתמכות.
