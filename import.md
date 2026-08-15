# Importare un progetto in meno di un’ora

Obiettivo: non “capire tutto”, ma creare abbastanza guardrail perché Hermes e Codex possano fare il primo task senza inventare architettura.

## Minuti 0–10 · Inventario read-only

```bash
cd /path/assoluto/al/repo
git status --short

# WHY: rg è veloce e rispetta meglio il repo rispetto a scansioni indiscriminate.
rg --files -g '!vendor' -g '!node_modules' | sed -n '1,160p'
find .. -name AGENTS.md -print
rg -n "require_once|mysql_|mysqli|PDO|\$CRUD->|fetch\(" --glob '*.{php,js}' | sed -n '1,120p'
```

Rispondi: entrypoint, bootstrap, DB, deploy, test, directory generate, segreti, URL live. Non leggere `.env` nei prompt e non copiare dati cliente.

## Minuti 10–25 · Estrai convenzioni, non desideri

Trova un esempio corretto per lista, form, funzione e chiamata DB. Scrivi ciò che il repo **fa**, separato da ciò che vuoi migliorare.

| Evidenza | Regola da estrarre |
|---|---|
| stesso bootstrap in molte pagine | ordine e path esatti |
| CRUD ricorrente | chiavi realmente supportate |
| nomi DB | PK, FK logiche, flag, computed |
| directory storiche | zone vietate/generated/vendor |
| script deploy | webroot, versione/cache, rollback |

> [!WARNING] Ho provato a “ripulire mentre importavo”: l’agente ha scambiato legacy intenzionale per debito da eliminare. L’import è descrittivo; il refactor è un task futuro.

## Minuti 25–40 · Crea `AGENTS.md`

```markdown
# AGENTS.md — Nome progetto

## Scopo
[Una frase: cosa fa il prodotto e per chi]

## Avvio e verifica
- Directory: /path/assoluto
- Avvio: [comando confermato]
- Sintassi/test: [comandi confermati]
- Live: [URL senza credenziali]

## Prima di modificare
- Leggi il target completo e un reference analogo.
- Esegui git status --short e preserva modifiche preesistenti.
- Annuncia file in scope.

## Convenzioni osservate
- [bootstrap]
- [naming]
- [DB/CRUD]
- [termini dominio]

## Vietato
- [directory generate, vendor, backup, core condiviso]
- niente segreti o refactor fuori task

## Definition of Done
- [parser/linter]
- git diff --check e review
- [test funzionale]
```

Per STH usa direttamente le regole di [stile.md](stile.md).

## Minuti 40–50 · Crea una skill solo se ricorre

Se le convenzioni sono specifiche e ricorrenti, crea una skill seguendo [Creating Your Own Skills](creating-skills.md). Se è un repo piccolo con un solo task, basta `AGENTS.md`: evitare duplicazione è una best practice.

## Minuti 50–60 · Prova controllata

```text
Read AGENTS.md and inspect the repository without modifying it.
Explain the bootstrap, one representative data flow, protected paths, and the
exact verification commands. Cite file paths and line numbers. Mark unknowns;
do not guess. Then propose a one-file, reversible first task. Do not edit.
```

Confronta la risposta con i file. Poi assegna un fix minuscolo e verifica:

```bash
git diff --name-only
git diff --check
# esegui parser/test confermati in AGENTS.md
```

## Gate di import riuscito

- [ ] l’agente cita entrypoint e reference reali
- [ ] sa cosa non deve toccare
- [ ] non propone di modernizzare il legacy senza richiesta
- [ ] comandi di verifica esistono e passano sulla baseline
- [ ] segreti esclusi
- [ ] primo task modifica solo lo scope autorizzato
