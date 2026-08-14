# MAURO-STYLE.md

> Stile personale di Mauro per coding con AI (Hermes + Codex)
> Stack: PHP/MySQL legacy (STH Assitec), Insta 2.0, dashboard, trading
> Obiettivo: codice manutenibile, niente sorprese, stile uniforme

---

## Regola #1: Bootstrap pattern

Ogni pagina inizia così:
```php
<?php
require_once($_SERVER['DOCUMENT_ROOT'].'/configure.php');
require_once($path_require.'/config.php');
require_once($path_require.'/functions.php');
```

**Non devi spiegarlo all'AI**: è implicito. Lo fa da sola.

---

## Regola #2: Struttura file

| Tipo | Path |
|------|------|
| Lista | `view/<modulo>/<modulo>_lista.php` |
| Dettaglio | `view/<modulo>/<modulo>_scheda.php` |
| Nuovo | `view/<modulo>/<modulo>_nuovo.php` |
| Modifica | `view/<modulo>/<modulo>_modifica.php` |
| Elimina | `view/<modulo>/<modulo>_elimina.php` |
| Feed JSON | `view/<modulo>/*.json.php` |
| Formazione | `view/<modulo>/modal_*.php` |

**Non devi spiegarlo all'AI**: è implicito. Lo fa da sola.

---

## Regola #3: CRUD pattern

```php
$CRUD->Page([
    'TipoPagina' => 'lista', // o 'scheda.cards', 'scheda'
    'Title' => 'Titolo',
    'Header' => [
        'Title' => 'Titolo',
        'Breadcrumbs' => [...],
        'Actions' => [...],
    ],
    'Table' => [
        'Ajax' => 1,
        ...
    ],
]);
```

```php
$CRUD->Form([
    'Tabella' => 'agenti',
    'Azione' => 'insert',
    'Location' => $path.'agenti_scheda.php?ID=#ID#',
    'Rows' => [
        [['Tipo'=>'text', 'Nome'=>'RagioneSociale', 'Label'=>'Ragione sociale', ...]],
        [['Tipo'=>'checkbox', 'Nome'=>'IS_Attivo', 'Label'=>'Attivo', ...]],
    ],
]);
```

**Non devi spiegarlo all'AI**: è implicito. Lo fa da sola.

---

## Regola #4: Tipi form validi

```
text, checkbox, hidden, email, separator-card, data, ora, data-mask,
ora-mask, tel, integer, real, file, file2, available, slide,
toggle, radio, stepper, autocomplete
```

**Non devi spiegarlo all'AI**: è implicito.

---

## Regola #5: Database naming

| Tipo | Formato |
|------|---------|
| Tabelle | `plurali_minuscole` (`studi`, `agenti`, `materiali`) |
| PK | `ID` (auto-increment) |
| FK | `ID`+NomeTabella (`IDStudio`, `IDTestata`) |
| Flag | `IS_*` (0/1) |
| Computed | `Tab_*` (NON scriverli in PHP, solo trigger) |
| Lookup | `tab_*` |
| Child | `{padre}_righe`, `_ore`, `_uscite` |
| History | `_log`, `_storico`, `_bkp` |

**Attenzione**: `Tab_*` sono mantenuti da **trigger**, non scriverli mai in PHP.

---

## Regola #6: Function conventions

```php
// Funzione tipica
function GetRow($params) {
    // $params è SEMPRE un array
    // Ritorna array o null
}

// Prefissi dominio
// Lab_* (laboratorio)
// ATT_* (attività/telemarketing)
// CTR_* (controlli)
// Solleciti_* (solleciti)
// Get* (lookup comune)
```

**Non devi spiegarlo all'AI**: è implicito.

---

## Regola #7: Domain naming

**NON TRADURRE MAI**:
- rapportini / interventi
- laboratorio
- controlli
- preventivi
- studi / clienti / depositi / agenti / tecnici
- solleciti / pagamenti / contratti
- materiali / articoli

**Non devi spiegarlo all'AI**: è implicito.

---

## Regola #8: File function location

| Tipo | File |
|------|------|
| Funzioni generali | `require/functions.php` |
| Funzioni business/AJAX | `require/functions_aggiuntive.php` |
| Funzioni CRUD | `require/functions_crud.php` |
| Classi layout CRUD | `require/layout_crud.php` |
| Hub JS client | `require/head_crud.php` |

**Non devi spiegarlo all'AI**: è implicito.

---

## Regola #9: Execute engine

```bash
# Dispatch generico
action/crud/execute.php?Funzione=NOME&Parametri=JSON_TOKENIZZATO

# Delete/Insert/Update
action/crud/crud_multiple.php
# con ParametriForm (serialized), Azione (insert/update/delete), Tabella, FunctionBefore/After
```

**Tokenizzazione**: `_GA_`=`{`, `_GC_`=`}`, `_DUEP_`=`:`, `_DQUOT_`=`"`, `_SQUOT_`=`'`, `_AND_`=`&`, ...

---

## Regola #10: Inline JS/PHP

Esempio di pattern usato:
```php
'Script' => "
    document.addEventListener('DOMContentLoaded', function () {
        // validazione, init, ...
    });
",
```

**Accettato**: inline `<script>` e inline `style`. Non è pulito ma è il tuo stile.

---

## Regola #11: Anti-pattern da evitare

1. **Non modernizzare SQL**: `mysql_*` è uno shim, va bene così
2. **Non aggiungere prepared statements**: non è il tuo stile
3. **Non rimuovere `mysql_*`**: richiederebbe troppo lavoro
4. **Non toccare `*_old/`**: sono backup morti
5. **Non toccare `action/crud/functions.php`**: è dead code
6. **Non toccare `app/`**: fork separato, out of scope

---

## Come usare questa guida con l'AI

**Fase 1**: all'inizio di ogni task, carica questo file come contesto
**Fase 2**: verifica che l'output rispetti queste regole
**Fase 3**: se non le rispetta, rifa il prompt con correzioni specifiche

Il file `PATTERNS.md` e `docs/convenzioni.md` in `dev/sth-assitec-gpt/` contengono approfondimenti tecnici.
