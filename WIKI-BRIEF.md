# Wiki Brief — Cosa deve essere

---

## Nome: Code Wiki
**Per:** Mauro (Stealth Software, Treviso)
**Scopo:** Sostituire il mio coding tradizionale con AI-coding, mantenendo il mio stile e le mie convenzioni.

---

## Cos'è

Una wiki personale che insegna a usare Hermes + Codex nel mio lavoro quotidiano. Non è una reference generica, ma un tutorial pratico:

- Mi dice **quale strumento usare** per ogni task
- Mi dà **la formula per scrivere prompt efficaci**
- Mi insegna a **verificare l'output** (non fidarmi mai del "fato")
- Mi mostra **esempi reali** dai miei progetti (STH, Insta, Dashboard)
- Mi ricorda **le mie convenzioni** (naming, pattern, stile)

---

## Stack

- PHP/MySQL legacy (gestionale STH Assitec: CRM/ERP per manutenzione apparecchi dentali)
- JavaScript vanilla + Tailwind CSS
- Insta 2.0: generatore caroselli Instagram
- Clawy Dashboard: dashboard operativa su claw.nswr.it
- Trading tools: position-size, exit-calc, journal

## Strumenti AI

- **Hermes** (Raspberry + MacBook + Telegram) → orchestratore, deploy, fix complessi
- **Codex** (CLI) → coding puro, batch conversioni
- **gbrain** → memoria semantica cross-session
- **GStack** → /review, /qa, /ship, /autoplan, /investigate, /careful

---

## Problemi che la wiki deve risolvere

1. L'AI dice "fatto" ma non funziona
2. L'AI modifica file che non doveva
3. L'AI ignora i vincoli
4. L'AI fa batch troppo grandi → si blocca, perde contesto
5. L'AI non rispetta le convenzioni STH
6. Non so quando usare Hermes vs Codex
7. Non so scrivere prompt efficaci
8. Disco claw.nswr.it si riempie di file inutili
9. Deploy FTP falliscono per permessi o cache
10. Kanban Hermes non usato come task manager

---

## Il mio stile (da rispettare SEMPRE)

### Bootstrap PHP
```php
require_once($_SERVER['DOCUMENT_ROOT'].'/configure.php');
require_once($path_require.'/config.php');
require_once($path_require.'/functions.php');
```

### DB Naming
- Tabelle: `plurali_minuscole` (studi, agenti, materiali)
- PK: `ID` (auto-increment)
- FK: `ID`+NomeTabella (IDStudio, IDTestata) — nessun FOREIGN KEY fisico
- Flag: `IS_*` (0/1)
- Computed: `Tab_*` — SOLO trigger, MAI in PHP
- Lookup: `tab_*` (tab_stati_intervento)
- Child: `{padre}_righe` (righe_intervento)

### Domain (MAI tradurre!)
rapportini, interventi, laboratorio, controlli, preventivi, studi, clienti, agenti, materiali, solleciti, pagamenti, contratti

### CRUD Pattern
```php
$CRUD->Page(['TipoPagina'=>'lista', 'Title'=>'...', 'Header'=>[...], 'Table'=>[...]]);
$CRUD->Form(['Tabella'=>'agenti', 'Azione'=>'insert', 'Rows'=>[[...]]]);
```

### Function
- `$params` array singolo
- Prefissi: `Lab_*`, `ATT_*`, `CTR_*`, `Solleciti_*`, `Get*`

### Anti-pattern (MAI)
- Non modernizzare SQL (`mysql_*` OK)
- Non toccare `*_old/`, `action/crud/functions.php`, `app/`, `plugins/`
- Non aggiungere segreti in configure.php

---

## Come deve essere strutturata

Navigabile (sidebar, ricerca, indice). Pagine:

1. **Start Here** — obiettivo, filosofia
2. **Il Metodo** — formula universale, flusso di lavoro, regole d'oro
3. **Il Tuo Stile** — 11 regole (quelle sopra)
4. **Hermes Setup** — config, personalities, skills, cron, kanban
5. **Codex Setup** — install, auth, comandi, sandbox, context file
6. **GStack Skills** — /review, /qa, /ship, /autoplan, /careful, /investigate
7. **gbrain Setup** — init, import, query, sync, MCP
8. **Tutorial: Fix Bug** — esempio reale STH (filtro A-Z mobile)
9. **Tutorial: Nuova Feature** — esempio reale Insta (visual style Neon)
10. **Tutorial: Batch** — esempio reale T2 con Codex (reference + verifica)
11. **Importare Progetti** — checklist per codebase nuove
12. **Kanban** — come gestire task da chat
13. **Deploy FTP** — workflow verificato, permessi, BitNinja
14. **Problemi Comuni** — 10 errori che ho fatto e come evitarli
15. **Cheat Sheet** — comandi rapidi, pattern, verifiche

## Formato

- Markdown con callout (danger, warn, tip)
- Tabelle per decisioni
- Codice commentato con "PERCHÉ"
- Esempi reali (non finti)
- Mermaid diagram per flusso

## Tono

- Diretto, concreto, niente teoria
- "Fai così perché ho provato X e non funzionava"
- Hands-on, non reference
- Insegna a SOSTITUIRE il coding tradizionale con l'AI, non ad affiancarlo
