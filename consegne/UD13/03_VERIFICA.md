# UD13 — Verifica

## 1.
Perché lo state Terraform contiene le informazioni che collegano il codice alle risorse realmente gestite. Se ogni membro del team ha una copia locale diversa, non è più chiaro quale sia lo stato corrente dell’infrastruttura e possono verificarsi conflitti o esecuzioni concorrenti. In un team è preferibile uno state remoto, condiviso e protetto.

## 2.
Lo stage è una fase logica della pipeline, ad esempio Validate o Deploy. Il job è un insieme di step eseguito da un Agent. Lo step è la singola operazione del job, ad esempio checkout, uno script o una task.

## 3.
Indica ad Azure Pipelines di recuperare il repository associato alla pipeline e copiarlo nel workspace dell’Agent, così che gli step successivi possano utilizzare i file del repository.

## 4.
Per applicare il principio del minimo privilegio. La pipeline può operare solo sul Resource Group necessario al laboratorio, invece di avere privilegi sull’intera subscription.

## 5.
Per permettere alla pipeline di autenticarsi verso Azure senza memorizzare un client secret statico. Azure DevOps ottiene un token federato che Microsoft Entra valida, riducendo la necessità di gestire e ruotare credenziali permanenti.

## 6.
Perché in UD13 Terraform viene usato per verificare che la configurazione IaC sia corretta, tramite fmt, init -backend=false e validate, ma il deployment persistente viene eseguito tramite Bicep. La pipeline non deve quindi applicare Terraform né gestire uno state remoto in questa unità.

## 7.
Devono sopravvivere il Resource Group rg-ud13-15-delivery e l’Azure Container Registry creato al suo interno, perché costituiscono la base che verrà riutilizzata nelle UD14 e UD15.

## 8.
Un problema di filesystem/path del repository. Cioè la pipeline sta cercando un file che non esiste nel percorso indicato.

## 9.
Per verificare concretamente l’Agent configurato nelle unità precedenti e osservare come Azure DevOps gli assegna il Job, crea il workspace, esegue il checkout e lancia gli step sul WSL2 preparato durante il corso.

## 10.
Serve a pulire completamente il workspace del Job prima dell’esecuzione. È importante sui self-hosted Agent perché il filesystem persiste tra una run e l’altra e potrebbero rimanere file prodotti da esecuzioni precedenti.

## 11.
La GitHub App permette ad Azure Pipelines di accedere al repository GitHub e fare il checkout del codice. La Azure Resource Manager service connection permette invece alla pipeline di autenticarsi verso Azure e operare sulle risorse autorizzate.

## 12.
Perché sarebbe un percorso specifico della macchina di una persona. La pipeline deve essere portabile e funzionare su qualsiasi Agent compatibile del pool, utilizzando il workspace gestito da Azure Pipelines e percorsi relativi.

## 13.
Non useremo più il nostro self-hosted Agent pool-ud09-wsl, ma un Microsoft-hosted Agent. Azure DevOps creerà una VM Ubuntu temporanea per il Job e la eliminerà al termine. La pipeline non potrà quindi dipendere da file o configurazioni persistenti presenti sulla macchina.