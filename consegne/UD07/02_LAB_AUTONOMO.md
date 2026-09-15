# UD07 — Consegna laboratorio autonomo

## 1. Script CLI idempotente

- logica: verificare se il Resource Group esiste; se non esiste, crearlo, mentre se esiste, riutilizzarlo. Successivamente applicare i tag `ManagedBy=Autonomo` e `UD=07` e infine mostrare nome, location e provisioning state.
- prima esecuzione: il Resource Group non esisteva, quindi lo script ha creato `rg-ud07-auto`.
- seconda esecuzione: il Resource Group esisteva già, quindi lo script lo ha riutilizzato senza crearne uno nuovo.
- verifica: tramite la query

```bash
az group show \
  --name rg-ud07-auto \
  --query "{Name:name,Location:location,State:properties.provisioningState,ManagedBy:tags.ManagedBy,UD:tags.UD}" \
  --output table
```

output:

| Name | Location | State | ManagedBy | UD |
|---|---|---|---|---|
| rg-ud07-auto | westeurope | Succeeded | Autonomo | 07 |

## 2. PowerShell equivalente

- controllo esistenza: il Resource Group viene cercato tramite `Get-AzResourceGroup -Name $RgName -ErrorAction SilentlyContinue`. Se `$rg` non contiene un oggetto, il controllo `if (-not $rg)` consente di crearlo; se invece esiste già, viene riutilizzato.
- modifica: tramite `Update-AzTag` sono stati applicati i tag `ManagedBy=Autonomo` e `UD=07`. Alla seconda esecuzione il Resource Group già esistente è stato riutilizzato, mantenendo lo stesso stato desiderato.
- output:

```text
ResourceGroupName   Location    ProvisioningState   Tags
rg-ud07-auto-ps     westeurope  Succeeded           {[UD, 07], [ManagedBy, Autonomo]}
```

## 3. Activity Log

- operazione: `Update resource group`
- status: `Succeeded`
- timestamp: `2026-09-15T15:46:49.6379015Z`

## 4. KQL

1. La query produce `2` righe aggregate, una per lo stato `WARN` e una per lo stato `OK`.
2. Lo stato con la latenza media più alta è `WARN`, con una durata media di `470 ms`, mentre `OK` ha una durata media di `150 ms`.
3. `summarize` cambia la granularità dei dati perché raggruppa più record originali in base a un criterio, in questo caso `Status`, e restituisce una riga aggregata per ogni gruppo. I quattro record iniziali vengono quindi trasformati in due righe, contenenti il numero di richieste e la durata media per ciascuno stato.

## 5. Metrics

- metrica scelta: `Transactions`
- unità: `Count`
- aggregazione primaria: `Total`
- punto dati disponibile: sì
- valore osservato: `0.0`
- interpretazione: nel periodo osservato la metrica `Transactions` ha restituito un campione con valore totale `0.0`, quindi nello specifico intervallo non risultano transazioni conteggiate.

## 6. Alert

- scope: Storage Account `stud0789474931`
- condition: metrica `Transactions`, aggregazione `Total`, operatore `GreaterThan`, soglia `0`
- severity: `3`
- evaluation frequency: `PT5M`, quindi ogni 5 minuti
- Action Group: presente, `ag-ud07`
- Enabled vs Fired: `Enabled` indica che la regola di alert è attiva e viene valutata da Azure. `Fired` indica invece che, durante una valutazione, la condizione configurata è stata effettivamente soddisfatta. Nel campione osservato `Transactions` aveva valore `0.0`, mentre la condizione richiede un valore maggiore di `0`, quindi quel campione non soddisfaceva la condizione dell'alert.

## 7. Guasto amministrativo

- sintomo: il comando `az group show` non riesce a recuperare il Resource Group `rg-ud07-NON-ESISTE`.
- errore: `ResourceGroupNotFound`.
- ipotesi: il nome del Resource Group potrebbe essere errato oppure il Resource Group potrebbe non esistere nella subscription attualmente selezionata.
- controllo: è stata verificata la subscription corrente, risultata `Enabled`. Successivamente, con `az group exists`, è stato verificato che `rg-ud07-NON-ESISTE` restituisce `false`, mentre `rg-ud07-auto` restituisce `true`.
- correzione: utilizzare il nome corretto del Resource Group esistente, senza creare `rg-ud07-NON-ESISTE`.
- verifica: ripetendo `az group show` con `rg-ud07-auto`, il Resource Group viene trovato correttamente e risulta in stato `Succeeded`.

## 8. Runbook

RUNBOOK — Risorsa Azure non trovata o contesto errato

### Sintomo

Un comando Azure CLI restituisce un errore come `ResourceGroupNotFound` e non riesce a recuperare la risorsa richiesta.
Il comando eseguito sul Resource Group `rg-ud07-NON-ESISTE` ha restituito:

```text
ResourceGroupNotFound
Resource group 'rg-ud07-NON-ESISTE' could not be found.
```

### Contesto

Prima di concludere che una risorsa non esiste, bisogna verificare che Azure CLI stia lavorando nella subscription prevista e che il nome o l'identificativo della risorsa siano corretti.
La subscription corrente risultava attiva con stato `Enabled`.

### Controlli

I controlli eseguiti sono stati:

- verifica della subscription corrente;
- verifica dell'esistenza del Resource Group indicato;
- confronto con un Resource Group che sappiamo essere esistente;
- verifica finale tramite `az group show`.

`rg-ud07-NON-ESISTE` ha restituito `false`, mentre `rg-ud07-auto` ha restituito `true`.

### Comandi

Verifica della subscription:

```bash
az account show \
  --query "{Subscription:name,State:state}" \
  --output table
```

Verifica del Resource Group inesistente:

```bash
az group exists \
  --name rg-ud07-NON-ESISTE
```

Verifica del Resource Group corretto:

```bash
az group exists \
  --name rg-ud07-auto
```

Verifica finale:

```bash
az group show \
  --name rg-ud07-auto \
  --query "{Name:name,Location:location,State:properties.provisioningState}" \
  --output table
```

### Interpretazione

L'errore `ResourceGroupNotFound` era dovuto al fatto che `rg-ud07-NON-ESISTE` non esisteva nella subscription corrente.

La subscription era corretta e risultava `Enabled`, quindi il problema non era il contesto Azure ma il nome della risorsa utilizzato nel comando.

### Correzione minima

La correzione minima consiste nell'utilizzare il nome corretto del Resource Group esistente, senza creare una nuova risorsa soltanto per eliminare l'errore. Quindi è stato utilizzato `rg-ud07-auto`.

### Verifica

Dopo la correzione, `az group show` ha restituito correttamente:

- nome: `rg-ud07-auto`
- location: `westeurope`
- provisioning state: `Succeeded`

La verifica conferma quindi che il comando funziona correttamente utilizzando il Resource Group esistente.

## 9. Cleanup

- `rg-ud07-auto`:
- `rg-ud07-auto-ps`: 