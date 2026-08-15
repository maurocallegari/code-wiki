# Tutorial · Nuova feature Insta

Scenario reale dal brief: aggiungere a Insta 2.0 uno stile visuale “Neon”. Non assumere file, classi o palette: prima estrai il contratto dagli stili esistenti.

## 1. Mappa l’integrazione

```bash
cd /home/clawy/dev/insta
git status --short

# WHY: trova dove gli stili sono definiti e dove vengono selezionati davvero.
rg -n "visual style|visual-style|cds-|style.*select|theme" --glob '*.{css,js,html,php}'

# WHY: identifica un visual style completo da usare come reference reale.
rg -l "cds-" --glob '*.css'
```

Annota file CSS, registro/select UI, preview e formato slide supportato. “CSS only” è valido solo se la selezione legge automaticamente le classi; se esiste un registro JS, va incluso nello scope.

## 2. Definisci l’accettazione

| Area | Deve essere vero |
|---|---|
| selezione | “Neon” compare nello stesso elenco degli stili esistenti |
| rendering | preview e output finale usano le stesse classi |
| leggibilità | titolo e body restano leggibili, il glow non sostituisce il testo |
| formato | nessun overflow nei formati realmente supportati da Insta |
| regressione | gli altri visual style non cambiano |

## 3. Prompt per Codex

```text
Read AGENTS.md and inspect the complete visual-style pipeline first.
Use [verified existing style file/section] as the reference.

TASK: add a selectable “Neon” visual style to Insta 2.0.
OUTPUT: implement the minimum CSS and registry/selector change required by the
existing architecture; preview and final render must match.
CONSTRAINTS: preserve every existing style, class contract, slide size, and
export behavior. Do not rename shared classes or refactor unrelated code.
STYLE: dark base with restrained cyan/magenta/lime signals only if compatible
with the existing palette API; readable text without relying on glow.
VERIFY: syntax checks for each changed file, git diff --check, render the same
sample slide in every supported aspect ratio, compare another existing style.
First report the exact files needed. Do not edit until the integration path is clear.
```

> [!WARNING] Ho provato ad aggiungere solo quattro classi CSS: il preview sembrava giusto, ma il selettore e l’export non conoscevano lo stile. Verifica tutta la pipeline.

## 4. Commenta il perché

```css
/* WHY: il colore del testo resta solido; il glow è un rinforzo, non la leggibilità. */
.cds-neon-title {
  color: var(--neon-title-color);
  text-shadow: 0 0 var(--neon-glow-radius) var(--neon-title-glow);
}

/* WHY: il bordo delimita la slide anche quando l’ombra viene attenuata in export. */
.cds-neon-border {
  border: var(--neon-border-width) solid var(--neon-border-color);
  box-shadow: 0 0 var(--neon-glow-radius) var(--neon-border-glow);
}
```

I token sopra sono illustrativi: usa il sistema di variabili reale trovato nel repo, non introdurre un secondo sistema.

## 5. Verifica

```bash
# WHY: lo scope deve corrispondere ai file annunciati al punto 3.
git diff --name-only
git diff --check

# WHY: usa i checker disponibili nel repo; non inventare un comando di build.
node --check path/reale/al/file-modificato.js

# WHY: revisiona ogni modifica agli stili esistenti, che dovrebbe essere minima o nulla.
git diff
```

- [ ] Neon selezionabile dopo reload pulito
- [ ] preview = export
- [ ] testo leggibile senza alone
- [ ] tutti i formati supportati senza clipping
- [ ] cambio Neon → stile esistente ripristina tutto
- [ ] nessun altro stile modificato visivamente

Bump versione/cache secondo il meccanismo reale del repo, poi deploy verificato.
