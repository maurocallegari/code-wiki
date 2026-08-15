# Cheat Sheet

## Prompt universale

```text
FILE: [path esatto/lista chiusa]
TASK: [verbo + risultato osservabile]
INPUT: [comportamento attuale + reference reale]
OUTPUT: [comportamento/file attesi]
VINCOLI: [non toccare + convenzioni]
VERIFICA: [parser + diff + test funzionale]
```

## Scelta rapida

| Task | Strumento |
|---|---|
| patch, refactor circoscritto, batch | Codex |
| server, Telegram, cron, Kanban, deploy | Hermes |
| audit nuovo repo | Hermes coordina, Codex ispeziona/codifica |
| regole STH ricorrenti | skill `sth-conventions` |

## Prima

```bash
pwd
git status --short
rg --files | sed -n '1,120p'
find .. -name AGENTS.md -print
```

## Dopo ogni patch

```bash
git diff --name-only
git diff --stat
git diff --check
git diff

git diff --name-only -- '*.php' | while IFS= read -r f; do php -l "$f"; done
git diff --name-only -- '*.js' | while IFS= read -r f; do node --check "$f"; done
```

## Guardrail STH

```bash
# Output = STOP e review.
git diff --name-only | rg '(^|/)([^/]+_old|app|plugins)/|action/crud/functions\.php'

# Revisiona match Tab_*: sono trigger-owned, non scritture PHP.
rg -n 'Tab_[A-Za-z0-9_]+' --glob '*.php'

# Non usare questo come “pattern da eliminare”: mysql_* è uno shim valido.
rg -n 'mysql_' --glob '*.php'
```

## Convenzioni STH

```php
<?php
require_once($_SERVER['DOCUMENT_ROOT'].'/configure.php');
require_once($path_require.'/config.php');
require_once($path_require.'/functions.php');

$CRUD->Page(['TipoPagina'=>'lista', 'Title'=>'...', 'Header'=>[], 'Table'=>[]]);
$CRUD->Form(['Tabella'=>'agenti', 'Azione'=>'insert', 'Rows'=>[]]);

function Lab_Esempio($params) {
    // WHY: un solo array mantiene stabile il contratto della funzione.
}
```

| Tipo | Pattern |
|---|---|
| tabella | `plural_lowercase` |
| PK | `ID` |
| FK logica | `IDStudio`, `IDTestata` |
| flag | `IS_*` 0/1 |
| computed | `Tab_*` solo trigger |
| lookup | `tab_*` |
| child | `{parent}_righe` |

## Mobile

- viewport 320, 375, 414 e 768 px
- nessuno scroll orizzontale della pagina
- target touch almeno circa 44 × 44 CSS px
- link/button su una riga
- prova swipe e tap, non solo screenshot

## Deploy

```bash
# Preflight
git status --short && git diff --check

# Smoke test; aggiungi marker/versione, perché 200 non basta.
curl --fail --silent --show-error --location 'https://URL-REALE' -o /tmp/smoke.html
rg -n 'MARKER_ATTESO' /tmp/smoke.html
```

## “Fatto” significa

- scope corretto
- sintassi e diff check passati
- test funzionale passato
- live verificato se deployato
- card Kanban con prove

> [!DANGER] Mai segreti nei prompt, `configure.php`, wiki, Git o output condiviso. Mai `chmod 777` come cura dei permessi.
