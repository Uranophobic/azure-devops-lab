# UD10 — Risposte alle domande sui concetti

## 1.
**Risposta:**
Il codice sorgente è il codice dell'applicazione. Una image è un artefatto immutabile che contiene codice, runtime e configurazione necessaria. Un container è un'istanza runtime creata a partire da un'image.

## 2.
**Risposta:**
`docker build `costruisce un'image seguendo le istruzioni del Dockerfile. Non avvia l'applicazione e non crea un container in esecuzione.

## 3.
**Risposta:**
Il Dockerfile descrive come costruire l'image; il build context contiene i file che Docker può utilizzare durante la build; .dockerignore esclude dal context file inutili o sensibili.

## 4.
**Risposta:**
Docker la recupera da un registry configurato, normalmente Docker Hub nel nostro laboratorio.

## 5.
**Risposta:**
`RUN` viene eseguito durante la build e modifica l'image. `CMD` definisce invece il comando che verrà eseguito quando parte il container.

## 6.
**Risposta:**
Perché durante la build una copia di `server.py` viene inserita nell'image. Se il file sorgente cambia, bisogna eseguire una nuova build per incorporare la modifica.

## 7.
**Risposta:**
Serve a identificare l'image: catalog-backend è il nome e ud10 è il tag, cioè l'etichetta associata a quella versione dell'image.

## 8.
**Risposta:**
Docker prende l'image `catalog-backend:ud10`, crea un container, applica la configurazione runtime e avvia il comando definito dal CMD dell'image.

## 9.
**Risposta:**
`EXPOSE 8000` documenta che il container utilizza la porta 8000, ma non la rende raggiungibile dall'host. `--publish `crea invece realmente il collegamento tra la porta 8000 dell'host e quella del container.

## 10.
**Risposta:**
Docker Compose permette di descrivere e gestire insieme più container, reti, volumi, porte, variabili e dipendenze tramite un unico file compose.yaml, evitando molti comandi manuali.

## 11.
**Risposta:**
Il Dockerfile descrive come costruire una singola image. compose.yaml descrive invece come più servizi/container devono essere configurati ed eseguiti insieme.

## 12.
**Risposta:**
Gestisce backend, frontend, rete catalog-net, volume catalog-runtime, variabili d'ambiente, porte, healthcheck e dipendenze tra i servizi.

## 13.
**Risposta:**
Perché nel compose.yaml entrambi i servizi hanno una sezione build che indica il proprio Dockerfile. Compose può quindi costruire entrambe le image prima di avviare i container.

## 14.
**Risposta:**
Perché il backend deve essere raggiunto solo dal frontend attraverso la rete Docker. L'host accede al frontend sulla porta 8080, mentre Nginx inoltra internamente le richieste al backend sulla porta 8000

## 15.
**Risposta:**
Perché `backend` è il nome del servizio risolto dal DNS interno di Docker. `localhost`, invece, dentro il frontend indica il container frontend stesso.

## 16.
**Risposta:**
È la rete Docker privata che collega frontend e backend e permette ai due container di comunicare usando i nomi dei servizi.

## 17.
**Risposta:**
È un named volume usato dal backend per conservare i dati di /runtime anche quando il container viene eliminato e ricreato.

## 18.
**Risposta:**
running significa che il processo del container è in esecuzione. healthy significa invece che anche l'healthcheck applicativo è stato eseguito con successo.

## 19.
**Risposta:**
Fa sì che il frontend venga avviato in base alla dipendenza dal backend solo quando il backend ha superato correttamente il proprio healthcheck ed è diventato healthy.

## 20.
**Risposta:**
Il rebuild ricostruisce l'image, ad esempio dopo una modifica al codice o al Dockerfile. Il recreate ricrea il container, ad esempio dopo una modifica alla configurazione runtime, riutilizzando la stessa image.

## 21.
**Risposta:**
Controllerei prima docker compose ps, poi i logs, la configurazione con docker compose config, quindi health, environment, porte, rete e volumi. Individuata la causa, farei una modifica minima e ripeterei il test.

## 22.
**Risposta:**
Le operazioni che oggi eseguiamo manualmente, come test, docker build e gestione delle image, verranno successivamente eseguite automaticamente dagli Agent delle pipeline CI/CD, fino alla pubblicazione e al deployment delle image in Azure.