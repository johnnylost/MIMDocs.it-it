---
title: Distribuire e configurare PAM | Documentazione Microsoft
description: Guida di orientamento per l&quot;installazione di MIM e la configurazione di quest&quot;ultimo per Privileged Access Management.
keywords: 
author: kgremban
ms.author: kgremban
manager: femila
ms.date: 07/15/2016
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: c4ca5b58-ad0c-48af-a9eb-b71b22d0c67c
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 1f545bfb2da0f65c335e37fb9de9c9522bf57f25
ms.openlocfilehash: a081b49ca8d0de7ce7d5f7385e5a652b09b722c3


---

# <a name="configure-the-mim-environment-for-privileged-access-management"></a>Configurazione dell'ambiente MIM per Privileged Access Management
Per la configurazione dell'ambiente per l'accesso tra foreste, l'installazione e la configurazione di Active Directory e Microsoft Identity Manager e la dimostrazione di una richiesta di accesso JIT è necessario completare sette passaggi.

Questi passaggi sono strutturati per iniziare da zero e creare un ambiente di test. Se si applica PAM a un ambiente esistente, è possibile usare i controller di dominio o gli account utente personali anziché crearne di nuovi affinché corrispondano agli esempi.

1.  Preparare il server *CORPDC* come controller di dominio e *CORPWKSTN* come workstation membro.

2.  Preparare il server *PRIVDC* come controller di dominio.

3.  Preparare il server *PAMSRV* nella foresta *PRIV* .

4.  Installare i componenti MIM in *PAMSRV* e i cmdlet in una workstation membro della foresta *CONTOSO* , e prepararli per Privileged Access Management.

5.  Stabilire una relazione di trust tra le foreste *PRIV* e *CONTOSO* .

6.  Preparazione di gruppi di sicurezza con privilegi con accesso a risorse protette e account membri per Gestione accessi con privilegi JIT.

7.  Illustrare la richiesta, la ricezione e l'utilizzo di accesso con privilegi elevati a una risorsa protetta.

>[!div class="step-by-step"]
[Inizio »](step-1-prepare-corp-domain.md)



<!--HONumber=Nov16_HO2-->


