# Trading Tools e Cron

## TL;DR

Usa Hermes per trading: cron finance, alert Telegram, journal.

---

## Setup attuale

1. **Finance Cron** — 2x/giorno prende prezzi Yahoo, verdetto COMPRA/MONITORA/EVITA
2. **Position Size Calculator** — tool standalone per dimensionare trade
3. **Exit Calculator** — target/stop per gain veloci
4. **Trading Pack PWA** — catena position+exit in 1 PWA installabile

---

## Come usarli

### Finance Cron
- Si aggiorna automaticamente alle 9:00 e 21:00
- Mostra verdetto + attendibilità
- Dashboard: `claw.nswr.it` → sezione Mercati

### Position Size
- Tool: `claw.nswr.it/demo/position-size.php`
- Input: capitale, rischio %, entry, stop
- Output: size ottimale, valore per pip, P/L atteso

### Exit Calculator
- Tool: `claw.nswr.it/demo/exit-calc.html`
- Input: entry, target, stop, size
- Output: P/L, risk/reward, % gain/loss

---

## Best practices trading con AI

1. **Mai eseguire trade solo su verdetto AI** → verifica sempre tu
2. **Position size max 2% del capitale** → mai più
3. **Stop loss sempre impostato** → senza eccezioni
4. **Journal** → annota ogni trade con motivazione
5. **Review settimanale** → `/retro` per analizzare performance

---

## Possibili miglioramenti

- [ ] Alert Telegram quando verdetto=COMPRA
- [ ] Journal automatico trade eseguiti
- [ ] Backtesting strategie su dati storici
- [ ] Correlazione mercati → portfolio
- [ ] Report settimanale performance
