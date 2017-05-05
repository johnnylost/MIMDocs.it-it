---
title: Ripristino di emergenza di PAM | Documentazione Microsoft
description: "Informazioni su come configurare Privileged Access Management per la disponibilità elevata e il ripristino di emergenza."
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/15/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 03e521cd-cbf0-49f8-9797-dbc284c63018
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: bfc73723bdd3a49529522f78ac056939bb8025a3
ms.openlocfilehash: 2fab9af837ed11b1f2f7f32c9ced6d79c8cc9d00
ms.lasthandoff: 05/02/2017


---

# <a name="high-availability-and-disaster-recovery-considerations-for-the-bastion-environment"></a>Considerazioni su disponibilità elevata e ripristino di emergenza nell'ambiente bastion
Questo articolo riporta considerazioni su disponibilità elevata e ripristino di emergenza durante la distribuzione di Servizi di dominio Active Directory e Microsoft Identity Manager 2016 (MIM) per Privileged Access Management (PAM).

Le aziende concentrano l'attenzione sulla disponibilità elevata e il ripristino di emergenza dei carichi di lavoro in Windows Server, SQL Server e Active Directory. Sono tuttavia molto importanti anche l'affidabilità e la disponibilità dell'ambiente bastion per Privileged Access Management. L'ambiente bastion è un elemento fondamentale dell'infrastruttura IT dell'organizzazione, dal momento che gli utenti interagiscono con i suoi componenti per assumere ruoli amministrativi. Per altre informazioni sulla disponibilità elevata, è possibile scaricare il white paper [Microsoft High Availability Overview](http://download.microsoft.com/download/3/B/5/3B51A025-7522-4686-AA16-8AE2E536034D/Microsoft%20High%20Availability%20Strategy%20White%20Paper.doc) (Panoramica sulla disponibilità elevata Microsoft).

## <a name="high-availability-and-disaster-recovery-scenarios"></a>Scenari di disponibilità elevata e ripristino di emergenza

Durante la pianificazione della disponibilità elevata e del ripristino di emergenza, è consigliabile prendere in considerazione le domande seguenti:

- Quali funzioni possono essere interessate da un'interruzione?
- Quali funzioni sono critiche per l'azienda e/o per le operazioni IT?
- Quali sono i rischi che possono causare un'interruzione in questi sistemi?

L'ambito di queste considerazioni influisce sul costo totale della distribuzione e delle operazioni. In questo modo, le organizzazioni possono assegnare a determinate funzioni un livello di priorità più alto rispetto ad altre e accettare il rischio di temporanee interruzioni per le funzioni a priorità più bassa. La tabella seguente riporta una possibile classificazione delle priorità utilizzabile in un'organizzazione:

| **Funzione della foresta bastion** | **Priorità relativa durante il ripristino** | **Mitigazione se la funzione non è disponibile** |
| --------------------------- | --------------------- | -------------- |
| Creazione del trust         | Basso | Attendere fino al ripristino dell'ambiente bastion |
| Migrazione di utenti e gruppi   | Basso | Attendere fino al ripristino dell'ambiente bastion |
| Amministrazione MIM          | Bassa | Attendere fino al ripristino dell'ambiente bastion |
| Attivazione del ruolo con privilegi  | Media | Account dedicati basati su smart card per aggiungere manualmente utenti ai gruppi amministrativi |
| Gestione delle risorse         | Alta | Account dedicati basati su smart card per aggiungere manualmente utenti ai gruppi amministrativi |
| Monitoraggio di utenti e gruppi nella foresta esistente | Basso | Attendere fino al ripristino dell'ambiente bastion |

Di seguito vengono descritte le diverse funzioni della foresta bastion.

### <a name="trust-establishment"></a>Creazione del trust

È necessario che sia presente un trust tra foreste tra i domini della foresta esistente e la foresta dell'ambiente bastion. In questo modo, gli utenti che eseguono l'autenticazione all'ambiente bastion possono amministrare risorse nelle foreste esistenti. Possono essere necessarie operazioni di configurazione aggiuntive, ad esempio per consentire la migrazione di utenti da domini esistenti in precedenti versioni di Windows Server.

Per la creazione del trust è necessario che i controller di dominio della foresta esistente siano online, così come i componenti MIM e Active Directory dell'ambiente bastion.  Se durante la creazione del trust si verifica un'interruzione di uno di questi elementi, l'amministratore può riprovare dopo che l'interruzione è stata risolta.  Se i controller di dominio della foresta esistente o l'ambiente bastion dono stati ripristinati a seguito di un'interruzione, MIM include anche i cmdlet PowerShell `Test-PAMTrust` e `Test-PAMDomainConfiguration`, da usare per verificare che sia ancora presente un trust.

### <a name="user-and-group-migration"></a>Migrazione di utenti e gruppi

Dopo aver creato il trust, è possibile creare gruppi shadow nell'ambiente bastion, nonché account utente per membri di tali gruppi e per responsabili approvazione. Questo consente agli utenti di attivare ruoli con privilegi e ottenere nuovamente un'appartenenza effettiva.

Per la migrazione di utenti e gruppi è necessario che i controller di dominio della foresta esistente siano online, così come i componenti MIM e Active Directory dell'ambiente bastion.   Se i controller di dominio della foresta esistente non sono raggiungibili, non è possibile aggiungere altri utenti e gruppi all'ambiente bastion. Gli utenti e i gruppi esistenti, tuttavia, non sono interessati dal problema. Se durante la migrazione si verifica un'interruzione di uno qualsiasi dei componenti, l'amministratore può riprovare dopo che l'interruzione è stata risolta.

### <a name="mim-administration"></a>Amministrazione MIM
Dopo che la migrazione degli utenti e dei gruppi è stata eseguita, l'amministratore può ulteriormente configurare l'assegnazione dei ruoli in MIM collegando gli utenti come candidati per l'attivazione con i ruoli desiderati.  L'amministratore può inoltre configurare i criteri MIM per l'approvazione e l'autenticazione a più fattori di Azure.  

L'amministrazione di MIM richiede che i componenti MIM e Active Directory dell'ambiente bastion siano online.

### <a name="privileged-role-activation"></a>Attivazione del ruolo con privilegi
Quando un utente desidera attivare un ruolo con privilegi, deve eseguire l'autenticazione al dominio dell'ambiente bastion e inviare una richiesta a MIM.  MIM include API REST e SOAP, oltre a interfacce utente in PowerShell e in una pagina Web.

Per l'attivazione di ruoli con privilegi è necessario che i componenti MIM e Active Directory dell'ambiente bastion siano online.  Inoltre, se MIM è configurato per l'uso di [Azure MFA](use-azure-mfa-for-activation.md) per l'attivazione del ruolo selezionato, è necessario disporre dell'accesso a Internet per contattare il servizio Azure MFA.

### <a name="resource-management"></a>Gestione delle risorse
Dopo che l'utente è stato correttamente attivato con il ruolo desiderato, il controller di dominio può generare per tale utente un ticket Kerberos utilizzabile dai controller di dominio nei domini esistenti. Il controller riconoscerà le nuove appartenenze temporanee dell'utente al gruppo.

Per la gestione delle risorse è necessario che sia online un controller di dominio per il dominio delle risorse, oltre che un controller di dominio nell'ambiente bastion.  Dopo che un utente è stato attivato, per la generazione del relativo ticket Kerberos non è necessario che MIM o SQL siano online nell'ambiente bastion.  Si tenga presente che, con Windows Server 2012 R2 come livello funzionale per l'ambiente bastion, è necessario che MIM sia online per terminare l'appartenenza temporanea al gruppo.

### <a name="monitoring-of-users-and-groups-in-the-existing-forest"></a>Monitoraggio di utenti e gruppi nella foresta esistente
MIM include anche un servizio di monitoraggio PAM che controlla regolarmente gli utenti e i gruppi nei domini esistenti e aggiorna di conseguenza il database di MIM e Active Directory.  Per l'attivazione dei ruoli o durante la gestione delle risorse non è necessario che questo servizio sia online.

Per il monitoraggio è necessario che i controller di dominio della foresta esistente siano online, così come i componenti MIM e Active Directory dell'ambiente bastion.  

## <a name="deployment-options"></a>Opzioni di distribuzione
L'articolo [Environment overview](environment-overview.md) (Panoramica dell'ambiente) descrive una topologia di base adatta alla comprensione della tecnologia non finalizzata alla disponibilità elevata. Questa sezione illustra in che modo è possibile espandere tale topologia per assicurare disponibilità elevata sia alle organizzazioni con un solo sito che a quelle con più siti esistenti.

### <a name="networking"></a>Funzionalità di rete

Il traffico di rete tra i computer presenti nell'ambiente bastion deve essere separato dalle reti esistenti, ad esempio mediante l'uso di una rete fisica o virtuale differente.  A seconda dei rischi per l'ambiente bastion, può anche essere necessario disporre di interconnessioni fisiche indipendenti tra i computer.  Determinate tecnologie di cluster di failover impongono requisiti aggiuntivi per le interfacce di rete.

I computer che ospitano Servizi di dominio Active Directory e quelli che ospitano i servizi MIM nell'ambiente bastion devono disporre di connettività bidirezionale nella foresta esistente per le finalità seguenti:
- autenticazione degli utenti da parte dei controller di dominio della foresta PRIV
- richiesta di attivazione da parte degli utenti
- disponibilità per gli utenti di ticket Kerberos utilizzabili da parte di risorse presenti nella foresta esistente
- monitoraggio dei domini della foresta esistente da parte di MIM
- invio di messaggi di posta elettronica da parte di MIM tramite server di posta elettronica presenti nella foresta esistente

### <a name="minimal-high-availability-topologies"></a>Topologie minime a disponibilità elevata
Un'organizzazione può selezionare le funzioni dell'ambiente bastion che richiedono disponibilità elevata, con i vincoli seguenti:

- Per la disponibilità elevata di qualsiasi funzione fornita dall'ambiente bastion sono necessari almeno due controller di dominio.  
- La disponibilità elevata per le richieste di attivazione richiede almeno due computer che ospitano il servizio MIM. Richiede inoltre la disponibilità elevata per SQL Server.
- Per la disponibilità elevata di SQL Server con cluster di failover sono necessari almeno due server che forniscono SQL Server. Tali server non possono coincidere con un controller di dominio.
- Il servizio MIM non deve essere installato nel controller di dominio, in modo da ridurre al minimo la superficie di attacco di ciascun server.

La più piccola topologia di disponibilità elevata per tutte le funzioni di un ambiente bastion è costituita da almeno quattro server e da uno spazio di archiviazione condiviso. Due dei server devono essere configurati come controller di dominio e fornire Servizi di dominio Active Directory. Gli altri due server possono essere configurati come cluster di failover e fornire SQL Server e il servizio MIM.

Inoltre, una tipica distribuzione dell'ambiente bastion deve includere anche una workstation di amministrazione con privilegi per la gestione dei server, nonché un componente di monitoraggio.

Il diagramma seguente mostra un'architettura possibile:

![Topologia bastion - Diagramma](media/bastion1.png)

È possibile configurare server aggiuntivi per ciascuna di queste funzioni allo scopo di assicurare prestazioni migliori in condizioni di carico elevato o per garantire ridondanza geografica, come descritto di seguito.

### <a name="deployments-supporting-multiple-sites"></a>Distribuzioni che supportano più siti
La scelta della topologia corretta per risorse distribuite su più siti dipende dai tre fattori seguenti:  
- Obiettivi e rischi per la disponibilità elevata e il ripristino di emergenza  
- Funzionalità hardware per l'hosting dell'ambiente bastion  
- Modello delle attività di amministrazione per ciascun sito

Uno degli approcci più semplici consiste nell'ospitare l'ambiente bastion in un sito particolare.  In condizioni normali, gli utenti possono connettersi alla distribuzione MIM nell'ambiente bastion di tale sito e richiedere l'attivazione. L'attivazione ha quindi effetto sulle risorse di ciascun sito.  Se il collegamento di rete viene interrotto o se il sito che ospita l'ambiente bastion non è disponibile, è possibile accedere alle credenziali offline in un altro sito, in modo da eseguire un'amministrazione temporanea fino alla riconnessione della rete.  Questo approccio può essere adatto alle situazioni in cui si prevede che le operazioni di amministrazione locale di un particolare sito, ad esempio una succursale, siano poco frequenti e limitate alla riconnessione di tale sito al resto della rete dell'organizzazione.

![Bastion singolo per topologia multisito - Diagramma](media/bastion2.png)

Per la disponibilità elevata e il ripristino di emergenza tra siti è possibile anche distribuire i componenti dell'ambiente bastion in ciascun sito, condividendo una directory PRIV e un database SQL comuni.  In questa topologia, se si verifica un'interruzione del collegamento di rete, gli utenti di ciascun sito possono continuare a operare in modo indipendente.

![Bastion multiplo per topologia multisito - Diagramma](media/bastion3.png)

Un vincolo di questa distribuzione è costituito dal fatto che SQL Server richiede un cluster che si estende su entrambi i siti, configurazione difficile da distribuire. In una situazione di questo tipo, un'alternativa possibile consiste nel replicare soltanto l'istanza di Active Directory (foresta PRIV) dell'ambiente bastion.  Se si verifica un'interruzione della rete tra i siti, gli utenti del sito B che in precedenza avevano già attivato i propri ruoli con privilegi potranno continuare a operare per amministrare le risorse nel sito B.

![Bastion replicato per topologia multisito - Diagramma](media/bastion4.png)

Se ciascun sito rappresenta un limite amministrativo separato, è possibile anche distribuire più ambienti bastion indipendenti.  Anche se tutti gli ambienti bastion avranno lo stesso software, i relativi nomi di dominio saranno differenti e non sarà presente alcuna caratteristica comune tra le directory e i database di ciascun ambiente. Se un utente desidera gestire risorse in un particolare sito, deve attivare un account utente nell'ambiente bastion di tale sito.

![Bastion indipendenti per topologia multisito - Diagramma](media/bastion5.png)

È infine possibile realizzare più distribuzioni complesse, poiché è possibile configurare più ambienti bastion in modo indipendente per gestire le risorse in un particolare dominio.

![Bastion complesso per topologia multisito - Diagramma](media/bastion6.png)

### <a name="hosted-bastion-environment"></a>Ambiente bastion ospitato
Alcune organizzazioni hanno anche preso in considerazione la possibilità di creare l'ambiente bastion tenendolo separato dai loro siti esistenti. Il software dell'ambiente bastion può essere ospitato su una piattaforma di virtualizzazione all'interno delle reti dell'organizzazione oppure su un provider di hosting esterno.  Quando si valuta questo approccio, tenere presente quanto segue:

- Per assicurare la protezione da attacchi provenienti da domini esistenti, l'amministrazione dell'ambiente bastion deve essere isolata dagli account amministrativi del dominio esistente.
- L'ambiente bastion richiede la connettività TCP/IP ai controller di dominio nel dominio esistente.  Nell'articolo [Come configurare un firewall per domini e trust](https://support.microsoft.com/kb/179442) è riportato un elenco di porte.
- Per una distribuzione virtualizzata di Servizi di dominio Active Directory sono necessarie funzionalità specifiche della piattaforma di virtualizzazione, come descritto nell'articolo [Distribuzione e configurazione di controller di dominio virtualizzati](https://technet.microsoft.com/library/jj574223.aspx).
- Per una distribuzione a disponibilità elevata di SQL Server per il servizio MIM, è necessaria una speciale configurazione di archiviazione, come descritto nella sezione [Archiviazione del database SQL Server](#sql-server-database-storage) riportata di seguito.  Non tutti i provider di hosting sono attualmente in grado di offrire hosting di Windows Server con configurazioni del disco adatte ai cluster di failover di SQL Server.

## <a name="deployment-preparation-and-recovery-procedures"></a>Preparazione della distribuzione e procedure di ripristino
La preparazione di una distribuzione dell'ambiente bastion a disponibilità elevata o pronta per il ripristino di emergenza richiede una riflessione sulla modalità di installazione di Windows Server Active Directory, di SQL Server e del relativo database nello spazio di archiviazione condiviso, nonché del servizio MIM e dei relativi componenti PAM.

### <a name="windows-server"></a>Windows Server
In Windows Server è presente una funzionalità incorporata per la disponibilità elevata che consente a più computer di operare insieme come un cluster di failover. I server del cluster sono connessi mediante cavi fisici e software. Se in uno o più nodi del cluster si verifica un errore, il servizio verrà garantito dagli altri nodi (un processo noto come failover).   Per maggiori dettagli, vedere [Panoramica di Clustering di failover](https://technet.microsoft.com/library/hh831579.aspx).

Verificare che il sistema operativo e le applicazioni dell'ambiente bastion ricevano gli aggiornamenti relativi ai problemi di sicurezza. Alcuni di questi aggiornamenti possono richiedere un riavvio del server. È quindi consigliabile coordinare gli orari di applicazione degli aggiornamenti ai diversi server in modo da evitare interruzioni estese. Un approccio consiste nell'usare [Aggiornamento compatibile con cluster](https://technet.microsoft.com/library/hh831694.aspx) per i server di un cluster di failover Windows Server.

I server dell'ambiente bastion verranno aggiunti a un dominio e dipenderanno dai servizi del dominio stesso. Assicurarsi di non configurare inavvertitamente tali server con una dipendenza da un particolare controller di dominio per servizi quali DNS.

### <a name="bastion-environment-active-directory"></a>Active Directory nell'ambiente bastion
Servizi di dominio Active Directory di Windows Server include in modo nativo il supporto per la disponibilità elevata e il ripristino di emergenza.

#### <a name="preparation"></a>Preparazione
Una tipica distribuzione di produzione per la gestione degli accessi con privilegi include almeno due controller di dominio nell'ambiente bastion. Le istruzioni per l'impostazione del primo controller di dominio nell'ambiente bastion sono incluse nell'articolo sulla distribuzione [Passaggio 2: preparare il primo controller di dominio PRIV](step-2-prepare-priv-domain-controller.md).

La procedura per l'aggiunta di un altro controller di dominio è riportata nell'articolo [Installare un Controller di dominio Windows Server 2012 Replica in un dominio esistente (livello 200)](https://technet.microsoft.com/library/jj574134.aspx).  

>[!NOTE]
> Se il controller di dominio deve essere ospitato in una piattaforma di virtualizzazione come Hyper-V, vedere le avvertenze riportate nell'articolo [Distribuzione e configurazione di controller di dominio virtualizzati](https://technet.microsoft.com/library/jj574223.aspx).

#### <a name="recovery"></a>Ripristino
Dopo un'interruzione, prima di riavviare altri server verificare che nell'ambiente bastion sia disponibile almeno un controller di dominio.

All'interno di un dominio, Active Directory distribuisce i ruoli FSMO (Flexible Single Master Operation) tra i controller di dominio, come descritto in [How Operations Masters Work](https://technet.microsoft.com/library/cc780487.aspx) (Come funzionano i master operazioni).  Se si verifica un errore in un controller di dominio, può essere necessario trasferire uno o più dei [ruoli di controller di dominio](https://technet.microsoft.com/library/cc786438.aspx) che erano stati assegnati al controller in questione.

Dopo aver stabilito che un controller di dominio non verrà reinserito in produzione, verificare se a tale controller erano stati assegnati dei ruoli e provvedere a riassegnarli nel modo opportuno. Le istruzioni sono disponibili in [View the Current Operations Master Role Holders](https://technet.microsoft.com/library/cc816893.aspx) (Visualizzare i titolari dei ruoli master operazioni) e negli articoli correlati.

È inoltre consigliabile controllare le impostazioni DNS dei computer aggiunti all'ambiente bastion, nonché i controller di dominio dei domini CORP che hanno una relazione di trust con il controller di dominio. In questo modo si assicura che nessuno di essi venga codificato con una dipendenza dall'indirizzo IP del computer di tale controller di dominio.

### <a name="sql-server-database-storage"></a>Archiviazione del database di SQL Server
Una distribuzione a disponibilità elevata richiede che i cluster di failover di SQL Server e le istanze di tali cluster si basino sull'archiviazione condivisa tra tutti i nodi per l'archiviazione di database e log. L'archiviazione condivisa può essere sotto forma di dischi cluster Windows Server Failover Clustering, dischi in una rete SAN (Storage Area Network) o condivisioni di file in un server SMB.  Si noti che queste soluzioni devono essere dedicate all'ambiente bastion. L'archiviazione condivisa all'esterno dell'ambiente bastion non è consigliata perché potrebbe compromettere l'integrità dell'ambiente bastion stesso.

### <a name="sql-server"></a>SQL Server
Per il servizio MIM è necessaria una distribuzione SQL Server nell'ambiente bastion.   Per quanto riguarda la disponibilità elevata, è possibile distribuire SQL usando un'istanza del cluster di failover. A differenza delle istanze autonome, nelle istanze del cluster di failover la disponibilità elevata di SQL Server è protetta dalla presenza di nodi ridondanti. Se si verifica un errore o un aggiornamento pianificato, la proprietà del gruppo di risorse passa a un altro nodo Windows Server Failover Clustering.

Se è necessario il supporto solo per la disponibilità elevata e non per il ripristino di emergenza, al posto del clustering di failover è possibile usare il log shipping, la replica di tipo transazionale, la replica snapshot o il mirroring del database.   

#### <a name="preparation"></a>Preparazione
Quando si installa SQL Server nell'ambiente bastion, questo deve essere indipendente da qualsiasi istanza di SQL Server già presente nella foresta CORP.  È inoltre consigliabile che SQL Server venga distribuito in un server dedicato, diverso da quello del controller di dominio.
Altre informazioni sono disponibili nell'articolo [Istanze del cluster di failover AlwaysOn (SQL Server)](https://msdn.microsoft.com/library/ms189134.aspx).

#### <a name="recovery"></a>Ripristino
Se SQL Server è stato configurato per il ripristino di emergenza usando il log shipping, è necessario aggiornare SQL Server durante il ripristino.  È inoltre necessario riavviare ciascuna istanza del servizio MIM.

Se si è verificato un errore in SQL Server o se la connettività tra SQL Server e il servizio MIM è andata persa, dopo il ripristino di SQL Server è consigliabile riavviare ciascuna istanza del servizio MIM.  In questo modo, si assicura che il servizio MIM ristabilirà la connessione a SQL Server.

### <a name="mim-service"></a>Servizio MIM
Il servizio MIM è necessario per elaborare le richieste di attivazione.  Per consentire il fermo per manutenzione di un computer che ospita il servizio MIM anche mentre vengono ricevute richieste di attivazione, è possibile distribuire più computer del servizio MIM.  Si noti che, dopo l'aggiunta di un utente a un gruppo, il servizio MIM non è coinvolto nelle operazioni Kerberos.  

#### <a name="preparation"></a>Preparazione
È consigliabile distribuire il servizio MIM su più computer aggiunti al dominio PRIV.
Per quanto riguarda la disponibilità elevata, vedere i documenti Windows Server [Requisiti hardware per il clustering di failover e opzioni di archiviazione](https://technet.microsoft.com/library/jj612869.aspx) e [Creating a Windows Server 2012 Failover Cluster](http://blogs.msdn.com/b/clustering/archive/2012/05/01/10299698.aspx) (Creazione di un cluster di failover Windows Server 2012).

Per la distribuzione della produzione su più server, è possibile usare la funzionalità Bilanciamento carico di rete per distribuire il carico di elaborazione.  È inoltre necessario avere un solo alias, ad esempio record A o CNAME, in modo che all'utente venga esposto un nome comune.

>[!IMPORTANT]
> Se si usa una tecnologia di bilanciamento del carico diversa dalla funzionalità Bilanciamento carico di rete di Windows Server 2012 R2, verificare che la soluzione reindirizzi una sessione allo stesso server e non a un server casuale.

In una distribuzione MIM multiserver ciascun servizio MIM dispone di un nome host esterno, un nome di servizio e un nome di partizione del servizio.  Il valore predefinito del nome del servizio è il nome del computer, mentre il valore predefinito del nome host esterno e della partizione del servizio vengono configurati durante l'installazione del servizio MIM, nella schermata in cui viene richiesto l'indirizzo del server del servizio MIM. Questi tre nomi vengono archiviati nel file %Programmi%\Microsoft Forefront Identity Manager\Service\Microsoft.ResourceManagementService.exe.config, come attributi `externalHostName`, `serviceName` e `servicePartitionName` del nodo di configurazione `resourceManagementService`.  

Quando un servizio MIM riceve una richiesta, il nome della partizione del servizio viene archiviato come attributo di tale richiesta.   Di conseguenza, possono interagire con la richiesta in questione solo altre installazioni del servizio MIM con lo stesso nome di partizione del servizio.  Pertanto, se lo scenario PAM include approvazioni manuali o altre elaborazioni di richieste di lunga durata, verificare che ciascun servizio MIM abbia lo stesso attributo `servicePartitionName` nel file di configurazione.

#### <a name="recovery"></a>Ripristino
Dopo un'interruzione, prima di riavviare il servizio MIM, verificare che nell'ambiente bastion siano disponibili SQL Server e almeno un controller di dominio Active Directory.  

Un'istanza del flusso di lavoro può essere completata solo da un server del servizio MIM con gli stessi nomi di servizio e di partizione del servizio del server del servizio MIM che l'ha avviata.  Se si verifica un errore in un particolare computer durante l'hosting di un servizio MIM che stava elaborando richieste, e si decide di non riattivare tale computer, sarà necessario installare il servizio MIM in un nuovo computer. Dopo l'installazione del nuovo servizio MIM, modificare il file *resourcemanagementservice.exe.config* e impostare gli attributi `serviceName` e `servicePartitionName` della nuova distribuzione MIM in modo che corrispondano al nome host e al nome della partizione del servizio del computer in cui si era verificato l'errore.

### <a name="mim-pam-components"></a>Componenti MIM PAM
Il programma di installazione del portale e del servizio MIM comprende anche componenti PAM aggiuntivi, inclusi moduli PowerShell e due servizi.

#### <a name="preparation"></a>Preparazione
È necessario installare i componenti PAM in ciascun computer dell'ambiente bastion in cui è in corso l'installazione del servizio MIM.  Tali componenti non possono essere aggiunti in un secondo momento.

#### <a name="recovery"></a>Ripristino
Dopo il ripristino a seguito di un'interruzione, verificare che il servizio MIM sia in esecuzione su almeno un server.  Verificare quindi che anche il servizio di monitoraggio PAM MIM sia in esecuzione su tale server usando `net start "PAM Monitoring service"`.

Se il livello funzionale della foresta dell'ambiente bastion è Windows Server 2012 R2, verificare che anche il servizio del componente PAM MIM sia in esecuzione sul server usando il comando `net start "PAM Component service"`.

