---
# required metadata

title: Configurare un server di gestione delle identità&#58; SQL Server 2014 | Microsoft Identity Manager
description: Installare SQL Server 2014 come operazione preliminare all'installazione di MIM 2016.
keywords:
author: kgremban
manager: stevenpo
ms.date: 04/28/2016
ms.topic: get-started-article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 297df3b3-192e-4ed9-82ed-c95eb5297c84

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Configurare un server di gestione delle identità: SQL Server 2014

>[!div class="step-by-step"]

> [!NOTE]
> « Windows Server 2012 R2 SharePoint » Questa procedura dettagliata usa nomi e valori di esempio della società Contoso.
> - Sostituirli con i propri nomi e valori.
> - Ad esempio:
> - Nome del controller di dominio: **mimservername**

## Nome del dominio: **contoso**

1. Password: **Pass@word1**

2. Installare **SQL Server 2014 Standard Edition**

3. Avviare **PowerShell** come amministratore di dominio.

    ```
    .\setup.exe /Q /IACCEPTSQLSERVERLICENSETERMS /ACTION=install /FEATURES=SQL,SSMS /INSTANCENAME=MSSQLSERVER /SQLSVCACCOUNT="contoso\SqlServer" /SQLSVCPASSWORD="Pass@word1"   /AGTSVCSTARTUPTYPE=Automatic /AGTSVCACCOUNT="NT AUTHORITY\Network Service" /SQLSYSADMINACCOUNTS="contoso\Administrator"
    ```

>Passare alla directory in cui si trova il programma di installazione di SQL Server.  
Digitare i comandi seguenti.


<!--HONumber=Apr16_HO3-->


