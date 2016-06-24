---
# required metadata

title: Installare MIM 2016&#58; Servizio e portale MIM | Microsoft Identity Manager
description: Procedura di configurazione e installazione del servizio e del portale MIM per Microsoft Identity Manager 2016
keywords:
author: kgremban
manager: stevenpo
ms.date: 04/28/2016
ms.topic: get-started-article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: b0b39631-66df-4c5f-80c9-a1774346f816

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Installare MIM 2016: servizio e portale MIM

>[!div class="step-by-step"]

> [!NOTE]
> « Servizio di sincronizzazione MIM Sincronizzare i database » Questa procedura dettagliata usa nomi e valori di esempio della società Contoso.
> - Sostituirli con i propri nomi e valori.
> - Ad esempio:
> - Nome del controller di dominio: **mimservername**
> - Nome del dominio: **contoso**

Password: **Pass@word1**


## Nome account del servizio: **MIMService**

1. Se non è stato configurato il pacchetto di installazione MIM nell'ultimo passaggio, tornare indietro e installare i componenti di Microsoft Identity Manager 2016 prima di procedere.

2. Configurare il servizio e il portale MIM per l'installazione

3. Eseguire il **programma di installazione del servizio e del portale MIM** dalla sottocartella **Service and Portal** decompressa.

4. Nella schermata di benvenuto, fare clic su **Avanti**.

5. Leggere il contratto di licenza con l'utente finale e, se si accettano le condizioni, fare clic su **Avanti**. Nella schermata **Analisi utilizzo software MIM**, fare clic su **Avanti**.

6. Quando si selezionano le funzionalità dei componenti per questa distribuzione, è necessario includere le funzionalità del servizio MIM (ad eccezione della creazione dei report MIM) e del portale MIM.

    ![È anche possibile selezionare il portale di reimpostazione della password MIM e il servizio di notifica di modifica della password MIM.](media/MIM-Install10.png)

7. Nella pagina **Configure the MIM database connection** (Configurare la connessione di database MIM) specificare **Create a new database** (Crea un nuovo database). Immagine relativa alla connessione di database MIM In **Configura la connessione del server della posta** immettere il nome del server Exchange come **Server della posta**.

    ![Se non è disponibile un server della posta configurato, specificare **localhost** come nome del server della posta e deselezionare le prime due caselle di controllo.](media/MIM-Install11.png)

8. Fare clic su **Avanti**.

9. Immagine relativa alla configurazione della connessione al server della posta

    ![Specificare che si desidera generare un nuovo certificato autofirmato oppure selezionare il certificato rilevante.](media/MIM-Install12.png)

10. Specificare il nome dell'account del servizio da usare, ad esempio, *MIMService*, e la password dell'account del servizio, ad esempio, *Pass@word1*, il dominio account del servizio, ad esempio *contoso*, e l'account di posta elettronica del servizio, ad esempio *contoso*.

11. Immagine relativa alla configurazione dell'account del servizio MIM

    ![Si noti che potrebbe essere visualizzato un avviso in cui si afferma che l'account del servizio non è protetto nella configurazione corrente.](media/MIM-Install13.png)

12. Accettare le impostazioni predefinite per il percorso del server di sincronizzazione e specificare l'account dell'agente di gestione MIM *contoso\MIMsync*.

13. Immagine relativa alla configurazione del servizio e del portale MIM

14. Specificare *CorpIDM* (nome del computer) come indirizzo del server del servizio MIM per il portale MIM.

15. Specificare *http://CorpIDM.contoso.local:82* come URL della raccolta siti di SharePoint.

16. Specificare *http://CorpIDM.contoso.local:8080* come URL di registrazione della password.

## Specificare *http://CorpIDM.contoso.local:8088* come URL di reimpostazione della password.

1.  Selezionare la casella di controllo per aprire le porte 5725 e 5726 nel firewall e la casella di controllo per concedere l'accesso al portale MIM a tutti gli utenti autenticati.

2.  Configurare il portale di registrazione della password MIM Impostare il nome dell'account del servizio per la registrazione SSPR su *contoso\MIMSSPR* e la relativa password su *Pass@word1*.

    ![Specificare *CORPIDM* come nome host per la registrazione della password MIM e impostare la porta su **8080**.](media/MIM-Install14.png)

3.  Abilitare l'opzione **Apri porta nel firewall**.

4. Immagine relativa all'immissione delle informazioni di configurazione usate da IIS

## Verrà visualizzato un messaggio di avviso: leggerlo e fare clic su **Avanti**.

1.  Nella schermata successiva di configurazione del portale di registrazione della password MIM specificare *http://CorpIDM.contoso.local* come indirizzo del server del servizio MIM per il portale di registrazione della password.

2.  Configurare il portale di reimpostazione della password MIM Impostare il nome dell'account del servizio per la registrazione SSPR su *Contoso\MIMSSPRService* e la relativa password su *Pass@word1*.

    ![Specificare *CORPIDM* come nome host per la registrazione della password MIM e impostare la porta su **8080**.](media/MIM-Install15.png)

3.  Abilitare l'opzione **Apri porta nel firewall**.

4. Immagine relativa all'immissione delle informazioni di configurazione usate da IIS

## Verrà visualizzato un messaggio di avviso: leggerlo e fare clic su **Avanti**.

Nella schermata successiva di configurazione del portale di registrazione della password MIM specificare *CorpIDname http://CorpIDname.domain.local* come indirizzo del server del servizio MIM per il portale di reimpostazione della password.

Installare Portale e Servizio MIM

1. Quando tutte le definizioni di pre-installazione sono pronte, fare clic su **Installa** per avviare l'installazione dei componenti del **servizio e del portale** selezionati. Al termine dell'installazione, verificare che il portale MIM sia attivo.

    - Avviare Internet Explorer e connettersi al portale MIM all'indirizzo *http://corpidm.contoso.local:82/identitymanagement*.

2. Si noti che nella prima visita a questa pagina potrebbe verificarsi un breve ritardo.  Se necessario, eseguire l'autenticazione come *contoso\Administrator* in Internet Explorer.

3. In Internet Explorer, aprire **Opzioni Internet**, passare alla scheda **Protezione** e aggiungere il sito alla zona **Intranet locale** , se non è già presente.

    1.  Chiudere la finestra di dialogo **Opzioni Internet** .

    2.  Consentire agli utenti di visualizzare la propria voce in MIM.

    3.  Con Internet Explorer, in **Portale MIM**fare clic su **Regole dei criteri di gestione**.

    4.  Cercare la regola di criteri di gestione **Gestione utenti: gli utenti possono leggere i propri attributi**.

4.  Selezionare questa regola di criteri di gestione e deselezionare **Criteri disabilitati**.

    1.  Fare clic su **OK** e quindi su **Invia**.

    2.  Verificare che il firewall consenta le connessioni in entrata sulla porta TCP 5725 e 5726.

    3.  Avviare **Strumenti di amministrazione » Windows Firewall** con **Sicurezza avanzata**.

        -   Fare clic su **Regole connessioni in entrata**.

        -   Verificare che siano visualizzate le due regole seguenti:

    4.  Forefront Identity Manager Service (STS).

    5.  Forefront Identity Manager Service (Webservice).

    6.  Completare la procedura guidata e chiudere l’applicazione **Windows Firewall** .

    7.  Avviare **Pannello di controllo » Rete e Internet » Visualizza attività e stato della rete**.

> Verificare che sia presente una rete attiva elencata come contoso.local come rete di dominio.

>Chiudere il **Pannello di controllo**.  
Facoltativo: a questo punto è possibile installare i componenti aggiuntivi e le estensioni di MIM.


<!--HONumber=Apr16_HO3-->


