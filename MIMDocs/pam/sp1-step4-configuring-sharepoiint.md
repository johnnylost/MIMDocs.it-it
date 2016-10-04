---
title: 'Passaggio 4: Configurazione di SharePoint'
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
ms.sourcegitcommit: 689c2ef0e4e4a681a398ba7e94fb3def525937ea
ms.openlocfilehash: aed3370e8dbc458c97c60686957f337cfd451d33


---

# Passaggio 4: Configurazione di SharePoint

La versione di SharePoint deve essere SharePoint Foundation 2013 con SP1.

Per i server aggiunti al dominio, accedere come MIMAdmin

1. Eseguire PowerShell come amministratore
2.  .\PAMDeployment.ps1
3.  Selezionare l'opzione di menu 4 (installazione di SharePoint)


Per i server del gruppo di lavoro

1. Eseguire PowerShell come amministratore
2.  cd $env:SYSTEMDRIVE\PAM
3.  .\PAMDeployment.ps1
4. Selezionare l'opzione di menu 4 (installazione di SharePoint)

Il computer verrà riavviato più volte durante l'installazione di SharePoint. Ogni volta è necessario eseguire nuovamente l'installazione di SharePoint assicurandosi di accedere con l'account MIMAdmin.
Se il computer di installazione di SharePoint non dispone di connettività Internet per scaricare i prerequisiti, è possibile scaricarli in modo indipendente e inserirli nella cartella locale. **Questo percorso di cartella locale deve essere aggiornata nel file PAMConfiguration.xml in <PrerequisitesBinaryLocation/>.** Per i collegamenti ai file di download, vedere l'appendice 5.
Dopo l'installazione, la GUI di configurazione di SharePoint verrà aperta e sarà quindi necessario seguire i passaggi per completare l'installazione di SharePoint. Selezionare Complete Server (Completa server) e scorrere il resto dell'interfaccia utente. Dopo l'installazione, verrà richiesto di eseguire la Configurazione guidata. Completare i passaggi seguendo questa procedura.

1. Nella scheda **Connect to server farm** (Connetti a una server farm) modificare in **crea una nuova server farm**.
2. Specificare **SQLServer** come server di database per il database di configurazione e **SharePoint ServiceAccount** come account di accesso del database che verrà usato da SharePoint.
3. Specificare una password come passphrase di sicurezza della farm **(non verrà usata in seguito in questa procedura dettagliata)**.
4. Accettare le altre impostazioni predefinite della Configurazione guidata di SharePoint per creare una farm a server singolo.

È possibile trovare informazioni dettagliate nella sezione **Configure SharePoint** (Configurare SharePoint) in [Passaggio 3: Preparare un server PAM](/microsoft-identity-manager/pam/step-3-prepare-pam-server). Al termine, eseguire nuovamente lo script ".PAMDeployment.ps1", selezionare l'opzione 4 (installazione di SharePoint) per completare questo passaggio.



<!--HONumber=Sep16_HO4-->


