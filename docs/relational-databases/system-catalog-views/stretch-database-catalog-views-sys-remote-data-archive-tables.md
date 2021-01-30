---
title: sys.remote_data_archive_tables (Transact-SQL) | Microsoft Docs
description: Découvrez comment sys.remote_data_archive_tables contient une ligne pour chaque table distante qui stocke des données à partir d’une table locale avec Stretch.
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.technology: stored-procedures
ms.topic: reference
f1_keywords:
- sys.remote_data_archive_tables
- sys.remote_data_archive_tables_TSQL
- remote_data_archive_tables
- remote_data_archive_tables_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.remote_data_archive_tables catalog view
ms.assetid: 765069b7-60fd-414c-875f-3455460b75cd
author: pmasl
ms.author: pelopes
ms.reviewer: mikeray
ms.openlocfilehash: cd0c62c78c93557c011c299c7cf0e65c7285fba8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209038"
---
# <a name="stretch-database-catalog-views---sysremote_data_archive_tables"></a>Affichages catalogue Stretch Database-sys.remote_data_archive_tables
[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]

  Contient une ligne pour chaque table distante qui stocke les données d’une table locale avec Stretch.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**object_id**|**int**|ID d’objet de la table locale prenant en charge Stretch.|  
|**remote_database_id**|**int**|Identificateur local généré automatiquement de la base de données distante.|  
|**remote_table_name**|**sysname**|Nom de la table dans la base de données distante qui correspond à la table locale avec Stretch.|  
|**filter_predicate**|**nvarchar(max)**|Prédicat de filtre, le cas échéant, qui identifie les lignes de la table à migrer. Si la valeur est null, la table entière est éligible à la migration.<br /><br /> Pour plus d’informations, consultez [activer Stretch Database pour une table](../../sql-server/stretch-database/enable-stretch-database-for-a-table.md) et [Sélectionner les lignes à migrer à l’aide d’un prédicat de filtre](~/sql-server/stretch-database/select-rows-to-migrate-by-using-a-filter-function-stretch-database.md).|  
|**migration_direction**|**tinyint**|Sens dans lequel les données sont actuellement en cours de migration. Les valeurs disponibles sont les suivantes.<br/>1 (sortant)<br/>2 (entrant)|  
|**migration_direction_desc**|**nvarchar(60)**|Description de la direction dans laquelle les données sont actuellement en cours de migration. Les valeurs disponibles sont les suivantes.<br/>sortant (1)<br/>entrant (2)|  
|**is_migration_paused**|**bit**|Indique si la migration est actuellement suspendue.|  
|**is_reconciled**|**bit**| Indique si la table distante et la table SQL Server sont synchronisées.<br/><br/>Lorsque la valeur de **is_reconciled** est 1 (true), la table distante et la table SQL Server sont synchronisées, et vous pouvez exécuter des requêtes qui incluent les données distantes.<br/><br/>Lorsque la valeur de **is_reconciled** est 0 (false), la table distante et la table SQL Server ne sont pas synchronisées. Les lignes récemment migrées doivent être à nouveau migrées. Cela se produit lorsque vous restaurez la base de données Azure distante ou lorsque vous supprimez manuellement des lignes de la table distante. Tant que vous n’avez pas concilié les tables, vous ne pouvez pas exécuter de requêtes incluant les données distantes. Pour rapprocher les tables, exécutez [sys.sp_rda_reconcile_batch](../../relational-databases/system-stored-procedures/sys-sp-rda-reconcile-batch-transact-sql.md). |  
  
## <a name="see-also"></a>Voir aussi  
 [Stretch Database](../../sql-server/stretch-database/stretch-database.md)  
  
  

