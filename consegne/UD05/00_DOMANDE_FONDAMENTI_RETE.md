# Consegna UD05 — Domande sui concetti

1. **Qual è la differenza tra switch e router?**

Uno switch collega dispositivi all'interno della stessa rete locale e inoltra i frame principalmente in base agli indirizzi MAC. Un router collega reti differenti e instrada i pacchetti in base agli indirizzi IP.

2. **Qual è la differenza tra MAC e IP?**

Il MAC address identifica un'interfaccia di rete a livello locale ed è utilizzato principalmente nel livello 2. L'indirizzo IP identifica logicamente un host all'interno di una rete ed è utilizzato per instradare il traffico tra reti.

3. **Che cosa significa `/24`?**

`/24` indica che i primi 24 bit dell'indirizzo IP rappresentano la parte di rete. In IPv4 corrisponde alla subnet mask `255.255.255.0` e comprende 256 indirizzi complessivi.

4. **Che cos'è il default gateway?**

Il default gateway è il dispositivo, normalmente un router, a cui un host invia il traffico destinato a reti diverse dalla propria rete locale.

5. **A cosa serve DHCP?**

DHCP assegna automaticamente ai dispositivi parametri di rete come indirizzo IP, subnet mask, default gateway e server DNS.

6. **A cosa serve DNS?**

DNS traduce i nomi di dominio o host, ad esempio `server.example.com`, nei relativi indirizzi IP, permettendo di raggiungere le risorse tramite nomi invece che tramite indirizzi numerici.

7. **Qual è la differenza tra TCP e UDP?**

TCP è un protocollo orientato alla connessione, affidabile e con controllo dell'ordine e della consegna dei dati. UDP è senza connessione, più leggero e veloce, ma non garantisce consegna, ordine o ritrasmissione dei pacchetti.

8. **Che cos'è una porta?**

Una porta è un identificatore numerico utilizzato dai protocolli di trasporto per distinguere i diversi servizi o applicazioni presenti su uno stesso host, ad esempio porta `80` per HTTP o `443` per HTTPS.

9. **Che cosa fa una route?**

Una route indica quale percorso deve seguire il traffico per raggiungere una determinata rete o destinazione, specificando normalmente una rete di destinazione e un next hop.

10. **Che cosa fa un firewall?**

Un firewall controlla il traffico di rete in ingresso e in uscita e lo consente o lo blocca in base a regole definite, ad esempio su indirizzi IP, protocolli e porte.

11. **Che cos'è una VLAN?**

Una VLAN è una rete locale virtuale che permette di suddividere logicamente una rete fisica in più domini di broadcast separati, anche quando i dispositivi sono collegati alla stessa infrastruttura di switching.

12. **Perché VLAN e VNet non sono la stessa cosa?**

Una VLAN è una tecnologia di segmentazione di rete di livello 2, tipica delle reti locali fisiche. Una VNet Azure è invece una rete virtuale di livello 3 definita nel cloud, basata su spazi di indirizzamento IP, subnet, routing e servizi di rete Azure.

13. **Che cosa fa NAT?**

NAT, Network Address Translation, modifica gli indirizzi IP nei pacchetti per consentire, ad esempio, a più dispositivi con indirizzi privati di comunicare verso reti esterne utilizzando uno o più indirizzi pubblici.

14. **Che cosa rappresenta una DMZ?**

Una DMZ è una zona di rete separata e controllata utilizzata per ospitare servizi esposti verso reti esterne, come Internet, mantenendoli isolati dalla rete interna più sensibile.

15. **Qual è lo scopo di una VPN?**

Una VPN crea un collegamento cifrato tra due endpoint o reti attraverso una rete non fidata, come Internet, permettendo una comunicazione sicura.

16. **Che informazioni fornisce `ipconfig /all`?**

`ipconfig /all` mostra la configurazione di rete dettagliata delle interfacce di un sistema Windows, inclusi indirizzo IP, subnet mask, default gateway, server DNS, DHCP, MAC address e altre informazioni dell'adattatore.

17. **Che cosa verifica `nslookup`?**

`nslookup` interroga il DNS per verificare la risoluzione di un nome in un indirizzo IP, o viceversa, e permette di controllare quale server DNS ha fornito la risposta.

18. **Perché un DNS funzionante non garantisce la connettività applicativa?**

Perché la risoluzione DNS verifica soltanto che il nome venga tradotto correttamente in un indirizzo IP. La connessione può comunque fallire a causa di problemi di routing, firewall o NSG, porta chiusa, servizio non in ascolto o configurazione errata dell'endpoint.







