---
title: Recuperare richieste PAM | Microsoft Docs
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
ms.assetid: 620eebd6-e4c3-473b-b824-ee6cfe83e509
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 00467b6443d3e6e0511ee1817a945ffe569b99b0
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/13/2017
---
# <a name="get-pam-requests"></a>Recuperare richieste PAM
Utilizzato da un account con privilegi per restituire una cronologia delle PAM registrate in precedenza.

**Nota**: gli URL mostrati in questo argomento sono relativi al nome host scelto durante la distribuzione delle API. Ad esempio: `http://api.contoso.com`.
##<a name="request"></a>Richiesta


Metodo  |URL della richiesta  
---------|---------
GET     |/api/pamresources/pamrequests

###<a name="query-parameters"></a>Parametri di query
Parametro | Descrizione
----------|--------------
$filter | Facoltativa. Specificare tutte le proprietà delle richieste PAM in un’espressione di filtro per restituire un elenco filtrato di risposte. Per altre informazioni sugli operatori supportati, vedere [Filtering (Filtro) in Dettagli del servizio API REST di PAM](privileged-access-management-rest-api-service-details.md#filtering)
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
Una risposta corretta contiene un elenco di oggetti di richiesta PAM con le seguenti proprietà.

Proprietà | Descrizione
--------|-------------
IDRichiesta | Identificatore univoco (GUID) per la richiesta PAM.
CreatorID | Un identificatore univoco (GUID) per l'account di Active Directory che ha creato la richiesta PAM.
Giustificazione | Il motivo per l'elevazione dei privilegi.
DisplayName | Nome visualizzato della richiesta PAM in MIM.
CreationTime | Ora di creazione della richiesta.
CreationMetodo | Il metodo utilizzato per creare la richiesta.
ExpirationTime | Ora di scadenza della richiesta.
RoleID| Identificatore univoco (GUID) del ruolo PAM.
RichiestaedTTL | Timeout di scadenza richiesta, in secondi.
RequestedTime | Il tempo richiesto per l'elevazione dei privilegi.
RichiestaedStatus | Lo stato della richiesta. Valori possibili: "Active", "Closed", "Closing", "Expired", "Pending Approval" e "Rejected".

##<a name="example"></a>Esempio

###<a name="request"></a>Richiesta
```
GET /api/pamresources/pamrequests?$filter=CreationTime%20gt%20datetime'2015-06-12T04:49:32.431Z'%20and%20CreationTime%20lt%20datetime'2015-07-13T04:49:32.432Z' HTTP/1.1
```

###<a name="response"></a>Risposta
```
HTTP/1.1 200 OK

{
    "odata.metadata":"http://localhost:8086/api/pamresources/%24metadata#pamrequests",
    "value":[
        {
            "RequestId":"b22e1343-9a2b-4e33-a70a-1bb7b2d405b9",
            "CreatorID":"73257e5e-00b3-4309-a330-f1e607ff113a",
            "Justification":null,
            "CreationTime":"2015-06-23T11:34:38.58Z",
            "CreationMethod":"PAM Web API",
            "ExpirationTime":"2015-06-23T12:34:38.847Z",
            "RoleId":"8f5cec1a-ecba-42ec-b76d-e6e0e4bf4c62",
            "RequestedTTL":"3600",
            "RequestedTime":"2015-06-23T11:34:36.417Z",
            "RequestStatus":"Expired"
        },
        {
            "RequestId":"3a98d1c7-524d-4b72-9da7-bd9f907eab55",
            "CreatorID":"73257e5e-00b3-4309-a330-f1e607ff113a",
            "Justification":"Reason for Request",
            "CreationTime":"2015-07-12T04:35:14.433Z",
            "CreationMethod":"PAM Web API",
            "ExpirationTime":"2015-07-12T04:43:51.95Z",
            "RoleId":"8f5cec1a-ecba-42ec-b76d-e6e0e4bf4c62",
            "RequestedTTL":"12960000",
            "RequestedTime":"2015-07-12T04:35:00Z",
            "RequestStatus":"Closed"
        },
        {
            "RequestId":"f5e80be1-e9a3-42c4-81f8-4be5a4a429f4",
            "CreatorID":"73257e5e-00b3-4309-a330-f1e607ff113a",
            "Justification":null,
            "CreationTime":"2015-07-12T04:48:17.46Z",
            "CreationMethod":"PAM Web API",
            "ExpirationTime":"2015-07-12T05:48:17.853Z",
            "RoleId":"8f5cec1a-ecba-42ec-b76d-e6e0e4bf4c62",
            "RequestedTTL":"3600",
            "RequestedTime":"2015-07-12T04:48:14.057Z",
            "RequestStatus":"Active"
        },
        {
            "RequestId":"b0f0ddc0-c809-4770-9d39-0d706f97a2de",
            "CreatorID":"73257e5e-00b3-4309-a330-f1e607ff113a",
            "Justification":"",
            "CreationTime":"2015-06-30T07:01:13.147Z",
            "CreationMethod":"PAM Web API",
            "ExpirationTime":"0001-01-01T00:00:00",
            "RoleId":"c28eab4a-95cf-4c08-a153-d5e8a9e660cd",
            "RequestedTTL":"3600",
            "RequestedTime":"2015-06-30T07:01:13.119Z",
            "RequestStatus":"Rejected"
        },
        {
            "RequestId":"5ec10e61-cdd1-404e-a18e-740467d87dbf",
            "CreatorID":"73257e5e-00b3-4309-a330-f1e607ff113a",
            "Justification":"Example Reason",
            "CreationTime":"2015-07-12T04:49:09.963Z",
            "CreationMethod":"PAM Web API",
            "ExpirationTime":"0001-01-01T00:00:00",
            "RoleId":"c28eab4a-95cf-4c08-a153-d5e8a9e660cd",
            "RequestedTTL":"12960000",
            "RequestedTime":"2015-07-12T04:50:00Z",
            "RequestStatus":"PendingApproval"
        }
    ]
}
```       
