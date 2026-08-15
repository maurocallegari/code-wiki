# Setup GStack

> 30 skills per workflow: review, QA, deploy, debug.

---

## Skills Installati

```bash
ls ~/.hermes/skills/gstack* | wc -l
# 30
```

## Skills più Utili per Te

| Skill | Descrizione | Quando usarlo |
|-------|-------------|---------------|
| `/office-hours` | Domande forzate per capire il problema | Prima di ogni nuova feature |
| `/plan-ceo-review` | Review strategica dell'idea | Valutare se vale la pena implementare |
| `/review` | Review tecnica del codice | Dopo ogni batch di modifiche |
| `/qa` | Test su URL reale | Prima di deploy |
| `/ship` | Deploy con test | Quando il codice è pronto |
| `/autoplan` | Plan automatico CEO→design→eng | Per feature complesse |
| `/careful` | Guardia per comandi distruttivi | Prima di `rm -rf`, `force-push` |
| `/investigate` | Debug sistematico | Quando il bug non si trova |
| `/freeze` | Blocca edit a una directory | Mentre debug, per evitare modifiche accidentali |
| `/retro` | Review settimanale | Ogni venerdì |

---

## Come Usarli

### Da Chat Telegram

Scrivi naturalmente:
- "Fammi una review di questo branch" → attiva `/review`
- "Devo testare staging" → attiva `/qa`
- "Sto per fare rm -rf, sii careful" → attiva `/careful`

### Da Terminale

```bash
# Se le skills sono integrate come comandi
hermes chat -q "/review branch=main"
```

---

## Prossimo

Vai a [Setup gbrain](setup-gbrain.md).
