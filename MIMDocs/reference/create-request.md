---
title: Creare una richiesta | Microsoft Docs
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
ms.assetid: 80fe0656-6fb2-400c-9ef8-5f62b61b2a1b
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: d5af8767583cb6999de399de58b321147846291a
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/13/2017
---
# <a name="create-request"></a>Crea richiesta
Creare una richiesta MIM CM

**Nota**: gli URL mostrati in questo argomento sono relativi al nome host scelto durante la distribuzione delle API. Ad esempio: `https://api.contoso.com`.
##<a name="request"></a>Richiesta


Metodo  |URL della richiesta  
---------|---------
POST     |/CertificateManagement/api/v1.0/requests

###<a name="url-parameters"></a>Parametri URL
Nessuno

###<a name="request-headers"></a>Intestazioni delle richieste
Per le intestazioni di richiesta comuni, vedere [HTTP Request and Response Headers](certificate-management-rest-api-service-details.md#http-request-and-response-headers) (Intestazioni di richiesta e risposta HTTP) in *Dettagli del servizio API REST di gestione certificati*.
###<a name="request-body"></a>Testo della richiesta
Il testo della richiesta contiene le proprietà seguenti.

Proprietà | Descrizione
---------|-----------
profiletemplateuuid | Obbligatorio. Il GUID del modello di profilo per cui l’utente crea la richiesta.
datacollection | Obbligatorio. Una raccolta di coppie nome-valore che rappresenta i dati che devono essere forniti dall'iscritto. La raccolta di dati necessari che devono essere forniti può essere recuperata dal criterio di flusso di lavoro del modello di profilo. È possibile specificare una raccolta vuota.
target | Facoltativo Il GUID dell'utente di destinazione per cui deve essere creata la richiesta. Se non specificato, l'impostazione predefinita corrisponde all'utente corrente.
tipo | Obbligatorio. Il tipo di richiesta che viene creato. I tipi di richiesta disponibili sono: "Enroll", "Duplicate", "OfflineUnblock", "OnlineUpdate", "Renew", "Recover", "RecoverOnBehalf", "Reinstate", "Retire", "Revoke", "TemporaryCards" e "Unblock".<br/>**Nota**: non tutti i tipi di richiesta sono supportati in tutti i modelli di profilo. Ad esempio, non è possibile specificare un'operazione di sblocco su un modello di profilo software.
commento | Obbligatorio. Eventuali commenti che possono essere immessi dall'utente. Il criterio di flusso di lavoro definisce se è necessario. È possibile specificare una stringa vuota.
priorità | Facoltativa. La priorità della richiesta. Se non specificata sarà utilizzata la priorità predefinita della richiesta, determinata dalle impostazioni del modello di profilo.


##<a name="response"></a>Risposta
###<a name="response-codes"></a>Codici di risposta
Codice  |Descrizione  
---------|---------
201     | Creato
403 | Non consentito
500 | Errore interno

###<a name="response-headers"></a>Intestazioni delle risposte
Per le intestazioni di risposta comuni, vedere [HTTP Request and Response Headers](certificate-management-rest-api-service-details.md#http-request-and-response-headers) (Intestazioni di richiesta e risposta HTTP) in *Dettagli del servizio API REST di gestione certificati*.
###<a name="response-body"></a>Testo della risposta
In caso di esito positivo restituisce l'URI per la richiesta appena creata.
##<a name="example"></a>Esempio

###<a name="request-1"></a>Richiesta 1
```
POST /CertificateManagement/api/v1.0/requests HTTP/1.1

{
    "datacollection":"[]",
    "type":"Enroll",
    "profiletemplateuuid":"a039b4d0-5ce8-4eff-8651-181c6576fda3",
    "comment":""
}
```
###<a name="response-1"></a>Risposta 1
```
HTTP/1.1 201 Created

"api/v1.0/requests/a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099"
```
###<a name="request-2"></a>Richiesta 2
```
POST /CertificateManagement/api/v1.0/requests HTTP/1.1

{  
    "datacollection":"[]",
    "type":"Unblock",
    "smartcard":"17cf063d-e337-4aa9-a822-346554ddd3c9",
    "comment":""
}
```
###<a name="response-2"></a>Risposta 2
```
HTTP/1.1 201 Created

"api/v1.0/requests/0c96d73f-967b-420e-854a-43ad2a1504bc"
```       

###<a name="request-3"></a>Richiesta 3
```
POST /CertificateManagement/api/v1.0/requests HTTP/1.1

{
    "profiletemplateuuid" : "97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1",
    "datacollection":
    [
        {"name" : "pavle"},
        {"city" : "seattle"}
    ],
    "target" : "97CC3493-F556-4C9B-9D8B-982434201527",
    "type" : "Enroll",
    "comment" : "LALALALA",
    "priority" :  "4"
}
```
##<a name="see-also"></a>Vedere anche

- [Metodo Microsoft.Clm.Provision.RequestOperations.InitiateEnroll](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.provision.requestoperations.initiateenroll.aspx)
- [Metodo Microsoft.Clm.Provision.RequestOperations.InitiateOfflineUnblock](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.provision.requestoperations.initiateofflineunblock.aspx)
- [Metodo Microsoft.Clm.Provision.RequestOperations.InitiateRecover](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.provision.requestoperations.initiaterecover.aspx)
- [Metodo Microsoft.Clm.Provision.RequestOperations.InitiateRetire](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.provision.requestoperations.initiateretire.aspx)
- [Metodo Microsoft.Clm.Provision.RequestOperations.InitiateUnblock](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.provision.requestoperations.initiateunblock.aspx)
