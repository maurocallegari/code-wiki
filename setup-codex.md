# Setup Codex

Codex è il banco da lavoro del codice: aprilo nel repository, dagli confini precisi, lascia che ispezioni prima di modificare e verifica il diff.

## Installazione e autenticazione

Le opzioni CLI sono versionate: usa la documentazione della versione installata, non flag ricordati da un vecchio appunto.

```bash
# WHY: conferma il binario effettivamente usato e la sua versione.
command -v codex
codex --version
codex --help

# WHY: il login interattivo evita di scrivere token nei prompt o nella shell history.
codex login
```

## Apri dal repo, non dalla home

```bash
# WHY: il workspace determina i file leggibili/scrivibili e rende il diff pertinente.
cd /home/clawy/dev/sth-assitec-gpt
git status --short
codex
```

> [!DANGER] Non avviare con permessi pieni per comodità. Per un audit usa sola lettura; per una patch usa `workspace-write`; rete e comandi distruttivi restano soggetti ad approvazione.

## `AGENTS.md` per STH

```markdown
# AGENTS.md

## Missione
Lavora su STH Assitec preservando il comportamento legacy e le convenzioni locali.

## Prima di modificare
- Leggi il file target e un reference corretto dello stesso modulo.
- Esegui `git status --short`; non sovrascrivere modifiche preesistenti.
- Elenca i file che intendi cambiare.

## Convenzioni obbligatorie
- Bootstrap: i tre require_once definiti in stile.md, nello stesso ordine.
- Tabelle plural_lowercase; PK ID; FK logiche ID+Tabella; flag IS_*.
- Tab_* soltanto via trigger; termini di dominio in italiano.
- Funzioni con un solo array $params e prefissi Lab_*, ATT_*, CTR_*, Solleciti_*, Get*.
- Segui $CRUD->Page() e $CRUD->Form() da un reference reale.

## Vietato
- Non modernizzare mysql_*: è uno shim.
- Non toccare *_old/, action/crud/functions.php, app/, plugins/.
- Non inserire segreti in configure.php.
- Non fare refactor fuori task.

## Verifica
- `php -l` su ogni PHP modificato.
- `git diff --check`, `git diff --stat`, review del diff completo.
- Test funzionale descritto dal task.
```

## Prompt da dare a Codex

```text
Read AGENTS.md and the complete target file first.
Target: view/agenti/agenti_lista.php
Reference: [path to a verified, analogous STH file]
Change: [one observable result].
Do not touch any other file and do not refactor unrelated code.
Preserve mysql_* and all Italian domain terms.
Verify with: php -l ..., git diff --check, and [functional check].
Report exact commands and outputs; do not claim the browser test was run unless it was.
```

## Igiene del contesto

- una sessione per un obiettivo
- reference mirato, non l’intero repository
- batch piccoli e omogenei
- nuova sessione se il piano cambia sostanzialmente
- riassunto nel task Kanban, non affidarti alla memoria della chat

Ho provato a continuare conversazioni lunghe con “come prima”: dopo la compressione sparivano proprio i vincoli importanti. Ripeti nel prompt file, risultato, divieti e verifica.

## Verifica minima

```bash
# WHY: nessun errore sintattico nei file toccati.
git diff --name-only -- '*.php' | while IFS= read -r f; do php -l "$f"; done

# WHY: fallisce su whitespace e conflict marker lasciati dalla patch.
git diff --check

# WHY: confronta lo scope reale con quello autorizzato.
git status --short
git diff --stat
git diff
```
