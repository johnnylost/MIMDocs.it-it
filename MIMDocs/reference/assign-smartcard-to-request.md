---
title: Assegnare una smart card a una richiesta | Microsoft Docs
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
ms.assetid: 20f0bf6e-9ae0-4d21-8117-ed63e29315e6
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 2886186ade7058b034565501e200ab3773b0af31
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/13/2017
---
# <a name="assign-smart-card-to-a-request"></a>Assegnare una smart card a una richiesta
Associa la smart card specificata alla richiesta specificata. Una volta associata, la richiesta può essere eseguita solo con questa scheda.

**Nota**: gli URL mostrati in questo argomento sono relativi al nome host scelto durante la distribuzione delle API. Ad esempio: `https://api.contoso.com`.
##<a name="request"></a>Richiesta


Metodo  |URL della richiesta  
---------|---------
POST     |/CertificateManagement/api/v1.0/smartcards

###<a name="url-parameters"></a>Parametri URL
Nessuno
###<a name="request-headers"></a>Intestazioni delle richieste
Per le intestazioni di richiesta comuni, vedere [HTTP Request and Response Headers](certificate-management-rest-api-service-details.md#http-request-and-response-headers) (Intestazioni di richiesta e risposta HTTP) in *Dettagli del servizio API REST di gestione certificati*.
###<a name="request-body"></a>Testo della richiesta
Il testo della richiesta contiene le proprietà seguenti.

Proprietà | Descrizione
---------|-----------
IDRichiesta | ID della richiesta a cui la smart card deve essere associata.
cardID | Elemento cardid della smart card.
atr | Stringa ATR (Answer-To-Reset) della smart card.


##<a name="response"></a>Risposta
###<a name="response-codes"></a>Codici di risposta
Codice  |Descrizione  
---------|---------
201     | Creato
204 | Nessun contenuto
403 | Non consentito
500 | Errore interno

###<a name="response-headers"></a>Intestazioni delle risposte
Per le intestazioni di risposta comuni, vedere [HTTP Request and Response Headers](certificate-management-rest-api-service-details.md#http-request-and-response-headers) (Intestazioni di richiesta e risposta HTTP) in *Dettagli del servizio API REST di gestione certificati*.
###<a name="response-body"></a>Testo della risposta
In caso di esito positivo, restituisce un URI per l'oggetto smart card appena creato.
##<a name="example"></a>Esempio

###<a name="request"></a>Richiesta
```
POST /CertificateManagement/api/v1.0/smartcards HTTP/1.1

{
    "requestid":"a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099",
    "cardid":"bc88f13f-83ba-4037-8262-46eba1291c6e",
    "atr":"3b8d0180fba000000397425446590301c8"
}

```
###<a name="response"></a>Risposta
```
HTTP/1.1 201 Created

"api/v1.0/smartcards/17cf063d-e337-4aa9-a822-346554ddd3c9"
```       
##<a name="see-also"></a>Vedere anche

- [Metodo Microsoft.Clm.Provision.RequestOperations.CreateSmartcard (stringa, stringa, richiesta)](https://msdn.microsoft.com/library/windows/desktop/bb456812.aspx)
