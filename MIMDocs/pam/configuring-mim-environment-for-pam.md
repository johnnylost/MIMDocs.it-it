---
title: Distribuire e configurare PAM | Microsoft Identity Manager
description: Guida di orientamento per l'installazione di MIM e la configurazione di quest'ultimo per Privileged Access Management.
keywords: 
author: kgremban
manager: femila
ms.date: 07/15/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: c4ca5b58-ad0c-48af-a9eb-b71b22d0c67c
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: ae4c40c73dd9d5860f42e00765a7e34e8ca397a9
ms.openlocfilehash: 4b4953089cb676baae97988f380debbfefcd1083


---

# Configurazione dell'ambiente MIM per Privileged Access Management
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



<!--HONumber=Jul16_HO3-->

