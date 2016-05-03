---
# required metadata

title: Servizio di creazione report ibridi di Identity Manager in Azure | Microsoft Identity Manager
description: Il servizio di creazione report ibridi di Azure Active Directory consente di creare report personalizzati che includono gli eventi cloud e locali.
keywords:
author: kgremban
manager: stevenpo
ms.date: 04/28/2016
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

# Gestione identità ibride Reporting in Azure
Se si dispone di una sottoscrizione di Azure, è possibile ora creare facilmente un report degli eventi sia in locale e nel cloud. I report possono quindi essere visualizzati nel portale di Azure. Ancora meglio, i report vengono combinati con le attività di Azure Active Directory. Di Reporting di gestione identità ibride, portale di gestione di Azure AD può visualizzare report di attività di gestione di identità per le attività sia in locale e cloud. Questa funzionalità di creazione report offre quanto segue:

-   Un'esperienza unificata: report unificati per attività IAM, sia basata su cloud che locale

-   Eliminando la necessità dell'infrastruttura di data warehouse per la generazione di report in locale, si riducono i costi

-   I dati appartengono all'utente: i dati dei report possono essere facilmente esportati da Identity Manager locale o da Azure AD e possono usati per generare report con visualizzazione personalizzata

## Che cos'è Azure AD ibrido Reporting?
Con reporting ibrido, il portale di gestione di Azure AD visualizzare report attività di gestione unificata di identità. Si tratta indipendentemente dal fatto che a cui è stata effettuata l'attività, gestione di identità o Azure AD. Ad esempio, nel portale di gestione di Azure AD è possibile sapere chi ha effettuato la registrazione per la reimpostazione della password self-service (SSPR) nell'ultimo mese. In questo report verranno visualizzati gli utenti che hanno effettuato la registrazione a SSPR sia nel [riquadro di accesso alle applicazioni](https://myapps.microsoft.com) che in Identity Manager.

![Immagine dell'attività di reimpostazione della password di Azure](media/MIM-Hybrid-passwordreset.jpg)

## Perché usarla?
Reporting ibrido consente alcune comuni di gestione identità reporting sfide indirizzo ai professionisti IT.

1.  Attività Gestione identità report eseguiti in sistemi diversi: Ora è possibile visualizzare report di gestione delle identità da attività di Windows Azure e gestione di identità nel portale di gestione di Azure AD.

2.  Esportare dati di report e creare report personalizzati: Oltre ai report in Azure AD, con questa nuova funzionalità sono stati aggiunti gli eventi di windows che riflettono l'attività di gestione di identità. Questo rende molto più semplice rispetto a prima di integrare sistemi SIEM, visualizzare l'attività di gestione di identità e creare report personalizzati.

3.  Ridurre al minimo l'infrastruttura del sistema reporting dei costi: distribuzione di questa nuova funzionalità richiederà alcuni minuti. Sarà sufficiente consiste nell'installare un agente di report nel server di gestione di identità.

L'agente di reporting viene scaricato dal portale di gestione di Azure AD, nella schermata di configurazione di directory:

![Immagine di download dell'agente del servizio di creazione di report MIM](media/MIM-Hybrid-downloadReportAgent.jpg)

## Come funziona BitLocker?
Dopo aver installato l'agente di reporting, i dati dell'attività di gestione delle identità viene inviati al registro eventi di Windows. Dichiarante elabora gli eventi e li carica in Azure. In Azure i dati dell'attività sono memorizzati, attualmente su base mensile. Quando si recupera il report, gli eventi dell'attività sono analizzati e filtrati per i report necessari. Infine, il portale di gestione di Azure consente di recuperare i dati dei report e viene eseguito il rendering del report di attività.

![Diagramma del servizio di creazione di report ibridi](media/MIM-Hybrid-howitworks.png)


<!--HONumber=Apr16_HO2-->


