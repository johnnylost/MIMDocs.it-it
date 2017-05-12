---
title: Microsoft Identity Manager 2016 | Documentazione Microsoft
description: "MIM include le funzionalità di gestione dell&quot;accesso di FIM 2010 e consente di gestire gli utenti, le credenziali, i criteri e l&quot;accesso all&quot;interno dell&quot;organizzazione."
keywords: 
author: kgremban
ms.author: kgremban
manager: femila
ms.date: 01/18/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: ccdd8a9f-02da-440a-81a8-354800dcd2a8
ms.reviewer: mwahl
ms.suite: ems
ms.translationtype: Human Translation
ms.sourcegitcommit: 3797f5789bb4e48836eb21776dafd5a2e0e11613
ms.openlocfilehash: 3f5024cf760834ba2181252325a13e40bda520cd
ms.contentlocale: it-it
ms.lasthandoff: 05/09/2017


---

# <a name="microsoft-identity-manager-2016"></a>Microsoft Identity Manager 2016
Microsoft Identity Manager (MIM) 2016 è compilato sulla base delle funzionalità di gestione delle identità e degli accessi di [FIM 2010 R2](https://technet.microsoft.com/library/jj133885.aspx). Come il suo predecessore, MIM consente di gestire utenti, credenziali, criteri e accessi all'interno dell'organizzazione.  In aggiunta, MIM 2016 fornisce un'esperienza ibrida, funzionalità di gestione degli accessi con privilegi e il supporto di nuove piattaforme.

Questa versione di Microsoft Identity Manager fornisce nuove funzionalità, ad esempio la gestione delle identità con privilegi e il supporto della gestione dei certificati per l'accesso all'API REST. Nella gestione dei certificati attualmente è stato aggiunto il supporto per le topologie con più foreste, un’app di Windows store per la gestione delle smart card virtuali e del ciclo di vita dei certificati, eventi aggiornati e funzionalità di risoluzione dei problemi. Gli scenari self-service includono lo sblocco degli account e il controllo dell’autenticazione a più fattori per la reimpostazione della password.

## <a name="hybrid-experience"></a>Esperienza ibrida
Microsoft Identity Manager 2016 collabora con Azure per offrire all'utente controllo completo sull'ambiente. Il servizio di creazione di report ibridi in Azure aggrega i dati locali e nel cloud in un'unica posizione. Il portale di reimpostazione della password self-service supporta anche Azure Multi-Factor Authentication (MFA).

## <a name="privileged-identity-management"></a>Gestione identità con privilegi
Gestione identità con privilegi controlla e gestisce l'accesso amministrativo fornendo alle risorse sensibili accesso temporaneo basato sulle attività. In questo modo è possibile concedere agli utenti solo le autorizzazioni necessarie, riducendo le possibilità che un utente malintenzionato ottenga accesso amministrativo completo. Inoltre, Gestione identità con privilegi estrae e isola gli account amministrativi dalle foreste Active Directory esistenti.

MIM supporta una soluzione di Privileged Identity Management locale per la gestione di Active Directory. Per iniziare, [usare Privileged Access Management](./pam/privileged-identity-management-for-active-directory-domain-services.md).

## <a name="related-topics"></a>Argomenti correlati
Microsoft Identity Manager è ancora strettamente correlato al suo predecessore, Forefront Identity Manager. Se si usa ancora FIM o si vogliono consultare altri documenti, vedere [FIM 2010 R2 Documentation Roadmap](https://technet.microsoft.com/library/jj133885.aspx) (Guida di orientamento sulla documentazione di FIM 2010 R2).

