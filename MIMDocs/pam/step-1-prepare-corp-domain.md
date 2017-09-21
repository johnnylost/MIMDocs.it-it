---
title: 'Distribuire PAM, passaggio 1: Dominio CORP | Documentazione Microsoft'
description: "Preparare il dominio CORP con identità nuove o esistenti da gestire con Privileged Identity Manager"
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/13/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: d14d2f40972686305abea2426e20f4c13e3e267b
ms.sourcegitcommit: 2be26acadf35194293cef4310950e121653d2714
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/14/2017
---
# <a name="step-1---prepare-the-host-and-the-corp-domain"></a>Passaggio 1: preparare l'host e il dominio CORP

>[!div class="step-by-step"]
[Passaggio 2 »](step-2-prepare-priv-domain-controller.md)

Questo passaggio descrive le operazioni necessarie per ospitare l'ambiente bastion. Se necessario, viene inoltre descritto come creare un controller di dominio e una workstation membro in un nuovo dominio e in una nuova foresta (la foresta *CORP*) con identità gestite dall'ambiente bastion. Questa foresta CORP simula una foresta esistente con risorse da gestire. Questo documento include una risorsa di esempio da proteggere: una condivisione file.

Se si dispone già di un dominio di Active Directory (AD) esistente con un controller di dominio che esegue Windows Server 2012 R2 o versioni successive, in cui si ha il ruolo di amministratore di dominio, è possibile usare tale dominio.  

## <a name="prepare-the-corp-domain-controller"></a>Preparare il controller di dominio CORP

Questa sezione descrive come configurare un controller di dominio per un dominio CORP. Nel dominio CORP, gli utenti amministrativi sono gestiti dall'ambiente bastion. Il nome DNS (Domain Name System) del dominio CORP usato in questo esempio è *contoso.local*.

### <a name="install-windows-server"></a>Installare Windows Server

In una macchina virtuale, installare Windows Server 2012 R2 o Windows Server 2016 Technical Preview 4 o versioni successive per creare un computer denominato *CORPDC*.

1. Scegliere **Windows Server 2012 R2 Standard (server con GUI) x64** o **Windows Server 2016 Technical Preview (Server con Esperienza desktop)**.

2. Leggere e accettare le condizioni di licenza.

3. Poiché il disco sarà vuoto, selezionare **Personalizzata: installa solo Windows** e usare lo spazio su disco non inizializzato.

4. Accedere al nuovo computer come amministratore. Passare al Pannello di controllo. Impostare il nome del computer su *CORPDC* e assegnare un indirizzo IP statico nella rete virtuale. Riavviare il server.

5. Dopo aver riavviato il server, accedere come amministratore. Passare al Pannello di controllo. Configurare il computer per verificare la necessità di installare eventuali aggiornamenti. Riavviare il server.

### <a name="add-roles-to-establish-a-domain-controller"></a>Aggiungere ruoli per stabilire un controller di dominio

In questa sezione, si aggiungeranno i ruoli di Servizi di dominio Active Directory (AD DS), Server DNS e File server (parte della sezione Servizi file e archiviazione) e si promuoverà il server a controller di dominio di una nuova foresta contoso.local.

> [!NOTE]  
> Se si dispone già di un dominio da usare come dominio CORP e tale dominio usa Windows Server 2012 R2 o versioni successive come controller di dominio, è possibile passare a [Creare altri utenti e gruppi a scopo dimostrativo](#create-additional-users-and-groups-for-demonstration-purposes).

1. Dopo essersi connessi come amministratore, avviare PowerShell.

2. Digitare i comandi seguenti.

  ```PowoerShell
  import-module ServerManager

  Add-WindowsFeature AD-Domain-Services,DNS,FS-FileServer –restart –IncludeAllSubFeature -IncludeManagementTools

  Install-ADDSForest –DomainMode Win2008R2 –ForestMode Win2008R2 –DomainName contoso.local –DomainNetbiosName contoso –Force -NoDnsOnNetwork
  ```

  Sarà necessario utilizzare una password di amministratore in modalità sicura. Si noti che verranno visualizzati messaggi di avviso per le impostazioni di crittografia e delega DNS. Si tratta di un comportamento normale.

3. Dopo aver completato la creazione della foresta, disconnettersi. Il server verrà riavviato automaticamente.

4. Dopo il riavvio del server, accedere a CORPDC come amministratore del dominio, ossia utente CONTOSO\\Administrator, con la password creata durante l'installazione di Windows su CORPDC.

### <a name="create-a-group"></a>Creare un gruppo

Creare un gruppo a scopo di controllo da Active Directory, se il gruppo non esiste già. Il nome del gruppo deve essere il nome di dominio NetBIOS seguito da tre simboli del dollaro, ad esempio *CONTOSO$$$*.

Per ogni dominio, accedere a un controller di dominio come amministratore di dominio ed eseguire i passaggi seguenti:

1. Avviare PowerShell.

2. Digitare i comandi seguenti, ma sostituire "CONTOSO" con il nome NetBIOS del dominio.

  ```PowerShell
  import-module activedirectory

  New-ADGroup –name 'CONTOSO$$$' –GroupCategory Security –GroupScope DomainLocal –SamAccountName 'CONTOSO$$$'
  ```

In alcuni casi, è possibile che il gruppo esista già. Si tratta di una situazione normale se il dominio è stato usato anche in scenari di migrazione di AD.

### <a name="create-additional-users-and-groups-for-demonstration-purposes"></a>Creare altri utenti e gruppi a scopo dimostrativo

Se è stato creato un nuovo dominio CORP, è necessario creare altri utenti e gruppi per illustrare lo scenario PAM. L'utente e il gruppo a scopo dimostrativo non devono essere amministratori di dominio o controllati dalle impostazioni adminSDHolder in AD.

> [!NOTE]
> Se si ha già un dominio, questo verrà usato come dominio CORP e disporrà di un utente e un gruppo da usare a scopo dimostrativo, quindi è possibile passare alla sezione [Configurare il controllo](#configure-auditing).

Si intende creare un gruppo di sicurezza denominato *CorpAdmins* e un utente denominato *Jen*. È possibile usare nomi diversi.

1. Avviare PowerShell.

2. Digitare i comandi seguenti. Sostituire la password 'Pass@word1' con una stringa di password diversa.

  ```PowerShell
  import-module activedirectory

  New-ADGroup –name CorpAdmins –GroupCategory Security –GroupScope Global –SamAccountName CorpAdmins

  New-ADUser –SamAccountName Jen –name Jen

  Add-ADGroupMember –identity CorpAdmins –Members Jen

  $jp = ConvertTo-SecureString "Pass@word1" –asplaintext –force

  Set-ADAccountPassword –identity Jen –NewPassword $jp

  Set-ADUser –identity Jen –Enabled 1 -DisplayName "Jen"
  ```

### <a name="configure-auditing"></a>Configurare il controllo

È necessario abilitare il controllo in foreste esistenti per stabilire la configurazione di PAM in tali foreste.  

Per ogni dominio, accedere a un controller di dominio come amministratore di dominio ed eseguire i passaggi seguenti:

1. Passare a **Start** > **Strumenti di amministrazione** (o, in Windows Server 2016, **Strumenti di amministrazione Windows**) e avviare **Gestione Criteri di gruppo**.

2. Passare ai criteri del controller di dominio per questo dominio.  Se è stato creato un nuovo dominio per contoso.local, passare a **Foresta: contoso.local** > **Domini** > **contoso.local** > **Controller di dominio** > **Criterio controller di dominio predefinito**. Viene visualizzato un messaggio di tipo informativo.

3. Fare clic con il pulsante destro del mouse su **Criterio controller di dominio predefinito** e scegliere **Modifica**. Viene visualizzata una nuova finestra.

4. Nella finestra Editor Gestione Criteri di gruppo, sotto l'albero Criterio controller di dominio predefinito, passare a **Configurazione computer** > **Criteri** > **Impostazioni di Windows** > **Impostazioni di sicurezza** > **Criteri locali** > **Criteri di controllo**.

5. Nel riquadro dei dettagli fare clic con il pulsante destro del mouse su **Controlla gestione degli account** e selezionare **Proprietà**. Selezionare **Definisci le impostazioni relative ai criteri**, inserire una casella di controllo su **Operazione riuscita**, inserire una casella di controllo su **Operazione non riuscita**, fare clic su **Applica** e su **OK**.

6. Nel riquadro dei dettagli fare clic con il pulsante destro del mouse su **Controlla accesso al servizio directory** e selezionare **Proprietà**. Selezionare **Definisci le impostazioni relative ai criteri**, inserire una casella di controllo su **Operazione riuscita**, inserire una casella di controllo su **Operazione non riuscita**, fare clic su **Applica** e su **OK**.

7. Chiudere la finestra Editor Gestione Criteri di gruppo e la finestra Gestione Criteri di gruppo.

8. Applicare le impostazioni di controllo avviando una finestra di PowerShell e digitando:

  ```cmd
  gpupdate /force /target:computer
  ```

Dopo pochi minuti viene visualizzato il messaggio **Aggiornamento dei criteri computer completato**.

### <a name="configure-registry-settings"></a>Configurare le impostazioni del Registro di sistema

Questa sezione descrive come configurare le impostazioni del Registro di sistema necessarie per la migrazione della cronologia SID, che verrà usata per la creazione del gruppo Privileged Access Management.

1. Avviare PowerShell.

2. Digitare i comandi seguenti per configurare il dominio di origine per consentire la chiamata RPC (Remote Procedure Call) al database Gestione account di sicurezza (SAM).

  ```PowerShell
  New-ItemProperty –Path HKLM:SYSTEM\CurrentControlSet\Control\Lsa –Name TcpipClientSupport –PropertyType DWORD –Value 1

  Restart-Computer
  ```

Verrà riavviato il controller di dominio CORPDC. Per altre informazioni sull'impostazione del Registro di sistema, vedere [How to troubleshoot inter-forest sIDHistory migration with ADMTv2](http://support.microsoft.com/kb/322970) (Risoluzione dei problemi di migrazione di sIDHistory tra foreste diverse con ADMTv2).

## <a name="prepare-a-corp-workstation-and-resource"></a>Preparare una workstation CORP e le relative risorse

Se non si ha già una workstation aggiunta al dominio, seguire queste istruzioni per prepararne una.  

> [!NOTE]
> Se si ha già una workstation aggiunta al dominio, passare a [Creare una risorsa a scopo dimostrativo](#create-a-resource-for-demonstration-purposes).

### <a name="install-windows-81-or-windows-10-enterprise-as-a-vm"></a>Installare Windows 8.1 o Windows 10 Enterprise come macchina virtuale

In un'altra macchina virtuale nuova in cui non è installato alcun software, installare Windows 8.1 Enterprise o Windows 10 Enterprise per creare un computer *CORPWKSTN*.

1. Usare le impostazioni rapide durante l'installazione.

2. Si noti che l'installazione potrebbe non essere in grado di connettersi a Internet. Selezionare **Crea un account locale**. Specificare un nome utente diverso; non utilizzare "Administrator" o "Jen".

3. Utilizzando il pannello di controllo, assegnare un indirizzo IP statico a questo computer nella rete virtuale e impostare il server DNS preferito dell’interfaccia in modo che sia quello del server CORPDC.

4. Tramite il Pannello di controllo, unire il dominio del computer CORPWKSTN al dominio contoso.local. Sarà necessario fornire le credenziali di amministratore del dominio Contoso. Al termine dell'operazione, riavviare il computer CORPWKSTN.

### <a name="create-a-resource-for-demonstration-purposes"></a>Creare una risorsa a scopo dimostrativo

È necessario disporre di una risorsa per dimostrare il controllo di accesso basato su gruppi di sicurezza con PAM.  Se si ha già una risorsa, usare una cartella di file a scopo dimostrativo.  In questo modo, verranno usati gli oggetti "Jen" e "CorpAdmins" di AD creati nel dominio contoso.local.

1. Connettersi alla workstation CORPWKSTN. Fare clic sull'icona **Cambia utente** e quindi su **Altro utente**. Assicurarsi che l'utente CONTOSO\\Jen possa accedere a CORPWKSTN.

2. Creare una nuova cartella denominata *CorpFS* e condividerla con il gruppo *CorpAdmins*.

3. Aprire PowerShell come amministratore.

4. Digitare i comandi seguenti.

  ```PowerShell
  mkdir c:\corpfs

  New-SMBShare –Name corpfs –Path c:\corpfs –ChangeAccess CorpAdmins

  $acl = Get-Acl c:\corpfs

  $car = New-Object System.Security.AccessControl.FileSystemAccessRule( "CONTOSO\CorpAdmins", "FullControl", "Allow")

  $acl.SetAccessRule($car)

  Set-Acl c:\corpfs $acl
  ```

Nel passaggio successivo si preparerà il controller di dominio PRIV.

>[!div class="step-by-step"]
[Passaggio 2 »](step-2-prepare-priv-domain-controller.md)
