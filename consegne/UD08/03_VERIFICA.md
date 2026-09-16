# UD08 — Verifica

## Parte A

1. B
2. A
3. B
4. B
5. B
6. B
7. B
8. A

## Parte B

9. La Continuous Delivery consiste nel mantenere il software in uno stato che ne consenta il rilascio. Build, test e controlli vengono automatizzati quanto possibile, ma il passaggio in produzione può ancora richiedere una decisione o un'approvazione esplicita.
La Continuous Deployment, invece, prevede che una modifica che supera tutti i controlli previsti possa essere distribuita automaticamente in produzione, senza un normale passaggio di approvazione manuale.

10. È utile creare un feature branch perché permette di sviluppare una modifica in modo isolato senza intervenire direttamente su `main`, che può così rimanere stabile. La modifica può essere verificata, sottoposta a Pull Request e review e integrata solo quando è pronta.

11. Quando vengono aggiunti nuovi commit allo stesso head branch e viene eseguito il push, la Pull Request si aggiorna automaticamente. I nuovi commit entrano nella stessa PR e viene aggiornato anche il diff.

12. In una code review è utile controllare almeno:

- che la modifica risponda effettivamente al requisito;
- che il diff contenga soltanto le modifiche necessarie e non file estranei;
- che non siano presenti token, password o altri segreti;
- che il codice o la documentazione siano leggibili;
- che siano presenti test o verifiche coerenti;
- che la modifica non introduca possibili regressioni.

13. L'accesso temporaneo di un collaboratore deve essere rimosso quando la collaborazione termina per applicare il principio del least privilege: un utente deve avere soltanto l'accesso necessario, alla sola risorsa necessaria e per il solo tempo necessario. Mantenere autorizzazioni non più necessarie aumenta inutilmente la superficie di rischio.

14. Il frontend è `static/index.html` ed è la parte visualizzata dal browser; il JavaScript richiama l'API per ottenere i prodotti.
Il backend/API è `server.py`: avvia il server HTTP, legge configurazione e dati, gestisce gli endpoint e restituisce le risposte, comprese quelle JSON.
La configurazione è contenuta in `config.json` e mantiene separati dal codice i valori utilizzati per configurare il comportamento dell'applicazione.
I dati sono contenuti in `data/products.json` e rappresentano i prodotti gestiti dall'applicazione.

## Parte C

15. Prima del merge individuo diversi problemi. La PR contiene troppe modifiche e molto poco coese fra loro, andrebbe fatto un commit per ogni modifica specifica.
La presenza di `token.txt` rappresenta inoltre un potenziale problema di sicurezza, perché nella PR non dovrebbero essere presenti token, password o altri dati sensibili.
Infine i test HTTP non sono documentati, quindi manca l'evidenza che le modifiche a `config.json` e `server.py` siano state verificate e che l'applicazione continui a comportarsi come previsto.

16. Prima di approvare richiederei di rendere la PR coerente con il requisito, rimuovendo il file di consegna appartenente all'altra UD e qualsiasi file estraneo alla modifica.
Richiederei inoltre di rimuovere `token.txt` e verificare che nella PR non siano presenti token, password o altri dati sensibili.
Successivamente chiederei di eseguire e documentare i test HTTP necessari per verificare il comportamento dell'applicazione, controllando gli endpoint interessati dalla modifica.
Il contributor dovrebbe quindi creare un nuovo commit con le correzioni e fare push sulla stessa branch, in modo che la stessa Pull Request si aggiorni automaticamente.
A quel punto controllerei nuovamente il diff, verificherei che contenga soltanto le modifiche necessarie, che non siano presenti dati sensibili e che i test abbiano esito corretto. Solo dopo questi controlli approverei la PR e procederei con il merge.
