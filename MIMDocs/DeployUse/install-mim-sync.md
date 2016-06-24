---
# required metadata

title: Installare MIM 2016 & #58; Servizio di sincronizzazione MIM | Microsoft Identity Manager
description: Per iniziare a usare i componenti di MIM 2016, installare e configurare il servizio di sincronizzazione.
keywords:
author: kgremban
manager: stevenpo
ms.date: 04/28/2016
ms.topic: get-started-article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 2585e9c5-ce34-46c7-bdcf-8c08773901dc

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Installare il servizio di sincronizzazione di MIM 2016: servizio di sincronizzazione MIM

>[!div class="step-by-step"]

> [!NOTE]
> « Exchange Server Servizio e portale MIM » Questa procedura dettagliata usa nomi e valori di esempio della società Contoso.
> - Sostituirli con i propri nomi e valori.
> - Ad esempio:
> - Nome del controller di dominio: **mimservername**

Nome del dominio: **contoso**

1. Password: **Pass@word1**

2. Per installare Microsoft Identity Manager 2016, configurare innanzitutto il pacchetto di installazione.

## Accedere come *contoso\Administrator* al server usato per la gestione delle identità.

1. Decomprimere il pacchetto di installazione MIM o montare il DVD dell'immagine MIM.

2. Installare il servizio di sincronizzazione di MIM 2016 Nella cartella di installazione di MIM decompressa, passare alla cartella **Servizio di sincronizzazione** .

3. Eseguire il **programma di installazione del servizio di sincronizzazione di MIM**.

    ![Seguire le istruzioni del programma di installazione e completare l'installazione.](media/MIM-Install1.png)

4. Nella schermata di benvenuto, fare clic su **Avanti**.

5. Immagine iniziale del programma di installazione guidata di MIM

    ![Leggere le condizioni di licenza e fare clic su **Avanti** per accettarle.](media/MIM-Install2.png)

6.  Nella schermata **Installazione personalizzata** fare clic su **Avanti**.

    1.  Immagine dell'installazione personalizzata

    2.  Nella schermata di configurazione del database di sincronizzazione, selezionare:

    ![SQL Server si trova in: **Questo computer**.](media/MIM-Install3.png)

7.  L’istanza SQL Server è: **Istanza predefinita**

    1.  Immagine della connessione di database

    2.  Configurare l’account del servizio di sincronizzazione in base all'account creato in precedenza:

    3.  Account del servizio: *MIMSync*

    ![Password: *Pass@word1*](media/MIM-Install4.png)

8.  Dominio dell'account del servizio o nome del computer locale: *contoso*

    1. Immagine dell'account del servizio

    2. Fornire al programma di installazione di MIM i gruppi di sicurezza pertinenti:

    3. Amministratore = *contoso\MIMSyncAdmins*

    4. Operatore = *contoso\MIMSyncOperators*

    5. Partecipante = *contoso\MIMSyncJoiners*

    ![Navigazione connettore = *contoso\MIMSyncBrowse*](media/MIM-Install5.png)

9. Gestione password WMI = *contoso\MIMSyncPasswordReset*

10. Immagine dei gruppi di sicurezza

    1. Nella schermata delle impostazioni di protezione, selezionare **Abilita regole firewall per le comunicazioni RPC in ingresso** e fare clic su **Avanti**.

    2. Fare clic su **Installa** per avviare l'installazione di Sincronizzazione MIM.

    3. Potrebbe apparire un avviso riguardante l'account del servizio di sincronizzazione MIM. Fare clic su **OK**.

        ![Sincronizzazione MIM verrà installato.](media/MIM-Install7.png)

    4. Viene visualizzato un avviso relativo alla creazione di un backup della chiave di crittografia. Fare clic su **OK**, quindi selezionare una cartella per archiviare il backup della chiave di crittografia.

    5. Immagine di avviso relativa al backup della chiave di crittografia di Sincronizzazione MIM Quando il programma di installazione completata correttamente l'installazione, fare clic su **Fine**.

>È necessario disconnettersi e accedere in modo da rendere effettive le modifiche dell'appartenenza al gruppo.  
Fare clic su **Sì** per disconnettersi.


<!--HONumber=Apr16_HO3-->


