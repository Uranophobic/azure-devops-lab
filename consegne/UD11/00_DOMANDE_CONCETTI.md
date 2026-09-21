# UD11 — Risposte alle domande sui concetti

## 1.
**Risposta:**
Un registry è un servizio nel quale possiamo pubblicare le immagini, che vengono quindi ospitate e distribuite, come Azure Container Registry (ACR). Il repository raggruppa logicamente le diverse versioni della stessa immagine. Il tag è un'etichetta leggibile associata a un'immagine, mentre il digest identifica in modo univoco uno specifico contenuto dell'immagine.

## 2.
**Risposta:**
Perché latest è semplicemente un tag: non significa che Docker o Azure abbiano verificato quale sia la versione cronologicamente più recente o approvata. È quindi preferibile utilizzare tag espliciti, come v1 o v2, per mantenere una maggiore tracciabilità della versione da distribuire.

## 3.
**Risposta:**
Non conviene costruirlo manualmente perché l'immagine viene referenziata tramite il login server. È quindi preferibile leggerlo direttamente dalla risorsa, in modo da ridurre errori e dipendenze dalle convenzioni di naming.

## 4.
**Risposta:**
Il Container Apps Environment è il contesto infrastrutturale condiviso nel quale possono essere ospitate una o più Container App. La Container App descrive invece l'applicazione che vogliamo eseguire e la sua configurazione, come:

- image;
- variabili d'ambiente;
- CPU e memoria;
- ingress;
- scaling;
- identità;
- modalità di gestione delle revision.

## 5.
**Risposta:**
Una revision rappresenta uno snapshot immutabile della configurazione revision-scope della Container App. Può essere generata, ad esempio, dalla modifica dell'immagine, delle variabili d'ambiente, di CPU e memoria o delle regole di scaling. La replica, invece, è una concreta istanza runtime di una revision.

## 6.
**Risposta:**
L'ingress viene utilizzato per rendere raggiungibile il backend. Riceve la richiesta e la inoltra al container sulla target port configurata.

## 7.
**Risposta:**
La target port non è una porta scelta arbitrariamente: deve coincidere con la porta sulla quale il processo all'interno del container è realmente in ascolto. In caso contrario, l'ingress non riuscirebbe a inoltrare correttamente le richieste all'applicazione.

## 8.
**Risposta:**
Perché permette alla Container App di autenticarsi verso ACR senza utilizzare username, password o altre credenziali statiche. I permessi vengono assegnati direttamente all'identità tramite Azure RBAC.

## 9.
**Risposta:**
AcrPull è il ruolo che permette a un'identità autorizzata di scaricare le immagini da Azure Container Registry, senza concedere permessi di pubblicazione o modifica.

## 10.
**Risposta:**
Quando viene aggiornata l'immagine della Container App, viene generalmente creata una nuova revision contenente la nuova configurazione. In single revision mode, la nuova revision diventa quella attiva.

## 11.
**Risposta:**
Per comprendere prima quali operazioni vengono eseguite durante build, push e deployment. In questo modo, quando queste attività verranno automatizzate tramite pipeline, sarà più semplice capire cosa sta facendo la pipeline e individuare eventuali errori.

## 12.
**Risposta:**
Con RBAC Registry Permissions vengono utilizzati ruoli come AcrPull e AcrPush per autorizzare l'accesso al registry. Con RBAC+ABAC è possibile applicare autorizzazioni più specifiche, anche a livello di repository, utilizzando ruoli e condizioni più granulari.

## 13.
**Risposta:**
Perché in UD11 le operazioni vengono ancora eseguite manualmente dal nostro ambiente locale tramite Azure CLI e Docker. Il self-hosted Agent diventerà utile quando queste stesse operazioni verranno eseguite automaticamente da una pipeline.

## 14.
**Risposta:**
La system-assigned managed identity viene creata direttamente sulla risorsa e il suo ciclo di vita è legato a essa: se la risorsa viene eliminata, viene eliminata anche l'identità. La user-assigned managed identity è invece una risorsa indipendente e può essere assegnata e riutilizzata da più risorse Azure.

## 15.
**Risposta:**
Significa che la Container App può ridurre il numero di repliche fino a zero quando non deve elaborare richieste o eventi, riducendo così il compute inutilizzato.

## 16.
**Risposta:**
Controllerei:

- che il FQDN sia corretto;
- che l'ingress sia abilitato;
- che la target port coincida con la porta dell'applicazione;
- che la nuova revision sia attiva e correttamente avviata;
- che esistano repliche funzionanti;
- i log della Container App;
- che l'immagine e il tag presenti in ACR siano corretti;
- che la managed identity abbia i permessi necessari per eseguire il pull da ACR;
- che le variabili d'ambiente siano configurate correttamente.


## APPROFONDIMENTO: MANAGED IDENTITY, IL SUO MECCANISMO E PERCHÉ È PIÙ CONVENIENTE RISPETTO AD ALTRI SISTEMI 

Una Managed Identity è un’identità gestita da Azure e associata a una risorsa. Consente a quella risorsa di autenticarsi verso altri servizi Azure senza dover salvare password, secret o altre credenziali di accesso.

Quando la risorsa deve accedere a un altro servizio Azure, non utilizza una password. Tramite il meccanismo della Managed Identity, Azure riconosce l’identità associata alla risorsa e Microsoft Entra ID rilascia un token temporaneo che rappresenta quella identità. Il token viene poi utilizzato per autenticarsi verso il servizio di destinazione.

Il servizio di destinazione è la risorsa Azure a cui si vuole accedere. Nel nostro laboratorio, il servizio di destinazione è Azure Container Registry (ACR), perché la Container App deve effettuare il pull dell’immagine privata.

Una volta ricevuto il token, ACR riconosce l’identità rappresentata dal token e verifica tramite Azure RBAC quali operazioni è autorizzata a eseguire. Nel nostro caso, alla Managed Identity della Container App è stato assegnato il ruolo `AcrPull`, che permette di scaricare l’immagine dal registry.

Esistono due tipi principali di Managed Identity:

- `system-assigned`: viene creata direttamente sulla risorsa e il suo ciclo di vita è legato a essa. Se la risorsa viene eliminata, viene eliminata anche l’identità;
- `user-assigned`: è una risorsa Azure indipendente, che può essere associata a più risorse e riutilizzata.

In conclusione, una Managed Identity può essere vista come la "carta d’identità" di una risorsa Azure. Questa identità viene gestita tramite Microsoft Entra ID, che è il servizio che gestisce le identità digitali.

Quando la risorsa deve accedere a un altro servizio, utilizza la propria Managed Identity per ottenere da Microsoft Entra ID un token temporaneo. Questo token attesta l’identità della risorsa verso il servizio di destinazione. Il servizio riconosce quindi chi sta effettuando la richiesta e verifica, tramite Azure RBAC, quali operazioni quell’identità è autorizzata a eseguire.