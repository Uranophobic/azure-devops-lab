# Consegna UD03 — Laboratorio autonomo

## Analisi dell'accesso

| Principal anonimizzato | Ruolo | Scope | Origine | Accesso effettivo |
| --- | --- | --- | --- | --- |
| Utente | `Owner` | `/subscriptions/<omitted>` | Ereditata | Gestione completa delle risorse e degli accessi |
| Gruppo di Sicurezza `grp-cea-readers-6dc911` | `Reader` | `/subscriptions/<omitted>/resourceGroups/rg-cea-identity-6dc911` | Diretta | Consentire la consultazione delle risorse del resource group senza permetterne la modifica. |
| Gruppo di TeamFinOps `grp-TeamFinOps-6dc911` | `Reader` | `/subscriptions/<omitted>/resourceGroups/rg-cea-identity-6dc911` | Diretta | Consentire la consultazione delle risorse del resource group senza permetterne la modifica. |

Il team **FinOps** deve poter consultare costi e risorse di un solo ambiente applicativo.  
È stato creato il gruppo  `grp-TeamFinOps-6dc911` e gli è stato assegnato il ruolo `Reader` con scope limitato al resource group `rg-cea-identity-6dc911`.
L'assegnazione è stata successivamente verificata tramite Azure CLI con:
`az role assignment list --scope "$RG_SCOPE" --include-inherited --output table`

Il controllo ha mostrato:
- un'assegnazione `Owner` ereditata dalla sottoscrizione;
- l'assegnazione `Reader` diretta del gruppo `grp-cea-readers-6dc911`;
- l'assegnazione `Reader` diretta del gruppo `grp-TeamFinOps-6dc911`.

L'assegnazione `Reader` non riduce eventuali privilegi più ampi già posseduti tramite altre assegnazioni. Nel mio caso, il ruolo `Owner` assegnato a livello di sottoscrizione continua quindi ad applicarsi anche al resource group. Mentre al gurppo **FinOps** è stato assegnato il ruolo `Reader` per le seguenti motivazioni:
- `Reader` permette di visualizzare le risorse, ma non di modificarle;
- limitare lo scope al solo resource group consente di circoscrivere l'accesso all'ambiente applicativo necessario;
- `Contributor` sarebbe eccessivo, perché permette di creare, modificare ed eliminare risorse;
- `Owner` sarebbe ancora più permissivo, perché oltre alla gestione delle risorse consente anche di gestire gli accessi.

Successivamente è stato configurato un budget mensile di **100 €**, associato al resource group `rg-cea-identity-6dc911`.
È stata impostata una soglia di alert del **75%**, con alert tramite email.


## Diagnosi dei casi

### Caso A — `Please run 'az login' to setup account`

- **Sintomo:** Azure CLI richiede di eseguire `az login`.
- **Causa probabile:** non è presente una sessione autenticata valida oppure la sessione precedente non è più disponibile.
- **Verifica:** eseguire `az account show --output table` per verificare la presenza di un account attivo.
- **Correzione:** eseguire `az login` e autenticarsi con l'account corretto.
- **Risultato atteso:** Azure CLI riconosce l'identità autenticata e permette di eseguire i comandi compatibili con i relativi permessi.

### Caso B — `AuthorizationFailed ... Microsoft.Authorization/roleAssignments/write ...`

- **Sintomo:** l'utente è autenticato, ma Azure restituisce `AuthorizationFailed` durante la creazione di una role assignment.
- **Causa probabile:** l'identità non dispone dei privilegi necessari per eseguire l'azione `Microsoft.Authorization/roleAssignments/write` sullo scope interessato.
- **Verifica:** controllare le role assignment applicabili all'identità e allo scope mediante `az role assignment list`.
- **Correzione:** utilizzare un'identità che possieda i permessi necessari, ad esempio un ruolo che consenta la gestione delle assegnazioni RBAC, oppure assegnare il privilegio minimo necessario sullo scope corretto.
- **Risultato atteso:** l'identità autorizzata riesce a creare la role assignment.

### Caso C — `ScopeLocked`

- **Sintomo:** il tentativo di eliminare il resource group restituisce l'errore `ScopeLocked`.
- **Causa probabile:** sul resource group è presente un management lock di tipo `CanNotDelete`.
- **Verifica:** eseguire: `az lock list --resource-group "$LAB_RG" --output table`. Il controllo ha mostrato il lock `lock-cea-delete` di tipo `CanNotDelete`.
- **Correzione:** rimuovere il lock solamente quando l'eliminazione della risorsa è realmente prevista e autorizzata.
- **Risultato atteso:** dopo la rimozione del lock, l'eliminazione può essere eseguita se l'identità possiede anche le autorizzazioni RBAC necessarie.


## Budget, lock e cleanup

Nel laboratorio è stato assegnato il ruolo `Reader` al gruppo `grp-TeamFinOps-6dc911` permettendo loro di poter solamente consultare le risorse e non poterle modificare o eliminare. 

Il budget ha una funzione di monitoraggio e alert: il raggiungimento della soglia non blocca automaticamente la creazione di risorse e non impedisce di superare l'importo configurato.
Nel laboratorio è stato configurato:
- **budget:** `100 €`;
- **periodo:** mensile;
- **soglia:** `75%`;
- **tipo:** costo effettivo;
- **valore corrispondente alla soglia:** `75 €`.

Sul resource group è presente il lock `lock-cea-delete` di tipo: `CanNotDelete`
Il lock impedisce l'eliminazione del resource group, ma non ne impedisce la consultazione.
Questo comportamento è stato verificato eseguendo prima una lettura tramite `az group show` e successivamente un tentativo di eliminazione, che ha restituito `ScopeLocked`.

### Cleanup

Da completare al termine del laboratorio con la rimozione degli oggetti temporanei e la verifica finale dell'eliminazione del resource group.


## Risultato finale

- output anonimizzati utilizzati: output di `az role assignment list`, `az lock list`, `az group show` e dell'errore `ScopeLocked`, omettendo o anonimizzando dati personali e identificativi non necessari;
- cleanup verificato:
- hash abbreviato e messaggio del commit: