---
# required metadata

title: Installare MIM 2016 & #58; Sincronizzare Active Directory e il servizio MIM | Microsoft Identity Manager
description: Usare gli agenti di gestione e il servizio Sincronizzazione MIM per sincronizzare i database di Active Directory e MIM.
keywords:
author: kgremban
manager: stevenpo
ms.date: 04/28/2016
ms.topic: get-started-article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 5e532b67-64a6-4af6-a806-980a6c11a82d

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Installare MIM 2016: sincronizzare Active Directory e il servizio MIM

>[!div class="step-by-step"]
[« Servizio e portale MIM](install-mim-service-portal.md)

> [!NOTE]
> In tutti gli esempi riportati di seguito **mimservername** rappresenta il nome del controller di dominio, **contoso** rappresenta il nome di dominio e **Pass@word1** rappresenta una password di esempio.

Per impostazione predefinita, non sono presenti connettori configurati per il servizio di sincronizzazione MIM (Sincronizzazione).  Il primo passaggio prevede in genere l'uso di Sincronizzazione MIM per popolare il database del servizio MIM con gli account di Active Directory esistenti. A tale scopo, si utilizzerà l'applicazione Servizio di sincronizzazione MIM.

## Creare l'agente di gestione MIM
L'agente di gestione MIM è un connettore per la sincronizzazione MIM al servizio MIM. Per creare questo connettore, utilizzare la procedura guidata Creazione dell'agente di gestione.

Quando si configura un agente di gestione MIM, è necessario specificare un account utente. Questo documento usa **MIMMA** come nome per l'account.

> [!CAUTION]
> L'account utilizzato per l'agente di gestione MIM deve essere lo stesso account specificato durante l'installazione del servizio MIM.

###Per creare l’agente di gestione MIM

1.  Aprire Synchronization Service Manager.

2.  Per aprire la procedura guidata Creazione dell'agente di gestione, fare clic su **Crea** dal menu **Azioni**.

3.  Nella pagina **Crea agente di gestione** specificare le impostazioni seguenti e fare clic su **Avanti**.

    -   Agente di gestione per: agente di gestione per il servizio MIM

    -   Nome: MIMMA

4.  Nella pagina **Connetti al database** specificare le impostazioni seguenti e fare clic su **Avanti**

    -   Server: localhost

    -   Database: MIMService

    -   Indirizzo di base del servizio MIM: http://localhost:5725

    -   Modalità di autenticazione: Autenticazione integrata di Windows

    -   Nome utente: mimma

    -   Password: Pass@word

    -   Dominio: contoso

5.  Nella pagina **Tipi di oggetto selezionati** verificare che i tipi di oggetto elencati di seguito siano selezionati e fare clic su **Avanti**

    -   ExpectedRuleEntry

    -   DetectedRuleEntry

    -   SynchronizationRule

    -   Person

    -   Gruppo

6.  Nella pagina **Attributi selezionati** verificare che tutti gli attributi elencati siano selezionati e fare clic su **Avanti**.

7.  Nella pagina **Configura filtro connettore** fare clic su **Avanti**.

8.  Nella pagina **Configura mapping tipi di oggetto** aggiungere il mapping seguente e fare clic su **Avanti**

    -   Nell'elenco **Tipo di oggetto origine dati** selezionare **Persona**.

    -   Per aprire la finestra di dialogo Mapping, fare clic su **Aggiungi mapping**.

    -   Nell'elenco **Tipo di oggetto metaverse** selezionare **persona**.

    -   Per chiudere la finestra di dialogo Mapping fare clic su **OK**.

9.  Nella pagina **Configura flusso di attributi** applicare i seguenti mapping di flusso di attributo e fare clic su **Avanti**

    ||||
    |-|-|-|
    | **Direzione del flusso** | **Attributo di origine dati** | **Attributo metaverse** |
    |Importare|Importare|accountName|
    |Importare|Importare|company|
    |Importare|Importare|displayName|
    |Importare|Importare|employeeID|
    |Importare|Importare|employeeTipo|
    |Importare|Importare|firstName|
    |Importare|Importare|lastName|
    |Importare|Importare|Manager|
    |Importare|Importare|objectSid|
    |Esportare|Esportare|accountName|
    |Esportare|Esportare|company|
    |Esportare|Esportare|displayName|
    |Esportare|Esportare|dominio|
    |Esportare|Esportare|employeeID|
    |Esportare|Esportare|employeeTipo|
    |Esportare|Esportare|firstName|
    |Esportare|Esportare|lastName|
    |Esportare|Export|manager|
    |Export|Esportare|objectSid|

10.  Selezionare **Persona** come tipo di oggetto di origine dati.

    -   Select **Person** as the Metaverse object type.

    -   Select **Direct** as the Mapping Type.

    -   For each row in the previous table, complete the following steps:

        -   Select the **Flow direction** shown for that row in the table.

        -   Select the **Data source attribute** shown for that row in the table.

        -   Select the **Metaverse attribute** shown for that row in the table.

        -   To apply the flow mapping, click **New**.

    -   Select **Group** as the data source type and as the metaverse object type.

    -   Select **Direct** as the Mapping Type.

    -   For each row in the following table, complete these steps:

        -   Select the **Flow direction** shown for that row in the table.

        -   Select the **Data source attribute** shown for that row in the table.

        -   Select the **Metaverse attribute** shown for that row in the table.

        -   To apply the flow mapping, click **New**.

    | Flow Direction | Data Source Attribute | Metaverse Attribute |
    |-|-|-|
    | Export | AccountName | accountName |
    | Export | DisplayName | displayName |
    | Export | Domain | domain |
    | Export | Scope | scope |
    | Export | Type | type |
    | Export | Member | member |
    | Export | MembershipLocked | membershipLocked |
    | Export | MembershipAddWorkflow | membershipAddWorkflow |
    | Export | Manager | manager |

11.  Nella pagina **Configura deprovisioning** fare clic su **Avanti**

12.  Per creare l'agente di gestione, nella pagina **Configura estensioni** fare clic su **Fine**.

## Creare l'agente di gestione di Active Directory
L'agente di gestione di Active Directory è un connettore per i Servizi di dominio Active Directory. Per creare questo connettore, utilizzare la procedura guidata Creazione dell'agente di gestione.

1. Per aprire la procedura guidata Creazione dell'agente di gestione, fare clic su **Crea** dal menu **Azioni**.

2. Nella pagina **Crea agente di gestione** specificare le impostazioni seguenti e fare clic su **Avanti**:

    - Agente di gestione per: Servizi di dominio Active Directory
    - Nome: ADMA

3. Nella pagina **Connetti a foresta Active Directory** specificare le impostazioni seguenti e fare clic su **Avanti**:

    - Nome foresta: contoso.local
    - Nome utente: administrator
    - Password: &lt;la password dell'account&gt;
    - Dominio: contoso

4. Nella pagina **Configura partizioni di directory** specificare le impostazioni seguenti e fare clic su **Avanti**:

    - Nell'elenco **Seleziona partizioni di directory** selezionare **DC=CONTOSO, DC=local**.

    - Per aprire la finestra di dialogo Seleziona contenitori, fare clic su **Contenitori**.

    - Per modificare il contenitore in modo che includa solo oggetti di gestione MIM, fare clic sul nodo **DC=CONTOSO,DC=local** e quindi fare clic sul nodo per il contenitore di interesse.

    - Per chiudere la finestra di dialogo Seleziona contenitori, fare clic su **OK**.

5. Nella pagina **Configura gerarchia di provisioning** fare clic su **Avanti**.

6. Nella pagina **Seleziona tipi di oggetto** specificare le impostazioni seguenti e fare clic su **Avanti**:

    - Nell'elenco **Tipi di oggetto** selezionare **utente** e **gruppo**.

7. Nella pagina **Seleziona attributi** specificare le impostazioni seguenti e fare clic su **Avanti**:

    - Selezionare **Mostra tutto**.

8. Nell'elenco **Attributi** selezionare gli attributi seguenti:

    -   company
    -   displayName
    -   employeeID
    -   employeeTipo
    -   givenName
    -   groupType
    -   manager
    -   managedBy
    -   membro
    -   objectSid
    -   sAMAccountName
    -   sAMAccountTipo
    -   sn
    -   unicodePwd
    -   userAccountControl

9. Nella pagina **Configura filtro connettore** fare clic su **Avanti**.

10. Nella pagina **Configura regole di unione e proiezione** fare clic su **Avanti**.

11. Nella pagina **Configura flusso di attributi** fare clic su **Avanti**.

12. Nella pagina **Configura deprovisioning** fare clic su **Avanti**.

13. Nella pagina **Configura estensioni** fare clic su **Fine**.


## Creare profili di esecuzione

Creare profili di esecuzione per i connettori ADMA e MIMMA.

### Creare profili di esecuzione per il connettore ADMA

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

### Creare profili di esecuzione per il connettore MIMMA

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

## Configurazione del servizio MIM

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
    -   Tipo di risorsa sistema esterno: persona

6. Nella scheda **Relazione** specificare le informazioni seguenti e fare clic su **Avanti**:

    -   Per configurare i criteri di relazione, selezionare **ObjectSID** dall'elenco MetaverseObject:person(Attribute) e dall'elenco ConnectedSystemObject:person(Attribute).

    -   Selezionare **Crea risorsa in MIM**.

7. Nella pagina **Flusso in ingresso dell'attributo** specificare le informazioni seguenti e fare clic su **Avanti**:

    | Regola di flusso | Origine | Destination |
    |-|-|-|
    |Regola 1|samAccountName|f|
    |Regola 2|displayName|displayName|
    |Regola 3|EmployeeTipo|EmployeeTipo|
    |Regola 4|givenName|givenName|
    |Regola 5|sn|lastName|
    |Regola 6|Manager|manager|
    |Regola 7|objectSID|ObjectSID|
    |Regola 8|"Contoso"|dominio|

    Per ogni riga di questa tabella, eseguire i passaggi seguenti:

    -   Per aprire la finestra di dialogo Definizione di flusso, fare clic su **Nuovo flusso dell'attributo**.

    -   Nella scheda **Origine** selezionare l'attributo mostrato per la riga della tabella.

    -   Nella scheda **Destinazione** selezionare l'attributo mostrato per la riga della tabella.

    -   Per applicare la configurazione del flusso dell'attributo, fare clic su **OK**.

8. Nella scheda **Riepilogo** fare clic su **Invia**.

## Inizializzazione dell'ambiente di test
Prima di poter verificare la configurazione con i dati di Active Directory, è necessario inizializzare la configurazione. Questo processo prevede quattro passaggi:

### Abilitare il provisioning

1. Aprire Synchronization Service Manager.

2. Per aprire la finestra di dialogo Opzioni fare clic su **Opzioni** dal menu **Strumenti**

3. Selezionare **Abilita provisioning regola di sincronizzazione**.

4. Per chiudere la finestra di dialogo Opzioni, fare clic su **OK**.

### Inizializzare MIMMA

Eseguire un ciclo di sincronizzazione completa su questo connettore. Il ciclo completo è costituito dalle esecuzioni dei profili di esecuzione seguenti:

    -   Full Import

    -   Full Synchronization

    -   Export

    -   Delta Import

1. Aprire Synchronization Service Manager e fare clic su **Agenti di gestione** dal menu **Strumenti**.

2. Nell'elenco **Agenti di gestione** selezionare **MIMMA**.

3. Per aprire la finestra di dialogo Esegui agente di gestione, fare clic su **Esegui** dal menu **Azioni**.

4. Per ogni profilo di esecuzione indicato sopra, completare i passaggi seguenti:

    - Per aprire la finestra di dialogo Esegui agente di gestione, fare clic su **Esegui** dal menu **Azioni**.

    - Nell'elenco **Esegui profili** selezionare il profilo di esecuzione da configurare.

    - Per avviare il profilo di esecuzione, fare clic su **OK**.

#### Configurazione della precedenza del flusso dell’attributo

Durante l'inizializzazione del connettore MIM, le regole di sincronizzazione configurate sono state introdotte nel metaverse.

Regolare la precedenza del flusso dell’attributo per gli attributi forniti da questo connettore in modo da garantire che gli attributi già presenti in Active Directory possano essere trasmessi al metaverse e successivamente al database del servizio MIM.

### Inizializzazione di ADMA

Per inizializzare il connettore di Active Directory, è necessario eseguire un'importazione completa e una sincronizzazione completa su connettore stesso. L’importazione completa è necessaria per portare gli oggetti esistenti da Active Directory nello spazio del connettore. La sincronizzazione completa è necessaria perché le regole di sincronizzazione sono state modificate proiettando le nuove regole di sincronizzazione dallo spazio connettore MIM nel metaverse. Effettuare le operazioni seguenti

    -   Full Import

    -   Full Synchronization

1. Aprire Synchronization Service Manager e fare clic su **Agenti di gestione** dal menu **Strumenti**.

2. Nell'elenco **Agenti di gestione** selezionare **ADMA**.

3. Per aprire la finestra di dialogo Esegui agente di gestione, fare clic su **Esegui** dal menu **Azioni**.

4. Per ogni profilo di esecuzione indicato sopra, completare i passaggi seguenti:

    - Per aprire la finestra di dialogo Esegui agente di gestione, fare clic su **Esegui** dal menu **Azioni**.

    - Nell'elenco **Esegui profili** selezionare il profilo di esecuzione da configurare.

    - Per avviare il profilo di esecuzione, fare clic su **OK**.

### Popolamento del database del servizio MIM

Per popolare il database del servizio MIM con gli oggetti, è necessario eseguire un ciclo di sincronizzazione nel connettore MIMMA. Il ciclo consiste nelle esecuzioni dei seguenti profili di esecuzione:

    -   Export

    -   Full Import

    -   Full Sync

    1. Open the Synchronization Service Manager and, on the **Tools** menu, click **Management Agents**.

    2. In the **Management Agents** list, select **MIMMA**.

    3. To open the Run Management Agent dialog box, on the **Actions** menu, click **Run**.

    4. For each run profile listed above, complete the following steps:

        - To open the Run Management Agent dialog box, on the **Actions** menu, click **Run**.

        - In the **Run profiles** list, select the run profile you want to run.

        - To start the run profile, click **OK**.

>[!div class="step-by-step"]
[« Servizio e portale MIM](install-mim-service-portal.md)


<!--HONumber=Apr16_HO2-->


