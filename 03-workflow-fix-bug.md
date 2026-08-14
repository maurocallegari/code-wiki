# Fix Bug Workflow

> Il problema: ti dico "fixato" ma non funziona. Ecco come evitare.

## TL;DR

1. **Isolare**: un bug alla volta
2. **Capire**: perché succede (non sintomo)
3. **Fixare**: modifica minima
4. **Verificare**: prova reale, non "dovrebbe funzionare"
5. **Deployare**: solo dopo verifica

---

## Step 1: Isolare il bug

Prima di tutto, identifica esattamente cosa non funziona.

**❌ SBAGLIATO:**
> "La dashboard non va"

**✅ GIUSTO:**
> "In `claw.nswr.it`, la sezione 'Idee' mostra 'Ancora nulla qui.' anche se `data/idee.json` ha 10 elementi. Il problema è in `js/app.js` riga circa 150: `getIdee()` legge `d.idee` ma il JSON ha `items`."

**Cosa fare:**
- Apri il file, leggi il codice attorno al bug
- Identifica la riga esatta
- Cerca nel browser console errori JS
- Cerca nel server errori PHP (`tail -f /var/log/php*.log`)

---

## Step 2: Dire a Hermes COSA fare

**Formula:**
```
In [FILE:RIGA], [DESCRIZIONE PROBLEMA]
CAUSA PROBABILE: [ipotesi]
FIX: [cosa deve fare]
VERIFICA: [come controllo]
```

**Esempio reale (mio):**

> **File:** `dev/insta/assets/js/slide-preview.html` (riga 45)
> **Prometti:** "Preview slide 16:9 ma il titolo esce tagliato a destra"
> **Causa:** `.cds-title` ha `max-width: 300px` che non scala con il viewport
> **Fix:** Cambia in `max-width: 90vw` e aggiungi `word-break: break-word`
> **Verifica:** Apri `slide-preview.html` con slide 16:9, il titolo "Test lungo titolo" deve stare nel box
> **Vincolo:** Non toccare gli altri visual styles (newspaper, bold)

---

## Step 3: Verifica REALE (non "dovrebbe funzionare")

Dopo che Hermes (o tu) avete fatto la modifica, verificate:

**Verifica codice:**
```bash
# Cerca il pattern che non dovrebbe esserci più
grep -n "max-width: 300px" dev/insta/assets/css/visual-styles2.css
# Deve tornare 0 risultati

# Cerca il fix
grep -n "max-width: 90vw" dev/insta/assets/css/visual-styles2.css
# Deve tornare almeno 1 risultato
```

**Verifica visiva:**
```bash
# Se è un file web, apri nel browser
# Oppure usa il browser tool
browser_navigate("https://claw.nswr.it/projects/insta/slide-preview.html")
# Controlla che il titolo non esca più
```

**Verifica funzionale:**
```bash
# Se è un'API
curl -s "https://claw.nswr.it/api/demo.php" | grep "idée"
# Deve tornare i dati corretti
```

---

## Step 4: Deploy SOLO dopo verifica

Se la verifica passa:
1. **Bump versione** (per cache bust): `sed -i "s/APP_VERSION = '...'/APP_VERSION = '20260814b'/" index.php`
2. **FTP deploy**: `lftp mirror` o `put`
3. **Verifica live**: `curl` o browser su URL reale

---

## Errori che ho fatto (imparati dai tuoi errori)

| Errore | Conseguenza | Come evitare |
|---|---|---|
| "Fixato" senza testare | Bug ancora lì, solo nascosto | Verifica sempre prima di dire "fato" |
| Fix su 10 file in una volta | Non so quale fix ha rotto cosa | Un fix alla volta, test, poi prossimo |
| Non bumpare versione | Browser mostra cache vecchia | Ogni deploy = nuova versione |
| Deploy su production diretto | Se rompo, tutti lo vedono | Prima in staging, poi live |
| Fare fix "a occhio" | Introduco altri bug | Leggi il codice attutto prima |

---

## Template prompt fix bug

Copia e incolla:

```
BUG: [descrizione sintomo]
DOVE: [file:rigacodice attorno al problema]
CAUSA IPOTIZZATA: [perché secondo te succede]
FIX PROPOSTO: [cosa deve cambiare]
VERIFICA: [comando o azione per verificare]
VINCOLI: [cosa NON deve toccare]
```
