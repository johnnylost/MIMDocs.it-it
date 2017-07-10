---
title: Modello a livelli dell'ambiente PAM | Documentazione Microsoft
description: "Informazioni sul modello a livelli che mantiene separate le diverse parti del sistema in base alla vulnerabilità al rischio."
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/15/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: c6e3cd02-1e32-4194-a8ed-3a0b3d022a43
ms.reviewer: mwahl
ms.suite: ems
ms.translationtype: Human Translation
ms.sourcegitcommit: bfc73723bdd3a49529522f78ac056939bb8025a3
ms.openlocfilehash: 4c3b43e50403890572e77773191a821cf247269c
ms.contentlocale: it-it
ms.lasthandoff: 07/10/2017


---

<a id="tier-model-for-partitioning-administrative-privileges" class="xliff"></a>
# Modello di livello per il partizionamento dei privilegi amministrativi

Nell'ambiente a rischio di oggi, il problema non è se un utente malintenzionato riuscirà ad accedere ai sistemi in uso, ma quando. Ciò significa che la sicurezza interna è importante quanto una solida difesa perimetrale. Questo articolo descrive un modello di sicurezza ideato per proteggere dall'elevazione dei privilegi isolando le attività con privilegi elevati dalle aree ad alto rischio. Questo modello garantisce un'esperienza utente soddisfacente rimanendo comunque conforme alle procedure consigliate e ai principi di sicurezza.

<a id="elevation-of-privilege-in-active-directory-forests" class="xliff"></a>
## Elevazione dei privilegi nelle foreste di Active Directory

Gli account utente, di servizi o di applicazioni ai quali vengono concessi privilegi amministrativi completi permanenti per foreste di Windows Server Active Directory (AD) introducono una quantità significativa di rischio alla missione e alle attività dell'organizzazione. Questi account sono spesso presi di mira da utenti malintenzionati perché se sono compromessi, l'autore dell'attacco dispone del privilegio di connettersi ad altri server o applicazioni nel dominio.

Il modello di livello crea divisioni tra gli amministratori in base alle risorse che gestiscono. Gli amministratori con controllo sulle workstation degli utenti sono separati da quelli che controllano le applicazioni o gestiscono le identità dell'organizzazione. Informazioni su questo modello sono disponibili in [Securing privileged access reference material](http://aka.ms/tiermodel) (Materiale di riferimento sulla sicurezza dell'accesso con privilegi).

<a id="restricting-credential-exposure-with-logon-restrictions" class="xliff"></a>
## Limitare l'esposizione delle credenziali con le restrizioni di accesso

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

L'articolo successivo, [Planning a bastion environment](planning-bastion-environment.md) (Pianificazione di un ambiente bastion), descrive come aggiungere una foresta amministrativa dedicata per Microsoft Identity Manager per stabilire gli account amministrativi.

