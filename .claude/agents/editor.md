---
name: editor
description: STUB — not yet implemented. Will refine drafts for voice/tone, brand alignment, and editorial quality before publishing. CEO must NOT dispatch this agent until status changes to 'active'.
tools: Read
model: sonnet
status: stub
---

# Editor — Stub

This sub-agent is a placeholder. The CEO orchestrator references it in its dispatch matrix but must refuse to dispatch and instead return a `blocking_questions` entry until this file is filled in with a complete role definition.

## Planned responsibilities

- עריכה ועידון של draft שנוצר ע"י [[writer]] — voice, tone, dynamics.
- אכיפת Brand Guidelines (מתוך `Vault/Brand Guidelines/`).
- ניקוי שגיאות, חזרות, קלישאות.
- אישור לפרסום או חזרה ל-[[writer]] עם feedback ממוקד.

## Will integrate with

- [[ceo]] orchestrator — מקבל draft + brand guidelines, מחזיר edited version או feedback list.
- Skills צפויים: `obsidian-markdown` (לעריכה במקום), `verification-before-completion` (לוודא שכל הקריטריונים נבדקו).
- כותב לתוך אותו קובץ של [[writer]] בקטע "Edited v1" או יוצר feedback list לחזרה.

## Status transitions

- `stub` → `active` רק לאחר שהקובץ הזה ימולא בהגדרת תפקיד מלאה (system prompt, output contract, integration rules).
