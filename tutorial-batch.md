# Tutorial: Batch Conversione

> Esercizio pratico: convertire più file da mysql_* a T2 CRUD.

---

## Scenario

**Task:** Converti 3 file PHP da `mysql_*` a pattern T2 CRUD.

---

## Step 1: Prepara Reference

```bash
# Prendi un file già convertito come esempio
cat dev/sth-assitec-gpt/view/agenti/agenti_nuovo.php | head -40 > /tmp/reference.txt
```

---

## Step 2: Spezza in Batch

```bash
# Lista file da convertire
echo "view/agenti/agenti_lista.php
view/clienti/clienti_lista.php
view/studi/studi_lista.php" > /tmp/batch1.txt
```

---

## Step 3: Prompt per Codex

```bash
cd /home/clawy/dev/sth-assitec-gpt

while read file; do
  echo "=== Convertendo $file ==="
  codex exec --sandbox workspace-write "Convert $file from mysql_* to T2 CRUD pattern.
  Reference: /tmp/reference.txt
  Rules:
  - Replace mysql_query() with \$CRUD->Query()
  - Replace mysql_fetch_assoc() with \$CRUD->FetchAssoc()
  - Wrap content in <card> not <div class='content-page'>
  - Use \$CRUD->Id() not \$_GET['id']
  - Keep same functionality
  Verify: php -l must pass" < /dev/null
done < /tmp/batch1.txt
```

---

## Step 4: Verifica Automatica

```bash
while read file; do
  echo "=== $file ==="
  php -l "$file"
  grep -c "mysql_query" "$file"  # should be 0
  grep -c 'CRUD->Query' "$file"  # should be > 0
done < /tmp/batch1.txt
```

---

## Step 5: Correggi (se necessario)

Se un file non passa la verifica:

```bash
codex exec --sandbox workspace-write "Fix $file: missing \$CRUD->Page, has <div class='content-page'> instead of <card>. Reference: /tmp/reference.txt"
```

---

## Cosa Hai Imparato

1. Come preparare un reference
2. Come spezzare in batch
3. Come verificare automaticamente
4. Come correggere i fallimenti

---

## Prossimo Passo

Vai a [Problemi Comuni](problemi.md) per evitare gli errori che ho fatto io.
