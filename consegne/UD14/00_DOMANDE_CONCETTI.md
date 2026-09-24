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

## 9.
**Risposta:**

## 10.
**Risposta:**

## 11.
**Risposta:**

## 12.
**Risposta:**

## 13.
**Risposta:**

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

## 17.
**Risposta:**

## 18.
**Risposta:**

