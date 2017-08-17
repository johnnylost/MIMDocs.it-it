---
title: Personalizzazioni del portale di Microsoft Identity Manager 2016 | Microsoft Docs
description: 
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 08/01/2017
ms.topic: reference
ms.prod: identity-manager-2016
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 
ms.openlocfilehash: bebb299ece077a6ab2e0d75f3b30ad1776678303
ms.sourcegitcommit: 5ba5d916c0ca1e5aa501592af0cef714bfdc8afe
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/02/2017
---
# <a name="microsoft-identity-manager-2016-portal-customization"></a>Personalizzazioni del portale di Microsoft Identity Manager 2016


>[!Warning]
Verificare di aver cancellato la cache del browser quando si effettuano personalizzazioni di CSS.

In Microsoft Identity Manager 2016 (MIM) è possibile personalizzare elementi selezionati dei portali password, tra cui il logo del banner, le risorse stringa e i fogli di stile CSS.

In base al livello di personalizzazione è necessario eseguire alcune operazioni. Di seguito è riportato un elenco di elementi interessati dalla personalizzazione dei portali di registrazione e reimpostazione della password di MIM 2016.

-   Cartella Customizations: cartella delle personalizzazioni che MIM 2016 controllerà prima di usare le impostazioni predefinite. Per ogni portale personalizzato è necessaria una cartella Customizations. Le personalizzazioni devono essere eseguite solo in questa cartella poiché il programma di installazione non la sovrascrive durante gli aggiornamenti, le installazioni in modalità di modifica o le installazioni in modalità di ripristino.

-   Strings.resources: file basato su XML che consente di modificare le stringhe che vengono visualizzate nel portale. Questo file deve trovarsi nella cartella Customizations.

-   Style.css: foglio di stile CSS usato dai portali per la personalizzazione. Questo foglio di stile deve essere creato e modificato per cambiare il logo oppure può essere completamente sostituito dalle personalizzazioni dell'utente.

Per istruzioni dettagliate sulla personalizzazione dei portali di registrazione e reimpostazione della password, vedere la guida al lab di test relativa alla personalizzazione dei portali di registrazione e reimpostazione della password in MIM 2016.

>[! AVVISO] Affinché MIM riconosca le modifiche personalizzate, è necessario riavviare IIS eseguendo iisreset.


## <a name="creating-the-customizations-folder"></a>Creazione della cartella Customizations

All'avvio MIM cerca il file Strings.resources nella cartella Customizations prima di usare le impostazioni predefinite. È necessario creare una cartella per le personalizzazioni nella directory del portale da personalizzare, ovvero il portale di registrazione della password o il portale di reimpostazione della password. Per personalizzare entrambi i portali, è necessario creare una cartella Customizations in ognuno dei percorsi seguenti:

-   C:\\Programmi\\Microsoft Forefront Identity Manager\\2010\\Password Registration Portal

-   C:\\Programmi\\Microsoft Forefront Identity Manager\\2010\\Password Reset Portal

### <a name="to-create-the-customization-folder"></a>Per creare la cartella delle personalizzazioni

1.  Passare alla cartella C:\\Programmi\\Microsoft Forefront Identity Manager\\2010\\Password Registration Portal.

2.  Creare una cartella denominata Customizations.

3.  Tornare indietro di un livello fino alla cartella Password Reset Portal e creare la cartella Customizations.

## <a name="customizing-strings"></a>Personalizzazione delle stringhe

Molte delle stringhe nell'interfaccia utente del portale possono essere personalizzate creando un file Strings.resources e aggiungendo il file alla cartella Customizations. È necessario creare un file Strings.resources per ogni portale che si vuole personalizzare.

### <a name="to-customize-strings"></a>Per personalizzare le stringhe

1.  Copiare il codice riportato di seguito in Blocco note e salvarlo nella cartella Customizations come Strings.resources

   ```xml
   <?xml version="1.0" encoding="utf-8"?>
   <root>
     <resheader name="resmimetype">
       <value>text/microsoft-resx</value>
     </resheader>
     <resheader name="version">
       <value>2.0</value>
     </resheader>
     <resheader name="reader">
       <value>System.Resources.ResXResourceReader, System.Windows.Forms, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089</value>
    </resheader>
     <resheader name="writer">
       <value>System.Resources.ResXResourceWriter, System.Windows.Forms, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089</value>
     </resheader>

    <!-- Customizations begin here -->
     <data name=" QAGateResetTitle " xml:space="preserve">
       <value>Contoso Question and Answer Reset</value>
     </data>
     <data name="ResetPageTitle" xml:space="preserve">
       <value>Contoso Self-Service Password Reset</value>
     </data>
   </root>

   ```
2.  Sotto la sezione `<!-- Customizations begin here -->` modificare il nome dei dati in modo che corrisponda alle stringhe da personalizzare e immettere il valore per tale stringa tra i tag <value></value>. Vedere l'elenco riportato di seguito per le stringhe che possono essere personalizzate e i relativi valori predefiniti.

>[!NOTE]
Il file **Strings.resources** è indipendente dalla lingua. Per creare stringhe personalizzate specifiche di una lingua, è necessario installare il relativo Language Pack e salvare il file in formato Strings. `<language>-<culture>.resources`, ad esempio Strings.en-us.resources.

Di seguito è riportato un elenco di stringhe del portale che possono essere personalizzate.

<a name="portal-strings"></a>Stringhe del portale
--------------

| Nome                                                     | Valore predefinito                                                                                                                                                                                                                                                                                                                                         | Commento                                                                                                                                                                                                                                            |
|---------------------------|-------------------|--------------|
| AboutLinkText                                            | Informazioni su         |       |
| ButtonCancel                                             | Annulla                                                                                 |     |
| ButtonFinish                                             | Fine    |    |
| ButtonNext                                               | Avanti    |    |
| ButtonOk                                                 | OK     |   |
| CancelFinishedMessage                                    | La sessione utente non è più attiva. Chiudere la finestra oppure riavviare facendo clic sul collegamento seguente.         |      |
| CancelFinishedTitle                                      | Sessione terminata                                                              |      |
| ErrorDescription_3000                                    | Errore: Riprovare. Se il problema persiste, contattare il supporto tecnico o l'amministratore di sistema. (Errore 3000)       |          |
| ErrorDescription_3001                                    | Assicurarsi di immettere il nome correttamente. Se non è ancora possibile reimpostare la password, contattare      |           |
|                                                          | il supporto tecnico per assistenza. (Errore 3001)   |                   |
| ErrorDescription_3002                                    | Sessione terminata. Tornare alla home page e ricominciare. (Errore 3002)                                     |                |
| ErrorDescription_3003                                    | L'account utente corrente non è riconosciuto da Forefront Identity Manager. Contattare il supporto tecnico o l'amministratore di sistema. (Errore 3003)           |               |
| ErrorDescription_3004                                    | Non si dispone delle autorizzazioni necessarie per eseguire la registrazione per la reimpostazione della password. Contattare il supporto tecnico o l'amministratore di sistema. (Errore 3004)     |              |
| ErrorDescription_3005                                    | Una o più risposte fornite non corrispondono alle risposte fornite durante la registrazione della password. Per reimpostare la password, è necessario che le risposte fornite ora corrispondano alle risposte fornite durante la registrazione. È possibile ricominciare dalla home page oppure contattare il supporto tecnico o l'amministratore di sistema. (Errore 3005) |           |
| ErrorDescription_3006                                    | La password immessa non è corretta. È necessario immettere la password corretta per eseguire la registrazione per la reimpostazione della password. (Errore 3006)            |              |
| ErrorDescription_3007                                    | Non è al momento consentito reimpostare la password. Riprovare più tardi oppure contattare il supporto tecnico o l'amministratore di sistema per assistenza. (Errore 3007)     |         |
| ErrorDescription_3008                                    | Errore: Riprovare. Se il problema persiste, contattare il supporto tecnico o l'amministratore di sistema. (Errore 3008)        |          |
| ErrorDescription_3009                                    | L'input contiene testo in un formato non consentito. Riprovare con un input differente o contattare il supporto tecnico o l'amministratore di sistema. (Errore 3009)     |             |
| ErrorDescription_3010_Registration                       | L'esecuzione di script non è abilitata nel browser. Abilitare l'esecuzione di script e tornare alla pagina iniziale di Registrazione password o contattare il supporto tecnico per assistenza.      |               |
| ErrorDescription_3010_Reset                              | L'esecuzione di script non è abilitata nel browser. Abilitare l'esecuzione di script e tornare alla pagina iniziale di Reimpostazione password o contattare il supporto tecnico per assistenza.     |            |
| ErrorDescription_3011                                    | Il sito utilizza i cookie. Configurare il browser in modo che accetti i cookie e riprovare oppure contattare il supporto tecnico per assistenza.          |                                           |
| ErrorDescription_3012                                    | I dati inseriti non corrispondono al codice di protezione fornito. Provare a ripristinare nuovamente la password oppure, per ottenere assistenza, contattare il supporto tecnico.      |                                                                                                                                                                                                                                                    |
| ErrorDescription_3013                                    | Impossibile inviare un codice di protezione. Per ottenere assistenza, contattare il supporto tecnico.                                                                                                                                                                                                                                                                         |                                                                                                                                                                                                                                                    |
| ErrorMessageDomainUsernameFormat                         | Immettere il nome utente in formato corretto.                                                                                                                                                                                                                                                                                                           |                                                                                                                                                                                                                                                    |
| ErrorMessageDomainUsernameRequired                       | Immettere un nome utente per continuare.                                              |                         |
| ErrorMessagePasswordRequired                             | Immettere una password.              |                                                |
| ErrorMessagePasswordsDoNotMatch                          | Verificare che entrambe le password corrispondano.                                |           |
| ErrorPageDefaultHeading                                  | Errore dell'applicazione                                            |               |
| ErrorPageServerTime                                      | Ora server: {0:T}                     | {0} è l'ora in cui è stata rilevata l'eccezione. Il valore "T" fa sì che l'ora passata venga formattata come "ora estesa". Indicherà l'ora, i minuti e i secondi e probabilmente la designazione AM/PM, in base alle impostazioni cultura correnti. |
| ErrorPageTitle                                           | Forefront Identity Management - Errore password                     |   |
| ErrorTitle_3000                                          | Errore                                  |  |
| ErrorTitle_3001                                          | Accesso negato                          |  |
| ErrorTitle_3002                                          | Sessione terminata                          |  |
| ErrorTitle_3003                                          | Utente non riconosciuto                      |  |
| ErrorTitle_3004                                          | Utente non autorizzato                      |  |
| ErrorTitle_3005                                          | Risposte non corrispondenti                    |  |
| ErrorTitle_3006                                          | Password non corretta                     |  |  
| ErrorTitle_3007                                          | Accesso temporaneamente negato              |  |
| ErrorTitle_3008                                          | Errore di comunicazione                    |  |
| ErrorTitle_3009                                          | Input non consentita                       |  |
| ErrorTitle_3010                                          | Errore di configurazione del browser            |  |
| ErrorTitle_3011                                          | Errore di configurazione del browser            |  |
| ErrorTitle_3012                                          | Verifica non riuscita                    |  |
| ErrorTitle_3013                                          | Impossibile inviare un codice di protezione   |  |
| FinalizeRegistrationHeading1                             | Se è necessario reimpostare la password:        |   |
| FinalizeRegistrationSubHeading1                          | Andare al portale per la reimpostazione della password   |   |
| FinalizeRegistrationSubHeading2                          | Verificare l'identità   |   |
| FinalizeRegistrationSubHeading3                          | Scegliere la nuova password    |     |
| FinishingDescription                                     | Scegliere la nuova password        |    |
| FinishingTitle                                           | Reimpostazione della password:      |     |
| GotoPortalPrefix                                         | Vai a     |        |
| GotoPortalSuffix                                         | home page     |      |
| LabelTroubleshootingLinkText                             | Visualizza dettagli           |    |
| LoadingText                                              | Caricamento in corso     |  |
| NoScriptTagErrorMessage                                  | L'esecuzione di script non è abilitata nel browser. Abilitare l'esecuzione di script e tornare alla pagina iniziale o contattare il supporto tecnico per assistenza.      |   |
| PasswordResetOperationGeneralErrorMessage                | Errore durante il tentativo di reimpostare la password.       |   |
| PasswordResetOperationPolicyViolationErrorMessage            | La password non è conforme ai criteri per la password dell'organizzazione.             |       |
| PasswordResetOperationUserCantChangePasswordErrorMessage | Errore durante la reimpostazione della password. L'utente non può modificare la password.    |   |
| PrivacyStatement                                         | Informativa sulla privacy                                                      |    |
| RegistrationDescription                                  | Registrazione della password self-service     |    |
| RegistrationMission                                      | Se si dimentica la password, è possibile reimpostarla senza dover contattare il supporto tecnico.   |      |
| RegistrationPageTitle                                    | Forefront Identity Management - Registrazione password                                          |   |
| RegistrationSteps                                        | Fare clic su 'Avanti' per iniziare il processo di registrazione.   |      |
| RegistrationSuccessDescription                           | Registrazione completata        |   |
| RegistrationSuccessTitle                                 | Completata:   |    |
| RegistrationWelcomeTitle                                 | Registrazione password:    | |
| ResetDescription                                         | Reimpostazione della password self-service  |    |
| ResetEnterNamePrompt                                     | Immettere il nome utente di seguito     |     |
| ResetEnterPassword                                       | Immettere una nuova password:                                                  |   |
| ResetExample1                                            | contoso\\mmeyers                                                                            |      |
| ResetExample2                                            | mmeyers\@contoso.com     |      |
| ResetExamples                                            | Esempi:  |    |
| ResetPageTitle                                           | Forefront Identity Management - Reimpostazione password       |     |
| ResetReenterPassword                                     | Reimmettere la password:    | |
| ResetSuccessDescription                                  | La password è stata reimpostata    |    |
| ResetSuccessTitle                                        | Operazione riuscita:                                |    |
| ResetUseNewPassword                                      | È ora possibile utilizzare la nuova password per eseguire l'accesso.     |      |
| ResetUsernameTextFormat                                  | (verrà ripristinata la password di {0})       | {0} è il nome di accesso dell'utente             |
| ResetWelcomeTitle                                        | Reimpostazione della password:     |      |
| TroubleshootingEmailSubject                              | Dettagli dell'errore di elaborazione della richiesta FIM     |       |
| TroubleshootingLabelAttributes                           | Attributi:    |    |
| TroubleshootingLabelAttributes                          | Chiudi       |    |
| TroubleshootingLabelCopyToClipboard                      | Copia negli Appunti     |     |
| TroubleshootingLabelAttributes                        | ID di correlazione:      |          |
| TroubleshootingLabelDetails                              | Dettagli:                                                             |    |
| TroubleshootingLabelPostCopyClipboardMessage             | Le informazioni sono state copiate negli Appunti.           |    |
| TroubleshootingLabelRequestId                            | ID richiesta:                  |    |
| TroubleshootingLabelRequestId                            | Invia informazioni per posta elettronica                            |    |
| TroubleshootingLabelSource                               | Motivo:                                                                     |    |
| TroubleshootingLabelViewRequestDetails                   | Visualizza dettagli richiesta        |              |                                                    
| TroubleshootingLinkText                                  | Informazioni sulla risoluzione dei problemi          |                  | |



Di seguito è riportato un elenco di stringhe del controllo di autenticazione che possono essere personalizzate.

<a name="authentication-gate-strings"></a>Stringhe del controllo di autenticazione
---------------------------

| Nome                                          | Valore predefinito                                                                                                                                                                                                                | Commento |
|-----------|--------------|----------|
| OTPEmailRegistraionEmailTextboxLabel          | Indirizzo di posta elettronica:             |          |
| OTPEmailRegistrationEmailRequiredErrorMessage | Il campo dell'indirizzo di posta elettronica non può essere vuoto.     |          |
| OTPEmailRegistrationFooterReadOnly            | Per aggiornare l'indirizzo di posta elettronica, seguire la procedura definita dall'organizzazione o chiamare il supporto tecnico.                   |          |
| OTPEmailRegistrationFooterReadWrite           | L'indirizzo di posta elettronica viene archiviato dall'organizzazione in Forefront Identity Manager.                       |          |
| OTPEmailRegistrationGateTitle                 | Verifica dell'indirizzo di posta elettronica   |          |
| OTPEmailRegistrationHeaderReadOnly            | Quando si avvia il ripristino della password, un codice di protezione per la verifica viene inviato all'indirizzo di posta elettronica registrato per l'utente. Se l'indirizzo di posta elettronica mostrato in basso non è corretto, è necessario aggiornarlo per utilizzare il servizio di ripristino della password self-service. |          |
| OTPEmailRegistrationHeaderReadWrite           | Immettere l'indirizzo di posta elettronica in basso. Quando si avvia il ripristino della password, un codice di protezione per la verifica viene inviato a questo indirizzo.                 |          |
| OTPEmailResetGateTitle                        | Verificare l'identità: verifica di posta elettronica         |          |
| OTPEmailResetHeader                           | Immettere il codice di protezione in basso. Un codice di protezione è stato inviato all'indirizzo di posta elettronica registrato con l'organizzazione.                                                                                                             |          |
| OTPRegularExpressionErrorMessage              | Il valore specificato non corrisponde al formato previsto.                   |          |
| OTPResetOneTimePasswordRequiredErrorMessage   | Il campo del codice di protezione non può essere vuoto.        |          |
| OTPResetVerificationLabel                     | Codice di protezione:                    |          |
| OTPSmsRegistrationFooterReadOnly                      | Per aggiornare il numero del cellulare, seguire la procedura definita dall'organizzazione o chiamare il supporto tecnico.   ||
| OTPSmsRegistrationFooterReadWrite                     | Il numero del cellulare viene archiviato dall'organizzazione in Forefront Identity Manager.                                                                                                                                                     |   |
| OTPSmsRegistrationGateTitle                           | Verifica del cellulare                      |   |
| OTPSmsRegistrationHeaderReadOnly                      | Quando si avvia il ripristino della password, un codice di protezione per la verifica viene inviato al cellulare registrato per l'utente. Se il numero di telefono mostrato in basso non è corretto, è necessario aggiornarlo per utilizzare il servizio di ripristino della password self-service. |   |
| OTPSmsRegistrationHeaderReadWrite                     | Immettere il numero di cellulare in basso. Quando si avvia il ripristino della password, un codice di protezione per la verifica viene inviato a questo numero.                                                                                                     |   |
| OTPSmsRegistrationMobilePhoneRequiredErrorMessage     | Il campo del numero di cellulare non può essere vuoto.      |   |
| OTPSmsRegistrationSMSTextBoxLabel                     | Cellulare:                    |   |
| OTPSmsResetGateTitle                                  | Verificare l'identità: verifica del cellulare         |   |
| OTPSmsResetHeader                                     | Immettere il codice di protezione in basso. Un codice di protezione è stato inviato al cellulare registrato con l'organizzazione.                                                                                                                           |   |
| PasswordGateDescriptionText                           | Immettere la password corrente di seguito, quindi fare clic su 'Avanti'.        |   |
| PasswordGateErrorMessagePasswordRequired              | Immettere la password corrente.                  |   |
| PasswordGateGateTitle                                 | La password corrente               |   |
| PasswordGatePasswordLabelText                         | Password:                 |   |
| PasswordGateUsernameTextFormat                        | `<i>`(connesso come: `<b>{0}</b>)</i>`                                                          |   |
| QAGateErrorNotEnoughQuestionsAnswered                 | È necessario rispondere ad almeno {0} domande.        |   |
| QAGateIncorrectAnswer                                 | Le risposte non sono corrette.       |   |
| QAGatePrivacyNotice                                   | Le risposte fornite vengono archiviate dall'organizzazione in Forefront Identity Manager.                                                                                                                                                  |   |
| QAGateRegistrationNumberOfQuestionsExplanation_Format | È necessario rispondere ad almeno {0} domande per la registrazione.     |   |
| QAGateRegistrationOneOrMoreAnswersFailedValidation    | Una o più risposte non soddisfano i criteri.      |   |
| QAGateRegistrationThisAnswerValidationFailed          | La risposta non soddisfa i criteri.      |   |
| QAGateRegistrationTitle                               | Registra le risposte         |   |
| QAGateResetNumberOfQuestionsExplanation_Format        | È necessario rispondere a {0} delle seguenti {1} domande.   |   |
| QAGateResetTitle                                      | Verifica dell'identità: inviare le risposte                                  |   |



## <a name="customizing-the-logo-banner"></a>Personalizzazione del banner con il logo

È possibile personalizzare l'intestazione predefinita delle pagine del portale per l'organizzazione.
Per personalizzare il banner con il logo:

1.  Creare le intestazioni personalizzate e salvarle come file PNG. I file devono soddisfare i requisiti seguenti:

 - Dimensioni: 490 x 50 pixel.

 - Profondità bit: 32

2.  Copiare i file nella cartella \\Customizations di ogni portale che si vuole personalizzare.

3.  Creare un file Style.css in ogni cartella. Fare in modo che punti alla cartella Customizations e al nuovo logo. È possibile modificare il nome del logo, se applicabile (ad esempio /Customizations/contosologo.png). Il codice deve essere simile al seguente:

   **Esempio 1:**

  `.title-block{background:url(../Customizations/fimlogo.png) no-repeat scroll 0 0 transparent;}`

4.  Se si usa Internet Explorer 6.0, è necessario specificare un logo alternativo non trasparente e aggiungere il codice seguente a Style.css:

  `.ie6 .title-block{background-image:url(../Customizations/fimlogo-ie6.png);}`

  **Esempio 2:**

  `.title-block{background:url(../Customizations/contosologo.png) no-repeat scroll 0 0 transparent;}`

Se si usa Internet Explorer 6.0, è necessario specificare un logo alternativo non trasparente e aggiungere il codice seguente a Style.css:

  `.ie6 .title-block{background-image:url(../Customizations/contosologo-ie6.png);}`

## <a name="customizing-image-for-smartphones"></a>Personalizzazione dell'immagine per gli smartphone

È possibile personalizzare l'immagine per gli smartphone usando gli elementi seguenti. Per personalizzare l'immagine per uno smartphone:

1.  Creare l'immagine e salvarla come file PNG. I file devono soddisfare i requisiti seguenti:

    - Dimensioni: 190 x 50 pixel.
    - Profondità bit: 32

2.  Copiare i file nella cartella \\Customizations di ogni portale che si vuole personalizzare.

2.  Creare un file Style.css in ogni cartella. Fare in modo che punti alla cartella Customizations e al nuovo logo. È possibile modificare il nome del logo, se applicabile (ad esempio /Customizations/contosologo.png). Il codice deve essere simile al seguente:

  **Esempio 1:**

```css
@media only screen and (max-width: 480px)

   {

    .title-block

     {
       background: url("path_to_image/imagename.png") no-repeat scroll 0 0 transparent;
     }

   }
   ```

## <a name="customizing-style-sheets"></a>Personalizzazione dei fogli di stile

È possibile modificare il layout e lo stile dei portali delle password usando un foglio di stile CSS personalizzato. Per usare un foglio di stile CSS personalizzato:

1.  Creare i file CSS personalizzati e salvarli come Style.css.

2.  Copiare i file nella cartella \\Customizations di ogni portale che si vuole personalizzare.

Di seguito è riportato un esempio di base di file Style.css:

```css
body
{
  font: 15px Algerian;
  color: \#303030;
  background: white;
}

.pad
{
  padding: 30px;
  padding-top: 50px;
  background: white;
}

.backgroundWhite
{
  border: \#e9e9e9 2px solid;
} .
title-block
{
background:url(../Customizations/contosologo.png) no-repeat scroll 0 0 transparent;
}
```


>[!IMPORTANT]
Per consentire a MIM di riconoscere le modifiche personalizzate, è necessario riavviare IIS eseguendo iisreset.                                                                                                                                                                                       

Di seguito è riportato un esempio più avanzato di file Style.css. Questo file mette a disposizione di smartphone e ipad informazioni specifiche per la visualizzazione dei portali su questi dispositivi.

```css
/****************
BASE
*****************/

body {
    font-size: 14px; /*Customizeable- Body Font Size */
    background-color: #ced5ec; /*Customizeable- Backgound Color behind the product */
}

body, button, input, select, textarea {
    font-family: Segoe UI, Arial, Verdana, Sans-Serif, Helvetica; /*Customizeable- Body Font Family */
    color: #595959; /*Customizeable- Body Font Color */
}

/****************
LINKS
*****************/

a { color: #396faf; text-decoration: none; } /*Customizeable- Link Color and Underline */
a:visited { color: #396faf; text-decoration: none; } /*Customizeable- After Link is clicked color and underline */
a:hover { color: #6486ae; text-decoration: none; } /*Customizeable- Hover mouse over Link color and underline */
a:focus { outline: thin dotted; } /*Customizeable- Keyboard event to Link and Link is in focus outline*/
a:hover, a:active { outline: 0; } /*Customizeable- Hover and Active Link outline */

/****************
Typography
*****************/

hr { border-top: 1px solid #acd9ec; } /*Customizeable- Horizontal Rule Color Above the Footer */

/****************
Layout
*****************/
#wrapper {
    background: url(../images/bg-top-slice.png) repeat-x 0 0; /*Customizeable-remove this line to remove top gradient */
}

#container {
    background: url(../images/bg-bottom-slice.png) repeat-x 100% 100% transparent;  /*Customizeable-remove this line to remove bottom gradient */
}

.title-block {
    background: url("../images/fimlogo.png") no-repeat scroll 0 0 transparent;  /*Customizeable- Logo must be 600px or less in width. Logo must be 50px or less in height. */
    border-bottom: 2px solid #acd9ec;/*Customizeable- 2px border color under logo */
}

.ie6 .title-block {
    background-image: url(../images/fimlogo-ie6.png);   /*Customizeable- Can make a non-transparent image for IE6 only */
}

h2 {
    color: #578e4c; /*Customizeable- h2 page header color */
}

h3 {
    color: #999; /*Customizeable- h3 page header color */
}

input[type=text]:focus, input[type=password]:focus {
    border: #82bd3b 2px solid; /*Customizeable- Highlight color around textbox when cursor is inside */
}

.chromeButton, .chromeButton:visited {
    background-color: #333; /*Customizeable- Color of button */
    color: #fff; /*Customizeable- Color of text on the button */
    border: 1px solid #666; /*Customizeable- Border color of button */
}

.chromeButton:hover {
    background-color: #666; /*Customizeable- Hover color of button */
    border: 1px solid #999; /*Customizeable- Hover border color of button */
}

.qcol /*Style from QAgate.css */ {
    color: #7a7a7a; /*Customizeable- Font color of Q&A container */
    background-color:#e6e7e9; /*Customizeable- Background color of Q&A container */
}

/****************
Media Queries
*****************/

/* Smartphones ----------- */
@media only screen and (max-width: 480px) {
    body {
        font-size:12px; /*Customizeable- Body Font Size for devices */
    }

    .title-block {
        background: url("../images/fim-logo-portrait.png") no-repeat scroll 0 0 transparent;  /*Customizeable- Logo must be 190px (landscape) or less in width. Logo must be 50px or less in height. */
    }
    h2, h3 {
        font-size:14px; /*Customizeable- H2 and H3 Heading Size for devices */
    }
}


/* iPads (landscape) ----------- */
@media only screen and (min-device-width : 768px) and
(max-device-width : 1024px) and
(orientation : landscape)
{
}

/* iPads (portrait) ----------- */
@media only screen and (min-device-width : 768px) and
(max-device-width : 1024px) and
(orientation : portrait)
{
}
```


## <a name="common-customization-issues"></a>Problemi di personalizzazione comuni

La tabella seguente è un elenco di problemi noti che si possono verificare quando si aggiornano il servizio FIM e il portale. La tabella include anche le risoluzioni dei problemi

|Problema |Risoluzione                                                                    |
|------|------------------------------|
| È stata eseguita una personalizzazione di stringhe ma l'operazione non ha avuto effetto sull'interfaccia utente         | Le personalizzazioni delle stringhe in strings.resources richiedono sempre iisreset         |
| Dopo aver apportato una modifica a strings.resources, le modifiche apportate alle stringhe non vengono più visualizzate     | Il formato di strings.resources probabilmente non è corretto e per questo viene ignorato dal portale. Controllare il registro eventi in Registri di Windows - Registri applicazioni e servizi - Forefront Identity Manager                        |
| Dopo aver aggiunto per la prima volta Style.css, le modifiche dello stile non sono apparse nel portale            | La prima volta che si introduce un file Style.css, è necessario eseguire un iisreset     |
| I nuovi stili vengono aggiunti o modificati in Style.css ma le modifiche non appaiono nel browser      | Cancellare la cache del browser e aggiornare la pagina <br/> Controllare la sintassi CSS    |
| I contenuti della cartella CSS sono stati modificati direttamente in path_to_sspr_portal\\css\\\*.css o il logo del banner è stato modificato in path_to_sspr_portal\\images\\fimlogo.png e le modifiche sono andate perdute con l'aggiornamento | Per prima cosa questi file non devono mai essere modificati. Usare solo la cartella Customizations per specificare le personalizzazioni del logo del banner e dello stile CSS in Style.css. La cartella Customizations non viene deliberatamente sovrascritta dagli aggiornamenti principali <br/><br/>Non tentare di usare strumenti come ILSpy e Reflector per modificare le stringhe negli assembly del portale. Usare strings.resources per eseguire l'override delle stringhe predefinite. Gli assembly vengono sostituiti durante l'aggiornamento  |
| Il logo del banner non viene visualizzato nei portali / il logo FIM continua a non essere visibile     | Il nome o il percorso dell'immagine in Style.css non è valido o non è stata cancellata la cache del browser          |
| Il logo del banner viene visualizzato in modo insoddisfacente in IE6       | È necessario usare un'immagine non trasparente per IE6 e allegare uno stile speciale in style.css        |
