---
description: sys.dm_user_db_resource_governance (Transact-SQL)
title: sys.dm_user_db_resource_governance (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/17/2019
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.resource_governance
- sys.resource_governance_TSQL
- resource_governance
- resource_governance_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.resource_governance catalog view
ms.assetid: ''
author: joesackmsft
ms.author: josack
monikerRange: =azuresqldb-current
ms.openlocfilehash: 1044577add08fe1c9d6337f67244a185d98db8e4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100065558"
---
# <a name="sysdm_user_db_resource_governance-transact-sql"></a>sys.dm_user_db_resource_governance (Transact-SQL)

[!INCLUDE[appliesto-xx-asdb-xxxx-xxx-md](../../includes/appliesto-xx-asdb-xxxx-xxx-md.md)]

Retourne les paramètres réels de configuration et de capacité utilisés par les mécanismes de gouvernance des ressources dans la base de données ou le pool élastique actuel.
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**database_id**|int|ID de la base de données, unique au sein d’un serveur Azure SQL Database.|
|**logical_database_guid**|UNIQUEIDENTIFIER|GUID logique pour la base de données utilisateur qui reste au sein de la durée de vie d’une base de données utilisateur.  Si vous renommez la base de données ou modifiez son objectif de niveau de service, cette valeur ne sera pas modifiée.|
|**physical_database_guid**|UNIQUEIDENTIFIER|GUID physique d’une base de données utilisateur qui reste dans la durée de vie de l’instance physique de la base de données utilisateur. La modification de l’objectif de niveau de service de base de données entraîne la modification de cette valeur.|
|**server_name**|NVARCHAR|Nom du serveur logique.|
|**database_name**|NVARCHAR|Nom de la base de données logique.|
|**slo_name**|NVARCHAR|Objectif de niveau de service, y compris la génération de matériel.|
|**dtu_limit**|int|Limite de DTU de la base de données (NULL pour vCore).|
|**cpu_limit**|int|limite vCore de base de données (NULL pour les bases de données DTU).|
|**min_cpu**|TINYINT|Valeur MIN_CPU_PERCENT du pool de ressources de la charge de travail utilisateur. Consultez [concepts du pool de ressources](../resource-governor/resource-governor-resource-pool.md#resource-pool-concepts).|
|**max_cpu**|TINYINT|Valeur MAX_CPU_PERCENT du pool de ressources de la charge de travail utilisateur. Consultez [concepts du pool de ressources](../resource-governor/resource-governor-resource-pool.md#resource-pool-concepts).|
|**cap_cpu**|TINYINT|Valeur CAP_CPU_PERCENT du pool de ressources de la charge de travail utilisateur. Consultez [concepts du pool de ressources](../resource-governor/resource-governor-resource-pool.md#resource-pool-concepts).|
|**min_cores**|SMALLINT|À usage interne uniquement|
|**max_dop**|SMALLINT|Valeur MAX_DOP pour le groupe de charge de travail utilisateur. Consultez [créer un groupe de charge de travail](../../t-sql/statements/create-workload-group-transact-sql.md).|
|**min_memory**|int|Valeur MIN_MEMORY_PERCENT du pool de ressources de la charge de travail utilisateur. Consultez [concepts du pool de ressources](../resource-governor/resource-governor-resource-pool.md#resource-pool-concepts).|
|**max_memory**|int|Valeur MAX_MEMORY_PERCENT du pool de ressources de la charge de travail utilisateur. Consultez [concepts du pool de ressources](../resource-governor/resource-governor-resource-pool.md#resource-pool-concepts).|
|**max_sessions**|int|Nombre maximal de sessions autorisées dans le groupe de charges de travail utilisateur.|
|**max_memory_grant**|int|Valeur REQUEST_MAX_MEMORY_GRANT_PERCENT pour le groupe de charge de travail utilisateur. Consultez [créer un groupe de charge de travail](../../t-sql/statements/create-workload-group-transact-sql.md).|
|**max_db_memory**|int|À usage interne uniquement|
|**govern_background_io**|bit|À usage interne uniquement|
|**min_db_max_size_in_mb**|bigint|Valeur minimale max_size d’un fichier de données, en Mo. Consultez [sys.database_files](../system-catalog-views/sys-database-files-transact-sql.md).|
|**max_db_max_size_in_mb**|bigint|Valeur maximale max_size d’un fichier de données, en Mo. Consultez [sys.database_files](../system-catalog-views/sys-database-files-transact-sql.md).|
|**default_db_max_size_in_mb**|bigint|Valeur par défaut max_size d’un fichier de données, en Mo. Consultez [sys.database_files](../system-catalog-views/sys-database-files-transact-sql.md).|
|**db_file_growth_in_mb**|bigint|Incrément de croissance par défaut pour un fichier de données, en Mo. Consultez [sys.database_files](../system-catalog-views/sys-database-files-transact-sql.md).|
|**initial_db_file_size_in_mb**|bigint|Taille par défaut du nouveau fichier de données, en Mo. Consultez [sys.database_files](../system-catalog-views/sys-database-files-transact-sql.md).|
|**log_size_in_mb**|bigint|Taille par défaut du nouveau fichier journal, en Mo. Consultez [sys.database_files](../system-catalog-views/sys-database-files-transact-sql.md).|
|**instance_cap_cpu**|int|À usage interne uniquement|
|**instance_max_log_rate**|bigint|Limite du taux de génération de journaux pour l’instance de SQL Server, en octets par seconde. S’applique à tous les journaux générés par l’instance, y compris les `tempdb` autres bases de données système. Dans un pool élastique, s’applique au journal généré par toutes les bases de données dans le pool.|
|**instance_max_worker_threads**|int|Limite de threads de travail pour l’instance SQL Server.|
|**replica_type**|int|Type de réplica, où 0 est principal et 1 est secondaire.|
|**max_transaction_size**|bigint|Espace maximal du journal utilisé par une transaction, en Ko.|
|**checkpoint_rate_mbps**|int|À usage interne uniquement|
|**checkpoint_rate_io**|int|À usage interne uniquement|
|**last_updated_date_utc**|DATETIME|Date et heure du dernier changement ou de la reconfiguration du paramètre, en heure UTC.|
|**primary_group_id**|int|ID de groupe de charge de travail de la charge de travail utilisateur sur le réplica principal et sur les réplicas secondaires.|
|**primary_group_max_workers**|int|Limite de threads de travail pour le groupe de charge de travail utilisateur.|
|**primary_min_log_rate**|bigint|Taux de journal minimal, en octets par seconde, au niveau du groupe de charge de travail utilisateur. La gouvernance des ressources ne tentera pas de réduire la fréquence des journaux en dessous de cette valeur.|
|**primary_max_log_rate**|bigint|Taux de journal maximal, en octets par seconde, au niveau du groupe de charge de travail utilisateur. La gouvernance des ressources n’autorise pas le taux de journal au-dessus de cette valeur.|
|**primary_group_min_io**|int|Nombre minimal d’e/s par seconde pour le groupe de charge de travail utilisateur. La gouvernance des ressources ne tentera pas de réduire les e/s par seconde en dessous de cette valeur.|
|**primary_group_max_io**|int|Nombre maximal d’e/s par seconde pour le groupe de charge de travail utilisateur. La gouvernance des ressources n’autorise pas l’IOPS au-dessus de cette valeur.|
|**primary_group_min_cpu**|float|Pourcentage d’UC minimal pour le niveau de groupe de charges de travail utilisateur. La gouvernance des ressources ne tentera pas de réduire l’utilisation de l’UC au-dessous de cette valeur.|
|**primary_group_max_cpu**|float|Pourcentage processeur maximal pour le niveau de groupe de charges de travail utilisateur. La gouvernance des ressources n’autorise pas l’utilisation du processeur au-dessus de cette valeur.|
|**primary_log_commit_fee**|int|Frais de validation de la gouvernance du taux de journal pour le groupe de charge de travail utilisateur, en octets. Un tarif de validation augmente la taille de chaque e/s de journal d’une valeur fixe uniquement dans le cadre de la gestion des journaux de taux de journal. Les e/s du journal réelles dans le stockage ne sont pas augmentées.|
|**primary_pool_max_workers**|int|Limite du thread de travail pour le pool de ressources de la charge de travail utilisateur.|
|**pool_max_io**|int|Limite maximale d’e/s par seconde pour le pool de ressources de la charge de travail utilisateur.|
|**govern_db_memory_in_resource_pool**|bit|À usage interne uniquement|
|**volume_local_iops**|int|À usage interne uniquement|
|**volume_managed_xstore_iops**|int|À usage interne uniquement|
|**volume_external_xstore_iops**|int|À usage interne uniquement|
|**volume_type_local_iops**|int|À usage interne uniquement|
|**volume_type_managed_xstore_iops**|int|À usage interne uniquement|
|**volume_type_external_xstore_iops**|int|À usage interne uniquement|
|**volume_pfs_iops**|int|À usage interne uniquement|
|**volume_type_pfs_iops**|int|À usage interne uniquement|
|||

## <a name="permissions"></a>Autorisations

Cette vue nécessite l'autorisation VIEW DATABASE STATE.

## <a name="remarks"></a>Notes

Pour obtenir une description de la gouvernance des ressources dans Azure SQL Database, consultez [limites des ressources de SQL Database](/azure/sql-database/sql-database-resource-limits-database-server).

> [!IMPORTANT]
> La plupart des données retournées par cette DMV sont destinées à la consommation interne et sont susceptibles de changer à tout moment.

## <a name="examples"></a>Exemples

La requête suivante, exécutée dans le contexte d’une base de données utilisateur, retourne le taux maximal de journalisation et le nombre maximal d’e/s par seconde au niveau du groupe de charges de travail et du pool de ressources utilisateur. Pour une base de données unique, une ligne est retournée. Pour une base de données dans un pool élastique, une ligne est retournée pour chaque base de données dans le pool.

```
SELECT database_name,
       primary_group_id,
       primary_max_log_rate,
       primary_group_max_io,
       pool_max_io
FROM sys.dm_user_db_resource_governance
ORDER BY database_name;  
```

## <a name="see-also"></a> Voir aussi

- [gouverneur de ressources](../resource-governor/resource-governor.md)
- [sys.dm_resource_governor_resource_pools (Transact-SQL)](./sys-dm-resource-governor-resource-pools-transact-sql.md)
- [sys.dm_resource_governor_workload_groups (Transact-SQL)](./sys-dm-resource-governor-workload-groups-transact-sql.md)
- [sys.dm_resource_governor_resource_pools_history_ex (Transact-SQL)](./sys-dm-resource-governor-resource-pools-history-ex-azure-sql-database.md)
- [sys.dm_resource_governor_workload_groups_history_ex (Azure SQL Database)](./sys-dm-resource-governor-workload-groups-history-ex-azure-sql-database.md)
- [Gouvernance relative au taux de journalisation des transactions](/azure/sql-database/sql-database-resource-limits-database-server#transaction-log-rate-governance)
- [Limites des ressources DTU des bases de données uniques](/azure/sql-database/sql-database-dtu-resource-limits-single-databases)
- [Limites des ressources vCore des bases de données uniques](/azure/sql-database/sql-database-vcore-resource-limits-single-databases)