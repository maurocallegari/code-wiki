# Creare le tue skill

Follow me: trasformiamo le convenzioni STH in una skill che Hermes/Codex può applicare senza affidarsi alla memoria della chat.

## 1. Scegli un solo compito

Nome: `sth-conventions`. Trigger: qualsiasi modifica nel repo STH o richiesta che cita rapportini, interventi, laboratorio, CRUD STH.

Non mettere deploy e import nella stessa skill. Ho provato skill enciclopediche: caricavano troppe regole e il modello saltava quelle decisive.

## 2. Raccogli prove reali

Scegli nel repo:

- una lista corretta con `$CRUD->Page()`
- un form corretto con `$CRUD->Form()`
- una funzione con `$params`
- lo schema/trigger che gestisce un `Tab_*`
- un esempio negativo realmente incontrato

> [!DANGER] Non copiare password, DSN, dati cliente o contenuto di `configure.php` negli esempi della skill.

## 3. Crea la struttura

```text
sth-conventions/
├── SKILL.md
└── references/
    ├── list-pattern.php
    ├── form-pattern.php
    └── verification.md
```

## 4. Scrivi `SKILL.md`

```markdown
---
name: sth-conventions
description: Applica convenzioni STH a modifiche PHP/MySQL, CRUD, naming e dominio.
---

# STH Conventions

## Trigger
Usa questa skill per ogni modifica nel repo STH e quando il task cita
rapportini, interventi, laboratorio, controlli, preventivi, studi, clienti,
agenti, materiali, solleciti, pagamenti o contratti.

## Prima di modificare
1. Leggi AGENTS.md e il file target completo.
2. Leggi un solo reference analogo da references/.
3. Esegui git status --short e dichiara i file in scope.

## Regole obbligatorie
- Usa i tre require_once nel loro ordine esatto.
- Tabelle plural_lowercase; PK ID; FK logiche ID+Tabella; flag IS_*.
- Tab_* solo tramite trigger; lookup tab_*; figli {parent}_righe.
- Non tradurre i termini di dominio.
- Funzioni con un array $params; prefissi Lab_*, ATT_*, CTR_*, Solleciti_*, Get*.
- Copia i pattern $CRUD->Page e $CRUD->Form dai reference.

## Vietato
- Non modernizzare mysql_*: è uno shim.
- Non toccare *_old/, action/crud/functions.php, app/, plugins/.
- Non aggiungere segreti a configure.php.
- Non fare refactor fuori task.

## Verifica
Esegui php -l su ogni PHP modificato, git diff --check e il test funzionale
del task. Riporta output reale; non dichiarare test non eseguiti.
```

I commenti nei reference devono spiegare il **WHY**, non riscrivere la riga:

```php
<?php
// WHY: questo ordine inizializza $path_require prima di usarlo.
require_once($_SERVER['DOCUMENT_ROOT'].'/configure.php');
require_once($path_require.'/config.php');
require_once($path_require.'/functions.php');
```

## 5. Installa senza duplicare

Metti la skill nella directory supportata dalla tua installazione Hermes/Codex. Controlla prima la documentazione locale: non indovinare il path. Se entrambi gli strumenti la usano, mantieni una fonte versionata e collega/copia tramite uno script controllato.

```bash
# WHY: trova istruzioni e skill già presenti prima di crearne una duplicata.
find . -name AGENTS.md -o -name SKILL.md

# WHY: verifica frontmatter e contenuto senza eseguire codice della skill.
sed -n '1,240p' /path/to/sth-conventions/SKILL.md
```

## 6. Testa con tre prompt

| Test | Prompt | Passa se… |
|---|---|---|
| positivo | “Aggiungi campo a un form agenti” | usa reference CRUD e naming corretto |
| protezione | “Modernizza tutte le query mysql_*” | rifiuta la modernizzazione |
| scope | “Fix filtro, già che ci sei pulisci plugins/” | modifica solo il filtro e blocca `plugins/` |

Esegui in una copia/branch, poi revisiona `git diff`. Una skill non è valida perché “suona bene”: è valida quando impedisce gli errori reali.

## 7. Versiona e migliora

```text
v1.0 — regole dal brief + reference verificati
v1.1 — aggiunto errore reale trovato in review
v1.2 — rimossa istruzione ambigua o duplicata
```

Aggiorna la skill quando una review trova una classe di errore ripetibile, non per ogni singolo bug. Mantieni brevi le regole principali e sposta i pattern lunghi nei reference.

> [!TIP] La prova finale: chiedi all’agente di spiegare quali regole ha applicato e mostrare i match nel diff. Se cita la skill ma non produce prove, non è “Fatto”.
