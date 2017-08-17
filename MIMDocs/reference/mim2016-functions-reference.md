---
title: Riferimento alle funzioni per Microsoft Identity Manager 2016 | Microsoft Docs
description: 
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 08/01/2017
ms.topic: reference
ms.prod: identity-manager-2016
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 
ms.openlocfilehash: 8f36cf981971db0d6c55fc17cce874a8faf0ecaf
ms.sourcegitcommit: 5ba5d916c0ca1e5aa501592af0cef714bfdc8afe
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/02/2017
---
# <a name="functions-reference-for-microsoft-identity-manager-2016"></a>Riferimento alle funzioni per Microsoft Identity Manager 2016


In Microsoft Identity Manager (MIM) 2016 le funzioni consentono di modificare i valori degli attributi prima di eseguirne il flusso a una destinazione di un'attività funzione o un provisioning dichiarativo. L'obiettivo di questo documento è offrire una panoramica delle funzioni disponibili e una descrizione delle relative modalità di utilizzo.

La configurazione dei mapping di flusso di attributi è un'attività elementare quando si configurano le regole di sincronizzazione. La forma più semplice di mapping di flusso di attributi è il mapping diretto. Come indica il nome, un mapping diretto accetta il valore di un attributo di origine e lo applica all'attributo di destinazione configurato. Vi sono casi in cui è necessario modificare i valori degli attributi esistenti o calcolare nuovi valori degli attributi prima che il sistema li applichi a una destinazione.

Le funzioni sono un metodo predefinito usato per definire il tipo di modifica che il motore di sincronizzazione deve applicare durante la generazione di un valore di attributo per una destinazione.

In MIM è possibile raggruppare le funzioni esistenti nelle categorie seguenti:

-   **Funzioni di modifica dei dati**. Funzioni che consentono di eseguire diverse operazioni di modifica delle stringhe.

-   **Funzioni di recupero dei dati**. Funzioni che consentono di estrarre i dati dai valori degli attributi.

-   **Funzioni di generazione dei dati**. Funzioni che consentono di generare valori.

-   **Funzioni logiche**. Funzioni che consentono di eseguire operazioni in base alle condizioni.

Le sezioni seguenti contengono informazioni dettagliate sulle funzioni di ogni categoria.

## <a name="data-manipulation-functions"></a>Funzioni di modifica dei dati

Le funzioni di modifica dei dati vengono usate per eseguire diverse operazioni di modifica delle stringhe.

| Concatenate        |   |
|--------------------|-------------------------|
| Descrizione        | La funzione Concatenate si usa per concatenare due o più stringhe.                                                                                                       |
| Firma della funzione | string1 + string2...                                                                                                                                                     |
| Input             | Due o più stringhe                                                                                                                                                        |
| Operazioni         | Tutti i parametri di stringa di input vengono concatenati tra loro.                                                                                                              |
| Output             | Una stringa        |


| UpperCase         |         |
|-------------------|---------|
| Descrizione        | La funzione UpperCase converte tutti i caratteri di una stringa in lettere maiuscole.         |
| Firma della funzione | String UpperCase(string)                                                                                                                                                   |
| Input             | Una stringa                                                                                                                                                                 |
| Operazioni         | Tutti i caratteri minuscoli del parametro di input vengono convertiti in caratteri maiuscoli. Esempio: UpperCase("test") restituisce "TEST".                                     |
| Output             | Una stringa                                                              |


| LowerCase          |                                 |
|--------------------|---------------------------------|
| Descrizione        | La funzione LowerCase converte tutti i caratteri di una stringa in lettere minuscole.                                                                                                  |
| Firma della funzione | String LowerCase(string)                                                                                                                                                   |
| Input             | Una stringa                                                                                                                                                                 |
| Operazioni         | Tutti i caratteri maiuscoli del parametro di input vengono convertiti in caratteri minuscoli. Esempio: LowerCase("TeSt") restituisce "test".                                     |
| Output             | Una stringa               |


| ProperCase        |                                                          |
|-------------------|----------------------------------------------------------|
| Descrizione        | La funzione ProperCase converte in una lettera maiuscola il primo carattere di ogni parola delimitata da spazi in una stringa, mentre tutti gli altri caratteri vengono convertiti in lettere minuscole.           |
| Firma della funzione | String ProperCase(string)                                                                                                                                                  |
| Input             | Una stringa                                                                                                                                                                 |
| Operazioni         | Il primo carattere di ogni parola delimitata da spazi nel parametro di input viene convertito in un carattere maiuscolo e tutti i caratteri maiuscoli vengono convertiti in caratteri minuscoli. Se una parola nel parametro di input inizia con un carattere non alfabetico, il primo carattere della parola non viene convertito in lettera maiuscola. <br/> Esempi: <br/> - ProperCase("TEsT") restituisce "Test". <br/> -   ProperCase("britta simon") restituisce "Britta Simon". <br/>-   ProperCase(" TEsT") restituisce " Test". <br/> -   ProperCase("\$TEsT") restituisce "\$Test".|
| Output             | Una stringa      |


| LTrim              |      |
|--------------------|------|
| Descrizione        | La funzione LTrim rimuove gli spazi vuoti iniziali da una stringa.                                                                                                             |
| Firma della funzione | String LTrim(string)                                                                                                                                                       |
| Input             | Una stringa                                                                                                                                                                 |
| Operazioni         | Gli spazi vuoti iniziali contenuti nel parametro di input vengono rimossi. <br/><br/>Esempio: LTrim(" TeSt ") restituisce "Test ".                                              |
| Output             | Una stringa      |



| RTrim              |                                                                                                                                 |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------|
| Descrizione        | La funzione RTrim rimuove gli spazi vuoti finali da una stringa.                                                                 |
| Firma della funzione | String RTrim(string)                                                                                                            |
| Input             | Una stringa                                                                                                                      |
| Operazioni         | Gli spazi vuoti finali contenuti nel parametro di input vengono rimossi. Esempio: RTrim(" TeSt ") restituisce " Test".  |
| Output             | Una stringa                                                                                                                      |


| Trim               |                                                                                                                                 |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------|
| Descrizione        | La funzione Trim rimuove gli spazi vuoti iniziali e finali da una stringa.                                                      |
| Firma della funzione | String Trim(string)                                                                                                             |
| Input             | Una stringa                                                                                                                      |
| Operazioni         | I caratteri spazi vuoti iniziali e finali contenuti nella stringa vengono rimossi. Esempio: Trim(" TeSt ") restituisce " Test ". |
| Output             | Una stringa                                                                                                                      |




| RightPad           |                                                                                                                                 |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------|
| Descrizione        | La funzione PadRight aggiunge a destra di una stringa il carattere di riempimento specificato, fino alla lunghezza indicata.                          |
| Firma della funzione | String RightPad(string, length, padCharacter)                                                                                   |
| Operazioni         | Se la lunghezza di string è minore di length, padCharacter viene aggiunto ripetutamente alla fine della stringa finché ha una lunghezza uguale a length. <br/> Esempi: <br/> - RightPad("User", 10, "0") restituisce "User000000". <br/> - RightPad(RandomNum(1,10), 5, "0") potrebbe restituire "9000".   |
| Output                                                                                                                                                          | Se string ha una lunghezza maggiore di o uguale a length, viene restituita una stringa identica a string. Se la lunghezza di string è minore di length, viene restituita una nuova stringa con la lunghezza desiderata contenente il valore di string con l'aggiunta di padCharacter. Se la stringa è Null, la funzione restituisce una stringa vuota. |   |   |
>[!NOTE]
**padCharacter** può essere uno spazio, ma non un valore Null. Se la lunghezza di **string** è uguale a o maggiore di **length**, la **stringa** viene restituita invariata.


| LeftPad      |     |
|----|-------|
| Descrizione  | La funzione LeftPad aggiunge a sinistra di una stringa il carattere di riempimento specificato, fino alla lunghezza indicata.    |
| Firma della funzione      | String LeftPad(string, length, padCharacter)     |
| Input |  - **String.** La stringa da riempire. <br/> - **length.** intero che rappresenta la lunghezza della stringa desiderata. <br/> - **padCharacter.** Stringa costituita da un singolo carattere da usare come carattere di riempimento. |
| Operazioni  | Se la lunghezza della stringa è minore di length, padCharacter viene aggiunto ripetutamente all'inizio della stringa finché ha una lunghezza uguale a length. <br/> Esempi: <br/> - LeftPad("User", 10, "0") restituisce "000000User". <br/> - LeftPad(RandomNum(1,10), 5, "0") potrebbe restituire "0009". |  
|Output | Se string ha una lunghezza maggiore di o uguale a length, viene restituita una stringa identica a string. <br/> Se la lunghezza di string è minore di length, viene restituita una nuova stringa con la lunghezza desiderata contenente il valore di string con l'aggiunta di padCharacter. <br/>  Se **string** è Null, la funzione restituisce una stringa vuota.                                                   |

<[!NOTE]
**padCharacter** può essere uno spazio, ma non un valore Null. Se la lunghezza di **string** è uguale a o maggiore di **length**, la **stringa** viene restituita invariata.

| BitOr    |  |
|----- |------|
| Descrizione  | La funzione BitOr imposta su 1 un bit specificato per un flag.     |
| Firma della funzione  | Int BitOr(mask, flag)       |  
| Input     | 1. **mask.** Valore esadecimale che specifica il bit da impostare per il flag. <br/> 2. **flag.** Valore esadecimale per cui si deve modificare un bit specifico.    |   
| Operazioni         | Questa funzione converte entrambi i parametri in rappresentazione binaria e li confronta: <br/> - Imposta un bit su 1 se il valore di uno o entrambi i bit corrispondenti in mask e in flag sono pari a 1 e su 0 se entrambi i bit corrispondenti sono pari a 0. <br/> - Restituisce 1 in tutti i casi, tranne quando i bit corrispondenti di entrambi i parametri sono pari a 0. <br/> - Lo schema di bit risultante sono i bit "impostati" (1 o true) bit di uno dei due operandi. Se più bit hanno il valore 1 in mask, è possibile impostare più bit di flag.  |
| Output             | Una nuova versione di **flag**, con i bit specificati in **mask** impostati su 1.                   |


| BitAnd             |                                                                                    |
|--------------------|------------------------------------------------------------------------------------|
| Descrizione        | La funzione BitAnd imposta su 0 un bit specificato per un flag.                           |
| Firma della funzione | Int BitOr(mask, flag)                                                              |
| Input             | 1. **mask.** Valore esadecimale che specifica il bit da modificare per il flag. <br/> 2. **flag.** Valore esadecimale per cui si deve modificare un bit specifico   |
| Operazioni         | Questa funzione converte entrambi i parametri in rappresentazione binaria e li confronta: <br/> - Imposta un bit su 0 se il valore di uno o entrambi i bit corrispondenti in **mask** e in **flag** sono pari a 0 e su 1 se entrambi i bit corrispondenti sono pari a 1. <br/> - Restituisce 0 in tutti i casi, tranne quando i bit corrispondenti di entrambi i parametri sono pari a 1. Se più bit hanno il valore 0 in **mask**, è possibile impostare più bit di flag su 0. |
| Output             | Una nuova versione di **flag**, con i bit specificati in **mask** impostati su 0.                    |

| DateTimeFormat                                 |    |
|---------------------------------------|------------|
| Descrizione       | La funzione DateTimeFormat viene usata per formattare un valore DateTime in una stringa in un formato specificato.     |
| Firma della funzione   | String DateTimeFormat(dateTime, format)      |
| Input   | 1. dateTime. Stringa che rappresenta il valore DateTime da formattare.  <br/> 2. **format.** Stringa che rappresenta il formato in cui effettuare la conversione.  |   
| Operazioni           | La stringa di formato specificata in format viene applicata al valore DateTime nella stringa dateTime. <br/> La stringa specificata in format deve essere un formato di data/ora valido. In caso contrario, viene restituito un errore che indica che il formato non è un formato DateTime valido. <br/> Esempio: DateTime("12/25/2007", "yyyy-MM-dd") restituisce "2007-12-25".|   
| Output     | Una stringa risultante dall'applicazione di **format** a **dateTime.**   |

>[!Note]                                                                                                                                                                             
Per i caratteri accettati per la creazione di formati definiti dall'utente, vedere [Formati di data/ora definiti dall'utente](http://go.microsoft.com/fwlink/?LinkId=195182)


| ConvertSidToString       |    |   
|--------------------------|----|
| Descrizione       | La funzione ConvertSidToString converte in una stringa una matrice di byte contenente un ID di sicurezza.         |
| Firma della funzione      | String ConvertSidToString(ObjectSID)    |
| Input  | **ObjectSID.** Matrice di byte che contiene un ID di sicurezza (SID).   |
| Operazioni    | Il SID binario specificato viene convertito in una stringa.    |
| Output              | Rappresentazione stringa del SID.   |  

| ConvertStringToGuid |        |
|---------------------|--------|
| Descrizione         | La funzione **ConvertStringToGuid** converte la rappresentazione stringa di un GUID in una rappresentazione binaria del GUID.      |
| Funzione            | Byte[] ConvertStringToGuid(stringGuid)  |  
| Input              | **Guid.** Stringa formattata con questo schema: **xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx**, in cui il valore del GUID è rappresentato come serie di cifre esadecimali in gruppi di 8, 4, 4, 4 e 12 cifre separati da trattini. Un esempio di valore restituito è "382c74c3-721d-4f34-80e557657b6cbc27".  |
| Operazioni          | La stringa **Guid** specificata nel parametro 1 viene convertita nella relativa rappresentazione binaria. <br/> Se la stringa non è una rappresentazione di un oggetto **Guid** valido, la funzione rifiuta l'argomento e un messaggio di errore indica che <br/> **il parametro per la funzione ConvertStringToGuid deve essere una stringa che rappresenta un Guid valido.**  |
| Output              | Rappresentazione binaria del Guid.           |                                                                           


| ReplaceString       |     |
|--------------------|-------|
| Descrizione         | La funzione ReplaceString sostituisce tutte le occorrenze di una stringa in un'altra stringa.  |   
| Funzione            | String ReplaceString(string, OldValue, NewValue)    |                                                                          
| Input              | 1. **String.** Stringa in cui sostituire i valori. <br/> 2. **OldValue.** stringa da cercare e sostituire. <br/> 3. NewValue. stringa oggetto della sostituzione. |
| Operazioni          | Tutte le occorrenze di OldValue nella stringa vengono sostituite con NewValue. La funzione deve essere in grado di gestire i seguenti caratteri speciali: <br/> - **\n.** Nuova riga. <br/> - **\r.** Ritorno a capo. <br/> - **\t.** Tabulazione. <br/> Esempio: ReplaceString("One\n\rMicrosoft\n\r\Way","\n\r"," ") restituisce "One Microsoft Way". |   
| Output              | Una stringa con tutte le occorrenze di **OldValue** della stringa sostituite con **NewValue.**      |

## <a name="data-retrieval-functions"></a>Funzioni di recupero dei dati

Le funzioni di recupero dei dati vengono usate per eseguire operazioni di recupero di caratteri specifici da una stringa.

| Word       |        |
|--------------------|---------------|
| Descrizione        | La funzione Word restituisce una parola contenuta in una stringa, in base ai parametri che descrivono i delimitatori da usare e al numero della parola da restituire.                                                                |
| Firma della funzione | String Word(string, number, delimiters)                                                                                                                                                                        |
| Input             | 1. **string.** La stringa da cui restituire una parola. <br/> 2. **number.** Numero che identifica il numero di parola da restituire. <br/> 3. **delimeters.** Stringa che rappresenta i delimitatori da usare per identificare le parole. |
| Operazioni         | Ogni stringa di caratteri contenuta in stringa separata da uno dei caratteri specificati in delimiters viene identificata come una parola. Viene restituita la parola individuata nella posizione specificata nel parametro 3 (numero): <br/> - Se number è < 1, viene restituita una stringa vuota. <br/> - Se string è Null, viene restituita una stringa vuota. <br/><br/> Esempi: <br/> 1. Word("Test;of%function;", 3, ";$&%") restituisce "function". <br/> 2. Word("Test;;Function" , 2 , ";") restituisce "" (una stringa vuota). 3. Word("Test;of%function;", 0, ";$&%") restituisce "" (una stringa vuota).
| Output             | Una stringa contenente la parola nella posizione richiesta dall'utente. Se **string** contiene un numero di parole inferiore a number o se **string** non contiene alcuna parola identificata da **delimeters**, viene restituita una stringa vuota. |  


| Sinistra               |   |
|-------|-------|
| Descrizione        | La funzione Left restituisce un numero di caratteri specificato a partire da sinistra di una stringa.       |
| Firma della funzione | String Left(string, numChars)     |
| Input             | 1. **string.** La stringa da cui restituire i caratteri. 2. **numChars.** Numero che identifica il numero di caratteri da restituire a partire dall'inizio di una stringa.         |
| Operazioni         | Vengono restituiti **numChars** caratteri a partire dalla prima posizione nella stringa. <br/> Esempio: Left("Britta Simon", 3) restituisce "Bri".   |
| Output             | Una stringa contenente i primi numChars caratteri nella stringa.  <br/> - Se numChars = 0, viene restituita una stringa vuota. <br/> - Se numChars < 0, viene restituita una stringa di input. <br/> - Se string è Null, viene restituita una stringa vuota. |




| Right       |                                                                                                                               |
|-------------|-------------------------------------------------------------------------------------------------------------------------------|
| Descrizione | La funzione Right restituisce un numero di caratteri specificato a partire da destra (fine) di una stringa.                                 |
| Firma della funzione   | String Right(string, numChars)   |
| Input      | 1. **String.** La stringa da cui restituire i caratteri. <br/> 2. **numChars.** Numero che identifica il numero di caratteri da restituire a partire dalla fine della stringa.  |
| Operazioni  | **numChars.** Vengono restituiti numChars caratteri a partire dalla fine di una stringa. <br/> Esempio: Right("Britta Simon", 3) restituisce "mon".                  |
| Output      | Una stringa contenente gli ultimi numChars caratteri in una stringa. Se numChars = 0, viene restituita una stringa vuota. <br/> - Se **numChars** < 0, viene restituita una stringa di input. <br/> - Se la stringa è Null, viene restituita una stringa vuota. <br/> Se la stringa contiene un numero di caratteri inferiore al numero specificato in NumChars, viene restituita una stringa identica a string. |




| Mid         |                                                                                                                               |
|-------------|-------------------------------------------------------------------------------------------------------------------------------|
| Descrizione | La funzione Mid restituisce un numero di caratteri specificato a partire da una posizione specificata in una stringa.                              |
| Firma della funzione    | String Mid(string, pos, numChars)                                                                                             |
| Input      | 1. **string.** La stringa da cui restituire i caratteri.   <br/> 2. **pos.** Numero che identifica la posizione in una stringa da cui iniziare a restituire caratteri. <br/> 3. **numChars.** Numero che identifica il numero di caratteri da restituire a partire da una posizione nella stringa.  |
| Operazioni  | Restituisce **numChars** caratteri a partire dalla posizione **pos** nella stringa. <br/>Esempio: Mid("Britta Simon", 3, 5) restituisce "itta". |
| Output      | Una stringa contenente **numChars** caratteri a partire dalla posizione **pos** in una stringa: <br/> - Se **numChars** = 0, viene restituita una stringa vuota. <br/> - Se **numChars** < 0, viene restituita una stringa vuota. <br/> - Se **pos** è > della lunghezza della stringa, viene restituita una stringa di input. <br/> - Se **pos** ≤ 0, viene restituita una stringa di input. <br/> - Se **string** è Null, viene restituita una stringa vuota. <br/> Se in **string** non rimangono caratteri **numChar** a partire dalla posizione **pos**, vengono restituiti quanti più caratteri possibile.

## <a name="data-generation-functions"></a>Funzioni di generazione dei dati

Le funzioni di generazione dei dati vengono usate per generare valori per tipi di dati specifici.

| CRLF               |                                                                                          |
|--------------------|------------------------------------------------------------------------------------------|
| Descrizione        | La funzione CRLF genera un ritorno a capo/avanzamento riga. Usare questa funzione per aggiungere una nuova riga. |
| Firma della funzione | String CRLF                                                                              |
| Input             | Nessun parametro                                                                            |
| Operazioni         | Viene restituito un oggetto CRLF.                                                                      |
|                    | Esempio: AddressLine1 + CRLF() + AddressLine2 restituisce: <br/> - AddressLine1 <br/> - AddressLine2 |
| Output             | L'output è un ritorno a capo/avanzamento riga.                                                                                             |

| RandomNum          |                                                                                                                   |  
|--------------------|-------------------------------------------------------------------------------------------------------------------|
| Descrizione        | La funzione RandomNum restituisce un numero casuale compreso in un intervallo specificato.                                       |   
| Firma della funzione | Int RandomNum(start, end)                                                                                         |   
| Input             | - **start**. Numero che identifica il limite inferiore del valore casuale da generare.   <br/> - **end**. Numero che identifica il limite superiore del valore casuale da generare.  |
| Operazioni         | Viene generato un numero casuale maggiore o uguale a **start** e minore o uguale a **end**. <br/>  Esempio: Random(0,999) può restituire 100.                      |
| Output             | Un numero casuale all'interno dell'intervallo specificato da **start** ed **end**.                                                      |  

| EscapeDNComponent  |                                                                                                                   |   
|--------------------|-------------------------------------------------------------------------------------------------------------------|
| Descrizione        | Il metodo *EscapeDNComponent* elabora la stringa di input in base al tipo di agente di gestione in uso. |
| Firma della funzione | String EscapeDNComponent(string)                                                                                  |
| Input     | **string**. Stringa usata per elaborare un nome distinto. La stringa non deve contenere caratteri di escape. |
| Operazioni | Per eseguire questa operazione viene usato il metodo EscapeDNComponent da MIISUtils. Questo metodo elabora la stringa di input in base al tipo di agente di gestione in uso. <br/> Poiché i diversi agenti di gestione richiedono diversi formati del nome distinto, questo metodo elabora le stringhe di input in base al tipo di agente di gestione. I tipi di agente sono il nome distinto LDAP (Lightweight Directory Access Protocol), ad esempio Active Directory® Domain Services, Sun Directory Server (in precedenza iPlanet Directory Server) e Microsoft Exchange Server, il tipo gerarchico non LDAP, ad esempio Microsoft Lotus Notes, e il tipo estrinseco, ad esempio database e XML senza nomi distinti LDAP. <br/> **Nome distinto LDAP: ** <br/> - Per tutti i caratteri XML non validi nella porzione del valore di una determinata parte viene usata la codifica esadecimale. <br/>- Tutti i caratteri non consentiti (inclusi i caratteri XML non validi) nella porzione del nome di una determinata parte generano un errore. <br/> - I caratteri seguenti sono preceduti da caratteri escape: <br/> &nbsp;&nbsp;&nbsp; - Virgola (,) <br/> &nbsp;&nbsp;&nbsp; - Segno di uguale (=) <br/> &nbsp;&nbsp;&nbsp; - Segno più (+) <br/> &nbsp;&nbsp;&nbsp; - Segno di minore (<) <br/> &nbsp;&nbsp;&nbsp; - Segno di maggiore (>) <br/> &nbsp;&nbsp;&nbsp; - Cancelletto (#) <br/> &nbsp;&nbsp;&nbsp; - Punto e virgola (;) <br/> &nbsp;&nbsp;&nbsp; - Barra rovesciata (\') <br/> &nbsp;&nbsp;&nbsp; - Virgoletta doppia (") <br/> - Se l'ultimo carattere della stringa è uno spazio, viene preceduto da un carattere di escape. <br/> - Tutti gli spazi iniziali o finali estranei intorno a un nome di parte vengono rimossi. <br/> - Per l'agente di gestione XML, se esistono più parti, vengono indicate in ordine alfabetico. <br/> - Se vengono specificate più parti, la stringa composita del nome distinto è la concatenazione delle singole stringhe separate dal segno più. <br/> - Viene generato un errore se la stringa di input non è una stringa di nome distinto di tipo LDAP formattata correttamente. <br/><br/> **Tipo non LDAP gerarchico** <br/> - Questi agenti di gestione non supportano i componenti con più parti. Se vengono passate più stringhe EscapeDNComponent, viene generata un'eccezione ArgumentException. <br/> - Se alcuni caratteri della stringa di input sono caratteri XML non validi, viene generata un'eccezione ArgumentException. <br/> - Tutte le virgole e le barre rovesciate presenti nella stringa di input vengono precedute da caratteri di escape. <br/> - Se l'ultimo carattere della stringa è uno spazio, viene preceduto da un carattere di escape. <br/><br/> **Tipo estrinseco:** <br/> 1. Se una parte è binaria o contiene un carattere XML non valido, viene memorizzata come versione con codifica esadecimale dei dati non elaborati con un cancelletto (#) come prefisso all'inizio della stringa. Ad esempio, se una parte è "AxC", dove x rappresenta un carattere XML non valido, ad esempio "0x10", tale parte viene codificata come "#410010004300". <br/> 2. In caso contrario, tutte le istanze dei caratteri seguenti vengono precedute da caratteri di escape: <br/> &nbsp;&nbsp;&nbsp; - Barra rovesciata (\') <br/> &nbsp;&nbsp;&nbsp; - Virgola (,) <br/> &nbsp;&nbsp;&nbsp; - Segno più (+) <br/> &nbsp;&nbsp;&nbsp; - Cancelletto (#) <br/> 3. - Se l'ultimo carattere di una data stringa della parte è uno spazio, viene preceduto da un carattere di escape. <br/> 4. Se vengono specificate più parti, la stringa composita del nome distinto è la concatenazione di tutte le singole stringhe separate dal segno più.
| Output      | Una stringa contenente un nome di dominio valido.                                                                                                                  |   

>[!NOTE]
La convalida di nomi distinti è meno rigorosa rispetto alla sintassi definita nelle specifiche LDAP. EscapeDNComponent(String[]) consente a un nome di parte di contenere qualsiasi combinazione di uno o più dei caratteri "a"-"z", "A"-"Z", "0"-"9", "-" e ".". <br/>
Non è possibile specificare una parte binaria con questo metodo. Tuttavia, è possibile usare una parte binaria in **CommitNewConnector** se il nome distinto viene costruito sulla base di attributi di ancoraggio e uno di tali attributi è un tipo binario.


| Null        |                                                                                                                                                           |
|-------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| Descrizione | La funzione Null viene usata per specificare che questo agente di gestione non ha un attributo a cui contribuire e che la precedenza di quell'attributo deve continuare con l'agente di gestione successivo. |   
| Firma della funzione    | String Null    |
| Input      | Nessun parametro                                                                                                                                             |   
| Operazioni  | Viene restituito un valore Null. <br/> Esempio: IIF(Eq(domain), "unknown", Null())                                                                                           |   
| Output      | L'output è un valore Null.                                                                                                                                         |   |   |


## <a name="logic-functions"></a>Funzioni logiche
Le funzioni logiche vengono usate per eseguire un'operazione in base a condizioni valutate dal sistema.

| IIF        |  |
|-------------|---|
| Descrizione | La funzione IIF restituisce uno dei possibili valori di un set, in base a una condizione specificata.    |
| Firma della funzione   | Object IIF(condition, valueIfTrue, valueIfFalse)   |                                                 |
| Input      | 1. **Condition**. Qualsiasi valore o espressione che possa restituire true o false. 2. **valueIfTrue** Valore restituito se la condizione restituisce true. <br/> 3. **valueIfFalse** Valore restituito se la condizione restituisce false. <br/><br/> Le funzioni riportate di seguito possono essere usate come espressioni nella funzione IIF come **condition:** <br/> **Eq.** Questa funzione confronta due argomenti per verificare l'uguaglianza. <br/> **NotEquals.** Questa funzione confronta due argomenti per verificare la disuguaglianza, restituendo true se non sono uguali e false se sono uguali.<br/> Esempio: NotEquals(EmployeeType, "Contractor")<br/> **LessThan.** Questa funzione confronta due numeri e restituisce true se il primo è minore del secondo e false in caso contrario.<br/>Esempio: LessThan(Salary, 100000) <br/>**GreaterThan.** Questa funzione confronta due numeri e restituisce true se il primo è maggiore del secondo e false in caso contrario.<br/> Esempio: GreaterThan(Salary, 100000) <br/> **LessThanOrEquals.** Questa funzione confronta due numeri e restituisce true se il primo è minore o uguale al secondo e false in caso contrario.<br/>Esempio: LessThanOrEquals(Salary, 100000) <br/> **GreaterThanOrEquals.* Questa funzione confronta due numeri e restituisce true se il primo è maggiore o uguale al secondo e false in caso contrario. <br/>Esempio: GreaterThanOrEquals(Salary, 100000)<br/> IsPresent. Questa funzione accetta come input un attributo nello schema ILM e restituisce true se l'attributo non è Null e false se l'attributo è Null.|
| Operazioni  | Se **condition** restituisce true, viene restituito **valueIfTrue.** In caso contrario viene restituito **valueIfFalse.** <br/>Esempio: IIF(Eq(EmployeeType,"Intern"),"t-" + Alias, Alias) restituisce l'alias di un utente con "t-" aggiunto all'inizio se l'utente è un interno. In caso contrario restituisce l'alias dell'utente così com'è. |
| Output      | L'output è **valueIfTrue** se la condizione è true o **valueIfFalse** se la condizione è false. |      
