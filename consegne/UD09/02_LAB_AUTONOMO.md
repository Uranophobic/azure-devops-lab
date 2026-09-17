# UD09 — Consegna laboratorio autonomo

## 1. Organization e Project

- Organization: `azdo-alessia-01`
- Project: `az900-az104-devops`
- Visibility: `Private`
- Source repository: `Uranophobic/azure-devops-lab`

1. Il Project è privato perché contiene configurazioni e risorse di lavoro che devono essere accessibili solo agli utenti autorizzati.
2. Il source repository rimane GitHub perché è già il repository utilizzato nel corso e verrà collegato ad Azure Pipelines senza duplicarlo in Azure Repos.

## 2. Gruppi

| Gruppo | Scopo | Tutti? |
|---|---|---|
| Project Administrators | gruppo per gli amministratori di progetto i quali hanno permessi amministrativi completi, incluse configurazioni e autorizzazioni | no, perché concede privilegi elevati e va assegnato solo a chi deve amministrare il progetto. |
| Contributors | gruppo per utenti che possono lavorare sul progetto eseguendo azioni come creare, modificare contenuti o risorse operative | no, perché va assegnato solo agli utenti che devono contribuire attivamente al progetto. |
| Readers | gruppo per utenti i quali hanno permessi solamente in lettura delle risorse del progetto | no, perché va assegnato solo a chi deve consultare il progetto senza modificarlo. |
| Build Administrators | gruppo per amministratori con permessi relativi a build e pipeline | no, perché i permessi sulle build e pipeline devono essere concessi solo a chi ne gestisce configurazione e funzionamento. |

## 3. GitHub integration readiness

- repository: `Uranophobic/azure-devops-lab`
- private: `yes`
- gh auth: `logged in`
- git fetch: 

1. In UD09 non abbiamo creato una service connection GitHub OAuth/PAT perché la connessione verrà configurata quando creeremo la prima pipeline, evitando credenziali inutili o non ancora utilizzate.
2. Quando creeremo la prima pipeline GitHub useremo Azure Pipelines GitHub App.
3. Evitiamo di duplicare il repository in Azure Repos perché GitHub resta il repository sorgente del corso e mantenerne una seconda copia creerebbe duplicazione e possibili problemi di sincronizzazione.

## 4. Parallel jobs

- hosted: 1 parallel job
- self-hosted: 1 parallel job

1. Nel caso in cui microsft hosted fosse 0, il corso non sarebbe bloccato perché è comunque disponibile un parallel job self-hosted, quindi le pipeline possono essere eseguite tramite il self-hosted Agent.
2. Con un solo parallel job self-hosted può essere eseguito un solo job alla volta.
3. No, registrare 3 Agent non aumenta automaticamente a 3 i job concorrenti, perché il numero di job eseguibili contemporaneamente dipende dai Parallel Jobs disponibili, non solo dal numero di Agent.

## 5. Agent audit

- pool: `pool-ud09-wsl`
- agent: `lab-ud09`
- status: `Online`
- version: `5.279.0`
- Agent.OS: `Linux`
- Agent.Version: `5.279.0`
- git: `/usr/bin/git`
- python: `/usr/bin/python3`

## 6. Registration authentication audit

- metodo: PAT
- status PAT, se applicabile: `Revoked`

1. L'agent resta Online dopo la revoca del PAT perché il PAT è servito solo per la registrazione iniziale dell'agent; dopo la registrazione l'agent utilizza proprie credenziali per comunicare con Azure DevOps.
2. Non serve un PAT Full access perché per registrare l'agent è sufficiente lo scope minimo `Agent Pools (Read & manage)`, rispettando il principio del least privilege.
3. Device Code Flow è un fallback migliore perché consente l'autenticazione interattiva senza ampliare i permessi di un PAT o modificare policy di sicurezza.

## 7. Offline/Online

- stop: `Ctrl+C`
- stato: `Offline`
- restart: `cd "$HOME/azdo-agent"` e `./run.sh`
- stato: `Online`


## 8. Troubleshooting

1. Verificare che il processo `run.sh` sia effettivamente in esecuzione.
2. Verificare la connettività di rete e l'accesso HTTPS verso Azure DevOps.
3. Verificare che l'Organization URL configurato sia corretto.
4. Verificare che l'agent sia registrato nel pool corretto.
5. Verificare la directory dell'agent e che la configurazione sia ancora presente.
6. Se il problema persiste, eseguire `./run.sh --diagnostics` e analizzare le informazioni diagnostiche prodotte.


## 9. Readiness

| Controllo | PASS/FAIL | Nota |
|---|---|---|
| Organization | PASS |  |
| Project | PASS |  |
| GitHub readiness | PASS |  |
| Hosted | PASS |  |
| Pool | PASS | |
| Agent | PASS | |
| PAT / Device Code Flow | PASS |  |
| Capability | PASS |  |
| Restart | PASS |  |
| Secrets | PASS |  |
