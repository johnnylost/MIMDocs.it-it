---
title: Cosa sono i report ibridi in Azure AD? | Microsoft Docs
description: "I report delle attività di controllo ibridi in Azure Active Directory consentono di visualizzare gli eventi controllati sia nel cloud che in locale."
keywords: 
author: davidste
ms.author: davidste
manager: bhu
ms.date: 02/20/2018
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 7320f014-8b60-4866-92de-cfbd3e6edc48
ms.suite: ems
ms.openlocfilehash: eb9725df484fb5ac2ee44bd9a0423bdb4fbe7e86
ms.sourcegitcommit: b4a39928c5fa1d7718046563c0809bcbf11d833d
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 02/20/2018
---
# <a name="hybrid-identity-management-audit-reporting-in-azure-active-directory"></a>Creazione di report di controllo di gestione delle identità ibridi in Azure Active Directory
Con i report delle attività di controllo di Azure Active Directory (Azure AD) è possibile monitorare le attività di gestione delle identità sia in locale che nel cloud. La gestione di tutti i dati sulle identità e l'accesso in un singolo report consente di risparmiare tempo e ridurre i costi complessivi.

## <a name="what-is-azure-active-directory-hybrid-reporting"></a>Che cos'è il servizio di creazione di report ibridi di Azure Active Directory
I report di controllo ibridi consentono ai professionisti IT di soddisfare alcune esigenze comuni relative alla gestione delle identità, ad esempio:

* **Raccolta delle le attività di gestione delle identità da diversi sistemi**. i report ibridi visualizzano le attività di gestione delle identità di Azure AD e Identity Manager.

* **Esportazione dei dati dei report e creazione di report personalizzati**. Oltre a visualizzare i report nel portale di Azure, è possibile esportare i dati per generare visualizzazioni personalizzate.

* **Riduzione dei costi di infrastruttura per il sistema di creazione di report**. La creazione di report ibridi nel cloud significa avere la possibilità di evitare i costi associati all'infrastruttura di data warehousing in locale.

## <a name="how-does-it-work"></a>Come funziona BitLocker?

Per raccogliere i dati in locale, è innanzitutto necessario installare un agente per la creazione di report nel server Identity Manager 2016. [Scaricare gli agenti per la creazione di report ibridi di Microsoft Identity Manager](https://www.microsoft.com/download/details.aspx?id=55112).

Il processo per la creazione di report ibridi è il seguente:
1. Dopo aver installato l'agente per la creazione di report, i dati delle attività di Identity Manager vengono inviati al registro eventi di Windows.
2. L'agente per la creazione di report elabora gli eventi delta ogni 10 minuti o al riavvio del servizio Registro eventi di Windows. L'agente carica quindi gli eventi nel portale di Azure.
3. Il portale di Azure elabora i dati ricevuti entro un'ora dalla ricezione.
4. In Azure i dati dell'attività rimangono memorizzati per un mese.
5. Il portale di Azure recupera i dati dei report di controllo e li visualizza nella finestra dei report di controllo di Azure.

## <a name="next-steps"></a>Passaggi successivi
Sono disponibili altre informazioni su:
- [Utilizzare il servizio per la creazione report ibridi in Identity Manager](working-with-identity-manager-hybrid-reporting.md)
- [Report delle attività di controllo nel portale di Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-audit-logs)
- [Reporting retention policies](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-retention) (Criteri di conservazione dei report)
- [Introduzione all'integrazione dei log di Microsoft Azure](https://docs.microsoft.com/azure/security/security-azure-log-integration-overview)
- [Introduzione all'API di creazione report di Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-getting-started)
