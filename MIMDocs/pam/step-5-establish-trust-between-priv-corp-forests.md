---
title: 'Distribuire PAM, passaggio 5: Collegamento di foreste | Documentazione Microsoft'
description: Stabilire una relazione di trust tra le foreste PRIV e CORP in modo che gli utenti con privilegi nella foresta PRIV possano accedere alle risorse CORP.
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 11/29/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: eef248c4-b3b6-4b28-9dd0-ae2f0b552425
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: ba4b94c1f0f0879436e370a7f2f041c720bd1f60
ms.sourcegitcommit: 362475d4018e74e5a17ba574ccaec47a2caebaff
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/01/2017
---
# <a name="step-5--establish-trust-between-priv-and-corp-forests"></a>Passaggio 5: stabilire una relazione di trust tra le foreste PRIV e CORP

>[!div class="step-by-step"]
[« Passaggio 4](step-4-install-mim-components-on-pam-server.md)
[Passaggio 6 »](step-6-transition-group-to-pam.md)

Per ogni dominio CORP, ad esempio contoso.local, i controller di dominio PRIV e CONTOSO devono essere associati da un trust. In questo modo, gli utenti del dominio PRIV possono accedere alle risorse nel dominio CORP.

## <a name="connect-each-domain-controller-to-its-counterpart"></a>Connettere ogni controller di dominio alla controparte

Prima di stabilire una relazione di trust, ogni controller di dominio deve essere configurato per la risoluzione dei nomi DNS per la controparte, in base all'indirizzo IP dell'altro controller di dominio/server DNS.

1.  Se i server o i controller di dominio con il software MIM vengono distribuiti come macchine virtuali, assicurarsi che non siano presenti altri server DNS che forniscono servizi per la denominazione dei domini a tali computer.
    - Se le macchine virtuali dispongono di più interfacce di rete, incluse le interfacce di rete connesse alle reti pubbliche, può essere necessario disattivare temporaneamente tali connessioni o sostituire le impostazioni dell'interfaccia di rete di Windows. È importante assicurarsi che le macchine virtuali non usino un indirizzo server DNS fornito da DHCP.

2.  Verificare che ogni controller di dominio CORP esistente sia in grado di instradare i nomi alla foresta PRIV. In ogni controller di dominio esterno alla foresta PRIV, ad esempio CORPDC, avviare PowerShell e digitare il comando seguente:

    ```cmd
    nslookup -qt=ns priv.contoso.local.
    ```
    Verificare che l'output indichi un record server dei nomi per il dominio PRIV con l'indirizzo IP corretto.

3.  Se il controller di dominio non è in grado di instradare il dominio PRIV, usare **Gestore DNS** (in **Start** > **Strumenti applicazioni** > **DNS**) per configurare l'inoltro del nome DNS per il dominio PRIV all'indirizzo IP PRIVDC. Se si tratta di un dominio di livello superiore, ad esempio contoso.local, espandere i nodi di questo controller di dominio e il relativo dominio, ad esempio **CORPDC** > **Zone di ricerca diretta** > **contoso.local**, garantendo che una chiave denominata **priv** sia presente come tipo di server dei nomi.

    ![Struttura del file della chiave priv - Screenshot](./media/PAM_GS_DNS_Manager.png)

## <a name="establish-trust-on-pamsrv"></a>Stabilire una relazione di trust in PAMSRV

In PAMSRV stabilire una relazione di trust unidirezionale con CORPDC, in modo che i controller di dominio CORP stabiliscano una relazione di trust con la foresta PRIV.

1. Accedere a PAMSRV come amministratore del dominio PRIV (PRIV\Administrator).

2.  Avviare PowerShell.

3.  Digitare i comandi di PowerShell seguenti per ogni foresta esistente. Quando richiesto, immettere le credenziali per l'amministratore del dominio CORP (CONTOSO\Administrator).

    ```PowerShell
    $ca = get-credential
    New-PAMTrust -SourceForest "contoso.local" -Credentials $ca
    ```

4.  Digitare i comandi di PowerShell seguenti per ogni dominio nelle foreste esistenti. Quando richiesto, immettere le credenziali per l'amministratore del dominio CORP (CONTOSO\Administrator).

    ```PowerShell
    $ca = get-credential
    New-PAMDomainConfiguration -SourceDomain "contoso" -Credentials $ca
    ```

## <a name="give-forests-read-access-to-active-directory"></a>Concedere alle foreste l'accesso in lettura ad Active Directory

Per ogni foresta esistente, abilitare l'accesso in lettura ad Active Directory da parte degli amministratori PRIV e del servizio di monitoraggio.

1.  Accedere al controller di dominio della foresta CORP esistente, (CORPDC), come amministratore di dominio per il dominio di primo livello in tale foresta (Contoso\Administrator).  
2.  Avviare **Utenti e computer di Active Directory**.  
3.  Fare clic con il pulsante destro del mouse sul dominio **contoso.local** e selezionare **Delega controllo**.  
4.  Nella scheda Gruppi e utenti selezionati fare clic su **Aggiungi**.  
5.  Nella finestra Seleziona utenti, computer o gruppi fare clic su **Percorsi** e cambiare il percorso in *priv.contoso.local*.  Nel nome dell'oggetto digitare *Domain Admins* e fare clic su **Controlla nomi**. Quando viene visualizzata una finestra popup, immettere il nome utente *priv\administrator* e la relativa password.  
6.  Dopo Domain Admins, aggiungere "*; MIMMonitor*". Quando i nomi **Domain Admins** e **MIMMonitor** risultano sottolineati, fare clic su **OK** e quindi su **Avanti**.  
7.  Nell'elenco delle attività comuni selezionare **Legge tutte le informazioni utente**, quindi fare clic su **Avanti** e su **Fine**.  
8.  Chiudere Utenti e computer di Active Directory.

9.  Aprire una finestra di PowerShell.
10.  Usare `netdom` per garantire che la cronologia SID sia abilitata e che il filtro SID sia disabilitato. Digitare il comando seguente:
    ```cmd
    netdom trust contoso.local /quarantine:no /domain priv.contoso.local
    netdom trust /enablesidhistory:yes /domain priv.contoso.local
    ```
    L'output deve indicare l'**abilitazione della cronologia SID per il trust** o che la **cronologia SID è già abilitata per il trust**.

    L'output dovrebbe inoltre indicare che il **filtro dei SID non è abilitato per il trust**. Per altre informazioni, vedere [Disable SID filter quarantining](http://technet.microsoft.com/library/cc772816.aspx) (Disabilitare la quarantena per il filtro dei SID).

## <a name="start-the-monitoring-and-component-services"></a>Avviare i servizi di monitoraggio e del componente

1.  Accedere a PAMSRV come amministratore del dominio PRIV (PRIV\Administrator).

2.  Avviare PowerShell.

3.  Digitare i comandi di PowerShell seguenti.

    ```cmd
    net start "PAM Component service"
    net start "PAM Monitoring service"
    ```

Nel passaggio successivo, un gruppo verrà spostato in PAM.

>[!div class="step-by-step"]
[« Passaggio 4](step-4-install-mim-components-on-pam-server.md)
[Passaggio 6 »](step-6-transition-group-to-pam.md)
