---
description: sys.elastic_pool_resource_stats (Azure SQL Database)
title: sys.elastic_pool_resource_stats
titleSuffix: Azure SQL Database
ms.date: 01/28/2019
ms.service: sql-database
ms.prod_service: sql-database
ms.reviewer: ''
ms.topic: reference
f1_keywords:
- sys.elastic_pool_resource_stats catalog view
helpviewer_keywords:
- sys.elastic_pool_resource_stats_TSQL
- sys.elastic_pool_resource_stats
- elastic_pool_resource_stats_TSQL
- elastic_pool_resource_stats
ms.assetid: f242c1bd-3cc8-4c8b-8aaf-c79b6a8a0329
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.custom: seo-dt-2019
monikerRange: = azuresqldb-current
ms.openlocfilehash: 1b97fe07119f28211186e2efa3986308af477947
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98102767"
---
# <a name="syselastic_pool_resource_stats-azure-sql-database"></a>sys.elastic_pool_resource_stats (Azure SQL Database)
[!INCLUDE[Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/asdb-asdbmi.md)]

  Renvoie les statistiques d’utilisation de ressources pour tous les pools élastiques dans un serveur SQL Database. Pour chaque pool élastique, il existe une ligne pour chaque fenêtre de création de rapports de 15 secondes (quatre lignes par minute). Cela inclut la consommation de stockage, le journal, les E/S, l’UC et l’utilisation de session/requête simultanée par toutes les bases de données du pool. Ces données sont conservées pendant 14 jours. 
  
||  
|-|  
|**S’applique à**:  [!INCLUDE[ssSDS](../../includes/sssds-md.md)] v12.|  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**heure-début**|**datetime2**|Heure UTC indiquant le début de l’intervalle de création de rapports de 15 secondes.|  
|**heure-fin**|**datetime2**|Heure UTC indiquant la fin de l’intervalle de création de rapports de 15 secondes.|  
|**elastic_pool_name**|**nvarchar(128)**|Nom du pool de bases de données élastiques.|  
|**avg_cpu_percent**|**décimal (5, 2)**|Utilisation moyenne des ressources de calcul en pourcentage de la limite du pool.|  
|**avg_data_io_percent**|**décimal (5, 2)**|Utilisation moyenne des E/S en pourcentage de la limite du pool.|  
|**avg_log_write_percent**|**décimal (5, 2)**|Utilisation moyenne des ressources d’écriture en pourcentage de la limite du pool.|  
|**avg_storage_percent**|**décimal (5, 2)**|Utilisation moyenne du stockage en pourcentage de la limite de stockage du pool.|  
|**max_worker_percent**|**décimal (5, 2)**|Nombre maximal d’ouvriers simultanés (demandes) en pourcentage de la limite du pool.|  
|**max_session_percent**|**décimal (5, 2)**|Nombre maximal de sessions simultanées en pourcentage de la limite du pool.|  
|**elastic_pool_dtu_limit**|**int**|Nombre maximal de DTU de pool élastique pour ce pool élastique au cours de cet intervalle.|  
|**elastic_pool_storage_limit_mb**|**bigint**|Espace de stockage de pool élastique maximal pour ce pool élastique (en Mo) au cours de cet intervalle.|
|**avg_allocated_storage_percent**|**décimal (5, 2)**|Pourcentage d’espace de données alloué par toutes les bases de données dans le pool élastique.  Il s’agit du rapport entre l’espace de données alloué et la taille maximale des données pour le pool élastique.  Pour plus d’informations [, consultez Gestion de l’espace de fichiers dans SQL Database](/azure/sql-database/sql-database-file-space-management)|  
  
## <a name="remarks"></a>Notes

 Cette vue existe dans la base de données Master du serveur SQL Database. Vous devez être connecté à la base de données Master pour interroger **sys.elastic_pool_resource_stats**.  
  
## <a name="permissions"></a>Autorisations

 Requiert l’appartenance au rôle **dbmanager** .  
  
## <a name="examples"></a>Exemples

 L’exemple suivant retourne les données d’utilisation des ressources classées par l’heure la plus récente pour tous les pools de bases de données élastiques du serveur de SQL Database actuel.  
  
```sql
SELECT * FROM sys.elastic_pool_resource_stats
ORDER BY end_time DESC;  
```

 L’exemple suivant calcule la consommation moyenne en pourcentage de DTU pour un pool donné.  

```sql
SELECT start_time, end_time,
  (SELECT Max(v)
FROM (VALUES (avg_cpu_percent), (avg_data_io_percent), (avg_log_write_percent)) AS value(v)) AS [avg_DTU_percent]
FROM sys.elastic_pool_resource_stats
WHERE elastic_pool_name = '<your pool name>'
ORDER BY end_time DESC;  
```

## <a name="see-also"></a>Voir aussi

 [Maîtriser la croissance explosive avec les bases de données élastiques](/azure/azure-sql/database/elastic-pool-overview)   
 [Créer et gérer un pool de bases de données élastiques SQL Database](/azure/azure-sql/database/elastic-pool-overview)   
 [sys.resource_stats &#40;Azure SQL Database&#41;](../../relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database.md)   
 [sys.dm_db_resource_stats &#40;Azure SQL Database&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database.md)  
  
