# Setup Hermes

> Configurazione completa di Hermes per il tuo stack.

---

## Config (`~/.hermes/config.yaml`)

Sezioni critiche:

```yaml
model:
  default: tencent/hy3:free  # gratuito
  provider: nous

agent:
  max_turns: 150
  personalities:
    helpful: "..."
    technical: "..."
    concise: "..."
    # Rimuovi: kawaii, catgirl, pirate, shakespeare, uwu, philosopher, hype, noir, surfer

skills:
  creation_nudge_interval: 15

memory:
  memory_enabled: true
  user_profile_enabled: true
  memory_char_limit: 2200
  user_char_limit: 1375

delegation:
  max_iterations: 50
  provider: openai-codex
  model: gpt-5.6-sol

web:
  backend: ddgs  # DuckDuckGo (no API key)

tool_loop_guardrails:
  warnings_enabled: true
  hard_stop_enabled: false
  warn_after:
    exact_failure: 2
    same_tool_failure: 3
```

---

## Verifica Config

```bash
hermes doctor  # health check
hermes config show  # vedi config attuale
hermes config set agent.max_turns 150  # esempio modifica
```

---

## Verifica Skills

```bash
# Lista skills
ls ~/.hermes/skills/ | grep -v gstack | wc -l

# Skills gstack
ls ~/.hermes/skills/gstack* | wc -l
```

---

## Verifica Cron

```bash
cronjob list  # vedi tutti i job attivi
```

---

## Verifica Kanban

```bash
hermes kanban list  # vedi task attivi
```

---

## Prossimo

Vai a [Setup Codex](setup-codex.md).
