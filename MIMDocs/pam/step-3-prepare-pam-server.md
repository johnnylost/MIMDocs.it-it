---
title: 'Distribuire PAM, passaggio 3: Server PAM | Documentazione Microsoft'
description: Preparare un server PAM in cui ospitare sia SQL che SharePoint per la distribuzione di Privileged Access Management.
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/15/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 68ec2145-6faa-485e-b79f-2b0c4ce9eff7
ROBOTS: noindex,nofollow
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 9a262a256062688542040827653a7df8d82e1044
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/13/2017
---
# Passaggio 3: preparare un server PAM
<a id="step-3--prepare-a-pam-server" class="xliff"></a>

>[!div class="step-by-step"]
[« Passaggio 2](step-2-prepare-priv-domain-controller.md)
[Passaggio 4 »](step-4-install-mim-components-on-pam-server.md)

## Installare Windows Server 2012 R2
<a id="install-windows-server-2012-r2" class="xliff"></a>
In una terza macchina virtuale, installare Windows Server 2012 R2, in particolare Windows Server 2012 R2 Standard (server con GUI) x64, per creare *PAMSRV*. Poiché SQL Server e SharePoint 2013 vengono installati sul computer sono richiesti almeno 8 GB di RAM.

1. Selezionare **Windows Server 2012 R2 Standard (server con GUI) x64**.

    ![Scegliere Windows Server Standard con GUI - Screenshot](media/PAM_GS_Select_WS2012.png)

2. Leggere e accettare le condizioni di licenza.

3.  Poiché il disco sarà vuoto, selezionare **Personalizzata: installa solo Windows** e usare lo **spazio su disco non inizializzato**.

4.  Accedere al nuovo computer come amministratore.  Tramite il Pannello di controllo, fornire al computer un indirizzo IP statico nella rete virtuale, configurare l'interfaccia di rete per l'invio di query DNS all'indirizzo IP del sistema PRIVDC e impostare il nome del computer su *PAMSRV*.  Sarà necessario riavviare il server.

5.  Se il computer si trova in una rete virtuale che non fornisce la connettività Internet, aggiungere un'altra interfaccia di rete al computer che fornisca una connessione a Internet.  L'interfaccia è necessaria per l'installazione di SharePoint e può essere disabilitata dopo aver completato questo passaggio.

6.  Dopo aver riavviato il server, accedere come amministratore. Utilizzando il Pannello di controllo, configurare il computer per verificare la necessità di installare eventuali aggiornamenti.  Potrebbe essere necessario il riavvio del server.

7.  Dopo il riavvio del server, accedere come amministratore, aprire il Pannello di controllo e aggiungere PAMSRV al dominio PRIV (priv.contoso.local).  Sarà necessario specificare il nome utente e le credenziali di amministratore del dominio PRIV (PRIV\Administrator). Una volta visualizzato il messaggio di benvenuto, chiudere la finestra di dialogo e riavviare nuovamente il server.


### Aggiungere i ruoli Server Web (IIS) e Server applicazioni
<a id="add-the-web-server-iis-and-application-server-roles" class="xliff"></a>
Aggiungere i ruoli Server Web (IIS) e Server applicazioni, le funzionalità .NET Framework 3.5, il modulo Active Directory per Windows PowerShell e altre funzionalità richieste da SharePoint.

1.  Accedere come amministratore del dominio PRIV (PRIV\Administrator) e avviare PowerShell.

2.  Digitare i comandi seguenti. Si noti che potrebbe essere necessario specificare un percorso diverso per i file di origine delle funzionalità .NET Framework 3.5. Queste funzionalità in genere non sono presenti quando si installa Windows Server, ma sono disponibili nella cartella affiancata (SxS) della cartella delle origini del disco di installazione del sistema operativo, ad esempio, "d:\Sources\SxS\".

    ```
    import-module ServerManager
    Install-WindowsFeature Web-WebServer, Net-Framework-Features,
    rsat-ad-powershell,Web-Mgmt-Tools,Application-Server,
    Windows-Identity-Foundation,Server-Media-Foundation,
    Xps-Viewer –includeallsubfeature -restart -source d:\sources\SxS
    ```

### Configurare i criteri di sicurezza server
<a id="configure-the-server-security-policy" class="xliff"></a>
Configurare i criteri di sicurezza del server per consentire agli account appena creati di essere eseguiti come servizi.

1.  Avviare il programma **Criteri di sicurezza locali** .   
2.  Passare a **Criteri locali** > **Assegnazione diritti utente**.  
3.  Nel riquadro dei dettagli, fare clic con il pulsante destro del mouse su **Accedi come servizio**e selezionare **Proprietà**.  
4.  Fare clic su **Aggiungi utente o gruppo** e in Nomi utenti e gruppi digitare *priv\mimmonitor; priv\MIMService; priv\SharePoint; priv\mimcomponent; priv\SqlServer*. Fare clic su **Controlla nomi** e quindi su **OK**.  

5.  Fare clic su **OK** per chiudere la finestra di dialogo Proprietà.  
6.  Nel riquadro Dettagli fare clic con il pulsante destro del mouse su **Nega accesso al computer della rete** e selezionare **Proprietà**.  
7.  Fare clic su **Aggiungi utente o gruppo** e in Nomi utenti e gruppi digitare *priv\mimmonitor; priv\MIMService; priv\mimcomponent* e fare clic su **OK**.  
8.  Fare clic su **OK** per chiudere la finestra di dialogo Proprietà.  

9. Nel riquadro Dettagli fare clic con il pulsante destro del mouse su **Nega accesso locale**, quindi selezionare **Proprietà**.  
10. Fare clic su **Aggiungi utente o gruppo** e in Nomi utenti e gruppi digitare *priv\mimmonitor; priv\MIMService; priv\mimcomponent* e fare clic su **OK**.  
11. Fare clic su **OK** per chiudere la finestra di dialogo Proprietà.  
12. Chiudere la finestra Criteri di sicurezza locali.  

13. Aprire il Pannello di controllo e passare a **Account utente**.  
14. Fare clic su **Concedi ad altri utenti l'accesso al computer**.  
15. Fare clic su **Aggiungi**, immettere il nome utente *MIMADMIN* nel dominio *PRIV* e, nella schermata successiva della procedura guidata, fare clic su **Add this user as an Administrator**  (Aggiungi utente come amministratore).  
16. Fare clic su **Aggiungi**, immettere il nome utente *SharePoint* nel dominio *PRIV* e, nella schermata successiva della procedura guidata, fare clic su **Add this user as an Administrator** (Aggiungi utente come amministratore).  
17. Chiudere il Pannello di controllo.  

### Modificare la configurazione di IIS
<a id="change-the-iis-configuration" class="xliff"></a>
Per modificare la configurazione di IIS per consentire alle applicazioni di usare la modalità di autenticazione di Windows, sono disponibili due opzioni. Assicurarsi di aver eseguito l'accesso come MIMAdmin e quindi seguire una di queste opzioni.

Se si vuole usare PowerShell:
1.  Fare clic con il pulsante destro del mouse su PowerShell e scegliere **Esegui come amministratore**.  
2.  Arrestare IIS e sbloccare le impostazioni dell'applicazione host utilizzando i comandi  
    ```
    iisreset /STOP
    C:\Windows\System32\inetsrv\appcmd.exe unlock config /section:windowsAuthentication -commit:apphost
    iisreset /START
    ```

Se si vuole usare un editor di testo, ad esempio il Blocco note:   
1. Aprire il file **C:\Windows\System32\inetsrv\config\applicationHost.config**   
2. Scorrere fino alla riga 82 del file. Il valore del tag di **overrideModeDefault** deve essere **<section name="windowsAuthentication" overrideModeDefault="Deny" />**  
3. Modificare il valore di **overrideModeDefault** su *Consenti*  
4. Salvare il file e riavviare IIS con il comando PowerShell `iisreset /START`

## Installare SQL Server
<a id="install-sql-server" class="xliff"></a>
Se SQL Server non è già nell'ambiente bastion, installare SQL Server 2012 (Service Pack 1 o versioni successive) o SQL Server 2014. I passaggi seguenti presuppongono la presenza di SQL 2014.

1. Assicurarsi di aver eseguito l'accesso come MIMAdmin.
2. Fare clic con il pulsante destro del mouse su PowerShell e scegliere **Esegui come amministratore**.   
3. Passare alla directory in cui si trova il programma di installazione di SQL Server.  
4. Digitare il comando seguente.  
    ```
    .\setup.exe /Q /IACCEPTSQLSERVERLICENSETERMS /ACTION=install /FEATURES=SQL,SSMS /INSTANCENAME=MSSQLSERVER /SQLSVCACCOUNT="PRIV\SqlServer" /SQLSVCPASSWORD="Pass@word1" /AGTSVCSTARTUPTYPE=Automatic /AGTSVCACCOUNT="NT AUTHORITY\Network Service" /SQLSYSADMINACCOUNTS="PRIV\MIMAdmin"
    ```

## Installare SharePoint Foundation 2013
<a id="install-sharepoint-foundation-2013" class="xliff"></a>

Utilizzando SharePoint Foundation 2013 con il programma di installazione di SP1, installare i prerequisiti software di SharePoint in PAMSRV.

> [!NOTE]
> Il programma di installazione richiede una connessione Internet per scaricare i prerequisiti. Dopo averli installati, il server verrà riavviato.

1. Fare clic con il pulsante destro del mouse su PowerShell e scegliere **Esegui come amministratore**.  
2. Passare alla directory in cui è stato decompresso SharePoint.  
3. Digitare il comando `.\prerequisiteinstaller.exe`.

Una volta installati i prerequisiti di SharePoint, installare SharePoint Foundation 2013 con SP1.

1.  Fare clic con il pulsante destro del mouse su PowerShell e scegliere **Esegui come amministratore**.  
2.  Passare alla directory in cui è stato decompresso SharePoint.  
3.  Digitare il comando `.\setup.exe`.  
4.  Selezionare il tipo di **server completo** .  
5.  Al termine dell'installazione, eseguire la procedura guidata.  

### Configurare SharePoint
<a id="configure-sharepoint" class="xliff"></a>
Eseguire la Configurazione guidata Prodotti SharePoint per configurare SharePoint.

1.  Nella scheda **Connessione a una server farm** modificare per creare una nuova server farm.  
2.  Specificare **PAMSRV** come server di database per il database di configurazione e **PRIV\SharePoint** come account di accesso del database affinché vengano usati da SharePoint.  
3.  Specificare una password come passphrase di sicurezza della farm (non verrà usata in seguito in questa procedura dettagliata).  
4.  Per il momento, accettare le altre impostazioni predefinite della configurazione guidata di SharePoint per creare una farm server singolo.    
5.  Quando la configurazione guidata completa l'attività 10 di 10, fare clic su **Fine** e verrà visualizzato un Web browser.  
6.  Eseguire l'autenticazione come amministratore di dominio PRIV\MIMAdmin in Internet Explorer per continuare.  
7.  Avviare la procedura guidata (all'interno dell'app Web) per configurare la farm di SharePoint.  
8.  Selezionare questa opzione per usare l'account gestito (PRIV\SharePoint), deselezionarla per disabilitare i servizi facoltativi e fare clic su **Avanti**.  
9. Nella finestra Creating a Site Collection (Creazione di una raccolta di siti) fare clic su **Skip** (Ignora) e quindi su **Finish** (Fine).  

## Creare un'applicazione Web SharePoint Foundation 2013
<a id="create-a-sharepoint-foundation-2013-web-application" class="xliff"></a>
Dopo aver completato la procedura guidata, usare PowerShell per creare un'applicazione Web SharePoint Foundation 2013 che funzioni da host per il portale MIM. Poiché questa procedura dettagliata ha esclusivamente uno scopo dimostrativo, SSL non verrà abilitato.

1.  Fare clic con il pulsante destro del mouse su Shell di gestione SharePoint 2013, selezionare **Esegui come amministratore** ed eseguire lo script di PowerShell seguente:

    ```
    $dbManagedAccount = Get-SPManagedAccount -Identity PRIV\SharePoint
    New-SpWebApplication -Name "MIM Portal" -ApplicationPool "MIMAppPool"            -ApplicationPoolAccount $dbManagedAccount -AuthenticationMethod "Kerberos" -Port 82 -URL http://PAMSRV.priv.contoso.local
    ```

2. Verrà visualizzato un messaggio di avviso in cui si comunica che viene usato il metodo di autenticazione classico di Windows e che la restituzione del comando finale può richiedere alcuni minuti.  Al termine, l'output restituirà l'URL del nuovo portale.

> [!NOTE]
> Mantenere la finestra Shell di gestione SharePoint 2013 aperta per usarla in un passaggio successivo.

## Creare una raccolta siti di SharePoint
<a id="create-a-sharepoint-site-collection" class="xliff"></a>
Successivamente, creare una Raccolta siti SharePoint associata all’applicazione Web per funzionare da host al Portale MIM.

1.  Avviare la finestra **Shell di gestione SharePoint 2013**, se non è già aperta, ed eseguire lo script PowerShell seguente.

    ```
    $t = Get-SPWebTemplate -compatibilityLevel 14 -Identity "STS#1"
    $w = Get-SPWebApplication http://pamsrv.priv.contoso.local:82
    New-SPSite -Url $w.Url -Template $t -OwnerAlias PRIV\MIMAdmin                -CompatibilityLevel 14 -Name "MIM Portal" -SecondaryOwnerAlias PRIV\BackupAdmin
    $s = SpSite($w.Url)
    $s.AllowSelfServiceUpgrade = $false
    $s.CompatibilityLevel
    ```

    Verificare che la variabile **CompatibilityLevel** sia impostata su *14*. Se viene restituito *15*, la raccolta siti non è stata creata per la versione 2010. Eliminare la raccolta siti e ricrearla.

2.  Eseguire il comando di PowerShell seguente dalla **Shell di gestione SharePoint 2013**. In questo modo, viene disabilitato SharePoint Server-Side Viewstate e l'attività di SharePoint **Processo di analisi integrità (Ogni ora, Timer di Microsoft SharePoint Foundation, Tutti i server)**.

    ```
    $contentService = [Microsoft.SharePoint.Administration.SPWebService]::ContentService;
    $contentService.ViewStateOnServer = $false;
    $contentService.Update();
    Get-SPTimerJob hourly-all-sptimerservice-health-analysis-job | disable-SPTimerJob
    ```

## Modificare le impostazioni di aggiornamento
<a id="change-update-settings" class="xliff"></a>

1. Aprire il Pannello di controllo, passare a **Windows Update** e fare clic su **Modifica impostazioni**.  
2. Modificare le impostazioni per ricevere gli aggiornamenti di Windows Update e altri prodotti dagli aggiornamenti Microsoft.  
3. Prima di continuare, verificare la disponibilità di nuovi aggiornamenti e che eventuali aggiornamenti importanti in sospeso siano installati.

## Impostare il sito Web come Intranet locale
<a id="set-the-website-as-the-local-intranet" class="xliff"></a>

1. Avviare Internet Explorer e aprire una nuova scheda del Web browser.
2. Passare a http://pamsrv.priv.contoso.local:82/ and sign in as PRIV\MIMAdmin.  Verrà visualizzato un sito SharePoint denominato "Portale MIM".  
3. In Internet Explorer aprire **Opzioni Internet**, modificare la scheda **Sicurezza**, selezionare **Intranet locale** e aggiungere l'URL `http://pamsrv.priv.contoso.local:82/`.

Se l'accesso ha esito negativo, è possibile che sia necessario aggiornare i nomi SPN Kerberos creati in precedenza nel [Passaggio 2](step-2-prepare-priv-domain-controller.md).

## Avviare il servizio di amministrazione di SharePoint
<a id="start-the-sharepoint-administration-service" class="xliff"></a>

Usando **Servizi**, in Strumenti di amministrazione, avviare il servizio **Amministrazione SharePoint**, se non è già in esecuzione.

Nel passaggio 4 sarà possibile iniziare l'installazione dei componenti MIM nel server PAM.

>[!div class="step-by-step"]
[« Passaggio 2](step-2-prepare-priv-domain-controller.md)
[Passaggio 4 »](step-4-install-mim-components-on-pam-server.md)
