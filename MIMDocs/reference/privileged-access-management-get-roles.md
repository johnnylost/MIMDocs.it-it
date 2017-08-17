---
title: Ottenere ruoli PAM | Microsoft Docs
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
ms.assetid: d3c4f528-c3c8-41c1-905e-7eb84f074ce4
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 4cb4bbc6c7696f354e5759a677ece88a4e606544
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/13/2017
---
# <a name="get-pam-roles"></a>Ottenere ruoli PAM
Utilizzato da un account con privilegi per elencare i ruoli PAM per i quali l'account è candidato.

**Nota**: gli URL mostrati in questo argomento sono relativi al nome host scelto durante la distribuzione delle API. Ad esempio: `http://api.contoso.com`.
##<a name="request"></a>Richiesta


Metodo  |URL della richiesta  
---------|---------
GET     |/api/pamresources/pamroles

###<a name="query-parameters"></a>Parametri di query
Parametro | Descrizione
----------|--------------
$filter | Facoltativa. Specifica tutte le proprietà del ruolo PAM in un’espressione di filtro per restituire un elenco filtrato di risposte. Per altre informazioni sugli operatori supportati, vedere [Filtering (Filtro) in Dettagli del servizio API REST di PAM](privileged-access-management-rest-api-service-details.md#filtering)
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
Una risposta corretta contiene una raccolta di uno o più ruoli PAM, ognuno dei quali presenta le seguenti proprietà.

Proprietà | Descrizione
--------|-------------
RoleID | Identificatore univoco (GUID) del ruolo PAM.
DisplayName | Nome visualizzato del ruolo PAM nel servizio MIM.
Descrizione | Descrizione del ruolo PAM nel servizio MIM.
TTL | Il timeout di scadenza massimo in secondi dei diritti di accesso del ruolo.
AvailableFrom | La prima ora del giorno in cui verrà attivata una richiesta.
AvailableTo | L’ultima ora del giorno in cui verrà attivata una richiesta.
MFAEnabled | Valore booleano che indica se le richieste di attivazione per questo ruolo richiedono una richiesta di autenticazione a più fattori.
ApprovalEnabled | Valore booleano che indica se le richieste di attivazione per questo ruolo richiedono l’approvazione da parte del proprietario di un ruolo.
AvailibitlyWindowEnabled | Valore booleano che indica se il ruolo può essere attivato solo durante un intervallo di tempo specificato.

##<a name="example"></a>Esempio

###<a name="request"></a>Richiesta
```
GET /api/pamresources/pamroles HTTP/1.1
```
###<a name="response"></a>Risposta
```
HTTP/1.1 200 OK

{
    "odata.metadata":"http://localhost:8086/api/pamresources/%24metadata#pamroles",
    "value":[
        {
            "RoleId":"8f5cec1a-ecba-42ec-b76d-e6e0e4bf4c62",
            "DisplayName":"Allow AD Access ",
            "Description":null,
            "TTL":"3600",
            "AvailableFrom":"0001-01-01T00:00:00",
            "AvailableTo":"0001-01-01T00:00:00",
            "MFAEnabled":false,
            "ApprovalEnabled":false,
            "AvailabilityWindowEnabled":false
        },
        {
            "RoleId":"c28eab4a-95cf-4c08-a153-d5e8a9e660cd",
            "DisplayName":"ApprovalRole",
            "Description":null,
            "TTL":"3600",
            "AvailableFrom":"0001-01-01T00:00:00",
            "AvailableTo":"0001-01-01T00:00:00",
            "MFAEnabled":false,
            "ApprovalEnabled":true,
            "AvailabilityWindowEnabled":false
        }
    ]
}
```       
