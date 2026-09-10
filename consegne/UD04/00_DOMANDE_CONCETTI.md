# Consegna UD04 — Domande sui concetti

Per ciascuna domanda 1–8 riporta una risposta motivata.

1. Blob Storage e Azure Files sono servizi diversi perché utilizzano modelli di archiviazione differenti. I **Blob** sono oggetti organizzati all'interno di container, mentre **Azure Files** mette a disposizione vere e proprie file share, organizzate in cartelle e file e accessibili tramite protocolli come SMB e NFS.
Di conseguenza, i due servizi sono destinati a **utilizzi differenti**.
2. Il **management plane** è il piano di gestione delle risorse Azure ed è gestito tramite Azure Resource Manager (ARM). Riguarda operazioni come la creazione di uno Storage Account, la modifica della configurazione di rete oppure la lettura delle proprietà della risorsa. 
Il **data plane**, invece, riguarda i dati contenuti all'interno del servizio. Ad esempio, tramite il data plane è possibile caricare, leggere oppure eliminare un Blob.
3. Un ruolo che permette di gestire lo Storage Account non consente necessariamente di leggere o modificare i file contenuti al suo interno. La gestione della risorsa appartiene infatti al **management plane**, mentre l'accesso ai dati appartiene al **data plane** e può richiedere autorizzazioni specifiche.
4. La **ridondanza** non equivale a un **backup**, perché avere più copie dello stesso dato non significa conservarne necessariamente una copia indipendente e recuperabile. Esistono inoltre diversi tipi di ridondanza, alcuni dei quali distribuiscono i dati all'interno della stessa region, mentre altri utilizzano una region primaria e una region secondaria.
5. Prima di scegliere il tier **Archive** bisogna valutare se e con quale frequenza i dati dovranno essere utilizzati. I dati archiviati si trovano infatti in uno stato offline e non possono essere utilizzati immediatamente. Prima di poterli leggere devono essere sottoposti a un processo di rehydration, cioè riportati nuovamente in uno stato online.
6. Una **account key** consente un accesso molto ampio allo Storage Account e deve quindi essere considerata un segreto ad alto impatto. Se qualcuno ne entra in possesso, può effettuare operazioni sui dati in base alle capacità associate all'account. Una **SAS (Shared Access Signature)**, invece, permette di delegare l'accesso tramite un token, definendo in maniera più precisa quali operazioni sono consentite, su quali risorse e per quanto tempo. Inoltre utilizza permessi minimi, uno scope ristretto, una scadenza breve e una connessione tramite HTTPS. 
7. Una SAS dovrebbe prevedere:
    * **permessi minimi**, limitati alle sole operazioni necessarie;
    * uno **scope ristretto**, limitato alle risorse necessarie;
    * una **scadenza breve**;
    * l'utilizzo di **HTTPS** per il trasporto. 
8. Le operazioni definite tramite una **Lifecycle Management Policy** non vengono eseguite immediatamente. L'elaborazione delle regole è gestita direttamente dal servizio Azure e può richiedere anche diverse ore prima che le modifiche previste dalla policy vengano applicate.

