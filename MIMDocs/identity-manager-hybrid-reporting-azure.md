---
title: "Che cos'è il servizio di creazione di report ibridi | Documentazione Microsoft"
description: "I report attività di controllo ibridi in Azure Active Directory consentono di visualizzare gli eventi controllati del cloud e locali."
keywords: 
author: fimguy
ms.author: fimguy
manager: femila
ms.date: 04/28/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 7320f014-8b60-4866-92de-cfbd3e6edc48
ms.reviewer: mwahl
ms.suite: ems
ms.translationtype: Human Translation
ms.sourcegitcommit: 3797f5789bb4e48836eb21776dafd5a2e0e11613
ms.openlocfilehash: 678626e7c32659570de88d8178c16821cceaf7ee
ms.contentlocale: it-it
ms.lasthandoff: 07/10/2017


---

<a id="hybrid-identity-management-audit-reports-in-azure-active-directory---public-previewrefresh" class="xliff"></a>
# Report di controllo di gestione delle identità ibride in Azure Active Directory - Anteprima pubblica (Aggiornamento)
Con i report attività di controllo di Azure Active Directory (AD) è possibile visualizzare un singolo report per monitorare le attività di gestione delle identità locali o nel cloud. Questa funzionalità consente di gestire tutti i dati di identità e accesso in un'unica posizione, risparmiando tempo e riducendo i costi totali.

<a id="what-is-azure-active-directory-hybrid-reporting" class="xliff"></a>
## Che cos'è il servizio di creazione di report ibridi di Azure Active Directory
I report di controllo ibridi consentono ai professionisti IT di risolvere alcuni problemi comuni relativi alla gestione delle identità.

1. **Consente di raccogliere le attività di gestione delle identità da diversi sistemi:** i report ibridi visualizzano le attività di gestione delle identità di Azure AD e Identity Manager.

2. **Consente di esportare i dati dei report e creare report personalizzati:** oltre a visualizzare i report nel portale di Azure, è anche possibile esportare i dati per generare visualizzazioni personalizzate.

3. **Consente di ridurre i costi dell'infrastruttura del sistema di creazione di report:** i report ibridi nel cloud permettono di eliminare l'infrastruttura data warehouse locale per la creazione dei report.

<a id="how-does-it-work" class="xliff"></a>
## Come funziona BitLocker?

Per raccogliere i dati in locale, è innanzitutto necessario installare un agente per la creazione di report nel server Identity Manager 2016. È possibile scaricare l'agente per la creazione di report dalla pagina di download Microsoft disponibile [qui](https://www.microsoft.com/en-us/download/details.aspx?id=55112).

Il processo di creazione di report ibridi prevede i passaggi seguenti:
1. Dopo aver installato l'agente per la creazione di report, i dati dell'attività di Identity Manager vengono inviati al Registro eventi di Windows.
2. L'agente per la creazione di report elabora gli eventi delta ogni 10 minuti o al riavvio del servizio nel Registro eventi di Windows e li carica nel portale di Azure.
3. Il portale di Azure elabora i dati ricevuti entro 1 ora dalla ricezione
4. In Azure i dati dell'attività rimangono memorizzati per un mese.
5. Il portale di Azure recupera i dati dei report di controllo e ne esegue il rendering nel pannello dei report di controllo di Azure.

<a id="see-also" class="xliff"></a>
## Vedere anche
- Altre informazioni su [Working with Identity Manager Hybrid Reporting](working-with-identity-manager-hybrid-reporting.md) (Uso del servizio di creazione report ibridi di Identity Manager)
- Altre informazioni su [Report delle attività di controllo nel portale di Azure Active Directory](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-reporting-activity-audit-logs)
