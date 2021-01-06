---
description: sys.dm_resource_governor_resource_pools_history_ex (Transact-SQL)
title: sys.dm_resource_governor_resource_pools_history_ex (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/05/2021
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.resource_governor
- sys.resource_governor_TSQL
- resource_governor
- resource_governor_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.resource_governor catalog view
ms.assetid: ''
author: joesackmsft
ms.author: josack
monikerRange: =azuresqldb-current
ms.openlocfilehash: 32082b09a5e583e68f2f06794d80d1d3f21b4e76
ms.sourcegitcommit: 11ca2305a8d7e420daf772eb97861706c9e08e31
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/06/2021
ms.locfileid: "97928438"
---
# <a name="sysdm_resource_governor_resource_pools_history_ex-transact-sql"></a>sys.dm_resource_governor_resource_pools_history_ex (Transact-SQL)

[!INCLUDE[appliesto-xx-asdb-xxxx-xxx-md](../../includes/appliesto-xx-asdb-xxxx-xxx-md.md)]

Chaque ligne représente un instantané périodique des statistiques du pool de ressources dans Azure SQL Database. Une capture instantanée est effectuée au démarrage du moteur de base de données, puis toutes les quelques secondes. L’intervalle entre l’instantané actuel et l’instantané précédent peut varier et est fourni dans la `duration_ms` colonne. Les derniers instantanés disponibles sont retournés, jusqu’à 128 instantanés pour chaque pool de ressources.

|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**pool_id**|int|ID du pool de ressources. N'accepte pas la valeur NULL.
|**name**|sysname|Nom du pool de ressources. N'accepte pas la valeur NULL.|
|**snapshot_time**|datetime2|Date et heure de l’instantané des statistiques du pool de ressources|
|**duration_ms**|int|Durée entre l’instantané actuel et l’instantané précédent|
|**statistics_start_time**|datetime2|Heure à laquelle les statistiques ont été réinitialisées pour ce pool. N'accepte pas la valeur NULL.|
|**active_session_count**|int|Nombre total de sessions actives dans l’instantané actuel|
|**active_worker_count**|int|Nombre total de Workers dans l’instantané actuel|
|**delta_cpu_usage_ms**|int|Utilisation de l’UC en millisecondes depuis le dernier instantané. N'accepte pas la valeur NULL.|
|**delta_cpu_usage_preemptive_ms**|int|Appels de préemption Win32 non régis par le RG de l’UC SQL depuis le dernier instantané|
|**used_data_space_kb**|bigint|Espace total utilisé dans les bases de données utilisateur associées au pool d’utilisateurs|
|**allocated_disk_space_kb**|bigint|Taille totale du fichier de données des bases de données utilisateur dans le associé au pool d’utilisateurs|
|**target_memory_kb**|bigint|Quantité de mémoire cible, en kilo-octets, que le pool de ressources tente d'atteindre. Cette valeur est basée sur les paramètres actuels et l'état du serveur. N'accepte pas la valeur NULL.|
|**used_memory_kb**|bigint|Quantité de mémoire utilisée, en kilo-octets, pour le pool de ressources. N'accepte pas la valeur NULL.|
|**cache_memory_kb**|bigint|Utilisation de la mémoire cache totale actuelle en kilo-octets. N'accepte pas la valeur NULL.|
|**compile_memory_kb**|bigint|Utilisation de la mémoire occultée totale actuelle en kilo-octets (Ko). Cette utilisation est essentiellement destinée à la compilation et l'optimisation, mais d'autres utilisations de la mémoire peuvent exister. N'accepte pas la valeur NULL.|
|**active_memgrant_count**|bigint|Nombre actuel d'allocations de mémoire. N'accepte pas la valeur NULL.|
|**active_memgrant_kb**|bigint|Somme, en kilo-octets (Ko), des allocations de mémoire actuelles. N'accepte pas la valeur NULL.|
|**used_memgrant_kb**|bigint|Quantité totale de la mémoire utilisée (occultée) actuelle provenant des allocations de mémoire. N'accepte pas la valeur NULL.|
|**delta_memgrant_timeout_count**|int|nombre de délais d’expiration d’allocation de mémoire dans ce pool de ressources au cours de cette période. N'accepte pas la valeur NULL.|
|**delta_memgrant_waiter_count**|int|Nombre de requêtes actuellement en attente d'allocations de mémoire. N'accepte pas la valeur NULL.|
|**delta_out_of_memory_count**|int|Nombre d’échecs d’allocation de mémoire dans le pool depuis la dernière capture instantanée. N'accepte pas la valeur NULL.|
|**delta_read_io_queued**|int|Nombre total d’IOs lus mis en file d’attente depuis le dernier instantané. Autorise la valeur NULL. Null si le pool de ressources n'est pas régi pour les E/S.|
|**delta_read_io_issued**|int|Nombre total d’IOs lu émis depuis le dernier instantané. Autorise la valeur NULL. Null si le pool de ressources n'est pas régi pour les E/S.|
|**delta_read_io_completed**|int|L’e/s de lecture totale s’est terminée depuis le dernier instantané. N'accepte pas la valeur NULL.|
|**delta_read_io_throttled**|int|Nombre total d’IOs de lecture limitées depuis la capture instantanée. Autorise la valeur NULL. Null si le pool de ressources n'est pas régi pour les E/S.|
|**delta_read_bytes**|bigint|Nombre total d’octets lus depuis le dernier instantané. N'accepte pas la valeur NULL.|
|**delta_read_io_stall_ms**|int|Durée totale (en millisecondes) entre l’arrivée et l’achèvement des e/s de lecture depuis le dernier instantané. N'accepte pas la valeur NULL.|
|**delta_read_io_stall_queued_ms**|int|Durée totale (en millisecondes) entre l’arrivée des e/s de lecture et le problème depuis le dernier instantané. Autorise la valeur NULL. Null si le pool de ressources n'est pas régi pour les E/S. Une delta_read_io_stall_queued_ms différente de zéro signifie que les e/s sont affectées par RG.|
|**delta_write_io_queued**|int|Nombre total d’IOs d’écriture mis en file d’attente depuis le dernier instantané. Autorise la valeur NULL. Null si le pool de ressources n'est pas régi pour les E/S.|
|**delta_write_io_issued**|int|Nombre total d’e/s d’écriture émises depuis le dernier instantané. Autorise la valeur NULL. Null si le pool de ressources n'est pas régi pour les E/S.|
|**delta_write_io_completed**|int|L’e/s d’écriture totale est terminée depuis le dernier instantané. N'accepte pas la valeur NULL|
|**delta_write_io_throttled**|int|Nombre total d’IOs d’écriture limité depuis le dernier instantané. N'accepte pas la valeur NULL|
|**delta_write_bytes**|bigint|Nombre total d’octets écrits depuis la dernière capture instantanée. N'accepte pas la valeur NULL.|
|**delta_write_io_stall_ms**|int|Durée totale (en millisecondes) entre l’arrivée des e/s d’écriture et la fin depuis le dernier instantané. N'accepte pas la valeur NULL.|
|**delta_write_io_stall_queued_ms**|int|Durée totale (en millisecondes) entre l’arrivée des e/s d’écriture et le problème depuis le dernier instantané. Autorise la valeur NULL. Null si le pool de ressources n'est pas régi pour les E/S.|
|**delta_io_issue_delay_ms**|int|Durée totale (en millisecondes) entre le problème planifié et le problème réel des e/s depuis le dernier instantané. Autorise la valeur NULL. Null si le pool de ressources n'est pas régi pour les E/S.|
|**max_iops_per_volume**|int|Paramètre maximum d’e/s par seconde (IOPS) par volume de disque pour ce pool. Autorise la valeur NULL. Null si le pool de ressources n'est pas régi pour les E/S.|
|**max_memory_kb**|bigint|Quantité maximale de mémoire, en kilo-octets, dont peut disposer le pool de ressources. Cette valeur est basée sur les paramètres actuels et l'état du serveur. N'accepte pas la valeur NULL.
|**max_log_rate_kb**|bigint|Taux de journal maximal (kilo-octets par seconde) au niveau du pool de ressources.|
|**max_data_space_kb**|bigint|Paramètre de limite de stockage du pool élastique maximal pour ce pool élastique, en kilo-octets.|
|**max_session**|int|Limite de session pour le pool|
|**max_worker**|int|Limite de travail pour le pool|
|**min_cpu_percent**|int|Configuration actuelle de la bande passante de l'UC moyenne garantie pour toutes les demandes dans le pool de ressources en cas de contention du processeur. N'accepte pas la valeur NULL.|
|**max_cpu_percent**|int|Configuration actuelle de la bande passante processeur moyenne maximale pour toutes les demandes dans le pool de ressources en cas de contention du processeur. N'accepte pas la valeur NULL.|
|**cap_cpu_percent**|int|Limite maximale d'utilisation fixe sur la bande passante de l'UC que toutes les demandes dans le pool de ressources recevront. Limite le niveau de bande passante processeur maximal au niveau spécifié. La plage autorisée pour la valeur est comprise entre 1 et 100. N'accepte pas la valeur NULL.|
|**min_vcores**|décimal (5, 2)|Configuration actuelle de la bande passante de l'UC moyenne garantie pour toutes les demandes dans le pool de ressources en cas de contention du processeur.  En unités de vCores|
|**max_vcores**|décimal (5, 2)|Configuration actuelle de la bande passante processeur moyenne maximale pour toutes les demandes dans le pool de ressources en cas de contention du processeur.  Dans l’unité de vCores|
|**cap_vcores**|décimal (5, 2)|Limite maximale d'utilisation fixe sur la bande passante de l'UC que toutes les demandes dans le pool de ressources recevront.  Dans l’unité sur vCores|
|**instance_cpu_count**|int|Nombre de PROCESSEURs configurés pour l’instance|
|**instance_cpu_percent**|décimal (5, 2)|Pourcentage d’UC configuré pour l’instance|
|**instance_vcores**|décimal (5, 2)|Nombre de vCores configurés pour l’instance|
|**delta_log_bytes_used**|décimal (5, 2)|Génération totale du journal (en octets) au niveau du pool depuis le dernier instantané|
|**avg_login_rate_percent**|décimal (5, 2)|Nombre de connexions depuis le dernier instantané, par rapport à la limite de connexion|
|**delta_vcores_used**|décimal (5, 2)|Utilisation du calcul du nombre de vCores depuis le dernier instantané.|
|**cap_vcores_used_percent**|décimal (5, 2)|Utilisation moyenne des ressources de calcul en pourcentage de la limite du pool.|
|**instance_vcores_used_percent**|décimal (5, 2)|Utilisation moyenne du calcul en pourcentage de la limite de l’instance SQL.|
|**avg_data_io_percent**|décimal (5, 2)|Utilisation moyenne des E/S en pourcentage de la limite du pool.|
|**avg_log_write_percent**|décimal (5, 2)|Utilisation moyenne des ressources d’écriture en pourcentage de la limite du pool.|
|**avg_storage_percent**|décimal (5, 2)|Utilisation moyenne du stockage en pourcentage de la limite de stockage du pool.|
|**avg_allocated_storage_percent**|décimal (5, 2)|Pourcentage d’espace de données alloué par toutes les bases de données dans le pool élastique. Il s’agit du rapport entre l’espace de données alloué et la taille maximale des données pour le pool élastique. Pour plus d’informations, consultez Gestion de l’espace de fichiers dans SQL Database|
|**max_worker_percent**|décimal (5, 2)|Nombre maximal d’ouvriers simultanés (demandes) en pourcentage de la limite du pool.|
|**max_session_percent**|décimal (5, 2)|Nombre maximal de sessions simultanées en pourcentage de la limite du pool.|
|||

## <a name="permissions"></a>Autorisations

Cette vue nécessite l’autorisation VIEW SERVER STATE.

## <a name="remarks"></a>Notes

Les utilisateurs peuvent accéder à cette vue de gestion dynamique pour surveiller la consommation des ressources en temps quasi réel pour le pool de charges de travail utilisateur, ainsi que pour les pools internes du système de Azure SQL Database instance.

> [!IMPORTANT]
> La plupart des données en surface par cette DMV sont destinées à la consommation interne et peuvent faire l’objet de modifications.

## <a name="examples"></a>Exemples

L’exemple suivant retourne la quantité maximale de données de taux de journal et la consommation de chaque instantané par pool d’utilisateurs  

```sql
select snapshot_time, name, max_log_rate_kb, delta_log_bytes_used from sys.dm_resource_governor_resource_pools_history_ex where name like 'UserPool%' order by snapshot_time desc
```

L’exemple suivant retourne des informations similaires en tant que sys.elastic_pool_resource_stats sans avoir à se connecter au maître logique

```sql
select snapshot_time, name, cap_vcores_used_percent,
  avg_data_io_percent,  
  avg_log_write_percent,
  avg_storage_percent,
  avg_allocated_storage_percent,
  max_data_space_kb,
  max_worker_percent,
  max_session_percent
    from sys.dm_resource_governor_resource_pools_history_ex where name like 'UserPool%' order by snapshot_time desc
```

## <a name="see-also"></a>Voir aussi

- [Gouvernance du taux de journal des traductions](/azure/sql-database/sql-database-resource-limits-database-server#transaction-log-rate-governance)
- [Limites des ressources DTU des pools élastiques](/azure/sql-database/sql-database-dtu-resource-limits-elastic-pools)
- [Limites des ressources vCore des pools élastiques](/azure/sql-database/sql-database-vcore-resource-limits-elastic-pools)