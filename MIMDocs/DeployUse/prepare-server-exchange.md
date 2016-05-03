---
# required metadata

title: Configurare un server di gestione delle identità&#58; Exchange | Microsoft Identity Manager
description: Come passaggio facoltativo, distribuire Exchange Server per abilitare l'invio della posta elettronica e la creazione di cassette postali in MIM 2016. 
keywords:
author: kgremban
manager: stevenpo
ms.date: 04/28/2016
ms.topic: get-started-article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 34a8c16e-3bed-4e16-939b-b9fe17dd834b

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Configurare un server di gestione delle identità: Exchange

>[!div class="step-by-step"]
[« SharePoint](prepare-server-sharepoint.md)
[Servizio di sincronizzazione MIM »](install-mim-sync.md)

> [!NOTE]
> In tutti gli esempi riportati di seguito **mimservername** rappresenta il nome del controller di dominio, **contoso** rappresenta il nome di dominio e **Pass@word1** rappresenta una password di esempio.

## Distribuire Microsoft Exchange Server
Se si desidera configurare MIM per inviare e ricevere posta elettronica o eseguire il provisioning delle cassette postali, è necessario disporre di Exchange presente nell'ambiente. Se Exchange non è stato distribuito, è possibile installare una versione di valutazione per scopi di valutazione.

1. Scaricare e installare Microsoft Office 2010 Filter Pack - versione 2.0 e Microsoft Office 2010 Filter Pack - versione 2.0 SP1

    - [MS Office10 FP2.0](http://www.microsoft.com/en-us/download/details.aspx?id=17062)

    - [MS Office10 FP2.0 SP1](http://www.microsoft.com/en-us/download/details.aspx?id=26604)

2. Scaricare e installare [Microsoft Unified Communications Managed API 4.0, Core Runtime 64-bit](http://www.microsoft.com/en-us/download/details.aspx?id=34992)

3. Scaricare e installare la [versione di valutazione di 180 giorni di MS Exchange Server 2013](http://www.microsoft.com/en-us/evalcenter/evaluate-exchange-server-2013)

>[!div class="step-by-step"]  
[« SharePoint](prepare-server-sharepoint.md)
[Servizio di sincronizzazione MIM »](install-mim-sync.md)


<!--HONumber=Apr16_HO2-->


