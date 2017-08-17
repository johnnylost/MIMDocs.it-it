---
title: Creare una richiesta PAM | Microsoft Docs
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
ms.assetid: fe8b3374-9d32-4cc3-9328-f1eafeadfe8e
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: de28af5c49eb98c5a1ccfbd8ed9353cf202e9e66
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/13/2017
---
# <a name="create-pam-request"></a>Creare una richiesta PAM
Utilizzato da un account con privilegi per elevare a un ruolo PAM.

**Nota**: gli URL mostrati in questo argomento sono relativi al nome host scelto durante la distribuzione delle API. Ad esempio: `http://api.contoso.com`.
##<a name="request"></a>Richiesta


Metodo  |URL della richiesta  
---------|---------
POST     |/api/pamresources/pamrequests

###<a name="query-parameters"></a>Parametri di query
Parametro | Descrizione
--------|-------------
Giustificazione | Facoltativa. Il motivo specificato dall'utente per la richiesta di elevazione dei privilegi.
RoleId| Obbligatorio. Identificatore univoco (GUID) del ruolo PAM da elevare a.
RequestedTTL| Obbligatorio. Ora di scadenza richiesta, in secondi.
RichiestaedTime | Facoltativo. Tempo necessario ad elevare i privilegi.  
v | Facoltativa. La versione delle API. Se omesso, verrà utilizzata la versione corrente (rilasciata più di recente) delle API. Per altre informazioni, vedere [Versioning (Controllo versioni) in Dettagli del servizio API REST di PAM](privileged-access-management-rest-api-service-details.md#versioning)

**Nota**: è possibile specificare i parametri *Justification*, *RoleId*, *RequestedTTL*e *RequestedTime* come proprietà del testo della richiesta, anziché come parametri di query. Il parametro *v* può essere specificato solo come un parametro di query.

###<a name="request-headers"></a>Intestazioni delle richieste
Per le intestazioni di richiesta comuni, vedere [HTTP Request and Response Headers](privileged-access-management-rest-api-service-details.md#http-request-and-response-headers) (Intestazioni di richiesta e risposta HTTP) in *Dettagli del servizio API REST di PAM*.
###<a name="request-body"></a>Testo della richiesta
Facoltativa. Come notato in precedenza, i parametri *Justification*, *RoleId*, *RequestedTTL*e *RequestedTime* possono essere specificati come proprietà di un testo della richiesta invece di specificarli nella stringa di query dell'URL.

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
Una risposta corretta contiene un oggetto di richiesta PAM con le seguenti proprietà.

Proprietà | Descrizione
--------|-------------
IDRichiesta | Identificatore univoco (GUID) per la richiesta PAM.
CreatorID | Identificatore univoco (GUID) nel servizio MIM per l'account che ha creato la richiesta.
Giustificazione | Il motivo per l'elevazione dei privilegi.
CreationTime | Ora di creazione della richiesta.
CreationMetodo | Il metodo utilizzato per creare la richiesta.
ExpirationTime | Ora di scadenza della richiesta.
RoleID| Identificatore univoco (GUID) del ruolo PAM.
RichiestaedTTL | Timeout di scadenza richiesta, in secondi.
RequestedTime | Il tempo richiesto per l'elevazione dei privilegi.
RequestStatus | Lo stato della richiesta. Valori possibili: "Processing", "Active", "Closed", "Closing", "Expired", "PendingApproval", "PendingMFA" e "Rejected".

##<a name="example"></a>Esempio

###<a name="request-1"></a>Richiesta 1
```
POST /api/pamresources/pamrequests?Justification=Sample+Reason&RoleId=c28eab4a-95cf-4c08-a153-d5e8a9e660cd&RequestedTTL=7200&RequestedTime=2015%2F07%2F11+23%3A40 HTTP/1.1
```
###<a name="response-1"></a>Risposta 1
```
HTTP/1.1 201 Created

{  
    "odata.metadata":"http://localhost:8086/api/pamresources/%24metadata#pamrequests/@Element",
    "RequestId":"c0112f13-b16b-40ad-b547-07f23a7fba52",
    "CreatorID":"73257e5e-00b3-4309-a330-f1e607ff113a",
    "Justification":"Sample Reason",
    "CreationTime":"2015-07-11T23:38:09.036164-07:00",
    "CreationMethod":"PAM Web API",
    "ExpirationTime":"0001-01-01T00:00:00",
    "RoleId":"c28eab4a-95cf-4c08-a153-d5e8a9e660cd",
    "RequestedTTL":"7200",
    "RequestedTime":"2015-07-12T06:40:00Z",
    "RequestStatus":"PendingApproval"
}
```       

###<a name="request-2"></a>Richiesta 2
```
POST /api/pamresources/pamrequests?Justification=&RoleId=c28eab4a-95cf-4c08-a153-d5e8a9e660cd&RequestedTTL=3600&RequestedTime= HTTP/1.1
```
###<a name="response-2"></a>Risposta 2
```
HTTP/1.1 201 Created

{
    "odata.metadata":"http://localhost:8086/api/pamresources/%24metadata#pamrequests/@Element",
    "RequestId":"504f9c49-00db-42bd-a157-ee5664617189",
    "CreatorID":"73257e5e-00b3-4309-a330-f1e607ff113a",
    "Justification":null,
    "CreationTime":"2015-07-11T23:07:30.2200123-07:00",
    "CreationMethod":"PAM Web API",
    "ExpirationTime":"0001-01-01T00:00:00",
    "RoleId":"c28eab4a-95cf-4c08-a153-d5e8a9e660cd",
    "RequestedTTL":"3600",
    "RequestedTime":"2015-07-12T06:07:27.7229894Z",
    "RequestStatus":"PendingApproval"
}
```       
