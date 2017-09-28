---
title: Installazione dell'integrazione BHOLD per FIM/MIM | Microsoft Docs
description: Il modulo di integrazione BHOLD aggiunge la gestione dei ruoli self-service a FIM e MIM
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/12/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 
ms.openlocfilehash: ef68de19bd0eabd6d9203469ecc991d496f05846
ms.sourcegitcommit: ed8dd5563e77ef4a3345b2a52a1426859c95576a
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/15/2017
---
# <a name="bhold-fimmim-integration-installation"></a>Installazione dell'integrazione BHOLD per FIM/MIM

Il modulo BHOLD FIM Integration aggiunge la gestione dei ruoli self-service a Microsoft Identity Manager, consentendo agli utenti di richiedere ruoli aggiuntivi e stabilire a chi assegnarli. Il modulo BHOLD FIM Integration estende il portale FIM in modo da semplificare la gestione dei ruoli degli utenti come parte dell'amministrazione generale di FIM. Questo argomento illustra come deve essere configurata l'infrastruttura di rete per consentire l'installazione e l'uso del modulo BHOLD FIM Integration. Spiega inoltre come installare il modulo BHOLD FIM Integration e che tipo di configurazione è necessaria dopo l'installazione dell'integrazione BHOLD FIM.

## <a name="bhold-fim-integration-installation-requirements"></a>Requisiti di installazione dell'integrazione BHOLD FIM

Il modulo BHOLD FIM Integration estende il portale FIM e il servizio FIM in modo da consentire agli utenti di gestire i propri ruoli all'interno del portale FIM. Per questo motivo, è essenziale che il modulo BHOLD Core e le funzionalità necessarie di FIM siano installati e configurati prima di installare il modulo BHOLD FIM Integration.
Di seguito sono indicati i componenti software che devono essere presenti nel computer per poter installare il modulo BHOLD FIM Integration:

- Portale e servizio di Microsoft Identity Manager 2016
- Microsoft Silverlight 3 o versione successiva
- Internet Information Services e ASP.NET
- Microsoft Silverlight Tools

Inoltre, i moduli Core e Access Management Connector di BHOLD devono già essere distribuiti in un server dell'ambiente e FIM deve essere configurato con uno o più agenti di gestione BHOLD. Per informazioni sull'installazione e la configurazione del modulo BHOLD Core, vedere [BHOLD Core Installation](https://technet.microsoft.com/en-us/library/jj134095(v=ws.10).aspx) (Installazione di BHOLD Core). Per informazioni sull'installazione e l'uso del modulo Access Management Connector, vedere [Access Management Connector Installation](https://technet.microsoft.com/en-us/library/jj874042(v=ws.10).aspx) (Installazione di Access Management Connector) e [Test Lab Guide: BHOLD Access Management Connector](https://technet.microsoft.com/en-us/library/jj853085(v=ws.10).aspx) (Guida del lab di sviluppo/test: BHOLD Access Management Connector).

>[!IMPORTANT]
Il nome del database del servizio FIM deve essere FIMService. L'installazione di BHOLD FIM Integration avrà esito negativo se FIM non è stato installato con il nome di database predefinito del servizio FIM.

## <a name="before-you-begin"></a>Prima di iniziare

Prima di iniziare a installare il modulo BHOLD FIM Integration, è necessario creare una directory BHOLD nella directory radice dell'unità disco C: (C:\BHOLD).

Inoltre, è necessario essere pronti a inserire le informazioni richieste dall'installazione guidata di BHOLD FIM Integration per completare l'installazione. Il foglio di lavoro riportato di seguito consente di registrare le informazioni in modo che siano pronte quando è necessario inserirle.

### <a name="bholdfim-account-settings"></a>Impostazioni dell'account BHOLDFim

| **Elemento**                            | **Descrizione**                                                                                                                                                                                                               | **Valore**                                                                                                                                                                                                                                                                                                            |
|-------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Use Security Provider on Domain** (Usa provider di sicurezza per il dominio) | Se selezionata, questa opzione indica che la protezione di Active Directory Domain Services controllerà l'accesso a BHOLD Core.                                                                                                                    | Selezionare la casella di controllo. **Importante:** l'installazione avrà esito negativo se questa casella di controllo non è selezionata.                                                                                                                                                                                                                   |
| **Dominio**                          | Specifica il dominio che contiene l'**account del servizio** creato durante l'installazione di BHOLD Core. Per altre informazioni, vedere [BHOLD Core Installation](https://technet.microsoft.com/en-us/library/jj134095(v=ws.10).aspx) (Installazione di BHOLD Core). | Il nome di dominio viene inserito automaticamente dalla procedura guidata. Modificare il nome solo se non è corretto. **Importante:** specificare il nome di dominio usando il nome NetBIOS (breve), non il nome di dominio completo (FQDN). Se ad esempio il nome FQDN del dominio è fabrikam.com, specificare il nome di dominio come FABRIKAM. |
| **Nome utente**                        | Specifica il nome di accesso dell'account utente del servizio BHOLD Core.                                                                                                                                                              | Scrivere il nome dell'account utente qui:                                                                                                                                                                                                                                                                                    |
| **Password**                        | Specifica la password dell'account utente del servizio.                                                                                                                                                                           | Scrivere la password qui: **Importante:** assicurarsi di conservare questa password in una posizione protetta nascosta.                                                                                                                                                                                                                  |

### <a name="fim-service-settings"></a>Impostazioni del servizio FIM

| **Elemento**            | **Descrizione**                                                                                                                                                                                                                               | **Valore**                                                                                           |
|---------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|
| **Utente**            | Specifica il nome di accesso di un account con privilegi di amministratore per FIM. Microsoft consiglia vivamente di non usare l'account associato all'utente radice in BHOLD Core, che per impostazione predefinita è l'account usato per installare BHOLD Core. | Scrivere il nome dell'account utente qui:                                                                   |
| **Password**        | Specifica la password dell'account utente dell'amministratore di FIM.                                                                                                                                                                                 | Scrivere la password qui: **Importante:** assicurarsi di conservare questa password in una posizione protetta nascosta. |
| **FIM Database**    | Specifica il nome del database del servizio FIM.                                                                                                                                                                                               | FIMService                                                                                          |
| **Website IP/Port** (Indirizzo IP/porta sito Web) | Specifica il nome o l'indirizzo IP del server del portale di FIM e la porta del sito Web.                                                                                                                                                               | Scrivere il nome del server o l'indirizzo e la porta qui:                                                     |

### <a name="bhold-core-connection"></a>Connessione a BHOLD Core

| **Elemento**               | **Descrizione**                                                                                                                                                                                                                                                                                                                                                                               | **Valore**                                                                                           |
|------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|
| **Dominio**             | Specifica il nome del dominio dell'account indicato sotto come **utente**. Specificare il dominio in formato NetBIOS (breve).                                                                                                                                                                                                                                                                   | Scrivere il nome del dominio dell'account utente qui:                                                            |
| **Utente**               | Specifica il nome di accesso dell'account di un **utente BHOLD che è supervisore** di tutti gli utenti e i ruoli ed è autorizzato a collegare e scollegare i ruoli utente. Microsoft consiglia vivamente di non usare l'account associato all'utente radice in BHOLD Core, che per impostazione predefinita è l'account usato per installare BHOLD Core. Questo account può essere lo stesso account usato per connettersi a FIM | Scrivere il nome dell'account utente qui:                                                                   |
| **Password**           | Specifica la password dell'account utente specificato in **User** (Utente).                                                                                                                                                                                                                                                                                                                             | Scrivere la password qui: **Importante:** assicurarsi di conservare questa password in una posizione protetta nascosta. |
| **IP/Machine Address** (Indirizzo IP/computer) | Specifica l'indirizzo IP del server del sito Web di BHOLD Core. Non usare il nome del server.                                                                                                                                                                                                                                                                                                        | Scrivere l'indirizzo IP qui:                                                                          |
| **Numero porta**        | Specifica il numero di porta del sito Web di BHOLD Core.                                                                                                                                                                                                                                                                                                                                          | Scrivere il numero di porta qui:                                                                         |

## <a name="bhold-fim-integration-setup"></a>Impostazione di BHOLD FIM Integration

Per installare il modulo BHOLD FIM Integration, accedere come membro del gruppo Domain Admins, scaricare il file seguente ed eseguirlo come amministratore nel server in cui si intende installare il modulo BHOLD FIM Integration:

- BholdFIMIntegration*\<Versione\>*\_Release.msi

Sostituire *\<Versione\>* con il numero della versione di BHOLD FIM Integration che si sta installando.

Per eseguire il file di programma come amministratore, fare clic sul file con il pulsante destro del mouse e quindi fare clic su **Esegui come amministratore**.

![esecuzione msi](media/bhold-integration-installation/cmd.png)

## <a name="post-installation-tasks"></a>Attività post-installazione

Dopo avere installato BHOLD FIM Integration, è necessario configurare Microsoft SharePoint per concedere le autorizzazioni di proprietario del sito all'account del servizio BHOLD. Inoltre, se il portale di FIM è configurato per l'uso della protezione di Secure Sockets Layer (SSL), è necessario modificare i file che contengono riferimenti agli indirizzi delle pagine BHOLD aggiunte al portale di FIM.

### <a name="configuring-sharepoint"></a>Configurazione di SharePoint

Per funzionare correttamente, BHOLD FIM Integration richiede che l'account del servizio BHOLD abbia le autorizzazioni di membro del sito in Microsoft SharePoint.

### <a name="to-grant-site-member-permissions-to-the-bhold-service-account"></a>Per concedere le autorizzazioni di membro del sito all'account del servizio BHOLD

1.  Accedere al server che esegue BHOLD FIM Integration con privilegi di amministratore.

2.  Fare clic su **Start**, quindi su **Internet Explorer**.

3.  Nella barra degli indirizzi digitare <https://localhost> se SharePoint è configurato per l'uso della protezione SSL, in caso contrario digitare <http://localhost>.

4.  Sul lato sinistro della pagina **Sito del team** fare clic su **Utenti e gruppi**.

5.  In **Gruppi** fare clic su **Sito del team - Membri** e nella barra degli strumenti del riquadro centrale fare clic su **Nuovo**, quindi su **Aggiungi utenti**.

6.  Nella pagina **Aggiungi utenti: Sito del team**, in **Utenti/Gruppi**, digitare BHOLDApplicationGroup, quindi fare clic sul pulsante Controlla nomi sotto la casella **Utenti/Gruppi**. Il nome del gruppo deve essere risolto in modo da includere il nome del dominio.

7.  Fare clic su **Give users permissions directly** (Assegna autorizzazioni direttamente agli utenti), selezionare **Controllo completo – Dispone del controllo completo** e quindi scegliere **OK**.

8.  Verificare che BHOLDApplicationGroup sia indicato in **Autorizzazioni: Sito del team** e quindi chiudere Internet Explorer.

![esecuzione msi](media/bhold-integration-installation/sharepoint.png)

### <a name="configuring-bhold-to-support-ssl"></a>Configurazione di BHOLD per il supporto di SSL

Se il portale di FIM è configurato per l'uso della protezione SSL, è necessario modificare i file nel server FIM in modo che i collegamenti alle pagine BHOLD si aprano. I file sono contenuti nella cartella seguente: ```C:\Program Files\Common Files\Microsoft Shared\Web Server Extensions\12\TEMPLATE\LAYOUTS\BHOLD```.

Nella tabella che segue sono riportati i file e le versioni originali e modificate delle stringhe da usare.

| **File**                  | **Stringa originale**                                                                                                                   | **Stringa modificata**                                                                                                                                |
|---------------------------|---------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------|
| Analytics.aspx            |   http://<BHOLD_Server>/bhold/Analytics/index_fim.html | https://<BHOLD_Server_FQDN>/bhold/Analytics/index_fim.html       |
| AttestationCampaigns.aspx |    http://<BHOLD_Server>/bhold/Attestation/Campaigns.aspx?hideMenu=1 | https://<BHOLD_Server_FQDN>/bhold/Attestation/Campaigns.aspx?hideMenu=1 | 
| AttestationMain.aspx      |  http://<BHOLD_Server>/bhold/Attestation/Dashboard.aspx?hideMenu=1        | https://<BHOLD_Server_FQDN>/bhold/Attestation/Dashboard.aspx?hideMenu=1 |
| Reporting.aspx            | http://<BHOLD_Server>/bhold/Reporting/index_fim.html |  https://<BHOLD_Server_FQDN>/bhold/Reporting/index_fim.html |
| Selfservice.aspx          | RoleExchangePoint=http://\<*FIM_Server*\>: \<*FIM_Port*\>/BHOLD/RoleExchangePoint/ BHOLDRoleExchangePoint.svc,TransportMode=Transport | RoleExchangePoint=https://\<*FIM_Server_FQDN*\>: \<*FIM_SSL_Port\>*\>/BHOLD/RoleExchangePoint/ BHOLDRoleExchangePoint.svc,TransportMode=Transport |

Dove:

-   *\<BHOLD_Server\>* specifica il nome del server BHOLD come indicato nella versione originale del file

-   *\<MIM_Server\>* specifica il nome del server FIM come indicato nella versione originale del file

-   *\<BHOLD_Server_FQDN\>* specifica il nome di dominio completo (FQDN) del server BHOLD

-   *\<MIM_Port\>* specifica il numero di porta del server FIM come indicato nella versione originale del file

-   *\<MIM_Server_FQDN\>* specifica il nome FQDN del server FIM

-   *\<MIM_SSL_Port\>* specifica una porta diversa da usare con SSL nel server FIM

### <a name="enable-approval-workflows-in-bhold-core"></a>Abilitare flussi di lavoro di approvazione in BHOLD Core

Quando FIM e BHOLD sono integrati per la modalità self-service, i flussi di lavoro per le approvazioni vengono eseguiti nel servizio FIM. La situazione è simile a quella del modello di flusso di lavoro per le richieste create nel portale di FIM, ad esempio quando un utente invia una richiesta per essere aggiunto a una lista di distribuzione. Esistono tuttavia differenze fondamentali tra i flussi di lavoro del ruolo BHOLD e altri flussi di lavoro ospitati nel servizio FIM. Nel caso di BHOLD, i parametri del flusso di lavoro che specificano gli utenti che devono approvare una richiesta sono originati in BHOLD, anziché essere archiviati nelle definizioni di flusso di lavoro nel database del servizio FIM. Questi parametri vengono passati al servizio FIM da BHOLD quando viene fatta la prima richiesta e un flusso di lavoro di azione comunica i risultati a BHOLD Core.

BHOLD seleziona un responsabile approvazione per una richiesta self-service in uno dei tre modi seguenti:

-   **Responsabile linea come responsabile approvazione: selezione basata sui ruoli per un'unità organizzativa** Se un ruolo ha un attributo denominato roletype impostato su Approver (Responsabile approvazione) o Escalator (Responsabile riassegnazione) e se tale ruolo è collegato a uno o più utenti nel contesto di un'unità organizzativa, le richieste provenienti dagli utenti di tale unità organizzativa devono essere approvate da uno degli utenti collegati al ruolo con attributo roletype impostato su Approver o Escalator.

-   **Responsabile linea come responsabile approvazione: selezione basata sugli attributi per un'unità organizzativa** Ogni unità organizzativa può avere uno o più attributi che specificano gli alias degli utenti che possono approvare le assegnazioni dei ruoli per altri utenti dell'unità organizzativa. Questi attributi sono denominati approver1, approver2 e così via. Quando un utente dell'unità organizzativa richiede un'assegnazione di ruolo, BHOLD indirizza la richiesta, attraverso FIM, agli utenti specificati dagli attributi approver dell'unità organizzativa. Se per un'unità organizzativa non è impostato alcun attributo, BHOLD controlla le unità organizzative padre fino all'unità organizzativa radice.

-   **Gestione ruoli come responsabile approvazione: selezione basata sugli attributi per un ruolo** Un ruolo può avere uno o più attributi (denominati approver1 e così via) che specificano gli alias degli utenti che possono approvare l'assegnazione del ruolo. Quando un utente chiede che gli venga assegnato un ruolo per cui sono impostati gli attributi approver, BHOLD indirizza la richiesta agli utenti specificati dagli attributi.

Se per una richiesta di ruolo self-service non viene specificato un responsabile approvazione da uno di questi metodi, per impostazione predefinita BHOLD assegna automaticamente il ruolo senza richiedere l'approvazione. Per questo motivo, subito dopo l'installazione di BHOLD FIM Integration, è necessario configurare l'unità organizzativa radice con l'alias di un responsabile approvazione, ad esempio l'account radice. In questo modo si impedirà che a un utente venga involontariamente concesso un ruolo prima che possano essere implementati criteri di approvazione più completi.

#### <a name="to-configure-an-approver-for-the-root-orgunit"></a>Per configurare un responsabile approvazione per l'unità organizzativa radice

1.  Accedere al server di BHOLD Core come amministratore.

2.  Fare clic su **Start**, quindi su **Internet Explorer**.

3.  Nella barra degli indirizzi di Internet Explorer digitare <http://localhost:5151/bhold/core> e quindi premere INVIO.

4.  Nella home page di BHOLD Core, in **Attribute def** (Definizione attributi), fare clic su **Attribute types** (Tipi di attributo).

5.  Nella pagina **Attribute type** (Tipo di attributo) fare clic su **Add** (Aggiungi).

6.  Nella pagina **Add attribute type** (Aggiungi tipo di attributo), in **Identity** (Identità), digitare approver1, nell'elenco **Data type** (Tipo di dati) fare clic su **AlphaNumeric** (Alfanumerico), in **Maximum length** (Lunghezza massima) digitare 255, in **List of values** (Elenco di valori) fare clic su **No**, in **English** (Inglese) digitare Approver 1, fare clic su **OK** e quindi scegliere **Done** (Chiudi).

7.  Nel riquadro a sinistra, sotto **Attribute def** (Definizione attributo) fare clic su **Attribute type sets** (Set di tipi di attributo).

8.  Nella pagina **Attribute type sets** (Set di tipi di attributo) fare clic su **Add** (Aggiungi).

9.  Nella pagina **Add attribute type set** (Aggiungi set di tipi di attributo), in **Description** (Descrizione), digitare OrgUnit Attributes e quindi fare clic su **OK**.

10. Nella pagina **OrgUnit Attributes** (Attributi unità organizzativa) espandere **Attribute types** (Tipi di attributo) e quindi fare clic su **Modify** (Modifica).

11. Nell'elenco **Attribute type** (Tipo di attributo) fare clic su **approver1**, su **Add** (Aggiungi) e infine su **Done** (Chiudi).

12. Nel riquadro sinistro fare clic su **Object types** (Tipi di oggetto).

13. Nella pagina **Object types** (Tipi di oggetto) fare clic su **OrgUnit**.

14. Nella pagina **Object type/OrgUnit** (Tipo di oggetto/OrgUnit) espandere **Attribute type sets** (Set di tipi di attributo) e quindi fare clic su **Modify** (Modifica).

15. Nella pagina **Link attribute type set/OrgUnit** (Collega set di tipi di attributo) digitare 10 in **Order** (Ordine), nell'elenco **Attribute type set** (Set di tipi di attributo) fare clic su **OrgUnit Attributes** (Attributi unità organizzativa), fare clic su **Add** (Aggiungi) e quindi su **Done** (Chiudi).

16. Nel riquadro a sinistra, sotto **Model** (Modello), fare clic su **Organizational units** (Unità organizzative).

17. Nella pagina **Organizational units** (Unità organizzative) fare clic su **root** (radice).

18. Nella pagina **Organizational unit/root** (Unità organizzativa/radice) fare clic su **Modify** (Modifica).

19. Nella pagina **Modify organizational unit attributes/root** (Modifica attributi unità organizzativa/radice), in **Approver** (Responsabile approvazione), digitare il dominio e il nome utente dell'utente che approverà le richieste di assegnazione dei ruoli, nel formato *\<dominio\>*\\*\<utente\>*, dove *\<dominio\>* è il nome di dominio NetBIOS (breve) e *\<utente\>* è il nome di accesso dell'utente.
20. Fare clic su **OK**.

>[!IMPORTANT]
Il dominio e il nome utente devono corrispondere all'alias predefinito di un utente nel database di BHOLD Core.

In alternativa alla definizione di un responsabile approvazione per le unità organizzative, è possibile specificare un responsabile approvazione per i ruoli proposti nel database di BHOLD Core. A tale scopo, creare l'attributo approver1, aggiungerlo a un attributo typeset associato al tipo di oggetto Role (Ruolo) e quindi modificare ogni ruolo proposto per specificare il responsabile approvazione.

Per garantire una maggiore protezione del flusso di lavoro, oltre ai responsabili approvazione è necessario definire ulteriori modalità di approvazione e utenti creando e inserendo i seguenti attributi per le unità organizzative e i ruoli:

- escalator*\<n\>*

- owner*\<n\>*

- securityOfficer*\<n\>*

- notification*\<n\>*

dove *\<n\>* indica un suffisso numerico facoltativo per specificare più attributi dello stesso tipo.

### <a name="verify-approval-workflows-configured-in-the-fim-service"></a>Verificare i flussi di lavoro di approvazione configurati nel servizio FIM

L'installazione di BHOLD FIM Integration crea set, definizioni di flussi di lavoro e regole di criteri di gestione per il servizio FIM. Se la distribuzione di FIM è stata personalizzata in modo da modificare i set di amministratori o i set di utenti che possono eseguire richieste, è necessario verificare se le regole di criteri di gestione fanno riferimento ai set di utenti corretti.

>[!NOTE]
Prima che gli utenti del portale di FIM usino le funzionalità self-service offerte da BHOLD, è necessario sincronizzare gli account degli utenti nel database di BHOLD dal servizio di sincronizzazione di FIM. In particolare, deve esistere un record utente nel database di BHOLD Core e nel database del servizio FIM per ogni utente che può effettuare una richiesta self-service o che è specificato come responsabile approvazione o responsabile riassegnazione per le richieste self-service.

## <a name="next-steps"></a>Passaggi successivi

- Per informazioni sull'installazione del portale di FIM e su altre funzionalità di FIM, vedere [Planning and Architecture](https://technet.microsoft.com/library/ee808044.aspx) (Pianificazione e architettura) nella raccolta di documentazione tecnica di Microsoft Forefront.
- [Guida all'installazione di BHOLD](bhold-installation-guide.md)
- [Riferimento per gli sviluppatori BHOLD](../reference/mim2016-bhold-developer-reference.md)
- [Cronologia delle versioni BHOLD](../reference/version-bhold-history.md)
