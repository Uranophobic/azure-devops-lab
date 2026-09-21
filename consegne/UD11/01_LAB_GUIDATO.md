# UD11 — Consegna laboratorio guidato

## Preflight

- Azure: autenticazione eseguita correttamente sulla subscription Azure
- Docker: funzionante, Client e Server disponibili tramite Docker Desktop
- containerapp extension: installata, versione 1.3.0b5
- Microsoft.App: Registered
- Microsoft.OperationalInsights: Registered
- Microsoft.ContainerRegistry: Registered

## ACR
Creata l'ACR nella località `italynorth`, appartenente al Resource Group `rg-ud11-containers`.

- nome: `acr1789986260ud11`
- SKU: `Basic`
- login server: `acr1789986260ud11.azurecr.io`
- repository: `catalog-backend`
- tag v1: `v1`
- tag v2: da completare
- admin user abilitato: NO

## v1
Costruita e testata localmente la prima versione dell'applicazione, poi pubblicata in ACR e distribuita manualmente tramite Azure Container Apps.

- local test: riuscito, endpoint `/health` con `status: ok` e `version: v1`
- push: riuscito
- ACR tag: `v1`
- ACA environment: `acaenv-ud11`
- Container App: `catalog-api-ud11`
- managed identity: `SystemAssigned`
- registry identity: `system`
- ingress: external
- target port: `8000`
- FQDN: `catalog-api-ud11.gentleground-309289d6.italynorth.azurecontainerapps.io`
- health: `status: ok`
- version: `v1`
- revision: `catalog-api-ud11--v1`
- logs: backend `v1` in ascolto sulla porta `8000`; richieste `/health` e `/api/products` completate con HTTP `200`

## v2
Creata una nuova versione dell'immagine, pubblicata con il tag `v2` e utilizzata per aggiornare la Container App. L'aggiornamento ha generato una nuova revision, permettendo di verificare il passaggio da `v1` a `v2`.

- Dockerfile version: `APP_VERSION=v2`
- local test: riuscito, endpoint `/health` con `version: v2`
- push: riuscito
- ACR tag: `v2`
- update: aggiornamento della Container App riuscito con image `v2`
- revision: `catalog-api-ud11--v2`
- health: `Healthy`
- version: `v2`

## Scaling
Verificata la configurazione di scaling della Container App e la possibilità di ridurre il numero di repliche fino a zero quando non ci sono richieste o eventi da elaborare. 

- minReplicas: `0`
- maxReplicas: `1`
- scale-to-zero possibile: sì
