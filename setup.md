# Setup Strumenti

> Configurazione completa di Hermes, Codex, GStack, gbrain.

---

## Hermes

### Config (`~/.hermes/config.yaml`)

```yaml
model:
  default: tencent/hy3:free
  provider: nous
agent:
  max_turns: 150
  personalities:
    helpful: ...
    technical: ...
    concise: ...
skills:
  creation_nudge_interval: 15
memory:
  memory_enabled: true
  user_profile_enabled: true
delegation:
  max_iterations: 50
  provider: openai-codex
  model: gpt-5.6-sol
```

### Personalities

<div class="callout callout-warn">

**Usa solo 3:** helpful, technical, concise. Rimuovi le altre (kawaii, catgirl, pirate, shakespeare, uwu, philosopher, hype, noir, surfer).

</div>

### Skills Installate (44)

| Categoria | Skills |
|-----------|--------|
| Dev/Ops | devops, clawy-dashboard-deploy, clawy-dashboard-feeds, clawy-workspace-sync |
| Code | github, software-development, stealth-crud |
| AI | hermes-agent, codex, autonomous-ai-agents |
| Productivity | productivity, recap-generator, teams-meeting-pipeline |
| Research | research, grounded-citations, polymarket, polymarket-arbitrage |
| Media | media, gif-search, youtube-content |
| Creative | creative, p5js, ascii-art, excalidraw |
| GStack | 30 skills (gstack-*) |

### Cron Jobs (8)

| Job | Frequenza | Cosa fa |
|-----|-----------|---------|
| hermes-backup | settimanale | Backup Hermes state |
| clawy-workspace-sync | 3x/giorno | Sync dashboard |
| clawy-weekly-research | settimanale | Ricerca utile |
| clawy-reddit-scan | 2x/giorno | Aggiorna Reddit |
| Clawy Reddit sync | 3x/giorno | Commenti proattivi |
| Clawy Finance sync | 2x/giorno | Verdetto mercati |
| Clawy Idee proattive | 2x/giorno | 10 idee nuove |
| Claw backup giornaliero | giornaliero | Backup completo |

---

## Codex

### Installazione

```bash
# CLI
which codex  # /home/clawy/.local/bin/codex
codex --version  # 0.146.1
```

### Auth

```bash
# OAuth (consigliato)
codex login

# Oppure API key
export OPENAI_API_KEY="sk-..."
```

### Comandi Base

```bash
# One-shot
cd ~/dev/progetto
codex exec --sandbox workspace-write "Aggiungi commento header a index.php"

# Background (task lunghi)
codex exec --sandbox workspace-write "Refactor modulo auth" --background

# Con contesto
codex exec --context CODEX-CONTEXT.md "Converti file X in T2"
```

### Sandbox Modes

| Flag | Effetto |
|------|---------|
| `--sandbox workspace-write` | Auto-approva modifiche nel workspace (consigliato) |
| `--dangerously-bypass-approvals-and-sandbox` | No sandbox, no approvals (pericoloso) |
| `--sandbox danger-full-access` | No sandbox, host access (quando bubblewrap fallisce) |

---

## GStack

30 skills installate in `~/.hermes/skills/gstack-*/`.

### Skills più utili per te

| Skill | Quando usarlo |
|-------|---------------|
| `/office-hours` | Quando parti una nuova feature |
| `/plan-ceo-review` | Review di un'idea |
| `/review` | Review codice su un branch |
| `/qa` | Test su URL di staging |
| `/ship` | Deploy con test |
| `/autoplan` | Plan automatico CEO→design→eng |
| `/careful` | Prima di `rm -rf`, `force-push` |
| `/investigate` | Debug sistematico |
| `/freeze` | Blocca edit a una directory |
| `/retro` | Review settimanale |

---

## gbrain

### Setup

```bash
# Installazione (già fatta)
export PATH="$HOME/.bun/bin:$PATH"
bun install -g github:garrytan/gbrain

# Inizializzazione (locale, no Docker)
gbrain init --pglite
```

### Comandi

```bash
gbrain import ~/notes/      # Indicizza markdown
gbrain query "temi ricorrenti"  # Ricerca semantica
gbrain remember "preferenza"   # Salva memoria
gbrain recall --entity people/me  # Ricorda
gbrain sync --strategy code    # Re-indicizza repo
```

### MCP Server

```bash
# Registra come MCP per Hermes
gbrain serve
# Poi: hermes mcp add gbrain -- gbrain serve
```

---

## Prossimo Passo

Vai a [Il Tuo Stile](stile.md) per capire come l'AI scrive il tuo codice.
