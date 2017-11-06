---
title: Script di distribuzione di MIM 2016 SP1 per PAM
description: "Questa pagina fa parte della serie di articoli sulla configurazione di Privileged Identity Manager tramite script. È incluso un elenco dei presupposti sull'ambiente."
keywords: 
author: barclayn
ms.author: barclayn
manager: MBaldwin
ms.date: 10/17/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: 
ms.suite: ems
ms.openlocfilehash: 77a222c0a36f4e244a5114eddfc0edadb168d1cd
ms.sourcegitcommit: 06add1a636720f74bc0c0f25b4100b19f1bd31da
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/17/2017
---
# <a name="mim2016-sp1-pam-deployment-scripts"></a>Script di distribuzione di MIM 2016 SP1 per PAM

In questo Service Pack è stato introdotto un set di script di distribuzione per semplificare la distribuzione di PAM. Questi script sono disponibili nell'area download. Prima di provare a usare gli script è importante assicurarsi che siano soddisfatti i requisiti seguenti:

1. Il sistema operativo in tutti i server deve essere almeno Windows Server 2012 R2.
2. Il DNS deve essere configurato in modo da consentire la risoluzione dei nomi tra i controller di dominio e i server del componente.
3. L'installazione dei file binari deve essere disponibile localmente nei server designati per l'installazione di SQL, SharePoint e MIM.
4. L'ambiente dispone di tre macchine (fisiche o virtuale) dedicate in esecuzione in modo indipendente CORPDC, PRIVDC e PAMSERVER.
5. Per l'opzione di convalida è necessario avere una workstation dedicata.

>[!NOTE]
>Se si riscontrano problemi con l'esecuzione dello script può essere necessario esaminare i log. Tutti i log degli script vengono salvati in %AppData%\MIMPAMInstall. Comprimere la cartella in un file con estensione zip e includere quest'ultimo, con i dettagli dell'operazione e l'errore, nel caso di supporto.

Adesso si può cominciare con gli script di distribuzione per PAM. Iniziare da [Configurare PAM tramite script](./pam/sp1-pam-configure-using-scripts.md).
