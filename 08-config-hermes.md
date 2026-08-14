# Config Hermes Ottimizzata

## Cosa hai (funziona)
- 28 skills installate (troppe, ma OK)
- 8 cron jobs attivi
- max_turns: 150
- busy_input_mode: queue
- compression: attiva

## Problemi che ho trovato

### 1. Personalities: 15 è troppo
Configurati: helpful, concise, technical, creative, teacher, kawaii, catgirl, pirate, shakespeare, surfer, noir, uwu, philosopher, hype.

**Realtà:** usi sempre `helpful` o `technical`. Gli altri sono distrazioni.

**Consiglio:** lascia solo:
```yaml
agent:
  personalities:
    helpful: ...
    technical: ...
    concise: ...
```

### 2. Providers: troppi modelli custom
Hai 100+ modelli custom su OpenRouter. Pochi li usi davvero.

**Consiglio:** lascia solo quelli che usi:
```yaml
model:
  default: tencent/hy3:free
  fallback: gpt-5.6-sol
```

### 3. Skills: pulizia
Skills installate ma non usate (es. `apple`, `data-science`, `smart-home`, `email`).

**Consiglio:** rimuovi quelle che non ti servono, tieni:
- devops, github, software-design, stealth-crud, clawy-dashboard-deploy
- productivity, research, media, creative (per Insta)

### 4. Cron jobs: duplicati
Hai "clawy-reddit-scan" e "Clawy Reddit sync" che fanno la stessa cosa.

**Consiglio:** unifica.

---

## Config ottimizzata consigliata

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
web:
  backend: ddgs
```

---

## Skills utili (quelle che usi davvero)

### Dev/Ops
- `devops` (claw-ftp-deploy, verified-ftp-deploy)
- `github` (github-pr-workflow, github-code-review)
- `software-design` (codebase-design, systematic-debugging)
- `stealth-crud` (per i tuoi gestionali PHP)

### Productivity
- `productivity` (recap-generator, teams-meeting-pipeline)
- `research` (grounded-citations, polymarket)

### Media
- `media` (gif-search, youtube-content)
- `creative` (p5js, ascii-art, excalidaw)

### Clawy specifiche
- `clawy-dashboard-deploy`
- `clawy-dashboard-feeds`
- `clawy-workspace-sync`
