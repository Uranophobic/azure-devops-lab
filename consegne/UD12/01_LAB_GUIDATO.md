# UD12 — Consegna LAB guidato

## Preparazione

- materiali UD12: `~/workspace/corso-azure-devops/UD12/partecipanti`
- repository personale: `~/workspace/azure-devops-lab`
- directory `infra/bicep`: `~/workspace/azure-devops-lab/infra/bicep`
- directory `infra/terraform`: `~/workspace/azure-devops-lab/infra/terraform`
- directory consegne: `~/workspace/azure-devops-lab/consegne/UD12`

## Azure

- subscription verificata: si 
- identità verificata: si

## Bicep

- Bicep version: `v0.47.16`
- lint: completato senza errori
- Resource Group: `rg-ud12-bicep`
- Storage Account:  `stud12b90073732`
- What-If change type: `Create`
- deployment:  `ud12-bicep-deploy`
- output `storageAccountName`: `stud12b90073732`
- output `blobEndpoint`: `https://stud12b90073732.blob.core.windows.net/`
- verifica CLI: si
- Resource Group Bicep eliminato: si
- `az group exists`: `false`

## Terraform

- Terraform version: `v1.16.3`
- provider AzureRM: `v5.6.0`
- `terraform init`: `Terraform has been successfully initialized!`
- `terraform fmt -check`: ok
- `terraform validate`: `Success! The configuration is valid.`
- plan add: 2
- plan change: 0
- plan destroy: 0
- apply: `Apply complete! Resources: 2 added, 0 changed, 0 destroyed.`
- output Resource Group: `rg-ud12-tf`
- output Storage: `stud12t90082614`
- verifica CLI: superata
- `terraform state list`: 
    - `azurerm_resource_group.lab`
    - `azurerm_storage_account.lab`

## Fine LAB guidato

- risorse Terraform mantenute per LAB autonomo: sì