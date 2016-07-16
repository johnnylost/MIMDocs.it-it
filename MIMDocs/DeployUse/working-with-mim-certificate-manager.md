---
title: Uso di Gestione certificati MIM | Microsoft Identity Manager
description: Informazioni su come distribuire l'app Gestione certificati per consentire agli utenti di gestire i propri diritti di accesso.
keywords: 
author: kgremban
manager: stevenpo
ms.date: 04/28/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 66060045-d0be-4874-914b-5926fd924ede
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: f9b01ac2cee2b96f64a9fda917f4f4146ca2eeda
ms.openlocfilehash: 3e0e6cea0b268836bb6347e81694deec93320ce3


---

# Utilizzo con il gestore di certificati MIM
Dopo avere configurato correttamente MIM 2016 e Gestione certificati, è possibile distribuire l'applicazione di gestione certificati MIM di Windows Store in modo che gli utenti possano gestire facilmente le smart card fisiche e virtuali e i certificati software. I passaggi per distribuire app di gestione certificati MIM CM sono i seguenti:

1.  Creare un modello di certificato.

2.  Creare un modello del profilo.

3.  Preparare l'app.

4.  Distribuire l'app tramite SCCM o Intune.

## Creare un modello di certificato
Creare un modello di certificato per l'applicazione CM esattamente come di consueto, ad eccezione del fatto che è necessario assicurarsi che la versione del modello di certificato sia la versione 3 e versioni successive.

1.  Accedere al server su cui è in esecuzione Servizi certificati Active Directory (il server dei certificati).

2.  Aprire la MMC.

3.  Fare clic su**File &gt; Aggiungi/Rimuovi snap-in**.

4.  Nell'elenco di snap-in disponibili, fare clic su **Modelli di certificato**, quindi su **Aggiungi**.

5.  I **Modelli di certificato** sono visibili nella **Directory principale** nella MMC. Fare doppio clic per visualizzare tutti i modelli di certificato disponibili.

6.  Fare clic con il pulsante destro del mouse sul modello **Accesso con smart card**, quindi fare clic su **Duplica modello**.

7.  Nella scheda Compatibilità in Autorità di certificazione, selezionare Windows Server 2008 e in Destinatario certificato selezionare Windows 8.1 oppure Windows Server 2012 R2.
    Questo passaggio è fondamentale poiché garantisce che si disponga di un modello di certificato versione 3 (o versione successiva) e funziona solo nella versione 3 con l'applicazione di gestione certificati. Poiché la versione viene impostata la prima volta che si crea e salva il modello di certificato, se il modello di certificato non è stato creato in questo modo, non esiste modo per modificarlo con la versione corretta ed è necessario crearne uno nuovo prima di continuare.

8.  Nella scheda **Generale** nel campo **Nome visualizzato** , digitare il nome da visualizzare nell'interfaccia utente dell'applicazione, ad esempio **Accesso con smart card virtuali**.

9. Nella scheda **Gestione richiesta** impostare **Scopo** su **Firma e crittografia** e in **Effettuare le seguenti operazioni...** Selezionare **Chiedi conferma all’utente durante la registrazione**.

10. Nella scheda **Crittografia** nella **Categoria Provider**selezionare **Provider di archiviazione chiavi e Le richieste possono utilizzare qualsiasi provider disponibile nel computer del soggetto**.

    > [!NOTE]
    > Il Provider di archiviazione chiavi sarà visibile solo come un'opzione se la versione del modello è la 3. Se non è visibile, probabilmente non è stato creato il modello di certificato in modo corretto con la versione corretta. Ricominciare con il passaggio 5 sopra.

11. Nella scheda **Sicurezza** , aggiungere il gruppo di sicurezza a cui si desidera assegnare l'accesso di **Registrazione** . Ad esempio, se si desidera concedere l'accesso a tutti gli utenti, selezionare il gruppo **Authenticated users** , quindi selezionare **Autorizzazioni di registrazione** delle autorizzazioni per gli utenti.

12. Fare clic su **OK** per rendere effettive le modifiche e creare il nuovo modello. Il nuovo modello verrà ora visualizzato nell'elenco Modelli di certificato.

13. Selezionare **File**, quindi fare clic su **Aggiungi/Rimuovi snap-in** per aggiungere lo snap-in Autorità di certificazione alla console di MMC. Quando viene richiesto quale computer si desidera gestire, selezionare **Computer locale**.

14. Nel riquadro sinistro di MMC, espandere **Autorità di certificazione (locale)** , quindi espandere l'autorità di certificazione in uso all'interno dell'elenco di autorità di certificazione.

15. Fare clic con il pulsante destro del mouse su **Modelli di certificato**, scegliere **Nuovo &gt; Modello di certificato** da rilasciare.

16. Selezionare il nuovo modello creato nell'elenco e fare clic su **OK**.

## Creare un modello del profilo
Assicurarsi che quando si crea un modello di profilo per l'impostazione per creare o eliminare il vSC e rimuovere la raccolta dei dati. L'applicazione CM non può gestire i dati raccolti, pertanto è importante per disabilitarlo, come indicato di seguito.

1.  Accedere al portale CM come utente con privilegi amministrativi.

2.  Passare ad Amministrazione &gt; Gestisci modelli di profilo e assicurarsi che la casella accanto a Esempio di modello di profilo di MIM CM per accesso tramite smart card sia selezionata, quindi fare clic su Copia modello di profilo selezionato.

3.  Digitare il nome del modello di profilo e fare clic su **OK**.

4.  Nella schermata successiva, fare clic su **Aggiungi nuovo modello di certificato** e assicurarsi di selezionare la casella accanto al nome della CA.

5.  Selezionare la casella accanto al nome del modello di profilo di **Accesso** e fare clic su **Aggiungi**.

6.  Rimuovere il modello SmartCardLogon selezionando la casella accanto e facendo clic su **Elimina i modelli di certificato selezionati** , quindi su **OK**.

7.  Scorrere fino alla fine e fare clic su **Modifica impostazioni**.

8.  Selezionare le caselle accanto a **Crea/elimina smart card virtuale** e **Diversifica chiave amministratore**.

9. In **Criteri PIN utente** selezionare **Forniti dall'utente**.

10. Nel riquadro sinistro fare clic su **Criterio Rinnovo &gt; Modifica impostazioni generali**. Selezionare **Riutilizza smart card al rinnovo** e fare clic su **OK**.

11. È necessario disabilitare gli elementi di raccolta dei dati per ogni criterio facendo clic sul criterio nel riquadro a sinistra e quindi deselezionando la casella accanto a **Elemento di dati di esempio** . Quindi fare clic su **Elimina elemento raccolta dati**. Fare quindi clic su **OK**.

## Preparare l'applicazione CM per la distribuzione

1.  Nel prompt dei comandi, eseguire il comando seguente per decomprimere l'applicazione ed estrarre il contenuto in una nuova sottocartella denominata appx e creare una copia in modo da non modificare il file originale.

    ```
    makeappx unpack /l /p <app package name>.appx /d ./appx
    ren <app package name>.appx <app package name>.appx.original
    cd appx
    ```

2.  Nella cartella appx, modificare il nome del file denominato CustomDataExample.xml in Custom.data.

3.  Aprire il file Custom.data e modificare i parametri in base alle esigenze.

    |||
    |-|-|
    |URL MIMCM|Il nome completo del portale utilizzato per configurare CM. Ad esempio, https://mimcmServerAddress/certificatemanagement|
    |URL DI ADFS|Se si utilizza ADFS, inserire l'URL di AD FS. Ad esempio, https://adfsServerSame/adfs|
    |PrivacyUrl|È possibile includere un URL in una pagina Web che indichi le operazioni effettuate con i dettagli dell'utente raccolti per la registrazione del certificato.|
    |SupportMail|È possibile includere un indirizzo di posta elettronica per problemi di supporto.|
    |LobComplianceEnable|È possibile impostare questa variabile su true o false. L'impostazione predefinita è true.|
    |MinimumPinLength|L'impostazione predefinita è 6.|
    |NonAdmin|È possibile impostare questa variabile su true o false. L'impostazione predefinita è false. Modificare solo se si desidera che gli utenti che non sono amministratori nei computer siano in grado di registrare e rinnovare i certificati.|

4.  Salvare il file e chiudere l’editor.

5.  La firma del pacchetto comporta la creazione di un file di firma, pertanto è necessario eliminare la firma del file originale, denominato AppxSignature.p7x.

6.  Il file AppxManifest.xml specifica il nome del soggetto del certificato di firma. Aprire il file per la modifica.

7.  È necessario ottenere un certificato di firma prima di iniziare questa sezione. Vedere sotto Abilitazione del rinnovo della smart card per utenti non amministratori in MIM 2016 Certificate Manager, passaggio 1.

8.  Nell'elemento &lt;Identity&gt; modificare il valore dell'attributo Publisher affinché corrisponda al soggetto del certificato di firma, ad esempio "CN=SUBJECT".

9. Salvare il file e chiudere l’editor.

10. Nel prompt dei comandi, eseguire il comando seguente per riassemblare e firmare il file con estensione AppX.

    ```
    cd ..
    makeappx pack /l /d .\appx /p <app package name>.appx
    ```
    Dove il nome del pacchetto dell'app è lo stesso nome utilizzato per creare la copia.

    ```
    signtool sign /f <path\>mysign.pfx /p <pfx password> /fd "sha256" <app package name>.ap
    px
    ```
    In questo modo viene creato il nuovo file con estensione AppX. Il file pfx fornisce un percorso per il certificato di firma e una password per il file con estensione pfx.

11. Per usare l'autenticazione AD FS:

    -   Aprire l'applicazione virtuale Smart Card. Ciò rende più semplice individuare i valori necessari per il passaggio successivo.

    -   Per aggiungere l'applicazione come client nel server AD FS e configurare CM nel server, aprire Windows PowerShell nel server AD FS ed eseguire il comando `ConfigureMimCMClientAndRelyingParty.ps1 –redirectUri <redirectUriString> -serverFQDN <MimCmServerFQDN>`

        Ecco lo script ConfigureMimCMClientAndRelyingParty.ps1:

        ```
        # HELP

        <#
        .SYNOPSIS
                        Configure ADFS for CM client app and server.
        .DESCRIPTION
           What the Script does:
                                        a. Registers the MIM CM client app on the ADFS server.
                                        b. Registers the MIM CM server relying party (Tells the ADFS server that it issues tokens for the CM server).
                        For parameter information, see 'get-help -detailed'
        .PARAMETER redirectUri
                        The redirectUri for CM client app. Will be added to ADFS server.
                        It can be found as follows:
                        1. Open the settings panel. Under settings, there is a "Redirect Uri" text box (an ADFS server address must be configured in order for the text to display).
                        2. Open MIM CM client app. Navigate to 'C:\Users\<your_username>\AppData\Local\Packages\CmModernAppv.<version>\LocalState', and open 'Logs_Virtual Smart Card Certificate Manager_<version>'. Search for "Redirect URI".
        .PARAMETER serverFqdn
                        Your deployed MIM CM server’s FQDN.
        .EXAMPLE
           .\ConfigureMimCMClientAndRelyingParty.ps1 -redirectUri ms-app://s-1-15-2-0123456789-0123456789-0123456789-0123456789-0123456789-0123456789-0123456789/ -serverFqdn WIN-TRUR24L4CFS.corp.cmteam.com
        #>

        # Parameter declaration
        [CmdletBinding()]
        Param(
          [Parameter(Mandatory=$True)]
           [string]$redirectUri,

           [Parameter(Mandatory=$True)]
           [string]$serverFqdn
        )

        Write-Host "Configuring ADFS Objects for OAuth.."

        #Configure SSO to get persistent sign on cookie
        Set-ADFSProperties -SsoLifetime 2880

        #Configure Authentication Policy
        #Intranet to use Kerberos
        #Extranet to use U/P

        #Create Client Objects

        Write-Host "Creating Client Objects..."

        $existingClient = Get-ADFSClient -Name "MIM CM Modern App"

        if ($existingClient -ne $null)
        {
            Write-Host "Found existing instance of the MIM CM Modern App, removing"
            Remove-ADFSClient -TargetName "MIM CM Modern App"
            Write-Host "Client object removed"
        }

        Write-Host "Adding Client Object for MIM CM Modern App client"
        Add-ADFSClient -Name "MIM CM Modern App" -ClientId "70A8B8B1-862C-4473-80AB-4E55BAE45B4F" -RedirectUri $redirectUri
        Write-Host "Client Object for MIM CM Modern App client Created"

        #Create Relying Parties
        Write-Host "Creating Relying Party Objects"

        $existingRp = Get-ADFSRelyingPartyTrust -Name "MIM CM Service"
        if ($existingRp -ne $null)
        {
            Write-Host "Found existing instance of the MIM CM Service RP, removing"
            Remove-ADFSRelyingPartyTrust -TargetName "MIM CM Service"
            Write-Host "RP object Removed"
        }

        $authorizationRules =
        "@RuleTemplate = `"AllowAllAuthzRule`"
        => issue(Type = `"http://schemas.microsoft.com/authorization/claims/permit`", Value = `"true`");"

        $issuanceRules =
        "@RuleTemplate = `"LdapClaims`"
        @RuleName = `"Emit UPN and common name`"
        c:[Type == `"http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname`", Issuer == `"AD AUTHORITY`"]
        => issue(store = `"Active Directory`", types =
        (`"http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn`",
        `"http://schemas.xmlsoap.org/claims/CommonName`"), query =
        `";userPrincipalName,cn;{0}`", param = c.Value);

        @RuleTemplate = `"PassThroughClaims`"
        @RuleName = `"Pass through Windows Account Name`"
        c:[Type ==`"http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname`"] => issue(claim = c);"

        Write-Host "Creating RP Trust for MIM CM Service"
        Add-ADFSRelyingPartyTrust -Name "MIM CM Service" -Identifier ("https://"+$serverFqdn+"/certificatemanagement") -IssuanceAuthorizationRules $authorizationRules -IssuanceTransformRules $issuanceRules
        Write-Host "RP Trust for MIM CM Service has been created"
        ```

    -   Aggiornare i valori di redirectUri e serverFQDN.

    -   Per trovare il valore di redirectUri, nell'applicazione di Smart Card virtuale, aprire il pannello Impostazioni applicazione, fare clic su **Impostazioni**e l'URI di reindirizzamento verrà riportata sotto la barra degli indirizzi del server AD FS. L'URI verrà visualizzata solo se è configurato un indirizzo del server AD FS.

    -   serverFQDN è solo il nome completo del server MIMCM.

    -   Per assistenza per lo script **ConfigureMIimCMClientAndRelyingParty.ps1** eseguire `get-help  -detailed ConfigureMimCMClientAndRelyingParty.ps1`

## Distribuire l'app
Quando si configura l'app CM nell'Area download, scaricare il file MIMDMModernApp_&lt;version&gt;_AnyCPU_Test.zip ed estrarre il relativo contenuto. Il file con estensione AppX è il programma di installazione. È possibile distribuire l'app nello stesso modo in cui vengono normalmente distribuite le applicazioni Windows Store, usando [System Center Configuration Manager](https://technet.microsoft.com/library/dn613840.aspx)o [Intune](https://technet.microsoft.com/library/dn613839.aspx) per trasferire localmente l'app in modo che gli utenti dovranno accedervi usando il portale aziendale o direttamente nei propri computer.



<!--HONumber=Jun16_HO4-->


