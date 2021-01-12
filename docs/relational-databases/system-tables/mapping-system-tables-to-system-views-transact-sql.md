---
description: Mappage des tables système avec les vues du système (Transact-SQL)
title: Mappage de tables système à des vues système (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- catalog views [SQL Server], mapping system tables to
- mapping system tables to system views [SQL Server]
- system tables [SQL Server], mapping to catalog views
ms.assetid: a616fce9-b4c1-49da-87a7-9d6f74911d8f
author: cawrites
ms.author: chadam
ms.openlocfilehash: 86e629050907b1ffbd7a1748082e683db85affbf
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092430"
---
# <a name="mapping-system-tables-to-system-views-transact-sql"></a>Mappage des tables système avec les vues du système (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Cette rubrique présente le mappage entre les tables et les fonctions système et entre les vues et les fonctions système.  
  
 Le tableau suivant mappe les tables système aux vues ou fonctions système correspondantes dans [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].  
  
|Table système|Vues système ou fonctions|Type de vue ou fonction|  
|------------------|-------------------------------|------------------------------|  
|sysaltfiles|[sys.master_files](../../relational-databases/system-catalog-views/sys-master-files-transact-sql.md)|Affichage catalogue|  
|syscacheobjects|[sys.dm_exec_cached_plans](../../relational-databases/system-dynamic-management-views/sys-dm-exec-cached-plans-transact-sql.md)<br /><br /> [sys.dm_exec_plan_attributes &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-plan-attributes-transact-sql.md)<br /><br /> [sys.dm_exec_sql_text](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sql-text-transact-sql.md)<br /><br /> [sys.dm_exec_cached_plan_dependent_objects](../../relational-databases/system-dynamic-management-views/sys-dm-exec-cached-plan-dependent-objects-transact-sql.md)|Vue de gestion dynamique<br /><br /> Vue de gestion dynamique<br /><br /> Vue de gestion dynamique<br /><br /> Vue de gestion dynamique|  
|syscharsets|[sys.syscharsets](../../relational-databases/system-compatibility-views/sys-syscharsets-transact-sql.md)|Vue de compatibilité|  
|sysconfigures|[sys.configurations](../../relational-databases/system-catalog-views/sys-configurations-transact-sql.md)|Affichage catalogue|  
|syscurconfigs|[sys.configurations](../../relational-databases/system-catalog-views/sys-configurations-transact-sql.md)|Affichage catalogue|  
|sysdatabases|[sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)|Affichage catalogue|  
|sysdevices|[sys.backup_devices](../../relational-databases/system-catalog-views/sys-backup-devices-transact-sql.md)|Affichage catalogue|  
|syslanguages|[sys.syslanguages](../../relational-databases/system-compatibility-views/sys-syslanguages-transact-sql.md)|Vue de compatibilité|  
|syslockinfo|[sys.dm_tran_locks](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md)|Vue de gestion dynamique|  
|syslocks|[sys.dm_tran_locks](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md)|Vue de gestion dynamique|  
|syslogins|[sys.server_principals](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md)<br /><br /> [sys.sql_logins](../../relational-databases/system-catalog-views/sys-sql-logins-transact-sql.md)|Affichage catalogue|  
|sysmessages|[sys.messages](../../relational-databases/system-catalog-views/messages-for-errors-catalog-views-sys-messages.md)|Affichage catalogue|  
|sysoledbusers|[sys.linked_logins](../../relational-databases/system-catalog-views/sys-linked-logins-transact-sql.md)|Affichage catalogue|  
|sysopentapes|[sys.dm_io_backup_tapes](../../relational-databases/system-dynamic-management-views/sys-dm-io-backup-tapes-transact-sql.md)|Vue de gestion dynamique|  
|sysperfinfo|[sys.dm_os_performance_counters](../../relational-databases/system-dynamic-management-views/sys-dm-os-performance-counters-transact-sql.md)|Vue de gestion dynamique|  
|sysprocesses|[sys.dm_exec_connections](../../relational-databases/system-dynamic-management-views/sys-dm-exec-connections-transact-sql.md)<br /><br /> [sys.dm_exec_sessions](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sessions-transact-sql.md)<br /><br /> [sys.dm_exec_requests](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)|Vue de gestion dynamique<br /><br /> Vue de gestion dynamique<br /><br /> Vue de gestion dynamique|  
|sysremotelogins|[sys.remote_logins](../../relational-databases/system-catalog-views/sys-remote-logins-transact-sql.md)|Affichage catalogue|  
|sysservers|[sys.servers](../../relational-databases/system-catalog-views/sys-servers-transact-sql.md)|Affichage catalogue|  
  
 Le tableau suivant mappe les tables ou fonctions système de chaque base de données de [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] avec les vues ou fonctions système correspondantes dans [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].  
  
|Table système ou fonction|Vue système ou fonction|Type de vue ou fonction|  
|------------------------------|-----------------------------|------------------------------|  
|fn_virtualfilestats|[sys.dm_io_virtual_file_stats](../../relational-databases/system-dynamic-management-views/sys-dm-io-virtual-file-stats-transact-sql.md)|Vue de gestion dynamique|  
|syscolumns|[sys.columns](../../relational-databases/system-catalog-views/sys-columns-transact-sql.md)|Affichage catalogue|  
|syscomments|[sys.sql_modules](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md)|Affichage catalogue|  
|sysconstraints|[sys.check_constraints](../../relational-databases/system-catalog-views/sys-check-constraints-transact-sql.md)<br /><br /> [sys.default_constraints](../../relational-databases/system-catalog-views/sys-default-constraints-transact-sql.md)<br /><br /> [sys.key_constraints](../../relational-databases/system-catalog-views/sys-key-constraints-transact-sql.md)<br /><br /> [sys.foreign_keys](../../relational-databases/system-catalog-views/sys-foreign-keys-transact-sql.md)|Affichage catalogue<br /><br /> Affichage catalogue<br /><br /> Affichage catalogue<br /><br /> Affichage catalogue|  
|sysdepends|[sys.sql_expression_dependencies](../../relational-databases/system-catalog-views/sys-sql-expression-dependencies-transact-sql.md)|Affichage catalogue|  
|sysfilegroups|[sys.filegroups](../../relational-databases/system-catalog-views/sys-filegroups-transact-sql.md)|Affichage catalogue|  
|sysfiles|[sys.database_files](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md)|Affichage catalogue|  
|sysforeignkeys|[sys.foreign_key_columns](../../relational-databases/system-catalog-views/sys-foreign-key-columns-transact-sql.md)|Affichage catalogue|  
|sysindexes|[sys.indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)<br /><br /> [sys.partitions](../../relational-databases/system-catalog-views/sys-partitions-transact-sql.md)<br /><br /> [sys.allocation_units](../../relational-databases/system-catalog-views/sys-allocation-units-transact-sql.md)<br /><br /> [sys.dm_db_partition_stats](../../relational-databases/system-dynamic-management-views/sys-dm-db-partition-stats-transact-sql.md)|Affichage catalogue<br /><br /> Affichage catalogue<br /><br /> Affichage catalogue<br /><br /> Vue de gestion dynamique|  
|sysindexkeys|[sys.index_columns](../../relational-databases/system-catalog-views/sys-index-columns-transact-sql.md)|Affichage catalogue|  
|sysmembers|[sys.database_role_members](../../relational-databases/system-catalog-views/sys-database-role-members-transact-sql.md)|Affichage catalogue|  
|sysobjects|[sys.objects](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)|Affichage catalogue|  
|syspermissions|[sys.database_permissions](../../relational-databases/system-catalog-views/sys-database-permissions-transact-sql.md)<br /><br /> [sys.server_permissions](../../relational-databases/system-catalog-views/sys-server-permissions-transact-sql.md)|Affichage catalogue<br /><br /> Affichage catalogue|  
|sysprotects|[sys.database_permissions](../../relational-databases/system-catalog-views/sys-database-permissions-transact-sql.md)<br /><br /> [sys.server_permissions](../../relational-databases/system-catalog-views/sys-server-permissions-transact-sql.md)|Affichage catalogue<br /><br /> Affichage catalogue|  
|sysreferences|[sys.foreign_keys](../../relational-databases/system-catalog-views/sys-foreign-keys-transact-sql.md)|Affichage catalogue|  
|systypes|[sys.types](../../relational-databases/system-catalog-views/sys-types-transact-sql.md)|Affichage catalogue|  
|sysusers|[sys.database_principals](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md)|Affichage catalogue|  
|sysfulltextcatalogs|[sys.fulltext_catalogs](../../relational-databases/system-catalog-views/sys-fulltext-catalogs-transact-sql.md)|Affichage catalogue|  
  
## <a name="see-also"></a>Voir aussi  
 [Affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Fonctions et vues de gestion dynamique &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Tables système &#40;Transact-SQL&#41;](../../relational-databases/system-tables/system-tables-transact-sql.md)  
  
  
