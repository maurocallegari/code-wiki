# Scrivere Prompt che Funzionano

## TL;DR

- **Hermes**: italiano, contesto + azione + verifica
- **Codex**: inglese, azione + input + output atteso
- **Mai**: prompt vaghi tipo "migliora", "fixa", "ottimizza"
- **Sempre**: dice cosa, dove, come verificare

---

## La formula per Hermes (italiano)

```
[AZIONE] in [PROGETTO/PATH] + [CONTESTO] + [OUTPUT ATTESTATO] + [VERIFICA]
```

### Esempi

**❌ SBAGLIATO:**
> "Fixa il bug di Insta"

**✅ GIUSTO:**
> "Nel file `/home/clawy/dev/insta/assets/js/app.js`, la funzione `calcSize()` non tiene conto del ratio 1:1. 
>  1. Aggungi un check: se width === height, usa 1:1 
>  2. Testa con una slide 1080x1080 
>  3. Verifica che `slidePreview.html` mostri il risultato corretto"

---

## La formula per Codex (inglese)

```
[TASK] in [FILE/PATH] + [CONSTRAINTS] + [VERIFY WITH]
```

### Esempi

**❌ BAD:**
> "Improve the site"

**✅ GOOD:**
> "Refactor `/home/clawy/public_html/demo/sito-5-giorni/index.html`:
>  - Extract inline CSS to external file
>  - Keep same visual output (screenshot before/after)
>  - Validate: HTML must pass W3C validator
>  - Constraint: zero external dependencies"

---

## Italiano vs Inglese

| Contesto | Lingua | Motivazione |
|---|---|---|
| Conversazione con Hermes | 🇮🇹 IT | Sei più veloce, esprimi meglio i contesti |
| Codice (Codex) | 🇬🇧 EN | Terminologia tecnica, pattern, nomi variabili |
| Commenti nel codice | 🇬🇧 EN | Standard universale |
| Nomi variabili/funzioni | 🇬🇧 EN | `getUserData()` non `prendiDatiUtente()` |
| Commit messages | 🇬🇧 EN | Standard: `feat:`, `fix:`, `refactor:` |

---

## Esempio reale: Fix bug countdown Insta

### ❌ Prompt SBAGLIATO
> "Il timer non funziona, fixalo"

Perché fallisce: dove? Quale timer? Non dice cosa deve succedere.

### ✅ Prompt GIUSTO (Hermes)
> "In `/home/clawy/dev/insta/assets/js/timer.js`:
> 1. Quando utente seleziona 'custom' duration e mette 30, il countdown parte da 0 invece che 30
> 2. Controlla la funzione `startCountdown()` — non legge il valore da `#customDuration`
> 3. Fix: prendi il valore, se > 0 usa quello, altrimenti default 60
> 4. Verifica: apri `real-preview.html`, seleziona custom+30, countdown deve partire da 30"

### ✅ Prompt GIUSTO (Codex, da Hermes)
> "In `/home/clawy/dev/insta/assets/js/timer.js`, function `startCountdown()`:
> - Reads duration from `#durationSelect` but ignores `#customDuration` input
> - Fix: if customDuration.value > 0, use it; else use select value
> - Constraint: no new dependencies, keep existing API
> - Verify: `node -e \"const t=new CustomTimer(); t.startCountdown(30); assert(t.remaining===30)\"`"

---

## Errori comuni

| Errore | Perché è sbagliato | Come correggere |
|---|---|---|
| "Migliora il sito" | Non dice cosa, dove, come | "Migliora velocità caricamento Insta: comprimi immagini, minify CSS" |
| "Fixa il bug" | Quale bug? Dove? | "Fixa: in `app.js:45` undefined reading 'value' — aggiungi null check" |
| "Converti in T2" | Un file? Tutti? Come? | "Converti `agenti_elimina.php` in T2: cambia `$_GET` in `$CRUD->Id`, wrap in `Rows()`" |
| "Fai un audit" | Di cosa? Con quali criteri? | "Fai audit sicurezza `/home/clawy/public_html/`: cerca SQL injection, XSS, eval()" |

---

## Checklist prompt

Prima di inviare un prompt, chiediti:

- [ ] **Dove**: ho specificato il path/file?
- [ ] **Cosa**: ho detto esattamente cosa deve succedere?
- [ ] **Output**: ho descritto cosa mi aspetto alla fine?
- [ ] **Verifica**: ho detto come verifico che funzioni?
- [ ] **Vincoli**: ho detto cosa NON deve toccare?
