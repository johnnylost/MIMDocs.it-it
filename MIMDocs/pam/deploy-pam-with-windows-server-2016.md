---
title: Distribuire MIM Privileged Access Management con Windows Server 2016 | Documentazione Microsoft
description: Informazioni sulla distribuzione di Privileged Access Management con Windows Server 2016
keywords: ''
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 08/18/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: ''
ms.openlocfilehash: fca3ed1b37a1cc3bf9833c2de4d606845867d5d8
ms.sourcegitcommit: ace4d997c599215e46566386a1a3d335e991d821
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/15/2018
ms.locfileid: "49332902"
---
# <a name="deploy-mim-pam-with-windows-server-2016"></a>Distribuire MIM PAM con Windows Server 2016


In questo scenario MIM 2016 SP1 può sfruttare le funzionalità di Windows Server 2016 come controller di dominio per la foresta "PRIV".  Dopo la configurazione dello scenario, il ticket Kerberos di un utente verrà limitato al tempo rimanente dell'attivazione del proprio ruolo. 

> [!Note]
> Con questa versione di MIM non è possibile usare Technical Preview di Windows Server 2016 precedenti la 5.

## <a name="preparation"></a>Preparazione

Per l'ambiente lab sono necessarie almeno due VM:

-   Una VM ospita il controller di dominio PRIV, con Windows Server 2016

-   Una VM ospita il servizio MIM, con Windows Server 2016 (scelta consigliata) o Windows Server 2012 R2

> [!NOTE]
> Se nell'ambiente lab non è già configurato un dominio "CORP", è necessario un controller di dominio aggiuntivo per tale dominio. Il controller di dominio "CORP" può eseguire Windows Server 2016 o Windows Server 2012 R2.


Eseguire l'installazione come descritto nella [guida introduttiva](privileged-identity-management-for-active-directory-domain-services.md), **fatta eccezione per quanto indicato di seguito**:

- Se si sta creando un nuovo dominio CORP, quando si seguono le istruzioni di [Passaggio 1: preparare il controller di dominio CORP](step-1-prepare-corp-domain.md), è possibile scegliere di configurare il dominio CORP al livello di funzionalità di Windows Server 2016. **Se si sceglie questa opzione, apportare le modifiche seguenti**:

  - Se si usano i supporti di Windows Server 2016, l'opzione di installazione verrà chiamata Windows Server 2016 (Server con Esperienza desktop).

  - È possibile definire il livello di funzionalità di Windows Server 2016 per il dominio e la foresta CORP specificando 7 come numero di versione del dominio e della foresta nell'argomento del comando Install-ADDSForest, come indicato di seguito:
    ```
    Install-ADDSForest –DomainMode 7 –ForestMode 7 –DomainName contoso.local –DomainNetbiosName contoso –Force –NoDnsOnNetwork
    ```
  - In "Creare nuovi utenti e gruppi" il comando finale (New-ADGroup -name 'CONTOSO\$\$\$' …) **non è necessario quando i controller di dominio CORP e PRIV sono al livello di funzionalità di dominio di Windows Server 2016**.

  - Le modifiche descritte in "Configurare il controllo" (punto 8) e "Configurare le impostazioni del Registro di sistema" (punto 10) sono **consigliate ma non obbligatorie** quando i controller di dominio CORP e PRIV sono al livello di funzionalità di dominio di Windows Server 2016.

- Se si sceglie di usare Windows Server 2012 R2 come sistema operativo per CORPDC, è necessario installare gli hotfix 2919442 e 2919355 [e aggiornare 3155495](http://support.microsoft.com/kb/3156418) su CORPDC.

- Seguire le istruzioni descritte in [Passaggio 2: preparare il controller di dominio PRIV](step-2-prepare-priv-domain-controller.md), fatta eccezione per queste modifiche:

  -   Eseguire l'installazione usando i supporti di Windows Server 2016. L'opzione di installazione verrà chiamata Windows Server 2016 (Server con Esperienza desktop).

  -   Nelle istruzioni "Aggiungere i ruoli" (punto 4) **è necessario specificare il numero di versione 7 per il dominio e la foresta nella quarta riga dei comandi di PowerShell** per consentire l'abilitazione delle funzionalità di Windows Server Active Directory descritte più avanti.

      ```
      Install-ADDSForest -DomainMode 7 -ForestMode 7 -DomainName priv.contoso.local  -DomainNetbiosName priv -Force -CreateDNSDelegation -DNSDelegationCredential $ca
      ```  

  -   Quando si configurano i diritti di accesso e controllo, si noti che il programma Gestione Criteri di gruppo è incluso nella cartella Strumenti di amministrazione di Windows.

  -   La configurazione delle impostazioni del Registro di sistema necessarie per la migrazione della cronologia SID (punto 8) **non è richiesta quando il dominio PRIV è al livello di funzionalità di dominio di Windows Server 2016**.

  -   Dopo la configurazione della delega e prima del riavvio del server, abilitare le funzionalità di Privileged Access Management in Windows Server 2016 Active Directory avviando una finestra di PowerShell come amministratore e digitando i comandi seguenti.

  ```
  $of = get-ADOptionalFeature -filter "name -eq 'privileged access management feature'"
  Enable-ADOptionalFeature $of -scope ForestOrConfigurationSet -target "priv.contoso.local"
  ```

  - Dopo la configurazione della delega e prima del riavvio del server, autorizzare gli amministratori MIM e l'account del servizio MIM a creare e aggiornare le entità shadow.

    a. Avviare una finestra di PowerShell e digitare ADSIEdit.

    b. Aprire il menu Azioni e fare clic su "Connetti a". Nell'impostazione del punto di connessione modificare il contesto dei nomi da "Contesto dei nomi predefinito" a "Configuration" e fare clic su OK.

    c. Dopo aver stabilito la connessione, sul lato sinistro della finestra sotto "Modifica ADSI" espandere il nodo Configuration in modo da visualizzare "CN=Configuration, DC=priv,...". Espandere CN=Configuration e quindi CN=Services.

    d. Fare clic con il pulsante destro su "CN=Shadow Principal Configuration" e fare clic su Proprietà. Quando viene visualizzata la finestra di dialogo delle proprietà, passare alla scheda relativa alla sicurezza.

    e. Fare clic su Aggiungi. Specificare gli account "MIMService" ed eventuali altri amministratori MIM che eseguiranno New-PAMGroup in un secondo momento per creare gruppi PAM aggiuntivi. Per ogni utente, nell'elenco delle autorizzazioni consentite aggiungere "Scrittura", "Crea tutti gli oggetti figlio" ed "Elimina tutti gli oggetti figlio". Aggiungere le autorizzazioni.

    f. Modificare le impostazioni di Sicurezza avanzata. Nella riga che consente l'accesso MIMService fare clic su Modifica. Modificare l'impostazione di "Si applica a" selezionando "Questo oggetto e tutti i discendenti". Aggiornare questa impostazione di autorizzazione e chiudere la finestra di dialogo della sicurezza.

    g. Chiudere Modifica ADSI.

  - Dopo la configurazione della delega e prima del riavvio del server, autorizzare gli amministratori MIM a creare e aggiornare i criteri di autenticazione.

    a.  Avviare un **prompt dei comandi** con privilegi elevati e digitare i comandi seguenti, sostituendo "mimadmin" con il nome dell'account amministratore MIM in ognuna delle quattro righe:
    ```
    dsacls "CN=AuthN Policies,CN=AuthN Policy
    Configuration,CN=Services,CN=configuration,DC=priv,DC=contoso,DC=local" /g
    mimadmin:RPWPRCWD;;msDS-AuthNPolicy /i:s

    dsacls "CN=AuthN Policies,CN=AuthN Policy
    Configuration,CN=Services,CN=configuration,DC=priv,DC=contoso,DC=local" /g
    mimadmin:CCDC;msDS-AuthNPolicy

    dsacls "CN=AuthN Silos,CN=AuthN Policy
    Configuration,CN=Services,CN=configuration,DC=priv,DC=contoso,DC=local" /g
    mimadmin:RPWPRCWD;;msDS-AuthNPolicySilo /i:s

    dsacls "CN=AuthN Silos,CN=AuthN Policy
    Configuration,CN=Services,CN=configuration,DC=priv,DC=contoso,DC=local" /g
    mimadmin:CCDC;msDS-AuthNPolicySilo
    ```


- Seguire le istruzioni in [Passaggio 3: preparare un server PAM](step-3-prepare-pam-server.md), con queste modifiche.

  -   Se si esegue l'installazione in Windows Server 2016, si noti che il ruolo "ApplicationServer" non è disponibile.

  -   Se si installa MIM in Windows Server 2016, **non è possibile installare SharePoint 2013**.

- Seguire le istruzioni in [Passaggio 4: installare i componenti MIM nel server e nella workstation PAM](step-4-install-mim-components-on-pam-server.md), con queste modifiche.

  -   L'utente che installa il servizio MIM e i componenti PAM **deve disporre dell'accesso in scrittura al dominio PRIV in Active Directory**, poiché l'installazione di MIM crea una nuova unità organizzativa di Active Directory "PAM objects".

  -   Se SharePoint non è installato, non installare il portale MIM.

- Seguire le istruzioni in [Passaggio 5: stabilire una relazione di trust](step-5-establish-trust-between-priv-corp-forests.md), con queste modifiche:

  - Quando si stabilisce una relazione di trust unidirezionale, eseguire solo i primi due comandi di PowerShell, get-credential e New-PAMTrust, **non eseguire il comando New-PAMDomainConfiguration**.

  - Dopo aver stabilito una relazione di trust, accedere a PRIVDC come PRIV\\Administrator, avviare PowerShell e digitare i comandi seguenti:
    ```
    netdom trust contoso.local /domain:priv.contoso.local /enablesidhistory:yes
    /usero:contoso\administrator /passwordo:Pass@word1

    netdom trust contoso.local /domain:priv.contoso.local /quarantine:no
    /usero:contoso\administrator /passwordo:Pass@word1  

    netdom trust contoso.local /domain:priv.contoso.local /enablepimtrust:yes
    /usero:contoso\administrator /passwordo:Pass@word1
    ```

- Il punto 5 (verifica della relazione di trust) **non è necessario quando i domini CORP e PRIV sono al livello di funzionalità di dominio di Windows Server 2016**.

## <a name="more-information"></a>Altre informazioni

- [Privileged Access Management per Active Directory Domain Services](privileged-identity-management-for-active-directory-domain-services.md)
- [Configurazione dell'ambiente MIM per Privileged Access Management](configuring-mim-environment-for-pam.md)
- [Configurare PAM tramite gli script](sp1-pam-configure-using-scripts.md)
