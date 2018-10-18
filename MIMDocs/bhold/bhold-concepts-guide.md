---
title: Guida ai concetti di Microsoft BHOLD Suite | Microsoft Docs
description: Per iniziare a usare i componenti di MIM 2016, installare e configurare il servizio di sincronizzazione.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 09/14/2017
ms.assetid: ''
ms.prod: microsoft-identity-manager
ms.openlocfilehash: 32bd77140cf70047eaa02d363a1348e73783f87a
ms.sourcegitcommit: 7de35aaca3a21192e4696fdfd57d4dac2a7b9f90
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/16/2018
ms.locfileid: "49358840"
---
# <a name="microsoft-bhold-suite-concepts-guide"></a>Guida ai concetti di Microsoft BHOLD Suite

Microsoft Identity Manager 2016 (MIM) consente alle organizzazioni di gestire l'intero ciclo di vita delle identità utente e delle credenziali associate. Può essere configurato per la sincronizzazione delle identità, la gestione centralizzata di certificati e password e il provisioning degli utenti in sistemi eterogenei. MIM consente alle organizzazioni IT di definire e automatizzare i processi di gestione delle identità, dalla creazione al ritiro.

Microsoft BHOLD Suite estende queste funzionalità di MIM aggiungendo il controllo degli accessi in base al ruolo. Con BHOLD le organizzazioni possono definire i ruoli utente e controllare l'accesso ai dati e alle applicazioni sensibili. L'accesso si basa su ciò che è appropriato per i diversi ruoli. BHOLD Suite include servizi e strumenti che semplificano la modellazione delle relazioni tra i ruoli dell'organizzazione. BHOLD esegue il mapping di questi ruoli a diritti e verifica che le definizioni dei ruoli e i diritti associati siano applicati correttamente agli utenti. Queste funzionalità sono completamente integrate con MIM e garantiscono un'esperienza ottimale sia per gli utenti finali che per lo staff IT.

Questa guida illustra il funzionamento di BHOLD Suite con MIM e tratta i seguenti argomenti:

- Controllo di accesso in base ai ruoli
- Attestazione
- Analisi
- Reporting
- Access Management Connector
- Integrazione MIM

## <a name="role-based-access-control"></a>Controllo di accesso in base ai ruoli

Il metodo più comune per il controllo dell'accesso degli utenti a dati e applicazioni è il controllo di accesso discrezionale (DAC, Discretionary Access Control). Nella maggior parte delle implementazioni comuni ogni oggetto significativo dispone di un proprietario identificato. Il proprietario ha la facoltà di concedere o negare l'accesso all'oggetto ad altri utenti sulla base dell'identità individuale o dell'appartenenza a un gruppo. Nella pratica il controllo di accesso discrezionale origina molti gruppi di sicurezza, alcuni corrispondenti alla struttura organizzativa, altri che rappresentano raggruppamenti funzionali (quali tipi di attività o assegnazioni di progetto) e altri ancora che sono gruppi temporanei di utenti e dispositivi, raggruppati per scopi di durata limitata. Mano a mano che le organizzazioni crescono, la gestione dell'appartenenza a questi gruppi diventa sempre più complessa. Se ad esempio un dipendente viene trasferito da un progetto a un altro i gruppi usati per il controllo dell'accesso agli asset dei progetti vanno aggiornati manualmente. In tali casi si verificano spesso errori, che possono compromettere la sicurezza e la produttività del progetto.

MIM include funzionalità che contribuiscono a limitare questo problema, grazie al controllo automatico dell'appartenenza a gruppi e liste di distribuzione. Tuttavia tali funzionalità non risolvono la complessità intrinseca della proliferazione di gruppi non necessariamente correlati in modo strutturato.

Un metodo per ridurre notevolmente tale proliferazione è l'implementazione del controllo degli accessi in base al ruolo (RBAC, Role-Based Access Control). Il controllo RBAC non elimina il controllo di accesso discrezionale (DAC).  Il controllo RBAC espande la funzionalità DAC offrendo una struttura per la classificazione degli utenti e delle risorse IT. Ciò consente di esplicitare le relazioni e i diritti di accesso appropriati in base a tale classificazione. Se ad esempio si assegnano a un utente attributi che specificano la qualifica e le assegnazioni a progetti, sarà possibile concedere a tale utente l'accesso agli strumenti necessari per l'attività e ai dati di cui ha bisogno per contribuire a un progetto specifico. Quando la qualifica e le assegnazioni dell'utente cambiano, è sufficiente modificare gli attributi che specificano la qualifica e i progetti dell'utente per bloccare automaticamente l'accesso alle risorse riservate esclusivamente alla posizione precedente.

Poiché i ruoli possono essere contenuti in altri ruoli in modo gerarchico, (ad esempio i ruoli di responsabile vendite e rappresentante di vendita possono essere inclusi nel ruolo più generale delle vendite), è facile assegnare i diritti appropriati a ruoli specifici, pur fornendo diritti adeguati a chi appartiene al ruolo più generico. Ad esempio in una struttura ospedaliera è possibile concedere a tutto il personale medico l'autorizzazione a visualizzare la cartella clinica di un paziente, ma soltanto ai medici (sottoruolo del ruolo personale medico) l'autorizzazione a creare ricette mediche per tale paziente. In modo analogo è possibile negare l'accesso agli utenti che appartengono alla categoria degli impiegati, salvo agli impiegati addetti alla fatturazione (un sottoruolo del ruolo di impiegato), ai quali si potrà concedere l'accesso alle parti del record del paziente necessarie per calcolare l'addebito dei servizi erogati dalla struttura.

Un vantaggio aggiuntivo del controllo degli accessi in base al ruolo è la capacità di definire e implementare la separazione dei compiti (SoD, Separation Of Duties). L'organizzazione può definire combinazioni di ruoli che concedono autorizzazioni non assegnabili allo stesso utente: così ad esempio sarà possibile non assegnare a un determinato utente ruoli che consentono di creare un pagamento e anche di autorizzare lo stesso pagamento. Il controllo degli accessi in base al ruolo consente di implementare questi criteri automaticamente, evitando di doverne valutare l'implementazione un utente alla volta.

### <a name="bhold-role-model-objects"></a>Oggetti del modello a ruoli BHOLD

Con BHOLD Suite è possibile specificare e strutturare i ruoli nell'organizzazione, eseguire il mapping di utenti a determinati ruoli ed eseguire il mapping delle autorizzazioni appropriate a ogni ruolo. Questa struttura, detta modello a ruoli, contiene e connette tra loro cinque tipi di oggetti: 

- Unità organizzative
- Users
- Ruoli
- Autorizzazioni
- Applicazioni

#### <a name="organizational-units"></a>Unità organizzative

Le unità organizzative sono lo strumento principale di organizzazione degli utenti nel modello a ruoli BHOLD. Ogni utente deve appartenere ad almeno un'unità organizzativa. Quando un utente viene rimosso dall'ultima unità organizzativa in BHOLD, il record dei dati dell'utente viene eliminato dal database BHOLD.

> [!Important]
> Le unità organizzative nel modello a ruoli BHOLD non vanno confuse con le unità organizzative in Active Directory Domain Services (AD DS). In genere la struttura dell'unità organizzativa in BHOLD dipende dall'organizzazione e dai criteri dell'azienda e non dai requisiti dell'infrastruttura di rete.

Anche se non è obbligatorio, nella maggior parte dei casi le unità organizzative in BHOLD sono definite in modo da rappresentare la struttura gerarchica dell'organizzazione, simile a quella illustrata di seguito:

![](media/bhold-concepts-guide/org-chart.png)

In questo esempio il modello a ruoli contiene un'unità organizzativa per l'azienda nel suo complesso (rappresentata dal presidente, dato che non appartiene a un'unità organizzativa più specifica). In alternativa è possibile usare a tale scopo l'unità organizzativa radice BHOLD (che è sempre presente). Le unità organizzative che rappresentano le divisioni aziendali con a capo i vicepresidenti vengono posizionate nell'unità organizzativa aziendale. Quindi le unità organizzative corrispondenti ai direttori vendite e marketing vengono aggiunte alle unità organizzative di vendite e marketing e le unità organizzative che rappresentano i responsabili vendite regionali vengono posizionate nell'unità organizzativa del responsabile vendite area orientale. I venditori, che non gestiscono altri utenti, vengono inclusi nell'unità organizzativa del responsabile vendite area orientale. Si noti che gli utenti possono essere membri di un'unità organizzativa a qualsiasi livello. Ad esempio un assistente amministrativo, che non è un manager e risponde direttamente a un vicepresidente, è un membro dell'unità organizzativa del vicepresidente.

Oltre che per rappresentare la struttura organizzativa, le unità organizzative possono essere usate anche per raggruppare utenti e altre unità organizzative in base a criteri funzionali, ad esempio in base al progetto o alla specializzazione. Il diagramma seguente visualizza un esempio d'uso delle unità organizzative per raggruppare i venditori in base al tipo di cliente:

![](media/bhold-concepts-guide/org-chart-02.png)

In questo esempio ogni venditore appartiene a due unità organizzative: una che rappresenta la posizione del venditore nella struttura di gestione dell'organizzazione e una che rappresenta la base di clienti del venditore (aziende o commercio al dettaglio). È possibile assegnare a ogni unità organizzativa ruoli diversi, ai quali è possibile attribuire autorizzazioni diverse per l'accesso alle risorse IT dell'organizzazione. I ruoli possono anche essere ereditati dalle unità organizzative padre, semplificando il processo di propagazione dei ruoli agli utenti. D'altro canto è possibile impedire che determinati ruoli vengano ereditati, per garantire che un ruolo specifico sia associato solo alle unità organizzative appropriate.

È possibile creare le unità organizzative in BHOLD Suite con il portale Web BHOLD Core o usando BHOLD Model Generator.

#### <a name="users"></a>Users

Come indicato in precedenza, ogni utente deve appartenere ad almeno un'unità organizzativa. Dato che le unità organizzative sono il meccanismo principale per associare un utente ai ruoli desiderati, nella maggior parte delle organizzazioni un determinato utente appartiene a più unità organizzative, in modo da facilitare l'associazione di ruoli a tale utente. In alcuni casi però può essere necessario associare un ruolo a un utente indipendentemente dalle unità organizzative alle quali appartiene l'utente stesso. Di conseguenza un utente può essere assegnato direttamente a un ruolo ma allo stesso tempo ottenere ruoli dalle unità organizzative a cui appartiene.

Quando un utente non è attivo nell'organizzazione (ad esempio in caso di congedo per motivi di salute) è possibile sospenderlo, revocando tutte le autorizzazioni dell'utente senza però rimuoverlo dal modello a ruoli. Quando torna in attività, l'utente può essere riattivato e ottiene nuovamente tutte le autorizzazioni concesse dai ruoli di cui dispone.

È possibile creare individualmente oggetti per gli utenti in BHOLD con il portale Web BHOLD Core o importarli con un'operazione bulk con BHOLD Model Generator o ancora usare Access Management Connector con il servizio di sincronizzazione FIM per importare i dati utente da origini come Active Directory Domain Services o le applicazioni per la gestione delle risorse umane.

È possibile creare direttamente gli utenti in BHOLD senza usare il servizio di sincronizzazione FIM. Questa funzionalità può essere utile durante la modellazione di ruoli in un ambiente di preproduzione o di test. È anche possibile consentire l'assegnazione di ruoli a utenti esterni (ad esempio, i dipendenti di un terzista) che quindi potranno accedere alle risorse IT senza essere aggiunti al database dei dipendenti. Questi utenti non potranno usare le funzionalità self-service di BHOLD.

#### <a name="roles"></a>Ruoli

Come indicato in precedenza, nel modello di controllo degli accessi in base al ruolo (RBAC) le autorizzazioni sono associate ai ruoli anziché ai singoli utenti. In questo modo è possibile modificare i ruoli utente per concedere a ogni utente le autorizzazioni necessarie per le attività che deve svolgere, invece di concedere o negare autorizzazioni su base individuale. Di conseguenza, per l'assegnazione delle autorizzazioni non è più necessario l'intervento del personale IT: questo compito può essere incluso nelle attività di gestione aziendale. Un ruolo può aggregare autorizzazioni per l'accesso a diversi sistemi, direttamente o tramite i ruoli secondari, riducendo ancora la necessità di interventi IT nella gestione delle autorizzazioni utente.

È importante osservare che i ruoli sono una funzionalità del modello RBAC e in genere non vengono resi disponibili alle applicazioni di destinazione. Il modello RBAC può essere usato insieme ad applicazioni esistenti non progettate per l'uso dei ruoli oppure per modificare le definizioni dei ruoli e soddisfare le esigenze di modelli di business in evoluzione senza dover modificare le applicazioni stesse. Se un'applicazione di destinazione è progettata per l'uso dei ruoli, è possibile associare i ruoli del modello a ruoli BHOLD con i ruoli corrispondenti dell'applicazione, considerando questi ultimi come autorizzazioni.

In BHOLD è possibile assegnare un ruolo a un utente con due metodi principali:

- Assegnazione di un ruolo a un'unità organizzativa di cui è membro l'utente
- Assegnazione diretta di un ruolo a un utente

Un ruolo assegnato a un'unità organizzativa padre può facoltativamente essere ereditato dalle unità organizzative figlio. Quando un ruolo viene assegnato a o ereditato da un'unità organizzativa, può essere designato come ruolo attivo o proposto. Se è un ruolo attivo, viene assegnato a tutti gli utenti dell'unità organizzativa. Se è un ruolo proposto, deve essere attivato per ogni utente o unità organizzativa per diventare attivo per l'utente o per i membri dell'unità organizzativa. In questo modo è possibile assegnare agli utenti un sottoinsieme dei ruoli associati a un'unità organizzativa, anziché assegnare automaticamente tutti i ruoli dell'unità organizzativa a tutti i membri. È anche possibile corredare i ruoli di date di inizio e possibile limitare la percentuale di utenti di un'unità organizzativa per i quali un determinato ruolo può essere attivo.

Il diagramma seguente illustra come assegnare un ruolo a un utente singolo in BHOLD:

![](media/bhold-concepts-guide/org-chart-flow.png)

In questo diagramma il ruolo A viene assegnato a un'unità organizzativa come ruolo ereditabile e pertanto viene ereditato dalle unità organizzative membro e da tutti gli utenti appartenenti a queste unità organizzative. Il ruolo B viene assegnato come ruolo proposto per un'unità organizzativa. Per far sì che un utente dell'unità organizzativa possa avere le autorizzazioni corrispondenti al ruolo, è necessario attivarlo. Il ruolo C è un ruolo attivo, pertanto le autorizzazioni sono immediatamente valide per tutti gli utenti nell'unità organizzativa. Il ruolo D è collegato direttamente a un utente, pertanto le autorizzazioni sono immediatamente valide per l'utente.

È anche possibile attivare un ruolo per un utente in base agli attributi dell'utente. Per altre informazioni, vedere Autorizzazione basata su attributi più avanti.

#### <a name="permissions"></a>Autorizzazioni

Un'autorizzazione in BHOLD corrisponde a un'autorizzazione importata da un'applicazione di destinazione. In altre parole quando BHOLD viene configurato per funzionare con un'applicazione, questa riceve un elenco di autorizzazioni che BHOLD può collegare ai ruoli. Ad esempio, quando Active Directory Domain Services (AD DS) viene aggiunto a BHOLD come applicazione, riceve un elenco di gruppi di sicurezza che, come autorizzazioni di BHOLD, possono essere collegati a ruoli di BHOLD.

Le autorizzazioni sono specifiche per le applicazioni. BHOLD offre una vista singola unificata delle autorizzazioni, che possono quindi essere associate ai ruoli senza che i gestori dei ruoli debbano conoscere i dettagli di implementazione delle autorizzazioni. Nella pratica, sistemi diversi possono applicare un'autorizzazione in modi diversi. Il connettore specifico tra l'applicazione e il servizio di sincronizzazione FIM determina in che modo le modifiche delle autorizzazioni per un utente vengono rese disponibili all'applicazione. 

#### <a name="applications"></a>Applicazioni

BHOLD implementa un metodo per l'applicazione del controllo degli accessi in base al ruolo (RBAC) alle applicazioni esterne. Quando BHOLD riceve utenti e autorizzazioni da un'applicazione può associare le autorizzazioni agli utenti assegnando ruoli agli utenti, quindi collegando le autorizzazioni ai ruoli. Il processo in background dell'applicazione può quindi eseguire il mapping tra le autorizzazioni appropriate e gli utenti, sulla base del mapping ruolo/autorizzazione di BHOLD.

### <a name="developing-the-bhold-suite-role-model"></a>Sviluppo del modello a ruoli di BHOLD Suite

Per favorire lo sviluppo del modello a ruoli, BHOLD Suite include Model Generator, uno strumento completo ma anche facile da usare.

Prima di usare Model Generator è necessario creare una serie di file che definiscono gli oggetti usati da Model Generator per costruire il modello a ruoli. Per informazioni su come creare questi file, vedere la Guida tecnica di Microsoft BHOLD Suite.

La prima operazione per l'uso di BHOLD Model Generator è l'importazione di questi file per caricare i blocchi costitutivi di base in Model Generator. Dopo che i file sono stati caricati correttamente, è possibile specificare i criteri usati da Model Generator per creare diverse classi di ruoli:

- Ruoli di appartenenza, assegnati a un utente in base alle unità organizzative alle quali appartiene
- Ruoli attributo, assegnati a un utente in base agli attributi dell'utente nel database BHOLD
- Ruoli proposti, che sono collegati a un'unità organizzativa ma devono essere attivati per utenti specifici
- Ruoli di proprietà, che concedono a un utente il controllo delle unità organizzative e dei ruoli per i quali non è specificato un proprietario nei file importati

> [!Important]
> Durante il caricamento di file, selezionare la casella di controllo **Retain Existing Model** (Mantieni modello esistente) solo negli ambienti di test. Negli ambienti di produzione è necessario usare Model Generator per creare il modello a ruoli iniziale. Non è possibile usarlo per modificare un modello a ruoli esistente nel database BHOLD.

Dopo che Model Generator ha creato questi ruoli nel modello a ruoli, è possibile esportare il modello a ruoli nel database BHOLD come file con estensione XML.

### <a name="advanced-bhold-features"></a>Funzionalità avanzate di BHOLD

Le sezioni precedenti descrivono le funzionalità di base del controllo degli accessi in base al ruolo (RBAC) in BHOLD. Questa sezione descrive funzionalità aggiuntive di BHOLD che offrono una maggiore sicurezza e flessibilità all'implementazione RBAC dell'organizzazione. Questa sezione presenta una panoramica delle funzionalità BHOLD seguenti:

- Cardinalità
- Separazione dei compiti
- Autorizzazioni adattabili al contesto
- Autorizzazione basata sugli attributi
- Tipi di attributo flessibili


#### <a name="cardinality"></a>Cardinalità

La *cardinalità* indica l'implementazione di regole business progettate per limitare il numero di volte per il quale due entità possono essere correlate tra loro. Nel caso di BHOLD è possibile definire regole di cardinalità per ruoli, autorizzazioni e utenti.

È possibile configurare un ruolo per limitare quanto segue:

- Il numero massimo di utenti per i quali può essere attivato un ruolo proposto
- Il numero massimo di sottoruoli che possono essere collegati al ruolo
- Il numero massimo di autorizzazioni che possono essere collegate al ruolo

È possibile configurare un'autorizzazione per limitare quanto segue:

- Il numero massimo di ruoli che possono essere collegati all'autorizzazione
- Il numero massimo di utenti ai quali può essere concessa l'autorizzazione

È possibile configurare un utente per limitare quanto segue:

- Il numero massimo di ruoli che possono essere collegati all'utente
- Il numero massimo di autorizzazioni che possono essere assegnate all'utente tramite le assegnazioni di ruolo

#### <a name="separation-of-duties"></a>Separazione dei compiti

La separazione dei compiti è un principio operativo che si propone di impedire a un singolo individuo di eseguire azioni diverse che non dovrebbero essere disponibili a un unico utente. Ad esempio, un dipendente non dovrebbe essere in grado di richiedere un pagamento e anche di autorizzare tale pagamento. Grazie al principio di separazione dei compiti le organizzazioni implementano un sistema di verifiche ed equilibri, che riduce al minimo l'esposizione al rischio di errori o comportamenti inappropriati dei dipendenti.

BHOLD implementa la separazione dei compiti consentendo di definire autorizzazioni non compatibili. Dopo la definizione delle autorizzazioni, BHOLD applica la separazione dei compiti impedendo la creazione di ruoli collegati (direttamente o per eredità) ad autorizzazioni non compatibili e impedendo che agli utenti vengano assegnati (direttamente o per eredità) più ruoli che, se combinati, concederebbero autorizzazioni non compatibili. Facoltativamente è possibile eseguire l'override dei conflitti.

#### <a name="context-adaptable-permissions"></a>Autorizzazioni adattabili al contesto

La creazione di autorizzazioni modificabili automaticamente in base a un attributo di oggetto consente di ridurre il numero complessivo di autorizzazioni da gestire. Le autorizzazioni adattabili al contesto consentono di definire come attributo di un'autorizzazione una formula che modifica il modo in cui viene applicata l'autorizzazione dall'applicazione associata. Ad esempio è possibile creare una formula che cambia l'autorizzazione di accesso a una cartella di file (tramite un gruppo di sicurezza associato all'elenco di controllo di accesso della cartella) a seconda che un utente appartenga a un'unità organizzativa contenente dipendenti a tempo indeterminato o dipendenti a tempo determinato. Se l'utente viene spostato da un'unità organizzativa a un'altra, viene applicata automaticamente la nuova autorizzazione e l'autorizzazione precedente viene disattivata. 

La formula delle autorizzazioni adattabili al contesto può verificare i valori degli attributi applicati ad autorizzazioni, applicazioni, unità organizzative e utenti.

#### <a name="attribute-based-authorization"></a>Autorizzazione basata sugli attributi

Un metodo per controllare se un ruolo collegato a un'unità organizzativa è attivato per un determinato utente dell'unità organizzativa è l'uso dell'autorizzazione basata su attributi (ABA, Attribute-Based Authorization). Con l'autorizzazione basata su attributi è possibile attivare automaticamente un ruolo solo se vengono soddisfatte determinate regole basate sugli attributi di un utente. Ad esempio è possibile collegare un ruolo a un'unità organizzativa che diventa attiva per un utente solo se la posizione professionale dell'utente corrisponde alla posizione professionale specificata nella regola ABA. In questo modo si elimina la necessità di attivare manualmente un ruolo proposto per un utente. In alternativa è possibile attivare un ruolo per tutti gli utenti di un'unità organizzativa che dispongono di un valore di attributo che soddisfa la regola ABA del ruolo. È possibile combinare le regole per ottenere che un ruolo venga attivato solo quando gli attributi di un utente soddisfano tutte le regole ABA specificate per il ruolo.

È importante osservare che i risultati dei test regola ABA sono limitati dalle impostazioni di cardinalità. Se ad esempio l'impostazione di cardinalità di una regola specifica che un ruolo non può essere assegnato a più di due utenti e per contro una regola ABA prevede l'attivazione del ruolo per quattro utenti, il ruolo verrà attivato solo per i primi due utenti che superano il test ABA.

#### <a name="flexible-attribute-types"></a>Tipi di attributo flessibili

Il sistema di attributi di BHOLD è ampiamente estendibile. È possibile definire nuovi tipi di attributo per oggetti come utenti, unità organizzative e ruoli. È possibile definire come valori per gli attributi valori interi, booleani (sì/no), alfanumerici, di data, di ora e indirizzi di posta elettronica. Gli attributi possono essere specificati come valori singoli o come elenco di valori.

## <a name="attestation"></a>Attestazione

BHOLD Suite include strumenti che consentono di verificare che i singoli utenti dispongano delle autorizzazioni appropriate per eseguire le attività operative. L'amministratore può usare il portale disponibile con il modulo BHOLD Attestation per configurare e gestire il processo di attestazione.

Il processo di attestazione viene eseguito mediante campagne nelle quali gli amministratori della campagna sono in grado di verificare se gli utenti di cui sono responsabili dispongono dell'accesso appropriato alle applicazioni gestite BHOLD e delle autorizzazioni corrette all'interno di tali applicazioni. Il proprietario della campagna viene designato per controllare lo svolgimento corretto della campagna. È possibile creare una campagna da eseguire una sola volta o su base periodica.

In genere un amministratore di campagna è un manager che attesta i diritti di accesso degli utenti appartenenti a una o più unità organizzative di cui è responsabile. È possibile selezionare automaticamente gli amministratori di campagna in base agli attributi degli utenti sottoposti ad attestazione oppure è possibile definire gli amministratori elencandoli in un file che associa ogni utente sottoposto ad attestazione a un amministratore di campagna.

Quando viene avviata una campagna, BHOLD invia una notifica di posta elettronica al proprietario e agli amministratori della campagna, quindi invia promemoria periodici per contribuire allo svolgimento corretto della campagna. Gli amministratori vengono indirizzati a un portale in cui possono visualizzare un elenco degli utenti di cui sono responsabili e i ruoli assegnati a tali utenti. Gli amministratori possono quindi verificare se sono di fatto responsabili per ogni utente elencato e approvare o negare i diritti di accesso di ogni utente.

Anche i proprietari della campagna usano il portale per monitorare lo stato della campagna e le attività registrate, al fine di analizzare nel dettaglio le azioni portate a termine nel corso della campagna.

## <a name="analytics"></a>Analisi

Una delle considerazioni importanti per l'implementazione di un sistema di controllo degli accessi in base al ruolo (RBAC) è l'equilibrio tra un controllo degli accessi efficace e la riduzione al minimo degli ostacoli all'accesso non necessari o peggio ancora imprevisti. La gestione di questo equilibrio spesso dà origine a una struttura di controllo degli accessi così complessa che le interazioni impreviste tra i criteri sono quasi inevitabili.

Per questo motivo è importante essere in grado di analizzare gli effetti dei criteri di controllo degli accessi prima della loro implementazione. Il modulo Analytics di BHOLD Suite consente di eseguire questa analisi, sviluppando regole che rappresentano i criteri e quindi visualizzando gli utenti le cui autorizzazioni soddisfano la regola o entrano in conflitto con la regola. In base a questa analisi è possibile modificare i criteri o alterare i ruoli e le autorizzazioni, per eliminare eventuali conflitti con i criteri stessi.

Il portale BHOLD Analytics consente di creare set di regole con una o più regole per testare un criterio o un gruppo di criteri. Una regola è costituita dalle parti principali seguenti:

- Un titolo e una descrizione che consentono di identificare e descrivere la regola
- Uno stato che indica se la regola è pronta per la revisione, in fase di revisione o approvata
- Un set di elementi (ad esempio utenti o autorizzazioni) che la regola deve sottoporre a test
- Filtri di subset facoltativi, espressioni da usare per selezionare un sottogruppo appropriato dell'elemento da esaminare
- Uno o più filtri di regole, espressioni che rappresentano i criteri da sottoporre a test.

Una regola può sottoporre a test uno dei set di elementi che seguono:

- Users
- Unità organizzative
- Ruoli
- Autorizzazioni
- Applicazioni
- Account

Il diagramma seguente illustra una regola semplice composta da due regole di subset e due regole di filtro:

![](media/bhold-concepts-guide/rules.png)

Si noti la differenza nel risultato quando non viene soddisfatta la condizione di un filtro di subset e quando non viene soddisfatta la condizione di un filtro di regola. Quando non viene soddisfatta la condizione di un filtro di subset l'oggetto elemento viene rimosso dai test dei filtri successivi; quando non viene soddisfatta la condizione di un filtro di regola l'oggetto viene segnalato come non conforme. Vengono segnalati come conformi solo gli oggetti che soddisfano i requisiti di tutti i filtri di subset e di tutti i filtri di regola.

Ogni filtro è costituito da un tipo, un operatore (dipendente dal tipo), una chiave (uno degli elementi) e un valore con il quale l'operatore sottopone a test la chiave. Ad esempio, il filtro seguente verifica se il numero di utenti in un subset dell'elemento è superiore a 10:


|   |   |   |   |   |
|---|---|---|---|---|
|**Tipo:**   | Numero di   |
| **Chiave:**  | Users  |
| **Operatore**  | >  |
| **Valore:** | 10 |

I filtri di regole possono essere di tre tipi e usare operatori specifici del tipo, come indicato di seguito:

- Attributo
  - < e >
  - = e !=
  - **Contains** (Contiene)
  - **Does not contain** (Non contiene)
- Numero di
  - < e >
  - = e !=
- Restrittivo
  - **Must have any (Deve avere uno) e Must have all (Deve avere tutti)**
  - **Cannot have any (Non può avere uno) e Cannot have all (Non può avere nessuno)**
  - **Can only have any (Può avere solo uno) e Can only have all (Può avere solo tutti)**
  - **Exclusively have any (Esclusivamente provvisti di uno) ed Exclusively have all (Esclusivamente provvisti di tutti)**

> [!Note]
> I filtri restrittivi possono usare gli operatori specificati per il test di una chiave rispetto a un set di più valori.

Ad esempio, per sottoporre a test l'implementazione di criteri di separazione dei compiti (SoD) in base ai quali un utente che ha l'autorizzazione Request Payment (Richiedi pagamento) non può avere anche l'autorizzazione Approve Payment (Approva pagamento), è possibile costruire una regola simile alla seguente:

|   |  |
|---|--|
|Nome:| Test separazione dei compiti - Pagamento|
|Elemento:| Users|
|Filtro di subset:| Having any permission Request Payment (Deve avere un'autorizzazione Richiedi pagamento)|
|Filtro regola: | Cannot have any permission Approve Payment (Non può avere un'autorizzazione Approva pagamento)|

Quando si applica questa regola il modulo BHOLD Analytics visualizza il numero di utenti nel subset selezionato (il numero di utenti con l'autorizzazione Request Payment - Richiedi pagamento), il numero di utenti che soddisfano i requisiti della regola e il numero di utenti che non soddisfano i requisiti della regola. È quindi possibile visualizzare gli utenti non conformi e procedere a risolvere l'incoerenza.

Oltre a visualizzare i risultati, è possibile salvare il report come file con valori delimitati da virgole (CSV) o file con estensione XML per analizzare i risultati in un secondo momento. È anche possibile personalizzare il report risultante per visualizzare informazioni aggiuntive che consentono di comprendere meglio l'effetto dei risultati. Se ad esempio si stanno effettuando test relativi agli utenti, è possibile visualizzare (o includere in un report) gli account degli utenti conformi o non conformi per trovare le applicazioni coinvolte.

Dato che una regola può contenere più filtri, è possibile connettere i filtri per verificare se esiste una determinata combinazione di condizioni. Per impostazione predefinita, il risultato è il prodotto di un test booleano AND per di tutti i filtri, ma è possibile richiedere l'esecuzione di un test OR per la combinazione di filtri.

Ad esempio, se in base ai criteri operativi i manager devono avere l'autorizzazione Modify Payment (Modifica pagamento) oppure l'autorizzazione Approve Payment (Approva pagamento), è possibile verificare la conformità ai criteri creando una regola come la seguente:


|  |  |
|--|--|
|Nome: | Test separazione dei compiti - Modifica pagamento|
|Elemento: | Users |
|Filtro di subset: | Having any role Manager (Deve avere un ruolo Manager)|
| Filtri regola: |Must have any permission Modify Payment (Deve avere un'autorizzazione Modifica pagamento) </br> Must have any permission Approve Payment (Deve avere un'autorizzazione Approva pagamento)|

Per impostazione predefinita, tutti gli utenti che sono manager e dispongono dell'autorizzazione Modify Payment (Modifica pagamento) e dell'autorizzazione Approve Payment (Approva pagamento) vengono segnalati come conformi. Tuttavia i criteri richiedono che un manager disponga di una delle due autorizzazioni e non necessariamente di entrambe. Per verificare la conformità effettiva con i criteri è necessario usare l'operatore booleano OR con la regola per determinare se sono presenti manager che non dispongono dell'autorizzazione Modify Payment (Modifica pagamento) o dell'espressione Approve Payment (Approva pagamento).

A differenza di altri operatori, **Exclusively have any** (Esclusivamente provvisti di uno) ed **Exclusively have all** (Esclusivamente provvisti di tutti) garantiscono la conformità a oggetti che altrimenti verrebbero esclusi da un filtro di subset. Ad esempio, per eseguire il test di criteri per i quali tutti i manager (e solo i manager) devono avere l'autorizzazione Approve Reviews (Approva revisioni) è possibile costruire una regola come indicato di seguito:

|  |  |
|--|--|
|Nome: | Test di approvazione revisioni|
|Elemento: | Users|
| Filtro di subset: | Having any role Manager (Deve avere un ruolo Manager)
|Filtro regola: | Exclusively have any permission Approve Reviews (Esclusivamente provvisti di un'autorizzazione Approva revisioni)|

Questa regola segnala come conformi gli utenti con qualifica di manager e provvisti dell'autorizzazione Approve Reviews (Approva revisioni) e gli utenti che non hanno qualifica di manager e non sono provvisti dell'autorizzazione Approve Reviews (Approva revisioni). Per contro, i manager che non dispongono di questa autorizzazione e gli utenti che non sono manager ma dispongono dell'autorizzazione vengono segnalati come non conformi.

Come indicato in precedenza è possibile combinare le regole in un set di regole, semplificando la categorizzazione e la gestione delle regole per soddisfare i requisiti operativi.

È anche possibile definire un set di filtri globali che, se abilitato, si applica a qualsiasi regola sottoposta a test. Se è spesso necessario escludere un subset di record specifico durante il test delle regole di set diversi, è possibile specificare filtri globali e quindi abilitarli o disabilitarli in base alle esigenze.

## <a name="reporting"></a>Reporting

Il modulo BHOLD Reporting consente di visualizzare le informazioni del modello di ruolo attraverso una serie di report. Il modulo offre una vasta gamma di report predefiniti e include una procedura guidata che consente di creare report personalizzati sia di base che avanzati. Quando si esegue un report è possibile visualizzare immediatamente i risultati o salvarli in un file di Microsoft Excel (file con estensione XLSX). Per visualizzare il file con Microsoft Excel 2000, Microsoft Excel 2002 o Microsoft Excel 2003 è possibile scaricare e installare Microsoft Office Compatibility Pack per formati di file di Word, Excel e PowerPoint.


Il modulo BHOLD Reporting viene usato soprattutto per la creazione di report basati sulle informazioni correnti relative ai ruoli. Per generare report per il controllo delle modifiche delle informazioni di identità, Forefront Identity Manager 2010 R2 dispone di una funzionalità di creazione report per il servizio FIM implementata nel data warehouse di System Center Service Manager. Per altre informazioni sulla creazione di report FIM, vedere la documentazione di Forefront Identity Manager 2010 e Forefront Identity Manager 2010 R2 nella libreria tecnica di Forefront Identity Manager.

Le categorie per le quali sono disponibili report predefiniti includono:

- Administration
- Attestazione
- Controlli
- Controllo di accesso interno
- Registrazione
- Modello
- Statistiche
- Flusso di lavoro

È possibile creare report e aggiungerli a queste categorie oppure definire categorie e inserire in tali categorie report predefiniti e personalizzati.

Quando si crea un report, la procedura guidata consente di specificare i parametri seguenti:

- Informazioni di identificazione, inclusi nome, descrizione, categoria, uso e destinatari
- Campi da visualizzare nel report
- Query che specificano gli elementi da analizzare
- Ordinamento previsto per le righe
- Campi usati per suddividere il report in sezioni
- Filtri per limitare gli elementi restituiti nel report

In ogni fase della procedura guidata è possibile visualizzare in anteprima il report come è stato definito fino a quel momento e salvarlo se non è necessario specificare parametri aggiuntivi. È anche possibile tornare a uno dei passaggi precedenti per modificare i parametri specificati in precedenza nella procedura guidata.

## <a name="access-management-connector"></a>Access Management Connector

Il modulo BHOLD Suite Access Management Connector supporta la sincronizzazione sia iniziale che continuativa dei dati in BHOLD. Access Management Connector funziona con il servizio di sincronizzazione FIM per lo spostamento di dati tra il database BHOLD Core, il metaverse MIM e le applicazioni e gli archivi identità di destinazione.

Le versioni precedenti di BHOLD richiedevano più agenti di gestione (MA, Management Agent) per il controllo del flusso di dati tra MIM e le tabelle di database BHOLD intermedie. In BHOLD Suite SP1 la soluzione Access Management Connector consente di definire agenti di gestione in MIM che garantiscono direttamente il trasferimento di dati tra BHOLD e MIM.

## <a name="mim-integration"></a>Integrazione MIM

Una funzionalità importante ed efficace di Forefront Identity Manager 2010 e Forefront Identity Manager 2010 R2 è il portale self-service che consente agli utenti finali di visualizzare e gestire le loro informazioni di identità e appartenenza. L'integrazione MIM amplia le funzionalità del portale MIM con la gestione self-service dei ruoli. Ad esempio un utente può usare le funzionalità BHOLD nel portale MIM per richiedere l'assegnazione di un ruolo e visualizzare i ruoli attivi e le richieste in sospeso. È anche possibile concedere funzionalità aggiuntive agli amministratori autorizzati, ad esempio la possibilità di richiedere l'assegnazione di ruoli per altri utenti.

È importante osservare che le estensioni BHOLD al portale MIM supportano la gestione self-service di ruoli e flussi di lavoro e la creazione di report. Le altre funzioni di amministrazione di BHOLD e l'attestazione sono rese disponibili dai portali Web dei moduli BHOLD, ospitati separatamente dal portale MIM.

## <a name="next-steps"></a>Passaggi successivi

- [Guida all'installazione di BHOLD](bhold-installation-guide.md)
- [Riferimento per gli sviluppatori BHOLD](../reference/mim2016-bhold-developer-reference.md)
- [Cronologia delle versioni BHOLD](../reference/version-bhold-history.md)
