# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project: The Five Agents

צוות סוכני AI ליצירת תוכן, המנוהל על ידי סוכן ראשי (מנכ"ל) שמתאם ומנהל את צוות הסוכנים שלו. פרטי הסוכנים יעודכנו בהמשך.

## Claude Code Customization

תחת `.claude/` יושבים הגדרות מותאמות לפרויקט:

- `agents/` — הגדרות סוכנים מותאמים
- `skills/` — skills מותאמים
- `commands/` — slash commands מותאמים

## Mandatory Workflow — Obsidian Vault

**חובה:** בתחילת **כל** סשן או פקודה, ולפני נגיעה בקוד/תוכן/החלטה, יש להפעיל את ה-Skill `obsidian-vault-workflow` (Phase 1: לקרוא את קובץ הנושא הרלוונטי ב-`Vault/`, את 2-3 ה-Meeting Notes האחרונים, ובריפים/Brand Guidelines רלוונטיים). בסוף כל משימה — Phase 2: לאַפֵּנְד session log entry בתאריך ולעדכן Overview אם השתנה.

הסקיל מקור האמת: `.claude/skills/obsidian-vault-workflow/SKILL.md`. תיעוד קובץ-אחר-קובץ של הפרויקט נמצא ב-`Vault/Project Files/`.

## Mandatory Workflow — CEO Auto-Invocation

**חובה:** מיד אחרי `obsidian-vault-workflow` Phase 1, ולפני כל פעולה אחרת, קרא ל-`Agent(subagent_type='ceo')` עם בקשת המשתמש המקורית כפי שהיא.

ה-CEO יחזיר JSON output contract. Main-Claude חייב לכבד את `next_action`:
- `plan` → המתן לסיום פאזת התכנון של ה-CEO; הצג למשתמש לאישור לפני המשך.
- `dispatch` → בצע את הסקילים שה-CEO ביקש (parallel-agents / subagent-driven / executing-plans).
- `verify` → הרץ את פקודות ה-verification ש-CEO רשם והחזר את הפלט אליו.
- `done` → סכם למשתמש; אל תוסיף הצהרות הצלחה משלך.

אם `blocking_questions` לא ריק — העבר למשתמש כשאלות, אל תנחש.

מקור האמת: `.claude/agents/ceo.md`. סדר הפעולות לכל בקשה:
`obsidian-vault-workflow Phase 1` → `ceo` → executor skills → `verification-before-completion` → `obsidian-vault-workflow Phase 2`.

**Sub-agents קיימים** (כולם stub כרגע, פרט ל-`ceo`): `researcher`, `writer`, `editor`, `publisher`. ה-CEO יחזיר `blocking_questions` אם ידרש לדפצ' אותם לפני שישתנו ל-`status: active`.
