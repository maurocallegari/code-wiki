# Setup Hermes

Hermes è l’orchestratore: riceve il task in italiano, raccoglie contesto, gestisce server/cron/Kanban/deploy e chiama Codex quando serve scrivere codice.

> [!WARNING] I nomi dei comandi Hermes cambiano tra installazioni. Prima esegui `hermes --help` e il sotto-comando `--help`; non copiare sintassi non confermata da questa wiki.

## Configurazione minima

Tieni separati configurazione versionabile e segreti. Ho provato a mettere tutto in un file comodo: il backup ha finito per contenere credenziali.

```yaml
# ~/.hermes/config.yaml
# WHY: la directory di lavoro limita dove l’orchestratore deve operare.
workspace: /home/clawy/dev

# WHY: italiano per il dialogo; i prompt di coding possono restare in inglese.
language: it

# WHY: conferma esplicita prima di deploy, delete o operazioni remote.
confirm_destructive_actions: true
```

```bash
# ~/.hermes/.env — NON versionare
# WHY: i file pubblici e configure.php non devono contenere segreti.
FTP_HOST=...
FTP_USER=...
FTP_PASSWORD=...
```

## Personalità operativa

Inserisci queste regole nel file di istruzioni supportato dalla tua installazione:

```text
Sei l’orchestratore operativo di Mauro.
Parla italiano, sii diretto e non dichiarare “fatto” senza prove.
Prima di scrivere: mostra file in scope e modifiche preesistenti.
Per codice puro usa Codex con workspace ristretto.
Per STH applica la skill sth-conventions.
Non eseguire deploy, delete o chmod senza target risolto e conferma.
Al termine riporta: diff, comandi eseguiti, output, test live ancora necessario.
```

## Skill essenziali

| Skill | Tienila? | Perché |
|---|---:|---|
| `sth-conventions` | sì | protegge naming, CRUD e legacy |
| `verified-deploy` | sì | trasforma deploy in checklist con rollback |
| `repo-import` | sì | crea inventario e `AGENTS.md` |
| `kanban-daily` | sì | impone una sola fonte per lo stato |
| skill generiche duplicate | no | aumentano ambiguità e contesto |
| skill non usate da 60–90 giorni | archivia | meno routing errato, meno manutenzione |

Vedi [Creare le tue skill](creating-skills.md) per implementare la prima.

## Cron che meritano di esistere

| Frequenza | Job | Controllo |
|---|---|---|
| giornaliera | spazio disco e inode su claw.nswr.it | alert prima della soglia scelta |
| giornaliera | backup configurazioni, senza segreti in chiaro | restore testato |
| giornaliera | riepilogo Kanban bloccati/in verifica | nessuna modifica automatica |
| settimanale | repository sporchi e branch abbandonati | report, non delete |
| settimanale | link e sintassi della wiki | exit code non zero in caso di errore |

```cron
# WHY: il monitoraggio è read-only; non cancella file per “risolvere” il disco.
15 7 * * * /path/assoluto/check-disk.sh >> /path/assoluto/log/check-disk.log 2>&1

# WHY: un report giornaliero mantiene il Kanban utile senza promuovere task da solo.
30 7 * * 1-5 /path/assoluto/kanban-digest.sh >> /path/assoluto/log/kanban.log 2>&1
```

> [!DANGER] Non mettere password nella crontab e non creare cron di pulizia con `rm -rf`. Prima misura, poi archivia/elimina con una procedura separata e verificata.

## Prompt quotidiano

```text
Crea/aggiorna il task Kanban per [risultato].
Ispeziona il repo senza modificare e proponi file in scope, rischi e verifiche.
Se il piano è circoscritto, fai eseguire il codice a Codex.
Applica le convenzioni STH e blocca qualsiasi file protetto.
Non deployare. Restituisci diff e prove per la mia verifica.
```

## Check finale

- `hermes --help` e diagnostica disponibile passano
- workspace ristretto a directory reali
- segreti esclusi da Git e backup pubblici
- conferma richiesta per mutazioni remote
- cron usa path assoluti, lock e log ruotati
- Telegram non riceve segreti o dump completi
