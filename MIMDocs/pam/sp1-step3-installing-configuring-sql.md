---
title: 'Passaggio 3: Configurazione di SQL'
description: "Questo articolo è il passaggio 3 della serie di articoli sul processo di configurazione di Privileged Identity Manager tramite script e illustra i passaggi di configurazione di SQL Server."
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
ms.openlocfilehash: 114607c460fbeb392e0ea79115ac4f3ae556927e
ms.sourcegitcommit: 8edd380f54c3e9e83cfabe8adfa31587612e5773
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/19/2017
---
# <a name="step-3-configuring-sql"></a>Passaggio 3: Configurazione di SQL

>[!div class="step-by-step"]
[« Passaggio 2](sp1-step2-configuring-corp-domain.md)
[Passaggio 4 »](sp1-step4-configuring-sharepoint.md)

Prima di proseguire con la procedura riportata di seguito, verificare che si stia usando SQL Server 2012 SP1 o versione successiva o SQL Server 2014. Per i server aggiunti al dominio, accedere tramite l'account MIMAdmin. In caso contrario, accedere come amministratore locale
1. Eseguire PowerShell come amministratore
2. cd $env:SYSTEMDRIVE\PAM
3. .\PAMDeployment.ps1
4. Selezionare l'opzione di menu 3 (installazione di SQL Server)

  Se il server non è ancora aggiunto al dominio PRIV, verrà chiesto di specificare le credenziali e aggiungere il server al dominio.
  Dopo l'aggiunta al dominio, il computer verrà riavviato. In seguito al riavvio del sistema, accedere al server con l'account MIMAdmin.

1. Eseguire PowerShell come amministratore
2. cd $env:SYSTEMDRIVE\PAM
3. .\PAMDeployment.ps1
4. Selezionare l'opzione di menu 3 (installazione di SQL Server)

Quando richiesto, specificare la password per l'account del servizio MIMAdmin e procedere con l'installazione. Al termine, passare al passaggio 4.

>[!div class="step-by-step"]
[« Passaggio 2](sp1-step2-configuring-corp-domain.md)
[Passaggio 4 »](sp1-step4-configuring-sharepoint.md)
