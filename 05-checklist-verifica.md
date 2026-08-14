# Checklist Verifica Output AI

> **Regola dura:** non fidarti MAI del "fato". Verifica SEMPRE.

## Perché

Gli AI agent (Hermes, Codex) dicono spesso "ho fatto" o "finito" senza che sia vero. Non mentono — pensano di aver fatto, ma:
- Il codice è un wrapper non una fix reale
- Hanno ignorato un vincolo
- Hanno modificato file sbagliati
- L'output sembra giusto ma non funziona

---

## Checklist post-AI (prima di dire "fato")

### Verifica sintassi
```bash
# PHP
php -l file.php          # syntax check
python3 -m py_compile file.py  # python

# JS/JSON
node -e "JSON.parse(require('fs').readFileSync('file.json'))"  # valid JSON
```

### Verifica contenuto
```bash
# Cerca pattern che NON dovrebbero esserci
grep -n "mysql_query" file.php       # se uso mysqli, non deve esserci
grep -n "<div.*class=\"legacy\"" file.html  # se uso T2, non deve esserci
grep -n "TODO\|FIXME\|XXX" file.js   # codice incompiuto
```

### Verifica differenze (se sapevi cosa doveva cambiare)
```bash
# Confronta con prima
git diff file.php
# O se non-git, confronta con backup
diff file.php.bak file.php
```

### Verifica funzionale (la più importante)
```bash
# Se è un'API/endpoint
curl -s "https://claw.nswr.it/api/demo.php" | head -c 200
# Atteso: JSON valido, non 404 o 500

# Se è un comando
./script.sh  # esegui e guarda output

# Se è una pagina web
browser_navigate("https://claw.nswr.it/demo/json-tree/")
# Guarda visualmente, interagisci
```

### Verifica vincoli
```bash
# Se ho detto "non toccare style.css"
git diff --stat style.css  # deve essere vuoto

# Se ho detto "zero dipendenze"
grep -c "<script src=\"http" file.html  # deve essere 0
```

---

## Esempio reale: conversione T2

**Prompt:** "Converti `agenti_elimina.php` in T2"

**❌ Verifica SBAGLIATA (quella che facevo):**
- "Ok, ho convertito" → deploy → si rompe perché era un wrapper `<div>` attorno al vecchio codice

**✅ Verifica GIUSTA:**
```bash
# 1. Deve avere i marker T2
grep -c '\$CRUD' agenti_elimina.php     # deve essere > 0
grep -c '\$CRUD->Page' agenti_elimina.php  # deve essere > 0

# 2. NON deve avere markup legacy
grep -c '<div class="content-page"' agenti_elimina.php  # deve essere 0
grep -c '<table' agenti_elimina.php  # deve essere 0 (o molto pochi)

# 3. Deve essere PHP valido
php -l agenti_elimina.php  # deve dire "No syntax errors"

# 4. Deve funzionare live
curl -s "https://claw.nswr.it/projects/assitec/agenti_elimina.php" | head
# atteso: HTML T2, non errore 500
```

---

## Errori comuni di verifica

| Errore | Pericolosità | Come evitarlo |
|---|---|---|
| "Funziona da me" | Il loro ambiente è diverso | Verifica sempre su URL reale/pubblico |
| "Test passati" | Test incompleti o sbagliati | Esegui tu i test su input reali |
| "100% coverage" | Coverage non = correttezza | Guarda cosa testano i test |
| "Linter pulito" | Linter non cattura bug logici | Verifica il comportamento, non lo stile |
| "No errors in log" | Errori silenziati | Controlla tutti i log, non solo errori |

---

## Decision tree post-AI

```
AI dice "fatto"
    │
    ▼
Verifica sintassi OK?
    │
    ├── NO → Ripara, ripeti
    │
    └── SÌ ▼
Verifica contenuto OK? (pattern sbagliati assenti?)
    │
    ├── NO → Ripara, ripeti
    │
    └── SÌ ▼
Verifica vincoli OK? (niente toccato dove non doveva)
    │
    ├── NO → Ripara, ripeti
    │
    └── SÌ ▼
Verifica funzionale OK? (funziona davvero?)
    │
    ├── NO → Ripara, ripeti
    │
    └── SÌ → OK, puoi deployare
```
