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

> [!NOTE]
> « Servizio e portale MIM Questa procedura dettagliata usa nomi e valori di esempio della società Contoso. Sostituirli con i propri nomi e valori.
> - Ad esempio:
> - Nome del controller di dominio: **mimservername**
> - Nome del dominio: **contoso**

Password: **Pass@word1**  Per impostazione predefinita, non sono presenti connettori configurati per il servizio di sincronizzazione MIM (Sincronizzazione). Il primo passaggio prevede in genere l'uso di Sincronizzazione MIM per popolare il database del servizio MIM con gli account di Active Directory esistenti.

## A tale scopo, si utilizzerà l'applicazione Servizio di sincronizzazione MIM.
Creare l'agente di gestione MIM L'agente di gestione MIM è un connettore per la sincronizzazione MIM al servizio MIM.

Per creare questo connettore, utilizzare la procedura guidata Creazione dell'agente di gestione. Quando si configura un agente di gestione MIM, è necessario specificare un account utente.

> Questo documento usa **MIMMA** come nome per l'account.

###L'account utilizzato per l'agente di gestione MIM deve essere lo stesso account specificato durante l'installazione del servizio MIM.

1.  Per creare l’agente di gestione MIM

2.  Aprire Synchronization Service Manager.

3.  Per aprire la procedura guidata Creazione dell'agente di gestione, fare clic su **Crea** dal menu **Azioni**.

    -   Nella pagina **Crea agente di gestione** specificare le impostazioni seguenti e fare clic su **Avanti**.

    -   Agente di gestione per: agente di gestione per il servizio MIM

4.  Nome: MIMMA

    -   Nella pagina **Connetti al database** specificare le impostazioni seguenti e fare clic su **Avanti**

    -   Server: localhost

    -   Database: MIMService

    -   Indirizzo di base del servizio MIM: http://localhost:5725

    -   Modalità di autenticazione: Autenticazione integrata di Windows

    -   Nome utente: mimma

    -   Password: Pass@word

5.  Dominio: contoso

    -   Nella pagina **Tipi di oggetto selezionati** verificare che i tipi di oggetto elencati di seguito siano selezionati e fare clic su **Avanti**

    -   ExpectedRuleEntry

    -   DetectedRuleEntry

    -   SynchronizationRule

    -   Person

6.  Gruppo

7.  Nella pagina **Attributi selezionati** verificare che tutti gli attributi elencati siano selezionati e fare clic su **Avanti**.

8.  Nella pagina **Configura filtro connettore** fare clic su **Avanti**.

    - Nella pagina **Configura mapping tipi di oggetto** aggiungere il mapping seguente e fare clic su **Avanti**
    - Nell'elenco **Tipo di oggetto origine dati** selezionare **Persona**.
    - Per aprire la finestra di dialogo Mapping, fare clic su **Aggiungi mapping**.
    - Nell'elenco **Metaverse object type** (Tipo di oggetto metaverse) selezionare **persona**.

9.  Fare clic su **OK** per chiudere la finestra di dialogo Mapping.

    | **Nella pagina **Configura flusso di attributi** applicare i seguenti mapping di flusso di attributo e fare clic su **Avanti**** | **Direzione del flusso** | **Attributo di origine dati** |
    |-|-|-|
    |Attributo metaverse|Importare|Importare|
    |accountName|Importare|Importare|
    |company|Importare|Importare|
    |displayName|Importare|Importare|
    |employeeID|Importare|Importare|
    |employeeTipo|Importare|Importare|
    |firstName|Importare|Importare|
    |lastName|Importare|Importare|
    |Manager|Importare|Importare|
    |objectSid|Esportare|Esportare|
    |accountName|Esportare|Esportare|
    |company|Esportare|Esportare|
    |displayName|Esportare|Esportare|
    |dominio|Esportare|Esportare|
    |employeeID|Esportare|Esportare|
    |employeeTipo|Esportare|Esportare|
    |firstName|Esportare|Esportare|
    |lastName|Esportare|Export|
    |manager|Export|Esportare|

10.  objectSid

    -   Selezionare **Persona** come tipo di oggetto di origine dati.

    -   Nella pagina **Configura deprovisioning** fare clic su **Avanti**

    -   Per creare l'agente di gestione, nella pagina **Configura estensioni** fare clic su **Fine**.

        -   Creare l'agente di gestione di Active Directory

        -   L'agente di gestione di Active Directory è un connettore per i Servizi di dominio Active Directory.

        -   Per creare questo connettore, utilizzare la procedura guidata Creazione dell'agente di gestione.

        -   Per aprire la procedura guidata Creazione dell'agente di gestione, fare clic su **Crea** dal menu **Azioni**.

    -   Nella pagina **Crea agente di gestione** specificare le impostazioni seguenti e fare clic su **Avanti**:

    -   Agente di gestione per: Servizi di dominio Active Directory

    -   Nome: ADMA

        -   Nella pagina **Connetti a foresta Active Directory** specificare le impostazioni seguenti e fare clic su **Avanti**:

        -   Nome foresta: contoso.local

        -   Nome utente: administrator

        -   Password: &lt;la password dell'account&gt;

    Dominio: contoso

11.  Nella pagina **Configura partizioni di directory** specificare le impostazioni seguenti e fare clic su **Avanti**:

12.  Nell'elenco **Seleziona partizioni di directory** selezionare **DC=CONTOSO, DC=local**.

## Per aprire la finestra di dialogo Seleziona contenitori, fare clic su **Contenitori**.
Per modificare il contenitore in modo che includa solo oggetti di gestione MIM, fare clic sul nodo **DC=CONTOSO,DC=local** e quindi fare clic sul nodo per il contenitore di interesse. Per chiudere la finestra di dialogo Seleziona contenitori, fare clic su **OK**.

1. Nella pagina **Configura gerarchia di provisioning** fare clic su **Avanti**.

2. Nella pagina **Seleziona tipi di oggetto** specificare le impostazioni seguenti e fare clic su **Avanti**:

    - Nell'elenco **Tipi di oggetto** selezionare **utente** e **gruppo**.
    - Nella pagina **Seleziona attributi** specificare le impostazioni seguenti e fare clic su **Avanti**:

3. Selezionare **Mostra tutto**.

    - Nell'elenco **Attributi** selezionare gli attributi seguenti:
    - company
    - displayName&gt;
    - employeeID

4. employeeTipo

    - givenName

    - groupType

    - manager

    - managedBy

5. membro

6. objectSid

    - sAMAccountName

7. sAMAccountTipo

    - sn

8. unicodePwd

    -   userAccountControl
    -   Nella pagina **Configura filtro connettore** fare clic su **Avanti**.
    -   Nella pagina **Configura regole di unione e proiezione** fare clic su **Avanti**.
    -   Nella pagina **Configura flusso di attributi** fare clic su **Avanti**.
    -   Nella pagina **Configura deprovisioning** fare clic su **Avanti**.
    -   Nella pagina **Configura estensioni** fare clic su **Fine**.
    -   Creare profili di esecuzione
    -   Creare profili di esecuzione per i connettori ADMA e MIMMA.
    -   Creare profili di esecuzione per il connettore ADMA
    -   Questa tabella mostra i cinque profili di esecuzione che verranno creati per il connettore ADMA:
    -   Nome
    -   Tipo
    -   Profilo 1
    -   Importazione completa (solo temporanea)
    -   Profilo 2

9. Sincronizzazione completa

10. Profilo 3

11. Importazione delta (solo temporanea)

12. Profilo 4

13. Sincronizzazione differenziale


## Profilo 5

Export

### Per creare profili di esecuzione per il connettore ADMA:

Aprire Synchronization Service Manager e fare clic su **Agenti di gestione** dal menu **Strumenti**.

| Nell'elenco **Agenti di gestione** selezionare **ADMA**. | Per aprire la finestra di dialogo Configura profili di esecuzione, fare clic su **Configura profili di esecuzione** dal menu **Azioni**. |
| ---- | ---- |
| Per ogni profilo di esecuzione incluso nella tabella completare i passaggi seguenti: | Per aprire la procedura guidata Configura profilo di esecuzione, fare clic su **Nuovo profilo**. |
| Nella casella **Nome** digitare il nome del profilo mostrato nella tabella e fare clic su **Avanti**. | Nell'elenco **Tipo** selezionare il tipo di passaggio mostrato nella tabella e fare clic su **Avanti**. |
| Fare clic su **Fine** per creare il profilo di esecuzione. | Per chiudere la finestra di dialogo Configura profili di esecuzione, fare clic su **OK**. |
| Creare profili di esecuzione per il connettore MIMMA | Questa tabella mostra i cinque profili di esecuzione corrispondenti per il connettore MIMMA: |
| Nome | Tipo |

Profilo 1

1. Importazione completa (solo temporanea)

2. Profilo 2

3. Sincronizzazione completa

4. Profilo 3

    - Importazione delta (solo temporanea)

    - Profilo 4

    - Sincronizzazione differenziale

    - Profilo 5

5. Export

### Per creare profili di esecuzione per il connettore MIMMA:

Aprire Synchronization Service Manager e fare clic su **Agenti di gestione** dal menu **Strumenti**.

| Nell'elenco **Agenti di gestione** selezionare **MIMMA**. | Per aprire la finestra di dialogo Configura profili di esecuzione, fare clic su **Configura profili di esecuzione** dal menu **Azioni**. |
| -------- | -------- |
| Per ogni profilo di esecuzione incluso nella tabella completare i passaggi seguenti: | Per aprire la procedura guidata Configura profilo di esecuzione, fare clic su **Nuovo profilo**. |
| Nella casella **Nome** digitare il nome del profilo mostrato nella tabella e fare clic su **Avanti**. | Nell'elenco **Tipo** selezionare il tipo di passaggio mostrato nella tabella e fare clic su **Avanti**. |
| Fare clic su **Fine** per creare il profilo di esecuzione. | Per chiudere la finestra di dialogo Configura profili di esecuzione, fare clic su **OK**. |
| Configurare il servizio MIM | Usando il portale MIM verrà creata la regola di sincronizzazione in entrata degli utenti di Active Directory per il servizio MIM. |
| Per creare la regola di sincronizzazione in entrata per gli utenti di Active Directory: | Nella pagina iniziale del portale MIM scegliere **Amministrazione** nella barra di spostamento. |

Per aprire la pagina Regole di sincronizzazione, fare clic su **Regole di sincronizzazione**.

1. Per aprire la procedura guidata Crea regola di sincronizzazione, fare clic su **Nuovo** nella barra degli strumenti.

2. Nella scheda **Generale** specificare le informazioni seguenti e fare clic su **Avanti**:

3. Nome visualizzato: Regola di sincronizzazione in entrata per utenti AD

4. Direzione del flusso di dati: In entrata

    - Nella scheda **Ambito** specificare le informazioni seguenti e fare clic su **Avanti**:

    - Tipo di risorsa metaverse: persona

    - Sistema esterno: ADMA

    - Tipo di risorsa sistema esterno: persona

5. Nella scheda **Relazione** specificare le informazioni seguenti e fare clic su **Avanti**:

## Per configurare i criteri di relazione, selezionare **ObjectSID** dall'elenco MetaverseObject:person(Attribute) e dall'elenco ConnectedSystemObject:person(Attribute).

Selezionare **Crea risorsa in MIM**.

Nella pagina **Flusso in ingresso dell'attributo** specificare le informazioni seguenti e fare clic su **Avanti**:

1. Regola di flusso

2. Origine

3. Destination

4. Regola 1

    -   samAccountName
    -   f

5. Regola 2

    -   displayName
    -   displayName
    -   Regola 3

6. EmployeeTipo

    -   EmployeeTipo

    -   Regola 4

7. givenName

    | givenName | Regola 5 | sn |
    |-|-|-|
    |lastName|Regola 6|Manager|
    |manager|Regola 7|objectSID|
    |ObjectSID|Regola 8|"Contoso"|
    |dominio|Per ogni riga di questa tabella, eseguire i passaggi seguenti:|Per aprire la finestra di dialogo Definizione di flusso, fare clic su **Nuovo flusso dell'attributo**.|
    |Nella scheda **Origine** selezionare l'attributo mostrato per la riga della tabella.|Nella scheda **Destinazione** selezionare l'attributo mostrato per la riga della tabella.|Per applicare la configurazione del flusso dell'attributo, fare clic su **OK**.|
    |Nella scheda **Riepilogo** fare clic su **Invia**.|Inizializzare l'ambiente di test|Prima di poter testare la configurazione MIM con i dati di AD, è necessario eseguire quattro passaggi:|
    |Abilitare il provisioning|Aprire Synchronization Service Manager.|Per aprire la finestra di dialogo Opzioni fare clic su **Opzioni** dal menu **Strumenti**|
    |Selezionare **Abilita provisioning regola di sincronizzazione**.|Per chiudere la finestra di dialogo Opzioni, fare clic su **OK**.|Inizializzare MIMMA|

    Eseguire un ciclo di sincronizzazione completa su questo connettore.

    - Il ciclo completo è costituito dalle esecuzioni dei profili di esecuzione seguenti:

    - Importazione completa

    - Sincronizzazione completa

    - Esportare

8. Importazione delta

## Seguire questa procedura per eseguire ognuno dei quattro profili di esecuzione.
Aprire Synchronization Service Manager e fare clic su **Agenti di gestione** dal menu **Strumenti**.

### Nell'elenco **Agenti di gestione** selezionare **MIMMA**.

1. Per aprire la finestra di dialogo Esegui agente di gestione, fare clic su **Esegui** dal menu **Azioni**.

2. Per ogni profilo di esecuzione indicato sopra, completare i passaggi seguenti:

3. Per aprire la finestra di dialogo Esegui agente di gestione, fare clic su **Esegui** dal menu **Azioni**.

4. Nell'elenco **Esegui profili** selezionare il profilo di esecuzione da configurare.

### Per avviare il profilo di esecuzione, fare clic su **OK**.

Configurare la precedenza del flusso dell'attributo Durante l'inizializzazione del connettore MIM, le regole di sincronizzazione configurate sono state introdotte nel metaverse.

- Regolare la precedenza del flusso dell’attributo per gli attributi forniti da questo connettore in modo da garantire che gli attributi già presenti in Active Directory possano essere trasmessi al metaverse e successivamente al database del servizio MIM.
- Inizializzare ADMA
- Per inizializzare il connettore di Active Directory, è necessario eseguire un'importazione completa e una sincronizzazione completa su connettore stesso.
- L'importazione completa sposta gli oggetti esistenti da AD nello spazio del connettore.

La sincronizzazione completa aggiorna le regole di sincronizzazione affinché corrispondano a quelle del connettore MIM.

1. Aprire Synchronization Service Manager e fare clic su **Agenti di gestione** dal menu **Strumenti**.

2. Nell'elenco **Agenti di gestione** selezionare **ADMA**.

3. Per aprire la finestra di dialogo Esegui agente di gestione, fare clic su **Esegui** dal menu **Azioni**.

4. Per ogni profilo di esecuzione indicato sopra, completare i passaggi seguenti:

    - Per aprire la finestra di dialogo Esegui agente di gestione, fare clic su **Esegui** dal menu **Azioni**.

    - Nell'elenco **Esegui profili** selezionare il profilo di esecuzione da configurare.

    - Per avviare il profilo di esecuzione, fare clic su **OK**.

#### Popolare il database del servizio MIM

Per popolare il database del servizio MIM con gli oggetti, è necessario eseguire un ciclo di sincronizzazione nel connettore MIMMA.

Il ciclo è costituito da:

### Export

Importazione completa Sincronizzazione completa Seguire questa procedura per eseguire ognuno dei tre profili di esecuzione.

1. Aprire Synchronization Service Manager e fare clic su **Management Agents** (Agenti di gestione) dal menu **Strumenti**.

2. Nell'elenco **Agenti di gestione** (Agenti di gestione) selezionare **MIMMA** (MIMMA).

3. Per aprire la finestra di dialogo Run Management Agent (Esegui agente di gestione), fare clic su **Esegui** dal menu **Azioni**.

4. Per ogni profilo di esecuzione indicato sopra, completare i passaggi seguenti:

    - Per aprire la finestra di dialogo Run Management Agent (Esegui agente di gestione), fare clic su **Esegui** dal menu **Azioni**.

    - Nell'elenco **Run profiles** (Esegui profili) selezionare il profilo di esecuzione da configurare.

    - Per avviare il profilo di esecuzione, fare clic su **OK**.

### [!div class="step-by-step"]

« Servizio e portale MIM The cycle consists of:

- Export
- Full Import
- Full Synchronization

Follow these steps to run each of the three run profiles.

1. Open the Synchronization Service Manager and click <bpt id="p1">**</bpt>Management Agents<ept id="p1">**</ept> on the <bpt id="p2">**</bpt>Tools<ept id="p2">**</ept> menu.

2. Select <bpt id="p1">**</bpt>MIMMA<ept id="p1">**</ept> in the <bpt id="p2">**</bpt>Management Agents<ept id="p2">**</ept> list.

3. Click <bpt id="p1">**</bpt>Run<ept id="p1">**</ept>  on the <bpt id="p2">**</bpt>Actions<ept id="p2">**</ept> menu to open the Run Management Agent dialog box.

4. For each run profile listed above, complete the following steps:

    - Click <bpt id="p1">**</bpt>Run<ept id="p1">**</ept> on the <bpt id="p2">**</bpt>Actions<ept id="p2">**</ept> menu to open the Run Management Agent dialog box.
    - Select the run profile you want to run from the <bpt id="p1">**</bpt>Run profiles<ept id="p1">**</ept> list.
    - Click <bpt id="p1">**</bpt>OK<ept id="p1">**</ept> to start the run profile.

>[!div class="step-by-step"] <bpt id="p1">[</bpt>« MIM Service and Portal<ept id="p1">](install-mim-service-portal.md)</ept>


<!--HONumber=Apr16_HO3-->


