# Metodo Completo — Il Sistema di Mauro

> Wiki personale per lavorare efficientemente con Hermes + Codex
> Stack: PHP/MySQL legacy, Insta 2.0, Dashboard, Trading
> Obiettivo: sostituire il coding tradizionale con AI-coding, mantenendo il mio stile

---

## Setup giornaliero

### Mattina (5 min)
1. `cronjob list` → vedo se ci sono task programmati
2. `hermes kanban list` → viero task in corso
3. Se nuovo task → applico il metodo

### Task in arrivo
1. Classifico: codice vs operazioni vs entrambi
2. Scrivo prompt con la formula
3. Verifico output con checklist
4. Deploy solo dopo verifica

---

## Strumenti configurati

### Hermes
- Skills: 44 totali (30 gstack + 14 custom)
- Cron: 8 job (backup, workspace, research, reddit, finance, idee, backup claw)
- Personalities: helpful, technical, concise

### Codex
- CLI: /home/clawy/.local/bin/codex
- Auth: OAuth OpenAI

### GitHub
- Token: disponibile in ~/.hermes/.env
- Repo: maurocallegari/code-wiki (privata), maurocallegari/clawy-dashboard

### GStack skills (30)
| Skill | Quando usarlo |
|-------|---------------|
| /office-hours | Nuova feature |
| /plan-ceo-review | Review idea |
| /review | Review codice |
| /qa | Test su URL |
| /ship | Deploy con test |
| /autoplan | Plan automatico |
| /careful | Prima di rm -rf, force-push |
| /investigate | Debug sistematico |
| /cso | Audit sicurezza OWASP |
| /freeze | Blocca edit a una dir |
| /retro | Review settimanale |

### Skills rimossi (11)
apple, data-science, email, hallmark, incremental-redesign, mlops, note-taking, preflight-scan, smart-home, yuanbao, osint-domain-scan

---

## Formule prompt

### Universale (Hermes, italiano)
```
FILE: [path esatto]
TASK: [cosa fare]
INPUT: [dati, esempio, contesto]
OUTPUT: [cosa mi aspetto]
VINCOLI: [cosa NON toccare]
VERIFICA: [comando per testare]
```

### Codice (Codex, inglese)
```
[FILE]: [path]
[TASK]: [action]
[CONSTRAINTS]: [limits]
[VERIFY WITH]: [command]
[REFERENCE]: [path to example file]
```

### Fix bug
```
BUG: [sintomo]
CAUSE: [ipotesi]
FIX: [modifica]
VERIFY: [test]
```

---

## Checklist post-AI (obbligatoria)

```bash
# 1. Sintassi
php -l file.php
node --check file.js

# 2. Pattern (STH)
grep -c '$CRUD->Page' file.php        # > 0
grep -c 'mysql_query' file.php        # = 0 (dopo conv)
grep -c '<table' file.php             # = 0

# 3. Pattern (Demo/Dashboard)
grep -c '__DATA__' file.html          # > 0
curl -s -o /dev/null -w "%{http_code}" URL  # 200

# 4. Scope modifiche
git diff --stat  # solo file attesi

# 5. Domain terms (STH)
grep -c 'rapportini\|interventi\|laboratorio' file.php  # > 0
```

---

## Deploy (sempre così)

```bash
# Bump versione
sed -i "s/APP_VERSION = '.*'/APP_VERSION = '$(date +%Y%m%d)b'/" index.php

# Deploy
mirror -R -p dir/ /dir/

# Verifica
curl -s -o /dev/null -w "%{http_code}" URL
```

---

## Progetti gestiti

| Progetto | Path | Tipo | Note |
|----------|------|------|------|
| STH Assitec | dev/sth-assitec-gpt/ | Legacy PHP | AGENTS.md, PATTERNS.md |
| Insta 2.0 | dev/insta/ | JS+PHP+Tailwind | AGENTS.md |
| Dashboard | public_html/ | PHP+JS | AGENTS.md |
| Trading Pack | public_html/demo/trading-pack/ | JS | |
| Code Wiki | dev/code-wiki/ | MD | Questa wiki |

---

## Regole d'oro

1. Non fidarti MAI del "fato" → verifica SEMPRE
2. Un task alla volta → batch max 5-10 file
3. Backup prima di batch grandi → cp -r dir dir.bak
4. Bump versione a ogni deploy → evita cache
5. Non toccare *_old/ → sono backup morti
6. Non modernizzare SQL → mysql_* OK
7. Non tradurre dominio → rapportini, interventi, laboratorio

---

## L'AI scrive codice, tu controlli

L'AI non sostituisce il tuo giudizio. Lo amplifica.
Tu decidi cosa fare, l'AI lo esegue, tu verifichai.
Se qualcosa non va → correggi tu, aggiungi la regole, ripeti.

---

## File correlati

- `STYLE.md` — Il mio stile (11 regole)
- `COURSE.md` — Corso pratico (tutorial)
- `CODEX-CONTEXT.md` — Contesto per Codex (esportabile)
- `IMPORT-PROJECT.md` — Come importare progetti
- `IMPORT.md` — Come usare Codex
- `01-hermes-vs-codex.md` — Quando usare quale
- `02-prompt-engineering.md` — Prompt efficaci
- `03-workflow-fix-bug.md` — Fix bug verificato
- `04-workflow-new-feature.md` — Feature dalla A alla Z
- `05-checklist-verifica.md` — Verifica output AI
- `06-codex-batch.md` — Batch conversione T2
- `07-problemi-comuni.md` — Errori che ho fatto
- `08-config-hermes.md` — Config Hermes
- `09-skills-utili.md` — Skills utili
- `10-cron-setup.md` — Cron jobs
- `11-deploy-ftp.md` — Deploy FTP
- `12-trading-finance.md` — Trading
