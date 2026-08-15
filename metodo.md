# Il Metodo

Il sistema è semplice: definisci il contratto, fai eseguire all’AI, poi tenta di smentirla con verifiche concrete.

## Hermes o Codex?

| Segnale | Hermes | Codex |
|---|---:|---:|
| Telegram, Raspberry, MacBook, cron | sì | no |
| deploy, backup, più sistemi | sì | no |
| patch di codice in un repo | può coordinare | sì |
| refactor ripetitivo e circoscritto | può lanciare | sì |
| task ambiguo che richiede ispezione | sì | sì, in modalità read-only prima |

Ho provato a usare un solo strumento per tutto: il contesto operativo finiva nel coding e il coding finiva in una conversazione troppo lunga. Separa orchestrazione ed esecuzione.

## La formula universale

```text
FILE: path esatto o lista chiusa
TASK: un verbo e un risultato osservabile
INPUT: bug, esempio reale, comportamento attuale
OUTPUT: file e comportamento atteso
VINCOLI: cosa non toccare, convenzioni, niente refactor
VERIFICA: comandi + test browser/API
```

Esempio STH:

```text
FILE: view/agenti/agenti_lista.php
TASK: rendi utilizzabile su mobile il filtro A–Z.
INPUT: i 26 link esistono già e la loro logica non va cambiata.
OUTPUT: scorrimento orizzontale, link leggibili e cliccabili a 320 px.
VINCOLI: modifica solo il contenitore del filtro; non toccare Table, Header,
Sidebar, *_old/, action/crud/functions.php, app/ o plugins/.
VERIFICA: php -l; git diff --check; test a 320/375/414 px.
```

## Il ciclo in sette mosse

1. **Isola.** Trova file, comportamento e confini.
2. **Fotografa il prima.** Salva `git status`, riproduzione e output del test.
3. **Dai un reference.** Un file corretto vale più di “segui lo stile”.
4. **Fai una patch piccola.** Un obiettivo, pochi file.
5. **Verifica staticamente.** Sintassi, diff, pattern vietati.
6. **Verifica funzionalmente.** Browser, API o flusso utente reale.
7. **Registra e deploya.** Kanban, versione/cache, controllo live.

```bash
# WHY: fotografa modifiche preesistenti; non attribuirle all’AI.
git status --short

# WHY: il parser trova errori che una lettura del diff non trova.
php -l view/agenti/agenti_lista.php

# WHY: spazi finali e conflict marker sono segnali di patch sporca.
git diff --check

# WHY: controlla scope e contenuto, non solo il riepilogo del modello.
git diff --stat && git diff -- view/agenti/agenti_lista.php
```

> [!WARNING] “Nessun errore nel terminale” non è una verifica funzionale. Se il task riguarda mobile, devi toccare e scorrere il controllo su mobile.

## Dimensione dei batch

Parti da 3 file omogenei. Salire a 5–10 è sensato solo dopo che il primo batch passa la matrice di test. Ho provato batch enormi: al timeout rimanevano conversioni a metà e non era più chiaro quali file fossero affidabili.

## Definizione di fatto

- [ ] comportamento riprodotto prima e corretto dopo
- [ ] solo file autorizzati modificati
- [ ] convenzioni STH rispettate
- [ ] test statici eseguiti con output leggibile
- [ ] test funzionale eseguito nell’ambiente pertinente
- [ ] diff revisionato
- [ ] Kanban spostato in “Fatto” dopo verifica, non prima
