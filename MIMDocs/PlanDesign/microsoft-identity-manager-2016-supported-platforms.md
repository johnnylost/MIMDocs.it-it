---
title: Piattaforme software supportate | Documentazione Microsoft
description: Trovare i prodotti e le versioni compatibili con ciascuno dei componenti MIM 2016
keywords: 
author: kgremban
ms.author: kgremban
manager: femila
ms.date: 03/28/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 4978f60d-044d-4e84-8d93-65801fce1144
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: f1faed3a09023e288a68a8950dae43725f19eb3e
ms.openlocfilehash: 33c84afa4d6fd2ed7bde33de39dd151f83f07fd4
ms.lasthandoff: 04/12/2017


---

# <a name="supported-platforms-for-mim-2016"></a>Piattaforme supportate per MIM 2016

La tabella seguente descrive la versione e le piattaforme supportate per ogni componente di Microsoft Identity Manager 2016. Le versioni contrassegnate con un asterisco (*) sono supportate solo in MIM 2016 Service Pack 1.


| **Componente MIM** | **Piattaforma** | **Versione** |
|-------------------|--------------|-------------|
| **Sincronizzazione MIM** | Windows Server | Windows Server 2008 R2 SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2<br/>Windows Server 2016 * |
| | Database di sincronizzazione MIM | SQL Server 2008 R2 SP3<br/>SQL Server 2012 SP2<br/>SQL Server 2014 SP1 <br/> SQL Server 2016 * |
| | Active Directory per il provisioning degli utenti, PCNS e sincronizzazione GAL (facoltativo)|Windows Server 2008 R2 SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | Exchange per il provisioning delle cassette postali e sincronizzazione GAL (facoltativo)|Exchange Server 2010 SP3<br/>Exchange Server 2013 SP1<br/>Exchange Server 2016* |
| | Ambiente di sviluppo (facoltativo) | Visual Studio 2012<br/>Visual Studio 2013 <br/> Visual Studio 2015 <br/> Visual Studio 2017* |
| | Sistema connesso aggiuntivo (facoltativo) | Servizi di dominio di Active Directory<br/>Active Directory<br/>Lightweight Directory Services<br/>SQL Server 2000 o versioni successive<br/>SharePoint Server 2013<br/> SharePoint Server 2016 * <br/> Altri prodotti di terze parti |
| **Servizio MIM** (eccetto scenario PAM) | Windows Server | Windows Server 2008 R2 SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | Database del servizio MIM | SQL Server 2008 R2 SP3<br/>SQL Server 2012 SP2<br/>SQL Server 2014 SP1 <br/> SQL Server 2016 * |
| | Exchange per l'approvazione servizio MIM e indirizzi di posta elettronica per la gestione del gruppo (facoltativo) | Exchange Server 2010 SP3<br/>Exchange Server 2013 SP1 <br/> Exchange Server 2016 * <br/> Exchange Online * (solo notifica) |
| **Servizio e portale MIM** (solo scenario PAM)| Windows Server | Windows Server 2012<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | Active Directory per la foresta PAM dell’ambiente bastion | Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | Active Directory per le foreste esistenti | Windows Server 2008 <br/> Windows Server 2008 R2 * <br/> Windows Server 2012 * <br/> Windows Server 2012 R2 * <br/> Windows Server 2016 * |
| | Database del servizio MIM | SQL Server 2008 R2 SP3<br/>SQL Server 2012 SP2<br/>SQL Server 2014 SP1 <br/> SQL Server 2016 * |
| | SharePoint | SharePoint Foundation 2010<br/>SharePoint Foundation 2013 SP1 <br/> SharePoint 2016 * |
| | Server di posta elettronica per l'approvazione del servizio MIM e indirizzi di posta elettronica per la gestione del gruppo (facoltativo) | Exchange Server 2010 SP3<br/>Exchange Server 2013 SP1 <br/> Exchange Server 2016 * <br/> Exchange Online * (solo notifica) |
| | Browser | Tutti i principali browser |
| **Servizio MIM Reporting** | Windows Server | Windows Server 2012 <br/> Windows Server 2016 * |
| | Data warehouse | System Center 2012 Service Manager <br/> System Center 2012 R2 Service Manager </br> System Center 2016 Service Manager * (con 4.4.1459)<br/> [Compatibilità delle versioni di SQL Server per System Center 2016](https://technet.microsoft.com/en-us/system-center-docs/system-requirements/sql-server-version-compatibility)
 |
| **Reimpostazione della password MIM e portali di registrazione** | Windows Server | Windows Server 2008 R2 SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | Web browser | Tutti i principali browser |
| **Componenti aggiuntivi ed estensioni MIM** | Windows | Windows 7<br/>Windows 8<br/>Windows 8.1<br/>Windows 10 |
| | Integrazione di Outlook (facoltativo) | Outlook 2007 SP2<br/>Outlook 2010<br/>Outlook 2013 <br/> Outlook 2016 (in Windows 10) * |
| | Cmdlet richiedente PAM PowerShell (facoltativo) | Windows 8.1<br/>Windows 10 |
| **Gestione certificati MIM** (integrazione di server e autorità di certificazione) | Windows server | Windows Server 2008 R2 SP1<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | Autorità di certificazione | Windows Server 2008 R2 SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | Database di gestione certificati MIM | SQL Server 2008 R2 SP3<br/>SQL Server 2012 SP2<br/>SQL Server 2014 SP1 <br/> SQL Server 2016 * |
| **Gestione certificati MIM** (applicazione) | Windows | Windows 8<br/>Windows 8.1<br/>Windows 10 |
| **Gestione certificati MIM** (client e client bulk) | Windows | Windows 7 |
| **MIM BHOLD Suite** | Windows Server | Windows Server 2008 R2 SP1<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | Database BHOLD | SQL Server 2008 R2 SP3<br/>SQL Server 2012 SP2 <br/> SQL Server 2014 * <br/> SQL Server 2016 * |
| | Server di posta elettronica (facoltativo) | Exchange Server 2010 SP3<br/>Exchange Server 2013 SP1 <br/> Exchange Server 2016 * |
| | Web browser | Internet Explorer 7, 8, 9, 10 o 11 con Silverlight |

