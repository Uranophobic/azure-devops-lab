# UD10 — Consegna laboratorio autonomo

## Baseline

- compose ps:  backend `Up (healthy)` e frontend `Up`, con frontend pubblicato su `127.0.0.1:8080->80/tcp`
- health: `HTTP/1.1 200 OK`, con `"status": "ok"`

## Errore

- variabile: `LOW_STOCK_THRESHOLD `
- valore errato:`"not-a-number"`
- stato backend: `dependency failed to start: container catalogo-prodotti-backend-1 exited (1)`
- log/errore: 
```console
File "/app/server.py", line 28, in <module>
backend-1  |     LOW_STOCK_THRESHOLD = read_int_env("LOW_STOCK_THRESHOLD", 5)
backend-1  |   File "/app/server.py", line 23, in read_int_env
backend-1  |     raise ValueError(f"{name} deve essere un intero, ricevuto: {raw!r}") from exc
backend-1  | ValueError: LOW_STOCK_THRESHOLD deve essere un intero, ricevuto: 'not-a-number'
```

## Diagnosi

- Sintomo: Il backend non si avvia correttamente e risulta `Exited (1)`, mentre il frontend rimane attivo.
- Risultato atteso: Il backend dovrebbe essere `running` e raggiungere lo stato `healthy`.
- Evidenza: I log mostrano `ValueError: LOW_STOCK_THRESHOLD deve essere un intero, ricevuto: 'not-a-number'`.
`docker compose config` mostra che `LOW_STOCK_THRESHOLD` è impostato a `not-a-number`.
- Ipotesi: Il backend termina all'avvio perché prova a convertire `LOW_STOCK_THRESHOLD` in un intero.
- Causa: La variabile runtime `LOW_STOCK_THRESHOLD` contiene un valore non numerico nel `compose.yaml`.

## Fix

- modifica minima: cambiare `LOW_STOCK_THRESHOLD` da `"not-a-number"` a `"5"`
- rebuild necessario?: no
- motivazione: l'errore riguardava una variabile di configurazione runtime definita nel `compose.yaml`, non il codice applicativo né il Dockerfile. L'image quindi non doveva essere ricostruita: era sufficiente ricreare il container con la configurazione corretta tramite `docker compose up -d`.

## Test

- backend healthy: sì, dopo il fix il backend è tornato Up (healthy)
- health: `HTTP/1.1 200` OK, con `"status": "ok"`
- products: API raggiungibile correttamente e restituisce `"count": 4`
- browser: frontend raggiungibile correttamente su `http://127.0.0.1:8080/`

## Modifica conservata

- APP_ENV: `APP_ENV="local-docker"`
- compose config: `APP_ENV` presente nell'environment del backend
- test: `HTTP/1.1 200` OK sull'endpoint `/health`; verificato inoltre nel container con `docker compose exec backend env | grep APP_ENV`, ottenendo `APP_ENV=local-docker`

## Git

- branch: `fix/ud10-invalid-threshold`
- commit: fix: `validate Docker runtime configuration (#2)`
- PR: `(#2)`
- diff verificato: si
- merge: si, tramite squash merge con eliminazione del branch locale e remoto


## Passaggio alla verifica

- stack mantenuto attivo: sì
