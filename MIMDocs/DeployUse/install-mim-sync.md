---
title: Installare il servizio di sincronizzazione di MIM | Microsoft Identity Manager
description: Per iniziare a usare i componenti di MIM 2016, installare e configurare il servizio di sincronizzazione.
keywords: 
author: kgremban
manager: femila
ms.date: 08/11/2016
ms.topic: get-started-article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 2585e9c5-ce34-46c7-bdcf-8c08773901dc
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 739797502e09c2b92e35767e2c943308cd1de5c9
ms.openlocfilehash: ba399da1431ea1d2d1633d0469e054ffd89f1d34


---

# Installare il servizio di sincronizzazione di MIM 2016: servizio di sincronizzazione MIM

>[!div class="step-by-step"]
[« Exchange Server](prepare-server-exchange.md)
[Servizio e portale MIM »](install-mim-service-portal.md)

> [!NOTE]
> Questa procedura dettagliata usa nomi e valori di esempio della società Contoso. Sostituirli con i propri nomi e valori. Ad esempio:
> - Nome del controller di dominio: **mimservername**
> - Nome del dominio: **contoso**
> - Password: **Pass@word1**

Per installare Microsoft Identity Manager 2016, configurare innanzitutto il pacchetto di installazione.

1. Accedere come *contoso\Administrator* al server usato per la gestione delle identità.

2. Decomprimere il pacchetto di installazione MIM o montare il DVD dell'immagine MIM.

## Installare il servizio di sincronizzazione di MIM 2016

1. Nella cartella di installazione di MIM decompressa, passare alla cartella **Servizio di sincronizzazione** .

2. Eseguire il **programma di installazione del servizio di sincronizzazione di MIM**. Seguire le istruzioni del programma di installazione e completare l'installazione.

3. Nella schermata di benvenuto, fare clic su **Avanti**.

    ![Immagine iniziale del programma di installazione guidata di MIM](media/MIM-Install1.png)

4. Leggere le condizioni di licenza e fare clic su **Avanti** per accettarle.

5. Nella schermata **Installazione personalizzata** fare clic su **Avanti**.

    ![Immagine dell'installazione personalizzata](media/MIM-Install2.png)

6.  Nella schermata di configurazione del database del servizio di sincronizzazione, selezionare:

    1.  SQL Server si trova in: **Questo computer**.

    2.  L’istanza SQL Server è: **Istanza predefinita**

    ![Immagine della connessione di database](media/MIM-Install3.png)

7.  Configurare l’account del servizio di sincronizzazione in base all'account creato in precedenza:

    1.  Account del servizio: *MIMSync*

    2.  Password: *Pass@word1*

    3.  Dominio dell'account del servizio o nome del computer locale: *contoso*

    ![Immagine dell'account del servizio](media/MIM-Install4.png)

8.  Fornire al programma di installazione del servizio di sincronizzazione MIM i gruppi di sicurezza pertinenti:

    1. Amministratore = *contoso\MIMSyncAdmins*

    2. Operatore = *contoso\MIMSyncOperators*

    3. Partecipante = *contoso\MIMSyncJoiners*

    4. Navigazione connettore = *contoso\MIMSyncBrowse*

    5. Gestione password WMI = *contoso\MIMSyncPasswordReset*

    ![Immagine dei gruppi di sicurezza](media/MIM-Install5.png)

9. Nella schermata delle impostazioni di protezione, selezionare **Abilita regole firewall per le comunicazioni RPC in ingresso** e fare clic su **Avanti**.

10. Fare clic su **Installa** per avviare l'installazione del servizio di sincronizzazione MIM.

    1. Potrebbe apparire un avviso riguardante l'account del servizio di sincronizzazione MIM. Fare clic su **OK**.

    2. Verrà installato il servizio di sincronizzazione MIM.

    3. Viene visualizzato un avviso relativo alla creazione di un backup della chiave di crittografia. Fare clic su **OK**, quindi selezionare una cartella per archiviare il backup della chiave di crittografia.

        ![Immagine di avviso relativa al backup della chiave di crittografia di Sincronizzazione MIM](media/MIM-Install7.png)

    4. Quando il programma di installazione completata correttamente l'installazione, fare clic su **Fine**.

    5. È necessario disconnettersi e accedere in modo da rendere effettive le modifiche dell'appartenenza al gruppo. Fare clic su **Sì** per disconnettersi.

>[!div class="step-by-step"]  
[« Exchange Server](prepare-server-exchange.md)
[Servizio e portale MIM »](install-mim-service-portal.md)



<!--HONumber=Aug16_HO2-->


