# Deploy FTP Verificato

## TL;DR

```bash
# Deploy singolo file
rm remoto && put locale

# Deploy cartella
mirror -R -p locale remoto

# Verifica
curl -s -o /dev/null -w "%{http_code}" URL
```

---

## Il problema dei deploy

1. FTP put non sovrascrive → fare `rm` prima
2. Permessi 600 → 500 PHP-FPM → chmod 644
3. Cache browser → bump versione
4. WAF BitNinja → file troncati → usare home `/` non `.php` espliciti

---

## Workflow deploy sicuro

### Step 1: Backup
```bash
cp file.php file.php.bak
```

### Step 2: Modifica + bump versione
```bash
sed -i "s/APP_VERSION = '...'/APP_VERSION = '20260814b'/" index.php
```

### Step 3: Verifica locale
```bash
php -l file.php
grep -c "marker" file.php
```

### Step 4: Deploy
```bash
ftp put file.php
oppure
mirror -R -p demo/ /demo/
```

### Step 5: Verifica remota
```bash
curl -s -o /dev/null -w "%{http_code}" "https://claw.nswr.it/percorso"
curl -s "https://claw.nswr.it/api/endpoint" | head -c 300
```

### Step 6: Se rotto, rollback
```bash
cp file.php.bak file.php
ftp put file.php
```

---

## Script deploy rapido

```bash
#!/bin/bash
# deploy.sh — uso: ./deploy.sh percorso/file.php

FILE="$1"
BUMP="${2:-yes}"

if [ ! -f "$FILE" ]; then echo "File non trovato"; exit 1; fi

# Backup
cp "$FILE" "$FILE.bak"

# Bump versione
if [ "$BUMP" = "yes" ]; then
  DATE=$(date +%Y%m%d)
  sed -i "s/APP_VERSION = '.*'/APP_VERSION = '${DATE}b'/" index.php 2>/dev/null
fi

# Deploy
source ~/.hermes/.env 2>/dev/null
lftp -u "${FTP_USER:-claw@nswr.it},${FTP_PASS}" -p 21 "ftp://${FTP_HOST:-86.107.34.20}" -e "
  set ftp:ssl-allow no; set ssl:verify-certificate no; set ftp:passive-mode on;
  cd /; put $FILE; bye" 2>&1 | tail -1

# Verifica
URL="https://claw.nswr.it/${FILE#public_html/}"
CODE=$(curl -s -o /dev/null -w "%{http_code}" "$URL")
echo "Deployed $URL → $CODE"

if [ "$CODE" -ge 400 ]; then
  echo "ERRORE! Rollback..."
  cp "$FILE.bak" "$FILE"
  lftp -u "${FTP_USER:-claw@nswr.it},${FTP_PASS}" -p 21 "ftp://${FTP_HOST:-86.107.34.20}" -e "
    set ftp:ssl-allow no; set ftp:passive-mode on; cd /; put $FILE; bye" 2>&1 | tail -1
fi
```

---

## Permessi file

| Tipo | Permessi | Perché |
|------|----------|--------|
| PHP | 644 | PHP-FPM gira come utente diverso |
| HTML/CSS/JS | 644 | Idem |
| Cartelle | 755 | Idem |
| API/JSON | 644 | Idem |
| .env | 600 | Solo root/clawy |
