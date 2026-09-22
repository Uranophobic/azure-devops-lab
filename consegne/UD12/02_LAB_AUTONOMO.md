# UD12 — Consegna LAB autonomo

## Baseline

- `terraform plan`: eseguito
- stato atteso: `No changes.`
- stato osservato: `No changes. Your infrastructure matches the configuration.`

## Modifica

- tag aggiunto: `Environment = "Training"`
- plan add: 0
- plan change: 2
- plan destroy: 0
- motivazione change e non recreate: il tag è una proprietà modificabile della risorsa e può essere aggiornato in-place. Terraform propone quindi una modifica delle risorse esistenti senza distruggerle e ricrearle.

## Apply e verifica

- apply: `Apply complete! Resources: 0 added, 2 changed, 0 destroyed.`
- tag verificato con Azure CLI: sì, presente `Environment = "Training"`

## Errore controllato

- riferimento errato: `location = azurerm_resource_group.training.location`
- messaggio `terraform validate`:  `Error: Reference to undeclared resource`
- causa: `A managed resource "azurerm_resource_group" "training" has not been declared in the root module.`
- correzione:  ripristinato `location = azurerm_resource_group.lab.location`
- validate finale: `Success! The configuration is valid.`

## Git

- `.gitignore` verificato: si
- state non committato: si
- lock file: `.terraform.lock.hcl` mantenuto e versionato
- commit:
- push/PR:

## Cleanup Terraform

- `terraform plan -destroy`: `Plan: 0 to add, 0 to change, 2 to destroy.`
- risorse previste:
    - `azurerm_storage_account.lab`
    - `azurerm_resource_group.lab`
- `terraform destroy`: `Destroy complete! Resources: 2 destroyed.`
- `az group exists`: `false`
- `terraform state list` finale: nessuna risorsa presente

## Elementi conservati per UD13–UD15

- Bicep: si
- Terraform: si
- file IaC: si
- agent configurato: si 
