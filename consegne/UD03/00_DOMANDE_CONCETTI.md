# Consegna UD03 — Domande sui concetti

Per ciascuna domanda 1–8 riporta una risposta motivata.

1. L'**autenticazione** verifica l'identità dell'utente, cioè stabilisce chi sta effettuando l'accesso.
L'**autorizzazione**, invece, stabilisce quali operazioni quell'identità può eseguire su una determinata risorsa.
Un utente può quindi autenticarsi correttamente su Azure ma non avere i permessi necessari per creare, modificare o eliminare una determinata risorsa.
2. Un **ruolo Microsoft Entra** riguarda principalmente la gestione delle identità e del tenant, ad esempio utenti, gruppi e configurazioni di Microsoft Entra ID.
Un **ruolo Azure RBAC**, invece, stabilisce quali operazioni un'identità può eseguire sulle risorse Azure, come resource group, macchine virtuali o storage account.
Quindi i ruoli Entra riguardano soprattutto la gestione delle identità, mentre i ruoli Azure riguardano l'accesso e la gestione delle risorse.
3. Una role assignment è formata da: `Principal + Role definition + Scope`
    - **Principal**: l'identità a cui viene assegnato il ruolo, ad esempio un utente, un gruppo o una managed identity.
    - **Role definition**: l'insieme delle azioni consentite o escluse dal ruolo, ad esempio `Reader`, `Contributor` o `Owner`.
    - **Scope**: il livello sul quale l'assegnazione viene applicata, ad esempio sottoscrizione, resource group o singola risorsa.
4. Il ruolo `Contributor` permette anche di creare, modificare ed eliminare risorse, mentre `Reader` permette soltanto di consultarle.
Assegnare `Contributor` a livello di sottoscrizione concederebbe privilegi molto più ampi del necessario, aumentando inutilmente l'impatto di un errore o di una compromissione. Quindi è preferibile assegnare `Reader` direttamente sul relativo resource group.
5. Un **ruolo ereditato** non si può rimuovere dalla risorsa figlia perché l’assegnazione è stata creata su uno scope superiore. La risorsa figlia riceve automaticamente quel ruolo per ereditarietà. Per modificarlo o rimuoverlo bisogna intervenire sullo scope in cui la role assignment è stata originariamente definita.
6. No.
Il tag `deleteAfter` è soltanto un metadato associato alla risorsa ed è usato soltanto per indicare una data prevista di eliminazione, quindi il tag, da solo, non impone alcun blocco tecnico e non costituisce un limite. Per impedire realmente l'eliminazione è necessario utilizzare un meccanismo specifico, come un lock `CanNotDelete`.
7. Il ruolo `Reader` è un'autorizzazione RBAC assegnata a un'identità e stabilisce quali operazioni quell'identità può eseguire. Nel caso di `Reader` può visualizzare le risorse ma non può modificarle o eliminarle.
Il lock `CanNotDelete`, invece, è una protezione di governance applicata alla risorsa o allo scope. Anche se un utente possiede normalmente i permessi necessari per eliminare la risorsa, il lock `CanNotDelete` ne impedisce l'eliminazione finché il lock non viene rimosso.
8. Un **budget Azure** è uno strumento di monitoraggio dei costi. Permette di definire una soglia e generare notifiche quando la spesa raggiunge una determinata percentuale del budget, ma non rappresenta un limite automatico alla spesa. Se il budget viene superato, Azure non blocca automaticamente le risorse né ne interrompe necessariamente l'utilizzo. Per questo motivo un budget serve principalmente a monitorare e segnalare i costi, non a impedire tecnicamente che vengano superati.

