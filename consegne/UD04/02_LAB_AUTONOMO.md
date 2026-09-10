# Consegna UD04 — Laboratorio autonomo

## Scelta del servizio e della ridondanza

Per lo scenario richiesto sceglierei **Blob**, perché è progettato per l'archiviazione di oggetti organizzati in container. Queue è invece destinato alla gestione di messaggi e alla comunicazione asincrona, Files mette a disposizione file share condivise, Table è un servizio NoSQL per dati strutturati in entità.

Per quanto riguarda la ridondanza, **LRS** replica localmente i dati all'interno della stessa region ed è la soluzione più economica, quindi è adatta a un laboratorio. Mentre ZRS distribuisce le copie dei dati tra Availability Zone distinte della stessa region, garantendo maggiore resilienza rispetto a LRS.

## Operazioni e verifica

Ho creato da CLI un secondo container privato denominato `archive`, utilizzando `--auth-mode login`.
In questo modo l'operazione è stata effettuata tramite la mia identità Microsoft Entra ID e le autorizzazioni RBAC, senza utilizzare una account key. 
Nel container `archive` ho successivamente caricato una copia di `01_DOCUMENTO_LAB.txt` con il nome `current/documento.txt`, sempre utilizzando `--auth-mode login`.

Durante il primo tentativo di upload ho ricevuto un errore di autorizzazione. La verifica delle assegnazioni RBAC ha mostrato che il ruolo `Storage Blob Data Contributor` era già presente sul container `documents`, ma non sul nuovo container `archive`. Ho quindi assegnato lo stesso ruolo al mio utente direttamente sul container `archive`. Subito dopo l'assegnazione, un nuovo tentativo ha continuato a restituire errore; dopo alcuni minuti, senza modificare il comando, l'upload è invece riuscito correttamente. Questo comportamento è dovuto al tempo necessario per la propagazione dell'assegnazione RBAC.

Ho verificato il Blob presente nel container con:

```bash
az storage blob list \
  --account-name "$LAB_STORAGE" \
  --container-name archive \
  --auth-mode login \
  --query "[].{Blob:name,Tier:properties.blobTier,Bytes:properties.contentLength}" \
  --output table
```

Output ottenuto:

```text
Blob                   Tier    Bytes
---------------------  ------  -----
current/documento.txt  Hot     49
```

La verifica conferma quindi che il Blob `current/documento.txt` è presente nel container `archive`.
Per consentire un accesso temporaneo al singolo Blob, ho generato una **User Delegation SAS** tramite Microsoft Entra ID, la quale è limitata solo a questo specifico blob, con permesso di lettura e durata di 15 minuti. 

Ho verificato che l'accesso tramite SAS consentisse correttamente il download del file e, al termine della prova ho rimosso la variabile.

## Diagnosi

### `AuthorizationPermissionMismatch`

Durante un upload tramite `--auth-mode login` può verificarsi un errore di autorizzazione quando l'utente è correttamente autenticato tramite Microsoft Entra ID, ma non possiede i permessi necessari per eseguire l'operazione richiesta.
Nel mio caso il ruolo `Storage Blob Data Contributor` era assegnato al mio utente sul container `documents`, ma non sul nuovo container `archive`. Di conseguenza ero correttamente autenticata, ma non autorizzata a caricare Blob in `archive`.
Questo dimostra anche che possedere il ruolo `Owner` sulla sottoscrizione non significa poter eseguire automaticamente tutte le operazioni sul data plane dello Storage Account.

- piano: data plane;
- causa: manca un ruolo dati adeguato sullo scope corretto;
- controllo: verificare le assegnazioni RBAC e il relativo scope con:

```bash
az role assignment list \
  --all \
  --query "[].{Role:roleDefinitionName,Scope:scope}" \
  --output table
```

### `ResourceNotFound: The specified container does not exist`

Questo errore si verifica quando si tenta di eseguire un'operazione su un container che Azure non riesce a trovare o che non esiste.

- piano: data plane;
- causa: il container non è stato creato, il nome è errato oppure si sta utilizzando lo Storage Account sbagliato;
- controllo: verificare l'elenco dei container presenti nello Storage Account con:

```bash
az storage container list \
  --account-name "$LAB_STORAGE" \
  --auth-mode login \
  --query "[].name" \
  --output table
```

### `curl: (22) The requested URL returned error: 403`

L'errore HTTP `403 Forbidden` indica che la richiesta ha raggiunto il servizio Azure Storage, ma l'accesso alla risorsa non è autorizzato.

- piano: data plane;
- causa: SAS non valida per l'operazione richiesta, ad esempio perché scaduta oppure generata per una risorsa differente;
- controllo: verificare che la SAS abbia scope, permessi e scadenza corretti.

## Lifecycle, costi e cleanup

La Lifecycle Management Policy configurata nel laboratorio guidato utilizza il prefisso:

```text
documents/temporary/
```

Il prefisso comprende sia il nome del container sia il percorso del Blob. Di conseguenza la regola si applica soltanto ai Blob che si trovano nel container `documents` e il cui nome inizia con `temporary/`.

Il Blob creato nel laboratorio autonomo si trova invece in archive/current/documento.txt dunque la Lifecycle managment non può quindi applicarsi a questo Blob.

Nel laboratorio è stato utilizzato `Standard_LRS` e sono stati memorizzati file di dimensioni molto ridotte, mantenendo quindi limitato il consumo di risorse.

### Cleanup

Da effettuare al termine di tutte le attività UD04.

## Risultato finale

- nessun segreto pubblicato: si
- hash abbreviato e messaggio del commit: