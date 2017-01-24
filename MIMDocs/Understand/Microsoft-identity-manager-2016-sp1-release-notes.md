---
title: Microsoft Identity Manager 2016 Service Pack 1 | Documentazione Microsoft
description: "Comprendere il funzionamento di MIM 2016 per creare un&quot;esperienza di gestione delle identità più pratica e sicura nel cloud e in locale."
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 01/10/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: ccdd8a9f-02da-440a-81a8-354800dcd2a8
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: f0947f186b5206d06a67140706ada33a5bc0e016
ms.openlocfilehash: 4f293a349916ae1a55d8551ef949758cab851b74


---
# <a name="whats-new-for-microsoft-identity-manager-2016-service-pack-1"></a>Novità di Microsoft Identity Manager 2016 Service Pack 1 #

Come parte del ciclo di rilascio regolare per la manutenzione e l'aggiornamento di Microsoft Identity Manager, Microsoft è lieta di annunciare il lancio di [Microsoft Identity Manager (MIM) 2016 Service Pack 1 (SP1)](https://msdn.microsoft.com/subscriptions/downloads/?fileid=70212#searchTerm=&Languages=en&PageSize=10&PageIndex=0&FileId=70212). Questo documento descrive gli aggiornamenti, i miglioramenti, le funzionalità e le modifiche incluse in questa versione.

Se si verificano problemi durante una distribuzione di produzione di MIM SP1, contattare il supporto tecnico Microsoft.

Il feedback degli utenti è fondamentale. Per lasciare commenti e suggerimenti o sottoporre problemi al team del prodotto, scrivere un messaggio di posta elettronica all'indirizzo [mim2016@microsoft.com.](mailto:mim2016@microsoft.com)



## <a name="updates-in-this-service-pack"></a>Aggiornamenti inclusi in questo Service Pack #

### <a name="mim"></a>Microsoft Identity Manager (MIM)

- **Compatibilità tra browser del portale MIM per utenti finali self-service:** in questo Service Pack viene introdotto il supporto per la maggior parte dei browser principali. Gli utenti possono ora accedere e interagire con il portale MIM per la gestione self-service di profili e gruppi da Edge, Chrome e Safari.

- **Supporto del servizio MIM per Exchange Online:** il servizio MIM ha supportato a lungo l'invio e la ricezione di messaggi di posta elettronica per notifiche e approvazioni. Prima di MIM SP1 il supporto era limitato a Exchange Server o SMTP. Con il Service Pack 1, il servizio MIM può inviare e ricevere richieste e notifiche di posta elettronica usando un account di Office 365 Exchange Online.

- **Convalida del formato del file di immagini al momento del caricamento:** MIM è ora in grado di convalidare il formato del file delle immagini quando viene caricato nel portale.

### <a name="privileged-access-managementpam"></a>Privileged Access Management (PAM)

- **Supporto della foresta "PRIV" (bastion) di PAM per il livello di funzionalità di Windows Server 2016:** il servizio PAM di MIM può essere configurato in un ambiente con controller di dominio in esecuzione al livello di funzionalità della foresta Active Directory Domain Services di Windows Server 2016. Dopo la configurazione, un ticket Kerberos dell'utente verrà limitato per il tempo rimanente della propria attivazione del ruolo.

    >[!Note]
    Se si sceglie di mantenere il livello di funzionalità della foresta di Windows Server 2012 R2 nel dominio CORP, è consigliabile installare [KB 2919442](https://support.microsoft.com/en-us/kb/2919442) e [KB 2919355](https://support.microsoft.com/en-us/kb/2919355) sul controller di dominio CORP.

- **Elevazione degli account con privilegi in gruppi esclusivi della foresta "PRIV" (bastion):** gli amministratori possono ora informare il servizio MIM della presenza di gruppi e utenti esclusivi della foresta "PRIV". In questo modo, si consente l'inclusione di gruppi e utenti nei ruoli PAM.  I gruppi e gli utenti possono essere attivati per un ruolo e può essere loro assegnata l'appartenenza ai gruppi nella foresta "PRIV".

- **Script di distribuzione PAM:** gli script di distribuzione PAM consentono agli amministratori di semplificare l'installazione dell'ambiente PAM.

- **Cmdlet PAM per la configurazione di silo di criteri di autenticazione:** il Service Pack 1 introduce nuovi cmdlet per rafforzare la sicurezza della foresta bastion. Questi cmdlet creano automaticamente un silo di criteri di autenticazione, associato a un modello di criteri di autenticazione.

    >[!Note]
    Questi cmdlet vengono eseguiti automaticamente come parte degli script di distribuzione.


## <a name="platform-support"></a>Supporto della piattaforma
Informazioni aggiornate sul supporto della piattaforma sono disponibili nel documento denominato [Piattaforme supportate per MIM 2016](/microsoft-identity-manager/plan-design/microsoft-identity-manager-2016-supported-platforms).  Le nuove piattaforme supportate in questo Service Pack includono SQL Server 2016 e SharePoint 2016.

## <a name="issues-fixed-in-this-release-from-mim-2016-general-availability"></a>Problemi risolti in questa versione rispetto alla versione di disponibilità generale di MIM 2016

### <a name="pam"></a>PAM
- Il cmdlet New-PAMGroup non creava gli oggetti MIM per i gruppi di dominio locali nella foresta PRIV
- Il cmdlet New-PAMDomainConfiguration aveva esito negativo con un messaggio di errore "netdom"
- Il servizio di monitoraggio PAM registrava avvisi per i gruppi nella foresta PRIV

## <a name="how-to-upgrade-to-service-pack-1"></a>Come aggiornare il Service Pack 1

I clienti che eseguono l'aggiornamento a Microsoft Identity Manager 2016 Service Pack 1 devono seguire le indicazioni seguenti su tutti i servizi applicabili alla distribuzione.

>[!Note]
>I clienti che eseguono Forefront Identity Manager 2010 R2 SP1 o versione precedente devono prima eseguire l'aggiornamento del proprio ambiente a Microsoft Identity Manager 2016 rilasciato ad agosto 2015, quindi attenersi alla procedura seguente.

Prima di iniziare

Prima di aggiornare il servizio e il portale MIM, è necessario aggiornare il motore di sincronizzazione MIM.
È necessario eseguire il backup dei database di sincronizzazione MIM e del servizio MIM.

  1. Disinstallare il componente Microsoft Identity Manager di cui si esegue l'aggiornamento
  2. Al termine della disinstallazione, aprire la pagina iniziale di benvenuto disponibile nel supporto di installazione "FIMSplash.htm"
  3. Selezionare il componente MIM per eseguire l'aggiornamento
  4. Procedere con l'installazione seguendo le istruzioni
    * Installazione del portale e del servizio MIM: quando si sceglie Exchange Online come account di posta elettronica, immettere l'indirizzo di posta elettronica e le credenziali dell'account di Exchange Online nella schermata successiva.



<!--HONumber=Jan17_HO2-->


