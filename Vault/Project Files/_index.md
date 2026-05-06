# Project Files — Index

תיעוד קובץ-אחר-קובץ של מבנה הפרויקט. כל ערך מסביר מה הקובץ עושה, למי הוא משויך (תפקיד/תחום בפרויקט), ולאילו קבצים הוא קשור.

## Topics

### Agents
- [[agent-sasson]] — Sasson, סוכן-מנכ"ל מתזמר, point of entry לכל בקשה (status: active).
- [[agent-sami-image-gen]] — Sami, סוכן קריאייטיב ויזואלי, יצירת תמונות עם עקביות סגנונית (status: active).
- [[agent-researcher]] — חקר ומקורות (status: stub).
- [[agent-writer]] — יצירת draft (status: stub).
- [[agent-editor]] — עריכה ו-brand voice (status: stub).
- [[agent-publisher]] — תזמון ופרסום (status: stub).

### Configuration & root
- [[root-claude-md]] — `CLAUDE.md` בשורש: הוראות פרויקט ל-Claude Code (תיאור, תקיות, מדיניות).
- [[root-env-files]] — `.env` ו-`.env.example`: מפתחות API ומשתני סביבה.
- [[root-gitignore]] — `.gitignore`: דפוסי קבצים שלא נכנסים ל-git.
- [[obsidian-config]] — תיקיית `.obsidian/`: הגדרות Obsidian (core plugins, appearance, sync).
- [[claude-settings]] — `.claude/settings.json`: הגדרות פלאגינים של Claude Code (Superpowers).
- [[claude-folder-structure]] — תיקיית `.claude/` כולה: agents/, commands/, skills/.

### Skills — Superpowers core
- [[skill-using-superpowers]] — שורש השימוש בכל ה-skills. נכפה בתחילת כל סשן.
- [[skill-brainstorming]] — חובה לפני כל יצירה: דיון בכוונות, דרישות ועיצוב.
- [[skill-writing-plans]] — כתיבת תוכניות יישום מפורטות.
- [[skill-executing-plans]] — ביצוע תוכניות עם checkpoints.
- [[skill-test-driven-development]] — כתיבת בדיקות לפני קוד (TDD).
- [[skill-systematic-debugging]] — איתור באגים שיטתי, מחפש שורש הבעיה.
- [[skill-verification-before-completion]] — חובה לוודא לפני הצהרת השלמה.
- [[skill-finishing-a-development-branch]] — סגירת ענף פיתוח (merge/PR/cleanup).

### Skills — Collaboration
- [[skill-using-git-worktrees]] — בידוד עבודה ב-git worktrees נפרדים.
- [[skill-dispatching-parallel-agents]] — הפעלת סוכנים מקבילים למשימות עצמאיות.
- [[skill-subagent-driven-development]] — פיתוח מונחה תת-סוכנים בתוך הסשן.
- [[skill-requesting-code-review]] — בקשת code review על שינויים.
- [[skill-receiving-code-review]] — קבלת ושילוב code review feedback.
- [[skill-writing-skills]] — יצירה ועריכה של skills חדשים.

### Skills — Obsidian (project-specific)
- [[skill-obsidian-vault-workflow]] — חובה: זרימת עבודה עם ה-Vault בכל משימה.
- [[skill-obsidian-markdown]] — תחביר Markdown של Obsidian (wikilinks, callouts, frontmatter).
- [[skill-obsidian-bases]] — קבצי `.base` של Obsidian (טבלאות/views דמויי-מסד-נתונים).

### Skills — Project-specific (custom)
- [[skill-gpt-image-gen]] — מעטפת טכנית ל-OpenAI Images API (gpt-image-2). משמש את [[agent-sami-image-gen]].
