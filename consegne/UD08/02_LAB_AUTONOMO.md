# UD08 — Consegna laboratorio autonomo

## Baseline

- health: `/health` → `HTTP 200`, servizio attivo e raggiungibile
- products: `/api/products` → `HTTP 200`, restituiti 4 prodotti

## Errore introdotto

- configurazione modificata: nel file di configurazione config.json è stato modificato `api_prefix` da `/api` a `/api-v2`
- endpoint funzionante: `/api-v2/products` con `HTTP 200`
- endpoint non funzionante: `/api/products` con `HTTP 404`
- comportamento frontend: 404 Not Found, errore http. Il servizio non è guasto: il problema è l’incoerenza tra il prefisso API atteso dal frontend e quello configurato nel backend.

## Diagnosi

1. **Il backend è avviato?**
Sì. Il server Python è in esecuzione e risponde alle richieste HTTP.
2. **/health funziona?**
Sì. L’endpoint /health continua a restituire HTTP 200, quindi il servizio è attivo e raggiungibile.
3. **Quale endpoint prodotti funziona?**
/api-v2/products
4. **Quale endpoint usa il frontend?**
/api/products, perché index.html non è stato modificato ma solo config. 
5. **Il problema è di rete, backend spento o contratto/configurazione incoerente?**
È un problema di configurazione/contratto applicativo incoerente. Il backend è acceso e raggiungibile, ma frontend e backend non concordano più sul prefisso delle API: il frontend richiede /api/products, mentre il backend espone /api-v2/products. Per questo la richiesta del frontend riceve 404.

## Fix

- causa: incoerenza tra il prefisso API configurato nel backend (`/api-v2`) e quello utilizzato dal frontend (`/api`)
- fix: ripristinato `"api_prefix": "/api"`
- environment aggiunto: `"environment": "local"`

## Test finali

- `/health`: `200`
- `/api/products`: `200`
- `/api/products/P001`: `200`
- `/api/products/XXX`: `404`
- browser: funzionante

## Git

- branch: `fix/ud08-api-prefix`
- `git diff` verificato: si
- commit: `fix: restore API contract and mark local environment`
- push: si
- PR: #1
- `gh pr diff` verificato: sì, la PR conteneva soltanto la modifica a `app/catalogo-prodotti/config.json`, con `api_prefix` impostato a `/api` e l'aggiunta di `"environment": "local"`
- merge: `Squash and merge`
- main sincronizzata: si

## Distinzione

- **review collaborativa**: in una PR collaborativa la modifica viene controllata da una persona diversa dall'autore. Il reviewer legge il diff, verifica che la modifica rispetti il requisito e che non ci siano problemi o cose mancanti. In quel caso può richiedere correzioni tramite `Request changes`. Dopo le modifiche del contributor, il reviewer controlla nuovamente la PR e può approvarla prima del merge.
- **auto-verifica PR individuale**: in una PR individuale autore e revisore coincidono, quindi non si tratta di una review indipendente. Prima del merge ho eseguito `gh pr diff` per controllare direttamente il contenuto della PR, verificando che fosse modificato soltanto `app/catalogo-prodotti/config.json`, che `api_prefix` fosse correttamente impostato a `/api`, che fosse presente `"environment": "local"` e che non fossero presenti file estranei o dati sensibili.