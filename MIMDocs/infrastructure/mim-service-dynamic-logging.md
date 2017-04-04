---
title: Registrazione dinamica del servizio MIM | Microsoft Docs
description: Abilitare la registrazione dinamica del servizio MIM senza dover riavviare il servizio di gestione
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 03/24/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 
translationtype: Human Translation
ms.sourcegitcommit: 90a0f144b7674bbfaf13138dfd926dbfc3c74f28
ms.openlocfilehash: ddd707210d5cd6b618709a477d40e7771d73cfa1
ms.lasthandoff: 03/27/2017



---
# <a name="mim-sp1-4414360--service-dynamic-logging"></a>Registrazione dinamica del servizio MIM SP1 (4.4.1436.0)
In 4.4.1436.0 è stata introdotta una nuova funzionalità di registrazione. Ciò consente all'amministratore e ai tecnici di supporto di attivare la registrazione senza dover riavviare il servizio di gestione.

Dopo l'installazione verrà visualizzata la riga seguente nell'oggetto Microsoft.ResourceManagement.Service.exe.config chiamato

*    Riga 6: ``<section name="dynamicLogging" type="Microsoft.ResourceManagement.Utilities.DynamicLoggingSection, Microsoft.ResourceManagement.Service" />``
*    Riga 8:  ``<dynamicLogging mode="true" loggingLevel="Verbose" />``
*    Riga 266 ``</system.diagnostics> ``

![Le sezioni evidenziate indicano le nuove voci di registrazione dinamica](/media/mim-service-dynamic-logging/screen01.png)

I livelli di registrazione dinamica sono disponibili [qui](https://msdn.microsoft.com/library/ms733025(v=vs.110).aspx#Anchor_3)

- Critico = il servizio di livello predefinito scrive solo gli eventi critici
- Aggiornare la riga 8 (dynamicLogging mode="true" loggingLevel="Critical") con il valore di registrazione preferito

La configurazione della registrazione dinamica è alla riga 266: Microsoft.ResourceManagement.Service.exe.config

![Le sezioni evidenziate contengono le righe con le varie aree di registrazione disponibili](/media/mim-service-dynamic-logging/screen02.png)

Per impostazione predefinita il percorso di registrazione sarà **C:\Program Files\Microsoft Forefront Identity Manager\2010\Service**. Per generare il log dinamico, l'account del servizio FIM dovrà essere autorizzato a scrivere in questo percorso.

![Percorso della cartella dei log](/media/mim-service-dynamic-logging/screen03.png)

 >[!NOTE]
 In caso di errori imprevisti, ad esempio errori di sintassi nel file di configurazione Microsoft.ResourceManagement.Service.exe.config o di altro tipo, verrà scritto un messaggio di errore corrispondente nel file Microsoft.ResourceManagement.Service.exe_Emergency.log nel seguente percorso %TMP% o %TEMP% o %USERPROFILE% (il primo esistente).  
1. "%TMP%\Microsoft.ResourceManagement.Service.exe_Emergency.log"
2. "%TEMP%\Microsoft.ResourceManagement.Service.exe_Emergency.log"
3. "% USERPROFILE %\Microsoft.ResourceManagement.Service.exe_Emergency.log"

Per visualizzare la traccia, è possibile usare lo strumento [Service Trace Viewer](https://msdn.microsoft.com//library/aa751795(v=vs.110).aspx)

 ![Schermata di Service Trace Viewer](/media/mim-service-dynamic-logging/screen04.png)

