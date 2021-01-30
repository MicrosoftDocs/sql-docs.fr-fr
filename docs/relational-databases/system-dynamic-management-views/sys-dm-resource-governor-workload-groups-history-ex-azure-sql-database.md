---
description: sys.dm_resource_governor_workload_groups_history_ex (Azure SQL Database)
title: sys.dm_resource_governor_workload_groups_history_ex (Azure SQL Database) | Microsoft Docs
ms.custom: ''
ms.date: 01/05/2021
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_resource_governor_workload_groups_history_ex_TSQL
- sys.dm_resource_governor_workload_groups_history_ex
- dm_resource_governor_workload_groups_history_ex
- dm_resource_governor_workload_groups_history_ex_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
- sys.dm_resource_governor_workload_groups_history_ex dynamic management view
author: joesackmsft
ms.author: josack
ms.openlocfilehash: ac8bc675c5a059c211e6ecfbed063d54daf6ee92
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203310"
---
# <a name="sysdm_resource_governor_workload_groups_history_ex-azure-sql-database"></a>sys.dm_resource_governor_workload_groups_history_ex (Azure SQL Database)
[!INCLUDE[Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/asdb-asdbmi.md)]

Chaque ligne représente un instantané périodique des statistiques des groupes de charges de travail dans Azure SQL Database. Une capture instantanée est effectuée au démarrage du moteur de base de données, puis toutes les quelques secondes. L’intervalle entre l’instantané actuel et l’instantané précédent peut varier et est fourni dans la `duration_ms` colonne. Les derniers instantanés disponibles sont retournés, jusqu’à 128 instantanés pour chaque groupe de charges de travail.
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**pool_id**| int |ID du pool de ressources. N'accepte pas la valeur NULL.|
|**group_id**| int |ID du groupe de charges de travail. N'accepte pas la valeur NULL.|
|**name**| nvarchar(256) |Nom du groupe de charges de travail. N'accepte pas la valeur NULL.|
|**snapshot_time**| DATETIME |Date et heure de la capture instantanée des statistiques du groupe de ressources.|
|**duration_ms**| int |Durée entre l’instantané actuel et l’instantané précédent.|
|**active_worker_count**| int |Nombre total de travailleurs dans l’instantané actuel.|
|**active_request_count**| int |Nombre de demandes en cours. N'accepte pas la valeur NULL.|
|**active_session_count**| int |Nombre total de sessions actives dans l’instantané actuel.|
|**total_request_count**| bigint |Nombre cumulatif de demandes traitées dans le groupe de charges de travail. N'accepte pas la valeur NULL.|
|**delta_request_count**| int |Nombre de demandes terminées dans le groupe de charge de travail depuis le dernier instantané. N'accepte pas la valeur NULL.|
|**total_cpu_usage_ms**| bigint |Utilisation cumulative de l'UC, en millisecondes, par ce groupe de charges de travail. N'accepte pas la valeur NULL.|
|**delta_cpu_usage_ms**| int |Utilisation de l’UC en millisecondes depuis le dernier instantané. N'accepte pas la valeur NULL.|
|**delta_cpu_usage_preemptive_ms**| int |Les appels de PreEmptive Win32 ne sont pas régis par le RG de l’UC SQL depuis le dernier instantané.|
|**delta_reads_reduced_memgrant_count**| int |Nombre d’allocations de mémoire ayant atteint la limite de taille de requête maximale depuis le dernier instantané. N'accepte pas la valeur NULL.|
|**reads_throttled**| int |Nombre total de lectures limitées.|
|**delta_reads_queued**| int |Nombre total d’IOs lus mis en file d’attente depuis le dernier instantané. Autorise la valeur NULL. NULL si le groupe de ressources n’est pas régi pour les e/s.|
|**delta_reads_issued**| int |Nombre total d’IOs lu émis depuis le dernier instantané. Autorise la valeur NULL. NULL si le groupe de ressources n’est pas régi pour les e/s.|
|**delta_reads_completed**| int |L’e/s de lecture totale s’est terminée depuis le dernier instantané. N'accepte pas la valeur NULL.|
|**delta_read_bytes**| bigint |Nombre total d’octets lus depuis le dernier instantané. N'accepte pas la valeur NULL.|
|**delta_read_stall_ms**| int |Durée totale (en millisecondes) entre l’arrivée et l’achèvement des e/s de lecture depuis le dernier instantané. N'accepte pas la valeur NULL.|
|**delta_read_stall_queued_ms**| int |Durée totale (en millisecondes) entre l’arrivée des e/s de lecture et le problème depuis le dernier instantané. Autorise la valeur NULL. NULL si le groupe de ressources n’est pas régi pour les e/s. Une delta_read_stall_queued_ms différente de zéro signifie que les e/s sont affectées par RG.|
|**delta_writes_queued**| int |Nombre total d’IOs d’écriture mis en file d’attente depuis le dernier instantané. Autorise la valeur NULL. NULL si le groupe de ressources n’est pas régi pour les e/s.|
|**delta_writes_issued**| int |Nombre total d’e/s d’écriture émises depuis le dernier instantané. Autorise la valeur NULL. NULL si le groupe de ressources n’est pas régi pour les e/s.|
|**delta_writes_completed**| int |L’e/s d’écriture totale est terminée depuis le dernier instantané. N'accepte pas la valeur NULL.|
|**delta_writes_bytes**| bigint |Nombre total d’octets écrits depuis la dernière capture instantanée. N'accepte pas la valeur NULL.|
|**delta_write_stall_ms**| int |Durée totale (en millisecondes) entre l’arrivée des e/s d’écriture et la fin depuis le dernier instantané. N'accepte pas la valeur NULL.|
|**delta_background_writes**| int |Nombre total d’écritures effectuées par les tâches en arrière-plan depuis le dernier instantané.|
|**delta_background_write_bytes**| bigint |Taille totale des écritures effectuées par les tâches en arrière-plan depuis le dernier instantané, en octets.|
|**delta_log_bytes_used**| bigint |Journal utilisé depuis le dernier instantané, en octets.|
|**delta_log_temp_db_bytes_used**| bigint |Journal tempdb utilisé depuis le dernier instantané, en octets.|
|**delta_query_optimizations**| bigint |Nombre d’optimisations de requêtes dans ce groupe de charges de travail depuis le dernier instantané. N'accepte pas la valeur NULL.|
|**delta_suboptimal_plan_generations**| bigint |Nombre de générations de plans non optimaux qui se sont produites dans ce groupe de charges de travail en raison de la sollicitation de la mémoire depuis le dernier instantané. N'accepte pas la valeur NULL.
|**max_memory_grant_kb**| bigint |Allocation de mémoire maximale pour le groupe en Ko.|
|**max_request_cpu_msec**| bigint |Utilisation maximale de l'UC, en millisecondes, pour une demande unique. N'accepte pas la valeur NULL.|
|**max_concurrent_request**| int |Paramètre actuel du nombre maximal de demandes simultanées. N'accepte pas la valeur NULL.|
|**max_io**| int |Limite maximale d’e/s pour le groupe.|
|**max_global_io**| int |Identifié à titre d'information uniquement. Non pris en charge. La compatibilité future n'est pas garantie.
|**max_queued_io**| int |Identifié à titre d'information uniquement. Non pris en charge. La compatibilité future n'est pas garantie.|
|**max_log_rate_kb**| bigint |Taux de journal maximal (kilo-octets par seconde) au niveau du groupe de ressources.|
|**max_session**| int |Limite de session pour le groupe.|
|**max_worker**| int |Limite de travail pour le groupe.|
|||

## <a name="permissions"></a>Autorisations

Cette vue nécessite l’autorisation VIEW SERVER STATE.

## <a name="remarks"></a>Notes

Les utilisateurs peuvent accéder à cette vue de gestion dynamique pour surveiller la consommation des ressources en temps quasi réel pour le pool de charges de travail utilisateur, ainsi que pour les pools internes du système de Azure SQL Database instance.

> [!IMPORTANT]
> La plupart des données en surface par cette DMV sont destinées à la consommation interne et peuvent faire l’objet de modifications.

## <a name="examples"></a>Exemples

L’exemple suivant retourne la quantité maximale de données de taux de journal et la consommation de chaque instantané par pool d’utilisateurs :

```sql
SELECT snapshot_time,
       name,
       max_log_rate_kb,
       delta_log_bytes_used
FROM sys.dm_resource_governor_workload_groups_history_ex
WHERE name LIKE 'User%'
ORDER BY snapshot_time DESC;
```

## <a name="see-also"></a>Voir aussi

- [Gouvernance du taux de journal des traductions](/azure/sql-database/sql-database-resource-limits-database-server#transaction-log-rate-governance)
- [Limites des ressources DTU des pools élastiques](/azure/sql-database/sql-database-dtu-resource-limits-elastic-pools)
- [Limites des ressources vCore des pools élastiques](/azure/sql-database/sql-database-vcore-resource-limits-elastic-pools)