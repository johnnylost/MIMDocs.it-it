---
title: Utilizzo del servizio di creazione di report ibridi in Azure con MIM 2016 | Documentazione Microsoft
description: Informazioni su come combinare i dati cloud locali in report ibridi in Azure e come gestire e visualizzare questi report.
keywords: 
author: kgremban
ms.author: kgremban
manager: femila
ms.date: 01/27/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 68df2817-2040-407d-b6d2-f46b9a9a3dbb
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 3623bffb099a83d0eba47ba25e9777c3d590e529
ms.openlocfilehash: 9e64f930a8fe8422c7f6c8d98e558961ae8b88f2


---

# <a name="working-with-identity-manager-hybrid-reporting"></a>Utilizzo del servizio di creazione report ibridi di Identity Manager

## <a name="available-hybrid-reports"></a>Report ibridi
I primi tre report di Microsoft Identity Manager (MIM) disponibili in Azure AD sono **Attività di reimpostazione password**, **Registrazione di reimpostazione della password** e **Attività dei gruppi self-service**.

-   Con l'attività di reimpostazione della password viene visualizzata ogni istanza in cui un utente ha eseguito una reimpostazione della password tramite l'SSPR di reimpostazione della password e fornisce i gate o **Metodi** utilizzati per l'autenticazione.

    ![Servizio di creazione di report ibridi di Azure: immagine dell'attività di reimpostazione della password](media/MIM-Hybrid-passwordreset.jpg)

-   Consente di visualizzare la registrazione ogniqualvolta un utente esegue la registrazione per l'SSPR di reimpostazione della password e il **Metodi** utilizzati per autenticare, ad esempio un numero di telefono cellulare o domande e risposte.
    Si noti che per la registrazione di reimpostazione della password, non viene fatta alcuna distinzione tra Gate SMS e l'autenticazione a più fattori sono entrambi considerati attività associate a un **telefono cellulare**.

-   Attività di gruppi self-service consente di visualizzare ogni tentativo eseguito da un utente per aggiungersi o eliminarsi da un gruppo e la relativa creazione del gruppo.

> [!NOTE]
> I report attualmente presentano i dati fino a un mese prima.
>
> Se si desidera disinstallare i report ibridi, disinstallare l'agente MIMreportingAgent.msi.

## <a name="prerequisites"></a>Prerequisiti

1.  Installare Microsoft Identity Manager 2016 incluso il servizio MIM.

2.  Assicurarsi di disporre di un tenant di Azure AD Premium con un amministratore dotato di licenza nella directory.

3.  Assicurarsi di disporre della connettività Internet in uscita da Microsoft Identity Manager in Azure.

## <a name="install-microsoft-identity-manager-reporting-in-azure-ad"></a>Installare Microsoft Identity Manager Reporting in Azure AD
Dopo aver installato l'agente di creazione report, i dati relativi all'attività di Microsoft Identity Manager vengono esportati da MIM nel Registro eventi di Windows. L'agente di creazione report di MIM elabora gli eventi e li carica in Azure. In Azure, gli eventi vengono analizzati, decrittografati e filtrati per i report necessari.

1.  Installare Microsoft Identity Manager 2016.

2.  Scaricare gli agent di creazione report di Microsoft Identity Manager in Azure AD

    1.  Accedere al portale di gestione di Azure AD e fare clic sull'icona di Active Directory.

    2.  Fare doppio clic sulla directory per cui si è un amministratore globale e si dispone di una sottoscrizione di Azure AD Premium.

    3.  Fare clic su **Configurazione** e scaricare l'agente di creazione report.

3.  Installare l'agente di creazione report di Microsoft Identity Manager:

    1.  Creare una directory nel computer.

    2.  Estrarre i file `MIMHybridReportingAgent.msi` e `tenant.cert` nella directory.

    3.  Eseguire il programma di installazione dell'agente.

    4.  Assicurarsi che sia in esecuzione il servizio agente di creazione report di MIM

    5.  Riavviare il servizio MIM.

4.  Verificare che la funzionalità di creazione report in Microsoft Identity Manager sia attiva e funzionante in Azure.

    È possibile creare i dati del report tramite il portale self-service di reimpostazione password di Microsoft Identity Manager per reimpostare la password dell'utente. Assicurarsi che la reimpostazione della password sia stata completata correttamente e quindi verificare che i dati siano visualizzati nel portale di gestione di Azure AD.

## <a name="view-hybrid-reports-in-the-azure-classic-portal"></a>Visualizzare i report ibridi nel portale di Azure classico

1.  Accedere al [portale di Azure classico](https://manage.windowsazure.com/) con l'account di amministratore globale per il tenant.

2.  Fare clic sull'icona **Active Directory** .

3.  Selezionare la directory tenant dall'elenco delle directory disponibili per la sottoscrizione.

4.  Fare clic su **Report** quindi **Attività di reimpostazione della password**.

5.  Assicurarsi di selezionare **Identity Manager** nel menu di origine.

> [!WARNING]
> Può richiedere del tempo prima che i dati di Microsoft Identity Manager vengano visualizzati in Azure AD.

## <a name="stop-creating-hybrid-reports"></a>Interrompere la creazione di report ibridi
Se si vuole interrompere il caricamento dei dati di creazione di report da Microsoft Identity Manager in Azure Active Directory, disinstallare l'agente per la creazione di report ibridi. Usare lo strumento **Installazione applicazioni** di Windows per disinstallare il servizio di creazione report ibridi di Microsoft Identity Manager.

## <a name="windows-events-used-for-hybrid-reporting"></a>Eventi di Windows usati per la creazione di report ibridi
Gli eventi generati da Microsoft Identity Manager vengono registrati nel Registro eventi di Windows e sono visibili nel Visualizzatore eventi in: Registri applicazioni e servizi-&gt; **Registro richieste di Identity Manager**. Ogni richiesta di MIM viene esportata come un evento nel Registro eventi di Windows nella struttura JSON. Può inoltre essere esportato nella soluzione SIEM in uso.

|Tipo evento|ID|Dettagli evento|
|--------------|------|-----------------|
|Informazioni|4121|Dati dell'evento MIM che include tutti i dati della richiesta.|
|Informazioni|4137|Estensione dell'evento 4121 MIM, nel caso vi siano troppi dati per un singolo evento. In questo caso l'intestazione è nel formato seguente: `"Request: <GUID> , message <xxx> out of <xxx>`|



<!--HONumber=Jan17_HO4-->


