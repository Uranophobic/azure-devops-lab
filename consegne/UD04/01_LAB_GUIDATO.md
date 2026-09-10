# Consegna UD04 — Laboratorio guidato

## Contesto anonimizzato

- resource group: `rg-cea-storage-1ef6d0b4`
- storage account: `stcea1ef6d0b4`
- region: `italynorth`
- tipo e ridondanza: `StorageV2` con `Standard_LRS`

## Servizi e configurazione

| Elemento | Configurazione | Motivazione |
|---|---|---|
| Blob container | `documents` | Contenere e organizzare i Blob utilizzati nel laboratorio. |
| access tier | `Hot` | Adatto a dati utilizzati frequentemente e immediatamente disponibili. |
| accesso pubblico | Disabilitato | Evitare accessi anonimi ai Blob e mantenere i dati privati. |
| trasferimento/TLS | HTTPS obbligatorio, TLS minimo `TLS1_2` | Proteggere i dati durante il trasferimento e impedire l'utilizzo di protocolli non sicuri. |

## Autorizzazione e lifecycle

Per accedere ai dati tramite Microsoft Entra ID è stato assegnato al mio utente il ruolo `Storage Blob Data Contributor` con scope limitato allo Storage Account.
L'accesso tramite Entra ID utilizza l'identità dell'utente e le autorizzazioni definite tramite Azure RBAC, permettendo quindi di controllare in modo preciso chi può accedere ai dati.
La Shared Key, invece, è una chiave associata allo Storage Account che consente un accesso molto ampio e deve essere trattata come un segreto ad alto impatto. Nel laboratorio è stata utilizzata temporaneamente per verificare l'accesso ai Blob.
È stata generata una User Delegation SAS tramite Microsoft Entra ID per il Blob `01_DOCUMENTO_LAB.txt`, con permesso di sola lettura e una scadenza di 30 minuti. La SAS è stata utilizzata per scaricare il file e verificarne il contenuto.

È stata configurata una Lifecycle Management Policy con le seguenti caratteristiche:

- nome regola: `delete-temporary`
- stato: abilitata
- tipo Blob: Block blobs
- sottotipo: Base blobs
- prefisso: `documents/temporary/`
- azione: eliminazione dei Blob dopo 1 giorno dall'ultima modifica

Il filtro sul prefisso permette di applicare la regola soltanto ai Blob temporanei senza coinvolgere gli altri file presenti nel container.

## Verifiche, costi e cleanup

### Verifiche
L'accesso al data plane tramite **Microsoft Entra ID** è stato verificato con:

```bash
az storage container list \
  --account-name "$LAB_STORAGE" \
  --auth-mode login \
  --query "[].{Container:name,PublicAccess:properties.publicAccess}" \
  --output table
```

Risultato:

```text
Container
---------
documents
```

La presenza del Blob è stata verificata con:

```bash
az storage blob list \
  --account-name "$LAB_STORAGE" \
  --container-name "$LAB_CONTAINER" \
  --auth-mode login \
  --query "[].{Blob:name,Tier:properties.blobTier,Bytes:properties.contentLength}" \
  --output table
```

Risultato:

```text
Blob                  Tier    Bytes
--------------------  ------  -----
01_DOCUMENTO_LAB.txt  Hot     49
```

Dopo il download tramite Microsoft Entra ID, l'integrità del file è stata verificata con:

```bash
cmp consegne/UD04/01_DOCUMENTO_LAB.txt /tmp/documento-lab-scaricato.txt
```

Il comando non ha restituito output, confermando che il file scaricato coincide con l'originale.

L'accesso tramite **Shared Key** è stato verificato con:

```bash
az storage blob list \
  --account-name "$LAB_STORAGE" \
  --container-name "$LAB_CONTAINER" \
  --auth-mode key \
  --query "[].name" \
  --output table
```

Risultato:

```text
Result
------------------------
01_DOCUMENTO_LAB.txt
temporary/temporaneo.txt
```

Dopo il download tramite **User Delegation SAS**, il file è stato verificato con:

```bash
cmp consegne/UD04/01_DOCUMENTO_LAB.txt /tmp/documento-sas.txt
```

Anche in questo caso il comando non ha restituito output, confermando che il file ottenuto tramite SAS coincide con l'originale.

La configurazione della **Lifecycle Management Policy** è stata verificata con:

```bash
az storage account management-policy show \
  --account-name "$LAB_STORAGE" \
  --resource-group "$LAB_RG" \
  --query "policy.rules[].{Name:name,Enabled:enabled,Prefixes:definition.filters.prefixMatch,DeleteAfter:definition.actions.baseBlob.delete.daysAfterModificationGreaterThan}" \
  --output jsonc
```

Risultato:

```json
[
  {
    "DeleteAfter": 1.0,
    "Enabled": true,
    "Name": "delete-temporary",
    "Prefixes": [
      "documents/temporary/"
    ]
  }
]
```

La regola `delete-temporary` risulta quindi abilitata, limitata al prefisso `documents/temporary/` e configurata per l'eliminazione dopo 1 giorno dall'ultima modifica.

### Costi
I principali elementi che incidono sul costo di Azure Storage sono:

- capacità utilizzata;
- tipo di ridondanza;
- access tier;
- numero e tipo di operazioni;
- recupero dei dati;
- trasferimento dei dati.

Nel laboratorio è stato utilizzato `Standard_LRS` e sono stati caricati file di dimensioni molto ridotte.

### Cleanup finale

Al termine delle attività ho rimosso le assegnazioni temporanee del ruolo `Storage Blob Data Contributor` associate al mio utente e ho eliminato i file e le variabili temporanee utilizzate durante il laboratorio.

## Rilevanza professionale

Per la memorizzazione di documenti, immagini, log o altri file applicativi sceglierei Azure Blob Storage, perché è progettato per l'archiviazione di oggetti organizzati in container.

Utilizzerei invece Azure Files quando un'applicazione necessita di una vera e propria file share, organizzata in cartelle e file e accessibile tramite protocolli come SMB o NFS.

Per l'autorizzazione preferirei Microsoft Entra ID con Azure RBAC, perché permette di associare le autorizzazioni a identità specifiche e di applicare il principio del minimo, evitando quando possibile la distribuzione di Shared Key.
