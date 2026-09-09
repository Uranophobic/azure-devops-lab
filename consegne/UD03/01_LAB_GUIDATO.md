# Consegna UD03 — Laboratorio guidato

## Contesto anonimizzato

- sottoscrizione e tenant verificati: **Sì**
- percorso Entra eseguito: **A**
- resource group temporaneo: **`rg-cea-identity-6dc911`**

| Principal anonimizzato | Ruolo | Scope | Diretta/ereditata | Motivo |
|---|---|---|---|---|
| Utente | `Owner` | `/subscriptions/<omitted>` | Ereditata | Gestione completa delle risorse e degli accessi |
| Gruppo di sicurezza  `grp-cea-readers-6dc911` | `Reader` | `/subscriptions/<omitted>/resourceGroups/rg-cea-identity-6dc911` | Diretta | Consentire la consultazione delle risorse del resource group senza permetterne la modifica. |

Come prima cosa ho verificato di avere le autorizzazioni necessarie per creare un **utente** e un **gruppo** all'interno di Microsoft Entra ID.
Dopo aver verificato questa possibilità, ho creato un utente di test denominato `cea-lab-6dc911`.
Successivamente ho creato il gruppo di sicurezza `grp-cea-readers-6dc911` e ho aggiunto al suo interno l'utente di test.

All'interno del resource group `rg-cea-identity-6dc911`, nella sezione **Access Control (IAM)**, ho quindi creato un'assegnazione **RBAC**, assegnando al gruppo il ruolo `Reader`.
Il ruolo `Reader` permette di visualizzare le risorse e le relative configurazioni, ma non consente di modificarle o eliminarle.
Attraverso la funzione **Check access** ho verificato che il gruppo avesse effettivamente il ruolo `Reader` sul resource group.

Successivamente, tramite **Cost Analysis**, ho controllato lo stato dei costi del resource group. Non risultavano costi disponibili e il portale mostrava il messaggio `No cost reported during this period`, situazione coerente con il fatto che il resource group era stato appena creato e non conteneva risorse a consumo.
Ho poi creato il budget `budget-cea-6dc911`, impostando un importo mensile di **10 euro** e configurando un alert all'**80% del costo effettivo**, associato al mio indirizzo email.

Infine ho creato il resource lock `lock-cea-delete` di tipo `Delete`, corrispondente al livello `CanNotDelete`.
Per verificarne il funzionamento ho tentato di eliminare il resource group tramite Azure CLI. L'operazione è stata correttamente bloccata con l'errore `ScopeLocked`, dimostrando che il lock impedisce l'eliminazione della risorsa finché rimane attivo.

## Governance e costi

- tag e significato: durante la creazione del resource group sono stati aggiunti i tag `course=cloud-engineer-academy`, `unit=UD03`, `environment=lab` e `deleteAfter=<data>`. I tag permettono di classificare e organizzare le risorse, oltre a fornire informazioni utili sul loro utilizzo e ciclo di vita.
- lock e operazione impedita: è stato creato il lock `lock-cea-delete` di livello `CanNotDelete`, che ha impedito l'eliminazione del resource group.
- stato di Cost Analysis:`No cost reported during this period`.
- budget creato o limitazione documentata: è stato creato il budget `budget-cea-6dc911` con importo mensile di **10 euro** e alert impostato all'**80% del costo effettivo**.
- motivo per cui il budget non blocca la spesa: il budget non blocca la spesa in quanto esso rappresenta uno strumento di notifica, non è un limite reale. 

## Cleanup

Sono stati rimossi gli oggetti temporanei creati durante il laboratorio guidato:

- budget `budget-cea-6dc911`;
- assegnazione RBAC `Reader` del gruppo `grp-cea-readers-6dc911`;
- management lock `lock-cea-delete`;
- utente temporaneo `cea-lab-6dc911`;
- gruppo di sicurezza `grp-cea-readers-6dc911`;
- resource group `rg-cea-identity-6dc911`.

Dopo la rimozione del lock è stato verificato che `az lock list` non restituisse più lock applicati.
Dopo l'eliminazione del resource group è stato eseguito il comando `az group show --name "$LAB_RG" --output table`. Il controllo ha restituito `ResourceGroupNotFound`, confermando che `rg-cea-identity-6dc911` non era più presente.

## Rilevanza professionale

L'**autenticazione** serve a verificare l'identità di un utente, l'utente è intera come "persona normale". 
L'**autorizzazione**, attraverso Azure RBAC, stabilisce invece che cosa quell'identità è autorizzata a fare. L'accesso viene determinato dalle assegnazioni di ruolo, dal ruolo assegnato e dallo scope sul quale il ruolo è applicato.
Il **blocco di governance** è un controllo applicato alla risorsa per proteggerla da determinate operazioni. A differenza di RBAC, non stabilisce quali permessi possiede l'utente, ma impone una protezione sulla risorsa stessa. Nel laboratorio, nonostante l'account disponesse dei permessi necessari per eliminare il resource group, il lock CanNotDelete ha impedito l'operazione restituendo l'errore ScopeLocked.
