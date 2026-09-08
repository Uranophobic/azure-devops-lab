# Consegna UD02 — Laboratorio guidato

## Contesto verificato

- Azure Portal accessibile: sì.
- Azure CLI autenticata: sì, verificata tramite `az account show`.
- Sottoscrizione corretta verificata senza pubblicarne l'ID: sì, tramite `az account list`; è risultata disponibile una sola sottoscrizione, `Azure subscription 1`, con stato `Enabled` e impostata come predefinita.
- Località scelta e motivo: `italynorth`, perché è risultata disponibile per la sottoscrizione tramite `az account list-locations`.

## Ambiente creato

| Elemento | Nome tecnico | Tipo | Località | Scopo |
|---|---|---|---|---|
| Resource group | `rg-cea-ud02-3175f0df` | Resource Group | Italy North | Raggruppare e gestire le risorse create per il laboratorio |
| Rete virtuale | `vnet-cea-ud02` | `Microsoft.Network/virtualNetworks` | Italy North | Creare una rete privata virtuale con spazio di indirizzi `10.20.0.0/16` e subnet `snet-app` con prefisso `10.20.1.0/24` |
| Storage account | `stcea3175f0df` | `Microsoft.Storage/storageAccounts` | Italy North | Creare un account di archiviazione |

## Decisioni e verifiche

Le risorse sono state inserite nello stesso Resource Group perché appartengono allo stesso contesto e hanno lo stesso ciclo di vita. In questo modo possono essere individuate, gestite e successivamente eliminate in modo centralizzato.

Anche utilizzando servizi gestiti da Azure, alcune responsabilità rimangono al cliente, tra cui la corretta configurazione delle risorse, la gestione degli accessi e delle identità, la protezione dei dati e la scelta delle impostazioni di sicurezza appropriate. Azure gestisce invece l'infrastruttura cloud sottostante.

Sono stati utilizzati nomi coerenti con il contesto e un suffisso casuale per rendere univoci i nomi che lo richiedono, come quello dello Storage Account. Sono stati inoltre applicati i tag:

- `course = cloud-engineer-academy`
- `unit = UD02`
- `environment = lab`
- `deleteAfter = 2026-09-10`

I tag permettono di identificare lo scopo delle risorse e facilitano inventario, gestione e cleanup.

Per verificare che i Resource Provider necessari fossero utilizzabili sono stati controllati `Microsoft.Network` e `Microsoft.Storage`. Entrambi risultavano inizialmente `NotRegistered` e sono stati registrati; al termine risultavano `Registered`.

La configurazione del Resource Group è stata verificata con:

```bash
az group show \
  --name "$LAB_RG" \
  --query "{Name:name,Location:location,State:properties.provisioningState,Tags:tags}" \
  --output jsonc
```

Il risultato ha confermato la località `italynorth`, lo stato `Succeeded` e la presenza dei quattro tag.

La VNet e la subnet sono state controllate tramite Azure CLI con il comando:

```bash
az network vnet show \
  --resource-group "$LAB_RG" \
  --name "$LAB_VNET" \
  --query "{Name:name,Location:location,Address:addressSpace.addressPrefixes,Subnets:subnets[].{Name:name,Prefixes:addressPrefixes},Tags:tags}" \
  --output jsonc
```

Il risultato è stato:

```json
{
  "Address": [
    "10.20.0.0/16"
  ],
  "Location": "italynorth",
  "Name": "vnet-cea-ud02",
  "Subnets": [
    {
      "Name": "snet-app",
      "Prefixes": [
        "10.20.1.0/24"
      ]
    }
  ],
  "Tags": {
    "course": "cloud-engineer-academy",
    "deleteAfter": "2026-09-10",
    "environment": "lab",
    "unit": "UD02"
  }
}
```

Prima della creazione dello Storage Account è stata verificata la disponibilità globale del nome ed il risultato è stato:

```json
{
  "Available": true
}
```

Dopo la creazione, lo Storage Account è stato verificato con:

```bash
az storage account show \
  --resource-group "$LAB_RG" \
  --name "$LAB_STORAGE" \
  --query "{Name:name,Location:location,Kind:kind,Sku:sku.name,HttpsOnly:enableHttpsTrafficOnly,MinimumTls:minimumTlsVersion,PublicBlobAccess:allowBlobPublicAccess,State:provisioningState,Tags:tags}" \
  --output jsonc
```

Il risultato è stato:

```json
{
  "HttpsOnly": true,
  "Kind": "StorageV2",
  "Location": "italynorth",
  "MinimumTls": "TLS1_2",
  "Name": "stcea3175f0df",
  "PublicBlobAccess": false,
  "Sku": "Standard_LRS",
  "State": "Succeeded",
  "Tags": {
    "course": "cloud-engineer-academy",
    "deleteAfter": "2026-09-10",
    "environment": "lab",
    "unit": "UD02"
  }
}
```

L'inventario finale del Resource Group ha mostrato le due risorse create. Per riportare gli ID nell'evidenza senza pubblicare l'ID reale della sottoscrizione è stato utilizzato:

```bash
az resource list \
  --resource-group "$LAB_RG" \
  --query "[].id" \
  --output tsv \
  | sed -E 's#/subscriptions/[^/]+#/subscriptions/<omitted>#'
```

Ha prodotto il seguente output anonimizzato:

```text
/subscriptions/<omitted>/resourceGroups/rg-cea-ud02-3175f0df/providers/Microsoft.Network/virtualNetworks/vnet-cea-ud02
/subscriptions/<omitted>/resourceGroups/rg-cea-ud02-3175f0df/providers/Microsoft.Storage/storageAccounts/stcea3175f0df
```

## Cleanup

- operazione di eliminazione:
- controllo utilizzato:
- risultato finale:
- eventuale anomalia e soluzione:

## Rilevanza professionale

L'inventario permette di verificare quali risorse fanno realmente parte di un ambiente e di controllarne rapidamente tipo, località e proprietà principali. L'uso di nomi coerenti e tag rende più semplice identificare le risorse, distinguerne lo scopo e gestirne il ciclo di vita.

La verifica tramite CLI rende inoltre i controlli ripetibili, perché gli stessi comandi possono essere rieseguiti in momenti diversi ottenendo informazioni precise e confrontabili. Infine, verificare esplicitamente il cleanup permette di accertarsi che le risorse temporanee siano state realmente eliminate, evitando di lasciare risorse inutilizzate attive nella sottoscrizione.

## Differenza fra Portale Azure e Azure CLI

Durante il laboratorio ho utilizzato sia il portale grafico di Azure sia la CLI. Il portale mi ha permesso di vedere in modo più chiaro e visivo come fossero organizzate le risorse create e di comprenderne meglio le proprietà. Inoltre, il processo di creazione di risorse come lo Storage Account, la VNet e la subnet tramite il portale rende i vari passaggi più semplici da seguire.

D'altra parte, la CLI permette di effettuare controlli in modo più rapido e mirato, mostrando soltanto i campi necessari. Ad esempio, ho utilizzato il comando `az network vnet show` per verificare dalla mia WSL che i valori configurati nel portale fossero stati effettivamente salvati correttamente su Azure. Ho trovato utile anche il comando `az storage account check-name`, che permette di verificare rapidamente se il nome scelto per lo Storage Account è disponibile e quindi univoco a livello globale.

In conclusione, ho trovato il portale più comodo per creare le risorse ed esplorarne le proprietà, mentre la CLI è più efficace per eseguire controlli rapidi, mirati e facilmente ripetibili.