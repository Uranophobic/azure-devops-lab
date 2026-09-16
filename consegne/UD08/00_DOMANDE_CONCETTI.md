# UD08 — Risposte domande concetti

## 1.
**Risposta: DevOps è un modo di lavorare basato sulla collaborazione tra persone, processi e strumenti, con l'obiettivo di collegare sviluppo, verifica, rilascio, gestione e feedback in un processo continuo e ripetibile. Azure DevOps, invece, è una piattaforma Microsoft che mette a disposizione strumenti e servizi, come repository Git, pipeline, gestione delle attività e artifact, che possono essere utilizzati per applicare pratiche DevOps.**

## 2.
**Risposta: Continuous Integration significa integrare frequentemente modifiche relativamente piccole nella base comune del progetto e verificarle rapidamente. In questo modo si evita di accumulare per molto tempo modifiche separate, riducendo la difficoltà dell'integrazione e dei conflitti. Continuous Delivery significa mantenere il software in uno stato che ne consenta il rilascio. Build, test e controlli vengono automatizzati quanto possibile, mentre il rilascio in produzione può richiedere una decisione o approvazione esplicita. Continuous Deployment, invece consiste in una modifica che supera tutti i controlli previsti può essere distribuita automaticamente in produzione, senza il normale passaggio di approvazione manuale.**

## 3.
**Risposta: La working tree contiene i file sui quali stiamo attualmente lavorando. La staging area contiene le modifiche che abbiamo scelto, tramite git add, per il prossimo commit. Il commit registra invece uno snapshot delle modifiche presenti nella staging area nella cronologia del repository locale.**

## 4.
**Risposta: Conviene creare un feature branch partendo da una main aggiornata perché in questo modo il nuovo branch nasce dalla versione più recente del progetto presente sul repository remoto. Questo riduce il rischio successivamente di avere molte differenze o conflitti.**

## 5.
**Risposta: Head branch è il branch che contiene le nuove modifiche che vogliamo proporre. Base branch è invece il branch di destinazione nella quale vogliamo integrare quelle modifiche.**

## 6.
**Risposta: Una review non dovrebbe limitarsi a verificare che il codice funzioni perché deve valutare la qualità complessiva della modifica, come ad esempio se la modifica risponde realmente al requisito, se il diff contiene soltanto ciò che serve o se la modifica può introdurre regressioni.**

## 7.
**Risposta: Se il contributor crea un nuovo commit e fa push sulla stessa branch utilizzata come head branch della Pull Request, la PR si aggiorna automaticamente. Il nuovo commit entra nella stessa PR e viene aggiornato anche il diff.**

## 8.
**Risposta: Il conflitto emerge normalmente durante un'operazione di integrazione, come merge o rebase. Un caso tipico è quando due branch modificano diversamente la stessa riga o la stessa parte di un file. Git segnala il problema e sarà lo sviluppatore a decidere quale deve essere il contenuto finale corretto risolvendo il conflitto.**

## 9.
**Risposta: L'accesso del collaboratore deve essere rimosso quando la collaborazione termina per applicare il principio del least privilege. Mantenere un'autorizzazione che non serve più aumenta inutilmente la superficie di rischio.**

## 10.
**Risposta: Il frontend è static/index.html ed è la parte visualizzata dal browser. Il backend/API è server.py che avvia il server, legge configurazione e dati, gestisce gli endpoint e restituisce le risposte. La configurazione è contenuta in config.json e mantiene separati dal codice i valori utilizzati per configurare il comportamento dell'applicazione. I dati sono contenuti in data/products.json e rappresentano i prodotti gestiti dall'applicazione.**

## 11.
**Risposta:**

- **GET /health, indica se il servizio HTTP risponde**
- **GET /api/products, restituisce la collezione dei prodotti**
- **GET /api/products/<id>, restituisce il prodotto dato dall'identificativo id**
- **GET /, restituisce invece il frontend dell'applicazione**

## 12.
**Risposta: È utile eseguire git diff prima del commit perché permette di controllare le modifiche effettuate prima di registrarle nella cronologia, verificando che non siano presenti modifiche accidentali, codice di debug o informazioni che non dovrebbero essere committate.**

