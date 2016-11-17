---
title: 'Distribuire PAM, passaggio 6: Spostare un gruppo | Documentazione Microsoft'
description: Eseguire la migrazione di un gruppo nella foresta PRIV in modo che possa essere gestito con Privileged Access Management.
keywords: 
author: kgremban
ms.author: kgremban
manager: femila
ms.date: 07/15/2016
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 7b689eff-3a10-4f51-97b2-cb1b4827b63c
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 1f545bfb2da0f65c335e37fb9de9c9522bf57f25
ms.openlocfilehash: 9e70bdd3c586a16a9a55c91bd0e099a500950a64


---

# <a name="step-6-transition-a-group-to-privileged-access-management"></a>Passaggio 6: transizione di un gruppo a Privileged Access Management

>[!div class="step-by-step"]
[« Passaggio 5 ](step-5-establish-trust-between-priv-corp-forests.md)
[Passaggio 7 »](step-7-elevate-user-access.md)

La creazione di account con privilegi nella foresta PRIV avviene usando i cmdlet di PowerShell. Per installare i cmdlet, eseguire le operazioni seguenti:

- Creare un nuovo gruppo nella foresta PRIV con lo stesso ID di sicurezza (SID) di un gruppo nella foresta CORP.  
- Creare un oggetto nel database del servizio MIM corrispondente al gruppo nella foresta PRIV.  
- Per ogni account utente, creare due oggetti nel database del servizio MIM, corrispondenti all'utente nella foresta CORP e al nuovo account utente nella foresta PRIV.  
- Creare un oggetto Ruolo PAM nel database del servizio MIM.  

I cmdlet devono essere eseguiti una volta per ogni gruppo e una volta per ogni membro di un gruppo. I cmdlet di migrazione non cambiano o modificano alcun utente o gruppo nella foresta CORP. Tale operazione viene eseguita manualmente dall'amministratore PAM in un secondo momento.

1. Accedere a PAMSRV, direttamente o da una workstation PRIV, come *PRIV\MIMAdmin*.

2.  Avviare PowerShell e digitare i comandi seguenti.

    ```
    Import-Module MIMPAM
    Import-Module ActiveDirectory
    ```

3.  Creare un account utente corrispondente in PRIV per un account utente in una foresta esistente, a scopo dimostrativo.

    In PowerShell digitare i comandi seguenti.  Se non è stato usato il nome *Jen* per creare l'utente in contoso.local in precedenza, modificare i parametri del comando in modo appropriato. La password 'Pass@word1' è solo un esempio e deve essere modificata in un valore di password univoca.

    ```
    $sj = New-PAMUser –SourceDomain CONTOSO.local –SourceAccountName Jen
    $jp = ConvertTo-SecureString "Pass@word1" –asplaintext –force
    Set-ADAccountPassword –identity priv.Jen –NewPassword $jp
    Set-ADUser –identity priv.Jen –Enabled 1
    ```

4. Copiare un gruppo e il relativo membro, Jen, dal dominio CONTOSO al dominio PRIV, a scopo dimostrativo.

    Eseguire i comandi seguenti, specificando, quando richiesto, la password dell'amministratore di dominio CORP (CONTOSO\Administrator):

        ```
        $ca = get-credential –UserName CONTOSO\Administrator –Message "CORP forest domain admin credentials"
        $pg = New-PAMGroup –SourceGroupName "CorpAdmins" –SourceDomain CONTOSO.local                 –SourceDC CORPDC.contoso.local –Credentials $ca
        $pr = New-PAMRole –DisplayName "CorpAdmins" –Privileges $pg –Candidates $sj
        ```

    Per riferimento, il comando **New-PAMGroup** accetta i parametri seguenti:

        -   The CORP forest domain name in NetBIOS form  
        -   The name of the group to copy from that domain  
        -   The CORP forest Domain Controller NetBIOS name  
        -   The credentials of an domain admin user in the CORP forest  

5.  (Facoltativo) In CORPDC rimuovere l'account di Jen dal gruppo **CONTOSO CorpAdmins**, se è ancora presente.  Questo è necessario solo a scopo dimostrativo, per illustrare come è possibile associare le autorizzazioni agli account creati nella foresta PRIV.

    1.  Accedere a CORPDC come *CONTOSO\Administrator*.

    2.  Avviare PowerShell, eseguire il comando seguente e confermare la modifica.

        ```
        Remove-ADGroupMember -identity "CorpAdmins" -Members "Jen"
        ```


Se si vuole dimostrare che i diritti di accesso tra foreste sono efficaci per l'account dell'utente amministratore, continuare al passaggio successivo.

>[!div class="step-by-step"]
[« Passaggio 5 ](step-5-establish-trust-between-priv-corp-forests.md)
[Passaggio 7 »](step-7-elevate-user-access.md)



<!--HONumber=Nov16_HO2-->


