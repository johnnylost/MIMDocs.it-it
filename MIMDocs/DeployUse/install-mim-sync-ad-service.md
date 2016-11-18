---
title: Sincronizzare Active Directory e il servizio MIM | Documentazione Microsoft
description: Usare gli agenti di gestione e il servizio Sincronizzazione MIM per sincronizzare i database di Active Directory e MIM.
keywords: 
author: kgremban
ms.author: kgremban
manager: femila
ms.date: 07/21/2016
ms.topic: get-started-article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 5e532b67-64a6-4af6-a806-980a6c11a82d
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 1f545bfb2da0f65c335e37fb9de9c9522bf57f25
ms.openlocfilehash: 59e050c8ccd811586e2da8476f842b853d37f2f1


---

# <a name="install-mim-2016-synchronize-active-directory-and-mim-service"></a>Installare MIM 2016: sincronizzare Active Directory e il servizio MIM

>[!div class="step-by-step"]
[« Servizio e portale MIM](install-mim-service-portal.md)

> [!NOTE]
> Questa procedura dettagliata usa nomi e valori di esempio della società Contoso. Sostituirli con i propri nomi e valori. Ad esempio:
> - Nome del controller di dominio: **mimservername**
> - Nome del dominio: **contoso**
> - Password: **Pass@word1**

Per impostazione predefinita, non sono presenti connettori configurati per il servizio di sincronizzazione MIM (Sincronizzazione).  Il primo passaggio prevede in genere l'uso di Sincronizzazione MIM per popolare il database del servizio MIM con gli account di Active Directory esistenti. A tale scopo, si utilizzerà l'applicazione Servizio di sincronizzazione MIM.

## <a name="create-the-mim-management-agent"></a>Creare l'agente di gestione MIM
L'agente di gestione MIM è un connettore per la sincronizzazione MIM al servizio MIM. Per creare questo connettore, utilizzare la procedura guidata Creazione dell'agente di gestione.

Quando si configura un agente di gestione MIM, è necessario specificare un account utente. Questo documento usa **MIMMA** come nome per l'account.

> [!NOTE]
> L'account utilizzato per l'agente di gestione MIM deve essere lo stesso account specificato durante l'installazione del servizio MIM.

###<a name="to-create-the-mim-ma"></a>Per creare l’agente di gestione MIM

1.  Aprire Synchronization Service Manager.

2.  Per aprire la procedura guidata per la creazione dell'agente di gestione, accedere alla pagina **Azioni di gestione** e quindi, dal menu **Azioni**, selezionare **Crea**.

3.  Nella pagina **Crea agente di gestione** specificare le impostazioni seguenti e fare clic su **Avanti**.

    -   Agente di gestione per: Agente di gestione del servizio FIM

    -   Nome: MIMMA

4.  Nella pagina **Connetti al database** specificare le impostazioni seguenti e fare clic su **Avanti**

    -   Server: localhost

    -   Database: FIMService

    -   Indirizzo di base del servizio MIM: http://localhost:5725

    -   Modalità di autenticazione: Autenticazione integrata di Windows

    -   Nome utente: mimma

    -   Password: Pass@word

    -   Dominio: contoso

5.  Nella pagina **Tipi di oggetto selezionati** verificare che i tipi di oggetto elencati di seguito siano selezionati e fare clic su **Avanti**

    -   DetectedRuleEntry

    -   ExpectedRuleEntry

    -   Group

    -   Person

    -   SynchronizationRule

6.  Nella pagina **Attributi selezionati** selezionare **Mostra tutto**, verificare che tutti gli attributi elencati siano selezionati e fare clic su **Avanti**.

7.  Nella pagina **Configura filtro connettore** fare clic su **Avanti**.

8.  Nella pagina **Configura mapping tipi di oggetto** aggiungere il mapping seguente e fare clic su **Avanti**

    - Nell'elenco **Tipo di oggetto origine dati** selezionare **Persona**.
    - Per aprire la finestra di dialogo Mapping, fare clic su **Aggiungi mapping**.
    - Nell'elenco **Metaverse object type** (Tipo di oggetto metaverse) selezionare **persona**.
    - Fare clic su **OK** per chiudere la finestra di dialogo Mapping.
    - Nell'elenco **Tipo di oggetto origine dati** selezionare **Gruppo**.
    - Per aprire la finestra di dialogo Mapping, fare clic su **Aggiungi mapping**.
    - Nell'elenco **Metaverse object type** (Tipo di oggetto metaverse) selezionare **Gruppo**.
    - Fare clic su **OK** per chiudere la finestra di dialogo Mapping.

9.  Nella pagina **Configure Attribute Flow** (Configura flusso di attributi) creare i mapping di flusso di attributo illustrati in seguito e fare clic su **Avanti**

    -   Selezionare **Persona** come tipi di oggetto origine dati e metaverse.

    -   Selezionare **Diretto** come tipo di mapping.

    -   Per ogni riga della tabella seguente, completare i passaggi qui illustrati:

        -   Selezionare la **direzione del flusso** specificata per la riga della tabella.

        -   Selezionare l’**attributo di origine dati** specificato per la riga della tabella.

        -   Selezionare l’**attributo metaverse** specificato per la riga della tabella.

        -   Per applicare il mapping di flusso, fare clic su **Nuovo**.

    | **Attributo di origine dati** | **Direzione del flusso** | **Attributo metaverse** |
    |-|-|-|
    | AccountName | Export | accountName |
    | DisplayName | Export | displayName |
    | Dominio | Export | dominio |
    | Posta elettronica | Export | mail |
    | ID dipendente | Export | employeeID |
    | EmployeeTipo | Export | employeeTipo |
    | Nome | Export | firstName |
    | Cognome | Export | lastName |
    | ObjectSID | Export | objectSid |

    -   Selezionare **Gruppo** come tipo di origine dati e come tipo di oggetto metaverse.

    -   Selezionare **Diretto** come tipo di mapping.

    -   Per ogni riga della tabella seguente, completare i passaggi qui illustrati:

        -   Selezionare la **direzione del flusso** specificata per la riga della tabella.

        -   Selezionare l’**attributo di origine dati** specificato per la riga della tabella.

        -   Selezionare l’**attributo metaverse** specificato per la riga della tabella.

        -   Per applicare il mapping di flusso, fare clic su **Nuovo**.

    | **Attributo di origine dati** | **Direzione del flusso** | **Attributo metaverse** |
    |-|-|-|
    | AccountName | Export | accountName |
    | DisplayName | Export | displayName |
    | Dominio | Export | dominio |
    | Posta elettronica | Export | mail |
    | MailNickName | Export | mailNickName |
    | Membro | Export | membro |
    | ObjectSID | Export | objectSid |
    | Ambito | Export | scope |
    | Tipo | Export | tipo |
    | MembroshipAddWorkflow | Export | membershipAddWorkflow |
    | MembroshipLocked | Export | membershipLocked |
    | AccountName | Importare | accountName |
    | DisplayedOwner | Importare | displayedOwner |
    | DisplayName | Importare | displayName |
    | MailNickName | Importare | mailNickName |
    | Membro | Importare | membro |
    | Ambito | Importare | scope |
    | Tipo | Importare | tipo |

10.  Nella pagina **Configura deprovisioning** fare clic su **Avanti**

11.  Per creare l'agente di gestione, nella pagina **Configura estensioni** fare clic su **Fine**.

## <a name="create-the-ad-management-agent"></a>Creare l'agente di gestione di Active Directory
L'agente di gestione di Active Directory è un connettore per i Servizi di dominio Active Directory. Per creare questo connettore, utilizzare la procedura guidata Creazione dell'agente di gestione.

1. Per aprire la procedura guidata Creazione dell'agente di gestione, fare clic su **Crea** dal menu **Azioni**.

2. Nella pagina **Crea agente di gestione** specificare le impostazioni seguenti e fare clic su **Avanti**:

    - Agente di gestione per: Servizi di dominio Active Directory
    - Nome: ADMA

3. Nella pagina **Connetti a foresta Active Directory** specificare le impostazioni seguenti e fare clic su **Avanti**:

    - Nome foresta: contoso.local
    - Nome utente: administrator
    - Password : &lt;la password dell'account&gt;
    - Dominio: contoso

4. Nella pagina **Configura partizioni di directory** specificare le impostazioni seguenti e fare clic su **Avanti**:

    - Nell'elenco **Seleziona partizioni di directory** selezionare **DC=CONTOSO, DC=local**.

    - Per aprire la finestra di dialogo Seleziona contenitori, fare clic su **Contenitori**.

    - Per modificare il contenitore in modo che includa solo oggetti di gestione MIM, fare clic sul nodo **DC=CONTOSO,DC=local** e quindi fare clic sul nodo per il contenitore di interesse.

    - Per chiudere la finestra di dialogo Seleziona contenitori, fare clic su **OK**.

5. Nella pagina **Configura gerarchia di provisioning** fare clic su **Avanti**.

6. Nella pagina **Seleziona tipi di oggetto** specificare le impostazioni seguenti e fare clic su **Avanti**:

    - Nell'elenco **Tipi di oggetto** selezionare **utente** e **gruppo**.

7. Nella pagina **Seleziona attributi** selezionare **Mostra tutto**, scegliere gli attributi seguenti e quindi fare clic su **Avanti**:

    -   company
    -   displayName
    -   employeeID
    -   employeeType
    -   givenName
    -   groupTipo
    -   managedBy
    -   manager
    -   membro
    -   objectSid
    -   sAMAccountName
    -   sAMAccountTipo
    -   sn
    -   unicodePwd
    -   userAccountControl

8. Nella pagina **Configura filtro connettore** fare clic su **Avanti**.

9. Nella pagina **Configura regole di unione e proiezione** fare clic su **Avanti**.

10. Nella pagina **Configura flusso di attributi** fare clic su **Avanti**.

11. Nella pagina **Configura deprovisioning** fare clic su **Avanti**.

12. Nella pagina **Configura estensioni** fare clic su **Fine**.


## <a name="create-run-profiles"></a>Creare profili di esecuzione

Creare profili di esecuzione per i connettori ADMA e MIMMA.

### <a name="create-run-profiles-for-the-adma-connector"></a>Creare profili di esecuzione per il connettore ADMA

Questa tabella mostra i cinque profili di esecuzione che verranno creati per il connettore ADMA:

| Nome | Tipo |
| ---- | ---- |
| Profilo 1 | Importazione completa (solo temporanea) |
| Profilo 2 | Sincronizzazione completa |
| Profilo 3 | Importazione delta (solo temporanea) |
| Profilo 4 | Sincronizzazione differenziale |
| Profilo 5 | Export |

Per creare profili di esecuzione per il connettore ADMA:

1. Aprire Synchronization Service Manager e fare clic su **Agenti di gestione** dal menu **Strumenti**.

2. Nell'elenco **Agenti di gestione** selezionare **ADMA**.

3. Per aprire la finestra di dialogo Configura profili di esecuzione, fare clic su **Configura profili di esecuzione** dal menu **Azioni**.

4. Per ogni profilo di esecuzione incluso nella tabella completare i passaggi seguenti:

    - Per aprire la procedura guidata Configura profilo di esecuzione, fare clic su **Nuovo profilo**.

    - Nella casella **Nome** digitare il nome del profilo mostrato nella tabella e fare clic su **Avanti**.

    - Nell'elenco **Tipo** selezionare il tipo di passaggio mostrato nella tabella e fare clic su **Avanti**.

    - Fare clic su **Fine** per creare il profilo di esecuzione.

5. Per chiudere la finestra di dialogo Configura profili di esecuzione, fare clic su **OK**.

### <a name="create-run-profiles-for-the-mimma-connector"></a>Creare profili di esecuzione per il connettore MIMMA

Questa tabella mostra i cinque profili di esecuzione corrispondenti per il connettore MIMMA:

| Nome | Tipo |
| -------- | -------- |
| Profilo 1 | Importazione completa (solo temporanea) |
| Profilo 2 | Sincronizzazione completa |
| Profilo 3 | Importazione delta (solo temporanea) |
| Profilo 4 | Sincronizzazione differenziale |
| Profilo 5 | Export |

Per creare profili di esecuzione per il connettore MIMMA:

1. Aprire Synchronization Service Manager e fare clic su **Agenti di gestione** dal menu **Strumenti**.

2. Nell'elenco **Agenti di gestione** selezionare **MIMMA**.

3. Per aprire la finestra di dialogo Configura profili di esecuzione, fare clic su **Configura profili di esecuzione** dal menu **Azioni**.

4. Per ogni profilo di esecuzione incluso nella tabella completare i passaggi seguenti:

    - Per aprire la procedura guidata Configura profilo di esecuzione, fare clic su **Nuovo profilo**.

    - Nella casella **Nome** digitare il nome del profilo mostrato nella tabella e fare clic su **Avanti**.

    - Nell'elenco **Tipo** selezionare il tipo di passaggio mostrato nella tabella e fare clic su **Avanti**.

    - Fare clic su **Fine** per creare il profilo di esecuzione.

5. Per chiudere la finestra di dialogo Configura profili di esecuzione, fare clic su **OK**.

## <a name="configure-the-mim-service"></a>Configurare il servizio MIM

Usando il portale MIM verrà creata la regola di sincronizzazione in entrata degli utenti di Active Directory per il servizio MIM.

Per creare la regola di sincronizzazione in entrata per gli utenti di Active Directory:

1. Nella pagina iniziale del portale MIM scegliere **Amministrazione** nella barra di spostamento.

2. Per aprire la pagina Regole di sincronizzazione, fare clic su **Regole di sincronizzazione**.

3. Per aprire la procedura guidata Crea regola di sincronizzazione, fare clic su **Nuovo** nella barra degli strumenti.

4. Nella scheda **Generale** specificare le informazioni seguenti e fare clic su **Avanti**:

    -   Nome visualizzato: Regola di sincronizzazione in entrata per utenti AD
    -   Direzione del flusso di dati: In entrata

5. Nella scheda **Ambito** specificare le informazioni seguenti e fare clic su **Avanti**:

    -   Tipo di risorsa metaverse: persona
    -   Sistema esterno: ADMA
    -   Tipo di risorsa sistema esterno: utente

6. Nella scheda **Relazione** specificare le informazioni seguenti e fare clic su **Avanti**:

    -   Per configurare i criteri di relazione, selezionare **ObjectSID** dall'elenco MetaverseObject:person(Attribute) e dall'elenco ConnectedSystemObject:person(Attribute).

    -   Selezionare **Crea risorsa in FIM**.

7. Nella pagina **Flusso in ingresso dell'attributo** specificare le informazioni seguenti e fare clic su **Avanti**:

    | Regola di flusso | Origine | Destination |
    |-|-|-|
    |Regola 1|samAccountName|accountName|
    |Regola 2|displayName|displayName|
    |Regola 3|EmployeeTipo|employeeTipo|
    |Regola 4|givenName|firstName|
    |Regola 5|sn|lastName|
    |Regola 6|Manager|manager|
    |Regola 7|objectSID|ObjectSID|
    |Regola 8|"Contoso"|dominio|

    Per ogni riga di questa tabella, eseguire i passaggi seguenti:

    - Per aprire la finestra di dialogo Definizione di flusso, fare clic su **Nuovo flusso dell'attributo**.

    - Nella scheda **Origine** selezionare l'attributo mostrato per la riga della tabella.

    - Nella scheda **Destinazione** selezionare l'attributo mostrato per la riga della tabella.

    - Per applicare la configurazione del flusso dell'attributo, fare clic su **OK**.

8. Nella scheda **Riepilogo** fare clic su **Invia**.

## <a name="initialize-the-testing-environment"></a>Inizializzare l'ambiente di test
Prima di poter testare la configurazione MIM con i dati di AD, è necessario eseguire quattro passaggi:

### <a name="enable-provisioning"></a>Abilitare il provisioning

1. Aprire Synchronization Service Manager.

2. Per aprire la finestra di dialogo Opzioni fare clic su **Opzioni** dal menu **Strumenti**

3. Selezionare **Abilita provisioning regola di sincronizzazione**.

4. Per chiudere la finestra di dialogo Opzioni, fare clic su **OK**.

### <a name="initialize-the-mimma"></a>Inizializzare MIMMA

Eseguire un ciclo di sincronizzazione completa su questo connettore. Il ciclo completo è costituito dalle esecuzioni dei profili di esecuzione seguenti:

- Importazione completa
- Sincronizzazione completa
- Export
- Importazione delta

Seguire questa procedura per eseguire ognuno dei quattro profili di esecuzione.

1. Aprire Synchronization Service Manager e fare clic su **Agenti di gestione** dal menu **Strumenti**.

2. Nell'elenco **Agenti di gestione** selezionare **MIMMA**.

3. Per aprire la finestra di dialogo Esegui agente di gestione, fare clic su **Esegui** dal menu **Azioni**.

4. Per ogni profilo di esecuzione indicato sopra, completare i passaggi seguenti:

    - Per aprire la finestra di dialogo Esegui agente di gestione, fare clic su **Esegui** dal menu **Azioni**.

    - Nell'elenco **Esegui profili** selezionare il profilo di esecuzione da configurare.

    - Per avviare il profilo di esecuzione, fare clic su **OK**.

#### <a name="configure-attribute-flow-precedence"></a>Configurare la precedenza del flusso dell'attributo

Durante l'inizializzazione del connettore MIM, le regole di sincronizzazione configurate sono state introdotte nel metaverse.

Regolare la precedenza del flusso dell’attributo per gli attributi forniti da questo connettore in modo da garantire che gli attributi già presenti in Active Directory possano essere trasmessi al metaverse e successivamente al database del servizio MIM.

### <a name="initialize-the-adma"></a>Inizializzare ADMA

Per inizializzare il connettore di Active Directory, è necessario eseguire un'importazione completa e una sincronizzazione completa su connettore stesso. L'importazione completa sposta gli oggetti esistenti da AD nello spazio del connettore. La sincronizzazione completa aggiorna le regole di sincronizzazione affinché corrispondano a quelle del connettore MIM.

1. Aprire Synchronization Service Manager e fare clic su **Agenti di gestione** dal menu **Strumenti**.

2. Nell'elenco **Agenti di gestione** selezionare **ADMA**.

3. Per aprire la finestra di dialogo Esegui agente di gestione, fare clic su **Esegui** dal menu **Azioni**.

4. Per ogni profilo di esecuzione indicato sopra, completare i passaggi seguenti:

    - Per aprire la finestra di dialogo Esegui agente di gestione, fare clic su **Esegui** dal menu **Azioni**.

    - Nell'elenco **Esegui profili** selezionare il profilo di esecuzione da configurare.

    - Per avviare il profilo di esecuzione, fare clic su **OK**.

### <a name="populate-the-mim-service-database"></a>Popolare il database del servizio MIM

Per popolare il database del servizio MIM con gli oggetti, è necessario eseguire un ciclo di sincronizzazione nel connettore MIMMA. Il ciclo è costituito da:

- Export
- Importazione completa
- Sincronizzazione completa

Seguire questa procedura per eseguire ognuno dei tre profili di esecuzione.

1. Aprire Synchronization Service Manager e fare clic su **Management Agents** (Agenti di gestione) dal menu **Strumenti**.

2. Nell'elenco **Agenti di gestione** (Agenti di gestione) selezionare **MIMMA** (MIMMA).

3. Per aprire la finestra di dialogo Run Management Agent (Esegui agente di gestione), fare clic su **Esegui** dal menu **Azioni**.

4. Per ogni profilo di esecuzione indicato sopra, completare i passaggi seguenti:

    - Per aprire la finestra di dialogo Run Management Agent (Esegui agente di gestione), fare clic su **Esegui** dal menu **Azioni**.
    - Nell'elenco **Run profiles** (Esegui profili) selezionare il profilo di esecuzione da configurare.
    - Per avviare il profilo di esecuzione, fare clic su **OK**.

>[!div class="step-by-step"]
[« Servizio e portale MIM](install-mim-service-portal.md)



<!--HONumber=Nov16_HO2-->


