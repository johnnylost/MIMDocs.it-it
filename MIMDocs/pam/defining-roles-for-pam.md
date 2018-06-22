---
title: Definire i ruoli con privilegi per PAM | Documentazione Microsoft
description: Decidere quali ruoli con privilegi devono essere gestiti e definire i criteri di gestione per ognuno.
keywords: ''
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 08/31/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 1a368e8e-68e1-4f40-a279-916e605581bc
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: cfd7c5bee0038740db0ad526072ec248ed9f221d
ms.sourcegitcommit: 210195369d2ecd610569d57d0f519d683ea6a13b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/01/2017
ms.locfileid: "21943758"
---
# <a name="define-roles-for-privileged-access-management"></a>Definire ruoli per Privileged Access Management

Con Privileged Access Management è possibile assegnare utenti a ruoli con privilegi che questi ultimi possono attivare in base alle esigenze per l'accesso JIT. Questi ruoli, definiti manualmente, vengono stabiliti nell'ambiente bastion. Questo articolo illustra il processo per definire i ruoli da gestire tramite PAM e quelli con restrizioni e autorizzazioni appropriate.

Un approccio semplice per la definizione dei ruoli per Privileged Access Management consiste nel compilare tutte le informazioni in un foglio di calcolo. Elencare i ruoli nelle righe e usare le colonne per identificare le autorizzazioni e i requisiti di governance.

I requisiti di governance variano in base all'identità esistente e ai criteri di accesso o ai requisiti di conformità. I parametri per l'identificazione di ogni ruolo possono includere:

- Il proprietario del ruolo.
- Gli utenti candidati al ruolo.
- I controlli di autenticazione, approvazione o notifica che devono essere associati all'uso del ruolo.

Le autorizzazioni del ruolo variano in base alle applicazioni gestite. Questo articolo usa Active Directory come applicazione di esempio, dividendo le autorizzazioni in due categorie:

- Quelle necessarie per gestire il servizio Active Directory (ad esempio, configurare la topologia di replica)

- Quelle necessarie per gestire i dati contenuti in Active Directory (ad esempio, creare utenti e gruppi)

## <a name="identify-roles"></a>Identificare i ruoli

Iniziare identificando tutti i ruoli da gestire con PAM. Nel foglio di calcolo, ogni ruolo potenziale disporrà di una riga.

Per trovare i ruoli appropriati, esaminare tutte le applicazioni nell'ambito di gestione:

- Si tratta dell'applicazione di [livello 0, 1 o 2](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/securing-privileged-access-reference-material)?
- Quali sono i privilegi che influiscono su riservatezza, integrità o disponibilità dell'applicazione?
- L'applicazione ha dipendenze con altri componenti del sistema? Ad esempio, l'applicazione ha dipendenze con database, rete, infrastruttura di sicurezza, piattaforma di virtualizzazione o hosting?

Determinare in che modo raggruppare tali considerazioni sulle app. Si vogliono ruoli con limiti ben definiti e si vogliono assegnare solo le autorizzazioni necessarie per completare le attività amministrative comuni all'interno dell'app.

Si vogliono progettare i ruoli per l'assegnazione dei privilegi minimi. Questo aspetto si può basare sulle responsabilità aziendali correnti (o pianificate) per gli utenti e include il privilegio richiesto dalle attività dell'utente. Può anche includere i privilegi che semplificano le operazioni, senza creare rischi.

Altre informazioni sulla definizione dell'ambito delle autorizzazioni in cui includere un ruolo sono:

- Quante persone lavorano in un determinato ruolo? Se non sono almeno 2, è possibile che la definizione sia troppo ristretta o che sono stati definiti i compiti di un particolare utente.

- Quanti ruoli esegue una persona? Gli utenti sarebbero in grado di selezionare il ruolo più appropriato per la propria attività?

- La popolazione di utenti interagirebbe e in quale modo con le applicazioni compatibili con la gestione accessi con privilegi?

- È possibile separare l'amministrazione e il controllo, in modo che un utente in un ruolo amministrativo non possa cancellare i record di controllo delle proprie azioni?

## <a name="establish-role-governance-requirements"></a>Stabilire i requisiti della governance del ruolo

Non appena si identificano i ruoli del candidato, iniziare a compilare il foglio di calcolo. Creare colonne per i requisiti rilevanti per l'organizzazione. Alcuni requisiti da considerare sono:

- Chi è il proprietario del ruolo che sarà responsabile per un'ulteriore definizione del ruolo, la scelta delle autorizzazioni e il mantenimento delle impostazioni della governance per il ruolo?

- Chi sono titolari del ruolo (utenti) che eseguono le attività o i compiti del ruolo?

- Quale metodo di accesso (descritto nella sezione successiva) sarebbe ideale per i titolari del ruolo?

- È obbligatoria l'approvazione manuale da un proprietario del ruolo quando un utente attiva il proprio ruolo?

- È necessaria la notifica quando un utente attiva il proprio ruolo?

- L’utilizzo di questo ruolo genererà un avviso o una notifica in un sistema SIEM, per la verifica?

- È necessario limitare l’attivazione del ruolo da parte degli utenti solo all’accesso ai computer in cui è necessario accedere per svolgere i compiti del ruolo e in cui è presente una verifica dell'host sufficiente a proteggere credenziali/privilegi da un utilizzo improprio?

- È necessario fornire una workstation amministrativa dedicata ai titolari del ruolo?

- Quali autorizzazioni dell'applicazione (vedere di seguito l'elenco di esempio per AD) sono associate a questo ruolo?

## <a name="select-an-access-method"></a>Selezionare un metodo di accesso

In un sistema di gestione degli accessi tramite privilegi con le stesse autorizzazioni ad essi assegnate, possono essere presenti più ruoli. Questa situazione può verificarsi se diverse comunità di utenti hanno requisiti di governance di accesso differenti. Ad esempio, un'organizzazione può applicare criteri diversi per i propri dipendenti a tempo pieno rispetto ai dipendenti IT in outsourcing di un'altra organizzazione.

In alcuni casi, un utente può essere assegnato in modo permanente a un ruolo. In tal caso, non devono richiedere o attivare un'assegnazione di ruolo. Esempi di scenari di assegnazione permanente:

- Account del servizio gestito nella foresta esistente

- Un account utente nella foresta esistente, con una credenziale gestita al di fuori di PAM. Potrebbe essere un account "break glass". L'account break glass potrebbe necessitare di un ruolo, ad esempio "manutenzione dominio/controller di dominio" per risolvere i problemi quali la relazione di trust e i problemi di integrità del controller di dominio. In quanto account break glass, avrebbe il ruolo assegnato in modo permanente con una password protetta fisicamente)

- Un account utente nella foresta amministrativa che esegue l'autenticazione con una password. Ovvero, un utente che necessita di autorizzazioni amministrative permanenti 24x7 e accede da un dispositivo che non supporta l'autenticazione avanzata.

- Un account utente nella foresta amministrativa, con una smart card o smart card virtuale (ad esempio, un account con una smart card offline, necessaria per attività di manutenzione rare)

Per le organizzazioni che vogliono evitare il rischio di furto di credenziali o dell'uso improprio, la guida [Using Azure MFA for activation](use-azure-mfa-for-activation.md) (Uso di Azure MFA per l'attivazione) include istruzioni su come configurare MIM per richiedere un'ulteriore verifica fuori banda al momento dell'attivazione del ruolo.

## <a name="delegate-active-directory-permissions"></a>Delegare le autorizzazioni di Active Directory

Quando vengono creati nuovi domini, Windows Server crea automaticamente gruppi predefiniti, ad esempio "Domain Admins". Questi gruppi semplificano le attività iniziali e possono essere appropriati per le organizzazioni più piccole. Le organizzazioni di dimensioni più grandi, o quelle che richiedono maggiore livello di isolamento dei privilegi amministrativi, devono svuotare questi gruppi e sostituirli con i gruppi che forniscono autorizzazioni specifiche.

Un limite del gruppo Domain Admins è che non è possibile avere membri da un dominio esterno. Un'altra limitazione è che vengono concesse le autorizzazioni per tre funzioni distinte:

- Gestione del servizio Active Directory
- Gestione dei dati contenuti in Active Directory
- Attivazione dell'accesso remoto in computer che fanno parte del dominio

Al posto di gruppi predefiniti come Domain Admins, creare nuovi gruppi di sicurezza che forniscono solo le autorizzazioni necessarie. È quindi necessario usare Microsoft Identity Manager per fornire in modo dinamico agli account Administrator tali appartenenze ai gruppi.

### <a name="service-management-permissions"></a>Autorizzazioni di gestione del servizio

Nella tabella seguente vengono forniti alcuni esempi di autorizzazioni che sarebbe rilevante includere nei ruoli per la gestione di AD.

| Ruolo | Descrizione |
| ---- | ---- |
| Dominio/manutenzione del controller di dominio | L'appartenenza al gruppo Domain\Administrators consente la risoluzione dei problemi e la modifica del sistema operativo del controller di dominio. Operazioni quali l'innalzamento di livello di un nuovo controller di dominio in un dominio esistente nella foresta e la delega del ruolo di AD.
|Gestione dei controller di dominio virtuali | Gestire macchine virtuali (VM) del controller di dominio (DC) utilizzando il software di gestione della virtualizzazione. Questo privilegio è concesso tramite il controllo completo di tutte le macchine virtuali nello strumento di gestione o tramite la funzionalità di controllo degli accessi in base al ruolo (RBAC). |
| Schema estensione | Gestire lo schema, inclusa l'aggiunta di nuove definizioni di oggetto, modificando le autorizzazioni per gli oggetti dello schema e modificando le autorizzazioni predefinite dello schema per i tipi di oggetto. |
| Database di Active Directory di backup | Eseguire una copia di backup del Database di Active Directory nella sua interezza, inclusi tutti i segreti affidati al controller di dominio e al dominio. |
| Gestire trust e livelli di funzionalità | Creare ed eliminare relazioni di trust con domini esterni e foreste. |
| Gestire siti, subnet e replica | Gestire gli oggetti di topologia di replica di Active Directory come la modifica di siti, subnet e oggetti di collegamento del sito e avviare le operazioni di replica. |
| Gestire oggetti Criteri di gruppo | Creare, eliminare e modificare oggetti Criteri di gruppo in tutto il dominio. |
| Gestire le zone | Creare, eliminare e modificare oggetti e zone DNS in Active Directory. |
| Modificare le OU di livello 0 | Modificare le OU di livello 0 e gli oggetti contenuti in Active Directory |

### <a name="data-management-permissions"></a>Autorizzazioni di gestione dei dati

Nella tabella seguente vengono forniti alcuni esempi di autorizzazioni che sarebbe rilevante includere nei ruoli per la gestione o l'utilizzo dei dati contenuti in AD.

| Ruolo | Descrizione |
| ---- | ---- |
| Modificare la OU amministrativa di livello 1                 | Modificare le OU contenenti oggetti di amministrazione di livello 1 in Active Directory |
| Modificare la OU amministrativa di livello 2                 | Modificare le OU contenenti oggetti di amministrazione di livello 2 in Active Directory |
| Gestione degli account: Creazione/eliminazione/spostamento | Modificare gli account utente standard                                      |
| Gestione degli account: Reimposta Sblocca       | Reimpostare le password e sbloccare gli account                                  |
| Gruppo di sicurezza: Crea Modifica          | Creare e modificare i gruppi di sicurezza in Active Directory              |
| Gruppo di sicurezza: Elimina                 | Eliminare i gruppi di sicurezza in Active Directory                         |
| Gestione oggetto Criteri di gruppo                         | Gestire tutti gli oggetti Criteri di gruppo nel dominio o foresta che non influiscono sui server di livello 0             |
| Aggiungi PC/Amministratore locale                    | Diritti amministrativi locali per tutte le workstation                               |
| Aggiungi Srv/Amministratore locale                   | Diritti amministrativi locali per tutti i server                                    |

## <a name="example-role-definitions"></a>Definizioni di ruolo di esempio

La scelta delle definizioni di ruolo variano a seconda del livello dei server gestiti. Questo dipende anche dalla scelta delle applicazioni gestite. Le applicazioni come prodotti enterprise di Exchange o di terze parti, ad esempio SAP, porteranno le proprie definizioni di ruolo aggiuntive per l'amministrazione delegata.

Nelle sezioni seguenti vengono illustrati esempi di scenari tipici aziendali.

### <a name="tier-0---administrative-forest"></a>Livello 0 - foresta amministrativa

I ruoli appropriati per gli account nell'ambiente bastion possono includere:

- Accesso di emergenza alla foresta amministrativa
- Amministratori "Red card": utenti amministratori della foresta amministrativa
- Utenti amministratori della foresta di produzione
- Utenti a cui sono stati delegati diritti amministrativi limitati per le applicazioni nella foresta di produzione

### <a name="tier-0---enterprise-production-forest"></a>Livello 0 - foresta di produzione aziendale

I ruoli appropriati per la gestione degli account della foresta di produzione di livello 0 e delle risorse possono includere:

- Accesso di emergenza alla foresta di produzione
- Amministratori di criteri di gruppo
- Amministratori DNS
- Amministratori PKI
- Amministratori di replica e di topologia di AD
- Amministratori di virtualizzazione per server di livello 0
- Amministratori di archiviazione
- Amministratori di anti-malware per i server di livello 0
- Amministratori SCCM per SCCM di livello 0
- Amministratori SCOM per SCOM di livello 0
- Amministratori di backup di livello 0
- Utenti di controller di gestione fuori banda e baseboard (per la gestione di KVM o Lights-Out) connessi agli host di livello 0

### <a name="tier-1"></a>Livello 1

I ruoli per la gestione e il backup dei server di livello 1 possono includere:

- Manutenzione del server
- Amministratori di virtualizzazione per server di livello 1
- Account scanner di sicurezza
- Amministratori di anti-malware per i server di livello 1
- Amministratori SCCM per SCCM di livello 1
- Amministratori SCOM per SCOM di livello 1
- Amministratori di backup per server di livello 1
- Utenti di controller di gestione fuori banda e baseboard (per la gestione di KVM o Lights-Out) connessi agli host di livello 1

Inoltre, i ruoli per la gestione di applicazioni aziendali nel livello 1 possono includere:

- Amministratori DHCP
- Amministratori di Exchange
- Amministratori di Skype for Business
- Amministratori di farm di SharePoint
- Amministratori di un servizio cloud, ad esempio, un sito Web aziendale o DNS pubblico
- Amministratori di sistemi HCM, finanziari o legali

### <a name="tier-2"></a>Livello 2

I ruoli per la gestione di utenti e computer non amministrativi possono includere:

- Amministratori degli account
- Supporto tecnico
- Amministratori del gruppo di sicurezza
- Supporto deskside workstation

## <a name="next-steps"></a>Passaggi successivi

- [Materiale di riferimento per la protezione dell'accesso con privilegi](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/securing-privileged-access-reference-material)
- [Uso di Azure MFA per l'attivazione](use-azure-mfa-for-activation.md)
