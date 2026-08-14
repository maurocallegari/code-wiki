# Corso Pratico: AI-Coding per Mauro

> Come usare Hermes e Codex per migliorare i tuoi progetti esistenti
> (STH Assitec, Insta 2.0, Dashboard)
>
> **Non leggere e basta.** Apri Hermes e Codex, e fai insieme a me.

---

## Setup (una volta sola)

### Hermes

1. Carica `STYLE.md` come contesto base:
   ```
   Leggi /home/clawy/dev/code-wiki/STYLE.md e usalo come guida per ogni task su progetti PHP legacy.
   ```

2. Verifica che il tuo AGENTS.md in ogni progetto contenga le convenzioni (vedi 01-hermes-vs-codex.md)

3. Cron jobs attivi: `cronjob list` per vedere quali task sono automatizzati

### Codex

1. Installato in `/home/clawy/.local/bin/codex`
2. Auth configurata (OAuth o OPENAI_API_KEY)
3. Per testarlo: `cd ~/dev/insta && codex exec --sandbox workspace-write "Describe this project in one sentence"`

---

## Il Metodo (funzionerà SEMPRE)

### Quando arriva un task, chiediti:

1. **Dove?** Quale progetto? (STH, Insta, Dashboard)
2. **Cosa?** Fix bug, nuova feature, refactoring, conversione?
3. **Stile?** Rispetta STYLE.md
4. **Verifica?** Come controllo che funzioni?

### Il prompt perfetto

```
[AZIONE] in [PROGETTO]

FILE: [path esatto]
PROBLEMA: [descrivi sintomo]
CAUSA: [ipotesi]
FIX: [cosa deve cambiare]
VERIFICA: [comando o azione per testare]
VINCOLI: [cosa NON deve toccare]
STILE: leggi /home/clawy/dev/code-wiki/STYLE.md
```

---

## Tutorial 1: Fix Bug in STH Assitec

**Scenario**: La lista agenti non mostra il filtro A-Z su mobile.

**Passo 1 — Isola**
```bash
# Trova il file
grep -r "filtro" dev/sth-assitec-gpt/view/agenti/agenti_lista.php | head -5
```

**Passo 2 — Guarda**
```bash
head -60 dev/sth-assitec-gpt/view/agenti/agenti_lista.php
```

**Passo 3 — Capisci**
Il filtro A-Z è nell'array `$Left` con 26 elementi. Su mobile (`IS_MOBILE`) le colonne si riducono ma il filtro rimane. Il problema potrebbe essere CSS o logica.

**Passo 4 — Chiedi a Hermes**
```
Leggi /home/clawy/dev/code-wiki/STYLE.md

Fix in dev/sth-assitec-gpt/view/agenti/agenti_lista.php:
- Su mobile (IS_MOBILE), il filtro A-Z non è utilizzabile perchè troppo largo
- Aggiungi un wrap scrollabile orizzontalmente per il filtro su mobile
- Modifica SOLO la sezione `$Left`, non toccare `Table` o `Header`
- Verifica: apri la pagina su mobile (o con Chrome DevTools dispositivo) e scorri il filtro
- Stile: inline CSS accettato, come da convenzione
```

**Passo 5 — Verifica**
```bash
# Dopo che Hermes ha modificato
php -l dev/sth-assitec-gpt/view/agenti/agenti_lista.php
# Controlla che esista il wrap scrollable
grep -n "overflow-x" dev/sth-assitec-gpt/view/agenti/agenti_lista.php
# Deploy e test live
```

---

## Tutorial 2: Nuova Feature in Insta 2.0

**Scenario**: Aggiungere un nuovo visual style "Neon" alle slide.

**Passo 1 — Prepara**
```bash
# Guarda gli stili esistenti
ls dev/insta/assets/css/visual-styles*.css
head -30 dev/insta/assets/css/visual-styles2.css
```

**Passo 2 — Definisci**
```
Aggiungi in dev/insta/assets/css/visual-styles2.css:
- Nuovo stile "Neon" con glow effect colorato
- Classi: .neon-glow, .neon-text, .neon-border
- Palette: cyan (#00ffff), magenta (#ff00ff), lime (#00ff00)
- Compatibile con slide 16:9 e 1:1
- Non toccare gli altri visual styles
```

**Passo 3 — Implementa con Hermes**
```
Leggi /home/clawy/dev/code-wiki/STYLE.md

Crea il visual style "Neon" in dev/insta/assets/css/visual-styles2.css:
- Usa classi CSS con prefisso .cds-neon-*
- Effetto glow: text-shadow multiplo colorato
- Border neon: box-shadow con spread
- Test: apri dev/insta/slide-preview.html, seleziona Neon, verifica glow
- Vincoli: non modificare .cds-title esistente, solo aggiungere nuove classi
```

**Passo 4 — Testa**
```bash
# Preview locale
php -S localhost:8000 -t dev/insta
# Apri http://localhost:8000/slide-preview.html
```

**Passo 5 — Deploy**
```bash
# Bump versione
sed -i "s/APP_VERSION = '.*'/APP_VERSION = '20260815a'/" dev/insta/index.php
# FTP deploy
# Verifica live
```

---

## Tutorial 3: Batch Conversione con Codex

**Scenario**: Converti 5 file da `mysql_*` a `$CRUD` (esempio semplice, non T2).

**Passo 1 — Prepara un reference**
```bash
# Prendi un file già convertito come esempio
cat dev/sth-assitec-gpt/view/agenti/agenti_scheda.php | head -40 > /tmp/reference.txt
```

**Passo 2 — Spezza in batch**
```bash
# Lista file da convertire
echo "view/agenti/agenti_lista.php
view/clienti/clienti_lista.php
view/studi/studi_lista.php" > /tmp/batch1.txt
```

**Passo 3 — Lancia Codex per ogni batch**
```bash
cd /home/clawy/dev/sth-assitec-gpt
while read file; do
  codex exec --sandbox workspace-write "Convert $file:
  - Replace mysql_query() with \$CRUD->Query()
  - Replace mysql_fetch_assoc() with \$CRUD->FetchAssoc()
  - Reference pattern: /tmp/reference.txt
  - Keep same functionality
  - Verify: php -l must pass" < /dev/null
done < /tmp/batch1.txt
```

**Passo 4 — Verifica automatica**
```bash
while read file; do
  echo "=== $file ==="
  php -l "$file"
  grep -c "mysql_query" "$file"  # should be 0
  grep -c 'CRUD->Query' "$file"  # should be > 0
done < /tmp/batch1.txt
```

---

## Buone Pratiche (la mia lista personale)

### ✅ FARE
- Caricare SEMPRE STYLE.md come contesto all'inizio
- Spezzare task grandi in micro-task (1-2 file alla volta)
- Verificare SEMPRE con `php -l`, `grep`, `curl`, browser
- Dopo ogni batch: `git diff --stat` per vedere l'entità delle modifiche
- Tenere un backup prima di batch grandi: `cp -r dir dir.bak`
- Verificare vincoli: `grep "mysql_query" file.php` deve tornare 0 dopo conversione

### ❌ NON FARE
- Non dire "converti tutto" — specifica file per file
- Non accettare "fatto" senza verifica
- Non fare batch > 10 file senza verifica intermedia
- Non toccare `*_old/`, `action/crud/functions.php`, `app/`
- Non tradurre termini dominio (rapportini, interventi, laboratorio)
- Non modernizzare SQL senza richiesta esplicita

---

## Checklist Post-Task

Prima di considerare completato un task:

- [ ] `php -l` passa
- [ ] `grep` pattern sbagliati = 0
- [ ] `grep` pattern corretti > 0
- [ ] `git diff --stat` mostra solo file attesi
- [ ] Test funzionale (browser/curl) OK
- [ ] Vincoli rispettati (niente toccato dove non doveva)
- [ ] Domain naming preservato
- [ ] Stile rispettato (STILE.md)

---

## Risorse

- [STYLE.md](STYLE.md) — Il tuo stile
- [PATTERNS.md](https://github.com/maurocallegari/sth-assitec-gpt/blob/main/PATTERNS.md) — Pattern STH
- [docs/convenzioni.md](https://github.com/maurocallegari/sth-assitec-gpt/blob/main/docs/convenzioni.md) — Convenzioni STH
- [01-hermes-vs-codex.md](01-hermes-vs-codex.md) — Quando usare quale
- [05-checklist-verifica.md](05-checklist-verifica.md) — Verifica output AI
