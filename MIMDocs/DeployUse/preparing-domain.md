---
# required metadata

title: Configurare un dominio | Microsoft Identity Manager
description: Creare un controller di dominio di Active Directory prima di installare MIM 2016
keywords:
author: kgremban
manager: stevenpo
ms.date: 04/28/2016
ms.topic: get-started-article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 50345fda-56d7-4b6e-a861-f49ff90a8376

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Configurare un dominio

>[!div class="step-by-step"]  
[Windows Server 2012 R2 »](prepare-server-ws2012r2.md)

Microsoft Identity Manager (MIM) funziona con il dominio di Active Directory (AD). Dopo aver installato AD, verificare di disporre di un controller di dominio nel proprio ambiente per un dominio che è possibile amministrare.

Questo articolo illustra la procedura per preparare il domino alla collaborazione con MIM.

## Creare account utente e gruppi di utenti

Tutti i componenti della distribuzione di MIM devono avere le proprie identità nel dominio. Questo include i componenti MIM come il servizio e la sincronizzazione, SharePoint e SQL.

> [!NOTE]
> Questa procedura dettagliata usa nomi e valori di esempio della società Contoso. Sostituirli con i propri nomi e valori. Ad esempio:
> - Nome del controller di dominio: **mimservername**
> - Nome del dominio: **contoso**
> - Password: **Pass@word1**

1. Accedere al controller di dominio come amministratore di dominio (*ad es. Contoso\Administrator*).

2. Creare gli account utente seguenti per i servizi MIM. Avviare PowerShell e digitare lo script di PowerShell seguente per aggiornare il dominio.

    ```
    import-module activedirectory
    $sp = ConvertTo-SecureString "Pass@word1" –asplaintext –force
    New-ADUser –SamAccountName MIMMA –name MIMMA
    Set-ADAccountPassword –identity MIMMA –NewPassword $sp
    Set-ADUser –identity MIMMA –Enabled 1 –PasswordNeverExpires 1
    New-ADUser –SamAccountName MIMSync –name MIMSync
    Set-ADAccountPassword –identity MIMSync –NewPassword $sp
    Set-ADUser –identity MIMSync –Enabled 1 –PasswordNeverExpires 1
    New-ADUser –SamAccountName MIMService –name MIMService
    Set-ADAccountPassword –identity MIMService –NewPassword $sp
    Set-ADUser –identity MIMService –Enabled 1 –PasswordNeverExpires 1
    New-ADUser –SamAccountName MIMSSPR –name MIMSSPR
    Set-ADAccountPassword –identity MIMSSPR –NewPassword $sp
    Set-ADUser –identity MIMSSPR –Enabled 1 –PasswordNeverExpires 1
    New-ADUser –SamAccountName SharePoint –name SharePoint
    Set-ADAccountPassword –identity SharePoint –NewPassword $sp
    Set-ADUser –identity SharePoint –Enabled 1 –PasswordNeverExpires 1
    New-ADUser –SamAccountName SqlServer –name SqlServer
    Set-ADAccountPassword –identity SqlServer –NewPassword $sp
    Set-ADUser –identity SqlServer –Enabled 1 –PasswordNeverExpires 1
    New-ADUser –SamAccountName BackupAdmin –name BackupAdmin
    Set-ADAccountPassword –identity BackupAdmin –NewPassword $sp
    Set-ADUser –identity BackupAdmin –Enabled 1 -PasswordNeverExpires 1
    ```

2.  Creare i gruppi di sicurezza per tutti i gruppi.

    ```
    New-ADGroup –name MIMSyncAdmins –GroupCategory Security –GroupScope Global      –SamAccountName MIMSyncAdmins
    New-ADGroup –name MIMSyncOperators –GroupCategory Security –GroupScope Global       –SamAccountName MIMSyncOperators
    New-ADGroup –name MIMSyncJoiners –GroupCategory Security –GroupScope Global         –SamAccountName MIMSyncJoiners
    New-ADGroup –name MIMSyncBrowse –GroupCategory Security –GroupScope Global      –SamAccountName MIMSyncBrowse
    New-ADGroup –name MIMSyncPasswordReset –GroupCategory Security –GroupScope Global          –SamAccountName MIMSyncPasswordReset
    Add-ADGroupMember -identity MIMSyncAdmins -Members Administrator
    Add-ADGroupmember -identity MIMSyncAdmins -Members MIMService
    ```

3.  Aggiungere i nomi SPN per abilitare l'autenticazione Kerberos per gli account di servizio

    ```
    setspn -S http/mimservername.contoso.local Contoso\SharePoint
    setspn -S http/mimservername Contoso\SharePoint
    setspn -S MIMService/mimservername.contoso.local Contoso\MIMService
    setspn -S MIMSync/mimservername.contoso.local Contoso\MIMSync
    ```

>[!div class="step-by-step"]  
[Windows Server 2012 R2 »](prepare-server-ws2012r2.md)


<!--HONumber=May16_HO3-->


