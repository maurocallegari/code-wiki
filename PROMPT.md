# Prompt per generare la Code Wiki di Mauro

> Copia e incolla questo prompt in Hermes, Codex, o qualsiasi altro tool AI per rigenerare o migliorare la wiki.

---

Crea una wiki personale di guida operativa per AI-coding. Deve essere calata su di me, le mie esigenze, i miei problemi ricorrenti. Non una reference generica, ma un tutorial pratico che impara a usare gli strumenti AI nel MIO stack e con MIO stile.

## CHI SONO

- Nome: Mauro
- Azienda: Stealth Software (stealthsoftware.it), Treviso
- Ruolo: developer/freelance, preventivi/audit PMI
- Stack principale:
  - PHP/MySQL legacy (gestionale STH Assitec: CRM/ERP per manutenzione apparecchi dentali)
  - JavaScript vanilla + Tailwind CSS
  - Insta 2.0: generatore caroselli Instagram (carousel desk)
  - Clawy Dashboard: dashboard operativa su claw.nswr.it
  - Trading: position-size calc, exit calc, journal
- Strumenti AI:
  - Hermes (Nous Research) su Raspberry Pi + MacBook Air + Telegram
  - Codex (OpenAI CLI)
  - gbrain (Garry Tan)
  - GStack (Garry Tan, 30 skills: /review, /qa, /ship, /autoplan, /investigate, /careful, /cso)
- Target: gestisco Hermes da iPhone via Telegram, voglio accesso unificato

## I MIEI PROBLEMI RICORRENTI (cosa la wiki deve risolvere)

1. L'AI dice "fatto" ma non funziona → devo verificare SEMPRE
2. L'AI modifica file che non doveva
3. L'AI ignora i vincoli (es. "usa T2" ma fa un wrapper)
4. L'AI fa batch troppo grandi → va in timeout, perde contesto
5. L'AI genera codice che sembra giusto ma non rispetta le convenzioni STH
6. Non so quando usare Hermes vs Codex
7. Non so scrivere prompt efficaci
8. Il disco su claw.nswr.it si riempie di file inutili
9. I deploy FTP falliscono per permessi 600 o cache
10. Il Kanban Hermes non viene usato come task manager quotidiano

## IL METODO CHE VOGLIO

### Formula Universale Prompt
```
FILE: [path esatto]
TASK: [cosa fare]
INPUT: [dati, esempio, contesto]
OUTPUT: [cosa mi aspetto alla fine]
VINCOLI: [cosa NON deve toccare]
VERIFICA: [comando per testare]
```

### Quando usare cosa
- **Hermes**: orchestratore, deploy, fix complessi, ricerche, cron → scrivigli in ITALIANO
- **Codex**: coding puro, batch conversioni, refactoring → scrivigli in INGLESE
- **Entrambi**: Hermes genera prompt inglese per Codex quando serve coding pesante

### Checklist post-AI (OBBLIGATORIA)
```bash
php -l file.php                    # sintassi
grep -c 'CRUD->Page' file.php      # > 0 (pattern atteso)
grep -c 'mysql_query' file.php     # = 0 (pattern sbagliato)
grep -c '<table' file.php          # = 0 (usa CRUD Table)
git diff --stat                    # solo file attesi
curl -s -o /dev/null -w "%{http_code}" URL  # 200
```

### Deploy (sempre così)
```bash
sed -i "s/APP_VERSION = '.*'/APP_VERSION = '$(date +%Y%m%d)b'/" index.php
mirror -R -p public_html/demo/ /demo/
curl -s -o /dev/null -w "%{http_code}" URL
```

## IL MIO STILE (STH Assitec)

### Bootstrap pattern (ogni pagina PHP)
```php
<?php
require_once($_SERVER['DOCUMENT_ROOT'].'/configure.php');
require_once($path_require.'/config.php');
require_once($path_require.'/functions.php');
```

### DB naming
- Tabelle: `plurali_minuscole` (studi, agenti, materiali)
- PK: `ID` auto-increment
- FK: `ID`+NomeTabella (IDStudio, IDTestata), nessun FOREIGN KEY fisico
- Flag: `IS_*` (0/1)
- Computed: `Tab_*` (SOLO trigger, mai in PHP!)
- Lookup: `tab_*` (tab_stati_intervento)
- Child: `{padre}_righe` (righe_intervento)
- History: `_log`, `_storico`, `_bkp`

### Domain naming (MAI tradurre!)
rapportini, interventi, laboratorio, controlli, preventivi, studi, clienti, depositi, agenti, tecnici, solleciti, pagamenti, contratti, materiali, articoli, anagrafiche

### CRUD pattern
```php
$CRUD->Page(['TipoPagina'=>'lista', 'Title'=>'...', 'Header'=>[...], 'Table'=>[...]]);
$CRUD->Form(['Tabella'=>'agenti', 'Azione'=>'insert', 'Location'=>'...', 'Rows'=>[[...]]]);
```

### Tipi form validi
text, checkbox, hidden, email, separator-card, data, ora, data-mask, ora-mask, tel, integer, real, file, file2, available, slide, toggle, radio, stepper, autocomplete

### Function conventions
- Singolo argomento `$params` array
- Prefissi dominio: `Lab_*`, `ATT_*`, `CTR_*`, `Solleciti_*`, `Get*`

### Anti-pattern (MAI)
- Non modernizzare SQL (mysql_* OK, è uno shim)
- Non aggiungere prepared statements
- Non toccare `*_old/`, `action/crud/functions.php`, `app/`, `plugins/`
- Non aggiungere segreti in configure.php

## STRUTTURA DELLA WIKI

Organizza in pagine navigabili (sidebar, ricerca, indice):

1. **Start Here** — obiettivo, filosofia, come usarla
2. **Il Metodo** — formula universale, flusso di lavoro, regole d'oro
3. **Il Tuo Stile** — 11 regole (bootstrap, DB naming, CRUD, function, domain, anti-pattern)
4. **Hermes Setup** — config, personalities, skills, cron, kanban, deploy
5. **Codex Setup** — install, auth, comandi, sandbox, context file
6. **GStack Skills** — /review, /qa, /ship, /autoplan, /careful, /investigate, /cso
7. **gbrain Setup** — init, import, query, sync, MCP
8. **Tutorial: Fix Bug** — esempio reale con STH (filtro A-Z mobile)
9. **Tutorial: Nuova Feature** — esempio reale con Insta (visual style Neon)
10. **Tutorial: Batch** — esempio reale T2 con Codex (reference + verifica)
11. **Importare Progetti** — checklist per codebase nuove
12. **Kanban** — come gestire task da chat (crea, verifica, completa)
13. **Deploy FTP** — workflow verificato, permessi, BitNinja
14. **Problemi Comuni** — 10 errori che ho fatto e come evitarli
15. **Cheat Sheet** — comandi rapidi, pattern prompt, verifiche

## FORMATTO

- Markdown con callout (danger, warn, tip)
- Tabelle per ogni decisione
- Codice commentato con "PERCHÉ"
- Esempi reali dal mio codice (non finti)
- Anti-pattern evidenziati
- Mermaid diagram per il flusso di lavoro

## TONO

- Diretto, concreto, niente teoria astrutta
- "Fai così perché ho provato X e non funzionava"
- Tutorial hands-on, non reference
- Deve insegnare a usare l'AI per SOSTITUIRE il coding tradizionale, non affiancarlo
