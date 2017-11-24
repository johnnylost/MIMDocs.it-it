---
title: Utilizzo del portale per la reimpostazione self-service della password | Documentazione Microsoft
description: "Vedere le novità relative alla reimpostazione della password self-service in MIM 2016, tra cui il funzionamento di SSPR con l'autenticazione a più fattori."
keywords: 
author: billmath
ms.author: barclayn
manager: mbaldwin
ms.date: 10/12/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 94a74f1c-2192-4748-9a25-62a526295338
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 0463a91275f3e181a66eb460c167bb9a2008f444
ms.sourcegitcommit: 27a23142393bbb0f66a3d533d89a5a8366a29e41
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/20/2017
---
>[!IMPORTANT]
In seguito all'annuncio della deprecazione di Azure Multi-Factor Authentication Software Development Kit. Azure MFA SDK sarà supportato per i clienti esistenti fino alla data di ritiro, ovvero 14 novembre 2018. I nuovi clienti e i clienti attuali non saranno in grado di scaricare più l'SDK tramite il portale di Azure classico. Per scaricarlo sarà necessario rivolgersi al supporto tecnico clienti di Azure per ricevere il pacchetto di credenziali del servizio MFA generato. <br> Il team di sviluppo Microsoft sta pianificando modifiche per MFA tramite l'integrazione di MFA Server SDK. Tali modifiche verranno incluse in un prossimo hotfix agli inizi del 2018.

# <a name="working-with-self-service-password-reset"></a>Uso della reimpostazione della password self-service
Microsoft Identity Manager 2016 fornisce funzionalità aggiuntive alla funzionalità di reimpostazione della password self service. Questa funzionalità è stata migliorata con alcune importanti funzionalità:

-   Il portale di reimpostazione della password self-service e la schermata di accesso di Windows ora consentono agli utenti di sbloccare i propri account senza modificare le password o richiedere il supporto degli amministratori. Gli account degli utenti vengono bloccati per diversi motivi legittimi, ad esempio perché immettono una vecchia password, usano computer bilingue con la tastiera impostata su una lingua non corretta o provano ad accedere a una workstation condivisa già aperta per l'account di un altro utente.

-   È stato aggiunto un nuovo controllo di autenticazione, il controllo tramite telefono, che consente di effettuare l'autenticazione utente tramite chiamata telefonica.

-   È stato aggiunto il supporto per il servizio di autenticazione a più fattori di Microsoft Azure. Può essere usato per il controllo password monouso con SMS e per il nuovo controllo del telefono.

## <a name="azure-for-multi-factor-authentication"></a>Azure per Multi-Factor Authentication
Microsoft Azure Multi-Factor Authentication è un servizio di autenticazione che richiede agli utenti di verificare i tentativi di accesso con un'app per dispositivi mobili, una chiamata telefonica o un SMS. È disponibile per l'uso con Microsoft Azure Active Directory e come servizio per le applicazioni aziendali cloud e locali.

Azure MFA fornisce un meccanismo di autenticazione aggiuntivo che è possibile integrare nei processi di autenticazione esistenti per rafforzarli, ad esempio in quello effettuato da MIM per l'assistenza per l'accesso self-service.

Quando si usa Azure MFA, gli utenti vengono autenticati nel sistema per verificarne l'identità quando provano a riottenere l'accesso al proprio account e alle risorse. È possibile eseguire l'autenticazione tramite SMS o chiamata telefonica.   Più forte è l'autenticazione, maggiore sarà la sicurezza che la persona che sta provando ad accedere sia realmente l'utente titolare dell'identità. Una volta autenticato, l'utente può scegliere una nuova password per sostituire quella precedente.

## <a name="prerequisites-to-set-up-self-service-account-unlock-and-password-reset-using-mfa"></a>Prerequisiti per configurare lo sblocco degli account self-service e la reimpostazione della password usando MFA
In questa sezione si presuppone di aver scaricato e completato la distribuzione di Microsoft Identity Manager 2016, inclusi i seguenti componenti e servizi:

-   Un computer in cui è installato Windows Server 2008 R2 o versioni successive è stato configurato come server Active Directory che include Servizi di dominio Active Directory e un controller di dominio con un dominio designato (un dominio "aziendale")

-   Un criterio di gruppo è definito per il blocco degli account

-   Il servizio di sincronizzazione MIM 2016 (Sincronizzazione) deve essere installato e in esecuzione in un server con dominio appartenente al dominio di Active Directory

-   Il servizio e il portale MIM 2016, incluso il portale di registrazione SSPR e il portale di reimpostazione SSPR, sono installati e in esecuzione su un server (possono condividere il percorso di Sincronizzazione)

-   Sincronizzazione MIM è configurato per la sincronizzazione delle identità MIM di Active Directory, tra cui:

    -   Configurazione dell’agente di gestione di Active Directory (ADMA) per la connettività con Active Directory DS e la capacità di importare i dati di identità ed esportarli in Active Directory.

    -   Configurazione dell'agente di gestione MIM (MIM MA) per la connettività con il database del servizio FIM e la funzionalità di importazione dei dati di identità dal database FIM e di esportazione in tale database.

    -   Configurazione delle regole di sincronizzazione nel portale MIM per consentire la sincronizzazione dei dati utente e semplificare le attività basate sulla sincronizzazione nel servizio MIM.

-   I componenti aggiuntivi e le estensioni di MIM 2016, incluso il client integrato di accesso SSPR Windows, possono quindi essere distribuiti nel server o in un computer client separato.

## <a name="prepare-mim-to-work-with-multi-factor-authentication"></a>Preparare MIM all'uso dell'autenticazione a più fattori
Configurare la sincronizzazione MIM per supportare la funzionalità di reimpostazione della password e sblocco dell’account. Per altre informazioni, vedere [Installazione dei componenti aggiuntivi e delle estensioni di FIM](https://technet.microsoft.com/library/ff512688%28v=ws.10%29.aspx), [installazione di FIM SSPR](https://technet.microsoft.com/library/hh322891%28v=ws.10%29.aspx), [Controlli di autenticazione SSPR](https://technet.microsoft.com/library/jj134288%28v=ws.10%29.aspx) e [Guida al lab di test SSPR](https://technet.microsoft.com/library/hh826057%28v=ws.10%29.aspx)

Nella sezione successiva si imposterà il provider Azure MFA in Microsoft Azure Active Directory. Come parte di questa operazione, verrà generato un file che include il materiale di autenticazione richiesto da MFA per contattare Azure MFA.  Per continuare, è necessaria una sottoscrizione di Azure.

### <a name="register-your-multi-factor-authentication-provider-in-azure"></a>Registrare il provider di autenticazione a più fattori in Azure

1.  Andare al [portale di Azure classico](http://manage.windowsazure.com) e accedere come amministratore della sottoscrizione di Azure.

2.  Nell'angolo inferiore sinistro, fare clic su **Nuovo**.

3.  Fare clic su **Servizi app &gt; Active Directory &gt; Provider di Autenticazione a più fattori &gt; Creazione rapida**.

![Immagine della creazione rapida di MFA nei portali di Azure](media/MIM-SSPR-Azureportal.png)

4.  Nel campo **Nome** immettere **SSPRMFA**, quindi fare clic su **Crea**.

![Immagine di MFA del portale di Azure](media/MIM-SSPR-Azureportal-2.png)

5.  Fare clic su **Active Directory** dal menu del portale di Azure classico, quindi scegliere la scheda **Provider di Autenticazione a più fattori**.

6.  Fare clic su **SSPRMFA** , quindi fare clic su **Gestisci** nella parte inferiore della schermata.

    ![Icona di gestione del portale di Azure](media/MIM-SSPR-ManageButton.png)

7.  Nel riquadro sinistro della nuova finestra, sotto **Configura**, fare clic su **Impostazioni**.

8.  In **Avviso di illecito** deselezionare l'opzione **Blocca utente se viene segnalato illecito. In tal modo si evita il blocco dell'intero servizio.

9. Nella finestra **Autenticazione a più fattori di Azure** visualizzata, fare clic su **SDK** in **Download** nel menu a sinistra.

10. Nella colonna ZIP fare clic sul collegamento **Download** relativo al file con il linguaggio **ASP.NET 2.0 C#** per l'SDK.

    ![Immagine del file ZIP di download di Azure MFA](media/MIM-SSPR-Azure-MFA.png)

11. Copiare il file ZIP risultante in ogni sistema in cui è installato il servizio MIM.  Tenere presente che il file ZIP contiene il materiale per le chiavi usato per l'autenticazione nel servizio Azure MFA.

### <a name="update-the-configuration-file"></a>Aggiornare il file di configurazione

1. Accedere al computer in cui è installato il servizio MIM, con l'account utente usato per installare MIM.

2. Creare una nuova cartella nella directory in cui è stato installato il servizio MIM, ad esempio **C:\Programmi\Microsoft Forefront Identity Manager\2010\Service\MfaCerts**.

3. Usando Esplora risorse, passare alla cartella **\pf\certs** del file ZIP scaricato nella sezione precedente e copiare il file **cert_key.p12** nella nuova directory.

4.  Nel file ZIP SDK che si trova nella cartella **\pf** aprire il file **pf_auth.cs**.

5.  Trovare i tre parametri indicati di seguito: `LICENSE_KEY, GROUP_KEY, CERT_PASSWORD`.

    ![immagine del codice pf_auth.cs](media/MIM-SSPR-pFile.png)

6.  In **C:\Program Files\Microsoft Forefront Identity Manager\2010\Service**aprire il file: **MfaSettings**.xml.

7.  Copiare i valori dei parametri `LICENSE_KEY, GROUP_KEY, CERT_PASSWORD` nel file pf_aut.cs nei rispettivi elementi xml nel file MfaSettings.xml.

8.  Nel file zip SDK, in \pf\certs, estrarre il file **cert_key.p12** e immettere il percorso completo del file MfaSettings.xml nell’elemento xml `<CertFilePath>` .

9. Nell’elemento `<username>` immettere un nome utente qualsiasi.

10. Nell’elemento `<DefaultCountryCode>` immettere il codice paese predefinito. Nel caso in cui i numeri di telefono siano registrati per gli utenti senza un codice paese, questo è il codice paese che verrà visualizzato. Se un utente dispone di un codice di paese internazionale, deve essere incluso nel numero di telefono registrato.

11. Salvare il file MfaSettings.xml con lo stesso nome, nello stesso percorso.

#### <a name="configure-the-phone-gate-or-the-one-time-password-sms-gate"></a>Configurare il controllo del telefono o il controllo della password monouso tramite SMS

1.  Avviare Internet Explorer, accedere al portale MIM eseguendo l'autenticazione come amministratore MIM e quindi fare clic su  **Flussi di lavoro** nella barra di spostamento a sinistra.

    ![Immagine di navigazione del portale MIM](media/MIM-SSPR-workflow.jpg)

2.  Selezionare **Flusso di lavoro autenticazione reimpostazione password**

    ![Immagine dei flussi di lavoro del portale MIM](media/MIM-SSPR-PwdResetAuthNworkflow.jpg)

3.  Fare clic sulla scheda **Attività** e scorrere verso il basso fino a **Aggiungi attività**.

4.  Selezionare **Controllo telefono** o **Controllo SMS password monouso**, fare clic su **Seleziona** e quindi su **OK**.

Gli utenti dell'organizzazione possono ora registrarsi per la reimpostazione della password.  Durante questo processo, dovranno immettere il proprio numero di telefono dell'ufficio o di cellulare, in modo che il sistema abbia le informazioni necessarie per chiamarli o inviare loro SMS.

#### <a name="register-users-for-password-reset"></a>Registrare gli utenti per la reimpostazione della password

1.  Un utente avvierà un Web browser e passerà al portale di registrazione per la reimpostazione della password MIM.  In genere, questo portale è configurato con l'autenticazione di Windows.  All'interno del portale gli utenti forniranno di nuovo nome utente e password per confermare la propria identità.

    Gli utenti devono accedere al portale di registrazione password ed effettuare l’autenticazione con nome utente e password.

2.  Nel campo **Numero di telefono** o **Cellulare**  , è necessario immettere un codice paese, uno spazio e il numero di telefono, quindi fare clic su **Avanti**.

    ![Immagine della verifica telefonica di MIM](media/MIM-SSPR-PhoneVerification.JPG)

    ![Immagine della verifica del telefono cellulare di MIM](media/MIM-SSPR-mobilephoneverification.JPG)

## <a name="how-does-it-work-for-your-users"></a>Come funziona per gli utenti?
Ora che tutto è configurato e in esecuzione, è possibile sapere cosa dovranno fare gli utenti se dimenticano le proprie password.

È possibile usare la funzionalità di reimpostazione della password e sblocco dell'account in due modi: dalla schermata di accesso di Windows oppure dal portale self-service.

Installando Componenti aggiuntivi ed estensioni MIM in un computer aggiunto al dominio connesso tramite la rete aziendale al servizio MIM, gli utenti possono recuperare una password dimenticata al momento dell'accesso al desktop.  I passaggi seguenti descrivono il processo in modo dettagliato.

#### <a name="windows-desktop-login-integrated-password-reset"></a>Reimpostazione della password integrata all'accesso al desktop di Windows

1.  Se l'utente immette la password errata più volte, nella schermata di accesso ha la possibilità di fare clic su **Problemi di accesso?** .

    ![Immagine della schermata di accesso](media/MIM-SSPR-problemsloggingin.JPG)

    Facendo clic su questo collegamento viene visualizzata la schermata Reimpostazione password MIM è possibile modificare la password o sbloccare l'account.

    ![Immagine di Reimpostazione password MIM](media/MIM-SSPR-keepcurrentorsetnewpwd.JPG)

2.  L'utente verrà indirizzato per l'autenticazione. Se è stata configurata l'autenticazione a più fattori, l'utente riceverà una telefonata.

3.  In background, Azure MFA effettua una chiamata al numero fornito dall'utente in fase di iscrizione al servizio.

4.  Quando un utente risponde al telefono, gli viene chiesto di premere il tasto cancelletto (#). Quindi l'utente fa clic su **Avanti** nel portale.

    Se si impostano anche altri controlli, all'utente verrà chiesto di fornire ulteriori informazioni nelle schermate successive.

    > [!NOTE]
    > Se l'utente fa clic su **Avanti** prima di premere il tasto #, l'autenticazione non riesce.

5.  Dopo l'autenticazione, l'utente potrà scegliere se sbloccare l'account e mantenere la password corrente oppure impostare una nuova password.

6.  L’utente deve quindi immettere due volte una nuova password e la password sarà reimpostata.

#### <a name="access-from-the-self-service-portal"></a>Accedere dal portale self-service.

1.  Gli utenti possono aprire un Web browser, passare al **portale per la reimpostazione della password** , immettere il proprio nome utente e fare clic su **Avanti** .

    Se è stata configurata l'autenticazione a più fattori, l'utente riceverà una telefonata. In background, Azure MFA effettua una chiamata al numero fornito dall'utente in fase di iscrizione al servizio.

    Quando un utente risponde al telefono, gli viene chiesto di premere il tasto cancelletto (#) sul telefono. Quindi l'utente fa clic su **Avanti** nel portale.

2.  Se si impostano anche altri controlli, all'utente verrà chiesto di fornire ulteriori informazioni nelle schermate successive.

    > [!NOTE]
    > Se l'utente fa clic su **Avanti** prima di premere il tasto #, l'autenticazione non riesce.

3.  L'utente dovrà scegliere se desidera reimpostare la password o sbloccare il suo account. Se sceglie di sbloccare l’account, l'account sarà sbloccato.

    ![Immagine di sblocco dell'account dell'Accesso guidato MIM](media/MIM-SSPR-accountUnlock.JPG)

4.  Dopo l'autenticazione, all'utente vengono fornite due opzioni: sbloccare l’account e mantenere la password corrente o impostare una nuova password.

5.  ![Immagine dell'account MIM sbloccato](media/MIM-SSPR-account-unlock.JPG)

6.  Se l'utente sceglie di reimpostare la password, dovrà digitare una nuova password due volte e fare clic su **Avanti** per modificare la password corrente.

    ![Immagine di reimpostazione della password di Accesso guidato MIM](media/MIM-SSPR-PR1.JPG)
