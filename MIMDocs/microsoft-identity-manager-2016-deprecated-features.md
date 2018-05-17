---
title: Funzionalità deprecate di MIM e pianificazione per il futuro | Microsoft Docs
description: Questo articolo documenta le funzionalità deprecate di Microsoft Identity Manager (MIM) 2016 SP1.
keywords: ''
author: barclayn
ms.author: davidste
manager: mbaldwin
ms.date: 2/28/2018
ms.topic: reference
ms.prod: identity-manager-2016
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: ''
ms.openlocfilehash: 50f7b135ce0d5a46ea08068a7658b229759d2b50
ms.sourcegitcommit: 24bb3e82f55971696bdefa6c240f1a27f856e110
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/17/2018
---
# <a name="deprecated-features"></a>Funzionalità deprecate

Questo articolo descrive le funzionalità deprecate di Microsoft Identity Manager 2016 SP1. Se la funzionalità è ancora presente in Microsoft Identity Manager, è ancora supportata. L'uso delle funzionalità deprecate non è consigliato per le nuove distribuzioni, perché potrebbero essere rimosse in una versione futura.  Per gli sviluppatori, è consigliabile non usare le funzionalità deprecate in applicazioni o soluzioni nuove.

>[!NOTE]
Le caratteristiche e funzionalità rimosse in Microsoft Identity Manager SP1 sono identificate con **. <br>
Altre informazioni sul [ciclo di vita del supporto per Microsoft Identity Manager](https://support.microsoft.com/en-us/lifecycle/search?alpha=Microsoft%20Forefront%20Identity%20Manager%202010%20R2%20Service%20Pack%201,Microsoft%20Identity%20Manager%202016,Microsoft%20Forefront%20Identity%20Manager%202010)


## <a name="bhold"></a>BHOLD 

Microsoft non consiglia ai clienti di avviare nuove distribuzioni dei componenti di Microsoft BHOLD Suite. Le distribuzioni esistenti di BHOLD continueranno a essere supportate. Azure AD fornisce [verifiche di accesso](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-azure-ad-controls-access-reviews-overview) che sostituiscono alcune delle funzionalità delle campagne di attestazione BHOLD.

## <a name="certificate-management"></a>Gestione dei certificati 
| **Categoria**                | **Funzionalità deprecata**              | **Sostituzione e commento**           |
|-----------------------------|-------------------------------------|----------------------------------------------|
| Agenti di gestione | **Gestione dei certificati FIM | L'agente di gestione dei certificati FIM è stato rimosso in MIM 2016.                                                             |

## <a name="service-and-portal"></a>Servizio e portale

| **Categoria**                | **Funzionalità deprecata**              | **Sostituzione e commento**           |
|-----------------------------|-------------------------------------|----------------------------------------------|
| Configurazione a livello di codice | Interfaccia di configurazione del servizio Web (ma-data e mv-data) | La possibilità di configurare il servizio di sincronizzazione FIM tramite il servizio Web del servizio FIM verrà rimossa in una versione successiva.
|

## <a name="synchronization-service"></a>Servizio di sincronizzazione 

| **Categoria**                | **Funzionalità deprecata**              | **Sostituzione e commento**           |
|-----------------------------|-------------------------------------|----------------------------------------------|
| Configurazione a livello di codice | Interfaccia di configurazione del servizio Web | La possibilità di configurare il servizio di sincronizzazione FIM tramite il servizio FIM verrà rimossa in una versione successiva.                                                          |
| Agenti di gestione           | Agenti di gestione predefiniti                        | I seguenti agenti di gestione sono stati rimossi in MIM 2016: </br> 1.  **Agente di gestione per la gestione dei certificati FIM </br>2.  **Agente di gestione per Lotus Notes</br> 3.  **Agente di gestione per SAP R/3 </br> Gli agenti di gestione Lotus Notes e SAP R/3 sono stati sostituiti con nuove versioni. Per altre informazioni, vedere la [cronologia delle versioni più recenti dei connettori e download](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnectsync-connector-version-history)                                                                                                                                                                                                                                              |
| Agenti di gestione           | ECMA1                               | Il framework di estendibilità ECMA1/XMA è stato sostituito da ECMA 2.0. È richiesto l'aggiornamento degli agenti di gestione esistenti ECMA1 con connettori ECMA 2.0.                                                                                                                                          |
| Agenti di gestione           | Esecuzione dei connettori out-of-process      | Questa funzionalità non verrà sostituita. Il servizio di sincronizzazione chiamerà sempre il connettore nello stesso processo. È responsabilità del connettore avviare e gestire l'altro processo. |
| Agenti di gestione           | Configurare il nome visualizzato della partizione    | Questa funzionalità non verrà sostituita. Questa opzione veniva usata solo per fornire un nome alternativo per una partizione nelle interfacce WMI.                                                                                                                                                                       |
| Profili di esecuzione                | Profili combinati                   | Le funzionalità di importazione/sincronizzazione delta, importazione completa/sincronizzazione delta e importazione/sincronizzazione completa per i profili combinati verranno rimosse. È invece consigliabile usare profili di esecuzione con due passaggi. 

>[!NOTE]
È consigliabile mantenere i profili di esecuzione combinati solo in ambienti in cui le prestazioni potrebbero risentire di un numero elevato di oggetti non connettore esistenti.


| **Categoria**                | **Funzionalità deprecata**              | **Sostituzione e commento**           |
|--------|-------|---|    
| Precedenza attributi | Multi-valore/precedenza uguale                       | La precedenza uguale verrà rimossa. Non è prevista una sostituzione per questa funzionalità. È invece necessario configurare la precedenza manuale. È possibile continuare a usare questa funzionalità se nell'ambiente è distribuito un agente di gestione del servizio FIM. Questo agente di gestione non offre funzionalità di precedenza manuale per evitare l'esportazione di valori senza precedenza per il provisioning dichiarativo. |
| Regole di unione           | Unione sul tipo di oggetto "Any"                             | Questa funzionalità non verrà sostituita. Tutte le regole di unione devono definire esplicitamente il tipo di oggetto metaverse di destinazione del tentativo di unione.       |
| Flussi di attributi      | Deselezionare "Consenti valori null" per i valori esportati            | Questa funzionalità non verrà sostituita. "Consenti valori null" sarà sempre selezionato. Accertarsi di avere selezionato "Consenti valori null" nell'ambiente corrente.  |
| Flussi di attributi      | "Non richiamare gli attributi"                            | Questa funzionalità non verrà sostituita. Gli attributi verranno sempre richiamati, come da procedura consigliata.  |
| Estensione delle regole      | Esecuzione out-of-process dell'estensione delle regole metaverse e ma | Questa funzionalità non verrà sostituita. Le regole del flusso di attributi e metaverse verranno eseguito nello stesso processo del motore di sincronizzazione.       |
| Estensione delle regole      | Proprietà di transazione                                | Questa funzionalità non verrà sostituita. È consigliabile evitare di passare dati tra sincronizzazioni in ingresso, di provisioning e in uscita tramite questa classe di utilità.  |
| Estensione delle regole      | ExchangeUtils: metodi Create55\*                     | I metodi per creare oggetti per i server Exchange 5.5 verranno rimossi.        |
| Interfaccia            | Mms_Metaverse                                        | Tutti i membri della classe ClmUtils verranno rimossi in una versione successiva.   |

## <a name="next-steps"></a>Passaggi successivi
Sono disponibili altre informazioni su:

- Microsoft Identity Manager è ancora strettamente correlato al suo predecessore, Forefront Identity Manager. Se si usa ancora FIM o si vogliono consultare altri documenti, vedere [FIM 2010 R2 Documentation Roadmap](https://technet.microsoft.com/library/jj133885.aspx) (Guida di orientamento sulla documentazione di FIM 2010 R2).
- [Considerazioni relative alla topologia per la distribuzione di MIM](topology-considerations.md) Questo articolo presenta varie topologie di distribuzione che è possibile implementare.
- [Guida alla pianificazione della capacità](capacity-planning-guide.md) È possibile consultare questa guida con i relativi ambienti di test per comprendere l'ambito appropriato per la distribuzione.
