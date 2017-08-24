---
title: "Passaggio 6: Installazione dell'attendibilità PAM"
description: "Passaggio 6 della configurazione di PAM tramite script. Questa sezione descrive l'impostazione dell'attendibilità necessaria tra i domini PRIV e CORP"
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
ms.openlocfilehash: a24b2a5a196e633379b696ee82e9428dd67454ca
ms.sourcegitcommit: 8edd380f54c3e9e83cfabe8adfa31587612e5773
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/19/2017
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
