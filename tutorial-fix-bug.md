# Tutorial: Fix Bug

> Esercizio pratico: fixare un bug reale seguendo il metodo.

---

## Scenario

**Bug:** Nella lista agenti di STH Assitec, il filtro A-Z non scorre su mobile.

---

## Step 1: Isola

```bash
# Trova il file
grep -r "filtro" dev/sth-assitec-gpt/view/agenti/agenti_lista.php | head -5

# Guarda il codice
head -60 dev/sth-assitec-gpt/view/agenti/agenti_lista.php
```

---

## Step 2: Capisci

Il filtro A-Z è nell'array `$Left` con 26 elementi. Su mobile le colonne si riducono ma il filtro potrebbe non essere scrollabile.

---

## Step 3: Prompt per Hermes

```
FILE: dev/sth-assitec-gpt/view/agenti/agenti_lista.php

TASK: Su mobile, il filtro A-Z (array $Left) non è utilizzabile perché troppo largo per lo schermo.

INPUT: Il filtro ha 26 link (A-Z) in linea. Su mobile si sovrappongono.

OUTPUT: Aggiungi un container scrollabile orizzontalmente per il filtro su mobile. Il filtro deve restare visibile e utilizzabile.

VINCOLI:
- Modifica SOLO la sezione $Left
- Non toccare Table, Header, Sidebar
- Stile inline CSS accettato (come da convenzione)
- Non toccare logica PHP dei link (Active, Link, Label)

VERIFICA:
- Aprire la pagina su mobile (Chrome DevTools → dispositivo)
- Il filtro deve scorrere orizzontalmente
- I link A-Z devono restare cliccabili
```

---

## Step 4: Verifica Output

```bash
# Sintassi
php -l dev/sth-assitec-gpt/view/agenti/agenti_lista.php

# Pattern atteso
grep -n "overflow-x\|white-space:nowrap" dev/sth-assitec-gpt/view/agenti/agenti_lista.php

# Visuale (apri nel browser)
```

---

## Step 5: Deploy

```bash
# Bump versione (se applicabile)
# Deploy FTP
# Verifica live
```

---

## Cosa Hai Imparato

1. La formula FILE/TASK/OUTPUT/VINCOLI/VERIFICA
2. Come isolare un bug
3. Come verificare l'output
4. Il flusso completo

---

## Prossimo Esercizio

Vai a [Nuova Feature](tutorial-new-feature.md) per aggiungere una funzionalità.
