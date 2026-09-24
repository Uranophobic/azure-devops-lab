# UD13 — Consegna LAB autonomo

- branch:  `fix/ud13-pipeline-path`
- path errato: `infra/bicep/delivery-NON-ESISTE.bicep`
- stage: `Validate Iac`
- job: `Validate Terraform and Bicep`
- step:  `Bicep lint`
- messaggio:  `ERROR: An error occurred reading file. Could not find file '/home/alessia/azdo-agent/_work/1/s/infra/bicep/delivery-NON-ESISTE.bicep'.`
- classe del problema: l'errore non riguarda azure, non riguarda il pool, non riguarda l'agent, non rigurdare l'autenticazione git hub e ne tanto meno la service connection di azure. l'errore riguarda il filesystem / path del repository (metti un altra po' di spiegazione), in particolare dai log si evinche che prima sono andati a buon fine i checkout del repository, l'identificazione del self hosted agenti, il terraform fmt, init e validate, il fallimento arriva solo quando bicep prova a leggere un file che non esiste 
- correzione: correggere il path e impostarlo nuovamente a `infra/bicep/delivery.bicep`
- run finale: `Build succeeded`
- PR/merge: `Fix/ud13 pipeline path`
