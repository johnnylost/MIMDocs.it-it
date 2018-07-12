---
title: Usare Azure MFA per l'attivazione di PAM | Documentazione Microsoft
description: Configurare Azure MFA come secondo livello di sicurezza quando gli utenti attivano ruoli in Privileged Access Management.
keywords: ''
author: billmath
ms.author: billmath
ms.reviewer: fimguy
manager: mtillman
ms.date: 07/06/2018
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 5134a112-f73f-41d0-a5a5-a89f285e1f73
ms.openlocfilehash: ad47de279dd18239ff55d89c1b717ccafe16374f
ms.sourcegitcommit: 0b6cb02d1d6e0d821b00c17090622ba354252188
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/07/2018
ms.locfileid: "37895504"
---
# <a name="using-azure-mfa-for-activation"></a>Uso di Azure MFA per l'attivazione
> [!IMPORTANT]
> In seguito all'annuncio della deprecazione di Azure Multi-Factor Authentication Software Development Kit. Azure MFA SDK sarà supportato per i clienti esistenti fino alla data di ritiro, ovvero 14 novembre 2018. I nuovi clienti e i clienti attuali non potranno più scaricare l'SDK tramite il portale di Azure classico. Per scaricarlo sarà necessario rivolgersi al supporto tecnico clienti di Azure per ricevere il pacchetto di credenziali del servizio MFA generato. <br> Il team di sviluppo Microsoft sta lavorando a modifiche per MFA tramite l'integrazione con MFA Server SDK.  Queste modifiche verranno incluse in un prossimo hotfix. Vedere la [cronologia delle versioni](/reference/version-history.md) per gli annunci. 


Quando si configura un ruolo PAM, è possibile scegliere di autorizzare gli utenti che richiedono l'attivazione del ruolo. Le opzioni implementate dall'attività di autorizzazione PAM sono:

- Approvazione del proprietario del ruolo
- [Azure Multi-Factor Authentication (MFA)](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication)

Se nessuna delle due opzioni è abilitata, gli utenti candidati vengono attivati automaticamente per il proprio ruolo.

Microsoft Azure Multi-Factor Authentication (MFA) è un servizio di autenticazione che richiede agli utenti di verificare i tentativi di accesso mediante un'app per dispositivi mobili, una telefonata o un SMS. È disponibile per l'uso con Microsoft Azure Active Directory e come servizio per le applicazioni aziendali cloud e locali. Per lo scenario PAM, Microsoft Azure Multi-Factor Authentication fornisce un meccanismo di autenticazione aggiuntivo. Microsoft Azure Multi-Factor Authentication può essere usato per l'autorizzazione, indipendentemente dalla modalità di autenticazione dell'utente al dominio PRIV di Windows.

## <a name="prerequisites"></a>Prerequisiti

Per usare Azure MFA con MIM, è necessario:

- Accesso a Internet da ogni servizio MIM fornendo PAM, per contattare il servizio Azure MFA
- Sottoscrizione di Azure
- Licenze di Azure Active Directory Premium per gli utenti candidati o un metodo alternativo di licenze di Azure MFA
- Numeri di telefono di tutti gli utenti candidati

## <a name="creating-an-azure-mfa-provider"></a>Creazione di un provider di Azure MFA

In questa sezione viene impostato il provider Azure MFA in Microsoft Azure Active Directory.  Se si usa già un provider Azure MFA autonomo o configurato con Azure Active Directory Premium, passare alla sezione successiva.

1.  Aprire un Web browser e connettersi al [portale di Azure classico](https://manage.windowsazure.com) come amministratore della sottoscrizione di Azure.

2.  Nell'angolo inferiore sinistro, fare clic su **Nuovo**.

3.  Fare clic su **Servizi app > Active Directory > Provider di Autenticazione a più fattori > Creazione rapida**.

4.  Nel campo **Nome** immettere **PAM**, quindi nel campo Modello di utilizzo selezionare Per utente abilitato. Se si dispone già di una directory di Azure AD, selezionarla. Infine, fare clic su **Crea**.

## <a name="downloading-the-azure-mfa-service-credentials"></a>Download delle credenziali del servizio Azure MFA

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

## <a name="configuring-the-mim-service-for-azure-mfa"></a>Configurazione del servizio MIM per Azure MFA

1.  Nel computer in cui è installato il servizio MIM, accedere come amministratore o con l'account utente usato per installare MIM.

2.  Creare una nuova cartella nella directory in cui il servizio MIM è stato installato, ad esempio ```C:\Program Files\Microsoft Forefront Identity Manager\2010\Service\MfaCerts```.

3.  Usando Esplora risorse, passare alla cartella ```pf\certs``` del file con estensione zip scaricato nella sezione precedente. Copiare il file ```cert\_key.p12``` nella nuova directory.

4.  Usando Esplora risorse, passare alla cartella ```pf``` del file con estensione zip e aprire il file ```pf\_auth.cs``` in un editor di testo, ad esempio Wordpad.

5. Trovare i tre parametri seguenti: ```LICENSE\_KEY```, ```GROUP\_KEY``` e ```CERT\_PASSWORD```.

![Copia dei valori dal file pf\_auth.cs - Screenshot](media/PAM-Azure-MFA-Activation-Image-2.png)

6. Tramite il Blocco note, aprire **MfaSettings.xml** in ```C:\Program Files\Microsoft Forefront Identity Manager\2010\Service```.

7. Copiare i valori dei parametri LICENSE\_KEY, GROUP\_KEY e CERT\_PASSWORD nel file pf\_auth.cs nei rispettivi elementi xml nel file MfaSettings.xml.

8. Nell'elemento XML **<CertFilePath>** specificare il nome del percorso completo del file cert\_key.p12 estratto in precedenza.

9. Nell'elemento **<username>** immettere un nome utente qualsiasi.

10. Nell'elemento **<DefaultCountryCode>** immettere il codice paese per la connessione degli utenti, ad esempio 1 per Stati Uniti e Canada. Questo valore viene usato in caso di utenti registrati con numeri di telefono senza codice paese. Se il numero di telefono di un utente dispone di un codice paese internazionale diverso da quello configurato per l'organizzazione, il codice paese deve essere incluso nel numero di telefono che verrà registrato.

11. Salvare e sovrascrivere il file **MfaSettings.xml** nella cartella del servizio MIM ```C:\Program Files\Microsoft Forefront Identity Manager\2010\\Service```.

> [!NOTE]
> Al termine del processo, verificare che il file **MfaSettings.xml**, o le relative copie, o il file con estensione zip non sia leggibile pubblicamente.

## <a name="configure-pam-users-for-azure-mfa"></a>Configurare gli utenti PAM per Azure MFA

Affinché un utente possa attivare un ruolo che richiede Azure MFA, il numero di telefono dell'utente deve essere archiviato in MIM. Per l'impostazione di questo attributo sono disponibili due opzioni.

Innanzitutto, il comando `New-PAMUser` copia un attributo numero di telefono dalla voce della directory dell'utente nel dominio CORP nel database del servizio MIM. Si noti che questa operazione viene eseguita una sola volta.

Successivamente, il comando `Set-PAMUser` aggiorna l'attributo del numero di telefono nel database del servizio MIM. Il seguente esempio sostituisce un numero di telefono dell'utente PAM esistente nel servizio MIM. La voce di directory rimane invariata.

```PowerShell
Set-PAMUser (Get-PAMUser -SourceDisplayName Jen) -SourcePhoneNumber 12135551212
```

## <a name="configure-pam-roles-for-azure-mfa"></a>Configurare i ruoli PAM per Azure MFA

Quando i numeri di telefono di tutti gli utenti candidati per un ruolo PAM sono archiviati nel database del servizio MIM, il ruolo può essere configurato per Azure MFA. Questa operazione viene eseguita usando il comando `New-PAMRole` o `Set-PAMRole`. Ad esempio:

```PowerShell
Set-PAMRole (Get-PAMRole -DisplayName "R") -MFAEnabled 1
```

Azure MFA può essere disabilitato per un ruolo specificando il parametro "-MFAEnabled 0" per il comando `Set-PAMRole`.

## <a name="troubleshooting"></a>Risoluzione dei problemi

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

## <a name="next-steps"></a>Passaggi successivi

- [Che cos'è Azure Multi-Factor Authentication?](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication)
- [Crea subito il tuo account Azure gratuito](https://azure.microsoft.com/free/)