# Problemi comuni

Dieci errori già abbastanza costosi da meritare una regola.

## 1. “Ha detto fatto”

**Causa:** il modello ha completato la patch, non il test. **Fai:** chiedi comandi, output e test live. Se non può aprire il browser, scrive “non verificato”.

## 2. File fuori scope

**Causa:** “già che ci sei” o refactor opportunistico. **Fai:** lista chiusa di file e `git diff --name-only`. Non usare `git checkout --` alla cieca: potresti cancellare lavoro tuo.

## 3. Vincoli dimenticati

**Causa:** chat lunga/compressione. **Fai:** `AGENTS.md` + skill + vincoli ripetuti nel task. Un reference reale è obbligatorio per T2.

## 4. Batch troppo grande

**Causa:** timeout e conversioni parziali. **Fai:** 3 file omogenei, matrice per file, stop al primo fallimento; poi 5–10.

## 5. STH “modernizzato”

**Causa:** il modello vede `mysql_*` e presume debito. **Fai:** scrivi che è uno shim e verifica il diff. Non aggiungere FK fisiche e non scrivere `Tab_*` dal PHP.

## 6. Diagnosi dopo venti tentativi

**Causa:** modifica prima di riprodurre. **Fai:** audit read-only, baseline, un’ipotesi alla volta. Dopo tre ipotesi fallite, torna ai dati e riduci il problema.

## 7. Mobile “responsivo” ma inutilizzabile

**Causa:** DevTools largo o controllo visto, non toccato. **Fai:** 320/375/414 px, touch target, scroll interno, nessun overflow pagina, test su iPhone quando conta.

## 8. FTP riuscito, live vecchio

**Causa:** target errato, cache, opcache o upload parziale. **Fai:** manifest, marker/checksum, bump versione, curl e test funzionale.

## 9. Disco claw.nswr.it pieno

**Causa:** `.bak`, log, cache e mirror indiscriminati. **Fai:** misura `df -h`/`df -i`, identifica directory, escludi temporanei dal deploy, ruota log. Nessuna cancellazione automatica.

## 10. Kanban decorativo

**Causa:** task resta in chat o va “Fatto” alla fine della patch. **Fai:** card prima del lavoro, output durante, “Verifica” prima del deploy, “Fatto” solo dopo live.

> [!TIP] Quando un errore si ripete, non scrivere un prompt più arrabbiato. Trasformalo in una regola verificabile di `AGENTS.md`, una skill o un controllo automatico.

## Triage in 60 secondi

```bash
# WHY: prima separa modifiche tue, dell’agente e file non tracciati.
git status --short
git diff --stat
git diff --check

# WHY: trova errori sintattici PHP nei file modificati.
git diff --name-only -- '*.php' | while IFS= read -r f; do php -l "$f"; done

# WHY: verifica zone STH protette; qualsiasi output richiede stop e review.
git diff --name-only | rg '(^|/)([^/]+_old|app|plugins)/|action/crud/functions\.php'
```
