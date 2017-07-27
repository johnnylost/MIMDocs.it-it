---
title: Panoramica sull'ambiente PAM | Documentazione Microsoft
description: Individuare la configurazione e il numero necessario di macchine virtuali per una corretta distribuzione di Privileged Access Management
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/15/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 479db14c-1bfb-4d7c-a344-cd718a01f328
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 3e6c5a70c6b9ed140a56135676bbd14a84504317
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/13/2017
---
# <a name="environment-overview"></a>Panoramica sull'ambiente

Privileged Access Management usa quattro macchine virtuali (VM) con unità separate connesse l'una all'altra in una rete condivisa. Queste macchine virtuali possono essere ospitate da Windows 8.1, Windows Server 2012 R2 o altre piattaforme di sistema operativo.

![Server PAM: relazioni e piattaforme supportate - Diagramma](media/pam-test-lab-architecture.png)

Sono necessarie almeno tre macchine virtuali.  Se non si dispone già di un dominio AD per PAM per la gestione, sarà necessaria un'ulteriore VM da usare come controller di dominio CORP.  Se si vuole configurare il software PRIV per la disponibilità elevata, saranno necessarie anche due VM aggiuntive.

Le unità in cui archiviare le immagini disco di macchina virtuale deve avere almeno 120 GB di spazio libero su disco per contenere tutte le VM.  Se si intende distribuire la disponibilità elevata, assicurarsi che il sottosistema del disco soddisfi i requisiti per l'archiviazione condivisa di SQL.  L'archiviazione condivisa può essere sotto forma di dischi cluster di Windows Server Failover Clustering, dischi in una rete di archiviazione (SAN) o condivisioni di file in un server SMB. Si noti che questi elementi devono essere specifici dell'ambiente bastion. L'archiviazione condivisa con altri carichi di lavoro esterni all'ambiente bastion non è consigliabile perché potrebbe compromettere l'integrità dell'ambiente bastion.

> [!NOTE]
> La versione CTP corrente non è compatibile con il contenuto della directory o del database della versione CTP precedente. Se è stato precedentemente valutato MIM per PAM o altri scenari, eseguire il backup e archiviare le macchine virtuali utilizzate per il test e avviare la distribuzione con nuove immagini di macchina virtuale che non siano state precedentemente utilizzate per scenari MIM.
