---
title: Recuperare richieste PAM in sospeso | Microsoft Docs
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
ms.assetid: 005dc8fd-d73e-4557-b485-5566f16537eb
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: aa1e8cd48b1bcca6e856bd553b6b92a062a08ff4
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/13/2017
---
# <a name="get-pending-pam-requests"></a>Recuperare richieste PAM in sospeso
Utilizzato da un account con privilegi per restituire un elenco di richieste in sospeso che richiedono l’approvazione.

**Nota**: gli URL mostrati in questo argomento sono relativi al nome host scelto durante la distribuzione delle API. Ad esempio: `http://api.contoso.com`.
##<a name="request"></a>Richiesta

Metodo  |URL della richiesta  
---------|---------
GET     |/api/pamresources/pamrequeststoapprove

###<a name="query-parameters"></a>Parametri di query
Parametro | Descrizione
----------|--------------
$filter | Facoltativa. Specificare tutte le proprietà delle richieste PAM in sospeso in un’espressione di filtro per restituire un elenco filtrato di risposte. Per altre informazioni sugli operatori supportati, vedere [Filtering (Filtro) in Dettagli del servizio API REST di PAM](privileged-access-management-rest-api-service-details.md#filtering)
v | Facoltativa. La versione delle API. Se omesso, verrà utilizzata la versione corrente (rilasciata più di recente) delle API. Per altre informazioni, vedere [Versioning (Controllo versioni) in Dettagli del servizio API REST di PAM](privileged-access-management-rest-api-service-details.md#versioning)

###<a name="request-headers"></a>Intestazioni delle richieste
Per le intestazioni di richiesta comuni, vedere [HTTP Request and Response Headers](privileged-access-management-rest-api-service-details.md#http-request-and-response-headers) (Intestazioni di richiesta e risposta HTTP) in *Dettagli del servizio API REST di PAM*.
###<a name="request-body"></a>Testo della richiesta
Nessuno

##<a name="response"></a>Risposta
###<a name="response-codes"></a>Codici di risposta
Codice  |Descrizione  
---------|---------
200 | OK
401 | Non autorizzato
403 | Non consentito
408 | Timeout della richiesta   
500 | Errore interno del server
503 | Servizio non disponibile

###<a name="response-headers"></a>Intestazioni delle risposte
Per le intestazioni di risposta comuni, vedere [HTTP Request and Response Headers](privileged-access-management-rest-api-service-details.md#http-request-and-response-headers) (Intestazioni di richiesta e risposta HTTP) in *Dettagli del servizio API REST di PAM*.
###<a name="response-body"></a>Testo della risposta
Una risposta corretta contiene un elenco di oggetti di approvazione delle richieste PAM con le seguenti proprietà.

Proprietà | Descrizione
---------|-------------
RoleName | Il nome visualizzato del ruolo per cui è necessaria l'approvazione.
Utente supporto tecnico | Il nome utente del richiedente da approvare.
Giustificazione | La giustificazione fornita dall'utente.
RequestedTTL | Ora di scadenza richiesta, in secondi.
RichiestaedTime | Il tempo richiesto per l'elevazione dei privilegi.
CreationTime | Ora di creazione della richiesta.
FIMRichiestaID | Contiene un singolo elemento, "Value", con l'identificatore univoco (GUID) della richiesta PAM.
RichiestaorID | Contiene un singolo elemento, "Value" con l'identificatore univoco (GUID) per l'account di Active Directory che ha creato la richiesta PAM.
ApprovalObjectID | Contiene un singolo elemento "Value" con l'identificatore univoco (GUID) per l’oggetto dell’approvazione.

##<a name="example"></a>Esempio

###<a name="request"></a>Richiesta
```
GET /api/pamresources/pamrequeststoapprove HTTP/1.1
```
###<a name="response"></a>Risposta
```
HTTP/1.1 200 OK

{
    "odata.metadata":"http://localhost:8086/api/pamresources/%24metadata#pamrequeststoapprove",
    "value":[
        {
            "RoleName":"ApprovalRole",
            "Requestor":"PRIV.Jen",
            "Justification":"Justification Reason",
            "RequestedTTL":"3600",
            "RequestedTime":"2015-07-11T22:25:00Z",
            "CreationTime":"2015-07-11T22:24:52.51Z",
            "FIMRequestID":{
                "Value":"9802d7b7-b4e9-4fe4-8f5c-649cda127e49"
            },
            "RequestorID":{
                "Value":"73257e5e-00b3-4309-a330-f1e607ff113a"
            },
            "ApprovalObjectID":{
                "Value":"5dbd9d0c-0a9d-4f75-8cbd-ff6ffdc00143"
            }
        }
    ]
}
```       
