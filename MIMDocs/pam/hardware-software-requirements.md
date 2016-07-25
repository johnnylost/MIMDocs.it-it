---
title: Requisiti software di PAM | Microsoft Identity Manager
description: Individuare i requisiti hardware e software per una corretta distribuzione di Privileged Access Management
keywords: 
author: kgremban
manager: femila
ms.date: 07/15/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 82a9085c-9667-4b3b-8079-657eab1d1e58
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: ae4c40c73dd9d5860f42e00765a7e34e8ca397a9
ms.openlocfilehash: 75a748f7035cfb10e833e4fdbfdc208b5245d3ea


---

# Requisiti hardware e software

Per Privileged Access Management non esistono requisiti hardware oltre a quelli delle piattaforme software sottostanti. Assicurarsi che la memoria e lo spazio su disco siano sufficienti e che la connettività di rete sia attiva.

Questo articolo descrive i requisiti minimi per una distribuzione di base. Non è destinato a illustrare le prestazioni, la scalabilità o la disponibilità elevata e non rappresenta una topologia di distribuzione consigliata per grandi imprese o ambienti di produzione.

## Installazione da pacchetti software

Il software seguente può essere scaricato da TechNet Evaluation Center o MSDN:  
- Microsoft Identity Manager 2016
  - Servizio e portale: contiene il programma di installazione per il servizio MIM e il portale MIM e per la funzionalità PAM
  - Componenti aggiuntivi ed estensioni: contiene il programma di installazione per i cmdlet di PowerShell per il richiedente

Il software seguente può essere scaricato da GitHub:  
- PAMSamplePortal: contiene l'applicazione Web di esempio per l'API REST

## Software richiesto

- Windows Server 2012 R2  
- Windows 8.1 Enterprise o Windows 10 Enterprise  
- SQL Server 2012 Service Pack 1 o SQL Server 2014  

## Software di valutazione

Se non si dispone di licenze per Windows, SQL Server o Windows Server è possibile scaricare le versioni di valutazione.

### TechNet Evaluation Center

- [Windows Server 2012 R2](https://www.microsoft.com/evalcenter/evaluate-windows-server-2012-r2)  
- [Windows 8,1 Enterprise](https://www.microsoft.com/evalcenter/evaluate-windows-8-1-enterprise)  
- [Windows 10 Enterprise](https://www.microsoft.com/evalcenter/evaluate-windows-10-enterprise)  

### Area download Microsoft

- [SQL Server](https://www.microsoft.com/download/details.aspx?id=29066)  
- [SharePoint Foundation 2013 SP1 e relativi prerequisiti](https://www.microsoft.com/download/details.aspx?id=42039)

## Requisiti hardware

Per ogni componente di PAM, consultare i requisiti di sistema dei prodotti software.

Per CORPDC:  
- [Windows Server 2012 R2](https://technet.microsoft.com/library/dn303418.aspx) o versioni precedenti

Per CORPWKSTN:  
- [Windows 8.1](http://windows.microsoft.com/windows-8/system-requirements)

Per PRIVDC:  
- [Windows Server 2012 R2](https://technet.microsoft.com/library/dn303418.aspx)

Per PAMSRV:
- [Windows Server 2012 R2](https://technet.microsoft.com/library/dn303418.aspx)  
- [SQL Server 2012](https://msdn.microsoft.com/library/ms143506(sql.110).aspx) o [SQL Server 2014](https://msdn.microsoft.com/en-us/library/ms143506(v=sql.120).aspx)



<!--HONumber=Jul16_HO3-->

