---
title: Lingue supportate di Microsoft Identity Manager 2016 SP1 | Microsoft Docs
description: Elenco di lingue supportate da Microsoft Identity Manager 2016 SP1.
keywords: ''
author: fimguy
ms.author: davidste
manager: mbaldwin
ms.date: 05/23/2018
ms.topic: get-started-article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 50345fda-56d7-4b6e-a861-f49ff90a8376
ms.openlocfilehash: 834343243dfeefa8d1874414fa369751288fd64d
ms.sourcegitcommit: 35f2989dc007336422c58a6a94e304fa84d1bcb6
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36288871"
---
# <a name="supported-languages"></a>Lingue supportate

Questo articolo illustra le lingue supportate e il mapping degli aggiornamenti da Microsoft Identity Manager 2016 SP1 versione 4.5.x o successiva.

## <a name="mim-service-and-portal-and-add-ins-and-extensions-language-pack"></a>MIM Add-ins and extensions Language Pack per il servizio e il portale 

Il Language Pack per il servizio e il portale di Microsoft MIM supporta le 33 lingue seguenti.  

> [!NOTE]
> Nella versione [4.4.1642.0](https://support.microsoft.com/en-us/help/4021562/hotfix-rollup-package-build-4-4-1642-0-is-available-for-microsoft) una chiave del Registro di sistema denominata "OverrideDefaultUILocale" è stata aggiunta a Componenti aggiuntivi ed estensioni MIM. Il Language Pack proverà a eseguire il mapping di tutte le lingue simili a quella supportata. Se ad esempio la lingua di visualizzazione di Windows è ES-CL (Spagnolo - Cile) o ES-**, proverà a eseguire il mapping a ES-ES (Spagnolo - Spagna).

> [!IMPORTANT]
> Il testo nel componente aggiuntivo SSPR e nel portale verrà localizzato, a differenza delle domande che richiedono operazioni aggiuntive. Sarà necessario creare i flussi di lavoro AuthN (e i set e le regole di criteri di gestione associati per specificarli come destinazione) alle domande di destinazione in ogni lingua per la posizione di destinazione.

|       Language        | FIM (4.3.x.x)/MIM (4.4.xx) | MIM (4.5.x.x) |
|-----------------------|--------------------------|--------------|
|       Bulgaro       |          bg-BG           |      bg      |
| Cinese (semplificato)  |          zh-CN           |   zh-hans    |
|   Cinese (Taiwan)    |          zh-TW           |   zh-hant    |
|       Croato        |          hr-HR           |      hr      |
|         Ceco         |          cs-CZ           |      cs      |
|        Danese         |          da-DK           |      da      |
|         Olandese         |          nl-NL           |      nl      |
|       Estone        |          et-EE           |      et      |
|        Francese         |          fr-FR           |      fr      |
|        Finlandese        |          fi-FI           |      fi      |
|        Tedesco         |          de-DE           |      de      |
|         Greco         |          el-GR           |      el      |
|         Hindi         |          hi-IN           |      hi      |
|       Ungherese       |          hu-HU           |      hu      |
|        Italiano        |          it-IT           |      it      |
|       Giapponese        |          ja-JP           |      ja      |
|        Coreano         |          ko-KR           |      ko      |
|      Lituano       |          lt-LT           |      lt      |
|        Lettone        |          lv-LV           |      lv      |
|       Norvegese       |          nb-NO           |    nb-NO     |
|        Polacco         |          pl-PL           |      pl      |
| Portoghese (Portogallo) |          pt-PT           |      pt      |
|  Portoghese (Brasile)  |          pt-BR           |    pt-BR     |
|        Russo        |          ru-RU           |      ru      |
|       Rumeno        |          ro-RO           |      ro      |
|        Spagnolo        |          es-ES           |      es      |
|        Slovacco         |          sk-SK           |      sk      |
|        Svedese        |          sv-SE           |      sv      |
|       Sloveno       |          sl-SI           |      sl      |
|   Serbo - Serbia    |  sr-latn-CS (deprecato)  |  sr-Latn-RS  |
|         Thailandese          |          th-TH           |      th      |
|        Turco        |          tr-TR           |      tr      |
|       Ucraino       |          uk-UA           |      uk      |

## <a name="certificate-management"></a>Gestione dei certificati 
Gestione certificati Microsoft supporta le 9 lingue seguenti. 

|Language|FIM (4.3.x.x)/MIM (4.4.xx)|Nuova versione di MIM (4.5.x.x)
|-----|-----|-----|-----|
|Cinese (semplificato)|zh-CN|zh-hans|
|Cinese (Taiwan)|zh-TW|zh-hant|
|Olandese|nl-NL|nl|
|Francese|fr-FR|fr|
|Tedesco|de-DE|de|
|Italiano|it-IT|it|
|Giapponese|ja-JP|ja|
|Portoghese (Portogallo)|pt-PT|pt-PT|
|Spagnolo|es-ES|es|

## <a name="certificate-management-modern-application"></a>Certificate Manager Modern Application  
Microsoft Certificate Manager Modern Application supporta le 33 lingue seguenti. 

|Language | [1.0.225.104](https://www.microsoft.com/en-us/download/details.aspx?id=54954) | |
|-----|-----|-----|-----|
|Olandese|nl-NL|nl|
|Cinese (semplificato)|zh-CN|zh-hans|
|Cinese (Taiwan)|zh-TW|zh-hant|
|Ceco|cs-CZ|cs|
|Danese|da-DK|da|
|Francese|fr-FR|fr|
|Finlandese|fi-FI|fi|
|Tedesco|de-DE|de|
|Greco|el-GR|el|
|Ungherese|hu-HU|hu|
|Italiano|it-IT|it|
|Giapponese|ja-JP|ja|
|Coreano|ko-KR|ko|
|Norvegese|nb-NO|nb-NO|
|Polacco|pl-PL|pl|
|Portoghese (Portogallo)|pt-PT|pt|
|Portoghese (Brasile)|pt-BR|pt-BR|
|Russo|ru-RU|ru|
|Rumeno|ro-RO|ro|
|Spagnolo|es-ES|es|
|Svedese|sv-SE|sv|
|Turco|tr-TR|tr|

## <a name="next-steps"></a>Passaggi successivi

- [Prima distribuzione](microsoft-identity-manager-deploy.md)
- [Cronologia delle versioni](/reference/version-history.md)