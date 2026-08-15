# Kanban quotidiano

Il Kanban è la memoria operativa. La chat non lo è: ho provato a lasciare decisioni in una conversazione e il giorno dopo mancavano stato, prove e prossimo passo.

## Colonne

```text
Da fare → In corso → Verifica → Fatto
                ↘ Bloccato
```

| Colonna | Regola di ingresso | Regola di uscita |
|---|---|---|
| Da fare | risultato e progetto chiari | scope e verifica definiti |
| In corso | una persona/agente responsabile | patch pronta, test statici passati |
| Verifica | diff e prove allegati | test funzionale accettato |
| Fatto | Definition of Done completa | nessuna |
| Bloccato | impedimento concreto scritto | owner e prossimo controllo |

## Una card fatta bene

```markdown
Titolo: STH · filtro A–Z agenti usabile su mobile

Risultato: a 320–414 px tutte le lettere sono raggiungibili e cliccabili.
File atteso: view/agenti/agenti_lista.php
Vincoli: solo filtro; niente Table/Header/Sidebar; zone STH protette.
Verifica: php -l, diff check, test 320/375/414 px, desktop invariato.
Deploy: non autorizzato finché la card è in Verifica.

Log:
- baseline: [comando/output o screenshot]
- patch: [commit/diff]
- prove: [comandi/output]
- live: [URL e risultato, senza credenziali]
```

## Dalla chat al board

```text
Crea una card Kanban per [risultato].
Non iniziare ancora. Inserisci progetto, comportamento attuale, risultato,
file probabili (marcati come ipotesi), vincoli, verifiche e Definition of Done.
Se manca un dato che cambia lo scope, metti una domanda nella card.
```

Poi:

```text
Prendi la card [ID reale]. Spostala in In corso, fai l’audit read-only e aggiorna
i file in scope. Esegui la patch solo se i confini restano quelli approvati.
Spostala in Verifica con diff e output. Non marcarla Fatto e non deployare.
```

> [!DANGER] Non lasciare che Hermes inventi ID o sintassi CLI. Usa il comando `--help` dell’integrazione Kanban installata oppure opera dalla UI/API documentata.

## Rituale da 10 minuti

1. Mattina: scegli una card “Da fare”, definisci test, portala “In corso”.
2. Durante: aggiungi decisioni e output alla card, non solo in chat.
3. Prima del deploy: porta in “Verifica”; controlla tu il comportamento.
4. Fine giornata: una riga di prossimo passo per ogni “In corso”.
5. Venerdì: chiudi duplicati e rivedi i bloccati; non cancellare la storia utile.

## WIP limit

Una card “In corso” per progetto e massimo due totali. I task AI sembrano paralleli, ma la verifica resta umana: troppo WIP crea una coda di patch non affidabili.
