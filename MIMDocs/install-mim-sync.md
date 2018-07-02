---
title: Installare il servizio di sincronizzazione di Microsoft Identity Manager | Documentazione Microsoft
description: Per iniziare a usare i componenti di MIM 2016, installare e configurare il servizio di sincronizzazione.
keywords: ''
author: fimguy
ms.author: barclayn
manager: mbaldwin
ms.date: 05/01/2018
ms.topic: get-started-article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 2585e9c5-ce34-46c7-bdcf-8c08773901dc
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: c68b33b2ff28d75b6f4e63fa8caf0c87727a5927
ms.sourcegitcommit: 35f2989dc007336422c58a6a94e304fa84d1bcb6
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36289398"
---
# <a name="install-mim-2016-mim-synchronization-service"></a>Installare il servizio di sincronizzazione di MIM 2016: servizio di sincronizzazione MIM

> [!div class="step-by-step"]
> [« Exchange Server](prepare-server-exchange.md)
> [Servizio e portale MIM »](install-mim-service-portal.md)
> 
> [!NOTE]
> Questa procedura dettagliata usa nomi e valori di esempio della società Contoso. Sostituirli con i propri nomi e valori. Ad esempio:
> - Nome del controller di dominio: **corpdc**
> - Nome del dominio: **contoso**
> - Nome del server del servizio MIM: **corpservice**
> - Nome del server di sincronizzazione MIM: **corpservice**
> - Nome SQL Server: **corpsql**
> - Password: <strong>Pass@word1</strong>

Per installare Microsoft Identity Manager 2016, configurare innanzitutto il pacchetto di installazione.

1. Accedere come *contoso\miminstall* al server in uso come server di sincronizzazione per la gestione delle identità **corpsync**.

2. Decomprimere il pacchetto di installazione MIM o montare il DVD dell'immagine MIM.

## <a name="install-mim-2016-sp1-synchronization-service"></a>Installare il servizio di sincronizzazione di MIM 2016 SP1

1. Nella cartella di installazione di MIM decompressa, passare alla cartella **Servizio di sincronizzazione** .

2. Eseguire il **programma di installazione del servizio di sincronizzazione di MIM**. Seguire le istruzioni del programma di installazione e completare l'installazione.

3. Nella schermata di benvenuto, fare clic su **Avanti**.

    ![Immagine iniziale del programma di installazione guidata di MIM](media/install-mim-sync/MIM_Install1.png)

4. Leggere le condizioni di licenza e fare clic su **Avanti** per accettarle.

5. Nella schermata **Installazione personalizzata** fare clic su **Avanti**.

    ![Immagine dell'installazione personalizzata](media/install-mim-sync/MIM_Install2.png)

6. Nella schermata di configurazione del database del servizio di sincronizzazione, selezionare:

   1.  SQL Server si trova in: **un computer remoto** denominato **corpsql.contoso.com**.

   2.  L’istanza SQL Server è: **Istanza predefinita**

   ![Immagine della connessione di database](media/install-mim-sync/MIM_Install3.png)

7. Configurare l’account del servizio di sincronizzazione in base all'account creato in precedenza:

   1. Account del servizio: *MIMSync*

   2. Password: <em>Pass@word1</em>

   3. Dominio dell'account del servizio o nome del computer locale: *contoso*

   ![Immagine dell'account del servizio](media/install-mim-sync/MIM_Install4.png)

8. Fornire al programma di installazione del servizio di sincronizzazione MIM i gruppi di sicurezza pertinenti:

   1. Amministratore = *contoso\MIMSyncAdmins*

   2. Operatore = *contoso\MIMSyncOperators*

   3. Partecipante = *contoso\MIMSyncJoiners*

   4. Navigazione connettore = *contoso\MIMSyncBrowse*

   5. Gestione password WMI = *contoso\MIMSyncPasswordReset*

   ![Immagine dei gruppi di sicurezza](media/install-mim-sync/MIM_Install5.png)

9. Nella schermata delle impostazioni di protezione, selezionare **Abilita regole firewall per le comunicazioni RPC in ingresso** e fare clic su **Avanti**.

10. Fare clic su **Installa** per avviare l'installazione del servizio di sincronizzazione MIM.

    1. Potrebbe apparire un avviso riguardante l'account del servizio di sincronizzazione MIM. Fare clic su **OK**.

    2. Verrà installato il servizio di sincronizzazione MIM.

    3. Viene visualizzato un avviso relativo alla creazione di un backup della chiave di crittografia. Fare clic su **OK**, quindi selezionare una cartella per archiviare il backup della chiave di crittografia.

        ![Immagine di avviso relativa al backup della chiave di crittografia di Sincronizzazione MIM](media/MIM-Install7.png)

    4. Quando il programma di installazione completata correttamente l'installazione, fare clic su **Fine**.

    5. È necessario disconnettersi e accedere in modo da rendere effettive le modifiche dell'appartenenza al gruppo. Fare clic su **Sì** per disconnettersi.

> [!div class="step-by-step"]  
> [« Exchange Server](prepare-server-exchange.md)
> [Servizio e portale MIM »](install-mim-service-portal.md)
