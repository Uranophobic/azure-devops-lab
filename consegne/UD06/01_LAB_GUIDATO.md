# UD06 — Consegna laboratorio guidato

## VM

- image: `Ubuntu Server 24.04 LTS`
- size: `Standard_DC1s_v3`
- private IP: `172.16.0.4`
- public IP: `172.213.209.65`
- NIC: `vm-ud06-linux474`
- subnet: `vnet-ud06/snet-vm`
- OS disk: `vm-ud06-linux_OsDisk_1_0e94339141274a98aef1efa21033567c`

## Accesso e workload

- SSH: accesso tramite utente `azureuser` e chiave SSH `Ed25519`
- Nginx: installato correttamente; servizio `active (running)`
- test localhost: `HTTP/1.1 200 OK` tramite `curl -I http://localhost`
- test esterno: `HTTP/1.1 200 OK` tramite `curl -I http://$LAB_VM_IP`
- IP Flow Verify: `Access allowed`
- regola responsabile: `Allow-HTTP-MyIP`

## Azure Monitor

- metrica VM: `Percentage CPU`, `Network In Total`, `Network Out Total`
- intervallo: ultimi `30 minuti`
- aggregazione: `Average` per `Percentage CPU`; `Sum` per `Network In Total` e `Network Out Total`
- osservazione: la CPU è rimasta generalmente molto bassa e quasi stabile, con due brevi picchi, uno vicino al `3%` e uno vicino all'`1%`. Il traffico di rete in ingresso e in uscita è rimasto generalmente basso, con un picco evidente in entrambe le metriche.

## VMSS / Autoscale

- min: `1`
- default: `1`
- max: `3`
- metrica: `Percentage CPU`
- soglia: CPU media `> 70%` per `5 minuti`
- azione: `scale out` di `1` istanza
- perché serve un max: il limite massimo impedisce una crescita incontrollata del numero di istanze, limitando sia il consumo delle risorse sia l'aumento dei costi.

## App Service

- App Service Plan: `plan-ud06-26559`
- tier: `Free (F1)`
- Web App: `ud06-web-1789397426-13257`
- hostname: `ud06-web-1789397426-13257.azurewebsites.net`
- test HTTPS: `HTTP/1.1 200 OK`

## Scaling App Service

| Modalità | Basata su |
|---|---|
| Manual | Numero di istanze impostato manualmente dall'amministratore |
| Azure Monitor Autoscale | Regole basate su metriche, soglie, intervalli temporali e limiti minimi/massimi |
| Automatic Scaling | Gestione automatica del numero di istanze in base al traffico HTTP e ai limiti configurati per App Service |

## Azure Monitor App Service

- metrica: `Requests`
- osservazione: sono state registrate `5 richieste` nell'intervallo osservato.

- metrica: `Response Time`
- osservazione: il tempo medio di risposta è stato di circa `8,80 ms`.

## Backup

- Recovery Services vault: non creato; configurazione osservata dal Portale
- frequenza ipotizzata: giornaliera alle `02:00`
- retention: `7 giorni`
- recovery point: non creato; rappresenta uno stato della VM generato dal backup e utilizzabile successivamente per il ripristino
- backup reale avviato?: no

## HA / Backup / DR

- Scenario A: `High Availability`, ridurre l'impatto del guasto di una singola istanza
- Scenario B: `Backup`, recuperare il contenuto della VM a uno stato precedente
- Scenario C: `Disaster Recovery`, ripristinare il workload in un'altra regione dopo un grave outage

## Cleanup

- Resource Group eliminato:
- `az group exists`: