# UD09 — Consegna laboratorio guidato

## Organization

- nome: `azdo-alessia-01`
- geography: `Europe`
- Azure subscription collegata: sì

## Project

- nome: `az900-az104-devops`
- visibility: Private
- version control: Git
- process: Agile

## Gruppi

| Gruppo | Funzione essenziale |
|---|---|
| Project Administrators | gruppo per gli amministratori di progetto i quali hanno persmetti amministrativi completi, incluse configurazioni e autorizzazioni |
| Contributors | gruppo per utenti che possono lavorare sul progetto eseguendo azioni come creare, modificare contenuti o risorse operative |
| Readers |  gruppo per utenti i quali hanno permessi solamente in lettura delle risorse del progetto |
| Build Administrators | gruppo per amministratori con permessi relativi a build e pipeline |

## GitHub

- repository: `Uranophobic/azure-devops-lab`
- repository privato: si
- `gh auth status`: logged
- `git fetch`: 
- connessione OAuth/PAT creata in UD09: NO
- metodo raccomandato per CI futura: Azure Pipelines GitHub App

## Parallel jobs

- Microsoft-hosted: 1 parallel job
- esito hosted: `Free tier - 1 parallel job, up to 1800 min/month`
- self-hosted: 1 parallel job
- billing verificato: `yes`

## Agent Pool

- creato da Project settings: sì
- nome: `pool-ud09-wsl`
- tipo: Self-hosted
- accesso automatico a tutte le pipeline: no

## Autenticazione registrazione agent

- metodo usato: PAT
- PAT name, se usato: `ud09-agent-registration`
- PAT scope, se usato: `Agent Pools (Read & manage)`
- expiration: 30 days
- inserito in file/repository: NO
- PAT revocato, se usato: si

## Agent

- nome: `lab-ud09`
- pool: `pool-ud09-wsl`
- OS: linux
- version: 5.279.0
- status: online
- modalità: interattiva
- credenziale di registrazione chiusa e agent ancora Online: sì

## Capability

- Agent.OS: Linux
- Agent.Version: 5.279.0
- git: `/usr/bin/git`
- python: `/usr/bin/python3`
- PATH verificato: si

## Readiness

- Organization: `azdo-alessia-01`
- Project: `az900-az104-devops`
- GitHub: `Uranophobic/azure-devops-lab`
- hosted:  `Free tier - 1 parallel job, fino a 1800 min/mese`
- self-hosted:  `1 parallel job disponibile`
- agent: `lab-ud09`
- security: PAT temporaneo revocato
