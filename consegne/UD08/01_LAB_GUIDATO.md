# UD08 — Consegna laboratorio guidato

## Repository

- repository: con il mio account principale ho creato il repo `ud08_azure-lab`
- branch principale: main
- `gh auth status` verificato: sì, account `alessia-crispo2`
- working tree iniziale pulito: sì

## Collaborazione ricevuta sul mio repository

- collaborator: alessia-crispo2
- invito accettato: si
- branch contributor: `feature/ud08-collab-alessia-crispo2`
- numero PR: #1
- URL PR: https://github.com/Uranophobic/ud08_azure-lab/pull/1
- prima review: `Request changes`
- modifica richiesta: aggiungere la sezione `## Esito` con la frase `Review completata e modifica corretta.`
- nuovo commit: `docs: address review feedback`
- approvazione/commento finale: `Approve`,  UD08: collaboration evidence (#1)
- merge: `Squash and merge`
- branch remota eliminata: si
- collaboratore rimosso: si

## Collaborazione eseguita sul repository altrui

- repository: con il mio account secondario ho creato il repo `ud08_B-azure-lab`
- branch:  `feature/ud08-collab-Uranophobic`
- numero PR: #1
- prima review ricevuta: `Request changes`
- correzione eseguita: aggiunta la sezione `## Esito`
- merge completato: si

## Conflitto locale

- branch A: `lab/conflict-a`
- branch B: `lab/conflict-b`
- file: `ud08-conflict.txt`
- marker osservati: `<<<<<<< HEAD`, `=======`, `>>>>>>> lab/conflict-a`
- contenuto finale scelto: `PORT=8000`
- commit risoluzione: `lab: resolve port conflict`
- cleanup completato: sì; eliminato `ud08-conflict.txt` da `main` ed eliminate le branch locali `lab/conflict-a` e `lab/conflict-b`

## Catalogo prodotti

- server avviato: sì, con `python3 server.py` su `http://127.0.0.1:8000`
- `/health`: `HTTP 200 OK`, stato `"ok"`
- `/api/products`: `HTTP 200`, restituiti 4 prodotti
- `/api/products/P001`: `HTTP 200 OK`, prodotto `Notebook Pro 14`
- `/api/products/XXX`: `HTTP 404 Not Found`, errore `product_not_found`
- frontend browser: verificato correttamente su `http://127.0.0.1:8000/`
- prodotti restituiti: 4
- prodotto LOW: `P002 - Monitor 27 UHD` e `P004 - Keyboard Business`

## Architettura

- frontend: `static/index.html`
- backend/API: `server.py`
- configurazione: `config.json`
- dati: `data/products.json`
- endpoint:
  - `GET /`
  - `GET /health`
  - `GET /api/products`
  - `GET /api/products/<id>`

## Baseline Git
- commit: `feat: add local product catalog`
- push/PR: push diretto su `main`
- stato finale main: sincronizzata con `origin/main`
