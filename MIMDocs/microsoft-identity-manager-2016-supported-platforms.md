---
title: Piattaforme software supportate | Documentazione Microsoft
description: Trovare i prodotti e le versioni compatibili con ciascuno dei componenti MIM 2016
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 04/11/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 4978f60d-044d-4e84-8d93-65801fce1144
ms.reviewer: ''
ms.suite: ems
ms.custom: mim
ms.openlocfilehash: 0501dbeb279dc37655d1d9a5e99545b07eea0623
ms.sourcegitcommit: 7de35aaca3a21192e4696fdfd57d4dac2a7b9f90
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/16/2018
ms.locfileid: "49358687"
---
# <a name="supported-platforms-for-mim-2016"></a>Piattaforme supportate per MIM 2016

La tabella seguente descrive la versione e le piattaforme supportate per ogni componente di Microsoft Identity Manager 2016. Le versioni contrassegnate con un asterisco (*) sono supportate solo in MIM 2016 Service Pack 1 con l'hotfix più recente.


| **Componente MIM** | **Piattaforma** | **Versione** |
|-------------------|--------------|--------------|
| **Sincronizzazione MIM** | Windows Server | Windows Server 2008 R2 SP1<br/>Windows Server 2012<br/>R2 per Windows Server 2012<br/>Windows Server 2016 * |
| | Livello funzionale di Active Directory per provisioning utenti, PCNS e sincronizzazione elenco indirizzi globale | Windows 2000 <br/>Windows Server 2003<br/>Windows Server 2008<br/>Windows Server 2008 R2<br/>Windows Server 2012<br/>R2 per Windows Server 2012<br/>Windows Server 2016 *
| | Database di sincronizzazione MIM | SQL Server 2008 R2 SP3<br/>SQL Server 2012 SP2<br/>SQL Server 2014 SP1 <br/> SQL Server 2016 * |
| | Active Directory per provisioning utenti, PCNS e sincronizzazione elenco indirizzi globale (facoltativo)|Windows Server 2008 R2 SP1<br/>Windows Server 2012<br/>R2 per Windows Server 2012 <br/> Windows Server 2016 * |
| | Exchange per il provisioning delle cassette postali e sincronizzazione GAL (facoltativo)|Exchange Server 2010 SP3<br/>Exchange Server 2013 SP1<br/>Exchange Server 2016* |
| | Ambiente di sviluppo (facoltativo) | Visual Studio 2012<br/>Visual Studio 2013 <br/> Visual Studio 2015 <br/> Visual Studio 2017* |
| | Sistema connesso aggiuntivo (facoltativo) | Servizi di dominio di Active Directory<br/>Active Directory<br/>Lightweight Directory Services<br/>SQL Server 2008 o versioni successive<br/>SharePoint Server 2013<br/> SharePoint Server 2016 * <br/> Altri prodotti di terze parti |
| **Servizio e portale MIM** | Windows Server | Windows Server 2008 R2 SP1<br/>Windows Server 2012<br/>R2 per Windows Server 2012 <br/> Windows Server 2016 * |
| |Scenario PAM: Windows Server | R2 per Windows Server 2012 <br/> Windows Server 2016 * |
| |Scenario PAM: Active Directory per la foresta PAM dell'ambiente bastion | R2 per Windows Server 2012 <br/> Windows Server 2016 * |
| |Scenario PAM: Active Directory per le foreste esistenti (CORP) degli scenari PAM | Windows Server 2008 <br/> Windows Server 2008 R2 * <br/> Windows Server 2012 * <br/> Windows Server 2012 R2 * <br/> Windows Server 2016 * |
| | Database del servizio MIM | SQL Server 2008 R2 SP3<br/>SQL Server 2012 SP2<br/>SQL Server 2014 SP1 <br/> SQL Server 2016 |
| | SharePoint | SharePoint Foundation 2010<br/>SharePoint Foundation 2013 SP1 <br/> SharePoint 2016 * |
| | Server di posta elettronica per l'approvazione del servizio MIM e indirizzi di posta elettronica per la gestione del gruppo (facoltativo) | Exchange Server 2010 SP3<br/>Exchange Server 2013 SP1 <br/> Exchange Server 2016 * <br/> Exchange Online * (solo notifica prima della build [4.4.1749.0](https://docs.microsoft.com/microsoft-identity-manager/reference/version-history#version-4417490)) |
| | Browser | Tutti i principali browser supportati * (solo dispositivi mobili)|
| **Servizio MIM Reporting** | Windows Server |  Windows Server 2008 R2 SP1<br/>Windows Server 2012 <br/>R2 per Windows Server 2012 <br/> Windows Server 2016 * |
| | Data warehouse | System Center 2012 Service Manager <br/> System Center 2012 R2 Service Manager </br> System Center 2016 Service Manager * (con 4.4.1459)<br/> [Compatibilità delle versioni di SQL Server per System Center 2016](https://docs.microsoft.com/system-center/scsm/upgrade-to-sm-2016)
 |
| **Reimpostazione della password MIM e portali di registrazione** | Windows Server | Windows Server 2008 R2 SP1<br/>Windows Server 2012<br/>R2 per Windows Server 2012 <br/> Windows Server 2016 * |
| | Web browser | Tutti i principali browser supportati |
| **Componenti aggiuntivi ed estensioni MIM** | Windows | Windows 7<br/>Windows 8<br/>Windows 8.1<br/>Windows 10 |
| | Integrazione di Outlook (facoltativo) | Outlook 2010<br/>Outlook 2013 <br/> Outlook 2016 (in Windows 10) * |
| | Cmdlet richiedente PAM PowerShell (facoltativo) | Windows 8.1<br/>Windows 10 |
| **Gestione certificati MIM** (integrazione di server e autorità di certificazione) | Windows server | Windows Server 2008 R2 SP1<br/>R2 per Windows Server 2012 <br/> Windows Server 2016 * |
| | Autorità di certificazione | Windows Server 2008 R2 SP1<br/>Windows Server 2012<br/>R2 per Windows Server 2012 <br/> Windows Server 2016 * |
| | Database di gestione certificati MIM | SQL Server 2008 R2 SP3<br/>SQL Server 2012 SP2<br/>SQL Server 2014 SP1 <br/> SQL Server 2016 * |
| **Gestione certificati MIM**  (applicazione) | Windows | Windows 8<br/>Windows 8.1<br/>Windows 10 |
| **Gestione certificati MIM**  (Bulk Client) | Windows | Windows 7 |
| **Gestione certificati MIM** (smart card basata su ActiveX per client) | Windows | Windows 7 </br> Windows 8 </br> Windows 8.1 </br> Windows 10 |
| **MIM BHOLD Suite** | Windows Server | Windows Server 2008 R2 SP1<br/>R2 per Windows Server 2012 <br/> Windows Server 2016 * |
| | Database BHOLD | SQL Server 2008 R2 SP3<br/>SQL Server 2012 SP2 <br/> SQL Server 2014 * <br/> SQL Server 2016 * |
| | Server di posta elettronica (facoltativo) | Exchange Server 2010 SP3<br/>Exchange Server 2013 SP1 <br/> Exchange Server 2016 * |
| | Web browser | Browser supportati Internet Explorer con Silverlight |
