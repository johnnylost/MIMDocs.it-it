---
title: 'Passaggio 1: Configurazione del dominio PRIV'
description: "Preparare il dominio CORP con identità nuove o esistenti da gestire con Privileged Identity Manager tramite gli script"
keywords: 
author: barclayn
manager: MBaldwin
ms.date: 09/26/2016
ms.topic: article
ms.prod: microsoft-identity-manager
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: 
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: c7c5266f3d1c51e933855031f4128cbcb967d6e2
ms.openlocfilehash: 37ac600701ed9d90790e557d2f282be45eed43b4


---
# Passaggio 1: Configurazione del dominio PRIV

1. Accedere a PRIVDC come amministratore
  * Se si tratta di un ambiente PRIVOnly, accedere a CORPDC
2. Eseguire PowerShell come amministratore
3. cd $env:SYSTEMDRIVE\PAM
4. .\PAMDeployment.ps1
5. Selezionare l'opzione di menu 1 (configurazione della foresta PRIV)


Gli account di servizio necessari per la gestione di SQL/SharePoint e MIM vengono creati automaticamente se non sono già presenti nel dominio. Verrà richiesto di immettere le password per la creazione di questi account di servizio durante l'esecuzione dello script.
Se il dominio PRIV è Windows Server 2016, con il livello di funzionalità impostato su Windows Server 2016 Technical Preview 5, lo script richiederà di abilitare la funzionalità facoltativa 'Privileged Access Management' di Active Directory richiesta da PAM. Scegliere Sì per continuare.
Per i livelli di funzionalità inferiori a Windows Server 2016, ignorare l'avviso relativo al fatto che non verrà eseguita una configurazione aggiuntiva. È necessario eseguire nuovamente il file PAMDeployment.ps1 e la configurazione della foresta PAM, quando l'amministratore aumenta il livello di funzionalità a Windows Server 2016.

>[!Note] I passaggi seguenti non sono necessari per le configurazioni di PRIVOnly

Copiare il file SIDs.txt che viene generato in $env:SYSTEMDRIVE\PAM nella cartella analoga di CORPDC. Questa operazione è necessaria per CORPDC per configurare le autorizzazioni per gli utenti PRIV al fine di leggere le proprietà utente CORP.
Al completamento dello script verrà chiesto di riavviare il computer per rendere effettive le modifiche.



<!--HONumber=Sep16_HO4-->


