# UD06 — Consegna laboratorio autonomo

## 1. Baseline

- VM: creata durante il laboratorio guidato, `vm-ud06-linux`
- Nginx: servizio attivo e funzionante
- HTTP: richiesta esterna verso `172.213.209.65` riuscita con `HTTP/1.1 200 OK`
- regola NSG: `Allow-HTTP-MyIP`, TCP porta `80`, sorgente `My IP`, azione `Allow`

| Priorità | Nome | Porta | Protocollo | Sorgente | Azione |
|---|---|---:|---|---|---|
| 200 | `Deny-HTTP-Auto` | 80 | TCP | My IP | Deny |
| 300 | `Allow-SSH-MyIP` | 22 | TCP | My IP | Allow |
| 310 | `Allow-HTTP-MyIP` | 80 | TCP | My IP | Allow |
Successivamente è stata aggiunta la regola `Deny-HTTP-Auto` per simulare il guasto controllato.


## 2–4. Guasto, diagnosi, ripristino

- regola introdotta: `Deny-HTTP-Auto`, priorità `200`, TCP porta `80`, sorgente `My IP`, azione `Deny`
- sintomo: la richiesta HTTP esterna verso il Public IP della VM fallisce e `curl` non riesce a connettersi alla porta `80`
- ipotesi: una regola NSG sta bloccando il traffico HTTP in ingresso
- controllo: verifica delle regole NSG, `IP Flow Verify`, controllo dello stato di Nginx con `systemctl status nginx --no-pager` e test locale con `curl -I http://localhost`
- causa: `Deny-HTTP-Auto` ha priorità `200` e viene quindi valutata prima di `Allow-HTTP-MyIP`, che ha priorità `310`
- correzione minima: eliminare la sola regola `Deny-HTTP-Auto`
- verifica: dopo la rimozione della regola, `curl -I "http://$LAB_VM_IP"` restituisce nuovamente `HTTP/1.1 200 OK`

## 5. Monitoring

- metrica: `Percentage CPU`
- intervallo: ultimi `30 minuti`
- aggregazione: `Average`
- deduzione: l'utilizzo della CPU è rimasto molto basso e quasi stabile, intorno allo `0,2-0,3%`, con un breve picco vicino all'`1,2%`. La VM non presenta quindi un carico CPU significativo
- cosa non posso dedurre: la metrica può evidenziare un possibile problema, ma osservando la sola CPU non posso individuarne la causa precisa, né stabilire se dipenda dall'applicazione, dalla rete o da un processo.

## 6. VMSS Autoscale

- min: `1`
- default: `1`
- max: `4`
- metrica: `Percentage CPU`
- condizione: CPU media `> 70%` per `5 minuti`
- azione: `scale out` di `1` istanza
- perché max=4: impedisce una crescita incontrollata del numero di istanze, mantenendo sotto controllo sia l'utilizzo delle risorse sia i costi

## 7. App Service scaling

- A:
- B:
- C:
- D:

## 8. Backup policy

- frequenza: giornaliera
- orario: `02:00`
- retention: `7 giorni`
- motivazione: la frequenza giornaliera permette di avere un punto di ripristino aggiornato ogni giorno; le `02:00` sono state scelte come fascia di basso utilizzo, così il backup incide meno sul workload; una retention di `7 giorni` consente di mantenere una settimana di punti di ripristino recenti senza conservare copie per un periodo eccessivo.

## 9. HA / Backup / DR

- A:
- B:
- C:

## 10. RPO / RTO

- RPO:
- RTO: