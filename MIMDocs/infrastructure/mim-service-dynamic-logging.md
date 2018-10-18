---
title: Registrazione dinamica del servizio MIM | Microsoft Docs
description: Abilitare la registrazione dinamica del servizio MIM senza dover riavviare il servizio di gestione
keywords: ''
author: fimguy
ms.author: davidste
manager: mbaldwin
ms.date: 06/25/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: ''
ms.openlocfilehash: ff82b2fce31abe417509347ce7b477dd1b4056f2
ms.sourcegitcommit: ace4d997c599215e46566386a1a3d335e991d821
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/15/2018
ms.locfileid: "49332338"
---
# <a name="mim-sp1-4414360--service-dynamic-logging"></a>Registrazione dinamica del servizio MIM SP1 (4.4.1436.0)
In 4.4.1436.0 è stata introdotta una nuova funzionalità di registrazione. Ciò consente all'amministratore e ai tecnici di supporto di attivare la registrazione senza dover riavviare il servizio di gestione.

Dopo l'installazione verrà visualizzata la riga seguente nell'oggetto Microsoft.ResourceManagement.Service.exe.config chiamato

*   Riga 6: ``<section name="dynamicLogging" type="Microsoft.ResourceManagement.Utilities.DynamicLoggingSection, Microsoft.ResourceManagement.Service" />``
*   Riga 8:  ``<dynamicLogging mode="true" loggingLevel="Verbose" />``
*   Riga 266 ``</system.diagnostics> ``

![Le sezioni evidenziate indicano le nuove voci di registrazione dinamica](media/mim-service-dynamic-logging/screen01.png)

I livelli di registrazione dinamica sono disponibili [qui](https://msdn.microsoft.com/library/ms733025(v=vs.110).aspx#Anchor_3)

- Critico = il servizio di livello predefinito scrive solo gli eventi critici
- Aggiornare la riga 8 (dynamicLogging mode="true" loggingLevel="Critical") con il valore di registrazione preferito

La configurazione della registrazione dinamica è alla riga 266: Microsoft.ResourceManagement.Service.exe.config

![Le sezioni evidenziate contengono le righe con le varie aree di registrazione disponibili](media/mim-service-dynamic-logging/screen02.png)

Per impostazione predefinita il percorso di registrazione sarà **C:\Programmi\Microsoft Forefront Identity Manager\2010\Service. Per generare il log dinamico, l'account del servizio FIM dovrà essere autorizzato a scrivere in questo percorso.

![Percorso della cartella dei log](media/mim-service-dynamic-logging/screen03.png)

> [!NOTE]
>  In caso di errori imprevisti, ad esempio errori di sintassi nel file di configurazione Microsoft.ResourceManagement.Service.exe.config o di altro tipo, verrà scritto un messaggio di errore corrispondente nel file Microsoft.ResourceManagement.Service.exe_Emergency.log nel seguente percorso %TMP% o %TEMP% o %USERPROFILE% (il primo esistente).  
> 1. "%TMP%\Microsoft.ResourceManagement.Service.exe_Emergency.log"
> 2. "%TEMP%\Microsoft.ResourceManagement.Service.exe_Emergency.log"
> 3. "% USERPROFILE %\Microsoft.ResourceManagement.Service.exe_Emergency.log"

Per visualizzare la traccia, è possibile usare lo strumento [Service Trace Viewer](https://msdn.microsoft.com//library/aa751795(v=vs.110).aspx)

 ![Schermata di Service Trace Viewer](media/mim-service-dynamic-logging/screen04.png)

# <a name="updates-build-45xx-or-greater"></a>Aggiornamenti: build 4.5.x.x o versione successiva

Nella build 4.5.x.x è stata modificata la funzionalità di registrazione per specificare il livello di registrazione predefinito **"Avviso"**. Il servizio scrive i messaggi in due file (gli indici "00" e "01" vengono aggiunti prima dell'estensione). I file sono disponibili nella directory "C:\Programmi\Microsoft Forefront Identity Manager\2010\Service". Quando il file supera le dimensioni massime, il servizio inizia a scrivere in un altro file. Se esiste un altro file, verrà sovrascritto. Le dimensioni massime predefinite di questo file sono pari a 1 GB. Per modificare le dimensioni massime predefinite, è necessario aggiungere il parametro **"maxOutputFileSizeKB"** con valore di dimensione massima del file in KB nel listener (vedere l'esempio riportato di seguito) e riavviare il servizio MIM. Quando viene avviato, il servizio aggiunge i log nel file più recente (in caso di superamento dei limiti di spazio viene sovrascritto il file meno recente). 

> [!NOTE] Dato che il servizio controlla le dimensioni del file prima di scrivere il messaggio, le dimensioni del file potrebbero essere maggiori delle dimensioni massime per un messaggio. Per impostazione predefinita le dimensioni dei log possono essere di circa 6 GB (tre listener con due file di dimensioni di 1 GB).

> [!NOTE] L'account del servizio deve avere le autorizzazioni per scrivere nella directory "C:\Programmi\Microsoft Forefront Identity Manager\2010\Service". Se l'account del servizio non ha questi diritti, i file non verranno creati.

Esempio di come impostare le dimensioni massime del file su 200 MB (200 * 1024 KB) per i file svclog e su 100 MB *(100 * 1024 KB) per i file txt

`<add initializeData="Microsoft.ResourceManagement.Service_tracelog.svclog" type="Microsoft.IdentityManagement.CircularTraceListener.CircularXmlTraceListener, Microsoft.IdentityManagement.CircularTraceListener, PublicKeyToken=31bf3856ad364e35" name="ServiceModelTraceListener" traceOutputOptions="LogicalOperationStack, DateTime, Timestamp, ProcessId, ThreadId, Callstack" maxOutputFileSizeKB="204800">`
