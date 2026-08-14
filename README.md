# Code Wiki — Guida Operativa Hermes + Codex

> **Per Mauro** — Stealth Software, Treviso
> Stack: PHP/MySQL legacy (STH Assitec), Insta 2.0, Dashboard, Trading
> **Obiettivo:** diventare produttivo con Hermes + Codex, senza farsi prendere in giro.

---

## 🚀 Start Here

| Leggi questo... | Se vuoi... |
|---|---|
| [STYLE.md](STYLE.md) | Capire il tuo stile (impara a scrivere come te) |
| [COURSE.md](COURSE.md) | Il corso pratico (tutorial, non reference) |
| [01-hermes-vs-codex.md](01-hermes-vs-codex.md) | Quando usare quale |
| [02-prompt-engineering.md](02-prompt-engineering.md) | Scrivere prompt che funzionano |
| [05-checklist-verifica.md](05-checklist-verifica.md) | Verificare output AI |

---

## Struttura

### Metodo (leggi in quest'ordine)
1. **[STYLE.md](STYLE.md)** — Il tuo stile di coding (regole d'oro)
2. **[COURSE.md](COURSE.md)** — Il corso pratico (fai insieme a me)

### Reference (consulta quando serve)
3. **[01-hermes-vs-codex.md](01-hermes-vs-codex.md)** — Quando usare Hermes vs Codex
4. **[02-prompt-engineering.md](02-prompt-engineering.md)** — Prompt efficaci
5. **[03-workflow-fix-bug.md](03-workflow-fix-bug.md)** — Fix bug verificato
6. **[04-workflow-new-feature.md](04-workflow-new-feature.md)** — Feature dalla A alla Z
7. **[05-checklist-verifica.md](05-checklist-verifica.md)** — Non fidarti del "fatto"
8. **[06-codex-batch.md](06-codex-batch.md)** — Batch conversione T2
9. **[07-problemi-comuni.md](07-problemi-comuni.md)** — Errori che ho fatto io e tu

### Config & Tools
10. **[08-config-hermes.md](08-config-hermes.md)** — Config Hermes ottimizzata
11. **[09-skills-utili.md](09-skills-utili.md)** — Skills utili vs inutili
12. **[10-cron-setup.md](10-cron-setup.md)** — Cron jobs attivi
13. **[11-deploy-ftp.md](11-deploy-ftp.md)** — Deploy FTP verificato
14. **[12-trading-finance.md](12-trading-finance.md)** — Trading tools

---

## Come usare

1. **Copia STYLE.md** nella root dei tuoi progetti (o rimanda ad esso)
2. **In ogni task AI**, inizia con: `Leggi /home/clawy/dev/code-wiki/STYLE.md`
3. **Usa COURSE.md** come guida pratica per ogni task
4. **Verifica SEMPRE** con le checklist

---

## Il mio stile in 3 frasi

1. Bootstrap: `require_once($_SERVER['DOCUMENT_ROOT'].'/configure.php')` sempre
2. DB: `Tab_*` solo in trigger, `IS_*` per flag, `ID`+Tabella per FK
3. Forms: `Form` helper con `ParametriForm` serialized, non prepared statements

---

## GStack installato

Skills gstack (30) in `~/.hermes/skills/gstack-*/`. Usa:
- `/office-hours` per nuova feature
- `/review` per review codice
- `/qa` per test su URL
- `/careful` prima di comandi pericolosi
- `/investigate` per debug sistematico
- `/cso` per audit sicurezza
- `/ship` per deploy con test
- `/autoplan` per plan automatico

---

## Note

- Tutti i prompt sono in **italiano** per Hermes, **inglese** per Codex
- Ogni workflow include checklist di verifica
- Esempi reali dai tuoi progetti (Insta, Dashboard, T2, trading)
