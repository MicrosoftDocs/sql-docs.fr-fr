---
description: sys.dm_db_xtp_index_stats (Transact-SQL)
title: sys.dm_db_xtp_index_stats (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/29/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_db_xtp_index_stats
- dm_db_xtp_index_stats
- sys.dm_db_xtp_index_stats_TSQL
- dm_db_xtp_index_stats_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_xtp_index_stats dynamic management view
ms.assetid: 8d0a50b8-2015-4576-930f-e3307dfc888e
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 94af612964f33f9f463701593f9cddc4b3b992bb
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/01/2021
ms.locfileid: "99235980"
---
# <a name="sysdm_db_xtp_index_stats-transact-sql"></a>sys.dm_db_xtp_index_stats (Transact-SQL)
[!INCLUDE[sql-asdb-asdbmi](../../includes/applies-to-version/sql-asdb-asdbmi.md)]

  Contient les statistiques collectées depuis le dernier redémarrage de la base de données.  
  
 Pour plus d’informations, consultez [OLTP en mémoire &#40;In-Memory optimisation&#41;](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md) et [instructions pour l’utilisation d’index sur des tables Memory-Optimized](/previous-versions/sql/sql-server-2016/dn133166(v=sql.130)).  

  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|object_id|**bigint**|ID de l'objet auquel appartient cet index.|  
|xtp_object_id|**bigint**|ID interne correspondant à la version actuelle de l’objet.<br /><br /> Remarque : s’applique à [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] .|  
|index_id|**bigint**|Identificateur de l'index. L'index_id n'est unique qu'à l'intérieur de l'objet.|  
|scans_started|**bigint**|Nombre d'analyses d'index de l'OLTP en mémoire effectuées. Chaque sélection, insertion, mise à jour ou suppression nécessite une analyse d'index.|  
|scans_retries|**bigint**|Nombre d'analyses d'index qui doivent être retentées.|  
|rows_returned|**bigint**|Nombre cumulatif de lignes retournées depuis la création de la table ou le démarrage de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|rows_touched|**bigint**|Nombre cumulatif de lignes auxquelles il a été possible d'accéder depuis la création de la table ou le démarrage de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|rows_expiring|**bigint**|À usage interne uniquement|  
|rows_expired|**bigint**|À usage interne uniquement|  
|rows_expired_removed|**bigint**|À usage interne uniquement|  
|phantom_scans_started|**bigint**|À usage interne uniquement|  
|phatom_scans_retries|**bigint**|À usage interne uniquement|  
|phantom_rows_touched|**bigint**|À usage interne uniquement|  
|phantom_expiring_rows_encountered|**bigint**|À usage interne uniquement|  
|phantom_expired_rows_encountered|**bigint**|À usage interne uniquement|  
|phantom_expired_removed_rows_encountered|**bigint**|À usage interne uniquement|  
|phantom_expired_rows_removed|**bigint**|À usage interne uniquement|  
|object_address|**varbinary (8)**|À usage interne uniquement|  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'autorisation VIEW DATABASE STATE sur la base de données active.  
  
## <a name="see-also"></a>Voir aussi  
 [Vues de gestion dynamique des tables à mémoire optimisée &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/memory-optimized-table-dynamic-management-views-transact-sql.md)  
  
