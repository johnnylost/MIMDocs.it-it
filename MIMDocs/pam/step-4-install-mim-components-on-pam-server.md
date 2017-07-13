---
title: 'Distribuire PAM, passaggio 4: Installare MIM | Documentazione Microsoft'
description: Installare e configurare servizio e portale MIM nel server e nelle workstation Privileged Access Management.
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/15/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: ef605496-7ed7-40f4-9475-5e4db4857b4f
ROBOTS: noindex,nofollow
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 3a1ec9db6da0a77f963dde76a3efe8d92f89078d
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/13/2017
---
# Passaggio 4: installare i componenti MIM nel server e nella workstation PAM
<a id="step-4--install-mim-components-on-pam-server-and-workstation" class="xliff"></a>

>[!div class="step-by-step"]
[« Passaggio 3](step-3-prepare-pam-server.md)
[Passaggio 5 »](step-5-establish-trust-between-priv-corp-forests.md)


In PAMSRV accedere come PRIV\Administrator per poter installare il servizio e il portale MIM e l'applicazione Web del portale di esempio.

  > [!NOTE]
  > È necessario essere un amministratore di dominio. Se i comandi seguenti non vengono eseguiti come amministratore di dominio, i controlli di convalida dell'attendibilità nel passaggio successivo non verranno completati.

Se è stato scaricato MIM, decomprimere l'archivio di installazione MIM in una nuova cartella.

##  Eseguire il programma di installazione del servizio e del portale.
<a id="run-the-service-and-portal-install-program" class="xliff"></a>  

Seguire le istruzioni del programma di installazione e completare l'installazione.

1.  Quando si selezionano le funzionalità dei componenti, includere il servizio MIM (con Privileged Access Management, ma non la creazione di report MIM) e il portale MIM.  

    ![Installazione personalizzata - Screenshot](./media/PAM_GS_MIM_2015_Service_Portal.png)

2.  Quando si configurano i servizi comuni e la connessione al database MIM, specificare **Crea un nuovo database**.

    > [!NOTE]
    > Se si installa il servizio MIM più volte per la disponibilità elevata, specificare **Utilizza un database esistente** per tutte le installazioni successive.

3.  Quando si configura una connessione server di posta elettronica, impostare il server di posta elettronica sul nome host di un server di Exchange o SMTP per l'ambiente CORP (usare corpdc.contoso.local se non si dispone di un server di posta elettronica) e deselezionare le caselle di controllo **Usa SSL** e **Server di posta in Exchange Server 2007 o Exchange Server 2010**.

4.  Scegliere di generare un nuovo certificato autofirmato.

5.  Impostare le credenziali degli account seguenti:
    - Nome account del servizio: *MIMService*  
    - Password dell'account del servizio: *Pass@word1* (o la password creata nel Passaggio 2)  
    - Dominio account del servizio: *PRIV*  
    - Account di posta elettronica del servizio:*MIMService@priv.contoso.local*  

6.  Accettare le impostazioni predefinite per il nome host del server di sincronizzazione e specificare l'account dell'agente di gestione MIM *PRIV\MIMMA*. Verrà visualizzato un messaggio di avviso in cui si informa che il servizio di sincronizzazione MIM non esiste. Si tratta di uno scenario accettabile, poiché il servizio di sincronizzazione MIM non viene usato in questo scenario.

7.  Impostare *PAMSRV* come indirizzo del server del servizio MIM.

8.  Impostare *http://pamsrv.priv.contoso.local:82* come URL della raccolta del sito di SharePoint.

9. Lasciare vuoto lo spazio dell'URL del portale di registrazione.

10. Selezionare la casella di controllo per aprire le porte 5725 e 5726 nel firewall e la casella di controllo per concedere l'accesso al sito del portale MIM a tutti gli utenti autenticati.

11. Lasciare vuoto il nome host dell'API REST PAM e specificare *8086* come numero di porta.

  ![Informazioni di associazione per l'API REST PAM - Screenshot](./media/PAM_GS_MIM_2015_Service_Portal_configure_application_pool.png)

12. Configurare l'account dell'API REST PAM MIM in modo da usare lo stesso account di SharePoint (in quanto il portale MIM è posizionato su questo server):
    - Nome dell'account del pool di applicazioni: *SharePoint*  
    - Password dell'account del pool di applicazioni: *Pass@word1* (o la password creata nel Passaggio 2)  
    - Dominio dell'account del pool di applicazioni: *PRIV*  

    ![Credenziali dell'account del pool di applicazioni - Screenshot](./media/PAM_GS_Configure_Component_Service.png)

    È possibile che venga visualizzato un messaggio di avviso in cui si afferma che l'account del servizio non è sicuro nella configurazione corrente. Si tratta di una situazione normale.

13. Configurare il servizio del componente PAM MIM:
    - Nome account del servizio: *MIMComponent*
    - Password dell'account del servizio: *Pass@word1* (o la password creata nel Passaggio 2)  
    - Dominio account del servizio: *PRIV*

  ![Credenziali dell'account del servizio del componente PAM - Screenshot](./media/PAM_GS_Configure_MIM_PAM_component_service.png)

14. Configurare il servizio di monitoraggio PAM:
    - Nome account del servizio: *MIMMonitor*  
    - Password dell'account del servizio: *Pass@word1* (o la password creata nel Passaggio 2)  
    - Dominio account del servizio: *PRIV*  

  ![Credenziali dell'account del servizio di monitoraggio PAM - Screenshot](./media/PAM_GS_Configur_PAM_Monitoring_service.png)

15. Nella pagina di immissione delle informazioni per i portali delle password MIM, lasciare vuote le caselle di controllo e continuare. Fare clic su **Avanti** per continuare l'installazione.

Al termine dell'installazione, il server verrà riavviato, quindi verificare che il portale MIM sia attivo e consentire agli utenti di visualizzare la propria risorsa oggetto in MIM.

## Impostare le regole dei criteri di gestione del portale MIM
<a id="set-up-mim-portal-management-policy-rules" class="xliff"></a>

1. Dopo il riavvio di PAMSRV, accedere come PRIV\Administrator.

2. Avviare Internet Explorer e connettersi al portale MIM all'indirizzo http://pamsrv.priv.contoso.local:82/identitymanagement. Può verificarsi un breve ritardo la prima volta che questa pagina viene individuata.

3. Se necessario, eseguire l'autenticazione come PRIV\Administrator a Internet Explorer.

4. In Internet Explorer aprire **Opzioni Internet**, passare alla scheda **Sicurezza** e aggiungere il sito ad **Area Intranet locale**, se non è già presente. Chiudere la finestra di dialogo Opzioni Internet.

5. Se si usa Internet Explorer per visualizzare il portale MIM, fare clic su **Regole di criteri di gestione**.

6. Cercare la regola dei criteri di **Gestione utenti: gli utenti possono leggere i propri attributi**.

7. Selezionare questa regola dei criteri di gestione, deselezionare **Criteri disabilitati**, fare clic su **OK** e quindi su **Invia**.

## Verificare le connessioni firewall
<a id="verify-the-firewall-connections" class="xliff"></a>

Il firewall deve consentire le connessioni in entrata sulle porte TCP 5725, 5726, 8086 e 8090.

1.  In Strumenti di amministrazione avviare **Windows Firewall con sicurezza avanzata**.  
2.  Fare clic su **Regole connessioni in entrata**.  
3.  Verificare siano elencate le due regole seguenti:  
    - Forefront Identity Manager Service (STS)
    - Forefront Identity Manager Service (Webservice)  
4.  Fare clic su **Nuova regola** > **Porta** > **TCP** e digitare le porte locali specifiche *8086* e *8090*. Fare clic nella procedura guidata accettando le impostazioni predefinite, dare un nome alla regola e fare clic su **Fine**.  
5.  Dopo aver completato la procedura guidata, chiudere l'applicazione Windows Firewall.

6.  Avviare il **Pannello di controllo**.  
7.  In Rete e Internet selezionare **Visualizza attività e stato della rete**.  
8.  Verificare che sia presente una rete attiva elencata come priv.contoso.local e una rete di dominio.  
9. Chiudere il **Pannello di controllo**.

## Impostare l'applicazione Web di esempio
<a id="set-up-the-sample-web-application" class="xliff"></a>

In questa sezione viene installata e configurata l'applicazione Web di esempio per l'API REST PAM MIM.

1.  Dall'archivio delle applicazioni Web di esempio scaricare gli [esempi di Identity Management](https://github.com/Azure/identity-management-samples) come file con estensione zip.

2. Decomprimere il contenuto della cartella **identity-management-samples-master\Privileged-Access-Management-Portal\src** in una nuova cartella **C:\Programmi\Microsoft Forefront Identity Manager\2010\Privileged Access Management Portal**.

3.  Creare il nuovo sito Web in IIS con nome sito Portale di esempio Privileged Access Management MIM, percorso fisico C:\Programmi\Microsoft Forefront Identity Manager\2010\Privileged Access Management Portal e porta 8090.  Tale operazione può essere eseguita con il comando PowerShell seguente:

  ```
  New-WebSite -Name "MIM Privileged Access Management Example Portal" -Port 8090   -PhysicalPath "C:\Program Files\Microsoft Forefront Identity Manager\2010\Privileged Access Management Portal\"
  ```

4.  Configurare l'applicazione Web di esempio in modo che sia in grado di reindirizzare gli utenti all'API REST PAM MIM. Tramite un editor di testo come Blocco note, modificare il file **C:\Programmi\Microsoft Forefront Identity Manager\2010\Privileged Access Management REST API\web.config**. Nella sezione **<system.webServer>** aggiungere le righe seguenti:

  ```
  <httpProtocol>
    <customHeaders>
      <add name="Access-Control-Allow-Credentials" value="true"  />
      <add name="Access-Control-Allow-Headers" value="content-type" />
      <add name="Access-Control-Allow-Origin" value="http://pamsrv:8090" />  
    </customHeaders>
  </httpProtocol>
  ```

5.  Configurare l'applicazione Web di esempio. Tramite un editor di testo come Blocco note, modificare il file **C:\Programmi\Microsoft Forefront Identity Manager\2010\Privileged Access Management Portal\js\utils.js**. Impostare il valore di **pamRespApiUrl** su *http://pamsrv.priv.contoso.local:8086/api/pamresources/*.

6.  Riavviare IIS con il comando seguente per rendere effettive le modifiche.

  ```
  iisreset
  ```

7.  (Facoltativo) Verificare che l'utente sia in grado di eseguire l'autenticazione all'API REST. Aprire un Web browser come amministratore in PAMSRV.  Passare all'URL del sito Web http://pamsrv.priv.contoso.local:8086/api/pamresources/pamroles/, effettuare l'autenticazione (se necessario) e assicurarsi che si verifichi il download.

## Installare i cmdlet richiedente PAM MIM
<a id="install-the-mim-pam-requestor-cmdlets" class="xliff"></a>

Installare i cmdlet richiedente PAM MIM nella workstation configurata al Passaggio 1.

1.  Accedere a CORPWKSTN come amministratore.

2.  Scaricare i **componenti aggiuntivi e le estensioni** nel computer CORPWKSTN, se non sono già presenti.

3.  Decomprimere dall'archivio la cartella dei **componenti aggiuntivi e delle estensioni** in una nuova cartella.

4.  Eseguire il programma di installazione **setup.exe**.

5.  Nel programma di installazione personalizzato specificare che deve essere installato il **client PAM**, ma non il **componente aggiuntivo MIM per Outlook** o la **password MIM e le estensioni di autenticazione**.

6.  Nell'indirizzo del server PAM specificare il nome host del server MIM PRIV *pamsrv.priv.contoso.local*.

Al termine dell'installazione, riavviare CORPWKSTN per completare la registrazione del nuovo modulo di PowerShell.

Nel passaggio successivo viene stabilita una relazione di trust tra le foreste PRIV e CORP.

>[!div class="step-by-step"]
[« Passaggio 3](step-3-prepare-pam-server.md)
[Passaggio 5 »](step-5-establish-trust-between-priv-corp-forests.md)
