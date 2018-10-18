---
title: Microsoft Identity Manager 2016 | Documentazione Microsoft
description: Viene illustrato il processo di creazione degli utenti in Servizi di dominio Active Directory tramite Microsoft Identity Manager 2016
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 08/18/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 88473df88271937b07450df409353c0b3ca08684
ms.sourcegitcommit: 7de35aaca3a21192e4696fdfd57d4dac2a7b9f90
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/16/2018
ms.locfileid: "49358789"
---
# <a name="how-do-i-provision-users-to-ad-ds"></a>Come si esegue il provisioning di utenti in Servizi di dominio Active Directory

Si applica a: Microsoft Identity Manager 2016 SP1 (MIM)

Uno dei requisiti di base di un sistema di gestione delle identità è la capacità di eseguire il provisioning delle risorse in un sistema esterno.

In questa guida vengono illustrati i blocchi predefiniti principali che sono coinvolti nel processo di provisioning degli utenti da Microsoft® Identity Manager (MIM) 2016 in Servizi di dominio Active Directory® (AD DS), viene spiegato come verificare se lo scenario funziona come previsto, vengono offerti suggerimenti per la gestione di utenti di Active Directory mediante MIM 2016 e vengono elencate altre fonti di informazione.

## <a name="before-you-begin"></a>Operazioni preliminari


Questa sezione contiene informazioni sull'ambito del documento. Questa sezione è destinata a lettori che hanno già un'esperienza di base nella sincronizzazione di oggetti con MIM, come illustrato nelle [guide introduttive](http://go.microsoft.com/FWLink/p/?LinkId=190486) relative.

### <a name="audience"></a>Destinatari


Questa guida è destinata a professionisti dell'IT che hanno già una conoscenza di base del funzionamento del processo di sincronizzazione MIM e che sono interessati a fare un'esperienza pratica e a ottenere maggiori informazioni su scenari specifici.

### <a name="prerequisite-knowledge"></a>Competenze necessarie


Questo documento presuppone l'accesso a un'istanza in esecuzione di MIM e che l'utente abbia esperienza nella configurazione di scenari di sincronizzazione semplici, come descritto nei documenti seguenti:

-   [Introduzione alla sincronizzazione in uscita](http://go.microsoft.com/FWLink/p/?LinkId=189652)

-   [Introduction to Outbound Synchronization](http://go.microsoft.com/FWLink/p/?LinkId=189653) (Introduzione alla sincronizzazione in uscita)

Il contenuto di questo documento è da intendersi come estensione dei documenti introduttivi.

### <a name="scope"></a>Ambito


Lo scenario descritto in questo documento è stato semplificato per soddisfare i requisiti di un ambiente lab di base. Lo scopo del documento è offrire una panoramica dei concetti e delle tecnologie illustrate.

Questo documento consente di sviluppare una soluzione che prevede la gestione dei gruppi in Servizi di dominio Active Directory tramite MIM.

### <a name="time-requirements"></a>Requisiti di tempo


Le procedure descritte in questo documento richiedono da 90 a 120 minuti per essere completate.

Queste stime orarie presuppongono che l'ambiente di testing sia già configurato e non includono il tempo necessario per configurare l'ambiente di test.

### <a name="getting-support"></a>Richiesta di assistenza


In caso di domande riguardanti il contenuto di questo documento o se l'utente ha commenti e suggerimenti da sottoporre, è possibile inviare un messaggio sul [forum di Forefront Identity Manager 2010](http://go.microsoft.com/FWLink/p/?LinkId=189654).

## <a name="scenario-description"></a>Descrizione dello scenario


Fabrikam, una società fittizia, prevede di usare MIM per gestire gli account utente in Servizi di dominio Active Directory aziendale. Come parte di questo processo, Fabrikam deve eseguire il provisioning degli utenti in Servizi di dominio Active Directory. Per avviare il test iniziale, Fabrikam ha installato un ambiente lab di base che è costituito da MIM e da Servizi di dominio Active Directory.
In questo ambiente lab Fabrikam sta testando uno scenario costituito da un utente che è stato creato manualmente nel portale MIM. L'obiettivo di questo scenario è eseguire il provisioning dell'utente come un utente abilitato con una password predefinita per Servizi di dominio Active Directory.

## <a name="scenario-design"></a>Progettazione dello scenario


Per usare questa guida, sono necessari tre componenti dell'architettura:

-   Controller di dominio Active Directory

-   Computer che esegue il servizio di sincronizzazione FIM
-   Computer che esegue il portale FIM

Nella figura seguente viene descritto l'ambiente obbligatorio.

![Ambiente obbligatorio](media/how-provision-users-adds/image001.png)


È possibile eseguire tutti i componenti in un solo computer.

> [!NOTE]
> Per altre informazioni sulla configurazione di MIM, vedere [Installation Guide](http://go.microsoft.com/FWLink/p/?LinkId=165845) (Guida all'installazione).

## <a name="scenario-components-list"></a>Elenco dei componenti dello scenario


Nella tabella seguente sono elencati i componenti che fanno parte dello scenario della guida.

| ![Unità organizzativa](media/how-provision-users-adds/image005.jpg)   | Unità organizzativa                | Oggetti MIM: unità organizzativa (OU) che viene usata come destinazione per gli utenti con provisioning.                                                       |
|----------------------------------------|------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------|
| ![Account utente](media/how-provision-users-adds/image006.jpg)   | Account utente                      | &#183; **ADMA**: account utente di Active Directory con diritti sufficienti per connettersi a Servizi di dominio Active Directory.<br/> &#183; **FIMMA**: account utente di Active Directory con diritti sufficienti per connettersi a MIM.
                                                                 |
| ![Agenti di gestione e profili di esecuzione](media/how-provision-users-adds/image007.jpg)  | Agenti di gestione e profili di esecuzione | &#183; **Fabrikam ADMA**: agente di gestione che consente di scambiare dati con Servizi di dominio Active Directory. <br/> &#183; Fabrikam FIMMA: agente di gestione che consente di scambiare dati con MIM.                                                                                 |
| ![Regole di sincronizzazione](media/how-provision-users-adds/image008.jpg)  | Regole di sincronizzazione              | Regola di sincronizzazione in uscita gruppo Fabrikam: regola di sincronizzazione in uscita che esegue il provisioning degli utenti in Servizi di dominio Active Directory.                                     |
| ![Operazioni set](media/how-provision-users-adds/image009.jpg)   | Operazioni set                               | Tutti i terzisti: set con appartenenza dinamica per tutti gli oggetti con un valore dell'attributo EmployeeType impostato su Terzista.                                |
| ![Flussi di lavoro](media/how-provision-users-adds/image010.jpg)  | Flussi di lavoro                          | Flusso di lavoro del provisioning di Active Directory: flusso di lavoro per portare l'utente MIM nell'ambito della regola di sincronizzazione in uscita di Active Directory.                                |
| ![Regole dei criteri di gestione](media/how-provision-users-adds/image011.jpg)   | Regole dei criteri di gestione            | Regola dei criteri di gestione del provisioning di AD: regola dei criteri di gestione che viene attivata quando una risorsa diventa membro del set Tutti i terzisti. |
| ![Utenti MIM](media/how-provision-users-adds/image012.jpg) | Utenti MIM                          | Britta Simon: utente MIM di cui è stato eseguito il provisioning in Servizi di dominio Active Directory.                                                                                             |



## <a name="scenario-steps"></a>Passaggi dello scenario


Lo scenario descritto in questa guida include i blocchi predefiniti illustrati nella figura seguente.

![Passaggi dello scenario](media/how-provision-users-adds/image013.png)


## <a name="configuring-the-external-systems"></a>Configurazione dei sistemi esterni


In questa sezione vengono messe a disposizione le istruzioni per le risorse che è necessario creare e che sono fuori dall'ambiente MIM.

### <a name="step-1-create-the-ou"></a>Passaggio 1: Creare la OU


L'unità organizzativa funge da contenitore per l'utente di esempio con provisioning. Per altre informazioni sulla creazione delle OU, vedere [Creare una nuova unità organizzativa](http://go.microsoft.com/FWLink/p/?LinkId=189655).

Creare una OU denominata MIMObjects in Servizi di dominio Active Directory.

### <a name="step-2-create-the-active-directory-user-accounts"></a>Passaggio 2: Creare gli account utente di Active Directory

Per lo scenario della guida, sono necessari due account utente di Active Directory:

- **ADMA**: usato dall'agente di gestione di Active Directory.

- **FIMMA**: usato dall'agente di gestione del servizio FIM.

In entrambi i casi basta creare gli account utente normali. Più avanti nel documento vengono illustrati i requisiti specifici di entrambi gli account. Per altre informazioni sulla creazione di utenti, vedere [Creare un nuovo account utente](http://go.microsoft.com/FWLink/p/?LinkId=189656).


## <a name="configuring-the-fim-synchronization-service"></a>Configurazione del servizio di sincronizzazione FIM


Per i passaggi di configurazione in questa sezione, è necessario avviare FIM Synchronization Service Manager.

### <a name="creating-the-management-agents"></a>Creazione di agenti di gestione

Per lo scenario in questa guida, è necessario creare due agenti di gestione:

-   **Fabrikam ADMA**: agente di gestione per Servizi di dominio Active Directory.

-   **Fabrikam FIMMA**: agente di gestione per l'agente di gestione del servizio FIM.

### <a name="step-3-create-the-fabrikam-adma-management-agent"></a>Passaggio 3: Creare l'agente di gestione Fabrikam ADMA

Quando si configura un agente di gestione per Servizi di dominio Active Directory, è necessario specificare un account che viene usato dall'agente di gestione nello scambio di dati con Servizi di dominio Active Directory. È consigliabile usare un account utente normale. Tuttavia, per importare dati da Servizi di dominio Active Directory, l'account deve avere i diritti per eseguire il polling delle modifiche dal controllo DirSync. Se si vuole che l'agente di gestione esporti dati in Servizi di dominio Active Directory, è necessario concedere all'account i diritti sufficienti per le OU di destinazione. Per altre informazioni su questo argomento, vedere [Configuring the ADMA Account](http://go.microsoft.com/FWLink/p/?LinkId=189657) (Configurazione dell'account ADMA).

Per creare un utente in Servizi di dominio Active Directory, è necessario trasferire il DN dell'oggetto. Oltre a questo, è buona norma trasferire il nome, il cognome e il nome visualizzato in modo da garantire che gli oggetti siano individuabili.

In Servizi di dominio Active Directory molto spesso gli utenti usano ancora l'attributo sAMAccountName per accedere al servizio directory. Se non si specifica un valore per questo attributo, il servizio directory genera un valore casuale. Tuttavia, questi valori casuali non sono semplici da usare, motivo per cui solitamente l'esportazione in Servizi di dominio Active Directory include una versione dell'attributo più semplice da usare. Per consentire agli utenti di accedere a Servizi di dominio Active Directory, è necessario anche includere una password creata mediante l'attributo unicodePwd nella logica di esportazione.

> [!Note]
> Verificare che il valore specificato come unicodePwd sia conforme ai criteri password dell'account di Servizi di dominio Active Directory di destinazione.

Quando si imposta una password per gli account di Servizi di dominio Active Directory, è necessario anche creare un account come account abilitato. A tale scopo, impostare l'attributo userAccountControl. Per altre informazioni sull'attributo userAccountControl, vedere [Using FIM to Enable or Disable Accounts in Active Directory](http://go.microsoft.com/FWLink/p/?LinkId=189658) (Uso di FIM per abilitare o disabilitare gli account in Active Directory).

Nella tabella seguente sono elencate le impostazioni specifiche per lo scenario più importanti che devono essere configurate.

| Pagina di progettazione dell'agente di gestione                          | Configurazione                                                  |
|---------------------------------------------------------|----------------------------------------------------------------|
| Creazione dell'agente di gestione                                 | 1. **Agente di gestione per:** Servizi di dominio Active Directory  <br/> 2.  **Nome:** Fabrikam ADMA |
| Connessione a una foresta di Active Directory                      | 1. **Selezionare le partizioni di directory:** "DC=Fabrikam,DC=com"   <br/>   2. Fare clic su **Containers**  (Contenitori) per aprire la finestra di dialogo **Select Containers** (Selezione contenitori) e assicurarsi che **MIMObjects** sia l'unica OU selezionata.        |
| Selezione tipi di oggetti                                     | Oltre ai tipi di oggetto già selezionati, selezionare **utente.** |
| Selezione degli attributi                                       | 1. Fare clic su **Mostra tutto** <br/>   2. Selezionare gli attributi seguenti: <br/> &nbsp;&nbsp;&nbsp;&#176; **displayName** <br/> &nbsp;&nbsp;&nbsp;&#176; **givenName** <br/> &nbsp;&nbsp;&nbsp;&#176;  **sn** <br/> &nbsp;&nbsp;&nbsp;&#176;  **SamAccountName** <br/> &nbsp;&nbsp;&nbsp;&#176;  **unicodePwd** <br/> &nbsp;&nbsp;&nbsp;&#176;  **userAccountControl**     

Per altre informazioni, vedere gli argomenti seguenti nella Guida:
- Creare un agente di gestione
- Connessione a una foresta di Active Directory
- Uso dell'agente di gestione per Active Directory
- Configurazione delle partizioni di directory

> [!Note]
> Assicurarsi di avere una regola del flusso di attributi di importazione configurata per l'attributo ExpectedRulesList.

### <a name="step-4-create-the-fabrikam-fimma-management-agent"></a>Passaggio 4: Creare l'agente di gestione Fabrikam FIMMA

Quando si configura un agente di gestione per il servizio FIM, è necessario specificare un account che viene usato dall'agente di gestione nello scambio di dati con il servizio FIM.

È consigliabile usare un account utente normale. L'account deve essere lo stesso account specificato durante l'installazione di MIM. Per creare uno script da usare per determinare il nome dell'account FIMMA specificato durante la configurazione e per verificare se questo account è ancora valido, vedere [FIM MA Account Configuration Quick Test](http://go.microsoft.com/FWLink/p/?LinkId=189659) (Test rapido di configurazione account FIMMA).

Nella tabella seguente sono elencate le impostazioni specifiche per lo scenario più importanti che devono essere configurate. Creare l'agente di gestione in base alle informazioni specificate nella tabella seguente.  

| Pagina di progettazione dell'agente di gestione | Configurazione |
|------------|------------------------------------|
| Creazione dell'agente di gestione | 1. **Agente di gestione per:** agente di gestione del servizio FIM <br/> 2. **Nome:** Fabrikam FIMMA |
| Connessione al database     | Usare le seguenti impostazioni: <br/> &#183; **Server:** localhost <br/> &#183; **Database:** FIMService <br/> &#183; **Indirizzo di base del servizio FIM:**  http://localhost:5725 <br/> <br/> Specificare le informazioni relative all'account creato per l'agente di gestione |
| Selezione tipi di oggetti                                     | Oltre ai tipi di oggetto già selezionati, selezionare **Persona**.   |
| Configurare i mapping dei tipi di oggetto                          | Oltre ai mapping dei tipi di oggetto già esistenti, aggiungere un mapping per il **Tipo di oggetto origine dati** Persona alla persona con tipo oggetto **Metaverse**. |
| Configurare il flusso di attributi                                | Oltre ai mapping del flusso di attributi già esistenti, aggiungere i mapping del flusso di attributi seguenti: <br/><br/> ![Flusso di attributi](media/how-provision-users-adds/image018.jpg) |




Per altre informazioni, vedere gli argomenti seguenti nella Guida:
-   Creare un agente di gestione

-   Connessione a un database di Active Directory

-   Uso dell'agente di gestione per Active Directory

-   Configurazione delle partizioni di directory

> [!NOTE]
>  Assicurarsi di avere una regola del flusso di attributi di importazione configurata per l'attributo ExpectedRulesList.

### <a name="step-5-create-the-run-profiles"></a>Passaggio 5: Creare profili di esecuzione

Nella tabella seguente vengono elencati i profili di esecuzione che è necessario creare per lo scenario della guida.

| Agente di gestione  | Profilo di esecuzione     |
|-------------------|--------------------------------------|
| Fabrikam ADMA     | 1. Importazione completa  <br/> 2. Sincronizzazione completa <br/> 3. Importazione delta <br/> 4. Sincronizzazione delta <br/> 5. Export                                                                    |
| Fabrikam FIMMA   | 1. Importazione completa <br> 2. Sincronizzazione completa <br/> 3. Importazione delta <br/> 4. Sincronizzazione delta <br/> 5. Export|                                                                                                                                                                                   

Creare profili di esecuzione per ogni agente di gestione in base alla tabella precedente.


> [!Note]
> Per altre informazioni, vedere la creazione del profilo di esecuzione dell'agente di gestione nella Guida di MIM.                                                                                                                  
> 
> 
> [!Important]
>  Verificare che il provisioning sia abilitato nell'ambiente in uso. È possibile fare ciò eseguendo lo script contenuto in Using Windows PowerShell to Enable Provisioning (Uso di Windows PowerShell per abilitare il provisioning) all'indirizzo http://go.microsoft.com/FWLink/p/?LinkId=189660).


## <a name="configuring-the-fim-service"></a>Configurazione del servizio FIM


Per lo scenario in questa guida, è necessario configurare un criterio di provisioning, come illustrato nella figura seguente.

![Criteri di provisioning](media/how-provision-users-adds/image019.png)

L'obiettivo di questo criterio di provisioning consiste nel portare i gruppi nell'ambito della regola di sincronizzazione in uscita di Active Directory. Portando la risorsa nell'ambito della regola di sincronizzazione, si consente al motore di sincronizzazione di eseguire il provisioning della risorsa in Servizi di dominio Active Directory in base alla configurazione.

Per configurare il servizio FIM, passare a http://localhost/identitymanagement in Windows Internet Explorer®. Nella pagina del portale MIM per creare i criteri di provisioning, accedere alle pagine relative nella sezione dell'amministrazione. Per verificare la configurazione, è necessario eseguire lo script contenuto in [Using Windows PowerShell to document your provisioning policy configuration](http://go.microsoft.com/FWLink/p/?LinkId=189661) (Uso di Windows PowerShell per documentare la configurazione dei criteri di provisioning).

### <a name="step-6-create-the-synchronization-rule"></a>Passaggio 6: Creare la regola di sincronizzazione

Le tabelle seguenti illustrano la configurazione della regola di sincronizzazione del provisioning Fabrikam obbligatoria. Creare la regola di sincronizzazione in base ai dati nelle tabelle seguenti.

| Configurazione della regola di sincronizzazione                                                                         |                                                                             |                                                           
|------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------|-----------------------------------------------------------|
| Name                                                                                                       | Regola di sincronizzazione in uscita utente di Active Directory                         |                                                          
| Descrizione                                                                                               |                                                                             |                                                           
| Precedenza                                                                                                | 2                                                                           |                                                           
| Direzione del flusso di dati   | In uscita             |       
| Dipendenza       |         |                                         


| Ambito |                                                                             |                                                           
|--------|-------|
| Tipo di risorsa metaverse | persona |                                                         
| Sistema esterno                   |Fabrikam ADMA                                                               |                                                       
| Tipo di risorsa sistema esterno                                                                              | utente      



| Relationship ||
|------------|---------|
| Crea risorsa in sistema esterno                                                                         | True                                                                        |                                                           
| Abilita deprovisioning                                                                                      | False                                                                       |                                                           

| Criteri di relazione                                                                                      | |
|------------|----------|
| Attributo ILM     | Attributo di origine dati                                                       |
| Attributo di origine dati         | sAMAccountName    |

| Flussi di attributi in uscita iniziali        | |                                                             |
|-------------------|---------------------- |---------------|
| Consenti valori Null                 | Destination                                                                 | Origine                                                    |
| false                       | dn                                                                          | \+("CN=",displayName,",OU=MIMObjects,DC=fabrikam,DC=com") |
| false                       | userAccountControl                                                          | **Costante:** 512                                         |
| false                                                                     | unicodePwd                    | Costante: P\@\$\$W0rd                                    |

| Flussi di attributi in uscita persistenti  |                                                                     |                                                           |
|--------------------------------------|---------------------------------------------------------------------|-----------------------------------------------------------|
| Consenti valori Null                                                                                                | Destination                                                                 | Origine                                                    |
| false                                                                                                      | sAMAccountName                                                              | accountName                                               |
| false                                                                                                      | displayName                                                                 | displayName                                               |
| false                                                                                                      | givenName                                                                   | firstName                                                 |
| false                                                                                                      | sn                                                                          | lastName                                                  |



> [!NOTE]
>  Importante. Verificare di aver selezionato il flusso iniziale solo per il flusso di attributi che ha DN come destinazione.                                                                          

### <a name="step-7-create-the-workflow"></a>Passaggio 7: Creare il flusso di lavoro

L'obiettivo del flusso di lavoro del provisioning di Active Directory consiste nell'aggiungere la regola di sincronizzazione del provisioning Fabrikam a una risorsa. Le tabelle seguenti illustrano la configurazione.  Creare un flusso di lavoro in base ai dati nelle tabelle seguenti.

| Configurazioni del flusso di lavoro               |                                                                 |
|--------------------------------------|-----------------------------------------------------------------|
| Name                                 | Flusso di lavoro del provisioning utente di Active Directory                     |
| Descrizione                          |                                                                 |
| Tipo flusso di lavoro                        | Azione                                                          |
| Esecuzione in aggiornamento criteri                 | False                                                           |

| Regola di sincronizzazione                 |                                                                 |
|--------------------------------------|-----------------------------------------------------------------|
| Name                                 | Regola di sincronizzazione in uscita utente di Active Directory             |
| Azione                               | Aggiunta                                                             |




### <a name="step-8-create-the-mpr"></a>Passaggio 8: Creare la Regola di criteri di gestione

La Regola di criteri di gestione richiesta è di tipo Transizione del set e si attiva quando una risorsa diventa un membro del set Tutti i terzisti. Le tabelle seguenti illustrano la configurazione.  Creare una Regola di criteri di gestione richiesta in base ai dati nelle tabelle seguenti.

| Configurazione Regola di criteri di gestione richiesta                    |                                                             |
|--------------------------------------|-------------------------------------------------------------|
| Name                                 | Regola dei criteri di gestione del provisioning utente di Active Directory                 |
| Descrizione                          |                                                             |
| Tipo                                 | Transizione del set                                              |
| Concede autorizzazioni                   | False                                                       |
| Disabilitato                             | False                                                       |

| Definizione della transizione                |                                                             |
|--------------------------------------|-------------------------------------------------------------|
| Tipo di transizione                      | Inizio transizione                                               |
| Set transizione                       | Tutti i terzisti                                             |

| Flussi di lavoro criteri                     |                                                             |
|--------------------------------------|-------------------------------------------------------------|
| Tipo                                 | Azione                                                      |
| Nome visualizzato                         | Flusso di lavoro del provisioning utente di Active Directory                 |




## <a name="initializing-your-environment"></a>Inizializzazione dell'ambiente


Gli obiettivi della fase di inizializzazione sono i seguenti:

-   Portare la regola di sincronizzazione nel metaverse.

-   Portare la struttura di Active Directory nello spazio connettore di Active Directory.

### <a name="step-9-run-the-run-profiles"></a>Passaggio 9: Eseguire i profili di esecuzione

Nella tabella seguente vengono elencati i profili di esecuzione che fanno parte della fase di inizializzazione.  Eseguire i profili di esecuzione in base alla tabella riportata di seguito.

| Esegui                                                                                                           | Agente di gestione                                      | Profilo di esecuzione          |
|---------------------------------------------------------------------------------------------------------------|-------------------------------------------------------|----------------------|
| 1                                                                                                             | Fabrikam FIMMA                                        | Importazione completa          |
| 2                                                                                                             |                                                       | Sincronizzazione completa |
| 3                                                                                                             |                                                       | Export               |
| 4                                                                                                             |                                                       | Importazione delta         |
|                                                                                                               |                                                       |                      |
| 5                                                                                                             | Fabrikam ADMA                                         | Importazione completa          |
| 6                                                                                                             |                                                       | Sincronizzazione completa |



> [!NOTE]
> È necessario verificare che la regola di sincronizzazione in uscita sia stata proiettata nel metaverse.

## <a name="testing-the-configuration"></a>Test della configurazione


L'obiettivo di questa sezione consiste nel testare la configurazione effettiva. Per testare la configurazione:

1.  Creare un utente di esempio nel portale di FIM.

2.  Verificare i requisiti di provisioning dell'utente di esempio.

3.  Eseguire il provisioning dell'utente di esempio in Servizi di dominio Active Directory.

4.  Verificare che l'utente esista in Servizi di dominio Active Directory.

### <a name="step-10-create-a-sample-user-in-mim"></a>Passaggio 10: Creare un utente di esempio in MIM


La tabella seguente elenca le proprietà dell'utente di esempio. Creare un utente di esempio in base ai dati nella tabella seguente.

| Attributo                              | Valore                                                          |
|----------------------------------------|----------------------------------------------------------------|
| Nome proprio                             | Britta                                                         |
| Cognome                              | Giuseppe                                                          |
| Nome visualizzato                           | Laura Giussani                                                   |
| Nome account                           | BSimon                                                         |
| Dominio                                 | Fabrikam                                                       |
| Tipo di dipendente                          | Terzista                                                     |



### <a name="verify-the-provisioning-requisites-of-the-sample-user"></a>Verificare i requisiti di provisioning dell'utente di esempio


Per eseguire il provisioning dell'utente di esempio in Servizi di dominio Active Directory, devono essere soddisfatti due prerequisiti:

1.  L'utente deve essere un membro del set Tutti i terzisti.

2.  L'utente del set deve essere nell'ambito della regola di sincronizzazione in uscita.

### <a name="step-11-verify-the-user-is-a-member-of-all-contractors"></a>Passaggio 11: Verificare che l'utente sia un membro di Tutti i terzisti

Per verificare se l'utente è un membro del set Tutti i terzisti, aprire il set e fare clic su Visualizza membri.

![Verificare che l'utente sia membro di Tutti i terzisti](media/how-provision-users-adds/image022.jpg)


### <a name="step-12-verify-the-user-is-in-the-scope-of-the-outbound-synchronization-rule"></a>Passaggio 12: Verificare che l'utente faccia parte dell'ambito della regola di sincronizzazione in uscita

Per verificare se l'utente è nell'ambito della regola di sincronizzazione, aprire la pagina delle proprietà dell'utente e verificare l'attributo Elenco di regole previste nella scheda Provisioning. L'attributo Elenco di regole previste dovrebbe elencare l'utente di Active Directory

Regola di sincronizzazione in uscita. Lo screenshot seguente illustra un esempio dell'attributo Elenco di regole previste.

![Stato della regola di sincronizzazione](media/how-provision-users-adds/image023.jpg)

A questo punto del processo, lo stato della regola di sincronizzazione è In sospeso. Ciò significa che la regola di sincronizzazione non è ancora stata applicata all'utente.



### <a name="step-13-synchronize-the-sample-group"></a>Passaggio 13: Sincronizzare il gruppo di esempio


Prima di iniziare il primo ciclo di sincronizzazione per un oggetto di test, è necessario rilevare lo stato previsto dell'oggetto dopo ogni profilo di esecuzione che viene eseguito in un piano di test. Il piano di test deve includere accanto allo stato generale dell'oggetto (Creato, Aggiornato o Eliminato) anche i valori dell'attributo previsti.
Usare il piano di test per verificare le aspettative del piano di test. Se un passaggio non restituisce i risultati previsti, non procedere al passaggio successivo fino a quando non è stata risolta la discrepanza tra il risultato previsto e il risultato effettivo.

Per verificare le aspettative, è possibile usare le statistiche di sincronizzazione come primo indicatore. Ad esempio, se si prevede che i nuovi oggetti vengano aggiunti temporaneamente in uno spazio connettore, ma le statistiche di importazione non restituiscono alcun valore "Adds", c'è qualcosa nell'ambiente che non funziona come previsto.

![Statistiche di sincronizzazione](media/how-provision-users-adds/image024.jpg)

Mentre le statistiche di sincronizzazione possono dare una prima indicazione sul funzionamento dello scenario, usare la funzionalità di ricerca dello spazio connettore e del metaverse di Synchronization Service Manager per verificare i valori degli attributi previsti.

Per sincronizzare l'utente a Servizi di dominio Active Directory:

1.  Importare l'utente nello spazio connettore FIMMA.

2.  Proiettare l'utente nel metaverse.

3.  Eseguire il provisioning dell'utente nello spazio connettore di Active Directory.

4.  Esportare le informazioni sullo stato in FIM.

5.  Esportare l'utente in Servizi di dominio Active Directory.

6.  Confermare la creazione dell'utente.

Per eseguire queste operazioni, eseguire i profili di esecuzione seguenti.

| Agente di gestione | Profilo di esecuzione  |
|------------------|--------------|
| Fabrikam FIMMA   | 1. Importazione delta <br/> 2. Sincronizzazione differenziale <br/> 3. Export <br/> 4. Importazione delta |
| Fabrikam FIMMA   | 1. Export <br/> 2. Importazione delta       |


Dopo l'importazione dal database del servizio FIM, Britta Simon e l'oggetto ExpectedRuleEntry che collega Britta alla regola di sincronizzazione in uscita di Active Directory vengono gestiti nello spazio connettore Fabrikam FIMMA. Quando si visualizzano le proprietà di Britta nello spazio connettore, accanto ai valori di attributo che sono stati configurati nel portale di FIM, è presente anche un riferimento valido all'oggetto Voce della regola prevista. Lo screenshot seguente illustra un esempio relativo a questo.

![Proprietà oggetto spazio connettore](media/how-provision-users-adds/image025.jpg)

L'obiettivo della sincronizzazione delta eseguita in Fabrikam FIMMA è quello di eseguire diverse operazioni:

-   Proiezione: il nuovo oggetto utente e l'oggetto correlato Voce della regola prevista vengono proiettati nel metaverse.

-   Provisioning: viene eseguito il provisioning dell'oggetto Britta Simon appena proiettato nello spazio del connettore di Fabrikam ADMA.

-   Esportazione dei flussi di attributi: l'esportazione dei flussi di attributi avviene in entrambi gli agenti di gestione. In Fabrikam ADMA l'oggetto Britta Simon appena sottoposto a provisioning viene popolato con i nuovi valori di attributo. In Fabrikam FIMMA l'oggetto Britta Simon esistente e l'oggetto ExpectedRuleEntry correlato vengono aggiornati con valori di attributo che sono il risultato della proiezione.

![Statistiche di sincronizzazione](media/how-provision-users-adds/image026.jpg)

Come già indicato dalle statistiche di sincronizzazione, è stata eseguita un'attività di provisioning nello spazio connettore di Fabrikam ADMA. Quando si esaminano le proprietà dell'oggetto metaverse di Britta Simon, si noterà che questa attività è il risultato dell'attributo ExpectedRulesList che è stato popolato con un riferimento valido.

![Proprietà oggetto metaverse](media/how-provision-users-adds/image027.jpg)

Durante l'esportazione seguente in Fabrikam FIMMA, lo stato della regola di sincronizzazione di Britta Simon viene aggiornato da In sospeso ad Applicato. Ciò indica che la regola di sincronizzazione in uscita è attiva per l'oggetto nel metaverse.

![Regola di sincronizzazione applicata](media/how-provision-users-adds/image028.jpg)

Poiché è stato eseguito il provisioning di un nuovo oggetto nello spazio connettore ADMA, è necessario avere un'aggiunta esportazione in sospeso per l'agente di gestione. Usando uno script apposito, è possibile verificare l'aggiunta esportazione in sospeso per Fabrikam ADMA. Per usare lo script, vedere [Using Windows PowerShell to Display the Export State of a Management Agent](http://go.microsoft.com/FWLink/p/?LinkId=189664) (Uso di Windows PowerShell per visualizzare lo stato dell'esportazione di un agente di gestione).

![Esportazioni in sospeso per agente di gestione](media/how-provision-users-adds/image029.jpg)

In FIM tutte le esecuzioni di esportazione richiedono un'importazione delta successiva per completare l'operazione di esportazione. L'importazione delta che viene eseguita dopo l'esecuzione di un'esportazione precedente è detta importazione di conferma. Le importazioni di conferma sono necessarie per consentire al servizio di sincronizzazione FIM di aggiornare i requisiti appropriati durante le esecuzioni della sincronizzazione successive.


Eseguire i profili di esecuzione seguendo le istruzioni contenute in questa sezione.

> [!IMPORTANT]
> Ogni esecuzione del profilo di esecuzione deve essere completata senza errori.

### <a name="step-14-verify-the-provisioned-user-in-ad-ds"></a>Passaggio 14: Verificare l'utente con provisioning in Servizi di dominio Active Directory

Per verificare che sia stato eseguito il provisioning dell'utente di esempio in Servizi di dominio Active Directory, aprire l'unità organizzativa FIMObjects. Britta Simon dovrebbe trovarsi nella OU FIMObjects.

![verifica della presenza dell'utente nella OU FIMObjects](media/how-provision-users-adds/image033.jpg)

<a name="summary"></a>Riepilogo
=======

L'obiettivo di questo documento è presentare i blocchi predefiniti principali per la sincronizzazione di un utente in MIM con Servizi di dominio Active Directory. Nel testing iniziale è consigliabile iniziare con il valore minimo di attributi necessari per completare un'attività e aggiungere altri attributi allo scenario quando i passaggi generali funzionano come previsto. Minimizzare la complessità semplifica il processo di risoluzione dei problemi.

Quando si esegue il test della configurazione, è molto probabile che si eliminino e ricreino nuovi oggetti di test. Per gli oggetti con un

attributo ExpectedRulesList popolato, ciò può comportare la generazione di oggetti ERE orfani.
Per una descrizione di come rimuovere tali oggetti dall'ambiente di test, vedere [A Method to Remove Orphaned ExpectedRuleEntry Objects from Your Environment](http://go.microsoft.com/FWLink/p/?LinkId=189667) (Un metodo per rimuovere gli oggetti ExpectedRuleEntry orfani dall'ambiente).

In uno scenario di sincronizzazione tipico che include Servizi di dominio Active Directory come destinazione della sincronizzazione, MIM non è autorevole per tutti gli attributi di un oggetto. Ad esempio, quando si gestiscono oggetti utente in Servizi di dominio Active Directory mediante FIM, il dominio e gli attributi objectSID dovranno essere specificati dall'agente di gestione di Servizi di dominio Active Directory.
Il nome dell'account, il dominio e gli attributi objectSID sono obbligatori se si vuole abilitare l'accesso utente al portale di FIM. Per popolare questi attributi da Servizi di dominio Active Directory, è necessaria una regola di sincronizzazione in entrata aggiuntiva per lo spazio connettore di Servizi di dominio Active Directory. Quando si gestiscono oggetti con valori di attributo con più origini, è necessario assicurarsi di configurare correttamente la precedenza del flusso di attributi. Se la precedenza del flusso di attributi non è configurata correttamente, il motore di sincronizzazione blocca il popolamento dei valori di attributo. È possibile trovare altre informazioni sulla precedenza del flusso di attributi nell'articolo [About Attribute Flow Precedence](http://go.microsoft.com/FWLink/p/?LinkId=189675) (Informazioni sulla precedenza del flusso di attributi).

<a name="see-also"></a>Vedere anche
=========

<a name="other-resources"></a>Risorse aggiuntive
---------------

[Using FIM to Enable or Disable Accounts in Active Directory](http://go.microsoft.com/FWLink/p/?LinkId=189670) (Uso di FIM per abilitare o disabilitare account in Active Directory)

[About Reference Attributes](http://go.microsoft.com/FWLink/p/?LinkId=189671) (Informazioni sugli attributi di riferimento)

[How Can I Manage My FIM MA Account](http://go.microsoft.com/FWLink/p/?LinkId=189672) (Come gestire l'account FIMMA)

[Detecting Nonauthoritative Accounts – Part 1: Envisioning](http://go.microsoft.com/FWLink/p/?LinkId=189673) (Rilevamento account non autorevoli - Parte 1: Definizione degli obiettivi)

[How to Detect Connectors in FIM](http://go.microsoft.com/FWLink/p/?LinkId=189674) (Rilevamento dei connettori in FIM)

[Configuring the ADMA Account](http://go.microsoft.com/FWLink/p/?LinkId=189657) (Configurazione account ADMA)

[A Method to Remove Orphaned ExpectedRuleEntry Objects from Your Environment](http://go.microsoft.com/FWLink/p/?LinkId=189667) (Un metodo per rimuovere gli oggetti ExpectedRuleEntry orfani dall'ambiente)

[About Attribute Flow Precedence](http://go.microsoft.com/FWLink/p/?LinkId=189675) (Informazioni sulla precedenza del flusso di attributi)

[About Exports](http://go.microsoft.com/FWLink/p/?LinkId=189676) (Informazioni sulle esportazioni)
