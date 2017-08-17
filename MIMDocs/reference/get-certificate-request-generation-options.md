---
title: Ottenere le opzioni di generazione di richiesta di certificato | Microsoft Docs
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
ms.assetid: 36bd1fc9-3443-4028-90e7-a24fef0ec0ae
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 5822da1b9cf8558becccff815fd0208923fcaaa0
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/13/2017
---
# <a name="get-certificate-request-generation-options"></a>Ottenere le opzioni di generazione di richiesta di certificato

Ottiene i parametri per la generazione di una richiesta certificato lato client.

**Nota**: gli URL mostrati in questo argomento sono relativi al nome host scelto durante la distribuzione delle API. Ad esempio: `https://api.contoso.com`.
##<a name="request"></a>Richiesta


Metodo  |URL della richiesta  
---------|---------
GET     |/CertificateManagement/api/v1.0/requests/{IDRichiesta}/certificaterequestgenerationoptions

###<a name="url-parameters"></a>Parametri URL
Parametro | Descrizione
--------|--------------
IDRichiesta| Obbligatorio. Identificatore GUID della richiesta CIM MIM per cui i parametri di generazione di richiesta di certificato devono essere recuperati.

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
In caso di esito positivo, restituisce l'elenco di oggetti CertificateRequestGenerationOptions. Ogni oggetto CertificateRequestGenerationOptions corrisponde a una richiesta di certificato singolo che il client deve generare e che presenta le seguenti proprietà:

Proprietà| Descrizione
--------|-----------
Exportable | Un valore che specifica se la chiave privata creata per la richiesta può essere esportata.
FriendlyName | Il nome visualizzato del certificato registrato.
HashAlgorithmName | L'algoritmo hash utilizzato durante la creazione della firma della richiesta di certificato.
KeyAlgorithmName | L'algoritmo a chiave pubblica.
KeyProtectionLevel | Il livello di protezione avanzata della chiave.
KeySize | La dimensione in bit della chiave privata da generare.
KeyStorageProviderNames | Un elenco di provider accettabile di archiviazione chiavi (KSP) che può essere utilizzato per generare la chiave privata. Nel caso il primo KSP non possa essere utilizzato per generare la richiesta di certificato, è possibile specificare qualsiasi KSP fino a quando non ha esito positivo.
KeyUsages | Operazione che può essere eseguita mediante la chiave privata creata per la richiesta di certificato. Il valore predefinito è Signing.
Subject | Il nome del soggetto.

**Nota**: sono disponibili altre informazioni su queste proprietà nella descrizione della [classe Windows.Security.Cryptography.Certificates.CertificateRequestProperties](https://msdn.microsoft.com/library/windows/apps/br212079.aspx), tuttavia occorre fare attenzione che non sia presente una corrispondenza uno a uno tra questa classe e oggetti CertificateRequestGenerationOptions.

##<a name="example"></a>Esempio

###<a name="request"></a>Richiesta
```
GET /certificatemanagement/api/v1.0/requests/a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099/certificaterequestgenerationoptions HTTP/1.1

```
###<a name="response"></a>Risposta
```
HTTP/1.1 200 OK

[
    {
        "Subject":"",
        "KeyAlgorithmName":"RSA",
        "KeySize":2048,
        "FriendlyName":"",
        "HashAlgorithmName":"SHA1",
        "KeyStorageProviderNames":[
            "Contoso Smart Card Key Storage Provider"
        ],
        "Exportable":0,
        "KeyProtectionLevel":0,
        "KeyUsages":3
    }
]
```       
