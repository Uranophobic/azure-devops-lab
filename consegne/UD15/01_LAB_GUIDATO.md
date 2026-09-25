# UD15 — Consegna LAB guidato

## Controlli iniziali
- ACR: presente nel Resource Group `rg-ud13-15-delivery`
- `LegacyRegistryPermissions`: verificato
- `sc-azure-ud13-15`:  presente e configurata con Workload Identity Federation
- `AcrPush`: assegnato al Service Principal associato a `sc-azure-ud13-15`
- Storage state: creato Storage Account `sttf15b83364ad236c4acf`
- container state:  creato container `tfstate`
- `Storage Blob Data Contributor`:  assegnato al Service Principal della pipeline sul solo container `tfstate`
- managed identity:  creata `id-ud15-acrpull`
- `AcrPull`: assegnato alla managed identity sull'ACR
- Agent:  `lab-ud09` nel pool `pool-ud09-wsl`

## Terraform
- init:  eseguito localmente con `terraform init -backend=false`
- validate:  completato con `Success! The configuration is valid.`
- plan iniziale: eseguito nello stage `IaC` della pipeline tramite `terraform plan`
- apply: eseguito nello stage `Deploy` tramite `terraform apply`
- state remoto: creato nel backend AzureRM come blob `ud15.tfstate` nel container `tfstate`

## Pipeline
- Build ID: `15`
- IaC: completato, eseguiti `terraform init`, `terraform fmt -check`, `terraform validate` e `terraform plan`
- Test: completato
- BuildPush: completato tramite `AzureCLI@2`, `az acr login`, `docker build` e `docker push`
- Deploy: completato, eseguiti `terraform init`, `terraform plan` e `terraform apply`
- Smoke: completato, endpoint applicativo verificato dopo il deployment

## Deployment
- image tag: `15`
- FQDN: `catalog-api-ud15.mangosand-dc1ffd1a.italynorth.azurecontainerapps.io`
- APP_VERSION: `15`
- revision: `catalog-api-ud15--thkkio9`
- health: endpoint `/health` verificato con risposta `status: ok`, `service: catalog-backend`, `version: 15`
