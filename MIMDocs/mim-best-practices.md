---
title: Procedure consigliate per Microsoft Identity Manager 2016 | Microsoft Docs
description: 
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 11/15/2017
ms.topic: reference
ms.prod: identity-manager-2016
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 
ms.openlocfilehash: 7f56882bf005de6c888997c1bf6a9e2feaea410c
ms.sourcegitcommit: 42253562ac2f9ed689e9db9d0c470213b7926883
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/16/2017
---
# <a name="microsoft-identity-manager-2016-best-practices"></a>Procedure consigliate per Microsoft Identity Manager 2016

Questo argomento descrive le procedure consigliate per la distribuzione e il funzionamento di Microsoft Identity Manager 2016 (MIM)

## <a name="sql-setup"></a>Configurazione di SQL
>[!NOTE]
I consigli seguenti per la configurazione di un server che esegue SQL presumono la presenza di un'istanza SQL dedicata al database del servizio FIMService e di un'istanza SQL dedicata al database del servizio FIMSynchronizationService. Se si esegue FIMService in un ambiente consolidato, è necessario apportare modifiche adeguate per la configurazione in uso.

La configurazione del server SQL (Structured Query Language) è fondamentale per ottenere le prestazioni ottimali del sistema. La possibilità di ottenere prestazioni ottimali da un'implementazione MIM su larga scala dipende dall'applicazione delle procedure consigliate per i server che eseguono SQL. Per altre informazioni, vedere gli argomenti seguenti sulle procedure consigliate per SQL:

-   [Le dieci procedure di archiviazione migliori](http://go.microsoft.com/fwlink/?LinkID=183663)

-   [Ottimizzazione delle prestazioni di tempdb](http://go.microsoft.com/fwlink/?LinkID=188267)

-   [SQL Server Best Practices Article](http://go.microsoft.com/fwlink/?LinkID=188268) (Articolo sulle procedure consigliate per SQL Server)

-   [Reorganize and Rebuild Indexes](http://go.microsoft.com/fwlink/?LinkID=188269) (Riorganizzare e ricompilare gli indici)

### <a name="presize-data-and-log-files"></a>Definire in anticipo le dimensioni dei file di dati e di log

Non fare affidamento sulla funzione di aumento automatico delle dimensioni. Preferire invece la gestione manuale della crescita di questi file. È possibile lasciare attiva la funzione di aumento automatico delle dimensioni per motivi di sicurezza, ma l'aumento delle dimensioni dei file di dati deve essere gestito in modo proattivo. Per esempi di dimensioni del database MIM, vedere [FIM Capacity Planning Guide](http://go.microsoft.com/fwlink/?LinkID=185246) (Guida alla pianificazione della capacità di FIM).

### <a name="to-presize-sql-data-and-log-files"></a>Per definire in anticipo le dimensioni dei file di dati e di log SQL

1.  Avviare SQL Server Management Studio.

2.  Passare al database di FIMService, fare clic con il pulsante destro del mouse su FIMService e quindi scegliere Proprietà.

3.  Nella pagina File espandere i file di database fino alla dimensione necessaria.

### <a name="isolate-log-from-data-files"></a>Isolare i file di log dai file di dati

Seguire le procedure consigliate per il server SQL per isolare i file di log dai file delle transazioni, mantenendoli su dischi fisici separati.

Creare file tempdb aggiuntivi

Per ottenere prestazioni ottimali, è consigliabile creare un file di dati per core CPU nel file tempdb.

### <a name="to-create-additional-tempdb-files"></a>Per creare file tempdb aggiuntivi

1.  Avviare SQL Server Management Studio.

2.  Individuare il file tempdb del database in Database di sistema, fare clic con il pulsante destro del mouse su tempdb e quindi scegliere Proprietà.

3.  Nella pagina File creare un file di dati per ogni core CPU. Assicurarsi di separare i file dati e i file di log di tempdb, salvandoli in unità e spindle diversi.

### <a name="ensure-adequate-space-for-log-files"></a>Garantire ai file di log una quantità di spazio adeguata

È importante comprendere i requisiti del disco per il modello di recupero. La modalità di recupero con registrazione minima può essere appropriata durante il caricamento iniziale del sistema per limitare l'uso di spazio su disco, ma espone al rischio di perdita i dati creati dopo il backup più recente. Se si usa la modalità di recupero con registrazione completa, è necessario gestire l'utilizzo del disco tramite backup. Per evitare un utilizzo elevato dello spazio su disco, sono necessari backup frequenti del log delle transazioni. Per altre informazioni, vedere [Recovery Model Overview](http://go.microsoft.com/fwlink/?LinkID=185370) (Panoramica dei modelli di recupero).

### <a name="limit-sql-server-memory"></a>Limitare la quantità di memoria usata dal server SQL

A seconda della quantità di memoria disponibile nel server SQL e della condivisione o meno di questo con altri servizi (ad esempio MIM 2016 e il relativo servizio di sincronizzazione), è necessario limitare il consumo di memoria da parte di SQL. Questa operazione è possibile tramite la procedura seguente.

1.  Avviare SQL Server Enterprise Manager.

2.  Selezionare Nuova query.

3.  Eseguire la query seguente:

  ```SQL
  USE master

  EXEC sp_configure 'show advanced options', 1

  RECONFIGURE WITH OVERRIDE

  USE master

  EXEC sp_configure 'max server memory (MB)', 12000--- max=12G RECONFIGURE
  WITH OVERRIDE
  ```

  Questo esempio riconfigura il server SQL in modo che usi non più di 12 GB di memoria.

4.  Verificare l'impostazione tramite la query seguente:

  ```SQL
  USE master

  EXEC sp_configure 'max server memory (MB)'--- verify the setting

  USE master

  EXEC sp_configure 'show advanced options', 0

  RECONFIGURE WITH OVERRIDE
  ```

### <a name="backup-and-recovery-configuration"></a>Configurazione di backup e ripristino

In generale, è consigliabile collaborare con l'amministratore del database per la progettazione di una strategia di backup e ripristino. Ecco alcune raccomandazioni:
- Eseguire i backup dei database secondo i criteri di backup dell'organizzazione. 
- Se non sono previsti backup incrementali del file di log, è consigliabile impostare il database sulla modalità di recupero con registrazione minima. 
- Prima di implementare la strategia di backup, assicurarsi di aver compreso le implicazioni dei diversi modelli di recupero. Raccogliere informazioni sui requisiti relativi allo spazio su disco per questi modelli. Il modello di recupero con registrazione completa richiede backup del log frequenti per evitare un utilizzo elevato dello spazio su disco. 

Per altre informazioni, vedere [Recovery Model Overview](http://go.microsoft.com/fwlink/?LinkID=185370) (Panoramica dei modelli di recupero) e [FIM 2010 Backup and Restore Guide](http://go.microsoft.com/fwlink/?LinkID=165864) (Guida al backup e al ripristino di FIM 2010).

## <a name="create-a-backup-administrator-account-for-the-fim-service-after-installation"></a>Creare un account amministratore del backup per il servizio FIM dopo l'installazione

I membri del gruppo Administrators di FIMService dispongono in esclusiva di autorizzazioni cruciali per il funzionamento della distribuzione MIM. Se un membro del gruppo Administrators non riesce ad accedere, l'unica soluzione è ripristinare un backup precedente del sistema. Per attenuare la gravità di questa situazione, è consigliabile aggiungere altri utenti al gruppo degli amministratori FIM durante la configurazione successiva all'installazione.

## <a name="fim-service"></a>Servizio FIM


### <a name="configuring-fim-service-service-exchange-mailbox"></a>Configurazione delle cassette postali di Exchange del servizio FIM

Di seguito sono descritte le procedure consigliate per la configurazione di Microsoft Exchange Server per l'account del servizio MIM 2016.

- Configurare l'account del servizio in modo che possa accettare messaggi solo da indirizzi di posta elettronica interni. In particolare, la cassetta postale dell'account del servizio non deve mai essere in grado di ricevere posta da server SMTP esterni.

#### <a name="to-configure-the-service-account"></a>Per configurare l'account del servizio

1.  In Exchange Management Console selezionare l'**account del servizio FIM**.

2.  Selezionare Proprietà, Impostazioni flusso di posta e quindi **Mail Delivery Restrictions** (Restrizioni recapito posta).

3.  Selezionare la casella di controllo **Richiedi l'autenticazione di tutti i mittenti**.

Per altre informazioni, vedere [Configurazione di restrizioni di recapito messaggi per una cassetta postale](http://go.microsoft.com/fwlink/?LinkID=183625).

-   Configurare l'account del servizio in modo che rifiuti i messaggi con dimensioni maggiori di 1 MB. Seguire la procedura consigliata per le cassette postali o per le cartelle pubbliche abilitate per la posta in [Configure message size limits for a mailbox](http://go.microsoft.com/fwlink/?LinkID=183626) (Configurare limiti per le dimensioni dei messaggi per una cassetta postale).

-   Configurare l'account del servizio in modo che la quota di archiviazione delle cassette postali corrisponda a 5 GB. Per ottenere risultati ottimali, seguire le procedure consigliate descritte in [Configure storage quotas for a mailbox](http://go.microsoft.com/fwlink/?LinkID=156929) (Configurare quote di archiviazione per le cassette postali).

## <a name="mim-portal"></a>Portale MIM


### <a name="disable-sharepoint-indexing"></a>Disabilitare l'indicizzazione di SharePoint

È consigliabile disabilitare l'indicizzazione di Microsoft Office SharePoint®. Non sono disponibili documenti che devono essere indicizzati. L'indicizzazione comporta la creazione di molte voci nel log degli errori e potenziali problemi di prestazioni in MIM. Per disabilitare l'indicizzazione di SharePoint eseguire i passaggi seguenti:

1.  Nel server che ospita il portale MIM 2016 fare clic sul pulsante Start.

2.  Fare clic su Tutti i programmi.

3.  Nell'elenco Tutti i programmi fare clic su Strumenti di amministrazione.

4.  In Strumenti di amministrazione fare clic su Amministrazione centrale SharePoint.

5.  Nella pagina Amministrazione centrale fare clic su Operazioni.

6.  Nella sezione Configurazione globale della pagina Operazioni fare clic su Definizioni processi timer.

7.  Nella pagina Definizioni processi timer fare clic su Aggiornamento servizio di ricerca di Windows SharePoint Services.

8.  Dalla pagina Modifica processo timer, fare clic su Disattiva.

## <a name="mim-2016-initial-data-load"></a>Caricamento iniziale dei dati in MIM 2016

Questa sezione elenca una serie di passaggi che consentono di migliorare le prestazioni del caricamento iniziale dei dati dal sistema esterno a MIM. È importante comprendere che alcuni di questi passaggi vengono eseguiti solo durante il popolamento iniziale del sistema. Devono quindi essere reimpostati a caricamento completato. Questa operazione viene eseguita una sola volta e non è una sincronizzazione continua.

>[!NOTE]
Per altre informazioni sulla sincronizzazione degli utenti tra MIM e Active Directory Domain Services (AD DS), vedere [How do I Synchronize Users from Active Directory to FIM](http://go.microsoft.com/fwlink/?LinkID=188277) (Come posso eseguire la sincronizzazione degli utenti tra Active Directory e FIM) nella documentazione di FIM.

>[!IMPORTANT]
Assicurarsi di aver applicato le procedure consigliate descritte nella sezione Configurazione di SQL di questa guida. 

### <a name="step-1-configure-the-sql-server-for-initial-data-load"></a>Passaggio 1: Configurare il server SQL per il caricamento iniziale dei dati
Il caricamento iniziale dei dati può essere un processo lungo. Quando si intende caricare inizialmente una grande quantità di dati, è possibile ridurre il tempo necessario a popolare il database disattivando temporaneamente la ricerca full-text e attivandola nuovamente dopo il completamento dell'esportazione nell'agente di gestione MIM 2016 (FIM MA).

Per disattivare temporaneamente la ricerca full-text:

1.  Avviare SQL Server Management Studio.

2.  Selezionare Nuova query.

3.  Eseguire le istruzioni SQL seguenti:

```SQL
ALTER FULLTEXT INDEX ON [fim].[ObjectValueString] SET CHANGE_TRACKING = MANUAL
ALTER FULLTEXT INDEX ON [fim].[ObjectValueXml] SET CHANGE_TRACKING = MANUAL
```

>[!IMPORTANT]
La mancata implementazione di queste procedure causa un utilizzo molto elevato dello spazio su disco, che potrebbe esaurirsi. Altri dettagli su questo argomento sono disponibili in [Recovery Model Overview](http://go.microsoft.com/fwlink/?LinkID=185370) (Panoramica dei modelli di recupero). [FIM Backup and Restore Guide](http://go.microsoft.com/fwlink/?LinkID=165864) (Guida al backup e al ripristino di FIM) contiene informazioni aggiuntive.

### <a name="step-2-apply-the-minimum-necessary-mim-configuration-during-the-load-process"></a>Passaggio 2: Applicare la configurazione MIM minima necessaria durante il processo di caricamento

Durante il processo di caricamento iniziale, è consigliabile applicare a FIM solo la configurazione minima necessaria per le regole di criteri di gestione e per le definizioni di set. Al termine del caricamento dei dati, creare i set aggiuntivi necessari per la distribuzione. Usare l'impostazione Esegui in base all'aggiornamento criteri per i flussi di lavoro azione per applicare tali criteri in modo retroattivo ai dati caricati.

### <a name="step-3-configure-and-populate-the-fim-service-with-external-identity-data"></a>Passaggio 3: Configurare e popolare il servizio FIM con dati di identità esterni

A questo punto è necessario seguire le procedure descritte nella guida How Do I Synchronize Users from Active Directory Domain Services to FIM (Come eseguire la sincronizzazione degli utenti tra Active Directory Domain Services e FIM) per configurare e sincronizzare il sistema con gli utenti provenienti da Active Directory. Se è necessario sincronizzare le informazioni sui gruppi, le procedure necessarie per questo processo sono descritte nella guida [How Do I Synchronize Groups from Active Directory Domain Services to FIM](https://technet.microsoft.com/library/ff686936(v=ws.10).aspx) (Come eseguire la sincronizzazione dei gruppi tra Active Directory Domain Services e FIM).

#### <a name="synchronization-and-export-sequences"></a>Sequenze di sincronizzazione ed esportazione

Per ottimizzare le prestazioni, eseguire un'esportazione dopo una sincronizzazione che abbia dato come risultato un numero elevato di operazioni di esportazione in sospeso in uno spazio connettore. Eseguire quindi un'importazione di conferma per l'agente di gestione associato allo spazio connettore interessato. Se, ad esempio, nell'ambito del caricamento iniziale dei dati è necessario usare profili di esecuzione della sincronizzazione per diversi agenti di gestione, dopo l'esecuzione di ogni sincronizzazione è consigliabile eseguire un'esportazione seguita da un'importazione delta.
Per ogni agente di gestione di origine compreso nel ciclo di inizializzazione eseguire i passaggi seguenti:

1.  Importazione completa in un agente di gestione di origine.

2.  Sincronizzazione completa dell'agente di gestione di origine.

3.  Esportazione in tutti gli agenti di gestione di destinazione interessati con operazioni di esportazione temporanea.

4.  Importazione delta in tutti gli agenti di gestione di destinazione interessati con operazioni di esportazione temporanea.

### <a name="step-4-apply-your-full-mim-configuration"></a>Passaggio 4: Applicare la configurazione MIM completa

Al termine del caricamento iniziale dei dati, è consigliabile applicare la configurazione MIM completa per la distribuzione.

A seconda dello scenario, può essere necessaria la creazione di set, regole di criteri di gestione e flussi di lavoro aggiuntivi. Per tutti i criteri da applicare in modo retroattivo a tutti gli oggetti esistenti nel sistema, usare l'impostazione Esegui in base all'aggiornamento criteri per i flussi di lavoro azione per applicare tali criteri in modo retroattivo ai dati caricati.

### <a name="step-5-reconfigure-sql-to-previous-settings"></a>Passaggio 5: Ripristinare le impostazioni precedenti di SQL

Ricordarsi di ripristinare le impostazioni di SQL normali. Sono inclusi:

-   Attivazione della ricerca full-text

-   Aggiornamento dei criteri di backup in base ai criteri dell'organizzazione

Dopo aver completato il caricamento iniziale dei dati, è necessario riattivare la ricerca full-text. Per riattivare nuovamente la ricerca full-text, eseguire le istruzioni SQL seguenti:

```SQL
ALTER FULLTEXT INDEX ON [fim].[ObjectValueString] SET CHANGE_TRACKING = AUTO

ALTER FULLTEXT INDEX ON [fim].[ObjectValueXml] SET CHANGE_TRACKING = AUTO
```

Se è stato necessario passare alla modalità di recupero con registrazione minima, assicurarsi di riconfigurare la pianificazione del backup secondo i criteri di backup dell'organizzazione. Dettagli aggiuntivi sulle pianificazioni del backup di FIM sono disponibili in [FIM Backup and Restore Guide](http://go.microsoft.com/fwlink/?LinkID=165864) (Guida al backup e al ripristino di FIM).

## <a name="configuration-migration"></a>Migrazione della configurazione


### <a name="avoid-changing-display-names"></a>Evitare di modificare i nomi visualizzati

Per molti tipi di oggetto, ad esempio le regole di criteri di gestione, lo script syncproduction.ps1 usa il nome visualizzato come unico attributo di ancoraggio tra due sistemi. Di conseguenza, la modifica del nome visualizzato di una regola di criteri di gestione comporta l'eliminazione della regola esistente e la creazione di una nuova regola. Questo risultato è dovuto al fatto che il processo di migrazione non è in grado di eseguire il join di regole di criteri di gestione i cui criteri di join sono stati modificati. Per evitare questo problema, è possibile associare un attributo personalizzato a tutti i tipi di oggetti di configurazione e usare tale attributo come criterio di join. Ciò consente di modificare i nomi visualizzati senza influire sul processo di migrazione.

### <a name="avoid-changing-the-content-of-intermediate-files"></a>Evitare di modificare il contenuto di file intermedi

Anche se il formato di file e l'interfaccia API (Application Programming Interface) degli oggetti di basso livello sono pubblici e la manipolazione è supportata dagli sviluppatori, non è consigliabile modificare il contenuto dei formati intermedi durante la migrazione. Potrebbe tuttavia essere necessario rimuovere completamente ImportObjects da changes.xml o eseguire operazioni di ricerca e sostituzione in pilot.xml per sostituire i numeri di versione o le informazioni DNS (Domain Name System) pilota con le informazioni DNS di produzione.

### <a name="ensure-that-the-version-number-is-correct-in-pilotxml-when-migrating-across-versions"></a>Quando si esegue una migrazione tra versioni diverse, assicurarsi che il numero di versione in pilot.xml sia corretto

Le migrazioni tra versioni diverse non sono consigliate né supportate. Spesso, tuttavia, è possibile eseguirle sostituendo il numero di versione pilota con il numero di versione di produzione in pilot.xml. In particolare, gli oggetti WorkflowDefinition e

ActivityInformationConfiguration richiedono il numero di versione per fare riferimento in modo preciso alle attività dei flussi di lavoro nell'ambiente di produzione. Se non si sostituisce il numero di versione, il cmdlet Compare-FIMConfig rileva differenze tra gli attributi XOML (Extensible Object Markup Language) in WorkflowDefinitions ed esegue la migrazione del numero di versione del pilota. Se il numero di versione non è corretto, il servizio FIM di produzione potrebbe non essere in grado di avviare le attività dei flussi di lavoro.

### <a name="avoid-cyclic-references"></a>Evitare riferimenti ciclici

In una configurazione MIM i riferimenti ciclici non sono in genere consigliati. Possono tuttavia verificarsi cicli se il set A fa riferimento al set B e quest'ultimo a sua volta fa riferimento al set A. Per evitare problemi dovuti ai riferimenti ciclici, modificare la definizione del set A o del set B in modo nessuno dei due faccia riferimento all'altro. Riavviare quindi il processo di migrazione. Se sono presenti riferimenti ciclici e il cmdlet Compare-FIMConfig genera un errore, è necessario interrompere il ciclo manualmente. Poiché il cmdlet Compare-FIMConfig restituisce un elenco di modifiche in ordine di precedenza, è necessario che non esistano cicli tra i riferimenti degli oggetti di configurazione.

## <a name="security"></a>Sicurezza

### <a name="mim-ma-account"></a>Account dell'agente di gestione MIM

L'account dell'agente di gestione MIM non è considerato un account del servizio e deve corrispondere a un account utente normale. Perché l'account del servizio di sincronizzazione FIM possa rappresentare l'account, questo deve essere in grado di accedere in locale.

Per consentire all'account dell'agente di gestione MIM di accedere localmente

1.  Fare clic sul pulsante Start, scegliere Strumenti di amministrazione e quindi Criteri di sicurezza locali.

2.  Aprire il nodo Criteri locali e quindi fare clic su Assegnazione diritti utente.

3.  Nel criterio Consenti accesso locale assicurarsi che l'account dell'agente di gestione FIM sia specificato esplicitamente. In caso contrario, aggiungerlo a uno dei gruppi ai quali ha già accesso.

### <a name="fim-synchronization-service-and-fim-services-accounts"></a>Account del servizio di sincronizzazione FIM e del servizio FIM

Per configurare in modo sicuro i server che eseguono i componenti server MIM, è necessario sottoporre a restrizioni gli account dei servizi. Tramite la procedura usata in precedenza per attivare l'account dell'agente di gestione MIM impostare le restrizioni seguenti per gli account del servizio di sincronizzazione FIM e del servizio FIM:

-   Nega accesso come processo batch

-   Nega accesso in locale

-   Nega acceso al computer dalla rete

Gli account dei servizi non devono essere membri del gruppo Administrators locale.

L'account del servizio di sincronizzazione FIM non deve essere un membro dei gruppi di sicurezza usati per controllare l'accesso al servizio stesso, ovvero di gruppi con nome che inizia con FIMSync, ad esempio FIMSyncAdmins.

>[!IMPORTANT]
 Se si selezionano le opzioni che consentono di usare lo stesso account per entrambi i servizi e si mantiene il servizio FIM separato dal servizio di sincronizzazione FIM, non è possibile impostare Nega accesso al computer dalla rete nel server del servizio di sincronizzazione MMS. Se l'accesso è negato, il servizio FIM non è in grado di contattare il servizio di sincronizzazione FIM per modificare la configurazione e gestire le password.

### <a name="password-reset-deployed-to-kiosk-like-computers-should-set-local-security-to-clear-virtual-memory-pagefile"></a>Una reimpostazione della password distribuita a computer di tipo chiosco multimediale deve impostare la sicurezza locale per la cancellazione del file di paging della memoria virtuale

Quando si distribuisce una reimpostazione della password FIM in una workstation usata come chiosco multimediale, è consigliabile attivare l'impostazione dei criteri di sicurezza locale Arresto del sistema; cancella file di paging della memoria virtuale, per assicurarsi che le informazioni riservate nella memoria dei processi non siano disponibili per utenti non autorizzati.

### <a name="implementing-ssl-for-the-fim-portal"></a>Implementazione del protocollo SSL per il portale FIM

Per garantire la sicurezza del traffico tra i client e il server è consigliabile proteggere il server del portale FIM con il protocollo SSL (Secure Socket Layer).

Per implementare il protocollo SSL:

1.  Dal server del portale MIM aprire Gestione IIS.

2.  Fare clic sul nome computer locale.

3.  Fare clic su Certificati del server.

4.  Fare clic su Crea richiesta di certificato.

5.  Nella casella di testo Nome comune immettere il nome del server.

6.  Fare clic su Avanti e quindi di nuovo su Avanti.

7.  Salvare il file in una posizione qualsiasi. Nei passaggi successivi sarà necessario accedere a questa posizione.

8.  Passare a https://nomeserver/certsrv. Sostituire nomeserver con il nome del server che rilascia certificati.

9.  Fare clic su Richiedi nuovo certificato.

10. Fare clic su Submit an Advanced Request (Invia richiesta avanzata).

11. Fare clic su Inviare una richiesta di certificato utilizzando un file CMC o PKCS #10 codificato in base 64.

12. Incollare il contenuto del file salvato durante il passaggio precedente.

13. In Modello di certificato selezionare Server Web.

14. Fare clic su Invia.

15. Salvare il certificato sul desktop.

16. In Gestione IIS fare clic su Completa richiesta di certificato.

17. Indicare a Gestione IIS il certificato appena salvato sul desktop.

18. In Nome descrittivo digitare il nome del server.

19. Fare clic su Siti e quindi selezionare SharePoint - 80.

20. Fare clic su Binding e quindi su Aggiungi.

21. Selezionare https.

22. Come certificato selezionare quello con lo stesso nome del server. Si tratta del certificato appena importato.

23. Fare clic su OK.

24. Rimuovere il binding HTTP.

25. Fare clic su Impostazioni SSL e quindi selezionare Richiedi SSL.

26. Salvare le impostazioni.

27. Fare clic sul pulsante Start, scegliere Strumenti di amministrazione e quindi fare clic su SharePoint 3.0 - Amministrazione centrale.

28. Fare clic su Operazioni e quindi su Mapping di accesso alternativo.

29. Fare clic su http://nomeserver.

30. Modificare http://nomeserver in https://nomeserver e quindi fare clic su OK.

31. Fare clic sul pulsante Start, scegliere Esegui, digitare iisreset e quindi fare clic su OK.

## <a name="performance"></a>Prestazioni

Per una configurazione che garantisca prestazioni ottimali:

-   Applicare le procedure consigliate per la configurazione di SQL come descritto nella sezione Configurazione di SQL in questo documento.

-   Disattivare l'indicizzazione di SharePoint nel sito del portale di MIM. Per altre informazioni, vedere la sezione Disabilitare l'indicizzazione di SharePoint in questo documento.

## <a name="feature-specific-best-practices--i-want-to-remove-this-and-collapse-this-section-and-just-have-the-specific-features-at-header-2-level-versus-3"></a>Procedure consigliate per funzionalità specifiche (rimuovere questo paragrafo, comprimere la sezione e mettere le funzionalità specifiche a livello di intestazione 2 anziché 3)


### <a name="request-management"></a>Gestione richieste

Per impostazione predefinita, MIM 2016 ripulisce ogni 30 giorni gli oggetti di sistema scaduti, comprese le richieste completate con le approvazioni associate, le risposte di approvazione e le istanze del flusso di lavoro. Se per l'organizzazione è necessario mantenere le richieste per un periodo più lungo di 30 giorni, è consigliabile esportare le richieste da MIM e archiviarle in un database ausiliario. L'intervallo di 30 giorni per l'eliminazione delle richieste è configurabile, ma una sua estensione può influire negativamente sulle prestazioni a causa del numero maggiore di oggetti nel sistema.

### <a name="management-policy-rules"></a>Regole di criteri di gestione

#### <a name="use-the-appropriate-mpr-type"></a>Usare il metodo delle regole di criteri di gestione appropriato

In MIM sono disponibili due tipi di regole di criteri di gestione, il tipo Richiesta e il tipo Set di transizione:

-  Richiesta

  - Tipo usato per definire i criteri di controllo di accesso (autenticazione, autorizzazione e azione) per le operazioni di creazione, lettura, aggiornamento o eliminazione (CRUD) sulle risorse.
  - Applicato quando viene eseguita un'operazione CRUD su una risorsa di destinazione in MIM.
  - Ambito stabilito dai criteri di corrispondenza definiti nella regola, ovvero le richieste CRUD a cui si applica la regola stessa.

- Set di transizione
  - Tipo usato per definire i criteri indipendentemente dal modo in cui l'oggetto è passato allo stato corrente rappresentato dal set di transizione. Usare una regola Set di transizione per modellare criteri di diritto.
  - Questo tipo viene applicato quando una risorsa entra o esce da un set associato.
  - Ambito corrispondente ai membri del set.

>[NOTA] Per altri dettagli, vedere [Designing Business Policy Rules](http://go.microsoft.com/fwlink/?LinkID=183691) (Progettazione di regole di criteri di business).

#### <a name="only-enable-mprs-as-necessary"></a>Abilitare regole di criteri di gestione solo limitatamente alle esigenze

Quando si applica una configurazione, basarsi sul principio del privilegio minimo. Le regole di criteri di gestione controllano i criteri di accesso per la distribuzione MIM. Abilitare solo le funzionalità usate dalla maggior parte degli utenti. Non tutti gli utenti, ad esempio, usano MIM per la gestione dei gruppi. Pertanto è consigliabile disabilitare le regole di criteri di gestione di gruppi associate. Per impostazione predefinita, in MIM la maggior parte delle autorizzazioni non amministrative è disabilitata.

#### <a name="duplicate-built-in-mprs-instead-of-directly-modifying"></a>Duplicare le regole di criteri di gestione predefinite anziché modificarle direttamente
Se è necessario modificare le regole di criteri di gestione predefinite, è consigliabile creare una nuova regola con la configurazione necessaria e disattivare la regola predefinita corrispondente. Ciò consente di evitare che eventuali modifiche future alle regole predefinite, introdotte tramite il processo di aggiornamento, compromettano la configurazione del sistema.

#### <a name="end-user-permissions-should-use-explicit-attribute-lists-scoped-to-users-business-needs"></a>Le autorizzazioni degli utenti finali devono usare elenchi di attributi espliciti con ambito limitato alle esigenze aziendali degli utenti stessi
L'uso di elenchi di attributi espliciti consente di evitare che vengano accidentalmente concesse autorizzazioni a utenti senza privilegi quando si aggiungono attributi agli oggetti. È preferibile che gli amministratori debbano concedere l'accesso a nuovi attributi in modo esplicito, anziché rimuovere tale accesso.

È consigliabile limitare l'accesso ai dati all'ambito definito dalle esigenze aziendali degli utenti. I membri di un gruppo, ad esempio, non devono avere accesso all'attributo di filtro del gruppo a cui appartengono. Il filtro può rivelare inavvertitamente dati aziendali a cui l'utente non avrebbe normalmente accesso.

#### <a name="mprs-should-reflect-effective-permissions-in-the-system"></a>Le regole di criteri di gestione devono riflettere le autorizzazioni effettivamente utilizzabili nel sistema
Evitare di concedere autorizzazioni ad attributi che gli utenti non possono mai usare. Ad esempio, non concedere l'autorizzazione per la modifica di attributi di risorse core, ad esempio objectType. Nonostante la regola di criteri di gestione, qualsiasi tentativo di modificare il tipo di una risorsa dopo averla creata viene negato dal sistema.

#### <a name="read-permissions-should-be-separate-from-modify-and-create-permissions-when-using-explicit-attributes-in-mprs"></a>Se nelle regole di criteri di gestione si usano attributi espliciti, separare le autorizzazioni di lettura da quelle di modifica e creazione

Se nelle regole di criteri di gestione gli attributi sono elencati in modo esplicito, gli attributi necessari per operazioni di creazione e modifica sono in genere diversi da quelli disponibili per la lettura. L'autorizzazione di lettura, ad esempio, può essere concessa tramite attributi di sistema quali Creator o objectId, mentre non è possibile specificare autorizzazioni di creazione o modifica per gli attributi di sistema.

#### <a name="create-permissions-should-be-separate-from-modify-permissions-when-using-explicit-attributes-in-rules"></a>Se nelle regole si usano attributi espliciti, separare le autorizzazioni di creazione da quelle di modifica

L'operazione di creazione richiede, tra l'altro, che l'utente selezioni objectType. Questo è un attributo di sistema core che non può essere modificato dopo un'operazione di creazione.

#### <a name="use-one-request-mpr-for-all-attributes-with-the-same-access-requirements"></a>Usare una regola di criteri di gestione di tipo Richiesta per tutti gli attributi con gli stessi requisiti di accesso

Per una maggiore efficienza, gli attributi con gli stessi requisiti di accesso per i quali non si prevedono modifiche possono essere combinati in un'unica regola di tipo Richiesta.

#### <a name="avoid-giving-unrestricted-access-even-to-selected-principal-groups"></a>Evitare di concedere accesso senza restrizioni anche a gruppi di entità selezionati

In MIM le autorizzazioni sono definite come asserzioni positive. MIM non supporta la negazione di autorizzazioni. Di conseguenza, se si concede l'accesso senza restrizioni a una risorsa, la successiva esclusione di autorizzazioni per tale risorsa è un'operazione complessa. Come procedura consigliata, concedere solo le autorizzazioni necessarie.

#### <a name="use-tmprs-to-define-custom-entitlements"></a>Definire diritti personalizzati tramite regole di criteri di gestione di tipo Set di transizione

Per definire diritti personalizzati, usare regole di criteri di gestione di tipo Set di transizione anziché di tipo Richiesta. Le regole di criteri di gestione di tipo Set di transizione rappresentano un modello basato sugli stati per assegnare o rimuovere diritti in base all'appartenenza ai set di transizione, o ruoli, definiti, e alle attività del flusso di lavoro associato. Queste regole devono essere sempre definite in coppia, una per le risorse in fase di transizione in entrata e una per le risorse in fase di transizione in uscita. Ogni regola di criteri di gestione di transizione, poi, deve contenere flussi di lavoro separati per le attività di provisioning e deprovisioning.

>[!NOTE]
Per tutti i flussi di lavoro di deprovisioning assicurarsi che l'attributo Esegui in base all'aggiornamento criteri sia impostato su true.

#### <a name="enable-the-set-transition-in-mpr-last"></a>Abilitare per ultima la regola di criteri di gestione di tipo Set di transizione in entrata

Quando si crea una coppia di regole di tipo Set di transizione, attivare la regola di transizione in entrata per ultima. Questo ordine assicura che il diritto non rimanga assegnato a una risorsa che viene aggiunta e rimossa dal set mentre la regola di transizione in entrata è attiva ma prima che la regola di transizione in uscita venga attivata.

#### <a name="workflows-in-tmpr-should-check-target-resource-state-first"></a>I flussi di lavoro in una regola di criteri di gestione di tipo Set di transizione devono controllare prima di tutto lo stato della risorsa di destinazione

I flussi di lavoro di provisioning devono prima di tutto verificare se per la risorsa di destinazione è già stato eseguito il provisioning in conformità al diritto. In caso affermativo, non deve essere eseguita alcuna operazione.

I flussi di lavoro di deprovisioning devono prima di tutto verificare se per la risorsa di destinazione è stato eseguito il provisioning. In caso affermativo, è necessario eseguire il deprovisioning per la risorsa di destinazione. In caso contrario, non deve essere eseguita alcuna operazione.

#### <a name="select-run-on-policy-update-for-tmprs"></a>Selezionare Esegui in base all'aggiornamento criteri per le regole di criteri di gestione di tipo Set di transizione

Questo garantisce l'applicazione del comportamento di provisioning corretto quando vengono implementati aggiornamenti di criteri e questi usano il flag Esegui in base all'aggiornamento criteri per i flussi di lavoro azione associati alle regole stesse. In questo modo, le modifiche alle definizioni dei criteri applicano i flussi di lavoro azione ai nuovi membri del set di transizione.

#### <a name="avoid-associating-the-same-entitlement-with-two-different-transition-sets"></a>Non associare lo stesso diritto a due set di transizione diversi

L'associazione dello stesso diritto a due set di transizione diversi può causare una revoca e una nuova concessione di diritti, entrambe non necessarie, se la risorsa passa da un set all'altro. Come procedura consigliata, assicurarsi che un set contenga tutte le risorse che richiedono il diritto associato. Ciò garantisce una relazione uno a uno tra il set di transizione e il diritto che concede l'accesso al flusso di lavoro.

#### <a name="use-an-appropriate-sequence-of-operations-when-removing-entitlements-in-the-system"></a>Eseguire le operazioni di rimozione di diritti dal sistema nella sequenza appropriata

L'ordine dei passaggi eseguiti durante la rimozione di diritti dal sistema può causare due risultati operativi diversi. Assicurarsi di aver compreso l'ordine da applicare per ottenere l'effetto voluto.

Per rimuovere un diritto dal sistema (e revocarlo per tutti i membri a cui è stato concesso):

1.  Disabilitare la regola di criteri di gestione di tipo Set di transizione in entrata. Questo consente di evitare nuove concessioni.

2.  Eliminare il filtro del set di transizione o modificarlo in modo che il set sia vuoto. In questo modo tutti i membri esistenti effettuano una transizione in uscita e vengono applicati i criteri di transizione in uscita, compresi i flussi di lavoro di deprovisioning configurati associati al diritto.

3.  Disabilitare la regola di criteri di gestione di tipo Set di transizione in uscita.

Per rimuovere un diritto senza disturbare i membri correnti (ad esempio, per interrompere l'uso di MIM per la gestione del diritto):

1.  Disabilitare la regola di criteri di gestione di tipo Set di transizione in entrata. Questo consente di evitare nuove concessioni.

2.  Disabilitare la regola di criteri di gestione di tipo Set di transizione in uscita.

3.  Eliminare il filtro del set di transizione o modificarlo in modo che il set sia vuoto. Poiché il set non è più associato a una regola di criteri di gestione di tipo Set di transizione, non vengono applicati flussi di lavoro di deprovisioning.

### <a name="sets"></a>Operazioni set

Quando si applicano le procedure consigliate per le operazioni set, è necessario prendere in considerazione l'impatto delle ottimizzazioni sulla gestibilità e sulla facilità di amministrazione future. Prima di applicare questi consigli, è necessario eseguire test appropriati nella scala di produzione prevista per individuare il giusto equilibrio tra prestazioni e gestibilità.

>[!NOTE]
> Tutte le linee guida seguenti si applicano ai set dinamici e ai gruppi dinamici.


#### <a name="minimize-the-use-of-dynamic-nesting"></a>Ridurre al minimo l'uso dell'annidamento dinamico

Questo consiglio si riferisce al filtro di un set che fa riferimento all'attributo ComputedMember di un altro set. Un motivo frequente per l'annidamento di set è la necessità di evitare la duplicazione di una condizione di appartenenza in molti set. Questo approccio dà come risultato una migliore gestibilità dei set, ma la contropartita è un peggioramento delle prestazioni. È possibile ottimizzare le prestazioni duplicando le condizioni di appartenenza di un set annidato anziché annidando il set stesso.

In alcuni casi, per soddisfare un requisito funzionale l'annidamento di set è inevitabile. Ecco le situazioni principali in cui è necessario ricorrere a set annidati. Ad esempio, per definire il set di tutti i gruppi senza proprietari Full Time Employee, l'annidamento di set deve essere usato come segue: `/Group[not(Owner = /Set[ObjectID = ‘X’]/ComputedMember]`, dove "X" corrisponde all'ObjectID del set di tutti i dipendenti a tempo pieno.

#### <a name="minimize-the-use-of-negative-conditions"></a>Ridurre al minimo l'uso di condizioni negative

Le condizioni negative sono le condizioni di appartenenza che usano gli operatori o le funzioni seguenti: `!=`, `not()`, `\<` , `\<=`. Per ottimizzare le prestazioni, se possibile, esprimere la condizione voluta con più condizioni positive anziché con una condizione negativa.

#### <a name="minimize-the-use-of-membership-conditions-based-on-multivalued-reference-attributes"></a>Ridurre al minimo l'uso di condizioni di appartenenza basate su attributi di riferimento multivalore

È consigliabile ridurre al minimo l'uso di condizioni di appartenenza basate su attributi di riferimento multivalore, perché un numero elevato di questi set può influire sulle prestazioni delle operazioni eseguite sull'attributo usato nella condizione di appartenenza.

### <a name="password-reset"></a>Reimpostazione della password

#### <a name="kiosk-like-computers-that-are-used-for-password-reset-should-set-local-security-to-clear-the-virtual-memory-pagefile"></a>I computer di tipo chiosco multimediale usati per la reimpostazione della password devono impostare la sicurezza locale per la cancellazione del file di paging della memoria virtuale

Quando si distribuisce la reimpostazione della password MIM in una workstation usata come chiosco multimediale, è consigliabile attivare l'impostazione dei criteri di sicurezza locale Arresto del sistema: cancella file di paging della memoria virtuale, per assicurarsi che le informazioni riservate nella memoria dei processi non siano disponibili per utenti non autorizzati.

#### <a name="users-should-always-register-for-a-password-reset-on-a-computer-that-they-are-logged-on-to"></a>Gli utenti devono sempre effettuare la registrazione per eseguire la reimpostazione della password per un computer a cui sono connessi

Quando un utente tenta di effettuare la registrazione per la reimpostazione della password tramite un portale Web, MIM avvia sempre la registrazione per conto dell'utente connesso, indipendentemente dall'utente connesso al sito Web. Gli utenti devono sempre effettuare la registrazione per eseguire la reimpostazione della password per un computer a cui sono connessi.

#### <a name="do-not-set-the-avoidpdconwan-registry-key-to-true"></a>Non impostare la chiave del Registro di sistema AvoidPdcOnWan su true

Quando si usa la reimpostazione della password di MIM 2016, non impostare la chiave del Registro di sistema AvoidPdcOnWan su true.

Se questa chiave del Registro di sistema è impostata su true, è probabile che l'utente superi il controllo password, effettui la reimpostazione della password sul controller di dominio primario e tenti l'accesso. A causa di questa chiave del Registro di sistema, il controller di dominio locale non esegue la convalida secondaria con il controller di dominio primario e quindi nega l'accesso. Se l'accesso viene negato un certo numero di volte, l'utente viene bloccato ed escluso dal dominio ed è necessario rivolgersi al supporto tecnico.

#### <a name="do-not-turn-on-logging-of-clear-text-passwords"></a>Non attivare la registrazione delle password non crittografate

È possibile registrare le password non crittografate quando si attiva la traccia del livello di servizio a scopo di diagnostica in Windows

Communication Foundation (WCF). Questa opzione non è attivata per impostazione predefinita ed è consigliabile evitare di attivarla in ambienti di produzione. Quando gli utenti effettuano la registrazione per la reimpostazione della password, queste password sono visibili come elementi non crittografati all'interno di un messaggio SOAP (Simple Object Access Protocol) crittografato. Per altre informazioni, vedere [Configurazione della registrazione dei messaggi](http://go.microsoft.com/fwlink/?LinkID=168572).

#### <a name="do-not-map-an-authorization-workflow-to-the-password-reset-process"></a>Non eseguire il mapping di un flusso di lavoro di autorizzazione al processo di reimpostazione della password

Non associare un flusso di lavoro di autorizzazione a un'operazione di reimpostazione della password. La reimpostazione della password richiede una risposta sincrona, mentre i flussi di lavoro di autorizzazione che contengono attività quali l'approvazione sono asincroni.

#### <a name="do-not-map-multiple-action-activities-to-password-reset"></a>Non eseguire il mapping di più attività di azione a un'operazione di reimpostazione della password

Evitare di associare un flusso di lavoro che contiene più attività di azione a un'operazione di reimpostazione della password. Uno scenario di esempio può essere costituito dall'associazione di una seconda attività di reimpostazione della password di AD DS a una regola di criteri di gestione di reimpostazione della password. Questo scenario non è supportato.

#### <a name="require-reregistration-when-adding-removing-or-changing-the-order-of-activities-in-an-existing-workflow"></a>Richiedere una nuova registrazione per l'aggiunta, la rimozione o la modifica dell'ordine delle attività in un flusso di lavoro esistente

Se è necessario aggiungere, rimuovere o modificare l'ordine delle attività di autenticazione in un flusso di lavoro esistente, selezionare sempre l'opzione di richiesta di una nuova registrazione. Gli utenti che tentano di effettuare l'autenticazione per la reimpostazione della password dopo che un'attività è stata aggiunta o rimossa da un flusso di lavoro, ma prima di aver effettuato una nuova registrazione, possono riscontrare effetti indesiderati.

### <a name="portal-configuration-and-resource-control-display-configuration"></a>Configurazione del portale e della visualizzazione controllo risorse

#### <a name="consider-adding-a-privacy-disclaimer-to-the-user-profile-page"></a>Prendere in considerazione la possibilità di aggiungere una dichiarazione di non responsabilità per la privacy alla pagina del profilo utente

Per impostazione predefinita, in MIM alcune informazioni del profilo utente possono essere visualizzate da altri utenti. Come atto di cortesia nei confronti degli utenti, è consigliabile che gli amministratori aggiungano alla pagina del profilo utente testo personalizzato coerente con i criteri aziendali. Per altre informazioni sull'aggiunta di testo personalizzato a una pagina del portale MIM, vedere [Introduction to Configuring and Customizing the FIM Portal](http://go.microsoft.com/fwlink/?LinkID=165848) (Introduzione alla configurazione e alla personalizzazione del portale FIM).

### <a name="schema"></a>Schema

#### <a name="do-not-delete-person-or-group-resource-types"></a>Non eliminare i tipi di risorsa Persona o Gruppo

Anche se i tipi di risorsa Persona e Gruppo non sono contrassegnati come fondamentali, non è consigliabile eliminare le risorse stesse o gli attributi assegnati a queste. L'interfaccia utente del portale MIM richiede la presenza dei tipi di risorsa Persona e Gruppo e dei relativi attributi.

#### <a name="do-not-modify-the-core-attributes"></a>Non modificare gli attributi core

A tutti i tipi di risorsa sono assegnati 13 attributi core. Evitare di modificare in alcun modo la relazione di questi con qualsiasi tipo di risorsa. I 13 attributi core sono:

-   CreatedTime

-   Creator

-   DeletedTime

-   Descrizione

-   DetectedRulesList • DisplayName

-   ExpectedRulesList

-   ExpirationTime

-   Impostazioni locali

-   MVObjectID

-   ObjectId

-   ObjectType

-   ResourceTime

Non eliminare una risorsa di schema che dipende da requisiti di controllo

Non eliminare risorse di schema se per queste esistono ancora requisiti di controllo.

#### <a name="making-regular-expressions-case-insensitive"></a>Disattivare la distinzione tra maiuscole e minuscole per le espressioni regolari

In MIM può essere utile fare in modo che alcune espressioni regolari non facciano distinzione tra maiuscole e minuscole. È possibile ignorare la differenza tra maiuscole e minuscole all'interno di un gruppo tramite ?!:. Ad esempio, per Employee Type usare

`\^(?!:contractor\|full time employee)%.`

#### <a name="calculation-of-the-member-attribute"></a>Calcolo dell'attributo Member

L'attributo Member esposto al motore di sincronizzazione è in realtà mappato a ComputedMembers. È una combinazione di membri basati su criteri e membri selezionati manualmente. Anche se si aggiungono tutti i tre attributi (Filter, ExplicitMembers e ComputedMembers), il calcolo dinamico dell'attributo Member si verifica solo per i tipi di risorsa Gruppo e Set.

#### <a name="leading-and-trailing-spaces-in-strings-are-ignored"></a>Nelle stringhe gli spazi iniziali e finali vengono ignorati

In MIM è possibile immettere stringhe con spazi iniziali e finali, ma il sistema FIM ignora tali spazi. Se si invia una stringa con uno spazio iniziale e uno finale, il motore di sincronizzazione e i servizi Web ignorano tali spazi.

#### <a name="empty-strings-do-not-equal-null"></a>Stringhe vuote non equivalgono a Null

In questa versione di MIM le stringhe vuote non equivalgono a Null. Una stringa vuota è considerata un valore di input valido. Una stringa non presente è considerata equivalente a Null.

### <a name="workflow-and-request-processing"></a>Elaborazione di flussi di lavoro e richieste

#### <a name="do-not-delete-default-workflows-that-are-shipped-with-mim-2016"></a>Non eliminare i flussi di lavoro predefiniti in dotazione con MIM 2016

I flussi di lavoro seguenti vengono forniti con MIM e non devono essere eliminati:

-   Expiration Workflow (Flusso di lavoro di scadenza)

-   Filter Validation Workflow for Administrators (Flusso di lavoro di convalida filtri per amministratori)

-   Filter Validation Workflow for Non-Administrators (Flusso di lavoro di convalida filtri per utenti non amministratori)

-   Group Expiration Notification Workflow (Flusso di lavoro di notifica scadenza gruppo)

-   Group Validation Workflow (Flusso di lavoro di convalida gruppo)

-   Owner Approval Workflow (Flusso di lavoro di approvazione proprietario)

-   Password Reset Action Workflow (Flusso di lavoro di azione reimpostazione password)

-   Password Reset AuthN Workflow (Flusso di lavoro di autenticazione reimpostazione password)

-   Requestor Validation With Owner Authorization (Convalida richiedente con autorizzazione proprietario)

-   Requestor Validation Without Owner Authorization (Convalida richiedente senza autorizzazione proprietario)

-   System Workflow Required for Registration (Flusso di lavoro di sistema obbligatorio per la registrazione)

#### <a name="do-not-run-two-or-more-approvalactivities-in-parallel"></a>Non eseguire due o più ApprovalActivities in parallelo

Non è consigliabile eseguire due o più ApprovalActivities in parallelo. In caso contrario, la richiesta potrebbe bloccarsi in fase di autorizzazione. Per più approvazioni, includere nell'approvazione un elenco più ampio di responsabili approvazione o disporre le attività in sequenza back-to-back.

#### <a name="authorization-activities-should-not-modify-mim-resources-data"></a>Le attività di autorizzazione non devono modificare i dati delle risorse MIM

Evitare di usare attività che modificano le risorse MIM, ad esempio l'attività Analizzatore funzione, all'interno di flussi di lavoro di autorizzazione. Poiché non è stato eseguito il commit della richiesta al momento dell'autorizzazione nel corso dell'elaborazione, eventuali modifiche apportate alle informazioni di identità possono essere applicate nonostante la richiesta possa essere rifiutata.

### <a name="understanding-fim-service-partitions"></a>Informazioni sulle partizioni del servizio FIM

L'obiettivo di MIM è elaborare le richieste che possono essere avviate da diversi client MIM, ad esempio dal servizio di sincronizzazione e dai componenti self-service MIM, in base ai criteri di business configurati dall'utente. Da progettazione, ogni istanza del servizio FIM appartiene a un gruppo logico costituito da uno o più istanze del servizio FIM, noto anche come partizione del servizio FIM. Se è stata distribuita una sola istanza del servizio FIM per la gestione di tutte le richieste, possono verificarsi latenze di elaborazione. Alcune operazioni possono addirittura superare i valori di timeout predefiniti ritenuti appropriati per le operazioni self-service. Le partizioni del servizio FIM consentono di risolvere il problema.

Per altre informazioni, vedere [Informazioni sulle partizioni del servizio FIM](https://social.technet.microsoft.com/wiki/contents/articles/2363.understanding-fim-service-partitions.aspx).

## <a name="next-steps"></a>Passaggi successivi
- [Guida al backup e al ripristino in FIM](http://go.microsoft.com/fwlink/?LinkID=165864)
- [How Do I Synchronize Users from Active Directory Domain Services to FIM](http://go.microsoft.com/fwlink/?LinkID=188277) (Come eseguire la sincronizzazione degli utenti tra Active Directory Domain Services e FIM) 
- [Panoramica del modello di recupero](http://go.microsoft.com/fwlink/?LinkID=185370)