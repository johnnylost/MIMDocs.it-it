---
title: Modello a livelli dell'ambiente PAM | Documentazione Microsoft
description: Informazioni sul modello a livelli che mantiene separate le diverse parti del sistema in base alla vulnerabilità al rischio.
keywords: ''
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 08/30/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: c6e3cd02-1e32-4194-a8ed-3a0b3d022a43
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 0d4ae72b897af3c6e737b412b7f8971b249ffa23
ms.sourcegitcommit: ace4d997c599215e46566386a1a3d335e991d821
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/15/2018
ms.locfileid: "49334245"
---
# <a name="tier-model-for-partitioning-administrative-privileges"></a>Modello di livello per il partizionamento dei privilegi amministrativi

Questo articolo descrive un modello di sicurezza ideato per proteggere dall'elevazione dei privilegi isolando le attività con privilegi elevati dalle aree ad alto rischio. Questo modello garantisce un'esperienza utente soddisfacente rimanendo comunque conforme alle procedure consigliate e ai principi di sicurezza.

## <a name="elevation-of-privilege-in-active-directory-forests"></a>Elevazione dei privilegi nelle foreste di Active Directory

Gli account utente, di servizi o di applicazioni ai quali vengono concessi privilegi amministrativi completi permanenti per foreste di Windows Server Active Directory (AD) introducono una quantità significativa di rischio alla missione e alle attività dell'organizzazione. Questi account sono spesso presi di mira da utenti malintenzionati perché se sono compromessi, l'autore dell'attacco dispone di autorizzazioni per connettersi ad altri server o applicazioni nel dominio.

Il modello di livello crea divisioni tra gli amministratori in base alle risorse che gestiscono. Gli amministratori con controllo sulle workstation degli utenti sono separati da quelli che controllano le applicazioni o gestiscono le identità dell'organizzazione. Informazioni su questo modello sono disponibili in [Securing privileged access reference material](http://aka.ms/tiermodel) (Materiale di riferimento sulla sicurezza dell'accesso con privilegi).

## <a name="restricting-credential-exposure-with-logon-restrictions"></a>Limitare l'esposizione delle credenziali con le restrizioni di accesso

Ridurre il rischio di furto delle credenziali per gli account amministrativi, rende necessarie procedure amministrative modificate per limitare l'esposizione agli attacchi. Come primo passaggio, le organizzazioni sono invitate a:

- Limitare il numero di host in cui vengono esposte le credenziali amministrative.
- Limitare i privilegi di ruolo al minimo necessario.
- Verificare che le attività amministrative non vengano eseguite su host utilizzati per le attività degli utenti standard (ad esempio, posta elettronica e browser Web).

Il passaggio successivo consiste nell’implementare le restrizioni di accesso e abilitare i processi e le procedure consigliate per rispettare i requisiti del modello di livello. In teoria, l'esposizione delle credenziali deve essere anche ridotta al privilegio minimo richiesto per il ruolo all'interno di ogni livello.

Le restrizioni di accesso devono essere applicate per garantire che gli account con privilegi elevati non abbiano accesso alle risorse meno sicure. Ad esempio:

- Il gruppo Domain admins (livello 0) non possa accedere al server aziendale (livello 1) e alle workstation degli utenti standard (livello 2).
- Gli amministratori del server (livello 1) non possano accedere alla workstation degli utenti standard (livello 2).

>[!NOTE]
> Gli amministratori del server non devono essere nel gruppo di amministratori di dominio. Al personale con la responsabilità di gestire i controller di dominio e i server aziendali è necessario assegnare account distinti.

Le restrizioni di accesso possono essere applicate con:

- Restrizioni dei diritti di accesso dei criteri di gruppo, tra cui:
    - Nega acceso al computer dalla rete
    - Nega accesso come processo batch
    - Nega accesso come servizio
    - Nega accesso in locale
    - Nega accesso tramite le impostazioni Desktop remoto  
- Criteri di autenticazione e silo, se si usa Windows Server 2012 o versione successiva
- Autenticazione selettiva, se l'account è in una foresta amministrativa dedicata

## <a name="next-steps"></a>Passaggi successivi

- L'articolo successivo, [Planning a bastion environment](planning-bastion-environment.md) (Pianificazione di un ambiente bastion), descrive come aggiungere una foresta amministrativa dedicata per Microsoft Identity Manager per stabilire gli account amministrativi.
- Le [Privileged Access Workstation](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/privileged-access-workstations) dispongono di un sistema operativo dedicato per le attività sensibili che devono essere protette dagli attacchi provenienti da Internet e dai vettori di minacce di qualsiasi tipo.