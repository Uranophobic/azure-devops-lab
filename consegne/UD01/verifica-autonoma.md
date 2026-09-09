# Verifica autonoma 

L'ambiente di lavoro utilizza Ubuntu 26.04 tramite WSL 2.
Il repository personale è stato mantenuto nel filesystem Linux di WSL, nel percorso "~/workspace/azure-devops-lab", inoltre è collegato al repository su GitHub "https://github.com/Uranophobic/azure-devops-lab.git".

La Working Tree in Git è la cartella che contiene i file del progetto su cui è possibile lavorare e apportare modifiche localmente.
La Staging Area rappresenta la preparazione del commit, ovvero il luogo virtuale in cui vengono inserite, tramite git add, le modifiche che si vogliono includere nel commit successivo.
Il commit locale rappresenta il salvataggio delle modifiche nel repository locale.
Il repository remoto è una versione del progetto salvata e condivisa su un server remoto, nel mio caso GitHub.

Azure CLI è considerata compatibile se az login e az account show funzionano correttamente, indipendentemente dalla versione installata.

Il docente risulta tra i collaboratori su GitHub.

Un possibile errore di contesto consiste nel lavorare sul filesystem Windows invece che nel filesystem Linux di WSL. Il comando pwd permette di riconoscerlo, se il percorso inizia con "/home/..." si sta lavorando nel filesystem Linux di WSL, mentre un percorso che inizia con "/mnt/c/..." indica che si sta lavorando sul disco Windows montato in WSL.


## Autovalutazione

Assegna a ogni capacità `Completato` oppure `Da ripetere`:

| Capacità | Valutazione |
|---|---|
| Distinguo PowerShell dal terminale Ubuntu | `Completato` |
| Verifico che Ubuntu utilizzi WSL 2 | `Completato` |
| Riconosco la radice del repository Git | `Completato` |
| Verifico il remote prima del push | `Completato` |
| Distinguo file non tracciato e file in staging | `Completato` |
| Inserisco in staging soltanto il file richiesto | `Completato` |
| Verifico lo stesso commit in locale e su GitHub | `Completato` |
| Verifico lo stato dell'invito al docente | `Completato` |
| Riconosco ed escludo dati riservati | `Completato` |