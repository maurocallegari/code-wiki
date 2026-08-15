# Wiki Brief — What It Must Be

---

## Name: Code Wiki
**For:** Mauro (Stealth Software, Treviso, Italy)
**Goal:** Become a master of AI-assisted development. Stop writing code by hand. Work with AI on both legacy repos and new imports, following my conventions and style.

---

## What It Is

A personal wiki that teaches me to use Hermes + Codex in my daily work. Not a generic reference. A practical hands-on tutorial:

- Tells me **which tool to use** for each task
- Gives me **the formula for writing effective prompts**
- Teaches me to **verify output** (never trust "done")
- Shows me **real examples** from my projects (STH, Insta, Dashboard)
- Reminds me **my conventions** (naming, patterns, style)
- Tells me **what skills, tools, and configurations I need** based on current trends
- Mentors me: shows me how to create my own skills with my conventions, step by step

---

## Stack

- PHP/MySQL legacy (STH Assitec: CRM/ERP for dental equipment maintenance)
- JavaScript vanilla + Tailwind CSS
- Insta 2.0: Instagram carousel generator
- Clawy Dashboard: operational dashboard on claw.nswr.it
- Trading tools: position-size, exit-calc, journal

## AI Tools

- **Hermes** (Raspberry + MacBook + Telegram) → orchestrator, deploy, complex fixes, cron
- **Codex** (CLI) → pure coding, batch refactoring

---

## Problems the Wiki Must Solve

1. AI says "done" but it doesn't work
2. AI modifies files it shouldn't
3. AI ignores constraints
4. AI does batches too large → times out, loses context
5. AI doesn't follow STH conventions
6. I don't know when to use Hermes vs Codex
7. I don't know how to write effective prompts
8. Disk on claw.nswr.it fills with useless files
9. FTP deploys fail due to permissions or cache
10. Kanban not used as daily task manager
11. I don't know what skills/tools/configs are current best practice

---

## My Style (MUST be respected ALWAYS)

### Bootstrap PHP
```php
require_once($_SERVER['DOCUMENT_ROOT'].'/configure.php');
require_once($path_require.'/config.php');
require_once($path_require.'/functions.php');
```

### DB Naming
- Tables: `plural_lowercase` (studi, agenti, materiali)
- PK: `ID` (auto-increment)
- FK: `ID`+TableName (IDStudio, IDTestata) — no physical FOREIGN KEY
- Flags: `IS_*` (0/1)
- Computed: `Tab_*` — ONLY via triggers, NEVER in PHP
- Lookup: `tab_*` (tab_stati_intervento)
- Child: `{parent}_righe` (righe_intervento)

### Domain (NEVER translate!)
rapportini, interventi, laboratorio, controlli, preventivi, studi, clienti, agenti, materiali, solleciti, pagamenti, contratti

### CRUD Pattern
```php
$CRUD->Page(['TipoPagina'=>'lista', 'Title'=>'...', 'Header'=>[...], 'Table'=>[...]]);
$CRUD->Form(['Tabella'=>'agenti', 'Azione'=>'insert', 'Rows'=>[[...]]]);
```

### Functions
- Single `$params` array
- Prefixes: `Lab_*`, `ATT_*`, `CTR_*`, `Solleciti_*`, `Get*`

### Anti-patterns (NEVER)
- Don't modernize SQL (`mysql_*` is fine, it's a shim)
- Don't touch `*_old/`, `action/crud/functions.php`, `app/`, `plugins/`
- Don't add secrets to configure.php

---

## What the Wiki Must Include

### Current Trends Section
The wiki must recommend what skills, tools, and configurations I should have based on current best practices. For example:
- Which Hermes skills are essential (and which to remove)
- Which Cron jobs should run
- What deploy workflow to use
- How to structure AGENTS.md for my repos
- What model/providers work best for my use case
- How to create my own skills with my conventions (step by step tutorial)

### Mentoring Approach
The wiki acts as my mentor:
- "To create a skill for your STH conventions, do this: [step by step]"
- "Your AGENTS.md should look like this: [template]"
- "Install these skills because: [reasoning]"
- "Configure Cron like this: [config] because: [reasoning]"

---

## Structure (navigable: sidebar, search, index)

1. **Start Here** — goal, philosophy, how to use
2. **The Method** — universal formula, workflow, golden rules
3. **My Style** — 11 rules (above)
4. **Hermes Setup** — config, personalities, skills, cron, kanban, deploy
5. **Codex Setup** — install, auth, commands, sandbox, context file
6. **Current Best Practices** — what skills/tools/configs I need (2026 trends)
7. **Creating Your Own Skills** — step by step tutorial for STH conventions
8. **Tutorial: Fix Bug** — real example STH (A-Z filter mobile)
9. **Tutorial: New Feature** — real example Insta (Neon visual style)
10. **Tutorial: Batch Conversion** — real example T2 with Codex (reference + verify)
11. **Importing New Projects** — checklist for repos with only CRUD/conventions
12. **Kanban** — managing tasks from chat (create, verify, complete)
13. **Deploy FTP** — verified workflow, permissions, BitNinja
14. **Common Problems** — 10 mistakes I made and how to avoid them
15. **Cheat Sheet** — quick commands, patterns, verifications

## Format

- Markdown with callouts (danger, warn, tip)
- Tables for decisions
- Code commented with "WHY"
- Real examples (not faked)
- Mermaid diagram for workflow

## Tone

- Direct, concrete, no theory
- "Do this because I tried X and it didn't work"
- Hands-on, not reference
- Teaches me to REPLACE traditional coding with AI, not supplement it
- Mentor mode: "Follow me, I'll show you how"

---

## Success Criteria

After reading and following this wiki:
- I can work on legacy repos (STH, Insta) without writing code by hand
- I can import any new repo (even with just basic CRUD) and bring it under AI control in <1 hour
- I know what skills/tools/configs are current best practice
- I can create my own skills with my conventions
- I verify everything (never trust "done")
- I use Kanban as daily task manager
- I deploy without failures
- I am a master of AI-assisted development
