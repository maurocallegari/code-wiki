# Code Wiki — Guida Operativa Hermes + Codex

> **Per Mauro** — Stealth Software, Treviso
> Stack: PHP/MySQL legacy, Insta 2.0, trading, Hermes, Codex, Raspberry Pi
> **Obiettivo:** diventare bravissimo e produttivo con Hermes + Codex, senza farsi più prendere in giro.

---

## 🚀 Start Here

| Leggi questo... | Se vuoi... |
|---|---|
| [01-hermes-vs-codex.md](01-hermes-vs-codex.md) | Capire quando usare quale |
| [02-prompt-engineering.md](02-prompt-engineering.md) | Scrivere prompt che funzionano |
| [03-workflow-fix-bug.md](03-workflow-fix-bug.md) | Fix bug senza rompere tutto |
| [04-workflow-new-feature.md](04-workflow-new-feature.md) | Nuova feature end-to-end |
| [05-checklist-verifica.md](05-checklist-verifica.md) | Verificare output AI (non fidarti) |
| [06-codex-batch.md](06-codex-batch.md) | Batch conversione tipo T2 |
| [07-problemi-comuni.md](07-problemi-comuni.md) | Errori che ho fatto e come evitarli |
| [08-config-hermes.md](08-config-hermes.md) | Config Hermes ottimizzata |
| [09-skills-utili.md](09-skills-utili.md) | Skills che uso davvero vs inutili |
| [10-cron-setup.md](10-cron-setup.md) | Cron jobs che girano |
| [11-deploy-ftp.md](11-deploy-ftp.md) | Deploy FTP verificato |
| [12-trading-finance.md](12-trading-finance.md) | Trading tools e cron |

---

## Struttura

- **01-07**: guide operative (workflow, prompt, problemi)
- **08-12**: reference (config, skills, cron, deploy, trading)

---

## Come usare

1. Parti da `01-hermes-vs-codex.md`
2. Leggi `02-prompt-engineering.md`
3. Usa `03-04` per i workflow
4. **Sempre** controlla `05-checklist-verifica.md` dopo ogni output AI

---

## GStack installato

Skills gstack (30) installate in `~/.hermes/skills/gstack-*/`. Usa:
- `/office-hours` per nuova feature
- `/review` per review codice
- `/qa` per test su URL
- `/ship` per deploy con test
- `/careful` prima di comandi pericolosi
- `/investigate` per debug sistematico
- `/cso` per audit sicurezza

---

## Note

- Tutti i prompt sono in **italiano** per Hermes, **inglese** per Codex
- Ogni workflow include checklist di verifica
- Esempi reali dai tuoi progetti (Insta, Dashboard, T2, trading)
