---
title: Usare Azure MFA per l'attivazione di PAM | Documentazione Microsoft
description: Configurare Azure MFA come secondo livello di sicurezza quando gli utenti attivano ruoli in Privileged Access Management.
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/15/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 5134a112-f73f-41d0-a5a5-a89f285e1f73
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: b937b30da2dff9bbfeabf7dceb43fcaca99a1b63
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/13/2017
---
# Uso di Azure MFA per l'attivazione
<a id="using-azure-mfa-for-activation" class="xliff"></a>
Quando si configura un ruolo PAM, è possibile scegliere di autorizzare gli utenti che richiedono l'attivazione del ruolo. Le opzioni implementate dall'attività di autorizzazione PAM sono:

- Approvazione del proprietario del ruolo
- Azure Multi-Factor Authentication (MFA)

Se nessuna delle due opzioni è abilitata, gli utenti candidati vengono attivati automaticamente per il proprio ruolo.

Microsoft Azure Multi-Factor Authentication (MFA) è un servizio di autenticazione che richiede agli utenti di verificare i tentativi di accesso mediante un'app per dispositivi mobili, una telefonata o un SMS. È disponibile per l'uso con Microsoft Azure Active Directory e come servizio per le applicazioni aziendali cloud e locali. Per lo scenario PAM, Azure MFA fornisce un meccanismo di autenticazione aggiuntivo che può essere usato al momento dell'autorizzazione, indipendentemente dalla modalità con cui un utente candidato si è precedentemente autenticato al dominio PRIV di Windows.

## Prerequisiti
<a id="prerequisites" class="xliff"></a>

Per usare Azure MFA con MIM, è necessario:

- Accesso a Internet da ogni servizio MIM fornendo PAM, per contattare il servizio Azure MFA
- Sottoscrizione di Azure
- Licenze di Azure Active Directory Premium per gli utenti candidati o un metodo alternativo di licenze di Azure MFA
- Numeri di telefono di tutti gli utenti candidati

## Creazione di un provider di Azure MFA
<a id="creating-an-azure-mfa-provider" class="xliff"></a>

In questa sezione si imposterà il provider Azure MFA in Microsoft Azure Active Directory.  Se si usa già un provider Azure MFA autonomo o configurato con Azure Active Directory Premium, passare alla sezione successiva.

1.  Aprire un Web browser e connettersi al [portale di Azure classico](https://manage.windowsazure.com) come amministratore della sottoscrizione di Azure.

2.  Nell'angolo inferiore sinistro, fare clic su **Nuovo**.

3.  Fare clic su **Servizi app > Active Directory > Provider di Autenticazione a più fattori > Creazione rapida**.

4.  Nel campo **Nome** immettere **PAM**, quindi nel campo Modello di utilizzo selezionare Per utente abilitato. Se si dispone già di una directory di Azure AD, selezionarla. Infine, fare clic su **Crea**.

## Download delle credenziali del servizio Azure MFA
<a id="downloading-the-azure-mfa-service-credentials" class="xliff"></a>

Successivamente, verrà generato un file che include il materiale di autenticazione di PAM per contattare Azure MFA.

1. Aprire un Web browser e connettersi al [portale di Azure classico](https://manage.windowsazure.com) come amministratore della sottoscrizione di Azure.

2.  Fare clic su **Active Directory** nel menu del portale di Azure, quindi scegliere la scheda **Provider di Autenticazione a più fattori** .

3.  Fare clic sul provider Azure MFA che verrà usato per PAM e quindi su **Gestisci**.

4.  Nella nuova finestra, nel pannello di sinistra, sotto **Configura**fare clic su **Impostazioni**.

5.  Nella finestra **Multi-Factor Authentication di Azure** visualizzata fare clic su **SDK** in **Download**.

6.  Nella colonna ZIP fare clic sul collegamento **Download** relativo al file con il linguaggio **ASP.NET 2.0 C\#** per l'SDK.

![Download di Multi-Factor Authentication SDK - Screenshot](media/PAM-Azure-MFA-Activation-Image-1.png)

7.  Copiare il file ZIP risultante in ogni sistema in cui è installato il servizio MIM. 

>[!NOTE]
> Il file ZIP contiene il materiale per le chiavi usato per l'autenticazione nel servizio Azure MFA.

## Configurazione del servizio MIM per Azure MFA
<a id="configuring-the-mim-service-for-azure-mfa" class="xliff"></a>

1.  Nel computer in cui è installato il servizio MIM, accedere come amministratore o con l'account utente usato per installare MIM.

2.  Creare una nuova cartella nella directory in cui il servizio MIM è stato installato, ad esempio `C:\\Program Files\\Microsoft Forefront Identity Manager\\2010\\Service\\MfaCerts`.

3.  Usando Esplora risorse, passare alla cartella **pf\\certs** del file con estensione zip scaricato nella sezione precedente e copiare il file **cert\_key.p12** nella nuova directory.

4.  Usando Esplora risorse, passare alla cartella **pf** del file con estensione zip e aprire il file **pf\_auth.cs** in un editor di testo, ad esempio Wordpad.

5.  Trovare questi tre parametri: **LICENSE\_KEY**, **GROUP\_KEY** e **CERT\_PASSWORD**.

![Copia dei valori dal file pf\_auth.cs - Screenshot](media/PAM-Azure-MFA-Activation-Image-2.png)

6.  Tramite il Blocco note, aprire **MfaSettings.xml** in `C:\\Program Files\\Microsoft Forefront Identity Manager\\2010\\Service`.

7.  Copiare i valori dei parametri LICENSE\_KEY, GROUP\_KEY e CERT\_PASSWORD nel file pf\_auth.cs nei rispettivi elementi xml nel file MfaSettings.xml.

8.  Nell'elemento XML **<CertFilePath>** specificare il nome del percorso completo del file cert\_key.p12 estratto in precedenza.

9.  Nell'elemento **<username>** immettere un nome utente qualsiasi.

10.  Nell'elemento **<DefaultCountryCode>** immettere il codice paese per la connessione degli utenti, ad esempio 1 per Stati Uniti e Canada. Questo valore viene usato in caso di utenti registrati con numeri di telefono senza codice paese. Se il numero di telefono di un utente dispone di un codice paese internazionale diverso da quello configurato per l'organizzazione, il codice paese deve essere incluso nel numero di telefono che verrà registrato.

11.  Salvare e sovrascrivere il file **MfaSettings.xml** nella cartella del servizio MIM `C:\\Program Files\\Microsoft Forefront Identity Manager\\2010\\Service`. 

> [!NOTE]
> Al termine del processo, verificare che il file **MfaSettings.xml**, o le relative copie, o il file con estensione zip non sia leggibile pubblicamente.

## Configurare gli utenti PAM per Azure MFA
<a id="configure-pam-users-for-azure-mfa" class="xliff"></a>

Affinché un utente possa attivare un ruolo che richiede Azure MFA, il numero di telefono dell'utente deve essere archiviato in MIM. Per l'impostazione di questo attributo sono disponibili due opzioni.

Innanzitutto, il comando `New-PAMUser` copia un attributo numero di telefono dalla voce della directory dell'utente nel dominio CORP nel database del servizio MIM. Si noti che questa operazione viene eseguita una sola volta.

Successivamente, il comando `Set-PAMUser` aggiorna l'attributo del numero di telefono nel database del servizio MIM. Il seguente esempio sostituisce un numero di telefono dell'utente PAM esistente nel servizio MIM. La voce di directory rimane invariata.

```
Set-PAMUser (Get-PAMUser -SourceDisplayName Jen) -SourcePhoneNumber 12135551212
```


## Configurare i ruoli PAM per Azure MFA
<a id="configure-pam-roles-for-azure-mfa" class="xliff"></a>

Quando i numeri di telefono di tutti gli utenti candidati per un ruolo PAM sono archiviati nel database del servizio MIM, il ruolo può essere configurato per Azure MFA. Questa operazione viene eseguita usando il comando `New-PAMRole` o `Set-PAMRole`. Ad esempio:

```
Set-PAMRole (Get-PAMRole -DisplayName "R") -MFAEnabled 1
```

Azure MFA può essere disabilitato per un ruolo specificando il parametro "-MFAEnabled 0" per il comando `Set-PAMRole`.

## Risoluzione dei problemi
<a id="troubleshooting" class="xliff"></a>

Nel registro eventi Privileged Access Management sono disponibili i seguenti eventi:

| ID  | Gravità | Generato da | Descrizione |
|-----|----------|--------------|-------------|
| 101 | Errore       | Servizio MIM            | L'utente non ha completato Azure MFA (ad esempio, non ha risposto al telefono) |
| 103 | Informazioni | Servizio MIM            | L'utente ha completato Azure MFA durante l'attivazione                       |
| 825 | Avviso     | Servizio di monitoraggio PAM | Il numero di telefono è stato modificato                                |

Per trovare altre informazioni sulle chiamate telefoniche con esito negativo (evento 101), è inoltre possibile visualizzare o scaricare un report da Azure MFA.

1.  Aprire un Web browser e connettersi al [portale di Azure classico](https://manage.windowsazure.com) come amministratore globale di Azure AD.

2.  Fare clic su **Active Directory** nel menu del portale di Azure e quindi scegliere la scheda **Provider di Autenticazione a più fattori**.

3.  Fare clic sul provider Azure MFA che verrà usato per PAM e quindi fare clic su **Gestisci**.

4.  Nella nuova finestra fare clic su **Utilizzo**e quindi su **Dettagli utente**.

5.  Selezionare l'intervallo di tempo e la casella di controllo relativa al **nome** nella colonna report aggiuntiva. Fare clic su **Esporta in CSV**.

6.  Quando il report è stato generato, è possibile visualizzarlo nel portale oppure se il report di MFA è dettagliato, è possibile scaricarlo in un file con estensione csv. I valori **SDK** nella colonna **TIPO DI AUTENTICAZIONE** indicano le righe rilevanti come richieste di attivazione PAM: ossia eventi provenienti da MIM o un altro software locale. Il campo **NOME UTENTE** è il GUID dell'oggetto utente nel database del servizio MIM. Se una chiamata ha esito negativo, il valore nella colonna **AUTENTICAZIONE** sarà **No** e il valore della colonna **RISULTATO CHIAMATA** conterrà i dettagli della causa dell'errore.
