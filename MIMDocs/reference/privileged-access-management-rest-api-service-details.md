---
title: Dettagli del servizio API REST di PAM | Microsoft Docs
description: 
keywords: 
author: msmbaldwin
ms.author: mbaldwin
manager: mbaldwin
ms.date: 10/17/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 54c78bbd-8da1-42ff-9edc-47d913011941
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 6e248ba4a65e75b1e914c61de70072c7e094123a
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/13/2017
---
# <a name="pam-rest-api-service-details"></a>Dettagli servizio API REST di gestione certificati
Nelle sezioni seguenti vengono illustrati i dettagli delle API REST di gestione accessi con privilegi (PAM) di Microsoft Identity Manager (MIM).

## <a name="http-request-and-response-headers"></a>Intestazioni di richiesta e risposta HTTP

Le richieste HTTP inviate all'API devono includere le intestazioni seguenti (questo elenco non è completo):

Intestazione | Descrizione
-------|------------
Autorizzazione | Obbligatorio. Il contenuto dipenderà dal metodo di autenticazione, che è configurabile e può essere basato sull’Autenticazione integrata di Windows o su ADFS.
Content-Type | Obbligatorio se la richiesta ha un corpo. Deve essere "application/json".
Content-Length | Obbligatorio se la richiesta ha un corpo. 
Cookie | Il cookie di sessione. Potrebbero essere necessari a seconda del metodo di autenticazione.
<br/>
Le risposte HTTP inviate includeranno le intestazioni seguenti (questo elenco non è completo):

Header | Descrizione
-------|------------
Content-Type | L'API restituisce sempre "application/json".
Content-Length | La lunghezza del corpo della richiesta, se presente, indicato in byte.

## <a name="versioning"></a>Controllo versioni 
La versione corrente delle API è 1. La versione dell'API può essere specificata mediante un parametro di query nell'URL della richiesta, come nell'esempio seguente: `http://localhost:8086/api/pamresources/pamrequests?v=1` se la versione non è specificata nella richiesta, la richiesta viene eseguita in base alla versione rilasciata più di recente dell'API. 

## <a name="security"></a>Sicurezza 
L'accesso all'API richiede Autenticazione integrata di Windows (IWA). Questa funzionalità deve essere configurata manualmente in IIS prima dell'installazione di Microsoft Identity Manager (MIM).

HTTPS (TLS) è supportato, ma deve essere configurato manualmente in IIS. Per informazioni, vedere: **Implement Secure Sockets Layer (SSL) for the FIM Portal** in [Step 9: Perform FIM 2010 R2 Post-Installation Tasks](https://technet.microsoft.com/library/hh322875(v=ws.10%29.aspx) nella Guida per l'installazione di FIM 2010 R2 in un laboratorio di testing. 

È possibile generare un nuovo certificato server SSL eseguendo il comando seguente nel prompt dei comandi di Visual Studio:
```
Makecert -r -pe -n CN="test.cwap.com" -b 05/10/2014 -e 12/22/2048 -eku 1.3.6.1.5.5.7.3.1 -ss my -sr localmachine -sky exchange -sp "Microsoft RSA SChannel Cryptographic Provider" -sy 12
```
 
Il comando crea un certificato autofirmato che può essere utilizzato per testare un'applicazione Web che utilizza SSL in un server Web il cui URL è test.cwap.com. L'OID definito dall'opzione - eku identifica il certificato come certificato server SSL. Il certificato viene archiviato nell'archivio personale ed è disponibile a livello di computer. pertanto, è possibile esportarlo dallo snap-in Certificati in mmc.exe

## <a name="cross-domain-access-cors"></a>Accesso tra domini diversi (CORS) 
L'accesso tra domini diversi è supportato, ma deve essere configurato manualmente in IIS. Aggiungere i seguenti elementi al file web.config delle API distribuita per configurare l'API affinché consenta le chiamate tra domini: 

```
<system.webServer>       
    <httpProtocol> 
        <customHeaders> 
            <add name="Access-Control-Allow-Credentials" value="true"  /> 
            <add name="Access-Control-Allow-Headers" value="content-type" /> 
            <add name="Access-Control-Allow-Origin" value="http://<hostname>:8090" /> 
        </customHeaders> 
    </httpProtocol> 
</system.webServer> 
```
<br/>

## <a name="error-handling"></a>Gestione degli errori 
L'API restituisce le risposte agli errori HTTP per indicare le condizioni di errore. Gli errori sono conformi a OData. Nella tabella seguente sono riportati i codici di errore che possono essere restituiti a un client.

Codice di stato HTTP | Descrizione
-----------------|------------
401 | Non autorizzato 
403 | Non consentito 
408 | Timeout della richiesta   
500 | Errore interno del server 
503 | Servizio non disponibile 
<br/>

## <a name="filtering"></a>Filtro 
Le richieste API REST PAM possono includere filtri per specificare le proprietà che devono essere incluse nella risposta. La sintassi di filtro è basata su espressioni OData.

I filtri possono specificare le proprietà delle richieste PAM, dei ruoli PAM o delle richieste PAM in sospeso. Ad esempio: *ExpirationTime*, *DisplayName*o qualsiasi altra proprietà valida di una richiesta PAM, di un ruolo PAM o di una richiesta in sospeso.

L'API supporta i seguenti operatori nelle espressioni di filtro: *And*, *Equal*, *NotEqual*, *GreaterThan*, *LessThan*, *GreaterThenOrEqueal*e *LessThanOrEqual*. 

Le richieste di esempio seguenti includono filtri:

- La richiesta restituisce tutte le richieste PAM comprese tra date specifiche: `http://localhost:8086/api/pamresources/pamrequests?$filter=ExpirationTime gt datetime"2015-01-09T08:26:49.721Z" and ExpirationTime lt datetime"2015-02-10T08:26:49.722Z" `
 
- Questa richiesta restituisce il ruolo PAM con il nome visualizzato "Accesso File SQL": `http://localhost:8086/api/pamresources/pamroles?$filter=DisplayName eq "SQL File Access" `
