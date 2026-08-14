# Codex Import Script

> Esegui questo script per importare il contesto di Mauro in qualsiasi progetto.
> In questo modo Codex sa già come lavorare senza doverglielo ripetere ogni volta.

## Come usare

```bash
# 1. Copia questo file nella root del progetto
cp /home/clawy/dev/code-wiki/CODEX-CONTEXT.md ./CODEX-CONTEXT.md

# 2. Quando lanci Codex, usa il context flag
cd /percorso/progetto
codex exec --context CODEX-CONTEXT.md "Converti view/agenti/agenti_lista.php da mysql_* a CRUD"

# 3. Per task multipli, tieni il context nel prompt
codex exec --context CODEX-CONTEXT.md --sandbox workspace-write "Spiega il progetto"
```

---

## Versione inline (per prompt senza --context)

Copia questo blocco all'inizio di ogni prompt Codex:

```
LEGGERE ATTAMENTE — STILE DI SVILUPPO:

1. Bootstrap: ogni pagina PHP inizia con:
   require_once($_SERVER['DOCUMENT_ROOT'].'/configure.php');
   require_once($path_require.'/config.php');
   require_once($path_require.'/functions.php');

2. Struttura view/<modulo>/ con: _lista.php, _scheda.php, _nuovo.php, _modifica.php, _elimina.php

3. CRUD pattern:
   $CRUD->Page(['TipoPagina'=>'lista', ...])
   $CRUD->Form(['Tabella'=>'...', 'Azione'=>'insert', 'Rows'=>[[...]]])

4. DB: tabelle plurali lowercase, PK ID, FK ID+Tabella, flag IS_* (0/1), Tab_* (trigger only)

5. MAI tradurre: rapportini, interventi, laboratorio, controlli, preventivi, studi, clienti, agenti, materiali, solleciti, pagamenti, contratti

6. MAI toccare: *_old/, action/crud/functions.php, app/, plugins/

7. mysql_* OK (shim-based), non modernizzare SQL

8. Verificare SEMPRE: php -l, grep pattern, git diff --stat

9. Vedere esempi esistenti prima di codificare
```

---

## Progetti esistenti

| Progetto | Path | Tipo | Context extra |
|----------|------|------|---------------|
| STH Assitec | dev/sth-assitec-gpt/ | Legacy PHP | AGENTS.md, PATTERNS.md, docs/convenzioni.md |
| Insta 2.0 | dev/insta/ | JS+PHP+Tailwind | README.md |
| Clawy Dashboard | public_html/ | PHP+JS | AGENTS.md |
| Trading Pack | public_html/demo/trading-pack/ | JS standalone | |
| STH 3C Donor | dev/sth-3c-donor/ | Legacy PHP | AGENTS.md |

---

## Workflow tipo per progetto esistente

```bash
# 1. Posizionati nel progetto
cd dev/sth-assitec-gpt  # o altro

# 2. Leggi il contesto
cat CODEX-CONTEXT.md
cat AGENTS.md  # se esiste

# 3. Esempi esistenti (guarda 2-3 file simili)
head -50 view/agenti/agenti_lista.php
head -50 view/clienti/clienti_lista.php

# 4. Prompt per Codex
codex exec --sandbox workspace-write "
  Converti view/agenti/agenti_elimina.php da mysql_* a T2 CRUD.
  Rispetta CODEX-CONTEXT.md.
  Riferimento: view/agenti/agenti_nuovo.php (già T2).
  Verifica: php -l, grep -c 'mysql_query' deve essere 0.
"

# 5. Verifica
php -l view/agenti/agenti_elimina.php
grep -c 'mysql_query' view/agenti/agenti_elimina.php
git diff --stat
```

---

## Per nuovi progetti

```bash
# 1. Crea cartella
mkdir -p ~/dev/nuovo-progetto && cd $_

# 2. Inizializza (tipo STH)
git init
cp /home/clawy/dev/code-wiki/CODEX-CONTEXT.md ./

# 3. Crea struttura base
mkdir -p require action/crud view includes design plugins

# 4. Prima pagina con contesto Codex
codex exec --context CODEX-CONTEXT.md --sandbox workspace-write "
  Crea configure.php root con session_start, gzip, costanti \$path_*.
  Rispetta CODEX-CONTEXT.md sezione 1, 8.
"
```

---

## Anti-pattern da evitare nel prompt

| ❌ SBAGLIATO | ✅ GIUSTO |
|---|---|
| "Converti tutto" | "Converti SOLO view/agenti/agenti_elimina.php" |
| "Usa PDO" | "Rispetta CODEX-CONTEXT.md — mysql_* OK" |
| "Rinomina $cliente a $customer" | "Rispetta domain naming (clienti, non customers)" |
| "Aggiungi prepared statements" | "Non modernizzare SQL (regola 9 CODEX-CONTEXT.md)" |
| "Elimina i file _old" | "MAI toccare *_old/ (regola 6)" |
