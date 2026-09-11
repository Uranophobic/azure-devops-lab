# Consegna UD05 — Verifica

## Parte A — Scelta singola

Per le domande 1–8 riporta risposta e motivazione.

1. B
2. B
3. B
4. A
5. A
6. A
7. B
8. B

## Parte B — Risposte brevi

9. **Spiega perché le subnet devono lasciare margine di crescita**: ritagliano porzioni non sovrapposte di indirizzi all'interno della VNet, quindi devono essere abbastanza ampie da consentire una crescita futura degli indirizzi necessari, non solo di quelli utilizzati oggi.
10. **Distingui NSG, route e DNS**: l'NSG protegge la subnet tramite regole che consentono o negano il traffico, la route indica l'instradamento dei pacchetti, mentre il DNS si occupa della risoluzione dei nomi associandoli agli indirizzi IP.
11. **Spiega la statefulness di un NSG**: significa che, quando un flusso viene consentito in una direzione, il traffico di risposta relativo allo stesso flusso non richiede una regola speculare nella direzione opposta. Questo non consente però automaticamente una nuova connessione indipendente nella direzione opposta.
12. **Perché una baseline NSG sulla subnet può essere più semplice da governare?** Perché la regola dell'NSG associato a una subnet vale per tutte le NIC presenti nella subnet, rendendo più semplice mantenere regole comuni.
13. **Quali verifiche sono possibili su una NIC senza VM e quale prova manca?** Su una NIC senza VM è possibile verificare la configurazione della NIC, l'indirizzo IP privato, la subnet di appartenenza e l'associazione dell'NSG alla subnet. Non è invece possibile verificare gli Effective NSG e le Effective Routes, perché la NIC non è collegata a una VM in esecuzione; manca inoltre la prova del flusso di traffico reale.

## Parte C — Caso situazionale

14. Il cambio di nome non modifica l'esito perché le regole NSG non vengono valutate in base al nome, ma in base alla priorità. La regola `Deny-Web` con priorità `150` viene valutata prima della regola `Allow-Web` con priorità `400`, quindi il traffico TCP 443 proveniente da `10.60.10.0/24` resta negato.
15. La correzione minima consiste nel rimuovere o modificare la regola `Deny-Web` che blocca lo stesso traffico.
16. Se dopo la correzione l'applicazione resta irraggiungibile, controllerei prima che il nome venga risolto correttamente dal DNS, poi che il routing verso la destinazione sia corretto, che non siano presenti altri NSG o regole `Deny` applicabili, che l'indirizzo e la porta `443` siano corretti e infine che il servizio applicativo sia effettivamente in ascolto sulla porta prevista.
