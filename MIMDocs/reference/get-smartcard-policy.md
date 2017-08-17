---
title: Ottenere i criteri per smart card | Microsoft Docs
description: 
keywords: 
author: msmbaldwin
ms.author: mbaldwin
manager: mbaldwin
ms.date: 10/17/2016
ms.topic: article
ms.prod: microsoft-identity-manager
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: c015ffc7-5c94-427e-a3b3-870ec8ab92b6
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 1164795e81ff0ef2f93086144b721e760f8dd028
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/13/2017
---
# <a name="get-smartcard-policy"></a>Ottieni criterio smartcard

Ottiene i criteri dei modelli di profilo per il flusso di lavoro specificato. Questi dati vengono utilizzati durante la creazione della richiesta. Il criterio di flusso di lavoro specifica i dati di cui il client necessita per creare una richiesta. Tali dati possono includere: diversi elementi di raccolta dati, i commenti richiesta e criteri password monouso.

**Nota**: gli URL mostrati in questo argomento sono relativi al nome host scelto durante la distribuzione delle API. Ad esempio: `https://api.contoso.com`.
##<a name="request"></a>Richiesta


Metodo  |URL della richiesta  
---------|---------
GET     |/CertificateManagement/api/v1.0/profiletemplates/{ID}/policy/workflow/{tipo}

###<a name="url-parameters"></a>Parametri URL
Parametro| Descrizione
--------|-------------
ID| Obbligatorio. Il GUID corrispondente al modello di profilo da cui deve essere estratto il criterio.
tipo| Obbligatorio. Il tipo di criterio richiesto. Valori possibili: *Enroll*, *Duplicate*, *OfflineUnblock*, *OnlineUpdate*, *Renew*, *Recover*, *RecoverOnBehalf*, *Reinstate*, *Retire*, *Revoke*, *TemporaryEnroll*, *Unblock*.

###<a name="request-headers"></a>Intestazioni delle richieste
Per le intestazioni di richiesta comuni, vedere [HTTP Request and Response Headers](certificate-management-rest-api-service-details.md#http-request-and-response-headers) (Intestazioni di richiesta e risposta HTTP) in *Dettagli del servizio API REST di gestione certificati*.
###<a name="request-body"></a>Testo della richiesta
Nessuno

##<a name="response"></a>Risposta
###<a name="response-codes"></a>Codici di risposta
Codice  |Descrizione  
---------|---------
200     | OK
403 | Non consentito
204 | Nessun contenuto
500 | Errore interno

###<a name="response-headers"></a>Intestazioni delle risposte
Per le intestazioni di risposta comuni, vedere [HTTP Request and Response Headers](certificate-management-rest-api-service-details.md#http-request-and-response-headers) (Intestazioni di richiesta e risposta HTTP) in *Dettagli del servizio API REST di gestione certificati*.
###<a name="response-body"></a>Testo della risposta
In caso di esito positivo, restituisce un oggetto criterio basato su un oggetto [ProfileTemplatePolicy](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.profiletemplates.profiletemplatepolicy.aspx). L'oggetto criterio conterrà almeno le proprietà nella tabella seguente, ma può contenere proprietà aggiuntive in base ai criteri richiesti. Ad esempio, una richiesta per un criterio di registrazione restituirà un oggetto [EnrollPolicy](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.profiletemplates.enrollpolicy). Per ulteriori informazioni, vedere la documentazione per l'oggetto criterio associato al parametro di {type} della richiesta. La documentazione per i diversi tipi di oggetti criterio è reperibile nella documentazione [Microsoft.Clm.Shared.ProfileTemplates Namespace](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.profiletemplates).

Proprietà | Descrizione
---------|------------
ApprovalsNeeded | Il numero di approvazioni necessarie per le richieste CM FIM per il criterio.
AuthorizedApprover | Il descrittore di sicurezza per gli utenti autorizzati ad approvare le richieste CM FIM per il criterio.
AuthorizedEnrollmentAgent | Il descrittore di sicurezza per gli utenti che possono fungere da agenti di registrazione per il criterio.
AuthorizedInitiator | Il descrittore di sicurezza per gli utenti che possono avviare le richieste CM FIM per il criterio.
CollectComments | Valore booleano che indica se l'insieme dei commenti è abilitato per le richieste CM FIM per il criterio.
CollectRichiestaPriority | Valore booleano che indica se l'insieme delle priorità delle richieste abilitato per le richieste CM FIM per il criterio.
DefaultRichiestaPriority | La priorità predefinita per le richieste CM FIM per il criterio.
Documenti | I documenti dei criteri che sono configurati per il criterio.
Enabled | Valore booleano che indica se il criterio è abilitato.
EnrollAgentRequired | Valore booleano che indica se sono richiesti gli agenti di registrazione per le richieste CM FIM per il criterio.
OneTimePasswordPolicy | Ottiene il modo in cui vengono distribuite le password monouso per le richieste CM FIM per il criterio.
Personalization | Le opzioni di personalizzazione delle smart card per il criterio.
PolicyDataCollection | Gli elementi di raccolta dati che sono associati ai criteri.
SelfServiceEnabled | Valore booleano che indica se l’avvio self-service delle richieste CM FIM è abilitato per il criterio.

##<a name="example"></a>Esempio

###<a name="request-1"></a>Richiesta 1
```
GET /CertificateManagement/api/v1.0/profiletemplates/97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1/policies/enroll HTTP/1.1
```
###<a name="response-1"></a>Risposta 1
```
HTTP/1.1 200 OK

... body coming soon
```       
###<a name="request-2"></a>Richiesta 2
```
GET /CertificateManagement/api/v1.0/profiletemplates/97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1/policies/renew HTTP/1.1
```
###<a name="response-2"></a>Risposta 2
```
HTTP/1.1 200 OK

... body coming soon
```       
##<a name="see-also"></a>Vedere anche

- [Classe Microsoft.Clm.Shared.ProfileTemplates.ProfileTemplatePolicy](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.profiletemplates.profiletemplatepolicy.aspx)
- [Spazio dei nomi Microsoft.Clm.Shared.ProfileTemplates](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.profiletemplates.aspx)
