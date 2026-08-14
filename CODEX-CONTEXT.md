# Codex Context — Mauro's Development Style

> In questo file: tutto il necessario per lavorare sui progetti di Mauro seguendo il suo stile.
> Stack: PHP/MySQL legacy, JavaScript, Tailwind, PHP/MySQL STH (gestionale), Insta 2.0, dashboard

---

## Obiettivo

Lavorare sul codice come farebbe Mauro: stesso stile, stesse convenzioni, stessi pattern.
Non inventare. Seguire le regole esistenti. Chiedere se incerto.

---

## 1. Bootstrap Pattern (obbligatorio)

Ogni pagina PHP inizia così:

```php
<?php
require_once($_SERVER['DOCUMENT_ROOT'].'/configure.php');
require_once($path_require.'/config.php');
require_once($path_require.'/functions.php');
```

Mai cambiare questa struttura.

---

## 2. Struttura file progetto

### STH Assitec (PHP legacy)

```
<progetto>/
├── configure.php           # root: session, gzip, $path_*, encryption keys
├── require/
│   ├── config.php          # login gate, DB connect, perms
│   ├── functions.php       # general functions (~2900 lines)
│   ├── functions_aggiuntive.php  # business/AJAX (~3200 lines)
│   ├── functions_crud.php  # CRUD utilities
│   ├── layout_crud.php     # Form + CrudUtility classes
│   └── head_crud.php       # JS client CRUD hub
├── action/
│   ├── crud/               # generic save/delete engines
│   └── <modulo>/           # thin action scripts
├── view/
│   └── <modulo>/
│       ├── <modulo>_lista.php
│       ├── <modulo>_scheda.php
│       ├── <modulo>_nuovo.php
│       ├── <modulo>_modifica.php
│       ├── <modulo>_elimina.php
│       └── *.json.php      # tablesorter feeds
├── includes/
│   ├── header.php
│   └── footer.php
├── design/                 # static/theme
└── plugins/                # vendored libs (non toccare)
```

### Insta 2.0 / Dashboard (modern)

```
<progetto>/
├── index.php               # entry point
├── assets/
│   ├── css/
│   │   └── style.css
│   └── js/
│       └── app.js
├── api/                    # endpoints PHP
├── data/                   # JSON data files
└── demo/                   # standalone HTML demos
```

---

## 3. CRUD Pattern

```php
$CRUD->Page([
    'TipoPagina' => 'lista', // 'scheda.cards', 'scheda'
    'Title' => 'Titolo pagina',
    'Header' => [
        'Title' => 'Titolo',
        'Breadcrumbs' => [
            ['Link' => $local_path.'/index.php', 'Label' => 'Home'],
            ['Link' => '#', 'Label' => 'Sezione', 'Active' => 1],
        ],
        'Actions' => [],
    ],
    'Sidebar' => ['Active' => 0],
    'Table' => [
        'Ajax' => 1,
        // ...
    ],
]);

$CRUD->Form([
    'Tabella' => 'nome_tabella',
    'Azione' => 'insert', // 'update', 'delete'
    'Location' => $path.'pagina.php?ID=#ID#',
    'Submit' => 'Salva',
    'FunctionBefore' => [],
    'FunctionAfter' => [],
    'Rows' => [
        [['Tipo'=>'text', 'Nome'=>'Campo', 'Label'=>'Etichetta', 'ID'=>'Campo', 'Attributi'=>' required ', 'Valore'=>'', 'NomeSuTabella'=>'Campo']],
        [['Tipo'=>'checkbox', 'Nome'=>'IS_Flag', 'Label'=>'Attivo', 'ID'=>'IS_Flag', 'Attributi'=>'', 'Valore'=>1, 'Checked'=>0, 'NomeSuTabella'=>'IS_Flag']],
        [['Tipo'=>'separator-card', 'Label'=>'Sezione']],
    ],
]);
```

---

## 4. Form Types

Valid types for `Tipo` in Form rows:

```
text, checkbox, hidden, email, separator-card, data, ora, data-mask,
ora-mask, tel, integer, real, file, file2, available, slide,
toggle, radio, stepper, autocomplete
```

---

## 5. Database Naming (obbligatorio)

| Element | Format | Example |
|---------|--------|---------|
| Tables | plural lowercase | `studi`, `agenti`, `materiali` |
| Primary Key | `ID` auto-increment | |
| Foreign Key | `ID`+TableName | `IDStudio`, `IDTestata` |
| Boolean flags | `IS_*` (0/1) | `IS_Attivo`, `IS_Eliminato` |
| Computed fields | `Tab_*` | `Tab_Cliente`, `Tab_Totale` |
| Lookup tables | `tab_*` | `tab_stati_intervento` |
| Child tables | `{padre}_righe` | `righe_intervento` |
| History | `_log`, `_storico` | `testata_intervento_log` |

**Tab_* are maintained by TRIGGERS only. Never write them from PHP.**

---

## 6. Domain Names — NEVER translate

```
rapportini / interventi, laboratorio, controlli, preventivi,
studi / clienti / depositi / agenti / tecnici, solleciti,
pagamenti, contratti, materiali / articoli, anagrafiche
```

These are business terms. Never convert to English or generic terms.

---

## 7. Function Conventions

```php
// Single $params array argument
function GetRow($params) { /* ... */ }

// Domain prefixes
// Lab_* (laboratorio), ATT_* (attività/CRM), CTR_* (controlli)
// Solleciti_*, FILES_*, RAPP_*, Azianda_*, Login_*
// Get* (lookups), UpdateAjaxCampo, SelectAjax

// AJAX dispatch
// execute.php?Funzione=NOME&Parametri=JSON_TOKENIZZATO
// Tokenization: _GA_={ _GC_=}, _DUEP_=:, _DQUOT_=", _SQUOT_='
```

---

## 8. Code Style

```php
// PHP short tags OK
<?= $var ?>

// String interpolation OK
$query = "SELECT * FROM $table WHERE ID=$id";

// Inline JS/CSS accepted
'Script' => "
    document.addEventListener('DOMContentLoaded', function() {
        // ...
    });
",

// mysql_* API OK (shim-based, do not modernize)
$result = mysql_query($query);
$row = mysql_fetch_assoc($result);
```

---

## 9. Anti-patterns (never do)

1. Never change DB naming or function naming
2. Never add prepared statements or modernize SQL unless explicitly asked
3. Never touch `*_old/` directories (dead backups)
4. Never touch `action/crud/functions.php` (dead code)
5. Never touch `app/` directory (separate fork)
6. Never modify vendored libs in `plugins/`
7. Never add secrets to `configure.php`
8. Never translate domain names

---

## 10. Checklist (verify before reporting done)

```bash
# Syntax
php -l file.php

# Pattern presence (should be > 0)
grep -c 'CRUD->Page' file.php
grep -c 'CRUD->Form' file.php

# Anti-pattern absence (should be 0)
grep -c 'mysql_query' file.php  # after conversion
grep -c '<div class="content-page"' file.php  # if T2

# Domain names preserved
grep -c 'interventi\|rapportini\|laboratorio' file.php

# File diff scope
git diff --stat  # should show only expected files
```

---

## 11. When starting a new project

1. Read this file fully
2. Read project's AGENTS.md if exists
3. Read project's STYLE.md if exists
4. Identify project type (STH / Insta / Dashboard / Other)
5. Apply corresponding structure above
6. Verify with checklist

---

## 12. When modifying existing project

1. Read the specific file(s) to modify
2. Read adjacent files for context (es. `_lista.php` → check `_nuovo.php`)
3. Identify existing patterns in those files
4. Match that pattern exactly
5. Verify with checklist
