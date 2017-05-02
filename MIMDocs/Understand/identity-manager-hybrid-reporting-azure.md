---
title: "Che cos&quot;è il servizio di creazione di report ibridi | Documentazione Microsoft"
description: "I report attività di controllo ibridi in Azure Active Directory consentono di visualizzare gli eventi controllati del cloud e locali."
keywords: 
author: kgremban
ms.author: fimguy
manager: femila
ms.date: 04/27/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 7320f014-8b60-4866-92de-cfbd3e6edc48
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 3144ee195675df5dc120896cc801a7124ee12214
ms.openlocfilehash: 8ca0af93f2d72ccf2e314b20d13323b631eb02bc
ms.lasthandoff: 04/27/2017


---

# <a name="hybrid-identity-management-audit-reports-in-azure-active-directory"></a>Report di controllo di gestione delle identità ibridi in Azure Active Directory
Con i report attività di controllo di Azure Active Directory (AD) è possibile visualizzare un singolo report per monitorare le attività di gestione delle identità locali o nel cloud. Questa funzionalità consente di gestire tutti i dati di identità e accesso in un'unica posizione, risparmiando tempo e riducendo i costi totali.

## <a name="what-is-azure-active-directory-hybrid-reporting"></a>Che cos'è il servizio di creazione di report ibridi di Azure Active Directory
I report di controllo ibridi consentono ai professionisti IT di risolvere alcuni problemi comuni relativi alla gestione delle identità.

1. **Consente di raccogliere le attività di gestione delle identità da diversi sistemi:** i report ibridi visualizzano le attività di gestione delle identità di Azure AD e Identity Manager.

2. **Consente di esportare i dati dei report e creare report personalizzati:** oltre a visualizzare i report nel portale di Azure, è anche possibile esportare i dati per generare visualizzazioni personalizzate.

3. **Consente di ridurre i costi dell'infrastruttura del sistema di creazione di report:** i report ibridi nel cloud permettono di eliminare l'infrastruttura datawarehouse locale per la creazione dei report.

## <a name="how-does-it-work"></a>Come funziona BitLocker?

Per raccogliere i dati in locale, è innanzitutto necessario installare un agente per la creazione di report nel server Identity Manager 2016. È possibile scaricare l'agente per la creazione di report dalla pagina di download Microsoft disponibile [qui](https://www.microsoft.com/en-us/download/details.aspx?id=####/).

Il processo di creazione di report ibridi prevede i passaggi seguenti:
1. Dopo aver installato l'agente per la creazione di report, i dati dell'attività di Identity Manager vengono inviati al Registro eventi di Windows.
2. L'agente per la creazione di report elabora gli eventi nel Registro eventi di Windows e li carica nel portale di Azure.
3. In Azure i dati dell'attività rimangono memorizzati per un mese.
4. Quando si richiede un report, gli eventi dell'attività vengono analizzati e filtrati per i report richiesti.
5. Il portale di Azure recupera i dati dei report di controllo e ne esegue il rendering nel report di attività.

## <a name="see-also"></a>Vedere anche
- Altre informazioni su [Working with Identity Manager Hybrid Reporting](/microsoft-identity-manager/deploy-use/working-with-identity-manager-hybrid-reporting) (Uso del servizio di creazione report ibridi di Identity Manager)
- Altre informazioni su [Report delle attività di controllo nel portale di Azure Active Directory](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-reporting-activity-audit-logs)

