---
title: 'Distribuire PAM, passaggio 7: Accesso utente | Documentazione Microsoft'
description: Nel passaggio finale concedere un accesso temporaneo a un utente con privilegi per dimostrare la riuscita della distribuzione di Privileged Access Management.
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 01/17/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 5325fce2-ae35-45b0-9c1a-ad8b592fcd07
ms.openlocfilehash: 150e850e9184fef189b00e6aee3fab50939f47b9
ms.sourcegitcommit: ace4d997c599215e46566386a1a3d335e991d821
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/15/2018
ms.locfileid: "49332817"
---
# <a name="step-7--elevate-a-users-access"></a>Passaggio 7: elevare l'accesso dell'utente

> [!div class="step-by-step"]
> [« Passaggio 6 ](step-6-transition-group-to-pam.md)


Questo passaggio illustra come un utente possa richiedere l'accesso a un ruolo tramite MIM.

## <a name="verify-that-jen-cannot-access-the-privileged-resource"></a>Verificare che Jen non possa accedere alla risorsa privilegiata

Senza privilegi elevati, Jen non può accedere alla risorsa privilegiata nella foresta CORP.

1. Disconnettersi da CORPWKSTN per rimuovere eventuali connessioni aperte memorizzate nella cache.
2. Accedere a CORPWKSTN come *CONTOSO\Jen* e passare alla visualizzazione **Desktop**.
3. Aprire un prompt dei comandi DOS.
4. Digitare il comando `dir \\corpwkstn\corpfs`. Viene visualizzato il messaggio di errore **Accesso negato**.
5. Lasciare la finestra del prompt dei comandi aperta.

## <a name="request-privileged-access-from-mim"></a>Richiedere l'accesso con privilegi da MIM

> [!NOTE]
> È consigliabile che la workstation sia una workstation con accesso con privilegi.  Per altre informazioni, vedere [Privileged Access Workstations](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/privileged-access-workstations) (Workstation con accesso con privilegi).

1. In PRIVWKSTN effettuare l'accesso come PRIV\priv.jen.
2. Fare clic su **Start**, **Esegui** e immettere **PowerShell.exe**.
3. Digitare il comando seguente.

    ```cmd
    runas /user:Priv.Jen@priv.contoso.local powershell
    ```

2. Quando richiesto, digitare la password per l'account PRIV.Jen. Verrà visualizzata una nuova finestra del prompt dei comandi.
3. Quando viene visualizzata la finestra di PowerShell digitare i comandi seguenti.

    > [!NOTE]
    > Dopo aver eseguito questi comandi, tutti i passaggi seguenti hanno una scadenza.

    ```PowerShell
    Import-module MIMPAM
    $r = Get-PAMRoleForRequest | ? { $_.DisplayName –eq "CorpAdmins" }
    New-PAMRequest –role $r
    klist purge
    ```

4. Dopo aver completato l'operazione chiudere la finestra di PowerShell.
5. Nella finestra dei comandi DOS, digitare il comando seguente:

    ```cmd
    runas /user:Priv.Jen@priv.contoso.local powershell
    ```

6. Digitare la password per l'account PRIV.Jen. Verrà visualizzata una nuova finestra del prompt dei comandi.

## <a name="validate-the-elevated-access"></a>Convalidare l'accesso con privilegi elevati.
Nella finestra appena aperta, digitare il comando seguente:

```cmd
whoami /groups
dir \\corpwkstn\corpfs
```

Se il comando dir ha esito negativo con il messaggio di errore **Accesso negato**, ricontrollare la relazione di trust.

## <a name="activate-the-privileged-role"></a>Attivare il ruolo con privilegi

Attivare richiedendo l'accesso con privilegi elevati tramite il portale di esempio PAM.

1. In CORPWKSTN assicurarsi di essere connessi come CORP\Jen.
2. In una finestra dei comandi DOS, digitare il comando seguente.

    ```cmd
    runas /user:Priv.Jen@priv.contoso.local "c:\program files\Internet Explorer\iexplore.exe"
    ```

3. Quando richiesto, digitare la password per l'account PRIV.Jen. Verrà visualizzata una nuova finestra del browser Web.
4. Passare a http://pamsrv.priv.contoso.local:8090 e assicurarsi che sia visibile una pagina Web dal portale di esempio.
5. In Internet Explorer selezionare **Strumenti** > **Opzioni Internet** e fare clic sulla scheda **Sicurezza**.
6. Fare clic su **Area Intranet locale** > **Siti** > **Avanzate** e aggiungere il sito Web all'area.
7. Chiudere le finestre di dialogo **Opzioni Internet** .
8. Nella scheda di sinistra, fare clic su **Attiva**. Selezionare il **ruolo PAM** e quindi fare clic su **Attiva**.

> [!Note]
> In questo ambiente è possibile anche imparare a sviluppare applicazioni che usano l'API REST PAM, descritta in [Privileged Access Management REST API Reference](/microsoft-identity-manager/reference/privileged-access-management-rest-api-reference) (Informazioni di riferimento sull'API REST di Privileged Access Management).

## <a name="summary"></a>Riepilogo

Dopo aver completato i passaggi descritti in questa Guida, sarà stato dimostrato uno scenario di Privileged Access Management, in cui i privilegi dell'utente vengono elevati per un periodo di tempo limitato, consentendo all'utente di accedere alle risorse protette con un account privilegiato separato. Non appena la sessione di elevazione dei privilegi scade, l'account con privilegi non potrà più accedere alla risorsa protetta. La decisione in merito a quali gruppi di sicurezza rappresentano i ruoli con privilegi è coordinata dall'amministratore PAM. Dopo che i diritti di accesso sono stati migrati al sistema Privileged Access Management, l'accesso reso possibile in passato con l'account utente originale ora è invece reso possibile solo mediante l'accesso con un account con privilegi speciali e su richiesta. Di conseguenza, le appartenenze ai gruppi con privilegi elevati sono valide per un periodo di tempo limitato.

> [!div class="step-by-step"]
> [« Passaggio 6 ](step-6-transition-group-to-pam.md)
