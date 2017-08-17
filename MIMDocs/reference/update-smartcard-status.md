---
title: Aggiornare lo stato della smart card | Microsoft Docs
description: 
keywords: 
author: msmbaldwin
ms.author: mbaldwin
manager: mbaldwin
ms.date: 10/17/2016
ms.topic: reference
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 598dace3-c6f2-447a-9301-c0b63ee38276
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: cb7b04539b8568a8b2ae83172b2aa8cddbf6174d
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/13/2017
---
# <a name="update-smartcard-status"></a>Update Smartcard Status
Aggiorna lo stato di una smart card.

**Nota**: gli URL mostrati in questo argomento sono relativi al nome host scelto durante la distribuzione delle API. Ad esempio: `https://api.contoso.com`.
## <a name="request"></a>Richiesta


Metodo  |URL della richiesta  
---------|---------
GET     |/CertificateManagement/api/v1.0/requests/{reqid}/smartcards/{scid}

### <a name="url-parameters"></a>Parametri URL
Parametro | Descrizione
---------|------------
reqid | Obbligatorio. Identificatore della richiesta (specifico di CM MIM).
scid | Obbligatorio. Identificatore smart card (specifico di CM MIM). Si tratta della proprietà "uuid" nell'oggetto [Microsoft.Clm.Shared.Smartcards.Smartcard](http://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx).

### <a name="request-headers"></a>Intestazioni delle richieste
Per le intestazioni di richiesta comuni, vedere [HTTP Request and Response Headers](certificate-management-rest-api-service-details.md#http-request-and-response-headers) (Intestazioni di richiesta e risposta HTTP) in *Dettagli del servizio API REST di gestione certificati*.
### <a name="request-body"></a>Testo della richiesta
Il testo della richiesta contiene le proprietà seguenti.

Proprietà | Descrizione
---------|-----------
stato | Lo stato su cui impostare la richiesta. Ad esempio, "Retired".


## <a name="response"></a>Risposta
### <a name="response-codes"></a>Codici di risposta
Codice  |Descrizione  
---------|---------
200     | OK
204 | Nessun contenuto
403 | Non consentito
500 | Errore interno

### <a name="response-headers"></a>Intestazioni delle risposte
Per le intestazioni di risposta comuni, vedere [HTTP Request and Response Headers](certificate-management-rest-api-service-details.md#http-request-and-response-headers) (Intestazioni di richiesta e risposta HTTP) in *Dettagli del servizio API REST di gestione certificati*.
### <a name="response-body"></a>Testo della risposta
In caso di esito positivo, restituisce un oggetto [Microsoft.Clm.Shared.Smartcards.Smartcard](http://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx) serializzato nel formato JSON con le proprietà seguenti:

Nome | Descrizione
-----|-----------
AssignedUserUuid | Identificatore dell'utente a cui è assegnata la smart card.
Atr | Stringa ATR (Answer To Reset) di smart card per la scheda in fase di inizializzazione.
Commento | Il commento che descrive la smart card.
Flag | I flag che descrivono la smart card.
Middleware | Il middleware per la smart card.
ParentSmartcardUuid | Identificatore della smart card precedente sostituita dalla smart card.
PermanentSmartcardUuid | Identificatore della smart card permanente associata alla smart card.
PrimarySmartcardUuid | Identificatore della smart card primaria.
ProfileTemplateUuid | Identificatore del modello di profilo che contiene criteri e impostazioni che controllano la smart card.
ProfileTemplateVersion | La versione del modello di profilo al momento della creazione del profilo della smart card.
SerialNumber | Numero di serie della smart card.
Stato | Lo stato della smart card.
Uuid | Identificatore del profilo della smart card.

## <a name="example"></a>Esempio

### <a name="request"></a>Richiesta
```
PUT /certificatemanagement/api/v1.0/requests/b105403d-d021-41ea-9f11-be3d677d229e/smartcards/17cf063d-e337-4aa9-a822-346554ddd3c9 HTTP/1.1

```
### <a name="response"></a>Risposta
```
HTTP/1.1 200 OK

{
    "Uuid":"17cf063d-e337-4aa9-a822-346554ddd3c9",
    "Status":6,
    "Flags":1,
    "ParentSmartcardUuid":"00000000-0000-0000-0000-000000000000",
    "PrimarySmartcardUuid":"00000000-0000-0000-0000-000000000000",
    "PermanentSmartcardUuid":"00000000-0000-0000-0000-000000000000",
    "AssignedUserUuid":"8f1590dc-d932-4b66-8e68-2e91c5880780",
    "ProfileTemplateUuid":"a039b4d0-5ce8-4eff-8651-181c6576fda3",
    "ProfileTemplateVersion":46,
    "Comment":"",
    "SerialNumber":"{bc88f13f-83ba-4037-8262-46eba1291c6e}",
    "Middleware":"MSBaseCSP",
    "Atr":"3b8d0180fba000000397425446590301c8"
}
```       
## <a name="see-also"></a>Vedere anche

- [Classe Microsoft.Clm.Shared.Smartcards.Smartcard](https://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx))
