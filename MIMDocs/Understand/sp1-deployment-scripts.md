---
title: Script di distribuzione di MIM 2016 SP1 per PAM
description: "Preparare il dominio CORP con identità nuove o esistenti da gestire con Privileged Identity Manager tramite gli script"
keywords: 
author: barclayn
manager: MBaldwin
ms.date: 09/26/2016
ms.topic: article
ms.prod: microsoft-identity-manager
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: 
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: c7c5266f3d1c51e933855031f4128cbcb967d6e2
ms.openlocfilehash: 43a176ed2f1375eb98851064c460515ec09de132


---

# Script di distribuzione di MIM 2016 SP1 per PAM

In questo Service Pack è stato introdotto un set di script di distribuzione per semplificare la distribuzione di PAM. Questi script sono disponibili nell'area download. Prima di tentare di usare gli script è importante assicurarsi che i presupposti seguenti si applichino all'ambiente.

Presupposti importanti:
1. Il sistema operativo in tutti i computer è almeno Windows Server 2012 R2. Se si sta tentando di usare Windows Server 2016 Technical Preview 5, è necessario installare il controller di dominio PRIV con la compilazione TP5.
2. Il DNS deve essere configurato in modo che la risoluzione dei nomi tra i controller di dominio e i server del componente sia automatica.
3. L'installazione dei file binari deve essere disponibile localmente nei server designati per l'installazione di SQL, SharePoint e MIM.
4. L'ambiente dispone di tre macchine (fisiche o virtuale) dedicate in esecuzione in modo indipendente CORPDC, PRIVDC e PAMSERVER.
5. Per l'opzione di convalida, si presuppone che esista un computer client dedicato per eseguire questo passaggio.

>[!NOTE] Se si riscontrano problemi con l'esecuzione dello script può essere necessario esaminare i log. Tutti i log degli script vengono salvati in %AppData%\MIMPAMInstall. Comprimere la cartella in un file con estensione zip e inviarla tramite posta elettronica all'indirizzo mim2016@microsoft.com con i dettagli dell'operazione e l'errore.



<!--HONumber=Sep16_HO4-->


