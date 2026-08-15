# Problemi Comuni

> Errori che ho fatto io e tu, e come non ripeterli.

---

## 1. "L'AI ha detto funziona ma non funziona"

**Perché:** L'AI non ha un ambiente reale. "Funziona" spesso significa "non ha dato errore evidente".

**Come evitare:**
- Chiedi SEMPRE: "Come hai verificato?"
- Verifica tu stesso su URL reale
- Non accettare "dovrebbe funzionare" — solo "ho testato con X e Y, ecco output"

---

## 2. "L'AI ha modificato file che non doveva"

**Perché:** L'AI cerca di essere utile, fa modifiche "migliorative" non richieste.

**Come evitare:**
- Vincolo esplicito: "NON toccare [file]"
- Dopo il fix: `git diff --stat` per vedere cosa ha toccato
- Se ha toccato troppo: `git checkout -- [file]` e ripeti con vincoli più stretti

---

## 3. "L'AI ha ignorato i vincoli"

**Perché:** L'AI non capisce il contesto profondo. "T2" per lui è una parola, non un pattern specifico.

**Come evitare:**
- Fornisci esempio di riferimento (file T2 perfetto)
- Dopo il fix: verifica che i pattern sbagliati siano assenti
- Se fallisce: correggi tu manualmente e aggiungi l'errore come esempio negativo nel prompt

---

## 4. "L'AI ha rotto qualcos'altro"

**Perché:** L'AI non ha visione d'insieme. Modifica locale senza considerare impatto globale.

**Come evitare:**
- Dopo ogni fix: testa TUTTE le funzionalità toccate, non solo quella fixata
- Usa `git diff` per vedere l'entità della modifica
- Se la modifica è > 50 righe: sospetta, chiedi di spezzettare

---

## 5. "L'AI ha fatto troppo"

**Perché:** L'AI cerca di essere "completa", fa refactor non richiesti.

**Come evitare:**
- Vincolo: "Modifica solo [file specifico]"
- Vincolo: "Non fare refactor, solo feature X"
- Dopo: `git diff --stat` — se > 3 file modificati, qualcosa è andato storto

---

## 6. "L'AI ha dimenticato il contesto"

**Perché:** Context window limitato, compressione perde dettagli.

**Come evitare:**
- Task grandi = sessioni separate
- Ogni sessione: prompt con contesto completo (non "continua prima")
- Usa file di stato: `echo "Step 1 done" > /tmp/progress.txt`

---

## 7. "L'AI ha fatto output ingannevole"

**Perché:** L'AI ottimista, o conta i file toccati non quelli convertiti davvero.

**Come evitare:**
- Verifica: `grep -l 'pattern' *.php | wc -l` → conta reali
- Non fidarti del numero che dice l'AI

---

## 8. "L'AI ha ignorato errori PHP"

**Perché:** L'AI non esegue il codice, lo scrive.

**Come evitare:**
- Dopo ogni fix: `php -l file.php`
- Se possibile: `php -d error_reporting=E_ALL file.php` e guarda output
- Usa `grep -n "undefined\|warning\|notice" output.log`

---

## 9. "L'AI ha fatto codice non manutenibile"

**Perché:** L'AI ottimizza per "funziona ora", non per "si capisce tra 6 mesi".

**Come evitare:**
- Vincolo: "Codice leggibile, commenti in italiano per logica business"
- Dopo: leggi il codice tu, se non capisci → riscrivi tu
- Non accettare codice che non sai spiegare

---

## 10. "L'AI ha perso ore per un fix semplice"

**Perché:** L'AI non ha il "feeling" del bug. Prova 20 approcci sistematicamente.

**Come evitare:**
- Se dopo 3 tentativi non ha risolto: fermalo
- Diagnosi tu: leggi il codice, capisci il bug, dai la soluzione specifica
- Usa l'AI per eseguire, non per diagnosticare
