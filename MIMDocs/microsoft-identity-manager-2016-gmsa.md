---
title: Conversione di servizi specifici di MIM per gMSA | Microsoft Docs
description: Argomento che descrive i passaggi di base per configurare gMSA.
author: fimguy
ms.author: billmath
manager: mtillman
ms.date: 06/27/2018
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.openlocfilehash: 090e82cac6c734beb9767e2d2e6230320e44c26f
ms.sourcegitcommit: c6cb2556bb9f2256b959a3c95db7ca5bbfc2b437
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/28/2018
ms.locfileid: "37065143"
---
# <a name="conversion-of-mim-specific-services-to-gmsa"></a>Conversione di servizi specifici di MIM per gMSA

Questa guida descriverà i passaggi di base per configurare gMSA per i servizi supportati. Il processo di conversione per gMSA è semplice dopo aver preconfigurato l'ambiente.

Hotfix richiesti: \<collegamento alla Knowledge Base più recente\>

Servizi supportati:

-   Sincronizzazione del servizio MIM (FIMSynchronizationService)
-   Servizio MIM (FIMService)
-   Registrazione della password MIM
-   Reimpostazione della password MIM
-   Servizio di monitoraggio PAM (PamMonitoringService)
-   Servizio componenti PAM (PrivilegeManagementComponentService)

Servizi non supportati:

-   Non è supportato il portale MIM perché fa parte dell'ambiente di SharePoint e richiederebbe la distribuzione in modalità farm e la [configurazione della modifica automatica delle password in SharePointServer](https://docs.microsoft.com/sharepoint/administration/configure-automatic-password-change)
-   Tutti gli agenti di gestione
-   Gestione certificati Microsoft
-   BHOLD

<a name="general-information"></a>Informazioni generali 
--------------------

Letture necessarie per completare l'installazione e ottenere una maggiore comprensione

-   [Panoramica degli account del servizio gestiti del gruppo](https://docs.microsoft.com/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview)

-   <https://docs.microsoft.com/en-us/powershell/module/addsadministration/new-adserviceaccount?view=win10-ps>

-   <https://technet.microsoft.com/en-us/library/jj128430(v=ws.11).aspx>

Primo passaggio nel controller di dominio Windows

1.  Creare la chiave radice del servizio distribuzione chiavi (solo una volta per dominio) se necessaria. La chiave radice viene usata dal servizio distribuzione chiavi nei controller di dominio, insieme ad altre informazioni per generare le password.

    -   Add-KDSRootKey –EffectiveImmediately

    -   "–EffectiveImmediately" significa un'attesa fino a \~10 ore per la replica in tutti i controller di dominio. L'attesa è di circa 1 ora per due controller di dominio.

![](media/7fbdf01a847ea0e330feeaf062e30668.png)

## <a name="synchronization-service"></a>Servizio di sincronizzazione
-----------------------

1.  Nel primo passaggio viene creato un gruppo denominato "MIMSync_Servers" al quale vengono aggiunti tutti i server di sincronizzazione.

![](media/a4dc3f6c0cb1f715ba690744f54dce5c.png)

2.  Da Windows PowerShell eseguire quindi il comando seguente come amministratore di dominio con account del computer già aggiunto al dominio

    -   New-ADServiceAccount -Name MIMSyncGMSAsvc -DNSHostName MIMSyncGMSAsvc.contoso.com -PrincipalsAllowedToRetrieveManagedPassword "MIMSync_Servers"

![](media/f6deb0664553e01bcc6b746314a11388.png)

-   Ottenere i dettagli del servizio GSMA per la sincronizzazione:

![](media/c80b0a7ed11588b3fb93e6977b384be4.png)

-   Se il si esegue servizio PCNS sarà necessario aggiornare la delega

    -   Set-ADServiceAccount -Identity MIMSyncGMSAsvc -ServicePrincipalNames \@{Add="PCNSCLNT/mimsync.contoso.com"}

3. Nei servizi di sincronizzazione assicurarsi quindi di eseguire il backup della chiave di crittografia, perché verrà richiesta al momento della modifica della modalità di installazione

    -   Nel server in cui è installato il servizio di sincronizzazione individuare lo strumento di gestione delle chiavi del servizio sincronizzazione

    -   Per impostazione predefinita, è già selezionato il set di chiavi di esportazione

    -   Fare clic su **Avanti**

    -   Verrà ora richiesto di immettere le informazioni sull'account di sincronizzazione esistente

    -   Immettere e verificare le informazioni sull'account di sincronizzazione FIM

        -   Nome dell'account: nome dell'account del servizio di sincronizzazione usato durante l'installazione iniziale

        -   Password: password dell'account del servizio di sincronizzazione

        -   Dominio: dominio di cui fa parte l'account del servizio di sincronizzazione

    -   Fare clic su **Avanti**

    -   Se è stato immesso un valore in modo non corretto, si riceverà l'errore seguente

    -   Dopo aver immesso correttamente le informazioni sull'account, verrà visualizzata un'opzione per modificare la destinazione (percorso del file di esportazione) della chiave di crittografia di backup

        -   Per impostazione predefinita, il percorso del file di esportazione è **C:\\Windows\\system32**\\miiskeys-1.bin.

4. Installare il servizio di sincronizzazione di Microsoft Identity Manager SP1 build 4.4.1302.0, reperibile nell'Area download Volume License o nel sito di download di MSDN. Dopo aver completato l'installazione assicurarsi di salvare il set di chiavi miiskeys.bin.

![](media/ef5f16085ec1b2b1637fa3d577a95dbf.png)


5. Installare l'[hotfix 4.5.x.x](https://docs.microsoft.com/en-us/microsoft-identity-manager/reference/version-history) o versione successiva.

- Dopo aver applicato la patch arrestare il servizio di sincronizzazione FIM.
- Pannello di controllo - Programmi e funzionalità di Microsoft Identity Manager
- Servizio di sincronizzazione Modifica -\> Avanti -\> Configura -\> Avanti

![](media/dc98c011bec13a33b229a0e792b78404.png)

-  Cancellare il nome dell'account
-  Digitare il nome dell'account del servizio **MIMSyncGMSA** con il simbolo \$ come nello
- screenshot. Lasciare vuota la password.

![](media/38df9369bf13e1c3066a49ed20e09041.png)

- Avanti -\> Avanti -\> Installa
- Ripristinare il set di chiavi dal file con estensione bin salvato.

![](media/44cd474323584feb6d8b48b80cfceb9b.png)

![](media/03e7762f34750365e963f0b90e43717c.png)
> [!NOTE]
> Viene aggiunta un'autorizzazione SQL quando si crea l'account per l'accesso, pertanto è necessario consentire all'utente di applicare l'autorizzazione per la modalità di modifica per aggiungere account e dbo nel database del servizio di sincronizzazione

## <a name="mim-service"></a>Servizio MIM
-----------

>[!IMPORTANT]
>Il processo seguente deve essere usato per la prima conversione degli account correlati al servizio MIM come account gMSA. I cmdlet di PowerShell indicati nell'appendice possono essere usati solo per modificare le informazioni sull'account dopo la configurazione iniziale.*

1.  Creare account gestiti di gruppo per il servizio MIM, API Rest PAM, servizio di monitoraggio PAM, servizio componenti PAM, portale di registrazione SSPR, portale di ripristino SSPR.

    -   Assicurarsi di che aggiornare nome SPN e delega gMSA
        -   Set-ADServiceAccount -Identity \<account\> -ServicePrincipalNames \@{Add="\<SPN\>"}
        -   Delegation
            -   Set-ADServiceAccount -Identity \<gsmaaccount\> -TrustedForDelegation \$true
        -   Delega vincolata
            -   \$delspns = 'http/mim', 'http/mim.contoso.com'
            -   New-ADServiceAccount -Name \<gsmaaccount\> -DNSHostName \<gsmaaccount\>.contoso.com -PrincipalsAllowedToRetrieveManagedPassword \<group\> -ServicePrincipalNames \$spns -OtherAttributes \@{'msDS-AllowedToDelegateTo'=\$delspns }

2.  Aggiungere l'account per il servizio MIM nei gruppi di sincronizzazione. È necessario per SSPR.

![](media/0201f0281325c80eb70f91cbf0ac4d5b.jpg)

3.  **NOTA**.  Esiste un problema noto a causa del quale i servizi che usano un account gestito si bloccano dopo il riavvio di server perché il servizio distribuzione chiavi Microsoft non viene avviato dopo il riavvio di Windows. Non è stato possibile avviare il servizio e nemmeno riavviare Windows. Il problema è riproducibile almeno in Windows Server 2012 R2. Per risolvere questo problema, eseguire il comando 

-   **sc triggerinfo kdssvc start/networkon**

    per avviare il servizio distribuzione chiavi Microsoft quando la rete è attiva (in genere all'inizio del ciclo di avvio).

    Vedere la discussione su un problema simile: <https://social.technet.microsoft.com/Forums/en-US/a290c5c0-3112-409f-8cb0-ff23e083e5d1/ad-fs-windows-2012-r2-adfssrv-hangs-in-starting-mode?forum=winserverDS>

4.  Eseguire MSI con privilegi elevati del servizio MIM e selezionare Modifica.

5.  Nella pagina "Configure main server connection page" (Configura connessione server principale) selezionare la casella di controllo "Use different account for Exchange (for managed accounts)" (Usa account diverso per Exchange - per gli account gestiti). In questa fase sarà possibile scegliere di usare l'account precedente che ha una cassetta postale o una cassetta postale nel cloud.

![](media/0cd8ce521ed7945c43bef6100f8eb222.png)

6.  Nella pagina "Configure MIM Service account" (Configura account del servizio MIM) digitare l'account del servizio con il simbolo \$ alla fine. Digitare anche la password dell'account di posta elettronica del servizio. La password dell'account del servizio deve essere disabilitata.

![](media/db0d543df6e1b0174a47135617c23fcb.png)

7.  Dato che la funzione LogonUser non funziona per gli account gestiti, nella pagina successiva verrà visualizzato l'avviso "Please check if Service Account is secure in its current configuration" (Controllare se l'account del servizio è sicuro nella configurazione corrente).

![cid:image007.png\@01D36EB7.562E6CF0](media/d350bc13751b2d0a884620db072ed019.png)

8.  Nella pagina "Configure Privileged Access Management REST API" (Configura l'API REST Privileged Access Management), digitare il nome dell'account del pool di applicazioni con il simbolo \$ alla fine e lasciare vuoto il campo Password.

![](media/88db2f6f291fff8bcdd0da5d538aafc6.png)

9.  Nella pagina "Configure PAM Component Service" (Configura servizio componenti PAM), digitare il nome dell'account del servizio con il simbolo \$ alla fine e lasciare vuoto il campo Password.

![](media/93cfbcefb4d17635dd35c5ead690fd1e.png)

![cid:image010.png\@01D36EB8.A295A3F0](media/9d2b52f6faed10601e7e2166a339fb47.png)

10.  Nella pagina "Configure Privileged Access Management Monitoring Service" (Configura servizio di monitoraggio Privileged Access Management), digitare il nome dell'account del servizio con il simbolo \$ alla fine e lasciare vuoto il campo Password.

![](media/d1e824248edf12a77fc9ffb011475164.png)

11.  Nella pagina "Configure MIM Password Registration Portal" (Configura portale di registrazione password MIM), digitare il nome dell'account con il simbolo \$ alla fine e lasciare vuoto il campo Password.

![](media/601e935cdfda298b61ae753a2a152996.png)

12.  Nella pagina "Configure MIM Password Reset Portal" (Configura portale di ripristino password MIM), digitare il nome dell'account con il simbolo \$ alla fine e lasciare vuoto il campo Password.

![](media/10c8cfa8ff2b6d703d14bd0b7ddc6949.png)

13.  Completare l'installazione.

Nota:

-  Dopo l'installazione vengono create due nuove chiavi nel Registro di sistema nel percorso
    - "HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\Forefront Identity
    - Manager\\2010\\Service" per l'archiviazione della password di Exchange crittografata. Una per
    - Exchange Online e un'altra per Exchange in locale (una deve essere
    - vuota).

![cid:image014.jpg\@01D36F53.303D5190](media/73e2b8a3c149a4ec6bacb4db2c749946.jpg)

- Per aggiornare la password, viene fornito uno script [scaricabile qui](microsoft-identity-manager-2016-gmsascript.md) in modo che il cliente non debba eseguire la modalità di modifica

- Per crittografare la password di Exchange, il programma di installazione crea un servizio aggiuntivo e
    - lo esegue nel contesto dell'account gestito. I messaggi seguenti verranno aggiunti nel
    - log eventi dell'applicazione durante l'installazione.

![cid:image016.jpg\@01D36F53.303D5190](media/95b315454705cd4d939b55ac5ad910f5.jpg)
