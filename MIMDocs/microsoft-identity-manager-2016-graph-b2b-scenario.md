---
title: Configurazione del connettore di Microsoft Identity Manager per Microsoft Graph per B2B | Microsoft Docs
author: billmath
description: Il connettore di Microsoft Graph esegue la gestione del ciclo di vita degli account AD degli utenti esterni. In questo scenario un'organizzazione ha invitato alcuni guest nella directory di Azure AD e vuole concedere a tali guest l'accesso alle applicazioni locali basate su Kerberos o sull'autenticazione integrata di Windows
keywords: ''
ms.author: billmath
manager: mtillman
ms.date: 10/02/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 94a74f1c-2192-4748-9a25-62a526295338
ms.openlocfilehash: 139c58510117ad422529a4ff0facd23040023713
ms.sourcegitcommit: 7de35aaca3a21192e4696fdfd57d4dac2a7b9f90
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/16/2018
ms.locfileid: "49358772"
---
<a name="azure-ad-business-to-business-b2b-collaboration-with-microsoft-identity-managermim-2016-sp1-with-azure-application-proxy"></a>Collaborazione Business to Business (B2B) di Azure AD con Microsoft Identity Manager(MIM) 2016 SP1 con il proxy di applicazione Azure
============================================================================================================================

Lo scenario iniziale è la gestione del ciclo di vita degli account AD degli utenti esterni.   In questo scenario un'organizzazione ha invitato alcuni guest nella directory di Azure AD e vuole concedere a tali guest l'accesso alle applicazioni locali basate su Kerberos o sull'autenticazione integrata di Windows, tramite il proxy di [applicazione Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-publish) o altri meccanismi gateway. Il proxy di applicazione Azure AD richiede che ogni utente abbia il proprio account Active Directory Domain Services, per scopi di identificazione e delega.

## <a name="scenario-specific-guidance"></a>Guida specifica per lo scenario

Alcuni presupposti della configurazione di B2B con MIM e il proxy di applicazione Azure AD:

-   È già stata distribuita un'istanza locale di Active Directory, Microsoft Identity Manager è installato e ci sono configurazioni di base del servizio MIM, del portale MIM, di Active Directory Management Agent (ADMA) e di FIM Management Agent.
    <https://docs.microsoft.com/microsoft-identity-manager/microsoft-identity-manager-deploy>

-   Sono già state seguite le istruzioni dell'articolo su come scaricare e installare il [connettore Graph](microsoft-identity-manager-2016-connector-graph.md).

-   Azure AD Connect è configurato per la sincronizzazione di utenti e gruppi con Azure AD.

-   Azure AD Connect è configurato per la sincronizzazione dei gruppi di Office per controllare il ritorno dell'applicazione a [Active Directory Domain Services locale](http://robsgroupsblog.com/blog/how-to-write-back-an-office-group-in-azure-active-directory-to-a-mail-enabled-security-group-in-an-on-premises-active-directory)

-   I connettori e i gruppi di connettori del proxy di applicazione sono già stati configurati. In caso contrario, è possibile vedere [qui](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-enable#install-and-register-a-connector) per installarli e configurarli

-   Sono già state pubblicate una o più applicazioni, basate sull'Autenticazione integrata di Windows o su singoli account AD tramite il proxy app Azure AD

-   Sono stati invitati o si invita uno o più guest, che hanno generato la creazione di uno o più utenti in Azure AD <https://docs.microsoft.com/azure/active-directory/active-directory-b2b-self-service-portal>



## <a name="b2b-end-to-end-deployment-example-scenario"></a>Scenario di esempio della distribuzione end-to-end B2B

In questa guida si presuppone lo scenario seguente:

Contoso Pharmaceuticals collabora con Trey Research Inc. all'interno del reparto di ricerca e sviluppo. I dipendenti di Trey Research devono poter accedere all'applicazione di creazione di report sulla ricerca fornita da Contoso Pharmaceuticals.

-   Contoso Pharmaceuticals è in un tenant indipendente, con un dominio personalizzato configurato.

-   Un utente esterno è stato invitato nel tenant di Contoso Pharmaceuticals.
    Questo utente ha accettato l'invito e può accedere alle risorse condivise.

-   Contoso Pharmaceuticals ha pubblicato un'app tramite il proxy app. In questo scenario, l'applicazione di esempio è il portale MIM. Questo consente a un utente guest di partecipare ai processi MIM, ad esempio le attività del supporto tecnico o la richiesta di accesso ai gruppi in MIM.


## <a name="configure-ad-and-azure-ad-connect-to-exclude-users-added-from-azure-ad"></a>Configurare Active Directory e Azure AD Connect per escludere gli utenti aggiunti da Azure AD

Per impostazione predefinita, Azure AD Connect presuppone che gli utenti non amministratori di Active Directory debbano essere sincronizzati in Azure AD.  Se Azure AD Connect rileva un utente in Azure AD che corrisponde all'utente nell'istanza Active Directory locale, Azure AD Connect creerà una corrispondenza tra i due account e, presupponendo che si tratta di una sincronizzazione precedente dell'utente, renderà l'istanza AD locale autorevole.  Tuttavia, questo comportamento predefinito non è adatto al flusso di B2B, in cui l'account utente ha origine in Azure AD. 

Di conseguenza, gli utenti che MIM importa nell'istanza Active Directory Domain Services da Azure AD devono essere archiviati in un modo che impedisca ad Azure AD di tentare di sincronizzare questi utenti in Azure AD.
Un modo per raggiungere questo scopo consiste nel creare una nuova unità organizzativa in Active Directory Domain Services e configurare l'esclusione di tale unità in Azure AD Connect.  

Altre informazioni sono reperibili in [Servizio di sincronizzazione Azure AD Connect: Configurare il filtro](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnectsync-configure-filtering). 
 

## <a name="create-the-azure-ad-application"></a>Creare l'applicazione di Azure AD 


Nota: prima di creare l'agente di gestione per il connettore Graph nel servizio di sincronizzazione MIM, leggere la guida alla distribuzione del [connettore Graph](microsoft-identity-manager-2016-connector-graph.md) e creare un'applicazione con un client ID e un segreto.
Assicurarsi che l'applicazione sia stata autorizzata per almeno una di queste autorizzazioni: `User.Read.All`, `User.ReadWrite.All`, `Directory.Read.All` o `Directory.ReadWrite.All`. 

## <a name="create-the-new-management-agent"></a>Creare il nuovo agente di gestione


Nell'interfaccia utente di Synchronization Service Manager selezionare **Connettori** e **Crea**.
Selezionare **Graph (Microsoft)** e assegnargli un nome descrittivo.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/d95c6b2cc7951b607388cbd25920d7d0.png)

### <a name="connectivity"></a>Connettività

Nella pagina Connettività è necessario specificare la versione dell'API Graph. Il PAI per produzione è **V 1.0**, non di produzione è **Beta**.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/6fabfe20af0207f1556f0df18fd16f60.png)

### <a name="global-parameters"></a>Parametri globali

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/84c4dd62f63b82239cd0cf63d14fc671.png)

### <a name="configure-provisioning-hierarchy"></a>Configurazione della gerarchia di provisioning

Questa pagina viene usata per eseguire il mapping del componente DN, ad esempio l'unità organizzativa, al tipo di oggetto di cui deve essere effettuato il provisioning, ad esempio organizationalUnit. Ciò non è necessario per questo scenario, quindi lasciare l'impostazione predefinita e fare clic su Avanti.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/80016dc45b50a0b1b08ea51ad8b37977.png)

### <a name="configure-partitions-and-hierarchies"></a>Configurare partizioni e gerarchie

Nella pagina delle partizioni e gerarchie selezionare tutti gli spazi dei nomi con gli oggetti che si prevede di importare ed esportare.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/72f0adc789ed78c66d066768146fb874.png)

#### <a name="select-object-types"></a>Selezionare i tipi di oggetti

Nella pagina Tipi di oggetto, selezionare i tipi di oggetto che si intende importare. Selezionare almeno "Utente".

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e18921f65a0d0e4acf0775c8a01ac009.png)

#### <a name="select-attributes"></a>Selezione degli attributi

Nella schermata Seleziona attributi selezionare gli attributi di Azure AD che saranno necessari per gestire gli utenti B2B in Active Directory. L'attributo "ID" è obbligatorio.  Gli attributi `userPrincipalName` e `userType` verranno usati più avanti in questa configurazione.  Gli altri attributi sono facoltativi, tra cui

-   `displayName`

-   `mail`

-   `givenName`

-   `surname`

-   `userPrincipalName`

-   `userType`

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/58da80f5475cf01a97a6843dd279385c.png)

#### <a name="configure-anchors"></a>Configurazione ancoraggi

Nella schermata di configurazione degli ancoraggi, la configurazione dell'attributo di ancoraggio è un passaggio obbligatorio. Per impostazione predefinita, usare l'attributo ID per il mapping dell'utente.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/9377ab7b760221517a431384689c8c76.png)

#### <a name="configure-connector-filter"></a>Configurare il filtro del connettore

Nella pagina Configura filtro connettore MIM consente di filtrare gli oggetti in base al filtro degli attributi. In questo scenario per B2B l'obiettivo è di introdurre solo gli utenti in cui il valore dell'attributo `userType` è uguale a `Guest` e non gli utenti con userType uguale a `member`.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/d90691fce652ba41c7a98c9a863ee710.png)

#### <a name="configure-join-and-projection-rules"></a>Configurare le regole di aggiunta e proiezione

In questa guida si presuppone che verrà creata una regola di sincronizzazione.  Poiché la configurazione delle regole di unione e proiezione viene gestita dalla regola di sincronizzazione, non è necessario identificare un'unione e una proiezione nel connettore. Lasciare l'impostazione predefinita e fare clic su OK.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/34896440ae6ad404e824eb35d8629986.png)

#### <a name="configure-attribute-flow"></a>Configurare il flusso di attributi

In questa guida si presuppone che verrà creata una regola di sincronizzazione.  La proiezione non è necessaria per definire il flusso di attributi nella sincronizzazione MIM perché viene gestito dalla regola di sincronizzazione che verrà creata in seguito. Lasciare l'impostazione predefinita e fare clic su OK.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/b7cd0d294d4f361f0551bf2cb774d5f5.png)

#### <a name="configure-deprovision"></a>Configurare il deprovisioning

L'impostazione per configurare il deprovisioning consente di configurare la sincronizzazione MIM per eliminare l'oggetto, se l'oggetto del metaverse va eliminato. In questo scenario si creano dei sezionatori perché l'obiettivo è di lasciarli in Azure AD. In questo scenario non è prevista l'esportazione di alcunché in Azure AD e il connettore è configurato per la sola importazione.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/2394ad4d11546c6a5c69a6dad56fe6ca.png)

#### <a name="configure-extensions"></a>Configurazione delle estensioni

La configurazione delle estensioni in questo agente di gestione non è un'opzione obbligatoria, perché si usa una regola di sincronizzazione. Se in precedenza si fosse deciso per una regola avanzata nel flusso di attributi, sarebbe presente un'opzione per definire l'estensione della regola.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/74513d95b10f6ce47b7ac75fe7ab9889.png)

## <a name="extending-the-metaverse-schema"></a>Estensione dello schema metaverse


Prima di creare la regola di sincronizzazione, è necessario creare un attributo denominato userPrincipalName associato all'oggetto person che usa la finestra di progettazione MV.

Nel client di sincronizzazione selezionare Metaverse Designer (Progettazione metaverse)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/db3c1d353168a09aaa68678d39ea4f09.png)

Selezionare quindi il tipo di oggetto person

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/b5e3db86398aed558a481dd64be4f5db.png)

In Azioni fare quindi clic su Aggiungi attributo

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/47d0056eb496edd2e7b5da11a2c04718.png)

Completare i dettagli seguenti

Nome attributo: **userPrincipalName**

Tipo attributo: **String (Indexable)**

Indicizzato = **True**

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/9fba1ff9feefb17b82478ac7010edbfa.png)

## <a name="creating-mim-service-synchronization-rules"></a>Creazione di regole di sincronizzazione del servizio MIM

Nei passaggi seguenti si inizia il mapping dell'account guest B2B e del flusso di attributi. Si presume che Active Directory Management Agent sia già stato configurato e che FIM Management Agent sia configurato per importare utenti nel servizio e nel portale MIM.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e389ee78beac3bf469ddd97bddb5e9d5.png)

I passaggi seguenti richiederanno l'aggiunta di una configurazione minima di Active Directory Management Agent e di FIM Management Agent.

Per altre informazioni sulla configurazione, vedere How Do I Provision Users to AD DS (Come effettuare il provisioning degli utenti in Active Directory Domain Services) all'indirizzo <https://technet.microsoft.com/library/ff686263(v=ws.10).aspx>

### <a name="synchronization-rule-import-guest-user-to-mv-to-synchronization-service-metaverse-from-azure-active-directorybr"></a>Regola di sincronizzazione: importare un utente guest nel servizio di sincronizzazione metaverse da Azure Active Directory<br>

Passare al portale MIM, selezionare Regole di sincronizzazione e fare clic su Nuovo.  Creare una regola di sincronizzazione in ingresso per il flusso B2B tramite il connettore Graph.
![](media/microsoft-identity-manager-2016-graph-b2b-scenario/ba39855f54268aa824cd8d484bae83cf.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/de059b93474c39763f0b27874b716e15.png)

Al passaggio dei criteri di relazione, selezionare "Crea risorsa in FIM".
![](media/microsoft-identity-manager-2016-graph-b2b-scenario/9bc4a92136be1557d3596fa2eaa63e61.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/0ac7f4d0fd55f4bffd9e6508b494aa74.png)

Configurare le seguenti regole del flusso in ingresso dell'attributo.  Assicurarsi di compilare gli attributi `accountName`, `userPrincipalName` e `uid` perché verranno usati più avanti in questo scenario:

| **Solo flusso iniziale** | **Utilizza come test di esistenza** | **Flusso (valore di origine ⇒ attributo FIM)**                          |
|-----------------------|---------------------------|-----------------------------------------------------------------------|
|                       |                           | [displayName⇒displayName](javascript:void(0);)                        |
|                       |                           | [Left(id,20)⇒accountName](javascript:void(0);)                        |
|                       |                           | [id⇒uid](javascript:void(0);)                                         |
|                       |                           | [userType⇒employeeType](javascript:void(0);)                          |
|                       |                           | [givenName⇒givenName](javascript:void(0);)                            |
|                       |                           | [surname⇒sn](javascript:void(0);)                                     |
|                       |                           | [userPrincipalName⇒userPrincipalName](javascript:void(0);)            |
|                       |                           | [id⇒cn](javascript:void(0);)                                          |
|                       |                           | [mail⇒mail](javascript:void(0);)                                      |
|                       |                           | [mobilePhone⇒mobilePhone](javascript:void(0);)                        |

### <a name="synchronization-rule-create-guest-user-account-to-active-directory"></a>Regola di sincronizzazione: creare un account utente guest in Active Directory 

Questa regola di sincronizzazione crea l'utente in Active Directory.  Verificare che il flusso per `dn` inserisca l'utente nell'unità organizzativa che è stata esclusa da Azure AD Connect.  Inoltre, aggiornare il flusso per `unicodePwd` in funzione dei criteri password di AD. Non è necessario che l'utente conosca la password.  Si noti che il valore di `262656` per `userAccountControl` codifica i flag `SMARTCARD_REQUIRED` e `NORMAL_ACCOUNT`.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/3463e11aeb9fb566685e775d4e1b825c.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e53c7ac0f376382d890e79dad597c284.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/1c4fad7aa68dc9697fda8f811e9ad37b.png)

Regole di flusso:

| **Solo flusso iniziale** | **Utilizza come test di esistenza** | **Flusso (attributo destinazione ⇒ valore FIM)**                          |
|-----------------------|---------------------------|-----------------------------------------------------------------------|
|                       |                           | [accountName⇒sAMAccountName](javascript:void(0);)                     |
|                       |                           | [givenName⇒givenName](javascript:void(0);)                            |
|                       |                           | [mail⇒mail](javascript:void(0);)                                      |
|                       |                           | [sn⇒sn](javascript:void(0);)                                          |
|                       |                           | [userPrincipalName⇒userPrincipalName](javascript:void(0);)            |
| **S**                 |                           | ["CN="+uid+",OU=B2BGuest,DC=contoso,DC=com"⇒dn](javascript:void(0);) |
| **S**                 |                           | [RandomNum(0,999)+userPrincipalName⇒unicodePwd](javascript:void(0);)  |
| **S**                 |                           | [262656⇒userAccountControl](javascript:void(0);)                      |

### <a name="optional-synchronization-rule-import-b2b-guest-user-objects-sid-to-allow-for-login-to-mim"></a>Regola di sincronizzazione facoltativa: importare SID oggetti utente guest B2B per consentire l'accesso a MIM 

Questa regola di sincronizzazione in ingresso riporta l'attributo SID dell'utente da Active Directory a MIM, in modo che l'utente possa accedere al portale MIM.  Il portale MIM richiede che gli attributi `samAccountName`, `domain` e `objectSid` dell'utente siano compilati nel database del servizio MIM.

Configurare il sistema esterno di origine come `ADMA`, in quanto l'attributo `objectSid` verrà impostato automaticamente da Active Directory quando MIM crea l'utente.
 
Si noti che se si configura la creazione di utenti nel servizio MIM, è necessario assicurarsi che non siano nell'ambito di alcun set destinato alle regole dei criteri di gestione della reimpostazione password self-service dei dipendenti.  A questo scopo potrebbe essere necessario modificare le definizioni dei set per escludere gli utenti che sono stati creati dal flusso B2B. 

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/263df23fd588c4229b958aee240071f3.png)


![](media/microsoft-identity-manager-2016-graph-b2b-scenario/047ebc3084999246bdd44b1f05ca02b3.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/acc0a871c0bf6f45ee928bed5abd9861.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/80fb9d563ec088925477a645f19b0373.png)


| **Solo flusso iniziale** | **Utilizza come test di esistenza** | **Flusso (valore di origine ⇒ attributo FIM)**                          |
|-----------------------|---------------------------|-----------------------------------------------------------------------|
|                       |                           | [sAMAccountName⇒accountName](javascript:void(0);)                     |
|                       |                           | ["CONTOSO"⇒domain](javascript:void(0);)                            |
|                       |                           | [objectSid⇒objectSid](javascript:void(0);)                                      |


## <a name="run-the-synchronization-rules"></a>Eseguire le regole di sincronizzazione

Si invita quindi l'utente e si eseguono le regole di sincronizzazione dell'agente di gestione nell'ordine seguente:

-   Importazione completa e sincronizzazione dell'agente di gestione `MIMMA`.  Ciò garantisce che le regole di sincronizzazione della sincronizzazione MIM siano le più recenti.

-   Importazione completa e sincronizzazione dell'agente di gestione `ADMA`.  Ciò garantisce che ci sia concordanza tra MIM e Active Directory.  A questo punto, non ci saranno ancora esportazioni in sospeso per gli utenti guest.

-   Importazione completa e sincronizzazione dell'agente di gestione B2B Graph.  Questa operazione importa gli utenti guest nel metaverse.  A questo punto, ci saranno uno o più account in attesa di esportazione per `ADMA`.  Se non ci sono esportazioni in sospeso, verificare che gli utenti guest siano stati importati nello spazio connettore e che le regole siano state configurate per poter assegnare agli utenti gli account AD.

-   Esportazione, importazione differenziale e sincronizzazione dell'agente di gestione `ADMA`.  Se le esportazioni non sono riuscite, verificare la configurazione della regola e determinare se mancavano alcuni requisiti dello schema. 

-   Esportazione, importazione differenziale e sincronizzazione dell'agente di gestione `MIMMA`.  Al termine dell'operazione non dovrebbero più esserci esportazioni in sospeso.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/506f0a093c8b58cbb62cc4341b251564.png)


## <a name="optional-application-proxy-for-b2b-guests-logging-into-mim-portal"></a>Facoltativo: proxy app per gli utenti guest B2B che accedono al portale MIM

Ora che sono state create le regole di sincronizzazione in MIM, Nella configurazione del proxy app definire l'uso dell'entità cloud per autorizzare KCD nel proxy app.
Aggiungere anche manualmente l'utente agli utenti e gruppi da gestire. Le opzioni non visualizzano l'utente finché non è stata eseguita la creazione in MIM. Per aggiungere l'utente guest a un gruppo di Office dopo il provisioning, è necessario proseguire con la configurazione, ma tale procedura non è illustrata in questo documento.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/d0f0b253dbbc5edaf22b22f30f94dd3b.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/a284e69bb14ca19217954881ba860cf2.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/0c2361d137f3efcad9139069c0abcb4d.png)


Quando la configurazione è terminata, chiedere all'utente B2B di eseguire l'accesso e visualizzare l'applicazione.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/275fc989d20d2598df55cde4b4524dca.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e1a9d7b8c87021de4e43a3501826fa81.png)

<a name="next-steps"></a>Passaggi successivi
----------

[Come si effettua il provisioning di utenti ai servizi di dominio di Active Directory](https://technet.microsoft.com/library/ff686263(v=ws.10).aspx)

[Riferimento alle funzioni per FIM 2010](https://technet.microsoft.com/library/ff800820(v=ws.10).aspx)

[Come fornire l'accesso remoto sicuro alle applicazioni locali](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-get-started)

[Scaricare il connettore di Microsoft Identity Manager per Microsoft Graph](http://go.microsoft.com/fwlink/?LinkId=717495)
