---
description: sys.dm_xtp_transaction_stats (Transact-SQL)
title: sys.dm_xtp_transaction_stats (Transact-SQL)
ms.custom: ''
ms.date: 03/12/2021
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_xtp_transaction_stats_TSQL
- dm_xtp_transaction_stats
- sys.dm_xtp_transaction_stats_TSQL
- sys.dm_xtp_transaction_stats
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_xtp_transaction_stats dynamic management view
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 37a98ee4a73e7bcc0ce06cef44d3a142e7fba772
ms.sourcegitcommit: be74dc0966930f28b03d0429aed22b1f0a296d3b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/13/2021
ms.locfileid: "103421861"
---
# <a name="sysdm_xtp_transaction_stats-transact-sql"></a>sys.dm_xtp_transaction_stats (Transact-SQL)
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Fournit des statistiques sur les transactions qui ont été exécutées depuis que le serveur a démarré.  
  
 Pour plus d’informations, consultez [OLTP en mémoire &#40;optimisation en mémoire&#41;](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md).  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|total_count|**bigint**|Nombre total de transactions qui ont été exécutées dans le moteur de base de données de l'OLTP en mémoire.|  
|read_only_count|**bigint**|Nombre de transactions en lecture seule.|  
|total_aborts|**bigint**|Nombre total de transactions qui ont été interrompues, via un abandon du système ou de l'utilisateur.|  
|system_aborts|**bigint**|Nombre d'abandons initiés par le système. Par exemple, en raison de conflits d'écriture, d'échecs de validation ou de défaillances de dépendance.|  
|validation_failures|**bigint**|Nombre de fois qu'une transaction est interrompue en raison d'un problème de validation.|  
|dependencies_taken|**bigint**|À usage interne uniquement|  
|dependencies_failed|**bigint**|Nombre de fois qu'une transaction s'interrompt car une transaction de laquelle elle dépend s'interrompt.|  
|savepoint_create|**bigint**|Nombre de points de sauvegarde créés. Un point de sauvegarde est créé pour chaque bloc atomique.|  
|savepoint_rollbacks|**bigint**|Nombre de restaurations à un point de sauvegarde précédent.|  
|savepoint_refreshes|**bigint**|À usage interne uniquement|  
|log_bytes_written|**bigint**|Nombre total d'octets écrits dans les enregistrements de journal de l'OLTP en mémoire.|  
|log_IO_count|**bigint**|Nombre total de transactions nécessitant des E/S du journal. Considère uniquement les transactions sur les tables durables.|  
|phantom_scans_started|**bigint**|À usage interne uniquement|  
|phatom_scans_retries|**bigint**|À usage interne uniquement|  
|phantom_rows_touched|**bigint**|À usage interne uniquement|  
|phantom_rows_expiring|**bigint**|À usage interne uniquement|  
|phantom_rows_expired|**bigint**|À usage interne uniquement|  
|phantom_rows_expired_removed|**bigint**|À usage interne uniquement|  
|scans_started|**bigint**|À usage interne uniquement|  
|scans_retried|**bigint**|À usage interne uniquement|  
|rows_returned|**bigint**|À usage interne uniquement|  
|rows_touched|**bigint**|À usage interne uniquement|  
|rows_expiring|**bigint**|À usage interne uniquement|  
|rows_expired|**bigint**|À usage interne uniquement|  
|rows_expired_removed|**bigint**|À usage interne uniquement|  
|rows_inserted|**bigint**|À usage interne uniquement|  
|rows_updated|**bigint**|À usage interne uniquement|  
|rows_deleted|**bigint**|À usage interne uniquement|  
|write_conflicts|**bigint**|À usage interne uniquement|  
|unique_constraint_violations|**bigint**|Nombre total de violations de contrainte uniques.|  
  
## <a name="permissions"></a>Autorisations  
 requièrent l'autorisation VIEW SERVER STATE sur le serveur.  
  
## <a name="see-also"></a>Voir aussi  
 [Vues de gestion dynamique des tables à mémoire optimisée &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/memory-optimized-table-dynamic-management-views-transact-sql.md)  
  
  
