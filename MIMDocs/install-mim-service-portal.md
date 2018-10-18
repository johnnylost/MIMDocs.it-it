---
title: Installare il servizio e il portale di Microsoft Identity Manager | Documentazione Microsoft
description: Procedura di configurazione e installazione del servizio e del portale MIM per Microsoft Identity Manager 2016
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 04/30/2018
ms.topic: get-started-article
ms.prod: microsoft-identity-manager
ms.assetid: b0b39631-66df-4c5f-80c9-a1774346f816
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 535c80fa2ff1b6250ae9a3f340cb514e58f390a9
ms.sourcegitcommit: 7de35aaca3a21192e4696fdfd57d4dac2a7b9f90
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/16/2018
ms.locfileid: "49358619"
---
# <a name="install-mim-2016-mim-service-and-portal"></a>Installare MIM 2016: servizio e portale MIM

> [!div class="step-by-step"]
> [« Servizio di sincronizzazione MIM](install-mim-sync.md)
> [Sincronizzare i database »](install-mim-sync-ad-service.md)
> 
> [!NOTE]
> Questa procedura dettagliata usa nomi e valori di esempio della società Contoso. Sostituirli con i propri nomi e valori. Ad esempio:
> - Nome del controller di dominio: **mimservername**
> - Nome del dominio: **contoso**
> - Password: <strong>Pass@word1</strong>
> - Nome account del servizio: **MIMService**

Se non è stato configurato il pacchetto di installazione MIM nell'ultimo passaggio, tornare indietro e installare i componenti di Microsoft Identity Manager 2016 prima di procedere.


## <a name="configure-mim-service-and-portal-for-installation"></a>Configurare il servizio e il portale MIM per l'installazione

1. Eseguire il **programma di installazione del servizio e del portale MIM** dalla sottocartella **Service and Portal** decompressa.

2. Nella schermata di benvenuto, fare clic su **Avanti**.

3. Leggere il contratto di licenza con l'utente finale e, se si accettano le condizioni, fare clic su **Avanti**.

4. Nella schermata **Analisi utilizzo software MIM**, fare clic su **Avanti**.

5. Quando si selezionano le funzionalità dei componenti per questa distribuzione, è necessario includere le funzionalità del servizio MIM (ad eccezione della creazione dei report MIM) e del portale MIM. È anche possibile selezionare il portale di reimpostazione della password MIM e il servizio di notifica di modifica della password MIM.

6. Nella pagina **Configure the MIM database connection** (Configurare la connessione di database MIM) specificare **Create a new database** (Crea un nuovo database).

    ![Immagine relativa alla connessione di database MIM](media/install-mim-service-portal/MIM_Install10.png)

7. In **Configura la connessione del server della posta** immettere il nome del server Exchange come **Server della posta**. In alternativa, è possibile usare la **cassetta postale O365**. Se non è disponibile un server della posta configurato, specificare **localhost** come nome del server della posta e deselezionare le prime due caselle di controllo. Fare clic su **Avanti**.

    ![Immagine relativa alla configurazione della connessione al server della posta](media/install-mim-service-portal/MIM_Install11.png)

8. Specificare che si desidera generare un nuovo certificato autofirmato oppure selezionare il certificato rilevante.

9. Specificare il nome dell'account del servizio da usare, ad esempio *MIMService*, e la password dell'account del servizio, ad esempio <em>Pass@word1</em>, il dominio account del servizio, ad esempio *contoso*, e l'account di posta elettronica del servizio, ad esempio *contoso*.

    ![Immagine relativa alla configurazione dell'account del servizio MIM](media/install-mim-service-portal/MIM_Install12.png)

10. Si noti che potrebbe essere visualizzato un avviso in cui si afferma che l'account del servizio non è protetto nella configurazione corrente.

11. Accettare le impostazioni predefinite per il percorso del server di sincronizzazione e specificare l'account dell'agente di gestione MIM *contoso\MIMMA*.

    ![Immagine relativa alla configurazione del servizio e del portale MIM](media/install-mim-service-portal/MIM_Install13.png)

12. Specificare *CorpIDM* (nome del computer) come indirizzo del server del servizio MIM per il portale MIM.

13. Specificare *http://mim.contoso.com* come URL della raccolta siti di SharePoint.

14. Specificare *http://passwordregistration.contoso.com* come URL di registrazione della password sulla porta 80. È consigliabile eseguire l'aggiornamento in un secondo momento con un certificato SSL sulla porta 443.

15. Specificare *http://passwordreset.contoso.com* come URL di reimpostazione della password sulla porta 80. È consigliabile eseguire l'aggiornamento in un secondo momento con un certificato SSL sulla porta 443.

16. Selezionare la casella di controllo per aprire le porte 5725 e 5726 nel firewall e la casella di controllo per concedere l'accesso al portale MIM a tutti gli utenti autenticati.

## <a name="configure-mim-password-registration-portal"></a>Configurare il portale di registrazione della password MIM

1. Impostare il nome dell'account del servizio per la registrazione SSPR su *contoso\MIMSSPR* e la relativa password su <em>Pass@word1</em>.

2. Specificare *passwordregistration.contoso.com* come nome host per la registrazione della password MIM e impostare la porta su **80**. Abilitare l'opzione **Apri porta nel firewall**.

   ![Immagine relativa all'immissione delle informazioni di configurazione usate da IIS](media/install-mim-service-portal/MIM_Install14.png)

3. Verrà visualizzato un messaggio di avviso: leggerlo e fare clic su **Avanti**.

4. Nella schermata successiva di configurazione del portale di registrazione della password MIM specificare *mim.contoso.com* come indirizzo del server del servizio MIM per il portale di registrazione della password.

## <a name="configure-mim-password-reset-portal"></a>Configurare il portale di reimpostazione della password MIM

1. Impostare il nome dell'account del servizio per la registrazione SSPR su *Contoso\MIMSSPR* e la relativa password su <em>Pass@word1</em>.

2. Specificare *passwordreset.contoso.com* come nome host per il portale di reimpostazione della password MIM, quindi impostare la porta su **80**. Abilitare l'opzione **Apri porta nel firewall**.

   ![Immagine relativa all'immissione delle informazioni di configurazione usate da IIS](media/install-mim-service-portal/MIM_Install15.png)

3. Verrà visualizzato un messaggio di avviso: leggerlo e fare clic su **Avanti**.

4. Nella schermata successiva di configurazione del portale di registrazione della password MIM specificare *mim.contoso.com* come indirizzo del server del servizio MIM per il portale di reimpostazione della password.

## <a name="install-mim-service-and-portal"></a>Installare Portale e Servizio MIM

Quando tutte le definizioni di pre-installazione sono pronte, fare clic su **Installa** per avviare l'installazione dei componenti del **servizio e del portale** selezionati.

Al termine dell'installazione, verificare che il portale MIM sia attivo.

1. Avviare Internet Explorer e connettersi al portale MIM all'indirizzo *http://mim.contoso.com/identitymanagement*. Si noti che nella prima visita a questa pagina potrebbe verificarsi un breve ritardo.

    - Se necessario, eseguire l'autenticazione come *contoso\miminstall* in Internet Explorer.

2. In Internet Explorer, aprire **Opzioni Internet**, passare alla scheda **Protezione** e aggiungere il sito alla zona **Intranet locale** , se non è già presente.  Chiudere la finestra di dialogo **Opzioni Internet** .

3. Consentire agli utenti di visualizzare la propria voce in MIM.

    1.  Con Internet Explorer, in **Portale MIM**fare clic su **Regole dei criteri di gestione**.

    2.  Cercare la regola di criteri di gestione **Gestione utenti: gli utenti possono leggere i propri attributi**.

    3.  Selezionare questa regola di criteri di gestione e deselezionare **Criteri disabilitati**.

    4.  Fare clic su **OK** e quindi su **Invia**.

4.  Verificare che il firewall consenta le connessioni in entrata sulla porta TCP 5725 e 5726.

    1.  Avviare **Strumenti di amministrazione » Windows Firewall** con **Sicurezza avanzata**.

    2.  Fare clic su **Regole connessioni in entrata**.

    3.  Verificare che siano visualizzate le due regole seguenti:

        -   Forefront Identity Manager Service (STS).

        -   Forefront Identity Manager Service (Webservice).

    4.  Completare la procedura guidata e chiudere l’applicazione **Windows Firewall** .

    5.  Avviare **Pannello di controllo » Rete e Internet » Visualizza attività e stato della rete**.

    6.  Verificare che sia presente una rete attiva elencata come contoso.local come rete di dominio.

    7.  Chiudere il **Pannello di controllo**.

> [!NOTE]
> Facoltativo: a questo punto è possibile installare i componenti aggiuntivi e le estensioni di MIM.
> 
> [!div class="step-by-step"]  
> [« Servizio di sincronizzazione MIM](install-mim-sync.md)
> [Sincronizzare i database »](install-mim-sync-ad-service.md)
