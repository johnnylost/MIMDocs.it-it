---
title: Guida alla topologia per la distribuzione | Documentazione Microsoft
description: Comprendere i componenti di MIM 2016 e ottenere suggerimenti su come distribuirli nell'ambiente in uso.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 10/12/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 735dc357-dfba-4f68-a5b3-d66d6c018803
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 42562e92b3fe0daa63110d33d8952a3a1fc3de17
ms.sourcegitcommit: 7de35aaca3a21192e4696fdfd57d4dac2a7b9f90
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/16/2018
ms.locfileid: "49358081"
---
# <a name="topology-considerations"></a>Considerazioni relative alla topologia
È possibile distribuire i componenti di Microsoft Identity Manager (MIM) nello stesso server o tra più server in più configurazioni. La topologia selezionata per la distribuzione influisce sulle prestazioni che è possibile ottenere da MIM. Questo articolo presenta più topologie di distribuzione che si potrebbe considerare di implementare.


> [!NOTE]
> Per la gestione delle identità, queste opzioni si applicano alle distribuzioni esclusivamente tramite la sincronizzazione MIM, il servizio MIM e il portale MIM.  Le distribuzioni tramite Gestione certificati MIM, MIM BHOLD Suite o per la gestione dell'accesso con privilegi hanno opzioni diverse.


## <a name="mim-components"></a>Componenti MIM
Quando si progetta la topologia di distribuzione, è importante conoscere le funzionalità di ciascun componente e la modalità in cui tutti i componenti interagiscono.

- <a name="mim-portal---an-interface-for-password-resets-group-management-and-administrative-operations"></a>**Portale MIM**: un'interfaccia per la reimpostazione della password, la gestione di gruppo e le operazioni amministrative.
    -
- **Servizio MIM**: un servizio Web che implementa la funzionalità di gestione delle identità di MIM 2016.
- **Servizio di sincronizzazione MIM**: sincronizza i dati con altri sistemi di identità.
- **Microsoft SQL Server**: il servizio MIM e il servizio di sincronizzazione MIM archiviano i dati in database SQL.

Le tabelle seguenti illustrano le opzioni per l'hosting di ciascuno dei componenti MIM. Possono essere ospitati nello stesso computer o distribuiti tra più server e cluster.

| | Portale MIM | Servizio MIM | Servizio Sincronizzazione MIM | SQL Server |
| --- | --- | --- | --- | --- |
| Stesso computer | Sì | Sì | Sì | Sì |
| Server separato | Sì | Sì | Sì | Sì |
| Cluster di Bilanciamento carico di rete | Sì | Sì | | |
| Cluster di server | | | | Sì |


## <a name="multitier-topology"></a>Topologia a più livelli
La topologia a più livelli è la topologia più diffusa. Offre la massima flessibilità. Il portale MIM, il servizio MIM e i database sono suddivisi in livelli e distribuiti in più computer. Questa topologia aggiunge flessibilità al ridimensionamento dei diversi componenti MIM. È possibile, ad esempio, ridimensionare orizzontalmente il portale MIM aggiungendo altri server in un cluster di Bilanciamento carico di rete. Analogamente, è possibile ridimensionare il servizio MIM usando un cluster di Bilanciamento carico di rete e aumentando il numero di computer (nodi) del cluster in base alle esigenze.

Nella topologia a più livelli, viene allocato un computer dedicato per ospitare ogni database SQL (uno per il servizio MIM e un altro per il servizio di sincronizzazione MIM). Il ridimensionamento delle prestazioni dei computer che ospitano i database SQL può essere aumentata aggiungendo o aggiornando l'hardware, ad esempio, aggiornando le CPU, aggiungendo altre CPU, aumentando o aggiornando la memoria ad accesso casuale (RAM) o aggiornando le configurazioni del disco rigido per aumentare l'accesso in lettura e scrittura e ridurre la latenza.

![Diagramma della topologia a più livelli MIM](media/MIM-topo-multitier.png)

In questa configurazione il servizio di sincronizzazione MIM e il relativo database sono ospitati nello stesso computer. Tuttavia, si dovrebbe poter ottenere prestazioni simili se è disponibile una connessione di rete dedicata da un gigabit tra il servizio di sincronizzazione MIM e il relativo database quando sono ospitati in computer separati.


## <a name="multitier-topology-with-multiple-mim-services"></a>Topologia a più livelli con più servizi MIM
In questo intervallo di tempo, la sincronizzazione dei dati con i sistemi esterni può richiedere molto tempo e aggiungere un carico notevole al sistema. Se la configurazione della sincronizzazione determina l'attivazione di criteri con i flussi di lavoro, questi criteri si contendono le risorse con i flussi di lavoro dell'utente finale. Questi problemi possono risultare significativi con i flussi di lavoro di autenticazione, ad esempio la reimpostazione della password, che vengono eseguiti in tempo reale con un utente finale in attesa che venga completato il processo. Fornendo un'istanza del servizio MIM per le operazioni dell'utente finale e un portale separato per la sincronizzazione dei dati amministrativi, è possibile offrire una migliore velocità di risposta per le operazioni dell'utente finale.

![Diagramma della topologia a più livelli con più servizi MIM](media/MIM-topo-multitier-multiservice.png)

Come con la topologia a più livelli standard, è possibile aumentare le prestazioni del portale MIM usando un cluster di Bilanciamento carico di rete e aumentando il numero di nodi del cluster in base alle esigenze.

I computer che eseguono SQL Server e che ospitano il servizio di sincronizzazione MIM e il database del servizio MIM influiranno notevolmente sulle prestazioni generali della distribuzione di MIM. Seguire, quindi, le indicazioni contenute nella documentazione di SQL Server per l'ottimizzazione delle prestazioni del database. Per altre informazioni, vedere i documenti seguenti:

## <a name="see-also"></a>Vedere anche
- La [Guida alla pianificazione della capacità di Forefront Identity Manager (FIM) 2010](http://go.microsoft.com/fwlink/?LinkId=200180) scaricabile esamina più in dettaglio la compilazione di un test e i risultati dei test delle prestazioni.
