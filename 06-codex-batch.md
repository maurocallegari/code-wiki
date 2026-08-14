# Codex Batch — Conversione T2 e Task Ripetitivi

## TL;DR

Usa Codex per conversioni batch (es. T2) con questo flusso:
1. Prepara esempio di riferimento
2. Spezzetta in batch di 5-10 file
3. Per ogni batch: genera → verifica → correggi → deploy

---

## Perché Codex per il batch

- **Velocità**: parallelizza su più core
- **Consistenza**: stesso prompt = stile uniforme
- **No contesto necessario**: ogni file è isolato
- **Costo**: gratis con OpenAI subscription

---

## Il workflow T2 (caso reale)

### Step 0: Prepara il reference

Prima di tutto, un file T2 perfetto come esempio:
```bash
# Crea un file "perfetto" che userò come reference
cat > /tmp/t2-reference.php << 'EOF'
<?php
require_once __DIR__ . '/../config/config.php';
$CRUD = new CRUD();
$Page = $CRUD->Page('agenti', 'agenti_elimina.php');
$Id = $CRUD->Id($_GET['id'] ?? 0);
if (!$Page->Auth()) { header('Location: login.php'); exit; }
$Row = $CRUD->Row('SELECT * FROM agenti WHERE id = ?', [$Id]);
if (!$Row) { $Page->Show('Record non trovato'); exit; }
if ($_POST) {
    $CRUD->Query('DELETE FROM agenti WHERE id = ?', [$Id]);
    $Page->Show('Agente eliminato', 'success');
}
echo $Page->Header();
?>
<card>
    <header>Elimina Agente</header>
    <body>
        <p>Confermi eliminazione di <?= htmlspecialchars($Row['nome']) ?>?</p>
        <form method="post">
            <button type="submit" class="btn-danger">Elimina</button>
            <a href="agenti.php" class="btn-secondary">Annulla</a>
        </form>
    </body>
</card>
<?= $Page->Footer(); ?>
EOF
```

### Step 1: Spezzetta in batch

```bash
# Lista tutti i file da convertire
find /home/clawy/dev/sth-assitec-gpt -name "*.php" -path "*/agenti/*" > /tmp/t2-files.txt
# Dividi in batch da 10
split -l 10 /tmp/t2-files.txt /tmp/t2-batch-
```

### Step 2: Prompt per ogni batch

```bash
for batch in /tmp/t2-batch-*; do
  echo "Processing $batch..."
  files=$(paste -sd'|' "$batch")
  codex exec --sandbox workspace-write "Convert these PHP files to T2 CRUD format:
  Reference: /tmp/t2-reference.php
  Files: $files
  Rules:
  - Replace mysql_* with \$CRUD methods
  - Wrap content in <card> not <div class='content-page'>
  - Use \$CRUD->Id() not \$_GET['id']
  - Use \$Page->Show() for messages
  - Keep same functionality, only change structure
  Verify: each file must have \$CRUD->Page and zero <table tags"
done
```

### Step 3: Verifica automatica post-batch

```bash
# Per ogni file convertito
for f in $(cat /tmp/t2-files.txt); do
  echo "=== $f ==="
  grep -c '\$CRUD->Page' "$f"  # deve essere > 0
  grep -c '<table' "$f"        # deve essere 0
  php -l "$f"                  # syntax OK
done
```

### Step 4: Correggi manuale (se necessario)

Se un file non passa la verifica:
```bash
# Rilancia solo su quel file con feedback specifico
codex exec --sandbox workspace-write "Fix $f: missing \$CRUD->Page, has <div class='content-page'> instead of <card>. Reference: /tmp/t2-reference.php"
```

---

## Errori che ho fatto (e come evitarli)

| Errore | Conseguenza | Soluzione |
|---|---|---|
| "Converti 100 file" in un prompt | Si blocca, perde contesto | Batch da 5-10 file |
| Nessuna reference | Output inconsistente | Sempre fornire esempio |
| Nessuna verifica | Wrapper invece di conversione | Checklist automatica post-batch |
| Deploy diretto | Rotto tutto | Verifica → staging → live |
| Non avere backup | Impossibile rollback | `cp -r dir dir.bak` prima |

---

## Template prompt batch

```
Convert these PHP files to T2 CRUD format.

REFERENCE (esempio perfetto):
[percorso file reference]

FILES:
[lista file]

RULES:
- Replace mysql_* with \$CRUD methods
- Wrap content in <card> not <div>
- Use \$CRUD->Id() not \$_GET['id']
- Use \$Page->Show() for flash messages
- Keep same functionality

VERIFY AFTER:
- Each file must have \$CRUD->Page
- Zero <table tags
- Zero <div class="content-page">
- php -l must pass
```
