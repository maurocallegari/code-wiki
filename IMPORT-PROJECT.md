# Metodo: Importare una Codebase Esistente sotto AI

> Usa questa guida per ogni progetto esistente che vuoi gestire con Hermes e Codex.
> Obiettivo: l'AI conosce il tuo progetto, il tuo stile, le tue convenzioni — senza dovergliele ripetere.

---

## FASE 0: Prima di iniziare

Rispondi a queste domande per il progetto:

```
1. PATH: Dove sta il progetto? (es. ~/dev/mioprogetto)
2. TIPO: Che tipo di progetto è? (web app, API, CLI, mobile)
3. LINGUAGI: Che linguaggi usa? (PHP, JS, Python, CSS, SQL...)
4. DATABASE: Ha un DB? Dove? (MySQL, SQLite, nessuno)
5. STRUTTURA: Ha un framework o è vanilla?
6. GIT: È un repo git?
7. LIVE: Ha un URL live? (per verifiche)
```

Scrivi le risposte in `code-wiki/projects/<nome-progetto>/INFO.md`

---

## FASE 1: Audit automatico

Esegui questo script per analizzare la codebase:

```bash
#!/bin/bash
# save as: code-wiki/scripts/audit.sh
# usage: bash code-wiki/scripts/audit.sh /path/to/project

PROJECT="${1:-.}"
echo "=== AUDIT: $PROJECT ==="
echo ""
echo "--- Structure ---"
find "$PROJECT" -maxdepth 3 -type f | head -50
echo ""
echo "--- Languages ---"
find "$PROJECT" -type f | sed 's/.*\.//' | sort | uniq -c | sort -rn | head -10
echo ""
echo "--- Config files ---"
find "$PROJECT" -maxdepth 2 -name "config*" -o -name ".env*" -o -name "package.json" -o -name "composer.json" 2>/dev/null
echo ""
echo "--- Database refs ---"
grep -r -l "mysql\|mysqli\|PDO\|sqlite\|database" "$PROJECT" --include="*.php" --include="*.js" --include="*.py" 2>/dev/null | head -10
echo ""
echo "--- External services ---"
grep -r -l "curl\|file_get_contents\|fetch\|axios\|api\." "$PROJECT" --include="*.php" --include="*.js" 2>/dev/null | head -5
echo ""
echo "--- Git ---"
[ -d "$PROJECT/.git" ] && echo "GIT: yes" || echo "GIT: no"
echo ""
echo "--- Size ---"
du -sh "$PROJECT" 2>/dev/null
```

Risultato: sai cosa hai.

---

## FASE 2: Crea STYLE.md del progetto

Dall'audit, scrivi `STYLE.md` con:

### 2.1 Bootstrap/Entry pattern
```
Come si avvia il progetto? (index.php? app.js? main.py?)
```

### 2.2 Struttura file
```
Qual è la struttura delle cartelle? (view/, api/, src/, components/...)
```

### 2.3 Naming conventions
```
Tabelle DB: snake_case? camelCase?
Variabili: $camelCase? $snake_case?
Funzioni: VerboNome? verbo_nome?
Classi: PascalCase?
```

### 2.4 Database pattern
```
Come si accede al DB? (raw queries, ORM, wrapper?)
Quali tabelle esistono?
```

### 2.5 Code style
```
Commenti: IT o EN?
Lingua UI: IT o EN?
Framework JS: React, Vue, vanilla?
CSS: Tailwind, vanilla, SASS?
```

### 2.6 Domain terms
```
Quali termini di business NON devono essere tradotti?
(es. per uno store: prodotti, ordini, clienti, fornitori)
```

### 2.7 Anti-patterns
```
Cosa NON deve mai essere fatto?
(es. mai toccare X, mai usare Y, mai tradurre Z)
```

---

## FASE 3: Crea AGENTS.md del progetto

File: `/path/to/project/AGENTS.md`

```markdown
# AGENTS.md — [Nome Progetto]

## Setup
- **Path**: /path/to/project
- **Live URL**: https://... (se esiste)
- **DB**: [nome_db] su [host] (se esiste)
- **Stack**: [linguaggi]

## Avvio
```bash
[comando per avviare il progetto]
```

## Deploy
```bash
[comando per deployare]
```

## Verifica
```bash
[comando per testare che funzioni]
```

## Convenzioni
- [regola 1]
- [regola 2]
- [regola 3]

## Vincoli
- [cosa NON toccare]
- [cosa NON modernizzare]
- [cosa NON tradurre]
```

---

## FASE 4: Configura Codex

```bash
cd /path/to/project
cp /home/clawy/dev/code-wiki/CODEX-CONTEXT.md ./
```

Poi adatta `CODEX-CONTEXT.md` al progetto specifico:
- Sezione 1 → il pattern di avvio del progetto
- Sezione 4 → il pattern DB del progetto
- Sezione 5 → le regole di naming del progetto
- Sezione 6 → i termini dominio del progetto

---

## FASE 5: Test con Codex

Verifica che Codex abbia capito:

```bash
cd /path/to/project
codex exec --context CODEX-CONTEXT.md "Descrivi la struttura del progetto in 3 frasi"
```

Se la descrizione è corretta → OK. Se no → aggiungi dettagli a CODEX-CONTEXT.md.

Poi prova un micro-task:

```bash
codex exec --context CODEX-CONTEXT.md --sandbox workspace-write "Aggiungi commento header al file [file principale] con descrizione, data, autore"
```

Verifica:
- [ ] Il codice generato è nello stile del progetto?
- [ ] I nomi sono corretti?
- [ ] Il commento è nella lingua giusta?

---

## FASE 6: Configura Hermes

Per far sì Hermes sappia del progetto, due opzioni:

### Opzione A: AGENTS.md nel progetto
Già fatto (FASE 3). Hermes legge automaticamente l'AGENTS.md nel workdir.

### Opzione B: Skill dedicata (per progetti grandi)
Se il progetto è importante (es. STH, Insta), crea skill dedicata:

```bash
# In ~/.hermes/skills/<nome-progetto>/
# SKILL.md con contesto specifico
```

Vantaggio: disponibile in ogni sessione, anche da Telegram.

---

## FASE 7: Primo task reale

Il primo task deve essere piccolissimo. Esempi:
- Aggiungere un commento
- Correggere un typo
- Aggiungere un log
- Refactoring di una funzione semplice

Verifica che:
1. L'AI genera codice nel tuo stile
2. Il codice è corretto
3. I vincoli sono rispettati

Se il primo task fallisce → aggiungi il caso a `STYLE.md` come esempio negativo.

---

## Checklist riassuntiva

```markdown
Per ogni progetto nuovo da importare:

- [ ] FASE 0: Rispondo alle 7 domande iniziali
- [ ] FASE 1: Eseguo audit.sh per capire la struttura
- [ ] FASE 2: Scrivo STYLE.md con convenzioni del progetto
- [ ] FASE 3: Creo AGENTS.md con setup/deploy/verifica/vincoli
- [ ] FASE 4: Copio e adatto CODEX-CONTEXT.md nel progetto
- [ ] FASE 5: Testo con un task banale (commento, typo)
- [ ] FASE 6: Verifico che Hermes legga l'AGENTS.md
- [ ] FASE 7: Primo task reale, piccolo, verificato
- [ ] Aggiorno code-wiki/projects/<nome>/INFO.md con note
```

---

## Note finali

- Non saltare fasi. Anche se sembra lento, 10 minuti di setup risparmiamo ore di "no questo è sbagliato"
- Se un progetto è simile a uno già importato, copia lo STYLE.md e adatta
- Gli errori che fai con un progetto diventano regole per i progetti futuri
