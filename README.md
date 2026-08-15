# Code Wiki

<div class="hero">
  <h1>Code Wiki</h1>
  <p>Guida operativa per usare Hermes + Codex nel tuo lavoro quotidiano.</p>
  <p><b>Obiettivo:</b> sostituire il coding tradizionale con AI-coding, mantenendo il tuo stile.</p>
</div>

---

## Start Here

<div class="grid">
  <div class="card">
    <h3>🎯 Il Metodo</h3>
    <p>Il tuo sistema operativo: come lavorare ogni giorno, strumenti, formule, checklist.</p>
    <a href="#/metodo">Leggi →</a>
  </div>
  <div class="card">
    <h3>🛠 Setup Strumenti</h3>
    <p>Hermes: config, skills, cron, kanban. Codex: install, auth, context, import.</p>
    <a href="#/setup">Leggi →</a>
  </div>
  <div class="card">
    <h3>📝 Il Tuo Stile</h3>
    <p>Convenzioni, pattern, naming, domain terms — il tuo stile in 11 regole.</p>
    <a href="#/stile">Leggi →</a>
  </div>
  <div class="card">
    <h3>💡 Come Importare Progetti</h3>
    <p>Checklist per portare qualsiasi codebase esistente sotto Hermes + Codex.</p>
    <a href="#/import">Leggi →</a>
  </div>
  <div class="card">
    <h3>🎓 Tutorial</h3>
    <p>Esercizi pratici: fix bug, nuova feature, batch conversione.</p>
    <a href="#/tutorial">Leggi →</a>
  </div>
  <div class="card">
    <h3>⚠️ Problemi Comuni</h3>
    <p>Errori che ho fatto io e tu, e come non ripeterli.</p>
    <a href="#/problemi">Leggi →</a>
  </div>
</div>

---

## Strumenti

<div class="grid">
  <div class="card">
    <h3>Hermes</h3>
    <p>Orchestratore. Gli scrivi in italiano, lui capisce, delega a Codex, verifica, deploya.</p>
    <p><b>Path:</b> Raspberry + MacBook Air</p>
    <a href="#/setup-hermes">Config →</a>
  </div>
  <div class="card">
    <h3>Codex</h3>
    <p>Esecutore di codice puro. Lo chiami per batch, refactoring, conversioni. Prompt in inglese.</p>
    <p><b>CLI:</b> codex exec</p>
    <a href="#/setup-codex">Config →</a>
  </div>
  <div class="card">
    <h3>GStack</h3>
    <p>30 skills per workflow: /review, /qa, /ship, /autoplan, /careful, /investigate, /cso.</p>
    <p><b>Path:</b> ~/.hermes/skills/gstack-*/</p>
    <a href="#/gstack">Skills →</a>
  </div>
  <div class="card">
    <h3>gbrain</h3>
    <p>Memoria semantica per l'AI. Importi appunti/codice, l'AI fa ricerche cross-session.</p>
    <p><b>Engine:</b> PGLite (locale)</p>
    <a href="#/gbrain">Setup →</a>
  </div>
</div>

---

## Team

<div class="grid">
  <div class="card">
    <h3>📊 Kanban</h3>
    <p>Task reali: Da fare, In corso, Verifica, Fatto,Blocked.</p>
    <p><b>ID Board:</b> t_4a10fd31...</p>
    <a href="#/kanban">Stato →</a>
  </div>
  <div class="card">
    <h3>⏰ Cron</h3>
    <p>8 job: backup, reddit, finance, idee proattive, workspace sync, weekly research.</p>
    <p><b>Lista:</b> cronjob list</p>
    <a href="#/cron">Dettagli →</a>
  </div>
  <div class="card">
    <h3>🚀 Deploy</h3>
    <p>FTP verificato: rm + put, chmod 644, bump versione, mirror -R.</p>
    <p><b>Host:</b> claw@nswr.it</p>
    <a href="#/deploy">Workflow →</a>
  </div>
  <div class="card">
    <h3>🐙 GitHub</h3>
    <p>Token disponibile, repo private, push automatico.</p>
    <p><b>Repo:</b> clawy-dashboard, code-wiki, hermes-backup</p>
    <a href="#/github">Config →</a>
  </div>
</div>

<style>
.hero { text-align: center; padding: 2em 0; background: linear-gradient(135deg, #1a2238, #0b0f17); border-radius: 12px; margin-bottom: 2em; }
.hero h1 { font-size: 2.5em; margin-bottom: 0.2em; color: #7c5cff; }
.grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(220px, 1fr)); gap: 1em; margin: 1.5em 0; }
.card { background: rgba(255,255,255,.05); border: 1px solid rgba(255,255,255,.1); border-radius: 12px; padding: 1.2em; transition: .2s; }
.card:hover { border-color: #7c5cff; transform: translateY(-2px); }
.card h3 { margin: 0 0 0.5em; font-size: 1.1em; }
.card p { font-size: 0.9em; color: #8b97ad; margin: 0.5em 0; }
.card a { color: #22d3ee; font-weight: 600; text-decoration: none; }
</style>
