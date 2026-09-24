# UD13 — Consegna LAB guidato

## Terraform
- network.tf: copiato del repository del corso 
- plan: `Plan: 4 to add, 0 to change, 0 to destroy.`
- apply: `Apply complete! Resources: 4 added, 0 changed, 0 destroyed.`
- VNet verificata: si
- subnet verificata: si
- destroy: ` Destroy complete! Resources: 4 destroyed.`
- RG Terraform eliminato: si, `az group exists --name rg-ud12-tf` ha restituito `false`

## Delivery persistente
- Resource Group: `rg-ud13-15-delivery`
- service connection: `sc-azure-ud13-15`
- WIF: si
- scope: Resource Group `rg-ud13-15-delivery`
- accesso globale a tutte le pipeline: NO

## Pipeline
- YAML: `pipelines/azure-pipelines-iac.yml`
- agent pool: `pool-ud09-wsl`
- Validate: `1 job completed` 
- Deploy: `1 job completed` 
- What-If: `What-If and deploy ACR - 0 error(s), 0 warning(s)`
- deployment: ok 
- ACR: `acrud1315fo62jyh66yvzq`
- SKU: Basic
- admin user: false

## Fine UD13
- risorse delivery conservate: sì
