---
title: Ottenere il PIN proposto della smart card | Microsoft Docs
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
ms.assetid: ced93932-9912-4b32-9586-ada69b38a796
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 08d28819402dd56f996d39aa03b8ac829bef0838
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/13/2017
---
# <a name="get-smartcard-proposed-pin"></a>Ottieni PIN smart card proposto
Ottiene il PIN utente generato dal server

**Nota**: il server imposterà il PIN solo se i criteri dei modelli di profilo indicano che sarà necessario eseguire questa operazione. In caso contrario, l'utente dovrà provvedere.

**Nota**: gli URL mostrati in questo argomento sono relativi al nome host scelto durante la distribuzione delle API. Ad esempio: `https://api.contoso.com`.
##<a name="request"></a>Richiesta


Metodo  |URL della richiesta  
---------|---------
GET     |/CertificateManagement/api/v1.0/smartcards/{ID}/serverproposedpin

###<a name="url-parameters"></a>Parametri URL
Parametro | Descrizione
---------|------------
ID | Identificatore smart card (specifico di CM MIM). Ottenuto dall'oggetto Microsft.Clm.Shared.Smartcard.
###<a name="query-parameters"></a>Parametri di query
Parametro | Descrizione
---------|------------
atr | atr della scheda.
cardID | ID della scheda.
challenge | Stringa con codifica base 64 che rappresenta la richiesta di verifica emessa dalla smart card .

###<a name="request-headers"></a>Intestazioni delle richieste
Per le intestazioni di richiesta comuni, vedere [HTTP Request and Response Headers](certificate-management-rest-api-service-details.md#http-request-and-response-headers) (Intestazioni di richiesta e risposta HTTP) in *Dettagli del servizio API REST di gestione certificati*.
###<a name="request-body"></a>Testo della richiesta
Nessuno

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
In caso di esito positivo, restituisce una stringa che rappresenta il PIN proposto dal server.

##<a name="example"></a>Esempio

###<a name="request"></a>Richiesta
```
GET GET /CertificateManagement/api/v1.0/smartcards/C6BAD97C-F97F-4920-8947-BE980C98C6B5/serverproposedpin HTTP/1.1
```
###<a name="response"></a>Risposta
```
HTTP/1.1 200 OK

... body coming soon
```       
