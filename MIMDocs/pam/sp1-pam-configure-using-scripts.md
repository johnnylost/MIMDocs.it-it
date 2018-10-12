---
title: Configurare PAM tramite gli script
description: Questo articolo fa parte della serie per la configurazione di PAM tramite script. Viene illustrata la modifica del file XML che verrà usato dagli script di distribuzione di PAM.
keywords: ''
author: barclayn
ms.author: barclayn
manager: MBaldwin
ms.date: 07/20/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: ''
ms.suite: ems
ms.openlocfilehash: f5ae81fe8c0c695b26f2a28626512e056df4db8b
ms.sourcegitcommit: 46c68e2e0ebbf3cebae9fc04f1d2ba73bc987d5b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2018
ms.locfileid: "47414976"
---
# <a name="configure-pam-using-scripts"></a>Configurare PAM tramite gli script

Se si sceglie di installare SQL e SharePoint su server separati, è necessario configurarli tramite le istruzioni seguenti. Se SQL, SharePoint e i componenti PAM sono installati nello stesso computer, i passaggi seguenti devono essere eseguiti da tale computer.

La procedura seguente presuppone che sia già configurato un dominio PRIV. Per istruzioni sulla configurazione di un dominio PRIV, visualizzare l'appendice alla fine del documento.

passaggi:

1. Scaricare [Script di distribuzione per PAM](https://www.microsoft.com/download/details.aspx?id=53941)
2. Decomprimere il file compresso "PAMDeploymentScripts.zip" nella cartella %SYSTEMDRIVE%\PAM in tutti i computer.
3. Su uno qualsiasi dei computer, aprire il file **PAMDeploymentConfig.xml** e aggiornare i dettagli usando il grafico riportato di seguito o informazioni aggiuntive all'interno del file con estensione xml stesso. Se le foreste PRIV e CORP sono già configurate, per l'aggiornamento sono necessari solo i parametri **DNSName** e **NetbiosName.**
4. Nella sezione Ruoli aggiornare l'**account del servizio**, i **dettagli del computer** e il **percorso dei file binari di installazione** per i ruoli SQL, SharePoint e MIM.
    1. Il percorso binario MIM deve puntare alla directory contenente la cartella relativa al servizio e al portale. Il percorso binario del client deve puntare alla directory contenente "Add-ins and Extensions.msi".

5. Se si tratta di un ambiente PRIVOnly, il tag PRIVOnly deve essere impostato su True.
    1. Per gli ambienti PRIVOnly, aggiornare i parametri **DNSName** e **NetbiosName** del dominio PRIV in modo che corrisponda al dominio CORP. Assicurarsi che i suffissi del computer siano corretti per i computer in cui verranno installati SQL, SharePoint e MIM, in quanto il file modello predefinito presuppone una configurazione PRIV e CORP.
    2. Per altre informazioni sugli ambienti PRIVOnly, fare clic qui.

6. Copiare il file PAMDeploymentConfig.xml nella cartella %SYSTEMDRIVE%\PAM in tutti i computer, controller di dominio CORP, controller di dominio PRIV, server PAM, server SQL e server SharePoint.


## <a name="deployment-worksheet"></a>Foglio di lavoro di distribuzione

Prima di procedere, aggiornare il file PAMDeploymentConfig.xml e inserire la copia aggiornata in tutti i computer.

### <a name="setup"></a>Installazione

|Computer   | Esecuzione come   |Comandi   |
|---|---|---|
|  PRIVDC |Amministratore del dominio PRIV   | .\PAMDeployment.ps1 Selezionare l'opzione di menu 1 (configurazione della foresta PRIV)   |
|   |   |  Il passaggio precedente consente di generare un file SIDs.txt. Questo file deve essere copiato in $envDrive:PAM di CORPDC prima di eseguire il passaggio successivo. |
| CORPDC  |Amministratore del dominio CORP   | .\PAMDeployment.ps1 Selezionare l'opzione di menu 2 (configurazione della foresta CORP)   |
| PAMServer (o SQL Server)   |Amministratore del dominio CORP   |  .\PAMDeployment.ps1 Selezionare l'opzione di menu 2 (configurazione della foresta CORP)  |
|  PAMServer |  Amministratore locale (amministrazione MIM dopo l'aggiunta a un dominio) |  .\PAMDeployment.ps1 Selezionare l'opzione di menu 4 (installazione di SharePoint)  |
| PAMServer  | Amministratore locale (amministrazione MIM dopo l'aggiunta a un dominio)  | .\PAMDeployment.ps1 Selezionare l'opzione di menu 5 (installazione di PAM per MIM)   |
|  PAMServer |MIMAdmin   | .\PAMDeployment.ps1 Selezionare l'opzione di menu 6 (installazione attendibile di PAM) .\PAMDeployment.ps1 Selezionare l'opzione di menu 6 (installazione dell'attendibilità PAM) |

### <a name="validation"></a>Convalida

|  Computer | Esecuzione come   | Comandi   |
|---|---|---|
| CORPClient  | Utente CORP (amministratore locale)  |   .\PAMDeployment.ps1 Selezionare l'opzione di menu 7 (impostazione del client PAM per MIM)  |
| CORPDC  | Amministratore del dominio CORP   | Import-module .\PAMValidation.psm1 ; Create-PAMValidationCORPDCConfig   |
| PAMServer   | MIMAdmin  | Import-module .\PAMValidation.psm1 ; Move-PAMValidationUsersToPAM  |
| CORPClient  | Utente CORP (amministratore locale)   |   Import-module .\PAMValidation.psm1 ; Enable-PAMUsersCORPClientRemote |
|  CORPClient | <PRIV>Utente \PRIV.pamRequestor e in caso di PRIVOnly: <CORP>\pamrequestor   | Import-module .\PAMValidation.psm1 ; Test-PAMValidationScenarioNoApprovalRequest  |


> [!div class="step-by-step"]
> [Inizio »](sp1-step1-configuring-priv-domain.md)
