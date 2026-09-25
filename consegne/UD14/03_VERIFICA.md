# UD14 — Verifica

## 1.
Perché vogliamo verificare il codice prima di produrre e pubblicare l’artefatto. Se i test falliscono, significa che il codice non rispetta i controlli definiti. In quel caso non avrebbe senso costruire e pubblicare una nuova immagine Docker in ACR, perché rischieremmo di distribuire un artefatto già noto come non valido.

## 2.
Una pipeline verde indica che tutti gli step e i controlli configurati nella CI sono stati completati con successo. Non garantisce però l’assenza totale di bug, perché dipende dalla qualità e dalla completezza dei test eseguiti.

## 3.
Perché `Build.BuildId` identifica in modo univoco la singola run della pipeline. È più utile di `latest` perché `latest` è un tag mutabile: oggi può indicare un’immagine e domani un’altra. `Build.BuildId` invece mantiene una tracciabilità precisa tra la run della pipeline e l’immagine prodotta.

## 4.
Docker@2 usa la service connection sc-acr-ud14. È una Docker Registry service connection configurata verso Azure Container Registry. Serve a permettere a Docker@2 di autenticarsi verso ACR durante il buildAndPush, senza inserire username e password direttamente nel file YAML. Nel nostro caso l’autenticazione usa Workload Identity Federation.

## 5.
L’admin user può rimanere disabilitato perché Docker@2 si autentica verso ACR tramite la service connection sc-acr-ud14 con Workload Identity Federation, usando credenziali temporanee invece di username e password statici.

## 6.
No, perché se Test fallisce la pipeline non è ancora arrivata alla fase di push verso ACR. In quel caso bisogna analizzare prima test, codice e ambiente del Job; i permessi ACR diventano rilevanti solo se il problema si verifica durante l’autenticazione o il push dell’immagine.

## 7.
La CI viene attivata automaticamente da un nuovo commit sul branch main, perché il file YAML contiene un trigger configurato su main.

## 8.
Non eseguiamo cleanup alla fine di UD14 perché ACR, immagini e connessioni devono essere riutilizzati nella UD15. La UD14 prepara e pubblica l’artefatto, mentre la UD15 lo usa per il deployment.

## 9.
Microsoft-hosted è il percorso principale perché consente di eseguire la CI su un ambiente temporaneo gestito da Microsoft, senza dipendere dal computer personale o da un Agent self-hosted locale.

## 10.
Perché ogni Job Microsoft-hosted può essere eseguito su una VM temporanea diversa. Quindi un file creato localmente nel primo Job non è garantito che esista nel secondo.

## 11.
Si usa il fallback self-hosted quando il percorso Microsoft-hosted non è disponibile o non può essere usato per motivi esterni, ad esempio grant, billing o policy.

## 12.
workspace: clean: all è importante sul fallback self-hosted perché l’Agent riutilizza la stessa macchina e potrebbero rimanere file delle esecuzioni precedenti. Pulire il workspace aiuta a evitare che residui locali alterino il comportamento della nuova run.

## 13.
L’Agent è la macchina che esegue materialmente i Job e gli step della pipeline. Nel nostro caso può essere Microsoft-hosted (ubuntu-latest) oppure self-hosted (pool-ud09-wsl).
La Docker Registry service connection invece non esegue i comandi: fornisce alla pipeline l’identità/autenticazione necessaria per accedere ad ACR. Nel nostro caso è: sc-acr-ud14.

## 14.
Per verificare subito che sull’Agent siano disponibili gli strumenti necessari alla pipeline e con quali versioni. Questo facilita anche il troubleshooting, permettendo di distinguere un problema dell’ambiente di esecuzione da un problema del codice o dell’accesso ad ACR.

## Gate

- UD14_AGENT_MODE: `MICROSOFT_HOSTED`
- pipeline CI riuscita: si
- ACR/tag: sì, `acrud1315fo62jyh66yvzq`
- repository catalog-backend presente in ACR: si
- almeno un tag Build ID presente:  sì, ultimo tag `10`
- sc-azure-ud13-15 presente: si
- sc-acr-ud14 presente: si
