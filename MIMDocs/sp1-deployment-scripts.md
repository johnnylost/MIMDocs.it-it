---
title: Script di distribuzione di MIM 2016 SP1 per PAM
description: Questa pagina fa parte della serie di articoli sulla configurazione di Privileged Identity Manager tramite script. È incluso un elenco dei presupposti sull'ambiente.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 10/17/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: ''
ms.suite: ems
ms.openlocfilehash: 139ff94ecc38de37ac8eb6536d1b4d2a42909536
ms.sourcegitcommit: 7de35aaca3a21192e4696fdfd57d4dac2a7b9f90
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/16/2018
ms.locfileid: "49358040"
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
