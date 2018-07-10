---
title: Installazione di BHOLD Model Generator | Microsoft Docs
description: BHOLD Model consente di strutturare i dati provenienti da diverse origini
keywords: ''
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/07/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: ''
ms.openlocfilehash: 90e7da2a1e39b802723ff0714bd0caccf9649440
ms.sourcegitcommit: 35f2989dc007336422c58a6a94e304fa84d1bcb6
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36289136"
---
# <a name="bhold-model-generator-installation"></a>Installazione di BHOLD Model Generator

Mediante il modulo BHOLD Model Generator è possibile strutturare i dati provenienti da origini attendibili contenenti informazioni sugli utenti e sull'organizzazione insieme a elenchi di controllo di accesso (ACL) in un modello che può essere usato nella gestione di BHOLD.

## <a name="bhold-model-generator-installation-requirements"></a>Requisiti di installazione di BHOLD Model Generator 

Prima di installare il modulo BHOLD Model Generator, è necessario installare quanto segue:

1. Modulo BHOLD Core nel server in cui si prevede di installare il modulo BHOLD Model Generator. Per informazioni sull'installazione del modulo principale BHOLD Core, vedere [BHOLD Core Installation](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx) (Installazione di BHOLD Core).

2. Deve essere installato il Provider Microsoft OLE DB per Microsoft Jet. Per altre informazioni, vedere questo [articolo](http://support.microsoft.com/kb/271908).

> [!WARNING]
> Non installare BHOLD Model Generator nella propria rete di produzione. BHOLD Model Generator è destinato a essere usato offline in un ambiente di gestione temporanea per creare un modello di ruolo normalizzato che è possibile importare nel modello di ruolo dell'organizzazione. L'esecuzione di BHOLD Model Generator nella rete di produzione dell'organizzazione può comportare la perdita del modello di ruolo esistente.

## <a name="before-you-begin"></a>Prima di iniziare

Prima di installare il modulo BHOLD Model Generator, procurarsi le informazioni da specificare durante l'installazione guidata di BHOLD Model Generator necessarie per completare l'installazione. Il foglio di lavoro riportato di seguito consente di registrare le informazioni in modo che siano disponibili al momento dell'inserimento. È inoltre necessario disporre di

Microsoft Access Database Engine 2010 Redistributable

 

*Da \<*<http://daipvstf:8080/tfs/ActiveDirectory/IAM/_workitems>*\>*

 

<https://www.microsoft.com/en-us/download/confirmation.aspx?id=13255>

**Impostazioni account**

| **Elemento**                                    | **Descrizione**                                                                                                                                                                                                           | **Valore**                                                                                                                                                                                                                                                                                                            |
|---------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Use Security Provider on Domain/Machine** (Usa provider di sicurezza per il dominio/computer) | Se selezionata, questa opzione indica che la protezione di Active Directory Domain Services controllerà l'accesso a BHOLD Core.                                                                                                                | Selezionare la casella di controllo. **Importante:** l'installazione avrà esito negativo se questa casella di controllo non è selezionata.                                                                                                                                                                                                                   |
| **Dominio**                                  | Specifica il dominio che contiene l'account del servizio creato durante l'installazione di BHOLD Core. Per altre informazioni, vedere [Installazione di BHOLD Core](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx). | Il nome di dominio viene inserito automaticamente dalla procedura guidata. Modificare il nome solo se non è corretto. **Importante:** specificare il nome di dominio usando il nome NetBIOS (breve), non il nome di dominio completo (FQDN). Se, ad esempio, il nome FQDN del dominio è fabrikam.com, specificare il nome di dominio come FABRIKAM. |
| **User**                                    | Specifica il nome di accesso dell'account utente del servizio BHOLD Core.                                                                                                                                                          | Scrivere qui il nome dell'account utente:                                                                                                                                                                                                                                                                                    |
| **Password**                                | Specifica la password dell'account utente del servizio.                                                                                                                                                                       | Scrivere la password qui: **importante:** assicurarsi di conservare questa password in una posizione protetta nascosta.                                                                                                                                                                                                                  |

**Impostazioni database di backup**

| Elemento                                        | Descrizione                                                                                                                                                                                                                                                                                                                                                                                                                  | Valore                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
|---------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Use integrated Security** (Usa sicurezza integrata)                 | Specifica che per accedere al database viene usata l'autenticazione di Windows.                                                                                                                                                                                                                                                                                                                                                        | Selezionare la casella di controllo se viene usata l'autenticazione di Windows per connettersi a SQL Server. Deselezionare la casella di controllo se viene usata l'autenticazione di SQL Server. Se si usa l'autenticazione di SQL Server è necessario creare il database prima di eseguire l'installazione di BHOLD Core. **Nota:** se viene usata l'autenticazione di Windows, è necessario eseguire l'accesso con un account con il ruolo del server sysadmin nel server di database. **Importante:** usare l'autenticazione di SQL Server solo negli ambienti di test. Microsoft consiglia vivamente di usare l'autenticazione di Windows nelle distribuzioni di produzione. |
| **Utente database** e **Password database** | Specifica il nome utente e la password di un utente con ruolo del server sysadmin nel server di database. Questi valori vengono specificati solo quando si usa l'autenticazione di SQL Server.                                                                                                                                                                                                                                                  | Scrivere il nome utente di SQL Server qui: Scrivere la password dell'utente di SQL Server qui: </br></br> **Importante:** assicurarsi di conservare questa password in una posizione protetta nascosta.                                                                                                                                                                                                                                                                                                                                                                                                           |
| **Server database** e **Nome database**   | Specifica il nome NetBIOS del server di database e il nome del database di backup che verrà creato con l'impostazione di BHOLD Model Generator. Se non si usa l'istanza del server di database predefinita, specificare l'istanza del server di database nel formato *\<server\>*\\*\<istanza\>*.  Microsoft consiglia di assegnare un nome al database di backup usando il nome del database principale BHOLD seguito da \_BACKUP, ad esempio B1_BACKUP. | Scrivere qui il nome del server (server o istanza): </br> Scrivere qui il nome del database:

## <a name="bhold-model-generator-setup"></a>Configurazione di BHOLD Model Generator

Per installare il modulo BHOLD Model Generator, accedere come membro del gruppo Domain Admins, scaricare il file seguente ed eseguirlo come amministratore nel server in cui si intende installare il modulo BHOLD Core:

- BholdModelGenerator  *\<Versione\>*\_Release.msi

Sostituire *\<Versione\>* con il numero della versione di BHOLD Model Generator che si sta installando.

Per eseguire il file di programma come amministratore, fare clic con il pulsante destro del mouse sul file e selezionare **Esegui come amministratore**.

## <a name="next-steps"></a>Passaggi successivi

- Per informazioni su come creare i file di input, vedere la [Guida tecnica di Microsoft BHOLD Suite](https://technet.microsoft.com/library/jj134935(v=ws.10).aspx)
- [Guida all'installazione di BHOLD](bhold-installation-guide.md)
- [Riferimento per gli sviluppatori BHOLD](../reference/mim2016-bhold-developer-reference.md)
- [Cronologia delle versioni BHOLD](../reference/version-bhold-history.md)