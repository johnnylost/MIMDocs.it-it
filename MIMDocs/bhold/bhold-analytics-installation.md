---
title: Installazione di BHOLD Analytics | Microsoft Docs
description: Il modulo BHOLD Analytics consente di eseguire il test basato su regole dell'accesso ai dati
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 09/07/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: ec7069156aa033b33a139ae83e26abcdea7b482a
ms.sourcegitcommit: 7de35aaca3a21192e4696fdfd57d4dac2a7b9f90
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/16/2018
ms.locfileid: "49358330"
---
# <a name="bhold-analytics-installation"></a>Installazione di BHOLD Analytics

Il modulo BHOLD Analytics consente di eseguire il test basato su regole dell'accesso ai dati per garantire che l'organizzazione possa controllare in modo efficace l'accesso ai dati e sia conforme ai requisiti di accesso interno ed esterno. L'analisi automatizzata dell'impatto generata dal modulo BHOLD Analytics offre una panoramica che mostra il numero di utenti che sarebbero interessati dall'applicazione di una regola proposta, sia quelli che rispetterebbero la regola che quelli che la violerebbero. Il modulo BHOLD Analytics può offrire inoltre un elenco dettagliato degli utenti che rispetterebbero la regola e di quelli che la violerebbero.

## <a name="bhold-analytics-installation-requirements"></a>Requisiti dell'installazione di BHOLD Analytics

Prima di installare il modulo BHOLD Analytics, è necessario installare il modulo di base di BHOLD nel server in cui si prevede di installare il modulo BHOLD Analytics. Per informazioni sull'installazione e la configurazione del modulo BHOLD Core, vedere [Installazione di BHOLD Core](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx).

## <a name="before-you-begin"></a>Prima di iniziare

Prima di iniziare l'installazione del modulo BHOLD Analytics, procurarsi le informazioni da specificare durante l'installazione guidata di BHOLD Analytics necessarie per completare l'installazione. Il foglio di lavoro riportato di seguito consente di registrare le informazioni in modo che siano disponibili al momento dell'inserimento.

| **Elemento**                                    | **Descrizione**                                                                                                                                                                                                           | **Valore**                                                                                                                                                                                                                                                                                                            |
|---------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Use Security Provider on Domain/Machine** (Usa provider di sicurezza per il dominio/computer) | Se selezionata, questa opzione indica che la protezione di Active Directory Domain Services controllerà l'accesso a BHOLD Core.                                                                                                                | Selezionare la casella di controllo. **Importante:** l'installazione avrà esito negativo se questa casella di controllo non è selezionata.                                                                                                                                                                                                                   |
| **Dominio**                                  | Specifica il dominio che contiene l'account del servizio creato durante l'installazione di BHOLD Core. Per altre informazioni, vedere [Installazione di BHOLD Core](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx). | Il nome di dominio viene inserito automaticamente dalla procedura guidata. Modificare il nome solo se non è corretto. **Importante:** specificare il nome di dominio usando il nome NetBIOS (breve), non il nome di dominio completo (FQDN). Se, ad esempio, il nome FQDN del dominio è fabrikam.com, specificare il nome di dominio come FABRIKAM. |
| **User**                                    | Specifica il nome di accesso dell'account utente del servizio BHOLD Core.                                                                                                                                                          | Scrivere qui il nome dell'account utente:                                                                                                                                                                                                                                                                                    |
| **Password**                                | Specifica la password dell'account utente del servizio.                                                                                                                                                                       | Scrivere qui la password: **importante:** assicurarsi di conservare questa password in una posizione protetta nascosta.                                                                                                                                                                                                                  |

## <a name="bhold-analytics-installation"></a>Installazione di BHOLD Analytics

Per installare il modulo BHOLD Analytics, accedere come membro del gruppo Domain Admins, scaricare il file seguente ed eseguirlo come amministratore nel server in cui si intende installare il modulo BHOLD Analytics:

- BholdAnalytics<em>\<Versione\></em>\_Release.msi

Sostituire *\<Versione\>* con il numero della versione di BHOLD Analytics che si sta installando.

Per eseguire il file di programma come amministratore, fare clic con il pulsante destro del mouse sul file e selezionare **Esegui come amministratore**.

# <a name="next-steps"></a>Passaggi successivi

- [Installazione di BHOLD Core](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx)
- [Guida all'installazione di BHOLD](bhold-installation-guide.md)
- [Cronologia delle versioni BHOLD](../reference/version-bhold-history.md)
