# Consegna UD04 — Verifica

## Parte A — Scelta singola

Per le domande 1–8 riporta risposta e motivazione.

1. A
2. B
3. B
4. B
5. B
6. C
7. B
8. A

## Parte B — Risposte brevi

9. La **ridondanza** mantiene più copie dei dati per aumentare la disponibilità e la resilienza in caso di guasto. Le modifiche o eliminazioni effettuate sui dati possono essere replicate anche sulle altre copie.
Il **backup**, invece, conserva una copia o uno stato precedente dei dati con lo scopo di permetterne il recupero in caso di cancellazione, modifica accidentale o perdita.

10. Il **management plane** riguarda la gestione delle risorse Azure, ad esempio la lettura o modifica delle proprietà di uno Storage Account.  
Il **data plane**, invece, riguarda le operazioni sui dati contenuti nel servizio, ad esempio leggere, caricare o eliminare un Blob. 

11. Una **SAS** configurata secondo il principio del minimo privilegio dovrebbe avere: permessi minimi, scope ristretto,scadenza breve, utilizzo tramite HTTPS.

12. Il tier **Archive** non è appropriato per dati che devono essere recuperati immediatamente perché i dati sono mantenuti offline e non sono direttamente accessibili. Prima di poterli utilizzare devono essere sottoposti a **rehydration**, cioè riportati in uno stato online.

13. Non bisogna salvare account key o SAS nel repository perché sono credenziali che possono consentire l'accesso alle risorse e ai dati dello Storage Account. 

## Parte C — Caso situazionale

14. Il primo problema riguarda il ruolo `Contributor` sullo Storage Account, in quando essendo management plane esso può concedere automaticamente l'accesso ai Blob tramite Microsoft Entra ID. 
Il secondo problema è l'omissione di `--auth-mode login` che non rende esplicito l'utilizzo dell'identità Microsoft Entra ID per le operazioni sul data plane.
Il terzo problema è la condivisione di una account key in quanto è rischiosa perché concede un accesso molto ampio allo Storage Account e deve essere trattata come un segreto ad alto impatto.
Infine c'è anche un quarto problema ovvero una SAS con permessi completi e senza scadenza viola il principio del minimo privilegio.

15. Per un'applicazione che deve soltanto leggere documenti privati utilizzerei Microsoft Entra ID con Azure RBAC, evitando di distribuire account key. Assegnerei all'identità utilizzata dall'applicazione il ruolo: `Storage Blob Data Reader`mcon scope limitato al container che contiene i documenti, evitando di assegnare il ruolo all'intero Storage Account quando non necessario.

16. Per verificare l'accesso tramite Microsoft Entra ID utilizzerei esplicitamente `--auth-mode login`, ad esempio:

    ```bash
    az storage blob list \
    --account-name "$LAB_STORAGE" \
    --container-name <container> \
    --auth-mode login \
    --output table
    ```

    Verificherei inoltre le assegnazioni RBAC e i relativi scope con:

    ```bash
    az role assignment list \
    --all \
    --query "[].{Role:roleDefinitionName,Scope:scope}" \
    --output table
    ```
