---
title: Requisiti software di PAM | Documentazione Microsoft
description: Individuare i requisiti hardware e software per una corretta distribuzione di Privileged Access Management
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/15/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 82a9085c-9667-4b3b-8079-657eab1d1e58
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: bfc73723bdd3a49529522f78ac056939bb8025a3
ms.openlocfilehash: 2985215821db843d2f90d8a34250a8ca6a84b592
ms.lasthandoff: 05/02/2017


---

# <a name="hardware-and-software-requirements"></a>Requisiti hardware e software

Per Privileged Access Management non esistono requisiti hardware oltre a quelli delle piattaforme software sottostanti. Assicurarsi che la memoria e lo spazio su disco siano sufficienti e che la connettività di rete sia attiva.

Questo articolo descrive i requisiti minimi per una distribuzione di base. Non è destinato a illustrare le prestazioni, la scalabilità o la disponibilità elevata e non rappresenta una topologia di distribuzione consigliata per grandi imprese o ambienti di produzione.

## <a name="installing-from-software-packages"></a>Installazione da pacchetti software

Il software seguente può essere scaricato da TechNet Evaluation Center o MSDN:  
- Microsoft Identity Manager 2016
  - Servizio e portale: contiene il programma di installazione per il servizio MIM e il portale MIM e per la funzionalità PAM
  - Componenti aggiuntivi ed estensioni: contiene il programma di installazione per i cmdlet di PowerShell per il richiedente

Il software seguente può essere scaricato da GitHub:  
- PAMSamplePortal: contiene l'applicazione Web di esempio per l'API REST

## <a name="required-software"></a>Software richiesto

- Windows Server 2012 R2  
- Windows 8.1 Enterprise o Windows 10 Enterprise  
- SQL Server 2012 Service Pack 1 o SQL Server 2014  

## <a name="evaluation-software"></a>Software di valutazione

Se non si dispone di licenze per Windows, SQL Server o Windows Server è possibile scaricare le versioni di valutazione.

### <a name="technet-evaluation-center"></a>TechNet Evaluation Center

- [Windows Server 2012 R2](https://www.microsoft.com/evalcenter/evaluate-windows-server-2012-r2)  
- [Windows 8.1 Enterprise](https://www.microsoft.com/evalcenter/evaluate-windows-8-1-enterprise)  
- [Windows 10 Enterprise](https://www.microsoft.com/evalcenter/evaluate-windows-10-enterprise)  

### <a name="microsoft-download-center"></a>Area download Microsoft

- [SQL Server](https://www.microsoft.com/download/details.aspx?id=29066)  
- [SharePoint Foundation 2013 SP1 e relativi prerequisiti](https://www.microsoft.com/download/details.aspx?id=42039)

## <a name="hardware-requirements"></a>Requisiti hardware

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

