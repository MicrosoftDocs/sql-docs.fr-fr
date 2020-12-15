---
title: Programmation
description: Utilisez ces ressources pour comprendre SQL Server Native Client, une API d’accès aux données autonome, utilisée pour OLE DB et ODBC.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- SQLNCLI, about SQL Server Native Client
- SQL Server Native Client, about SQL Server Native Client
- data access [SQL Server Native Client], about SQL Server Native Client
- data access [SQL Server Native Client]
- SQL Server Native Client
- SQLNCLI
- native data access [SQL Server Native Client]
ms.assetid: 14ba2cb1-a424-4e4d-b224-0bf1015ab801
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 918094d0bbc5739ed00827cc239723379abe3cf1
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/14/2020
ms.locfileid: "97463290"
---
# <a name="sql-server-native-client-programming"></a>Programmation de SQL Server Native Client
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client est une interface de programmation d'applications (API, Application Programming Interface) autonome d'accès aux données, utilisée pour OLE DB et ODBC ; elle a été introduite avec [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client combine le fournisseur OLE DB SQL et le pilote ODBC SQL en une bibliothèque de liens dynamiques (DLL) native. Il fournit également de nouvelles fonctionnalités au-delà de celles fournies par Windows Data Access Components (Windows DAC, anciennement MDAC (Microsoft Data Access Components), ou MDAC). [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client permet de créer de nouvelles applications ou d'améliorer des applications existantes qui doivent tirer profit des fonctionnalités introduites dans [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)], telles que MARS (Multiple Active Result Sets), les types définis par l'utilisateur (UDT), les notifications de requêtes et la prise en charge des types de données XML.  
  
> [!NOTE]  
>  Pour obtenir la liste des différences entre [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client et Windows DAC, ainsi que des informations sur les problèmes à prendre en compte avant de mettre à jour une application Windows DAC vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client, consultez [mise à jour d’une application vers SQL Server Native Client à partir de MDAC](../../relational-databases/native-client/applications/updating-an-application-to-sql-server-native-client-from-mdac.md).  
  
 Le pilote ODBC [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client est toujours utilisé conjointement avec le Gestionnaire de pilote ODBC fourni avec Windows DAC. Le fournisseur OLE DB [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client peut être utilisé conjointement avec les services principaux OLE DB fournis avec Windows DAC, mais cela n'est pas obligatoire ; ce choix dépend des spécifications de l'application individuelle (par exemple, si le regroupement de connexions est requis).  
  
 Les applications ActiveX Data Object (ADO) peuvent utiliser le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournisseur de OLE DB Native Client, mais il est recommandé d’utiliser ADO conjointement avec le mot clé de chaîne de connexion **DataTypeCompatibility** (ou la propriété **DataSource** correspondante). Lors de l'utilisation du fournisseur OLE DB [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client, les applications ADO peuvent exploiter ces nouvelles fonctionnalités introduites dans [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] disponibles via [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client par le biais de mots clés de chaîne de connexion, de propriétés OLE DB ou de [!INCLUDE[tsql](../../includes/tsql-md.md)]. Pour plus d’informations sur l’utilisation de ces fonctionnalités avec ADO, consultez [utilisation d’ADO avec SQL Server Native Client](../../relational-databases/native-client/applications/using-ado-with-sql-server-native-client.md).  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client a été conçu en vue de fournir une méthode simplifiée pour obtenir un accès aux données natives à SQL Server à l'aide de OLE DB ou ODBC. Il s'agit d'une méthode simplifiée, dans le sens où elle associe les technologies OLE DB et ODBC sous la forme d'une bibliothèque et qu'il est possible d'améliorer et de faire évoluer les nouvelles fonctionnalités d'accès aux données sans modifier pour autant les composants Windows DAC actuels, qui appartiennent désormais à la plateforme Microsoft Windows.  
  
 Bien que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client utilise des composants dans Windows DAC, il n’est pas explicitement dépendant d’une version particulière de Windows DAC. Vous pouvez utiliser [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client avec la version de Windows DAC installée avec tout système d'exploitation pris en charge par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client.  
  
## <a name="in-this-section"></a>Dans cette section  
 [SQL Server Native Client](../../relational-databases/native-client/sql-server-native-client.md)  
 Répertorie les nouvelles fonctionnalités significatives de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client.  
  
 [Quand utiliser SQL Server Native Client](../../relational-databases/native-client/when-to-use-sql-server-native-client.md)  
 Traite de l'intégration de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client avec les technologies d'accès aux données de Microsoft, le compare à Windows DAC et ADO.NET et fournit des pointeurs pour vous aider à déterminer la technologie d'accès aux données à utiliser.  
  
 [Fonctionnalités de SQL Server Native Client](../../relational-databases/native-client/features/sql-server-native-client-features.md)  
 Décrit les fonctionnalités prises en charge par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client.  
  
 [Génération d’applications avec SQL Server Native Client](../../relational-databases/native-client/applications/building-applications-with-sql-server-native-client.md)  
 Fournit une vue d'ensemble du développement [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client, y compris la façon dont il diffère de Windows DAC, les composants qu'il utilise et la manière dont ADO peut être utilisé avec lui.  
  
 Cette section discute également de l'installation et du déploiement de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client, y compris comment redistribuer la bibliothèque [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client.  
  
 [Configuration requise pour SQL Server Native Client](../../relational-databases/native-client/system-requirements-for-sql-server-native-client.md)  
 Discute des ressources système nécessaires pour utiliser [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client.  
  
 [SQL Server Native Client &#40;OLE DB&#41;](../../relational-databases/native-client/ole-db/sql-server-native-client-ole-db.md)  
 Fournit des informations sur l'utilisation du fournisseur OLE DB [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client.  
  
 [SQL Server Native Client &#40;ODBC&#41;](../../relational-databases/native-client/odbc/sql-server-native-client-odbc.md)  
 Fournit des informations sur l'utilisation du pilote ODBC [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client.  
  
 [Recherche d'informations supplémentaires concernant SQL Server Native Client](../../relational-databases/native-client/finding-more-sql-server-native-client-information.md)  
 Fournit des ressources supplémentaires au sujet de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client, y compris des liens vers des ressources externes et permettant d'obtenir une assistance.  
  
 [Erreurs de SQL Server Native Client](./sql-server-native-client-error-mssqlserver-50000.md)  
 Contient des rubriques relatives aux erreurs d'exécution associées à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client.  
  
## <a name="see-also"></a>Voir aussi  
 [Mise à jour d’une application à partir de SQL Server Native Client 2005](../../relational-databases/native-client/applications/updating-an-application-from-sql-server-2005-native-client.md)   
 [Rubriques de procédures relatives à ODBC](../../relational-databases/native-client-odbc-how-to/odbc-how-to-topics.md)   
 [Rubriques de procédures liées à OLE DB](../../relational-databases/native-client-ole-db-how-to/ole-db-how-to-topics.md)  
  
