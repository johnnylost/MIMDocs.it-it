---
title: 'Passaggio 7: Installare la cronologia e il filtraggio SID'
description: Si tratta del passaggio 7 del processo di configurazione di Privileged Identity Manager tramite script. Questo passaggio riguarda l'impostazione della cronologia e del filtro SID.
keywords: ''
author: barclayn
ms.author: barclayn
manager: MBaldwin
ms.date: 08/18/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: ''
ms.suite: ems
ms.openlocfilehash: f10febeb02452517d49a897729f8d2ee12f85328
ms.sourcegitcommit: ace4d997c599215e46566386a1a3d335e991d821
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/15/2018
ms.locfileid: "49332961"
---
# <a name="step-7-set-up-sid-historysid-filtering"></a>Passaggio 7: Installare la cronologia e il filtraggio SID

> [!div class="step-by-step"]
> [« Passaggio 6](sp1-step6-setup-pam-trust.md)
> [Passaggio 8 »](sp1-step8-pam-deployment-verification.md)

**Non è obbligatoria per un ambiente PRIVOnly** Accedere a PAMServer con l'account MIMAdmin.

1. Accedere al controller di dominio CORP come amministratore
2. Eseguire PowerShell come amministratore
3. cd $env:SYSTEMDRIVE\PAM
4. .\PAMDeployment.ps1
5. Selezionare l'opzione di menu 8 (installare la cronologia e il filtraggio SID)

Al termine dell'esecuzione vengono visualizzati i messaggi seguenti:<br/></br>
Per il filtraggio SID: <br/></br>
"Impostazione del trust per non filtrare i SID" o "Filtro dei SID non attivato per il trust". </br></br>
Per la cronologia SID: </br></br>
"Attivazione della cronologia SID per il trust" o "Cronologia SID già attivata per il trust".

> [!div class="step-by-step"]
> [« Passaggio 6](sp1-step6-setup-pam-trust.md)
> [Passaggio 8 »](sp1-step8-pam-deployment-verification.md)
