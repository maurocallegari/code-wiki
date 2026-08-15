# Setup gbrain

> Memoria semantica per l'AI. Importi appunti/codice, l'AI fa ricerche cross-session.

---

## Installazione

```bash
# Bun (già installato)
export PATH="$HOME/.bun/bin:$PATH"
bun --version  # 1.3.14

# gbrain
bun install -g github:garrytan/gbrain
gbrain --version
```

---

## Inizializzazione

```bash
# Locale (no Docker, no server)
gbrain init --pglite

# Con API key (per semantic search)
gbrain init --pglite --embedding-model <id>
```

---

## Comandi

```bash
# Importa file
gbrain import ~/notes/
gbrain import ~/dev/sth-assitec-gpt/

# Ricerca semantica
gbrain query "temi ricorrenti nei miei progetti"
gbrain query "come ho gestito i trigger in STH"

# Salva memoria
gbrain remember "Preferisco dark mode in ogni editor" --provenance demo --entity people/me

# Ricorda
gbrain recall --entity people/me

# Sync repo
cd ~/dev/sth-assitec-gpt
gbrain sync --strategy code

# Verifica salute
gbrain doctor
```

---

## MCP Server (per Hermes)

```bash
# Avvia server MCP
gbrain serve

# In Hermes, registra
hermes mcp add gbrain -- gbrain serve
```

---

## Privacy

```bash
# Visibilità memorie
# visibility: "private"  → solo locale
# visibility: "shared"   → tutti gli agenti
# visibility: "public"   → cross-project
```

---

## Prossimo

Vai a [Cron Jobs](cron.md).
