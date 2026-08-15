# Importare Progetti

> Checklist per portare qualsiasi codebase esistente sotto Hermes + Codex.

---

## FASE 0: Domande Iniziali

```
1. PATH: Dove sta il progetto?
2. TIPO: Che tipo è? (web app, API, CLI)
3. LINGUAGI: Che linguaggi usa?
4. DATABASE: Ha un DB? Dove?
5. STRUTTURA: Framework o vanilla?
6. GIT: È un repo git?
7. LIVE: Ha un URL live?
```

---

## FASE 1: Audit

```bash
# Struttura
find /path/to/project -maxdepth 3 -type f | head -50

# Linguaggi
find /path/to/project -type f | sed 's/.*\.//' | sort | uniq -c | sort -rn | head -10

# Config
find /path/to/project -maxdepth 2 -name "config*" -o -name ".env*"

# Database refs
grep -r -l "mysql\|mysqli\|PDO" /path/to/project --include="*.php" 2>/dev/null | head -10
```

---

## FASE 2: Crea STILE.md

Dall'audit, scrivi le convenzioni del progetto:

```markdown
# STILE.md — [Nome Progetto]

## Bootstrap
[Come si avvia il progetto]

## Struttura
[Struttura cartelle]

## Naming
[Convenzioni nomi]

## Database
[Pattern DB]

## Code Style
[Stile codice]

## Domain Terms
[Termini da NON tradurre]

## Anti-Pattern
[Cosa NON fare]
```

---

## FASE 3: Crea AGENTS.md

```markdown
# AGENTS.md — [Nome Progetto]

## Setup
- Path: /path/to/project
- Live: https://...
- Stack: [linguaggi]

## Avvio
[comando]

## Deploy
[comando]

## Verifica
[comando]

## Convenzioni
[regole]

## Vincoli
[cosa NON toccare]
```

---

## FASE 4: Codex Context

```bash
cp /home/clawy/dev/code-wiki/CODEX-CONTEXT.md /path/to/project/
# Adatta CODEX-CONTEXT.md al progetto specifico
```

---

## FASE 5: Test

```bash
cd /path/to/project
codex exec --context CODEX-CONTEXT.md "Descrivi questo progetto in 3 frasi"
```

Se descrizione corretta → OK.

---

## Checklist Riassuntiva

- [ ] FASE 0: Risposto alle 7 domande
- [ ] FASE 1: Eseguito audit
- [ ] FASE 2: Scritto STILE.md
- [ ] FASE 3: Creato AGENTS.md
- [ ] FASE 4: Copiato CODEX-CONTEXT.md
- [ ] FASE 5: Testato con Codex
