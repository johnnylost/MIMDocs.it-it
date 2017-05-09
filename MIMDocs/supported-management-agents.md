---
title: Connettori supportati | Documentazione Microsoft
description: Usare i connettori per gestire il trasferimento dei dati tra MIM e le directory.
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/11/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 8bc2f6d2-9f53-4db6-aee6-a937ae468163
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: bfc73723bdd3a49529522f78ac056939bb8025a3
ms.openlocfilehash: b26fe7bc56ab8229054afb1409c3652e81464a3d
ms.lasthandoff: 05/02/2017


---

# <a name="connect-to-your-directories"></a>Connettersi alle directory

I connettori collegano le origini dati connesse specificate a Microsoft Identity Manager (MIM). Un connettore sposta i dati da un'origine dati connessa a MIM. Grazie al connettore è anche possibile esportare i dati all'origine dati connessa perché siano sincronizzati con MIM quando vengono modificati in MIM. Esiste in genere almeno un connettore per ogni directory connessa.

In Forefront Identity Manager i connettori erano detti agenti di gestione. Questa definizione è ancora usata in alcuni articoli o in parti del prodotto. È tuttavia importante sapere che entrambi i termini si riferiscono allo stesso concetto.

Questo articolo descrive i connettori inclusi in MIM. Il connettore per Extensible Connectivity 2.0 consente comunque di connettersi anche a più origini dati. Alcuni partner hanno creato connettori personalizzati. Nel wiki [FIM 2010: Management Agents from Partners](http://social.technet.microsoft.com/wiki/contents/articles/1589.fim-2010-management-agents-from-partners.aspx) (FIM 2010: Agenti di gestione dei partner) è disponibile un elenco completo.

## <a name="supported-connectors-in-mim-2016"></a>Connettori supportati in MIM 2016

| Nome | Versioni supportate dell'origine dati connessa |
| ---- | ----------------------------------------------- |
| Servizi di dominio di Active Directory | Active Directory 2000, 2003, 2003 R2, 2008, 2008 R2, 2012 |
| Active Directory Lightweight Directory Services (ADLDS) | Active Directory Lightweight Directory Services (ADLDS) |
| Elenco Indirizzi Globale (EIG) di Active Directory | Elenco Indirizzi Globale (EIG) di Active Directory – Exchange 2000, 2003, 2007, 2010, 2013 |
| Extensible Connectivity 2.0 | Qualsiasi origine dati basata su chiamata o file |
| Servizio MIM | Documentazione Microsoft 2016 |
| Database IBM DB2 Universal | IBM DB2 versione 9.1, 9.5 o 9.7; IBM DB2 OLEDB v9.5 FP5 o v9.7 FP1 |
| IBM Directory Server | IBM Tivoli Directory Server 6.x |
| Novell eDirectory | Novell eDirectory versioni 8.7.3, 8.8.5 e 8.8.6 |
| Database Oracle | Oracle Database 10g o 11g; client a 64 bit |
| Microsoft SQL Server | SQL Server 2000, 2005, 2008, 2008 R2, 2012 |
| Server di directory Oracle (precedentemente Sun e Netscape) | Server di directory Sun 6.x, 7.x e Oracle 11 |
| [Connettore di Windows PowerShell per FIM 2010 R2](https://msdn.microsoft.com/en-us/library/dn640417.aspx) | Windows PowerShell 2.0 o versioni successive |
| [Connettore di Microsoft Azure Active Directory per FIM 2010 R2](https://msdn.microsoft.com/en-us/library/dn511001.aspx) | Microsoft Azure Active Directory |
| [Connettore LDAP generico per FIM 2010 R2](https://msdn.microsoft.com/en-us/library/dn510997.aspx) | Server LDAP v3 (compatibile con RFC 4510) |
| [Connettore per Lotus Domino](https://msdn.microsoft.com/en-us/library/hh859750.aspx) | Lotus Notes versione v8.0.x o v8.5.x |
| [Connettore di SharePoint Services per FIM 2010 R2](https://msdn.microsoft.com/en-us/library/dn511003.aspx) | SharePoint Server 2013 o 2016 con Applicazione servizio profili utente |
| [Connettore per servizi Web](https://www.microsoft.com/en-us/download/details.aspx?id=51495) | SAP ECC 5.0 o 6.0; Oracle PeopleSoft 9.1; Oracle eBusiness 12.1 |
| File di testo per coppia valore-attributo | File di testo per coppia valore-attributo |
| File di testo delimitato | File di testo delimitati |
| Directory Services Mark-up Language (DSML) | Directory Services Markup Language (DSML) 2.0 |
| File di testo a larghezza fissa | File di testo a larghezza fissa |
| Formato interscambio dati LDIF | Formato interscambio dati LDIF |

## <a name="related-topics"></a>Argomenti correlati

[Agenti di gestione di FIM 2010 R2](https://technet.microsoft.com/library/jj133885.aspx)

