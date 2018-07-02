---
title: Connettori supportati | Documentazione Microsoft
description: Usare i connettori per gestire il trasferimento dei dati tra MIM e le origini dati connesse.
keywords: ''
author: fimguy
ms.author: fimguy
manager: bhu
ms.date: 1/24/2018
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 8bc2f6d2-9f53-4db6-aee6-a937ae468163
ms.reviewer: ''
ms.suite: ems
ms.openlocfilehash: 7b685ffb6f2a52bd2782395e4c1f26501ffe3101
ms.sourcegitcommit: c773edc8262b38df50d82dae0f026bb49500d0a4
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/05/2018
ms.locfileid: "34479347"
---
# <a name="connect-to-your-directories"></a>Connettersi alle directory

I connettori collegano le origini dati connesse specificate a Microsoft Identity Manager SP1 (MIM). Un connettore sposta i dati da un'origine dati connessa a MIM. Grazie al connettore è anche possibile esportare i dati all'origine dati connessa perché siano sincronizzati con MIM quando vengono modificati in MIM. Esiste in genere almeno un connettore per ogni directory connessa.

In Forefront Identity Manager i connettori erano detti agenti di gestione. Questa definizione è ancora usata in alcuni articoli o in parti del prodotto. È tuttavia importante sapere che entrambi i termini si riferiscono allo stesso concetto.

In questo articolo sono illustrati i connettori inclusi e supportati in MIM. Il connettore per Extensible Connectivity 2.0 consente comunque di connettersi anche a più origini dati. Alcuni partner hanno creato connettori personalizzati. Nel wiki [FIM 2010: Management Agents from Partners](http://social.technet.microsoft.com/wiki/contents/articles/1589.fim-2010-management-agents-from-partners.aspx) (FIM 2010: Agenti di gestione dei partner) è disponibile un elenco completo.

## <a name="supported-connectors-in-mim-2016-sp1"></a>Connettori supportati in MIM 2016 SP1

| Name | Versioni supportate dell'origine dati connessa e collegamenti tecnici |
| ---- | ----------------------------------------------- |
| Servizi di dominio di Active Directory | Active Directory 2012, 2016 |
| Active Directory Lightweight Directory Services (ADLDS) | Active Directory Lightweight Directory Services (ADLDS) |
| Elenco Indirizzi Globale (EIG) di Active Directory | Elenco indirizzi globale (GAL, Global Address List) di Active Directory - Exchange 2013, 2016 |
| Extensible Connectivity 2.0 | Qualsiasi origine dati basata su chiamata o file |
| Servizio FIM | La versione dell'agente di gestione del servizio FIM (servizio di sincronizzazione) deve essere la stessa del Servizio Forefront Identity Manager installato |
| Database IBM DB2 Universal | IBM DB2 versione 9.5 o 9.7; IBM DB2 OLEDB v9.5 FP5 o v9.7 FP1 |
| IBM Directory Server | IBM Tivoli Directory Server 6.x |
| Novell eDirectory | Novell eDirectory versioni 8.7.3, 8.8.5 e 8.8.6 |
| Database Oracle | Oracle Database 10g o 11g; client a 64 bit |
| Microsoft SQL Server | SQL Server 2012, 2014, 2016 |
| Server di directory Oracle (precedentemente Sun e Netscape) | Server di directory Sun 6.x, 7.x e Oracle 11 |
| [Connettore di Windows PowerShell per FIM 2010 R2](https://msdn.microsoft.com/library/dn640417.aspx) | Windows PowerShell 2.0 o versioni successive |
| [Connettore di Microsoft Azure Active Directory per FIM 2010 R2](https://msdn.microsoft.com/library/dn511001.aspx) | Microsoft Azure Active Directory |
| [Connettore LDAP generico per FIM 2010 R2](https://msdn.microsoft.com/library/dn510997.aspx) | [Server LDAP v3 (compatibile con RFC 4510)](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-connector-genericldap) |
| [Connettore SQL generico per FIM 2010 R2 / MIM](https://msdn.microsoft.com/library/dn510997.aspx) | [Il connettore è supportato con tutti i driver ODBC a 64 bit](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-connector-genericsql) |
| [Connettore per Lotus Domino](https://msdn.microsoft.com/library/hh859750.aspx) | Lotus Notes versione v8.5.x |
| [Connettore di SharePoint Services con Applicazione servizio profili utente ](https://msdn.microsoft.com/library/dn511003.aspx) | SharePoint Server 2013 o 2016 con Applicazione servizio profili utente |
| [Connettore per servizi Web](https://www.microsoft.com/en-us/download/details.aspx?id=51495) | [SAP ECC 5.0 o 6.0; Oracle PeopleSoft 9.1; Oracle eBusiness 12.1](https://docs.microsoft.com/microsoft-identity-manager/reference/microsoft-identity-manager-2016-ma-ws) |
| [File di testo per coppia valore-attributo](https://technet.microsoft.com/library/cc708644(v=ws.10).aspx) | File di testo per coppia valore-attributo |
| [File di testo delimitato](https://technet.microsoft.com/library/cc720612(v=ws.10).aspx) | File di testo delimitati |
| [Directory Services Mark-up Language (DSML)](https://technet.microsoft.com/library/cc720660(v=ws.10).aspx) | Directory Services Markup Language (DSML) 2.0 |
| [File di testo a larghezza fissa](https://technet.microsoft.com/library/cc720633(v=ws.10).aspx) | File di testo a larghezza fissa |
| [Formato interscambio dati LDIF](https://technet.microsoft.com/library/cc708662(v=ws.10).aspx) | Formato interscambio dati LDIF |

## <a name="related-topics"></a>Argomenti correlati

[Agenti di gestione di FIM 2010 R2](https://technet.microsoft.com/library/jj133885.aspx)
