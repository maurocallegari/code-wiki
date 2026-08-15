# Cheat Sheet

> Riferimento rapido per comandi e pattern.

---

## Comandi Hermes

```bash
# Status
hermes doctor
hermes config show

# Cron
cronjob list
cronjob action=run job_id=<id>

# Kanban
hermes kanban list
hermes kanban create "Titolo" --body "Desc"
hermes kanban promote <id>
hermes kanban complete <id>
```

---

## Comandi Codex

```bash
# One-shot
codex exec --sandbox workspace-write "Prompt"

# Con context
codex exec --context CODEX-CONTEXT.md "Prompt"

# Background
codex exec --sandbox workspace-write "Prompt" --background
```

---

## Comandi gbrain

```bash
gbrain init --pglite
gbrain import ~/notes/
gbrain query "temi ricorrenti"
gbrain sync --strategy code
gbrain serve
```

---

## Verifica Codice

```bash
# Sintassi
php -l file.php
node --check file.js

# Pattern STH (dopo conversione T2)
grep -c '$CRUD->Page' file.php        # > 0
grep -c 'mysql_query' file.php        # = 0
grep -c '<table' file.php             # = 0
grep -c '<div class="content-page"' file.php  # = 0

# Pattern Demo/Dashboard
grep -c '__DATA__' file.html          # > 0

# Funzionale
curl -s -o /dev/null -w "%{http_code}" URL  # 200

# Scope modifiche
git diff --stat
```

---

## Deploy

```bash
# Bump versione
sed -i "s/APP_VERSION = '.*'/APP_VERSION = '$(date +%Y%m%d)b'/" index.php

# Deploy
mirror -R -p public_html/demo/ /demo/

# Verifica
curl -s -o /dev/null -w "%{http_code}" URL
```

---

## Pattern Prompt

### Universale (Hermes)
```
FILE: [path]
TASK: [cosa fare]
INPUT: [dati]
OUTPUT: [cosa mi aspetto]
VINCOLI: [cosa NON toccare]
VERIFICA: [comando per testare]
```

### Codice (Codex)
```
[FILE]: [path]
[TASK]: [action]
[CONSTRAINTS]: [limits]
[VERIFY WITH]: [command]
[REFERENCE]: [path to example]
```

---

## GStack Skills Utili

| Skill | Quando |
|-------|--------|
| /office-hours | Nuova feature |
| /review | Review codice |
| /qa | Test su URL |
| /ship | Deploy con test |
| /careful | Prima di rm -rf |
| /investigate | Debug sistematico |
| /cso | Audit sicurezza |

---

## Config Paths

```
~/.hermes/config.yaml
~/.hermes/.env (secrets)
~/.hermes/skills/
~/.hermes/cron/
/home/clawy/dev/sth-assitec-gpt/ (progetto STH)
/home/clawy/dev/insta/ (progetto Insta)
/home/clawy/public_html/ (dashboard)
```

---

## Contatti

- GitHub: maurocallegari
- Email: clawy@stealthsoftware.it
