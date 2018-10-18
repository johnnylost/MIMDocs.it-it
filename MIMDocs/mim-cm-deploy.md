---
title: Distribuzione di Gestione certificati Microsoft Identity Manager | Microsoft Docs
description: Installare Gestione certificati Microsoft Identity Manager 2016
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 09/19/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 01c5c8357c8cb0424bd38b61836919f5c2c3e96a
ms.sourcegitcommit: 7de35aaca3a21192e4696fdfd57d4dac2a7b9f90
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/16/2018
ms.locfileid: "49358874"
---
# <a name="deploying-microsoft-identity-manager-certificate-manager-2016-mim-cm"></a>Distribuzione di Gestione certificati Microsoft Identity Manager 2016 (Gestione certificati MIM)

L'installazione di Gestione certificati Microsoft Identity Manager 2016 (Gestione certificati MIM) prevede diversi passaggi. Per semplificare il processo, i passaggi sono suddivisi in fasi. I passaggi preliminari devono essere eseguiti prima dei passaggi effettivi di Gestione certificati MIM. Se le operazioni preliminari non vengono eseguite è probabile che l'installazione abbia esito negativo.

La figura seguente illustra un esempio del tipo di ambiente che è possibile usare. I sistemi con i numeri sono inclusi nell'elenco sottostante l'immagine e sono necessari per l'esecuzione dei passaggi descritti in questo articolo. Infine, vengono usati i server Windows 2016 Datacenter Server:

![Diagramma dell'ambiente](media/mim-cm-deploy/image001.png)

1. CORPDC - Controller di dominio
2. CORPCM - Server Gestione certificati MIM
3. CORPCA - Autorità di certificazione
4. CORPCMR - Web API REST Gestione certificati MIM - Portale di Gestione certificati per API REST - Uso successivo
5. CORPSQL1 - SQL 2016 SP1
6. CORPWK1 - Windows 10 aggiunto a un dominio

## <a name="deployment-overview"></a>Cenni preliminari sulla distribuzione

- Installazione del sistema operativo di base

    Il lab è costituito da server Windows 2016 Datacenter Server.

    >[!NOTE]
    >Per altre informazioni sulle piattaforme supportate per MIM 2016, vedere l'articolo [Piattaforme supportate per MIM 2016](/microsoft-identity-manager/microsoft-identity-manager-2016-supported-platforms.md)

1. Passaggi pre-distribuzione

    - [Estensione dello schema](https://msdn.microsoft.com/library/ms676929(v=vs.85).aspx)

    - Creazione degli account di servizio

    - [Creazione dei modelli di certificato](https://technet.microsoft.com/library/cc753370(v=ws.11).aspx)

    - IIS

    - Configurazione di Kerberos

    - Passaggi correlati al database

        - Requisiti di configurazione SQL

        - Autorizzazioni database

2. Distribuzione

## <a name="pre-deployment-steps"></a>Passaggi pre-distribuzione

La procedura guidata di configurazione di Gestione certificati MIM richiede l'immissione di informazioni durante l'esecuzione per poter essere completata correttamente.

![diagramma](media/mim-cm-deploy/image003.png)

### <a name="extending-the-schema"></a>Estensione dello schema

Il processo di estensione dello schema è semplice ma deve essere eseguito con attenzione poiché è irreversibile.

>[!NOTE]
>Per eseguire questo passaggio è necessario che l'account usato abbia i diritti di amministratore per lo schema.

1. Cercare il percorso dei supporti MIM e passare alla cartella \\Certificate Management\\x64.

2. Copiare la cartella Schema in CORPDC quindi passare alla cartella.

    ![diagramma](media/mim-cm-deploy/image005.png)

3. Eseguire lo script resourceForestModifySchema.vbs per lo scenario con foresta singola. Per lo scenario con foresta di risorse eseguire gli script:
   - DomainA - Percorso selezionato dagli utenti (userForestModifySchema.vbs)
   - ResourceForestB - Percorso di installazione di Gestione certificati (resourceForestModifySchema.vbs).

     >[!NOTE]
     >Poiché le modifiche allo schema sono un'operazione irreversibile che richiede un ripristino della foresta per eseguire il rollback, assicurarsi di aver eseguito i backup necessari. Per informazioni dettagliate sulle modifiche apportate allo schema eseguendo questa operazione. leggere l'articolo [Forefront Identity Manager 2010 Certificate Management Schema Changes](https://technet.microsoft.com/library/jj159298(v=ws.10).aspx) (Modifiche dello schema di Gestione certificati di Forefront Identity Manager 2010)

     ![diagramma](media/mim-cm-deploy/image007.png)

4. Eseguire lo script. Al completamento dello script, viene inviato un messaggio di operazione riuscita.

    ![Messaggio di operazione riuscita](media/mim-cm-deploy/image009.png)

Lo schema in Active Directory è ora esteso per il supporto di Gestione certificati MIM.

### <a name="creating-service-accounts-and-groups"></a>Creazione di account di servizio e gruppi

Nella tabella seguente sono elencati gli account e le autorizzazioni necessarie per Gestione certificati MIM. È possibile consentire a Gestione certificati MIM di creare automaticamente gli account seguenti oppure è possibile crearli prima dell'installazione. I nomi di account effettivi possono essere modificati. Se si creano gli account manualmente, può essere utile assegnare agli account un nome che faciliti l'associazione del nome dell'account utente alla relativa funzione.

Utenti:

![Diagramma](media/mim-cm-deploy/image010.png)

![Diagramma](media/mim-cm-deploy/image012.png)

| **Ruolo**                   | **Nome di accesso utente** |
|----------------------------|---------------------|
| MIM CM Agent               | MIMCMAgent          |
| MIM CM Key Recovery Agent  | MIMCMKRAgent        |
| MIM CM Authorization Agent | MIMCMAuthAgent      |
| MIM CM CA Manager Agent    | MIMCMManagerAgent   |
| MIM CM Web Pool Agent      | MIMCMWebAgent       |
| MIM CM Enrollment Agent    | MIMCMEnrollAgent    |
| MIM CM Update Service      | MIMCMService        |
| MIM Install Account        | MIMINSTALL          |
| Help Desk Agent            | CMHelpdesk1-2       |
| CM Manager                 | CMManager1-2        |
| Subscriber User            | CMUser1-2           |

Groups:

| **Ruolo**               | **Gruppo**         |
|------------------------|-------------------|
| CM Helpdesk Members    | MIMCM-Helpdesk    |
| CM Manager Members     | MIMCM-Managers    |
| CM Subscribers Members | MIMCM-Subscribers |

Powershell: account agente:

```powershell
import-module activedirectory
## Agent accounts used during setup
$cmagents = @{
"MIMCMKRAgent" = "MIM CM Key Recovery Agent"; 
"MIMCMAuthAgent" = "MIM CM Authorization Agent"
"MIMCMManagerAgent" = "MIM CM CA Manager Agent";
"MIMCMWebAgent" = "MIM CM Web Pool Agent";
"MIMCMEnrollAgent" = "MIM CM Enrollment Agent";
"MIMCMService" = "MIM CM Update Service";
"MIMCMAgent" = "MIM CM Agent";
}
##Groups Used for CM Management
$cmgroups = @{
"MIMCM-Managers" = "MIMCM-Managers"
"MIMCM-Helpdesk" = "MIMCM-Helpdesk"
"MIMCM-Subscribers" = "MIMCM-Subscribers" 
}
##Users Used during testlab
$cmusers = @{
"CMManager1" = "CM Manager1"
"CMManager2" = "CM Manager2"
"CMUser1" = "CM User1"
"CMUser2" = "CM User2"
"CMHelpdesk1" = "CM Helpdesk1"
"CMHelpdesk2" = "CM Helpdesk2"
}

## OU Paths
$aoupath = "OU=Service Accounts,DC=contoso,DC=com" ## Location of Agent accounts
$oupath = "OU=CMLAB,DC=contoso,DC=com" ## Location of Users and Groups for CM Lab


#Create Agents – Update UserprincipalName
$cmagents.GetEnumerator() | Foreach-Object { 
New-ADUser -Name $_.Name -Description $_.Value -UserPrincipalName ($_.Name + "@contoso.com")  -Path $aoupath
$cmpwd = ConvertTo-SecureString "Pass@word1" –asplaintext –force
Set-ADAccountPassword –identity $_.Name –NewPassword $cmpwd
Set-ADUser -Identity $_.Name -Enabled $true
}


#Create Users
$cmusers.GetEnumerator() | Foreach-Object { 
New-ADUser -Name $_.Name -Description $_.Value -Path $oupath
$cmpwd = ConvertTo-SecureString "Pass@word1" –asplaintext –force
Set-ADAccountPassword –identity $_.Name –NewPassword $cmpwd
Set-ADUser -Identity $_.Name -Enabled $true
}
```

### <a name="update-corpcm-server-local-policy-for-agent-accounts"></a>Aggiornare i criteri locali del server **CORPCM** per gli account agente 

| **Nome di accesso utente** | **Descrizione e autorizzazioni**   |
|------|---------------------|
| MIMCMAgent          | Offre i servizi seguenti: </br>-   Recupera le chiavi private crittografate da CA. </br>-   Protegge le informazioni sul PIN della smart card nel database di FIM CM. </br>-   Protegge la comunicazione tra FIM CM e CA. </br></br> Questo account utente richiede le impostazioni di controllo dell'accesso seguenti:</br>Diritto utente -   **Consenti accesso locale**.</br>Diritto utente -   **Rilascio e gestione certificati**. </br>-   Autorizzazione di lettura e scrittura per la cartella Temp di sistema nel seguente percorso: %WINDIR%\\Temp.</br>-   Un certificato di crittografia e firma digitale rilasciato e installato nell'archivio dell'utente.
|MIMCMKRAgent        | Recupera le chiavi private archiviate da CA. Questo account utente richiede le impostazioni di controllo dell'accesso seguenti:</br> Diritto utente -   **Consenti accesso locale**.</br>-   Appartenenza al gruppo **Administrators** locale. </br>-   Autorizzazione di registrazione nel modello di certificato **KeyRecoveryAgent**. </br>-   Il certificato di Key Recovery Agent è emesso e installato nell'archivio dell'utente. Il certificato deve essere aggiunto all'elenco degli agenti di recupero chiavi della CA. </br>-   Autorizzazione di lettura e autorizzazione di scrittura per la cartella Temp di sistema nel seguente percorso: ```%WINDIR%\\Temp.```                                                                                                                     |
| MIMCMAuthAgent      | Determina i diritti utente e le autorizzazioni per utenti e gruppi. Questo account utente richiede le impostazioni di controllo dell'accesso seguenti: </br>-   Appartenenza al gruppo di dominio Accesso compatibile precedente a Windows 2000. </br> -   Diritto utente **Generazione di controlli di protezione**.             |
| MIMCMManagerAgent   | Esegue attività di gestione CA. </br> È necessario che all'utente sia assegnata l'autorizzazione Gestione CA.        |
| MIMCMWebAgent       | Specifica l'identità del pool di applicazioni IIS. FIM CM viene eseguito all'interno di un processo dell'interfaccia di programmazione dell'applicazione Microsoft Win32®che usa le credenziali dell'utente. </br> Questo account utente richiede le impostazioni di controllo dell'accesso seguenti:</br> -   Appartenenza al gruppo **IIS_WPG, windows 2016 = IIS_IUSRS** locale. </br>-   Appartenenza al gruppo **Administrators** locale.</br>-   Diritto utente **Generazione di controlli di protezione**. </br>-   Diritto utente **Agire come parte del sistema operativo**. </br>-   Diritto utente **Sostituzione di token a livello di processo**.</br>-   Assegnazione come identità del pool di applicazioni IIS, **CLMAppPool**. </br>-   Autorizzazione di lettura per la chiave del Registro di sistema    **HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\CLM\\v1.0\\Server\\WebUser**. </br>-   L'account deve anche essere Trusted per la delega.|
| MIMCMEnrollAgent    | Esegue la registrazione per conto dell'utente. Questo account utente richiede le impostazioni di controllo dell'accesso seguenti:</br>-   Un certificato di Enrollment Agent emesso e installato nell'archivio dell'utente.</br>Diritto utente -   **Consenti accesso locale**. </br>-   Autorizzazione di registrazione per il modello di certificato di **Enrollment Agent** o il modello personalizzato, se in uso.                 |

### <a name="creating-certificate-templates-for-mim-cm-service-accounts"></a>Creazione di modelli di certificato per l'account del servizio Gestione certificati MIM

Tre account del servizio usati da Gestione certificati MIM richiedono un certificato e la procedura guidata di configurazione richiede la specifica dei nomi dei modelli di certificato da usare per la richiesta dei certificati.

Gli account del servizio che richiedono i certificati sono:

- MIMCMAgent: questo account richiede un certificato utente

- MIMCMEnrollAgent: questo account richiede un certificato di Enrollment Agent

- MIMCMKRAgent: questo account richiede un certificato di **Key Recovery Agent**

Sebbene siano disponibili modelli in AD, è necessario creare versioni specifiche per l'uso di Gestione certificati MIM. È necessario modificare i modelli di base originali.

Tutti e tre i certificati indicati avranno diritti elevati all'interno dell'organizzazione e dovranno essere gestiti con attenzione.

#### <a name="create-the-mim-cm-signing-certificate-template"></a>Creare il modello di certificato di firma di Gestione certificati MIM

1. Da **Strumenti di amministrazione** aprire **Autorità di certificazione**.

2. Nell'albero della console **Autorità di certificazione** espandere **Contoso-CorpCA** e quindi fare clic su **Modelli di certificato**.

3. Fare clic con il pulsante destro del mouse su **Modelli di certificato**e quindi scegliere **Gestici**.

4. Nella **Console dei modelli di certificato** nel riquadro dei **dettagli** selezionare e fare clic con il pulsante destro del mouse su **Utente** e quindi su **Duplica modello**.

5. Nella finestra di dialogo **Duplica modello** selezionare **Windows Server 2003 Enterprise** e quindi fare clic su **OK**.

    ![Visualizzare le modifiche risultanti](media/mim-cm-deploy/image014.png)

    >[!NOTE]
    >Gestione certificati MIM non funziona con i certificati basati su modelli di certificato versione 3. È necessario creare un modello di certificato di Windows Server® 2003 Enterprise (versione 2). Per altre informazioni, vedere [V3 details](https://blogs.msdn.microsoft.com/ms-identity-support/2016/07/14/faq-for-fim-2010-to-support-sha2-kspcng-and-v3-certificate-templates-for-issuing-user-and-agent-certificates-and-mim-2016-upgrade) (Dettagli per la versione 3).

6. Nella finestra di dialogo **Proprietà nuovo modello** nella scheda **Generale** nella casella **Nome visualizzato modello** digitare **MIM CM Signing**. Modificare il **Periodo di validità** impostandolo su **2 anni** e quindi deselezionare la casella di controllo **Pubblica certificato in Active Directory**.

7. Nella scheda **Gestione richiesta** verificare che la casella di controllo **Rendi la chiave privata esportabile** sia selezionata e quindi fare clic sulla scheda **Crittografia**.

8. Nella finestra di dialogo **Crittografia** disabilitare **Microsoft Enhanced Cryptographic Provider v1.0**, abilitare **Microsoft Enhanced RSA and AES Cryptographic Provider** e quindi fare clic su **OK**.

9. Nella scheda **Nome soggetto** deselezionare le caselle di controllo **Includi nome posta elettronica nel nome soggetto** e **Nome posta elettronica**.

10. Nella scheda **Estensioni** nell'elenco **Estensioni incluse nel modello** verificare che l'opzione **Criteri di applicazione** sia selezionata e quindi fare clic su **Modifica**.

11. Nella finestra di dialogo **Modifica estensione criteri di applicazione** selezionare i criteri di applicazione **Crittografia file system** e **Posta elettronica protetta**. Fare clic su **Rimuovi**e quindi su **OK**.

12. Nella scheda **Security** seguire questa procedura:

    - Rimuovere **Administrator**.

    - Rimuovere **Domain Admins**.

    - Rimuovere **Domain Users**.

    - Assegnare solo le autorizzazioni **Lettura** e **Scrittura** a **Enterprise Admins**.

    - Aggiungere **MIMCMAgent**.

    - Assegnare le autorizzazioni **Lettura** e **Registrazione** a **MIMCMAgent**.

13. Nella finestra di dialogo **Proprietà nuovo modello** fare clic su **OK**.

14. Lasciare aperta la **Console dei modelli di certificato**.

#### <a name="create-the-mim-cm-enrollment-agent-certificate-template"></a>Creare il modello di certificato di Enrollment Agent di Gestione certificati MIM

1. Nella **Console dei modelli di certificato** nel riquadro dei **dettagli** selezionare e fare clic con il pulsante destro del mouse su **Agente di registrazione** e quindi fare clic su **Duplica modello**.

2. Nella finestra di dialogo **Duplica modello** selezionare **Windows Server 2003 Enterprise** e quindi fare clic su **OK**.

3. Nella finestra di dialogo **Proprietà nuovo modello** nella scheda **Generale** nella casella **Nome visualizzato modello** digitare **MIM CM Enrollment Agent**. Assicurarsi che **Periodo di validità** sia impostato su **2 anni**.

4. Nella scheda **Gestione richiesta** abilitare **Rendi la chiave privata esportabile** e quindi fare clic su **CSP o sulla scheda Crittografia**.

5. Nella finestra di dialogo **Selezione CSP** disabilitare **Microsoft Base Cryptographic Provider v1.0**, disabilitare **Microsoft Enhanced Cryptographic Provider v1.0**, abilitare **Microsoft Enhanced RSA and AES Cryptographic Provider** e quindi fare clic su **OK**.

6. Nella scheda **Security** seguire la procedura seguente:

    - Rimuovere **Administrator**.

    - Rimuovere **Domain Admins**.

    - Assegnare solo le autorizzazioni **Lettura** e **Scrittura** a **Enterprise Admins**.

    - Aggiungere **MIMCMEnrollAgent**.

    - Assegnare le autorizzazioni **Lettura** e **Registrazione** a **MIMCMEnrollAgent**.

7. Nella finestra di dialogo **Proprietà nuovo modello** fare clic su **OK**.

8. Lasciare aperta la **Console dei modelli di certificato**.

#### <a name="create-the-mim-cm-key-recovery-agent-certificate-template"></a>Creare il modello di certificato di Key Recovery Agent di Gestione certificati MIM

1. Nella **Console dei modelli di certificato** nel riquadro dei **dettagli** selezionare e fare clic con il pulsante destro del mouse su **Key Recovery Agent** e quindi fare clic su **Duplica modello**.

2. Nella finestra di dialogo **Duplica modello** selezionare **Windows Server 2003 Enterprise** e quindi fare clic su **OK**.

3. Nella finestra di dialogo **Proprietà nuovo modello** nella scheda **Generale** nella casella **Nome visualizzato modello** digitare **MIM CM Key Recovery Agent**. Nella scheda **Crittografia**, assicurarsi che **Periodo di validità** sia impostato su **2 anni**.

4. Nella finestra di dialogo **Provider** disabilitare **Microsoft Enhanced Cryptographic Provider v1.0**, abilitare **Microsoft Enhanced RSA and AES Cryptographic Provider** e quindi fare clic su **OK**.

5. Nella scheda **Requisiti di rilascio** verificare che **Approvazione gestore certificati CA** sia **disabilitata**.

6. Nella scheda **Security** seguire la procedura seguente:

    - Rimuovere **Administrator**.

    - Rimuovere **Domain Admins**.

    - Assegnare solo le autorizzazioni **Lettura** e **Scrittura** a **Enterprise Admins**.

    - Aggiungere **MIMCMKRAgent**.

    - Assegnare le autorizzazioni **Lettura** e **Registrazione** a **KRAgent**.

7. Nella finestra di dialogo **Proprietà nuovo modello** fare clic su **OK**.

8. Chiudere la **Console dei modelli di certificato**.

#### <a name="publish-the-required-certificate-templates-at-the-certification-authority"></a>Pubblicare i modelli di certificato richiesti nell'autorità di certificazione

1. Ripristinare la console **Autorità di certificazione**.

2. Nell'albero della console **Autorità di certificazione** fare clic con il pulsante destro del mouse su **Modelli di certificato**, selezionare **Nuovo** e quindi fare clic su **Modello di certificato da rilasciare**.

3. Nella finestra di dialogo **Attivazione modelli di certificato** selezionare **MIM CM Enrollment Agent**, **MIM CM Key Recovery Agent** e **MIM CM Signing**. Fare clic su **OK**.

4. Nell'albero della console fare clic su **Modelli di certificato**.

5. Verificare che i tre nuovi modelli siano visualizzati nel riquadro dei **dettagli** e quindi chiudere **Autorità di certificazione**.

    ![MIM CM Signing](media/mim-cm-deploy/image016.png)

6. Chiudere tutte le finestre aperte e disconnettersi.

### <a name="iis-configuration"></a>Configurazione di IIS

Per ospitare il sito Web per la gestione dei certificati, installare e configurare IIS.

#### <a name="install-and-configure-iis"></a>Installare e configurare IIS

1. Accedere a **CORLog** con l'account **MIMINSTALL**

    >[!IMPORTANT]
    >L'account di installazione MIM deve essere un amministratore locale

2. Aprire PowerShell ed eseguire il comando seguente

    `Install-WindowsFeature –ConfigurationFilePath`

>[!NOTE]
>Un sito denominato Sito Web predefinito viene installato per impostazione predefinita con IIS 7. Se il sito è stato rinominato o rimosso, per poter installare Gestione certificati MIM è necessario che sia disponibile un sito denominato Sito Web predefinito.

#### <a name="configuring-kerberos"></a>Configurazione di Kerberos

L'account MIMCMWebAgent esegue il portale di Gestione certificati MIM. Per impostazione predefinita in IIS viene usata l'autenticazione in modalità kernel. Viene disabilitata l'autenticazione in modalità kernel di Kerberos e vengono configurati SPN nell'account MIMCMWebAgent. Alcuni comandi richiedono autorizzazioni elevate in Active Directory e nel server CORPCM.

![Diagramma](media/mim-cm-deploy/image020.png)

```powershell
#Kerberos settings
#SPN
SETSPN -S http/cm.contoso.com contoso\MIMCMWebAgent
#Delegation for certificate authority
Get-ADUser CONTOSO\MIMCMWebAgent | Set-ADObject -Add @{"msDS-AllowedToDelegateTo"="rpcss/CORPCA","rpcss/CORPCA.contoso.com"}
```

**Aggiornamento di IIS in CORPCM**

![diagramma](media/mim-cm-deploy/image022.png)

```powershell
add-pssnapin WebAdministration

Set-WebConfigurationProperty -Filter System.webServer/security/authentication/WindowsAuthentication -Location 'Default Web Site' -Name enabled -Value $true
Set-WebConfigurationProperty -Filter System.webServer/security/authentication/WindowsAuthentication -Location 'Default Web Site' -Name useKernelMode -Value $false
Set-WebConfigurationProperty -Filter System.webServer/security/authentication/WindowsAuthentication -Location 'Default Web Site' -Name useAppPoolCredentials -Value $true
```

>[!NOTE]
>È necessario aggiungere un DNS, un record per "cm.contoso.com" e puntare all'IP di CORPCM

#### <a name="requiring-ssl-on-the-mim-cm-portal"></a>Richiedere SSL nel portale di Gestione certificati MIM

È consigliabile richiedere SSL nel portale di Gestione certificati MIM. Se l'operazione non viene eseguita, la procedura guidata visualizza un avviso.

1. Registrare il certificato Web per **cm.contoso.com** e assegnarlo al sito predefinito

2. Aprire **Gestione IIS** e passare a **Gestione certificati**

3. In Visualizzazione funzionalità fare doppio clic su Impostazioni SSL.

4. Nella pagina Impostazioni SSL selezionare **Richiedi SSL**.

5. Nel riquadro Azioni fare clic su **Applica**.

### <a name="database-configuration-corpsql-for-mim-cm"></a>Configurazione del database: **CORPSQL** per Gestione certificati MIM

1. Assicurarsi di essere connessi al server CORPSQL01.

2. Assicurarsi di avere eseguito l'accesso come DBA SQL.

3. Eseguire lo script T-SQL seguente per consentire all'account CONTOSO\\MIMINSTALL di creare il database durante il passaggio di configurazione

    >[!NOTE]
    >Quando si è pronti per uscire e per il modulo dei criteri è necessario tornare a SQL

    ```sql
    create login [CONTOSO\\MIMINSTALL] from windows;
    exec sp_addsrvrolemember 'CONTOSO\\MIMINSTALL', 'dbcreator';
    exec sp_addsrvrolemember 'CONTOSO\\MIMINSTALL', 'securityadmin';  
    ```

![Messaggio di errore della procedura guidata di configurazione di Gestione certificati MIM](media/mim-cm-deploy/image024.png)

## <a name="deployment-of-microsoft-identity-manager-2016-certificate-management"></a>Distribuzione di Gestione certificati Microsoft Identity Manager 2016

1. Assicurarsi di essere connessi al server CORPCM e che l'account **MIMINSTALL** sia membro del gruppo **Administrators** locale.

2. Assicurarsi di aver eseguito l'accesso come Contoso\\MIMINSTALL.

3. Montare l'ISO di Microsoft Identity Manager SP1.

4. **Aprire** la directory **Certificate Management\\x64**.

5. Nella finestra **x64** fare clic con il pulsante destro del mouse su **Imposta** e quindi fare clic su **Esegui come amministratore**.

6. Nella pagina dell'installazione guidata di Gestione certificati di Microsoft Identity Manager fare clic su **Avanti**.

7. Nella pagina del contratto di licenza con l'utente finale leggere il contratto, abilitare la **casella di controllo** Accetto i termini del Contratto di Licenza e quindi fare clic su Avanti.

8. Nella pagina Installazione personalizzata assicurarsi che il **Portale di Gestione certificati MIM** e i **componenti del Servizio di aggiornamento di Gestione certificati MIM** siano selezionati per l'installazione e quindi fare clic su **Avanti**.

9. Nella pagina Cartella Web virtuale assicurarsi che il nome della cartella virtuale sia **CertificateManagement** e quindi **fare clic su Avanti**.

10. Nella pagina di installazione di Gestione certificati di Microsoft Identity Manager fare clic su **Installa**.

11. Nella pagina **Completata** dell'installazione guidata di Gestione certificati di Microsoft Identity Manager fare clic su **Fine**.

![Procedura guidata di Gestione certificati MIM completata](media/mim-cm-deploy/image026.png)

### <a name="configuration-wizard-of-microsoft-identity-manager-2016-certificate-management"></a>Configurazione guidata di Gestione certificati di Microsoft Identity Manager 2016

Prima di eseguire l'accesso a CORPCM, aggiungere MIMINSTALL ai gruppi **Domain Admins, Schema Admins e al gruppo Administrators** locale per la configurazione guidata. È possibile rimuoverlo in un secondo momento dopo aver completato la configurazione.

![Messaggio di errore](media/mim-cm-deploy/image028.png)

1. Fare clic sul pulsante **Start**, scegliere **Configurazione guidata di Certificate Management**. Scegliere **Esegui come amministratore**

2. Nella pagina **Configurazione guidata** fare clic su **Avanti**.

3. Nella pagina **Configurazione CA** verificare che l'autorità di certificazione selezionata sia **Contoso-CORPCA-CA**, che il server selezionato sia **CORPCA.CONTOSO.COM** e quindi fare clic su **Avanti**.

4. Nella pagina **Configurazione database di SQL Server®** nella casella **Nome istanza di SQL Server** digitare **CORPSQL1**, selezionare la casella di controllo **Usa credenziali dell'utente corrente per creare il database** e quindi fare clic su **Avanti**.

5. Nella pagina **Impostazioni database** accettare il nome di database predefinito **FIMCertificateManagement**, assicurarsi che l'opzione **Autenticazione integrata SQL** sia selezionata e quindi fare clic su **Avanti**.

6. Nella pagina **Configurazione Active Directory** accettare il nome predefinito specificato per il punto di connessione del servizio e quindi fare clic su **Avanti**.

7. Nella pagina **Metodo di autenticazione** verificare che l'opzione **Autenticazione integrata di Windows** sia selezionata e quindi fare clic su **Avanti**.

8. Nella pagina **Agenti - FIM CM** deselezionare la casella di controllo **Usa impostazioni predefinite di FIM CM** e quindi fare clic su **Account personalizzati**.

9. Nella finestra di dialogo a più schede **Agenti - FIM CM** digitare le informazioni seguenti in ogni scheda:

   - Nome utente: **Update**

   - Password: **Pass\@word1**

   - Conferma password: **Pass\@word1**

   - Usa un utente esistente: **Enabled**

     >[!NOTE]
     >Questi account sono stati creati in precedenza. Assicurarsi che la procedura descritta nel passaggio 8 venga ripetuta per tutte le sei schede degli account agente.

     ![Account Gestione certificati MIM](media/mim-cm-deploy/image030.png)

10. Dopo aver immesso tutte le informazioni degli account agente, fare clic su **OK**.

11. Nella pagina **Agenti - FIM CM** fare clic su **Avanti**.

12. Nella pagina **Configurazione certificati server** abilitare i modelli di certificato seguenti:

    - Modello di certificato da usare per il certificato di Key Recovery Agent dell'agente di recupero dati: **MIMCMKeyRecoveryAgent**.

    - Modello di certificato da usare per il certificato di FIM CM Agent: **MIMCMSigning**.

    - Modello di certificato da usare per il certificato di Enrollment Agent: **FIMCMEnrollmentAgent**.

13. Nella pagina **Configurazione certificati server** fare clic su **Avanti**.

14. Nella pagina **Configurazione server di posta elettronica e stampa documenti** selezionare la casella **Specificare il nome del server SMTP da utilizzare per l'invio delle notifiche di registrazione tramite posta elettronica** e quindi fare clic su **Avanti**.

15. Nella pagina **Pronto per la configurazione** fare clic su **Configura**.

16. nella finestra di dialogo di avviso **Configurazione guidata - Microsoft Forefront Identity Manager 2010 R2** fare clic su **OK** per confermare che SSL non è abilitato nella directory virtuale IIS.

    ![media/image17.png](media/mim-cm-deploy/image032.png)

    >[!NOTE] 
    >Non fare clic sul pulsante Fine fino quando l'esecuzione della configurazione guidata non è completata. L'accesso alla procedura guidata è disponibile nel percorso seguente: **%programfiles%\\Microsoft Forefront Identity Management\\2010\\Certificate Management\\config.log**

17. Fare clic su **Fine**.

    ![Procedura guidata di Gestione certificati MIM completata](media/mim-cm-deploy/image033.png)

18. Chiudere tutte le finestre aperte.

19. Aggiungere https://cm.contoso.com/certificatemanagement alla zona Intranet locale nel browser.

20. Visitare il sito dal server CORPCM https://cm.contoso.com/certificatemanagement  

    ![diagramma](media/mim-cm-deploy/image035.png)

### <a name="verify-the-cng-key-isolation-service"></a>Verificare il servizio Isolamento chiavi CNG

1. In **Strumenti di amministrazione** aprire **Servizi**.

2. Nel riquadro dei **dettagli** fare doppio clic su **Isolamento chiavi CNG**.

3. Nella scheda **Generale** impostare **Tipo di avvio** su **Automatico**.

4. Nella scheda **Generale** avviare il servizio se non è già stato avviato.

5. Nella scheda **Generale** fare clic su **OK**.

### <a name="installing-and-configuring-the-ca-modules-"></a>Installazione e configurazione dei moduli CA:

In questo passaggio vengono installati e configurati i moduli CA FIM CM nell'autorità di certificazione.

1. Configurare FIM CM soltanto per analizzare le autorizzazioni utente per le operazioni di gestione

2. Nella finestra **C:\\Programmi\\Microsoft Forefront Identity Manager\\2010\\Certificate Management\\web** effettuare una copia di **web.config** e denominare la copia **web.1.config**.

3. Nella finestra **Web** fare clic con il pulsante destro del mouse su **Web.config** e quindi fare clic su **Apri**.

    >[!Note]
    >Il file Web.config viene aperto in Blocco note

4. Quando il file viene aperto, premere CTRL+F.

5. nella finestra di dialogo **Trova e sostituisci** nella casella **Trova** digitare **UseUser** e quindi fare clic su **Trova successivo** tre volte.

6. Chiudere la finestra di dialogo **Trova e sostituisci**.

7. Viene visualizza la riga **\<add key=”Clm.RequestSecurity.Flags” value=”UseUser,UseGroups” /\>**. Modificare la riga in **\<add key=”Clm.RequestSecurity.Flags” value=”UseUser” /\>**.

8. Chiudere il file e salvare tutte le modifiche.

9. Creare un account per il computer CA nel server SQL \<no script\>

10. Assicurarsi di essere connessi al server **CORPSQL01**.

11. Assicurarsi di avere eseguito l'accesso come **DBA**

12. Fare clic sul pulsante **Start**, scegliere **SQL Server Management Studio**.

13. Nella finestra di dialogo **Connetti al server** nella casella **Nome server** digitare **CORPSQL01** e quindi fare clic su **Connetti**.

14. Nell'albero della console espandere **Sicurezza** e quindi fare clic su **Accessi**.

15. Fare clic con il pulsante destro del mouse su **Account di accesso**, quindi fare clic su **Nuovo account di accesso**.

16. Nella pagina **Generale** nella casella **Nome accesso** digitare **contoso\\CORPCA\$**. Selezionare **Autenticazione di Windows**. Il database predefinito è **FIMCertificateManagement**.

17. Nel riquadro di sinistra selezionare **Mapping utente**. Nel riquadro di destra fare clic sulla casella di controllo nella colonna **Mappa** accanto a **FIMCertificateManagement**. Nell'elenco **Appartenenza a ruoli del database per: FIMCertificateManagement** abilitare il ruolo **clmApp**.

18. Fare clic su **OK**.

19. Chiudere **Microsoft SQL Server Management Studio**.

### <a name="install-the-fim-cm-ca-modules-on-the-certification-authority"></a>Installare i moduli CA FIM CM nell'autorità di certificazione

1. Assicurarsi di essere connessi al server **CORPCA**.

2. Nella finestra **X64** fare clic con il pulsante destro del mouse su **Setup.exe** e quindi fare clic su **Esegui come amministratore**.

3. Nella pagina dell'**installazione guidata di Gestione certificati di Microsoft Identity Manager** fare clic su **Avanti**.

4. Nella pagina del **contratto di licenza con l'utente finale** leggere il contratto. Selezionare la casella di controllo **Accetto i termini del Contratto di Licenza** e quindi fare clic su **Avanti**.

5. Nella pagina **Installazione personalizzata** selezionare **Portale di Gestione certificati MIM** e quindi fare clic su **La funzionalità specificata non sarà disponibile**.

6. Nella pagina **Installazione personalizzata** selezionare **Servizio di aggiornamento di Gestione certificati MIM** e quindi fare clic su **La funzionalità specificata non sarà disponibile**.

    >[!Note]
    >In questo modo la funzionalità File CA di Gestione certificati MIM sarà l'unica funzionalità abilitata per l'installazione.

7. Nella pagina **Installazione personalizzata** fare clic su **Avanti**.

8. Nella pagina di **installazione di Gestione certificati di Microsoft Identity Manager** fare clic su **Installa**.

9. Nella pagina **Completata** dell'installazione guidata di Gestione certificati di Microsoft Identity Manager fare clic su **Fine**.

10. Chiudere tutte le finestre aperte.

### <a name="configure-the-mim-cm-exit-module"></a>Configurare il modulo di post-elaborazione di Gestione certificati MIM

1. Da **Strumenti di amministrazione** aprire **Autorità di certificazione**.

2. Nell'albero della console fare clic con il pulsante destro del mouse su **contoso-CORPCA-CA** e quindi fare clic su **Proprietà**.

3. Nella scheda **Modulo uscita** selezionare **Modulo di post-elaborazione di FIM CM** e quindi fare clic su **Proprietà**.

4. Nella casella **Specificare la stringa di connessione del database di FIM CM** digitare **Connect Timeout=15;Persist Security Info=True; Integrated Security=sspi;Initial Catalog=FIMCertificateManagement;Data Source=CORPSQL01**. Lasciare abilitata la casella di controllo **Crittografare la stringa di connessione** e quindi fare clic su **OK**.
5. Nella finestra di messaggio **Gestione certificati FIM** fare clic su **OK**.

6. Nella finestra di dialogo **Proprietà contoso-CORPCA-CA** fare clic su **OK**.

7. Fare clic con il pulsante destro del mouse su **contoso-CORPCA-CA** *,*  scegliere **Tutte le attività** e quindi fare clic su **Arresta servizio**. Attendere che i Servizi certificati Active Directory vengano arrestati.

8. Fare clic con il pulsante destro del mouse su **contoso-CORPCA-CA** *,*  scegliere **Tutte le attività** e quindi fare clic su **Avvia servizio**.

9. Ridurre a icona la console **Autorità di certificazione**.

10. In **Strumenti di amministrazione** aprire il **Visualizzatore eventi**.

11. Nell'albero della console espandere **Registri applicazioni e servizi** e quindi fare clic su **Gestione certificati FIM**.

12. Nell'elenco di eventi verificare che gli ultimi eventi dall'ultimo riavvio di Servizi certificati *non* includano eventi **Avviso** o **Errore**.

    >[!NOTE] 
    >L'ultimo evento dovrebbe indicare che il modulo di post-elaborazione è stato caricato usando le impostazioni di `SYSTEM\CurrentControlSet\Services\CertSvc\Configuration\ContosoRootCA\ExitModules\Clm.Exit`

13. Ridurre a icona il **Visualizzatore eventi**.

### <a name="copy-the-mimcmagent-certificates-thumbprint-to-windows-clipboard"></a>Copiare l'identificazione personale del certificato MIMCMAgent negli Appunti di Windows®

1. Ripristinare la console **Autorità di certificazione**.

2. Nell'albero della console espandere **contoso-CORPCA-CA**, quindi fare clic su **Certificati emessi**.

3. Nel riquadro dei **dettagli** fare doppio clic sul certificato con **CONTOSO\\MIMCMAgent** nella colonna **Nome richiedente** e sul certificato con **FIM CM Signing** nella colonna **Modello di certificato**.

4. Nella scheda **Dettagli** selezionare il campo **Identificazione personale** .

5. Selezionare l'identificazione personale e quindi premere CTRL+C.

    >[!NOTE]
    >**Non** includere lo spazio iniziale nell'elenco dei caratteri dell'identificazione personale.

6. Nella finestra di dialogo **Certificato** fare clic su **OK**.

7. Fare clic sul pulsante **Start** e nella casella **Cerca programmi e file** digitare **Blocco note** e quindi premere INVIO.

8. Nel **Blocco note** scegliere **Incolla** dal menu **Modifica**.

9. Scegliere **Sostituisci** dal menu **Modifica**.

10. Nella casella **Trova** digitare uno spazio e quindi fare clic su **Sostituisci tutto**.

    >[!Note]
    >Vengono rimossi tutti gli spazi tra i caratteri nell'identificazione personale.

11. Nella finestra di dialogo **Sostituisci** fare clic su **Annulla**.

12. Selezionare *thumbprintstring* e quindi premere CTRL+C.

13. Chiudere il **Blocco note** senza salvare le modifiche.

### <a name="configure-the-fim-cm-policy-module"></a>Configurare il modulo criteri di FIM CM

1. Ripristinare la console **Autorità di certificazione**.

2. Fare clic con il pulsante destro del mouse su **contoso-CORPCA-CA** e quindi fare clic su **Proprietà**.

3. Nella finestra di dialogo **Proprietà contoso-CORPCA-CA** nella scheda **Modulo criteri** fare clic su **Proprietà**.

    - Nella scheda **Generale** assicurarsi che l'opzione **Passa richieste non FIM CM al modulo criteri predefinito per l'elaborazione** sia selezionata.

    - Nella scheda **Certificati di firma** fare clic su **Aggiungi**.

    - Nella finestra di dialogo Certificato fare clic con il pulsante destro del mouse sulla casella **Specificare l'hash del certificato con codifica esadecimale** e quindi fare clic su **Incolla**.

    - Nella finestra di dialogo **Certificato** fare clic su **OK**.

        >[!Note]
        >Se il pulsante **OK** non è abilitato significa che è stato immesso per errore un carattere nascosto nella stringa dell'identificazione personale quando l'identificazione personale è stata copiata dal certificato clmAgent. Ripetere tutti i passaggi a partire dall'attività 4 **Copiare l'identificazione personale del certificato MIMCMAgent negli Appunti di Windows** di questo esercizio.

4. Nella finestra di dialogo **Proprietà di configurazione** assicurarsi che l'identificazione personale sia inclusa nell'elenco **Certificati di firma validi** e quindi fare clic su **OK**.

5. Nella finestra di messaggio **Gestione certificati FIM** fare clic su **OK**.

6. Nella finestra di dialogo **Proprietà contoso-CORPCA-CA** fare clic su **OK**.

7. Fare clic con il pulsante destro del mouse su **contoso-CORPCA-CA** *,*  scegliere **Tutte le attività** e quindi fare clic su **Arresta servizio**.

8. Attendere che i Servizi certificati Active Directory vengano arrestati.

9. Fare clic con il pulsante destro del mouse su **contoso-CORPCA-CA** *,*  scegliere **Tutte le attività** e quindi fare clic su **Avvia servizio**.

10. Chiudere la console **Autorità di certificazione**.

11. Chiudere tutte le finestre aperte e disconnettersi.

L'**ultimo passaggio della distribuzione** consiste nell'assicurarsi che CONTOSO\\MIMCM-Managers possa essere distribuito, creare modelli e configurare il sistema senza essere amministratori dello schema e di dominio. Lo script successivo visualizza le autorizzazioni dell'ACL nei modelli di certificato usando dsacls. Eseguire lo script con un account con autorizzazione completa per la modifica delle autorizzazioni di sicurezza Lettura e Scrittura in ogni modello di certificato esistente nella foresta.

Primi passaggi: **Configurazione del punto di connessione del servizio e delle autorizzazioni del gruppo di destinazione e Delega della gestione dei modelli di profilo**

1. Configurare le autorizzazioni nel punto di connessione del servizio (SCP).

2. Configurare la gestione dei modelli di profilo delegata.

3. Configurare le autorizzazioni nel punto di connessione del servizio (SCP). **\<no script\>**

4.   Assicurarsi di essere connessi al server virtuale **CORPDC**.

5. Accedere come **contoso\\corpadmin**

6. Da **Strumenti di amministrazione** aprire **Utenti e computer di Active Directory**.

7. In **Utenti e computer di Active Directory** nel menu **Visualizza** verificare che **Funzionalità avanzate** sia abilitata.

8. Nell'albero della console espandere **Contoso.com** \| **System** \| **Microsoft** \| **Certificate Lifecycle Manager** e quindi fare clic su **CORPCM**.

9. Fare clic con il pulsante destro del mouse su **CORPCM** e quindi scegliere **Proprietà**.

10. Nella finestra di dialogo **Proprietà CORPCM** nella scheda **Sicurezza** aggiungere i gruppi seguenti con le autorizzazioni corrispondenti:

    | Group          | Autorizzazioni      |
    |----------------|------------------|
    | mimcm-Managers | Lettura </br> FIM CM Audit</br> FIM CM Enrollment Agent</br> FIM CM Request Enroll</br> FIM CM Request Recover</br> FIM CM Request Renew</br> FIM CM Request Revoke </br> FIM CM Request Unblock Smart Card |
    | mimcm-HelpDesk | Lettura</br> FIM CM Enrollment Agent</br> FIM CM Request Revoke</br> FIM CM Request Unblock Smart Card |

11. Nella finestra di dialogo **Proprietà CORPDC** fare clic su **OK**.

12. Lasciare aperta **Utenti e computer di Active Directory**.

**Configurare le autorizzazioni negli oggetti utente discendenti**

1. Assicurarsi di essere nella console **Utenti e computer di Active Directory**.

2. Nell'albero della console fare clic con il pulsante destro del mouse su **Contoso.com** e scegliere **Proprietà**.

3. Nella scheda **Sicurezza** fare clic su **Avanzate**.

4. Nella finestra di dialogo **Impostazioni avanzate di sicurezza per Contoso** fare clic su **Aggiungi**.

5. Nella finestra di dialogo **Seleziona utente, Computer, Account servizio o Gruppo** nella casella **Immettere il nome dell'oggetto da selezionare** digitare **mimcm-Managers** e quindi fare clic su **OK**.

6. Nella finestra di dialogo **Voci di autorizzazione per Contoso** nell'elenco **Applica a** selezionare **Descendant User objects** (Oggetti Utente discendente) e quindi selezionare la casella di controllo **Consenti** per le **Autorizzazioni** seguenti:

    - **Leggi tutte le proprietà**

    - **Autorizzazioni di lettura**

    - **FIM CM Audit**

    - **FIM CM Enrollment Agent**

    - **FIM CM Request Enroll**

    - **FIM CM Request Recover**

    - **FIM CM Request Renew**

    - **FIM CM Request Revoke**

    - **FIM CM Request Unblock Smart Card**

7. Nella finestra di dialogo **Voci di autorizzazione per Contoso** fare clic su **OK**.

8. Nella finestra di dialogo **Impostazioni avanzate di sicurezza per Contoso** fare clic su **Aggiungi**.

9. Nella finestra di dialogo **Seleziona utente, Computer, Account servizio o Gruppo** nella casella **Immettere il nome dell'oggetto da selezionare** digitare **mimcm-HelpDesk** e quindi fare clic su **OK**.

10. Nella finestra di dialogo **Voci di autorizzazione per Contoso** nell'elenco **Applica a** selezionare **Descendant User objects** (Oggetti Utente discendente) e quindi selezionare la casella di controllo **Consenti** per le **Autorizzazioni** seguenti:

    - **Leggi tutte le proprietà**

    - **Autorizzazioni di lettura**

    - **FIM CM Enrollment Agent**

    - **FIM CM Request Revoke**

    - **FIM CM Request Unblock Smart Card**

11. Nella finestra di dialogo **Voci di autorizzazione per Contoso** fare clic su **OK**.

12. Nella finestra di dialogo **Impostazioni avanzate di sicurezza per Contoso** fare clic su **OK**.

13. Nella finestra di dialogo **Proprietà contoso.com** fare clic su **OK**.

14. Lasciare aperta **Utenti e computer di Active Directory**.

**Configurare le autorizzazioni negli oggetti utente discendente \<no script\>**

1. Assicurarsi di essere nella console **Utenti e computer di Active Directory**.

2. Nell'albero della console fare clic con il pulsante destro del mouse su **Contoso.com** e scegliere **Proprietà**.

3. Nella scheda **Sicurezza** fare clic su **Avanzate**.

4. Nella finestra di dialogo **Impostazioni avanzate di sicurezza per Contoso** fare clic su **Aggiungi**.

5. Nella finestra di dialogo **Seleziona utente, Computer, Account servizio o Gruppo** nella casella **Immettere il nome dell'oggetto da selezionare** digitare **mimcm-Managers** e quindi fare clic su **OK**.

6. Nella finestra di dialogo **Voci di autorizzazione per CONTOSO** nell'elenco **Applica a** selezionare **Descendant User objects** (Oggetti Utente discendente) e quindi selezionare la casella di controllo **Consenti** per le **Autorizzazioni** seguenti:

    - **Leggi tutte le proprietà**

    - **Autorizzazioni di lettura**

    - **FIM CM Audit**

    - **FIM CM Enrollment Agent**

    - **FIM CM Request Enroll**

    - **FIM CM Request Recover**

    - **FIM CM Request Renew**

    - **FIM CM Request Revoke**

    - **FIM CM Request Unblock Smart Card**

7. Nella finestra di dialogo **Voci di autorizzazione per CONTOSO** fare clic su **OK**.

8. Nella finestra di dialogo **Impostazioni avanzate di sicurezza per CONTOSO** fare clic su **Aggiungi**.

9. Nella finestra di dialogo **Seleziona utente, Computer, Account servizio o Gruppo** nella casella **Immettere il nome dell'oggetto da selezionare** digitare **mimcm-HelpDesk** e quindi fare clic su **OK**.

10. Nella finestra di dialogo **Voci di autorizzazione per CONTOSO** nell'elenco **Applica a** selezionare **Descendant User objects** (Oggetti Utente discendente) e quindi selezionare la casella di controllo **Consenti** per le **Autorizzazioni** seguenti:

    - **Leggi tutte le proprietà**

    - **Autorizzazioni di lettura**

    - **FIM CM Enrollment Agent**

    - **FIM CM Request Revoke**

    - **FIM CM Request Unblock Smart Card**

11. Nella finestra di dialogo **Voci di autorizzazione per contoso** fare clic su **OK**.

12. Nella finestra di dialogo **Impostazioni avanzate di sicurezza per Contoso** fare clic su **OK**.

13. Nella finestra di dialogo **Proprietà contoso.com** fare clic su **OK**.

14. Lasciare aperta **Utenti e computer di Active Directory**.

Passaggi successivi: **Delega delle autorizzazioni di gestione dei modelli di certificato \<script\>**

- Delega delle autorizzazioni nel contenitore Modelli di certificato.

- Delega delle autorizzazioni nel contenitore ID oggetto.

- Delega delle autorizzazioni nei modelli di certificato esistenti.

Definire le autorizzazioni nel contenitore Modelli di certificato:

1. Ripristinare la console **Siti e servizi di Active Directory**.

2. Nell'albero della console espandere **Servizi**, espandere **Servizi chiave pubblica** e quindi fare clic su **Modelli di certificato**.

3. Nell'albero della console fare clic con il pulsante destro del mouse su **Modelli di certificato** e quindi fare clic su **Delega controllo**.

4. In **Delega guidata del controllo** fare clic su **Avanti**.

5. Nella pagina **Utenti o gruppi** fare clic su **Aggiungi**.

6. Nella finestra di dialogo **Seleziona utenti, computer o gruppi** nella casella **Immettere i nomi degli oggetti da selezionare** digitare **mimcm-Managers** e quindi fare clic su **OK**.

7. Nella pagina **Utenti o gruppi** fare clic su **Avanti**.

8. Nella pagina **Attività da delegare** fare clic su **Crea un'attività personalizzata per eseguire la delega** e quindi fare clic su **Avanti**.

9.  Nella pagina **Tipo di oggetti Active Directory** verificare che l'opzione **Questa cartella, gli oggetti che contiene e la creazione di nuovi oggetti in essa** sia selezionata e quindi fare clic su **Avanti**.

10. Nella pagina **Autorizzazioni** nell'elenco **Autorizzazioni** selezionare la casella di controllo **Controllo completo** e quindi fare clic su **Avanti**.

11. Nella pagina **Completamento della Delega guidata del controllo** fare clic su **Fine**.

Definire le autorizzazioni nel contenitore OID:

1. Nell'albero della console fare clic con il pulsante destro del mouse su **ID oggetto** e quindi scegliere **Proprietà**.

2. Nella finestra di dialogo **Proprietà ID oggetto** nella scheda **Sicurezza** fare clic su **Avanzate**.

3. Nella finestra di dialogo **Impostazioni avanzate di sicurezza per ID oggetto** fare clic su **Aggiungi**.

4. Nella finestra di dialogo **Seleziona utente, Computer, Account servizio o Gruppo** nella casella **Immettere il nome dell'oggetto da selezionare** digitare **mimcm-Managers** e quindi fare clic su **OK**.

5. Nella finestra di dialogo **Voci di autorizzazione per ID oggetto** verificare che le autorizzazioni si applichino a **Questo oggetto e tutti i discendenti**, fare clic su **Controllo completo** e quindi su **OK**.

6. Nella finestra di dialogo **Impostazioni avanzate di sicurezza per ID oggetto** fare clic su **OK**.

7. Nella finestra di dialogo **Proprietà ID oggetto** fare clic su **OK**.

8. Chiudere **Siti e servizi di Active Directory**.

**Script: Autorizzazioni per ID oggetto, modello di profilo e contenitore Modelli di certificato**

![diagramma](media/mim-cm-deploy/image021.png)

```powershell
import-module activedirectory
$adace = @{
"OID" = "AD:\\CN=OID,CN=Public Key Services,CN=Services,CN=Configuration,DC=contoso,DC=com";
"CT" = "AD:\\CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=contoso,DC=com";
"PT" = "AD:\\CN=Profile Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=contoso,DC=com"
}
$adace.GetEnumerator() | **Foreach-Object** {
$acl = **Get-Acl** *-Path* $_.Value
$sid=(**Get-ADGroup** "MIMCM-Managers").SID
$p = **New-Object** System.Security.Principal.SecurityIdentifier($sid)
##https://msdn.microsoft.com/library/system.directoryservices.activedirectorysecurityinheritance(v=vs.110).aspx
$ace = **New-Object** System.DirectoryServices.ActiveDirectoryAccessRule
($p,[System.DirectoryServices.ActiveDirectoryRights]"GenericAll",[System.Security.AccessControl.AccessControlType]::Allow,
[DirectoryServices.ActiveDirectorySecurityInheritance]::All)
$acl.AddAccessRule($ace)
**Set-Acl** *-Path* $_.Value *-AclObject* $acl
}
```

**Script: Delega delle autorizzazioni nei modelli di certificato esistenti.**  

![diagramma](media/mim-cm-deploy/image039.png)

```shell
dsacls "CN=Administrator,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=CA,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=CAExchange,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=CEPEncryption,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=ClientAuth,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=CodeSigning,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=CrossCA,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=CTLSigning,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=DirectoryEmailReplication,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=DomainController,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=DomainControllerAuthentication,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=EFS,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=EFSRecovery,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=EnrollmentAgent,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=EnrollmentAgentOffline,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=ExchangeUser,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=ExchangeUserSignature,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=FIMCMSigning,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=FIMCMEnrollmentAgent,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=FIMCMKeyRecoveryAgent,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=IPSecIntermediateOffline,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=IPSecIntermediateOnline,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=KerberosAuthentication,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=KeyRecoveryAgent,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=Machine,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=MachineEnrollmentAgent,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=OCSPResponseSigning,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=OfflineRouter,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=RASAndIASServer,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=SmartCardLogon,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=SmartCardUser,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=SubCA,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=User,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=UserSignature,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=WebServer,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=Workstation,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO
```
