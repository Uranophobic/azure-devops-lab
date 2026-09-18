# UD10 — Consegna laboratorio guidato

## Preflight

- `docker version`: `29.4.3`
- `docker compose version`: `Docker Compose version v5.1.4`
- daemon: `Docker Desktop 4.74.0 / Docker Engine 29.4.3 raggiungibile da WSL2`
- WSL2: `Ubuntu`, integrazione Docker Desktop attiva
- RESULT: `OK`

## Build backend
Ho costruito l'image del backend a partire dal `docker/backend.Dockerfile`. 

- image: `catalog-backend:ud10`
- tag: `ud10`
- build context: `app/catalogo-prodotti` (`.`)
- Dockerfile: `docker/backend.Dockerfile`
- layer/history osservati: riconosciuti `WORKDIR`, `COPY`, `RUN`, `USER`, `ENV`, `EXPOSE` e `CMD`. Sono inoltre presenti i layer e le configurazioni ereditati dall'image base `python:3.13-slim`.

## Container singolo
Ho verificato il funzionamento del backend avviando un singolo container dall'image `catalog-backend:ud10` e pubblicando la porta 8000 sull'host. I test dell'endpoint /health e dell'API dei prodotti hanno restituito i risultati attesi.

- nome: `catalog-backend-ud10`
- porta: con il comando `docker inspect catalog-backend-ud10 \ --format 'Ports={{json .NetworkSettings.Ports}}'`, verifichiamo come docker ha configurato le porte, ottenendo: 
 `Ports={"8000/tcp":[{"HostIp":"127.0.0.1","HostPort":"8000"}]}`
- health: con `curl -i http://127.0.0.1:8000/health `ho verificato lo stato e l'output è stato positivo con:
```bash
HTTP/1.0 200 OK
Server: CatalogBackend/2.0 Python/3.13.15
Date: Fri, 18 Sep 2026 10:02:47 GMT
Content-Type: application/json; charset=utf-8
Content-Length: 72

{
  "status": "ok",
  "service": "catalog-backend",
  "version": "2.0"
}
```
- products: ho verificato che l'API restituisse correttamente i quattro prodotti previsti tramite `curl -s http://127.0.0.1:8000/api/products | python3 -m json.tool`.
```bash
{
    "count": 4,
    "products": [
        {
            "id": "P001",
            "name": "Notebook Pro 14",
            "category": "Notebook",
            "price": 1299.0,
            "stock": 8,
            "stock_status": "OK"
        },
        {
            "id": "P002",
            "name": "Monitor 27 UHD",
            "category": "Monitor",
            "price": 349.0,
            "stock": 4,
            "stock_status": "LOW"
        },
        {
            "id": "P003",
            "name": "Dock USB-C",
            "category": "Accessori",
            "price": 119.0,
            "stock": 15,
            "stock_status": "OK"
        },
        {
            "id": "P004",
            "name": "Keyboard Business",
            "category": "Accessori",
            "price": 59.0,
            "stock": 2,
            "stock_status": "LOW"
        }
    ]
}
```
- log: `Catalog backend listening on http://0.0.0.0:8000 threshold=5 runtime=/runtime`
- environment verificato: si, tramite:
```console
docker inspect catalog-backend-ud10 \
  --format '{{range .Config.Env}}{{println .}}{{end}}' \
  | grep -E 'APP_PORT|LOW_STOCK|RUNTIME_DIR'
```
ottenendo come output: 
```console
LOW_STOCK_THRESHOLD=5
APP_PORT=8000
RUNTIME_DIR=/runtime
```
## Compose
Per avviare backend e frontend ho utilizzato Docker Compose. Il file `compose.yaml` descrive due servizi collegati alla stessa rete Docker: il backend utilizza la porta 8000, il frontend espone il servizio alla porta 8080 e inoltra le richieste verso il backend. Per validare la configurazione ho lanciato il comando `docker compose config`, che ha restituito le seguenti informazioni come output. 

- backend:
```console
  backend:
    build:
      context: /home/alessia/workspace/azure-devops-lab/app/catalogo-prodotti
      dockerfile: docker/backend.Dockerfile
    environment:
      APP_PORT: "8000"
      LOW_STOCK_THRESHOLD: "5"
      RUNTIME_DIR: /runtime
    expose:
      - "8000"
    healthcheck:
      test:
        - CMD
        - python
        - -c
        - import urllib.request; urllib.request.urlopen('http://127.0.0.1:8000/health', timeout=2)
      timeout: 3s
      interval: 5s
      retries: 10
      start_period: 3s
    image: catalog-backend:ud10
    networks:
      catalog-net: null
    volumes:
      - type: volume
        source: catalog-runtime
        target: /runtime
        volume: {}
```
- frontend:
```console
   frontend:
    build:
      context: /home/alessia/workspace/azure-devops-lab/app/catalogo-prodotti
      dockerfile: docker/frontend.Dockerfile
    depends_on:
      backend:
        condition: service_healthy
        required: true
    image: catalog-frontend:ud10
    networks:
      catalog-net: null
    ports:
      - mode: ingress
        host_ip: 127.0.0.1
        target: 80
        published: "8080"
        protocol: tcp
```
- rete:
```markdown
networks:
  catalog-net:
    name: catalogo-prodotti_catalog-net
    driver: bridge
```
- volume:
```console
volumes:
  catalog-runtime:
    name: catalogo-prodotti_catalog-runtime
```
- frontend URL: `http://127.0.0.1:8080/`. La porta `8080` dell'host viene mappata sulla porta `80` del container frontend. Il client accede quindi al frontend Nginx, che inoltra al backend le richieste destinate alle API.
- backend healthy: il backend dispone di un healthcheck che interroga periodicamente `http://127.0.0.1:8000/health` dall'interno del container, come specificato nel file di configurazione. Lo stato `healthy` indica quindi non soltanto che il processo è in esecuzione, ma anche che l'applicazione risponde correttamente al controllo configurato.
- `/health`: `HTTP/1.1 200 OK`
- `/api/products`: `"count": 4`
- P001: `HTTP/1.1 200 OK`
- XXX: `HTTP/1.1 404 Not Found`

## Networking

- `backend` risolto dal frontend: `172.18.0.2 backend backend`

- `localhost:8000` dal frontend: `Connection refused`

- spiegazione: Tramite `docker compose exec frontend getent hosts backend` chiediamo al container frontend di risolvere il nome `backend` e otteniamo l'indirizzo IP interno associato al servizio, grazie al DNS interno di Docker. Provando invece a raggiungere `localhost:8000` dal frontend otteniamo `Connection refused`, perché `localhost`, all'interno del container frontend, indica il frontend stesso e non il container backend.

## Environment

- threshold iniziale: impostato a 5
- threshold temporaneo: impostato a 10
- effetto osservato: modificando `LOW_STOCK_THRESHOLD` da `5` a `10` nel `compose.yaml`, e il prodotto P001 con stock pari a 8, è passato da `stock_status: "OK"` a `stock_status: "LOW"`. Non è stato necessario ricostruire l'image, perché non sono stati modificati né il codice né il Dockerfile: Compose ha ricreato il container utilizzando la stessa image con la nuova configurazione runtime.
- valore ripristinato: 5

## Volume
Ho richiamato tre volte l'endpoint `/api/counter`, osservando che il contatore aumentava di `1` a ogni richiesta. Successivamente ho eseguito `docker compose down`, che arresta e rimuove i container, le reti e le risorse create in precedenza. Infine ho riavviato l'applicazione con `docker compose up -d`.

Dopo la creazione dei container il contatore è passato da `3` a `4` invece di ripartire da zero. Questo dimostra che il dato viene memorizzato nel named volume `catalog-runtime` e continua quindi a esistere indipendentemente dal ciclo di vita del singolo container.

- counter before: `3`
- counter after recreate: `4`
- persistenza PASS/FAIL: `PASS`
- volume: `catalogo-prodotti_catalog-runtime`

## Lifecycle
Ho verificato i log dei singoli servizi limitando l'output alle ultime 30 righe tramite `--tail 30`. I log del backend mostrano le richieste periodiche dell'healthcheck concluse con risposta `200`, mentre quelli del frontend mostrano il corretto avvio di Nginx e le richieste HTTP ricevute.

Ho inoltre riavviato soltanto il servizio backend con `docker compose restart backend`. Dopo il restart il backend è tornato nello stato `healthy` e l'endpoint `/health` ha continuato a rispondere `200`, dimostrando che è possibile intervenire su un singolo servizio senza ricreare l'intero stack. 

- restart test: restart del backend Container `catalogo-prodotti-backend-1 Restarting`

- log backend: le richieste periodiche a `/health` restituiscono `200`, confermando che il controllo applicativo continua ad avere esito positivo
`backend-1  | [backend] 127.0.0.1 - "GET /health HTTP/1.1" 200 `

- log frontend: dai log si osserva il corretto avvio di Nginx
`frontend-1  | 2026/09/18 10:40:56 [notice] 1#1: start worker process 44`

## Git
 Prima del commit ho verificato con `git diff --cached` che non fossero presenti file runtime, log, `.env`, token o altri dati non necessari. 

- commit:
- push/PR: