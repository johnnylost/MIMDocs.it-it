---
# required metadata

title: Configurare un server di gestione delle identità&#58; Windows Server 2012 R2 | Microsoft Identity Manager
description: Ottenere i passaggi e i requisiti minimi per preparare Windows Server 2012 RS all'uso con MIM 2016.
keywords:
author: kgremban
manager: stevenpo
ms.date: 04/28/2016
ms.topic: get-started-article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 51507d0a-2aeb-4cfd-a642-7c71e666d6cd

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Configurare un server di gestione delle identità: Windows Server 2012 R2

>[!div class="step-by-step"]

> [!NOTE]
> « Preparazione di un dominio SQL Server 2014 » Questa procedura dettagliata usa nomi e valori di esempio della società Contoso.
> - Sostituirli con i propri nomi e valori.
> - Ad esempio:
> - Nome del controller di dominio: **mimservername**

## Nome del dominio: **contoso**

Password: **Pass@word1** Aggiungere Windows Server 2012 R2 al dominio

1. Iniziare con una macchina Windows Server 2012 R2, con un minimo di 8 GB di RAM.

2. Al momento dell’installazione, specificare la versione "Windows Server 2012 R2 Standard (server con un’interfaccia utente grafica) x64". Accedere al nuovo computer come amministratore.  Usando il pannello di controllo, assegnare al computer un indirizzo IP statico nella rete.

3. Configurare l'interfaccia di rete per l'invio di query DNS all'indirizzo IP del controller di dominio indicato nel passaggio precedente e impostare il nome del computer su **CORPIDM**.  Sarà necessario riavviare il server.  Aprire il pannello di controllo e aggiungere il computer al dominio che è stato configurato nel passaggio precedente, *contoso.local*.

4. È necessario fornire il nome utente e le credenziali di amministratore di dominio, ad esempio *Contoso\Administrator*.

5. Una volta visualizzato il messaggio di benvenuto, chiudere la finestra di dialogo e riavviare nuovamente il server.

    ```
    gpupdate /force /target:computer
    ```

    Accedere al computer *CorpIDM* come amministratore di dominio, ad esempio *Contoso\Administrator*.

6. Avviare una finestra di PowerShell come amministratore e digitare il comando seguente per aggiornare il computer in base alle impostazioni dei criteri di gruppo.

    ![Dopo non oltre un minuto, l'operazione verrà completata e verrà visualizzato il messaggio "Aggiornamento dei criteri computer completato".](media/MIM-DeployWS2.png)

7. Aggiungere i ruoli **Server Web (IIS)** e **Server applicazioni**, le funzionalità **.NET Framework** 3.5, 4.0 e 4.5, e il **modulo Active Directory per Windows PowerShell**. Immagine delle funzionalità di PowerShell In PowerShell digitare i seguenti comandi.

    ```
    import-module ServerManager
    Install-WindowsFeature Web-WebServer, Net-Framework-Features,rsat-ad-powershell,Web-Mgmt-Tools,Application-Server,Windows-Identity-Foundation,Server-Media-Foundation,Xps-Viewer –includeallsubfeature -restart -source d:\sources\SxS
    ```

## Si noti che potrebbe essere necessario specificare un percorso diverso per i file di origine delle funzionalità **.NET Framework** 3.5.

Queste funzionalità in genere non sono presenti quando si installa Windows Server, ma sono disponibili nella cartella affiancata (SxS) della cartella delle risorse del disco di installazione del sistema operativo, ad esempio, "*d:\Sources\SxS\*".

1. Configurare i criteri di sicurezza server

2. Configurare i criteri di sicurezza del server per consentire l'esecuzione come servizi degli account appena creati.

3. Avviare il programma Criteri di protezione locali

    ![Passare a **Criteri locali > Assegnazione diritti utente**.](media/MIM-DeployWS3.png)

4. Nel riquadro dei dettagli, fare clic con il pulsante destro del mouse su **Accedi come servizio**e selezionare **Proprietà**.

5. Immagine dei criteri di sicurezza locali

6.  Fare clic su **Aggiungi utente o gruppo** e nella casella di testo digitare `contoso\mimsync; contoso\mimma; contoso\MIMService; contoso\SharePoint; contoso\SqlServer; contoso\mimsspr`, fare clic su **Controlla nomi**, quindi fare clic su **OK**.

7. Fare clic su **OK** per chiudere la finestra delle **Proprietà relative ad Accedi come servizio**.

8. Nel riquadro Dettagli fare clic con il pulsante destro del mouse su **Nega accesso al computer della rete** e selezionare **Proprietà**.

9. Fare clic su **Aggiungi utente o gruppo** e nella casella di testo digitare `contoso\MIMSync; contoso\MIMService`, quindi fare clic su **OK**.

10. Fare clic su **OK** per chiudere la finestra **Proprietà di Nega accesso al computer della rete**.

11. Nel riquadro Dettagli fare clic con il pulsante destro del mouse su **Nega accesso locale**, quindi selezionare **Proprietà**.

12. Fare clic su **Aggiungi utente o gruppo** e nella casella di testo digitare `contoso\MIMSync; contoso\MIMService`, quindi fare clic su **OK**.


## Fare clic su **OK** per chiudere la finestra delle **Proprietà relativa all'opzione Nega accesso locale**.

1.  Chiudere la finestra Criteri di sicurezza locali.

2.  Cambiare la modalità Autenticazione di Windows di IIS

    ```
    iisreset /STOP
    C:\Windows\System32\inetsrv\appcmd.exe unlock config /section:windowsAuthentication -commit:apphost
    iisreset /START
    ```

>Aprire una finestra di PowerShell.  
Interrompere IIS con il comando *iisreset /STOP*


<!--HONumber=Apr16_HO4-->


