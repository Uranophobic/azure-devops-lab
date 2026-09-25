# UD15 — Consegna LAB autonomo

- branch errore: fix/ud15-target-port
- target port errato: `targetPort: '9999'`
- differenza nel Terraform plan: modifica della configurazione ingress della Container App con `targetPort` da `8000` a `9999`
- IaC: completato
- Test: completato
- BuildPush: completato
- Deploy: completato
- Smoke: fallito
- evidenza ingress: `targetPort` configurata a `9999`
- evidenza log: l'applicazione risulta avviata e in ascolto sulla porta `8000`
- causa: mismatch tra la porta configurata nell'ingress (`9999`) e la porta utilizzata dall'applicazione (`8000`)
- correzione minima: ripristinare `targetPort: '8000'`
- branch correzione: `fix/ud15-target-port-correct`
- run finale: Build ID `27`
- smoke finale: completato, endpoint raggiungibile dopo il ripristino di `targetPort: '8000'`