# Kanban

> Task reali tracciati su board Hermes.

---

## Colonne

```
Da fare → In corso → Verifica → Fatto → Blocked
```

---

## Task Attivi

| ID | Titolo | Stato |
|----|--------|-------|
| t_fb3b8106 | Pulizia branch sth-assitec-gpt | ▶ ready |
| t_754fbfb7 | Fix countdown custom duration in Insta | ▶ ready |
| t_184686a3 | Converti agenti_elimina.php in T2 CRUD | ▶ ready |
| t_a927239b | Aggiungere campo a tabella agenti in STH | ▶ ready |
| t_1302b212 | Dashboard Clienti OSINT da Excel | ▶ ready |
| t_f83d0040 | Micro-SaaS brainstorming (30 idee) | ▶ ready |
| t_d535763e | Setup Hermex Kanban per attività con Clawy | ▶ ready |
| t_4a10fd31 | Fix 3 demo reali | ✓ done |
| t_e2e0984e | Pulizia disco Pi | ✓ done |
| t_965bc715 | Gestionale AI via chat | ⊘ blocked |

---

## Come Usare

```bash
# Lista task
hermes kanban list

# Dettaglio
hermes kanban show t_fb3b8106

# Sposta colonna
hermes kanban promote t_fb3b8106  # → next column
hermes kanban block t_fb3b8106    # → Blocked
hermes kanban complete t_fb3b8106 # → Fatto

# Crea nuovo
hermes kanban create "Titolo" --body "Descrizione"
```

---

## Automatizzare dalla Chat

Quando scrivi un task a Hermes, chiedi di crearlo nel Kanban:

```
"Crea un task nel Kanban per: fix countdown Insta"
```

Hermes lo crea automaticamente.

---

## Prossimo

Vai a [Problemi Comuni](problemi.md).
