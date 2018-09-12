---
title: Usare un provider Multi-Factor Authentication alternativo tramite un'API per attivare PAM o in uno scenario SSPR | Microsoft Docs
description: Configurare un'API MFA personalizzata come secondo livello di sicurezza quando gli utenti attivano ruoli in Privileged Access Management e usano Reimpostazione password self-service (SSPR, Self Service Password Reset).
keywords: ''
author: billmath
ms.author: billmath
ms.reviewer: fimguy
manager: mtillman
ms.date: 09/04/2018
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.openlocfilehash: 85ab84231c2ebfede125ffaf5fb39964e8a8450c
ms.sourcegitcommit: acb2c61831cb634278acc439d6d9496ff51a6a54
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/04/2018
ms.locfileid: "43694840"
---
# <a name="use-a-custom-multi-factor-authentication-provider-via-an-api-during-pam-role-activation-or-in-sspr"></a>Usare un provider Multi-Factor Authentication personalizzato tramite un'API durante l'attivazione ruoli di PAM o in SSPR

I clienti di Azure AD Premium o Azure MFA possono integrare Azure MFA in due scenari MIM: attivazione del ruolo Privileged Access Management (PAM) e Reimpostazione password self-service (SSPR).

I clienti MIM dispongono di due opzioni aggiuntive:

 - Usare un provider di recapito con password monouso, applicabile solo nello scenario MIM SSPR e documentato nella guida in [Configure Self-Service Password Reset with OTP SMS Gate](https://docs.microsoft.com/en-us/previous-versions/mim/hh824692(v=ws.10)) (Configurare il ripristino della password self-service con controllo OTP SMS)
 - Usare un provider di telefonia con autenticazione a più fattori personalizzata. Questa opzione è applicabile a entrambi gli scenari MIM SSPR e PAM descritti in questo articolo

L'articolo illustra come usare MIM con un provider di autenticazione a più fattori personalizzato tramite un'API e un SDK di integrazione sviluppato dal cliente.  

## <a name="prerequisites"></a>Prerequisiti

Per usare un'API personalizzata di un provider di autenticazione a più fattori con MIM è necessario quanto segue:

- Numeri di telefono di tutti gli utenti candidati
- Hotfix MIM [4.5.202.0](https://www.microsoft.com/download/details.aspx?id=57278) o successive, vedere la [cronologia versioni](/reference/version-history.md) per gli annunci corrispondenti
- Servizio MIM configurato per SSPR o PAM

## <a name="approach-using-custom-multi-factor-authentication-code"></a>Approccio con codice di autenticazione a più fattori personalizzato

### <a name="step-1-ensure-mim-service-is-at-version-452020-or-later"></a>Passaggio 1: Verificare che la versione del servizio MIM sia 4.5.202.0 o successiva

Scaricare e installare l'hotfix MIM [4.5.202.0](https://www.microsoft.com/download/details.aspx?id=57278) o una versione successiva.

### <a name="step-2-create-a-dll-which-implements-the-iphoneserviceprovider-interface"></a>Passaggio 2: Creare una DLL che implementa l'interfaccia IPhoneServiceProvider

La DLL deve includere una classe che implementa tre metodi:

- `InitiateCall`: il servizio MIM chiama questo metodo. Il servizio passa il numero di telefono e l'ID richiesta come parametri.  Il metodo deve restituire un valore `PhoneCallStatus` pari a `Pending`, `Success` o `Failed`.
- `GetCallStatus`: se una chiamata precedente a `initiateCall` ha restituito `Pending`, il servizio MIM chiama questo metodo. Questo metodo restituisce anche un valore `PhoneCallStatus` pari a `Pending`, `Success` o `Failed`.
- `GetFailureMessage`: se una chiamata precedente di `InitiateCall` o `GetCallStatus` ha restituito `Failed`, il servizio MIM chiama questo metodo. Questo metodo restituisce un messaggio di diagnostica.

Le implementazioni di questi metodi devono essere thread-safe e l'implementazione di `GetCallStatus` e `GetFailureMessage` non deve presupporre che verranno chiamati dallo stesso thread usato da una chiamata precedente a `InitiateCall`.

Archiviare la DLL nella directory `C:\Program Files\Microsoft Forefront Identity Manager\2010\Service\`.

Codice di esempio che può essere compilato con Visual Studio 2010 o versione successiva.

```csharp
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Text;
using Microsoft.IdentityManagement.PhoneServiceProvider;

namespace CustomPhoneGate
{
    public class CustomPhoneGate: IPhoneServiceProvider
    {
        string path = @"c:\Test\phone.txt";
        public PhoneCallStatus GetCallStatus(string callId)
        {
            int res = 2;
            foreach (string line in File.ReadAllLines(path))
            {
                var info = line.Split(new char[] { ';' });
                if (string.Compare(info[0], callId) == 0)
                {
                    if (info.Length > 2)
                    {
                        bool b = Int32.TryParse(info[2], out res);
                        if (!b)
                        {
                            res = 2;
                        }
                    }
                    break;
                }
            }
            switch(res)
            {
                case 0:
                    return PhoneCallStatus.Pending;
                case 1:
                    return PhoneCallStatus.Success;
                case 2:
                    return PhoneCallStatus.Failed;
                default:
                    return PhoneCallStatus.Failed;
            }       
        }
        public string GetFailureMessage(string callId)
        {
            string res = "Call ID is not found";
            foreach (string line in File.ReadAllLines(path))
            {
                var info = line.Split(new char[] { ';' });
                if (string.Compare(info[0], callId) == 0)
                {
                    if (info.Length > 2)
                    {
                        res = info[3];
                    }
                    else
                    {
                        res = "Description is not found";
                    }
                    break;
                }
            }
            return res;            
        }
        
        public PhoneCallStatus InitiateCall(string phoneNumber, Guid requestId, Dictionary<string,object> deliveryAttributes)
        {
            // Here should be some logic for performing voice call
            // For testing purposes we just write details in file             
            string info = string.Format("{0};{1};{2};{3}", requestId, phoneNumber, 0, string.Empty);
            using (StreamWriter sw = File.AppendText(path))
            {
                sw.WriteLine(info);                
            }
            return PhoneCallStatus.Pending;    
        }
    }
}
```
### <a name="step-3-backup-the-mfasettingsxml-located-in-the-cprogram-filesmicrosoft-forefront-identity-manager2010service"></a>Passaggio 3: Eseguire il backup del file MfaSettings.xml in "C:\Programmi\Microsoft Forefront Identity Manager\2010\Service"

### <a name="step-4-edit-the-mfasettingsxml-file"></a>Passaggio 4: Modificare il file MfaSettings.xml

Aggiornare o cancellare le righe seguenti:

- Rimuovere o cancellare tutte le righe di voci di configurazione 

- Aggiornare o aggiungere le righe seguenti in MfaSettings.xml con il provider di telefonia personalizzato <br>
`<CustomPhoneProvider>C:\Program Files\Microsoft Forefront Identity Manager\2010\Service\CustomPhoneGate.dll</CustomPhoneProvider>`

### <a name="step-5-restart-mim-service"></a>Passaggio 5: Riavviare il servizio MIM

Dopo il riavvio del servizio, usare SSPR e/o PAM per convalidare la funzionalità con il provider di identità personalizzato.

> [!NOTE] 
> Per annullare le impostazioni, sostituire il file MfaSettings.xml con il file di backup creato nel passaggio 3


## <a name="next-steps"></a>Passaggi successivi

- [Introduzione al server Azure Multi-Factor Authentication](https://docs.microsoft.com/en-us/azure/active-directory/authentication/howto-mfaserver-deploy)
- [Che cos'è Azure Multi-Factor Authentication?](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication)
- [Cronologia di rilascio delle versioni di MIM](./reference/version-history.md)
