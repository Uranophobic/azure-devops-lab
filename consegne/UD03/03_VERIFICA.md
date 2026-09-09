# Consegna UD03 — Verifica

## Parte A — Scelta singola

Per le domande 1–8 riporta risposta e motivazione.

1. B
2. D
3. C
4. B
5. B
6. C
7. B
8. B

## Parte B — Risposte brevi

9. Assegnare i ruoli a un **gruppo** è spesso preferibile alle assegnazioni individuali perché permette di centralizzare la gestione degli accessi. Il ruolo viene assegnato una sola volta al gruppo e gli utenti ricevono i relativi permessi tramite la loro appartenenza al gruppo. Inoltre è più manutenibile in quanto l'entrata o uscita dal gruppo effettua l'assegnazione in automatico, senza ricrearla ogni volta.
10. Un **ruolo Microsoft Entra** riguarda la gestione delle identità e dei servizi della directory Microsoft Entra. Ad esempio, `User Administrator` può consentire la gestione degli utenti all'interno del tenant. Un **ruolo Azure RBAC** riguarda invece l'accesso alle risorse Azure e viene applicato a uno specifico scope. Ad esempio, `Reader` assegnato a un resource group consente di consultarne le risorse senza modificarle.
11. Se un utente possiede `Reader` sul resource group ma riceve anche `Contributor` dalla sottoscrizione, l'accesso effettivo sul resource group comprende i privilegi di `Contributor`.
L'assegnazione `Contributor` effettuata a livello di sottoscrizione viene infatti ereditata dal resource group. Azure RBAC utilizza normalmente un modello additivo: l'assegnazione più limitata `Reader` non elimina i privilegi più ampi già ottenuti tramite `Contributor`. L'utente potrà quindi non soltanto consultare le risorse, ma anche crearle, modificarle ed eliminarle nei limiti previsti dal ruolo `Contributor`.
12. Per diagnosticare un errore `AuthorizationFailed` effettuerei almeno i seguenti controlli:
    1. verificare l'**autenticazione** e l'account attivo tramite `az account show`;
    2. verificare di essere collegati alla **sottoscrizione e al tenant corretti**;
    3. individuare lo **scope** sul quale viene eseguita l'operazione;
    4. verificare le **role assignment dirette ed ereditate** applicabili all'identità;
    5. controllare se il ruolo posseduto comprende l'**azione richiesta** dall'operazione;
    6. verificare eventuali ulteriori condizioni o restrizioni applicabili allo scope.
In questo modo è possibile distinguere un problema di autenticazione da un problema di autorizzazione o di scope.
13. Il tag `deleteAfter` è un **metadato** associato alla risorsa. Può indicare, ad esempio, una data prevista per la sua eliminazione, ma da solo non impedisce né esegue automaticamente la cancellazione. Il lock `CanNotDelete` è invece un controllo di **protezione operativa** che impedisce l'eliminazione dello scope o delle risorse sulle quali è applicabile finché il lock rimane presente. Il budget è uno strumento di **monitoraggio dei costi**. Permette di impostare soglie e notifiche, ma non impedisce automaticamente di spendere oltre l'importo configurato.


## Parte C — Caso situazionale

14. Il primo errore è l'assegnazione di `Contributor` sull'intera sottoscrizione a un tecnico che deve soltanto consultare una VNet è eccessivamente permissiva rispetto all'attività richiesta.
Il secondo errore è interpretare il ruolo `Contributor` come sufficiente per gestire le role assignment. `Contributor` permette di gestire le risorse, ma normalmente non include il permesso necessario per assegnare ruoli Azure RBAC.
Inoltre l'errore `ScopeLocked` durante il cleanup segnala la presenza di un lock che ne impdesce l'eliminazione.
15. Per il tecnico che deve soltato consultare la VNet la scelta più appropriata sarebbe quella del ruolo `Reader`, sullo scope della singola VNet. Eventualmente fosse necessario si potrebbe pensare di assegnargli il ruolo `Reader` sull'intera resource group.
16. Il tecnico non riesce ad assegnare `Reader` al collega perché il ruolo `Contributor` normalmente non comprende il permesso necessario per creare role assignment. L'impossibilità di eliminare lo scope è invece un problema differente in quanto indica che è presente un lock `CanNotDelete`.
