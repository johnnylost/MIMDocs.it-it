---
title: Appendice
description: "Preparare il dominio CORP con identità nuove o esistenti da gestire con Privileged Identity Manager tramite gli script"
keywords: 
author: barclayn
manager: MBaldwin
ms.date: 09/27/2016
ms.topic: article
ms.prod: microsoft-identity-manager
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: 
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 689c2ef0e4e4a681a398ba7e94fb3def525937ea
ms.openlocfilehash: 482cfbbac3ea668ca4bf9d8a4a45469e61634f98


---
# Appendice:

## Appendice 1 - Configurazione del dominio PRIV

Dopo aver decompresso il file compresso nella cartella $env:SYSTEMDRIVE\PAM, modificare il file PAMDeploymentConfig.xml per specificare i dettagli della foresta PRIV. Aggiornare i parametri DNSName e NetbiosName, il nome del controller di dominio, il database, il percorso del log e il percorso Sysvol. Aggiornare inoltre DomainMode e ForestMode. Se si sta testando Windows Server Technical Preview 5, impostare DomainMode e ForestMode su WinThreshold.

1. Accedere al controller di dominio PRIV come amministratore
2. Eseguire PowerShell come amministratore
3. cd $env:SYSTEMDRIVE\PAM
4. import-module .\PAMDeployment.ps1
5. Selezionare l'opzione di menu 9 (installazione della foresta PRIV)


Il controller di dominio verrà riavviato automaticamente dopo il completamento. La password amministratore della modalità ripristino servizi directory deve corrispondere ai criteri seguenti:

  * La password ha una lunghezza minima di 15 caratteri
  * La password contiene almeno un carattere minuscolo
  * La password contiene almeno un carattere MAIUSCOLO
  * La password contiene almeno una cifra o un carattere speciale

## Appendice 2 - Configurazione del dominio CORP

Se si è appena iniziato a usare PAM e si vuole configurare un ambiente di test, lo script consente inoltre la configurazione di un dominio CORP. Dopo aver decompresso il file compresso nella cartella $env:SYSTEMDRIVE\PAM, modificare il file PAMDeploymentConfig.xml aggiungendo i dettagli della foresta CORP. Aggiornare i parametri DNSName e NetbiosName, il nome del controller di dominio il database, il percorso del log e il percorso Sysvol. Il livello di funzionalità deve essere almeno Windows Server 2012 R2.

1. Accedere al controller di dominio CORP come amministratore
2. Eseguire PowerShell come amministratore
3. cd $env:SYSTEMDRIVE\PAM
4. import-module .\PAMDeployment.ps1
5. Selezionare l'opzione di menu 10 (installazione della foresta CORP)

Il controller di dominio verrà riavviato automaticamente dopo il completamento

## Appendice 3 - Configurazione di un client CORP per eseguire la convalida

ClientBinaryLocation nel file di configurazione deve puntare al percorso in cui si trova setup.exe.
Accedere al client come amministratore locale ed eseguire i comandi seguenti in una finestra di PowerShell con privilegi elevati:

1. cd $env:SYSTEMDRIVE\PAM
2. Import-module .\PAMDeployment.ps1
3. Selezionare l'opzione di menu 7 (installazione del client PAM per MIM)


Se il computer non è aggiunto a un dominio, verranno richieste le credenziali di amministratore CORP per eseguire l'aggiunta al dominio. Il computer deve essere riavviato dopo l'aggiunta al dominio. Accedere al client nuovamente come amministratore locale ed eseguire i comandi seguenti da una finestra di PowerShell con privilegi elevati:

1. cd $env:SYSTEMDRIVE\PAM
2. Import-module .\PAMDeployment.ps1
3. Selezionare l'opzione di menu 7 (installazione del client PAM per MIM)

Procedere con il passaggio 8 riportato sopra.

## Appendice 4 - Se qualcosa non funziona

Tutti i log degli script vengono salvati in %AppData%\MIMPAMInstall. Comprimere la cartella in un file con estensione zip e inviarla tramite posta elettronica all'indirizzo [mim2016@microsoft.com](mim2016@microsoft.com) con i dettagli dell'operazione e l'errore.



<!--HONumber=Sep16_HO4-->

