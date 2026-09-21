# UD11 — Consegna laboratorio autonomo

## Baseline

- status: `ok`
- version: `v2`
- target port: `8000`

## Errore

- nuovo target port: `9999`
- sintomo: l'ingress è raggiungibile, ma non riesce a collegarsi correttamente al backend sulla porta configurata; il traffico arriva alla piattaforma, ma l'inoltro verso il container fallisce.
- HTTP/timeout: `HTTP 503 - upstream connect error / connection refused`

## Evidenze

- running status: `Running`
- revision: attva 
- health: `Healthy`
- log listen port: backend `v2` in ascolto su `http://0.0.0.0:8000`
- ingress target port: `9999`

## Diagnosi

- Sintomo: richiesta al FQDN restituisce HTTP `503` con `connection refused`.
- Risultato atteso: endpoint `/health` raggiungibile con `status: ok` e `version: v2`.
- Evidenza: i log mostrano che il backend `v2` è avviato e ascolta sulla porta `8000`, mentre l'ingress è configurato con target port `9999`.
- Ipotesi: l'ingress inoltra il traffico verso una porta sulla quale il backend non è in ascolto.
- Causa: mismatch tra target port dell'ingress (`9999`) e porta di ascolto del backend (`8000`).
- Correzione minima: riportare la target port dell'ingress a `8000`.

## Verifica

- target port: `8000`
- health: `status: ok`
- version: `v2`

## Domande

1. No, perché il problema non riguardava il contenuto dell'immagine. Il backend `v2` era già avviato correttamente e ascoltava sulla porta `8000`.
2. No, perché non era necessario modificare l'applicazione. Il problema dipendeva solo dalla configurazione dell'ingress.
3. Il problema era nell'ingress, perché la target port era stata impostata a `9999`, mentre il backend ascoltava sulla porta `8000`.
4. I log mostravano che il backend `v2` era correttamente in ascolto sulla porta `8000`, mentre la configurazione dell'ingress mostrava `targetPort = 9999`. Il confronto tra questi due valori ha identificato il mismatch.
5. Perché avrebbe reso più difficile capire quale modifica avesse realmente risolto il problema. Nel troubleshooting è preferibile modificare una sola variabile alla volta, verificare il risultato e mantenere chiaro il rapporto tra causa e correzione.



## Passaggio alla verifica

- target port ripristinato a 8000: si
- health v2 nuovamente OK: si
- Resource Group mantenuto disponibile: sì
