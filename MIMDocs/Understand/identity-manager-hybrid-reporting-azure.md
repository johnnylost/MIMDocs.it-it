---
# required metadata

title: Report ibridi di gestione delle identità | Microsoft Identity Manager
description: Il servizio di creazione report ibridi di Azure Active Directory consente di creare report personalizzati che includono gli eventi cloud e locali.
keywords:
author: kgremban
manager: stevenpo
ms.date: 05/13/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 7320f014-8b60-4866-92de-cfbd3e6edc48

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Report ibridi di gestione delle identità in Azure
Con Azure Active Directory (AD) è possibile creare un singolo report per monitorare le attività di gestione delle identità in locale o nel cloud. Questa funzionalità consente di gestire tutti i dati di identità e accesso in un'unica posizione, risparmiando tempo e riducendo i costi totali.

## Che cosa sono i report ibridi di Azure AD?
I report ibridi consentono ai professionisti IT di risolvere alcuni problemi comuni relativi alla gestione delle identità.

1. **Consente di raccogliere le attività di gestione delle identità da diversi sistemi:** i report ibridi visualizzano le attività di gestione delle identità di Azure AD e Identity Manager.

2. **Consente di esportare i dati dei report e creare report personalizzati:** oltre a visualizzare i report nel portale di Azure, è anche possibile esportare i dati per generare visualizzazioni personalizzate.

3. **Consente di ridurre i costi dell'infrastruttura del sistema di creazione di report:** i report ibridi nel cloud permettono di eliminare l'infrastruttura datawarehouse locale per la creazione dei report.

## Come funziona BitLocker?

Per raccogliere i dati in locale, è innanzitutto necessario installare un agente per la creazione di report sul server Identity Manager. L'agente per la creazione di report può essere scaricato dalla pagina di configurazione della directory nel [portale di Azure classico](https://manage.windowsazure.com/).

Il processo di creazione di report ibridi prevede i passaggi seguenti:
1. Dopo aver installato l'agente per la creazione di report, i dati dell'attività di Identity Manager vengono inviati al Registro eventi di Windows.
2. L'agente per la creazione di report elabora gli eventi nel Registro eventi di Windows e li carica in Azure.
3. In Azure i dati dell'attività rimangono memorizzati per un mese.
4. Quando si richiede un report, gli eventi dell'attività vengono analizzati e filtrati per i report richiesti.
5. Il portale di Azure classico recupera i dati dei report e ne esegue il rendering come report di attività.

## Vedere anche
- Altre informazioni su [Working with Identity Manager Hybrid Reporting](/microsoft-identity-manager/deploy-use/working-with-identity-manager-hybrid-reporting) (Uso del servizio di creazione report ibridi di Identity Manager)


<!--HONumber=May16_HO3-->


