---
title: Panoramica sull'ambiente PAM | Documentazione Microsoft
description: Individuare la configurazione e il numero necessario di macchine virtuali per una corretta distribuzione di Privileged Access Management
keywords: ''
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 08/31/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 479db14c-1bfb-4d7c-a344-cd718a01f328
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: e83c326d32645ce80541d5c415cd9c0e9d1dae54
ms.sourcegitcommit: 35f2989dc007336422c58a6a94e304fa84d1bcb6
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36288793"
---
# <a name="environment-overview"></a>Panoramica sull'ambiente

Privileged Access Management usa quattro macchine virtuali (VM) con unità separate connesse l'una all'altra in una rete condivisa. Queste macchine virtuali possono essere ospitate da Windows 8.1, Windows Server 2012 R2 o altre piattaforme di sistema operativo.

![Server PAM: relazioni e piattaforme supportate - Diagramma](media/pam-test-lab-architecture.png)

Sono necessarie almeno tre macchine virtuali.  Se non si dispone già di un dominio AD per PAM per la gestione, è necessaria una VM aggiuntiva da usare come controller di dominio CORP.  Se si vuole configurare il software PRIV per la disponibilità elevata, sono necessarie anche due VM aggiuntive.

Le unità in cui archiviare le immagini disco di VM devono avere almeno 120 GB di spazio libero su disco.  Se si intende distribuire la disponibilità elevata, assicurarsi che il sottosistema del disco soddisfi i requisiti per l'archiviazione condivisa di SQL.  L'archiviazione condivisa può essere sotto forma di dischi cluster Windows Server Failover Clustering, dischi in una rete SAN (Storage Area Network) o condivisioni di file in un server SMB.

> [!IMPORTANT]
> L'archiviazione deve essere dedicata all'ambiente bastion. L'archiviazione condivisa all'esterno dell'ambiente bastion non è consigliata perché potrebbe compromettere l'integrità dell'ambiente bastion stesso.

## <a name="next-steps"></a>Passaggi successivi

- [Privileged Access Management per Active Directory Domain Services](privileged-identity-management-for-active-directory-domain-services.md) offre una panoramica di PAM e del suo funzionamento.
- [Informazioni sui componenti di PAM](principles-of-operation.md) offre una panoramica sui vari componenti di PAM.