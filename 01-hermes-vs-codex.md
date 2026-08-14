# Hermes vs Codex — Quando usare quale

## TL;DR

| Strumento | Usalo per... | Evita per... |
|---|---|---|
| **Hermes** | Orchestrazione, deploy, fix complessi, ricerche, cron | Batch coding puro |
| **Codex** | Codice puro, batch, conversioni, refactoring | Operazioni multistep che richiedono contesto |

---

## Hermes → L'orchestratore

**È il tuo capo team.** Gli scrivi in italiano (o inglese), lui:
- Capisce il contesto (ha memoria, skills, cron)
- Scrive codice tramite terminal tool
- Delega a Codex quando serve coding pesante
- Fa deploy FTP
- Fa ricerche web
- Gestisce cron

**Quando usarlo:**
- Fix bug in un progetto che conosce
- Nuova feature che tocca più file (Insta, Dashboard)
- Deploy su claw.nswr.it
- Ricerca hotel / lead generation
- Gestione task ripetitivi (meteo, reddit, finance)
- Tutto ciò che richiede "contesto" di quello che fai

**Come scrivergli:**
```
 italiano va bene
```
Esempio: "Fixa il bug del countdown in Insta 2.0 — non parte il timer quando metto durata custom"

---

## Codex → L'esecutore

**È il tuo senior developer on-demand.** Lo chiami per coding puro. Gliscrivi in INGLESE, lui esegue subito.

**Quando usarlo:**
- Conversione batch (tipo T2: 100 file PHP → framework)
- Refactoring ripetitivo
- Fix su molti file con pattern prevedibile
- Codice standalone (non serve contesto di tutto il progetto)

**Come chiamarlo da Hermes:**
```bash
codex exec --sandbox workspace-write "Convert this PHP page to T2 CRUD format: [file]"
```

**Come chiamarlo da solo (CLI):**
```bash
cd ~/dev/insta
codex --sandbox workspace-write exec "Add dark mode toggle to slide preview"
```

---

## Il flusso ottimale

Tu scrivi a Hermes (italiano) → Hermes capisce → Se serve batch coding, Hermes genera prompt INGLESE e li passa a Codex → Codex esegue → Hermes verifica.

**Esempio reale — Fix T2 Assitec:**

1. Tu: "Converti la pagina agenti in T2"
2. Hermes: legge la pagina, capisce la struttura, delega a Codex con prompt specifico
3. Codex: genera il codice T2
4. Hermes: verifica che esista `$CRUD->Page` e non `<div>` legacy
5. Se OK → deploy. Se no → retry con feedback.

---

## Lingua

| Strumento | Lingua migliore | Perché |
|---|---|---|
| **Hermes** | 🇮🇹 Italiano | Lo parli bene, è più veloce |
| **Codex** | 🇬🇧 Inglese | Training data più ampio, coding meglio in EN |

**Nota:** Codex in italiano funziona ma per coding tecnico l'inglese dà risultati migliori (terminologia, pattern, nomi variabili).

---

## Errori che ho fatto

1. **Chiedere a Hermes di convertire 100 file in una volta** → si blocca, perde contesto. → **Spezzare in batch di 5-10 file**
2. **Chiedere a Codex di deployare** → non sa come, non ha le credenziali FTP. → **Hermes per deploy, Codex per codice**
3. **Scrivere prompt vaghi** → "migliora il sito" non significa nulla. → **Prompt specifici con contesto e output atteso**
4. **Non verificare l'output** → "convertito" ma è un wrapper. → **Checklist obbligatoria (vedi 05-checklist-verifica.md)**

---

## Riepilogo decisionale

```
Devo scrivere codice puro (batch, conversione, refactoring)?
  SÌ → Codex (inglese)
  NO  → Continua

Devo toccare più progetti, fare deploy, cercare online?
  SÌ → Hermes (italiano)
  NO  → Continua

Devo delegare con supervisione (subagent)?
  SÌ → Hermes + Codex (Hermes genera prompt, verifica output)
  NO  → Solo Hermes
```
