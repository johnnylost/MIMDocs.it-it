---
title: Configurare SharePoint per Microsoft Identity Manager 2016 | Documentazione Microsoft
description: Installare e configurare SharePoint Foundation in modo che possa ospitare la pagina del portale MIM.
keywords: ''
author: fimguy
ms.author: davidste
manager: mbaldwin
ms.date: 04/26/2018
ms.topic: get-started-article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: c01487f2-3de6-4fc4-8c3a-7d62f7c2496c
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: eceb1ed31b0212970d5cf0eae0bc8d96aa087ff5
ms.sourcegitcommit: 32d9a963a4487a8649210745c97a3254645e8744
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/27/2018
---
# <a name="set-up-an-identity-management-server-sharepoint"></a>Configurare un server di gestione delle identità: SharePoint

>[!div class="step-by-step"]
[« SQL Server 2016](prepare-server-sql2016.md)
[Exchange Server »](prepare-server-exchange.md)

> [!NOTE]
> Questa procedura dettagliata usa nomi e valori di esempio della società Contoso. Sostituirli con i propri nomi e valori. Ad esempio:
> - Nome del controller di dominio: **corpdc**
> - Nome del dominio: **contoso**
> - Nome del server del servizio MIM: **corpservice**
> - Nome del server di sincronizzazione MIM: **corpservice**
> - Nome SQL Server: **corpsql**
> - Password: **Pass@word1**


## <a name="install-sharepoint-2016"></a>Installare **SharePoint 2016**

> [!NOTE]
> Il programma di installazione richiede una connessione Internet per scaricare i relativi prerequisiti. Se il computer è in una rete virtuale che non fornisce la connettività Internet, aggiungere un'altra interfaccia di rete al computer che fornisce una connessione a Internet. L'interfaccia può essere disabilitata al termine dell'installazione.

Seguire questa procedura per installare SharePoint 2016. Al termine dell'installazione, il server verrà riavviato.

1.  Avviare **PowerShell** con un account di dominio con privilegi di amministratore locale su **corpservice** e **sysadmin** nel server di database SQL che verrà usato, **contoso\miminstall**.

    -   Passare alla directory in cui è stato decompresso SharePoint.

    -   Digitare il comando seguente.

        ```
        .\prerequisiteinstaller.exe
        ```

2.  Dopo aver installato i prerequisiti di **SharePoint**, installare **SharePoint 2016** immettendo il seguente comando:

    ```
    .\setup.exe
    ```

3.  Selezionare il tipo di server completo.

4.  Al termine dell'installazione, eseguire la procedura guidata.

## <a name="run-the-wizard-to-configure-sharepoint"></a>Eseguire la procedura guidata per configurare SharePoint

Seguire i passaggi descritti in **Configurazione guidata Prodotti SharePoint** per configurare SharePoint per l'uso con MIM.

1. Nella scheda **Connessione a una server farm** , modificare per creare una nuova server farm.

2. Specificare questo server come server di database **corpsql** per il database di configurazione e *Contoso\SharePoint* come account di accesso del database che verrà usato da SharePoint.
    a. Nella configurazione guidata è consigliabile selezionare il tipo [MinRole](https://docs.microsoft.com/en-us/sharepoint/install/overview-of-minrole-server-roles-in-sharepoint-server-2016) per **Front-end**
3. Creare una password per la passphrase di sicurezza della farm.

4. Al termine della configurazione guidata dell’attività 10 di 10, fare clic su Fine. Si aprirà un Web browser.

5. Nella finestra popup di Internet Explorer eseguire l'autenticazione come *Contoso\miminstall* (o con l'account amministratore equivalente) per continuare.

6. Nella procedura guidata Web (all'interno dell'app Web) fare clic su **Annulla/Ignora**.


## <a name="prepare-sharepoint-to-host-the-mim-portal"></a>Preparare SharePoint per ospitare il portale MIM

> [!NOTE]
> Inizialmente, SSL non verrà configurato. Assicurarsi di configurare SSL o equivalente prima di abilitare l'accesso a questo portale.

1. Avviare **SharePoint 2016 Management Shell** ed eseguire il seguente script di PowerShell per creare un'**applicazione Web SharePoint 2016**.

    ```
    New-SPManagedAccount ##Will prompt for new account enter contoso\mimpool 
    $dbManagedAccount = Get-SPManagedAccount -Identity contoso\mimpool
    New-SpWebApplication -Name "MIM Portal" -ApplicationPool "MIMAppPool" -ApplicationPoolAccount $dbManagedAccount -AuthenticationMethod "Kerberos" -Port 80 -URL http://mim.contoso.com
    ```

    > [!NOTE]
    > Verrà visualizzato un messaggio di avviso in cui si comunica che viene usato il metodo di autenticazione classico di Windows e che la restituzione del comando finale potrebbe richiedere alcuni minuti. Al termine, l'output indicherà l'URL del nuovo portale. Mantenere la finestra **SharePoint 2016 Management Shell** aperta per un secondo momento.

2. Avviare SharePoint 2013 Management Shell ed eseguire il seguente script di PowerShell per creare una **Raccolta siti di SharePoint** associata all'applicazione Web.

  ```
    $t = Get-SPWebTemplate -compatibilityLevel 15 -Identity "STS#1"
    $w = Get-SPWebApplication http://mim.contoso.com/
    New-SPSite -Url $w.Url -Template $t -OwnerAlias contoso\miminstall -CompatibilityLevel 15 -Name "MIM Portal"
    $s = SpSite($w.Url)
    $s.AllowSelfServiceUpgrade = $false
    $s.CompatibilityLevel
  ```

  > [!NOTE]
  > Verificare che il risultato della variabile *CompatibilityLevel* sia "15". Se il risultato è diverso da "15", la raccolta siti non è stata creata per la versione corretta. Eliminare la raccolta siti e ricrearla.

3. Disabilitare **SharePoint Server-Side Viewstate** e l'attività di SharePoint "Processo di analisi integrità (Ogni ora, Timer di Microsoft SharePoint Foundation, Tutti i server)" eseguendo i comandi PowerShell seguenti in **Shell di gestione SharePoint 2016**:

  ```
  $contentService = [Microsoft.SharePoint.Administration.SPWebService]::ContentService;
  $contentService.ViewStateOnServer = $false;
  $contentService.Update();
  Get-SPTimerJob hourly-all-sptimerservice-health-analysis-job | disable-SPTimerJob
  ```

4. Nel server di gestione delle identità aprire una nuova scheda del Web browser, passare a http://mim.contoso.com/ e accedere come *contoso\miminstall*.  Verrà visualizzato un sito SharePoint denominato *Portale MIM* .

    ![Immagine del portale MIM all'indirizzo http://mim.contoso.com/](media/MIM-DeploySP1.png)

5. Copiare l'URL, quindi in Internet Explorer aprire **Opzioni Internet**, passare alla scheda **Sicurezza**, selezionare **Intranet locale** e fare clic su **Siti**.

    ![Immagini delle opzioni Internet](media/MIM-DeploySP2.png)

6. Nella finestra **Intranet locale** fare clic su **Avanzate** e incollare l'URL copiato nella casella di testo **Aggiungi il sito Web all'area**. Fare clic su **Aggiungi**, quindi chiudere le finestre.

7. Aprire **Strumenti di amministrazione**, passare a **Servizi**, trovare il servizio di amministrazione di SharePoint e avviarlo, se non è già in esecuzione.

>[!div class="step-by-step"]  
[« SQL Server 2016](prepare-server-sql2016.md)
[Exchange Server »](prepare-server-exchange.md)
