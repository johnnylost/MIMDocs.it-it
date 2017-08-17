---
title: Dettagli del servizio API REST di gestione certificati| Microsoft Docs
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
ms.assetid: 530047f1-e43b-4a69-9542-75bc1da57bf7
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 3d724dea8b1536f0155f9fc287d0cd74f5f16b3c
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/13/2017
---
# <a name="cm-rest-api-service-details"></a>Dettagli servizio API REST di gestione certificati (CM)
Nelle sezioni seguenti vengono illustrati i dettagli delle API REST di gestione certificati (CM) di Microsoft Identity Manager (MIM).

## <a name="architecture"></a>Architettura 
Le chiamate API REST di gestion dei certificati MIM vengono gestite dai controller. Nella tabella seguente viene illustrato l'elenco completo dei controller e gli esempi del contesto in cui possono essere utilizzati.

Controller| Route di esempio
----------|-------------
CertificateDataController| /api/v1.0/requests/{requestid}/certificatedata /
CertificateRequestGenerationOptionsController| /api/v1.0/requests/{requestid}/certificaterequestgenerationoptions
CertificatesController| /api/v1.0/smartcards/{smartcardid}/certificates <br/> /api/v1.0/profiles/{profileid}/certificates
OperationsController| /api/v1.0/smartcards/{smartcardid}/operations <br/> /api/v1.0/profiles/{profileid}/operations
PoliciesController| /api/v1.0/profiletemplates/{profiletemplateid}/policies/{id}
ProfilesController| /api/v1.0/profiles/{id} <br/> /api/v1.0/Profiles <br/> /api/v1.0/requests/{requestid}/profiles/{id}
ProfileTemplatesController| /api/v1.0/profiletemplates/{id} <br/> /api/v1.0/profiletemplates <br/> /api/v1.0/profiletemplates/{profiletemplateid}/policies/{id}
RequestsController| /api/v1.0/requests/{id} <br/> /api/v1.0/requests
SmartcardsController| /api/v1.0/requests/{requestid}/smartcards/{id}/diversifiedkey <br/> /api/v1.0/requests/{requestid}/smartcards/{id}/serverproposedpin <br/> /api/v1.0/requests/{requestid}/smartcards/{id}/authenticationresponse <br/> /api/v1.0/requests/{requestid}/smartcards/{id} <br/> /api/v1.0/smartcards/{id} <br/> /api/v1.0/smartcards
SmartcardsConfigurationController| /api/v1.0/profiletemplates/{profiletemplateid}/configuration/smartcards
<br/>

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


## <a name="api-versioning"></a>Controllo versioni API 
La versione corrente delle API REST di gestione certificati è 1.0. La versione è specificata nel segmento direttamente dopo il segmento `/api` dell’'URI: `/api/v1.0`. In futuro, verrà modificato il numero di versione qualora dovessero esserci modifiche rilevanti per l'interfaccia API.


## <a name="enabling-the-api"></a>Abilitazione delle API 
La sezione `<ClmConfiguration>` sezione del file Web.config è stato esteso con una nuova chiave:

```
<add key="Clm.WebApi.Enabled" value="false" />
```
<br/>

Questa chiave determina se l'API REST di gestione certificati viene esposta ai client. Se la chiave è impostata su "false", il mapping della route per l'API non viene eseguito all'avvio dell'applicazione. Ciò significa che le successive richieste per gli endpoint delle API restituiranno un codice di errore HTTP 404. Per impostazione predefinita, la chiave è impostata su "disabled".
Dopo avere modificato questo valore su "true", ricordare di eseguire **iisreset** sul server.

## <a name="enabling-tracing-and-logging"></a>Abilitazione della traccia e della registrazione 
L'API REST di gestione certificati MIM genera dati di traccia per ogni richiesta HTTP inviata. È possibile impostare il livello di dettaglio delle informazioni di traccia generate impostando il valore di configurazione seguente:

```
<add name="Microsoft.Clm.Web.API" value="0" />
```
<br/>

## <a name="error-handling-and-troubleshooting"></a>Gestione degli errori e risoluzione dei problemi 
Quando si verificano eccezioni durante l'elaborazione di una richiesta, l'API REST di gestione certificati MIM restituisce un codice di stato HTTP al client Web. Per gli errori comuni, l'API restituisce un codice di stato HTTP appropriato e un codice di errore. 

Le eccezioni non gestite vengono convertite in una `HttpResponseException` con stato HTTP 500 ("Errore interno") del codice e tracciate sia nel registro eventi sia nel file di traccia di gestione certificati MIM. Ogni eccezione non gestita viene scritta nel registro eventi con un ID di correlazione corrispondente. L'ID di correlazione viene inoltre inviato al consumer delle API nel messaggio di errore. Per risolvere l'errore, un amministratore può eseguire la ricerca nel registro eventi per i dettagli di errore e l’ID di correlazione corrispondenti.

Per motivi di sicurezza, le analisi dello stack corrispondenti agli errori generati in seguito all’uso delle API REST di gestione certificati MIM non vengono reinviate al client.
