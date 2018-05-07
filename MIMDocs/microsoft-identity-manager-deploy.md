---
title: Passaggi necessari per distribuire Microsoft Identity Manager 2016 | Documentazione Microsoft
description: Ottenere l'elenco completo dei passaggi coinvolti nella distribuzione di Microsoft Identity Manager 2016, dalla preparazione dell'ambiente alla configurazione dei portali.
keywords: ''
author: billmath
ms.author: barclayn
manager: mbaldwin
ms.date: 10/12/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: fa0af422-b5e9-4599-9d9b-cb6c18ea07f9
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: a1f2a30dd6d8519ec09ea3765e5584123725fe03
ms.sourcegitcommit: 32d9a963a4487a8649210745c97a3254645e8744
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/27/2018
---
# <a name="deploy-microsoft-identity-manager-2016-sp1"></a>Distribuire Microsoft Identity Manager 2016 SP1
Gli articoli di questa sezione forniscono istruzioni dettagliate per la distribuzione di Microsoft Identity Manager (MIM) 2016 per scenari self-service dell'utente finale in un server aggiornato in cui FIM o MIM non sia stato distribuito in precedenza.

> [!NOTE]
> La topologia di distribuzione descritta in questa sezione è destinata solo all’introduzione e alla formazione relativa a MIM.  La [guida per la pianificazione della capacità](capacity-planning-guide.md) fornisce altre informazioni sulle topologie per le distribuzioni di produzione.  Si consiglia di esaminare la documentazione prima di distribuire il MIM per la scala di produzione o per l’utilizzo.

Lo scenario di gestione accesso con privilegi, viene distribuito in modo diverso rispetto ad altri scenari di MIM, poiché richiede un ambiente di foreste bastion dedicato.  Per altre informazioni sulla distribuzione di MIM per Privileged Identity Management, vedere [Configurazione dell'ambiente MIM per Privileged Access Management](./pam/configuring-mim-environment-for-pam.md).

Il processo di distribuzione di MIM è molto simile a quello del suo predecessore, FIM 2010 R2. Per consultare la documentazione di FIM, vedere la [guida alla distribuzione di Forefront Identity Manager 2010 R2](https://technet.microsoft.com/library/jj134310).

## <a name="first-prepare-a-domain"></a>Primo passaggio: preparare un dominio
MIM funziona con Active Directory (AD), quindi seguire questi passaggi per configurare il controller di dominio di Active Directory.
- [Configurazione del dominio](preparing-domain.md)

## <a name="next-prepare-an-identity-management-servers"></a>Passaggio successivo: preparare i server di gestione delle identità
Dopo aver configurato il dominio, preparare il server di gestione delle identità aziendali. Sono incluse le seguenti operazioni:
- [Windows Server 2012 R2](prepare-server-ws2016.md)
- [SQL Server 2016](prepare-server-sql2016.md)
- [SharePoint 2016](prepare-server-sharepoint.md)
- [Exchange Server](prepare-server-exchange.md) (facoltativo)

## <a name="finally-install-microsoft-identity-manager-2016-sp1-components"></a>Infine, installare i componenti di Microsoft Identity Manager 2016 SP1
Dopo avere configurato il dominio e il server, si è pronti per installare i componenti MIM e configurarli per la sincronizzazione con Active Directory.
- [Servizio di sincronizzazione MIM](install-mim-sync.md)
- [Servizio e portale MIM](install-mim-service-portal.md)
- [Sincronizzare i database di Active Directory e del servizio MIM](install-mim-sync-ad-service.md)
- [Piattaforme supportate per MIM 2016 o versione successiva](microsoft-identity-manager-2016-supported-platforms.md)
