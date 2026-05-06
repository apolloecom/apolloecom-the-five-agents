# Rename CEO Agent to Sasson — Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** להחליף את ה-identifier הטכני של הסוכן הראשי מ-`ceo` ל-`sasson` בכל מקום בפרויקט (קובץ הסוכן, frontmatter, הפניות ב-CLAUDE.md, ב-Vault/Project Files/, וב-Vault/Meeting Notes/), תוך שמירה על "CEO" / "מנכ"ל" כתיאור תפקיד בטקסט.

**Architecture:** Rename מתבצע בשלושה רבדים: (1) שם קובץ הסוכן `.claude/agents/ceo.md` → `sasson.md` ושדה `name:` ב-frontmatter; (2) כל ה-invocation references (`subagent_type='ceo'`, wikilinks `[[agent-ceo]]`, שם קובץ Vault `agent-ceo.md`); (3) הוספת פסקה אישית בתחילת system prompt שמזהה את הסוכן כ-Sasson. הביצוע נעשה ב-2 פאזות: פאזה A (פעולות נטולות-תלות שיכולות לרוץ במקביל) ופאזה B (עדכון Meeting Notes ו-Session Logs בסוף).

**Tech Stack:** Markdown files, YAML frontmatter, Obsidian wikilinks (`[[...]]`), git mv.

**Critical scope decision:**
- **משתנה:** identifier טכני (`ceo` כשם סוכן, `agent-ceo` כשם קובץ Vault, `name: ceo` ב-frontmatter, `subagent_type='ceo'` ב-CLAUDE.md, wikilinks `[[agent-ceo]]`).
- **לא משתנה:** "CEO" / "מנכ"ל" כתיאור תפקיד בטקסט (לדוגמה: "CEO — Orchestration & Command Agent" בכותרת system prompt, "סוכן-המנכ"ל" בתיאורים, "Mandatory Workflow — CEO Auto-Invocation" כותרת ב-CLAUDE.md). הסוכן יידע את עצמו כ-"Sasson, ה-CEO" — שני השמות יחיו זה לצד זה.

---

## File Inventory (full reference list)

### Files to rename (git mv)
1. `.claude/agents/ceo.md` → `.claude/agents/sasson.md`
2. `Vault/Project Files/agent-ceo.md` → `Vault/Project Files/agent-sasson.md`

### Files to modify (content edits)
3. `.claude/agents/sasson.md` — frontmatter `name: ceo` → `name: sasson`; הוספת פסקת זהות בתחילת system prompt; שינוי "אתה ה-CEO" → "אתה Sasson, ה-CEO" (שמירה על שני השמות).
4. `CLAUDE.md` — בשורה 25: `subagent_type='ceo'` → `subagent_type='sasson'`; בשורה 35: `.claude/agents/ceo.md` → `.claude/agents/sasson.md`.
5. `Vault/Project Files/_index.md` — שורה 8: `[[agent-ceo]]` → `[[agent-sasson]]` (ושינוי הטקסט לתאר את "סוכן Sasson — ה-CEO המתזמר").
6. `Vault/Project Files/agent-sasson.md` (אחרי rename) — כותרת `# agent-ceo` → `# agent-sasson`; שדה "מיקום" `/.claude/agents/ceo.md` → `/.claude/agents/sasson.md`; הטקסט הפנימי "סוכן-המנכ"ל" נשאר.
7. `Vault/Project Files/agent-researcher.md` — `[[agent-ceo]]` (2 הופעות) → `[[agent-sasson]]`.
8. `Vault/Project Files/agent-writer.md` — `[[agent-ceo]]` (2 הופעות) → `[[agent-sasson]]`.
9. `Vault/Project Files/agent-editor.md` — `[[agent-ceo]]` (2 הופעות) → `[[agent-sasson]]`.
10. `Vault/Project Files/agent-publisher.md` — `[[agent-ceo]]` (2 הופעות) → `[[agent-sasson]]`.
11. `.obsidian/workspace.json` — שורה 185: `"Vault/Project Files/agent-ceo.md"` → `"Vault/Project Files/agent-sasson.md"`.

### Files with CEO references that **stay as-is** (job title context)
- `CLAUDE.md` כותרת סעיף 23: `## Mandatory Workflow — CEO Auto-Invocation` — CEO הוא שם התפקיד, נשאר.
- `Vault/Plans/_index.md` שורה 3: "תוכניות יישום שה-CEO מייצר" — נשאר (תיאור תפקיד).
- `Vault/Project Files/claude-folder-structure.md` שורה 25: "ה-CEO + 5 סוכני התוכן" — נשאר.

### Files to update at the end (session logs)
12. `Vault/Meeting Notes/vault-initialization.md` — להחליף הופעות של `[[agent-ceo]]` ב-`[[agent-sasson]]` (שורות 4, 52). הטקסט "סוכן ה-CEO" וה-"Mandatory Workflow — CEO Auto-Invocation" נשאר כי זה תיעוד היסטורי ותיאור תפקיד.
13. `Vault/Project Files/_index.md` Session Log — append entry על השינוי.
14. `Vault/Meeting Notes/_index.md` — אם יש הפניה ל-CEO כ-identifier, להחליף.

### Verification commands
- `grep -rn "name: ceo" .claude/agents/` — must return 0 results
- `grep -rn "subagent_type='ceo'\|subagent_type=\"ceo\"" .` — must return 0 results
- `grep -rln "agent-ceo" Vault/` — must return 0 results
- `ls .claude/agents/ceo.md` — must fail (file not found)
- `ls .claude/agents/sasson.md` — must succeed
- `ls "Vault/Project Files/agent-ceo.md"` — must fail
- `ls "Vault/Project Files/agent-sasson.md"` — must succeed
- בדיקה ידנית: ה-CEO/Sasson יוזם בתחילת בקשה הבאה ומחזיר output contract תקני.

---

## Pre-flight: Checkpoint

- [ ] **Step 0.1: Confirm clean working tree (or commit current state)**

Run:
```bash
git status --porcelain
```

Expected: רק הקובץ הלא-tracked שכבר היה (`לללא כותרת.canvas`). אם יש שינויים אחרים tracked — לעשות checkpoint commit לפני המשך.

- [ ] **Step 0.2: Create pre-rename checkpoint**

Run:
```bash
git add -A && git commit -m "checkpoint: pre-rename ceo->sasson @ 2026-05-06" --allow-empty
git rev-parse HEAD
```

Expected: SHA חדש מודפס. שמור אותו — זהו `checkpoint_ref` שאליו ניתן לחזור אם משהו נשבר.

---

## Task 1: Rename agent file and update frontmatter

**Files:**
- Rename: `.claude/agents/ceo.md` → `.claude/agents/sasson.md`
- Modify: `.claude/agents/sasson.md` (frontmatter + identity paragraph)

- [ ] **Step 1.1: Rename the agent file via git**

Run:
```bash
git mv .claude/agents/ceo.md .claude/agents/sasson.md
```

Expected: ללא פלט. הקובץ זז ב-git history.

- [ ] **Step 1.2: Update frontmatter `name` field**

ב-`.claude/agents/sasson.md`, שורה 2:

מ:
```yaml
name: ceo
```

ל:
```yaml
name: sasson
```

- [ ] **Step 1.3: Update identity opening of system prompt**

ב-`.claude/agents/sasson.md`, שורה 8 (כותרת H1) — **נשארת**: `# CEO — Orchestration & Command Agent` (זו תיאור תפקיד).

שורה 12 (פתיחת סעיף Identity) — מ:
```
אתה ה-CEO של The Five Agents. תפקידך **מתזמר** בלבד: ...
```

ל:
```
אתה **Sasson**, ה-CEO של The Five Agents. תפקידך **מתזמר** בלבד: ...
```

(יתר הקובץ נשאר זהה — כל ההפניות הפנימיות "ה-CEO" בגוף הטקסט הן תיאור תפקיד ונשארות.)

- [ ] **Step 1.4: Verify rename and frontmatter**

Run:
```bash
ls -la .claude/agents/sasson.md && grep "^name:" .claude/agents/sasson.md
```

Expected:
```
-rw-r--r--  ... .claude/agents/sasson.md
name: sasson
```

- [ ] **Step 1.5: Verify ceo.md no longer exists**

Run:
```bash
ls .claude/agents/ceo.md 2>&1
```

Expected: `ls: .claude/agents/ceo.md: No such file or directory`

- [ ] **Step 1.6: Commit**

Run:
```bash
git add .claude/agents/sasson.md
git commit -m "refactor(agent): rename ceo -> sasson identifier (file + frontmatter)"
```

---

## Task 2: Update CLAUDE.md references

**Files:**
- Modify: `CLAUDE.md` (lines 25, 35)

- [ ] **Step 2.1: Update `subagent_type='ceo'` reference**

ב-`CLAUDE.md`, שורה 25:

מ:
```
**חובה:** מיד אחרי `obsidian-vault-workflow` Phase 1, ולפני כל פעולה אחרת, קרא ל-`Agent(subagent_type='ceo')` עם בקשת המשתמש המקורית כפי שהיא.
```

ל:
```
**חובה:** מיד אחרי `obsidian-vault-workflow` Phase 1, ולפני כל פעולה אחרת, קרא ל-`Agent(subagent_type='sasson')` עם בקשת המשתמש המקורית כפי שהיא.
```

- [ ] **Step 2.2: Update path reference**

ב-`CLAUDE.md`, שורה 35:

מ:
```
מקור האמת: `.claude/agents/ceo.md`. סדר הפעולות לכל בקשה:
```

ל:
```
מקור האמת: `.claude/agents/sasson.md`. סדר הפעולות לכל בקשה:
```

**הערות:**
- שורה 23 (`## Mandatory Workflow — CEO Auto-Invocation`) — **לא משתנה** (כותרת תיאור תפקיד).
- שורה 27 ("ה-CEO יחזיר JSON output contract...") — **לא משתנה** (תיאור תפקיד).
- שורה 36 (`obsidian-vault-workflow Phase 1` → `ceo` → executor skills) — **משתנה** ל-`sasson`.

- [ ] **Step 2.3: Update workflow chain reference**

ב-`CLAUDE.md`, שורה 36:

מ:
```
`obsidian-vault-workflow Phase 1` → `ceo` → executor skills → `verification-before-completion` → `obsidian-vault-workflow Phase 2`.
```

ל:
```
`obsidian-vault-workflow Phase 1` → `sasson` → executor skills → `verification-before-completion` → `obsidian-vault-workflow Phase 2`.
```

- [ ] **Step 2.4: Verify CLAUDE.md updates**

Run:
```bash
grep -n "ceo\|sasson" CLAUDE.md
```

Expected: כל ההופעות הן או "CEO" (תיאור תפקיד) או "sasson" (identifier). אסור שתופיע `ceo` קטנה כ-identifier (במיוחד בקונטקסט `subagent_type=` או נתיב קובץ).

- [ ] **Step 2.5: Commit**

Run:
```bash
git add CLAUDE.md
git commit -m "refactor(claude-md): update ceo->sasson identifier references"
```

---

## Task 3: Rename Vault Project File

**Files:**
- Rename: `Vault/Project Files/agent-ceo.md` → `Vault/Project Files/agent-sasson.md`
- Modify: `Vault/Project Files/agent-sasson.md` (title + path field)

- [ ] **Step 3.1: Rename via git**

Run:
```bash
git mv "Vault/Project Files/agent-ceo.md" "Vault/Project Files/agent-sasson.md"
```

Expected: ללא פלט.

- [ ] **Step 3.2: Update title (line 1)**

ב-`Vault/Project Files/agent-sasson.md`, שורה 1:

מ:
```
# agent-ceo
```

ל:
```
# agent-sasson
```

- [ ] **Step 3.3: Update path field (line 15)**

ב-`Vault/Project Files/agent-sasson.md`, שורה 15:

מ:
```
`/.claude/agents/ceo.md`
```

ל:
```
`/.claude/agents/sasson.md`
```

**הערה:** הטקסט בשורה 4 ("סוכן-המנכ"ל המתזמר") נשאר — תיאור תפקיד. ה-wikilinks `[[agent-researcher]]` וכו' בשורה 12 נשארים — הם הפניות לקבצים אחרים שלא משתנים.

- [ ] **Step 3.4: Verify file content**

Run:
```bash
head -20 "Vault/Project Files/agent-sasson.md"
```

Expected: שורה 1 = `# agent-sasson`, שורה 15 מכילה `/.claude/agents/sasson.md`.

- [ ] **Step 3.5: Commit**

Run:
```bash
git add "Vault/Project Files/agent-sasson.md"
git commit -m "refactor(vault): rename agent-ceo -> agent-sasson project file"
```

---

## Task 4: Update wikilinks in sub-agent project files

**Files:**
- Modify: `Vault/Project Files/agent-researcher.md` (lines 10, 18)
- Modify: `Vault/Project Files/agent-writer.md` (lines 10, 18)
- Modify: `Vault/Project Files/agent-editor.md` (lines 10, 18)
- Modify: `Vault/Project Files/agent-publisher.md` (lines 10, 18)

- [ ] **Step 4.1: Update agent-researcher.md**

ב-`Vault/Project Files/agent-researcher.md`:

שורה 10 — מ: `- **תפקיד:** sub-agent של [[agent-ceo]].`
ל: `- **תפקיד:** sub-agent של [[agent-sasson]].`

שורה 18 — מ: `- [[agent-ceo]] — הסוכן הקורא לו.`
ל: `- [[agent-sasson]] — הסוכן הקורא לו.`

- [ ] **Step 4.2: Update agent-writer.md**

ב-`Vault/Project Files/agent-writer.md`:

שורה 10 — מ: `- **תפקיד:** sub-agent של [[agent-ceo]].`
ל: `- **תפקיד:** sub-agent של [[agent-sasson]].`

שורה 18 — מ: `- [[agent-ceo]] — הסוכן הקורא לו.`
ל: `- [[agent-sasson]] — הסוכן הקורא לו.`

- [ ] **Step 4.3: Update agent-editor.md**

ב-`Vault/Project Files/agent-editor.md`:

שורה 10 — מ: `- **תפקיד:** sub-agent של [[agent-ceo]].`
ל: `- **תפקיד:** sub-agent של [[agent-sasson]].`

שורה 18 — מ: `- [[agent-ceo]] — הסוכן הקורא לו.`
ל: `- [[agent-sasson]] — הסוכן הקורא לו.`

- [ ] **Step 4.4: Update agent-publisher.md**

ב-`Vault/Project Files/agent-publisher.md`:

שורה 10 — מ: `- **תפקיד:** sub-agent של [[agent-ceo]].`
ל: `- **תפקיד:** sub-agent של [[agent-sasson]].`

שורה 18 — מ: `- [[agent-ceo]] — הסוכן הקורא לו.`
ל: `- [[agent-sasson]] — הסוכן הקורא לו.`

- [ ] **Step 4.5: Verify all 4 sub-agent files**

Run:
```bash
grep -l "agent-ceo" "Vault/Project Files/agent-researcher.md" "Vault/Project Files/agent-writer.md" "Vault/Project Files/agent-editor.md" "Vault/Project Files/agent-publisher.md" 2>&1
```

Expected: ללא פלט (אף קובץ לא מכיל `agent-ceo`).

```bash
grep -c "agent-sasson" "Vault/Project Files/agent-researcher.md" "Vault/Project Files/agent-writer.md" "Vault/Project Files/agent-editor.md" "Vault/Project Files/agent-publisher.md"
```

Expected: כל קובץ מציג `2` (שתי הופעות).

- [ ] **Step 4.6: Commit**

Run:
```bash
git add "Vault/Project Files/agent-researcher.md" "Vault/Project Files/agent-writer.md" "Vault/Project Files/agent-editor.md" "Vault/Project Files/agent-publisher.md"
git commit -m "refactor(vault): update sub-agent wikilinks ceo->sasson"
```

---

## Task 5: Update Vault Project Files index

**Files:**
- Modify: `Vault/Project Files/_index.md` (line 8)

- [ ] **Step 5.1: Update index entry**

ב-`Vault/Project Files/_index.md`, שורה 8:

מ:
```
- [[agent-ceo]] — סוכן-מנכ"ל מתזמר, point of entry לכל בקשה (status: active).
```

ל:
```
- [[agent-sasson]] — Sasson, סוכן-מנכ"ל מתזמר, point of entry לכל בקשה (status: active).
```

- [ ] **Step 5.2: Verify**

Run:
```bash
grep -n "agent-ceo\|agent-sasson" "Vault/Project Files/_index.md"
```

Expected: שורה 8 מכילה `[[agent-sasson]]`, אף הופעה של `agent-ceo`.

- [ ] **Step 5.3: Commit**

Run:
```bash
git add "Vault/Project Files/_index.md"
git commit -m "refactor(vault): update project files index ceo->sasson"
```

---

## Task 6: Update Obsidian workspace state

**Files:**
- Modify: `.obsidian/workspace.json` (line 185)

- [ ] **Step 6.1: Update workspace path**

ב-`.obsidian/workspace.json`, שורה 185:

מ:
```json
    "Vault/Project Files/agent-ceo.md",
```

ל:
```json
    "Vault/Project Files/agent-sasson.md",
```

- [ ] **Step 6.2: Verify**

Run:
```bash
grep -n "agent-ceo\|agent-sasson" .obsidian/workspace.json
```

Expected: שורה 185 מציגה `agent-sasson.md`, ללא הופעות של `agent-ceo`.

- [ ] **Step 6.3: Commit**

Run:
```bash
git add .obsidian/workspace.json
git commit -m "refactor(obsidian): update workspace path agent-ceo->agent-sasson"
```

---

## Task 7: Update Meeting Notes wikilinks

**Files:**
- Modify: `Vault/Meeting Notes/vault-initialization.md` (lines 4, 52)

- [ ] **Step 7.1: Update line 4 wikilink**

ב-`Vault/Meeting Notes/vault-initialization.md`, שורה 4:

מ:
```
... ב-2026-05-06 הוקם **סוכן ה-CEO** ([[agent-ceo]]) — point of entry יחיד לכל בקשה ...
```

ל:
```
... ב-2026-05-06 הוקם **סוכן ה-CEO Sasson** ([[agent-sasson]]) — point of entry יחיד לכל בקשה ...
```

(הטקסט "סוכן ה-CEO" נשאר כחלק מתיאור התפקיד; רק ה-wikilink משתנה והשם "Sasson" מתווסף.)

- [ ] **Step 7.2: Update line 38 reference**

ב-`Vault/Meeting Notes/vault-initialization.md`, שורה 38:

מ:
```
- יצירת 5 קבצי תיעוד ב-`Vault/Project Files/`: `agent-ceo.md` (status: active) + `agent-{researcher,writer,editor,publisher}.md` (status: stub).
```

ל:
```
- יצירת 5 קבצי תיעוד ב-`Vault/Project Files/`: `agent-sasson.md` (status: active) + `agent-{researcher,writer,editor,publisher}.md` (status: stub).
```

- [ ] **Step 7.3: Update line 52 wikilink**

ב-`Vault/Meeting Notes/vault-initialization.md`, שורה 52:

מ:
```
- **Related:** [[agent-ceo]], [[agent-researcher]], ...
```

ל:
```
- **Related:** [[agent-sasson]], [[agent-researcher]], ...
```

**הערה:** שורה 36 ("Mandatory Workflow — CEO Auto-Invocation") **לא משתנה** — זה ציטוט של כותרת ב-CLAUDE.md.

- [ ] **Step 7.4: Verify**

Run:
```bash
grep -n "agent-ceo\|agent-sasson" "Vault/Meeting Notes/vault-initialization.md"
```

Expected: כל ההופעות הן `agent-sasson`. אין `agent-ceo`.

- [ ] **Step 7.5: Commit**

Run:
```bash
git add "Vault/Meeting Notes/vault-initialization.md"
git commit -m "refactor(meeting-notes): update wikilinks ceo->sasson"
```

---

## Task 8: Final verification (full repo scan)

- [ ] **Step 8.1: Confirm zero `agent-ceo` references**

Run:
```bash
grep -rln "agent-ceo" . --exclude-dir=.git --exclude-dir=node_modules
```

Expected: אין פלט (exit code 1).

- [ ] **Step 8.2: Confirm zero `name: ceo` references**

Run:
```bash
grep -rn "name: ceo" .claude/ --exclude-dir=.git
```

Expected: אין פלט.

- [ ] **Step 8.3: Confirm zero `subagent_type='ceo'` references**

Run:
```bash
grep -rn "subagent_type='ceo'\|subagent_type=\"ceo\"" . --exclude-dir=.git --exclude-dir=node_modules
```

Expected: אין פלט.

- [ ] **Step 8.4: Confirm `sasson` is correctly placed**

Run:
```bash
grep -rn "name: sasson" .claude/agents/ && grep -rln "agent-sasson" Vault/
```

Expected:
- שורה אחת: `.claude/agents/sasson.md:2:name: sasson`
- 7 קבצים ב-Vault/ מכילים `agent-sasson` (sasson עצמו + 4 sub-agents + _index + meeting notes).

- [ ] **Step 8.5: Confirm file existence**

Run:
```bash
ls .claude/agents/sasson.md "Vault/Project Files/agent-sasson.md" 2>&1
```

Expected: שני הקבצים קיימים, ללא שגיאות.

- [ ] **Step 8.6: Confirm old files don't exist**

Run:
```bash
ls .claude/agents/ceo.md "Vault/Project Files/agent-ceo.md" 2>&1
```

Expected: שתי שגיאות `No such file or directory`.

- [ ] **Step 8.7: Confirm "CEO" job-title text remained intact**

Run:
```bash
grep -c "Mandatory Workflow — CEO Auto-Invocation" CLAUDE.md
grep -c "סוכן-המנכ" "Vault/Project Files/agent-sasson.md"
```

Expected: שני מספרים ≥ 1 (הטקסט הגנרי של תיאור תפקיד נשאר).

---

## Task 9: Vault Session Log update

**Files:**
- Modify: `Vault/Project Files/_index.md` (append session log entry — אם יש Session Log section; אם אין, יוצר אחד).
- Modify: `Vault/Meeting Notes/_index.md` (append entry).

- [ ] **Step 9.1: Append session log to Project Files _index**

הוסף בתחתית הקובץ:

```markdown

## Session Log

### 2026-05-06 — Rename CEO -> Sasson [active]
שינוי identifier טכני של הסוכן הראשי מ-`ceo` ל-`sasson` בכל הפרויקט. קובץ הסוכן עבר rename, frontmatter עודכן, וכל ה-wikilinks בקבצי Vault/Project Files/ ו-Vault/Meeting Notes/ עודכנו. תיאור התפקיד "CEO" / "מנכ"ל" נשמר בטקסט. הסוכן יודע כעת את עצמו כ-"Sasson, ה-CEO".
```

- [ ] **Step 9.2: Update Meeting Notes index**

הוסף ב-`Vault/Meeting Notes/_index.md` כניסה חדשה ל-rename event.

- [ ] **Step 9.3: Commit**

Run:
```bash
git add "Vault/Project Files/_index.md" "Vault/Meeting Notes/_index.md"
git commit -m "docs(vault): log ceo->sasson rename in session logs"
```

---

## Self-Review Checklist

✅ **Spec coverage:**
- [x] שם קובץ הסוכן הוחלף — Task 1.
- [x] frontmatter `name:` הוחלף — Task 1.
- [x] `subagent_type='ceo'` ב-CLAUDE.md הוחלף — Task 2.
- [x] נתיב `.claude/agents/ceo.md` ב-CLAUDE.md הוחלף — Task 2.
- [x] קובץ Vault `agent-ceo.md` הוחלף — Task 3.
- [x] wikilinks ב-4 sub-agents הוחלפו — Task 4.
- [x] wikilink ב-`Vault/Project Files/_index.md` הוחלף — Task 5.
- [x] `.obsidian/workspace.json` עודכן — Task 6.
- [x] wikilinks ב-Meeting Notes הוחלפו — Task 7.
- [x] verification מקיף — Task 8.
- [x] Session Log עודכן — Task 9.

✅ **Placeholder scan:** אין TBD/TODO; כל קוד-snippet שלם; כל פקודה כוללת expected output.

✅ **Type consistency:** כל ה-replacements עקביים: identifier `ceo` → `sasson` תמיד; `agent-ceo` → `agent-sasson` תמיד; "CEO" / "מנכ"ל" כתיאור תפקיד תמיד נשאר.

✅ **Edge cases:**
- כותרת `# CEO — Orchestration & Command Agent` ב-system prompt **לא** משתנה (תיאור תפקיד).
- הטקסט בתוך system prompt "אתה ה-CEO של The Five Agents" **משתנה** ל-"אתה Sasson, ה-CEO של The Five Agents" כי זו הזדהות אישית.
- שורה 36 ב-vault-initialization ("Mandatory Workflow — CEO Auto-Invocation") **לא** משתנה (ציטוט כותרת).
- `.obsidian/workspace.json` עודכן ידנית (לא Obsidian-rebuild) — כי השם בקובץ הוא reference ל-tab פתוח.

---

## Execution Notes

**Self-rename consideration:** הסוכן הזה (Sasson, לשעבר CEO) מתכנן rename של עצמו. ה-execution חייב להתבצע ע"י main-Claude (לא ע"י הסוכן עצמו), כי באמצע ה-execution קובץ הסוכן ישונה ושמו ישתנה — אם הסוכן יקרא לעצמו באמצע, הוא לא יימצא. סדר הפעולות הבטוח:
1. Main-Claude מקבל את התוכנית הזו.
2. Main-Claude מבצע את כל ה-tasks (1-9) בעצמו (ללא קריאה לסוכן sasson/ceo באמצע).
3. רק בסוף, אחרי commit אחרון, הבקשה הבאה תקרא ל-`subagent_type='sasson'` והסוכן החדש יענה.

**Rollback:** אם משהו נשבר, הסוכן (sasson/ceo) **לא** יריץ `git reset` בעצמו. במקום, יוצא `blocking_questions` עם ה-`checkpoint_ref` מ-Step 0.2, והמשתמש יחליט.
