---
title: Distribuire il servizio di notifica di modifica della password | Documentazione Microsoft
description: Procedura per installare e configurare il servizio di notifica di modifica della password di MIM nel controller di dominio.
keywords: 
author: billmath
ms.author: barclayn
manager: mbaldwin
ms.date: 10/12/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 97edae12-6f86-4f9f-8620-a95a096e482a
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 6ce6f8b78d7ea3518bd5d4beeada51cbc3fdc5a3
ms.sourcegitcommit: f077508b5569e2a96084267879c5b6551e1e0905
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/12/2017
---
# <a name="deploy-the-mim-password-change-notification-service-on-a-domain-controller"></a>Distribuire il servizio di notifica di modifica della password di MIM in un controller di dominio

## <a name="install-the-password-change-notification-service"></a>Installare il servizio di notifica di modifica della password
Il servizio di notifica di modifica della password (PCNS) è un servizio che viene installato nei controller di dominio e che consente la sincronizzazione delle password tra MIM ed altri sistemi, ad esempio un server di directory di un altro fornitore. Per la sincronizzazione delle password, installare PCNS in ogni server del controller di dominio.

1.  Accedere come amministratore di dominio a un server che esegue Windows Server con il ruolo Servizi di dominio Active Directory.

2.  Copiare la cartella di installazione del servizio PCNS nel computer.

3.  Individuare il file *Password Change Notification Service.msi* , fare clic con il pulsante destro su esso e creare un collegamento.

4.  Individuare il file collegamento, fare clic con il pulsante destro e visualizzare le **Proprietà**.

5.  Nel campo Destinazione aggiungere il preambolo *msiexec.exe /i* prima del percorso del file msi e il suffisso *SCHEMAONLY=TRUE* dopo il percorso del file msi. Ad esempio, se la cartella di installazione è *C:\PCNS*, il comando da eseguire sarà analogo al seguente (in una sola riga):

    ```
    msiexec.exe /i "C:\PCNS\x64\Password Change Notification Service.msi" SCHEMAONLY=TRUE
    ```

6.  Salvare le modifiche apportate al file di collegamento.

7.  Eseguire il file collegamento per avviare l'installazione di PCNS in modalità di estensione dello schema. Quando viene visualizzata la schermata seguente, fare clic su **Avanti**.

8.  Si riceverà la notifica che il programma di installazione aggiornerà lo schema di Active Directory per il servizio di notifica di modifica della password. Fare clic su **OK** per procedere con l'aggiornamento dello schema.

9. Al termine del processo di estensione dello schema, quando viene visualizzata la schermata seguente fare clic su **Fine**.

10. Eseguire di nuovo il file *Password Change Notification Service.msi* , questa volta direttamente (non è necessaria alcuna stringa di esecuzione).  Quando viene visualizzata la schermata seguente, fare clic su **Avanti**.

11. Accettare le condizioni del contratto di licenza e fare clic su **Avanti**.

12. Fare clic per avviare l'installazione.

13. Nella schermata di installazione completata correttamente, fare clic su **Fine**.

14. Riavviare il computer per rendere effettive le modifiche della configurazione apportate al servizio di notifica di modifica della password di MIM. A tale scopo, è possibile fare clic su **Sì** nella finestra popup visualizzata oppure è possibile riavviare in seguito.

## <a name="configuring-the-password-change-notification-service"></a>Configurazione del servizio di notifica di modifica della password
Dopo la riconnessione al server di controller di dominio come amministratore di dominio, passare a *C:\Programmi\Microsoft Password Change Notification*. Eseguire *pcnscfg.exe*.
