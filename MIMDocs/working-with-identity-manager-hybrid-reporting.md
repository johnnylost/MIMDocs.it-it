---
title: Utilizzare il servizio per la creazione report ibridi in Azure tramite Identity Manager 2016 | Microsoft Docs
description: Informazioni su come combinare i dati cloud locali in report ibridi in Azure e come gestire e visualizzare questi report.
keywords: 
author: fimguy
ms.author: barclayn
manager: mbaldwin
ms.date: 10/12/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 68df2817-2040-407d-b6d2-f46b9a9a3dbb
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: a96d79d6773a72c813d0cd76de26ea40d28769e1
ms.sourcegitcommit: 3d8a2493eae1218bfdb75a399ffa4adc8c2a8fdf
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/20/2018
---
# <a name="work-with-hybrid-reporting-in-identity-manager-public-preview-refresh"></a>Utilizzare il servizio per la creazione report ibridi in Identity Manager - Aggiornamento dell'anteprima pubblica

Questo articolo illustra come combinare i dati del cloud e locali in report ibridi in Azure e come gestire e visualizzare questi report.

## <a name="available-hybrid-reports"></a>Report ibridi disponibili
I primi tre report di Microsoft Identity Manager disponibili in Azure Active Directory (Azure AD) sono i seguenti:

- **Attività di reimpostazione password**: visualizza ogni istanza in cui un utente ha eseguito una reimpostazione della password tramite la reimpostazione della password self-service e fornisce i controlli o i metodi usati per l'autenticazione.

- **Registrazione reimpostazione password**: visualizza ogni registrazione dell'utente per la reimpostazione della password self-service e i metodi usati per l'autenticazione. Esempi di metodi possono essere un numero di telefono cellulare o domande e risposte.
   > [!NOTE]
   > Per i report *Registrazione reimpostazione password* non viene fatta alcuna distinzione tra controllo SMS e MFA. Entrambi sono considerati metodi con telefono cellulare.

- **Attività dei gruppi self-service**: visualizza ogni tentativo eseguito da un utente per aggiungersi o eliminarsi da un gruppo e la relativa creazione del gruppo.

    ![Servizio di creazione di report ibridi di Azure: immagine dell'attività di reimpostazione della password](media/MIM-Hybrid-passwordreset2.jpg)

> [!NOTE]
> * I report attualmente presentano i dati per al massimo un mese di attività.
> * È necessario disinstallare l'agente per la creazione di report ibridi precedente.
> * Per disinstallare i report ibridi, disinstallare l'agente MIMreportingAgent.msi.

## <a name="prerequisites"></a>Prerequisiti

* Il servizio Identity Manager 2016 RTM o Identity Manager SP1.

* Un tenant di Azure AD Premium con un amministratore dotato di licenza nella directory.

* Connettività Internet in uscita dal server Identity Manager in Azure.

## <a name="requirements"></a>Requisiti
I requisiti per l'uso del servizio per la creazione di report ibridi di Identity Manager sono elencati nella tabella seguente:

| Requisito | Descrizione |
| --- | --- |
| Azure AD Premium | Il servizio per la creazione report ibridi è una funzionalità di Azure AD Premium e richiede Azure AD Premium. </br>Per altre informazioni, vedere [Introduzione ad Azure Active Directory Premium](https://docs.microsoft.com/azure/active-directory/active-directory-get-started-premium). </br>Ottenere una [versione di valutazione gratuita per 30 giorni di Azure AD Premium](https://azure.microsoft.com/trial/get-started-active-directory/). |
| È necessario essere un amministratore globale di Azure AD |Per impostazione predefinita, solo gli amministratori globali possono installare e configurare gli agenti in grado di avviare il servizio, accedere al portale ed eseguire qualsiasi operazione all'interno di Azure. </br>**Importante:** l'account usato per installare gli agenti deve essere un account aziendale o dell'istituto di istruzione. Non è possibile usare un account Microsoft. Per altre informazioni, vedere [Iscrizione ad Azure come organizzazione](https://docs.microsoft.com/azure/active-directory/sign-up-organization). |
| L'agente ibrido di Identity Manager è installato in ogni server del servizio Identity Manager di destinazione | Per ricevere i dati e offrire le funzionalità di monitoraggio e analisi, il servizio per la creazione di report ibridi richiede che gli agenti siano installati e configurati nei server di destinazione.  </br>|
| Connettività in uscita agli endpoint del servizio di Azure | Durante l'installazione e il runtime l'agente richiede la connettività agli endpoint del servizio Azure. Se la connettività in uscita è bloccata da firewall, assicurarsi che gli endpoint seguenti vengano aggiunti all'elenco degli elementi consentiti:<ul><li>\*.blob.core.windows.net </li><li>\*.servicebus.windows.net - Port: 5671 </li><li>\*.adhybridhealth.azure.com/</li><li>https://management.azure.com </li><li>https://policykeyservice.dc.ad.msft.net/</li><li>https://login.windows.net</li><li>https://login.microsoftonline.com</li><li>https://secure.aadcdn.microsoftonline-p.com</li></ul> |
|Connettività in uscita in base agli indirizzi IP | Per l'applicazione di filtri in base agli indirizzi IP nei firewall, vedere [Azure IP Ranges](https://www.microsoft.com/download/details.aspx?id=41653) (Intervalli di indirizzi IP Azure).|
| L'ispezione SSL per il traffico in uscita è filtrata o disabilitata | La procedura di registrazione dell'agente o le operazioni di caricamento dei dati potrebbero non riuscire se è presente l'ispezione SSL o la chiusura per il traffico in uscita a livello di rete. |
| Porte del firewall nel server che esegue l'agente | Per comunicare con gli endpoint di servizio di Azure, l'agente richiede che le porte del firewall seguenti siano aperte:<ul><li>Porta TCP 443</li><li>Porta TCP 5671</li></ul> |
| Consentire determinati siti Web se la protezione avanzata di Internet Explorer è abilitata |Se la protezione avanzata di Internet Explorer è abilitata, i siti Web seguenti devono essere consentiti nel server in cui è installato l'agente:<ul><li>https://login.microsoftonline.com</li><li>https://secure.aadcdn.microsoftonline-p.com</li><li>https://login.windows.net</li><li>Server federativo per l'organizzazione ritenuto attendibile da Azure Active Directory (ad esempio, https://sts.contoso.com).</li></ul> |
</BR>

## <a name="install-identity-manager-reporting-agent-in-azure-ad"></a>Installare l'agente per la creazione di report di Identity Manager
Dopo aver installato l'agente per la creazione di report, i dati relativi all'attività di Identity Manager vengono esportati da Identity Manager nel registro eventi di Windows. L'agente per la creazione di report di Identity Manager elabora gli eventi e quindi li carica in Azure. In Azure, gli eventi vengono analizzati, decrittografati e filtrati per i report necessari.

1.  Installare Identity Manager 2016.

2.  Scaricare l'agente per la creazione di report di Identity Manager e quindi eseguire le operazioni seguenti:

    a. Accedere al portale di gestione di Azure AD e quindi selezionare **Active Directory**.

    b. Fare doppio clic sulla directory per cui si è un amministratore globale e si dispone di una sottoscrizione di Azure AD Premium.

    c. Selezionare **Configurazione** e quindi scaricare l'agente per la creazione di report.

3.  Installare l'agente per la creazione di report nel modo seguente:

    a.  Scaricare il [file MIMHReportingAgentSetup.exe](http://download.microsoft.com/download/7/3/1/731D81E1-8C1D-4382-B8EB-E7E7367C0BF2/MIMHReportingAgentSetup.exe) per il server del servizio Identity Manager.

    b.  Eseguire `MIMHReportingAgentSetup.exe`. 

    c.  Eseguire il programma di installazione dell'agente.

    d.  Assicurarsi che il servizio dell'agente per la creazione report di Identity Manager sia in esecuzione.

    e.  Riavviare il servizio Identity Manager.

4.  Verificare che l'agente per la creazione di report di Identity Manager sia funzionante in Azure.

    È possibile creare i dati del report tramite il portale di reimpostazione della password self-service di Identity Manager per reimpostare la password di un utente. Assicurarsi che la reimpostazione della password sia stata completata correttamente e quindi verificare che i dati siano visualizzati nel portale di gestione di Azure AD.

## <a name="view-hybrid-reports-in-the-azure-portal"></a>Visualizzare i report ibridi nel portale di Azure

1.  Accedere al [portale di Azure](https://portal.azure.com/) con l'account di amministratore globale per il tenant.

2.  Selezionare **Azure Active Directory**.

3.  Selezionare la directory tenant nell'elenco delle directory disponibili per la sottoscrizione.

4.  Selezionare **Log di controllo**.

5.  Nell'elenco a discesa **Categoria** assicurarsi che sia selezionata l'opzione **Servizio MIM**.

> [!IMPORTANT]
> È possibile che sia necessario attendere prima che i dati di controllo di Identity Manager vengano visualizzati nel portale di Azure.

## <a name="stop-creating-hybrid-reports"></a>Interrompere la creazione di report ibridi
Se si vuole interrompere il caricamento dei dati di controllo dei report da Identity Manager in Azure AD, disinstallare l'agente per la creazione di report ibridi. Usare lo strumento Installazione applicazioni di Windows per disinstallare il servizio per la creazione report ibridi di Identity Manager.

## <a name="windows-events-used-for-hybrid-reporting"></a>Eventi di Windows usati per la creazione di report ibridi
Gli eventi generati da Identity Manager vengono archiviati nel registro eventi di Windows. È possibile visualizzare gli eventi nel **Visualizzatore eventi** selezionando **Registri applicazioni e servizi** > **Identity Manager Request Log** (Log richieste Identity Manager). Ogni richiesta di Identity Manager viene esportata come evento nel registro eventi di Windows nella struttura JSON. È possibile esportare il risultato nel sistema di informazioni di sicurezza e gestione degli eventi in uso.

|Tipo evento|ID|Dettagli evento|
|--------------|------|-----------------|
|Informazioni|4121|Dati dell'evento di Identity Manager che include tutti i dati della richiesta.|
|Informazioni|4137|Estensione dell'evento 4121 di Identity Manager, nel caso vi siano troppi dati per un singolo evento. L'intestazione in questo evento viene visualizzata nel formato seguente: `"Request: <GUID> , message <xxx> out of <xxx>`.|
