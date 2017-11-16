---
title: Installazione di BHOLD Reporting | Microsoft Docs
description: Il modulo BHOLD Reporting consente di generare report sui ruoli e sui criteri di autorizzazione
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/07/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 
ms.openlocfilehash: aa6a263daadc4abdcad0eaaba554b6bc739fbd5f
ms.sourcegitcommit: 0d8b19c5d4bfd39d9c202a3d2f990144402ca79c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/14/2017
---
# <a name="bhold-reporting-installation"></a>Installazione di BHOLD Reporting

Il modulo BHOLD Reporting consente di generare report sui ruoli e su altri criteri di autorizzazione in BHOLD. Questi report sono spesso utili per verificare o dimostrare la conformità ai requisiti normativi. Con questo modulo è anche possibile gestire l'autorizzazione all'interno dell'organizzazione offrendo agli utenti le informazioni necessarie per analizzare l'appartenenza dei propri ruoli. È possibile che le visualizzazioni dei report siano limitate. In questo modo si garantisce agli utenti che creano i report soltanto la visualizzazione delle informazioni a loro consentite.

## <a name="bhold-reporting-installation-requirements"></a>Requisiti di installazione di BHOLD Reporting

Prima di installare il modulo BHOLD Reporting, è necessario installare il modulo BHOLD Core nel server in cui si vuole installare il modulo BHOLD Reporting. Per informazioni sull'installazione e la configurazione del modulo BHOLD Core, vedere [Installazione di BHOLD Core](https://technet.microsoft.com/en-us/library/jj134095(v=ws.10).aspx).

>[!IMPORTANT]
Se si installa sia il modulo BHOLD Reporting che BHOLD Attestation, installare prima BHOLD Reporting e poi BHOLD Attestation.

## <a name="before-you-begin"></a>Prima di iniziare

Prima di iniziare l'installazione del modulo BHOLD Reporting, procurarsi le informazioni da specificare durante la l'installazione guidata di BHOLD Reporting necessarie per completare l'installazione. Il foglio di lavoro riportato di seguito consente di registrare le informazioni in modo che siano disponibili al momento dell'inserimento.

| **Elemento**                                    | **Descrizione**                                                                                                                                                                                                           | **Valore**                                                                                                                                                                                                                                                                                                            |
|---------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Use Security Provider on Domain/Machine** (Usa provider di sicurezza per il dominio/computer) | Se selezionata, questa opzione indica che la protezione di Active Directory Domain Services controllerà l'accesso a BHOLD Core.                                                                                                                | Selezionare la casella di controllo. </br>**Importante:** l'installazione avrà esito negativo se questa casella di controllo non è selezionata.                                                                                                                                                                                                                   |
| **Dominio**                                  | Specifica il dominio che contiene l'account del servizio creato durante l'installazione di BHOLD Core. Per altre informazioni, vedere [Installazione di BHOLD Core](https://technet.microsoft.com/en-us/library/jj134095(v=ws.10).aspx). | Il nome di dominio viene inserito automaticamente dalla procedura guidata. Modificare il nome solo se non è corretto. **Importante:** specificare il nome di dominio usando il nome NetBIOS (breve), non il nome di dominio completo (FQDN). Se, ad esempio, il nome FQDN del dominio è fabrikam.com, specificare il nome di dominio come FABRIKAM. |
| **Utente**                                    | Specifica il nome di accesso dell'account utente del servizio BHOLD Core.                                                                                                                                                          | Scrivere qui il nome dell'account utente:                                                                                                                                                                                                                                                                                    |
| **Password**                                | Specifica la password dell'account utente del servizio.                                                                                                                                                                       | Scrivere qui la password: </br>**Importante:** assicurarsi di conservare questa password in una posizione protetta nascosta.                                                                                                                                                                                                                  |

## <a name="bhold-reporting-installation"></a>Installazione di BHOLD Reporting

Per installare il modulo BHOLD Reporting, accedere come membro del gruppo Domain Admins, scaricare il file seguente ed eseguirlo come amministratore nel server in cui si vuole installare il modulo BHOLD Reporting:

- BholdReporting*\<versione\>*\_Release.msi

Sostituire *\<versione\>* con il numero della versione di BHOLD Reporting che si sta installando.

Per eseguire il file di programma come amministratore, fare clic con il pulsante destro del mouse sul file e selezionare **Esegui come amministratore**.

## <a name="next-steps"></a>Passaggi successivi

- [Guida all'installazione di BHOLD](bhold-installation-guide.md)
- [Riferimento per gli sviluppatori BHOLD](../reference/mim2016-bhold-developer-reference.md)
- [Cronologia delle versioni BHOLD](../reference/version-bhold-history.md)