---
title: Creare certificati software | Microsoft Identity Manager
description: Informazioni su come usare Gestione certificati per creare e rinnovare certificati software con i modelli di profilo.
keywords: 
author: kgremban
manager: femila
ms.date: 07/21/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: fed5ada9-d80f-4825-aad7-4172ac5d71d3
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: b3ab1b9376c9b613739d87c812f4b16a4e17e6de
ms.openlocfilehash: d5c8fc4f9a3eaab95441f7a915f7e02d55042ae9


---

# Creare certificati software con Gestione certificati
Per registrare e rinnovare i certificati software non è necessario essere un amministratore e non occorre una smart card virtuale. Vale la pena notare che a un certo punto verrà richiesto di consentire un'operazione di certificato, e questo è normale.

## Creare un modello di profilo dei certificati software in Gestione certificati di MIM 2016

1.  Creare un modello per il certificato che verrà richiesto per la smart card virtuale. Aprire la MMC.

2.  Fare clic su **File**, quindi su **Aggiungi/Rimuovi snap-in**.

3.  Nell'elenco di snap-in disponibili, fare clic su **Modelli di certificato**, quindi su **Aggiungi**.

4.  **Modelli di certificato** è ora disponibile nella radice console di MMC. Fare doppio clic per visualizzare tutti i modelli di certificato disponibili.

5.  Fare clic con il pulsante destro del mouse su **Modello utente**, quindi fare clic su **Duplica modello**.

6.  Nella scheda **Compatibilità** in Autorità di certificazione, selezionare Windows Server 2008 e in Destinatario certificato selezionare Windows 8.1 oppure Windows Server 2012 R2.

    1.  Nella scheda **Generale** , digitare **Modello di certificato archiviato**nel campo nome visualizzato.

    2.  b.  Nella scheda **Gestione richiesta**

        1.  Impostare **Scopo** su Firma e crittografia.

        2.  Selezionare **Includi gli algoritmi simmetrici consentiti dal soggetto**.

        3.  Se si desidera archiviare la chiave, selezionare **Archivia chiave privata di crittografia del soggetto**.

        4.  In Eseguire le operazioni seguenti... Selezionare **Chiedi conferma all’utente durante la registrazione**.

    3.  Nella scheda **Crittografia**

        1.  In Categoria provider, selezionare **Provider di archiviazione chiavi**

        2.  Le richieste di **selezione possono utilizzare qualsiasi provider disponibile nel computer del soggetto**.

    4.  Nella scheda di **Protezione** , aggiungere il gruppo di sicurezza a cui si desidera assegnare l'accesso a **Registrazione** . Ad esempio, se si desidera concedere l'accesso a tutti gli utenti, selezionare il gruppo utenti **Autenticato** , quindi selezionare **Registrazione** delle autorizzazioni per gli utenti.

    5.  Nella scheda **Nome soggetto**

        1.  Deselezionare l'opzione **Includi nome posta elettronica nel nome soggetto**.

        2.  In **Includere le seguenti informazioni nel nome soggetto alternativo**, deselezionare l’opzione **Nome posta elettronica**.

    6.  Fare clic su **OK** per rendere effettive le modifiche e creare il nuovo modello. Il nuovo modello verrà ora visualizzato nell'elenco Modelli di certificato.

    7.  Selezionare **File**, quindi fare clic su **Aggiungi/Rimuovi snap-in** per aggiungere lo snap-in Autorità di certificazione alla console di MMC. Quando viene richiesto quale computer si desidera gestire, selezionare **Computer locale**.

    8.  Nel riquadro sinistro di MMC, espandere **Autorità di certificazione (locale)**, quindi espandere l'autorità di certificazione in uso all'interno dell'elenco di autorità di certificazione.

    9. Fare clic con il pulsante destro del mouse su **Modelli di certificato**, fare clic su **Nuovo**, quindi fare clic su **Modello di certificato da rilasciare**.

    10. Nell'elenco, selezionare il nuovo modello appena creato (**Modello di certificato archiviato**), quindi fare clic su **OK**.

## Creare il modello di profilo

1.  Accedere al portale CM come utente con privilegi amministrativi.

2.  Passare ad **Amministrazione &gt; Gestisci modelli di profilo** e assicurarsi che la casella accanto a **Esempio di modello di profilo di MIM CM per accesso tramite smart card** sia selezionata, quindi fare clic su **Copia modello di profilo selezionato**.

3.  Digitare il nome del modello di profilo e fare clic su **OK**.

4.  Nella schermata successiva, fare clic su **Aggiungi nuovo modello di certificato** e assicurarsi di selezionare la casella accanto al nome della CA.

5.  Selezionare la casella accanto al nome del certificato software archiviato e fare clic su **Aggiungi**.

6.  Rimuovere il modello utente selezionando la casella accanto, e quindi facendo clic su **Elimina i modelli di certificato selezionati** , quindi fare clic su **OK**.

7.  Fare clic su **Modifica impostazioni generali**.

8.  Selezionare le caselle alla sinistra di **Genera chiavi di crittografia sul server** e fare clic su **OK**. Nel riquadro sinistro, fare clic su **Criteri di recupero**.

9. Fare clic su **Modifica impostazioni generali**.

10. Se si desidera eseguire nuovamente i certificati archiviati, selezionare le caselle alla sinistra di **Riemetti certificati archiviati** e fare clic su **OK**.

11. Se si utilizza il CM Smart Card virtuale, è necessario disattivare gli elementi della raccolta dati, poiché non funziona con la raccolta dei dati attiva. Disabilitare la raccolta dei dati per ogni criterio facendo clic sul criterio nel riquadro a sinistra e quindi deselezionando la casella accanto a **Elemento di dati di esempio** . Quindi fare clic su **Eliminare elemento raccolta dati**. Fare quindi clic su **OK**.



<!--HONumber=Jul16_HO3-->


