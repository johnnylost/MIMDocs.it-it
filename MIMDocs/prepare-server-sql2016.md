---
title: Configurare SQL Server per Microsoft Identity Manager 2016 SP1 | Microsoft Docs
description: Installare SQL Server 2016 come operazione preliminare all'installazione di MIM 2016.
keywords: ''
author: billmath
ms.author: barclayn
manager: mbaldwin
ms.date: 04/26/2018
ms.topic: get-started-article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 297df3b3-192e-4ed9-82ed-c95eb5297c84
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 6fe251a3976167909aa55a687884585b1937ebf3
ms.sourcegitcommit: 28834821cbddd6384613d8ba45424c35f4c39ce6
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/13/2018
ms.locfileid: "45538558"
---
# <a name="set-up-an-identity-management-server-sql-server-2016"></a>Configurare un server di gestione delle identità: SQL Server 2016

> [!div class="step-by-step"]
> [« Windows Server 2016](prepare-server-ws2016.md)
> [SharePoint »](prepare-server-sharepoint.md)
> 
> [!NOTE]
> Questa procedura dettagliata usa nomi e valori di esempio della società Contoso. Sostituirli con i propri nomi e valori. Ad esempio:
> - Nome del controller di dominio: **corpdc**
> - Nome del dominio: **contoso**
> - Nome del server del servizio MIM: **corpservice**
> - Nome del server di sincronizzazione MIM: **corpservice**
> - Nome SQL Server: **corpsql**
> - Password: <strong>Pass@word1</strong>

## <a name="install-sql-server-2016-standardenterprise-edition"></a>Installare **SQL Server 2016 Standard/Enterprise Edition**

1. Avviare **PowerShell** come amministratore di dominio.

2. Passare alla directory in cui si trova il programma di installazione di SQL Server.

3. Digitare i comandi seguenti.

    ```
    .\setup.exe /Q /IACCEPTSQLSERVERLICENSETERMS /ACTION=install /FEATURES=SQL /INSTANCENAME=MSSQLSERVER /SQLSVCACCOUNT="contoso\SqlServer" /SQLSVCPASSWORD="Pass@word1"   /AGTSVCSTARTUPTYPE=Automatic /AGTSVCACCOUNT="NT AUTHORITY\Network Service" /SQLSYSADMINACCOUNTS="contoso\Administrator"

More info SQL deployment accounts and services can be found [here](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-windows-service-accounts-and-permissions?view=sql-server-2017)
> [!NOTE]
> SSMS is no longer included in SQL 2016 downlaod details can be found [here](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-2017)    ```
> 
> [!div class="step-by-step"]  
> [« Windows Server 2016](prepare-server-ws2016.md)
> [SharePoint »](prepare-server-sharepoint.md)
