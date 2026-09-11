# Consegna UD05 — Laboratorio guidato

## Piano di indirizzamento

| Elemento | CIDR | Scopo | Sovrapposizioni |
|---|---|---|---|
| VNet | `10.50.0.0/16` | Spazio di indirizzamento della rete virtuale | Nessuna |
| subnet web | `10.50.10.0/24` | Subnet destinata al livello web | Nessuna |
| subnet data | `10.50.20.0/24` | Subnet destinata al livello dati | Nessuna |

## NSG e associazioni

| NSG | Scope associato | Regola | Priorità | Origine | Porta | Esito |
|---|---|---|---:|---|---|---|
| `nsg-data-11868d` | subnet `snet-data` | `Allow-Web-Postgres` | 300 | `10.50.10.0/24` | `5432/TCP` | Allow |

È stata inoltre creata temporaneamente la regola `Deny-Web-Postgres` con priorità `200` per simulare un errore di configurazione.

Poiché una priorità numericamente più bassa viene valutata prima, la regola `Deny` con priorità `200` prevaleva sulla regola `Allow` con priorità `300`.

La regola di test è stata successivamente eliminata, ripristinando la configurazione prevista.

## Verifica effettiva

- NIC create e subnet:
  - `nic-data-01` → subnet `snet-data` → IP privato `10.50.20.4`
  - `nic-web-01` → subnet `snet-web` → IP privato `10.50.10.4`


- NSG effettivi osservati:
  non è stato possibile visualizzare gli Effective NSG tramite `az network nic list-effective-nsg`, perché la NIC non è ancora collegata a una VM in esecuzione. Il comando ha restituito l'errore `NicMustBeAttachedToRunningVmToGetEffectiveSecurityGroups`.

- Route effettive osservate:
  non è stato possibile completare la verifica delle Effective Routes perché la NIC non è ancora associata a una VM in esecuzione.

- Ciò che è stato verificato:
  sono state verificate la creazione della VNet e delle subnet, la creazione delle NIC, l'associazione dell'NSG alla subnet `snet-data`, la presenza della regola `Allow-Web-Postgres` e il corretto comportamento delle priorità NSG.

- Ciò che richiede ancora un workload:
  la verifica delle regole NSG effettive, delle route effettive e del flusso di traffico reale richiede una VM in esecuzione collegata alla NIC. 


## Costi e cleanup

### Risorse create

Durante il laboratorio ho creato inizialmente tramite Azure CLI il Resource Group `rg-cea-network-11868d` e, al suo interno, la VNet `vnet-cea-11868d` con spazio di indirizzamento 10.50.0.0/16. Successivamente ho creato le due subnet snet-web `10.50.10.0/24` e snet-data `10.50.20.0/24`.

Ho poi creato l'NSG `nsg-data-11868d` e l'ho associato alla subnet snet-data. Tramite il Portale Azure ho configurato la regola inbound `Allow-Web-Postgres`, con priorità 300.

Successivamente ho creato due interfacce di rete: `nic-data-01` tramite Portale Azure, associandola alla subnet `snet-data`, e `nic-web-01` tramite Azure CLI, associandola alla subnet `snet-web`. 

Durante il laboratorio è stata inoltre creata la regola `Deny-Web-Postgres` con priorità 200, utilizzata per verificare il comportamento delle priorità NSG. Dopo la verifica, la regola è stata eliminata.

### Possibili costi

Le risorse utilizzate in questo laboratorio, come VNet, subnet, NSG e NIC, non introducono normalmente un costo diretto significativo se utilizzate da sole.

### Cleanup

Il Resource Group `rg-cea-network-11868d` è stato eliminato insieme alle risorse create durante il laboratorio.

La rimozione è stata verificata tramite il comando `az group exists --name "$LAB_RG"`, che ha restituito `false`.

## Rilevanza professionale

La progettazione del piano CIDR deve precedere il deployment dei workload perché permette di evitare sovrapposizioni tra reti e di organizzare correttamente la segmentazione dell'infrastruttura.

Le regole NSG devono essere definite e verificate prima del deployment, in quanto definiscono regole per traffico in uscita e in entrata, consentendolo o negandolo in base a origine, destinazione, porta o protocollo.

