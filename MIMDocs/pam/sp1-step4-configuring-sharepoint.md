---
title: 'Passaggio 4: Configurazione di SharePoint'
description: Si tratta del passaggio 4 del processo di configurazione di PAM tramite script. In questo passaggio viene configurato SharePoint in modo che possa essere usato come parte della distribuzione di PAM.
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
ms.openlocfilehash: 12eb9a00584f72b9c628e870562a743fb603d4a3
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/13/2017
---
# <a name="step-4-configuring-sharepoint"></a>Passaggio 4: Configurazione di SharePoint

>[!div class="step-by-step"]
[ «Passaggio 3](sp1-step3-installing-configuring-sql.md)
[Passaggio 5 »](sp1-step5-configuring-pam.md)

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
Se il computer di installazione di SharePoint non dispone di connettività Internet per scaricare i prerequisiti, è possibile scaricarli in modo indipendente e inserirli nella cartella locale. **Questo percorso di cartella locale deve essere aggiornato nel file PAMConfiguration.xml in <PrerequisitesBinaryLocation/>.** Per i collegamenti ai file di download, vedere l'appendice 5.
Dopo l'installazione, la GUI di configurazione di SharePoint verrà aperta e sarà quindi necessario seguire i passaggi per completare l'installazione di SharePoint. Selezionare Complete Server (Completa server) e scorrere il resto dell'interfaccia utente. Dopo l'installazione, verrà richiesto di eseguire la Configurazione guidata. Completare i passaggi seguendo questa procedura.

1. Nella scheda **Connect to server farm** (Connetti a una server farm) modificare in **crea una nuova server farm**.
2. Specificare **SQLServer** come server di database per il database di configurazione e **SharePoint ServiceAccount** come account di accesso del database che verrà usato da SharePoint.
3. Specificare una password come passphrase di sicurezza della farm **(non verrà usata in seguito in questa procedura dettagliata)**.
4. Accettare le altre impostazioni predefinite della Configurazione guidata di SharePoint per creare una farm a server singolo.

È possibile trovare informazioni dettagliate nella sezione **Configure SharePoint** (Configurare SharePoint) in [Passaggio 3: Preparare un server PAM](/microsoft-identity-manager/pam/step-3-prepare-pam-server). Al termine, eseguire nuovamente lo script ".PAMDeployment.ps1", selezionare l'opzione 4 (installazione di SharePoint) per completare questo passaggio.

>[!div class="step-by-step"]
[ «Passaggio 3](sp1-step3-installing-configuring-sql.md)
[Passaggio 5 »](sp1-step5-configuring-pam.md)
