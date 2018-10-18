---
title: Usare il SDK del server Azure Multi-Factor Authentication per attivare scenari PAM o SSPR | Microsoft Docs
description: Configurare il SDK del server Azure Multi-Factor Authentication come secondo livello di sicurezza quando gli utenti attivano ruoli in Privileged Access Management e in Reimpostazione password self-service (SSPR, Self Service Password Reset).
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 09/02/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 94a74f1c-2192-4748-9a25-62a526295338
ms.openlocfilehash: b92a217dd86d9e4de177ebec9ecec7c76222d7b1
ms.sourcegitcommit: 7de35aaca3a21192e4696fdfd57d4dac2a7b9f90
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/16/2018
ms.locfileid: "49358279"
---
# <a name="use-azure-multi-factor-authentication-server-to-activate-pam-or-sspr"></a>Usare il server Azure Multi-Factor Authentication per attivare PAM o SSPR
Il documento seguente descrive come impostare il server Azure MFA come secondo livello di sicurezza quando gli utenti attivano ruoli in Privileged Access Management o in Reimpostazione password self-service.

> [!IMPORTANT]
> In seguito all'annuncio della deprecazione di Azure Multi-Factor Authentication Software Development Kit. Azure MFA SDK sarà supportato per i clienti esistenti fino alla data di ritiro, ovvero 14 novembre 2018. I nuovi clienti e i clienti attuali non potranno più scaricare l'SDK tramite il portale di Azure classico. Per scaricarlo sarà necessario rivolgersi al supporto tecnico clienti di Azure per ricevere il pacchetto di credenziali del servizio MFA generato. <br> Il team di sviluppo Microsoft sta lavorando a modifiche per MFA tramite l'integrazione con il SDK del server Azure Multi-Factor Authentication.

L'articolo seguente descrive l'aggiornamento della configurazione e la procedura per il passaggio semplice dal SDK di Azure MFA al SDK del server Azure Multi-Factor Authentication quando questo componente verrà rilasciato e incluso in un prossimo hotfix. Per gli annunci corrispondenti, vedere la [cronologia versioni](/reference/version-history.md). 

## <a name="prerequisites"></a>Prerequisiti

Per usare il server Azure Multi-Factor Authentication con MIM è necessario quanto segue:

- Accesso a Internet da ogni servizio MIM o server MFA che rende disponibile PAM, per contattare il servizio Azure MFA
- Sottoscrizione di Azure
- L'installazione sta già usando il SDK Azure MFA
- Licenze di Azure Active Directory Premium per gli utenti candidati o un metodo alternativo di licenze di Azure MFA
- Numeri di telefono di tutti gli utenti candidati
- Hotfix MIM 4.5 o successive, vedere la [cronologia versioni](/reference/version-history.md) per gli annunci corrispondenti

## <a name="azure-multi-factor-authentication-server-configuration"></a>Configurazione del server Azure Multi-Factor Authentication 
> [!NOTE] 
> La configurazione deve includere un certificato SSL valido installato per il SDK. 

### <a name="step-1-download-azure-multi-factor-authentication-server-from-the-azure-portal"></a>Passaggio 1: Scaricare il server Azure Multi-Factor Authentication dal portale di Azure 
Accedere al [portale di Azure](https://portal.azure.com/) e scaricare il server Azure MFA.
![working-with-mfaserver-for-mim_downloadmfa](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_downloadmfa.PNG)

### <a name="step-2-generate-activation-credentials"></a>Passaggio 2: Generare le credenziali di attivazione
Usare il collegamento **Genera le credenziali di attivazione per avviare l'uso** per generare le credenziali di attivazione. Dopo aver generato le credenziali, salvarle per usarle in seguito.

### <a name="step-3-install-the-azure-multi-factor-authentication-server"></a>Passaggio 3: Installare il server Azure Multi-Factor Authentication
Dopo aver scaricato il server, [eseguire l'installazione](https://docs.microsoft.com/en-us/azure/active-directory/authentication/howto-mfaserver-deploy#install-and-configure-the-mfa-server).  Sono necessarie le credenziali di attivazione. 

### <a name="step-4-create-your-iis-web-application-that-will-host-the-sdk"></a>Passaggio 4: Creare l'applicazione Web IIS che ospita il SDK
1. Aprire Gestione IIS ![working-with-mfaserver-for-mim_iis.PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_iis.PNG)
2.  Creare un nuovo sito Web con nome "MIM MFASDK" e collegarlo a una directory vuota ![working-with-mfaserver-for-mim_sdkweb.PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_sdkweb.PNG)
3. Aprire la console Multi-Factor Authentication e fare clic su SDK servizio Web ![working-with-mfaserver-for-mim_sdkinstall.PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_sdkinstall.PNG)
4. Nella procedura guidata procedere alla configurazione, quindi selezionare "MIM MFASDK" e Pool di app

> [!NOTE] 
> La procedura guidata richiederà la creazione di un gruppo di amministratori. Altre informazioni sono disponibili nella documentazione Azure >> server Azure Multi-Factor Authentication.

5. Ora è necessario importare l'account del servizio MIM. Aprire la console Multi-Factor Authentication e selezionare "Utenti" a. Fare clic su "Importa da Active Directory" b. Passare all'account del servizio denominato "contoso\mimservice" c. Fare clic su "Importa" e "Chiudi" ![working-with-mfaserver-for-mim_importmimserviceaccount.PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_importmimserviceaccount.PNG) 
6. Impostare lo stato dell'account del servizio MIM su Abilitato nella console Multi-Factor Authentication ![working-with-mfaserver-for-mim_enableserviceaccount.PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_enableserviceaccount.PNG)
7. Aggiornare l'autenticazione IIS nel sito Web "MIM MFASDK". In primo luogo disabilitare "Autenticazione anonima", quindi abilitare "Autenticazione di Windows" ![working-with-mfaserver-for-mim_iisconfig.PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_iisconfig.PNG)
8. Passaggio finale: Aggiungere l'account del servizio MIM al gruppo "PhoneFactor Admins" ![working-with-mfaserver-for-mim_addservicetomfaadmin.PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_addservicetomfaadmin.PNG)

## <a name="configuring-the-mim-service-for-azure-multi-factor-authentication-server"></a>Configurazione del servizio MIM per il server Azure Multi-Factor Authentication 

### <a name="step-1-patch-server-to-452020"></a>Passaggio 1: Applicare al server la patch 4.5.202.0
 
### <a name="step-2-backup-and-open-the-mfasettingsxml-located-in-the-cprogram-filesmicrosoft-forefront-identity-manager2010service"></a>Passaggio 2: Eseguire il backup e aprire il file MfaSettings.xml in "C:\Programmi\Microsoft Forefront Identity Manager\2010\Service"

### <a name="step-3-update-the-following-lines"></a>Passaggio 3: Aggiornare le righe seguenti
1. Rimuovere o cancellare le seguenti righe di voci di configurazione <br>
<LICENSE_KEY></LICENSE_KEY><br>
<GROUP_KEY></GROUP_KEY><br>
<CERT_PASSWORD></CERT_PASSWORD><br>
<CertFilePath></CertFilePath><br>

2. Aggiornare o aggiungere le righe seguenti in MfaSettings.xml <br>
`<Username>mimservice@contoso.com</Username>` <br>
`<LOCMFA>true</LOCMFA>`<br>
`<LOCMFASRV>https://CORPSERVICE.contoso.com:9999/MultiFactorAuthWebServiceSdk/PfWsSdk.asmx</LOCMFASRV>`

3. Riavviare il servizio MIM e verificare la funzionalità con il server Azure Multi-Factor Authentication.

> [!NOTE] 
> Per annullare le impostazioni, sostituire il file MfaSettings.xml con il file di backup creato nel passaggio 2


## <a name="next-steps"></a>Passaggi successivi

-    [Introduzione al server Azure Multi-Factor Authentication](https://docs.microsoft.com/en-us/azure/active-directory/authentication/howto-mfaserver-deploy)
- [Che cos'è Azure Multi-Factor Authentication?](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication)
- [Usare l'API personalizzata Azure Multi-Factor Authentication per attivare PAM o SSPR](Working-with-custommfaserver-for-mim.md)
- [Cronologia di rilascio delle versioni di MIM](./reference/version-history.md)
