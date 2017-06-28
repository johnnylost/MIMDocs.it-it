---
title: Rinnovo self-service di smart card di Microsoft Identity Manager senza accesso come amministratore | Documentazione Microsoft
description: Informazioni su come registrare le smart card per gli utenti che non dispongono dei diritti di accesso con privilegi di amministratore ai propri computer per l&quot;uso del Gestore di certificati.
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/23/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: bfabc562-a2f0-4cff-ac31-36927f41e102
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 54d03fbd03f6c44298139324ea2dc7d945f008bc
ms.openlocfilehash: 89e095cff66984140cdcef3617dd0ccc3d3714d8
ms.lasthandoff: 02/07/2017


---

# <a name="enroll-smart-cards-for-non-administrators"></a>Registrare smart card per utenti non amministratori
Se un utente non è un amministratore locale del proprio computer, non sarà in grado di registrare una smart card nei propri computer per impostazione predefinita. La procedura seguente consente di ovviare a questa limitazione.

## <a name="enabling-smart-card-renewal-for-non-admins-in-mim-2016-certificate-manager"></a>Abilitazione del rinnovo della smart card per utenti non amministratori in Gestore di certificati di MIM 2016

1.  **Decomprimere il file appx**

    Ottenere un certificato di firma. Seguire la procedura riportata nel blog relativo alla [firma di applicazioni Windows 8 usando un'infrastruttura PKI interna](http://blogs.technet.com/b/deploymentguys/archive/2013/06/14/signing-windows-8-applications-using-an-internal-pki.aspx). Fermarsi quando si visualizza “Firma l’applicazione”. Fornire un nome per il file pfx esportato. Eseguire l’esportazione anche in un file con estensione CER e importarlo nel client utilizzando il file con estensione CER del nuovo certificato di firma.

    Eseguire il comando seguente per decomprimere il file appx:

    `makeappx unpack /l /p <app package name>.appx /d ./appx`

    `ren <app package name>.appx <app package name>.appx.old`

    `cd appx`

2.  **Modificare il file di configurazione**

    Rinominare il file denominato `CustomDataExample.xml custom.data`. L'applicazione CM cercherà questo nome file.

    Modificare il file custom.data nel modo indicato di seguito:

    1.  Nell'elemento &lt;NonAdmin&gt; modificare il valore dell'attributo Value in "True"

    2.  Salvare il file e chiudere l’editor

    3.  Eliminare il file denominato AppxSignature.p7x

    4.  Modificare il file denominato AppxManifest.xml

    5.  Nell'elemento &lt;Identity&gt; modificare il valore dell'attributo Publisher nel soggetto del certificato di firma, ad esempio "CN=ABCD"

        L'oggetto qui deve corrispondere al soggetto nel certificato di firma che si utilizza per firmare l'app.

    6.  Salvare il file e chiudere l’editor.

3.  **Comprimere nuovamente il pacchetto dell'app e firmarlo (file appx)**

    Eseguire le operazioni seguenti per comprimere e firmare il file appx:

    `cd ..`

    `makeappx pack /l /d .\appx /p <app package name>.appx`

    s`igntool sign /f <path\>mysign.pfx /p <pfx password> /fd "sha256" <app package name>.appx`

4.  Duplicare il modello del profilo e aggiungere la chiave iniziale di amministrazione per configurare il server MIM:

    1.  Accedere al portale CM come utente con privilegi amministrativi.

    2.  Passare ad **Amministrazione** &gt; **Gestione modelli di profilo** e assicurarsi che la casella accanto al modello di profilo creato sia selezionata, quindi fare clic su Copia modello di profilo selezionato.

    3.  Digitare il nome del modello di profilo, aggiungere “nonAdmin” e fare clic su **OK**.

    4.  Quando vengono visualizzate le impostazioni generali del modello di profilo, scorrere verso il basso fino in fondo e in **Configurazione smart card** fare clic su **Modifica impostazioni**.

    5.  In **Valore iniziale chiave amministratore (esadecimale):** immettere la chiave di amministrazione predefinita: "010203040506070801020304050607080102030405060708"

    6.  Scorrere verso il basso e fare clic su **OK**.

5.  **Creare un account utente non amministratore sul computer client**

    Gli utenti non amministratori non possono creare la smart card virtuale sul TPM. Pertanto dovranno essere gli amministratori a crearla.

6.  **Creare una smart card virtuale tramite TpmVscMgr**

    Eseguire le operazioni seguenti (sempre come amministratore) per creare una smart card virtuale in un computer. Tale operazione può essere eseguita tramite Intune, SCCM o criteri di gruppo.

    `TpmVscMgr create /name MyVSC /pin default /adminkey default /generate`

7.  **Installare l'app CM nell'account utente non amministratore**

8.  **Avviare l'app CM e la registrazione di una smart card virtuale**
