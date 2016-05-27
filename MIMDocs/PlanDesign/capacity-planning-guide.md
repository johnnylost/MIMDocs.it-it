---
# required metadata

title: Guida alla pianificazione della capacità | Microsoft Identity Manager
description: Usare questa guida per comprendere le variabili da considerare prima di distribuire MIM 2016, inclusi i livelli di carico e le decisioni relative ai criteri.
keywords:
author: kgremban
manager: stevenpo
ms.date: 05/13/2016
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

Microsoft Identity Manager (MIM) consente di creare, aggiornare e rimuovere gli account utente all'interno dell'organizzazione. MIM offre agli utenti finali anche la possibilità di gestire le funzionalità self-service del proprio account. Anche in un ambiente di piccole dimensioni, tutte queste azioni possono sommarsi rapidamente.

Prima di iniziare a usare MIM, consultare questa guida con i relativi ambienti di test per comprendere l'ambito adeguato per la distribuzione. Questo articolo descrive numerosi fattori comuni che è opportuno tenere in considerazione. Tuttavia, dal momento che ogni distribuzione presenta particolarità specifiche, è opportuno testare i propri scenari in un laboratorio per determinare server, hardware o topologie appropriate alle proprie esigenze.

Se non si ha familiarità con MIM 2016 e i relativi componenti, è possibile ottenere altre informazioni su [Microsoft Identity Manager 2016](/microsoft-identity-manager/understand-explore/microsoft-identity-manager-2016) prima di continuare.

## Panoramica
Esistono numerose variabili che possono influenzare la capacità e le prestazioni complessive della distribuzione di Microsoft Identity Manager. I modi in cui vengono fisicamente distribuiti i componenti MIM (topologia), nonché l'hardware su cui tali componenti vengono eseguiti, sono fattori importanti per determinare le prestazioni e la capacità che è possibile attendersi dalla distribuzione di MIM. Il numero e la complessità degli oggetti di configurazione dei criteri MIM potrebbero essere meno evidenti, ma sono comunque fattori significativi di cui tenere conto durante la pianificazione della capacità. Infine, il ridimensionamento previsto della distribuzione, nonché il relativo carico, sono generalmente fattori che influiscono in modo più evidente su prestazioni e capacità.

I fattori principali che influiscono sulla capacità e sulle prestazioni che ci si può attendere da una distribuzione di MIM 2016 sono esaminati nella tabella seguente.

| Fattore di progettazione | Considerazioni |
| ------------- | -------------- |
| Topologia | La distribuzione dei servizi MIM tra i computer sulla rete. |
| Hardware | L'hardware fisico e qualsiasi specifica di hardware virtualizzato che si esegue per ogni componente MIM. Sono inclusi CPU, memoria, scheda di rete e configurazione dell'unità disco rigido. |
| Oggetti di configurazione dei criteri MIM | Il numero e il tipo di oggetti di configurazione dei criteri MIM, che include set, regole di criteri di gestione e flussi di lavoro. |
| Scale | Il numero di utenti, gruppi, gruppi calcolati e tipi di oggetto personalizzato, ad esempio i computer da gestire con MIM 2016. Prendere in considerazione anche la complessità dei gruppi dinamici e assicurarsi di includere l'annidamento gruppo. |
| Carica | Frequenza d'uso. Ad esempio, la frequenza in cui si prevede di creare nuovi gruppi o utenti, reimpostare password o visitare il portale in un determinato periodo di tempo. Si noti che il carico può variare nel corso di un'ora, di un giorno, di una settimana o di un anno. A seconda del componente, è possibile eseguire la progettazione per un carico di picco o un carico medio. |


## Hosting dei componenti di Microsoft Identity Manager

I componenti di Microsoft Identity Manager non sono necessariamente situati sullo stesso computer. Valutare questi componenti e le macchine fisiche o virtuali su cui verranno installati rappresenta una parte importante della pianificazione della capacità.

I fattori hardware possono influenzare le prestazioni dell'ambiente MIM. Ad esempio:
- Informazioni sulla configurazione del disco fisico per il computer che esegue il database SQL del servizio MIM 2016 Il numero di assi che costituiscono la configurazione del disco o la distribuzione di file di log e di dati possono influire notevolmente sulle prestazioni del sistema.

Durante la configurazione, tenere presenti anche i fattori esterni. Ad esempio:
- Se si usa una rete SAN come la configurazione del database del servizio MIM 2016, quali altre applicazioni condividono tale rete? Queste applicazioni potrebbero influire sulle prestazioni del database quando si contendono le risorse condivise del disco sulla rete SAN.


## Utenti e gruppi
Il numero di utenti e gruppi nell'ambiente è un aspetto di cui si tiene generalmente conto per il ridimensionamento della distribuzione. Esistono, tuttavia, diverse altre considerazioni correlate da considerare nella pianificazione,

- Gli utenti possono creare gruppi? In questo caso, è consigliabile effettuare una stima dell'influenza che la creazione di nuovi gruppi di utenti avrà sulla crescita dei gruppi nel proprio ambiente.

- Verranno distribuiti gruppi dinamici? Determinare quanti e quali tipi di gruppi dinamici sono previsti nel proprio ambiente.


## Livelli di carico previsto
È necessario anche considerare il tipo di carico che sarà posizionato sui componenti MIM. Queste informazioni possono essere probabilmente stimate esaminando le applicazioni correnti nell'ambiente in uso. Di seguito sono elencate alcune domande rilevanti da considerare:

- Con quale frequenza si prevede di ricevere una richiesta di aggiunta o rimozione da un gruppo?

- Con che frequenza si prevede che un utente creerà un gruppo statico o dinamico?

- Quante operazioni non basate sull'utente sono previste, ad esempio la sincronizzazione di modifiche provenienti da sistemi esterni? Assicurarsi di tenere conto del carico generato dalla sincronizzazione delle informazioni di identità con sistemi esterni.

- Che tipi di scenari si intende distribuire? I modelli di carico saranno diversi a seconda dei vari scenari. Ad esempio, i computer client in cui è installato il client MIM 2016 verificano periodicamente se al momento dell'accesso è necessaria la registrazione, operazione che aumenta il carico del sistema.

- Si prevedono notevoli variazioni nei livelli di carico, da un carico normale a un carico di picco? Ad esempio, dopo i periodi di festività si registra una quantità elevata di reimpostazioni di password. Assicurarsi di definire le pianificazioni di sincronizzazione e manutenzione del sistema al di fuori dei picchi di utilizzo previsto. Quando si considera la pianificazione della capacità, assicurarsi di tenere conto dei periodi di carico.


## Oggetti di configurazione dei criteri

Gli oggetti di configurazione dei criteri di Microsoft Identity Manager includono regole di criteri di gestione, set, flussi di lavoro e regole di sincronizzazione per una particolare distribuzione. Le distribuzioni MIM sono specifiche per ogni cliente poiché la configurazione dei criteri cambia per soddisfare le esigenze di ogni distribuzione. Le considerazioni sulle prestazioni chiave relative agli oggetti di configurazione dei criteri MIM includono quanto segue:

- **Set**: ogni operazione nel sistema deve essere valutata rispetto alle appartenenze a un set esistenti e agli aggiornamenti che causano modifiche nell'appartenenza al set. Ad esempio, una semplice modifica, come quella relativa al numero dell'edificio in cui ha sede l'ufficio di una persona, potrebbe non avere un impatto elevato. Altre modifiche, invece, potrebbero avere un impatto a cascata, ad esempio il cambiamento di un manager, che può influire su più oggetti a più livelli.

- Le **Regole di criteri di gestione** (MPR) gestiscono le regole di controllo di accesso e l'attivazione dei flussi di lavoro. Quando si creano le regole di criteri di gestione, si potrebbe scoprire che è necessario aumentare il numero di set in modo da acquisire vari stati di transizione degli oggetti. Questi set aggiuntivi possono attivare i flussi di lavoro aggiuntivi, in cui ogni flusso di lavoro esegue il mapping a richieste univoche nel sistema. Questo aspetto diventa quindi un altro elemento da includere nella pianificazione della capacità.

La configurazione dei criteri MIM include anche decisioni sull'esecuzione del provisioning nel proprio ambiente. Considerare i fattori seguenti:

- Verrà eseguito il provisioning dei principi di protezione esterna in più foreste di Servizi di dominio Active Directory (AD DS)? In caso di risposta affermativa verranno generati più flussi di lavoro e richieste, determinando un ulteriore carico per il sistema.

- Verrà usato il provisioning senza codice? Questa operazione, se effettuata, influisce sul numero di voci di regole previste, nonché sulle richieste e sui flussi di lavoro associati nel sistema.


## Vedere anche
- [Considerazioni relative alla topologia per la distribuzione di MIM](topology-considerations.md)
- La [Guida alla pianificazione della capacità di Forefront Identity Manager (FIM) 2010](http://go.microsoft.com/fwlink/?LinkId=200180) scaricabile esamina più in dettaglio la compilazione di un test e i risultati dei test delle prestazioni.


<!--HONumber=May16_HO3-->


