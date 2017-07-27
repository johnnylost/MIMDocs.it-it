---
title: Configurare SharePoint per Microsoft Identity Manager 2016 | Documentazione Microsoft
description: Installare e configurare SharePoint Foundation in modo che possa ospitare la pagina del portale MIM.
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/23/2017
ms.topic: get-started-article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: c01487f2-3de6-4fc4-8c3a-7d62f7c2496c
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 1114be2ce13ca012582676803eb1dc29cadae596
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/13/2017
---
# <a name="set-up-an-identity-management-server-sharepoint"></a>Configurare un server di gestione delle identità: SharePoint

>[!div class="step-by-step"]
[« SQL Server 2014](prepare-server-sql2014.md)
[Exchange Server »](prepare-server-exchange.md)

> [!NOTE]
> Questa procedura dettagliata usa nomi e valori di esempio della società Contoso. Sostituirli con i propri nomi e valori. Ad esempio:
> - Nome del controller di dominio: **mimservername**
> - Nome del dominio: **contoso**
> - Password: **Pass@word1**


## <a name="install-sharepoint-foundation-2013-with-sp1"></a>Installare **SharePoint Foundation 2013 con SP1**

> [!NOTE]
> Il programma di installazione richiede una connessione Internet per scaricare i relativi prerequisiti. Se il computer è in una rete virtuale che non fornisce la connettività Internet, aggiungere un'altra interfaccia di rete al computer che fornisce una connessione a Internet. L'interfaccia può essere disabilitata al termine dell'installazione.

Seguire questa procedura per installare SharePoint Foundation 2013 SP1. Al termine dell'installazione, il server verrà riavviato.

1.  Avviare **PowerShell** come amministratore di dominio.

    -   Passare alla directory in cui è stato decompresso SharePoint.

    -   Digitare il comando seguente.

        ```
        .\prerequisiteinstaller.exe
        ```

2.  Dopo aver installato i prerequisiti di **SharePoint** , installare **SharePoint Foundation 2013 con SP1** immettendo il seguente comando:

    ```
    .\setup.exe
    ```

3.  Selezionare il tipo di server completo.

4.  Al termine dell'installazione, eseguire la procedura guidata.

## <a name="run-the-wizard-to-configure-sharepoint"></a>Eseguire la procedura guidata per configurare SharePoint

Seguire i passaggi descritti in **Configurazione guidata Prodotti SharePoint** per configurare SharePoint per l'uso con MIM.

1. Nella scheda **Connessione a una server farm** , modificare per creare una nuova server farm.

2. Specificare questo server come server di database per il database di configurazione e *Contoso\SharePoint* come account di accesso del database che verrà usato da SharePoint.

3. Creare una password per la passphrase di sicurezza della farm.

4. Al termine della configurazione guidata dell’attività 10 di 10, fare clic su Fine. Si aprirà un Web browser.

5. Nella finestra popup di Internet Explorer, eseguire l'autenticazione come *Contoso\Administrator* (o con l'account amministratore di dominio equivalente) per continuare.

6. Avviare la procedura guidata (all'interno dell'app Web) per configurare la farm di SharePoint.

7. Selezionare l'opzione per usare l'account gestito esistente (*Contoso\SharePoint*) e fare clic su **Avanti**.

8. Nella finestra **Creazione di una raccolta di siti** , fare clic su **Ignora**.  Quindi fare clic su **Fine**.

## <a name="prepare-sharepoint-to-host-the-mim-portal"></a>Preparare SharePoint per ospitare il portale MIM

> [!NOTE]
> Inizialmente, SSL non verrà configurato. Assicurarsi di configurare SSL o equivalente prima di abilitare l'accesso a questo portale.

1. Avviare **SharePoint 2013 Management Shell** ed eseguire il seguente script di PowerShell per creare un'**applicazione Web SharePoint Foundation 2013**.

    ```
    $dbManagedAccount = Get-SPManagedAccount -Identity contoso\SharePoint
    New-SpWebApplication -Name "MIM Portal" -ApplicationPool "MIMAppPool" -ApplicationPoolAccount $dbManagedAccount -AuthenticationMethod "Kerberos" -Port 82 -URL http://corpidm.contoso.local
    ```

    > [!NOTE]
    > Verrà visualizzato un messaggio di avviso in cui si comunica che viene usato il metodo di autenticazione classico di Windows e che la restituzione del comando finale potrebbe richiedere alcuni minuti. Al termine, l'output indicherà l'URL del nuovo portale. Mantenere la finestra **SharePoint 2013 Management Shell** aperta per un secondo momento.

2. Avviare SharePoint 2013 Management Shell ed eseguire il seguente script di PowerShell per creare una **Raccolta siti di SharePoint** associata all'applicazione Web.

  ```
  $t = Get-SPWebTemplate -compatibilityLevel 14 -Identity "STS#1"
  $w = Get-SPWebApplication http://corpidm.contoso.local:82
  New-SPSite -Url $w.Url -Template $t -OwnerAlias contoso\Administrator
  -CompatibilityLevel 14 -Name "MIM Portal" -SecondaryOwnerAlias contoso\BackupAdmin
  $s = SpSite($w.Url)
  $s.AllowSelfServiceUpgrade = $false
  $s.CompatibilityLevel
  ```

  > [!NOTE]
  > Verificare che il risultato della variabile *CompatibilityLevel* sia “14”. Se il risultato è "15", la raccolta siti non è stata creata per la versione 2010. Eliminare la raccolta siti e ricrearla.

3. Disabilitare **SharePoint Server-Side Viewstate** e l'attività di SharePoint "Processo di analisi integrità (Ogni ora, Timer di Microsoft SharePoint Foundation, Tutti i server)" eseguendo i comandi PowerShell seguenti in **Shell di gestione SharePoint 2013**:

  ```
  $contentService = [Microsoft.SharePoint.Administration.SPWebService]::ContentService;
  $contentService.ViewStateOnServer = $false;
  $contentService.Update();
  Get-SPTimerJob hourly-all-sptimerservice-health-analysis-job | disable-SPTimerJob
  ```

4. Nel server di gestione delle identità aprire una nuova scheda del Web browser, passare a http://localhost:82/ e accedere come *contoso\Administrator*.  Verrà visualizzato un sito SharePoint denominato *Portale MIM* .

    ![Immagine del portale MIM all'indirizzo http://localhost:82/](media/MIM-DeploySP1.png)

5. Copiare l'URL, quindi in Internet Explorer aprire **Opzioni Internet**, passare alla scheda **Sicurezza**, selezionare **Intranet locale** e fare clic su **Siti**.

    ![Immagini delle opzioni Internet](media/MIM-DeploySP2.png)

6. Nella finestra **Intranet locale** fare clic su **Avanzate** e incollare l'URL copiato nella casella di testo **Aggiungi il sito Web all'area**. Fare clic su **Aggiungi**, quindi chiudere le finestre.

7. Aprire **Strumenti di amministrazione**, passare a **Servizi**, trovare il servizio di amministrazione di SharePoint e avviarlo, se non è già in esecuzione.

>[!div class="step-by-step"]  
[« SQL Server 2014](prepare-server-sql2014.md)
[Exchange Server »](prepare-server-exchange.md)
