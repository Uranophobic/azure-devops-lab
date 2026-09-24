# UD13 — Domande concetti

## 1.
**Risposta:**
Lo state di Terraform è il file che collega la configurazione dichiarata nel codice alle risorse gestite, quindi contiene identificativi, attributi e relazioni necessarie a Terraform per capire che cosa esiste già e quali sono le differenze rispetto alla configurazione desiderata.
In un laboratorio individuale, uno state locale è accettabile perché c'è soltanto una persona a lavorarci, le risorse sono temporanee e non esistono esecuzioni concorrenti. In un team, invece, questo può diventare problematico perché, se più persone possiedono copie diverse dello state, non sarà più chiaro quale sia lo stato corrente dell'infrastruttura.
Quindi una modalità adatta al singolo individuo non garantisce collaborazione, concorrenza, protezione e recuperabilità. Per questo, in un team si utilizza normalmente uno state remoto che permette di gestire questi aspetti.

## 2.
**Risposta:**
Un backend remoto sposta lo state da un file locale a una posizione condivisa e gestita; su Azure, ad esempio, può essere utilizzato Azure Storage. L'obiettivo è rendere lo state disponibile ai processi autorizzati e proteggerlo tramite controllo degli accessi, durabilità e, soprattutto, meccanismi di coordinamento tra più esecuzioni.

## 3.
**Risposta:**
Una pipeline Azure DevOps non è un unico blocco di istruzioni eseguito dall'inizio alla fine, ma è organizzata in livelli gerarchici che permettono di dividere il processo in parti con responsabilità diverse, in particolare:

- Stage: è una fase logica del processo, ad esempio Validate, Deploy o Test.
- Job: è un gruppo di Step eseguito da un Agent, ad esempio la validazione dell'IaC.
- Step: è una singola operazione del Job, ad esempio checkout, uno script o una task. 

## 4.
**Risposta:**
checkout: self indica ad Azure Pipelines di recuperare il repository associato alla pipeline e collocarlo nell'area di lavoro dell'Agent. Gli script possono quindi utilizzare percorsi relativi come infra/terraform o infra/bicep.

## 5.
**Risposta:**
Una pipeline self-hosted non deve assumere di trovarsi in un percorso fisso come ~/workspace/... perché è Azure Pipelines a gestire il workspace dell'Agent e la directory in cui viene effettuato il checkout del repository.
Il percorso può cambiare in base all'Agent, alla sua configurazione o all'ambiente in cui la pipeline viene eseguita. Utilizzare un percorso personale e fisso renderebbe quindi la pipeline dipendente da quella specifica macchina.
Per questo si utilizzano il checkout gestito da Azure Pipelines, i percorsi relativi oppure le variabili messe a disposizione dalla pipeline.

## 6.
**Risposta:**
Una Service Connection viene utilizzata in Azure DevOps perché una pipeline non dovrebbe riutilizzare automaticamente il login personale del partecipante per amministrare Azure.
La Service Connection rappresenta quindi l'identità autorizzata con cui la pipeline può autenticarsi verso Azure e definisce anche lo scope entro il quale tale identità può operare.

## 7.
**Risposta:**
Con Workload Identity Federation (WIF), Azure DevOps ottiene un token federato che Microsoft Entra valida e utilizza per autorizzare l'accesso alle risorse Azure. Non è quindi necessario memorizzare nella pipeline una password o un client secret statico.
Questo riduce anche il problema della gestione, protezione e rotazione di credenziali permanenti.

## 8.
**Risposta:**
Limitiamo la Service Connection a un Resource Group durante questo laboratorio per limitare la connessione a quello specifico scope, invece di concedere privilegi sull'intera subscription.
Questo applica il principio del minimo privilegio: ridurre l'area su cui un'identità può intervenire limita l'impatto di errori, configurazioni sbagliate o uso improprio della connessione.

## 9.
**Risposta:**
terraform validate richiede una directory inizializzata, perché Terraform deve conoscere provider e moduli della configurazione. In UD13, però, non vogliamo configurare né utilizzare uno state remoto, quindi l'inizializzazione viene eseguita con terraform init -backend=false
In questo modo Terraform prepara ciò che serve per eseguire la validazione, senza inizializzare il backend dello state.
La pipeline UD13 esegue quindi la validazione del codice Terraform, ma non terraform apply, perché in questa unità l'obiettivo è verificare il funzionamento della pipeline, dell'Agent e dell'autenticazione verso Azure senza modificare realmente l'infrastruttura gestita da Terraform.

## 10.
**Risposta:**
L'ACR creato in UD13 non deve essere eliminato perché viene conservato intenzionalmente per essere riutilizzato nelle unità successive, in particolare UD14 e UD15.
A differenza delle risorse temporanee create esclusivamente per un singolo laboratorio, l'Azure Container Registry fa quindi parte dell'ambiente che verrà utilizzato nelle fasi successive del corso.

## 11.
**Risposta:**
trigger: none è una configurazione che disabilita i trigger automatici della pipeline e lascia l'avvio manuale.
In questo modo, ad esempio, un nuovo commit o un push sul repository non provoca automaticamente l'esecuzione della pipeline.

## 12.
**Risposta:**
Un ordine razionale per diagnosticare una pipeline che non parte correttamente è controllare progressivamente:

- se la pipeline è stata effettivamente avviata;
- se il trigger configurato permette l'avvio previsto;
- se il file YAML è valido e viene letto correttamente;
- se lo Stage previsto viene avviato;
- se il Job viene schedulato;
- se è disponibile un Agent compatibile;
- se il checkout del repository riesce;
infine, quale Step o comando genera l'eventuale errore.

L'idea è partire dal livello più generale e scendere progressivamente fino alla singola operazione che causa il problema.

## 13.
**Risposta:**
L'ACR UD13–UD15 imposta esplicitamente LegacyRegistryPermissions perché rende esplicito il modello di autorizzazione richiesto dal laboratorio, evitando che il comportamento dipenda da eventuali impostazioni predefinite differenti.

In questo modo il Registry utilizza il modello di permessi previsto per le attività delle UD13–UD15 e la configurazione rimane riproducibile

## 14.
**Risposta:**
UD13 utilizza deliberatamente il self-hosted Agent come prima pipeline perché permette di verificare concretamente l'ambiente configurato nelle unità precedenti.

## 15.
**Risposta:**
workspace: clean: all esegue una pulizia completa del workspace del Job su un Agent self-hosted prima dell'esecuzione.
È particolarmente utile con gli Agent self-hosted perché la macchina viene riutilizzata tra più pipeline e potrebbero essere rimasti file o artefatti prodotti da esecuzioni precedenti.

## 16.
**Risposta:**

## 17.
**Risposta:**
La pipeline non deve utilizzare percorsi come ~/workspace/azure-devops-lab perché deve essere portabile su un altro Agent e non deve dipendere dalla struttura della home directory di una persona o dalla configurazione specifica di una macchina.

## 18.
**Risposta:**
Passando da pool-ud09-wsl a vmImage: ubuntu-latest, la pipeline non verrebbe più eseguita sul nostro Agent self-hosted, ma su un Agent Microsoft-hosted Ubuntu.

L'ambiente sarebbe quindi temporaneo e verrebbe ricreato a ogni esecuzione della pipeline. Non potremmo fare affidamento su file, configurazioni o strumenti installati manualmente in precedenza sul nostro Agent self-hosted.

La pipeline dovrebbe quindi essere sufficientemente autonoma da preparare durante l'esecuzione tutto ciò che le serve, oppure utilizzare gli strumenti già disponibili nell'immagine ubuntu-latest.

## 19.
**Risposta:**


## 20.
**Risposta:**

## 21.
**Risposta:**