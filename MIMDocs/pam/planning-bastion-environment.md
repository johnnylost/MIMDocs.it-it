---
title: Pianificazione di un ambiente bastion | Documentazione Microsoft
description: 
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/16/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: bfc7cb64-60c7-4e35-b36a-bbe73b99444b
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: bfc73723bdd3a49529522f78ac056939bb8025a3
ms.openlocfilehash: b459906f0c8d2c631e9b63813e208c9098ea5a4e
ms.lasthandoff: 05/02/2017


---

# <a name="planning-a-bastion-environment"></a>Pianificazione di un ambiente bastion

L'aggiunta di un ambiente bastion con una foresta amministrativa dedicata a un'istanza di Active Directory consente alle organizzazioni di gestire facilmente gli account amministrativi, le workstation e i gruppi in un ambiente che dispone di controlli di sicurezza più avanzati rispetto all'ambiente di produzione esistente.

Questa architettura consente un numero di controlli che non sono possibili o sono difficilmente configurabili in un'architettura a foresta singola, tra cui il provisioning degli account come utenti standard senza privilegi nella foresta amministrativa che hanno privilegi elevati nell'ambiente di produzione, consentendo maggiore imposizione tecnica della governance. Questa architettura consente inoltre l'utilizzo della funzionalità di autenticazione selettiva di un trust come mezzo per limitare gli accessi e l'esposizione delle credenziali solo a host autorizzati. In situazioni in cui si desidera un maggiore livello di sicurezza per la foresta di produzione senza maggiori costi e complessità di una ricompilazione completa, una foresta amministrativa può fornire un ambiente che consenta di aumentare il livello di garanzia dell'ambiente di produzione.

Oltre alla foresta amministrativa dedicata, è possibile utilizzare tecniche aggiuntive, tra cui limitare l’esposizione delle credenziali amministrative, limitare i privilegi di ruolo degli utenti in tale foresta e assicurare attività amministrative che non vengono eseguite su host utilizzati per le attività degli utenti standard (ad esempio, posta elettronica e browser web).

## <a name="best-practice-considerations"></a>Considerazioni sulle procedure consigliate

Una foresta amministrativa dedicata è una foresta Active Directory di un singolo dominio standard usata per la gestione di Active Directory. Un vantaggio relativo all'uso di domini e foreste amministrative è che possono avere misure di sicurezza maggiori rispetto alle foreste di produzione per via dei casi d'uso limitati. Poiché questa foresta è separata e non considera attendibili le foreste esistenti dell'organizzazione, una compromissione della sicurezza in un'altra foresta non si estenderà anche alla foresta dedicata.

La progettazione di una foresta amministrativa presenta le seguenti considerazioni:

### <a name="limited-scope"></a>Ambito limitato

Il valore di una foresta amministrativa è l'elevato livello di garanzia della sicurezza e la superficie di attacco ridotta. La foresta può ospitare applicazioni e funzioni di gestione aggiuntive, ma ogni incremento nell'ambito aumenta la superficie di attacco della foresta e delle relative risorse. L'obiettivo consiste nel limitare le funzioni della foresta per ridurre la superficie di attacco.

In base al [modello Tier](tier-model-for-partitioning-administrative-privileges.md) di partizionamento dei privilegi amministrativi, gli account in una foresta amministrativa dedicata devono essere in un singolo livello, in genere il livello 0 o 1. Se una foresta è nel livello 1, è consigliabile limitarla a un particolare ambito di applicazione (ad esempio, app finanziarie) o comunità di utenti (ad esempio, fornitori IT in outsourcing).

### <a name="restricted-trust"></a>Trust con restrizioni

La foresta *CORP* di produzione deve stabilire un trust con la foresta *PRIV* di amministrazione, ma non viceversa. Può trattarsi di un trust di dominio o un trust tra foreste. Non è necessario che il dominio della foresta amministrativa consideri attendibili i domini gestiti e le foreste per gestire Active Directory, anche se altre applicazioni possono richiedere una relazione di trust bidirezionale, una convalida di sicurezza e un test.

L'autenticazione selettiva dovrebbe essere usata per garantire che gli account nella foresta amministrativa usino solo gli host di produzione appropriati. Per mantenere i controller di dominio e delegare i diritti in Active Directory, questo in genere richiede la concessione del diritto "Accesso consentito" ai controller di dominio per account amministrativi designati di livello 0 nella foresta amministrativa. Per altre informazioni, vedere [Configuring Selective Authentication Settings](http://technet.microsoft.com/library/cc816580.aspx) (Configurazione delle impostazioni di autenticazione selettiva).

## <a name="maintain-logical-separation"></a>Mantenere la separazione logica

Per garantire che l'ambiente bastion non venga interessato da eventi di sicurezza esistenti o futuri nell'istanza aziendale di Active Directory, è necessario usare le linee guida seguenti durante la preparazione di sistemi per l'ambiente bastion:

- I server Windows non devono essere aggiunti a un dominio o utilizzare software o distribuire impostazioni dall'ambiente esistente.

- L'ambiente bastion deve contenere i propri servizi di dominio di Active Directory, fornendo Kerberos e LDAP, DNS, l'ora e servizi orari all'ambiente bastion.

- MIM non deve usare una farm di database SQL nell'ambiente esistente. SQL Server deve essere distribuito in server dedicati nell'ambiente bastion.

- L'ambiente bastion richiede Microsoft Identity Manager 2016, in particolare devono essere distribuiti i componenti di servizio MIM e PAM.

- Il software di backup e i supporti di memorizzazione per l'ambiente bastion devono essere mantenuti separati da quello dei sistemi nelle foreste esistenti, in modo che un amministratore della foresta esistente non possa compromettere un backup dell'ambiente bastion.

- Gli utenti che gestiscono i server dell’ambiente bastion devono accedere dalle workstation che non sono accessibili per gli amministratori dell'ambiente esistente, in modo che le credenziali per l'ambiente bastion non vengano comunicate.

## <a name="ensure-availability-of-administration-services"></a>Verificare la disponibilità dei servizi di amministrazione

Poiché l'amministrazione delle applicazioni viene passata all'ambiente bastion, considerare come fornire disponibilità sufficiente per soddisfare i requisiti di tali applicazioni. Le tecniche includono:

- Distribuire servizi di dominio di Active Directory in più computer nell'ambiente bastion. Ne sono necessari almeno due per garantire l'autenticazione continua, anche se un server viene riavviato temporaneamente per la manutenzione pianificata. Possono essere necessari altri computer per carichi più elevati o per gestire risorse e amministratori basati in più aree geografiche.

- Preparare account break glass nella foresta esistente e nella foresta amministrativa dedicata, per casi di emergenza.

- Distribuire SQL Server e servizio MIM in più computer nell'ambiente bastion.

- Mantenere una copia di backup di AD e SQL per ogni modifica apportata a utenti o definizioni di ruolo nella foresta amministrativa dedicata.

## <a name="configure-appropriate-active-directory-permissions"></a>Configurare autorizzazioni appropriate di Active Directory

La foresta amministrativa deve essere configurata per privilegi minimi, in base ai requisiti per l'amministrazione di Active Directory.

- Gli account nella foresta amministrativa utilizzati per amministrare l'ambiente di produzione non devono avere privilegi amministrativi per la foresta amministrativa, i domini o le workstation in essa contenuti.

- I privilegi amministrativi sulla foresta amministrativa devono essere controllati attentamente da un processo offline per ridurre la possibilità che un utente o dipendente malintenzionato cancelli i registri di controllo. Ciò consente di garantire che il personale con gli account amministrativi di produzione non possano ridurre le restrizioni per i loro account e aumentare i rischi per l'organizzazione.

- La foresta amministrativa deve seguire le configurazioni di Microsoft Security Compliance Manager (SCM) per il dominio, tra cui configurazioni complesse per i protocolli di autenticazione.

Quando si crea l'ambiente bastion, prima di installare Microsoft Identity Manager, identificare e creare gli account che verranno usati per l'amministrazione all'interno di questo ambiente. Ciò comprende:

- **Account break glass** che devono solo essere in grado di accedere ai controller di dominio nell'ambiente bastion.

- **Amministratori "Red card"** che effettuano il provisioning di altri account ed eseguono la manutenzione non pianificata. A questi account non viene fornito alcun accesso alle foreste o sistemi esistenti all'esterno dell'ambiente bastion. Le credenziali, ad esempio, smartcard, devono essere protette fisicamente e l'uso di questi account deve essere registrato.

- **Account del servizio** richiesti da Microsoft Identity Manager, SQL Server e altri software.

## <a name="harden-the-hosts"></a>Protezione avanzata degli host

Tutti gli host, inclusi controller di dominio, server e workstation connessi alla foresta amministrativa, devono avere i sistemi operativi più recenti e i service pack installati e aggiornati.

- Le applicazioni necessarie per l'amministrazione devono essere pre-installate nelle workstation in modo che gli account che le usano non debbano appartenere al gruppo di amministratori locali per installarle. La manutenzione del controller di dominio può essere eseguita in genere con RDP e strumenti di amministrazione remota del server.

- Gli host della foresta amministrativa devono essere aggiornati automaticamente con gli aggiornamenti della sicurezza. Anche se ciò può comportare il rischio di interrompere le operazioni di manutenzione del controller di dominio, fornisce una mitigazione significativa dei rischi di sicurezza delle vulnerabilità senza patch.

### <a name="identify-administrative-hosts"></a>Identificare gli host di amministrazione

Il rischio di un sistema o di una workstation deve essere misurato dall'attività di rischio più elevata che viene eseguita su di essa, ad esempio l'esplorazione Internet, l'invio e la ricezione di messaggi di posta elettronica o l'uso di altre applicazioni che elaborano contenuto sconosciuto o non attendibile.

Gli host amministrativi includono i seguenti computer:

- Un desktop in cui le credenziali dell'amministratore sono fisicamente digitate o immesse.

- I “server di collegamento” amministrativi nei quali le sessioni di amministrazione e gli strumenti vengono eseguiti.

- Tutti gli host in cui vengono eseguite le azioni amministrative, compresi quelli che utilizzano desktop utente standard con un client RDP per amministrare in remoto i server e le applicazioni.

- I server che ospitano le applicazioni che devono essere gestiti e non sono accessibili tramite RDP con Restricted Admin Mode o comunicazione remota Windows PowerShell.

### <a name="deploy-dedicated-administrative-workstations"></a>Distribuire workstation amministrative dedicate

Anche se risulta poco pratico, possono essere necessarie workstation separate con sicurezza avanzata dedicate agli utenti con credenziali amministrative ad alto impatto. È importante specificare un host con un livello di sicurezza uguale o maggiore a quello dei privilegi affidati alle credenziali. Si consideri l'inclusione delle misure di protezione aggiuntiva seguenti:

- **Verificare che tutti i supporti di memorizzazione nella build siano puliti** per ridurre i rischi malware installati in un'immagine master o inseriti in un file di installazione durante il download o l'archiviazione.

- Gli **standard di sicurezza** devono essere usati come configurazioni di avvio. Microsoft Security Compliance Manager (SCM) consente di configurare gli standard sugli host di amministrazione.

- **Avvio protetto** per impedire ai pirati informatici o ai malware, il tentativo di caricare codice non firmato nel processo di avvio.

- **Restrizione software** per garantire che solo il software amministrativo autorizzato venga eseguito negli host amministrativi. I clienti possono utilizzare AppLocker per questa attività con un elenco di applicazioni autorizzate, per prevenire l'esecuzione di applicazioni non supportate o software dannosi.

- **Codifica volume completa** per ridurre i rischi di perdita fisica del computer, ad esempio portatili amministrativi utilizzati in modalità remota.

- **Restrizioni USB** per proteggere da infezioni fisiche.

- **Isolamento rete** per proteggere da attacchi di rete e azioni di amministrazione accidentali. I firewall host dovrebbero bloccare tutte le connessioni in ingresso tranne quelle esplicitamente necessarie e bloccare completamente l'accesso Internet in uscita non necessario.

- **Antimalware** per proteggere da malware e minacce note.

- **Sfruttamento delle attenuazioni** per ridurre i rischi di minacce e attacchi sconosciuti, tra cui la soluzione Enhanced Mitigation Experience Toolkit (EMET).

- **Analisi della superficie di attacco** per impedire l'introduzione di nuovi vettori di attacchi a Windows durante l'installazione del nuovo software. Strumenti come l'analizzatore della superficie di attacco consentono di valutare le impostazioni di configurazione in un host e identificare i vettori di attacco introdotti da modifiche al software o alla configurazione.

- I **privilegi amministrativi** non devono essere assegnati agli utenti nel computer locale.

- **Modalità RestrictedAdmin** per le sessioni RDP in uscita, tranne quando richiesto dal ruolo. Per altre informazioni, vedere [Novità di Servizi Desktop remoto in Windows Server](https://technet.microsoft.com/library/dn283323.aspx).

Alcune di queste misure potrebbero sembrare estreme, ma rivelazioni pubbliche negli ultimi anni hanno illustrato le significative capacità che possiedono gli avversari esperti nel compromettere le destinazioni.

## <a name="prepare-existing-domains-to-be-managed-by-the-bastion-environment"></a>Preparare i domini esistenti per la gestione da parte dell'ambiente bastion

MIM usa i cmdlet PowerShell per la definizione di relazioni di trust tra i domini di AD esistenti e la foresta amministrativa dedicata nell'ambiente bastion. Dopo aver distribuito l'ambiente bastion e prima di convertire utenti o gruppi in JIT, i cmdlet `New-PAMTrust` e `New-PAMDomainConfiguration` aggiornano le relazioni di trust del dominio e creano gli artefatti necessari per AD e MIM.

Quando viene modificata la topologia di Active Directory esistente, i cmdlet `Test-PAMTrust`, `Test-PAMDomainConfiguration`, `Remove-PAMTrust` e `Remove-PAMDomainConfiguration` possono essere utilizzati per aggiornare le relazioni di trust.

## <a name="establish-trust-for-each-forest"></a>Stabilire relazioni di trust per ogni foresta

Il cmdlet `New-PAMTrust` deve essere eseguito una volta per ogni foresta esistente. Viene richiamato sul computer del servizio MIM nel dominio amministrativo. I parametri per questo comando sono il nome di dominio del dominio principale della foresta esistente e le credenziali di un amministratore del dominio.

```
New-PAMTrust -SourceForest "contoso.local" -Credentials (get-credential)
```

Una volta stabilita la relazione di trust, configurare ogni dominio per abilitare la gestione dall'ambiente bastion, come descritto nella sezione successiva.

## <a name="enable-management-of-each-domain"></a>Abilitare la gestione per ogni dominio

Esistono sette requisiti per l'abilitazione della gestione per un dominio esistente.

### <a name="1-a-security-group-on-the-local-domain"></a>1. Un gruppo di sicurezza nel dominio locale

È necessario un gruppo nel dominio esistente, il cui nome è il nome di dominio NetBIOS seguito da tre simboli del dollaro, ad esempio *CONTOSO$$$*. L'ambito del gruppo deve essere *locale di dominio* e il tipo di gruppo deve essere *Sicurezza*. Questo è necessario per creare i gruppi nella foresta amministrativa dedicata con lo stesso ID di sicurezza dei gruppi nel dominio. Creare questo gruppo tramite il comando di PowerShell seguente, eseguito da un amministratore del dominio esistente in una workstation aggiunta al dominio esistente:

```
New-ADGroup -name 'CONTOSO$$$' -GroupCategory Security -GroupScope DomainLocal -SamAccountName 'CONTOSO$$$'
```

### <a name="2-success-and-failure-auditing"></a>2. Controllo delle operazioni riuscite e non riuscite

Le impostazioni di Criteri di gruppo nel controller di dominio per il controllo devono includere degli eventi di controllo con esito positivo e negativo per Controlla gestione degli account e Controlla accesso al servizio directory. Questo può essere eseguito con la console Gestione criteri di gruppo, eseguita da un amministratore del dominio esistente ed eseguito in una workstation aggiunta al dominio esistente:

3. Passare a **Start** > **Strumenti di amministrazione** > **Gestione Criteri di gruppo**.

4. Passare a **Foresta: contoso.local** > **, Domini** > **contoso.local** > **Controller di dominio** > **Criterio controller di dominio predefinito**. Viene visualizzato un messaggio informativo.

    ![Criterio controller di dominio predefinito - Screenshot](media/pam-group-policy-management.jpg)

5. Fare clic con il pulsante destro del mouse su **Criterio controller di dominio predefinito** e scegliere **Modifica**. Verrà visualizzata una nuova finestra.

6. Nella finestra Editor Gestione Criteri di gruppo, sotto l'albero Criterio controller di dominio predefinito, passare a **Configurazione computer** > **Criteri** > **Impostazioni di Windows** > **Impostazioni di sicurezza** > **Criteri locali** > **Criteri di controllo**.

    ![Editor Gestione Criteri di gruppo - Screenshot](media/pam-group-policy-management-editor.jpg)

5. Nel riquadro dei dettagli fare clic con il pulsante destro del mouse su **Controlla gestione degli account** e selezionare **Proprietà**. Selezionare **Definisci le impostazioni relative ai criteri**, inserire una casella di controllo su **Operazione riuscita**, inserire una casella di controllo su **Operazione non riuscita**, fare clic su **Applica** e su **OK**.

6. Nel riquadro dei dettagli fare clic con il pulsante destro del mouse su **Controlla accesso al servizio directory** e selezionare **Proprietà**. Selezionare **Definisci le impostazioni relative ai criteri**, inserire una casella di controllo su **Operazione riuscita**, inserire una casella di controllo su **Operazione non riuscita**, fare clic su **Applica** e su **OK**.

    ![Impostazioni dei criteri delle operazioni riuscite e non riuscite - Screenshot](media/pam-group-policy-management-editor2.jpg)

7. Chiudere la finestra Editor Gestione Criteri di gruppo e la finestra Gestione Criteri di gruppo. Applicare le impostazioni di controllo avviando una finestra di PowerShell e digitando:

    ```
    gpupdate /force /target:computere
    ```

Il messaggio "Aggiornamento dei criteri computer completato." dovrebbe essere visualizzato dopo pochi minuti.

### <a name="3-allow-connections-to-the-local-security-authority"></a>3. Connessioni all'autorità di sicurezza locale

I controller di dominio devono consentire il traffico RPC su connessioni TCP/IP per l'autorità di sicurezza locale dall'ambiente bastion. Nelle versioni precedenti di Windows Server, il supporto per TCP/IP in LSA deve essere abilitato nel Registro di sistema:

```
New-ItemProperty -Path HKLM:SYSTEM\\CurrentControlSet\\Control\\Lsa -Name TcpipClientSupport -PropertyType DWORD -Value 1
```

### <a name="4-create-the-pam-domain-configuration"></a>4. Creazione della configurazione del dominio PAM

Il cmdlet `New-PAMDomainConfiguration` deve essere eseguito sul computer del servizio MIM nel dominio amministrativo. I parametri per questo comando sono il nome di dominio del dominio esistente e le credenziali di un amministratore del dominio.

```
 New-PAMDomainConfiguration -SourceDomain "contoso" -Credentials (get-credential)
```

### <a name="5-give-read-permissions-to-accounts"></a>5. Concessione di autorizzazioni di lettura agli account

Gli account nella foresta bastion utilizzati per stabilire i ruoli (amministratori che utilizzano i cmdlet `New-PAMUser` e `New-PAMGroup` ), nonché l'account utilizzato dal servizio di monitoraggio MIM necessitano di autorizzazioni di lettura in tale dominio.

La procedura seguente abilita l'accesso in lettura per l'utente *PRIV\Administrator* al dominio *Contoso* all'interno del controller di dominio *CORPDC*:

1. Assicurarsi di essere connessi a CORPDC come amministratore di dominio Contoso (ad esempio, Contoso\Administrator).

2. Avviare Utenti e computer di Active Directory.

3. Fare clic con il pulsante destro del mouse sul dominio **contoso.local** e selezionare **Delega controllo**.

4. Nella scheda Gruppi e utenti selezionati fare clic su **Aggiungi**.

5. Nella finestra popup Seleziona utenti, computer o gruppi fare clic su **Percorsi** e cambiare il percorso in *priv.contoso.local*. Nel nome dell'oggetto digitare *Domain Admins* e fare clic su **Controlla nomi**. Quando viene visualizzata una finestra popup, per il nome utente digitare *priv\administrator* e la password.

6. Dopo Domain Admins, digitare *; MIMMonitor*. Quando i nomi Domain Admins e MIMMonitor risultano sottolineati, fare clic su **OK**, quindi scegliere **Avanti**.

7. Nell'elenco delle attività comuni, selezionare **Legge tutte le informazioni utente**, quindi fare clic su **Avanti** e su **Fine**.

18. Chiudere Utenti e computer di Active Directory.

### <a name="6-a-break-glass-account"></a>6. Un account break glass

Se l'obiettivo di Privileged Access Management consiste nel ridurre il numero di account con privilegi di amministratore di dominio assegnati al dominio in modo permanente, deve essere presente un account *Break glass* nel dominio, nel caso in cui si verifichi un problema con la relazione di trust. Gli account di accesso di emergenza per la foresta di produzione devono essere disponibili in ogni dominio e devono essere in grado di accedere solo ai controller di dominio. Per le organizzazioni con più siti, potrebbero essere necessari altri account per la ridondanza.

### <a name="7-update-permissions-in-the-bastion-environment"></a>7. Aggiornamento delle autorizzazioni nell'ambiente bastion

Esaminare le autorizzazioni nell'oggetto *AdminSDHolder* nel contenitore di sistema di tale dominio. L’oggetto *AdminSDHolder* ha un elenco di controllo di accesso (ACL) univoco, che viene utilizzato per controllare le autorizzazioni delle entità di sicurezza che sono membri di gruppi di Active Directory con privilegi predefiniti. Si noti che se state apportate modifiche alle autorizzazioni predefinite influirebbero sugli utenti con privilegi amministrativi nel dominio, poiché tali autorizzazioni non verranno applicate agli utenti con account nell'ambiente bastion.

## <a name="select-users-and-groups-for-inclusion"></a>Selezionare utenti e gruppi per l'inclusione

Il passaggio successivo consiste nel definire i ruoli PAM, associando utenti e gruppi a cui essi hanno accesso. Ciò corrisponderà in genere a un sottoinsieme di utenti e gruppi per il livello identificato gestito nell’ambiente bastion. Altre informazioni sono disponibili in [Defining roles for Privileged Access Management](defining-roles-for-pam.md) (Definizione di ruoli per Privileged Access Management).

