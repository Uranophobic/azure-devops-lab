# Consegna UD02 — Verifica

## Parte A — Scelte operative

Per le domande 1–8 riporta risposta e motivazione.

1. B. IaaS
2. C. Configurazione dell'applicazione, identità, accessi e dati.
3. C. È un contenitore logico per risorse che possono condividere ciclo di vita e governance.
4. B. La località indica dove Azure conserva i metadati del resource group; le risorse possono avere località proprie.
5. B. az account show
6. C. stcea02a7f9
7. B. Il tag documenta l'intenzione, ma serve ancora una procedura o una policy che esegua l'eliminazione.
8. C. az group exists --name <NOME> restituisce false.

## Parte B — Risposte brevi

9. Un'azienda può utilizzare il **cloud pubblico** quando esegue le proprie applicazioni su risorse fornite da un provider come **Azure**.
Può utilizzare un **cloud privato** quando l'infrastruttura cloud è dedicata esclusivamente all'azienda e viene gestita all'interno della propria organizzazione o in un ambiente dedicato.
In uno scenario **ibrido**, invece, l'azienda può mantenere alcuni sistemi o dati nella propria infrastruttura privata e utilizzare contemporaneamente Azure per altre applicazioni o servizi.

10. Il **tenant Microsoft Entra** rappresenta l'ambiente organizzativo e di identità all'interno del quale vengono gestiti utenti, gruppi e accessi.
La **sottoscrizione** rappresenta un confine amministrativo e di fatturazione per le risorse Azure.
Il **resource group** è un contenitore logico interno a una sottoscrizione utilizzato per organizzare e gestire risorse correlate.
La **risorsa** rappresenta infine il singolo servizio Azure creato, ad esempio uno **storage account** o una **macchina virtuale**.

11. Una **region** è un'area geografica Azure che contiene uno o più datacenter.
Una **Availability Zone** è invece una zona fisicamente separata all'interno di una region, costituita da uno o più datacenter con alimentazione, raffreddamento e rete indipendenti.
 Una region può quindi contenere più Availability Zone.

12. **Azure Portal** permette di gestire le risorse attraverso un'interfaccia grafica e operazioni manuali.
**Azure CLI**, invece, permette di eseguire le stesse operazioni attraverso comandi testuali, che possono essere ripetuti, documentati e automatizzati tramite script.

13. I **tag** applicati a un resource group non vengono automaticamente ereditati dalle risorse contenute al suo interno. Per questo motivo, se si vuole che una risorsa possieda gli stessi tag del resource group, questi devono essere applicati **esplicitamente** alla risorsa.

## Parte C — Interpretazione tecnica

1. La sottoscrizione è anonimizzata. Il resource group è `rg-cea-test`. Il provider è `Microsoft.Storage`, quindi la risorsa è `storageAccounts` il cui nome è `stceatest01`

2. La località è `italynorth` mentre i tag che possiede sono:
    - `environment=lab`
    - `unit=UD02`

3. `az resource list --resource-group rg-cea-test --name stceatest01 --resource-type Microsoft.Storage/storageAccounts`

   Il comando dovrebbe restituire tra i risultati lo storage account `stceatest01` appartenente al resource group `rg-cea-test`

4. `az group delete --name rg-cea-test --yes --no-wait`

5. `az group exists --name rg-cea-test`

   Quando il comando restituisce: `false`
   significa che il resource group non esiste più e il cleanup può considerarsi concluso.

