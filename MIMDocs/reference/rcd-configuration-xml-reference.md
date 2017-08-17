---
title: Informazioni di riferimento per il file XML di configurazione visualizzazione controllo risorse | Microsoft Docs
description: 
keywords: 
author: fimguy
ms.author: fimguy
manager: mbaldwin
ms.date: 05/1/2017
ms.topic: reference
ms.prod: identity-manager-2016
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 
ms.openlocfilehash: c6ed843fb15b150fc934062945ab76ba32d72d33
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/13/2017
---
# <a name="resource-control-display-configuration-xml-reference"></a>Informazioni di riferimento per il file XML di configurazione visualizzazione controllo risorse

Le risorse di configurazione visualizzazione controllo risorse (RCDC, Resource Control Display Configuration) sono risorse definite dall'utente che è possibile usare per controllare la modalità di visualizzazione nell'interfaccia utente per l'utente finale di altre risorse nell'archivio dati di Microsoft Identity Manager (MIM) 2016 SP1. Ogni risorsa RCDC contiene un file di configurazione XML che è possibile modificare per aggiungere, modificare o rimuovere testo e controlli dell'interfaccia utente. MIM 2016 SP1 offre diverse risorse RCDC predefinite, ma è anche possibile creare risorse RCDC personalizzate per risorse personalizzate. Per altre informazioni sull'uso dell'interfaccia utente RCDC nel portale FIM, vedere [Introduction to Configuring and Customizing the FIM Portal](http://go.microsoft.com/fwlink/?LinkID=165848) (Introduzione alla configurazione e alla personalizzazione del portale FIM) nella documentazione di FIM.


## <a name="known-issues"></a>Problemi noti

Molti controlli RCDC non supportano un valore predefinito

In questa versione, l'impostazione di valori predefiniti nei controlli risorsa è supportata solo per i controlli pulsante di opzione. Come soluzione alternativa per le caselle di riepilogo a discesa è possibile specificare un valore predefinito non associato ad alcun valore, per forzare l'utente a modificare la selezione. Come soluzione alternativa per altri controlli, è necessario specificare un valore predefinito durante l'invio della richiesta tramite un flusso di lavoro di autorizzazione.

## <a name="basic-structure"></a>Struttura di base

I dati XML per una risorsa RCDC sono costituiti dal singolo elemento XML **ObjectControlConfiguration**

>[!NOTE]
Per lo schema XSD completo, vedere l'Appendice A: Schema XSD predefinito più avanti in questo documento.

Di seguito è riportato lo schema XSD per l'elemento ObjectControlConfiguration:

```XML
<xsd:element name="ObjectControlConfiguration"\>
  <xsd:complexType\>
    <xsd:sequence\>
      <xsd:element ref="my:ObjectDataSource" minOccurs="0" maxOccurs="32"/>
      <xsd:element ref="my:XmlDataSource" minOccurs="0" maxOccurs="32"/>
      <xsd:element ref="my:Panel"/>
      <xsd:element ref="my:Events" minOccurs="0" maxOccurs="1"/>
    </xsd:sequence>
    <xsd:attribute ref="my:TypeName"/>
    <xsd:anyAttribute processContents="lax" namespace="http://www.w3.org/XML/1998/namespace"/>
  </xsd:complexType>
</xsd:element>
```



L'elemento **ObjectControlConfiguration** contiene quanto segue:

1.  **ObjectDataSource**: questo elemento specifica il TypeName di una classe origine dati usata dal controllo risorsa. Per la descrizione e la definizione dello schema, vedere la prossima sezione, Origini dati, in questo documento. Un elemento **ObjectControlConfiguration** può contenere un massimo di 32 nodi dell'elemento **ObjectDataSource**.

2.  **XmlDataSource**: si tratta di un'origine dati semplice usata molto frequentemente per specificare la progettazione di una pagina di riepilogo. Per la descrizione e la definizione dello schema, vedere la prossima sezione, Origini dati, in questo documento. Un elemento **ObjectControlConfiguration** può contenere un massimo di 32 nodi dell'elemento **XmlDataSource**.

3.  **Panel**: l'amministratore può personalizzare il layout della pagina RCDC modificando gli elementi all'interno degli elementi Panel. Per altre informazioni, vedere la sezione Panel più avanti in questo documento. Un elemento **ObjectControlConfiguration** deve avere un solo elemento Panel.

4.  **Events**: gli amministratori non possono fornire code-behind personalizzato. Questa funzionalità è limitata. Questo è l'elemento Event che può essere generato da un pannello o da un controllo, in base a una modifica dello stato. Per altre informazioni, vedere la sezione Events più avanti in questo argomento. Facoltativamente, un elemento **ObjectControlConfiguration** può contenere un elemento **Event**. In generale, l'uso di elementi **Event** personalizzati non è supportato, a meno che non venga specificamente sviluppato nell'ambito di miglioramenti successivi.

## <a name="data-sources"></a>Origini dati

Microsoft Identity Manager usa le origini dati come metodo per associare i dati ai componenti dell'interfaccia utente. Ciò facilita la separazione dei dati dal livello di presentazione. Nei dati di configurazione delle risorse RCDC sono presenti due tipi di origini dati: **ObjectDataSource** e **XmlDataSource**.

-   **ObjectDataSource** specifica una classe Microsoft .NET che fornisce dati al controllo risorsa. Quando crea RCDC, l'amministratore ha a disposizione un set fisso di tipi di ObjectDataSource.

-   **XMLDataSource** consente di strutturare in modo semplice dati basati su XML. Gli amministratori possono usare questo tipo di origine dati per fornire dati personalizzati. Se non si usa la struttura XML incorporata predefinita, è necessario specificare i dati XML direttamente nella RCDC. La struttura XML incorporata viene usata per la generazione delle pagine di riepilogo nel controllo risorsa.

Per generare l'interfaccia utente, nella RCDC è possibile associare queste origini dati agli attributi dei controlli dell'interfaccia utente specificati dalla RCDC.

### <a name="objectdatasource"></a>ObjectDataSource

La tabella seguente descrive tipi di origine dati comuni forniti da Microsoft Identity Manager e disponibili per tutti i tipi di risorsa, salvo dove diversamente indicato.

| TypeName                        | Descrizione     | Supporto dell'associazione bidirezionale | Sintassi di associazione supportata        |
|----------------|-----------|-------------------------|--------------|
| PrimaryResourceObjectDataSource | Rappresenta la risorsa FIM 2010 in corso di creazione, modifica o visualizzazione. Il percorso nella stringa di associazione è il nome dell'attributo. Si noti che il tipo di risorsa è specificato dall'attributo TargetObjectType della RCDC anziché dall'attributo RCDC.ConfigurationData. | sì                     | [NomeAttributo] Valore dell'attributo dell'oggetto specificato dal nome.    |
| PrimaryResourceDeltaDataSource  | Questa origine dati crea il delta XML che confronta lo stato originale e lo stato corrente della risorsa di FIM 2010. Il delta XML generato viene utilizzato dal controllo di riepilogo del controllo risorsa per eseguire il rendering dell'interfaccia utente per la richiesta in corso di invio da parte dell'utente.                                    | No                      | DeltaXml: </br> usato con il controllo di riepilogo per visualizzare il delta.                                                 |
| PrimaryResourceRightsDataSource | Questa origine dati fornisce i diritti inline per ogni attributo della risorsa di FIM 2010. In questo modo il controllo risorsa può determinare prima dell'invio le autorizzazioni di cui l'utente dispone per tale attributo e quindi eseguire il rendering dell'interfaccia utente per tale attributo in modo appropriato.                     | No                      | [NomeAttributo]                                                                                         |
| SchemaDataSource                | Questa origine dati può essere usata per accedere alle informazioni relative allo schema, ad esempio nome visualizzato, descrizione e se l'attributo è obbligatorio, nonché informazioni sul tipo di risorsa.                                                                                             | No                      | [NomeAttributo].**Required** Valore booleano che indica se, per essere valido, l'attributo deve avere un valore. <br/> [NomeAttributo].**DisplayNameString** Valore che indica il nome visualizzato dell'associazione <br/>[NomeAttributo].**DescriptionString** Valore che indica la descrizione dell'associazione <br/>[NomeAttributo].StringRegex Valore stringa che indica l'espressione regolare della stringa di associazione. <br/>[NomeAttributo].**DisplayName** <br/> [NomeAttributo].**Description** <br/> [NomeAttributo]. [NomeAttributo]. **IntergerValueMinimum** <br/>[NomeAttributo].**IntergerValueMaximum** <br/>[NomeAttributo].**LocalizedAllowedValues**|
| DomainDataSource                | Questa origine dati fornisce un'enumerazione dei domini in base alle risorse di configurazione dei domini stessi. Si noti che questa origine dati può essere usata solo in RCDC destinate a risorse di gruppi e di utenti.                                                                           | sì                     | Dominio           |

Di seguito è riportato un frammento di codice RCDC di esempio che associa tre origini dati al controllo UocTextBox per modificare l'attributo Description di un gruppo:

```XML
<my:ObjectDataSource my:TypeName="PrimaryResourceObjectDataSource" my:Name="object" my:Parameters=""/>
<my:ObjectDataSource my:TypeName="SchemaDataSource" my:Name="schema"/>
<my:ObjectDataSource my:TypeName="PrimaryResourceRightsDataSource" my:Name="rights"/>

     <my:Control my:Name="Description" my:TypeName="UocTextBox" my:Caption="{Binding Source=schema, Path=Description.DisplayName}" my:RightsLevel="{Binding Source=rights, Path=Description}">
          <my:Properties>
               <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=DisplayName.Required}"/>
               <my:Property my:Name="Rows" my:Value="3"/>
               <my:Property my:Name="Columns" my:Value="60"/>
               <my:Property my:Name="MaxLength" my:Value="450"/>
               <my:Property my:Name="Text" my:Value="{Binding Source=object, Path=Description, Mode=TwoWay}"/>
          </my:Properties>
     </my:Control>
```

### <a name="xmldatasource"></a>XMLDataSource

Tramite **XMLDataSource** è possibile specificare dati personalizzati che la RCDC può usare per una risorsa specifica. In questo caso, i dati XML devono essere specificati nella RCDC. In alternativa, è possibile usare questa origine dati per fare riferimento a una struttura dei dati XML incorporata per eseguire il rendering dell'interfaccia utente per le pagine di riepilogo. Il tipo di **XMLDataSource** da usare al momento della definizione della RCDC viene controllato dall'utente.


| TypeName                 | Descrizione   | | |
|--------------------------|------------|
| **XMLDataSource**            | L'origine dati rappresenta i dati XML. Può essere nel formato XSL o XSL incorporato: <br/>**Formato XSL:** <br/> Microsoft.IdentityManagement.WebUI.Controls.dll<my:XmlDataSource my:Name=" <br/>summaryTransformXsl"my:Parameters=”Microsoft.IdentityManagement.WebUI.Controls.Resources.DefaultSummary.xsl”> </my:XmlDataSource><br/> **Formato XSL incorporato:** <br/> <my:XmlDataSource my:Name="RequestStatusTransformXsl"> <br/> <xsl:stylesheet version="1.0" xmlns:xsl=http://www.w3.org/1999/XSL/Transform <br/> xmlns:msxsl="urn:schemas-microsoft-com:xslt"><br/></xsl:stylesheet></my:XmlDataSource>                       |No | ```Xpath[;namespaces]``` <br/> Dove :Xpath è un xpath XML valido per selezionare la nota richiesta, in genere "/" (radice) <br/>namespaces è un elenco facoltativo di stringhe prefix=URI, delimitate da punti e virgola, se necessario per il funzionamento di xpath con XML con namespaces. |
| **ReferenceDeltaDataSource** | L'origine dati rappresenta i delta di attributi di riferimento multivalore. Usata solo nella RCDC per Group e Set. <br/> Anche se l'origine dati non è limitata a Group e Set,l'invio di tali delta richiede modifiche al codice nell'host della RCDC. Group e Set sono attualmente gli unici host che riconosce l'origine dati.  | Sì                      | [NomeAttributo].Add dove [NomeAttributo] rappresenta un attributo di riferimento e i dati restituiti sono le aggiunte nel delta. <br/> Esempio: [AttributoRiferimento].Add <br/>Esempio: ```<my:Property my:Name="Value" my:Value="{Binding Source=delta, Path=ExplicitMember.Add, Mode=TwoWay}" />``` <br/>[NomeAttributo].Remove dove [NomeAttributo] rappresenta un attributo di riferimento e i dati restituiti sono le rimozioni nel delta. <br/> DeltaXml |
|**RequestDetailsDataSource**| L'origine dati rappresenta l'attributo RequestParameter di oggetti Request. Il parametro imposta il numero massimo di valori di attributo da visualizzare per ogni attributo multivalore. Usata solo in RCDC per Request. ```<my:ObjectDataSource my:TypeName="RequestDetailsDataSource" my:Name="requestDetails" my:Parameters="1000" />```| No | DeltaXml |
|**RequestStatusDataSource**| L'origine dati rappresenta l'attributo**RequestStatusDetails** di oggetti Request. <br/>È usata solo in RCDC per Request.  | No | DeltaXml |

-   Per definire un'origine dati XML personalizzata:

 ```XML
   <my:XmlDataSource my:Name="MyCustomData" >
   %Insert custom, properly formatted XML data here%
   </my:XmlDataSource>
   ```

-   Per usare XSL di un controllo di riepilogo incorporato, definire l'origine dati come segue:

```XML
<my:XmlDataSource my:Name="summaryTransformXsl" my:Parameters="Microsoft.IdentityManagement.WebUI.Controls.Resources.DefaultSummary.xsl" />
```

 Se si sta creando una RCDC per un tipo di risorsa personalizzata, è possibile usare questo metodo per eseguire automaticamente il rendering di una pagina di riepilogo per la risorsa personalizzata in questione.

Di seguito è riportato un esempio di come creare una scheda di riepilogo nella RCDC, usando PrimaryResourceDeltaDataSource con XMLDataSource tramite l'XSL incorporato:

```XML
<my:ObjectDataSource my:TypeName="PrimaryResourceDeltaDataSource" my:Name="delta" />
<my:XmlDataSource my:Name="summaryTransformXsl" my:Parameters="Microsoft.IdentityManagement.WebUI.Controls.Resources.DefaultSummary.xsl" />

<my:Grouping my:Name="summaryGroup" my:Caption="Summary” my:IsSummary="true">
     <my:Control my:Name="summaryControl" my:TypeName="UocHtmlSummary" my:ExpandArea="true">
          <my:Properties>
               <my:Property my:Name="ModificationsXml" my:Value="{Binding Source=delta, Path=DeltaXml}" />
              <my:Property my:Name="TransformXsl" my:Value="{Binding Source=summaryTransformXsl, Path=/}" />
          </my:Properties>
     </my:Control>
</my:Grouping>
```

In alternativa, l'utente può sostituire l'elemento XmlDataSource specificato in precedenza con il formato seguente per definire un layout personalizzato di una pagina di riepilogo. Come riferimento, l'XSL di riepilogo di FIM 2010 è incluso nell'Appendice B: XSL di riepilogo predefinito, più avanti in questo documento.

```XML
<my:XmlDataSource my:Name="summaryTransformXsl">
     Insert valid XSL code here
</my:XmlDataSource>
```
### <a name="schema-for-data-sources"></a>Schema per origini dati
Di seguito è riportato lo schema XSD per i due tipi di origini dati:

```XML
<xsd:element name="ObjectDataSource">
     <xsd:complexType>
          <xsd:sequence/>
          <xsd:attribute ref="my:TypeName"/>
          <xsd:attribute ref="my:Name"/>
          <xsd:attribute ref="my:Parameters"/>
     </xsd:complexType>
</xsd:element>
<xsd:element name="XmlDataSource">
     <xsd:complexType  mixed="true">
          <xsd:sequence>
              <xsd:any minOccurs="0" maxOccurs="unbounded" processContents="lax"/>
          </xsd:sequence>
          <xsd:attribute ref="my:Name"/>
          <xsd:attribute ref="my:Parameters"/>
    </xsd:complexType>
</xsd:element>
```

## <a name="events"></a>Eventi
Event definisce lo stato in corso di modifica di un controllo. L'estendibilità di questa funzionalità è limitata, perché non è possibile scrivere una funzione personalizzata (Handler) per definire il comportamento dopo l'attivazione di un evento. Lo stesso elemento Event può essere usato nell'elemento Panel. Per altre informazioni, vedere la sezione Panel più avanti in questo documento. Di seguito è riportato lo schema XSD per l'elemento Event:

```XML
<xsd:element name="Events">
     <xsd:complexType>
          <xsd:sequence>
               <xsd:element ref="my:Event" minOccurs="1" maxOccurs="16"/>
          </xsd:sequence>
     </xsd:complexType>
</xsd:element>
<xsd:element name="Event">
     xsd:complexType>
          <xsd:simpleContent>
               <xsd:extension base="xsd:string">
                    xsd:attribute ref="my:Name"/>
                    <xsd:attribute ref="my:Handler"/>
               </xsd:extension>
          </xsd:simpleContent>
     </xsd:complexType>
     </xsd:element>
```


Event è un elemento vuoto e dispone di due attributi.

**Attributi:**

1.  **Name**: nome univoco di un evento. L'unico evento supportato in **ObjectControlConfiguration** è l'evento Load. Questo evento viene attivato quando la pagina viene caricata per la prima volta.

2.  **Handler**: nome univoco di un gestore. Quando viene attivato l'evento, in genere viene chiamato un programma per gestire la modifica dello stato del controllo. Non sono supportati i seguenti casi: rimozione di un gestore esistente da un controllo esistente, creazione di un nuovo gestore e associazione a un controllo esistente o nuovo.

Esempio:

Di seguito è riportato un esempio dell'elemento Events.
```XML
<my:Events>
    <my:Event my:Name="Load" my:Handler="OnLoad"/>
</my:Events>
```

**Panel**


L'elemento Panel è l'elemento principale di un layout RCDC. Di seguito è riportato lo schema XSD per l'elemento Panel:

```XML
<xsd:element name="Panel">
     <xsd:complexType>
          <xsd:sequence>
               <xsd:element ref="my:Grouping" minOccurs="1" maxOccurs="16"/>
          </xsd:sequence>
          <xsd:attribute ref="my:Name"/>
          <xsd:attribute ref="my:DisplayAsWizard"/>
          <xsd:attribute ref="my:Caption"/>
          <xsd:attribute ref="my:AutoValidate"/>
     </xsd:complexType>
</xsd:element>
```

Questo elemento contiene un elemento ricorrente, Grouping. Per altre informazioni, vedere la sezione Grouping in questo documento.

L'elemento Panel contiene quattro attributi:

1.  **Name**: nome dell'elemento Panel. Questo è un attributo obbligatorio di tipo stringa.

2.  **DisplayAsWizard**: questo attributo è attualmente deprecato. L'attributo VerbContext corrispondente nella RCDC determina se il layout della risorsa è in modalità guidata o in modalità scheda. Se è impostato su 0 (modalità di creazione), è anche in modalità guidata. In caso contrario, è in modalità scheda. Per altre informazioni, vedere Introduction to Configuring and Customizing the FIM Portal (Introduzione alla configurazione e alla personalizzazione del portale FIM) nella documentazione.

3.  **Caption**: questo attributo è attualmente deprecato. L'utente può specificare didascalie per una pagina includendo un elemento Group contenente solo informazioni di intestazione. Per altre informazioni, vedere la sezione Grouping in questo documento.

4.  **AutoValidate**: attributo booleano facoltativo. Quando è impostato su true, viene attivata la convalida per ogni controllo nella scheda corrente. Per impostazione predefinita, se l'attributo non è presente, è impostato su true. Può essere usato in combinazione con la proprietà RegularExpression. Per altre informazioni, vedere la sezione "RegularExpression" più avanti in questo documento.

## <a name="grouping"></a>Raggruppamento

L'elemento Grouping definisce il layout complessivo di un elemento Panel. Questo elemento funge da contenitore che raggruppa singoli controlli in schede e sezioni diverse. Di seguito è riportato lo schema XSD per l'elemento Grouping:

```XML
<xsd:element name="Grouping">
     <xsd:complexType>
          <xsd:sequence>
               <xsd:element ref="my:Help" minOccurs="0"  maxOccurs="1"/>
               <xsd:element ref="my:Control" minOccurs="1" maxOccurs="256"/>
               <xsd:element ref="my:Events" minOccurs="0" maxOccurs="1"/>
          </xsd:sequence>
          <xsd:attribute ref="my:Name"/>
          <xsd:attribute ref="my:Caption"/>
          <xsd:attribute ref="my:Description"/>
          <xsd:attribute ref="my:Enabled"/>
          <xsd:attribute ref="my:Visible"/>
          <xsd:attribute ref="my:IsHeader"/>
          <xsd:attribute ref="my:IsSummary"/>
     </xsd:complexType>
</xsd:element>
```


Esistono tre tipi di **Grouping**:

1.  **Grouping di intestazione**: facoltativo. In un elemento **Panel** può essere presente solo un elemento Grouping di intestazione. Un elemento Grouping di intestazione viene visualizzato nella parte superiore di un pannello come didascalia.
    In questo Grouping è consentito un solo UocCaptionControl. Per un esempio di Grouping di intestazione, vedere la sezione Esempio.

2.  **Grouping di contenuto**: è obbligatorio almeno un elemento Grouping di contenuto. In un elemento Panel possono essere presenti più Grouping di contenuto. Un Grouping di contenuto costituisce il contenuto principale di una pagina di RCDC. Ogni Grouping di contenuto compare come scheda nello stesso elemento Panel e può contenere da 1 a 256 controlli. Per un esempio di **Grouping di contenuto**, vedere la sezione Esempio più avanti.

3.  **Grouping di riepilogo**: un elemento Grouping di riepilogo è facoltativo. In un elemento Panel può essere presente solo un elemento Grouping di riepilogo. Un Grouping di riepilogo viene visualizzato come ultima scheda di un Panel. In un elemento Grouping di riepilogo può essere usato un solo **UocHtmlSummary** per visualizzare le modifiche eseguite dall'utente prima dell'invio di una richiesta. Per un esempio di Grouping di riepilogo, vedere la sezione Esempio più avanti.

Ogni tipo di Grouping contiene gli elementi seguenti:

1.  **Help**: questo elemento fornisce testo della Guida in una scheda. È possibile usare questo elemento anche per aggiungere un collegamento a un file della Guida per la scheda.

2.  **Control**: per informazioni su questo elemento, vedere la sezione Control in questo documento. Ogni elemento Grouping deve avere da 1 a 256 controlli, a seconda del tipo di raggruppamento.

3.  **Events**: per informazioni su questo elemento, vedere la sezione Events in questo documento. Ogni raggruppamento può avere, facoltativamente, un solo evento. Gli elementi Events supportati in un elemento Grouping sono i seguenti:

    - **BeforeLeave**: questo evento viene attivato quando l'utente sta per uscire da una scheda di un raggruppamento di contenuto.
    - **AfterEnter**: questo evento viene attivato quando l'utente sta per entrare in una scheda di un raggruppamento di contenuto.

Attributi:

1.  **Name**: nome, obbligatorio, dell'elemento Grouping. L'attributo **Name** deve essere univoco all'interno dell'elemento **Panel**.

2.  **Caption**: l'attributo **Caption** viene visualizzato come intestazione in un elemento Grouping di intestazione. Viene visualizzato come didascalia della scheda di un elemento Grouping di contenuto o di riepilogo.

3.  **Description**: attributo stringa facoltativo, **Description** è funzionale solo se usato in un elemento Grouping di contenuto. Usare questo elemento per fornire all'utente finale alcuni dettagli sulle informazioni all'interno della stessa scheda.

  >[!NOTE]
  Se questo attributo viene usato in un elemento Grouping di riepilogo, il codice XML viene considerato non valido. Se questo attributo viene usato in un elemento Grouping di intestazione, il codice XML viene considerato valido ma viene ignorato.

4.  **Enabled**: attributo booleano facoltativo. Se mancante, è impostato su true. Se Enabled è impostato su false, l'utente finale visualizza una scheda disabilitata. Questo attributo è funzionale solo in un elemento Grouping di contenuto.

  >[!NOTE]
  Se questo attributo viene usato in un elemento Grouping di riepilogo, il codice XML viene considerato non valido. Se questo attributo viene usato in un elemento Grouping di intestazione, il codice XML viene considerato valido ma viene ignorato.

5.  **Visible**: è possibile nascondere una scheda di una pagina di RCDC o la relativa intestazione impostando questo attributo su false. Per impostazione predefinita, questo attributo booleano facoltativo è impostato su true. Questo attributo è funzionale solo in un elemento Grouping di contenuto.

  >[!NOTE]
  Se in un elemento Panel è presente un solo elemento Grouping di contenuto, questa funzionalità non funziona. Se in un elemento Panel sono presenti più elementi Grouping di contenuto, la funzionalità si comporta come descritto sopra.

6.  **IsHeader**: questo attributo booleano facoltativo definisce se l'elemento Grouping è un Grouping di intestazione. Se questo attributo non è specificato, è impostato su false.

7.  **IsSummary**: attributo booleano facoltativo che definisce se l'elemento Grouping è grouping di riepilogo. Se questo attributo non è specificato, è impostato su false.

![XML di configurazione RCDC](media/rcd-configuration-xml-reference/image005.jpg)

Il codice XML di esempio seguente genera l'elemento Grouping di intestazione precedente. L'elemento Grouping di intestazione è l'area contenente il testo Sample Header Grouping.

```XML
<!--Sample for a Header Grouping-->
<my:Grouping my:Name="HeaderGroupingSample" my:IsHeader="true">
     <my:Control my:Name="SampleHeaderCaption" my:TypeName="UocCaptionControl" my:ExpandArea="true" my:Caption="Sample Header Grouping">
          <my:Properties>
               <my:Property my:Name="MaxHeight" my:Value="32"/>
               <my:Property my:Name="MaxWidth" my:Value="32"/>
          </my:Properties>
      </my:Control>
</my:Grouping>
<!--End of Header Grouping Sample-->
```

![XML di configurazione RCDC](media\rcd-configuration-xml-reference/image007.jpg)

Il codice XML di esempio seguente genera l'elemento Grouping di contenuto precedente. L'elemento Grouping di contenuto è la scheda all'estrema sinistra contenente il testo **Sample Content Grouping**.

```XML
<!--Sample for a Content Grouping-->
<my:Grouping my:Name="ContentGroupingSample" my:Caption="Sample Content Grouping" my:Description="Some description for content grouping">
     <my:Control my:Name="DisplayName" my:TypeName="UocTextBox" my:Caption="Display name" my:Description="This is the display name of the set.">
          <my:Properties>
               <my:Property my:Name="Required" my:Value="True"/>
               <my:Property my:Name="MaxLength" my:Value="128"/>
               <my:Property my:Name="Text" my:Value="{Binding Source=object, Path=DisplayName, Mode=TwoWay}"/>
          </my:Properties>
     </my:Control>
</my:Grouping>
<!--End of Content Grouping Sample-->
```

![XML di configurazione RCDC](media/rcd-configuration-xml-reference/image010.jpg)

Il codice XML di esempio seguente genera l'elemento Grouping di riepilogo precedente. L'elemento Grouping di riepilogo è la scheda all'estrema destra contenente il testo **Summary**.

```XML
<!--Sample for a Summary Grouping-->
<my:Grouping my:Name="Summary" my:Caption="Sample Summary Grouping" my:IsSummary="true">
     <my:Control my:Name="SummaryControl" my:TypeName="UocHtmlSummary" my:ExpandArea="true">
          <my:Properties>
               <my:Property my:Name="ModificationsXml" my:Value="{Binding Source=delta, Path=DeltaXml}"/>
               <my:Property my:Name="TransformXsl" my:Value="{Binding Source=summaryTransformXsl, Path=/}"/>
          </my:Properties>
     </my:Control>
</my:Grouping>
<!--End of Summary Grouping Sample-->
```
### <a name="help"></a>Help

L'elemento Help, facoltativo, può essere incluso in un elemento Grouping o Controllo. Se usato in un elemento Grouping, deve essere il primo elemento usato. Fornisce testo della Guida agli utenti finali per consentire a questi di offrire informazioni accurate. Di seguito è riportato lo schema XSD per l'elemento Help:

```XML
<xsd:element name="Help">
     <xsd:complexType>
          <xsd:sequence/>
          <xsd:attribute ref="my:HelpText"/>
          <xsd:attribute ref="my:Link"/>
     </xsd:complexType>
</xsd:element>
```
Di seguito è riportato un esempio dell'elemento Help:
```XML
<my:Help my:HelpText="Some Help Text for Group Basic Info" my:Link="03e258a0-609b-44f4-8417-4defdb6cb5e9.htm#bkmk_grouping_GroupingBasicInfo" />
```

### <a name="control"></a>Control

Un elemento Grouping contiene uno o più elementi Control. I Control sono gli elementi principali in una RCDC. È possibile personalizzare l'elemento Grouping definendo i vari elementi Control in esso contenuti. Di seguito è riportato lo schema XSD per l'elemento Control:

```XML
<xsd:element name="Control">
     <xsd:complexType>
          <xsd:sequence>
               <xsd:element ref="my:Help" minOccurs="0"  maxOccurs="1"/>
               <xsd:element ref="my:CustomProperties" minOccurs="0"  maxOccurs="1"/>
               <xsd:element ref="my:Options" minOccurs="0"  maxOccurs="1"/>
               <xsd:element ref="my:Buttons" minOccurs="0"  maxOccurs="1"/>
               <xsd:element ref="my:Properties" minOccurs="0"  maxOccurs="1"/>
               <xsd:element ref="my:Events" minOccurs="0" maxOccurs="1"/>
          </xsd:sequence>
          <xsd:attribute ref="my:Name"/>
          <xsd:attribute ref="my:TypeName"/>
          <xsd:attribute ref="my:Caption"/>
          <xsd:attribute ref="my:Enabled"/>
          <xsd:attribute ref="my:Visible"/>
          <xsd:attribute ref="my:Description"/>
          <xsd:attribute ref="my:ExpandArea"/>
          <xsd:attribute ref="my:Hint"/>
          <xsd:attribute ref="my:AutoPostback"/>
          <xsd:attribute ref="my:RightsLevel"/>
     </xsd:complexType>
</xsd:element>
```

Un elemento Control contiene gli elementi seguenti:

1.  **Help**: questo elemento viene ignorato. Funziona solo nell'elemento Grouping.

2.  **CustomProperties**: questo elemento non è supportato.

3.  **Options**: questo elemento viene usato solo in combinazione con i controlli **UocDropDownList** e **UocRadioButtonList**. Non funziona con altri elementi Control. Per la struttura di questo elemento, vedere la sezione Options in questo documento. Vedere i singoli elementi Control per vedere com'è usato nel contesto di ognuno.

4.  **Buttons**: questo elemento viene usato solo in combinazione con il controllo **UocListView**. Non funziona con gli altri elementi Control. Per altre informazioni, vedere la sezione UocListView in questo documento.

5.  Properties: questo elemento viene usato in tutti gli elementi Control per specificare comportamenti aggiuntivi per ognuno. Per informazioni su questo elemento, vedere la sezione Properties in questo documento.

6.  **Events**: per la struttura di questo elemento, vedere la sezione Events più indietro in questo documento. Vedere la definizione dei singoli elementi Control per determinare quale evento viene usato all'interno di ognuno.

Un elemento Control contiene gli attributi seguenti:

1.  **Name**: nome del controllo. Il nome di un elemento Control deve essere univoco all'interno di ogni elemento Panel. Questo è un attributo obbligatorio di tipo stringa.

2.  **TypeName**: questo attributo specifica il tipo dell'elemento Control. Questo è un attributo obbligatorio di tipo stringa. Vedere la sezione Elementi Control singoli in questo documento per il nome di ogni controllo.

3.  **Caption**: è possibile usare questo attributo per includere una didascalia per il controllo.
    In genere, la didascalia è il nome visualizzato dei dati visualizzati o ricevuto come input dal controllo. È possibile specificare un valore per la didascalia esplicitamente o associare quest'ultima con le informazioni del nome visualizzato dell'attributo dello schema. La didascalia viene visualizzata all'estremità sinistra di un controllo di dimensioni normali. Se un controllo si estende su tutto lo schermo, la didascalia viene visualizzata sopra il controllo. Questo è un attributo facoltativo di tipo stringa. Per informazioni su come associare un'origine dati a un attributo o al valore di una proprietà, vedere la sezione Properties.

   L'esempio seguente illustra come usare in modo esplicito una didascalia:

   ```XML
      <my:Control my:Name="ExplicitAlias" my:TypeName="UocTextBox" my:Caption="Explicit Alias">…<my:Control/>
   ```
     L'esempio seguente illustra come usare una didascalia con un'origine dati. Se si è usato il modello per un'origine dati illustrata in precedenza in questo documento, l'origine dati corrisponde allo schema. È consigliabile associare DisplayName dell'attributo con un attributo Caption.

    ```XML
    <my:Control my:Name="DynamicAlias" my:TypeName="UocTextBox" my:Caption="{Binding Source=schema, Path=Alias.DisplayName, Mode=OneWay}">…<my:Control/>
    ```
4.  Enabled: attributo di tipo booleano facoltativo. L'utente può disattivare un Control impostando il valore di questo attributo su false. Il valore predefinito è impostato su true.

5.  Visible: attributo di tipo booleano facoltativo. È possibile usare questo attributo per nascondere l'intero controllo. Il valore predefinito è impostato su true.

6.  Description: usare questo attributo facoltativo di tipo stringa per includere una descrizione che consenta all'utente finale di comprendere cosa deve inserire nel controllo o la funzione del controllo stesso. È possibile specificare un valore per la descrizione esplicitamente o associare quest'ultima con le informazioni della descrizione dell'attributo dello schema. <br/>La descrizione viene visualizzata all'estremità sinistra di un controllo di dimensioni normali, sotto la didascalia. Se un controllo si estende sull'intero schermo, la descrizione viene visualizzata nella parte superiore del controllo sotto la didascalia. Per informazioni su come associare un'origine dati a un attributo o al valore di una proprietà, vedere la sezione Properties in questo documento.

7.  L'esempio seguente illustra come usare in modo esplicito l'attributo Description:
  ```XML
  <my:Control my:Name="ExplicitAlias" my:TypeName="UocTextBox" my:Caption="Explicit Alias" my:Description="This is explicit description.">…<my:Control/>
  ```
  Questo esempio illustra come usare l'attributo Description con un'origine dati. Se si è usato il modello per un'origine dati illustrata in precedenza in questo documento, l'origine dati è lo **schema**. È consigliabile associare **Description** dell'attributo con un attributo Description.
  ```XML
  <my:Control my:Name="DynamicAlias" my:TypeName="UocTextBox" my:Caption="{Binding Source=schema, Path=Alias.DisplayName, Mode=OneWay}" my:Description="{Binding Source=schema, Path=Alias.Description, Mode=OneWay}">…<my:Control/>
  ````
8. ExpandArea: questo attributo indica se il controllo si estende sull'intero schermo. Questo è un attributo di tipo booleano facoltativo. Il valore predefinito è impostato su false.

    >[!NOTE]
    Se questo attributo è impostato su true, gli attributi Caption e Description sono disabilitati. Per specificare una didascalia per un controllo espanso, è necessario usare il controllo UocLabel.
9. **Hint**: attributo facoltativo di tipo stringa. Il testo dell'attributo Hint consente all'utente finale di definire l'input corretto per il controllo. Hint viene visualizzato sotto il controllo.

10.  **AutoPostback**: attributo di tipo booleano facoltativo. Il valore predefinito è false. Se è impostato su false e si aggiorna la pagina, il controllo potrebbe non aggiornarsi. Per informazioni su AutoPostback, cercare la proprietà del controllo dell'interfaccia utente di Microsoft ASP.NET con lo stesso nome.

11. **RightsLevel**: attributo facoltativo di tipo stringa. È possibile associare questo attributo a un'origine dati solo con diritti inline. Il controllo viene attivato o disabilitato dinamicamente in base ai diritti dell'utente. Per informazioni su come associare origini dati a un attributo o al valore di una proprietà, vedere la sezione Properties in questo documento.

    Questo esempio illustra come usare l'attributo **RightsLevel** con un'origine dati. Se si è usato il modello per un'origine dati illustrata in precedenza in questo documento, l'origine dati è **rights**. Per Path usare il nome dell'attributo.

### <a name="properties"></a>Proprietà

È possibile usare un elemento Properties per personalizzare ulteriormente il comportamento di ogni controllo. Property è un elemento vuoto. Di seguito è riportato lo schema XSD per l'elemento Property:
```XML
<xsd:element name="Properties">
     <xsd:complexType>
          <xsd:sequence>
               <xsd:element ref="my:Property" minOccurs="1" maxOccurs="32"/>
          </xsd:sequence>
     </xsd:complexType>
</xsd:element>
<xsd:element name="Property">
     <xsd:complexType>
          <xsd:simpleContent>
               <xsd:extension base="xsd:string">
                    <xsd:attribute ref="my:Name"/>
                    <xsd:attribute ref="my:Value"/>
               </xsd:extension>
          </xsd:simpleContent>
     </xsd:complexType>
</xsd:element>
```



Ogni elemento Property dispone di due attributi obbligatori:

1.  **Name**: attributo di tipo stringa, nome univoco dell'elemento Property.
    Controlli diversi hanno proprietà diverse. Esistono alcune proprietà comuni che possono essere usate da tutti i controlli. Per altre informazioni sui nomi disponibili per un controllo specifico, vedere le sezioni Proprietà comuni e Controlli singoli.

2.  **Value**: valore dell'elemento Property. Il tipo di dati del valore dipende dalla proprietà a cui viene assegnato. Per il formato consentito per il valore per proprietà specifiche, vedere la sezione seguente.

Alcuni elementi Property possono essere associati con le informazioni di un'origine dati. A tale scopo, è necessario usare il formato di stringa seguente. Vedere le singole proprietà nella sezione Controlli singoli per scoprire come associare le proprietà a un'origine dati.

```
<my:Property my:Name="Required" my:Value="[Formatted String]"/>

Formatted String :=  “{Binding “ + [SourceExpression] + “,” + [PathExpression] + “,” + [ModeExpression]? + “}”

SourceExpression:= “Source=” + [ObjectDataSourceName]

PathExpression:= “Path=” + [AttributeName]|[AttributePropertyName]

ModeExpression:= “Mode=” + [ModeChoice]

ModeChoice:= “OneWay”|”TwoWay”

ObjectDataSourceName:= The value of any string assign to node /ObjectControlConfiguration/ObjectDataSource/Name.

AttributeName:= valid schema attribute name from the data source.

AttributePropertyName:= valid property name of a schema attribute from the data source.

````
**ESEMPIO:**

```XML
<my:Property my:Name="Text" my:Value="{Binding Source=object, Path=DisplayName, Mode=TwoWay}"/>
<my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=DisplayName.Required}"/>

```



<a name="common-properties"></a>Proprietà comuni
-----------------

Tutti i controlli RCDC specificati in questo documento possono avere le proprietà comuni seguenti. È possibile usare queste proprietà insieme ad altre proprietà specifiche dei diversi controlli.

1.  Required: questa proprietà indica se il campo è obbligatorio o facoltativo. Un campo obbligatorio deve contenere un valore. In caso di input di una stringa, un valore vuoto non è supportato. Un campo facoltativo può essere lasciato vuoto. Se questo campo è obbligatorio ma non vi è stato inserito alcun valore, sul controllo di input viene visualizzato un messaggio di errore. È possibile specificare in modo esplicito se un campo è obbligatorio o facoltativo. È anche possibile associare il campo alle informazioni dello schema di un'associazione specifica tra un attributo e un tipo di risorsa. Per impostazione predefinita, se questa proprietà manca, il controllo è un controllo di input facoltativo.

   L'esempio seguente usa un valore esplicito per questa proprietà:

    ```XML
    <my:Property my:Name="Required" my:Value="True"/>
    ```
   Questo esempio usa un'origine dati dinamica per questa proprietà. Se si è usato il modello per un'origine dati illustrata in nella sezione precedente di questo documento, l'origine dati corrisponde allo schema. Per Path usare \<nome attributo\>.Required.

    ```XML
    <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=DisplayName.Required}"/>
    ```
2. **ReadOnly**: impostando questa proprietà su true, l'esperienza che si ottiene dal controllo è di sola lettura. Questo è un attributo di tipo booleano facoltativo.
    Il valore predefinito è impostato su false. In alcuni casi, tuttavia, il comportamento di questa proprietà viene sovrascritto dal tipo di diritti di cui l'utente dispone per i dati associati al controllo. Se ad esempio un utente non dispone dei diritti per aggiornare un campo e questo è associato a diritti inline, l'utente visualizza i dati in modalità di sola lettura anche questa proprietà è impostata su false.

3.  **RegularExpression**: questa proprietà specifica le restrizioni imposte sul valore del controllo. I formati del valore di questa proprietà sono quelli supportati dallo standard StringRegex di .NET. Per altre informazioni, vedere [.NET Framework Regular Expressions](http://go.microsoft.com/fwlink/?LinkId=165361) (Espressioni regolari in .Net Framework). Se il controllo viene usato per eseguire l'input di un valore, quando l'utente tenta di uscire dalla pagina corrente il valore viene confrontato con la restrizione specificata per questa proprietà.
    Il messaggio di errore viene visualizzato sopra il controllo con un input non valido. L'utente può specificare in modo esplicito un'espressione regolare stringa. L'utente può anche associarla alle informazioni dello schema di un attributo specificato. Per impostazione predefinita, se questa proprietà manca, significa che il controllo non verifica la presenza di restrizioni per le stringhe di input.
    L'esempio seguente usa un valore esplicito per questa proprietà:

    ```XML
    <my:Property my:Name="RegularExpression" my:Value="[A-Z]*"/>
    ```
    Questo esempio usa un'origine dati dinamica per questa proprietà. Se si è usato il modello per un'origine dati illustrata in precedenza in questo documento, l'origine dati corrisponde allo schema. Per Path usare <attribute name>.StringRegex.
    ```XML
    <my:Property my:Name="RegularExpression" my:Value="{Binding Source=schema, Path=Alias.StringRegex, Mode=OneWay}"/>
    ```
4.  Visible: attributo di tipo booleano facoltativo. È possibile usare questo attributo per nascondere l'intero controllo. Il valore predefinito è impostato su true.

### <a name="options"></a>Options

L'elemento Options include uno o più sottonodi Option. L'elemento Options viene usato solo con i controlli UocRadioButtonList e UocDropDownList. Per informazioni dettagliate su come usarli, vedere la sezione relativa a ogni controllo specifico. Di seguito è riportato lo schema XSD per l'elemento Options:

```XML
<xsd:element name="Options">
     <xsd:complexType>
          <xsd:sequence>
               <xsd:element ref="my:Option" minOccurs="0" maxOccurs="unbounded"/>
          </xsd:sequence>
     </xsd:complexType>
</xsd:element>
<xsd:element name="Option">
     <xsd:complexType>
          <xsd:simpleContent>
               <xsd:extension base="xsd:string">
                    <xsd:attribute ref="my:Value"/>
                    <xsd:attribute ref="my:Caption"/>
                    <xsd:attribute ref="my:Hint"/>
               </xsd:extension>
          </xsd:simpleContent>
     </xsd:complexType>
</xsd:element>
```


Attributi:

1.  Value: attributo obbligatorio di tipo stringa. L'attributo value deve essere univoco all'interno dello stesso controllo. Sono consentiti solo caratteri da A a Z, senza distinzione tra maiuscole e minuscole.

2.  Caption: attributo obbligatorio corrispondente al nome visualizzato di ogni elemento Option.

3.  Hint: attributo facoltativo. Usare questo attributo per offrire altre informazioni e suggerimenti all'utente finale.

### <a name="environment-variables"></a>Variabili di ambiente

Le variabili di ambiente nella tabella seguente possono essere usate in qualsiasi configurazione RCDC.

| variabile | description |
|--------|--------|
| `<LoginID>`       | Visualizza l'ID dell'utente attualmente connesso.           |
| `<LoginDomain>`   | Visualizza il dominio dell'utente attualmente connesso.       |
| `<Today>  `       | Visualizza la data e l'ora correnti                                |
| `<FromToday_nnn>` | Visualizza la data, più nnn e l'ora correnti. nnn è un numero intero.  |
| `<ObjectID> `     | ID della risorsa primaria RCDC.                                     |
| `<Attribute_xxx>` | Restituisce un attributo specifico, xxx, della risorsa primaria RCDC. |

### <a name="debugging-xml-configuration-files"></a>Debug di file di configurazione XML


Durante lo sviluppo o la modifica di file di configurazione XML per una RCDC, è possibile favorire la riduzione degli errori effettuando la convalida del codice XML a fronte dei file XSD tramite un editor, ad esempio Microsoft Visual Studio®. Per altre informazioni, vedere [An Introduction to the XML Tools in Visual Studio 2005](http://go.microsoft.com/fwlink/?LinkID=74512) (Introduzione agli strumenti XML in Visual Studio 2005).

### <a name="customizing-a-help-file"></a>Personalizzazione di un file della Guida

Se si creano nuove risorse e nuovi attributi, è necessario aggiornare i file della Guida presenti nel portale FIM con contenuto relativo alle risorse personalizzate. I file della Guida nel portale FIM sono in formato con estensione htm e possono essere modificati manualmente.

>[!IMPORTANT]
Per altre informazioni sulla creazione di attributi personalizzati, vedere Introduction to Custom Resource and Attribute Management (Introduzione alla gestione di risorse e attributi personalizzati) nella documentazione di FIM 2010.

>[!IMPORTANT]
Questa sezione non offre le nozioni di base per la formattazione e la modifica di codice HTML. Per modificare i file della Guida è necessario avere già una certa familiarità con le funzionalità di modifica del codice HTML


**Percorso dei file della Guida**: tutti i file della Guida per il portale di Microsoft Identity Management 2016 SP1 si trovano nella cartella seguente nel server del servizio MIM:

  `<ProgramFiles>\Common Files\Microsoft Shared\Web Server Extensions\12\Template\Layouts\MSILM2\Help\1033\html`

**Come individuare il file della Guida appropriato**: tutti i file della Guida per il portale FIM sono denominati con un identificatore univoco globale (GUID, Globally Unique Identifier). Per individuare il file corretto per la risorsa personalizzata:

1.  Nel portale FIM aprire il file della Guida nella pagina del portale che si vuole personalizzare.

2.  Fare clic con il pulsante destro del mouse sul file della Guida e quindi scegliere **Proprietà**.

3.  Evidenziare e copiare il file `<GUID\>.htm` nel campo 'Indirizzo URL'.

4.  Passare alla cartella in cui sono archiviati i file della Guida e cercare il file.

**Aggiunta di contenuto per un nuovo attributo**: per aggiungere contenuto descrittivo per un nuovo attributo all'interno di un elemento Grouping (scheda) esistente:

1.  Identificare e individuare il file della Guida appropriato.

2.  Aprire il file tramite un editor di HTML.

3.  Individuare il punto in cui si vuole aggiungere il contenuto. In genere, si tratta di un paragrafo aggiuntivo, ad esempio:

`<p xmlns="">A new paragraph with customized information.</p>`

Potrebbe anche essere un elemento inserito in un elenco esistente, ad esempio:
```
<li class="unordered"><b>First Name</b> – The first name of the User.<br>

<li class="unordered"><b>Last Name</b> - The last name of the User.<br>

<li class="unordered"><b>Added a new line</b><br>
```

**Aggiunta di contenuto per un nuovo elemento Grouping**: la maggior parte delle pagine del portale FIM contiene più elementi Grouping (o schede), per ognuno dei quali esiste un segnalibro all'interno dei file della Guida associati. Nel codice HTML i segnalibri sono specificati nelle sezioni. Ad esempio, questo è il codice HTML per la scheda Work Info nel file della Guida per la pagina Crea utente nel portale FIM:

```
<a name="bkmk_grouping_WorkInfo" xmlns=""></a><h3 class="subHeading" xmlns="">Work Info</h3><p class="subHeading" xmlns=""></p><div class="subSection" xmlns="">
```

Vi fa riferimento l'elemento Grouping **WorkInfo** nel file XML dei dati di configurazione per la RCDC **Configurazione per creazione utente**. Si noti che il nome file `\<GUID\>.htm` e il segnalibro sono specificati dal parametro my:Link:

```
<my:Grouping my:Name="WorkInfo" my:Caption="%SYMBOL_WorkInfoTabCaption_END%" my:Enabled="true" my:Visible="true"> <my:Help my:HelpText="%SYMBOL_WorkInfoTabHelpText_END%" my:Link="5e18a08b-4b20-48b8-90c6-c20f6cbeeb44.htm#bkmk_grouping_WorkInfo"/>
```

**Esempi di controlli semplici**

La figura seguente illustra alcuni controlli casella di testo in modalità diverse.

Esempio:

![](media/rcd-configuration-xml-reference/image016.gif)

Il segmento di codice seguente crea il primo controllo casella di testo, che usa testo esplicito per tutti gli attributi e tutte le proprietà:

```
<!-- Sample for a simple control to use explicit information. (with hints)-->
<my:Control my:Name="ExplicitControl" my:TypeName="UocTextBox" my:Caption="Explicit Control" my:Description="This is explicit description." my:Hint="This is a Hint (enter any text).">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="True"/>
          <my:Property my:Name="RegularExpression" my:Value="[A-Z]*"/>
          <my:Property my:Name="Text" my:Value="Enter Information Here"/>
     </my:Properties>
</my:Control>
<!-- End of Sample for a simple control to use explicit information.-->

```


Il segmento di codice seguente crea il secondo controllo casella di testo, che viene collegato a un'origine dati diversa tramite associazione dinamica:

```
<!-- Sample for a simple control to use stored data information.-->
<my:Control my:Name="DynamicControl" my:TypeName="UocTextBox" my:Caption="{Binding Source=schema, Path=DisplayName.DisplayName, Mode=OneWay}" my:Description="{Binding Source=schema, Path=DisplayName.Description, Mode=OneWay}" my:RightsLevel="{Binding Source=rights, Path=DisplayName, Mode=OneWay}">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=DisplayName.Required, Mode=OneWay}"/>
          <my:Property my:Name="RegularExpression" my:Value="{Binding Source=schema, Path=DisplayName.StringRegex, Mode=OneWay}"/>
          <my:Property my:Name="Text" my:Value="{Binding Source=object, Path=DisplayName, Mode=TwoWay}"/>
     </my:Properties>
</my:Control>
<!-- End of Sample for a simple control to use stored data information.-->
```


Il segmento di codice seguente crea il terzo controllo casella di testo, con un'etichetta espansa:

```
<!-- Sample for a simple expanded control with caption control.-->
<my:Control my:Name="SampleExpandLabel" my:TypeName="UocLabel" my:ExpandArea="true">
     <my:Properties>
          <my:Property my:Name="Text" my:Value="This is an expanded control."/>
     </my:Properties>
</my:Control>
<my:Control my:Name="ExpandedControl" my:TypeName="UocTextBox"
          my:ExpandArea="true">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="false"/>
          <my:Property my:Name="Columns" my:Value="40"/>
          <my:Property my:Name="Text" my:Value="Expanded control (enter text)"/>
     </my:Properties>
</my:Control>
<!-- End of Sample for a simple expanded control.-->
```

Il segmento di codice seguente crea il quarto controllo casella di testo, disabilitato.
Anche se per questo controllo la differenza tra stato disabilitato e stato abilitato non è visibile, l'utente non può più immettere dati nella casella di testo.

```
<!-- Sample for a simple disabled control.-->
<my:Control my:Name="DisabledControl" my:TypeName="UocTextBox" my:Caption="Disabled Control" my:Description="This is disabled simple control." my:Enabled="false">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="false"/>
          <my:Property my:Name="MaxLength" my:Value="128"/>
          <my:Property my:Name="Text" my:Value="Disabled control"/>
     </my:Properties>
</my:Control>
<!-- End of Sample for a simple disabled control.-->
```
## <a name="individual-controls"></a>Controlli singoli

### <a name="uocbutton"></a>UocButton

**Nome**: UocButton

**Descrizione**: controllo pulsante semplice che consente di attivare azioni specifiche. Tuttavia, poiché non è possibile specificare un gestore personalizzato, l'uso di questo controllo è limitato.

**Proprietà**:

1.  **Tutte le proprietà comuni**: per informazioni su questa proprietà, vedere la sezione Proprietà comuni in questo documento.

2.  **Text**: questa proprietà specifica il testo visualizzato sul pulsante. Questo è un attributo facoltativo di tipo stringa. La proprietà Text accetta un valore stringa esplicito.

Evento:

   • **OnButtonClicked**: evento generato quando si fa clic sul pulsante.

Esempio:

![](media/rcd-configuration-xml-reference/image017.png)


Il segmento XML seguente genera un pulsante semplice:

```
<!--Sample enabled simple button control-->
<my:Control my:Name="ButtonControl" my:TypeName="UocButton" my:Caption="SampleButton" my:Description="This is a simple button."
my:Hint="Click the button">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="True"/>
          <my:Property my:Name="Text" my:Value="Click Me"/>
     </my:Properties>
</my:Control>
<!--End of sample enabled simple button control -->
```

### <a name="uoccaptioncontrol"></a>UocCaptionControl

**Nome**: UocCaptionControl

**Descrizione**: controllo usato per visualizzare la didascalia di una pagina RCDC. Questo controllo è destinato a essere usato solo come controllo singolo in un elemento Grouping di intestazione.
L'uso in qualsiasi altro contesto può causare problemi di rendering o errori del portale.

**Modalità**: sola lettura (unidirezionale)

**Proprietà:**

1.  **Tutte le proprietà comuni**: per altre informazioni, vedere la sezione Proprietà comuni in questo documento.

2.  **MaxHeight:** questa proprietà specifica l'altezza massima dell'icona nella sezione della didascalia. Questa proprietà è facoltativa. Questa proprietà accetta un valore intero in pixel. Il valore predefinito è 32 pixel.

Esempio:

![](media/rcd-configuration-xml-reference/image020.jpg)

Il segmento di codice seguente genera **Header Caption**:
```
<!--Sample header caption control-->
<my:Control my:Name="SampleHeaderCaption" my:TypeName="UocCaptionControl" my:ExpandArea="true" my:Caption="Header Caption" my:Description="Description Starts here.">
     <my:Properties>
          <my:Property my:Name="MaxHeight" my:Value="32"/>
          <my:Property my:Name="MaxWidth" my:Value="32"/>
     </my:Properties>
</my:Control>
<!--End of sample header caption control-->

```


Questo segmento di codice genera **Explicit Content Caption:**

```
<my:Control my:Name="SampleContentCaption" my:TypeName="UocCaptionControl" my:ExpandArea="true" my:Caption="Sample Explicit Content Caption" my:Description="Explicit content caption with smaller icon">
     <my:Properties>
          <my:Property my:Name="MaxHeight" my:Value="20"/>
          <my:Property my:Name="MaxWidth" my:Value="20"/>
     </my:Properties>
</my:Control>
<!--End of sample caption-->
```

Il segmento di codice seguente genera la didascalia dinamica **Display Name**:
```
<!--Sample content dynamic caption-->
<my:Control my:Name="Caption3" my:TypeName="UocCaptionControl" my:Caption="{Binding Source=schema, Path=DisplayName.DisplayName, Mode=OneWay}" my:Description="{Binding Source=schema, Path=DisplayName.Description, Mode=OneWay}"/>
<!--End of sample caption -->
```

### <a name="uoccheckbox"></a>UocCheckBox

**Nome**: UocCheckBox

Descrizione: controllo casella di controllo semplice. È consigliabile associare questo controllo a dati di tipo booleano. Questo controllo può essere usato come controllo di sola lettura o aggiornabile, in base ai dati a cui è associato.

>[!NOTE]
In questa versione, se si usa il controllo casella di controllo in modalità di modifica per visualizzare un attributo booleano e all'attributo non è stato ancora assegnato alcun valore, quando si fa clic su **OK**, il controllo delle risorse aggiunge il valore **false** all'attributo. Come soluzioni alternative, creare sempre un attributo booleano che presuma che la mancata esistenza equivalga a **false** oppure usare sempre altri controlli, ad esempio un pulsante di opzione, per gli attributi booleani.

**Proprietà**:

1.  **Tutte le proprietà comuni**: per altre informazioni, vedere la sezione Proprietà comuni in questo documento.

2.  **DefaultValue**: proprietà facoltativa di tipo booleano. Il valore predefinito è impostato su false. Questo campo specifica il comportamento predefinito di una casella di controllo.
    Questo valore può essere specificato in modo esplicito.

3.  **Checked**: proprietà facoltativa di tipo booleano. Il valore predefinito è impostato su false. Se presente insieme alla proprietà DefaultValue, quest'ultima viene sovrascritta da questo valore. Questo campo specifica il comportamento di una casella di controllo. Come DefaultValue, può essere specificato in modo esplicito o associato con dati nel server.

4.  **Text**: attributo facoltativo di tipo stringa. Il testo viene visualizzato a destra della casella di controllo. È possibile usare questa proprietà per specificare testo con altre informazioni per l'utente finale.

**Eventi**:

   • CheckedChanged: questo evento viene generato quando lo stato della casella di controllo cambia.

L'esempio seguente crea un'associazione personalizzata tra il tipo di risorsa personalizzata e l'attributo **IsConfigurationType**. Il codice XML viene usato nella RCDC di un tipo di risorsa personalizzata.

Esempio:

![](media/rcd-configuration-xml-reference/image022.png)

Il segmento di codice seguente genera una **casella di controllo dinamica** come la Dynamic Check Box illustrata nella figura precedente. Questo tipo di associazione è in genere più versatile e utile rispetto a una casella di controllo esplicita. L'attributo deve appartenere al tipo di risorsa corrente.

```
<!--Sample dynamic check box-->
<my:Control my:Name="SampleDynamicCheckBox" my:TypeName="UocCheckBox" my:Caption="Dynamic Check Box" my:Description="This is a dynamic check box. It saves to data source." my:RightsLevel="{Binding Source=rights, Path=IsConfigurationType}">
     <my:Properties>
          <my:Property my:Name="Text" my:Value="{Binding Source=schema, Path=IsConfigurationType.DisplayName, Mode=OneWay}"/>
          <my:Property my:Name="Checked" my:Value="{Binding Source=object, Path=IsConfigurationType, Mode=TwoWay}"/>
     </my:Properties>
</my:Control>
<!--End of sample dynamic check box -->
```


### <a name="uoccommonmultivaluecontrol"></a>UocCommonMultiValueControl

**Descrizione**: controllo casella di testo multilinea che supporta la formattazione speciale delle stringhe. Nelle voci con più valori, ogni valore è separato dagli altri da un punto e virgola ; o, in una casella di testo, da un'interruzione di riga. È consigliabile associare questo controllo a dati di tipo stringa breve e Integer con più valori. Questo controllo può essere sia in modalità di sola lettura che in modalità aggiornabile.

**Proprietà**:

1.  **Tutte le proprietà comuni**: per altre informazioni, vedere la sezione Proprietà comuni in questo documento.

2.  **DataType**: attributo obbligatorio di tipo stringa. È possibile dichiararlo esplicitamente come **String, Integer**, o **DateTime**. È anche possibile associare l'attributo alla proprietà **DataType** dell'attributo dello schema. Un tipo riferimento multivalore deve essere gestito da **UOCListView** o **UOCIdentityPicker**. Il tipo di dati booleano multivalore non è supportato.

3.  **Rows**: attributo facoltativo di tipo Integer. È possibile definire l'altezza della casella come numero di caratteri. Il valore predefinito è impostato su 1.

4.  **Columns**: attributo facoltativo di tipo Integer. È possibile definire la larghezza della casella come numero di caratteri. Il valore predefinito è impostato su
    20.

5.  **Value**: attributo facoltativo di tipo stringa. È possibile associare questo attributo solo a origini dati.

Eventi:

   • **ValueListChanged**: evento attivato quando il valore corrente del controllo cambia.

Nell'esempio seguente, viene creato un attributo stringa multivalore denominato **AMultiValueString**, che viene quindi associato al tipo di risorsa personalizzata. Questo esempio funziona solo dopo la creazione di questa associazione.

**Esempio:**

![](media/rcd-configuration-xml-reference/image024.jpg)

Il segmento di codice seguente genera l'immagine precedente:

```
<!--Sample multivalue control-->
<my:Control my:Name="SampleDynamicMultiValueControl" my:TypeName="UocCommonMultiValueControl" my:Caption="{Binding Source=schema, Path=AMultiValueString.DisplayName, Mode=OneWay}" my:Description="{Binding Source=schema, Path=AMultiValueString.Description, Mode=OneWay}" my:RightsLevel="{Binding Source=rights, Path=AMultiValueString}">
     <my:Properties>
          <my:Property my:Name="Rows" my:Value="6"/>
          <my:Property my:Name="Columns" my:Value="60"/>
          <my:Property my:Name="DataType" my:Value="String"/>
          <!--not supported for above property my:Value={Binding Source=schema, Path=AMultiValueString.DataType, Mode=OneWay}"/>-->
          <my:Property my:Name="Value" my:Value="{Binding Source=object, Path=AMultiValueString, Mode=TwoWay}"/>
     </my:Properties>
</my:Control>
<!--End of sample multivalue control -->
```



### <a name="uocdatetimecontrol"></a>UocDateTimeControl


**Nome**: UocDateTimeControl

**Descrizione**: simile a un controllo casella di testo, ma **Description** accetta solo un formato specifico. In modalità di sola lettura ha l'aspetto di un'etichetta. Per il formato della stringa di input supportato, vedere la proprietà **DateTimeFormat** in questa sezione.

**Proprietà**:

1.  **Tutte le proprietà comuni**: per altre informazioni, vedere la sezione Proprietà comuni in questo documento.

2.  **DateTimeFormat**: attributo facoltativo di tipo stringa. I formati supportati sono DateTime o DateOnly. Il valore predefinito è impostato sul formato DateTime.
    a. Formato DateTime: attributo formattato come mm/gg/aaaa hh:mm:ss AM.

      <[!NOTE]
      Entrambi i formati **DateTime** e **DateOnly** sono supportati, indipendentemente dall'utente che specifica la differenza.
3.  **Value**: attributo facoltativo di tipo stringa. Associare questo attributo a un'origine dati della risorsa. Il valore di questo attributo deve essere conforme al formato datetime corretto.

Eventi:

   • **DateTimeChanged**: generato quando il valore datetime viene modificato.

Esempio:

![](media/rcd-configuration-xml-reference/image027.jpg)

Il segmento di codice seguente genera il primo controllo **DateTime**.

```
<!--Sample explicit DateTime control-->
<my:Control my:Name="SampleExplicitDateTimeControl" my:TypeName="UocDateTimeControl" my:Caption="Explicit Date Time Control" my:Description="The data shown here is explicit and in date time format.">
     <my:Properties>
          <my:Property my:Name="DateTimeFormat" my:Value="DateTime"/>
          <my:Property my:Name="Value" my:Value="11/11/2008 00:00:00"/>
     </my:Properties>
</my:Control>
<!--End of sample explicit DateTime control -->
```


Il segmento di codice seguente genera il secondo controllo **DateTime**. Se è stato usato il codice di esempio della sezione Origini dati, 'attributo **ExpirationTime** è associato a tutti i tipi di risorsa. Pertanto, è possibile usarlo con il codice seguente:

```
<!--Sample dynamic DateTime control-->
<my:Control my:Name="SampleDynamicDateTimeControl" my:TypeName="UocDateTimeControl" my:Caption="{Binding Source=schema, Path=ExpirationTime.DisplayName, Mode=OneWay}" my:Description="{Binding Source=schema, Path=ExpirationTime.Description, Mode=OneWay}" my:RightsLevel="{Binding Source=rights, Path=ExpirationTime}">
     <my:Properties>
          <my:Property my:Name="DateTimeFormat" my:Value="DateOnly"/>
          <my:Property my:Name="Value" my:Value="{Binding Source=object, Path=ExpirationTime, Mode=TwoWay}"/>
     </my:Properties>
</my:Control>
<!--End of dynamic explicit DateTime control -->
```



### <a name="uocdropdownlist"></a>UocDropDownList

**Nome**: UocDropDownList

Descrizione: controllo casella di riepilogo a discesa semplice. Questo controllo è in genere usato quando si vogliono selezionare opzioni all'interno di un set di scelte definito. I tipi di dati stringa, Integer, datetime e booleano sono buoni candidati per questo controllo.

**Proprietà**:

1.  **Tutte le proprietà comuni**: per altre informazioni, vedere la sezione Proprietà comuni in questo documento.

2.  **ValuePath**: proprietà usata per ottenere l'attributo Value da ItemSource. Se ItemSource è specificato come Custom, ValuePath è impostato su Value. È associato al campo Value dell'elemento Option definito più avanti in questo documento.

3.  **CaptionPath**: proprietà usata per ottenere l'attributo Value da ItemSource. Se ItemSource è specificato come Custom, ValuePath è impostato su Caption. È associato al campo Caption dell'elemento Option definito più avanti in questo documento.

4.  **HintPath**: proprietà usata per ottenere l'attributo Value da ItemSource. Se ItemSource è specificato come Custom, ValuePath è impostato su Hint. È associato al campo Hint dell'elemento Option definito più avanti in questo documento.

5.  **ItemSource**: raccolta di elementi controllo elenco che definisce le scelte nell'elenco. L'utente può impostare questa proprietà in modo esplicito su Custom e usare l'elemento Option per specificare il valore della stringa.

6.  **SelectedValue**: valore attualmente selezionato. Questa è una proprietà obbligatoria di tipo stringa. Questa proprietà è associata a dati di tipo stringa dell'origine dati.

Eventi:

  • SelectedIndexChanged: si verifica quando la selezione nella casella di riepilogo a discesa viene modificata.

Opzioni:

Per la struttura di un elemento Option, vedere la sezione "Options" in questo documento.

1.  **Value**: il valore di un singolo elemento Option può essere impostato su qualsiasi stringa che rappresenti un input valido dall'origine dati a cui è associato il controllo.

2.  **Caption**: può essere un qualsiasi valore stringa.

3.  **Hint**: può essere un qualsiasi valore stringa.

Esempio:

![](media/rcd-configuration-xml-reference/image030.jpg)


![](media/rcd-configuration-xml-reference/image031.jpg)

>[!NOTE]
Perché l'esempio funzioni, è necessario associare un **ambito** di attributo di tipo stringa esistente al tipo di risorsa personalizzata a cui si applica la RCDC.


Questo segmento di codice genera l'elenco a discesa:

```
<!--Sample for drop-down list control-->
<my:Control my:Name="Scope" my:TypeName="UocDropDownList" my:Caption="{Binding Source=schema, Path=Scope.DisplayName}" my:RightsLevel="{Binding Source=rights, Path=Scope}">
     <my:Options>
          <my:Option my:Value="DomainLocal" my:Caption="Domain Local" my:Hint="to secure a local resource (i.e. a file share on your computer)" />
          <my:Option my:Value="Global" my:Caption="Global" my:Hint="to secure resources across your team or division" />
          <my:Option my:Value="Universal" my:Caption="Universal" my:Hint="to use this group across your organization" />
     </my:Options>
     <my:Properties>
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=Scope.Required" />
          <my:Property my:Name="ValuePath" my:Value="Value" />
          <my:Property my:Name="CaptionPath" my:Value="Caption" />
          <my:Property my:Name="HintPath" my:Value="Hint" />
          <my:Property my:Name="ItemSource" my:Value="Custom" />
          <my:Property my:Name="SelectedValue" my:Value="{Binding Source=object, Path=Scope, Mode=TwoWay}" />
     </my:Properties>
</my:Control>
<!--End of Sample for drop-down list control-->
```


### <a name="uocfiledownload"></a>UocFileDownload

Nome: UocFileDownload

Descrizione: questo controllo contiene un collegamento ipertestuale. Quando si fa clic sul collegamento ipertestuale, viene visualizzata una pagina Salva file di Windows. L'utente può salvare il file nell'unità locale.
Anche l'opzione Apri è supportata se Internet Explorer è in grado di eseguire il rendering del formato del file. I tipi di dati consigliati per questo controllo sono i tipi stringa formattata (XML) e binario.

>[!NOTE]
In questa versione di Microsoft Identity Manager 2016 SP1 è necessario chiudere la finestra di Internet Explorer in cui è aperto il file e quindi aggiornare la pagina. Dopo l'aggiornamento della finestra di Internet Explorer è possibile avviare il download per salvare o aprire di nuovo lo stesso file nella finestra originale.


Proprietà:

1.  **Tutte le proprietà comuni**: per altre informazioni, vedere la sezione Proprietà comuni in questo documento.

2.  **Text**: attributo di tipo stringa facoltativo che definisce il testo del collegamento ipertestuale. È possibile specificare in modo esplicito una stringa per questa proprietà.

3.  **Value**: attributo obbligatorio. Specifica l'associazione di attributi nel server di cui è necessario scaricare il contenuto.

4.  **PromptedFileName**: attributo facoltativo di tipo stringa. Questo è il nome del file suggerito al momento di salvare il file scaricato.

5.  **ContentType**: attributo obbligatorio di tipo stringa. Questo è il tipo di file in cui vengono salvati i dati. Testo e binario sono i due tipi di stringa supportati. Se il tipo è testo, il valore restituito viene considerato di tipo stringa lunga.
    In caso contrario, se il tipo è binario, il valore restituito viene considerato di tipo byte[]. Se è selezionato il tipo testo, l'utente ha la possibilità di aggiungere un suffisso per specificare il tipo di formato in cui si trova il testo. Ad esempio, testo/xml è un valore valido.


>[!NOTE]
Se il valore associato a questo controllo è vuoto, il controllo è privo del collegamento ipertestuale da usare per attivare l'azione di download, poiché non c'è niente da scaricare.


**Eventi**:

Nessun evento per questo controllo.

**Esempio**:

![](media/rcd-configuration-xml-reference/image035.png)


>[!NOTE]
Prima di caricare questo file di esempio, è necessario creare un'associazione tra un tipo di risorsa personalizzata e l'attributo ConfigurationData esistente.


Il segmento di codice seguente genera il controllo di download di file nell'immagine precedente:

```
<!--Sample dynamic download control-->
<my:Control my:Name="SampleDynamicFileDownloadControl" my:TypeName="UocFileDownload" my:Caption="{Binding Source=schema, Path=ConfigurationData.DisplayName, Mode=OneWay}" my:Description="{Binding Source=schema, Path=ConfigurationData.Description, Mode=OneWay}" my:RightsLevel="{Binding Source=rights, Path=ConfigurationData}">
     <my:Properties>
          <my:Property my:Name="Text" my:Value="Download Dummy xml"/>
          <my:Property my:Name="PromptedFileName" my:Value="DummyXML.xml"/>
          <my:Property my:Name="ContentType" my:Value="text/xml"/>
          <my:Property my:Name="Value" my:Value="{Binding Source=object, Path=ConfigurationData}"/>
     </my:Properties>
</my:Control>
<!--End of dynamic download control -->
```


### <a name="uocfileupload"></a>UocFileUpload

**Nome**: UocFileUpload

**Descrizione**: il controllo contiene una casella di testo che visualizza il percorso del file locale da caricare, un pulsante per la ricerca di file e un pulsante di caricamento. Quando l'utente finale fa clic su un pulsante Sfoglia, viene visualizzata una finestra Apri file di Windows. Nell'unità locale l'utente finale può selezionare un file da caricare. Dopo che il file è stato selezionato, nella casella di testo è visualizzato il percorso del file. Quando si fa clic sul pulsante Carica, il file viene caricato nell'origine dati locale lato client. Il contenuto del file non è stato ancora inviato al server. I tipi di dati consigliati per questo controllo sono i tipi stringa formattata (XML) o binario.

>[!NOTE]
Non è presente alcuna indicazione dell'avanzamento o dello stato del caricamento. Quando il file è stato caricato nell'origine dati locale, la casella di testo viene deselezionata.


Proprietà:

1.  Tutte le proprietà comuni: per informazioni, vedere la sezione Proprietà comuni in questo documento.

2.  Value: attributo obbligatorio. Specifica l'associazione degli attributi dello schema nel server in cui i dati vengono caricati.

3.  ContentType: attributo facoltativo di tipo stringa. Questo è il tipo di dati in cui il file viene salvato nel server. Può essere impostato su Text o Binary. Se la proprietà non è impostata, il valore predefinito è Binary.

4.  MaxFileSize: attributo facoltativo di tipo stringa. MaxFileSize definisce la dimensione massima consentita per il file caricato. Per impostazione predefinita, se la proprietà non è impostata, la dimensione massima è di 1 MB.

5.  PromptedForNoValue: attributo facoltativo di tipo stringa. Definisce il testo visualizzato per l'utente quando un file non viene caricato.

Eventi:

   • FileUploaded: questo evento viene generato quando il file viene caricato correttamente.

Esempio:

![](media/rcd-configuration-xml-reference/image040.png)

>[!NOTE]
Perché il codice di esempio seguente funzioni, è necessario creare un nuovo attributo di tipo binario denominato ABinaryAttribute e quindi creare una nuova associazione tra un tipo di risorsa personalizzata e questo attributo.


Il segmento di codice seguente genera il controllo di caricamento dell'immagine precedente:
```
<!--Sample dynamic upload control-->
<my:Control my:Name="SampleDynamicFileUploadControl" my:TypeName="UocFileUpload" my:Caption="{Binding Source=schema, Path=ABinaryAttribute.DisplayName, Mode=OneWay}" my:Description="{Binding Source=schema, Path=ABinaryAttribute.Description, Mode=OneWay}” my:RightsLevel="{Binding Source=rights, Path=ABinaryAttribute}">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=ABinaryAttribute.Required}"/>
          <my:Property my:Name="ContentType" my:Value="Binary"/>
          <my:Property my:Name="Value" my:Value="{Binding Source=object, Path=ABinaryAttribute, Mode=TwoWay}"/>
     </my:Properties>
</my:Control>
<!--End of dynamic upload control -->
```

### <a name="uocfilterbuilder"></a>UocFilterBuilder

Nome: UocFilterBuilder

Descrizione: controllo complesso che consente all'utente di eseguire il rendering di un'espressione XPath di MIM 2016. Alcune espressioni XPath non sono supportate. Per informazioni su come usare il generatore di filtri, vedere le relative informazioni della Guida.

Proprietà:

1.  Tutte le proprietà comuni: per altre informazioni, vedere la sezione Proprietà comuni in questo documento.

2.  PermittedObjectTypes: definisce un elenco di tipi di risorsa da visualizzare nell'istruzione Select di un generatore di filtri. Per informazioni su come usare il generatore di filtri, vedere le informazioni della Guida corrispondenti. La stringa è nel formato TipoRisorsaA, TipoRisorsaB, in cui ogni tipo di risorsa è separato da una virgola (,).

3.  Value: valore con cui viene eseguito il rendering del generatore di filtri.
    È supportata solo l'associazione con dati di tipo stringa contenenti un'espressione XPath. L'attributo Filter è l'attributo consigliato per l'associazione di questo controllo.

4.  PreviewButtonVisible: proprietà facoltativa di tipo booleano. Se questa proprietà è impostata su false, l'utente non è in grado di visualizzare il pulsante Anteprima. Il valore predefinito è impostato su true. È possibile usare questo pulsante in combinazione con un controllo visualizzazione elenco per un'anteprima dei risultati di un'espressione XPath.

5.  ExcludeGroupMembership: proprietà booleana. Se questa proprietà è impostata su true, non è possibile creare un filtro che usi \<Attributo di riferimento\> (ad esempio, ResourceID) è membro di \<Oggetto gruppo\>. In altre parole, se questa proprietà è impostata su true, non è possibile creare un filtro che usi la directory di appartenenza a gruppi.

6.  PreviewButtonCaption: stringa facoltativa. Se la proprietà PreviewButtonVisible è impostata su true, è possibile usare questa proprietà per assegnare testo personalizzato al pulsante. Il testo viene visualizzato sul pulsante Anteprima.

Eventi:

   • OnFilterChanged: questo evento viene attivato quando il contenuto del generatore di filtri cambia.

Esempio:

![](media/rcd-configuration-xml-reference/image044.png)

>[!NOTE]
Prima di usare questo codice di esempio, creare una nuova associazione tra un attributo Filter esistente e un tipo di risorsa personalizzata.


Il codice di esempio riportato di seguito include un controllo UOCLabel, un generatore di filtri semplice con PermittedObjectTypes e una visualizzazione elenco di anteprima. È necessario impostare la proprietà ListFilter della visualizzazione elenco e la proprietà Value del generatore di filtri Generatore sullo stesso attributo di origine dati per collegare le due proprietà.

```
<!--Sample filter builder with preview list-->
<my:Control my:Name="ComplexFilterBuilderLabel" my:TypeName="UocLabel" my:ExpandArea="true">
     <my:Properties>
          <my:Property my:Name="Text" my:Value="This is a Filter Builder with preview."/>
     </my:Properties>
</my:Control>
<my:Control my:Name="ComplexFilterBuilder" my:TypeName="UocFilterBuilder" my:RightsLevel="{Binding Source=rights, Path=Filter}" my:ExpandArea="true">
     <my:Properties>
          <my:Property my:Name="PermittedObjectTypes" my:Value="Person,Group" />
          <my:Property my:Name="Value" my:Value="{Binding Source=object, Path=Filter, Mode=TwoWay}" />
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=Filter.Required, Mode=OneWay}" />
     </my:Properties>
</my:Control>
<my:Control my:Name="FilterBuilderwithpreview" my:TypeName="UocListView" my:ExpandArea="true">
     <my:Properties>
          <my:Property my:Name="ColumnsToDisplay" my:Value="DisplayName,ObjectType,AccountName" />
          <my:Property my:Name="EmptyResultText" my:Value="There is no members according to the filter definition." />
       <my:Property my:Name="PageSize" my:Value="10" />
       <my:Property my:Name="ShowTitleBar" my:Value="false" />
       <my:Property my:Name="ShowActionBar" my:Value="false" />
       <my:Property my:Name="ShowPreview" my:Value="false" />
       <my:Property my:Name="ShowSearchControl" my:Value="false" />
       <my:Property my:Name="EnableSelection" my:Value="false" />
       <my:Property my:Name="SingleSelection" my:Value="false" />
       <my:Property my:Name="ItemClickBehavior" my:Value=" ModelessDialog "/>
       <my:Property my:Name="ListFilter" my:Value="{Binding Source=object, Path=Filter}" />
     </my:Properties>
</my:Control>
<!--end of sample filter builder with preview-->
```


<a name="uochtmlsummary"></a>UocHtmlSummary
--------------

Nome: UocHtmlSummary

Descrizione: è possibile usare questo controllo per definire una pagina di riepilogo in una pagina RCDC.
Questa pagina di riepilogo viene visualizzata dopo che l'utente finale ha inviato una richiesta. Questo controllo può essere usato solo in un elemento Grouping di riepilogo e deve essere l'unico controllo. È consigliabile usare il codice di esempio fornito.

>[!NOTE]
Questo controllo non è stato testato in modo esteso.


Proprietà:

1.  Tutte le proprietà comuni: per informazioni su questa proprietà, vedere la sezione Proprietà comuni in questo documento.

2.  ModificationsXml: questa proprietà deve essere formattata come {Binding Source=delta, Path=DeltaXml}, dove delta è definito in ObjectDataSource dell'intestazione della configurazione.

3.  TransformXsl: questa proprietà è in genere formattata come {Binding Source=summaryTransformXsl, Path=/}, dove summaryTransformXsl è definito in XmlDataSource dell'intestazione della configurazione.

Per un esempio esistente di questo controllo, vedere "Grouping di riepilogo" nella sezione Grouping più indietro in questo documento.

### <a name="uochyperlink"></a>UocHyperLink

Nome: UocHyperLink

Descrizione: controllo collegamento ipertestuale semplice. È possibile usare questo controllo per visualizzare informazioni sotto forma di collegamento ipertestuale.

Proprietà:

1.  Tutte le proprietà comuni: per informazioni, vedere la sezione Proprietà comuni in questo documento.

2.  ObjectReference: proprietà facoltativa di tipo riferimento. Se tramite il GUID definito in questa proprietà si fa riferimento a una risorsa valida, il collegamento ipertestuale consente all'utente finale di accedere alla risorsa. Questa proprietà e la proprietà NavigateUrl (di seguito) si escludono reciprocamente.

3.  Text: proprietà facoltativa di tipo stringa. Questa proprietà viene usata per definire il testo visualizzato come collegamento ipertestuale.

4.  NavigateUrl: proprietà facoltativa di tipo stringa. Questa proprietà viene usata per definire l'URL completo di destinazione del collegamento ipertestuale. Questa proprietà e la proprietà ObjectReference (precedente) si escludono reciprocamente.

Esempio:

![](media/rcd-configuration-xml-reference/image049.jpg)

>[!NOTE]
È necessario il GUID valido di una risorsa a cui collegare questa proprietà. In questo caso, il secondo collegamento ipertestuale viene generato con un GUID valido. Il primo può corrispondere a qualsiasi sito Web.


Il segmento di codice seguente genera un collegamento ipertestuale di reindirizzamento:

```
<!--Sample for a hyperlink that redirects page.-->
<my:Control my:Name="RedirectHyperlink" my:TypeName="UocHyperLink" my:Caption="Redirect Hyperlink" my:Description="This is a hyperlink that takes you to other pages.">
     <my:Properties>
          <my:Property my:Name="NavigateUrl" my:Value="http://www.microsoft.com"/>
          <my:Property my:Name="Text" my:Value="Microsoft Home Page"/>
     </my:Properties>
</my:Control>
<!--End of Sample for a hyperlink that redirect page-->

```

Il segmento di codice seguente genera un collegamento ipertestuale che fa riferimento a una risorsa. Per l'associazione a un'origine dati, il riferimento esplicito può essere sostituito dall'espressione {Binding Source=object, Path=Creator}. Questo può essere valido solo se il gestore della risorsa esiste e il suo valore è di tipo riferimento.

```
<!--Sample for a hyperlink that reference object-->
<my:Control my:Name="ReferenceHyperlink" my:TypeName="UocHyperLink" my:Caption="Reference Hyperlink" my:Description="This is a hyperlink gives you an object view of the reference object">
     <my:Properties>
          <my:Property my:Name="ObjectReference" my:Value="e4e048b1-9e43-415e-806c-cf44c429c34c"/>
          <my:Property my:Name="Text" my:Value="View a group in FIM 2010."/>
     </my:Properties>
</my:Control>
<!--End of Sample for a hyperlink that reference object-->
```

### <a name="uocidentitypicker"></a>UocIdentityPicker

Nome: UocIdentityPicker

Descrizione: questo controllo è costituito da una casella Risolvi facoltativa e da una finestra Sfoglia. La casella Risolvi facoltativa è costituita da una casella di testo in cui immettere l'identità, da un pulsante Risolvi per risolvere quest'ultima e da un pulsante Sfoglia che richiama una finestra popup Sfoglia. La finestra Sfoglia consente di selezionare le identità tramite un controllo visualizzazione elenco. L'identità selezionata dalla finestra Sfoglia viene riflessa nella finestra Risolvi.

Proprietà:

1.  Tutte le proprietà comuni: per informazioni su questa proprietà, vedere la sezione Proprietà comuni in questo documento.

2.  UsageKeywords: proprietà stringa facoltativa. È possibile definire un elenco di ambiti di ricerca da usare in Selezione risorse specificando un elenco di parole chiave di utilizzo supportate dalla struttura SearchScopeConfiguration, in cui ogni parola chiave è separata da un apostrofo (').

3.  Filter: proprietà stringa facoltativa. L'espressione XPath specificata dall'utente definisce l'ambito di Selezione risorse in modo da visualizzare solo gli elementi che rientrano in un ambito definito. Questa proprietà e la proprietà UsageKeywords (precedente) si escludono reciprocamente. Se è applicato l'ambito di ricerca, questa proprietà non ha alcun effetto.

4.  ResultObjectType: proprietà facoltativa di tipo stringa. Il tipo di risorsa viene usato per il rendering delle risorse nell'elenco della finestra di dialogo popup. Viene usato con Filter per consentire a Selezione identità di identificare il tipo di risorsa restituito da Filter e di eseguire il rendering dei dati di conseguenza. Questa proprietà e la proprietà UsageKeywords (vedere sopra) si escludono reciprocamente. Se è applicato l'ambito di ricerca, questa proprietà non ha alcun effetto. Per questa proprietà, viene accettata qualsiasi stringa corrispondente a un singolo nome di tipo di risorsa valido, ad esempio Person.
    Se si prevede che il filtro restituisca più tipi di risorsa, viene usato Resource.

5.  PreviewTitle: titolo di anteprima usato in una visualizzazione elenco. Per informazioni su questa proprietà, vedere la sezione UocListView.

6.  ListViewTitle: proprietà stringa facoltativa. È possibile usare questa proprietà per definire il testo visualizzato nella parte superiore della visualizzazione elenco come titolo.

7.  Value: proprietà stringa facoltativa. È consigliabile associare questa proprietà con un attributo dello schema per connettere il valore a un'origine dati.

8.  Mode: proprietà stringa facoltativa. Questa proprietà viene usata per definire se Selezione identità può selezionare un solo valore o più identità. I valori consentiti sono SingleResult e MultipleResult. Per impostazione predefinita è impostato il valore SingleResult.

9.  ObjectTypes: proprietà facoltativa di tipo stringa. È possibile definire un elenco di tipi di risorse rispetto ai quali l'utente finale può risolvere le voci nella casella Risolvi di Selezione identità. L'elenco è costituito da una serie di nomi di tipi di risorsa separati da una virgola (,).

10. AttributesToSearch: proprietà facoltativa di tipo stringa. È possibile definire un elenco di attributi da usare per risolvere l'elemento in Selezione identità, dove l'elenco è costituito da una serie di attributi dello schema separati da una virgola (,). Ad esempio, se AttributesToSearch è impostato su DisplayName, Alias, significa che l'utente è in grado di cercare gli elementi con DisplayName = \<valore di ricerca\> o Alias=\<valore di ricerca\>. È consigliabile che i nomi di attributi immessi qui siano attributi validi per tipi di risorsa di destinazione dell'origine dati indicata in Value. I tipi di risorsa di destinazione sono disponibili nel campo ObjectTypes. Tutti gli attributi deve essere validi per i tutti i tipi di risorsa specificati nel campo ObjectTypes.

11. ColumnsToDisplay: proprietà facoltativa di tipo stringa. L'utente deve specificare un elenco di nomi di attributo dello schema separati da una virgola (,). Gli attributi definiti qui vanno a costituire la colonna della visualizzazione elenco in Selezione identità.

12. Rows: proprietà facoltativa di tipo Integer. Funziona solo se la proprietà Mode è impostata su MultipleResult. Usare questa proprietà per impostare l'altezza della casella di testo Risolvi su una dimensione specifica in unità carattere.

13. MainSearchScreenText: proprietà facoltativa di tipo stringa. Si tratta del testo personalizzato visualizzato mentre la ricerca è in esecuzione nella finestra Sfoglia.

Eventi:

 • SelectedObjectChanged: questo evento viene generato quando l'utente modifica le risorse selezionate.

Esempio:

![](media/rcd-configuration-xml-reference/image052.png)

>[!NOTE]
Perché questo esempio funzioni, è necessario creare una nuova associazione tra l'attributo Manager e tutti i tipi di risorsa personalizzata a cui si applica il codice XML.


Il segmento di codice seguente genera una Selezione identità in modalità singola tramite le proprietà Filter e ResultObjectType nell'ambito di una RCDC:

```
<!--Sample for a single-selection identity picker using Filter and Result Object Type-->
<my:Control my:Name="SingleSelectionIdentityPicker" my:TypeName="UocIdentityPicker" my:Caption="A Single Selection Identity Picker" my:Description="The user is allowed to select only one entry here." my:RightsLevel="{Binding Source=rights, Path=Manager}">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=Manager.Required}"/>
          <my:Property my:Name="Mode" my:Value="SingleResult" />
          <!--Columns displayed in list view in pop-up window-->
          <my:Property my:Name="ColumnsToDisplay" my:Value="DisplayName, ObjectType" />
          <!--Identities will be resolved against following attribute in the resolve textbox when resolve button is clicked.-->
          <my:Property my:Name="AttributesToSearch" my:Value="DisplayName, AccountName" />
          <!--single valued reference type attribute is used to bind the control-->
          <my:Property my:Name="Value" my:Value="{Binding Source=object, Path=Manager , Mode=TwoWay}" />
          <!--Scoping the list explicitly to All Persons name contains letter "e"-->
          <my:Property my:Name="Filter" my:Value="/Person[contains(JobTitle, 'Manager')]"/>
          <!--Result object type specify the type is Person-->
          <my:Property my:Name="ResultObjectType" my:Value="Person"/>
          <my:Property my:Name="ListViewTitle" my:Value="Select only one entry" />
          <my:Property my:Name="PreviewTitle" my:Value="Entry selected:" />
     </my:Properties>
</my:Control>
<!--End of sample for a single-selection identity picker.-->
```



Esempio:

![](media/rcd-configuration-xml-reference/image056.jpg)

>[!NOTE]
Perché questo codice di esempio funzioni, è necessario associare l'attributo ExplicitMember (attributo di riferimento multivalore) al tipo di risorsa personalizzata. È anche necessario creare ambiti di ricerca con la proprietà UsageKeyword impostata su Person e Group.


Il segmento di codice seguente crea il controllo dell'immagine precedente:

```
<!--Sample for a multiselection Identity Picker uses Search Scope-->
<my:Control my:Name="multiSelectionIdentityPicker" my:TypeName="UocIdentityPicker" my:Caption="A multi Selection Identity Picker" my:Description="The user is allowed to select more than one entry here" my:RightsLevel="{Binding Source=rights, Path=ExplicitMember}">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=ExplicitMember.Required}"/>
          <my:Property my:Name="Mode" my:Value="MultipleResult" />
          <my:Property my:Name="Rows" my:Value="10" />
          <!--There are existing search scopes that has key word "Person" and "Group" use both sets of search scopes here.-->
          <my:Property my:Name="UsageKeywords" my:Value="Person,Group"/>
          <!--Columns displayed in list view in pop-up window-->
          <my:Property my:Name="ColumnsToDisplay" my:Value="DisplayName, ObjectType" />
          <!--Identities will be resolved against following attribute in the resolve textbox when resolve button is clicked.-->
          <my:Property my:Name="AttributesToSearch" my:Value="DisplayName, AccountName" />
          <!--multi valued reference type attribute is used to bind the control-->
          <my:Property my:Name="Value" my:Value="{Binding Source=object, Path=ExplicitMember , Mode=TwoWay}" />
          <my:Property my:Name="ResultObjectType" my:Value="Resource"/>
          <my:Property my:Name="ListViewTitle" my:Value="Select multiple entries" />
          <my:Property my:Name="PreviewTitle" my:Value="Entries selected" />
     </my:Properties>
</my:Control>
<!--End of sample for a multiselection Identity Picker.-->
```


### <a name="uoclabel"></a>UocLabel

Nome: UocLabel

Descrizione: controllo etichetta di testo semplice di sola lettura. È consigliabile usare questo controllo per visualizzare dati di sola lettura.

Proprietà:

1.  Tutte le proprietà comuni: per informazioni su questa proprietà, vedere la sezione Proprietà comuni in questo documento.

2.  Text: attributo di tipo stringa. Per definire questa proprietà, specificare un valore stringa esplicito o associarla a un'origine dati. Un esempio di associazione per l'assegnazione del valore a questa proprietà è {Binding Source=object, Path=\<nome attributo valido\>.

Per un esempio del controllo UocLabel, vedere un controllo semplice nella sezione Esempi di controlli semplici.

### <a name="uoclistview"></a>UocListView

Nome: UocListView

Descrizione: controllo visualizzazione elenco avanzato. È costituito da una visualizzazione elenco semplice, da una ricerca semplice facoltativa, da un controllo di ricerca avanzata facoltativo, da una casella di anteprima selezione facoltativa e da una barra di pulsanti di azione. La ricerca semplice facoltativa è costituita da un ambito di ricerca e da una casella di testo di ricerca semplice. Il controllo di ricerca avanzata è un generatore di filtri. La visualizzazione elenco visualizza un elenco di risorse sottoposte a pre-rendering. Può anche visualizzare risultati di ricerca provenienti dai controlli di ricerca presenti in questo controllo. La barra dei pulsanti di azione definisce l'azione che è possibile eseguire in base alla selezione nella visualizzazione elenco. La casella di anteprima selezione visualizza le voci selezionate nella visualizzazione elenco.

>[!IMPORTANT]
UocListView non funziona con attributi di riferimento a valore singolo. Può essere usato solo con attributi di riferimento multivalore. Per gli attributi di riferimento a valore singolo, vedere UocIdentityPicker in questo documento.


Proprietà:

1.  Tutte le proprietà comuni: per informazioni su questa proprietà, vedere la sezione Proprietà comuni in questo documento.

2.  SelectedValue: proprietà di tipo stringa facoltativa a cui è in genere associato un attributo di riferimento multivalore che accetta un elenco di stringhe formattate come GUID.

3.  PageSize: proprietà facoltativa di tipo Integer. Consente di specificare il numero di voci che devono essere visualizzate in una pagina di un controllo visualizzazione elenco. Il valore predefinito è 10 voci. Qualsiasi Integer positivo è valido.

4.  UsageKeyword: proprietà facoltativa di tipo stringa. Consente di specificare un elenco di parole chiave che definiscono l'ambito di ricerca da usare per il controllo di ricerca della visualizzazione elenco. Nel server FIM 2010 sono disponibili risorse di ambito di ricerca. L'attributo UsageKeyword di una struttura SearchScopeConfiguration viene usato per raggruppare un set di ambiti di ricerca. Questo elenco di parole chiave viene utilizzato dalla visualizzazione elenco. Ogni parola chiave è separata da una virgola (,).
    Questo è l'attributo UsageKeyword usato nell'ambito di ricerca corrispondente che si vuole mostrare nella visualizzazione elenco. Attivo solo se la proprietà ShowSearchControl è impostata su true.

5.  SearchControlAutoPostback: proprietà booleana facoltativa. Impostare il valore di questa proprietà su true per eseguire AutoPostBack quando viene attivata una ricerca. Per impostazione predefinita, la proprietà SearchControlAutoPostback è impostata su false.

6.  EmptyResultText: proprietà facoltativa di tipo stringa. Per impostazione predefinita, è impostata su Nessun elemento, ma può essere impostata su qualsiasi valore stringa. Questo testo viene visualizzato in caso di risultati di ricerca vuoti.

7.  ButtonHeight: proprietà facoltativa di tipo Integer. Impostare il valore di questa proprietà su un qualsiasi Integer positivo. Questa proprietà definisce l'altezza in pixel dei pulsanti sulla barra delle azioni. Il valore predefinito è 32 pixel.

8.  ButtonWidth: proprietà facoltativa di tipo Integer. Impostare il valore di questa proprietà su un qualsiasi Integer positivo. Questa proprietà definisce la larghezza in pixel dei pulsanti sulla barra delle azioni. Il valore predefinito è 32 pixel.

9.  CaptionImageMaxHeight: proprietà facoltativa di tipo Integer. Impostare il valore di questa proprietà su un qualsiasi Integer positivo. Questa proprietà definisce l'altezza massima dell'icona di una didascalia facoltativa. Il valore predefinito è 32 pixel.

10. CaptionImageMaxWidth: proprietà facoltativa di tipo Integer. Impostare il valore di questa proprietà su un qualsiasi Integer positivo. Questa proprietà definisce la larghezza massima dell'icona di una didascalia facoltativa. Il valore predefinito è 32 pixel.

11. CaptionImageUrl: proprietà facoltativa di tipo stringa. Questa proprietà definisce l'URL di collegamento a un'immagine visualizzata come immagine della didascalia.

12. PreviewTitle: proprietà facoltativa di tipo stringa. Questa proprietà viene usata per definire il testo visualizzato al di sopra della casella di anteprima della selezione.

13. EnableSelection: proprietà facoltativa di tipo booleano. Questa proprietà viene usata per definire se una visualizzazione elenco è in modalità di selezione. In caso affermativo, la colonna più a sinistra della visualizzazione elenco è costituita da una colonna di caselle di controllo e nella parte inferiore della visualizzazione elenco viene visualizzata una casella di anteprima della selezione. Il valore predefinito di questa proprietà è true.

14. SingleSelection: proprietà facoltativa di tipo booleano. Se per la visualizzazione elenco la modalità di selezione è attivata e si imposta questo valore su true, l'utente finale può selezionare solo una voce dall'elenco. Per impostazione predefinita, il valore di questa proprietà è false. Ciò significa che per impostazione predefinita l'utente finale può selezionare più voci dall'elenco.

15. RedirectUrl: proprietà facoltativa di tipo stringa. Usare questa proprietà per specificare una pagina a cui reindirizzare quando si fa clic su una voce con collegamento ipertestuale nell'elenco. Questo URL può contenere segnaposto che vengono sostituiti dal valore effettivo in runtime. I segnaposto sono i seguenti:

     ◦ {0}: objectType

     ◦ {1}: objectID

     ◦ {2}: displayName

16.  ShowTitleBar: proprietà facoltativa di tipo booleano. Usare questa proprietà per specificare se la barra del titolo deve essere visibile. Il valore predefinito di questa proprietà è false.

17.  ShowActionBar: proprietà facoltativa di tipo booleano. Usare questa proprietà per specificare se l'area della barra delle azioni deve essere visibile. Il valore predefinito di questa proprietà è true.

18.  ShowPreview: proprietà facoltativa di tipo booleano. Usare questa proprietà per specificare se l'area di anteprima deve essere visibile. Il valore predefinito di questa proprietà è true.

19.  ShowSearchControl: proprietà facoltativa di tipo booleano. Usare questa proprietà per specificare se il controllo di ricerca deve essere visibile. Il valore predefinito di questa proprietà è true.

20.  ResultObjectType: proprietà facoltativa di tipo stringa. Usare questa proprietà per specificare il tipo di oggetto previsto dei risultati di ricerca. Il valore predefinito di questa proprietà è Resource. Se il risultato di ricerca contiene più tipi di risorsa, questo valore deve essere specificato come Resource.

21.  ColumnsToDisplay: proprietà facoltativa. Usare questa proprietà per specificare gli attributi che si vogliono visualizzare come colonne nella visualizzazione elenco. Il valore predefinito di questa proprietà è DisplayName, ResourceType. Ogni colonna è rappresentata dal nome di sistema di un attributo. Ogni colonna è separata da una virgola (,). Non è necessario specificare un valore per questa proprietà quando la visualizzazione elenco viene usata in modalità di selezione. In questa modalità, l'impostazione della colonna proviene dall'attributo SearchScopeColumn dell'ambito di ricerca selezionato.

22.  ListFilter: proprietà facoltativa di tipo stringa. Espressione XPath usata per il rendering della visualizzazione elenco e attiva solo se la proprietà ShowSearchControl è impostata su false. Se questo valore è specificato, la visualizzazione elenco usa il valore della proprietà per le query e la visualizzazione elenco non è in modalità di selezione. Il filtro può essere associato a un attributo stringa della risorsa, come indicato di seguito:<br/><br/>
     `<my:Property my:Name="ListFilter" my:Value="{Binding Source=object, Path=Filter}"/>` <br/> <br/>
     o essere una stringa contenente alcune variabili di ambiente predefinite, come indicato di seguito: <br/><br/>
     `<my:Property my:Name="ListFilter" my:Value="/Approval[Request=''%ObjectID%'']"/>`

23.  TargetAttribute: proprietà obsoleta. Il valore di questa deve essere il nome di sistema di un attributo multivalore a cui fa riferimento. È consigliabile non usare più questa proprietà. Ad esempio, nella gestione dei gruppi, anziché quanto segue:

  `<my:Property my:Name="TargetAttribute" my:Value="ExplicitMember"/>` <br/>

  è consigliabile usare: `<my:Property my:Name=”ListFilter” my:Value=”/Group[ObjectID=’%ObjectID%’]/ExplicitMember”/>` <br/><br/>
24.  ItemClickBehavior: proprietà facoltativa di tipo stringa. Usare questa proprietà per specificare se si vuole che il clic su una voce della visualizzazione elenco attivi un postback del server o visualizzi una vista dettagliata della voce. Sono supportate due opzioni: ModelessDialog e Server. Il valore predefinito è ModelessDialog.

25.  SearchOnLoad: proprietà facoltativa di tipo booleano che specifica se il controllo visualizzazione elenco deve eseguire una query al caricamento. Questa proprietà è applicabile solo se la visualizzazione elenco è in modalità di selezione. Il valore predefinito di questa proprietà è true. È possibile disattivare tale proprietà se si prevede che l'utente digiti il testo per la ricerca per ottenere un risultato significativo. In questo caso, la visualizzazione elenco inizialmente mostra un messaggio che comunica all'utente come eseguire una ricerca. Il testo può essere personalizzato tramite le proprietà seguenti:

26.  MainSearchScreenText: questa proprietà facoltativa di tipo stringa è applicabile solo se SearchOnload è impostata su true. Questa proprietà può essere usata per personalizzare il testo visualizzato al centro della visualizzazione elenco quando all'interno di questa la ricerca non viene eseguita automaticamente. Il valore predefinito per questa proprietà è Trova le risorse desiderate usando il pulsante Cerca in alto. È possibile specificare un valore per rendere il testo più pertinente al proprio scenario.

27.  SubSearchScreenText: questa proprietà facoltativa di tipo stringa viene usata per personalizzare il testo visualizzato sotto MainSearchScreenText. In genere, è necessario specificare un valore per questa proprietà solo se si vogliono aggiungere altre istruzioni sull'uso della visualizzazione elenco.

Per alcuni esempi di uso della visualizzazione elenco con il controllo UocFilterBuilder come elenco di anteprima, vedere gli esempi relativi a UocFilterBuilder più indietro in questo documento. UocListView può essere usato anche senza il generatore di filtri.

### <a name="uocnumericbox"></a>UocNumericBox

Nome: UocNumericBox

Descrizione: casella di testo semplice che accetta solo Integer. Questo controllo può essere sia in modalità di sola lettura che in modalità aggiornabile.

Proprietà:

1.  Tutte le proprietà comuni: per informazioni su questa proprietà, vedere la sezione Proprietà comuni in questo documento.

2.  MaxValue: proprietà facoltativa di tipo Integer. Usare questa proprietà per definire una convalida lato client per il controllo. Il valore immesso dall'utente finale non può superare questo valore. È possibile immettere un Integer esplicito o associare questa proprietà a dati di tipo Integer di un'origine dati usando {Binding Source=schema Path=IntegerMaximum}.

3.  MinValue: proprietà facoltativa di tipo Integer. Usare questa proprietà per definire una convalida lato client per il controllo. Il valore immesso dall'utente finale non può essere inferiore a questo valore. È possibile immettere un Integer esplicito o associare questa proprietà a dati di tipo Integer di un'origine dati usando {Binding Source=schema, Path=IntegerMinimum}.

4.  DefaultValue: proprietà facoltativa di tipo Integer. Usare questa proprietà per definire un valore predefinito per il controllo se il controllo viene usato per creare nuovi dati. Questo valore può essere impostato in modo esplicito solo su un Integer statico.

5.  Value: proprietà facoltativa di tipo Integer. Se si associa questa proprietà con dati di tipo Integer di un'origine dati, il valore di tale attributo viene visualizzato quando la pagina viene caricata e dopo l'invio viene salvato nell'origine dati.

Gestore:

   • TextChanged: evento generato quando il valore corrente all'interno del controllo cambia.

Esempio:

![](media/rcd-configuration-xml-reference/image061.jpg)

>[!NOTE]
L'esempio di codice seguente genera la prima casella numerica. La casella numerica non è connessa a un'origine dati o a informazioni dello schema.

```
<!--Sample for an explicit Numeric Box-->
<my:Control my:Name="SampleExplicitNumericBox" my:TypeName="UocNumericBox" my:Caption="An Explicit NumericBox" my:Description="This is a dummy numeric box that is not linked with data source.">
     <my:Properties>
          <my:Property my:Name="MinValue" my:Value="1"/>
          <my:Property my:Name="MaxValue" my:Value="100"/>
          <my:Property my:Name="DefaultValue" my:Value="1"/>
     </my:Properties>
</my:Control>
<!--End of sample for an explicit Numeric Box.-->

```

L'esempio di codice seguente genera la seconda casella numerica.

>[!NOTE]
Perché questo esempio funzioni, è necessario prima creare un nuovo attributo di tipo Integer, AnIntegerAttribute, e associarlo al tipo di risorsa personalizzata.

```
<!--Sample for a dynamically rendered numeric box-->
<my:Control my:Name="SampleDynamicNumericBox" my:TypeName="UocNumericBox" my:Caption="{Binding Source=schema, Path=AnIntegerAttribute.DisplayName}" my:Description="{Binding Source=schema, Path=AnIntegerAttribute.Description}" my:RightsLevel="{Binding Source=rights, Path=AnIntegerAttribute}">
     <my:Properties>
          <my:Property my:Name="MaxValue" my:Value="{Binding Source=schema, Path=AnIntegerAttribute.IntegerMaximum}"/>
          <my:Property my:Name="MinValue" my:Value="{Binding Source=schema, Path=AnIntegerAttribute.IntegerMinimum}"/>
          <my:Property my:Name="DefaultValue" my:Value="1"/>
          <my:Property my:Name="Value" my:Value="{Binding Source=object, Path=AnIntegerAttribute, Mode=TwoWay}"/>
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=AnIntegerAttribute.Required}"/>
     </my:Properties>
</my:Control>
<!--End of sample for a dynamically numeric box.-->
```


### <a name="uocpicturebox"></a>UocPictureBox

Nome: UocPictureBox

Descrizione: controllo usato per eseguire il rendering di immagini, dati di tipo binario. È consigliabile usare questo controllo con dati di tipo binario. Il rendering dell'immagine può essere eseguito dall'URL di un'immagine specificato,da dati di tipo binario o dall'origine dell'attributo che contiene dati di tipo di immagine.

Proprietà:

1.  Tutte le proprietà comuni: per informazioni su questa proprietà, vedere la sezione Proprietà comuni in questo documento.

2.  ImageUrl: proprietà facoltativa di tipo stringa. Immettere l'URL dell'immagine di destinazione.

3.  MaxHeight: proprietà facoltativa di tipo stringa. Definisce l'altezza massima in pixel dell'immagine da sottoporre a rendering.

4.  MaxWidth: proprietà facoltativa di tipo stringa. Definisce la larghezza massima in pixel dell'immagine da sottoporre a rendering.

5.  ImageData: proprietà di tipo binario. Usare questa proprietà per associare un'origine dati all'immagine visualizzata. L'origine dati associata deve essere di tipo binario.
    È anche possibile usare questo campo per impostare in modo esplicito un'immagine specificando i dati nel formato byte[].

6.  ImageResource: proprietà facoltativa di tipo binario.

7.  AlternativeText: proprietà facoltativa di tipo stringa. Questa proprietà funge da testo alternativo quando non è possibile visualizzare l'immagine.

Esempio:

>[!NOTE]
Per usare questo esempio, deve esistere un'associazione di dati immagine al controllo.


Il segmento di codice seguente genera un controllo casella di immagine che associa un'origine dati al controllo:

```
<!--Sample for a Picture Box control binding with a data source-->
<my:Control my:Name="SamplePictureBoxImageData" my:TypeName="UocPictureBox" my:RightsLevel="{Binding Source=rights, Path=Photo}">
     <my:Properties>
          <my:Property my:Name="MaxHeight" my:Value="100" />
          <my:Property my:Name="MaxWidth" my:Value="100" />
          <my:Property my:Name="ImageData" my:Value="{Binding Source=object, Path=Photo}" />
     </my:Properties>
</my:Control>
<!--End of Sample for a Picture Box control-->
```

Il segmento di codice seguente genera un controllo casella di immagine che associa un'immagine URL al controllo:

```
<!--Sample for a Picture Box control bind with explicit URL-->
<my:Control my:Name="SamplePictureBoxImageUrl" my:TypeName="UocPictureBox">
     <my:Properties>
          <my:Property my:Name="MaxHeight" my:Value="100" />
          <my:Property my:Name="MaxWidth" my:Value="100" />
          <my:Property my:Name="ImageUrl" my:Value="http://www.microsoft.com/dummypicture.jpg" />
     </my:Properties>
</my:Control>
<!--End of Sample for a Picture Box control-->
```

### <a name="uocradiobuttonlist"></a>UocRadioButtonList

Nome: UocRadioButtonList

Descrizione: elenco semplice di pulsanti di opzione. In questo elenco le opzioni si escludono a vicenda. Questo controllo è consigliato se gli utenti hanno un massimo di cinque opzioni da cui selezionare. In caso contrario, è consigliabile il controllo UOCListView.

Proprietà:

1.  Tutte le proprietà comuni: per informazioni su questa proprietà, vedere la sezione Proprietà comuni in questo documento.

2.  ValuePath: impostato su Value. È associato al campo Value dell'elemento Option definito in questo documento.

3.  CaptionPath: impostato su Caption. È associato al campo Caption dell'elemento Option definito in questo documento.

4.  HintPath: impostato su Hint. È associato al campo Hint dell'elemento Option definito in questo documento.

5.  SelectedValue: valore attualmente selezionato. Questa è una proprietà obbligatoria di tipo stringa. Questa proprietà è associata a dati di tipo stringa dell'origine dati.

Eventi:

1.  SelectedIndexChanged

2.  CheckedChanged

Opzioni:

Per questo controllo possono esistere solo due elementi Option in Options.

1.  Value: il campo Value in un singolo elemento Option deve essere impostato su True o False.

2.  Caption: può essere un qualsiasi valore stringa.

3.  Hint: può essere un qualsiasi valore stringa.

Esempio:

![](media/rcd-configuration-xml-reference/image063.jpg)

>[!NOTE]
Perché questo esempio funzioni, è necessario creare un nuovo attributo booleano, ABooleanAttribute, e associarlo al tipo di risorsa personalizzata.

Il segmento di codice seguente crea l'elenco di pulsanti di opzione dell'immagine precedente:

```
<!--Sample for option button list control-->
<my:Control my:Name="SampleRadioButtonList" my:TypeName="UocRadioButtonList" my:Caption="{Binding Source=schema, Path=ABooleanAttribute.DisplayName}" my:Description="{Binding Source=schema, Path=ABooleanAttribute.Description}" my:RightsLevel="{Binding Source=rights, Path=ABooleanAttribute}">
     <my:Options>
          <my:Option my:Value="False" my:Caption="Set Value To False" my:Hint="By selecting this option, you are setting the value of the attribute to false." />
          <my:Option my:Value="True" my:Caption="Set Value To True" my:Hint="By selecting this option, you are setting the value of the attribute to true." />
     </my:Options>
     <my:Properties>
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=ABooleanAttribute.Required}" />
          <my:Property my:Name="ValuePath" my:Value="Value" />
          <my:Property my:Name="CaptionPath" my:Value="Caption" />
          <my:Property my:Name="HintPath" my:Value="Hint" />
          <my:Property my:Name="SelectedValue" my:Value="{Binding Source=object, Path=ABooleanAttribute, Mode=TwoWay}" />
     </my:Properties>
</my:Control>
<!--End of Sample for option button list control-->
```


### <a name="uocsimpleradiobutton"></a>UocSimpleRadioButton


Nome: UocSimpleRadioButton

Descrizione: controllo semplice di pulsanti di opzione. L'uso di questo controllo è simile a quello di una semplice casella di controllo. Sono presenti due pulsanti di opzione affiancati all'etichetta di testo. È consigliabile associare il controllo a dati di tipo booleano.

Proprietà:

1.  Tutte le proprietà comuni: per informazioni su questa proprietà, vedere la sezione Proprietà comuni in questo documento

2.  TrueText: proprietà facoltativa di tipo stringa. Rappresenta il testo visualizzato quando il pulsante di opzione è selezionato.

3.  FalseText: proprietà facoltativa di tipo stringa. Rappresenta il testo visualizzato quando il pulsante di opzione non è selezionato.

4.  SelectedItem: proprietà facoltativa di tipo booleano. Questo valore indica che il pulsante di opzione è selezionato. Può essere associato a dati di tipo booleano di un'origine dati. Il valore predefinito è impostato su false.

Eventi:

   • CheckedChanged: se lo stato del pulsante di opzione passa da selezionato a non selezionato o viceversa, viene emesso questo segnale.

Esempio:

![](media/rcd-configuration-xml-reference/image066.png)

>[!NOTE]
Perché questo esempio funzioni, è necessario creare un nuovo attributo booleano, ABooleanAttribute, e associarlo al tipo di risorsa personalizzata. I dati RCDC vengono applicati allo stesso tipo di risorsa personalizzata.

Il segmento di codice seguente genera il pulsante di opzione dell'immagine precedente:

```
<!--Sample for simple option button control-->
<my:Control my:Name="SampleSimpleRadioButton" my:TypeName="UocSimpleRadioButton" my:Caption="{Binding Source=schema, Path=ABooleanAttribute.DisplayName}" my:Description="{Binding Source=schema, Path=ABooleanAttribute.Description}" my:RightsLevel="{Binding Source=rights, Path=ABooleanAttribute}">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=ABooleanAttribute.Required}" />
          <my:Property my:Name="FalseText" my:Value="False"/>
          <my:Property my:Name="TrueText" my:Value="True"/>
          <my:Property my:Name="SelectedItem" my:Value="{Binding Source=object, Path=ABooleanAttribute, Mode=TwoWay}" />
     </my:Properties>
</my:Control>
<!--End of Sample for simple option button control-->
```


### <a name="uoctextbox"></a>UocTextBox

Nome: UocTextBox

Descrizione: casella di testo semplice che supporta input di tipo stringa. Per usare questo controllo è consigliabile associarlo a dati di tipo stringa.

Proprietà:

1.  Tutte le proprietà comuni: per informazioni su questa proprietà, vedere la sezione Proprietà comuni in questo documento.

2.  MaxLength: attributo facoltativo di tipo Integer. Questa proprietà specifica la lunghezza massima di una stringa di input. Il valore predefinito di questa proprietà è 128 caratteri.

3.  Text: proprietà facoltativa di tipo stringa. Corrisponde al testo visualizzato nella casella di testo. È possibile definire una stringa esplicita da visualizzare nella casella di testo durante il caricamento iniziale del controllo o associare il controllo stesso a un attributo dello schema di tipo stringa.

4.  Rows: proprietà facoltativa di tipo Integer. Questa proprietà definisce l'altezza della casella di testo in unità carattere. Il valore predefinito è 1 carattere.

5.  Columns: proprietà facoltativa di tipo Integer. Questa proprietà definisce la larghezza della casella di testo in unità carattere. Il valore predefinito è 20 caratteri.

6.  Wrap: proprietà facoltativa di tipo booleano. Se si imposta il valore di questa proprietà su true, si abilita la funzionalità di ritorno a capo automatico nella casella di testo. Il valore predefinito di questa proprietà è true.

7.  UniquenessValidationXPath: proprietà facoltativa di tipo stringa. Accetta un'espressione di filtro XPath FIM valida e assicura che il valore di input immesso dall'utente sia univoco tra le risorse nell'ambito del filtro.
    Ad esempio, per garantire che il nome visualizzato richiesto dall'utente sia univoco tra tutti i gruppi di sicurezza abilitati per la posta nel database del servizio FIM, usare l'espressione XPath `/Group[DisplayName=’%VALUE%’ and Type=’MailEnabledSecurity’`. L'azione di convalida viene eseguita quando si esce dalla pagina. Questa proprietà è supportata solo nella RCDC per la creazione di una risorsa.

8.  UniquenessErrorMessage: proprietà facoltativa di tipo stringa. Questa stringa viene usata per visualizzare un messaggio di errore se la convalida di UniquenessValidationXPath ha esito negativo e può essere costituita da testo esplicito o da una variabile di risorsa stringa. Se questa proprietà non è specificata, il messaggio di errore predefinito per una convalida non riuscita è "%VALUE% già esistente. Provare un valore diverso".

Eventi:

   • TextChanged: evento generato quando il testo all'interno della casella di testo cambia.

Per un esempio completo di questo controllo, vedere la sezione Esempi di controlli semplici.

## <a name="appendix-a-default-xsd-schema"></a>Appendice A: Schema XSD predefinito

Di seguito è riportato lo schema XSD completo per tutte le RCDC predefinite fornite con Microsoft Identity Manager 2016 SP1:

```XML
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<xsd:schema targetNamespace="http://schemas.microsoft.com/2006/11/ResourceManagement" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:my="http://schemas.microsoft.com/2006/11/ResourceManagement" xmlns:xd="http://schemas.microsoft.com/office/infopath/2003" xmlns:xsd="http://www.w3.org/2001/XMLSchema">
    <xsd:attribute name="TypeName" type="my:requiredString"/>
    <xsd:attribute name="Name" type="my:requiredAlphanumericString"/>
    <xsd:attribute name="Parameters" type="xsd:string"/>
    <xsd:attribute name="DisplayAsWizard" type="xsd:boolean"/>
    <xsd:attribute name="Caption" type="xsd:string"/>
    <xsd:attribute name="AutoValidate" type="xsd:boolean"/>
    <xsd:attribute name="Enabled" type="xsd:string"/>
    <xsd:attribute name="Visible" type="xsd:string"/>
    <xsd:attribute name="IsSummary" type="xsd:boolean"/>
    <xsd:attribute name="IsHeader" type="xsd:boolean"/>
    <xsd:attribute name="HelpText" type="xsd:string"/>
    <xsd:attribute name="Link" type="xsd:string"/>
    <xsd:attribute name="Description" type="xsd:string"/>
    <xsd:attribute name="ExpandArea" type="xsd:boolean"/>
    <xsd:attribute name="Hint" type="xsd:string"/>
    <xsd:attribute name="AutoPostback" type="xsd:string"/>
    <xsd:attribute name="RightsLevel" type="my:requiredString"/>
    <xsd:attribute name="Value" type="xsd:string"/>
    <xsd:attribute name="Handler" type="my:requiredString"/>
    <xsd:attribute name="ImageUrl" type="xsd:string"/>
    <xsd:attribute name="RedirectUrl" type="xsd:string"/>
    <xsd:attribute name="ClickBehavior" type="xsd:string"/>
    <xsd:attribute name="EnableMode" type="xsd:string"/>
    <xsd:attribute name="ValueType" type="xsd:string"/>
    <xsd:attribute name="Condition" type="xsd:string"/>
    <xsd:element name="ObjectControlConfiguration">
        <xsd:complexType>
            <xsd:sequence>
                <xsd:element ref="my:ObjectDataSource" minOccurs="0" maxOccurs="32"/>
                <xsd:element ref="my:XmlDataSource" minOccurs="0" maxOccurs="32"/>
                <xsd:element ref="my:Panel"/>
                <xsd:element ref="my:Events" minOccurs="0" maxOccurs="1"/>
            </xsd:sequence>
            <xsd:attribute ref="my:TypeName"/>
            <xsd:anyAttribute processContents="lax" namespace="http://www.w3.org/XML/1998/namespace"/>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="ObjectDataSource">
        <xsd:complexType>
            <xsd:sequence/>
            <xsd:attribute ref="my:TypeName"/>
            <xsd:attribute ref="my:Name"/>
            <xsd:attribute ref="my:Parameters"/>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="XmlDataSource">
        <xsd:complexType  mixed="true">
      <xsd:sequence>
        <xsd:any minOccurs="0" maxOccurs="unbounded" processContents="lax"/>
      </xsd:sequence>
      <xsd:attribute ref="my:Name"/>
      <xsd:attribute ref="my:Parameters"/>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Panel">
        <xsd:complexType>
            <xsd:sequence>
                <xsd:element ref="my:Grouping" minOccurs="1" maxOccurs="16"/>
            </xsd:sequence>
            <xsd:attribute ref="my:Name"/>
            <xsd:attribute ref="my:DisplayAsWizard"/>
            <xsd:attribute ref="my:Caption"/>
            <xsd:attribute ref="my:AutoValidate"/>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Grouping">
        <xsd:complexType>
            <xsd:sequence>
                <xsd:element ref="my:Help" minOccurs="0"  maxOccurs="1"/>
                <xsd:element ref="my:Control" minOccurs="1" maxOccurs="256"/>
                <xsd:element ref="my:Events" minOccurs="0" maxOccurs="1"/>
            </xsd:sequence>
            <xsd:attribute ref="my:Name"/>
            <xsd:attribute ref="my:Caption"/>
            <xsd:attribute ref="my:Description"/>
            <xsd:attribute ref="my:Enabled"/>
            <xsd:attribute ref="my:Visible"/>
            <xsd:attribute ref="my:IsHeader"/>
            <xsd:attribute ref="my:IsSummary"/>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Help">
        <xsd:complexType>
            <xsd:sequence/>
            <xsd:attribute ref="my:HelpText"/>
            <xsd:attribute ref="my:Link"/>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Control">
        <xsd:complexType>
            <xsd:sequence>
                <xsd:element ref="my:Help" minOccurs="0"  maxOccurs="1"/>
                <xsd:element ref="my:CustomProperties" minOccurs="0"  maxOccurs="1"/>
                <xsd:element ref="my:Options" minOccurs="0"  maxOccurs="1"/>
                <xsd:element ref="my:Buttons" minOccurs="0"  maxOccurs="1"/>
                <xsd:element ref="my:Properties" minOccurs="0"  maxOccurs="1"/>
                <xsd:element ref="my:Events" minOccurs="0" maxOccurs="1"/>
            </xsd:sequence>
            <xsd:attribute ref="my:Name"/>
            <xsd:attribute ref="my:TypeName"/>
            <xsd:attribute ref="my:Caption"/>
            <xsd:attribute ref="my:Enabled"/>
            <xsd:attribute ref="my:Visible"/>
            <xsd:attribute ref="my:Description"/>
            <xsd:attribute ref="my:ExpandArea"/>
            <xsd:attribute ref="my:Hint"/>
            <xsd:attribute ref="my:AutoPostback"/>
            <xsd:attribute ref="my:RightsLevel"/>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="CustomProperties">
        <xsd:complexType mixed="true">
            <xsd:sequence>
                <xsd:any minOccurs="0" maxOccurs="unbounded" namespace="##targetNamespace" processContents="lax"/>
            </xsd:sequence>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Options">
        <xsd:complexType>
            <xsd:sequence>
                <xsd:element ref="my:Option" minOccurs="0" maxOccurs="unbounded"/>
            </xsd:sequence>
            <xsd:attribute ref="my:ValueType"/>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Option">
        <xsd:complexType>
            <xsd:simpleContent>
                <xsd:extension base="xsd:string">
                    <xsd:attribute ref="my:Value"/>
                    <xsd:attribute ref="my:Caption"/>
                    <xsd:attribute ref="my:Hint"/>
                </xsd:extension>
            </xsd:simpleContent>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Buttons">
        <xsd:complexType>
            <xsd:sequence>
                <xsd:element ref="my:Button" minOccurs="1" maxOccurs="8"/>
            </xsd:sequence>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Button">
        <xsd:complexType>
            <xsd:simpleContent>
                <xsd:extension base="xsd:string">
                    <xsd:attribute ref="my:Name"/>
                    <xsd:attribute ref="my:Caption"/>
                    <xsd:attribute ref="my:Hint"/>
                    <xsd:attribute ref="my:ImageUrl"/>
                    <xsd:attribute ref="my:ClickBehavior"/>
                    <xsd:attribute ref="my:RedirectUrl"/>
                    <xsd:attribute ref="my:Enabled"/>
                    <xsd:attribute ref="my:Visible"/>
                    <xsd:attribute ref="my:EnableMode"/>
                </xsd:extension>
            </xsd:simpleContent>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Properties">
        <xsd:complexType>
            <xsd:sequence>
                <xsd:element ref="my:Property" minOccurs="1" maxOccurs="32"/>
            </xsd:sequence>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Property">
        <xsd:complexType>
            <xsd:simpleContent>
                <xsd:extension base="xsd:string">
                    <xsd:attribute ref="my:Name"/>
                    <xsd:attribute ref="my:Value"/>
                    <xsd:attribute ref="my:Condition"/>                 
                </xsd:extension>
            </xsd:simpleContent>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Events">
        <xsd:complexType>
            <xsd:sequence>
                <xsd:element ref="my:Event" minOccurs="1" maxOccurs="16"/>
            </xsd:sequence>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Event">
        <xsd:complexType>
            <xsd:simpleContent>
                <xsd:extension base="xsd:string">
                    <xsd:attribute ref="my:Name"/>
                    <xsd:attribute ref="my:Handler"/>
          <xsd:attribute ref="my:Parameters"/>
        </xsd:extension>
            </xsd:simpleContent>
        </xsd:complexType>
    </xsd:element>
    <xsd:simpleType name="requiredString">
        <xsd:restriction base="xsd:string">
            <xsd:minLength value="1"/>
        </xsd:restriction>
    </xsd:simpleType>
  <xsd:simpleType name="requiredAlphanumericString">
    <xsd:restriction base="xsd:string">
      <xsd:pattern value="[A-Za-z0-9_]{1,128}"/>
    </xsd:restriction>
  </xsd:simpleType>
    <xsd:simpleType name="requiredAnyURI">
        <xsd:restriction base="xsd:anyURI">
            <xsd:minLength value="1"/>
        </xsd:restriction>
    </xsd:simpleType>
    <xsd:simpleType name="requiredBase64Binary">
        <xsd:restriction base="xsd:base64Binary">
            <xsd:minLength value="1"/>
        </xsd:restriction>
    </xsd:simpleType>
</xsd:schema>
```



## <a name="appendix-b-default-summary-xsl"></a>Appendice B: XSL di riepilogo predefinito

Di seguito è riportato l'XSL di riepilogo completo fornito con Microsoft Identity Manager 2016 SP1:
```XML
<xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform" xmlns:msxsl="urn:schemas-microsoft-com:xslt">
  <xsl:template name="output-attribute-value">
    <xsl:param name="attribute"/>
    <xsl:param name="type"/>
    <xsl:choose>
      <xsl:when test="$type='Binary'">
        <xsl:value-of select="$attribute" disable-output-escaping="yes"/>
      </xsl:when>
      <xsl:when test="$type='Text'">
        <xsl:text xml:space="preserve" disable-output-escaping="yes">(text data)</xsl:text>
      </xsl:when>
      <xsl:otherwise>
        <xsl:value-of select="translate($attribute,' ','&#160;')" disable-output-escaping="yes"/>
      </xsl:otherwise>
    </xsl:choose>
  </xsl:template>

  <xsl:template name="output-modified-value">
    <xsl:param name="name"/>
    <xsl:param name="attribute1"/>
    <xsl:param name="text1"/>
    <xsl:param name="attribute2"/>
    <xsl:param name="text2"/>
    <xsl:param name="type"/>
    <tr class="listViewRow" style="height:22px;">
      <xsl:if test="position() mod 2 != 0">
        <td class="commonSummaryListViewCellBR ms-vb">
          <xsl:value-of select="$name" disable-output-escaping="yes"/>
        </td>
        <xsl:choose>
          <xsl:when test="$attribute1 and $attribute1!=''">
            <td class="commonSummaryListViewCellBR ms-vb">
              <xsl:call-template name="output-attribute-value">
                <xsl:with-param name="attribute" select="$attribute1"/>
                <xsl:with-param name="type" select="$type"/>
              </xsl:call-template>
            </td>
          </xsl:when>
          <xsl:otherwise>
            <td class="commonSummaryListViewCellBR ms-vb">
              <xsl:copy-of select="$text1"/>
            </td>
          </xsl:otherwise>
        </xsl:choose>
        <xsl:choose>
          <xsl:when test="$attribute2 and $attribute2!=''">
            <td class="commonSummaryListViewCellBR ms-vb">
              <xsl:call-template name="output-attribute-value">
                <xsl:with-param name="attribute" select="$attribute2"/>
                <xsl:with-param name="type" select="$type"/>
              </xsl:call-template>
            </td>
          </xsl:when>
          <xsl:otherwise>
            <td class="commonSummaryListViewCellBR ms-vb">
              <xsl:copy-of select="$text2"/>
            </td>
          </xsl:otherwise>
        </xsl:choose>
      </xsl:if>
      <xsl:if test="position() mod 2 != 1">
        <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
          <xsl:value-of select="$name" disable-output-escaping="yes"/>
        </td>
        <xsl:choose>
          <xsl:when test="$attribute1 and $attribute1!=''">
            <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
              <xsl:call-template name="output-attribute-value">
                <xsl:with-param name="attribute" select="$attribute1"/>
                <xsl:with-param name="type" select="$type"/>
              </xsl:call-template>
            </td>
          </xsl:when>
          <xsl:otherwise>
            <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
              <xsl:copy-of select="$text1"/>
            </td>
          </xsl:otherwise>
        </xsl:choose>
        <xsl:choose>
          <xsl:when test="$attribute2 and $attribute2!=''">
            <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
              <xsl:call-template name="output-attribute-value">
                <xsl:with-param name="attribute" select="$attribute2"/>
                <xsl:with-param name="type" select="$type"/>
              </xsl:call-template>
            </td>
          </xsl:when>
          <xsl:otherwise>
            <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
              <xsl:copy-of select="$text2"/>
            </td>
          </xsl:otherwise>
        </xsl:choose>
      </xsl:if>
    </tr>
  </xsl:template>

  <xsl:template name="output-localized-attribute-value">
    <xsl:param name="locale"/>
    <xsl:param name="attribute"/>
    <xsl:param name="type"/>
    <tr class="listViewRow" style="height:22px;">
      <xsl:if test="position() mod 2 != 0">
        <td class="commonSummaryListViewCellBR ms-vb">
          <xsl:value-of select="$locale" disable-output-escaping="yes"/>
        </td>
        <td class="commonSummaryListViewCellBR ms-vb">
          <xsl:call-template name="output-attribute-value">
            <xsl:with-param name="attribute" select="$attribute"/>
            <xsl:with-param name="type" select="$type"/>
          </xsl:call-template>
        </td>
      </xsl:if>
      <xsl:if test="position() mod 2 != 1">
        <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
          <xsl:value-of select="$locale" disable-output-escaping="yes"/>
        </td>
        <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
          <xsl:call-template name="output-attribute-value">
            <xsl:with-param name="attribute" select="$attribute"/>
            <xsl:with-param name="type" select="$type"/>
          </xsl:call-template>
        </td>
      </xsl:if>
    </tr>
  </xsl:template>

  <xsl:template match="/">
    <xsl:choose>
      <xsl:when test="ModifiedAttributes[@ActionType='Create']">
        <!-- expected XML
        <ModifiedAttributes ActionType="Create">
          <Attribute Name="[attribute's system name]" DisplayName="[attribute's display name]" DataType="[all kinds of ILM data type]" InitializedValue="[the value]"/>
          other <Attribute> elements
        </ModifiedAttributes>
        -->
        <table cellspacing="0" cellpadding="3" class="commonSummaryListViewGridBorder">
          <tr align="left" class="listViewHeader" style="height:22px;">
            <th class="commonSummaryListViewHeaderCellBR">Attribute</th>
            <th class="commonSummaryListViewHeaderCellBR">Value</th>
          </tr>
          <xsl:for-each select="ModifiedAttributes/Attribute">
            <xsl:sort select="@DisplayName" order="ascending"/>
            <tr class="listViewRow" style="height:22px;">
              <xsl:if test="position() mod 2 != 0">
                <td class="commonSummaryListViewCellBR ms-vb">
                  <xsl:value-of select="@DisplayName" disable-output-escaping="yes"/>
                </td>
                <xsl:if test="count(LocalizedValue)!=0">
                  <td class="commonSummaryListViewCellBR ms-vb">
                    <table cellspacing="0" style="width:100%">
                      <tr class="listViewHeader">
                        <th align="left" class="commonSummaryListViewHeaderCellBR">Language</th>
                        <th align="left" class="commonSummaryListViewHeaderCellBR">Status</th>
                      </tr>
                      <xsl:if test="@InitializedValue and @InitializedValue != ''">
                        <xsl:call-template name="output-localized-attribute-value">
                          <xsl:with-param name="locale" select="@Locale"/>
                          <xsl:with-param name="attribute" select="@InitializedValue"/>
                          <xsl:with-param name="type" select="@DataType"/>
                        </xsl:call-template>
                      </xsl:if>
                      <xsl:for-each select="LocalizedValue">
                        <xsl:call-template name="output-localized-attribute-value">
                          <xsl:with-param name="locale" select="@Locale"/>
                          <xsl:with-param name="attribute" select="@InitializedValue"/>
                          <xsl:with-param name="type" select="../@DataType"/>
                        </xsl:call-template>
                      </xsl:for-each>
                    </table>
                  </td>
                </xsl:if>
                <xsl:if test="count(LocalizedValue)=0">
                  <td class="commonSummaryListViewCellBR ms-vb">
                    <xsl:call-template name="output-attribute-value">
                      <xsl:with-param name="attribute" select="@InitializedValue"/>
                      <xsl:with-param name="type" select="@DataType"/>
                    </xsl:call-template>
                  </td>
                </xsl:if>
              </xsl:if>
              <xsl:if test="position() mod 2 != 1">
                <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
                  <xsl:value-of select="@DisplayName" disable-output-escaping="yes"/>
                </td>
                <xsl:if test="count(LocalizedValue)!=0">
                  <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
                    <table cellspacing="0" style="width:100%">
                      <tr class="listViewHeader">
                        <th align="left" class="commonSummaryListViewHeaderCellBR">Language</th>
                        <th align="left" class="commonSummaryListViewHeaderCellBR">Status</th>
                      </tr>
                      <xsl:if test="@InitializedValue and @InitializedValue != ''">
                        <xsl:call-template name="output-localized-attribute-value">
                          <xsl:with-param name="locale" select="@Locale"/>
                          <xsl:with-param name="attribute" select="@InitializedValue"/>
                          <xsl:with-param name="type" select="@DataType"/>
                        </xsl:call-template>
                      </xsl:if>
                      <xsl:for-each select="LocalizedValue">
                        <xsl:call-template name="output-localized-attribute-value">
                          <xsl:with-param name="locale" select="@Locale"/>
                          <xsl:with-param name="attribute" select="@InitializedValue"/>
                          <xsl:with-param name="type" select="../@DataType"/>
                        </xsl:call-template>
                      </xsl:for-each>
                    </table>
                  </td>
                </xsl:if>
                <xsl:if test="count(LocalizedValue)=0">
                  <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
                    <xsl:call-template name="output-attribute-value">
                      <xsl:with-param name="attribute" select="@InitializedValue"/>
                      <xsl:with-param name="type" select="@DataType"/>
                    </xsl:call-template>
                  </td>
                </xsl:if>
              </xsl:if>
            </tr>
          </xsl:for-each>
        </table>
      </xsl:when>
      <xsl:when test="ModifiedAttributes[@ActionType='Modify']">
        <!-- expected XML
        <ModifiedAttributes ActionType="Modify">
          <SingleAttribute Name="[attribute's system name]" DisplayName="[attribute's display name]" DataType="[all kinds of ILM data type]" InitializedValue="[the old value]" SetValue="[the new value]"/>
          other <SingleAttribute> elements
          <MultipleAttribute Name="[attribute's system name]" DisplayName="[attribute's display name]" DataType="[all kinds of ILM data type]" InsertedItem="[inserted items separated by ';']" RemovedItem="[removed items separated by ';']"/>
          other <MultipleAttribute> elements
        </ModifiedAttributes>
        -->
        <table class="commonSummaryListViewGridBorder" cellspacing="0" cellpadding="3">
          <xsl:if test="ModifiedAttributes[count(SingleAttribute)!=0]">
            <tr align="left" class="listViewHeader">
              <th class="commonSummaryListViewHeaderCellBR">Single-Value Attributes</th>
              <th class="commonSummaryListViewHeaderCellBR">Old Value</th>
              <th class="commonSummaryListViewHeaderCellBR">New Value</th>
            </tr>
            <xsl:for-each select="ModifiedAttributes/SingleAttribute">
              <xsl:sort select="@DisplayName" order="ascending"/>
              <xsl:if test="count(LocalizedValue)!=0">
                <tr class="listViewRow">
                  <xsl:if test="position() mod 2 != 0">
                    <td class="commonSummaryListViewCellBR ms-vb">
                      <xsl:value-of select="@DisplayName" disable-output-escaping="yes"/>
                    </td>
                    <td colSpan="2">
                      <table cellspacing="0" cellpadding="0" style="width:100%">
                        <tr align="left" class="listViewHeader">
                          <th class="commonSummaryListViewHeaderCellBR">Language</th>
                          <th class="commonSummaryListViewHeaderCellBR">Old Value</th>
                          <th class="commonSummaryListViewHeaderCellBR">New Value</th>
                        </tr>
                        <xsl:if test="(@InitializedValue and @InitializedValue !='') or (@SetValue and @SetValue != '')">
                          <xsl:call-template name="output-modified-value">
                            <xsl:with-param name="name" select="@Locale"/>
                            <xsl:with-param name="attribute1" select="@InitializedValue"/>
                            <xsl:with-param name="text1">(no initial value)</xsl:with-param>
                            <xsl:with-param name="attribute2" select="@SetValue"/>
                            <xsl:with-param name="text2">(value removed)</xsl:with-param>
                            <xsl:with-param name="type" select="@DataType"/>
                          </xsl:call-template>
                        </xsl:if>
                        <xsl:for-each select="LocalizedValue">
                          <xsl:call-template name="output-modified-value">
                            <xsl:with-param name="name" select="@Locale"/>
                            <xsl:with-param name="attribute1" select="@InitializedValue"/>
                            <xsl:with-param name="text1">(no initial value)</xsl:with-param>
                            <xsl:with-param name="attribute2" select="@SetValue"/>
                            <xsl:with-param name="text2">(value removed)</xsl:with-param>
                            <xsl:with-param name="type" select="../@DataType"/>
                          </xsl:call-template>
                        </xsl:for-each>
                      </table>
                    </td>
                  </xsl:if>
                  <xsl:if test="position() mod 2 != 1">
                    <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
                      <xsl:value-of select="@DisplayName" disable-output-escaping="yes"/>
                    </td>
                    <td colSpan="2">
                      <table cellspacing="0" style="width:100%">
                        <tr align="left" class="listViewHeader">
                          <th class="commonSummaryListViewHeaderCellBR">Language</th>
                          <th class="commonSummaryListViewHeaderCellBR">Old Value</th>
                          <th class="commonSummaryListViewHeaderCellBR">New Value</th>
                        </tr>
                        <xsl:if test="(@InitializedValue and @InitializedValue !='') or (@SetValue and @SetValue != '')">
                          <xsl:call-template name="output-modified-value">
                            <xsl:with-param name="name" select="@Locale"/>
                            <xsl:with-param name="attribute1" select="@InitializedValue"/>
                            <xsl:with-param name="text1">(no initial value)</xsl:with-param>
                            <xsl:with-param name="attribute2" select="@SetValue"/>
                            <xsl:with-param name="text2">(value removed)</xsl:with-param>
                            <xsl:with-param name="type" select="@DataType"/>
                          </xsl:call-template>
                        </xsl:if>
                        <xsl:for-each select="LocalizedValue">
                          <xsl:call-template name="output-modified-value">
                            <xsl:with-param name="name" select="@Locale"/>
                            <xsl:with-param name="attribute1" select="@InitializedValue"/>
                            <xsl:with-param name="text1">(no initial value)</xsl:with-param>
                            <xsl:with-param name="attribute2" select="@SetValue"/>
                            <xsl:with-param name="text2">(value removed)</xsl:with-param>
                            <xsl:with-param name="type" select="../@DataType"/>
                          </xsl:call-template>
                        </xsl:for-each>
                      </table>
                    </td>
                  </xsl:if>
                </tr>
              </xsl:if>
              <xsl:if test="count(LocalizedValue)=0">
                <xsl:call-template name="output-modified-value">
                  <xsl:with-param name="name" select="@DisplayName"/>
                  <xsl:with-param name="attribute1" select="@InitializedValue"/>
                  <xsl:with-param name="text1">(no initial value)</xsl:with-param>
                  <xsl:with-param name="attribute2" select="@SetValue"/>
                  <xsl:with-param name="text2">(value removed)</xsl:with-param>
                  <xsl:with-param name="type" select="@DataType"/>
                </xsl:call-template>
              </xsl:if>
            </xsl:for-each>
          </xsl:if>
          <xsl:if test="ModifiedAttributes[count(MultipleAttribute)!=0]">
            <tr align="left" class="listViewHeader">
              <th class="commonSummaryListViewHeaderCellBR">Multiple-Value Attributes</th>
              <th class="commonSummaryListViewHeaderCellBR">Removed Items</th>
              <th class="commonSummaryListViewHeaderCellBR">Inserted Items</th>
            </tr>
            <xsl:for-each select="ModifiedAttributes/MultipleAttribute">
              <xsl:sort select="@DisplayName" order="ascending"/>
              <xsl:if test="count(LocalizedValue)!=0">
                <tr class="uocSummaryTitleTR">
                  <xsl:if test="position() mod 2 != 0">
                    <td class="commonSummaryListViewCellBR ms-vb">
                      <xsl:value-of select="@DisplayName" disable-output-escaping="yes"/>
                    </td>
                    <td>
                      <table cellspacing="0" style="width:100%">
                        <tr align="left" class="listViewHeader">
                          <th class="commonSummaryListViewHeaderCellBR">Language</th>
                          <th class="commonSummaryListViewHeaderCellBR">Removed Items</th>
                          <th class="commonSummaryListViewHeaderCellBR">Inserted Items</th>
                        </tr>
                        <xsl:if test="(@RemovedItem and @RemovedItem!='') or (@InsertedItem and @InsertedItem!='')">
                          <xsl:call-template name="output-modified-value">
                            <xsl:with-param name="name" select="@Locale"/>
                            <xsl:with-param name="attribute1" select="@RemovedItem"/>
                            <xsl:with-param name="text1">(no removed item)</xsl:with-param>
                            <xsl:with-param name="attribute2" select="@InsertedItem"/>
                            <xsl:with-param name="text2">(no inserted item)</xsl:with-param>
                            <xsl:with-param name="type" select="@DataType"/>
                          </xsl:call-template>
                        </xsl:if>
                        <xsl:for-each select="LocalizedValue">
                          <xsl:call-template name="output-modified-value">
                            <xsl:with-param name="name" select="@Locale"/>
                            <xsl:with-param name="attribute1" select="@RemovedItem"/>
                            <xsl:with-param name="text1">(no removed item)</xsl:with-param>
                            <xsl:with-param name="attribute2" select="@InsertedItem"/>
                            <xsl:with-param name="text2">(no inserted item)</xsl:with-param>
                            <xsl:with-param name="type" select="@DataType"/>
                          </xsl:call-template>
                        </xsl:for-each>
                      </table>
                    </td>
                  </xsl:if>
                  <xsl:if test="position() mod 2 != 1">
                    <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
                      <xsl:value-of select="@DisplayName" disable-output-escaping="yes"/>
                    </td>
                    <td>
                      <table cellspacing="0" style="width:100%">
                        <tr align="left" class="listViewHeader">
                          <th class="commonSummaryListViewHeaderCellBR">Language</th>
                          <th class="commonSummaryListViewHeaderCellBR">Removed Items</th>
                          <th class="commonSummaryListViewHeaderCellBR">Inserted Items</th>
                        </tr>
                        <xsl:if test="(@RemovedItem and @RemovedItem!='') or (@InsertedItem and @InsertedItem!='')">
                          <xsl:call-template name="output-modified-value">
                            <xsl:with-param name="name" select="@Locale"/>
                            <xsl:with-param name="attribute1" select="@RemovedItem"/>
                            <xsl:with-param name="text1">(no removed item)</xsl:with-param>
                            <xsl:with-param name="attribute2" select="@InsertedItem"/>
                            <xsl:with-param name="text2">(no inserted item)</xsl:with-param>
                            <xsl:with-param name="type" select="@DataType"/>
                          </xsl:call-template>
                        </xsl:if>
                        <xsl:for-each select="LocalizedValue">
                          <xsl:call-template name="output-modified-value">
                            <xsl:with-param name="name" select="@Locale"/>
                            <xsl:with-param name="attribute1" select="@RemovedItem"/>
                            <xsl:with-param name="text1">(no removed item)</xsl:with-param>
                            <xsl:with-param name="attribute2" select="@InsertedItem"/>
                            <xsl:with-param name="text2">(no inserted item)</xsl:with-param>
                            <xsl:with-param name="type" select="@DataType"/>
                          </xsl:call-template>
                        </xsl:for-each>
                      </table>
                    </td>
                  </xsl:if>
                </tr>
              </xsl:if>
              <xsl:if test="count(LocalizedValue)=0">
                <xsl:call-template name="output-modified-value">
                  <xsl:with-param name="name" select="@DisplayName"/>
                  <xsl:with-param name="attribute1" select="@RemovedItem"/>
                  <xsl:with-param name="text1">(no removed item)</xsl:with-param>
                  <xsl:with-param name="attribute2" select="@InsertedItem"/>
                  <xsl:with-param name="text2">(no inserted item)</xsl:with-param>
                  <xsl:with-param name="type" select="@DataType"/>
                </xsl:call-template>
              </xsl:if>
            </xsl:for-each>
          </xsl:if>
        </table>
      </xsl:when>
    </xsl:choose>
  </xsl:template>
</xsl:stylesheet>
```
