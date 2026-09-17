# UD09 — Risposte alle domande sui concetti

## 1.

**Risposta:** DevOps non può essere ridotto a un prodotto perché è un insieme di principi, pratiche organizzative e tecniche. I tool supportano queste pratiche, ma non le sostituiscono.

## 2.

**Risposta:** Le tre dimensioni principali di DevOps sono cultura, processi e tecnologia: la cultura riguarda la collaborazione tra le persone, i processi definiscono come viene organizzato il lavoro e la tecnologia fornisce gli strumenti e le automazioni che supportano queste attività.

## 3.

**Risposta:** Il lifecycle DevOps è un ciclo perché non termina con il deployment: monitoraggio e feedback producono nuove informazioni che tornano alla pianificazione e generano nuovo lavoro.

## 4.

**Risposta:** Agile riguarda soprattutto il modo in cui il team pianifica e sviluppa valore in cicli brevi e adattivi. DevOps estende questa continuità anche a integrazione, test, delivery, deployment, operations e monitoring.

## 5.

**Risposta:** Il backlog è una lista ordinata e prioritizzata del lavoro da realizzare, composta ad esempio da funzionalità, bug, miglioramenti, attività tecniche e debito tecnico.

## 6.

**Risposta:** Uno sprint è un intervallo di tempo breve e definito nel quale il team realizza un insieme selezionato di attività e produce un risultato verificabile.

## 7.

**Risposta:** Scrum organizza il lavoro in sprint e definisce ruoli, eventi e artefatti. Kanban visualizza invece il flusso di lavoro e aiuta a controllare il lavoro in corso e individuare eventuali colli di bottiglia.

## 8.

**Risposta:** Un Epic rappresenta un'iniziativa ampia, una Feature una capacità significativa, una User Story una necessità dell'utente, una Task un'attività concreta e un Bug un difetto da correggere.

## 9.

**Risposta:** Gli Acceptance Criteria definiscono le condizioni verificabili che una funzionalità deve soddisfare per essere considerata corretta.

## 10.

**Risposta:** Il Version Control è importante anche per IaC e pipeline YAML perché permette di versionare infrastruttura e automazioni, tracciando modifiche, autori e versioni e consentendo di tornare a configurazioni precedenti.

## 11.

**Risposta:** La build è il processo che trasforma il contenuto sorgente in qualcosa di eseguibile o distribuibile. Un artifact è invece l'output prodotto dalla build e utilizzato nelle fasi successive.

## 12.

**Risposta:** Un unit test verifica una piccola unità di codice in isolamento; un integration test verifica l'interazione tra più componenti; uno smoke test controlla rapidamente dopo il deployment che il sistema e le funzioni essenziali siano disponibili.

## 13.

**Risposta:** Shift-left significa spostare i controlli il più possibile verso le fasi iniziali del ciclo, così da individuare i problemi prima e ridurne costo, rischio e difficoltà di correzione.

## 14.

**Risposta:** La Continuous Integration (CI) consiste nell'integrare frequentemente le modifiche nella base di codice condivisa ed eseguire automaticamente controlli come build e test.

## 15.

**Risposta:** Uno Stage rappresenta una fase logica della pipeline, un Job è un insieme di step eseguiti su un Agent e uno Step è una singola operazione.

## 16.

**Risposta:** La Continuous Delivery mantiene il software pronto al rilascio, ma può prevedere un'approvazione manuale prima della produzione. La Continuous Deployment porta automaticamente in produzione ogni modifica che supera i controlli previsti.

## 17.

**Risposta:** Il Dockerfile contiene le istruzioni per costruire un'immagine; l'image è l'artefatto immutabile costruito; il container è un'istanza in esecuzione dell'immagine.

## 18.

**Risposta:** Un registry è un servizio utilizzato per conservare, versionare e distribuire container image.

## 19.

**Risposta:** Un orchestrator automatizza la gestione di più container, occupandosi ad esempio di scheduling, repliche, disponibilità e self-healing. Un esempio è Kubernetes.

## 20.

**Risposta:** Infrastructure as Code (IaC) significa descrivere e gestire l'infrastruttura tramite file versionabili, invece di configurarla esclusivamente in modo manuale.

## 21.

**Risposta:** DevSecOps integra la sicurezza direttamente nel lifecycle DevOps, applicando controlli, automazioni e principio del least privilege fin dalle prime fasi.

## 22.

**Risposta:** Una DevOps toolchain è l'insieme coordinato degli strumenti utilizzati nelle diverse fasi del lifecycle, ad esempio GitHub, Azure Pipelines, Docker, Terraform e Azure Monitor.

## 23.

**Risposta:** I cinque servizi principali di Azure DevOps sono Azure Boards, Azure Repos, Azure Pipelines, Azure Test Plans e Azure Artifacts.

## 24.

**Risposta:** Azure Boards serve alla pianificazione e al tracking del lavoro tramite Work Item, backlog, board e sprint.

## 25.

**Risposta:** Nel corso utilizziamo GitHub invece di Azure Repos perché GitHub rimane il repository sorgente già utilizzato nelle UD precedenti, mentre Azure DevOps viene usato principalmente per Pipelines, Agent e Service Connection.

## 26.

**Risposta:** Azure Test Plans è orientato soprattutto alla gestione di test manuali ed esplorativi. I test automatici vengono invece eseguiti direttamente dalle pipeline tramite appositi framework e comandi.

## 27.

**Risposta:** Azure Artifacts è utilizzato per gestire package come Maven, npm, NuGet e Python. Azure Container Registry (ACR) è invece un registry destinato principalmente alle container image.

## 28.

**Risposta:** Una Azure DevOps Organization è il contenitore amministrativo superiore e può contenere più Project. Un Project è uno spazio di lavoro interno all'Organization che contiene servizi e configurazioni del progetto.

## 29.

**Risposta:** L'Agent esegue materialmente un Job. Un Agent Pool è un insieme di Agent. Un Parallel Job indica invece quanti Job possono essere eseguiti contemporaneamente.

## 30.

**Risposta:** Un Microsoft-hosted Agent è creato e gestito da Microsoft e utilizza un ambiente temporaneo e pulito per ogni Job. Un self-hosted Agent gira invece su una macchina gestita dall'organizzazione.

## 31.

**Risposta:** Una Service Connection è un collegamento autenticato tra Azure DevOps e un sistema esterno, ad esempio GitHub, Azure o Azure Container Registry.

## 32.

**Risposta:** Il PAT serve per registrare il self-hosted Agent. Dopo aver verificato che l'Agent sia Online può essere revocato, perché non viene utilizzato per autenticare ogni Job successivo.

## 33.

**Risposta:** DevOps è un insieme di principi e pratiche indipendenti da uno specifico prodotto. Azure DevOps è invece una piattaforma Microsoft che fornisce strumenti per implementare alcune di queste pratiche.

## 34.

**Risposta:** Azure Pipelines orchestra e gestisce l'esecuzione del lavoro definito nella pipeline. L'Azure Pipelines Agent è invece l'esecutore che esegue materialmente i Job e i relativi comandi.

## 35.

**Risposta:** Azure Pipelines Agent, Jenkins Agent, GitHub Runner e GitLab Runner svolgono lo stesso ruolo concettuale: sono gli esecutori dei Job definiti dalle rispettive piattaforme CI/CD.

## 36.

**Risposta:** Nelle UD13–UD15 l'Agent eseguirà progressivamente validazioni IaC, test automatici, Docker build, push delle image, deployment, verifiche e smoke test.

## 37.

**Risposta:** Un Job Microsoft-hosted non deve dipendere dai file del Job precedente perché ogni Job viene eseguito in un nuovo ambiente temporaneo e pulito, che viene ricreato.

## 38.

**Risposta:** Nel corso utilizziamo WSL2 sul PC personale per ospitare un vero self-hosted Agent e comprenderne il funzionamento. In un ambiente aziendale, invece, l'Agent viene normalmente installato su VM o server dedicati e gestiti dall'organizzazione, così da essere disponibile indipendentemente dal computer di uno sviluppatore.

## 39.

**Risposta:** Un Agent Pool aziendale può contenere più Agent, ad esempio VM o server dedicati. Azure Pipelines assegna i Job agli Agent disponibili e compatibili presenti nel Pool.

## 40.

**Risposta:** Avere più Agent non significa automaticamente poter eseguire più Job contemporaneamente, perché oltre agli Agent disponibili serve anche una sufficiente capacità di Parallel Jobs.
