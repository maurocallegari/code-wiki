# Tutorial · Batch T2 con Codex

Scenario reale dal brief: convertire pagine omogenee al pattern T2 usando un reference e verifiche. **T2 non significa modernizzare `mysql_*`**: lo shim legacy resta.

## 1. Scegli reference e candidati

```bash
cd /home/clawy/dev/sth-assitec-gpt

# WHY: usa un file T2 completo e verificato, non un frammento decontestualizzato.
reference='view/modulo_verificato/pagina_t2_verificata.php'
sed -n '1,260p' "$reference"

# WHY: crea il primo batch con soli tre file dello stesso tipo.
printf '%s\n' \
  'view/modulo_a/pagina.php' \
  'view/modulo_b/pagina.php' \
  'view/modulo_c/pagina.php' > /tmp/sth-t2-batch-1.txt
```

Sostituisci i placeholder con path confermati dal repo. Non usare i nomi come prova che i file esistano.

## 2. Fotografa la baseline

```bash
# WHY: registra scope iniziale e sintassi prima del batch.
git status --short
while IFS= read -r file; do php -l "$file" || exit 1; done < /tmp/sth-t2-batch-1.txt

# WHY: salva un elenco, non una copia sparsa .bak che può finire sul server.
git rev-parse HEAD
```

> [!WARNING] Ho provato backup `file.php.bak` accanto ai sorgenti: finiscono nei mirror FTP e riempiono disco. Usa Git o una directory temporanea fuori dal webroot.

## 3. Un prompt per il batch

```text
Read AGENTS.md, every target file in /tmp/sth-t2-batch-1.txt, and the complete
reference [path].

TASK: convert exactly these three homogeneous pages to the same T2 page/form
structure demonstrated by the reference, preserving behavior.
CONSTRAINTS:
- Do not modernize or replace mysql_*; it is an intentional shim.
- Preserve bootstrap order, Italian domain terms, queries and user-visible behavior.
- Tab_* values remain trigger-owned and must not be written by PHP.
- Do not touch files outside the list, *_old/, action/crud/functions.php, app/, plugins/.
- No shared abstraction or unrelated cleanup.
PROCESS: convert one file, run its checks, then continue. Stop on first failure.
VERIFY: php -l per file; git diff --check; required T2 markers from the reference;
forbidden legacy-layout markers absent only when the reference proves replacement.
Report a per-file matrix with command outputs.
```

## 4. Verifica meccanica

```bash
while IFS= read -r file; do
  php -l "$file" || exit 1
  # WHY: Page/Form sono richiesti solo secondo il tipo di reference scelto.
  rg -n '\$CRUD->(Page|Form)' "$file" || exit 1
done < /tmp/sth-t2-batch-1.txt

# WHY: qualsiasi file extra ferma il batch prima della review.
git diff --name-only
git diff --check

# WHY: mysql_* deve restare se il file lo usava; non usare “zero match” come obiettivo.
git diff | rg '^[+-].*mysql_' || true
```

## 5. Matrice di accettazione

| File | `php -l` | marker T2 | comportamento | diff review | Stato |
|---|---:|---:|---:|---:|---|
| target 1 | pass/fail | pass/fail | pass/fail | pass/fail | — |
| target 2 | pass/fail | pass/fail | pass/fail | pass/fail | — |
| target 3 | pass/fail | pass/fail | pass/fail | pass/fail | — |

Promuovi a 5–10 file solo quando tutti e tre passano. Se uno fallisce, correggi quella regola nel prompt/skill, ripristina solo quel file con una procedura concordata e ripeti su un nuovo batch.

> [!DANGER] Non lanciare una sostituzione globale su PHP legacy. Un match testuale uguale può avere significato diverso in lista, scheda e AJAX.
