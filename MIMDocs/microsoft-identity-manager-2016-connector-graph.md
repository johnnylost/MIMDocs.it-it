---
title: Connettore di Microsoft Identity Manager per Microsoft Graph | Microsoft Docs
author: fimguy
description: Il connettore di Microsoft Identity Manager per Microsoft Graph consente di gestire il ciclo di vita degli account AD appartenenti a utenti esterni. In questo scenario un'organizzazione ha invitato alcuni guest nella directory di Azure AD e vuole concedere a tali guest l'accesso alle applicazioni locali basate su Kerberos o sull'autenticazione integrata di Windows
keywords: ''
ms.author: billmath
manager: mtillman
ms.date: 10/02/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 94a74f1c-2192-4748-9a25-62a526295338
ms.openlocfilehash: 2e376bcc88518b911f93ce9cd4ab920eb428815b
ms.sourcegitcommit: 7de35aaca3a21192e4696fdfd57d4dac2a7b9f90
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/16/2018
ms.locfileid: "49358653"
---
<a name="microsoft-identity-manager-connector-for-microsoft-graph"></a>Connettore di Microsoft Identity Manager per Microsoft Graph
=======================================================================================

<a name="summary"></a>Riepilogo 
=======

Il [connettore di Microsoft Identity Manager per Microsoft Graph](http://go.microsoft.com/fwlink/?LinkId=717495) abilita altri scenari di integrazione per i clienti di Azure AD Premium.  Questo connettore viene visualizzato negli oggetti metaverse aggiuntivi della sincronizzazione MIM ottenuti dall'[API Graph Microsoft](https://developer.microsoft.com/en-us/graph/) v1 e beta.

<a name="scenarios-covered"></a>Scenari coperti
=================

<a name="b2b-account-lifecycle-management"></a>Gestione del ciclo di vita di un account B2B
--------------------------------

Lo scenario iniziale per il connettore di Microsoft Identity Manager per Microsoft Graph consiste nell'automatizzazione della gestione del ciclo di vita degli account Active Directory Domain Services per gli utenti esterni. In questo scenario un'organizzazione sta sincronizzando i dipendenti con Azure AD da Active Directory Domain Services usando Azure AD Connect e ha anche invitato alcuni guest nella directory di Azure AD. L'invito di un guest ha come risultato di inserire un oggetto utente esterno nella directory di Azure AD dell'organizzazione che non è presente nell'istanza Active Directory Domain Services di tale organizzazione. Successivamente, l'organizzazione vuole concedere a tali guest l'accesso alle applicazioni locali basate su Kerberos o sull'autenticazione integrata di Windows, tramite il proxy di [applicazione Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-publish) o altri meccanismi gateway. Il proxy di applicazione Azure AD richiede che ogni utente abbia il proprio account Active Directory Domain Services, per scopi di identificazione e delega.  

Per informazioni su come configurare la sincronizzazione MIM per creare e gestire automaticamente gli account di Active Directory Domain Services per gli utenti guest, dopo aver letto le istruzioni riportate in questo articolo, continuare la lettura dell'articolo [Collaborazione Business to Business (B2B) di Azure AD con Microsoft Identity Manager(MIM) 2016 SP1 con il proxy di applicazione Azure](~/microsoft-identity-manager-2016-graph-b2b-scenario.md).  Questo articolo illustra le regole di sincronizzazione necessarie per il connettore.

<a name="other-identity-management-scenarios"></a>Altri scenari di gestione delle identità
---------------

Il connettore può essere usato per altri scenari specifici per la gestione delle identità che coinvolgono la creazione, la lettura, l'aggiornamento e l'eliminazione di oggetti utente, gruppo e contatto in Azure AD, oltre alla sincronizzazione di utenti e gruppi in Azure AD. Quando si valutano i potenziali scenari, tenere presente che questo connettore non può essere utilizzato in uno scenario che provocherebbe la sovrapposizione dei flussi di dati e un conflitto di sincronizzazione effettivo o potenziale con una distribuzione di Azure AD Connect.  [Azure AD Connect](https://www.microsoft.com/en-us/download/details.aspx?id=47594) è l'approccio consigliato per integrare le directory locali con Azure AD tramite la sincronizzazione degli utenti e dei gruppi delle directory locali con Azure AD.  Azure AD Connect dispone di molte altre funzionalità di sincronizzazione e abilita scenari come il writeback delle password e dei dispositivi, che non sono possibili con gli oggetti creati da MIM. Per i dati che vengano importati in Active Directory Domain Services, ad esempio, assicurarsi che Azure AD Connect non tenti di creare una corrispondenza tra tali oggetti e quelli della directory di Azure AD.  Il connettore non può essere usato neanche per apportare modifiche agli oggetti di Azure AD che sono stati creati da Azure AD Connect.



<a name="preparing-to-use-the-connector-for-microsoft-graph"></a>Preparazione all'uso del connettore per Microsoft Graph
=============================================================

<a name="authorizing-the-connector-to-retrieve-or-manage-objects-in-your-azure-ad-directory"></a>Autorizzare il connettore a recuperare o gestire gli oggetti nella directory di Azure AD
----------------------------------------------------

1.  Il connettore richiede la creazione di un'app Web o di un'API applicazione in Azure AD, in modo che possa essere dotata delle autorizzazioni appropriate per eseguire operazioni sugli oggetti di Azure AD tramite Microsoft Graph.

![](media/microsoft-identity-manager-2016-ma-graph/724d3fc33b4c405ab7eb9126e7fe831f.png)

Immagine 1. Registrazione nuova applicazione

2.  Nel portale di Azure aprire l'applicazione creata e salvare l'ID dell'applicazione come un ID client da usare in un secondo momento nella pagina di connettività dell'agente di gestione:

![](media/microsoft-identity-manager-2016-ma-graph/ecfcb97674790290aa9ca2dcaccdafbc.png)

Immagine 2. ID applicazione

3.  Generare il nuovo segreto client aprendo Tutte le impostazioni -\> Chiavi. Immettere la descrizione delle chiavi e selezionare la durata necessaria. Salvare le modifiche. Un valore segreto non sarà disponibile dopo avere lasciato la pagina.

![](media/microsoft-identity-manager-2016-ma-graph/fdbae443f9e6ccb650a0cb73c9e1a56f.png)

Immagine 3. Nuovo segreto client

4.  Aggiungere "API Microsoft Graph" all'applicazione aprendo "Autorizzazioni necessarie".

![](media/microsoft-identity-manager-2016-ma-graph/908788fbf8c3c75101f7b663a8d78a4b.png)

Immagine 4. Aggiungere la nuova API

L'autorizzazione seguente deve essere aggiunta all'applicazione per consentire l'utilizzo dell'API Graph Microsoft, a seconda dello scenario:

| Operazione con oggetto | Autorizzazione necessaria                                                                  | Tipo di autorizzazione |
|-----------------------|--------------------------------------------------------------------------------------|-----------------|
| Importare il gruppo          | `Group.Read.All` o `Group.ReadWrite.All`                                                | Applicazioni     |
| Importare l'utente           | `User.Read.All`, `User.ReadWrite.All`, `Directory.Read.All` o `Directory.ReadWrite.All` | Applicazioni     |

Per altre informazioni sulle autorizzazioni obbligatorie, vedere [qui](https://developer.microsoft.com/en-us/graph/docs/concepts/permissions_reference).

5. Concedere le autorizzazioni necessarie all'applicazione.


<a name="installing-the-connector"></a>Installazione del connettore
========================

6.  Prima di installare il connettore, assicurarsi che nel server di sincronizzazione sia presente quanto segue: 

 - Microsoft .NET Framework 4.5.2 o versione successiva
 - Microsoft Identity Manager 2016 SP1 e l'aggiornamento rapido 4.4.1642.0 [KB4021562](https://www.microsoft.com/en-us/download/details.aspx?id=55794) o versione successiva.

7. Il connettore per Microsoft Graph, oltre ad altri connettori per Microsoft Identity Manager 2016 SP1, sono disponibili come download nell'[Area download Microsoft](https://www.microsoft.com/en-us/download/details.aspx?id=51495).

8.  Riavviare il servizio di sincronizzazione MIM.
 
<a name="connector-configuration"></a>Configurazione del connettore
=======================


9.  Nell'interfaccia utente di Synchronization Service Manager selezionare **Connettori** e **Crea**.
Selezionare **Graph (Microsoft)**, creare un connettore e assegnargli un nome descrittivo.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/d95c6b2cc7951b607388cbd25920d7d0.png)


10. Nell'interfaccia utente del servizio di sincronizzazione MIM specificare l'ID applicazione e il segreto client generato. Ogni agente di gestione configurato nel servizio di sincronizzazione MIM deve avere la propria applicazione in Azure AD per evitare di eseguire l'importazione in parallelo per la stessa applicazione.


![](media/microsoft-identity-manager-2016-ma-graph/77c2eb73bab8d5187da06293938f5fd9.png)

Immagine 5. Pagina Connettività

La pagina Connettività (immagine 5) contiene la versione dell'API Graph usata e il nome del tenant. L'ID client e il segreto client rappresentano l'ID applicazione e il valore della chiave dell'applicazione per le API Web che deve essere creata in Azure AD.

11. Apportare le modifiche necessarie nella pagina Parametri globali:

![](media/microsoft-identity-manager-2016-ma-graph/e22d4ee99f2bb825704dd83c1b26dac2.png)

Immagine 6. Pagina Parametri globali (Parametri globali)

La pagina Global Parameters (Parametri globali) contiene le impostazioni seguenti:

- DateTime format (Formato data/ora): formato usato per gli attributi con tipo Edm.DateTimeOffset. Tutte le date vengono convertite in stringa e tale formato viene usato durante l'importazione. Il formato impostato viene applicato per qualsiasi attributo per il salvataggio della data.

 - HTTP timeout (seconds) (Timeout HTTP - secondi): timeout in secondi che verrà usato durante ogni chiamata HTTP all'applicazione per le API Web.

 - Force change password for created user at next sign (Forza la modifica della password per l'utente creato all'accesso successivo): questa opzione viene usata per il nuovo utente che verrà creato durante l'esportazione. Se l'opzione viene abilitata, la proprietà [forceChangePasswordNextSignIn](https://developer.microsoft.com/en-us/graph/docs/api-reference/v1.0/resources/passwordprofile) verrà impostata su true. In caso contrario, sarà false.

<a name="configuring-the-connector-schema-and-operations"></a>Configurazione delle operazioni e dello schema del connettore
=========================

12.   Configurare lo schema.  Il connettore supporta l'elenco seguente di tipi di oggetto:

-   Utente

    -   Importazione differenziale/completa

    -   Esportazione (aggiunta, aggiornamento, eliminazione)

-   Group

    -   Importazione differenziale/completa

    -   Esportazione (aggiunta, aggiornamento, eliminazione)


Elenco dei tipi di attributo supportati:

-   `Edm.Boolean`

-   `Edm.String`

-   `Edm.DateTimeOffset` (stringa nello spazio connettore)

-   `microsoft.graph.directoryObject` (riferimento nello spazio connettore a uno degli oggetti supportati)

-   `microsoft.graph.contact`

Per tutti i tipi dell'elenco precedente sono supportati anche gli attributi multivalore (raccolta).

Il connettore usa l'attributo "`id`" per l'ancoraggio e il DN di tutti gli oggetti.  Pertanto, la ridenominazione non è necessaria perché l'API Graph non consente a un oggetto di modificare il proprio attributo 'id'.


<a name="access-token-lifetime"></a>Durata dei token di accesso
=====================

Un'applicazione Graph richiede un token di accesso per l'accesso all'API Graph. Un connettore richiederà un nuovo token di accesso per ogni iterazione dell'importazione. L'iterazione dell'importazione dipende dalle dimensioni di pagina. Ad esempio:

-   Azure AD contiene 10000 oggetti

-   Le dimensioni della pagina configurate nel connettore sono pari a 5000

In questo caso verranno eseguite due iterazioni durante l'importazione e ognuna restituirà 5000 oggetti da sincronizzare. In tal caso, un nuovo token di accesso verrà richiesto due volte.

Durante l'esportazione verrà richiesto un nuovo token di accesso per ogni oggetto che deve essere aggiunto/aggiornato/eliminato.

<a name="troubleshooting"></a>Risoluzione dei problemi
===============

**Abilitare i log**

Se si verificano problemi in Graph, si possono usare i log per localizzare il problema. Le tracce possono quindi essere abilitate nello [stesso modo dei connettori generici](https://social.technet.microsoft.com/wiki/contents/articles/21086.fim-2010-r2-troubleshooting-how-to-enable-etw-tracing-for-connectors.aspx). Oppure semplicemente aggiungendo il codice seguente `miiserver.exe.config` (all'interno della sezione `system.diagnostics/sources`):


\<source name="ConnectorsLog" switchValue="Verbose"\>

\<listeners\>

>   \<add initializeData="ConnectorsLog"   type="System.Diagnostics.EventLogTraceListener, System, Version=4.0.0.0,   Culture=neutral, PublicKeyToken=b77a5c561934e089"   name="ConnectorsLogListener" traceOutputOptions="LogicalOperationStack,   DateTime, Timestamp, Call stack" /\>

\<remove name="Default" /\>

\</listeners\>

\</source\>

>[!NOTE]
>Se "Run this management agent in a separate process" (Esegui questo agente di gestione in un processo separato) è abilitato, usare `dllhost.exe.config` al posto di `miiserver.exe.config`.

**Errore di token di accesso scaduto**

Il connettore potrebbe restituire l'errore HTTP 401: accesso non autorizzato, con il messaggio "Access token has expired" (Il token di accesso è scaduto):

![](media/microsoft-identity-manager-2016-ma-graph/ce9e23ffe17e3dac79b58bba31cb5a8d.png)

Immagine 7. "Access token has expired" (Il token di accesso è scaduto) Errore

La causa di questo problema potrebbe dipendere dalla configurazione della durata del token di accesso sul lato Azure. Per impostazione predefinita, il token di accesso scade dopo 1 ora. Per spostare l'ora di scadenza, vedere [questo articolo](https://docs.microsoft.com/azure/active-directory/active-directory-configurable-token-lifetimes).

Esempio dell'uso della [versione di anteprima pubblica del modulo Azure AD PowerShell](https://www.powershellgallery.com/packages/AzureADPreview)

![](media/microsoft-identity-manager-2016-ma-graph/a26ded518f94b9b557064b73615c71f6.png)

New-AzureADPolicy -Definition \@('{"TokenLifetimePolicy":{"Version":1, **"AccessTokenLifetime":"5:00:00"**}}') -DisplayName "OrganizationDefaultPolicyScenario" -IsOrganizationDefault \$true -Type "TokenLifetimePolicy"

<a name="next-steps"></a>Passaggi successivi
----------
- [Graph Explorer, ideale per la risoluzione dei problemi relativi alle chiamate HTTP]( https://developer.microsoft.com/en-us/graph/graph-explorer)
- [Versioning, support, and breaking change policies for Microsoft Graph (Criteri di controllo delle versioni, supporto e modifica che causa un'interruzione per Microsoft Graph)](https://developer.microsoft.com/en-us/graph/docs/concepts/versioning_and_support)
- [Scaricare il connettore di Microsoft Identity Manager per Microsoft Graph](http://go.microsoft.com/fwlink/?LinkId=717495)

<a name="scenario-specific-guides"></a>Guide specifiche per lo scenario
----------------------------------
[Distribuzione end-to-end B2B MIM]( ~/microsoft-identity-manager-2016-graph-b2b-scenario.md)
