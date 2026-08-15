# Cron Jobs

> 8 job automatizzati che girano ogni giorno.

---

## Lista Job

| ID | Nome | Frequenza | Stato |
|----|------|-----------|-------|
| 5972e4d87e1a | hermes-backup | settimanale (dom 3am) | ✓ |
| 5bac2af95480 | clawy-workspace-sync | 3x/giorno | ✓ |
| 4b4be6db9dd1 | clawy-weekly-research | settimanale (lun) | ✓ |
| 6d187cacf972 | clawy-reddit-scan | 2x/giorno | ✓ |
| dba0807bfb06 | Clawy Reddit sync | 3x/giorno | ✓ |
| 27e82d9da818 | Clawy Finance sync | 2x/giorno | ⚠️ (errore) |
| e92ede7e1113 | Clawy Idee proattive | 2x/giorno | ✓ |
| 2cea120af83e | Claw backup giornaliero | giornaliero (3am) | ✓ |

---

## Come Gestirli

```bash
# Lista
cronjob list

# Dettaglio job
cronjob action=show job_id=5972e4d87e1a

# Esegui ora
cronjob action=run job_id=5972e4d87e1a

# Pausa
cronjob action=pause job_id=5972e4d87e1a

# Riprendi
cronjob action=resume job_id=5972e4d87e1a

# Elimina
cronjob action=remove job_id=5972e4d87e1a
```

---

## Come Crearne Uno Nuovo

```bash
cronjob action=create \
  schedule="30 9 * * *" \
  prompt="Cosa devo fare oggi?" \
  name="daily-brief"
```

---

## Errori

Se un job fallisce, Hermes ti avvisa. L'ultimo output è in `~/.hermes/cron/output/<id>/`.

---

## Prossimo

Vai a [Deploy FTP](deploy.md).
