---
title: Installazione di BHOLD SP1 | Microsoft Docs
description: Documentazione di installazione di BHOLD SP1
keywords: ''
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/11/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: ''
ms.openlocfilehash: 11cde4e3b2779f9c32d9849a47713acf5f120b3c
ms.sourcegitcommit: 35f2989dc007336422c58a6a94e304fa84d1bcb6
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36289697"
---
# <a name="microsoft-bhold-suite-sp1-60-installation-guide"></a>Guida all'installazione di Microsoft BHOLD Suite SP1 (6.0)

Microsoft® BHOLD Suite Service Pack 1 (SP1) è una raccolta di applicazioni che, se usata con Microsoft Identity Manager 2016 SP1 (MIM), aggiunge a MIM efficaci funzionalità di gestione dei ruoli, analisi e attestazione. Microsoft BHOLD Suite SP1 include i moduli seguenti:

- BHOLD Core
- Access Management Connector
- BHOLD FIM/MIM Integration
- BHOLD Model Generator
- BHOLD Analytics
- BHOLD Reporting
- BHOLD Attestation


> [!NOTE]
> **Si applica a**: Microsoft Identity Manager 2016 SP1

## <a name="what-this-document-covers"></a>Contenuto del documento

Questo documento spiega come pianificare la distribuzione di BHOLD per soddisfare le esigenze aziendali e installare ogni modulo BHOLD. Per ogni modulo vengono descritti in dettaglio i requisiti hardware, software e di infrastruttura rilevanti, la configurazione della rete di preinstallazione, le informazioni richieste durante la configurazione e gli eventuali passaggi necessari dopo l'installazione.

## <a name="pre-requisite-knowledge"></a>Competenze necessarie

In questo documento si presuppone una conoscenza di base di come installare il software nei computer server. Si presume anche che l'utente possieda una conoscenza di base del software di database Active Directory® Domain Services, Microsoft Identity Manager SP1 (FIM) e Microsoft SQL Server 2008. L'ambito di questa documentazione non prevede una descrizione di come installare e configurare le tecnologie dipendenti, ad esempio Active Directory Domain Services e FIM. Per informazioni sulle funzioni eseguite dai moduli di Microsoft BHOLD, vedere la [Guida ai concetti di Microsoft BHOLD Suite](https://technet.microsoft.com/library/jj134102(v=ws.10).aspx).

## <a name="audience"></a>Destinatari

Questo documento è destinato a progettisti IT, architetti di sistemi, responsabili di tecnologia, consulenti, addetti alla pianificazione dell'infrastruttura e personale IT che intendono distribuire Microsoft BHOLD Suite.

## <a name="bhold-infrastructure-considerations"></a>Considerazioni sull'infrastruttura BHOLD

In genere BHOLD e FIM vengono usati in ambienti di infrastruttura di grandi dimensioni. È possibile personalizzare l'architettura BHOLD e FIM per soddisfare le specifiche esigenze aziendali. Le sezioni seguenti offrono alcune possibili soluzioni di architettura. Questa panoramica non è un elenco completo di tutte le opzioni possibili, ma vengono suggerite alcune modalità con le quali si può distribuire BHOLD nella rete.
 
In questa sezione vengono trattati i seguenti argomenti:

- Architettura a server singolo
- Architettura a server doppio
- Architettura a due livelli
- Consigli per SQL Server

### <a name="single-server-architecture"></a>Architettura a server singolo

Per la distribuzione in organizzazioni di piccole dimensioni o per scopi di sviluppo, è possibile installare BHOLD e FIM nello stesso server di SQL Server e Active Directory Domain Services, come illustrato nella figura seguente.
 
![Architettura a server singolo](media/bhold-installation-guide/single.png)

Quando BHOLD Suite SP1 e il portale di FIM vengono installati insieme in un unico server, è necessario creare alias dell'host diversi (record CNAME o A) in DNS per BHOLD e per FIM. Ciò consente la creazione di nomi dell'entità servizio separati per i servizi BHOLD e FIM. Per altre informazioni, vedere [BHOLD Core Installation](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx) (Installazione di BHOLD Core).
Per istruzioni sull'installazione di FIM in una configurazione a server singolo, vedere [Common Configuration for Getting Started Guides](https://technet.microsoft.com/library/ff575965.aspx) (Guide alle configurazioni comuni per le attività iniziali) nella libreria Microsoft TechNet.

### <a name="dual-server-architecture"></a>Architettura a server doppio

L'installazione di BHOLD Core e FIM su server separati offre migliori prestazioni e flessibilità per le organizzazioni di medie dimensioni che non richiedono una distribuzione più complessa, ad esempio quella offerta da architetture a più livelli. La figura seguente illustra BHOLD e FIM installati nei propri server; il server FIM esegue anche SQL Server per offrire servizi di database a BHOLD e FIM. Il servizio di sincronizzazione FIM in esecuzione nel server FIM consente di sincronizzare le modifiche tra i database di FIM e BHOLD. Si noti che se è necessario il self-service per gli utenti finali il modulo di integrazione di BHOLD FIM deve essere installato nello stesso server del servizio FIM e del portale di FIM. Il modulo di integrazione di BHOLD FIM richiede che il servizio FIM e il modulo di integrazione di BHOLD FIM siano installati nello stesso server.

![Architettura a server doppio](media/bhold-installation-guide/dual.png)

> [!IMPORTANT]
> La funzionalità di creazione di report del modulo di integrazione di BHOLD FIM richiede che i database BHOLD e FIM siano installati nella stessa istanza di SQL Server e che l'account del servizio BHOLD possieda i diritti di accesso al database del servizio FIM.

### <a name="two-tier-architecture"></a>Architettura a due livelli

Nella maggior parte degli ambienti, specialmente quelli in cui le prestazioni sono importanti, è consigliabile eseguire BHOLD Suite SP1, FIM e SQL Server in server separati (architettura a due livelli). Con un'architettura a due livelli, memoria e risorse della CPU sono dedicate per ogni livello. Nella figura seguente viene illustrato uno dei possibili modi per configurare un'architettura a due livelli. Il servizio di sincronizzazione FIM in esecuzione nel server FIM consente di sincronizzare le modifiche tra i database di FIM e BHOLD. Si noti che se è necessario il self-service per gli utenti finali il modulo di integrazione di BHOLD FIM deve essere installato nello stesso server del servizio e del portale di FIM.

![architettura a due livelli](media/bhold-installation-guide/two-tier.png)

### <a name="sql-server-recommendations"></a>Consigli per SQL Server

Se si distribuisce BHOLD in un'organizzazione di grandi dimensioni, è consigliabile seguire queste linee guida per l'impostazione del database di Microsoft SQL Server:

- Distribuire SQL Server in un server diverso da qualsiasi servizio FIM o BHOLD.
- Isolare il file di log dal file di dati a livello di disco fisico.
- Se si usa RAID per garantire la ridondanza di archiviazione, usare il livello RAID 10 (1+0). Non usare il livello RAID 5.
- Verificare di configurare le impostazioni corrette quando si usano più di 2 GB di memoria fisica per il server che esegue SQL Server.
- Per ottenere prestazioni BHOLD ottimali, usare Microsoft SQL Server 2008 R2 o versione successiva.

Per altre informazioni sulle procedure consigliate di SQL Server, vedere [Le dieci procedure di archiviazione migliori](https://www.microsoft.com/technet/prodtechnol/sql/bestpractice/storage-top-10.mspx) nella libreria Microsoft TechNet.

### <a name="trusted-certificates-list-update"></a>Aggiornamento dell'elenco dei certificati attendibili

Windows può essere configurato per la convalida delle catene di certificati prima dell'avvio di un servizio. In questi sistemi, se il codice eseguibile del servizio è stato firmato con un certificato che non è presente nell'elenco dei certificati attendibili (TCL) del server non è possibile avviare un servizio. Il software Microsoft BHOLD Suite SP1 è firmato con una catena di certificati di firma del codice che ha origine con il certificato dell'Autorità di certificazione radice Microsoft 2010.
Windows può essere configurato per recuperare i certificati radice da Microsoft tramite una connessione Internet. In un sistema disconnesso, tuttavia, Windows Server include solo i certificati presenti nel programma radice in una fase precedente al rilascio di Windows. Nelle versioni di Windows Server precedenti a Windows Server 2010, questi certificati non includono il certificato radice necessario per convalidare la catena di certificati di firma del codice di BHOLD Suite SP1. Se si prevede di installare uno o più moduli di Microsoft BHOLD Suite SP1 in un sistema che potrebbe non avere un TCL aggiornato, è necessario scaricare e installare il pacchetto di aggiornamento radice o usare Criteri di gruppo per installare il pacchetto di aggiornamento radice prima di installare un modulo BHOLD Suite SP1. Per altre informazioni, vedere l'[elenco dei membri del programma Windows Root Certificate](http://support.microsoft.com/kb/931125).

### <a name="installing-bhold-suite-sp1-on-windows-server-20122016-required-step"></a>Passaggi necessari per l'installazione di BHOLD Suite SP1 in Windows Server 2012/2016 

![Installazione IIS di BHOLD](media/bhold-installation-guide/iis-install-bhold.png)

Se si installa BHOLD Suite SP1 in Windows Server 2012 o 2016, le pagine Web BHOLD non saranno disponibili finché non verrà modificato il file applicationHost.config situato in ```C:\Windows\System32\inetsrv\config```. Nella sezione ```<globalModules>``` aggiungere ```preCondition="bitness64``` alla voce che inizia con ```<add name="SPNativeRequestModule"``` in modo da avere il risultato seguente:

```<add name="SPNativeRequestModule" image="C:\Program Files\Common Files\Microsoft Shared\Web Server Extensions\15\isapi\spnativerequestmodule.dll" preCondition="bitness64"/>```

Dopo la modifica e il salvataggio del file, eseguire il comando iisreset per ripristinare il server IIS.


## <a name="upgrading-bhold-suite"></a>Aggiornamento di BHOLD Suite

Non è possibile aggiornare un'installazione esistente di BHOLD Suite. È invece necessario disinstallare un'installazione esistente di BHOLD Suite prima di aggiornare i moduli BHOLD. Se si possiede un modello di ruolo BHOLD esistente, è possibile aggiornare il database BHOLD e usarlo quando si installa il modulo BHOLD Core aggiornato. Per altre informazioni, vedere [Replacing BHOLD Suite with BHOLD Suite SP1](https://technet.microsoft.com/library/jj874043(v=ws.10).aspx) (Sostituzione di BHOLD Suite con BHOLD Suite SP1).


## <a name="next-steps"></a>Passaggi successivi

- [Riferimento per gli sviluppatori BHOLD](../reference/mim2016-bhold-developer-reference.md)
- [Cronologia delle versioni BHOLD](../reference/version-bhold-history.md)
