---
title: Ottenere i dati del profilo | Microsoft Docs
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
ms.assetid: 3eba062b-7adf-4766-9b94-cba1c7be2fd3
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 068497509b07b879fcadde6b60a9530d3d8db093
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/13/2017
---
# <a name="get-profile-data"></a>Ottieni dati profilo
Ottiene un elenco di profili di certificato software di un utente con un elenco di operazioni possibili che possono essere eseguite dall'utente corrente. È quindi possibile avviare una richiesta per una delle operazioni specificate.

**Nota**: il server imposterà il PIN solo se i criteri dei modelli di profilo indicano che sarà necessario eseguire questa operazione. In caso contrario, l'utente dovrà provvedere.

**Nota**: gli URL mostrati in questo argomento sono relativi al nome host scelto durante la distribuzione delle API. Ad esempio: `https://api.contoso.com`.
##<a name="request"></a>Richiesta


Metodo  |URL della richiesta  
---------|---------
GET     |/CertificateManagement/api/v1.0/profiles<br/>/CertificateManagement/api/v1.0/profiles/{id} <br/>/CertificateManagement/api/v1.0/requests/{requestid}/profiles

###<a name="url-parameters"></a>Parametri URL
Parametro | Descrizione
---------|------------
ID | L'identificatore (GUID) del profilo da restituire.
IDRichiesta | Identificatore della richiesta per cui restituire i profili.

###<a name="query-parameters"></a>Parametri di query
Parametro | Descrizione
---------|------------
stato | Facoltativa. Indica lo stato dei profili per cui recuperare i dati. Tipi di stato possibili sono: "Active", "Approved", "Canceled", "Completed", "Denied", "Executing", "Failed", "None" e "Pending". <br/>Se non è specificato alcuno stato, verranno restituiti tutti i profili, indipendentemente dallo stato.
###<a name="request-headers"></a>Intestazioni delle richieste
Per le intestazioni di richiesta comuni, vedere [HTTP Request and Response Headers](certificate-management-rest-api-service-details.md#http-request-and-response-headers) (Intestazioni di richiesta e risposta HTTP) in *Dettagli del servizio API REST di gestione certificati*.
###<a name="request-body"></a>Testo della richiesta
Nessuno

##<a name="response"></a>Risposta
###<a name="response-codes"></a>Codici di risposta
Codice  |Descrizione  
---------|---------
200 | OK
204 | Nessun contenuto
403 | Non consentito
500 | Errore interno

###<a name="response-headers"></a>Intestazioni delle risposte
Per le intestazioni di risposta comuni, vedere [HTTP Request and Response Headers](certificate-management-rest-api-service-details.md#http-request-and-response-headers) (Intestazioni di richiesta e risposta HTTP) in *Dettagli del servizio API REST di gestione certificati*.
###<a name="response-body"></a>Testo della risposta
In caso di esito positivo, restituisce un elenco di oggetti serializzati JSON [Microsoft.Clm.Shared.Profiles.Profile](https://msdn.microsoft.com/library/microsoft.clm.shared.profiles.profile.aspx) con le proprietà seguenti:

Proprietà | Descrizione
---------|------------
AssignedUserUuID | Identificatore dell'utente a cui viene assegnato il profilo.
Commento | Il commento che descrive il profilo.
Flag | I flag che descrivono il profilo.
ParentProfileUuID | Identificatore del profilo precedente sostituito dall’attuale profilo.
PrimaryProfileUuID | Identificatore del profilo primario.
ProfileOperations | Elenco di possibili operazioni che possono essere eseguite dall'utente corrente per il profilo specificato.
ProfileTemplateUuID | Identificatore del modello di profilo che contiene criteri e impostazioni che controllano il profilo.
ProfileTemplateVersion | La versione del modello di profilo al momento della creazione del profilo.
Stato | Lo stato del profilo.
UuID | Identificatore del profilo.


##<a name="example"></a>Esempio

###<a name="request"></a>Richiesta
```
GET /certificatemanagement/api/v1.0/profiles?status=Active HTTP/1.1
```
###<a name="response"></a>Risposta
```
HTTP/1.1 200 OK

[
    {
        "Uuid":"c0dd5c7d-ec35-4346-baca-3ad711e9722f",
        "Status":2,
        "Flags":1,
        "ParentProfileUuid":"1c9e2606-fea2-4048-a6ac-b014e54c22df",
        "PrimaryProfileUuid":"00000000-0000-0000-0000-000000000000",
        "AssignedUserUuid":"0378a33b-8650-4713-a727-efd233903643",
        "ProfileTemplateUuid":"8f31803f-8afc-49bb-911d-402ec264b589",
        "ProfileTemplateVersion":8,
        "Comment":"",
        "ProfileOperations":[
            "renew",
            "disable",
            "recover"
        ]
    },
    {
        "Uuid":"5ad77b40-aa42-4533-9396-c9c59fd021a8",
        "Status":2,
        "Flags":1,
        "ParentProfileUuid":"00000000-0000-0000-0000-000000000000",
        "PrimaryProfileUuid":"00000000-0000-0000-0000-000000000000",
        "AssignedUserUuid":"0378a33b-8650-4713-a727-efd233903643",
        "ProfileTemplateUuid":"8f31803f-8afc-49bb-911d-402ec264b589",
        "ProfileTemplateVersion":8,
        "Comment":"",
        "ProfileOperations":[
            "renew",
            "disable",
            "recover"
        ]
    },
    {
        "Uuid":"ff342953-c444-4dc7-b144-f5515d6460c6",
        "Status":2,
        "Flags":1,
        "ParentProfileUuid":"00000000-0000-0000-0000-000000000000",
        "PrimaryProfileUuid":"00000000-0000-0000-0000-000000000000",
        "AssignedUserUuid":"0378a33b-8650-4713-a727-efd233903643",
        "ProfileTemplateUuid":"1e3a31fe-699b-4a6b-945c-18b83c985bc1",
        "ProfileTemplateVersion":9,
        "Comment":"",
        "ProfileOperations":[
            "renew",
            "disable"
        ]
    }
]
```       
##<a name="see-also"></a>Vedere anche

- [Classe Microsoft.Clm.Shared.Profiles.Profile](https://msdn.microsoft.com/library/microsoft.clm.shared.profiles.profile.aspx)
