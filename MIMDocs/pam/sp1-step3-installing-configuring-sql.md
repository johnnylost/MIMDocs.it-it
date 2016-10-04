---
title: 'Passaggio 3: Configurazione di SQL'
description: "Preparare il dominio CORP con identità nuove o esistenti da gestire con Privileged Identity Manager tramite gli script"
keywords: 
author: barclayn
manager: MBaldwin
ms.date: 09/27/2016
ms.topic: article
ms.prod: microsoft-identity-manager
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: 
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 689c2ef0e4e4a681a398ba7e94fb3def525937ea
ms.openlocfilehash: a7d456b1c2baf31ef2d7ca801a567cf42eaef52e


---
# Passaggio 3: Configurazione di SQL

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



<!--HONumber=Sep16_HO4-->


