---
title: Informazioni sui componenti di PAM | Microsoft Identity Manager
description: Privileged Access Management condivide alcuni componenti con MIM e ha alcuni componenti propri. Informazioni sul funzionamento della combinazione di questi componenti.
keywords: 
author: kgremban
manager: femila
ms.date: 07/15/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 6498f68f-36d3-448c-8fe6-649ad5a7f97d
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: ae4c40c73dd9d5860f42e00765a7e34e8ca397a9
ms.openlocfilehash: d17feb5d78b864bef8f0b96bfbf92a18c3c91694


---

# Informazioni sui componenti di PAM

Privileged Access Management mantiene separati l'accesso amministrativo dall'accesso tramite account utente di uso quotidiano. Questa soluzione si basa su foreste parallele:

- *CORP*: foresta aziendale generica che include uno o più domini. Anche se è possibile avere più foreste CORP, per semplicità gli esempi in questi articoli presuppongono l'esistenza di una singola foresta con un singolo dominio.  
- *PRIV*: foresta dedicata creata appositamente per questo scenario PAM. Questa foresta include un dominio per la gestione di privilegi di gruppi e account nascosti da uno o più domini CORP.

La soluzione MIM come configurata per PAM include i componenti seguenti:  

- **Servizio MIM**: implementa la logica di business per le attività di gestione delle identità e degli accessi, tra cui la gestione degli account con privilegi e delle richieste di elevazione dei privilegi.   
- **Portale MIM**: portale di SharePoint, ospitato da SharePoint 2013, che fornisce l'interfaccia utente per la gestione e la configurazione degli amministratori.
- **Database del servizio MIM**: archiviato in SQL Server 2012 o 2014 con i dati e i metadati sull'identità necessari per il servizio MIM.
- **Servizio di monitoraggio PAM** e **Servizio componenti PAM**: due servizi che gestiscono il ciclo di vita degli account con privilegi e supportano AD di PRIV nella gestione del ciclo di vita dell'appartenenza ai gruppi.
- **Cmdlet PowerShell**: per la compilazione nel servizio MIM e AD di PRIV con utenti e gruppi corrispondenti a utenti e gruppi nella foresta CORP per gli amministratori PAM e per gli utenti finali che richiedono l'uso di JIT di privilegi su un account amministrativo.
- **API REST di PAM e portale di esempio**: per gli sviluppatori che integrano MIM nello scenario PAM con client personalizzati per l'elevazione dei privilegi, senza dover usare PowerShell o SOAP. Con un'applicazione Web di esempio viene illustrato l'utilizzo delle API REST.

Dopo l'installazione e la configurazione, ogni gruppo creato dalla procedura di migrazione nella foresta PRIV è un gruppo di sicurezza shadow basato su SIDHistory (o in un aggiornamento successivo con Windows Server vNext, un gruppo principale esterno) che esegue il mirroring del gruppo SID nella foresta CORP originale. Inoltre, il servizio MIM aggiunge i membri a questi gruppi della foresta PRIV solo temporaneamente.

Di conseguenza, quando un utente richiede l'elevazione dei privilegi utilizzando i cmdlet PowerShell e la richiesta viene approvata, il servizio MIM aggiungerà tale account nella foresta PRIV a un gruppo nella stessa foresta. Quando l'utente accede con l'account con privilegi, il token Kerberos conterrà un identificatore di protezione (SID) identico al SID del gruppo nella foresta CORP. Dato che la foresta CORP è configurata in modo da considerare attendibile la foresta PRIV, per una risorsa che controlla l’appartenenza ai gruppi Kerberos, l’account con privilegi elevati usato per accedere a una risorsa nella foresta CORP risulta appartenente agli stessi gruppi di sicurezza di quella risorsa. Tale controllo viene effettuato dall’autenticazione Kerberos tra foreste diverse.

L’appartenenza dei membri è temporanea, per cui dopo un certo intervallo di tempo, l’account amministrativo dell'utente non appartiene più al gruppo della foresta PRIV. Di conseguenza, tale account non saranno utilizzabile per l'accesso a risorse aggiuntive.



<!--HONumber=Jul16_HO3-->


