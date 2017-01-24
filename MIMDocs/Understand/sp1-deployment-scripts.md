---
title: Script di distribuzione di MIM 2016 SP1 per PAM
description: "Questa pagina fa parte della serie di articoli sulla configurazione di Privileged Identity Manager tramite script. È incluso un elenco dei presupposti sull&quot;ambiente."
keywords: 
author: barclayn
ms.author: barclayn
manager: MBaldwin
ms.date: 01/10/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: 
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: f08b0197341351bd5f33552f26b96132b1356239
ms.openlocfilehash: 10d06ae573e378797467ab1eb91e977d59b821d1


---

# <a name="mim2016-sp1-pam-deployment-scripts"></a>Script di distribuzione di MIM 2016 SP1 per PAM

In questo Service Pack è stato introdotto un set di script di distribuzione per semplificare la distribuzione di PAM. Questi script sono disponibili nell'area download. Prima di tentare di usare gli script è importante assicurarsi che i presupposti seguenti si applichino all'ambiente.

Presupposti importanti:
1. Il sistema operativo in tutti i computer è almeno Windows Server 2012 R2. Se si sta tentando di usare Windows Server 2016 Technical Preview 5, è necessario installare il controller di dominio PRIV con la compilazione TP5.
2. Il DNS deve essere configurato in modo che la risoluzione dei nomi tra i controller di dominio e i server del componente sia automatica.
3. L'installazione dei file binari deve essere disponibile localmente nei server designati per l'installazione di SQL, SharePoint e MIM.
4. L'ambiente dispone di tre macchine (fisiche o virtuale) dedicate in esecuzione in modo indipendente CORPDC, PRIVDC e PAMSERVER.
5. Per l'opzione di convalida, si presuppone che esista un computer client dedicato per eseguire questo passaggio.

>[!NOTE]
>Se si riscontrano problemi con l'esecuzione dello script può essere necessario esaminare i log. Tutti i log degli script vengono salvati in %AppData%\MIMPAMInstall. Comprimere la cartella in un file con estensione zip e inviarla tramite posta elettronica all'indirizzo mim2016@microsoft.com con i dettagli dell'operazione e l'errore.

Adesso si può cominciare con gli script di distribuzione per PAM. Iniziare da [Configurare PAM tramite script](/microsoft-identity-manager/pam/sp1-pam-configure-using-scripts).



<!--HONumber=Jan17_HO2-->


