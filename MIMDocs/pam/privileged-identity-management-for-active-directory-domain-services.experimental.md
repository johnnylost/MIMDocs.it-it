---
title: "Che cos'è PAM per Servizi di dominio Active Directory? | Documentazione Microsoft"
description: Privileged Access Management (PAM) consente alle organizzazioni di limitare l'accesso con privilegi in un ambiente Active Directory esistente.
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/10/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: cf3796f7-bc68-4cf7-b887-c5b14e855297
ms.reviewer: mwahl
ms.suite: ems
ms.translationtype: Human Translation
ms.sourcegitcommit: f0947f186b5206d06a67140706ada33a5bc0e016
ms.openlocfilehash: 9a047644d07e3ee3c2d1abfde7753849b5ddc63b
ms.contentlocale: it-it
ms.lasthandoff: 01/11/2017

---

<a id="privileged-access-management-for-active-directory-domain-services" class="xliff"></a>
# Privileged Access Management per Servizi di dominio Active Directory
Privileged Access Management (PAM) consente alle organizzazioni di limitare l'accesso con privilegi in un ambiente Active Directory esistente.

![Passaggi di PAM: preparazione, protezione, funzionamento, monitoraggio - Diagramma](media/MIM_PIM_SetupProcess.png)

Concentrandosi su un ciclo di preparazione, protezione e monitoraggio dell'ambiente, Privileged Access Management raggiunge due obiettivi:

- Ristabilire il controllo in un ambiente Active Directory compromesso mantenendo un ambiente bastion separato che non sarà interessato da attacchi dannosi.  
- Isolare l'uso degli account con privilegi per ridurre il rischio di furto delle credenziali.

> [!NOTE]
> PAM è un'istanza di [Privileged Identity Management](https://azure.microsoft.com/documentation/articles/active-directory-privileged-identity-management-configure/) (PIM) implementata usando Microsoft Identity Manager (MIM).

<a id="what-problems-does-pam-help-solve" class="xliff"></a>
## Quali problemi PAM contribuisce a risolvere?
Attualmente, un aspetto che costituisce una preoccupazione reale per le aziende è l'accesso alle risorse in un ambiente Active Directory. Sono particolarmente preoccupanti le notizie relative alle vulnerabilità, all'intensificarsi di privilegi non autorizzati e altri tipi di accesso non autorizzato, inclusi gli attacchi di tipo Pass-the-hash, Pass-the-ticket, spear phishing e i casi di compromissione di Kerberos.

Oggi è fin troppo facile per gli autori di un attacco ottenere le credenziali degli account amministratore ed è troppo difficile scoprire questi attacchi dopo che il fatto è avvenuto. L'obiettivo di PAM consiste nel ridurre le opportunità di accesso per gli utenti malintenzionati e aumentare il controllo e la consapevolezza dell'ambiente da parte dell'azienda.

PAM rende più difficile per gli autori di un attacco penetrare in una rete e ottenere l'accesso agli account con privilegi. PAM aggiunge la protezione ai gruppi con privilegi che controllano l'accesso per una gamma di computer appartenenti al dominio e per le applicazioni disponibili in tali computer. Aggiunge anche un maggior livello di monitoraggio, visibilità e controlli granulari, in modo che le organizzazioni possano vedere chi sono gli amministratori con privilegi e cosa stanno facendo. PAM offre alle organizzazioni informazioni più dettagliate sulla modalità d'uso degli account amministrativi nell'ambiente.

<a id="how-is-pam-set-up" class="xliff"></a>
## Come si configura PAM?
PAM si basa sul principio dell'amministrazione JIT, che fa riferimento a [Just Enough Administration (JEA)](http://channel9.msdn.com/Events/TechEd/NorthAmerica/2014/DCIM-B362). JEA è un toolkit di Windows PowerShell che definisce un set di comandi per l'esecuzione di attività con privilegi e un endpoint in cui gli amministratori possono ottenere l'autorizzazione per eseguire tali comandi. In JEA, un amministratore decide che gli utenti con privilegi specifici possono eseguire una determinata attività. Ogni volta che un utente idoneo deve eseguire tale attività, concedono l'autorizzazione. Le autorizzazioni scadono dopo un periodo di tempo specificato, in modo che un utente malintenzionato non possa rubare l'accesso.

Per l'installazione e il funzionamento di PAM sono previsti quattro passaggi.


1.  **Preparazione**: identificare i gruppi con privilegi significativi nella foresta esistente. Ricreare questi gruppi senza membri nella foresta bastion.

2.  **Protezione**: configurare la protezione del ciclo di vita e dell'autenticazione, ad esempio Multi-Factor Authentication (MFA), per le richieste di amministrazione JIT da parte degli utenti. MFA contribuisce a prevenire gli attacchi a livello di codice da parte di software dannoso o a seguito del furto di credenziali.

3.  **Funzionamento**: dopo aver soddisfatto i requisiti di autenticazione e aver approvato una richiesta, un account utente viene aggiunto temporaneamente a un gruppo con privilegi nella foresta bastion. Per un periodo di tempo preimpostato, l'amministratore ha tutti i privilegi e le autorizzazioni di accesso assegnati a tale gruppo. Trascorso il tempo definito, l'account viene rimosso dal gruppo.

4.  **Monitoraggio**: PAM aggiunge controllo, avvisi e report delle richieste di accesso con privilegi. È sempre possibile verificare la cronologia dell'accesso con privilegi e vedere chi ha eseguito una determinata attività. È possibile decidere se l'attività è valida o meno e identificare facilmente un'attività non autorizzata, ad esempio il tentativo di aggiungere un utente direttamente a un gruppo con privilegi nella foresta originale. Questo passaggio è importante non solo per identificare il software dannoso, ma anche per tenere traccia degli autori di attacchi "interni".

<a id="how-does-pam-work" class="xliff"></a>
## Come funziona PAM?
PAM si basa su nuove funzionalità di Servizi di dominio Active Directory, in particolare per l'autenticazione e l'autorizzazione di account di dominio, e su nuove funzionalità di Microsoft Identity Manager. PAM consente di separare gli account con privilegi da un ambiente Active Directory esistente. Quando è necessario usare un account con privilegi, deve prima essere richiesto e quindi approvato. Dopo l'approvazione all'account con privilegi viene concessa l'autorizzazione tramite un gruppo principale esterno in una nuova foresta bastione, invece che nella foresta corrente dell'utente o dell'applicazione. L'uso di una foresta bastione offre all'organizzazione maggiore controllo, ad esempio quando un utente può essere membro di un gruppo con privilegi e come deve autenticarsi.

Anche Active Directory, il servizio MIM e altre parti di questa soluzione possono essere distribuiti in una configurazione a disponibilità elevata.

L'esempio seguente mostra il funzionamento di PIM più in dettaglio.

![Processo e partecipanti di PIM - Diagramma](media/MIM_PIM_howitworks.png)

La foresta bastion rilascia criteri di appartenenza a gruppi con durata limitata, che a loro volta producono Ticket Granting Ticket (TGT) con durata limitata. I servizi o le applicazioni basate su Kerberos possono rispettare e applicare tali TGT, se i servizi e le app sono presenti nelle foreste che considerano attendibile la foresta bastion.

Gli account utente giornalieri non devono essere spostati in una nuova foresta. Ciò avviene anche per i computer, le applicazioni e i relativi gruppi. Rimangono dove sono attualmente in una foresta esistente. Si consideri l'esempio di un'organizzazione che è attualmente preoccupata per questi problemi riguardanti la sicurezza informatica, ma che non ha piani immediati di aggiornamento dell'infrastruttura server alla versione successiva di Windows Server. Questa organizzazione può comunque sfruttare il vantaggio offerto da questa soluzione combinata usando MIM e una nuova foresta bastion, in modo da controllare meglio l'accesso alle risorse esistenti.

PAM offre i vantaggi seguenti:

-   **Isolamento/Definizione dell'ambito dei privilegi**: gli utenti non dispongono di privilegi per gli account usati anche per attività senza privilegi, quali controllo della posta elettronica o esplorazione Internet. Gli utenti devono richiedere i privilegi. Le richieste vengono approvate o negate in base ai criteri MIM definiti da un amministratore di PAM. A meno che una richiesta sia approvata, l'accesso con privilegi non sarà disponibile.

-   **Aggiornamento all'edizione superiore e prova**: si tratta di nuovi test di autenticazione e autorizzazione per facilitare la gestione del ciclo di vita di account amministrativi distinti. L'utente può richiedere l'elevazione di un account amministrativo e tale richiesta viene inviata al flusso di lavoro di MIM.

-   **Registrazione aggiuntiva**: insieme ai flussi di lavoro predefiniti di MIM, esistono altre opzioni di registrazione per PAM che identificano la richiesta, come è stata autorizzata e gli eventi che si verificano dopo l'approvazione.

-   **Flusso di lavoro personalizzabile**: i flussi di lavoro MIM possono essere configurati per diversi scenari e si possono usare più flussi di lavoro, in base ai parametri dell'utente richiedente o dei ruoli richiesti.

<a id="how-do-users-request-privileged-access" class="xliff"></a>
## In che modo gli utenti possono richiedere l'accesso con privilegi?
Esistono diversi modi in cui un utente può inviare una richiesta, tra cui:  
- API dei servizi Web di MIM  
- Endpoint REST  
- Windows PowerShell (`New-PAMRequest`)

<a id="what-workflows-and-monitoring-options-are-available" class="xliff"></a>
## Quali flussi di lavoro e opzioni di monitoraggio sono disponibili?
Si supponga ad esempio che un utente è membro di un gruppo amministrativo prima della configurazione di PIM. Come parte dell'installazione di PIM l'utente viene rimosso dal gruppo amministrativo e vengono creati criteri in MIM. I criteri specificano che se l'utente richiede privilegi amministrativi e viene autenticato tramite MFA, la richiesta viene approvata e un account separato per l'utente verrà aggiunto al gruppo con privilegi nella foresta bastion.

Supponendo che la richiesta venga approvata, il flusso di lavoro azione comunica direttamente con la foresta Active Directory bastion per inserire un utente in un gruppo. Ad esempio, quando un utente chiede di amministrare il database delle risorse umane, entro pochi secondi il suo account amministrativo viene aggiunto al gruppo con privilegi nella foresta bastion. L'appartenenza dell'account amministrativo a tale gruppo scadrà dopo un limite di tempo prestabilito. Con Windows Server Technical Preview tale appartenenza è associata in Active Directory a un limite di tempo. Con Windows Server 2012 R2 nella foresta bastion, tale limite viene applicato da MIM.

> [!NOTE]
> Quando si aggiunge un nuovo membro a un gruppo, è necessario replicare la modifica in altri controller di dominio nella foresta bastion. La latenza di replica può influire sulla capacità per gli utenti di accedere alle risorse. Per altre informazioni sulla latenza di replica, vedere [Funzionamento della topologia di replica di Active Directory](https://technet.microsoft.com/library/cc755994.aspx).
>
> Al contrario, un collegamento scaduto viene valutato in tempo reale da Gestione account di sicurezza (SAM). Anche se l'aggiunta di un membro del gruppo deve essere replicata dal controller di dominio che riceve la richiesta di accesso, la rimozione di un membro del gruppo viene valutata immediatamente in ogni controller di dominio.

Questo flusso di lavoro è concepito appositamente per questi account amministrativi. Gli amministratori (o gli script) che necessitano solo occasionalmente dell'accesso per gruppi con privilegi, possono richiedere esattamente questo accesso. MIM registra la richiesta e le modifiche in Active Directory, ed è possibile visualizzarle nel Visualizzatore eventi o inviare i dati alle soluzioni di monitoraggio aziendale, ad esempio System Center 2012, Operations Manager Audit Collection Services (ACS) o altri strumenti di terze parti.

