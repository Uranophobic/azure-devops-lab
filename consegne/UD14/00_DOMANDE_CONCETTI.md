# UD14 — Domande concetti

## 1.
**Risposta:**
Trasformare ogni modifica integrata nel repository in una sequenza di controlli coerenti e ripetibili, in modo da ridurre le esecuzioni manuali, intercettare gli errori più velocemente e ottenere un artefatto tracciabile che possa essere utilizzato nelle fasi successive.

## 2.
**Risposta:**
Perché costruire e pubblicare un'immagine non verificata renderebbe il processo CI/CD meno affidabile. Se i test falliscono, la pipeline deve fermarsi prima di costruire e pubblicare l'immagine.

## 3.
**Risposta:**
Significa che tutti i controlli automatizzati previsti dalla pipeline sono stati superati con successo. Non significa però che il software sia privo di bug, completamente sicuro o automaticamente pronto per la produzione: dipende dalla qualità e dalla completezza dei test e dei controlli configurati.

## 4.
**Risposta:**
Perché latest è un tag mutabile e nel tempo può indicare immagini diverse, mentre Build.BuildId identifica in modo univoco l'immagine e permette di risalire direttamente alla run della pipeline che l'ha generata.

## 5.
**Risposta:**
sc-azure-ud13-15 è una Azure Resource Manager service connection, utilizzata per operare sulle risorse Azure, mentre sc-acr-ud14 è una Docker Registry service connection, utilizzata dal task Docker@2 per autenticarsi verso Azure Container Registry durante la build e il push dell'immagine.

## 6.
**Risposta:**
Perché l'autenticazione verso ACR viene effettuata tramite Workload Identity Federation: Azure DevOps e Microsoft Entra stabiliscono una relazione di trust e viene utilizzata un'identità temporanea, senza dover salvare username e password del registry nel file YAML.

## 7.
**Risposta:**
Stabilisce quando avviare automaticamente la pipeline. Un trigger configurato su main fa partire una nuova run quando arriva un nuovo commit su main, secondo le regole configurate. Il trigger non modifica il branch e non impedisce eventuali merge errati.

## 8.
**Risposta:**
Perché ogni Job Microsoft-hosted può essere eseguito su una VM temporanea diversa e quindi non può fare affidamento sui file presenti nel filesystem di un Job precedente. Per questo ciascun Job esegue checkout: self e ricostruisce il proprio contesto di lavoro a partire dal repository.

## 9.
**Risposta:**
Significa che il Job viene eseguito su una macchina virtuale predisposta da Microsoft per quella esecuzione. L'ambiente non è persistente e non bisogna fare affidamento su file o configurazioni lasciati da Job o run precedenti.

## 10.
**Risposta:**
L'Agent è la macchina che esegue materialmente i task e i comandi della pipeline. La service connection, invece, fornisce l'identità e l'autenticazione necessarie per permettere a quei task di accedere a un servizio esterno, come Azure Container Registry.

## 11.
**Risposta:**
Per verificare che sull'Agent siano disponibili gli strumenti necessari alla pipeline e conoscere le versioni utilizzate. Questo facilita il troubleshooting, perché permette di distinguere un problema dell'ambiente di esecuzione da un problema del codice o dell'autenticazione verso ACR.

## 12.
**Risposta:**
Lo utilizziamo quando il percorso Microsoft-hosted non è disponibile o non può essere utilizzato per motivi come grant, billing o policy esterne. In quel caso la pipeline viene eseguita sul pool self-hosted già configurato

## 13.
**Risposta:**
Serve a pulire il workspace prima dell'esecuzione del Job, riducendo il rischio che file rimasti da run precedenti influenzino quella nuova. È particolarmente importante sul self-hosted perché la stessa macchina può essere riutilizzata per più esecuzioni.

## 14.
**Risposta:**
Esegue in un'unica sequenza la build dell'immagine, l'applicazione del tag e il push verso Azure Container Registry. L'autenticazione non viene scritta manualmente nel file YAML tramite un docker login con username e password, perché il task utilizza la Docker Registry service connection sc-acr-ud14.

## 15.
**Risposta:**
Quando una pipeline fallisce, conviene analizzare i log seguendo progressivamente questo ordine:

Stage → Job → Step → comando → messaggio di errore

In questo modo è più semplice restringere il campo e individuare il punto esatto in cui si è verificato il problema.


## 16.
**Risposta:**
Perché se i test falliscono la pipeline non è ancora arrivata alla fase di build e push verso ACR. In quel caso bisogna controllare prima il codice, i test e l'ambiente del Job. I permessi ACR diventano rilevanti quando il problema si verifica durante l'autenticazione o il push dell'immagine.

## 17.
**Risposta:**
Dimostra che la CI esegue i controlli che abbiamo definito e può intercettare un'incoerenza tra applicazione e test. Dimostra anche che una pipeline non può sapere in senso assoluto se il software è corretto: la sua efficacia dipende dalla qualità e dalla completezza dei test configurati.

## 18.
**Risposta:**
Perché l'ACR e le immagini prodotte dalla CI devono essere utilizzati nella UD15. La UD14 costruisce e pubblica l'artefatto, mentre la UD15 utilizzerà quell'immagine come input del deployment.

## 19.
**Risposta:**
La UD14 verifica il codice, costruisce la container image, la identifica con un tag basato su Build.BuildId e la pubblica in Azure Container Registry. La UD15 partirà da quell'immagine per eseguire il deployment.