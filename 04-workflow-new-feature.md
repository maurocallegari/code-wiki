# Nuova Feature Workflow

> Come aggiungere una feature senza rompere tutto

## TL;DR

1. **Pianifica**: cosa, dove, come si integra
2. **Crea branch**: mai lavorare direttamente su main
3. **Implementa**: un pezzo alla volta
4. **Testa**: verifica ogni step
5. **Merge + Deploy**: solo dopo tutto verde

---

## Step 1: Pianifica (prima di toccare codice)

Rispondi a queste domande:
- **Cosa esattamente**: "aggiungere dark mode" non basta. "Toggle dark/light in header, persist in localStorage, rispettare prefers-color-scheme"
- **Dove**: quale file/cartella? Se nuovo, dove va?
- **Come si integra**: tocca CSS? JS? PHP? Database?
- **Vincoli**: non toccare X, mantenere compatibilità con Y

**Output:** un mini-plan di 3-5 punti.

---

## Step 2: Crea branch (per progetti git)

```bash
cd ~/dev/insta
git checkout -b feature/dark-mode
# lavori qui
# quando sei sicuro:
git checkout main
git merge feature/dark-mode
```

**Per progetti non-git (es. claw.nswr.it):**
- Fai una copia di backup dei file che modificherai
- Oppure lavora in un dir separato e poi copia

---

## Step 3: Implementa incrementale

**❌ MAI:**
> "Aggiungi dark mode a tutto il sito"

**✅ SEMPRE:**
> "1. Aggiungi toggle in `index.php` header
>  2. Aggiungi variabili CSS in `style.css` (`--bg`, `--fg`)
>  3. Aggiungi listener JS in `app.js` per toggle + localStorage
>  4. Testa solo questo pezzo
>  5. Poi espandi a tutte le sezioni"

**Perché:** se fai tutto in una volta e non funziona, non sai dove è il problema.

---

## Step 4: Testa ogni incremento

Dopo ogni micro-step:
1. Verifica codice (grep, php -l, ecc.)
2. Verifica visiva (browser)
3. Verifica funzionale (azioni utente)
4. Solo allora vai al prossimo step

---

## Esempio reale: Aggiunta "JSON Tree Viewer" alla Dashboard

**Pianifica:**
- Cosa: tool che visualizza JSON come albero espandibile
- Dove: `public_html/demo/json-tree/index.html`
- Integrazione: standalone HTML, linkata in `data/demos.json`
- Vincoli: nessun framework, dark glass style come le altre demo

**Implementazione:**
1. Creo struttura HTML base con style dark glass
2. Aggiogo `parseJSON()` con validazione
3. Aggiogo `renderTree()` con nodi espandibili
4. Verifico: apro `demo/json-tree/` nel browser, testo con JSON buono e sbagliato
5. Aggiungo `data/demos.json` voce per farla apparire nella sezione Demo
6. Deploy FTP

**Verifica finale:**
- Browser: apro `claw.nswr.it/demo/json-tree/`
- Testo: incollo JSON valido → albero appare
- Testo: incollo JSON invalido → errore chiaro
- Testo: apro Dashboard → Demo sezione mostra JSON Tree come prima card

---

## Step 5: Documenta (per te stesso)

Nel commit message o in un file note:
- Cosa hai fatto
- Dove
- Come testare
- Vincoli rispettati

```markdown
feat: add JSON Tree Viewer demo

- New demo at `demo/json-tree/index.html`
- Parses JSON, shows expandable tree
- Dark glass style (consistent with other demos)
- Added to `data/demos.json` (appears first in Demo section)
- No frameworks, zero dependencies

Test: open `claw.nswr.it/demo/json-tree/`, paste JSON, click nodes to expand
```
