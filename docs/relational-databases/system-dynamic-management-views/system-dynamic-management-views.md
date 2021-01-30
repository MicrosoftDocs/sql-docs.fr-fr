---
description: Vues de gestion dynamique (Transact-SQL)
title: Vues de gestion dynamique (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/29/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- database scoped dynamic management objects [SQL Server]
- dynamic management objects [SQL Server], about dynamic management objects
- server state information [SQL Server]
- dynamic management functions [SQL Server]
- metadata [SQL Server], dynamic management objects
- dynamic management views [SQL Server]
- DMVs [SQL Server]
- functions [SQL Server], dynamic management
- server scoped dynamic management objects [SQL Server]
- dynamic management objects [SQL Server]
ms.assetid: cf893ecb-0bf6-4cbf-ac00-8a1099e405b1
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 856777121b7d613c9ac28843c98520d07a93ceac
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196269"
---
# <a name="system-dynamic-management-views"></a>Vues de gestion dynamique système
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Les fonctions et les vues de gestion dynamique renvoient des informations sur l'état du serveur qu'il est possible d'utiliser pour surveiller l'état d'une instance du serveur, diagnostiquer des problèmes et améliorer les performances.  
  
> [!IMPORTANT]  
>  Les fonctions et vues de gestion dynamique renvoient des données d'état internes propres à l'implémentation. Leurs schémas et les données qu'elles renvoient sont susceptibles de changer dans les versions ultérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Les fonctions et vues de gestion dynamique des futures versions ne seront donc peut-être pas compatibles avec celles de cette version. Par exemple, dans les versions ultérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], Microsoft peut augmenter la définition de la vue de gestion dynamique en ajoutant des colonnes à la fin de la liste des colonnes. Nous déconseillons l'utilisation de la syntaxe `SELECT * FROM dynamic_management_view_name` dans le code de production car le nombre de colonnes retourné peut changer et altérer votre application.  
  
 Il existe deux types de fonctions et vues de gestion dynamique :  
  
-   les fonctions et vues de gestion dynamique dont l'étendue est définie au niveau du serveur, qui nécessitent l'autorisation VIEW SERVER STATE sur le serveur ;  
  
-   Les fonctions et vues de gestion dynamique dont l'étendue est définie au niveau de la base de données, qui nécessitent l'autorisation VIEW DATABASE STATE sur la base de données.  
  
## <a name="querying-dynamic-management-views"></a>Interrogation des vues de gestion dynamique  
 Il est possible de référencer des vues de gestion dynamique dans des instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] en utilisant des noms en deux, trois ou quatre parties. En revanche, il est possible de référencer les fonctions de gestion dynamique dans des instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] en utilisant des noms en deux ou trois parties. Il n'est pas possible de référencer les fonctions et vues de gestion dynamique dans des instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] en utilisant des noms en une seule partie.  
  
 Toutes les fonctions et vues de gestion dynamique existent dans le schéma sys et respectent la convention de noms dm_*. Lorsque vous utilisez une fonction ou une vue de gestion dynamique, vous devez faire précéder le nom de la vue ou de la fonction du schéma sys. Par exemple, pour effectuer une requête dans la vue de gestion dynamique dm_os_wait_stats, exécutez :  
  
 ```sql
SELECT wait_type, wait_time_ms  
FROM sys.dm_os_wait_stats;  
```  
  
### <a name="required-permissions"></a>Autorisations requises  
 Une requête sur une fonction ou une vue de gestion dynamique nécessite l'autorisation SELECT sur un objet et l'autorisation VIEW SERVER STATE ou VIEW DATABASE STATE. Cela permet de limiter sélectivement l'accès d'un utilisateur ou d'une connexion aux fonctions et vues de gestion dynamique. Pour cela, créez d'abord l'utilisateur dans master, puis refusez à cet utilisateur l'autorisation SELECT sur les fonctions et vues de gestion dynamique auxquelles vous souhaitez interdire l'accès. Après cela, l'utilisateur ne peut pas sélectionner ces fonctions et vues de gestion dynamique, quel que soit le contexte de la base de données de l'utilisateur.  
  
> [!NOTE]  
>  Du fait que DENY est prioritaire, si un utilisateur a les autorisations VIEW SERVER STATE mais n'a pas l'autorisation VIEW DATABASE STATE, il peut visualiser des informations au niveau serveur, mais pas au niveau base de données.  
  
## <a name="in-this-section"></a>Dans cette section  
 Les fonctions et vues de gestion dynamique sont organisées selon les catégories suivantes.  

:::row:::
    :::column:::
        [Fonctions et vues de gestion dynamique de groupes de disponibilité Always On (Transact-SQL)](../../relational-databases/system-dynamic-management-views/always-on-availability-groups-dynamic-management-views-functions.md)

        [Vues de gestion dynamique liées à la capture des données modifiées &#40;Transact-SQL&#41;](change-data-capture-sys-dm-cdc-errors.md)

        [Vues de gestion dynamique liées au suivi des données modifiées](change-tracking-sys-dm-tran-commit-table.md)

        [Vues de gestion dynamique liées au Common Language Runtime &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/common-language-runtime-related-dynamic-management-views-transact-sql.md)

        [Vues de gestion dynamique liées à la mise en miroir de bases de données &#40;Transact-SQL&#41;](database-mirroring-sys-dm-db-mirroring-auto-page-repair.md)

        [Vues de gestion dynamique liées à la base de données &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/database-related-dynamic-management-views-transact-sql.md)

        [Fonctions et vues de gestion dynamique relatives aux exécutions &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)

        [Vues de gestion dynamique des Événements étendus](../../relational-databases/system-dynamic-management-views/extended-events-dynamic-management-views.md)

        [Vues de gestion dynamique FileStream et filetable &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/filestream-and-filetable-dynamic-management-views-transact-sql.md)

        [Fonctions et vues de gestion dynamique de la recherche en texte intégral et de la recherche sémantique &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/full-text-and-semantic-search-dynamic-management-views-functions.md)

        [Fonctions et vues de gestion dynamique de la géo-réplication &#40;Azure SQL Database&#41;](../../relational-databases/system-dynamic-management-views/geo-replication-dynamic-management-views-and-functions-azure-sql-database.md)

        [Fonctions et vues de gestion dynamique relatives aux index &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/index-related-dynamic-management-views-and-functions-transact-sql.md)

        [I O fonctions et vues de gestion dynamique associées &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/i-o-related-dynamic-management-views-and-functions-transact-sql.md)
    :::column-end:::
    :::column:::
        [Vues de gestion dynamique des tables à mémoire optimisée &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/memory-optimized-table-dynamic-management-views-transact-sql.md)

        [Fonctions et vues de gestion dynamique relatives à l’objet &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/object-related-dynamic-management-views-and-functions-transact-sql.md)

        [Vues de gestion dynamique liées aux notifications de requête &#40;&#41;Transact-SQL ](query-notifications-sys-dm-qn-subscriptions.md)

        [Vues de gestion dynamique liées à la réplication &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/replication-related-dynamic-management-views-transact-sql.md)

        [Vues de gestion dynamique relatives au gouverneur de ressources &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/resource-governor-related-dynamic-management-views-transact-sql.md)

        [Fonctions et vues de gestion dynamique relatives à la sécurité &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/security-related-dynamic-management-views-and-functions-transact-sql.md)

        [Fonctions et vues de gestion dynamique relatives au serveur &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/server-related-dynamic-management-views-and-functions-transact-sql.md)

        [Vues de gestion dynamique liées à Service Broker &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/service-broker-related-dynamic-management-views-transact-sql.md)

        [Fonctions et vues de gestion dynamique liées aux données spatiales &#40;Transact-SQL&#41;](./spatial-data-sys-dm-db-objects-disabled-on-compatibility-level-change.md)

        [Azure Synapse Analytics et les vues de gestion dynamique Parallel Data Warehouse &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-and-parallel-data-warehouse-dynamic-management-views.md)

        [SQL Server vues de gestion dynamique liées au système d’exploitation &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)

        [Stretch Database vues de gestion dynamique &#40;Transact-SQL&#41;]()

        [Fonctions et vues de gestion dynamique relatives aux transactions &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/transaction-related-dynamic-management-views-and-functions-transact-sql.md)
    :::column-end:::
:::row-end:::

## <a name="see-also"></a>Voir aussi  
 [ACCORDER des autorisations de serveur &#40;Transact-SQL&#41;](../../t-sql/statements/grant-server-permissions-transact-sql.md)   
 [GRANT - Octroyer des autorisations sur une base de données &#40;Transact-SQL&#41;](../../t-sql/statements/grant-database-permissions-transact-sql.md)   
 [Vues système &#40;&#41;Transact-SQL ](../../t-sql/language-reference.md)  
  
