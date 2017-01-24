---
title: "Passaggio 6: Installazione dell&quot;attendibilità PAM"
description: "Passaggio 6 della configurazione di PAM tramite script. Questa sezione descrive l&quot;impostazione dell&quot;attendibilità necessaria tra i domini PRIV e CORP"
keywords: 
author: barclayn
ms.author: barclayn
manager: MBaldwin
ms.date: 01/10/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: 
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: f08b0197341351bd5f33552f26b96132b1356239
ms.openlocfilehash: 3b232dfa515b42fd42a5606d1beff9d3fe50389c


---

# <a name="step-6-set-up-the-pam-trust"></a>Passaggio 6: Installare l'attendibilità di PAM

>[!div class="step-by-step"]
[« Passaggio 5](sp1-step5-configuring-pam.md)
[Passaggio 7 »](sp1-step7-setup-sidhistory-sidfiltering.md)

**Non è obbligatoria per un ambiente PRIVOnly** Accedere a PAMServer con l'account MIMAdmin.

1. Accedere a PAMServer con l'account MIMAdmin
2. Eseguire PowerShell come amministratore
3. cd $env:SYSTEMDRIVE\PAM
4. .\PAMDeployment.ps1
5. Selezionare l'opzione di menu 6 (installazione dell'attendibilità PAM)

  Quando richiesto, immettere le credenziali dell'account amministratore CORP. Dopo aver specificato le credenziali, verrà stabilita la relazione di trust e la configurazione viene completata.

>[!div class="step-by-step"]
[« Passaggio 5](sp1-step5-configuring-pam.md)
[Passaggio 7 »](sp1-step7-setup-sidhistory-sidfiltering.md)



<!--HONumber=Jan17_HO2-->


