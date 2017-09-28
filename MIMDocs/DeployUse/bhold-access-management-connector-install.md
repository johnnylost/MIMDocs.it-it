---
title: Installazione di BHOLD Access Management Connector | Microsoft Docs
description: Il modulo BHOLD Connector supporta la sincronizzazione iniziale e continuativa dei dati
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/07/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 
ms.openlocfilehash: 6d7f19f470d0c0f82a68652115ab9265a13b3508
ms.sourcegitcommit: ed8dd5563e77ef4a3345b2a52a1426859c95576a
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/15/2017
---
# <a name="access-management-connector-installation"></a>Installazione di Access Management Connector

Il modulo Access Management Connector di BHOLD Suite supporta la sincronizzazione sia iniziale che continuativa dei dati in BHOLD. Access Management Connector funziona con il servizio di sincronizzazione Microsoft Identity Manager (MIM) per lo spostamento di dati tra il database BHOLD Core, il metaverse FIM 2010 e le applicazioni e gli archivi identità di destinazione. Dopo aver installato il modulo Access Management Connector, sarà possibile creare gli agenti di gestione FIM che controllano il flusso di dati tra BHOLD e MIM.

## <a name="access-management-connector-software-requirements"></a>Requisiti software di Access Management Connector

Prima di installare il modulo Access Management Connector, è necessario installare Microsoft .NET Framework 4. Per ulteriori informazioni su .NET Framework 4 e per le istruzioni di installazione, vedere la [home page di Microsoft .NET](http://www.microsoft.com/net).
È necessario installare Access Management Connector su un computer dotato del servizio di sincronizzazione FIM di MIM.

## <a name="access-management-connector-setup"></a>Configurazione di Access Management Connector

Per installare il modulo Access Management Connector, accedere come membro del gruppo Domain Admins, scaricare il file seguente ed eseguirlo come amministratore nel server in cui si intende installare il modulo BHOLD FIM Integration:

- AccessManagementConnector.msi

Per eseguire il file di programma come amministratore, fare clic con il pulsante destro del mouse sul file e selezionare **Esegui come amministratore**.

## <a name="next-steps"></a>Passaggi successivi

- [Installazione dell'integrazione BHOLD FIM](https://technet.microsoft.com/library/jj134093(v=ws.10).aspx) Per abilitare la funzionalità self-service dei ruoli per l'utente finale, è possibile installare il modulo di integrazione di BHOLD FIM
- [Guida all'installazione di BHOLD](bhold-installation-guide.md)
- [Riferimento per gli sviluppatori BHOLD](../reference/mim2016-bhold-developer-reference.md)
- [Cronologia delle versioni BHOLD](../reference/version-bhold-history.md)