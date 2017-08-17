---
title: Ottenere informazioni sulla sessione PAM | Microsoft Docs
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
ms.assetid: bc30e455-9a9c-413a-b8ca-9669286299cf
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 150bc7e863fc679e785526935155611fb75cd69b
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/13/2017
---
# <a name="get-pam-session-info"></a>Ottenere informazioni di sessione PAM
Utilizzato da un account con privilegi per ottenere il nome utente dell'account che ha effettuato l’accesso alla sessione.

**Nota**: gli URL mostrati in questo argomento sono relativi al nome host scelto durante la distribuzione delle API. Ad esempio: `http://api.contoso.com`.
##<a name="request"></a>Richiesta


Metodo  |URL della richiesta  
---------|---------
GET     |/api/session/sessioninfo

###<a name="query-parameters"></a>Parametri di query
Parametro | Descrizione
----------|--------------
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
Una risposta corretta contiene un oggetto di sessione PAM con le seguenti proprietà.

Proprietà| Descrizione
--------|-------------
Nome utente| Il nome utente dell'account che viene registrato in questa sessione.

##<a name="example"></a>Esempio

###<a name="request"></a>Richiesta
```
GET /api/session/sessioninfo/ HTTP/1.1
```
###<a name="response"></a>Risposta
```
HTTP/1.1 200 OK

{
    "odata.metadata":"http://localhost:8086/api/pamresources/%24metadata#sessioninfo",
    "value":[
        {
            "Username":"FIMCITONEBOXAD\\Jen"
        }
    ]
}
```       
