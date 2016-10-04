---
title: 'Passaggio 8: Verifica della distribuzione PAM'
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
ms.openlocfilehash: 743ba586374ccc04e9ddafff759a00574e13f6ac


---

# Passaggio 8: Verifica della distribuzione PAM

Il pacchetto di distribuzione viene fornito con gli script di verifica che possono eseguire uno scenario PAM per convalidare che la distribuzione PAM funzioni come previsto.
Per uszare la verifica della distribuzione, modificare la sezione denominata PAMDeploymentConfig.xml <PamValidation/>.

>[!Note] La convalida richiede un computer client aggiunto al dominio CORP con i componenti lato client di PAM installati. Per gli script su come installare un client, vedere l'appendice.

Il nome del computer client deve essere aggiornato nel tag <PAMValidationClient/> del file PAMDeploymentConfig.xml. Il resto dei dati nel nodo <PAMValidation/> deve essere modificato solo se è in conflitto con gli utenti o i gruppi esistenti, in quanto questa convalida tenterà di crearli.
Per eseguire la convalida, usare la procedura seguente:

Passaggio 1:

1. Accedere a CORPDC come amministratore del dominio CORP
2. Eseguire PowerShell come amministratore
3. cd $env:SYSTEMDRIVE\PAM
4. Import-module .\PAMValidation.psm1
5. Create-PAMValidationonCORPDCConfig

Verranno creati i gruppi e gli utenti necessari per la convalida

Passaggio 2:

1. Accedere al server PAM con l'account MIMAdmin
2. Eseguire PowerShell come amministratore
3. cd $env:SYSTEMDRIVE\PAM
4. import-module .\PAMValidation.psm1
5. move-PAMVAlidationUsersToPAM

Questo passaggio esegue la migrazione di utenti e gruppi all'ambiente PAM

Passaggio 3:

1. Accedere al client CORP come amministratore locale
2. Eseguire PowerShell come amministratore
3. cd $env:SYSTEMDRIVE\PAM
4. import-module .\PAMValidation.psm1
5. Enable-PAMUsersCORPClientRemote


Questo passaggio richiederà le credenziali CORPAdmin. Dopo averle specificate, gli utenti richiesti verranno aggiunti ai gruppi 'Utenti Desktop remoto' e 'Utenti gestione remota.
Nel client CORP, usare il comando seguente per aprire PowerShell come utente PRIV da convalidare. </br></br>
**Runas /u:<PRIV domain>\PRIV.pamRequestor powershell.exe**  </br></br>
Nella finestra di PowerShell, digitare:

1. cd $env:SYSTEMDRIVE\PAM
2. import-module .\PAMValidation.psm1
3. test-PAMValidationScenarioNoApprovalRequest


  Questa operazione consente di visualizzare lo stato della richiesta.
  Inizialmente l'utente non avrà accesso alla risorsa. Dopo che l'utente viene aggiunto al ruolo JIT, gli viene concesso l'accesso. Allo scadere della richiesta, l'utente non disporrà ancora dell'accesso.
  Lo script usa il valore predefinito (11 minuti) per la scadenza della richiesta.



<!--HONumber=Sep16_HO4-->


