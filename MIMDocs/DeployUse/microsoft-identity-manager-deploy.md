---
title: Distribuire MIM 2016 | Microsoft Identity Manager
description: Ottenere l'elenco completo dei passaggi coinvolti nella distribuzione di Microsoft Identity Manager 2016, dalla preparazione dell'ambiente alla configurazione dei portali.
keywords: 
author: kgremban
manager: stevenpo
ms.date: 04/28/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: fa0af422-b5e9-4599-9d9b-cb6c18ea07f9
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: ca7fdef81eb8a68aff46df528e1989f019f5d2a4
ms.openlocfilehash: a56ead9777f1dad1aa0d214a506cf1242f51e167


---

# Distribuire MIM 2016
Gli articoli di questa sezione forniscono istruzioni dettagliate per la distribuzione di Microsoft Identity Manager (MIM) 2016 per scenari self-service dell'utente finale in un server aggiornato in cui FIM o MIM non sia stato distribuito in precedenza.

> [!NOTE]
> La topologia di distribuzione descritta in questa sezione è destinata solo all’introduzione e alla formazione relativa a MIM.  La [guida per la pianificazione della capacità](/microsoft-identity-manager/plan-design/capacity-planning-guide) fornisce altre informazioni sulle topologie per le distribuzioni di produzione.  Si consiglia di esaminare la documentazione prima di distribuire il MIM per la scala di produzione o per l’utilizzo.

<!---
Comment: Restore after PAM content is included

The privileged access management scenario is deployed differently than other MIM scenarios, as it requires a dedicated bastion forest environment.  If you want to learn more about deploying MIM for Privileged Identity Management, see [Getting Started with Privileged Access Management](privileged-access-management-get-started.md).
--->

## Primo passaggio: preparare un dominio
MIM funziona con Active Directory (AD), quindi seguire questi passaggi per configurare il controller di dominio di Active Directory.
- [Configurazione del dominio](preparing-domain.md)

## Passaggio successivo: preparare un server di gestione delle identità
Dopo aver configurato il dominio, preparare il server di gestione delle identità aziendali. Sono incluse le seguenti operazioni:
- [Windows Server 2012 R2](prepare-server-ws2012r2.md)
- [SQL Server 2014](prepare-server-sql2014.md)
- [SharePoint](prepare-server-sharepoint.md)
- [Exchange Server](prepare-server-exchange.md) (facoltativo)

## Infine, installare i componenti di Microsoft Identity Manager 2016
Dopo avere configurato il dominio e il server, si è pronti per installare i componenti MIM e configurarli per la sincronizzazione con Active Directory.
- [Servizio di sincronizzazione MIM](install-mim-sync.md)
- [Servizio e portale MIM](install-mim-service-portal.md)
- [Sincronizzare i database di Active Directory e del servizio MIM](install-mim-sync-ad-service.md)



<!--HONumber=Jun16_HO4-->


