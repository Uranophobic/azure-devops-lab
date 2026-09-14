# UD06 — Verifica

## Parte A

1. A
2. B
3. B
4. A
5. B
6. B
7. A
8. B

## Parte B

9. Availability Zone è una zona fisicamente separata all'interno della stessa regione, con dipendenze infrastrutturali separate rispetto alle altre zone della regione. Availability Set è invece un raggruppamento logico di VM che Azure distribuisce tra diversi fault domain e update domain, per ridurre i guasti correlati e l'impatto della manutenzione. Infine, VM Scale Set gestisce un gruppo di VM coordinate, fornendo un modello comune per crearle, aggiungerle e scalarle.

10. Scale up significa assegnare più risorse alla stessa istanza. Scale out significa aumentare il numero di istanze.

11. Azure Monitor Autoscale usa regole esplicite basate su metriche, soglie, intervalli temporali o pianificazioni. App Service Automatic Scaling gestisce invece automaticamente il numero di istanze in base al traffico HTTP, rispettando i limiti configurati.

12. HA, Backup e DR sono tre livelli della resilienza. High Availability significa continuare a erogare il servizio o ridurre al minimo il downtime. Backup significa ripristinare dati o sistema da un recovery point. Disaster Recovery significa ripristinare il workload in un ambiente alternativo secondo un piano.

13. Il Recovery Services vault è il contenitore che gestisce i backup. La backup policy definisce frequenza e retention dei backup. Il recovery point è uno stato salvato della risorsa che può essere utilizzato per il ripristino

14. RPO indica la quantità massima di dati che si può accettare di perdere, espressa in tempo. RTO indica il tempo massimo entro il quale il servizio deve essere ripristinato dopo un'interruzione.

## Parte C

15. La causa più probabile è la regola Deny-HTTP con priorità 100. Negli NSG le regole con numero di priorità più basso vengono valutate prima, quindi il traffico HTTP proveniente da My IP viene bloccato dalla regola Deny-HTTP prima che Azure possa arrivare alla regola Allow-HTTP con priorità 300.

16. La correzione minima è modificare la priorità delle regole in modo che Allow-HTTP venga valutata prima di Deny-HTTP, oppure rimuovere la regola Deny-HTTP se non necessaria. Per verificare il ripristino userei:

```bash
curl -I http://localhost
```

sulla VM, per confermare che Nginx risponda localmente;

`IP Flow Verify`, per verificare che il traffico TCP sulla porta `80` dal mio IP sia consentito e quale regola NSG sia responsabile;

```bash
curl -I http://<Public-IP>
```

dall'esterno, per verificare il percorso completo fino a Nginx.



