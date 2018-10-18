---
title: Installazione di BHOLD Attestation | Microsoft Docs
description: Il modulo BHOLD Attestation consente di designare revisori e di eseguire revisioni
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 09/07/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: e4c3a6248585d55fddbbca3153f33734d7c5c429
ms.sourcegitcommit: 7de35aaca3a21192e4696fdfd57d4dac2a7b9f90
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/16/2018
ms.locfileid: "49358109"
---
# <a name="bhold-attestation-installation"></a>Installazione di BHOLD Attestation

Il modulo BHOLD Attestation consente di designare i revisori e di eseguire revisioni ricorrenti delle relazioni tra utenti e autorizzazioni e account per applicazione.

## <a name="bhold-attestation-installation-requirements"></a>Requisiti per l'installazione di BHOLD Attestation

Prima di installare il modulo BHOLD Attestation, è necessario installare il modulo principale BHOLD Core nel server in cui si prevede di installare il modulo BHOLD Attestation. Per informazioni sull'installazione e la configurazione del modulo BHOLD Core, vedere [Installazione di BHOLD Core](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx). Poiché i contatti del modulo BHOLD Attestation inviano messaggi di posta elettronica agli utenti, l'ambiente deve disporre di un server di posta elettronica Simple Mail Transfer Protocol (SMTP), come ad esempio Microsoft Exchange Server.

> [!IMPORTANT]
> Se si installa sia il modulo BHOLD Reporting che BHOLD Attestation, installare prima BHOLD Reporting e poi BHOLD Attestation.

## <a name="before-you-begin"></a>Prima di iniziare

Prima di iniziare l'installazione del modulo BHOLD Attestation, procurarsi le informazioni da specificare durante l'installazione guidata di BHOLD Attestation necessarie per completare l'installazione. Il foglio di lavoro riportato di seguito consente di registrare le informazioni in modo che siano disponibili al momento dell'inserimento.

| **Elemento**                                    | **Descrizione**                                                                                                                                                                                                           | **Valore**                                                                                                                                                                                                                                                                                                            |
|---------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Use Security Provider on Domain/Machine** (Usa provider di sicurezza per il dominio/computer) | Se selezionata, questa opzione indica che la protezione di Active Directory Domain Services controllerà l'accesso a BHOLD Core.                                                                                                                | Selezionare la casella di controllo. **Importante:** l'installazione avrà esito negativo se questa casella di controllo non è selezionata.                                                                                                                                                                                                                   |
| **Dominio**                                  | Specifica il dominio che contiene l'account del servizio creato durante l'installazione di BHOLD Core. Per altre informazioni, vedere [Installazione di BHOLD Core](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx). | Il nome di dominio viene inserito automaticamente dalla procedura guidata. Modificare il nome solo se non è corretto. **Importante:** specificare il nome di dominio usando il nome NetBIOS (breve), non il nome di dominio completo (FQDN). Se, ad esempio, il nome FQDN del dominio è fabrikam.com, specificare il nome di dominio come FABRIKAM. |
| **User**                                    | Specifica il nome di accesso dell'account utente del servizio BHOLD Core.                                                                                                                                                          | Scrivere qui il nome dell'account utente:                                                                                                                                                                                                                                                                                    |
| **Password**                                | Specifica la password dell'account utente del servizio.                                                                                                                                                                       | Scrivere la password qui: **Importante:** assicurarsi di conservare questa password in una posizione protetta nascosta.                                                                                                                                                                                                                  |

## <a name="bhold-attestation-installation"></a>Installazione dell'attestazione BHOLD

Per installare il modulo BHOLD Attestation, accedere come membro del gruppo Domain Admins, scaricare il file seguente ed eseguirlo come amministratore nel server in cui si intende installare il modulo BHOLD Attestation:

- BholdAttestation<em>\<Versione\></em>\_Release.msi

Sostituire *\<Versione\>* con il numero della versione di BHOLD Attestation che si sta installando.

Per eseguire il file di programma come amministratore, fare clic con il pulsante destro del mouse sul file e selezionare **Esegui come amministratore**.

## <a name="next-steps"></a>Passaggi successivi

- [Guida all'installazione di BHOLD](bhold-installation-guide.md)
- [Riferimento per gli sviluppatori BHOLD](../reference/mim2016-bhold-developer-reference.md)
- [Cronologia delle versioni BHOLD](../reference/version-bhold-history.md)
