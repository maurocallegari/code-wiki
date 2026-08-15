# Current Best Practices · 2026

Questa è una baseline pratica per i tuoi repo nel 2026. Non inseguire ogni nuova skill: riduci contesto, aumenta prove e tieni versionate le istruzioni vicine al codice.

> [!WARNING] Modelli, provider, flag e prezzi cambiano. Verifica documentazione e `--help` prima di cambiare configurazione; questa pagina definisce criteri, non nomi “migliori per sempre”.

## La configurazione consigliata

| Area | Scelta | Perché |
|---|---|---|
| istruzioni | `AGENTS.md` corto nel repo + skill dettagliate | il contesto locale vince sui prompt ripetuti |
| modello | forte reasoning per diagnosi/review; coding agent per patch | separa decisione ed esecuzione |
| sandbox | read-only per audit, workspace-write per patch | minimo privilegio |
| rete | spenta salvo dipendenza necessaria | meno esfiltrazione e variabilità |
| verifiche | comandi deterministici + test funzionale | “sembra corretto” non è evidenza |
| batch | 3 file, poi 5–10 se la matrice passa | limita fallimenti parziali |
| stato | Kanban + Git, non memoria chat | ripartenza affidabile |
| deploy | artifact/diff esatto, rollback pronto, smoke test | FTP non equivale a live aggiornato |

## Come scegliere modello/provider

1. Usa il modello più capace disponibile per comprendere STH, diagnosticare e fare review di modifiche rischiose.
2. Usa un agente coding con strumenti e sandbox per patch e batch ripetitivi.
3. Preferisci contesto lungo solo se serve davvero; un reference preciso batte migliaia di file irrilevanti.
4. Valuta sul tuo benchmark: 5 task reali STH/Insta, scope corretto, test passati, costo e tempo.
5. Non cambiare provider nel mezzo di un batch; rende i risultati difficili da confrontare.

## Tooling minimo

```text
Git + rg + parser/linter del linguaggio + test applicativi
Codex CLI nel repo
Hermes per orchestrazione, Kanban, cron e deploy
AGENTS.md per ogni repo
Skill: sth-conventions, repo-import, verified-deploy, kanban-daily
```

Rimuovi skill sovrapposte, skill “magiche” senza verifiche e integrazioni che hanno accesso remoto ma non vengono usate. Ho provato ad accumulare tool: il router sceglieva la skill quasi giusta invece di quella specifica.

## Sicurezza e supply chain

- pinna versioni e lockfile quando il progetto li supporta
- esamina script di installazione prima di eseguirli
- segreti solo in secret store o `.env` ignorato
- niente dump DB o log clienti nei prompt esterni
- limita plugin/connector ai permessi necessari
- conserva audit trail di comandi e deploy

> [!DANGER] Un tool AI con shell, rete e segreti è un amministratore. Non concedere le tre cose insieme se il task richiede solo una patch locale.

## Cron 2026: pochi e osservabili

- disk/inode check giornaliero su claw.nswr.it
- backup giornaliero con restore test periodico
- Kanban digest nei giorni lavorativi
- repository hygiene report settimanale
- dependency/security review periodica, senza auto-upgrade del legacy
- rotazione log basata su dimensione/retention

Ogni job deve avere path assoluti, lock contro doppia esecuzione, timeout, log e alert su exit code. Nessun job deve cancellare automaticamente file sconosciuti.

## Definition of Done automatizzabile

```bash
# WHY: scope, patch hygiene e sintassi sono controlli ripetibili.
git status --short
git diff --check
git diff --name-only -- '*.php' | while IFS= read -r f; do php -l "$f"; done

# WHY: cerca modifiche nelle zone STH proibite; qualsiasi output blocca il task.
git diff --name-only | rg '(^|/)([^/]+_old|app|plugins)/|action/crud/functions\.php'
```

Completa con un test browser/API specifico. Un exit code zero non dimostra che il filtro A–Z sia usabile con un dito.

## Riesame trimestrale

- quali skill sono state realmente invocate?
- quali verifiche hanno trovato bug prima del deploy?
- quali cron hanno fallito silenziosamente?
- quale modello ha rispettato meglio lo scope sui 5 task benchmark?
- quali accessi non servono più?

Archivia ciò che non dà evidenza. La configurazione migliore è quella che puoi spiegare e verificare.
