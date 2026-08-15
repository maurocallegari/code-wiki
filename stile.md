# Il Tuo Stile

> Le 11 regole d'oro del tuo stile di coding. L'AI le segue automaticamente.

---

## Regola 1: Bootstrap Pattern

Ogni pagina PHP inizia così:

```php
<?php
require_once($_SERVER['DOCUMENT_ROOT'].'/configure.php');
require_once($path_require.'/config.php');
require_once($path_require.'/functions.php');
```

**Non devi spiegarlo all'AI:** è implicito.

---

## Regola 2: Struttura File

| Tipo | Path |
|------|------|
| Lista | `view/<modulo>/<modulo>_lista.php` |
| Dettaglio | `view/<modulo>/<modulo>_scheda.php` |
| Nuovo | `view/<modulo>/<modulo>_nuovo.php` |
| Modifica | `view/<modulo>/<modulo>_modifica.php` |
| Elimina | `view/<modulo>/<modulo>_elimina.php` |
| Feed JSON | `view/<modulo>/*.json.php` |
| Formazione | `view/<modulo>/modal_*.php` |

---

## Regola 3: CRUD Pattern

```php
$CRUD->Page([
    'TipoPagina' => 'lista', // 'scheda.cards', 'scheda'
    'Title' => 'Titolo',
    'Header' => [
        'Title' => 'Titolo',
        'Breadcrumbs' => [...],
        'Actions' => [...],
    ],
    'Table' => ['Ajax' => 1, ...],
]);

$CRUD->Form([
    'Tabella' => 'agenti',
    'Azione' => 'insert',
    'Location' => $path.'agenti_scheda.php?ID=#ID#',
    'Rows' => [
        [['Tipo'=>'text', 'Nome'=>'RagioneSociale', ...]],
        [['Tipo'=>'checkbox', 'Nome'=>'IS_Attivo', ...]],
        [['Tipo'=>'separator-card', 'Label'=>'Sezione']],
    ],
]);
```

---

## Regola 4: Tipi Form Validi

```
text, checkbox, hidden, email, separator-card, data, ora, data-mask,
ora-mask, tel, integer, real, file, file2, available, slide,
toggle, radio, stepper, autocomplete
```

---

## Regola 5: Database Naming

| Tipo | Formato | Esempio |
|------|---------|---------|
| Tabelle | `plurali_minuscole` | `studi`, `agenti` |
| PK | `ID` auto-increment | |
| FK | `ID`+NomeTabella | `IDStudio`, `IDTestata` |
| Flag | `IS_*` (0/1) | `IS_Attivo` |
| Computed | `Tab_*` (solo trigger!) | `Tab_Cliente` |
| Lookup | `tab_*` | `tab_stati_intervento` |
| Child | `{padre}_righe` | `righe_intervento` |

<div class="callout callout-danger">

**`Tab_*` sono mantenuti da TRIGGERS.** Non scriverli mai in PHP.

</div>

---

## Regola 6: Function Conventions

```php
// Funzione tipica
function GetRow($params) {
    // $params è SEMPRE un array
    // Ritorna array o null
}

// Prefissi dominio
// Lab_* (laboratorio)
// ATT_* (attività/CRM)
// CTR_* (controlli)
// Solleciti_* (solleciti)
// Get* (lookup comune)
```

---

## Regola 7: Domain Naming (NON TRADURRE!)

```
rapportini, interventi, laboratorio, controlli, preventivi,
studi, clienti, depositi, agenti, tecnici, solleciti,
pagamenti, contratti, materiali, articoli, anagrafiche
```

---

## Regola 8: File Function Location

| Tipo | File |
|------|------|
| Funzioni generali | `require/functions.php` |
| Funzioni business/AJAX | `require/functions_aggiuntive.php` |
| Funzioni CRUD | `require/functions_crud.php` |
| Classi layout CRUD | `require/layout_crud.php` |
| Hub JS client | `require/head_crud.php` |

---

## Regola 9: Execute Engine

```bash
# Dispatch generico
action/crud/execute.php?Funzione=NOME&Parametri=JSON_TOKENIZZATO

# Delete/Insert/Update
action/crud/crud_multiple.php
# con ParametriForm (serialized), Azione (insert/update/delete), Tabella
```

**Tokenizzazione:** `_GA_`=`{`, `_GC_`=`}`, `_DUEP_`=`:`, `_DQUOT_`=`"`, `_SQUOT_`=`'`

---

## Regola 10: Inline JS/CSS

```php
'Script' => "
    document.addEventListener('DOMContentLoaded', function () {
        // validazione, init, ...
    });
",
```

**Accettato:** inline `<script>` e inline `style`. Non è pulito ma è il tuo stile.

---

## Regola 11: Anti-Pattern (MAI FARE)

<div class="callout callout-danger">

1. **Non modernizzare SQL:** `mysql_*` è uno shim, va bene così
2. **Non aggiungere prepared statements:** non è il tuo stile
3. **Non rimuovere `mysql_*`:** richiederebbe troppo lavoro
4. **Non toccare `*_old/`:** sono backup morti
5. **Non toccare `action/crud/functions.php`:** è dead code
6. **Non toccare `app/`:** fork separato, out of scope
7. **Non toccare vendored libs:** `plugins/`, `tablesorter`, `select2`

</div>

---

## Come usare con l'AI

1. All'inizio di ogni task, carica questo file come contesto
2. Verifica che l'output rispetti queste regole
3. Se non le rispetta, rifa il prompt con correzioni specifiche

Il file `CODEX-CONTEXT.md` esportabile contiene queste regole in formato per Codex.
