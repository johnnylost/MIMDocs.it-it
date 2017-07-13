---
title: 'Distribuire PAM, passaggio 2: controller di dominio PRIV | Documentazione Microsoft'
description: "Preparare il controller di dominio PRIV, che fornirà l'ambiente bastion in cui Privileged Access Management è isolato."
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/15/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 0e9993a0-b8ae-40e2-8228-040256adb7e2
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: edc15b41d4248887f4a93217f68d8125f6500585
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/13/2017
---
# Step 2: preparare il primo controller di dominio PRIV
<a id="step-2---prepare-the-first-priv-domain-controller" class="xliff"></a>

>[!div class="step-by-step"]
[« Passaggio 1](step-1-prepare-corp-domain.md)
[Passaggio 3 »](step-3-prepare-pam-server.md)

In questo passaggio si creerà un nuovo dominio che fornisce all'ambiente bastion l'autenticazione di amministrazione.  Per questa foresta sarà necessario almeno un controller di dominio e un server membro. Il server membro verrà configurato nel passaggio successivo.

## Creare un nuovo controller di dominio di Privileged Access Management
<a id="create-a-new-privileged-access-management-domain-controller" class="xliff"></a>

In questa sezione si imposterà una macchina virtuale come controller di dominio per una nuova foresta.

### Installare Windows Server 2012 R2
<a id="install-windows-server-2012-r2" class="xliff"></a>
In un'altra macchina virtuale nuova in cui non è installato alcun software, installare Windows Server 2012 R2 per creare un computer "PRIVDC".

1. Selezionare questa opzione per eseguire un'installazione personalizzata (non un aggiornamento) di Windows Server. Durante l'installazione, specificare **Windows Server 2012 R2 Standard (server con GUI) x64**; _non selezionare_ **Data Center né Server Core**.

2. Leggere e accettare le condizioni di licenza.

3. Poiché il disco sarà vuoto, selezionare **Personalizzata: installa solo Windows** e usare lo spazio su disco non inizializzato.

4. Dopo aver installato la versione del sistema operativo, accedere al nuovo computer come nuovo amministratore. Usare il Pannello di controllo per impostare il nome del computer su *PRIVDC*, assegnare un indirizzo IP statico nella rete virtuale e configurare come server DNS il sistema usato per il controller di dominio installato nel passaggio precedente. Sarà necessario riavviare il server.

5. Dopo aver riavviato il server, accedere come amministratore. Utilizzando il Pannello di controllo, configurare il computer per verificare la necessità di installare eventuali aggiornamenti. Potrebbe essere necessario il riavvio del server.

### Aggiungere ruoli
<a id="add-roles" class="xliff"></a>
Selezionare i ruoli di Servizi di dominio Active Directory(AD DS) e Server DNS.

1. Avviare PowerShell come amministratore.

2. Digitare i comandi seguenti per preparare un'installazione di Windows Server Active Directory.

  ```
  import-module ServerManager

  Install-WindowsFeature AD-Domain-Services,DNS –restart –IncludeAllSubFeature -IncludeManagementTools
  ```

### Configurare le impostazioni del Registro di sistema per la migrazione alla cronologia SID
<a id="configure-registry-settings-for-sid-history-migration" class="xliff"></a>

Avviare PowerShell e digitare i comandi seguenti per configurare il dominio di origine per consentire la chiamata RPC (Remote Procedure Call) al database Gestione account di sicurezza (SAM).

```
New-ItemProperty –Path HKLM:SYSTEM\CurrentControlSet\Control\Lsa –Name TcpipClientSupport –PropertyType DWORD –Value 1
```

## Creare una nuova foresta di Privileged Access Management
<a id="create-a-new-privileged-access-management-forest" class="xliff"></a>

Promuovere, successivamente, il server come controller di dominio di una nuova foresta.

In questo documento, il nome priv.contoso.local viene usato come nome di dominio della nuova foresta.  Il nome della foresta non è critico e non è necessario che sia subordinato a un nome di foresta esistente nell'organizzazione. Tuttavia, sia i nomi NetBIOS sia il dominio della nuova foresta devono essere univoci e diversi da quelli di qualsiasi altro dominio dell'organizzazione.  

### Creare un dominio e una foresta
<a id="create-a-domain-and-forest" class="xliff"></a>

1. In una finestra di PowerShell digitare i comandi seguenti per creare un nuovo dominio.  Verrà inoltre creata una delega DNS in un dominio di livello superiore (contoso.local) creato nel passaggio precedente.  Se si prevede di configurare DNS in un secondo momento, omettere i parametri `CreateDNSDelegation -DNSDelegationCredential $ca`.

  ```
  $ca= get-credential

  Install-ADDSForest –DomainMode 6 –ForestMode 6 –DomainName priv.contoso.local –DomainNetbiosName priv –Force –CreateDNSDelegation –DNSDelegationCredential $ca
  ```

2. Quando viene visualizzata la finestra popup, specificare le credenziali di amministratore della foresta CORP, ad esempio il nome utente CONTOSO\\Administrator e la password corrispondente come indicato nel passaggio 1.

3. Viene visualizzata una finestra di PowerShell con una richiesta d'uso di una password di amministratore in modalità sicura. Immettere una nuova password due volte. Verranno visualizzati messaggi di avviso per le impostazioni di crittografia e delega DNS. Si tratta di un comportamento normale.

Una volta completata la creazione della foresta, il server verrà riavviato automaticamente.

### Creare account utente e del servizio
<a id="create-user-and-service-accounts" class="xliff"></a>
Creare gli account utente e del servizio per l'installazione del servizio e del portale MIM. Questi account verranno inseriti nel contenitore Users del dominio priv.contoso.local.

1. Dopo aver riavviato il server, accedere a PRIVDC come amministratore di dominio (PRIV\\Administrator).

2. Avviare PowerShell e digitare i comandi seguenti. La password 'Pass@word1' è solo un esempio, per gli account è possibile usare una password diversa.

  ```
  import-module activedirectory

  $sp = ConvertTo-SecureString "Pass@word1" –asplaintext –force

  New-ADUser –SamAccountName MIMMA –name MIMMA

  Set-ADAccountPassword –identity MIMMA –NewPassword $sp

  Set-ADUser –identity MIMMA –Enabled 1 –PasswordNeverExpires 1

  New-ADUser –SamAccountName MIMMonitor –name MIMMonitor -DisplayName MIMMonitor

  Set-ADAccountPassword –identity MIMMonitor –NewPassword $sp

  Set-ADUser –identity MIMMonitor –Enabled 1 –PasswordNeverExpires 1

  New-ADUser –SamAccountName MIMComponent –name MIMComponent -DisplayName MIMComponent

  Set-ADAccountPassword –identity MIMComponent –NewPassword $sp

  Set-ADUser –identity MIMComponent –Enabled 1 –PasswordNeverExpires 1

  New-ADUser –SamAccountName MIMSync –name MIMSync

  Set-ADAccountPassword –identity MIMSync –NewPassword $sp

  Set-ADUser –identity MIMSync –Enabled 1 –PasswordNeverExpires 1

  New-ADUser –SamAccountName MIMService –name MIMService

  Set-ADAccountPassword –identity MIMService –NewPassword $sp

  Set-ADUser –identity MIMService –Enabled 1 –PasswordNeverExpires 1

  New-ADUser –SamAccountName SharePoint –name SharePoint

  Set-ADAccountPassword –identity SharePoint –NewPassword $sp

  Set-ADUser –identity SharePoint –Enabled 1 –PasswordNeverExpires 1

  New-ADUser –SamAccountName SqlServer –name SqlServer

  Set-ADAccountPassword –identity SqlServer –NewPassword $sp

  Set-ADUser –identity SqlServer –Enabled 1 –PasswordNeverExpires 1

  New-ADUser –SamAccountName BackupAdmin –name BackupAdmin

  Set-ADAccountPassword –identity BackupAdmin –NewPassword $sp

  Set-ADUser –identity BackupAdmin –Enabled 1 -PasswordNeverExpires 1

  New-ADUser -SamAccountName MIMAdmin -name MIMAdmin

  Set-ADAccountPassword –identity MIMAdmin  -NewPassword $sp

  Set-ADUser -identity MIMAdmin -Enabled 1 -PasswordNeverExpires 1

  Add-ADGroupMember "Domain Admins" SharePoint

  Add-ADGroupMember "Domain Admins" MIMService
  ```

### Configurare i diritti di accesso e di controllo.
<a id="configure-auditing-and-logon-rights" class="xliff"></a>

È necessario configurare il controllo per stabilire la configurazione PAM tra foreste.  

1. Assicurarsi di essere connessi come amministratore di dominio (PRIV\\Administrator).

2. Passare a **Start** > **Strumenti di amministrazione** > **Gestione Criteri di gruppo**.

3. Passare a **Foresta: priv.contoso.local** > **, Domini** > **priv.contoso.local** > **Controller di dominio** > **Criterio controller di dominio predefinito**. Verrà visualizzato un messaggio di avviso.

4. Fare clic con il pulsante destro del mouse su **Criterio controller di dominio predefinito** e scegliere **Modifica**.

5. Nell'albero della console di Editor Gestione Criteri di gruppo passare a **Configurazione computer** > **Criteri** > **Impostazioni di Windows** > **Impostazioni di sicurezza** > **Criteri locali** > **Criteri di controllo**.

6. Nel riquadro Dettagli fare clic con il pulsante destro del mouse su **Controlla gestione degli account** e selezionare **Proprietà**. Fare clic su **Definisci queste impostazioni dei criteri**, controllare la casella di controllo **Successo** , controllare la casella di controllo **Errore** , fare clic su **Applica** e quindi su **OK**.

7. Nel riquadro Dettagli fare clic con il pulsante destro del mouse su **Controlla accesso al servizio directory** e selezionare **Proprietà**. Fare clic su **Definisci queste impostazioni dei criteri**, controllare la casella di controllo **Successo** , controllare la casella di controllo **Errore** , fare clic su **Applica** e quindi su **OK**.

8. Passare a **Configurazione computer** > **Criteri** > **Impostazioni di Windows** > **Impostazioni di sicurezza** > **Criteri di account** > **Criteri Kerberos**.

9. Nel riquadro Dettagli fare clic con il pulsante destro del mouse su **Durata massima ticket utente** e selezionare **Proprietà**. Fare clic su **Definisci queste impostazioni dei criteri**, impostare il numero di ore su *1*, fare clic su **Applica** e quindi **OK**. Si noti che anche altre impostazioni nella finestra verranno modificate.

10. Nella finestra Gestione Criteri di gruppo selezionare i **criteri di dominio predefiniti** e quindi fare clic con il pulsante destro del mouse e scegliere **Modifica**.

11. Espandere **Configurazione computer** > **Criteri** > **Impostazioni di Windows** > **Impostazioni di sicurezza** > **Criteri locali** e quindi selezionare **Assegnazione diritti utente**.

12. Nel riquadro Dettagli fare clic con il pulsante destro del mouse su **Nega accesso come processo batch** e selezionare **Proprietà**.

13. Selezionare la casella di controllo **Definisci le impostazioni relative ai criteri**, fare clic su **Aggiungi utente o gruppo**, nel campo Nomi utenti e gruppi digitare *priv\\mimmonitor; priv\\MIMService; priv\\mimcomponent* e quindi fare clic su **OK**.

14. Fare clic su **OK** per chiudere la finestra.

15. Nel riquadro Dettagli fare clic con il pulsante destro del mouse su **Nega accesso tramite Servizi Desktop remoto** e selezionare **Proprietà**.

16. Fare clic sulla casella di controllo **Definisci le impostazioni relative ai criteri**, fare clic su **Aggiungi utente o gruppo**, nel campo Nomi utenti e gruppi digitare *priv\\mimmonitor; priv\\MIMService; priv\\mimcomponent* e quindi fare clic su **OK**.

17. Fare clic su **OK** per chiudere la finestra.

18. Chiudere la finestra Editor Gestione Criteri di gruppo e la finestra Gestione Criteri di gruppo.

19. Avviare una finestra di PowerShell come amministratore e digitare il comando seguente per aggiornare il controller di dominio in base alle impostazioni dei criteri di gruppo.

  ```
  gpupdate /force /target:computer
  ```

  Dopo un minuto, l’operazione verrà completata e verrà visualizzato il messaggio "Aggiornamento dei criteri computer completato."


### Configurare l'inoltro dei nomi DNS in PRIVDC
<a id="configure-dns-name-forwarding-on-privdc" class="xliff"></a>

Tramite PowerShell in PRIVDC, configurare l'inoltro del nome DNS affinché il dominio PRIV riconosca altre foreste esistenti.

1. Avviare PowerShell.

2. Per ogni dominio nella parte superiore di ogni foresta esistente, digitare il comando seguente, che specifica il dominio DNS esistente, ad esempio contoso.local, e l'indirizzo IP del server master di tale dominio.  

  Se nel passaggio precedente è stato creato un dominio contoso.local, specificare *10.1.1.31* per l'indirizzo IP della rete virtuale del computer CORPDC.

  ```
  Add-DnsServerConditionalForwarderZone –name "contoso.local" –masterservers 10.1.1.31
  ```

> [!NOTE]
> Le altre foreste devono essere in grado di instradare le query DNS per la foresta PRIV al controller di dominio.  Se si hanno più foreste di Active Directory, è necessario aggiungere anche un server d'inoltro condizionale DNS a ognuna di queste foreste.

### Configurare Kerberos
<a id="configure-kerberos" class="xliff"></a>

1. Tramite PowerShell, aggiungere i nomi dell'entità servizio in modo che SharePoint, l'API REST di PAM e il servizio MIM possano usare l'autenticazione Kerberos.

  ```
  setspn -S http/pamsrv.priv.contoso.local PRIV\SharePoint
  setspn -S http/pamsrv PRIV\SharePoint
  setspn -S FIMService/pamsrv.priv.contoso.local PRIV\MIMService
  setspn -S FIMService/pamsrv PRIV\MIMService
  ```

> [!NOTE]
> I passaggi successivi di questo documento descrivono come installare i componenti del server MIM 2016 in un computer singolo. Se si prevede di aggiungere un altro server per la disponibilità elevata, sarà necessaria un'altra configurazione di Kerberos come descritto in [FIM 2010: Kerberos Authentication Setup](http://social.technet.microsoft.com/wiki/contents/articles/3385.fim-2010-kerberos-authentication-setup.aspx) (FIM 2010: installazione dell'autenticazione Kerberos).

### Configurare la delega per consentire al servizio MIM l'accesso agli account
<a id="configure-delegation-to-give-mim-service-accounts-access" class="xliff"></a>

Seguire questa procedura in PRIVDC come amministratore di dominio.

1. Avviare **Utenti e computer di Active Directory**.  
2. Fare clic con il pulsante destro del mouse sul dominio **priv.contoso.local** e selezionare **Delega controllo**.  
3. Nella scheda Gruppi e utenti selezionati fare clic su **Aggiungi**.  
4. Nella finestra Seleziona utenti computer o gruppi digitare *mimcomponent; mimmonitor; mimservice* e fare clic su **Controlla nomi**. Dopo aver sottolineato i nomi fare clic su **OK** e quindi su **Avanti**.  
5. Nell'elenco delle attività comuni selezionare **Crea, elimina e gestisce gli account utente** e **Modifica appartenenza di un gruppo** e quindi fare clic su **Avanti** e **Fine**.

6. Fare nuovamente clic con il pulsante destro del mouse sul dominio **priv.contoso.local** e selezionare **Delega controllo**.  
7. Nella scheda Gruppi e utenti selezionati fare clic su **Aggiungi**.  
8. Nella finestra Seleziona utenti, computer o gruppi digitare *MIMAdmin* e fare clic su **Controlla nomi**. Dopo aver sottolineato i nomi fare clic su **OK** e quindi su **Avanti**.  
9. Selezionare **Personalizza attività**, applicare a **Questa cartella**, con **Autorizzazioni generali**.    
10. Nell'elenco delle autorizzazioni selezionare gli elementi seguenti:  
  - **Leggi**  
  - **Scrivi**  
  - **Crea tutti gli oggetti figlio**  
  - **Elimina tutti gli oggetti figlio**  
  - **Leggi tutte le proprietà**  
  - **Scrivi tutte le proprietà**  
  - **Esegui migrazione cronologia**  
  Fare clic su **Avanti** quindi **Fine**.

11. Fare nuovamente clic con il pulsante destro del mouse sul dominio **priv.contoso.local** e selezionare **Delega controllo**.  
12. Nella scheda Gruppi e utenti selezionati fare clic su **Aggiungi**.  
13. Nella finestra Seleziona utenti, computer o gruppi digitare *MIMAdmin* e quindi fare clic su **Controlla nomi**. Dopo aver sottolineato i nomi, fare clic su **OK**, quindi su **Avanti**.  
14. Selezionare **personalizza attività**, applicare a **Questa cartella**, quindi fare clic su **solo Oggetti utente**.    
15. Nell'elenco delle autorizzazioni, selezionare **Cambia password** e **Reimposta password**. Fare clic su **Avanti** e quindi su **Fine**.  
16. Chiudere Utenti e computer di Active Directory.

17. Aprire un prompt dei comandi.  
18. Esaminare l'elenco di controllo di accesso relativo all'oggetto Admin SD Holder nei domini PRIV. Ad esempio, se il dominio è "priv.contoso.local", digitare il comando  
  ```
  dsacls "cn=adminsdholder,cn=system,dc=priv,dc=contoso,dc=local"
  ```
19. Aggiornare l'elenco di controllo di accesso in base alle esigenze per garantire che il servizio MIM e il servizio del componente MIM possano aggiornare l'appartenenza dei gruppi protetti in base a questo elenco di controllo.  Digitare il comando:  
  ```
  dsacls "cn=adminsdholder,cn=system,dc=priv,dc=contoso,dc=local" /G priv\mimservice:WP;"member"  
  dsacls "cn=adminsdholder,cn=system,dc=priv,dc=contoso,dc=local" /G priv\mimcomponent:WP;"member"
  ```
20. Riavviare il server PRIVDC in modo che le modifiche avranno effetto.

## Preparare una workstation PRIV
<a id="prepare-a-priv-workstation" class="xliff"></a>

Se non si ha già una workstation che verrà aggiunta al dominio PRIV per la manutenzione delle risorse PRIV, ad esempio MIM, seguire queste istruzioni per preparare una workstation.  

### Installare Windows 8.1 o Windows 10 Enterprise
<a id="install-windows-81-or-windows-10-enterprise" class="xliff"></a>

In un'altra macchina virtuale nuova in cui non è installato alcun software, installare Windows 8.1 Enterprise o Windows 10 Enterprise per creare un computer *PRIVWKSTN*.

1. Utilizzare le impostazioni rapide durante l'installazione.

2. Si noti che l'installazione potrebbe non essere in grado di connettersi a Internet. Fare clic su **Crea un account locale**. Specificare un nome utente diverso; non utilizzare "Administrator" o "Jen".

3. Tramite il Pannello di controllo, assegnare un indirizzo IP statico a questo computer nella rete virtuale e impostare il server DNS preferito dell'interfaccia in modo che corrisponda a quello del server CORPDC.

4. Tramite il Pannello di controllo, aggiungere il computer PRIVWKSTN al dominio priv.contoso.local. Sarà necessario specificare le credenziali di amministratore del dominio PRIV. Al termine dell'operazione, riavviare il computer PRIVWKSTN.

Per altri dettagli, vedere l'articolo relativo alla [sicurezza delle workstation con accesso con privilegi](https://technet.microsoft.com/en-us/library/mt634654.aspx).

Nel passaggio successivo si preparerà un server PAM.

>[!div class="step-by-step"]
[« Passaggio 1](step-1-prepare-corp-domain.md)
[Passaggio 3 »](step-3-prepare-pam-server.md)
