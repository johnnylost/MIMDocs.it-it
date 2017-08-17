---
title: Procedura di registrazione di esempio | Microsoft Docs
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
ms.assetid: 92b97803-9475-4b90-9a6c-430f107a167d
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 574a4d6fc2d5d4a51483a371cf96c2d48c582a8b
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/13/2017
---
# <a name="sample-enrollment-walkthrough"></a>Procedura di registrazione di esempio
In questo argomento vengono illustrati i passaggi necessari per eseguire la registrazione self-service di una smart card virtuale. Viene mostrata la richiesta di approvazione automatica con un PIN impostato dall’utente.
1.  Il client autentica l'utente, quindi richiede un elenco di modelli di profilo a cui l'utente autenticato può registrarsi:

    ```
    GET /CertificateManagement/api/v1.0/profiletemplates.
    ```
    <br/>
2.  Il client visualizza l'elenco risultante all'utente. L'utente seleziona un modello di profilo vSC (Smart Card virtuale) con nome "VPN smart card virtuale" e UUID *97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1*.

3.  Il client recupera i criteri di registrazione del modello di profilo selezionato tramite l'UUID restituito nel passaggio precedente:

    ```
    GET /CertificateManagement/api/v1.0/profiletemplates/97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1/policies/enroll
    ```
 <br/>   
4.  Il client analizza il campo "DataCollection" nel criterio restituito, notando che viene visualizzato un singolo elemento di raccolta dati denominato "Motivo richiesta". Il client nota inoltre che il flag "collectComments" è impostato su *false*in modo da non richiedere alcuna immissione da parte dell’utente.

5.  Una volta che l’utente ha immesso il motivo per la richiesta di certificati, il client crea una richiesta:

    ```
    POST /CertificateManagement/api/v1.0/requests

    {
        "datacollection":"[{“Request Reason”:”Need VPN Certs”}]",
        "type":"Enroll",
        "profiletemplateuuid":"97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1",
        "comment":""
    }
    ```
<br/>
6.  Il server crea la richiesta correttamente e restituisce l'URI della richiesta al client: /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5.

7.  Il client recupera l'oggetto richiesta chiamando l'URI restituito:

    ```
    GET /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5
    ```
<br/>
8.  Il client verifica che la proprietà "stato" nella richiesta sia impostata su "approvata". L'esecuzione della richiesta può iniziare.

9.  Il client esamina la richiesta per verificare se esiste una smart card già associata alla richiesta analizzando il contenuto del parametro "newsmartcarduuid".

10. Poiché contiene solo un GUID vuoto, il client deve utilizzare una scheda esistente che non sia già usata da MIM CM oppure crearne una nel caso del modello di profilo configurato per smart card virtuali.

11. Poiché quest'ultimo è stato indicato al client tramite la query iniziale per i modelli di profilo registrabili (passaggio 1), il client deve creare adesso un dispositivo smart card virtuale.

12. Il client recupera il criterio di smart card dal modello di profilo (utilizzando l'UUID del modello selezionato nel passaggio 3):

    ```
    GET /CertificateManagement/api/v1.0/profiletemplates/97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1/configuration/smartcards
    ```
13. Il criterio di smart card conterrà la chiave di amministrazione predefinita per la scheda nella proprietà "DefaultAdminKeyHex". Quando si crea la smart card, il client deve impostare la chiave di amministrazione iniziale della smart card su questa chiave.  

14. Al momento della creazione del dispositivo di smart card, il client deve assegnarlo alla richiesta:

    ```
    POST /CertificateManagement/api/v1.0/smartcards

    {
        "requestid":" C6BAD97C-F97F-4920-8947-BE980C98C6B5",
        "cardid":"23CADD5F-020D-4C3B-A5CA-307B7A06F9C9",
    }
    ```
<br/>
15. Il server risponde al client con un URI per l'oggetto smart card appena creato: api/v1.0/smartcards/D700D97C-F91F-4930-8947-BE980C98A644.

16. Il client recupera l'oggetto smart card:

    ```
    GET /CertificateManagement/api/v1.0/smartcards/ D700D97C-F91F-4930-8947-BE980C98A644
    ```
<br/>
17. Controllando il valore del flag "diversifyadminkey" nel criterio di smart card ottenuto nel passaggio 12, il client sa che deve diversificare la chiave di amministrazione.

18. Il client recupera la chiave di amministratore proposta:

    ```
    GET /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5/smartcards/D700D97C-F91F-4930-8947-BE980C98A644/diversifiedkey?cardid=23CADD5F-020D-4C3B-A5CA-307B7A06F9C9
    ```
<br/>
19. Il client deve autenticarsi presso la scheda come amministratore per impostare la chiave di amministrazione. A tale scopo, il client effettua una richiesta di autenticazione di smart card e la invia al server:

    ```
    GET /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5/smartcards/D700D97C-F91F-4930-8947-BE980C98A644/authenticationresponse?cardid=23CADD5F-020D-4C3B-A5CA-307B7A06F9C9&challenge=CFAA62118BBD25&diversified=false
    ```

    Il server invia la risposta alla richiesta nel corpo della risposta HTTP. Individuare la stringa di richiesta esadecimale, convertirla in base64 e trasferirla come parametro nell'URL. L'URL restituirà un'altra risposta, che deve essere convertita in formato esadecimale.
<br/>
20. Il server invia la risposta alla richiesta nel corpo della risposta HTTP e il client la utilizza per diversificare la chiave di amministrazione.

21. Il client rileva che l'utente deve fornire il proprio PIN desiderato controllando il campo "UserPinOption" nel criterio di smart card.

22. Dopo che l'utente ha immesso il PIN desiderato in una finestra di dialogo, il client esegue l'autenticazione richiesta/risposta di autenticazione come descritto nel passaggio 19, con la sola differenza che il flag diversificato dovrebbe ora essere impostato su "true":

    ```
    GET /CertificateManagement/api/v1.0/ requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5/smartcards/D700D97C-F91F-4930-8947-BE980C98A644/authenticationresponse?cardid=23CADD5F-020D-4C3B-A5CA-307B7A06F9C9&challenge=CFAA62118BBD25&diversified=true
    ```
<br/>
23. Il client utilizza la risposta ricevuta dal server per impostare il PIN utente desiderato.

24. Il client è ora pronto per generare richieste di certificati. Esegue query dei parametri di generazione del certificato del modello di profilo per determinare la modalità di generazione delle chiavi/richieste.

    ```
    GET /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5/certificaterequestgenerationoptions.
    ```
<br/>
25. Il server risponde con un singolo oggetto serializzato JSON CertificateRequestGenerationOptions che riporta in dettaglio l’esportabilità della chiave, il nome descrittivo del certificato chiave, l’algoritmo di hash, l'algoritmo della chiave, la dimensione della chiave e così via. Il client utilizza questi parametri per generare una richiesta di certificato.

26. La richiesta del singolo certificato viene inviata al server. Il client può inoltre specificare una password PFX che deve essere utilizzata per decrittografare tutti i BLOB PFX nel caso in cui il modello di certificato specifichi l'archiviazione dei certificati sulla CA, vale a dire che la CA genera la coppia di chiavi e la invia al client. Il client può anche scegliere di aggiungere alcuni commenti.

    ```
    POST /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5/certificatedata

    {
       "pfxpassword":"",
       "certificaterequests":[
           "MIIDZDC…”
        ]
    }   
    ```
<br/>
27. Dopo alcuni secondi, il server risponde con un oggetto serializzato JSON Microsoft.Clm.Shared.Certificate. Selezionando il flag "isPkcs7", il client viene a conoscenza che questa risposta non è un BLOB PFX. Il client estrae il BLOB tramite decodifica base64 della stringa "pkcs7" e lo installa.

28. Il client contrassegna la richiesta come completata.

    ```
    PUT /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5

    {
        "status":"Completed"
    }
    ```
