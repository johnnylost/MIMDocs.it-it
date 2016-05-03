---
# required metadata

title: Eseguire l'aggiornamento da Forefront Identity Manager 2010 R2 | Microsoft Identity Manager
description: Informazioni su come aggiornare i componenti di FIM 2010 R2 e installare i nuovi componenti di MIM 2016.
keywords:
author: kgremban
manager: stevenpo
ms.date: 04/28/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 9471ccc1-bafe-46ee-b169-1464262380e1

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Eseguire l'aggiornamento da Forefront Identity Manager 2010 R2 a Microsoft Identity Manager 2016
In questa sezione viene illustrato l'aggiornamento di un sistema FIM 2010 R2 di test esistente a MIM 2016. I programmi di installazione utilizzati per l'aggiornamento sono gli stessi adoperati per la nuova distribuzione.

In questa sezione si presuppone che si disponga di una soluzione FIM 2010 R2 esistente distribuita in un ambiente di test. I server sono in esecuzione in Windows Server 2012, Windows Server 2012 R2 o Windows Server 2008 R2, che sono i sistemi operativi comuni per i server FIM 2010 R2, e tutti i prerequisiti locali e ambientali (SQL Server, Exchange Server, SharePoint Services e così via) sono configurati per FIM 2010 R2.

1.  Innanzitutto verrà installato il servizio di sincronizzazione MIM (Sincronizzazione), che verrà eseguito in un server con dominio aggiunto al dominio di Active Directory che sostituisce l'istanza di Sincronizzazione di FIM 2010 R2.

2.  Verranno quindi installate le funzionalità Servizio e Portale MIM, che possono includere il portale di registrazione SSPR e il portale del servizio SSPR, senza insieme di funzionalità di gestione di accesso con privilegi.

3.  I componenti aggiuntivi e le estensioni MIM, incluso il client integrato di accesso SSPR Windows, possono quindi essere distribuiti in un computer client separato.

## Preparazione

1.  Eseguire il backup del database del servizio FIM, del database di sincronizzazione FIM, dell’applicazione di sincronizzazione FIM e del software e della configurazione del servizio.

2.  In ogni server in cui sono installati componenti di FIM 2010 R2, ad esempio *CORPIDM*, accedere come Contoso\Administrator. In questo esempio di distribuzione, sono necessari diritti amministrativi per eseguire l'aggiornamento di FIM 2010 R2 a **MIM**.

3.  Scaricare o decomprimere il software MIM.

## Eseguire l'aggiornamento del servizio di sincronizzazione

1.  Accedere come amministratore a un server in cui viene distribuito il servizio di sincronizzazione FIM 2010 R2 ("Sincronizzazione").

2.  Assicurarsi di eseguire il backup del database prima di iniziare questa procedura.

3.  Aprire la console **Servizi** , individuare il **servizio di sincronizzazione di Forefront Identity Manager**e arrestarlo.

    ![Immagine della console del servizio](media/MIM-UpgFIM1.PNG)

4.  Eseguire il **programma di installazione del servizio di sincronizzazione di MIM**. Il programma di installazione rileverà la versione di Sincronizzazione esistente e suggerirà un aggiornamento. Fare clic sul pulsante **Aggiorna** per continuare.

5.  Se si accettano le condizioni di licenza, fare clic su **Avanti** .

6.  Immettere la password per l'account del servizio utilizzato da Sincronizzazione e fare clic su **Avanti**.

    ![Immagine dell'account del servizio di sincronizzazione MIM](media/MIM-UpgFIM3.png)

7.  Verificare che i nomi dei gruppi di sicurezza siano corretti e fare clic su **Avanti**.

    ![Immagine della configurazione dei gruppi di sicurezza del servizio di sincronizzazione MIM](media/MIM-UpgFIM4.png)

8.  Lasciare invariata la casella di controllo per le regole firewall per le comunicazioni RPC in ingresso.

9. Il programma di installazione è pronto per l'aggiornamento di Sincronizzazione da FIM 2010 R2 a MIM. Fare clic su **Aggiorna** per avviare il processo di aggiornamento.

10. L'aggiornamento è in corso. Non chiudere il programma di installazione o riavviare il computer mentre l'aggiornamento è in corso.

    ![Immagine di stato dell'installazione di Sincronizzazione MIM](media/MIM-UpgFIM7.png)

11. Durante l'aggiornamento, viene visualizzato un avviso riguardante l'aggiornamento di Sincronizzazione. È consigliabile eseguire il backup del e il database prima di iniziare.

12. Al temine dell’aggiornamento, scegliere **Fine**.

    ![Immagine relativa all'avvenuta installazione di Sincronizzazione MIM](media/MIM-UpgSP1.png)

13. Si noti che il **servizio di sincronizzazione** è stato riavviato.

## Eseguire l'aggiornamento del servizio e del portale

1.  Accedere come amministratore a un server in cui sono distribuite le funzionalità Servizio e Portale di FIM 2010 R2.

2.  Aprire la console **Servizi** , individuare il **servizio Forefront Identity Manager**e arrestarlo.

    ![Immagine della console del servizio](media/MIM-UpgFIM9.PNG)

3.  Eseguire il programma di installazione di Servizio e Portale MIM. Fare clic sul pulsante **Installa** per procedere.

    ![Immagine relativa all'installazione del servizio e del portale MIM](media/MIM-UpgSP2.png)

4.  Se si accettano le condizioni di licenza, fare clic su **Avanti** .

5.  Nella schermata Analisi utilizzo software di MIM, fare clic su **Avanti** per procedere.

6.  Selezionare le funzionalità e i componenti MIM da installare e al termine fare clic su **Avanti**.

    ![Immagine dell'installazione personalizzata](media/MIM-UpgSP4.png)

    1.  **Servizio MIM**: questa funzionalità è obbligatoria in almeno un server e richiede un server di database SQL Server in un percorso condiviso o in un altro server.

    2.  **Portale MIM:** questa funzionalità è obbligatoria in almeno un server e richiede SharePoint Foundation 2013.

    3.  **Portale di registrazione della password MIM**: questa funzionalità è necessaria per la reimpostazione della password self-service.

    4.  **Portale di reimpostazione password di MIM:** questa funzionalità è necessaria per la reimpostazione della password.

7.  Immettere i dettagli di SQL Server utilizzato per il database del servizio FIM. Selezionare l'opzione che consente di riutilizzare il database esistente e mantenere i dati. Fare clic su **Avanti** per continuare.

8. Se è stata selezionata l'opzione di riutilizzo del database esistente, viene visualizzato un promemoria per il backup del database.

9. Immettere i dettagli del server di posta. Se il server della posta si trova nel server corrente, immettere "localhost" come percorso del server della posta. Fare clic su **Avanti** per continuare.

    ![Immagine relativa alla configurazione della connessione al server della posta](media/MIM-UpgSP6.png)

10. Selezionare un certificato per il servizio da utilizzare per convalidare i client. È consigliabile utilizzare il certificato esistente dall'archivio certificati locale utilizzato precedentemente dal servizio FIM.

    ![Immagine relativa alla configurazione del certificato di servizio](media/MIM-UpgSP7.png)

    1.  Se è stata selezionata l'opzione archivio certificati, fare clic sul pulsante **Seleziona certificato** e selezionare un certificato dall'elenco nella finestra popup. Fare clic su **OK**, quindi su **Avanti**.

        ![Immagine relativa alla finestra popup della selezione del certificato](media/MIM-UpgSP8.PNG)

11. Configurare le credenziali dell'account del servizio per Servizio MIM. Si noti che l'account del servizio non può corrispondere a quello utilizzato dal servizio di sincronizzazione, deve essere lo stesso account utilizzato dal servizio FIM.

    ![Immagine relativa alla configurazione dell'account del servizio MIM](media/MIM-UpgSP9.png)

12. Configurare i dettagli del server di Sincronizzazione MIM in base alla distribuzione del servizio MIM configurato in un passaggio precedente.

    ![Immagine relativa alla configurazione della sincronizzazione del servizio e del portale MIM](media/MIM-UpgSP10.png)

13. Quando si installa il portale MIM, fornire l'indirizzo del server del servizio MIM. Fare clic su **Avanti**.

14. Quando si installa il portale MIM, fornire l'URL della raccolta siti di SharePoint in cui è attualmente ospitato il portale FIM. Fare clic su **Avanti**.

## Installare il portale di registrazione della password MIM

1. Se si sta installando il portale di registrazione password di MIM, fornire l'URL richiesto per il portale di registrazione password. Fare clic su **Avanti**.

2. Configurare la capacità per i client e gli utenti finali di utilizzare il servizio e il portale.

    1.  Decidere se si desidera **aprire le porte 5725 e 5726 nel firewall**.

    2.  Decidere se si vuole **concedere agli utenti autenticati l'accesso al sito portale MIM**.

    3.  Fare clic su **Avanti**.

3. Specificare i dettagli di accesso e le credenziali per la registrazione della password MIM.

    ![Immagine relativa alla configurazione del portale di registrazione della password MIM](media/MIM-UpgSP15.png)

    1.  Fornire il nome dell’account di servizio (incluso il dominio) e la password per la registrazione della password MIM.

    2.  Fornire i dettagli dell'host, nome e porta (ad esempio 8080), del portale di registrazione password.

    3.  Selezionare l’opzione per **aprire la porta nel firewall** .

    4.  Fare clic su **Avanti**.

4. Nella schermata di configurazione della registrazione password MIM:

    1.  Indicare alla funzionalità di registrazione password MIM dove si trova Servizio MIM, in genere nello stesso sistema.

    2.  Determinare se a questo portale possono accedere utenti di extranet e intranet o solo gli utenti dell’intranet, come in precedenza è stato configurato per la reimpostazione password FIM.

## Installare il portale di reimpostazione della password MIM

1. Se si sta installando il portale di reimpostazione password MIM, fornire i dettagli e le credenziali di accesso per la reimpostazione della password MIM.

    ![Immagine relativa alla configurazione del portale di reimpostazione della password MIM](media/MIM-UpgSP17.png)

    1.  Fornire il nome dell’account di servizio (incluso il dominio) e la password per la reimpostazione passwor MIM.

    2.  Fornire i dettagli dell'host, nome e porta (ad esempio 8088), del portale di reimpostazione password.

    3.  Selezionare l’opzione per **aprire la porta nel firewall** .

    4.  Fare clic su **Avanti**.

2. Nella schermata di configurazione della reimpostazione password MIM:

    1.  Indicare alla funzionalità di reimpostazione password MIM dove si trova Servizio MIM.

    2.  Specificare se a questo portale possono accedere utenti di extranet e intranet o solo gli utenti dell’intranet.

## Terminare l'installazione e l'aggiornamento

1. Una volta completate correttamente tutte le definizioni di configurazione, verrà visualizzata una pagina di installazione. Fare clic su **Installa** per iniziare l'installazione e l’aggiornamento di Servizio e Portale MIM.

2. L'installazione e aggiornamento di Servizio e Portale MIM è in corso. Non annullare il programma di installazione o riavviare il computer durante l'installazione.

3. Una volta completata l'installazione (aggiornamento) del servizio e del portale MIM, viene visualizzata una schermata di conferma. Fare clic su **Fine** per completare l'installazione e uscire dal programma di installazione.

4. Verificare che il servizio **Forefront Identity Manager** sia stato riavviato.

Nota: se le estensioni e i componenti aggiuntivi FIM sono attualmente installati nei computer dell'utente per SSPR, non configurare i nuovi controlli telefonici MFA per la reimpostazione della password fino a quando non verrà eseguito l'aggiornamento di tutti i componenti aggiuntivi e di tutte le estensioni FIM a MIM 2016.  Le estensioni e i componenti aggiuntivi di FIM 2010 e FIM 2010 R2 non riconoscono i nuovi controlli, quindi verrà generato un errore e l'utente non riuscirà a completare la reimpostazione della password.


<!--HONumber=Apr16_HO2-->


