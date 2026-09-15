# UD07 — Consegna laboratorio guidato

## CLI

- prima esecuzione script: il Resource Group non esisteva, quindi lo script lo ha creato.
- seconda esecuzione: il Resource Group esisteva già, quindi lo script lo ha riutilizzato.
- comportamento idempotente: la procedura verifica prima se la risorsa esiste; se non esiste la crea, mentre se esiste la riutilizza. Questa è l'idempotenza: ripetere l'esecuzione dello script non moltiplica le risorse e porta sempre allo stesso stato desiderato.
- esempio JMESPath: `--query "{Name:name,Location:location,Provisioning:properties.provisioningState}"`, utilizzato per selezionare dall'output soltanto il nome, la regione e lo stato di provisioning del Resource Group.
- quando usare `tsv`: lanciando

    az group show \
      --name "$LAB_RG" \
      --query name \
      --output tsv

  si ottiene il valore puro, in questo caso il nome del Resource Group, senza la struttura e le virgolette proprie dell'output JSON. È quindi utile quando il valore deve essere riutilizzato in altri comandi o salvato in una variabile.

## PowerShell

- `Get-AzContext` verificato: sì.
- Resource Group test: `rg-ud07-ps-test`.
- prima esecuzione: alla prima esecuzione, non essendo presente `rg-ud07-ps-test`, il Resource Group è stato creato.
- seconda esecuzione: il Resource Group esistente è stato recuperato e riutilizzato e i tag sono stati riportati allo stesso stato desiderato, applicando il principio di idempotenza.
- perché il controllo `if` è utile: alla seconda esecuzione `$rg` contiene già l'oggetto Resource Group, quindi `if (-not $rg)` restituisce `false` e il blocco di creazione non viene eseguito. Il controllo evita quindi di tentare inutilmente una nuova creazione della risorsa.

## Log Analytics

- workspace: `law-ud07-419`.
- regione: `northeurope`.
- stato: `Succeeded`.
- scelta della regione: inizialmente ho provato a creare il workspace in `westeurope`, ma Azure ha restituito `RequestDisallowedByAzure`, indicando che la regione non accettava nuove creazioni per la mia subscription. Ho quindi utilizzato `northeurope`, dove la creazione è andata a buon fine.
- query `print`:

```bash
az monitor log-analytics query \
  --workspace "$LAW_CUSTOMER_ID" \
  --analytics-query "print Course='AZ-104', UD=7, Status='OK'" \
  --output table
```

| Course | Status | TableName | UD |
|---|---|---|---|
| AZ-104 | OK | PrimaryResult | 7 |

- query `datatable`:

```bash
az monitor log-analytics query \
  --workspace "$LAW_CUSTOMER_ID" \
  --analytics-query "datatable(Component:string,Status:string)[
    'API','OK',
    'DB','WARN',
    'WEB','OK'
  ]
  | summarize Count=count() by Status" \
  --output table
```

| Count | Status | TableName |
|---|---|---|
| 2 | OK | PrimaryResult |
| 1 | WARN | PrimaryResult |

La query raggruppa i record in base al valore di `Status` e conta quanti record appartengono a ciascun gruppo.

## Activity Log

- evento osservato: `Update resource group`.
- status: `Succeeded`.
- timestamp: `2026-09-15T13:08:16.7419489Z`.
- dati personali omessi: sì.

L'evento corrisponde alla modifica effettuata sul Resource Group tramite l'aggiunta del tag `LastChange=UD07`. Il timestamp riportato dall'Activity Log è espresso in UTC.

## Diagnostic Setting

- nome: `ud07-activity-to-law`.
- esito: Diagnostic Setting creata correttamente.
- destinazione: Log Analytics Workspace `law-ud07-419`.
- categorie abilitate: `Administrative`, `ServiceHealth`, `Alert`, `Policy`, `ResourceHealth`.
- AzureActivity disponibile: sì.
- risultato query: al primo tentativo la tabella `AzureActivity` non restituiva ancora risultati, probabilmente perché la Diagnostic Setting era stata configurata da poco e i dati non erano ancora stati inseriti nel workspace. Ripetendo la query dopo alcune ore sono comparsi diversi risultati, tra cui operazioni relative alla registrazione di `Microsoft.Insights`, alla scrittura della Diagnostic Setting, alla creazione/aggiornamento dello Storage Account, dell'Action Group e della Metric Alert Rule.
- fallback usato, se necessario: nessuno.

## Metrics

- Storage Account: `stud0789474931`.
- regione: `northeurope`.
- stato: `Succeeded`.
- metrica: `UsedCapacity`.
- unità: `Bytes`.
- aggregazione: `Average`.
- punto dati disponibile: no.
- interpretazione: la metrica risulta supportata dallo Storage Account, ma al momento dell'interrogazione il campione numerico non era ancora disponibile.

È stata inoltre verificata la metrica `Transactions`:

- unità: `Count`.
- aggregazione primaria: `Total`.

## Alert

- nome: `alert-ud07-storage-transactions`
- scope corretto: sì, Storage Account `stud0789474931`
- condition: segnale `Transactions`, aggregazione `Total`, operatore `Greater than`, soglia `0`, lookback period `5 minutes`, check every `5 minutes`
- severity: `3 - Informational`
- Action Group: presente, `ag-ud07`
- perché non è necessario che sia Fired: perché è sufficiente verificare che la regola sia stata creata correttamente, sia abilitata e controlli la metrica `Transactions` con la condizione prevista.

## Correlazione

- modifica osservata: aggiornamento dello Storage Account tramite il tag `State=Changed`.
- evento Activity Log: `MICROSOFT.STORAGE/STORAGEACCOUNTS/WRITE`.
- la correlazione prova causalità?: no.
- motivazione: la presenza dell'evento dimostra che la modifica allo Storage Account è avvenuta ed è stata registrata da Azure, ma non dimostra automaticamente che un eventuale cambiamento osservato nelle metriche sia stato causato da quella modifica. La vicinanza temporale tra due eventi indica una correlazione; per dimostrare la causalità servirebbero ulteriori evidenze.


## Cleanup

- diagnostic setting rimossa:
- RG CLI test eliminato:
- RG PowerShell test eliminato:
- RG principale eliminato: