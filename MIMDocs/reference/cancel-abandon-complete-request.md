---
title: Annullare, abbandonare o completare una richiesta | Microsoft Docs
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
ms.assetid: e29e0068-7602-455e-8a3a-690da9ca8eb5
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 4f81c9d86006c477993b2ac8cc2e845de2c1d2f3
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/13/2017
---
# <a name="cancel-abandon-or-complete-a-request"></a>Annullare, abbandonare o completare una richiesta
Contrassegna una richiesta CM MIM come completata, annullata o abbandonata.

**Nota**: gli URL mostrati in questo argomento sono relativi al nome host scelto durante la distribuzione delle API. Ad esempio: `https://api.contoso.com`.
##<a name="request"></a>Richiesta


Metodo  |URL della richiesta  
---------|---------
PUT     |/CertificateManagement/api/v1.0/requests/{ID}

###<a name="url-parameters"></a>Parametri URL
Proprietà| Descrizione
---------|--------
ID| Obbligatorio. Il GUID del completamento della richiesta.


###<a name="request-headers"></a>Intestazioni delle richieste
Per le intestazioni di richiesta comuni, vedere [HTTP Request and Response Headers](certificate-management-rest-api-service-details.md#http-request-and-response-headers) (Intestazioni di richiesta e risposta HTTP) in *Dettagli del servizio API REST di gestione certificati*.
###<a name="request-body"></a>Testo della richiesta
Il testo della richiesta contiene le proprietà seguenti.

Proprietà | Descrizione
---------|-----------
stato | Lo stato su cui impostare la richiesta. Valori possibili: "Completed", "Canceled" o "Abandoned".


##<a name="response"></a>Risposta
###<a name="response-codes"></a>Codici di risposta
Codice  |Descrizione  
---------|---------
200     | OK
204 | Nessun contenuto
403 | Non consentito
500 | Errore interno

###<a name="response-headers"></a>Intestazioni delle risposte
Per le intestazioni di risposta comuni, vedere [HTTP Request and Response Headers](certificate-management-rest-api-service-details.md#http-request-and-response-headers) (Intestazioni di richiesta e risposta HTTP) in *Dettagli del servizio API REST di gestione certificati*.
###<a name="response-body"></a>Testo della risposta
In caso di esito positivo, restituisce un oggetto [Microsoft.Clm.Shared.Requests.Request](https://msdn.microsoft.com/library/microsoft.clm.shared.requests.request.aspx) con le seguenti proprietà che descrivono la richiesta CM MIM che è stata contrassegnata come completata:

Nome | Descrizione
-----|------------
Commento | Il commento associato alla richiesta CM MIM.
Completed | L’ora in cui è stata completata la richiesta CM MIM.
DataCollection | Gli elementi di raccolta dati che sono associati alla richiesta CM MIM.
DataCollectionFlag | Le opzioni per la raccolta di dati per la richiesta CM MIM.
Flag | Le opzioni associate alla richiesta CM MIM.
IsDataCollectionComplete | Valore booleano che indica se la raccolta dei dati è stata completata per la richiesta CM MIM.
IsEnrollmentAgent | Valore booleano che indica se è necessario un agente di registrazione per eseguire la richiesta CM MIM.
IsSmartcard | Valore booleano che indica se la richiesta CM MIMè una richiesta smart card o una richiesta di profilo software.
NewProfileUuID | L'identità del nuovo profilo software che viene generato dalla richiesta CM MIM.
NewSmartcardUuID | L'identità della nuova smart card che viene generata dalla richiesta CM MIM.
OldProfileUuID | L'identità del profilo software per cui è stata creata la richiesta CM MIM.
OldSmartcardUuID | L'identità della smart card per cui è stata creata la richiesta CM MIM.
OriginatorUserUuID | L'identità dell'utente che ha dato origine alla richiesta CM MIM
Priorità | La priorità della richiesta CM MIM.
ProfileTemplateUuID | L'identità del modello di profilo per cui è stata creata la richiesta CM MIM.
RichiestaType | Il tipo della richiesta CM MIM.
SecurityDescriptor | Descrittore di sicurezza per la richiesta CM MIM.
Stato | Lo stato della richiesta CM MIM.
Submitted | L’ora in cui è stata inviata la richiesta CM MIM.
TargetUserUuID | L'identità dell'utente di destinazione per la richiesta CM MIM.
UuID | L’identificatore della richiesta CM MIM.

##<a name="example"></a>Esempio

###<a name="request"></a>Richiesta
```
PUT /certificatemanagement/api/v1.0/requests/a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099 HTTP/1.1


{
    “status”:”Completed”
}
```
###<a name="response"></a>Risposta
```
HTTP/1.1 200 OK

{
    "Uuid":"a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099",
    "RequestType":1,
    "Status":8,
    "Flags":2,
    "DataCollection":[

    ],
    "DataCollectionFlags":32,
    "OriginatorUserUuid":"8f1590dc-d932-4b66-8e68-2e91c5880780",
    "TargetUserUuid":"8f1590dc-d932-4b66-8e68-2e91c5880780",
    "Submitted":"2015-07-07T23:36:21.93Z",
    "Completed":"2015-07-07T23:37:37.767Z",
    "NewProfileUuid":"b99ff38c-6653-471f-ae3c-325bb351a6bc",
    "OldProfileUuid":"00000000-0000-0000-0000-000000000000",
    "NewSmartcardUuid":"17cf063d-e337-4aa9-a822-346554ddd3c9",
    "OldSmartcardUuid":"00000000-0000-0000-0000-000000000000",
    "Priority":0,
    "Comment":"",
    "ProfileTemplateUuid":"a039b4d0-5ce8-4eff-8651-181c6576fda3",
    "SecurityDescriptor":"O:S-1-5-21-2127521184-1604012920-1887927527-14134865D:(D;;LC;;;S-1-5-21-2127521184-1604012920-1887927527-14995557)(A;;DCSW;;;S-1-5-21-2127521184-1604012920-1887927527-5094533)(A;;DCSW;;;S-1-5-21-2127521184-1604012920-1887927527-5094534)(A;;DCSW;;;S-1-5-21-2127521184-1604012920-1887927527-5219125)(A;;SWRC;;;S-1-5-21-2127521184-1604012920-1887927527-5094533)(A;;RC;;;WD)(A;;CCDCLCSWSDRC;;;S-1-5-21-2127521184-1604012920-1887927527-14134865)(A;;DCSW;;;S-1-5-21-2127521184-1604012920-1887927527-14995557)(A;;CCDCSW;;;S-1-5-21-2127521184-1604012920-1887927527-14995557)\u0000\u0000\u0000\u0000\u0000\u0000\u0000\u0000\u0000",
    "IsSmartcard":true,
    "IsEnrollmentAgent":false,
    "IsDataCollectionComplete":false
}
```       
##<a name="see-also"></a>Vedere anche

- [Metodo Microsoft.Clm.Provision.ExecuteOperations.Complete](https://msdn.microsoft.com/library/microsoft.clm.provision.executeoperations.complete.aspx)
- [Microsoft.Clm.Shared.Requests.Request](https://msdn.microsoft.com/library/microsoft.clm.shared.requests.request.aspx)
