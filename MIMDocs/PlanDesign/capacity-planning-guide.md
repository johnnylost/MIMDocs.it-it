---
# required metadata

title: Guida alla pianificazione della capacità | Microsoft Identity Manager
description: Usare questa guida per comprendere le variabili da considerare prima di distribuire MIM 2016, inclusi i livelli di carico e le decisioni relative ai criteri.
keywords:
author: kgremban
manager: stevenpo
ms.date: 04/28/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 3ac5b990-1678-4996-996d-cbd84b8426b4

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Guida per la pianificazione della capacità

Questa guida è stata realizzata per fornire assistenza nella pianificazione del cliente, ma non deve essere usata da sola per determinare le topologie, l'hardware o i server appropriati che sono necessari per una distribuzione di Microsoft Identity Manager (MIM). È opportuno che le aziende effettuino la configurazione dei propri ambienti di test per una stima più accurata di capacità e prestazioni. Microsoft non garantisce caratteristiche di capacità o prestazioni uguali per tutte le aziende, anche se i componenti di MIM 2016 vengono distribuiti e configurati in modo identico ai componenti descritti in questa guida.

Per preparare correttamente la distribuzione di MIM, simulare l'ambiente di produzione previsto in un ambiente di prova e testarlo. È possibile decidere di testare diverse topologie in esecuzione su diversi tipi di hardware, per poi eseguire diversi test di scenari di ridimensionamento e carico, che consentono di effettuare una stima migliore di ciò che può verificarsi quando si distribuisce MIM 2016 nel proprio ambiente.


## Panoramica
Esistono numerose variabili che possono influenzare la capacità e le prestazioni complessive della distribuzione di Microsoft Identity Manager. I modi in cui vengono fisicamente distribuiti i componenti MIM (topologia), nonché l'hardware su cui tali componenti vengono eseguiti, sono fattori importanti per determinare le prestazioni e la capacità che è possibile attendersi dalla distribuzione di MIM. Il numero e la complessità degli oggetti di configurazione dei criteri MIM potrebbero essere meno evidenti, ma sono comunque fattori significativi di cui tenere conto durante la pianificazione della capacità. Infine, il ridimensionamento previsto della distribuzione, nonché il relativo carico, sono generalmente fattori che influiscono in modo più evidente su prestazioni e capacità.

I fattori principali che influiscono sulla capacità e sulle prestazioni che ci si può attendere da una distribuzione di MIM 2016 sono esaminati nella tabella seguente.

| Fattore di progettazione | Considerazioni |
| ------------- | -------------- |
| Topologia | La distribuzione dei servizi MIM tra i computer sulla rete. Ad esempio, il servizio di sincronizzazione MIM 2016 sarà ospitato nello stesso computer che ospita il database? |
| Hardware | L'hardware fisico e qualsiasi specifica di hardware virtualizzato che si esegue per ogni componente MIM. Sono inclusi CPU, memoria, scheda di rete e configurazione dell'unità disco rigido. |
| Oggetti di configurazione dei criteri MIM | Il numero e il tipo di oggetti di configurazione dei criteri MIM, che include set, regole di criteri di gestione e flussi di lavoro. Ad esempio, quanti flussi di lavoro vengono attivati per le operazioni? Quante definizioni del set esistono e qual è la complessità relativa di ciascuna di esse? |
| Scale | Il numero di utenti, gruppi, gruppi calcolati e tipi di oggetto personalizzato, ad esempio i computer da gestire con MIM 2016. Prendere in considerazione anche la complessità dei gruppi dinamici e assicurarsi di includere l'annidamento gruppo. |
| Carica | Frequenza dell'uso previsto. Ad esempio, il numero di volte in cui si prevede di creare nuovi gruppi o utenti, reimpostare password o visitare il portale in un determinato periodo di tempo. Si noti che il carico può variare nel corso di un'ora, di un giorno, di una settimana o di un anno. A seconda del componente, è possibile progettare per un carico di picco o un carico medio.


## Hosting dei componenti di Microsoft Identity Manager
Microsoft Identity Manager ha molti componenti diversi, che in molti casi non si trovano nello stesso computer. Quando si esamina MIM dal punto di vista della pianificazione della capacità, i componenti e i computer fisici (e, probabilmente, le macchine virtuali) su cui sono ospitati costituiscono considerazioni chiave. Molti potenziali fattori possono influire sulle prestazioni dell'ambiente MIM, ad esempio, la configurazione del disco fisico del computer che esegue il database SQL del servizio MIM 2016. Il numero di assi che costituiscono la configurazione del disco o la distribuzione di file di log e di dati possono influire notevolmente sulle prestazioni del sistema. Tenere presenti anche i fattori esterni nella configurazione. Se si usa una rete SAN come la configurazione del database del servizio MIM 2016, quali altre applicazioni condividono tale rete? In che modo queste applicazioni influiscono sulle prestazioni del database quando si contendono le risorse condivise del disco sulla rete SAN? Quando più applicazioni si contendono le stesse risorse del disco, potrebbero verificarsi colli di bottiglia e prestazioni irregolari del disco.


## Utenti e gruppi
Il numero di utenti e gruppi nell'ambiente è un aspetto di cui si tiene generalmente conto per il ridimensionamento della distribuzione. Esistono, tuttavia, diverse altre considerazioni correlate da considerare nella pianificazione, tra cui:

- Gli utenti possono creare gruppi? In questo caso, è consigliabile effettuare una stima dell'influenza che la creazione di nuovi gruppi di utenti avrà sulla crescita dei gruppi nel proprio ambiente.

- Verranno distribuiti gruppi dinamici?
  - Che tipi di gruppi dinamici verranno distribuiti?
  - Quanti gruppi dinamici verranno probabilmente distribuiti?


## Livelli di carico previsto
È necessario anche considerare il tipo di carico che sarà posizionato sui componenti MIM. Di seguito sono elencate alcune domande rilevanti da considerare:

- Con quale frequenza si prevede di ricevere una richiesta di aggiunta o rimozione da un gruppo?

- Con che frequenza si prevede che un utente creerà un gruppo statico o dinamico?

- È possibile ottenere queste informazioni da applicazioni esistenti nell'ambiente in uso?

- Qual è la quantità di carico prevista di operazioni non basate sull'utente, ad esempio la sincronizzazione di modifiche provenienti da sistemi esterni? Assicurarsi di tenere conto del carico generato dalle operazioni di sincronizzazione delle informazioni di identità con sistemi esterni.

- Che tipi di scenari si intende distribuire? I modelli di carico saranno diversi a seconda dei vari scenari. Ad esempio, i computer client in cui è installato il client MIM 2016 verificano periodicamente se al momento dell'accesso è necessaria la registrazione, operazione che aumenta il carico del sistema.

- Si prevedono notevoli variazioni nei livelli di carico, da un carico normale a un carico di picco? Ad esempio, si prevede un numero elevato di reimpostazioni di password dopo i periodi di festività? Assicurarsi di definire le pianificazioni di sincronizzazione e manutenzione del sistema al di fuori dei picchi di utilizzo previsto. Quando si considera la pianificazione della capacità, assicurarsi di tenere conto dei periodi di carico.


## Oggetti di configurazione dei criteri

Gli oggetti di configurazione dei criteri di Microsoft Identity Manager rappresentano la logica di business per la distribuzione di MIM. Si tratta di un'area in cui ogni implementazione di MIM probabilmente sarà univoca perché la configurazione dei criteri è specifica per le esigenze di ogni distribuzione. Gli oggetti di configurazione dei criteri MIM includono regole di criteri di gestione, set, flussi di lavoro e regole di sincronizzazione per una particolare distribuzione. Le considerazioni sulle prestazioni chiave relative agli oggetti di configurazione dei criteri MIM includono quanto segue:

- **Set**: ogni operazione nel sistema deve essere valutata rispetto alle appartenenze a un set esistenti e agli aggiornamenti che causano modifiche nell'appartenenza al set. Ad esempio, una semplice modifica, come quella relativa al numero dell'edificio in cui ha sede l'ufficio di una persona, potrebbe non avere un impatto elevato. Altre modifiche, invece, potrebbero avere un impatto a cascata, ad esempio il cambiamento di un manager, che può influire su più oggetti a più livelli.

- **Regole di criteri di gestione**: vengono usate per controllare le regole di controllo di accesso e l'attivazione dei flussi di lavoro. Quando si creano le regole di criteri di gestione, si potrebbe scoprire che è necessario aumentare il numero di set in modo da acquisire vari stati di transizione degli oggetti. Questi set aggiuntivi possono attivare i flussi di lavoro aggiuntivi, in cui ogni flusso di lavoro esegue il mapping a richieste univoche nel sistema. Questo aspetto diventa quindi un altro elemento da includere nella pianificazione della capacità.

Quando si usano gli oggetti di configurazione dei criteri MIM, è necessario considerare anche quanto segue:

- Verrà eseguito il provisioning dei principi di protezione esterna in più foreste di Servizi di dominio Active Directory (AD DS)? In caso di risposta affermativa verranno generati più flussi di lavoro e richieste, determinando un ulteriore carico per il sistema.

- Verrà usato il provisioning senza codice? Questa operazione, se effettuata, influisce sul numero di voci di regole previste, nonché sulle richieste e sui flussi di lavoro associati nel sistema.


## Vedere anche
- La [Guida alla pianificazione della capacità di Forefront Identity Manager (FIM) 2010](http://go.microsoft.com/fwlink/?LinkId=200180) scaricabile esamina più in dettaglio la compilazione di un test e i risultati dei test delle prestazioni.


<!--HONumber=Apr16_HO2-->


