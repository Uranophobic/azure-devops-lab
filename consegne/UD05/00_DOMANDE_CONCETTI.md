# Consegna UD05 — Domande sui concetti

Per ciascuna domanda 1–8 riporta una risposta motivata.

1. **Perché due VNet da collegare non devono avere CIDR sovrapposti?** 

Perché i router di rete non saprebbero come instradare correttamente il traffico, gli spazi di indirizzamento devono quindi essere distinti per garantire che i pacchetti di dati raggiungano il destinatario corretto.
2. **Quale rete è più grande, /24 o /26, e perché?**

`/24`, perché contiene 256 indirizzi complessivi, mentre una `/26` ne contiene 64.
3. **Perché un public IP non garantisce raggiungibilità?**

Perché non basta che la risorsa disponga di un indirizzo IP pubblico, l'endpoint pubblico deve essere associato a una risorsa in ascolto e NSG, route e sistema operativo devono consentire il flusso di traffico.
4. **Come viene scelta una regola NSG tra più corrispondenti?**

Le regole hanno una priorità, rappresentata da un numero compreso tra 100 e 4096. Vengono valutate prima quelle con il numero di priorità più basso.
5. **Che cosa significa che un NSG è stateful?**

Significa che quando un flusso viene consentito in una direzione, il traffico di risposta relativo a quello stesso flusso non richiede una regola speculare. Questo però non garantisce che possa essere avviata una connessione indipendente nella direzione opposta.
6. **Perché un Allow sulla NIC non supera un Deny applicabile sulla subnet?**

Perché la NIC è una scheda di rete virtuale che, tipicamente, permette a una risorsa di collegarsi a una VNet tramite una determinata subnet. Se alla subnet è associato un NSG con una regola `Deny` applicabile, tale regola interessa tutte le NIC presenti nella subnet. Di conseguenza, un `Allow` presente nell'NSG associato alla NIC non può annullare il `Deny` applicato a livello di subnet.
7. **Qual è la differenza tra DNS, routing e NSG?**

Il DNS è il servizio di risoluzione dei nomi che associa nomi e indirizzi IP, ma non gestisce direttamente il traffico e non sostituisce route o NSG. Il routing indica invece quale percorso deve seguire il traffico per raggiungere una determinata destinazione. L'NSG contiene regole per il traffico in entrata e in uscita che consentono o negano il traffico in base a origine, destinazione, porta e protocollo.
8. **Perché IP Flow Verify verrà completato dopo la creazione della VM?**

Perché il suo compito è verificare se un determinato flusso di traffico verso o da una VM viene consentito oppure negato.



















