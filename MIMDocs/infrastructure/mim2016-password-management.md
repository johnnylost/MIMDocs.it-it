---
title: Gestione delle password di Microsoft Identity Manager 2016 | Microsoft Docs
description: ''
keywords: ''
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 08/01/2017
ms.topic: reference
ms.prod: identity-manager-2016
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: ''
ms.openlocfilehash: 86b8b9bdf5c6441d0708cd874742fa48b65177fa
ms.sourcegitcommit: 35f2989dc007336422c58a6a94e304fa84d1bcb6
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36289364"
---
# <a name="microsoft-identity-manager-2016-password-management"></a>Gestione delle password di Microsoft Identity Manager 2016

La gestione delle password per più account utente è uno degli aspetti complessi della gestione di un ambiente aziendale con più origini dati. Microsoft Identity Manager 2016 (MIM) offre due soluzioni per la gestione delle password:

-   Sincronizzazione password: usa il servizio di notifica della modifica della password (PCNS) per acquisire le modifiche della password da Active Directory e propagarle nelle altre origini dati connesse.

-   Gestione delle modifiche della password basata sugli utenti: usa Strumentazione gestione Windows (WMI) tramite l'Help Desk basato sul Web e le applicazioni self-service di reimpostazione della password.

Grazie alla sincronizzazione password e alla gestione delle modifiche della password basata sugli utenti, è possibile:

-   Ridurre il numero di password diverse che gli utenti devono ricordare.

-   Impostare o modificare contemporaneamente nella stessa password le password di più account utente.

-   Consente agli utenti di modificare le proprie password in Active Directory e propagare tale modifica in altri sistemi.

-   Eliminare il rischio di creare un archivio aggiuntivo di password e credenziali.

-   Sincronizzare le password in più origini dati usando Active Directory come origine autorevole.

-   Gestire le password in tempo reale, indipendente dalle operazioni di MIM.

## <a name="password-extensions"></a>Estensioni password

Per impostazione predefinita gli agenti di gestione per i server di directory supportano le operazioni di modifica e di impostazione della password. Per gli agenti di gestione su file, per database e per connettività estendibile che per impostazione predefinita non supportano le operazioni di modifica e di impostazione della password, è possibile creare una libreria di collegamento dinamico (DLL) di estensione password.
La DLL di estensione password .NET viene chiamata ogni volta in cui, per uno di questi agenti di gestione, viene richiamata la modifica o l'impostazione della password. Per questi agenti di gestione, le impostazioni di estensione password vengono configurate in Synchronization Service Manager. Per altre informazioni sulla configurazione delle estensioni password, vedere i riferimenti per sviluppatori FIM.

| La gestione delle password è supportata per impostazione predefinita negli agenti di gestione per: | Tramite un'estensione password, la gestione delle password è supportata anche negli agenti di gestione per: |
|---------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------|
| Active Directory                                                          | File di testo per coppia valore-attributo                                                                    |
| Active Directory Lightweight Directory Services (ADLDS)                   | File di testo delimitati                                                                               |
| IBM Directory Server                                                      | Directory Services Markup Language (DSML)                                                          |
| Lotus Notes                                                               | Connettività estendibile                                                                            |
| Novell eDirectory                                                         | File di testo a larghezza fissa                                                                             |
| Server di directory Sun e Netscape                                        | Database IBM DB2 Universal                                                                         |
|                                                                           | Formato interscambio dati LDIF                                                                |
|                                                                           | Microsoft SQL Server                                                                               |
|                                                                           | Database Oracle                                                                                    |

## <a name="password-synchronization"></a>Sincronizzazione password


La sincronizzazione password usa il servizio di notifica della modifica della password (PCNS) in un dominio di Active Directory e consente di propagare automaticamente in altre origini dati connesse le modifiche alle password apportate in Active Directory. In questo caso MIM viene eseguito come server RPC che riceve la notifica della modifica della password da un controller di dominio di Active Directory. Dopo aver ricevuto e autenticato la richiesta di modifica della password, MIM la elabora e la propaga agli agenti di gestione appropriati.

> [!IMPORTANT]
> La sincronizzazione password bidirezionale non è supportata da MIM. Se viene configurata la sincronizzazione password bidirezionale, si può creare un ciclo, che userà le risorse del server e avrà un effetto potenzialmente negativo su Active Directory e MIM.

PCNS viene eseguito in ogni controller di dominio di Active Directory. I sistemi che ricevano le notifiche della password sono detti destinazioni. Prima di inviare le notifiche della password, è necessario configurare il server MIM come destinazione di PCNS in Active Directory. Durante la configurazione di PCNS è necessario definire un gruppo di inclusione e, facoltativamente, un gruppo di esclusione. Questi gruppi vengono usati per limitare il flusso di password sensibili dal dominio. Ad esempio, per inviare le password per tutti gli utenti, ma non inviare le password amministrative, è possibile scegliere di usare gli utenti del dominio come gruppo di inclusione e gli amministratori del dominio come gruppo di esclusione. Per altre informazioni sulla configurazione del servizio di notifica della modifica della password, vedere [Using Password Synchronization](https://technet.microsoft.com/library/jj590288(v=ws.10).aspx) (Uso della sincronizzazione password)

Di seguito sono elencati i componenti coinvolti nel processo di sincronizzazione password:

-   **Servizio di notifica della modifica della password (Pcnssvc.exe)**: il servizio di notifica della modifica della password viene eseguito in un controller di dominio. Riceve le notifiche della modifica della password dal filtro password locale, le accoda per il server di destinazione che esegue MIM e usa RPC per recapitare le notifiche. Il servizio crittografa la password e verifica che rimanga protetta fino a quando viene recapitata al server di destinazione che esegue MIM.

-   **Nome dell'entità servizio (SPN)**: SPN è una proprietà nell'oggetto account in Active Directory che viene usata dal protocollo Kerberos per eseguire l'autenticazione reciproca di PCNS e della destinazione. SPN verifica che PCNS esegua l'autenticazione nel server corretto che esegue MIM e che nessun altro servizio possa ricevere le notifiche della modifica della password. SPN viene creato e assegnato con lo strumento setspn.exe. Per altre informazioni sulla configurazione di SPN, vedere Using Password Synchronization (Uso della sincronizzazione password).

-   **Filtro di notifica della modifica della password (Pcnsflt.dll)**: il filtro password viene usato per ottenere le password non crittografate da Active Directory. Questo filtro viene caricato dall'autorità di protezione locale (LSA) in ogni controller di dominio Windows Server che partecipa alla distribuzione delle password in un server di destinazione che esegue MIM. Dopo che il filtro è stato installato e il controller di dominio è stato riavviato, il filtro inizia a ricevere le notifiche che riguardano le modifiche della password che sono state eseguite in tale controller di dominio. Il filtro di notifica della password viene eseguito contemporaneamente con altri filtri che sono in esecuzione nel controller di dominio.

-   **Utilità di configurazione del servizio di notifica della modifica della password (Pcnscfg.exe)**: l'utilità pcnscfg.exe viene usata per gestire i parametri di configurazione del servizio di notifica della modifica della password archiviati in Active Directory. Questi parametri di configurazione, ad esempio la definizione di server di destinazione, l'intervallo tra tentativi di accodamento della password, l'abilitazione o disabilitazione di un server di destinazione, vengono usati durante l'autenticazione e l'invio di notifiche della password nel server di destinazione che esegue MIM.
    La configurazione del servizio viene archiviata in Active Directory, pertanto è necessario soltanto aggiornare la configurazione in un controller di dominio. Active Directory replicherà la modifica in tutti gli altri controller di dominio.

-   **Server RPC (Remote Procedure Call) nel server che esegue MIM**: quando la sincronizzazione password è abilitata , il server RPC viene avviato nel server che esegue MIM, in modo che possa ricevere le notifiche dal servizio di notifica della modifica della password. RPC seleziona in modo dinamico un intervallo di porte da usare. Se MIM deve comunicare con la foresta Active Directory tramite un firewall, è necessario aprire un intervallo di porte.

-   **DLL di estensione password**: la DLL di estensione password consente di implementare le operazioni di impostazione o modifica della password usando un'estensione delle regole per qualsiasi agente di gestione su file, per database connettività estendibile.
    È necessario creare un attributo crittografato di sola esportazione denominato "export_password" che non esiste nella directory connessa. È comunque possibile accedere a questo attributo e impostarlo nelle estensioni delle regole di provisioning. Può anche essere usato durante il flusso dell'attributo di esportazione. Per altre informazioni sulla configurazione delle estensioni password, vedere i [riferimenti per sviluppatori FIM](https://msdn.microsoft.com/library/windows/desktop/ee652263(v=vs.100).aspx).

## <a name="preparing-for-password-synchronization"></a>Preparazione per la sincronizzazione password

Prima di configurare la sincronizzazione password per MIM e Active Directory, è necessario verificare quanto segue:

-   MIM è installato in base alle istruzioni di installazione.

-   Gli agenti di gestione per le origini dati connesse da gestire per la sincronizzazione password sono già state create e gli oggetti sono stati aggiunti e sincronizzati.

Per configurare la sincronizzazione password:

-   Estendere lo schema di Active Directory per aggiungere le classi e gli attributi necessari per installare ed eseguire il servizio di notifica della modifica della password (PCNS).

-   Installare PCNS in ogni controller di dominio.

-   Configurare il nome dell'entità servizio (SPN) in Active Directory per l'account del servizio MIM.

-   Configurare PCNS per comunicare con il servizio MIM di destinazione.

-   Configurare gli agenti di gestione per le origini dati connesse da gestire per la sincronizzazione password.

-   Abilitare la sincronizzazione password in MIM.

Per altre informazioni sulla configurazione della sincronizzazione password, vedere Using Password Synchronization (Uso della sincronizzazione password).

## <a name="password-synchronization-process"></a>Processo di sincronizzazione password

Il diagramma seguente illustra il processo di sincronizzazione di una richiesta di modifica della password da parte di un controller di dominio di Active Directory in altre origini dati connesse:

1.  L'utente avvia la richiesta di modifica della password premendo CTRL+ALT+CANC. La richiesta di modifica della password, inclusa la nuova password, viene inviata al controller di dominio più vicino.

2.  Il controller di dominio registra la richiesta di modifica della password e invia una notifica al filtro di notifica della modifica della password (Pcnsflt.dll).

3.  Il filtro di notifica della modifica della password passa la richiesta al servizio di notifica della modifica della password (PCNS).

4.  PCNS verifica la richiesta di modifica della password, autentica il nome dell'entità servizio (SPN) usando Kerberos e inoltra la richiesta di modifica della password in RPC crittografata al server di destinazione MIM.

5.  MIM convalida il controller di dominio di origine, usa il nome di dominio per individuare l'agente di gestione che offre servizio a tale dominio e che usa le informazioni sull'account utente nella richiesta di modifica della password per individuare l'oggetto corrispondente nello spazio connettore.

6.  Usando le informazioni di una tabella join, MIM determina gli agenti di gestione che ricevono la modifica della password e propaga la modifica della password in tali agenti.

## <a name="password-synchronization-security"></a>Sicurezza della sincronizzazione password

Sono stati risolti i problemi seguenti relativi alla sicurezza della sincronizzazione password:

-   Autenticazione dell'origine della password: dopo aver ricevuto la notifica della modifica della password, MIM usa l'autenticazione Kerberos e il controller di dominio dell'origine per verificare che sia il mittente che il destinatario siano validi. Dopo aver ricevuto una notifica della modifica della password, MIM verifica che il chiamante abbia un account nel contenitore dei controller di dominio del dominio a cui appartiene.

-   Non è possibile eseguire la sincronizzazione password in un'origine dati di destinazione a causa di una connessione non protetta: se l'agente di gestione è stato configurato per richiedere una connessione protetta, ma non viene rilevata, la sincronizzazione ha esito negativo.
    La sincronizzazione viene comunque eseguita se l'agente di gestione è stato configurato per consentire connessioni non protette. È consigliabile consentire le connessioni non protette solo dopo aver esaminato e compreso gli eventuali rischi.

-   Archiviazione protetta delle password: MIM archivia solo le password crittografate temporaneamente. Tutte le password ricevute da MIM durante un'operazione di notifica della modifica della password vengono crittografate non appena immesse nel processo MIM.
    Nel momento in cui vengono inviate all'origine dati connessa di destinazione, vengono decrittografate e la memoria in cui viene archiviata la password viene immediatamente cancellata. Se non è possibile scrivere nell'origine dati connessa di destinazione, la password crittografata viene archiviata dopo aver ripetuto l'operazione per il numero di tentativi stabiliti. Dopo di che viene cancellata dalla memoria.

-   Code password protette: le password archiviate nelle code PCNS sono crittografate finché vengono recapitate.

## <a name="password-synchronization-error-recovery-scenarios"></a>Scenari di ripristino in caso di errore di sincronizzazione password

Idealmente, ogni volta che un utente modifica una password, la modifica viene sincronizzata senza errori. Gli scenari seguenti spiegano come MIM esegue il ripristino in caso di errori di sincronizzazione comuni:

-   **Non è possibile inviare la notifica password da Active Directory a MIM**: questo errore può verificarsi se la rete non è attiva o se il server che esegue MIM non è disponibile. La notifica della modifica della password viene accodata localmente nel controller di dominio da PCNS. PCNS ritenta di inviare la notifica in base all'intervallo tra tentativi configurato.

-   **Non è possibile eseguire la sincronizzazione password in un'origine dati di destinazione**: anche questo errore può verificarsi se la rete non è attiva o se l'origine dati di destinazione non è disponibile.
    La notifica della modifica della password viene accodata e ripetuta in base all'intervallo tra tentativi configurato per l'agente di gestione. Tutte le password sono crittografate finché rimangono archiviate in attesa di un nuovo tentativo ed eliminate quando l'operazione ha esito positivo o viene raggiunto il limite massimo di tentativi.

-   **Attivazione di un server warm standby che esegue MIM dopo un errore**: in caso di errore nel server primario che esegue MIM, è possibile configurare un server warm standby per la sincronizzazione password e attivarlo senza che le modifiche delle password vadano perse. Per altre informazioni, vedere [MIISactivate: Server Activation Tool](https://technet.microsoft.com/library/jj590194(v=ws.10).aspx) (MIISactivate: strumento di attivazione server)

Alcuni errori sono abbastanza gravi che, nonostante tutti i tentativi, l'operazione non può essere eseguita correttamente. In questi casi, viene registrato un evento di errore e il processo viene arrestato. Gli eventi seguenti non vengono ripetuti:

| Evento | Gravità    | Descrizione                                                                                                                                                            |
|-------|-------------|-----------|
| 6919  | Informazioni | Un'operazione di impostazione di sincronizzazione password non è stata eseguita perché il timestamp non era aggiornato.                                                                      |
| 6921  | Errore       | L'operazione di impostazione di sincronizzazione password non è stata elaborata perché la gestione delle password non è abilitata nell'agente di gestione di destinazione.                                |
| 6922  | Errore       | L'operazione di impostazione di sincronizzazione password non è stata elaborata perché la gestione delle password non è configurata nell'agente di gestione di destinazione.                             |
| 6923  | Avviso     | L'operazione di impostazione di sincronizzazione password non è stata elaborata perché l'oggetto spazio connettore di destinazione non è stato trovato nella directory connessa.                  |
| 6927  | Errore       | L'operazione di impostazione di sincronizzazione password non è riuscita perché la password non soddisfa i criteri password del sistema di destinazione.                                      |
| 6928  | Errore       | L'operazione di impostazione di sincronizzazione password non è riuscita perché l'estensione password per l'agente di gestione di destinazione non è configurato per supportare le operazioni di impostazione password. |

## <a name="user-based-password-change-management"></a>Gestione della modifica della password basata sugli utenti

MIM offre due applicazioni Web che usano Strumentazione gestione Windows (WMI) per la reimpostazione delle password. Come per la sincronizzazione password, è necessario attivare la gestione delle password durante la configurazione dell'agente di gestione in Progettazione agente di gestione. Per informazioni sulla gestione delle password e su WMI, vedere i riferimenti per sviluppatori MIM.

MIM crea due gruppi di sicurezza durante l'installazione che nello specifico supportano le operazioni di gestione delle password:

-   FIMSyncBrowse: i membri di questo gruppo sono autorizzati a raccogliere informazioni sugli account utente durante le operazioni di ricerca con query WMI.

-   FIMSyncPasswordSet: i membri di questo gruppo sono autorizzati a eseguire la ricerca di account, a eseguire operazioni di impostazione e di modifica password tramite le interfacce di gestione delle password con WMI.
