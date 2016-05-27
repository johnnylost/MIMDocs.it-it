---
# required metadata

title: Configurare un server di gestione delle identità&#58; SharePoint | Microsoft Identity Manager
description: Installare e configurare SharePoint Foundation in modo che possa ospitare la pagina del portale MIM.
keywords:
author: kgremban
manager: stevenpo
ms.date: 04/28/2016
ms.topic: get-started-article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: c01487f2-3de6-4fc4-8c3a-7d62f7c2496c

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Configurare un server di gestione delle identità: SharePoint

>[!div class="step-by-step"]

> [!NOTE]
> « SQL Server 2014 Exchange Server » Questa procedura dettagliata usa nomi e valori di esempio della società Contoso.
> - Sostituirli con i propri nomi e valori.
> - Ad esempio:
> - Nome del controller di dominio: **mimservername**


## Nome del dominio: **contoso**

> [!NOTE]
> Password: **Pass@word1** Installare **SharePoint Foundation 2013 con SP1** Il programma di installazione richiede una connessione Internet per scaricare i relativi prerequisiti.

Se il computer è in una rete virtuale che non fornisce la connettività Internet, aggiungere un'altra interfaccia di rete al computer che fornisce una connessione a Internet. L'interfaccia può essere disabilitata al termine dell'installazione.

1.  Seguire questa procedura per installare SharePoint Foundation 2013 SP1.

    -   Al termine dell'installazione, il server verrà riavviato.

    -   Avviare **PowerShell** come amministratore di dominio.

        ```
        .\prerequisiteinstaller.exe
        ```

2.  Passare alla directory in cui è stato decompresso SharePoint.

    ```
    .\setup.exe
    ```

3.  Digitare il comando seguente.

4.  Dopo aver installato i prerequisiti di **SharePoint** , installare **SharePoint Foundation 2013 con SP1** immettendo il seguente comando:

## Selezionare il tipo di server completo.

Al termine dell'installazione, eseguire la procedura guidata.

1. Eseguire la procedura guidata per configurare SharePoint

2. Seguire i passaggi descritti in **Configurazione guidata Prodotti SharePoint** per configurare SharePoint per l'uso con MIM.

3. Nella scheda **Connessione a una server farm** , modificare per creare una nuova server farm.

4. Specificare questo server come server di database per il database di configurazione e *Contoso\SharePoint* come account di accesso del database che verrà usato da SharePoint.

5. Creare una password per la passphrase di sicurezza della farm.

6. Al termine della configurazione guidata dell’attività 10 di 10, fare clic su Fine. Si aprirà un Web browser.

7. Nella finestra popup di Internet Explorer, eseguire l'autenticazione come *Contoso\Administrator* (o con l'account amministratore di dominio equivalente) per continuare.

8. Avviare la procedura guidata (all'interno dell'app Web) per configurare la farm di SharePoint.  Selezionare l'opzione per usare l'account gestito esistente (*Contoso\SharePoint*) e fare clic su **Avanti**.

## Nella finestra **Creazione di una raccolta di siti** , fare clic su **Ignora**.

> [!NOTE]
> Quindi fare clic su **Fine**. Preparare SharePoint per ospitare il portale MIM

1. Inizialmente, SSL non verrà configurato.

    ```
    $dbManagedAccount = Get-SPManagedAccount -Identity contoso\SharePoint
    New-SpWebApplication -Name "MIM Portal" -ApplicationPool "MIMAppPool"
    -ApplicationPoolAccount $dbManagedAccount -AuthenticationMethod "Kerberos" -Port 82 -URL http://corpidm.contoso.local
    ```

    > [!NOTE] Assicurarsi di configurare SSL o equivalente prima di abilitare l'accesso a questo portale. Avviare **SharePoint 2013 Management Shell** ed eseguire il seguente script di PowerShell per creare un'**applicazione Web SharePoint Foundation 2013**. Verrà visualizzato un messaggio di avviso in cui si comunica che viene usato il metodo di autenticazione classico di Windows e che la restituzione del comando finale potrebbe richiedere alcuni minuti.

2. Al termine, l'output indicherà l'URL del nuovo portale.

  ```
  $t = Get-SPWebTemplate -compatibilityLevel 14 -Identity "STS#1"
  $w = Get-SPWebApplication http://corpidm.contoso.local:82
  New-SPSite -Url $w.Url -Template $t -OwnerAlias contoso\Administrator
  -CompatibilityLevel 14 -Name "MIM Portal" -SecondaryOwnerAlias contoso\BackupAdmin
  $s = SpSite($w.Url)
  $s.AllowSelfServiceUpgrade = $false
  $s.CompatibilityLevel
  ```

  > [!NOTE] Mantenere la finestra **SharePoint 2013 Management Shell** aperta per un secondo momento. Avviare SharePoint 2013 Management Shell ed eseguire il seguente script di PowerShell per creare una **Raccolta siti di SharePoint** associata all'applicazione Web.

3. Verificare che il risultato della variabile *CompatibilityLevel* sia “14”.

  ```
  $contentService = [Microsoft.SharePoint.Administration.SPWebService]::ContentService;
  $contentService.ViewStateOnServer = $false;
  $contentService.Update();
  Get-SPTimerJob hourly-all-sptimerservice-health-analysis-job | disable-SPTimerJob
  ```

4. Se il risultato è "15", la raccolta siti non è stata creata per la versione 2010. Eliminare la raccolta siti e ricrearla.  Disabilitare **SharePoint Server-Side Viewstate** e l'attività di SharePoint "Processo di analisi integrità (Ogni ora, Timer di Microsoft SharePoint Foundation, Tutti i server)" eseguendo i comandi PowerShell seguenti in **Shell di gestione SharePoint 2013**:

    ![Nel server di gestione delle identità aprire una nuova scheda del Web browser, passare a http://localhost:82/ e accedere come *contoso\Administrator*.](media/MIM-DeploySP1.png)

5. Verrà visualizzato un sito SharePoint denominato *Portale MIM* .

    ![Immagine del portale MIM all'indirizzo http://localhost:82/](media/MIM-DeploySP2.png)

6. Copiare l'URL, quindi in Internet Explorer aprire **Opzioni Internet**, passare alla scheda **Sicurezza**, selezionare **Intranet locale** e fare clic su **Siti**. Immagini delle opzioni Internet

7. Nella finestra **Intranet locale** fare clic su **Avanzate** e incollare l'URL copiato nella casella di testo **Aggiungi il sito Web all'area**.

>Fare clic su **Aggiungi**, quindi chiudere le finestre.  
Aprire **Strumenti di amministrazione**, passare a **Servizi**, trovare il servizio di amministrazione di SharePoint e avviarlo, se non è già in esecuzione.


<!--HONumber=Apr16_HO3-->


