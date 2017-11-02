---
title: Utilizzo del servizio di creazione di report ibridi in Azure con MIM 2016 | Documentazione Microsoft
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
ms.openlocfilehash: cf8395583dcfcc2a84237bad80b6a4ca40ce166c
ms.sourcegitcommit: f077508b5569e2a96084267879c5b6551e1e0905
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/12/2017
---
# <a name="working-with-identity-manager-hybrid-reporting---public-preview-refresh"></a>Utilizzo del servizio di creazione report ibridi di Identity Manager - Anteprima pubblica (Aggiornamento)

## <a name="available-hybrid-reports"></a>Report ibridi
I primi tre report di Microsoft Identity Manager (MIM) disponibili in Azure AD sono **Attività di reimpostazione password**, **Registrazione di reimpostazione della password** e **Attività dei gruppi self-service**.

-   Con l'attività di reimpostazione della password viene visualizzata ogni istanza in cui un utente ha eseguito una reimpostazione della password tramite l'SSPR di reimpostazione della password e fornisce i gate o **Metodi** utilizzati per l'autenticazione.

-   Consente di visualizzare la registrazione ogniqualvolta un utente esegue la registrazione per l'SSPR di reimpostazione della password e il **Metodi** utilizzati per autenticare, ad esempio un numero di telefono cellulare o domande e risposte.
    Si noti che per la registrazione di reimpostazione della password, non viene fatta alcuna distinzione tra Gate SMS e l'autenticazione a più fattori sono entrambi considerati attività associate a un **telefono cellulare**.

-   Attività di gruppi self-service consente di visualizzare ogni tentativo eseguito da un utente per aggiungersi o eliminarsi da un gruppo e la relativa creazione del gruppo.

    ![Servizio di creazione di report ibridi di Azure: immagine dell'attività di reimpostazione della password](media/MIM-Hybrid-passwordreset2.jpg)

> [!NOTE]
> I report attualmente presentano i dati fino a un mese prima.</br>
> È necessario disinstallare l'agente ibrido precedente</br>
> Se si desidera disinstallare i report ibridi, disinstallare l'agente MIMreportingAgent.msi.

## <a name="prerequisites"></a>Prerequisiti

1.  Installare Microsoft Identity Manager 2016 RTM o SP1, incluso il servizio MIM.

2.  Assicurarsi di disporre di un tenant di Azure AD Premium con un amministratore dotato di licenza nella directory.

3.  Assicurarsi di disporre della connettività Internet in uscita da Microsoft Identity Manager in Azure.

## <a name="requirements"></a>Requisiti
Nella tabella seguente è riportato un elenco di requisiti per l'uso del servizio di creazione report ibridi di Identity Manager.

| Requisito | Descrizione |
| --- | --- |
| Azure AD Premium | Il servizio di creazione report ibridi è una funzionalità di Azure AD Premium e richiede Azure AD Premium. </br></br>Per altre informazioni, vedere [Introduzione ad Azure Active Directory Premium](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-get-started-premium) </br>Per avviare una versione di valutazione gratuita di 30 giorni, vedere [Attiva la versione di valutazione](https://azure.microsoft.com/trial/get-started-active-directory/). |
| Per iniziare è necessario essere un amministratore globale di Azure AD |Per impostazione predefinita, solo gli amministratori globali possono installare e configurare gli agenti in grado di avviare il servizio, accedere al portale ed eseguire qualsiasi operazione all'interno di Azure. </br></br>**Importante:** l'account usato per installare gli agenti deve essere un account aziendale o dell'istituto di istruzione. Non è possibile usare un account Microsoft. Per altre informazioni, vedere [Iscrizione ad Azure come organizzazione](https://docs.microsoft.com/en-us/azure/active-directory/sign-up-organization) |
| L'agente ibrido di Microsoft Identity Manager è installato in ogni server del servizio MIM di destinazione | Per la creazione di report ibridi è necessario che gli agenti siano installati e configurati nei server di destinazione per ricevere i dati e offrire le funzionalità di monitoraggio e analisi </br>|
| Connettività in uscita agli endpoint del servizio di Azure | Durante l'installazione e il runtime l'agente richiede la connettività agli endpoint del servizio Azure. Se la connettività in uscita è bloccata tramite firewall, assicurarsi che gli endpoint seguenti vengano aggiunti all'elenco degli elementi consentiti: </br></br><li>&#42;.blob.core.windows.net </li><li>&#42;.servicebus.windows.net - Porta: 5671 </li><li>&#42;.adhybridhealth.azure.com/</li><li>https://management.azure.com </li><li>https://policykeyservice.dc.ad.msft.net/</li><li>https://login.windows.net</li><li>https://login.microsoftonline.com</li><li>https://secure.aadcdn.microsoftonline-p.com</li> |
|Connettività in uscita in base agli indirizzi IP | Per l'applicazione di filtri in base agli indirizzi IP nei firewall, vedere [Azure IP Ranges](https://www.microsoft.com/en-us/download/details.aspx?id=41653) (Intervalli di indirizzi IP Azure).|
| L'ispezione SSL per il traffico in uscita è filtrata o disabilitata | La procedura di registrazione dell'agente o le operazioni di caricamento dei dati potrebbero non riuscire se è presente l'ispezione SSL o la chiusura per il traffico in uscita a livello di rete. |
| Porte del firewall nel server che esegue l'agente. |L'agente richiede che le porte del firewall seguenti siano aperte affinché l'agente comunichi con gli endpoint di servizio di Azure.</br></br><li>Porta TCP 443</li><li>Porta TCP 5671</li> |
| Consentire i siti Web seguenti se è abilitata la funzionalità Protezione avanzata di Internet Explorer |Se la funzionalità Protezione avanzata di Internet Explorer è abilitata, i siti Web seguenti devono essere consentiti nel server in cui verrà installato l'agente.</br></br><li>https://login.microsoftonline.com</li><li>https://secure.aadcdn.microsoftonline-p.com</li><li>https://login.windows.net</li><li>Server federativo per l'organizzazione ritenuto attendibile da Azure Active Directory. Ad esempio: https://sts.contoso.com</li> |
</BR>

## <a name="install-microsoft-identity-manager-reporting-agent-in-azure-ad"></a>Installare Microsoft Identity Manager Reporting Agent in Azure AD
Dopo aver installato l'agente di creazione report, i dati relativi all'attività di Microsoft Identity Manager vengono esportati da MIM nel Registro eventi di Windows. L'agente di creazione report di MIM elabora gli eventi e li carica in Azure. In Azure, gli eventi vengono analizzati, decrittografati e filtrati per i report necessari.

1.  Installare Microsoft Identity Manager 2016.

2.  Scaricare gli agent di creazione report di Microsoft Identity Manager in Azure AD

    1.  Accedere al portale di gestione di Azure AD e fare clic sull'icona di Active Directory.

    2.  Fare doppio clic sulla directory per cui si è un amministratore globale e si dispone di una sottoscrizione di Azure AD Premium.

    3.  Fare clic su **Configurazione** e scaricare l'agente di creazione report.

3.  Installare l'agente di creazione report di Microsoft Identity Manager:

    1.  Scaricare [MIMHReportingAgentSetup.exe](http://download.microsoft.com/download/7/3/1/731D81E1-8C1D-4382-B8EB-E7E7367C0BF2/MIMHReportingAgentSetup.exe) nel server del servizio Microsoft Identity Manager.
    2.  Eseguire `MIMHReportingAgentSetup.exe` 
    3.  Eseguire il programma di installazione dell'agente.

    4.  Assicurarsi che sia in esecuzione il servizio agente di creazione report di MIM

    5.  Riavviare il servizio MIM.

4.  Verificare che la funzionalità di creazione report in Microsoft Identity Manager sia attiva e funzionante in Azure.

    È possibile creare i dati del report tramite il portale self-service di reimpostazione password di Microsoft Identity Manager per reimpostare la password dell'utente. Assicurarsi che la reimpostazione della password sia stata completata correttamente e quindi verificare che i dati siano visualizzati nel portale di gestione di Azure AD.

## <a name="view-hybrid-reports-in-the-azure-portal"></a>Visualizzare i report ibridi nel portale di Azure

1.  Accedere al [portale di Azure](https://portal.azure.com/) con l'account di amministratore globale per il tenant.

2.  Fare clic sull'icona **Azure Active Directory** .

3.  Selezionare la directory tenant dall'elenco delle directory disponibili per la sottoscrizione.

4.  Fare clic su **Registri di controllo**.

5.  Assicurarsi di selezionare **Servizio MIM** nel menu a discesa Categoria.

> [!WARNING]
> È possibile sia necessario attendere prima che i dati di controllo di Microsoft Identity Manager vengano visualizzati nel portale di Azure.

## <a name="stop-creating-hybrid-reports"></a>Interrompere la creazione di report ibridi
Se si vuole interrompere il caricamento dei dati di controllo di creazione di report da Microsoft Identity Manager in Azure Active Directory, disinstallare l'agente per la creazione di report ibridi. Usare lo strumento **Installazione applicazioni** di Windows per disinstallare il servizio di creazione report ibridi di Microsoft Identity Manager.

## <a name="windows-events-used-for-hybrid-reporting"></a>Eventi di Windows usati per la creazione di report ibridi
Gli eventi generati da Microsoft Identity Manager vengono registrati nel Registro eventi di Windows e sono visibili nel Visualizzatore eventi in: Registri applicazioni e servizi-&gt; **Registro richieste di Identity Manager**. Ogni richiesta di MIM viene esportata come un evento nel Registro eventi di Windows nella struttura JSON. Può inoltre essere esportato nella soluzione SIEM in uso.

|Tipo evento|ID|Dettagli evento|
|--------------|------|-----------------|
|Informazioni|4121|Dati dell'evento MIM che include tutti i dati della richiesta.|
|Informazioni|4137|Estensione dell'evento 4121 MIM, nel caso vi siano troppi dati per un singolo evento. In questo caso l'intestazione è nel formato seguente: `"Request: <GUID> , message <xxx> out of <xxx>`|
