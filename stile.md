# Il mio stile STH

Queste sono istruzioni, non suggerimenti. Copiale in `AGENTS.md` e nella skill STH: ripeterle solo nel prompt ha fallito quando il contesto è stato compresso.

## Le 11 regole

| # | Regola | Convenzione |
|---:|---|---|
| 1 | Bootstrap PHP | tre `require_once` nell’ordine esatto |
| 2 | Tabelle | `plural_lowercase`: `studi`, `agenti`, `materiali` |
| 3 | Chiave primaria | `ID`, auto-increment |
| 4 | Chiavi logiche | `ID` + tabella: `IDStudio`, `IDTestata`; niente FK fisiche |
| 5 | Flag | `IS_*`, valori 0/1 |
| 6 | Computed | `Tab_*` aggiornati solo da trigger, mai dal PHP |
| 7 | Lookup e figli | `tab_*`; `{parent}_righe`, per esempio `righe_intervento` |
| 8 | Dominio | non tradurre mai i termini italiani elencati sotto |
| 9 | CRUD | usa `$CRUD->Page()` e `$CRUD->Form()` come nel repo |
| 10 | Funzioni | un solo array `$params`; prefissi approvati |
| 11 | Legacy protetto | non modernizzare `mysql_*`; non toccare directory/file vietati |

## Bootstrap esatto

```php
<?php
// WHY: configure.php definisce i path condivisi prima dei file applicativi.
require_once($_SERVER['DOCUMENT_ROOT'].'/configure.php');
// WHY: config.php carica la configurazione del progetto attraverso $path_require.
require_once($path_require.'/config.php');
// WHY: functions.php arriva dopo la configurazione da cui dipende.
require_once($path_require.'/functions.php');
```

## CRUD esatto

```php
<?php
// WHY: il renderer condiviso mantiene lista, header e tabella coerenti con STH.
$CRUD->Page([
    'TipoPagina'=>'lista',
    'Title'=>'Agenti',
    'Header'=>[/* copia la forma da una pagina STH già corretta */],
    'Table'=>[/* preserva le chiavi supportate dal CRUD esistente */],
]);

// WHY: non inventare HTML o campi quando il motore Form gestisce già il flusso.
$CRUD->Form([
    'Tabella'=>'agenti',
    'Azione'=>'insert',
    'Rows'=>[[/* usa un form reale dello stesso modulo come reference */]],
]);
```

## Database

```sql
CREATE TABLE agenti (
  ID INT AUTO_INCREMENT PRIMARY KEY,
  IDStudio INT,
  IS_Attivo TINYINT(1) NOT NULL DEFAULT 1,
  Tab_Cliente VARCHAR(255)
);
-- WHY: IDStudio è una relazione applicativa; STH non usa una FOREIGN KEY fisica.
-- WHY: Tab_Cliente deve essere mantenuto da un trigger, non da INSERT/UPDATE PHP.
```

> [!DANGER] Non aggiungere mai `Tab_*` a un array di salvataggio PHP. Il valore può sembrare corretto oggi e divergere al prossimo aggiornamento; il trigger è l’unica fonte.

## Funzioni e dominio

```php
<?php
// WHY: un unico contratto array consente di estendere i parametri senza cambiare firma.
function Lab_GetMateriali($params) {
    // ...
}
```

Prefissi ammessi: `Lab_*`, `ATT_*`, `CTR_*`, `Solleciti_*`, `Get*`.

Termini da non tradurre: **rapportini, interventi, laboratorio, controlli, preventivi, studi, clienti, agenti, materiali, solleciti, pagamenti, contratti**.

## Divieti assoluti

- non modernizzare SQL: `mysql_*` è uno shim intenzionale
- non modificare `*_old/`
- non modificare `action/crud/functions.php`
- non modificare `app/` o `plugins/`
- non aggiungere segreti a `configure.php`
- non creare una nuova astrazione se un reference STH risolve lo stesso caso

## Controlli automatici

```bash
# WHY: devono essere assenti modifiche nelle zone protette.
git diff --name-only | rg '(^|/)([^/]+_old|app|plugins)/|action/crud/functions\.php'

# WHY: Tab_* nel PHP è sospetto; revisiona ogni match, senza sostituzione automatica.
rg -n "Tab_[A-Za-z0-9_]+" --glob '*.php'

# WHY: traduzioni inglesi nel dominio spesso indicano che il modello ha inventato naming.
rg -n "reports|interventions|customers|payments" --glob '*.{php,js,sql}'
```
