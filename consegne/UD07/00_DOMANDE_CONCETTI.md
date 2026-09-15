# UD07 — Risposte alle domande sui concetti

## 1.
**Risposta: Perché consente di selezionare soltanto le proprietà che ti interessano, così da avere un output mirato, invece di cercare manualmente una stringa all'interno dell'intero output JSON.**

## 2.
**Risposta: `table` è orientato alla lettura umana; `tsv` è orientato al riuso di valori semplici.**

## 3.
**Risposta: Significa che PowerShell non lavora soltanto con semplici stringhe di testo, ma con oggetti che possiedono proprietà e valori. Un oggetto può essere memorizzato in una variabile, le sue proprietà possono essere interrogate direttamente e può essere passato da un comando all'altro attraverso la pipeline PowerShell.**

## 4.
**Risposta: Una procedura è idempotente quando più esecuzioni portano allo stesso stato desiderato senza moltiplicare effetti indesiderati.**

## 5.
**Risposta: L'Activity Log registra eventi relativi soprattutto alle operazioni di gestione della subscription e delle risorse.Una metrica è un valore numerico raccolto a intervalli regolari e associato a un timestamp. I log contengono record strutturati, non soltanto numeri. Un record può includere timestamp, risorsa, operazione, categoria, stato, identità e dettagli.**

## 6.
**Risposta: Un Log Analytics workspace è il datastore principale di Azure Monitor Logs. Contiene tabelle nelle quali arrivano i dati raccolti e offre l'ambiente Log Analytics per eseguire query, analizzare risultati, creare visualizzazioni e alimentare alert basati sui log.**

## 7.
**Risposta: Una diagnostic setting definisce l'instradamento di specifiche categorie di log e/o metriche da una sorgente verso una o più destinazioni supportate.**

## 8.
**Risposta: L'Activity Log può essere consultato direttamente dal Portale o tramite CLI. `AzureActivity` è la tabella di Log Analytics che contiene gli eventi dell'Activity Log esportati verso il workspace.**

## 9.
**Risposta: L'Alert Rule è una regola che valuta un segnale su uno scope e genera un alert quando la condizione è soddisfatta.L'Action Group è un insieme riutilizzabile di destinatari e azioni eseguite quando un alert scatta.**

## 10.
**Risposta:`Fired` è una condizione operativa che indica che i criteri della regola di alert sono stati soddisfatti, ma un alert non significa automaticamente che si sia verificato un incidente. Va osservato caso per caso e bisogna capire se si tratta soltanto di un picco o di un vero incidente.**

## 11.
**Risposta:Se una modifica e un'anomalia avvengono vicine nel tempo, possiamo parlare di correlazione temporale. Non possiamo ancora affermare che la prima abbia causato la seconda. Per parlare di causalità servono ulteriori evidenze che dimostrino che un evento ha effettivamente provocato l'altro.**

## 12.
**Risposta:I passaggi essenziali di un troubleshooting ripetibile sono:**
**1. descrivere il sintomo**
**2. chiarire il risultato atteso**
**3. raccogliere evidenze**
**4. formulare un'ipotesi**
**5. verificare l'ipotesi**
**6. applicare la modifica minima**
**7. ripetere il test**
**8. documentare**


