---
title: Configurare Windows Server 2012 R2 per MIM 2016 | Documentazione Microsoft
description: Ottenere i passaggi e i requisiti minimi per preparare Windows Server 2012 RS all'uso con MIM 2016.
keywords: 
author: billmath
ms.author: barclayn
manager: mbaldwin
ms.date: 10/12/2017
ms.topic: get-started-article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 51507d0a-2aeb-4cfd-a642-7c71e666d6cd
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 76a59292da97583887020c89025add77c7a64c80
ms.sourcegitcommit: f077508b5569e2a96084267879c5b6551e1e0905
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/12/2017
---
# <a name="set-up-an-identity-management-server-windows-server-2012-r2"></a>Configurare un server di gestione delle identità: Windows Server 2012 R2

>[!div class="step-by-step"]
[« Preparazione di un dominio](preparing-domain.md)
[SQL Server 2014 »](prepare-server-sql2014.md)

> [!NOTE]
> Questa procedura dettagliata usa nomi e valori di esempio della società Contoso. Sostituirli con i propri nomi e valori. Ad esempio:
> - Nome del controller di dominio: **mimservername**
> - Nome del dominio: **contoso**
> - Password: **Pass@word1**

## <a name="join-windows-server-2012-r2-to-your-domain"></a>Aggiungere Windows Server 2012 R2 al dominio

Iniziare con una macchina Windows Server 2012 R2, con un minimo di 8 GB di RAM. Al momento dell’installazione, specificare la versione "Windows Server 2012 R2 Standard (server con un’interfaccia utente grafica) x64".

1. Accedere al nuovo computer come amministratore.

2. Usando il pannello di controllo, assegnare al computer un indirizzo IP statico nella rete. Configurare l'interfaccia di rete per l'invio di query DNS all'indirizzo IP del controller di dominio indicato nel passaggio precedente e impostare il nome del computer su **CORPIDM**.  Sarà necessario riavviare il server.

3. Aprire il pannello di controllo e aggiungere il computer al dominio che è stato configurato nel passaggio precedente, *contoso.local*.  È necessario fornire il nome utente e le credenziali di amministratore di dominio, ad esempio *Contoso\Administrator*.  Una volta visualizzato il messaggio di benvenuto, chiudere la finestra di dialogo e riavviare nuovamente il server.

4. Accedere al computer *CorpIDM* come amministratore di dominio, ad esempio *Contoso\Administrator*.

5. Avviare una finestra di PowerShell come amministratore e digitare il comando seguente per aggiornare il computer in base alle impostazioni dei criteri di gruppo.

    ```
    gpupdate /force /target:computer
    ```

    Dopo non oltre un minuto, l'operazione verrà completata e verrà visualizzato il messaggio "Aggiornamento dei criteri computer completato".

6. Aggiungere i ruoli **Server Web (IIS)** e **Server applicazioni**, le funzionalità **.NET Framework** 3.5, 4.0 e 4.5, e il **modulo Active Directory per Windows PowerShell**.

    ![Immagine delle funzionalità di PowerShell](media/MIM-DeployWS2.png)

7. In PowerShell digitare i seguenti comandi. Si noti che potrebbe essere necessario specificare un percorso diverso per i file di origine delle funzionalità **.NET Framework** 3.5. Queste funzionalità in genere non sono presenti quando si installa Windows Server, ma sono disponibili nella cartella affiancata (SxS) della cartella delle origini del disco di installazione del sistema operativo, ad esempio, "*d:\Sources\SxS\*".

    ```
    import-module ServerManager
    Install-WindowsFeature Web-WebServer, Net-Framework-Features,rsat-ad-powershell,Web-Mgmt-Tools,Application-Server,Windows-Identity-Foundation,Server-Media-Foundation,Xps-Viewer –includeallsubfeature -restart -source d:\sources\SxS
    ```

## <a name="configure-the-server-security-policy"></a>Configurare i criteri di sicurezza server

Configurare i criteri di sicurezza del server per consentire l'esecuzione come servizi degli account appena creati.

1. Avviare il programma Criteri di protezione locali

2. Passare a **Criteri locali > Assegnazione diritti utente**.

3. Nel riquadro dei dettagli, fare clic con il pulsante destro del mouse su **Accedi come servizio**e selezionare **Proprietà**.

    ![Immagine dei criteri di sicurezza locali](media/MIM-DeployWS3.png)

4. Fare clic su **Aggiungi utente o gruppo** e nella casella di testo digitare `contoso\MIMSync; contoso\MIMMA; contoso\MIMService; contoso\SharePoint; contoso\SqlServer; contoso\MIMSSPR`, fare clic su **Controlla nomi**, quindi fare clic su **OK**.

5. Fare clic su **OK** per chiudere la finestra delle **Proprietà relative ad Accedi come servizio**.

6.  Nel riquadro Dettagli fare clic con il pulsante destro del mouse su **Nega accesso al computer della rete** e selezionare **Proprietà**.

7. Fare clic su **Aggiungi utente o gruppo** e nella casella di testo digitare `contoso\MIMSync; contoso\MIMService`, quindi fare clic su **OK**.

8. Fare clic su **OK** per chiudere la finestra **Proprietà di Nega accesso al computer della rete**.

9. Nel riquadro Dettagli fare clic con il pulsante destro del mouse su **Nega accesso locale**, quindi selezionare **Proprietà**.

10. Fare clic su **Aggiungi utente o gruppo** e nella casella di testo digitare `contoso\MIMSync; contoso\MIMService`, quindi fare clic su **OK**.

11. Fare clic su **OK** per chiudere la finestra delle **Proprietà relativa all'opzione Nega accesso locale**.

12. Chiudere la finestra Criteri di sicurezza locali.


## <a name="change-the-iis-windows-authentication-mode"></a>Cambiare la modalità Autenticazione di Windows di IIS

1.  Aprire una finestra di PowerShell.

2.  Interrompere IIS con il comando *iisreset /STOP*

    ```
    iisreset /STOP
    C:\Windows\System32\inetsrv\appcmd.exe unlock config /section:windowsAuthentication -commit:apphost
    iisreset /START
    ```

>[!div class="step-by-step"]  
[« Preparazione di un dominio](preparing-domain.md)
[SQL Server 2014 »](prepare-server-sql2014.md)
