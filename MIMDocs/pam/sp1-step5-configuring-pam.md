---
title: 'Passaggio 5: Installazione e configurazione di PAM'
description: "Preparare il dominio CORP con identità nuove o esistenti da gestire con Privileged Identity Manager tramite gli script"
keywords: 
author: barclayn
ms.author: barclayn
manager: MBaldwin
ms.date: 10/25/2016
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: 
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 365989693f844f117f76ee2b69db85df82f06f35
ms.openlocfilehash: c641865548f753a609ccee8dbf12c329bb6a1c9f


---
# <a name="step-5-installingconfiguring-pam"></a>Passaggio 5: Installazione e configurazione di PAM

>[!div class="step-by-step"]
[« Passaggio 4](sp1-step4-configuring-sharepoint.md)
[Passaggio 6 »](sp1-step6-setup-pam-trust.md)

Per PAMServer aggiunto al dominio, accedere come MIMAdmin. In caso contrario, accedere come amministratore locale.
1. Eseguire PowerShell come amministratore
2. cd $env:SYSTEMDRIVE\PAM
3. .\PAMDeploymnet.ps1
4. Selezionare l'opzione di menu 5 (installazione di PAM per MIM)

>[!NOTE]
>Se il computer non è già aggiunto a un dominio PRIV, verranno richieste le credenziali. Dopo l'aggiunta al dominio, il computer verrà riavviato.

Dopo il riavvio di PAMServer, accedere nuovamente al computer con l'account MIMAdmin.

1. Eseguire PowerShell come amministratore
2. cd $env:SYSTEMDRIVE\PAM
3. .\PAMDeployment.ps1
4. Selezionare l'opzione di menu 5 (installazione di PAM per MIM)

  Quando richiesto, immettere la password dell'account di monitoraggio MIM, del componente MIM, di MA MIM, del servizio MIM, dell'amministratore MIM e di SharePoint.
  Al termine dell'installazione, il computer verrà riavviato.

>[!div class="step-by-step"]
[« Passaggio 4](sp1-step4-configuring-sharepoint.md)
[Passaggio 6 »](sp1-step6-setup-pam-trust.md)



<!--HONumber=Nov16_HO2-->


