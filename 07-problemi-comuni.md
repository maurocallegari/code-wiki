# Problemi Comuni (e Come Evitarli)

> Errori che ho fatto io, errori che hai fatto tu, e come non ripeterli.

---

## Problema 1: "L'AI ha detto funziona ma non funziona"

**Cosa succede:** L'AI dice "ho testato, funziona" ma non ha testato davvero, o ha testato male.

**Perché:** L'AI non ha un ambiente reale. "Funziona" spesso significa "non ha dato errore evidente".

**Come evitare:**
- Chiedi SEMPRE: "Come hai verificato?"
- Verifica tu stesso su URL reale
- Non accettare "dovrebbe funziona" — solo "ho testato con X e Y, ecco output"

---

## Problema 2: "L'AI ha modificato file che non doveva"

**Cosa succede:** Chiedo di fixare un bug, l'AI modifica 5 file "per sicurezza".

**Perché:** L'AI cerca di essere utile, fa modifiche "migliorative" non richieste.

**Come evitare:**
- Vincolo esplicito: "NON toccare [file]"
- Dopo il fix: `git diff --stat` per vedere cosa ha toccato
- Se ha toccato troppo: `git checkout -- [file]` e ripeti con vincoli più stretti

---

## Problema 3: "L'AI ha ignorato i vincoli"

**Cosa succede:** Dico "usa T2" ma lui fa un wrapper `<div>` attorno al codice vecchio.

**Perché:** L'AI non capisce il contesto profondo. "T2" per lui è una parola, non un pattern specifico.

**Come evitare:**
- Fornisci esempio di riferimento (file T2 perfetto)
- Dopo il fix: verifica che i pattern sbagliati siano assenti
- Se fallisce: correggi tu manualmente e aggiungi l'errore come esempio negativo nel prompt

---

## Problema 4: "L'AI ha rotto qualcos'altro"

**Cosa succede:** Fix di un bug ne introduce un altro.

**Perché:** L'AI non ha visione d'insieme. Modifica locale senza considerare impatto globale.

**Come evitare:**
- Dopo ogni fix: testa TUTTE le funzionalità toccate, non solo quella fixata
- Usa `git diff` per vedere l'entità della modifica
- Se la modifica è > 50 righe: sospetta, chiedi di spezzettare

---

## Problema 5: "L'AI ha fatto troppo"

**Cosa succede:** Chiedo "aggiungi dark mode" e lui ristruttura tutto il CSS.

**Perché:** L'AI cerca di essere "completa", fa refactor non richiesti.

**Come evitare:**
- Vincolo: "Modifica solo [file specifico]"
- Vincolo: "Non fare refactor, solo feature X"
- Dopo: `git diff --stat` — se > 3 file modificati, qualcosa è andato storto

---

## Problema 6: "L'AI ha dimenticato il contesto"

**Cosa succede:** Dopo 20 minuti di lavoro, l'AI non ricorda cosa ha fatto all'inizio.

**Perché:** Context window limitato, compressione perde dettagli.

**Come evitare:**
- Task grandi = sessioni separate
- Ogni sessione: prompt con contesto completo (non "continua prima")
- Usa file di stato: `echo "Step 1 done, modified app.js" > /tmp/progress.txt`

---

## Problema 7: "L'AI ha fatto output ingannevole"

**Cosa succede:** L'AI dice "ho convertito 50 file" ma ne ha convertiti 5.

**Perché:** L'AI ottimista, o conta i file toccati non quelli convertiti davvero.

**Come evitare:**
- Verifica: `grep -l '\$CRUD->Page' *.php | wc -l` → conta reali
- Non fidarti del numero che dice l'AI

---

## Problema 8: "L'AI ha ignorato errori PHP"

**Cosa succede:** L'AI genera codice con warning/notices che lui non vede.

**Perché:** L'AI non esegue il codice, lo scrive.

**Come evitare:**
- Dopo ogni fix: `php -l file.php`
- Se possibile: `php -d error_reporting=E_ALL file.php` e guarda output
- Usa `grep -n "undefined\|warning\|notice" output.log`

---

## Problema 9: "L'AI ha fatto codice non manutenibile"

**Cosa succede:** Funziona ma è un groviglio di codice incomprensibile.

**Perché:** L'AI ottimizza per "funziona ora", non per "si capisce tra 6 mesi".

**Come evitare:**
- Vincolo: "Codice leggibile, commenti in italiano per logica business"
- Dopo: leggi il codice tu, se non capisci → riscrivi tu
- Non accettare codice che non sai spiegare

---

## Problema 10: "L'AI ha perso ore per un fix semplice"

**Cosa succede:** L'AI prova 20 approcci per un bug che si fixava in 2 righe.

**Perché:** L'AI non ha il "feeling" del bug. Prova tutto sistematicamente.

**Come evitare:**
- Se dopo 3 tentativi non ha risolto: fermalo
- Diagnosi tu: leggi il codice, capisci il bug, dai la soluzione specifica
- Usa l'AI per eseguire, non per diagnosticare
