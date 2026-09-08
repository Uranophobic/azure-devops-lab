# Consegna UD02 — Domande sui concetti

Per ciascuna domanda 1–8 riporta una risposta motivata.

## 1.
Una **macchina virtuale Azure** fornisce principalmente l'infrastruttura, mentre il cliente deve occuparsi della gestione del sistema operativo, degli aggiornamenti, delle configurazioni, del software installato e di buona parte della sicurezza.
Con **Azure App Service**, invece, Azure gestisce gran parte dell'infrastruttura e del sistema operativo sottostante. Il cliente può quindi concentrarsi maggiormente sull'applicazione.

## 2.
Il **tenant** rappresenta l'ambiente organizzativo e di identità associato a Microsoft Entra ID, all'interno del quale vengono gestiti utenti, gruppi e accessi.
La **sottoscrizione** rappresenta un confine amministrativo e di fatturazione per le risorse Azure. A una sottoscrizione sono inoltre associati limiti e quote di utilizzo dei servizi.
Il **resource group** è un contenitore logico interno a una sottoscrizione, utilizzato per organizzare e gestire risorse correlate.

## 3.
La località scelta per il resource group indica principalmente dove Azure conserva i metadati relativi al resource group.
Le singole risorse contenute al suo interno possono invece essere create in region differenti.

## 4.
Una **region** è un'area geografica Azure che contiene uno o più datacenter.
Una **Availability Zone** è invece una zona fisicamente separata all'interno di una region, costituita da uno o più datacenter con alimentazione, raffreddamento e rete indipendenti.

## 5.
Il comando:

`az account show`

permette di verificare con quale account e soprattutto con quale sottoscrizione Azure CLI sta lavorando.
Questo controllo è importante prima della creazione di una risorsa perché evita di creare accidentalmente risorse nella sottoscrizione sbagliata, con possibili problemi di costi, autorizzazioni o organizzazione delle risorse.

## 6.
I tag sono coppie chiave-valore utilizzate principalmente per classificare, organizzare e identificare le risorse Azure.
Non devono essere usati come meccanismo di sicurezza perché non controllano l'accesso alle risorse e possono essere modificati o rimossi dagli utenti che dispongono dei relativi permessi sulla risorsa.
Inoltre, non è possibile proteggere in modo dettagliato un singolo tag indipendentemente dal resto della risorsa.

## 7.
Azure CLI permette di gestire le risorse tramite comandi testuali ripetibili e automatizzabili.

## 8.
L'esecuzione del comando di eliminazione indica solamente che è stata richiesta l'eliminazione della risorsa.
Per verificare che il cleanup sia realmente concluso bisogna effettuare un controllo successivo, ad esempio con:

`az group exists --name NOME_RESOURCE_GROUP`
Se il comando restituisce false, significa che il resource group non esiste più e il cleanup può considerarsi concluso.
