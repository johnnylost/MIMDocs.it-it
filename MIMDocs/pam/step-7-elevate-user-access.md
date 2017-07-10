---
title: 'Distribuire PAM, passaggio 7: Accesso utente | Documentazione Microsoft'
description: Nel passaggio finale concedere un accesso temporaneo a un utente con privilegi per dimostrare la riuscita della distribuzione di Privileged Access Management.
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/15/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 5325fce2-ae35-45b0-9c1a-ad8b592fcd07
ms.reviewer: mwahl
ms.suite: ems
ms.translationtype: Human Translation
ms.sourcegitcommit: bfc73723bdd3a49529522f78ac056939bb8025a3
ms.openlocfilehash: 89d9b38177b91f64e746fea583684abcecc9d7ff
ms.contentlocale: it-it
ms.lasthandoff: 07/10/2017


---

<a id="step-7--elevate-a-users-access" class="xliff"></a>
# Passaggio 7: elevare l'accesso dell'utente

>[!div class="step-by-step"]
[« Passaggio 6 ](step-6-transition-group-to-pam.md)


Questo passaggio illustra come un utente possa richiedere l'accesso a un ruolo tramite MIM.

<a id="verify-that-jen-cannot-access-the-privileged-resource" class="xliff"></a>
## Verificare che Jen non possa accedere alla risorsa privilegiata
Senza privilegi elevati, Jen non può accedere alla risorsa privilegiata nella foresta CORP.

1. Disconnettersi da CORPWKSTN per rimuovere eventuali connessioni aperte memorizzate nella cache.
2. Accedere a CORPWKSTN come *CONTOSO\Jen* e passare alla visualizzazione **Desktop**.
3. Aprire un prompt dei comandi DOS.
4. Digitare il comando `dir \\corpwkstn\corpfs`. Viene visualizzato il messaggio di errore **Accesso negato**.
5. Lasciare la finestra del prompt dei comandi aperta.

<a id="request-privileged-access-from-mim" class="xliff"></a>
## Richiedere l'accesso con privilegi da MIM
1. In CORPWKSTN, ancora come CONTOSO\Jen, digitare il comando seguente.

    ```
    runas /user:Priv.Jen@priv.contoso.local powershell
    ```

2. Quando richiesto, digitare la password per l'account PRIV.Jen. Verrà visualizzata una nuova finestra del prompt dei comandi.
3. Quando viene visualizzata la finestra di PowerShell digitare i comandi seguenti.

    > [!NOTE]
    > Dopo aver eseguito questi comandi, tutti i passaggi seguenti hanno una scadenza.

    ```
    Import-module MIMPAM
    $r = Get-PAMRoleForRequest | ? { $_.DisplayName –eq "CorpAdmins" }
    New-PAMRequest –role $r
    klist purge
    ```

4. Dopo aver completato l'operazione chiudere la finestra di PowerShell.
5. Nella finestra dei comandi DOS, digitare il comando seguente:

    ```
    runas /user:Priv.Jen@priv.contoso.local powershell
    ```

6. Digitare la password per l'account PRIV.Jen. Verrà visualizzata una nuova finestra del prompt dei comandi.

<a id="validate-the-elevated-access" class="xliff"></a>
## Convalidare l'accesso con privilegi elevati.
Nella finestra appena aperta, digitare il comando seguente:

```
whoami /groups
dir \\corpwkstn\corpfs
```

Se il comando dir ha esito negativo con il messaggio di errore **Accesso negato**, ricontrollare la relazione di trust.

<a id="activate-the-privileged-role" class="xliff"></a>
## Attivare il ruolo con privilegi
Attivare richiedendo l'accesso con privilegi elevati tramite il portale di esempio PAM.

1. In CORPWKSTN assicurarsi di essere connessi come CORP\Jen.
2. In una finestra dei comandi DOS, digitare il comando seguente.

    ```
    runas /user:Priv.Jen@priv.contoso.local "c:\program files\Internet Explorer\iexplore.exe"
    ```

3. Quando richiesto, digitare la password per l'account PRIV.Jen. Verrà visualizzata una nuova finestra del browser Web.
4. Passare a http://pamsrv.priv.contoso.local:8090 e assicurarsi che una pagina Web dal portale di esempio sia visibile.
5. In Internet Explorer selezionare **Strumenti** > **Opzioni Internet** e fare clic sulla scheda **Sicurezza**.
6. Fare clic su **Area Intranet locale** > **Siti** > **Avanzate** e aggiungere il sito Web all'area.
7. Chiudere le finestre di dialogo **Opzioni Internet** .
8. Nella scheda di sinistra, fare clic su **Attiva**. Selezionare il **ruolo PAM** e quindi fare clic su **Attiva**.

> [!Note]
> In questo ambiente è possibile anche imparare a sviluppare applicazioni che usano l'API REST PAM, descritta in [Privileged Access Management REST API Reference](/microsoft-identity-manager/reference/privileged-access-management-rest-api-reference) (Informazioni di riferimento sull'API REST di Privileged Access Management).

<a id="summary" class="xliff"></a>
## Riepilogo
Dopo aver completato i passaggi descritti in questa Guida, sarà stato dimostrato uno scenario di Privileged Access Management, in cui i privilegi dell'utente vengono elevati per un periodo di tempo limitato, consentendo all'utente di accedere alle risorse protette con un account privilegiato separato. Non appena la sessione di elevazione dei privilegi scade, l'account con privilegi non potrà più accedere alla risorsa protetta. La decisione in merito a quali gruppi di sicurezza rappresentano i ruoli con privilegi è coordinata dall'amministratore PAM. Dopo che i diritti di accesso sono stati migrati al sistema Privileged Access Management, l'accesso reso possibile in passato con l'account utente originale ora è invece reso possibile solo mediante l'accesso con un account con privilegi speciali e su richiesta. Di conseguenza, le appartenenze ai gruppi con privilegi elevati sono valide per un periodo di tempo limitato.

>[!div class="step-by-step"]
[« Passaggio 6 ](step-6-transition-group-to-pam.md)

