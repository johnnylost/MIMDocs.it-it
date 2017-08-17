---
title: Ottenere modelli di profilo | Microsoft Docs
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
ms.assetid: b7d8ed76-168b-4cb8-b87c-cdb0976c179a
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 8736b328926445f6434bd037a1ecf2e5de9ee466
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/13/2017
---
# <a name="get-profile-templates"></a>Get Profile Templates
Ottiene un elenco di modelli di profilo per i quali l'utente specificato può effettuare la registrazione. Questo metodo restituisce una vista limitata del modello di profilo. I dati del modello di profilo restituiti dovrebbero essere sufficienti a consentire all'utente richiedente decidere a quale modello di profilo, se presente, deve effettuare la registrazione. Se non è specificato alcun flusso di lavoro e alcuna autorizzazione, verranno restituiti tutti i modelli di profilo visibili all'utente.

**Nota**: gli URL mostrati in questo argomento sono relativi al nome host scelto durante la distribuzione delle API. Ad esempio: `https://api.contoso.com`.
##<a name="request"></a>Richiesta


Metodo  |URL della richiesta  
---------|---------
GET     |/CertificateManagement/api/v1.0/profiletemplates?\[targetuser\] 

###<a name="url-parameters"></a>Parametri URL
Parametro| Descrizione
--------|-------------
targetuser| Facoltativa. Specifica l'utente di destinazione per il quale restituire i modelli di profilo. Se non specificato, verrà utilizzata l'identità dell'utente corrente. <br/>**Nota**: attualmente è supportato solo l'utente corrente.

###<a name="request-headers"></a>Intestazioni delle richieste
Vedere l'argomento di servizio per le intestazioni di richiesta comuni
###<a name="request-body"></a>Testo della richiesta
Nessuno

##<a name="response"></a>Risposta
###<a name="response-codes"></a>Codici di risposta
Codice  |Descrizione  
---------|---------
200     | OK
204 | Nessun contenuto
500 | Errore interno

###<a name="response-headers"></a>Intestazioni delle risposte
Vedere l'argomento di servizio per le intestazioni di risposta comuni
###<a name="response-body"></a>Testo della risposta
In caso di esito positivo, restituisce un elenco di oggetti ProfileTemplateLimitedView con le seguenti proprietà.

Proprietà| Tipo| Descrizione
--------|-----|--------
Nome| string| Il nome visualizzato del modello di profilo.
Descrizione| stringa| La descrizione del modello di profilo.
Uuid| GUID| L’identificatore del modello di profilo.
IsSmartcardProfileTemplate| bool| Indica se il modello è un modello di profilo delle smart card.
IsVirtualSmartcardProfileTemplate| bool| Indica se il modello di profilo richiede una smart card virtuale.

##<a name="example"></a>Esempio

###<a name="request"></a>Richiesta
```
GET /certificatemanagement/api/v1.0/profiletemplates HTTP/1.1
```
###<a name="response"></a>Risposta
```
HTTP/1.1 200 OK

[
    {
        "Name":"FIM CM Sample Profile Template",
        "Description":"Description of the template goes here",
        "Uuid":"12bd5120-86a2-4ee1-8d05-131066871578",
        "IsSmartcardProfileTemplate":false,
        "IsVirtualSmartcardProfileTemplate":false
    },
    {
        "Name":"FIM CM Sample Smart Card Logon Profile Template",
        "Description":"Description of the template goes here",
        "Uuid":"2b7044cf-aa96-4911-b886-177947e9271b",
        "IsSmartcardProfileTemplate":true,
        "IsVirtualSmartcardProfileTemplate":false
    }
]

```       
