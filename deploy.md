# Deploy FTP

> Workflow deploy verificato per claw.nswr.it.

---

## Host

```
FTP_HOST=86.107.34.20
FTP_USER=claw@nswr.it
FTP_WEBROOT=/home/clawy/public_html
```

---

## Comandi

### Singolo File

```bash
# Rimuovi remoto, poi carica
rm remoto && put locale

# Verifica
curl -s -o /dev/null -w "%{http_code}" URL
```

### Cartella (mirror)

```bash
# Mirror completo
mirror -R -p dir/ /dir/

# Mirror singolo file
put -R -p file.php
```

---

## Workflow Completo

```bash
# 1. Backup
cp file.php file.php.bak

# 2. Bump versione
sed -i "s/APP_VERSION = '.*'/APP_VERSION = '$(date +%Y%m%d)b'/" index.php

# 3. Verifica locale
php -l file.php
grep -c "pattern" file.php

# 4. Deploy
mirror -R -p public_html/demo/ /demo/

# 5. Verifica remota
curl -s -o /dev/null -w "%{http_code}" "https://claw.nswr.it/demo/file.php"

# 6. Se rotto, rollback
cp file.php.bak file.php
mirror -R -p public_html/demo/ /demo/
```

---

## Permessi

| Tipo | Permessi |
|------|----------|
| PHP | 644 |
| HTML/CSS/JS | 644 |
| Cartelle | 755 |
| .env | 600 |

---

## Note BitNinja/WAF

- `put` singolo spesso non aggiorna live → usa `mirror`
- File troncati → usa home `/` non `.php` espliciti
- Cache browser → bump versione APP_VERSION

---

## Prossimo

Vai a [Kanban](kanban.md).
