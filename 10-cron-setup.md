# Cron Jobs Attivi

## TL;DR

8 cron attivi. Verifica con `cronjob list`.

---

## Lista cron

| Job | Frequenza | Cosa fa |
|-----|-----------|---------|
| hermes-backup | settimanale (dom 3am) | Backup Hermes state |
| clawy-workspace-sync | 3x/giorno (6:30, 12:30, 17:30) | Sync dashboard |
| clawy-weekly-research | settimanale (lun 6:30) | Ricerca utile |
| clawy-reddit-scan | 2x/giorno (6:30, 21:30) | Aggiorna Reddit |
| Clawy Reddit sync | 3x/giorno (9, 15, 21) | Commenti proattivi |
| Clawy Finance sync | 2x/giorno (9, 21) | Verdetto mercati |
| Clawy Idee proattive | 2x/giorno (9, 18) | 10 idee nuove |
| Claw backup giornaliero | giornaliero (3am) | Backup completo |

---

## Come aggiungere un cron

```bash
# Via Hermes
cronjob action=create \
  schedule="30 9 * * *" \
  prompt="Cosa devo fare oggi?" \
  name="daily-brief"
```

---

## Cosa rimuovere (duplicati)

- `clawy-reddit-scan` + `Clawy Reddit sync` → unificare in uno
- Considerare di ridurre `clawy-workspace-sync` a 2x/giorno

---

## Monitoraggio

```bash
# Vedi output ultimo run
cronjob action=job_id

# Se un cron fallisce, Hermes ti avvisa
```
