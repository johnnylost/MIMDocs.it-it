---
title: Installazione di BHOLD Core | Microsoft Docs
description: Documento principale per l'installazione di BHOLD Suite
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 09/07/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 48ff4a06233ba95288432c4cfe48e37b4d1449ab
ms.sourcegitcommit: 7de35aaca3a21192e4696fdfd57d4dac2a7b9f90
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/16/2018
ms.locfileid: "49358738"
---
# <a name="bhold-core-installation"></a>Installazione di BHOLD Core

Il modulo BHOLD Core contiene le funzionalità principali di BHOLD Suite all'interno dell'ambiente. Per poter installare altri moduli di BHOLD Suite, è necessario installare e configurare il modulo BHOLD Core in un server della LAN.

## <a name="bhold-core-installation-requirements"></a>Requisiti di installazione di BHOLD Core

Il modulo BHOLD Core è la base di Microsoft BHOLD Suite. Prima di installare altri moduli di Microsoft BHOLD Suite, è necessario installare il modulo BHOLD Core.

### <a name="bhold-core-hardware-requirements"></a>Requisiti hardware di BHOLD Core

Il modulo BHOLD Core è la base di Microsoft BHOLD Suite. Prima di installare altri moduli di Microsoft BHOLD Suite, è necessario installare il modulo BHOLD Core.

|          |        |          |
|----------|--------|----------|
|**Componente** |**Minimo** | **Consigliato** |
|Processore | Processore a 64 bit | Processore a 64 bit multicore |
| Memory |3 GB | 6 GB o più |
|Archiviazione| 30 GB disponibili |Dipende dalle dimensioni della distribuzione |
|Scheda di rete| Connessione a 100 Mbps al server di SQL e Forefront Identity Manager (FIM) | Connessione a 1 Gbps al server di SQL e FIM|

Questi consigli si basano su implementazioni tipiche e non considerano altre applicazioni in esecuzione nel server. A seconda della specificità di un ambiente, potrebbe essere necessario usare componenti con prestazioni più elevate.

### <a name="bhold-core-software-requirements"></a>Requisiti software di BHOLD Core

Il modulo BHOLD Core deve essere installato in un computer che soddisfi i requisiti seguenti:

- Il server deve eseguire Windows Server 2012 R2 (64 bit), Windows Server 2016 
- Il server deve essere membro di un dominio di Active Directory Domain Services. In ambienti di test il server può essere un controller di dominio di Active Directory Domain Services.
- Core BHOLD deve essere installato da un utente che ha eseguito l'accesso usando un account nello stesso dominio del server e che appartiene al gruppo Domain Admins nel dominio e al gruppo Administrators nel server.
- Microsoft Internet Information Services (IIS) con ASP.NET deve essere installato nel server. È necessario configurare IIS con l'autenticazione di Windows abilitata. Se il modulo BHOLD Core viene installato in Windows Server 2012/2016, è necessario installare la compatibilità gestione con IIS 6. Se il modulo BHOLD Core viene installato in Windows Server 2012, è necessario installare gli strumenti di scripting di IIS 6
- .NET 3.5 Framework deve essere installato.
  - Per il modulo BHOLD Core è necessario .NET 3.5. L'installazione in Server Core non è quindi possibile.
- Silverlight 4 è necessario per altri moduli di BHOLD. È pertanto consigliabile installarlo prima di installare BHOLD Core.
- Microsoft SQL Server 2014 o Microsoft SQL Server 2016 deve essere installato nel server di BHOLD Core o in un altro server della LAN. 

Nei computer client di Windows deve essere in esecuzione la versione 6 di Microsoft Internet Explorer o una versione successiva e la versione 4 di Microsoft Silverlight 4 o una versione successiva.

## <a name="before-you-begin"></a>Prima di iniziare

Per il modulo BHOLD Core è necessario un account utente usato per autenticare e autorizzare il servizio di BHOLD Core nel server e in altre entità di rete. Questa sezione illustra come creare e configurare l'account utente e contiene un foglio di lavoro di preinstallazione in cui raccogliere le informazioni necessarie per completare l'installazione di BHOLD Core.

>{!IMPORTANTE} quando si installa BHOLD Core, è possibile usare un database di SQL Server esistente come database di BHOLD Core oppure è possibile crearne uno tramite l'installazione guidata di Core BHOLD. Se si sceglie di usare un database esistente, è necessario verificare che nessun altro utente possa accedere al database durante l'installazione di BHOLD Core. Prima di installare BHOLD Core, verificare che i controlli di accesso del database di SQL Server consentano l'accesso solo all'utente che sta eseguendo l'installazione di BHOLD Core.

## <a name="required-user-and-group"></a>Utente e gruppo necessari

È necessario che il modulo BHOLD Core acceda al dominio usando un account utente dedicato a tale scopo e che sia membro dei due gruppi di sicurezza specificati. Uno dei due gruppi deve essere creato appositamente per il modulo BHOLD Core. Per eseguire questa procedura è necessaria l'appartenenza al gruppo Domain Admins.
**Per creare e configurare l'utente e il gruppo di sicurezza di BHOLD Core**

1.  In un controller di dominio fare clic su **Avvia**, selezionare **Strumenti di amministrazione** e fare clic su **Utenti e computer di Active Directory**.

2.  Nell'albero della console espandere il dominio in cui deve essere creato l'account, fare clic con il pulsante destro del mouse su **Utenti**, scegliere **Nuovo** e fare clic su **Gruppo**.

3.  Nella finestra di dialogo **Nuovo oggetto - Gruppo** in **Nome gruppo** digitare il nome del gruppo (nome predefinito per BHOLD: BHOLDGruppoApplicazioni) e fare clic su **OK**.

4.  Fare clic con il pulsante destro del mouse su **Utenti**, scegliere **Nuovo**e quindi **Utente**.

5.  In **Nome completo** digitare un nome per identificare l'account, ad esempio Account del servizio di BHOLD Core.

6.  In **Nome accesso utente** digitare il nome utente dell'account del servizio di BHOLD Core (nome predefinito per BHOLD: b1user) e fare clic su **Avanti**.

7.  In **Password** e **Conferma password** digitare la password per l'account del servizio.

8.  Deselezionare **Cambiamento obbligatorio password all'accesso successivo**, selezionare **Cambiamento password non consentito** e **Nessuna scadenza password**, fare clic su **Avanti** e selezionare **Fine**.

9.  Nel riquadro dei risultati della console fare clic con il pulsante destro del mouse sull'account utente e scegliere **Aggiungi a un gruppo**.

10. Nella finestra di dialogo **Seleziona gruppi** digitare il nome visualizzato del gruppo creato in precedenza, un punto e virgola (;) e IIS_IUSRS.

11. Fare clic su **Controlla nomi** e su **OK**.  

La procedura seguente deve essere eseguita nel computer in cui viene installato il modulo BHOLD Core. Per eseguire questa procedura, è necessario accedere come membro del gruppo Domain Admins.

## <a name="bhold-core-installation-worksheet"></a>Foglio di lavoro per l'installazione di BHOLD Core

Prima di iniziare l'installazione del modulo BHOLD Core, procurarsi le informazioni da specificare durante la l'installazione guidata di BHOLD Core necessarie per completare l'installazione. Il foglio di lavoro riportato di seguito consente di registrare le informazioni in modo che siano disponibili al momento dell'inserimento. Ogni sezione corrisponde a una pagina dell'installazione guidata di BHOLD Core.

### <a name="account-settings"></a>Impostazioni account

| **Elemento**                                    | **Descrizione**                                                                                                                                                                                                                                                                                             | **Valore**                                                                                                                                                          |
|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Use Security Provider on Domain/Machine** (Usa provider di sicurezza per il dominio/computer) | Se selezionata, questa opzione indica che la protezione di Active Directory Domain Services controllerà l'accesso a BHOLD Core.                                                                                                                                                                                                  | Selezionare la casella di controllo. **Importante:** l'installazione avrà esito negativo se questa casella di controllo non è selezionata.                                                                 |
| **Dominio**                                  | Specifica il dominio contenente il server di BHOLD, l'account del servizio e il gruppo di applicazioni. **Importante:** specificare il nome di dominio usando il nome NetBIOS (breve), non il nome di dominio completo (FQDN). Se, ad esempio, il nome FQDN del dominio è fabrikam.com, specificare il nome di dominio come CONTOSO. | Scrivere qui il nome del dominio:                                                                                                                                        |
| **Gruppo applicazioni**                       | Specifica il nome del gruppo di sicurezza creato in precedenza in [Utente e gruppo necessari](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx#rug).                                                                                                                                  | Scrivere qui il nome del gruppo:                                                                                                                                         |
| **Service user** (Utente servizio)                            | Specifica il nome di accesso dell'account utente del servizio creato in precedenza in [Utente e gruppo necessari](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx#rug).                                                                                                                      | Scrivere qui il nome dell'account utente:                                                                                                                                  |
| **Password**                                | Specifica la password dell'account utente del servizio di BHOLD Core.                                                                                                                                                                                                                                              | Scrivere qui la password: **importante:** assicurarsi di conservare questa password in una posizione protetta nascosta.                                                                |
| **Website IP/Port** (Indirizzo IP/porta sito Web)                         | Specifica l'indirizzo IP e il numero di porta del sito Web da creare nel server intranet. Modificare il valore predefinito (\*) solo se non si userà lo stesso indirizzo IP del sito Web predefinito. Sostituire il numero di porta con una porta disponibile solo se la porta predefinita (5151) è già in uso.             | Se il sito Web predefinito usa un indirizzo IP non predefinito, scriverlo qui: se il numero di porta predefinito è già in uso, scrivere qui il numero di porta del sito Web BHOLD: |

### <a name="database-settings"></a>Impostazioni database

| **Elemento**                                       | **Descrizione**                                                                                                                                                                                                                                                           | **Valore**                                                                                                                                                                                                                                                                                                                                                                                             |
|------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Use integrated Security** (Usa sicurezza integrata)                    | Specifica che per accedere al database viene usata l'autenticazione di Windows.                                                                                                                                                                                                     | Selezionare la casella di controllo se viene usata l'autenticazione di Windows per connettersi a SQL Server. Deselezionare la casella di controllo se viene usata l'autenticazione di SQL Server. Se si usa l'autenticazione di SQL Server è necessario creare il database prima di eseguire l'installazione di BHOLD Core. **Nota:** se viene usata l'autenticazione di Windows, è necessario eseguire l'accesso con un account con il ruolo del server sysadmin nel server di database. |
| **Utente database** e **Password database** | Specifica il nome utente e la password di un utente con ruolo del server sysadmin nel server di database. Questi valori vengono specificati solo quando si usa l'autenticazione di SQL Server.                                                                                               | Scrivere qui il nome utente di SQL Server:  scrivere qui la password utente di SQL Server: **nota:** assicurarsi di conservare questa password in una posizione protetta nascosta.                                                                                                                                                                                                                                                  |
| **Server database** e **Nome server**   | Specifica il nome NetBIOS del server di database e il nome del database (nome predefinito: b1) che l'installazione di BHOLD Core creerà. Se non si usa l'istanza del server di database predefinita, specificare l'istanza del server di database nel formato *\<server\>*\\*\<istanza\>*. | Scrivere qui il nome del server (server o istanza): scrivere qui il nome del database:                                                                                                                                                                                                                                                                                                                   |
| **Make restrictions for the database user** (Crea limitazioni per l'utente del database)    | Obsoleta.                                                                                                                                                                                                                                                                 | Non modificare il valore predefinito                                                                                                                                                                                                                                                                                                                                                                       |
|                                                |                                                                                                                                                                                                                                                                           |                                                                                                                                                                                                                                                                                                                                                                                                       |
|                                                |                                                                                                                                                                                                                                                                           |                                                                                                                                                                                                                                                                                                                                                                                                       |

## <a name="bhold-core-setup"></a>Installazione di BHOLD Core

Per installare il modulo BHOLD Core, accedere come membro del gruppo Domain Admins, scaricare il file seguente ed eseguirlo come amministratore nel server in cui si intende installare il modulo BHOLD Core: 

- BholdCore *\<versione\>*\_release.msi

Sostituire *\<versione\>* con il numero della versione di BHOLD Core che si sta installando.

Per eseguire il file di programma come amministratore, fare clic con il pulsante destro del mouse sul file e selezionare **Esegui come amministratore**.

### <a name="postinstallation-settings"></a>Impostazioni di post-installazione

Dopo aver concluso l'installazione di BHOLD Core, è necessario configurare Windows Firewall e modificare le impostazioni avanzate nel pool di applicazioni di BHOLD Core in Internet Information Services per completare la configurazione di BHOLD Core. Se necessario, modificare anche gli attributi di sistema di BHOLD per soddisfare i requisiti.

#### <a name="configuring-windows-firewall"></a>Configurazione di Windows Firewall

Se gli utenti accederanno a BHOLD tramite Web browser in computer remoti, è necessario configurare Windows Firewall nel server di BHOLD Core in modo da consentire le connessioni in ingresso alla porta del sito Web specificato al momento dell'installazione di BHOLD Core.

Per eseguire questa procedura, è necessario essere membri del gruppo Administrators nel computer locale.

#### <a name="to-permit-incoming-connections-to-the-bhold-website"></a>Per consentire le connessioni in ingresso al sito Web di BHOLD

1.  Fare clic su **Avvia**, scegliere **Strumenti di amministrazione**, fare clic con il pulsante destro del mouse su **Windows Firewall with Advanced Settings** (Windows Firewall con impostazioni avanzate) e fare clic su **Esegui come amministratore**.

2.  Nel riquadro sinistro fare clic su **Regole connessioni in entrata** e nel riquadro destro fare clic su **Nuova regola**.

3.  Nella Creazione guidata nuova regola connessioni in entrata selezionare **Porta** e fare clic su **Avanti**.

4.  Verificare che l'opzione **TCP** sia selezionata. In **Porte locali specifiche** digitare il numero di porta predefinito per BHOLD Core (5151) o il numero di porta specificato durante l'installazione di BHOLD Core e fare clic su **Avanti**.

5.  Verificare che l'opzione **Consenti la connessione** sia selezionata e fare clic su **Avanti**.

6.  Nella pagina **Profilo** deselezionare le caselle di controllo corrispondenti alle posizioni da cui si vuole impedire l'accesso al sito Web di BHOLD e fare clic su **Avanti**.

7.  Nella pagina **Nome** digitare un nome per la regola (ad esempio, Consentire connessioni in ingresso a BHOLD Core) e fare clic su **Fine**.  

#### <a name="enabling-32-bit-applications-for-the-bhold-core-application-pool"></a>Abilitazione delle applicazioni a 32 bit per il pool di applicazioni di BHOLD Core

Per consentire a IIS di funzionare correttamente con il modulo BHOLD Core, è necessario configurare il pool di applicazioni di BHOLD Core in modo che possa supportare le applicazioni a 32 bit. Per eseguire questa procedura, è necessario accedere con l'account usato durante l'installazione del modulo BHOLD Core.

**Per abilitare il supporto delle applicazioni a 32 bit per il pool di applicazioni di BHOLD Core**

1.  Per aprire Gestione Internet Information Services, fare clic su **Avvia**, selezionare **Strumenti di amministrazione**e **Gestione Internet Information Services (IIS)**.

2.  Nell'albero della console espandere il nome del server e fare clic su **Pool di applicazioni**.

3.  Nel riquadro **Pool di applicazioni** fare clic con il pulsante destro su **CoreAppPool** e scegliere **Impostazioni avanzate**.

4.  Nella finestra di dialogo **Impostazioni avanzate** selezionare **True** dall'elenco **Attiva applicazioni a 32 bit** e fare clic su **OK**.

#### <a name="establishing-the-service-principal-name-for-the-bhold-website"></a>Definizione del nome dell'entità servizio per il sito Web di BHOLD

Se il nome della rete usato per contattare il sito Web di BHOLD non corrisponde al nome host del server, è necessario definire un nome dell'entità servizio (SPN) per HTTP. Ad esempio, se si usa un record di risorse CNAME in DNS per specificare un alias per il server o se si usa il bilanciamento carico di rete, è necessario registrare questi indirizzi di rete aggiuntivi in Active Directory. Altrimenti, Internet Explorer non potrà usare il protocollo Kerberos per contattare il sito Web di BHOLD.

> [!IMPORTANT]
> Se il modulo BHOLD Core è installato nello stesso computer del portale di FIM, è necessario creare record di risorse DNS (CNAME o A) con nomi host diversi per i server che eseguono BHOLD Core e il server che esegue il portale di FIM. È possibile definire un solo nome dell'entità servizio per una coppia specifica di tipo di servizio/alias server. Per BHOLD Core e il portale di FIM sono però necessari SPN separati perché vengono in genere eseguiti con account diversi. Il comando setspn segnala un errore se un SPN è già stato definito per un altro account.

L'appartenenza a **Domain Admins**, o equivalente è il requisito minimo necessario per completare questa procedura.

#### <a name="to-establish-the-spn-of-the-bhold-website"></a>Per definire un SPN del sito Web di BHOLD

1.  Nel controller di dominio di Active Directory Domain Services scegliere **Avvia**, fare clic su **Programmi**, selezionare **Accessori**, fare clic con il pulsante destro del mouse su **Prompt dei comandi** e poi selezionare **Esegui come amministratore**.

2.  Al prompt dei comandi, digitare il comando seguente e premere INVIO: setspn –S HTTP/ *\<aliasrete\> \<dominio\>* \\ *\<nomeaccount\>* dove:

    -   *\<aliasrete\>*  è l'indirizzo usato dai client per contattare il sito Web di BHOLD

    -   *\<dominio\>*\\*\<nomeaccount\>*  è il dominio e il nome utente dell'account del servizio di BHOLD Core creato durante l'installazione di BHOLD Core.

3.  Ripetere il passaggio precedente per tutti gli altri nomi usati dai client per contattare il sito Web di BHOLD, ad esempio, alias CNAME, nomi che includono un nome di dominio completo (FQDN ) o nomi che includono un nome di dominio NetBIOS (breve).

#### <a name="setting-bhold-system-attributes"></a>Impostazione degli attributi di sistema di BHOLD

Per verificare che il modulo BHOLD Core sia stato installato correttamente, aprire il portale di BHOLD Core e visualizzare gli attributi di sistema. Per garantire che il modulo BHOLD Core funzioni correttamente nell'ambiente in uso, è anche possibile modificare gli attributi di sistema di BHOLD seguenti, a seconda delle esigenze:

| **Attributo**                | **Descrizione**                                                                                                                                                                                                                                                                                                      |
|------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **NoHistory**                | Impostare su Y se il sito Web di BHOLD è in esecuzione in un servizio Web del cluster per verificare che gli elementi visualizzati di recente funzionino correttamente. Impostare su N se il sito Web di BHOLD è in esecuzione in un server IIS autonomo.                                                                                                                      |
| **MoveorgunitToSameorgtype** | Impostare su Y per verificare che le unità organizzative possono essere spostate solo in unità che hanno lo stesso tipo di organizzazione dell'unità padre. In questo modo si impedisce ad esempio che l'unità organizzativa di un progetto venga spostata in un'unità di un reparto. Impostare su N per consentire di inserire un'unità organizzativa in un'unità di tipo diverso. |
| **Days between ABA run** (Giorni tra esecuzioni con autorizzazione basata su attributi)     | Impostare un numero intero a due cifre per specificare l'intervallo (in giorni) tra due esecuzioni con autorizzazione basata su attributi. Ad esempio, digitare 02 per specificare che le esecuzioni con autorizzazione basata su attributi avverranno a distanza di due giorni.                                                                                                                     |
| **Start hour of ABA run** (Ora di inizio di un'esecuzione con autorizzazione basata su attributi)    | Impostare un numero intero a due cifre per specificare l'ora del giorno in cui avverrà l'esecuzione con autorizzazione basata su attributi. Ad esempio, per specificare che l'esecuzione con autorizzazione basata su attributi avverrà alle ore 23.00, digitare 23.                                                                                                             |
| **System Cardinality** (Cardinalità sistema)       | Impostare su N se non si vuole che la cardinalità del sistema sia controllata in BHOLD. Il valore predefinito è Y.                                                                                                                                                                                                                             |
| **Registrazione**                  | Impostare su N se si vogliono registrare le modifiche. Il valore predefinito è Y.                                                                                                                                                                                                                                            |
| **SystemQueue Processing** (Elaborazione coda di sistema)   | Impostare su N se non si vuole elaborare la coda di sistema. Modificare questo valore solo se consigliato dal supporto tecnico.                                                                                                                                                                                           |

Per eseguire questa procedura, è necessario accedere come membro del gruppo Domain Admins.

**Per impostare un attributo di sistema di BHOLD**

1.  Fare clic sul pulsante **Avvia**, scegliere **Programmi** e fare clic su **Internet Explorer**.

2.  Nella casella dell'indirizzo digitare, dove *\<server\>* è il nome del server del sito Web di BHOLD e *\<porta\>* è il numero di porta associato al sito Web.

3.  Fare clic su **Home**, selezionare **Valori** e fare clic su **Modifica**.

4.  Individuare il nome dell'attributo che si vuole modificare, digitare il nuovo valore nella casella accanto al nome dell'attributo e fare clic su **OK**.

## <a name="next-steps"></a>Passaggi successivi

Dopo aver installato BHOLD Core e aver verificato che il modulo è stato installato correttamente, è possibile installare moduli aggiuntivi. A questo punto, il database BHOLD sarà essenzialmente vuoto, conterrà solo un account utente, l'account radice, un'unità organizzativa e l'unità organizzativa radice. Per aggiungere altri utenti al database di BHOLD, è possibile installare il modulo Access Management Connector o il modulo BHOLD Model Generator, a seconda delle esigenze. È possibile usare il modulo Access Management Connector per importare i dati dell'utente dal servizio di sincronizzazione di FIM oppure è possibile usare BHOLD Model Generator per importare i dati dell'utente da un set di file strutturati. Per altre informazioni sull'uso del modulo Access Management Connector, vedere [Test Lab Guide: BHOLD Access Management Connector](https://technet.microsoft.com/library/jj853085(v=ws.10).aspx) (Guida al lab di test: BHOLD Access Management Connector).

Per altre informazioni sull'uso del modulo BHOLD Model Generator, vedere gli argomenti seguenti:

- [Microsoft BHOLD Suite Concepts Guide](https://technet.microsoft.com/library/jj134102(v=ws.10).aspx) (Guida ai concetti di Microsoft BHOLD Suite)
- [Microsoft BHOLD Suite TechnicalReference](https://technet.microsoft.com/library/jj134935(v=ws.10).aspx)(Guida di riferimento tecnico a Microsoft BHOLD Suite).
