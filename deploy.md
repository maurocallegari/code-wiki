# Deploy FTP verificato

Il deploy non finisce quando FTP risponde “success”. Finisce quando il file remoto è quello atteso, permessi e cache sono corretti, e lo smoke test live passa.

## Preflight

```bash
# WHY: deploya solo una patch conosciuta e già revisionata.
git status --short
git diff --check
git diff --stat

# WHY: esegui il parser su ogni PHP che entrerà nel deploy.
git diff --name-only -- '*.php' | while IFS= read -r f; do php -l "$f" || exit 1; done
```

> [!DANGER] Mai inserire host, user o password nel documento o nel comando copiato in chat. Usa variabili d’ambiente/secret store e verifica i target senza stamparne il valore.

## Manifest, non mirror cieco

Crea un elenco chiuso di file locali → path remoti. Per pochi file usa upload espliciti. Usa mirror solo quando hai revisionato dry-run/esclusioni e sei certo del webroot.

```text
assets/css/visual-styles2.css -> /path/remoto/assets/css/visual-styles2.css
index.php                    -> /path/remoto/index.php
```

Ho provato mirror ampi: includevano `.bak`, log e file temporanei, riempiendo il disco di claw.nswr.it. Escludi sempre `.git`, `.env`, `*.bak`, log, cache, node_modules e vendor non richiesti.

## Sequenza robusta

1. salva checksum/mtime remoto o scarica una copia di rollback fuori dal webroot
2. carica in un nome temporaneo nella stessa directory, se il server lo consente
3. imposta file 0644 e directory 0755; segreti 0600 quando applicabile
4. rinomina/sostituisci in modo atomico, oppure upload esplicito
5. bump della versione/cache secondo il progetto reale
6. verifica HTTP, contenuto/versione e flusso funzionale
7. conserva il rollback finché lo smoke test è concluso

```bash
# WHY: un 200 da solo può essere pagina cache o errore HTML con status errato.
curl --fail --silent --show-error --location 'https://URL-REALE/pagina' -o /tmp/deploy-check.html

# WHY: cerca un marker di versione/contenuto deciso prima del deploy.
rg -n 'MARKER_ATTESO' /tmp/deploy-check.html
```

## Permessi

| Oggetto | Default | Nota |
|---|---:|---|
| PHP, HTML, CSS, JS | 0644 | web server legge, owner scrive |
| directory | 0755 | attraversabile dal web server |
| file segreti | 0600 | meglio fuori dal webroot |

Se FTP rifiuta overwrite, **non** fare chmod 777. Controlla owner, directory target, BitNinja/WAF e strategia temp+rename. Un delete+put apre una finestra in cui il file manca: usalo solo se hai compreso e accettato il rischio.

## BitNinja, cache, disco

| Sintomo | Controllo | Azione sicura |
|---|---|---|
| upload “ok”, live vecchio | hash/marker remoto, cache/CDN/opcache | bump versione e purge documentato |
| 403/operazione bloccata | log hosting/BitNinja, path, estensione | approva/allowlist tramite pannello, non aggirare |
| permission denied | owner e permessi directory | correggi owner/ACL col provider |
| disco pieno | `df -h`, `df -i`, directory più grandi | archivia/elimina solo target confermati |

## Rollback

Se smoke test fallisce: rimetti l’artifact precedente, ripristina versione/cache, ripeti lo stesso smoke test e registra il fallimento nel Kanban. Non improvvisare fix direttamente sul server.
