# Tutorial · Fix bug STH

Scenario reale dal brief: nella lista agenti STH il filtro A–Z non è usabile su mobile. Non inventiamo la causa prima di leggere il file.

## 1. Riproduci e isola

```bash
cd /home/clawy/dev/sth-assitec-gpt

# WHY: conserva modifiche preesistenti e impedisce di attribuirle al fix.
git status --short

# WHY: localizza il controllo reale; non assumere nome classe o struttura.
rg -n "A-Z|alfabet|Left|agenti" view/agenti/agenti_lista.php
```

Apri la pagina a 320, 375 e 414 px. Registra: il filtro esce dal viewport? va a capo? i link sono troppo piccoli? non scorre? La diagnosi dipende dal comportamento osservato.

## 2. Dai il contratto a Codex

```text
Read AGENTS.md and view/agenti/agenti_lista.php completely.

FILE: view/agenti/agenti_lista.php
TASK: make the existing A–Z agents filter usable by touch at 320–414 px.
INPUT: preserve all 26 existing links and their Active/Link/Label logic.
OUTPUT: the filter remains on one row and scrolls horizontally on narrow screens;
every letter remains clickable, with no page-level horizontal overflow.
CONSTRAINTS: change only the filter container/presentation. Do not change Table,
Header, Sidebar, PHP link logic, mysql_*, *_old/, action/crud/functions.php,
app/, or plugins/. No unrelated refactor.
VERIFY: php -l; git diff --check; inspect diff; browser test at 320/375/414 px.
Do not claim the browser test unless you actually run it.
```

> [!TIP] Ho provato “sistema il mobile”: il modello ha ridisegnato la pagina. Il contratto sopra nomina comportamento, scope e viewport; non lascia spazio a un redesign.

## 3. Revisiona la patch

La soluzione deve adattarsi alla struttura trovata. Un possibile pattern CSS — non copiarlo senza verificare selettori reali — è:

```css
/* WHY: lo scroll resta confinato al filtro e non allarga l’intera pagina. */
.agents-alpha-filter {
  display: flex;
  gap: .5rem;
  overflow-x: auto;
  overscroll-behavior-inline: contain;
  -webkit-overflow-scrolling: touch;
}

/* WHY: ogni lettera resta un target singolo invece di andare a capo. */
.agents-alpha-filter a {
  flex: 0 0 auto;
  min-width: 2.75rem;
  min-height: 2.75rem;
}
```

## 4. Smentisci “done”

```bash
# WHY: verifica sintassi PHP dopo l’eventuale stringa CSS/markup inline.
php -l view/agenti/agenti_lista.php

# WHY: conferma che la patch riguarda un solo file e nessuna zona protetta.
git diff --stat
git diff -- view/agenti/agenti_lista.php
git diff --check

# WHY: deve esserci overflow sul filtro, non su body/html.
rg -n "overflow-x|overflow-inline|-webkit-overflow-scrolling" view/agenti/agenti_lista.php
```

Test manuale:

- [ ] nessuno scroll orizzontale della pagina
- [ ] swipe del filtro funziona a 320/375/414 px
- [ ] A e Z raggiungibili
- [ ] ogni lettera si attiva e filtra come prima
- [ ] desktop invariato

## 5. Chiudi

Porta il task in “Verifica”; fai deploy solo dopo review. Live, ripeti il test mobile e controlla cache/versione. Solo allora “Fatto”.
