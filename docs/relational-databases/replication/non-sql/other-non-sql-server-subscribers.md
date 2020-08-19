---
description: Autres abonnés non SQL Server
title: Autres abonnés non-SQL Server | Microsoft Docs
ms.custom: ''
ms.date: 03/08/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- non-SQL Server Subscribers, other types
ms.assetid: 96b8beb9-38e8-4ce4-97ca-c0f8656b73b4
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: e593b228d39cc84c35647e72135805e2994b305a
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88448131"
---
# <a name="other-non-sql-server-subscribers"></a>Autres abonnés non SQL Server
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Pour obtenir la liste des abonnés non-[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] pris en charge par [!INCLUDE[msCoName](../../../includes/msconame-md.md)], consultez [Abonnés non-SQL Server](../../../relational-databases/replication/non-sql/non-sql-server-subscribers.md). Cette rubrique propose des informations sur la configuration requise des pilotes ODBC et des fournisseurs OLE DB.  
  
## <a name="odbc-driver-requirements"></a>Configuration requise des pilotes ODBC  
 Le pilote ODBC :  
  
-   doit être conforme à ODBC niveau 1 ;  
  
-   Doit être un environnement de Serveur de distribution thread-safe.  
  
-   doit être capable d'exécuter des transactions ;  
  
-   doit prendre en charge le langage de définition de données (DDL - Data Definition Language) ;  
  
-   ne peut pas être en lecture seule ;  
  
-   doit prendre en charge les noms de table longs, tels que **MSreplication_subscriptions**.  
  
## <a name="replicating-using-ole-db-interfaces"></a>Réplication à l'aide d'interfaces OLE DB  
 Les fournisseurs OLE DB doivent prendre en charge les objets suivants pour la réplication transactionnelle :  
  
-   Objet**DataSource**  
  
-   Objet**Session**  
  
-   Objet**Command**  
  
-   Objet**Rowset**  
  
-   Objet**Error**  
  
### <a name="datasource-object-interfaces"></a>Interfaces de l'objet DataSource  
 Les interfaces suivantes sont nécessaires pour se connecter à une source de données :  
  
-   **IDBInitialize**  
  
-   **IDBCreateSession**  
  
-   **IDBProperties**  
  
 Si le fournisseur prend en charge l’interface **IDBInfo**, [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] utilise cette interface pour extraire des informations, telles que le caractère identificateur entre guillemets, la longueur maximale des instructions SQL et le nombre maximal de caractères que peuvent compter les noms de tables et de colonnes.  
  
### <a name="session-object-interfaces"></a>Interfaces de l'objet Session  
 Les interfaces suivantes sont nécessaires :  
  
-   **IDBCreateCommand**  
  
-   **ITransaction**  
  
-   **ITransactionLocal**  
  
-   **IDBSchemaRowset**  
  
### <a name="command-object-interfaces"></a>Interfaces de l'objet Command  
 Les interfaces suivantes sont nécessaires :  
  
-   **ICommand**  
  
-   **ICommandProperties**  
  
-   **ICommandText**  
  
-   **ICommandPrepare**  
  
-   **IColumnsInfo**  
  
-   **IAccessor**  
  
-   **ICommandWithParameters**  
  
 **IAccessor** est nécessaire pour créer des accesseurs de paramètre. Si le fournisseur prend en charge **ColumnRowset**, [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] utilise cette interface pour déterminer si une colonne est une colonne d'identité.  
  
### <a name="rowset-object-interfaces"></a>Interfaces de l'objet Rowset  
 Les interfaces suivantes sont nécessaires :  
  
-   **IRowset**  
  
-   **IAccessor**  
  
-   **IColumnsInfo**  
  
 Une application doit ouvrir un ensemble de lignes sur une table répliquée ayant été créée dans la base de données d'abonnement. **IColumnsInfo** et **IAccessor** sont nécessaires pour accéder aux données de l'ensemble de lignes.  
  
### <a name="error-object-interfaces"></a>Interfaces de l'objet Error  
 Utilisez les interfaces suivantes pour gérer les erreurs :  
  
-   **IErrorRecords**  
  
-   **IErrorInfo**  
  
 Utilisez **ISQLErrorInfo** si elle est prise en charge par le fournisseur OLE DB.  
  
 Pour plus d'informations, reportez-vous à la documentation qui accompagne votre fournisseur OLE DB.  
  
## <a name="see-also"></a>Voir aussi  
 [Non-SQL Server Subscribers](../../../relational-databases/replication/non-sql/non-sql-server-subscribers.md)  
  
  
