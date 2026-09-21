# UD11 — Verifica

## Parte A

1. B
2. B
3. A
4. B
5. A
6. A
7. B
8. B

## Parte B

9. Distingui tag e digest.
Il tag è un'etichetta leggibile associata a un'immagine, ad esempio `v1` o `v2`, e può essere riutilizzato. Il digest, invece, identifica in modo univoco uno specifico contenuto dell'immagine, normalmente tramite un hash `sha256`.

10. Il Container Apps Environment è il contesto infrastrutturale condiviso nel quale possono essere ospitate una o più Container App. La Container App rappresenta invece l'applicazione che vogliamo eseguire e la sua configurazione, come immagine, variabili d'ambiente, ingress, scaling e identità. Una revision è uno snapshot immutabile della configurazione della Container App. Una replica è una concreta istanza runtime di una revision.

11. Perché la Managed Identity permette alla Container App di autenticarsi verso ACR senza dover salvare username, password o altre credenziali statiche.

12. Perché l'immagine utilizzata dalla Container App fa parte della configurazione revision-scope. Quando viene modificata l'immagine, cambia quindi la configurazione dell'applicazione e Azure crea una nuova revision.

13. La target port dell'ingress deve coincidere con la porta sulla quale il backend all'interno del container è realmente in ascolto.

14. Controllerei:
    1. che il FQDN sia corretto;
    2. che l'ingress sia abilitato ed eventualmente external;
    3. che la target port coincida con la porta di ascolto del backend;
    4. che la revision sia attiva e `Healthy`;
    5. che esistano repliche funzionanti;
    6. i log applicativi;
    7. che immagine e tag presenti in ACR siano corretti;
    8. che la Managed Identity abbia i permessi necessari per il pull da ACR.

15. Per comprendere prima quali operazioni vengono eseguite durante build, push e deployment. In questo modo, quando queste attività verranno automatizzate tramite pipeline, sarà più semplice capire cosa sta facendo la pipeline e individuare eventuali errori.

16. Perché il ruolo corretto dipende dal modello di autorizzazione configurato sul registry. In modalità RBAC classica si possono usare ruoli come `AcrPull` e `AcrPush`, mentre con RBAC+ABAC possono essere utilizzati ruoli repository più specifici e condizioni più granulari.

17. UD11 permette di capire manualmente tutto il flusso: test dell'applicazione, `docker build`, push verso ACR, aggiornamento della Container App e verifica dell'endpoint. In UD14 e UD15 queste stesse operazioni verranno progressivamente automatizzate tramite pipeline.

18. La `system-assigned managed identity` viene creata direttamente sulla risorsa e il suo ciclo di vita è legato a essa: se la risorsa viene eliminata, viene eliminata anche l'identità. La `user-assigned managed identity` è invece una risorsa Azure indipendente, che può essere associata e riutilizzata da più risorse.

## Parte C

15. La causa è la `targetPort` errata: l'ingress inoltra il traffico sulla porta `9000`, mentre dai log risulta che il backend è in ascolto sulla porta `8000`. La correzione minima è quindi modificare la target port dell'ingress da `9000` a `8000`.

16. Dopo la correzione verificherei prima che l'ingress abbia effettivamente `targetPort=8000`:

```bash
az containerapp ingress show \
  --name "$ACA_APP" \
  --resource-group "$LAB_RG" \
  --query "{External:external,TargetPort:targetPort}" \
  --output table
```
Successivamente testerei nuovamente l'endpoint /health tramite FQDN:

```bash
curl -fsS "https://$ACA_FQDN/health" \ python3 -m json.tool
```

Mi aspetterei status: `ok` e version: `v2`. Se necessario, controllerei anche che la revision sia ancora `Healthy` e i log applicativi non mostrino errori.

## Cleanup finale

- resource list verificata: sì
- Resource Group eliminato: sì
- `az group exists` = `false`: sì
- immagini Docker locali UD11 rimosse: sì
- prune globale usato: NO


