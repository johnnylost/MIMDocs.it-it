---
title: Microsoft Identity Manager 2016 | Documentazione Microsoft
description: MIM include le funzionalità di gestione dell'accesso di FIM 2010 e consente di gestire gli utenti, le credenziali, i criteri e l'accesso all'interno dell'organizzazione.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 05/02/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ccdd8a9f-02da-440a-81a8-354800dcd2a8
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: abbd661fa1bef13ad92b916f8485934390905bf4
ms.sourcegitcommit: 7de35aaca3a21192e4696fdfd57d4dac2a7b9f90
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/16/2018
ms.locfileid: "49358313"
---
# <a name="microsoft-identity-manager-2016"></a>Microsoft Identity Manager 2016

Microsoft Identity Manager (MIM) 2016 è compilato sulla base delle funzionalità di gestione delle identità e degli accessi di [FIM 2010 R2](https://technet.microsoft.com/library/jj133885.aspx). Come il suo predecessore, MIM consente di gestire utenti, credenziali, criteri e accessi all'interno dell'organizzazione.  In aggiunta, MIM 2016 fornisce un'esperienza ibrida, funzionalità di gestione degli accessi con privilegi e il supporto di nuove piattaforme.

Oltre alle funzionalità di gestione delle identità esistenti incluse in [FIM](https://technet.microsoft.com/library/jj133868), MIM 2016 offre funzionalità nuove e migliorate, ad esempio:

- Gestione identità con privilegi
- Nuova funzionalità in gestione dei certificati
  - [Riferimento all'API REST per la gestione dei certificati](./reference/certificate-management-rest-api-reference.md)
  - Supporto per le topologie a più foreste.
  - [App Windows per smart card virtuali](working-with-mim-certificate-manager.md)
  - Funzionalità aggiornate per eventi e risoluzione dei problemi. 
- Gli [scenari self-service](working-with-self-service-password-reset.md) ora includono lo sblocco degli account e l'attività di controllo Azure MFA (Multi-Factor Authentication, autenticazione a più fattori) per la reimpostazione della password.

## <a name="hybrid-experience"></a>Esperienza ibrida

Microsoft Identity Manager 2016 funziona in combinazione con [Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-whatis) per offrire all'utente il controllo completo sull'ambiente. Il servizio di creazione di report ibridi in Azure AD aggrega i dati locali e nel cloud in un'unica posizione. Il [portale di reimpostazione password self-service](working-with-self-service-password-reset.md) supporta anche l'autenticazione a più fattori di Azure (MFA).

## <a name="privileged-identity-management"></a>Gestione identità con privilegi

Gestione identità con privilegi controlla e gestisce l'accesso amministrativo fornendo alle risorse sensibili accesso temporaneo basato sulle attività. In questo modo è possibile concedere agli utenti solo le autorizzazioni necessarie, riducendo le possibilità che un utente malintenzionato ottenga accesso amministrativo completo. Inoltre, Gestione identità con privilegi estrae e isola gli account amministrativi dalle foreste Active Directory esistenti.

MIM supporta una soluzione di Privileged Identity Management locale per la gestione di Active Directory. Per iniziare, [usare Privileged Access Management](./pam/privileged-identity-management-for-active-directory-domain-services.md).

## <a name="related-topics"></a>Argomenti correlati

- Microsoft Identity Manager è ancora strettamente correlato al suo predecessore, Forefront Identity Manager. Se si usa ancora FIM o si vogliono consultare altri documenti, vedere [FIM 2010 R2 Documentation Roadmap](https://technet.microsoft.com/library/jj133885.aspx) (Guida di orientamento sulla documentazione di FIM 2010 R2).
- [Considerazioni relative alla topologia per la distribuzione di MIM](topology-considerations.md) Questo articolo presenta varie topologie di distribuzione che è possibile implementare.
- [Guida alla pianificazione della capacità](capacity-planning-guide.md) È possibile consultare questa guida con i relativi ambienti di test per comprendere l'ambito appropriato per la distribuzione.
