---
title: 'Passaggio 2: Configurazione del dominio CORP'
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
ms.openlocfilehash: b7acc475c505b29559c510fd3fa350fed1c3157b


---

# <a name="step-2-configuring-the-corp-domain"></a>Passaggio 2: Configurazione del dominio CORP

>[!div class="step-by-step"]
[«Passaggio 1](sp1-step1-configuring-priv-domain.md)
[Passaggio 3»](sp1-step3-installing-configuring-sql.md)

Quando il file SIDs.txt copiato in CORPDC **non è necessario per le distribuzioni PRIVOnly**

1. Accedere a CORPDC come amministratore
2. Eseguire PowerShell come amministratore
3. cd $env:SYSTEMDRIVE\PAM
4. .\PAMDeployment.ps1
5. Selezionare l'opzione di menu 2 (configurazione della foresta CORP)

>[!div class="step-by-step"]
[«Passaggio 1](sp1-step1-configuring-priv-domain.md)
[Passaggio 3»](sp1-step3-installing-configuring-sql.md)



<!--HONumber=Nov16_HO2-->


