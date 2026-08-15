# Tutorial: Nuova Feature

> Esercizio pratico: aggiungere una feature reale.

---

## Scenario

**Feature:** Aggiungere un nuovo visual style "Neon" alle slide di Insta 2.0.

---

## Step 1: Pianifica

```
Cosa: Visual style "Neon" con glow effect colorato
Dove: dev/insta/assets/css/visual-styles2.css
Integrazione: CSS only, no JS
Vincoli: non toccare altri visual styles
```

---

## Step 2: Definisci per Hermes

```
FILE: dev/insta/assets/css/visual-styles2.css

TASK: Aggiungi un nuovo visual style "NeON" per le slide.

INPUT: Gli stili esistenti usano classi .cds-* (cds-title, cds-body, cds-visual). Lo style "Neon" deve avere:
- Glow effect colorato su testo e bordi
- Palette: cyan (#00ffff), magenta (#ff00ff), lime (#00ff00)
- Compatibile con slide 16:9 e 1:1

OUTPUT: Nel file visual-styles2.css, aggiungi:
- .cds-neon-title (testo con text-shadow glow)
- .cds-neon-body (testo secondario glow)
- .cds-neon-border (bordo con box-shadow glow)
- .cds-neon-glow (classe utility per glow generico)

VINCOLI:
- Non modificare gli altri visual styles
- Usa solo CSS, no JS
- Stile coerente con il dark glass esistente

VERIFIA:
- Apri dev/insta/slide-preview.html
- Seleziona "Neon" dalla lista
- Verifica glow effect su titolo e slide
```

---

## Step 3: Verifica Output

```bash
# Pattern attesi
grep -c "\.cds-neon" dev/insta/assets/css/visual-styles2.css  # > 0
grep -c "text-shadow" dev/insta/assets/css/visual-styles2.css  # > 0
grep -c "box-shadow" dev/insta/assets/css/visual-styles2.css  # > 0

# Visuale: apri slide-preview.html
```

---

## Step 4: Deploy

```bash
# Bump versione
sed -i "s/APP_VERSION = '.*'/APP_VERSION = '$(date +%Y%m%d)b'/" dev/insta/index.php

# Deploy
# Verifica live
```

---

## Cosa Hai Imparato

1. Come pianificare una feature
2. Come definire input/output chiari
3. Come verificare il risultato
4. Il flusso completo

---

## Prossimo Esercizio

Vai a [Batch Conversione](tutorial-batch.md) per convertire più file in una volta.
