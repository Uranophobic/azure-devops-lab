# UD13 — Consegna LAB autonomo

- branch:  `fix/ud13-pipeline-path`
- path errato: `infra/bicep/delivery-NON-ESISTE.bicep`
- stage: `Validate Iac`
- job: `Validate Terraform and Bicep`
- step:  `Bicep lint`
- messaggio:  `ERROR: An error occurred reading file. Could not find file '/home/alessia/azdo-agent/_work/1/s/infra/bicep/delivery-NON-ESISTE.bicep'.`
- classe del problema: l'errore non riguarda Azure, non riguarda il pool, non riguarda l'Agent, non riguarda l'autenticazione GitHub e nemmeno la Service Connection di Azure. L'errore riguarda il filesystem/path del repository, in particolare, dai log si evince che prima sono andati a buon fine il checkout del repository, l'identificazione del self-hosted Agent e i comandi Terraform fmt, init e validate. Il fallimento arriva solo quando Bicep prova a leggere un file che non esiste. 
- correzione: correggere il path e impostarlo nuovamente a `infra/bicep/delivery.bicep`
- run finale: `Build succeeded`
- PR/merge: `Fix/ud13 pipeline path`
