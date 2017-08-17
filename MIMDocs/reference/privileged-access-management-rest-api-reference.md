---
title: Riferimento all'API REST di Privileged Access Management | Microsoft Docs
description: 
keywords: 
author: msmbaldwin
ms.author: mbaldwin
manager: mbaldwin
ms.date: 10/17/2016
ms.topic: reference
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 541854b7-f285-4e8b-bbaf-3f15da69467f
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 02de09a7de843cb42080daa4ce43a5a0c74f2c18
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/13/2017
---
# <a name="privileged-access-management-rest-api-reference"></a>Riferimento all'API REST di gestione accessi con privilegi (PAM)
Con Microsoft Identity Manager (MIM) 2016 viene aggiunto un nuovo scenario denominato Gestione accessi con privilegi (PAM). PAM consente alle organizzazioni di avere maggiore controllo sui diritti di accesso degli account utente con privilegi elevati, come gli amministratori di sistema o di servizio, alle risorse sensibili. PAM controlla l'accesso di account con privilegi elevati, fornendo diritti di accesso per un periodo limitato di tempo, JIT (Just-In-Time), quando sono necessari i diritti di accesso.

Un utente può chiedere assistenza MIM per i diritti di accesso con privilegi (elevazione dei privilegi) in uno dei due seguenti modi:

- Tramite l'API REST PAM
- Tramite il cmdlet PAM di PowerShell New-PAMRequest

In questa guida viene illustrata l’API REST PAM. Per ulteriori informazioni sull'utilizzo del cmdlet di PowerShell, vedere la Guida dell'ambiente di testing: Dimostrazione della gestione dell'accesso con privilegi utilizzando Microsoft Identity Manager, disponibile sul sito Connect.

##<a name="pam-rest-api-resources-and-operations"></a>Operazioni e risorse di API REST PAM
L'API REST PAM opera sulle seguenti risorse
- **Ruolo PAM**: un ruolo PAM associa una raccolta di utenti a un insieme di diritti di accesso. I diritti di accesso vengono definiti facendo riferimento a gruppi di sicurezza.  Ogni ruolo PAM dispone di un elenco di account utente, denominati candidati, che danno diritto a elevare i privilegi al ruolo PAM. I filtri WMI consentono di eseguire le azioni seguenti:

    - [Ottenere ruoli PAM](privileged-access-management-get-roles.md)

- **Richiesta PAM**: Un utente che desidera elevare i propri diritti di accesso a un ruolo PAM deve inviare una richiesta PAM e ottenere l'approvazione della richiesta. L'oggetto di richiesta PAM tiene traccia del ciclo di vita della richiesta nel servizio MIM. È possibile eseguire le operazioni seguenti nelle richieste PAM:

    - [Creare una richiesta PAM](privileged-access-management-create-request.md)
    - [Ottenere richieste PAM](privileged-access-management-get-requests.md)
    - [Chiudere una richiesta PAM](privileged-access-management-close-request.md)

- **Pending PAM Request**: consente di approvare o rifiutare le richieste PAM che sono state inviate dagli utenti. È possibile eseguire le operazioni seguenti nelle richieste PAM in sospeso:

    - [Ottenere le richieste PAM in sospeso](privileged-access-management-get-pending-requests.md)
    - [Approvare o rifiutare una richiesta PAM in sospeso](privileged-access-management-approve-reject-pending-request.md)

- **PAM Session**: quando si utilizza l'API REST PAM, il client (ad esempio, un browser Web) avvia una sessione con l'endpoint delle API REST PAM. In questa sessione, il client viene autenticato per l'endpoint API REST. È possibile eseguire le operazioni seguenti nelle sessioni PAM:

     - [Ottenere informazioni sulla sessione PAM](privileged-access-management-get-session-info.md)

Per altre informazioni sul servizio, vedere [Dettagli servizio API REST di gestione accessi con privilegi](privileged-access-management-rest-api-service-details.md)

##<a name="pam-sample-portal-on-github"></a>Portale di esempio PAM su GitHub
Un modo per apprendere come utilizzare l'API REST PAM è tramite il portale di esempio PAM, un'applicazione Web di esempio che utilizza l'API. È possibile trovare il codice per il portale PAM di esempio nell'[archivio degli esempi per PAM su GitHub](http://go.microsoft.com/fwlink/?LinkID=618550&clcid=0x409). È possibile apprendere come distribuire il portale di esempio in PAM Guida dell'ambiente di Test.
