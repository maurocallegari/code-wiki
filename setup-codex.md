# Setup Codex

> Configurazione completa di Codex CLI.

---

## Installazione

```bash
which codex
# /home/clawy/.local/bin/codex

codex --version
# codex-cli 0.146.1
```

---

## Auth

### OAuth (consigliato)

```bash
codex login
# Apre browser, effettua login con OpenAI
```

### API Key (alternativa)

```bash
export OPENAI_API_KEY="sk-..."
echo 'export OPENAI_API_KEY="sk-..."' >> ~/.bashrc
```

---

## Comandi Base

```bash
# One-shot
cd ~/dev/progetto
codex exec --sandbox workspace-write "Descrivi questo progetto"

# Con contesto
codex exec --context CODEX-CONTEXT.md "Converti file X"

# Background (task lunghi >5 min)
codex exec --sandbox workspace-write "Refactor modulo auth" --background
```

---

## Sandbox Modes

| Flag | Effetto | Quando usarla |
|------|---------|---------------|
| `--sandbox workspace-write` | Auto-approva modifiche nel workspace | **Consigliato** per la maggior parte dei task |
| `--dangerously-bypass-approvals-and-sandbox` | No sandbox, no approvals | Task che richiedono accesso host (pericoloso) |
| `--sandbox danger-full-access` | No sandbox, host access | Quando bubblewrap fallisce |

---

## Context File (`CODEX-CONTEXT.md`)

File che Codex legge all'inizio per capire il progetto:

```markdown
# CODEX-CONTEXT.md

## Setup
- Path: /path/to/project
- Stack: [linguaggi]

## Conventions
- Bootstrap: require_once...
- DB: plural tables, ID PK, IS_* flags
- Forms: $CRUD->Form(), ParametriForm

## Constraints
- Never touch *_old/
- Never modernize SQL
- Never translate domain names
```

### Come usarlo

```bash
# Copia nel progetto
cp /home/clawy/dev/code-wiki/CODEX-CONTEXT.md ./

# Adatta al progetto specifico
# Modifica sezioni 1, 4, 5, 6, 7

# Lancia Codex con context
codex exec --context CODEX-CONTEXT.md "Task qui"
```

---

## Verifica Funzionamento

```bash
cd /home/clawy/dev/insta
codex exec --sandbox workspace-write "Scrivi 1 riga: cosa fa questo progetto?" < /dev/null
```

Se risponde correttamente → OK.

---

## Prossimo

Vai a [Setup GStack](setup-gstack.md).
