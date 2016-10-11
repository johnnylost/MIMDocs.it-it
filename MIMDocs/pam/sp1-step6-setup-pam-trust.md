---
title: "Passaggio 6: Installazione dell'attendibilità PAM"
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
ms.openlocfilehash: 46afda513e849e457f5f3644a46f244161467e50


---

# Installazione dell'attendibilità PAM

**Non è obbligatoria per un ambiente PRIVOnly** Accedere a PAMServer con l'account MIMAdmin.

1. Accedere a PAMServer con l'account MIMAdmin
2. Eseguire PowerShell come amministratore
3. cd $env:SYSTEMDRIVE\PAM
4. .\PAMDeployment.ps1
5. Selezionare l'opzione di menu 6 (installazione dell'attendibilità PAM)

  Quando richiesto, immettere le credenziali dell'account amministratore CORP. Dopo aver specificato le credenziali, verrà stabilita la relazione di trust e la configurazione viene completata.



<!--HONumber=Sep16_HO4-->


