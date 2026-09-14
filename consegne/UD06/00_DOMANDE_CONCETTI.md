# UD06 — Risposte alle domande sui concetti

## 1.
**Risposta: Compute, OS disk, eventuali data disk, NIC, indirizzo IP pubblico e privato, VNet e subnet.**

## 2.
**Risposta: L'indirizzo IP pubblico rappresenta l'indirizzo esposto sulla rete, ma per poter effettivamente raggiungere la VM servono anche regole di rete che consentano il traffico, ad esempio una regola NSG con Allow, e un servizio in ascolto sulla porta corretta.**

## 3.
**Risposta: Availability Zone è una zona fisicamente separata all'interno della stessa regione, con dipendenze infrastrutturali separate rispetto alle altre zone della regione. Availability Set è invece un raggruppamento logico di VM che Azure distribuisce tra diversi fault domain e update domain, per ridurre i guasti correlati e l'impatto della manutenzione. Infine, VM Scale Set gestisce un gruppo di VM coordinate, fornendo un modello comune per crearle, aggiungerle e scalarle.**

## 4.
**Risposta: Scale up significa assegnare più risorse alla stessa istanza. Scale out significa aumentare il numero di istanze.**

## 5.
**Risposta: Azure Monitor Autoscale permette di modificare automaticamente il numero di istanze di un VM Scale Set applicando profili e regole basati su metriche oppure su pianificazioni.**

## 6.
**Risposta: App Service Plan definisce la capacità di calcolo sulla quale viene eseguita l'applicazione. La Web App è invece l'applicazione che gira sopra quel piano.**

## 7.
**Risposta: Azure Monitor Autoscale usa regole esplicite basate su metriche, soglie, intervalli temporali o pianificazioni. App Service Automatic Scaling gestisce invece automaticamente il numero di istanze in base al traffico HTTP, rispettando i limiti configurati.**

## 8.
**Risposta: Le metriche sono valori numerici associati a timestamp e rispondono bene a domande numeriche e temporali. I log servono quando è necessario capire il dettaglio degli eventi.**

## 9.
**Risposta: Il Recovery Services vault è il contenitore che gestisce i backup. La backup policy definisce frequenza e retention dei backup. Il recovery point è uno stato salvato della risorsa che può essere utilizzato per il ripristino.**

## 10.
**Risposta: HA, Backup e DR sono tre livelli della resilienza. High Availability significa continuare a erogare il servizio o ridurre al minimo il downtime. Backup significa ripristinare dati o sistema da un recovery point. Disaster Recovery significa ripristinare il workload in un ambiente alternativo secondo un piano.**

## 11.
**Risposta: RPO indica la quantità massima di dati che si può accettare di perdere, espressa in tempo. RTO indica il tempo massimo entro il quale il servizio deve essere ripristinato dopo un'interruzione.**

## 12.
**Risposta: Perché la deallocazione interrompe il costo della capacità compute, ma risorse associate come dischi, eventuali IP pubblici e servizi di backup possono continuare a generare costi.**
