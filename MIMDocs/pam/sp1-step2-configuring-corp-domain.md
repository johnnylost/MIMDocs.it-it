---
title: 'Passaggio 2: Configurazione del dominio CORP'
description: Questo articolo descrive il secondo passaggio richiesto per configurare il dominio CORP che prevede l'esecuzione di uno script dopo che sids.txt viene copiato in CORPDC.
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
ms.openlocfilehash: 8480350f85b3543a32d4db3dbc6a388afcb16352
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/13/2017
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
