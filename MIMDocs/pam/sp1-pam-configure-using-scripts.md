---
title: Configurare PAM tramite gli script
description: "Preparare il dominio CORP con identità nuove o esistenti da gestire con Privileged Identity Manager tramite gli script"
keywords: 
author: barclayn
manager: MBaldwin
ms.date: 09/26/2016
ms.topic: article
ms.prod: microsoft-identity-manager
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: 
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 96c734ade75f5c206858387cf45106761bc0a881
ms.openlocfilehash: a1e4e5561bf8d38c56c3d27249d94f4bf7103b8c


---

# Configurare PAM tramite gli script

Se si sceglie di installare SQL e SharePoint su server separati, devono essere configurati tramite le istruzioni riportate di seguito. Se SQL, SharePoint e i componenti PAM sono installati nello stesso computer, i passaggi seguenti devono essere eseguiti da tale computer.

La procedura seguente presuppone che sia già configurato un dominio PRIV. Per istruzioni sulla configurazione di un dominio PRIV, visualizzare l'appendice alla fine del documento.

passaggi:

1. Decomprimere il file compresso "PAMDeploymentScripts.zip" nella cartella %SYSTEMDRIVE%\PAM in tutti i computer.
2. Su uno qualsiasi dei computer, aprire il file **PAMDeploymentConfig.xml** e aggiornare i dettagli usando il grafico riportato di seguito o informazioni aggiuntive all'interno del file con estensione xml stesso. Se le foreste PRIV e CORP sono già configurate, per l'aggiornamento sono necessari solo i parametri **DNSName** e **NetbiosName.**
3. Nella sezione Ruoli aggiornare l'**account del servizio**, i **dettagli del computer** e il **percorso dei file binari di installazione** per i ruoli SQL, SharePoint e MIM.
    1. Il percorso binario MIM deve puntare alla directory contenente la cartella relativa al servizio e al portale. Il percorso binario del client deve puntare alla directory contenente "Add-ins and Extensions.msi".

4. Se si tratta di un ambiente PRIVOnly, il tag PRIVOnly deve essere impostato su True.
    1. Per gli ambienti PRIVOnly, aggiornare i parametri **DNSName** e **NetbiosName** del dominio PRIV in modo che corrisponda al dominio CORP. Assicurarsi che i suffissi del computer siano corretti per i computer in cui verranno installati SQL, SharePoint e MIM, in quanto il file modello predefinito presuppone una configurazione PRIV e CORP.
    2. Per altre informazioni sugli ambienti PRIVOnly, fare clic qui.

5. Copiare il file PAMDeploymentConfig.xml nella cartella %SYSTEMDRIVE%\PAM in tutti i computer, controller di dominio CORP, controller di dominio PRIV, server PAM, server SQL e server SharePoint.


## Foglio di lavoro di distribuzione

Prima di procedere, aggiornare il file PAMDeploymentConfig.xml e inserire la copia aggiornata in tutti i computer.

### Installazione

|Computer   | Esecuzione come   |Comandi   |
|---|---|---|
|  PRIVDC |Amministratore del dominio PRIV   | .\PAMDeployment.ps1 Selezionare l'opzione di menu 1 (configurazione della foresta PRIV)   |
|   |   |  Il passaggio precedente consente di generare un file SIDs.txt. Questo file deve essere copiato in $envDrive:PAM di CORPDC prima di eseguire il passaggio successivo. |
| CORPDC  |Amministratore del dominio CORP   | .\PAMDeployment.ps1 Selezionare l'opzione di menu 2 (configurazione della foresta CORP)   |
| PAMServer (o SQL Server)   |Amministratore del dominio CORP   |  .\PAMDeployment.ps1 Selezionare l'opzione di menu 2 (configurazione della foresta CORP)  |
|  PAMServer |  Amministratore locale (amministrazione MIM dopo l'aggiunta a un dominio) |  .\PAMDeployment.ps1 Selezionare l'opzione di menu 4 (installazione di SharePoint)  |
| PAMServer  | Amministratore locale (amministrazione MIM dopo l'aggiunta a un dominio)  | .\PAMDeployment.ps1 Selezionare l'opzione di menu 5 (installazione di PAM per MIM)   |
|  PAMServer |MIMAdmin   | .\PAMDeployment.ps1 Selezionare l'opzione di menu 6 (installazione attendibile di PAM) .\PAMDeployment.ps1 Selezionare l'opzione di menu 6 (installazione dell'attendibilità PAM) |

### Convalida

|  Computer | Esecuzione come   | Comandi   |
|---|---|---|
| CORPClient  | Utente CORP (amministratore locale)  |   .\PAMDeployment.ps1 Selezionare l'opzione di menu 7 (impostazione del client PAM per MIM)  |
| CORPDC  | Amministratore del dominio CORP   | Import-module .\PAMValidation.psm1 ; Create-PAMValidationCORPDCConfig   |
| PAMServer   | MIMAdmin  | Import-module .\PAMValidation.psm1 ; Move-PAMValidationUsersToPAM  |
| CORPClient  | Utente CORP (amministratore locale)   |   Import-module .\PAMValidation.psm1 ; Enable-PAMUsersCORPClientRemote |
|  CORPClient | <PRIV>Utente \PRIV.pamRequestor in caso di PRIVOnly: <CORP>\pamrequestor   | Import-module .\PAMValidation.psm1 ; Test-PAMValidationScenarioNoApprovalRequest  |



<!--HONumber=Sep16_HO4-->


