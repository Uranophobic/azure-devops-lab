# Consegna UD05 — Laboratorio autonomo

## Inventario iniziale

### VNet e subnet

| Elemento | CIDR | Scopo | Sovrapposizioni |
|---|---|---|---|
| VNet | `10.50.0.0/16` | Spazio di indirizzamento della rete virtuale | Nessuna |
| subnet web | `10.50.10.0/24` | Subnet destinata al livello web | Nessuna |
| subnet data | `10.50.20.0/24` | Subnet destinata al livello dati | Nessuna |

### NSG e associazioni

| NSG | Scope associato | Regola | Priorità | Origine | Porta | Esito |
|---|---|---|---:|---|---|---|
| `nsg-data-11868d` | subnet `snet-data` | `Allow-Web-Postgres` | 300 | `10.50.10.0/24` | `5432/TCP` | Allow |

Le due NIC presenti sono `nic-web-01`, associata alla subnet `snet-web` con indirizzo privato `10.50.10.4`, e `nic-data-01`, associata alla subnet `snet-data` con indirizzo privato `10.50.20.4`. Entrambe sono prive di Public IP.

### Regole

All'inizio dell'attività era presente la regola `Allow-Web-Postgres` con priorità `300`, che consente il traffico TCP proveniente dalla subnet `10.50.10.0/24` verso la porta `5432`.

Durante il laboratorio è stata introdotta anche la regola `Deny-Web-Postgres-Auto` con priorità `250`, configurata sullo stesso traffico.

## Guasto e diagnosi

- regola introdotta: Deny-Web-Postgres-Auto
- ordine di priorità osservato: 250 per `Deny-Web-Postgres-Auto`, 300 per `Allow-Web-Postgres`
- sintomo: Il team segnala che il livello web non potrebbe collegarsi al database TCP 5432.
- ipotesi: Presenza di una regola 'Deny' che impedisce il traffico previsto 
- controllo: tramite l'elenco delle regole NSG ordinate per priorità è stato verificato che `Deny-Web-Postgres-Auto`, con priorità `250`, precede `Allow-Web-Postgres`, con priorità `300`. Poiché entrambe sono applicabili al traffico proveniente da `10.50.10.0/24` verso la porta `5432`, la regola `Deny` è valutata per prima.
- correzione minima:  eliminazione della regola conflittuale 
- verifica dopo la correzione: nella lista delle regole è presente la regola `Allow-Web-Postgres`, con priorità `300`, che consente il traffico proveniente da `10.50.10.0/24` verso la porta `5432/TCP`

## Casi ulteriori

### `InvalidAddressPrefix`

- livello: indirizzamento di rete / CIDR
- controllo: verificare che il prefisso IP inserito sia valido, correttamente scritto e coerente con lo spazio di indirizzamento della VNet o della subnet
- correzione minima: correggere il CIDR errato senza modificare altre risorse non coinvolte

### `SecurityRuleConflict`

- livello: sicurezza / NSG
- controllo: verificare regole esistenti, direzione e priorità per individuare eventuali conflitti
- correzione minima: modificare o rimuovere soltanto la regola in conflitto, mantenendo invariate le altre regole corrette

### Un nome DNS non viene risolto

- livello: DNS / risoluzione dei nomi
- controllo: verificare la risoluzione del nome, ad esempio con `nslookup`, e controllare la configurazione del server DNS
- correzione minima: correggere il record DNS o la configurazione DNS errata

### La porta risulta filtrata da un NSG

- livello: sicurezza di rete / NSG
- controllo: verificare le regole inbound e outbound applicabili, le relative priorità, il protocollo e la porta
- correzione minima: modificare o aggiungere soltanto la regola necessaria per consentire il traffico previsto, mantenendo lo scope il più limitato possibile

## Cleanup e risultato finale

- regola autonoma rimossa: `Deny-Web-Postgres-Auto`
- cleanup verificato: si
- hash abbreviato e messaggio del commit: 2e6a6fd  UD05: Laboratorio Autonomo



