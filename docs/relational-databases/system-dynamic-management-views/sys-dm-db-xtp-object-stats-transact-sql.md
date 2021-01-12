---
description: sys.dm_db_xtp_object_stats (Transact-SQL)
title: sys.dm_db_xtp_object_stats (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/29/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_db_xtp_object_stats_TSQL
- sys.dm_db_xtp_object_stats
- dm_db_xtp_object_stats
- sys.dm_db_xtp_object_stats_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_xtp_object_stats dynamic management view
ms.assetid: 07300b59-3cab-4d3e-8138-5ea8f584f88f
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 65852e4ecb6c26ca59ab0edffc18c1ea0c0ffd1c
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099842"
---
# <a name="sysdm_db_xtp_object_stats-transact-sql"></a>sys.dm_db_xtp_object_stats (Transact-SQL)
[!INCLUDE[sql-asdb-asdbmi](../../includes/applies-to-version/sql-asdb-asdbmi.md)]

  Retourne le nombre de lignes affectées par l'opération sur chaque objet [!INCLUDE[hek_2](../../includes/hek-2-md.md)] depuis le dernier redémarrage de la base de données. Les statistiques sont mises à jour lorsque l'opération s'exécute, que la transaction soit validée ou restaurée.  
  
 sys.dm_db_xtp_object_stats vous permet d'identifier les tables mémoire optimisées qui changent le plus. Vous pouvez décider de supprimer des index non utilisés ou rarement utilisés sur la table, car chaque index affecte les performances. S'il y a des index de hachage, vous devez réévaluer périodiquement le nombre de compartiments. Pour plus d'informations, consultez [Determining the Correct Bucket Count for Hash Indexes](/previous-versions/sql/).  
  
 sys.dm_db_xtp_object_stats vous permet d'identifier les tables mémoire optimisées qui ont des conflits de lecture-lecture, pouvant affecter les performances de votre application. Par exemple, si vous utilisez une logique de nouvelle tentative des transactions, il est possible que la même instruction doive être exécutée plus d'une fois. En outre, vous pouvez utiliser ces informations pour identifier les tables (et par conséquent la logique métier) qui nécessite une gestion des erreurs de lecture-lecture.  
  
 La vue contient une ligne pour chaque table optimisée en mémoire de la base de données.  
  
 Pour plus d’informations, consultez [OLTP en mémoire &#40;optimisation en mémoire&#41;](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md).  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|object_id|**bigint**|ID de l'objet.|  
|row_insert_attempts|**bigint**|Nombre de lignes insérées dans la table depuis le dernier redémarrage de la base de données par les transactions validées et abandonnées.|  
|row_update_attempts|**bigint**|Nombre de lignes mises à jour dans la table depuis le dernier redémarrage de la base de données par les transactions validées et abandonnées.|  
|row_delete_attempts|**bigint**|Nombre de lignes supprimées de la table depuis le dernier redémarrage de la base de données par les transactions validées et abandonnées.|  
|write_conflicts|**bigint**|Nombre de conflits d'écriture qui se sont produits depuis le dernier redémarrage de la base de données.|  
|unique_constraint_violations|**bigint**|Nombre de violations de contrainte unique qui se sont produites depuis le dernier redémarrage de la base de données.|  
|object_address|**varbinary (8)**|À usage interne uniquement|  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'autorisation VIEW DATABASE STATE sur la base de données active.  
  
## <a name="see-also"></a>Voir aussi  
 [Vues de gestion dynamique des tables à mémoire optimisée &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/memory-optimized-table-dynamic-management-views-transact-sql.md)  
  
