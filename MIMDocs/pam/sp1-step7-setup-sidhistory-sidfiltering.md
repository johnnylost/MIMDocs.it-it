---
title: 'Passaggio 7: Installare la cronologia e il filtraggio SID'
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
ms.openlocfilehash: 4c5cfa92f3111a6d298f586ba547a1eca2502853


---

# Installare la cronologia e il filtraggio SID

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



<!--HONumber=Sep16_HO4-->


