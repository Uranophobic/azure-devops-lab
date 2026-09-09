# Verifica — Preparazione dell'ambiente e metodo di lavoro

## Parte A — Scelte operative

### 1
B. Windows e Ubuntu possono possedere installazioni distinte di Git.

### 2
C. `wsl --list --verbose`

### 3
C. Per lavorare nel file system Linux, più adatto al successivo workflow con Docker e bind mount.

### 4
C. Verificarne il funzionamento e proseguire senza reinstallazione. 

### 5
A. Il commit esiste localmente, ma non è ancora stato eseguito `git push`. 

### 6
A. `git status` dopo `git add`

### 7
C. Usarlo nella pagina di autenticazione e non conservarlo nel repository. 

### 8
B. L'indicatore `WSL: Ubuntu` e un terminale con percorso Linux.

### 9
C. Per partecipare alle attività di scrittura, revisione e collaborazione previste dal percorso.

## Parte B — Risposte brevi

### 10
Git è un sistema di versionamento distribuito per il controllo delle modifiche al codice e ai file di un progetto. Permette di tracciare e gestire le modifiche effettuate in locale e tramite operazioni di commit e push portarle verso un repository remoto.
GitHub è una piattaforma online che ospita e permette di gestire repository remoti, consentendo di condividerli con altri collaboratori e di sincronizzare il lavoro tramite operazioni di push e pull.

### 11
git status                  # Mostra lo stato del repository e i file modificati
git diff                    # Mostra nel dettaglio le modifiche non ancora preparate
git add nomefile            # Aggiunge i file specificati all'area di staging
git status                  # Verifica che il file sia pronto per il commit
git commit -m "Messaggio"   # Crea un commit con le modifiche preparate
git push                    # Invia il commit al repository remoto

### 12
Prima di eseguire `wsl --update` bisogna verificare quale versione di WSL sta utilizzando la distribuzione.
Con `wsl --list --verbose` si controlla se Ubuntu sta utilizzando WSL 1 o WSL 2.
Inoltre, con `wsl --version` si verifica la versione di WSL attualmente installata, così da valutare se l’aggiornamento è realmente necessario.

### 13
Sospetto che Visual Studio Code sia stato aperto in Windows anziché nell’ambiente WSL. Per verificarlo controllerei che in basso a sinistra sia presente l’indicatore `WSL: Ubuntu` e, nel terminale integrato, eseguirei `pwd` per verificare che il percorso sia Linux, ad esempio `/home/.../workspace`.

### 14
Il problema non è risolto perché il token resta visibile nella history dei commit quindi essere ancora recuperato. La soluzione più efficace è quindi revocare o rigenerare immediatamente il token e, se necessario, rimuovere il precendete commit dalla cronologia del repository.
