---
title: Agente di gestione di Microsoft Identity Manager per Microsoft Graph | Microsoft Docs
author: fimguy
description: Microsoft Graph (anteprima) esegue la gestione del ciclo di vita degli account AD degli utenti esterni. In questo scenario un'organizzazione ha invitato alcuni guest nella directory di Azure AD e vuole concedere a tali guest l'accesso alle applicazioni locali basate su Kerberos o sull'autenticazione integrata di Windows
keywords: ''
ms.author: davidste
manager: bhu
ms.date: 04/25/2018
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 94a74f1c-2192-4748-9a25-62a526295338
ms.openlocfilehash: a66d424e8388005855ac8e64623f5a00f89682e9
ms.sourcegitcommit: c773edc8262b38df50d82dae0f026bb49500d0a4
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/05/2018
ms.locfileid: "34479367"
---
<a name="the-microsoft-identity-manager-management-agent-for-microsoft-graph-public-preview"></a>Agente di gestione di Microsoft Identity Manager per Microsoft Graph (anteprima pubblica)
=======================================================================================

<a name="summary"></a>Riepilogo 
=======

L'[agente di gestione di Microsoft Identity Manager per Microsoft Graph (anteprima)](http://go.microsoft.com/fwlink/?LinkId=717495) abilita altri scenari di integrazione per i clienti di Azure AD Premium.

[Azure AD Connect](https://www.microsoft.com/en-us/download/details.aspx?id=47594) integra le directory locali con Azure AD e garantisce che gli utenti abbiano un'identità comune e un'autenticazione coerente in Active Directory Domain Services, Office 365, Azure e applicazioni SaaS integrate con Azure AD, sincronizzando utenti e gruppi dalle directory locali ad Azure AD.   Questo agente di gestione può essere distribuito per operazioni specifiche di gestione delle identità e degli accessi, oltre che per la sincronizzazione di utenti e gruppi in Azure AD.  Questo agente di gestione viene visualizzato negli oggetti metaverse aggiuntivi della sincronizzazione MIM ottenuti dall'[API Microsoft Graph](https://developer.microsoft.com/en-us/graph/) v1 e beta. 

<a name="scenarios-covered"></a>Scenari coperti
=================

<a name="b2b-account-lifecycle-management"></a>Gestione del ciclo di vita di un account B2B
--------------------------------

Lo scenario iniziale in anteprima per l'agente di gestione di Microsoft Identity Manager per Microsoft Graph (anteprima) è la gestione del ciclo di vita degli account AD degli utenti esterni. In questo scenario un'organizzazione ha invitato alcuni guest nella directory di Azure AD e vuole concedere a tali guest l'accesso alle applicazioni locali basate su Kerberos o sull'autenticazione integrata di Windows, tramite il proxy di [applicazione Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-publish) o altri meccanismi gateway. Il proxy di applicazione Azure AD richiede che ogni utente abbia il proprio account Active Directory Domain Services, per scopi di identificazione e delega

In futuro potrebbero essere aggiunti altri scenari che verranno [documentati qui](./microsoft-identity-manager-2016-graph-b2b-scenario.md)

<a name="determining-your-deployment-topology"></a>Determinazione della topologia di distribuzione
====================================


<a name="preparing-to-use-the-management-agentma-for-microsoft-graph"></a>Preparazione all'uso dell'agente di gestione per Microsoft Graph
=============================================================

<a name="authorizing-the-ma-to-manage-your-azure-ad-directory"></a>Autorizzazione dell'agente di gestione alla gestione della directory Azure AD
----------------------------------------------------

1.  L'agente di gestione per Graph richiede che l'app Web/applicazione per le API venga creata in Azure AD.

![](media/microsoft-identity-manager-2016-ma-graph/724d3fc33b4c405ab7eb9126e7fe831f.png)

Immagine 1. Registrazione nuova applicazione

2.  Aprire l'applicazione creata e usare l'ID dell'applicazione come ID client nella pagina della connettività dell'agente di gestione:

![](media/microsoft-identity-manager-2016-ma-graph/ecfcb97674790290aa9ca2dcaccdafbc.png)

Immagine 2. ID applicazione

2.  Generare il nuovo segreto client aprendo Tutte le impostazioni -\> Chiavi. Immettere la descrizione delle chiavi e selezionare la durata necessaria. Salvare le modifiche. Un valore segreto non sarà disponibile dopo avere lasciato la pagina.

![](media/microsoft-identity-manager-2016-ma-graph/fdbae443f9e6ccb650a0cb73c9e1a56f.png)

Immagine 3. Nuovo segreto client

3.  Aggiungere "API Microsoft Graph" all'applicazione aprendo "Autorizzazioni necessarie".

![](media/microsoft-identity-manager-2016-ma-graph/908788fbf8c3c75101f7b663a8d78a4b.png)

Immagine 4. Aggiungere la nuova API

L'autorizzazione seguente deve essere aggiunta ad "API Microsoft Graph":

| Operazione con oggetto | Autorizzazione necessaria                                                                  | Tipo di autorizzazione |
|-----------------------|--------------------------------------------------------------------------------------|-----------------|
| Importare il gruppo          | Group.Read.All or Group.ReadWrite.All                                                | Applicazioni     |
| Importare l'utente           | User.Read.All or User.ReadWrite.All or Directory.Read.All or Directory.ReadWrite.All | Applicazioni     |

Per altre informazioni sulle autorizzazioni obbligatorie, vedere [qui](https://developer.microsoft.com/en-us/graph/docs/concepts/permissions_reference)

1.  Creare il connettore con l'ID applicazione e il segreto client generato. Ogni agente di gestione deve avere la propria applicazione in Azure AD per evitare di eseguire l'importazione in parallelo per la stessa applicazione. Il connettore Graph supporta l'elenco seguente di tipi di oggetto:

-   Utente

    -   Importazione differenziale/completa

    -   Esportazione (aggiunta, aggiornamento, eliminazione)

-   Group

    -   Importazione differenziale/completa

    -   Esportazione (aggiunta, aggiornamento, eliminazione)


Elenco dei tipi di attributo supportati:

-   Edm.Boolean

-   Edm.String

-   Edm.DateTimeOffset (stringa nello spazio connettore)

-   microsoft.graph.directoryObject (riferimento nello spazio connettore a uno degli oggetti supportati)

-   microsoft.graph.contact

Per tutti i tipi dell'elenco precedente sono supportati anche gli attributi multivalore (raccolta).

Il connettore Graph usa l'attributo "id" per l'ancoraggio e il DN di tutti gli oggetti.

La ridenominazione non è supportata in questo momento perché l'API Graph non consente di modificare l'attributo "id" per l'oggetto esistente.

<a name="access-token-lifetime"></a>Durata dei token di accesso
=====================

Un'applicazione Graph richiede un token di accesso per l'accesso all'API Graph. Un connettore richiederà un nuovo token di accesso per ogni iterazione dell'importazione. L'iterazione dell'importazione dipende dalle dimensioni di pagina. Ad esempio:

-   Azure AD contiene 10000 oggetti

-   Le dimensioni della pagina configurate nel connettore sono pari a 5000

In questo caso verranno eseguite due iterazioni durante l'importazione e ognuna restituirà 5000 oggetti da sincronizzare. In tal caso, un nuovo token di accesso verrà richiesto due volte.

Si noti che durante l'esportazione verrà richiesto un nuovo token di accesso per ogni oggetto che deve essere aggiunto/aggiornato/eliminato.

<a name="installing-the-connector"></a>Installazione del connettore
========================

Prima di usare il connettore, verificare che nel server di sincronizzazione siano presenti gli elementi seguenti: Microsoft .NET 4.5.2 Framework o versione successiva, Microsoft Identity Manager 2016 SP1. È necessario usare l'aggiornamento rapido 4.4.1642.0 [KB4021562](https://www.microsoft.com/en-us/download/details.aspx?id=55794) o versione successiva.

I connettori per Microsoft Identity Manager 2016 SP1 e il connettore Graph sono disponibili come download nell'[Area download Microsoft](https://www.microsoft.com/en-us/download/details.aspx?id=51495).

<a name="connector-configuration"></a>Configurazione del connettore
=======================

Pagina Connettività:

![](media/microsoft-identity-manager-2016-ma-graph/77c2eb73bab8d5187da06293938f5fd9.png)

Immagine 5. Pagina Connettività

La pagina Connettività (immagine 1) contiene la versione dell'API Graph usata e il nome del tenant. L'ID client e il segreto client rappresentano l'ID applicazione e il valore della chiave dell'applicazione per le API Web che deve essere creata in Azure AD.

Pagina Parametri globali:

![](media/microsoft-identity-manager-2016-ma-graph/e22d4ee99f2bb825704dd83c1b26dac2.png)

Immagine 6. Pagina Parametri globali (Parametri globali)

La pagina Global Parameters (Parametri globali) contiene le impostazioni seguenti:

DateTime format (Formato data/ora): formato usato per gli attributi con tipo Edm.DateTimeOffset. Tutte le date vengono convertite in stringa e tale formato viene usato durante l'importazione. Il formato impostato viene applicato per qualsiasi attributo per il salvataggio della data.

HTTP timeout (seconds) (Timeout HTTP - secondi): timeout in secondi che verrà usato durante ogni chiamata HTTP all'applicazione per le API Web.

Force change password for created user at next sign (Forza la modifica della password per l'utente creato all'accesso successivo): questa opzione viene usata per il nuovo utente che verrà creato durante l'esportazione. Se l'opzione viene abilitata, la proprietà [forceChangePasswordNextSignIn](https://developer.microsoft.com/en-us/graph/docs/api-reference/v1.0/resources/passwordprofile) verrà impostata su true. In caso contrario, sarà false.

<a name="troubleshooting"></a>Risoluzione dei problemi
===============

**Abilitare i log**

Se si verificano problemi in Graph, si possono usare i log per localizzare il problema. Il connettore Graph usa la stessa origine di tutti i connettori generici. Le tracce possono quindi essere abilitate nello [stesso modo dei connettori generici. Vedere il wiki](https://social.technet.microsoft.com/wiki/contents/articles/21086.fim-2010-r2-troubleshooting-how-to-enable-etw-tracing-for-connectors.aspx). È anche possibile aggiungere quanto segue a miiserver.exe.config (nella sezione system.diagnostics/sources):

\<source name="ConnectorsLog" switchValue="Verbose"\>

\<listeners\>

>   \<add initializeData="ConnectorsLog"   type="System.Diagnostics.EventLogTraceListener, System, Version=4.0.0.0,   Culture=neutral, PublicKeyToken=b77a5c561934e089"   name="ConnectorsLogListener" traceOutputOptions="LogicalOperationStack,   DateTime, Timestamp, Call stack" /\>

\<remove name="Default" /\>

\</listeners\>

\</source\>

Nota: se "Run this management agent in a separate process" (Esegui questo agente di gestione in un processo separato) è abilitato, si deve usare dllhost.exe.config invece di miiserver.exe.config.

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
- [Graph explorer (ideale per la risoluzione dei problemi relativi alle chiamate HTTP)]( https://developer.microsoft.com/en-us/graph/graph-explorer)
- [Versioning, support, and breaking change policies for Microsoft Graph (Criteri di controllo delle versioni, supporto e modifica che causa un'interruzione per Microsoft Graph)](https://developer.microsoft.com/en-us/graph/docs/concepts/versioning_and_support)
- [Scaricare l'agente di gestione di Microsoft Identity Manager per Microsoft Graph (anteprima)](http://go.microsoft.com/fwlink/?LinkId=717495)

<a name="scenario-specific-supported-guides"></a>Linee guida supportate specifiche dello scenario
----------------------------------
[Distribuzione end-to-end B2B MIM]( ~/microsoft-identity-manager-2016-graph-b2b-scenario.md)
