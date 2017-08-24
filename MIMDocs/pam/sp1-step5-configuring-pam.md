---
title: 'Passaggio 5: Installazione e configurazione di PAM'
description: Si tratta del passaggio 5 del processo di configurazione di Privileged Identity Manager tramite script e illustra i passaggi di distribuzione nel server PAM.
keywords: 
author: barclayn
ms.author: barclayn
manager: MBaldwin
ms.date: 08/18/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: 
ms.suite: ems
ms.openlocfilehash: 414851f8550f6419db7e268e982b88065730ab4e
ms.sourcegitcommit: 8edd380f54c3e9e83cfabe8adfa31587612e5773
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/19/2017
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
