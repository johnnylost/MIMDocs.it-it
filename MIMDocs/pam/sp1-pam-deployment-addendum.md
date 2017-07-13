---
title: Appendice
description: "Questa è l'Appendice ai documenti riguardanti la distribuzione con script di PAM. Viene illustrata la configurazione di domini PRIV e CORP, nonché l'impostazione di un client per eseguire la convalida e le informazioni su come richiedere assistenza."
keywords: 
author: barclayn
ms.author: barclayn
manager: MBaldwin
ms.date: 01/10/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: 
ms.suite: ems
ms.openlocfilehash: f69fe68dc63323c0945a4902e34ea8153f938c02
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/13/2017
---
# Appendice agli script di distribuzione di PAM:
<a id="pam-deployment-scripts-addendum" class="xliff"></a>

## Appendice 1 - Configurazione del dominio PRIV
<a id="addendum-1-setting-up-the-priv-domain" class="xliff"></a>

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
<a id="addendum-2-setting-up-the-corp-domain" class="xliff"></a>

Se si è appena iniziato a usare PAM e si vuole configurare un ambiente di test, lo script consente inoltre la configurazione di un dominio CORP. Dopo aver decompresso il file compresso nella cartella $env:SYSTEMDRIVE\PAM, modificare il file PAMDeploymentConfig.xml aggiungendo i dettagli della foresta CORP. Aggiornare i parametri DNSName e NetbiosName, il nome del controller di dominio il database, il percorso del log e il percorso Sysvol. Il livello di funzionalità deve essere almeno Windows Server 2012 R2.

1. Accedere al controller di dominio CORP come amministratore
2. Eseguire PowerShell come amministratore
3. cd $env:SYSTEMDRIVE\PAM
4. import-module .\PAMDeployment.ps1
5. Selezionare l'opzione di menu 10 (installazione della foresta CORP)

Il controller di dominio verrà riavviato automaticamente dopo il completamento

## Appendice 3 - Configurazione di un client CORP per eseguire la convalida
<a id="addendum-3-setting-up-a-corp-client-to-do-the-validation" class="xliff"></a>

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
<a id="addendum-4-if-something-goes-wrong" class="xliff"></a>

Tutti i log degli script vengono salvati in %AppData%\MIMPAMInstall. Comprimere la cartella in un file con estensione zip e inviarla tramite posta elettronica all'indirizzo [mim2016@microsoft.com](mailto:mim2016@microsoft.com) con i dettagli dell'operazione e l'errore.
