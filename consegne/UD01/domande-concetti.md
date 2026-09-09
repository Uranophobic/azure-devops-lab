# Domande

## Perché `git --version` può restituire due risultati diversi in PowerShell e in Ubuntu?

Perché Windows e Ubuntu sono due ambienti distinti e possono avere installazioni separate di Git, ciascuna con una propria versione. Una differenza di versione non indica necessariamente un problema, purché entrambe siano compatibili e funzionanti.

## Quale comando distingue una distribuzione WSL 1 da una WSL 2?

`wsl --list --verbose`

La colonna `version` indica se la distribuzione utilizza WSL 1 oppure WSL 2.

## Perché conserveremo i progetti in `~/workspace` e non principalmente in `/mnt/c`?

Per lavorare direttamente nel filesystem Linux di WSL, più adatto al workflow successivo con strumenti Linux, Docker e bind mount, evitando di lavorare principalmente sul filesystem Windows montato sotto `/mnt/c`.

## Che differenza c'è fra configurare l'autore di un commit e autenticarsi su GitHub?

Configurare l'autore con `git config user.name` e `git config user.email` stabilisce nome ed e-mail che vengono registrati nei commit. Autenticarsi su GitHub, invece, significa accedere al proprio account e dimostrare la propria identità, così da poter gestire i repository e autorizzare operazioni come `git push`, che altrimenti non potrebbero essere eseguite.

## Perché eseguire `git status` sia prima sia dopo `git add`?

Prima di `git add`, `git status` permette di vedere quali file sono modificati o non tracciati. Dopo `git add`, permette di verificare che siano stati inseriti correttamente nella staging area soltanto i file che si vogliono includere nel prossimo commit.


