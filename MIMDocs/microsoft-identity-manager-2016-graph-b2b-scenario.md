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
ms.openlocfilehash: ac11a4dfb23944d50dbbcf0b0d70c915f186c159
ms.sourcegitcommit: c773edc8262b38df50d82dae0f026bb49500d0a4
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/05/2018
ms.locfileid: "34479163"
---
<a name="azure-ad-business-to-business-b2b-collaboration-with-microsoft-identity-managermim-2016-sp1-with-azure-application-proxy-public-preview"></a>Collaborazione Business to Business (B2B) di Azure AD con Microsoft Identity Manager(MIM) 2016 SP1 con il proxy di applicazione Azure (anteprima pubblica)
============================================================================================================================

Lo scenario iniziale in anteprima è la gestione del ciclo di vita degli account AD degli utenti esterni.   In questo scenario un'organizzazione ha invitato alcuni guest nella directory di Azure AD e vuole concedere a tali guest l'accesso alle applicazioni locali basate su Kerberos o sull'autenticazione integrata di Windows, tramite il proxy di [applicazione Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-publish) o altri meccanismi gateway. Il proxy di applicazione Azure AD richiede che ogni utente abbia il proprio account Active Directory Domain Services, per scopi di identificazione e delega

## <a name="scenario-specific-supported-guidance"></a>Linee guida supportate specifiche dello scenario

In questo scenario un'organizzazione ha invitato alcuni guest nella directory di Azure AD e vuole concedere a tali guest l'accesso alle applicazioni locali basate su Kerberos o sull'autenticazione integrata di Windows, tramite il proxy di [applicazione Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-publish) o altri meccanismi gateway. Il proxy di applicazione Azure AD richiede che ogni utente abbia il proprio account Active Directory Domain Services, per scopi di identificazione e delega

Alcuni presupposti della configurazione di B2B con MIM e il proxy di applicazione Azure

-   L'[agente di gestione di Graph](microsoft-identity-manager-2016-connector-graph.md) è già stato installato.

-   È disponibile una configurazione di AD e Azure AD Connect locale per la sincronizzazione di utenti e gruppi con Azure AD.

    -   Gruppi di Office che controllano l'accesso all'applicazione con [Azure AD Connect](http://robsgroupsblog.com/blog/how-to-write-back-an-office-group-in-azure-active-directory-to-a-mail-enabled-security-group-in-an-on-premises-active-directory)

-   I connettori e i gruppi di connettori del proxy di applicazione sono già stati configurati. In caso contrario, è possibile vedere [qui](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-enable#install-and-register-a-connector) per installarli e configurarli

-   È stata pubblicata una o più applicazioni, basate sull'Autenticazione integrata di Windows o su singoli account AD tramite il proxy di app Azure

-   È stato invitato o si invita uno o più guest, creati in Azure AD <https://docs.microsoft.com/azure/active-directory/active-directory-b2b-self-service-portal>

-   Microsoft Identity Manager è installato e si usa la configurazione di base del servizio, del portale e dell'agente di gestione di Active Directory.
    <https://docs.microsoft.com/microsoft-identity-manager/microsoft-identity-manager-deploy>

## <a name="b2b-end-to-end-deployment"></a>Distribuzione end-to-end B2B

Scenario

Contoso Pharmaceuticals gestisce Trey Research Inc. all'interno del reparto di ricerca e sviluppo. I dipendenti di Trey Research devono poter accedere all'applicazione di creazione di report sulla ricerca fornita da Contoso Pharmaceuticals.

-   Contoso Pharmaceuticals è in un tenant indipendente, con un dominio personalizzato configurato.

-   Un utente esterno è stato invitato nel tenant di Contoso Pharmaceuticals.
    Questo utente ha accettato l'invito e può accedere alle risorse condivise.

-   Sono stati pubblicati un'applicazione tramite il proxy di app e, in questo scenario, scenari di supporto tecnico di esempio, come esempio di uso del servizio MIM e del portale per consentire all'utente guest di partecipare al processo MIM.

## <a name="create-the-graph-management-agent"></a>Creare l'agente di gestione di Graph

Nota: prima di creare il connettore, vedere [Agente di gestione di Graph](microsoft-identity-manager-2016-connector-graph.md).

Nell'interfaccia utente di Synchronization Service Manager selezionare **Connettori** e **Crea**.
Selezionare **Graph (Microsoft)** e assegnargli un nome descrittivo

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/d95c6b2cc7951b607388cbd25920d7d0.png)

### <a name="connectivity"></a>Connettività

Nella pagina Connettività per la versione dell'API Graph è necessario specificare **V 1.0**, se pronta per la produzione, oppure **Beta**, se non di produzione

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/6fabfe20af0207f1556f0df18fd16f60.png)

### <a name="capabilities"></a>Funzionalità

Nella pagina Global Parameters (Parametri globali) si configura il DN per il log delle modifiche differenziali e le altre funzionalità LDAP. La pagina è già popolata con le informazioni fornite dal server LDAP.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/84c4dd62f63b82239cd0cf63d14fc671.png)

### <a name="global-parameters"></a>Parametri globali

Nella pagina Global Parameters (Parametri globali) si configura il DN per il log delle modifiche differenziali e le altre funzionalità LDAP. La pagina è già popolata con le informazioni fornite dal server LDAP.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/84c4dd62f63b82239cd0cf63d14fc671.png)

### <a name="configure-provisioning-hierarchy"></a>Configurazione della gerarchia di provisioning

Questa pagina viene usata per eseguire il mapping del componente DN, ad esempio l'unità organizzativa, al tipo di oggetto di cui deve essere effettuato il provisioning, ad esempio organizationalUnit. Lasciare le impostazioni predefinite. Fare clic su Avanti

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/80016dc45b50a0b1b08ea51ad8b37977.png)

### <a name="configure-partitions-and-hierarchies"></a>Configurare partizioni e gerarchie

Nella pagina delle partizioni e gerarchie selezionare tutti gli spazi dei nomi con gli oggetti che si prevede di importare ed esportare.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/72f0adc789ed78c66d066768146fb874.png)

#### <a name="select-object-types"></a>Selezionare i tipi di oggetti

Nella pagina delle partizioni e gerarchie selezionare tutti gli spazi dei nomi con gli oggetti che si prevede di importare ed esportare.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e18921f65a0d0e4acf0775c8a01ac009.png)

#### <a name="select-attributes"></a>Selezione degli attributi

Nella schermata Seleziona attributi selezionare gli attributi necessari per gestire gli utenti B2B. L'attributo "id" è obbligatorio

-   ID

-   displayName

-   mail

-   givenName

-   surname

-   userPrincipalName

-   userType

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/58da80f5475cf01a97a6843dd279385c.png)

#### <a name="configure-anchors"></a>Configurazione ancoraggi

La configurazione degli ancoraggi è un passaggio obbligatorio. Per impostazione predefinita, usare l'attributo id per il mapping.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/9377ab7b760221517a431384689c8c76.png)

#### <a name="configure-connector-filter"></a>Configurare il filtro del connettore

La pagina Configura filtro connettore consente di filtrare gli oggetti in base al filtro degli attributi. In questo scenario per B2B l'obiettivo è quello di introdurre solo gli utenti con userType uguale a Guest e non Member.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/d90691fce652ba41c7a98c9a863ee710.png)

#### <a name="configure-join-and-projection-rules"></a>Configurare le regole di aggiunta e proiezione

La configurazione delle regole di unione e proiezione viene gestita dalla regola di sincronizzazione. Non è necessario identificare un'unione e una proiezione nel connettore. Lasciare l'impostazione predefinita e fare clic su OK.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/34896440ae6ad404e824eb35d8629986.png)

#### <a name="configure-attribute-flow"></a>Configurare il flusso di attributi

Come per l'aggiunta e la proiezione, non è necessario definire il flusso di attributi, perché viene gestito dalla regola di sincronizzazione che verrà creata in seguito. Lasciare l'impostazione predefinita e fare clic su OK.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/b7cd0d294d4f361f0551bf2cb774d5f5.png)

#### <a name="configure-deprovision"></a>Configurare il deprovisioning

La configurazione del deprovisioning consente di eliminare l'oggetto se l'oggetto metaverse viene eliminato. In questo test li si imposta come sezionatori perché l'obiettivo è di lasciarli in Azure. Non verranno inoltre esportati elementi in Azure, ma solo importati.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/2394ad4d11546c6a5c69a6dad56fe6ca.png)

#### <a name="configure-extensions"></a>Configurazione delle estensioni

La configurazione delle estensioni in questo agente di gestione non è un'opzione obbligatoria, perché si usa una regola di sincronizzazione. Se in precedenza si è deciso per una regola avanzata nel flusso di attributi, sarà invece necessario definire questa opzione.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/74513d95b10f6ce47b7ac75fe7ab9889.png)

## <a name="creating-mim-service-synchronization-rules"></a>Creazione di regole di sincronizzazione del servizio MIM

Nei passaggi seguenti si inizia il mapping dell'account guest B2B e del flusso di attributi. Si presume che l'agente di gestione di Active Directory sia già stato configurato e che l'agente di gestione FIM sia già stato configurato per comunicare con il servizio e il portale MIM.

Prima di creare la regola di sincronizzazione, è necessario creare un attributo denominato userPrincipalName associato all'oggetto person che usa la finestra di progettazione MV.

Nel client di sincronizzazione selezionare Metaverse Designer (Progettazione metaverse)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/db3c1d353168a09aaa68678d39ea4f09.png)

Selezionare quindi il tipo di oggetto person

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/b5e3db86398aed558a481dd64be4f5db.png)

In Azioni fare quindi clic su Aggiungi attributo

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/47d0056eb496edd2e7b5da11a2c04718.png)

Completare infine i dettagli seguenti

Nome attributo: **userPrincipalName**

Tipo attributo: **String (Indexable)**

Indicizzato = **True**

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/9fba1ff9feefb17b82478ac7010edbfa.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e389ee78beac3bf469ddd97bddb5e9d5.png)

I passaggi successivi richiederanno la configurazione minima dell'agente di gestione del servizio FIM e dell'agente di gestione di Active Directory Domain Services.

Per altre informazioni sulla configurazione, vedere How Do I Provision Users to AD DS (Come effettuare il provisioning degli utenti in Active Directory Domain Services) all'indirizzo <https://technet.microsoft.com/library/ff686263(v=ws.10).aspx>

### <a name="synchronization-rule-import-guest-user-to-mv-to-synchronization-service-metaverse-from-azure-active-directorybr"></a>Regola di sincronizzazione: importare un utente guest nel servizio di sincronizzazione metaverse da Azure Active Directory<br>

Passare al servizio e portale MIM, selezionare Sycronization Rules (Regole di sincronizzazione) e fare clic su Nuovo.
![](media/microsoft-identity-manager-2016-graph-b2b-scenario/ba39855f54268aa824cd8d484bae83cf.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/de059b93474c39763f0b27874b716e15.png) Selezionare "Crea risorsa in FIM" ![](media/microsoft-identity-manager-2016-graph-b2b-scenario/9bc4a92136be1557d3596fa2eaa63e61.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/0ac7f4d0fd55f4bffd9e6508b494aa74.png)

Regole di flusso:

| **Solo flusso iniziale** | **Utilizza come test di esistenza** | **Flusso (attributo destinazione ⇒ valore FIM)**                          |
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

Questa regola di sincronizzazione crea l'utente guest in Active Directory

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
| **S**                 |                           | ["CN="+uid+",OU=B2BGuest,DC=scontoso,DC=com"⇒dn](javascript:void(0);) |
| **S**                 |                           | [RandomNum(0,999)+userPrincipalName⇒unicodePwd](javascript:void(0);)  |
| **S**                 |                           | [262656⇒userAccountControl](javascript:void(0);)                      |

### <a name="synchronization-rule-import-b2b-guest-user-objects-sid-to-allow-for-login-to-mim"></a>Regola di sincronizzazione: importare SID oggetti utente guest B2B per consentire l'accesso a MIM 

Questa regola di sincronizzazione crea l'utente guest in Active Directory

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/263df23fd588c4229b958aee240071f3.png)


![](media/microsoft-identity-manager-2016-graph-b2b-scenario/047ebc3084999246bdd44b1f05ca02b3.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/acc0a871c0bf6f45ee928bed5abd9861.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/80fb9d563ec088925477a645f19b0373.png)

Si invita infine l'utente e quindi si esegue la gestione nell'ordine seguente:

-   Importazione completa e sincronizzazione dell'agente di gestione MIMMA

-   Importazione completa e sincronizzazione dell'agente di gestione ADMA_SCONTOSO_B2B

-   Importazione completa e sincronizzazione dell'agente di gestione B2B Graph

-   Esportazione, importazione differenziale e sincronizzazione dell'agente di gestione ADMA_SCONTOSO_B2B

-   Esportazione, importazione differenziale e sincronizzazione dell'agente di gestione MIMMA

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/506f0a093c8b58cbb62cc4341b251564.png)

## <a name="finally-application-proxy-with-b2b-guest-and-logging-into-mim"></a>Ultimo passaggio: proxy di applicazione con guest B2B e accesso a MIM

Ora che sono state create le regole di sincronizzazione in MIM, nella configurazione del proxy di app definire il principio cloud per consentire KCD nel proxy di app.
Aggiungere anche manualmente l'utente agli utenti e gruppi da gestire. Le opzioni non visualizzano l'utente finché non è stata eseguita la creazione in MIM. Per aggiungere l'utente guest a un gruppo di Office dopo il provisioning, è necessario proseguire con la configurazione, ma tale procedura non è illustrata in questo documento.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/d0f0b253dbbc5edaf22b22f30f94dd3b.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/a284e69bb14ca19217954881ba860cf2.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/0c2361d137f3efcad9139069c0abcb4d.png)

| **Solo flusso iniziale** | **Utilizza come test di esistenza** | **Flusso (attributo destinazione ⇒ valore FIM)**                          |
|-----------------------|---------------------------|-----------------------------------------------------------------------|
|                       |                           | [sAMAccountName⇒accountName](javascript:void(0);)                     |
|                       |                           | ["CONTOSO"⇒domain](javascript:void(0);)                            |
|                       |                           | [objectSid⇒objectSid](javascript:void(0);)                                      |

La configurazione è stata completata

L'utente B2B può infine eseguire l'accesso e visualizzare l'applicazione

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/275fc989d20d2598df55cde4b4524dca.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e1a9d7b8c87021de4e43a3501826fa81.png)

<a name="next-steps"></a>Passaggi successivi
----------

[Come si effettua il provisioning di utenti ai servizi di dominio di Active Directory](https://technet.microsoft.com/library/ff686263(v=ws.10).aspx)

[Riferimento alle funzioni per FIM 2010](https://technet.microsoft.com/library/ff800820(v=ws.10).aspx)

[Come fornire l'accesso remoto sicuro alle applicazioni locali](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-get-started)

[Scaricare l'agente di gestione di Microsoft Identity Manager per Microsoft Graph (anteprima)](http://go.microsoft.com/fwlink/?LinkId=717495)
