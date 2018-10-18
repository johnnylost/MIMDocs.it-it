---
title: Gestione dei dati di Microsoft Identity Manager | Microsoft Docs
description: Informazioni sulla gestione dei dati di Microsoft Identity Manager per identificare e creare report sui dati all'interno dell'ambiente ed eseguire le azioni necessarie in un determinato sistema in base alle funzioni ai requisiti operativi.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 05/22/2018
ms.topic: get-started-article
ms.prod: microsoft-identity-manager
ms.assetid: b0b39631-66df-4c5f-80c9-a1774346f816
ms.suite: ems
ms.openlocfilehash: 4102ffc450b993faaa62da66bb25f242b7e39280
ms.sourcegitcommit: 7de35aaca3a21192e4696fdfd57d4dac2a7b9f90
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/16/2018
ms.locfileid: "49358721"
---
# <a name="microsoft-identity-manager-data-handling"></a>Gestione dei dati di Microsoft Identity Manager 

Questo articolo fornisce indicazioni su come l'organizzazione possa usare le operazioni di ricerca, eliminazione, aggiornamento e creazione di report per prendere decisioni da mettere in pratica o implementare in più origini dati connesse. Prima di decidere per l'approccio di eliminazione o di aggiornamento, è importante conoscere la progettazione e la configurazione correnti del sistema di gestione delle identità (MIM). Sotto sono elencati alcuni scenari che i clienti dovranno prendere in considerazione, oltre a rispondere alle domande seguenti: 

- Quali dati sono necessari per semplificare il processo aziendale con la gestione delle identità?
- Dove verranno archiviati i dati correnti in MIM?
- Come si useranno questi dati nel sistema?
- Si condivideranno questi dati con origini dati di partner esterni (esportazione)?
- Qual è origine autorevole per i dati e la relativa elaborazione?
- Quale piano di conservazione ed eliminazione dei dati verrà applicato?
- È stata identificata tutta la tecnologia necessaria per elaborare e gestire i dati?

Per conoscere un ambiente MIM corrente, è possibile usare lo strumento seguente per documentare l'ambiente MIM oppure vedere i documenti sulla progettazione dell'implementazione.
- [MIM Documentor: consente di esportare la configurazione corrente](https://github.com/Microsoft/MIMConfigDocumenter)

## <a name="searching-for-and-identifying-personal-data"></a>Ricerca e identificazione dei dati personali
La ricerca dei dati all'interno di MIM dipenderà dalla configurazione e dall'installazione. La maggior parte degli ambienti è interconnessa, ma per maggiore chiarezza sono stati distinti in componenti generali.

### <a name="synchronization-service"></a>Servizio di sincronizzazione

Tutti i dati in MIM correlati agli utenti derivano da Active Directory (AD) e da origini dati relative alle risorse umane. Quando si cercano i dati personali, è consigliabile iniziare la ricerca da Active Directory o dalle origini dati connesse. 

Se non si è certi dell'origine dell'autorità, è possibile tenere traccia di questo utente dalla console di MIM Synchronization Service Manager. Fare clic sulla barra Metaverse Search (Ricerca metaverse) per visualizzare i dati personali identificabili archiviati nel database. Gli utenti possono cercare un utente specifico o un attributo specifico.

- Per eseguire una revisione o una ricerca dei dati degli oggetti utente
    - Aprire il client del servizio di sincronizzazione
        - Usando la finestra di progettazione metaverse, è possibile visualizzare la precedenza e le importazioni del flusso dell'attributo.
![mim-privacy-compliance_1.PNG](media/mim-privacy-compliance/mim-privacy-compliance_1.PNG)
        - Usando la ricerca metaverse, è possibile eseguire una ricerca in qualsiasi oggetto e attributo all'interno del database ![mim-privacy-compliance_2.PNG](media/mim-privacy-compliance/mim-privacy-compliance_2.PNG)
 
Dopo avere trovato l'oggetto, fare clic sull'oggetto per aprire la pagina del profilo utente. I dettagli dell'oggetto forniscono informazioni complete sull'oggetto, gli attributi, la data dell'ultima modifica, l'origine dell'autorità e l'origine dati connessa correlata, derivata dall'esempio di configurazione dell'agente di gestione seguente.

![mim-privacy-compliance.PNG](media/mim-privacy-compliance/mim-privacy-compliance.PNG)

### <a name="service-and-portal--pam"></a>Servizio e portale/PAM
Se si ha un'istanza del servizio e del portale o di PAM installata, è importante poter cercare gli utenti. 

Se è stato installato il portale, è possibile usare l'interfaccia utente per la ricerca di un determinato utente in qualsiasi attributo o query.

Se è stato installato solo il server del servizio (senza interfaccia utente del portale), è possibile eseguire una sintassi di ricerca basata sull'esempio [FIMAutomation PSSnapin] disponibile [qui](https://social.technet.microsoft.com/wiki/contents/articles/22713.fim-portals-use-powershell-to-find-all-users-without-a-manager.aspx).

PAM può usare la stessa sintassi precedente oppure è possibile usare il [modulo MIMPAM](https://docs.microsoft.com/powershell/module/mimpam/get-pamuser?view=idm-ps-2016sp1), in particolare il cmdlet get-pamuser, per cercare l'utente nell'ambiente PAM.

Altre opzioni di creazione di report per la ricerca nei dati disponibili si trovano nel servizio e nel portale.
- [Servizio di creazione di report ibridi](https://docs.microsoft.com/microsoft-identity-manager/identity-manager-hybrid-reporting-azure)
- [Reporting with SCSM](https://docs.microsoft.com/previous-versions/mim/jj133853%28v%3dws.10%29) (Creazione di reporting con SCSM)

### <a name="bhold"></a>BHOLD
Il servizio Bhold Core ha un'interfaccia utente che consente di cercare un utente o gli attributi. 

![Ricerca di Bhold](media/mim-privacy-compliance/mim-privacy-compliance-bhold.PNG)

Se si sincronizza BHOLD con [Access Management Connector](https://docs.microsoft.com/microsoft-identity-manager/bhold/bhold-access-management-connector-install) per il servizio di sincronizzazione, sarà possibile visualizzare gli oggetti utente connessi e gli attributi inviati a BHOLD Core.

È anche possibile caricare il modulo BHOLD Reporting.

- [BHOLD Reporting](https://docs.microsoft.com/microsoft-identity-manager/bhold/bhold-concepts-guide#reporting)

### <a name="certificate-management"></a>Gestione dei certificati
Il servizio Gestione certificati è integrato nell'interfaccia utente. L'amministratore eseguirà l'avvio e selezionerà "Trova utente per visualizzare o gestire le relative informazioni"  

![Ricerca con la gestione dei certificati](media/mim-privacy-compliance/mim-privacy-compliance-cm.PNG)

## <a name="exporting-personal-data"></a>Esportazione dei dati personali
Poiché i dati relativi alle entità in MIM sono derivati da più origini, la maggior parte dei dati viene archiviata nel database del servizio di sincronizzazione. Per questo motivo, è consigliabile esportare i dati relativi agli oggetti dal servizio di sincronizzazione MIM oppure è possibile determinare il proprietario di tali dati.

### <a name="synchronization-service"></a>Servizio di sincronizzazione
I servizi di sincronizzazione per l'esportazione dei dati selezionano semplicemente i dati dall'interfaccia utente di ricerca e li copiano e incollano in formato CSV o nel formato preferito. Un altro modo per esportare i dati consiste nel creare un agente di gestione basato sui file per rilasciare i dati correnti necessari relativi a un utente di interesse contrassegnato. Per un esempio di uso dell'agente di gestione basato sui file, vedere [qui](https://blogs.msdn.microsoft.com/connector_space/2016/11/17/management-agent-configuration-part-4-delimited-text-file-management-agent/).


### <a name="service-and-portal--pam"></a>Servizio e portale/PAM
Il servizio e il portale con PAM consentono di esportare questi dati e di eseguire una sintassi di ricerca basata su [FIMAutomation PSSnapin] (vedere [qui](https://social.technet.microsoft.com/wiki/contents/articles/22713.fim-portals-use-powershell-to-find-all-users-without-a-manager.aspx) per l'esempio) e di inviarli tramite pipe al file [CSV](https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/export-csv?view=powershell-6).

PAM può usare la stessa sintassi precedente oppure è possibile usare il [modulo MIMPAM](https://docs.microsoft.com/powershell/module/mimpam/get-pamuser?view=idm-ps-2016sp1), in particolare get-pamuser, per cercare l'utente nell'ambiente PAM e inviarlo tramite pipe a un file CSV.

- [Example Querying The MIM Service Using PowerShell](https://gallery.technet.microsoft.com/Querying-The-FIMMIM-dcb82de3) (Query di esempio del servizio MIM con PowerShell)

### <a name="bhold"></a>BHOLD
I dati di Bhold possono essere esportati nel formato preferito usando il modulo Bhold Reporting.

### <a name="certificate-management"></a>Gestione dei certificati
I dati di Gestione certificati correlati ai dati personali sono connessi ad Active Directory. Un amministratore può esportare i dati con Active Directory PowerShell.

## <a name="updating-personal-data"></a>Aggiornamento dei dati personali

I dati personali relativi a utenti o oggetti nelle soluzioni MIM sono in genere derivati dall'oggetto dell'utente nelle origini dati connesse dell'organizzazione. Eventuali modifiche apportate al profilo utente nell'origine delle risorse umane o in un altro sistema di record autorevole, ad esempio AD, vengono applicate al servizio di sincronizzazione MIM.

### <a name="synchronization-service"></a>Servizio di sincronizzazione

Per eseguire operazioni di gestione, gli amministratori devono fare parte delle operazioni o degli amministratori della sincronizzazione definiti [qui](https://docs.microsoft.com/previous-versions/mim/jj590183(v%3dws.10)).

L'aggiornamento dei dati viene eseguito definendo le regole dall'origine dell'autorità. La console di gestione consente di identificare l'origine dell'autorità per aggiornarla nell'origine. Un'altra opzione consiste nel creare la regola di sincronizzazione o l'estensione della regola per controllare l'aggiornamento dei dati se l'origine, ad esempio i dati delle risorse umane, deve essere conservati. Queste sono le opzioni supportate disponibili.

Per altre informazioni su modi diversi di aggiornare l'attributo, vedere di seguito. 

- [Using Rules Extensions](https://msdn.microsoft.com/library/windows/desktop/ms698810(v=vs.100).aspx) (Uso di estensioni delle regole)
- [Informazioni sulla sincronizzazione dei dati con sistemi esterni](https://docs.microsoft.com/previous-versions/mim/jj133850(v%3dws.10))

### <a name="service-and-portal--pam"></a>Servizio e portale/PAM

Il servizio e il portale per includere i dati PAM possono essere aggiornati usando i cmdlet FIMAutomation o PAM. Se è disponibile il portale, è anche possibile aggiornarlo direttamente cercando e modificando l'oggetto. Tenere presente che, a seconda della configurazione, il semplice aggiornamento dal portale non verrà necessariamente conservato, perché l'origine dell'autorità dipende in larga misura dalla configurazione generale.

### <a name="bhold"></a>BHOLD

Gli utenti possono essere aggiornati direttamente con l'interfaccia utente di BHOLD Core o con Access Management Connector.

### <a name="certificate-management"></a>Gestione dei certificati

Gli utenti nel servizio Gestione certificati sono tutti una reflection da Active Directory. Per l'aggiornamento, usare Active Directory per modificare i dettagli degli oggetti.

## <a name="deleting-personal-data"></a>Eliminazione dei dati personali

>[!Note] 
> Questo articolo fornisce indicazioni per eliminare i dati personali da Microsoft Identity Manager e può essere usato per supportare le obbligazioni derivanti dal Regolamento generale sulla protezione dei dati. Per informazioni sul Regolamento generale sulla protezione dei dati, vedere la [sezione GDPR di Service Trust Portal](https://servicetrust.microsoft.com/ViewPage/GDPRGetStarted).

I dati in MIM vengono sincronizzati e aggiornati sempre dall'origine dati connessa. Quando un oggetto viene eliminato nella destinazione, i dati dell'oggetto in MIM possono essere conservati per l'esecuzione di analisi di sicurezza. L'eliminazione degli oggetti viene configurata in base alle regole delle origini dati connesse o all'estensione delle regole (codice) e/o alle regole di eliminazione degli oggetti.

### <a name="synchronization-service"></a>Servizio di sincronizzazione
Il servizio di sincronizzazione consente di gestire o di eliminare i dati in più modi a seconda dei processi aziendali. Di seguito sono elencati alcuni articoli sulle opzioni di eliminazione e aggiornamento degli attributi: 

- [Informazioni sul deprovisioning](https://social.technet.microsoft.com/wiki/contents/articles/1270.understanding-deprovisioning-in-fim.aspx)
- [Using Rules Extensions](https://msdn.microsoft.com/library/windows/desktop/ms698810(v=vs.100).aspx) (Uso di estensioni delle regole)
- [Procedure consigliate per MIM](https://docs.microsoft.com/microsoft-identity-manager/mim-best-practices)

### <a name="service-and-portal--pam"></a>Servizio e portale/PAM

Per il servizio e il portale, è consigliabile mantenere la configurazione predefinita di conservazione delle risorse per 30 giorni. In questo modo il servizio saprà quando eliminare non solo i dati, ma anche gli oggetti che devono essere cancellati dal sistema. Quando il processo viene eseguito, tutti i dati collegati a questo oggetto vengono eliminati, inclusi tutti i dati di registrazione SSPR. Ciò rispecchia la configurazione dell'eliminazione degli oggetti precedente. È disponibile una tabella in cui viene archiviato il GUID degli oggetti. Per ridurre le dimensioni totali della tabella nella build 4.4.1459, è stato aggiunto un processo denominato FIM_DeleteExpiredSystemObjectsJob. Per informazioni dettagliate su questo processo, vedere [qui](https://support.microsoft.com/en-us/help/4012498/hotfix-rollup-package-build-4-4-1459-0-is-available-for-microsoft-iden).

![mim-privacy-compliance-srrc.PNG](media/mim-privacy-compliance/mim-privacy-compliance-srrc.PNG)


### <a name="bhold"></a>BHOLD

Bhold, come la maggior parte dei sistemi connessi al servizio di sincronizzazione, può essere configurato per l'eliminazione dopo che l'oggetto di origine, ad esempio le risorse umane, è stato rimosso. La configurazione viene eseguita nell'agente di gestione e controllata dalle regole di eliminazione degli oggetti, come descritto nelle funzionalità del servizio di sincronizzazione.

Un'altra opzione è la rimozione dell'oggetto utente direttamente dall'interfaccia utente di BHOLD Core. A seconda della configurazione, può funzionare bene, ma si noti che la logica di provisioning potrebbe creare di nuovo l'utente se non viene eliminato nell'origine.
![mim-privacy-compliance-bholdr.PNG](media/mim-privacy-compliance/mim-privacy-compliance-bholdr.PNG)


### <a name="certificate-management"></a>Gestione dei certificati
Per rimuovere un utente da Gestione certificati, è necessario eliminarlo in Active Directory.

In Gestione certificati viene infatti archiviato solo lo UID del profilo da Servizi certificati con il nome sAMAccountName di dominio. Dopo l'eliminazione dell'utente da AD, la cache utente è presente solo per i certificati registrati. È consigliabile non eliminare nulla dal database perché ciò potrebbe provocare danni al funzionamento dell'ambiente.

## <a name="opt-out-of-telemetry"></a>Rifiutare esplicitamente la telemetria
Le build precedenti di FIM/MIM raccoglievano dati di telemetria anonimi su ogni distribuzione e li trasmettevano tramite HTTPS ai server Microsoft. Questi dati venivano usati in passato da Microsoft per migliorare le future versioni di FIM/MIM.

>[!Note] 
> Nelle versioni 4.5.x.x e successive la raccolta dati verrà disabilitata.

Per disabilitare la raccolta dati nella versione precedente, eseguire la modalità di modifica e deselezionare il prompt seguente:

![mim-privacy-compliance-ceip.PNG](media/mim-privacy-compliance/mim-privacy-compliance-ceip.PNG)

È anche possibile modificare il Registro di sistema e impostare il valore su 0: (Componente)CEIP HKLM\SOFTWARE\Microsoft\Forefront Identity Manager\2010

![mim-privacy-compliance-ceip2.PNG](media/mim-privacy-compliance/mim-privacy-compliance-ceip2.PNG)

## <a name="next-steps"></a>Passaggi successivi 
- [Per indicazioni sulla privacy relative a SQL](https://docs.microsoft.com/sql/relational-databases/security/microsoft-sql-and-the-gdpr-requirements?view=sql-server-2017)
- [Sezione GDPR di Service Trust Portal](https://servicetrust.microsoft.com/ViewPage/GDPRGetStarted)
- [FIM 2010 Archive: Ramp Up - Implementing Forefront Identity Manager 2010 (Archivio di FIM 2010: incremento - Implementazione di Forefront Identity Manager 2010)](https://social.technet.microsoft.com/wiki/contents/articles/35789.fim-2010-archive-ramp-up-implementing-forefront-identity-manager-2010.aspx)
