# Consegna UD02 — Laboratorio autonomo

## Pianificazione

Le risorse verranno create all'interno di un Resource Group dedicato, così da raggruppare tutte le risorse appartenenti allo stesso contesto e con lo stesso ciclo di vita. Come località utilizzerò `Italy North`, già verificata in precedenza come disponibile per la mia sottoscrizione.

La VNet ha lo scopo di definire la rete virtuale e la relativa subnet, mentre lo Storage Account rappresenta il servizio di archiviazione. Entrambe le risorse risiederanno nello stesso Resource Group. Alle risorse verranno applicati dei tag, utilizzati come metadati per identificarne e descriverne alcune caratteristiche, come ad esempio il tag `deleteAfter`, che rappresenta la data prevista per il cleanup.

Al termine delle attività, il cleanup verrà dimostrato non soltanto eseguendo il comando di eliminazione del Resource Group, ma anche verificando esplicitamente tramite Azure CLI che il gruppo non esista più. In particolare, utilizzerò il controllo `az group exists --name "$AUTO_RG"` e considererò il cleanup completato solo se il comando restituirà `false`.

## Requisito e piano

- requisito interpretato: creare tramite Azure CLI un ambiente composto da un Resource Group dedicato, una VNet con subnet e uno Storage Account, utilizzando la località già verificata e applicando tag coerenti alle risorse. La configurazione dovrà essere verificata sia tramite CLI sia tramite Azure Portal.

- risorse previste:
  - Resource Group `rg-cea-ud02-auto-26be3545`
  - VNet `vnet-cea-auto` con spazio di indirizzi `10.30.0.0/16`
  - subnet `snet-workload` con prefisso `10.30.10.0/24`
  - Storage Account `stceaauto26be3545`

- nomi e tag scelti:
  - `course = cloud-engineer-academy`
  - `unit = UD02`
  - `environment = dev`
  - `scenario = autonomous`
  - `deleteAfter = 2026-09-10`

- verifiche preliminari: è stata verificata la sottoscrizione attiva tramite `az account show`. Prima della creazione dello Storage Account è stata controllata la disponibilità del nome tramite `az storage account check-name`, che ha restituito `Available: true`.

## Svolgimento

Il Resource Group è stato creato tramite Azure CLI con:

```bash
az group create \
  --name "$AUTO_RG" \
  --location "$AUTO_LOCATION" \
  --tags \
    course=cloud-engineer-academy \
    unit=UD02 \
    environment=dev \
    scenario=autonomous \
    deleteAfter="$AUTO_DELETE_AFTER"
```

La configurazione è stata verificata tramite:

```bash
az group show \
  --name "$AUTO_RG" \
  --query "{Name:name,Location:location,State:properties.provisioningState,Tags:tags}" \
  --output jsonc
```

Il controllo ha confermato:

```text
{
  "Location": "italynorth",
  "Name": "rg-cea-ud02-auto-26be3545",
  "State": "Succeeded",
  "Tags": {
    "course": "cloud-engineer-academy",
    "deleteAfter": "2026-09-10",
    "environment": "dev",
    "scenario": "autonomous",
    "unit": "UD02"
  }
}
```

La VNet e la relativa subnet sono state create tramite:

```bash
az network vnet create \
  --resource-group "$AUTO_RG" \
  --name "$AUTO_VNET" \
  --location "$AUTO_LOCATION" \
  --address-prefixes 10.30.0.0/16 \
  --subnet-name "$AUTO_SUBNET" \
  --subnet-prefixes 10.30.10.0/24 \
  --tags \
    course=cloud-engineer-academy \
    unit=UD02 \
    environment=dev \
    scenario=autonomous \
    deleteAfter="$AUTO_DELETE_AFTER"
```

La configurazione è stata successivamente verificata con:

```bash
az network vnet show \
  --resource-group "$AUTO_RG" \
  --name "$AUTO_VNET" \
  --query "{Name:name,Location:location,Address:addressSpace.addressPrefix,Subnets:subnets[].{Name:name,Prefixes:addressPrefixes},Tags:tags}" \
  --output jsonc
```

Output:

```json
{
  "Address": [
    "10.30.0.0/16"
  ],
  "Location": "italynorth",
  "Name": "vnet-cea-auto",
  "Subnets": [
    {
      "Name": "snet-workload",
      "Prefix": "10.30.10.0/24"
    }
  ],
  "Tags": {
    "course": "cloud-engineer-academy",
    "deleteAfter": "2026-09-10",
    "environment": "dev",
    "scenario": "autonomous",
    "unit": "UD02"
  }
}
```

Lo Storage Account è stato creato con le impostazioni richieste:

```bash
az storage account create \
  --name "$AUTO_STORAGE" \
  --resource-group "$AUTO_RG" \
  --location "$AUTO_LOCATION" \
  --sku Standard_LRS \
  --kind StorageV2 \
  --https-only true \
  --min-tls-version TLS1_2 \
  --allow-blob-public-access false \
  --tags \
    course=cloud-engineer-academy \
    unit=UD02 \
    environment=dev \
    scenario=autonomous \
    deleteAfter="$AUTO_DELETE_AFTER"
```

La verifica è stata effettuata tramite:

```bash
az storage account show \
  --resource-group "$AUTO_RG" \
  --name "$AUTO_STORAGE" \
  --query "{Name:name,Location:location,Kind:kind,Sku:sku.name,HttpsOnly:enableHttpsTrafficOnly,MinimumTls:minimumTlsVersion,PublicBlobAccess:allowBlobPublicAccess,State:provisioningState,Tags:tags}" \
  --output jsonc
```

Il risultato ha confermato:

```json
{
  "HttpsOnly": true,
  "Kind": "StorageV2",
  "Location": "italynorth",
  "MinimumTls": "TLS1_2",
  "Name": "stceaauto26be3545",
  "PublicBlobAccess": false,
  "Sku": "Standard_LRS",
  "State": "Succeeded",
  "Tags": {
    "course": "cloud-engineer-academy",
    "deleteAfter": "2026-09-10",
    "environment": "dev",
    "scenario": "autonomous",
    "unit": "UD02"
  }
}
```

Infine è stato eseguito l'inventario delle risorse presenti nel Resource Group:

```bash
az resource list \
  --resource-group "$AUTO_RG" \
  --query "[].{Name:name,Type:type,Location:location,Environment:tags.environment,Scenario:tags.scenario,DeleteAfter:tags.deleteAfter}" \
  --output table
```

Il risultato ha mostrato:

```text
Name               Type                               Location    Environment    Scenario    DeleteAfter
-----------------  ---------------------------------  ----------  -------------  ----------  -------------
vnet-cea-auto      Microsoft.Network/virtualNetworks  italynorth  dev            autonomous  2026-09-10
stceaauto26be3545  Microsoft.Storage/storageAccounts  italynorth  dev            autonomous  2026-09-10
```

La verifica è stata ripetuta anche tramite Azure Portal, come si può verificare nell'immagine sottostante, dove sono risultate presenti nello stesso Resource Group la VNet `vnet-cea-auto` e lo Storage Account `stceaauto26be3545`, entrambi nella località `Italy North`. Il portale ha inoltre confermato la presenza dei tag previsti.

(ud02-lab-auto-resource-group.png)

## Diagnosi

- errore o anomalia analizzata: durante il cleanup Azure CLI ha restituito l'errore `AADSTS530035: Access has been blocked by security defaults`, impedendo il completamento dell'autenticazione e quindi l'esecuzione dei comandi Azure.

- ipotesi: il problema era legato alla policy di autenticazione del tenant e non alla sottoscrizione o alle risorse create

- controllo: sono stati verificati l'accesso al portale Azure e la presenza della sottoscrizione e delle risorse. È stato inoltre controllato che l'errore si presentasse durante la fase di autenticazione della CLI.

- correzione: è stato ripristinato l'accesso ad Azure CLI effettuando nuovamente l'autenticazione con una configurazione compatibile con le policy del tenant.

- verifica successiva: dopo il ripristino dell'autenticazione, `az account show` ha nuovamente mostrato la sottoscrizione con stato `Enabled` e `IsDefault = True`. Il cleanup è stato quindi completato e il comando `az group exists --name "$AUTO_RG"` ha restituito `false`, confermando l'eliminazione del Resource Group.

## Cleanup e consegna

- risorse eliminate: eliminato il Resource Group `rg-cea-ud02-auto-26be3545` e, con esso, la VNet `vnet-cea-auto` e lo Storage Account `stceaauto26be3545`.
- controllo finale: eseguito `az group exists --name "$AUTO_RG"` dopo `az group wait --name "$AUTO_RG" --deleted`.
- hash abbreviato e messaggio del commit: da compilare dopo il commit finale.