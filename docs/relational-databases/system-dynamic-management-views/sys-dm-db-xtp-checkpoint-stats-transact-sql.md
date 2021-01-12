---
title: sys.dm_db_xtp_checkpoint_stats (Transact-SQL) | Microsoft Docs
description: Retourne des statistiques sur les opérations de point de contrôle de l'OLTP en mémoire dans la base de données active. Découvrez en quoi cette vue diffère pour les versions de SQL Server.
ms.custom: ''
ms.date: 03/20/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: language-reference
f1_keywords:
- dm_db_xtp_checkpoint_stats
- dm_db_xtp_checkpoint_stats_TSQL
- sys.dm_db_xtp_checkpoint_stats
- sys.dm_db_xtp_checkpoint_stats_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_xtp_checkpoint_stats dynamic management view
ms.assetid: 8d0b18ca-db4d-4376-9905-3e4457727c46
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 08cd6d78f4da2d0f5dc68ad43db29bc0542411e9
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092896"
---
# <a name="sysdm_db_xtp_checkpoint_stats-transact-sql"></a>sys.dm_db_xtp_checkpoint_stats (Transact-SQL)
[!INCLUDE[sql-asdb-asdbmi](../../includes/applies-to-version/sql-asdb-asdbmi.md)]

  Retourne des statistiques sur les opérations de point de contrôle de l'OLTP en mémoire dans la base de données active. Si la base de données n'a pas d'objets OLTP en mémoire, retourne un ensemble de résultats vide.  
  
 Pour plus d’informations, consultez [OLTP en mémoire &#40;optimisation en mémoire&#41;](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md).  
  
```  
USE In_Memory_db_name
SELECT * FROM sys.dm_db_xtp_checkpoint_stats;  
```  
  
**[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] est fondamentalement différent des versions plus récentes et est abordé plus bas dans la rubrique [SQL Server 2014](#bkmk_2014).**
  
## <a name="sssql15-and-later"></a>[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] et versions ultérieures  
 Le tableau suivant décrit les colonnes de `sys.dm_db_xtp_checkpoint_stats` , à partir de **[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)]** .  
  
|Nom de la colonne|Type|Description|  
|-----------------|----------|-----------------|  
|last_lsn_processed|**bigint**|Dernier LSN détecté par le contrôleur.|  
|end_of_log_lsn|**numérique (38)**|LSN de la fin du journal.|  
|bytes_to_end_of_log|**bigint**|Octets du journal non traités par le contrôleur, correspondant aux octets compris entre `last_lsn_processed` et `end_of_log_lsn` .|  
|log_consumption_rate|**bigint**|Taux de consommation du journal des transactions par le contrôleur (en Ko/s).|  
|active_scan_time_in_ms|**bigint**|Temps passé par le contrôleur à analyser activement le journal des transactions.|  
|total_wait_time_in_ms|**bigint**|Temps d’attente cumulé pour le contrôleur sans analyser le journal.|  
|waits_for_io|**bigint**|Nombre d’attentes pour les e/s du journal effectuées par le thread du contrôleur.|  
|io_wait_time_in_ms|**bigint**|Temps cumulé passé à attendre l’e/s du journal par le thread du contrôleur.|  
|waits_for_new_log_count|**bigint**|Nombre d’attentes effectuées par le thread du contrôleur pour la génération d’un nouveau journal.|  
|new_log_wait_time_in_ms|**bigint**|Temps cumulatif passé à attendre un nouveau journal par le thread du contrôleur.|  
|idle_attempts_count|**bigint**|Nombre de fois où le contrôleur est passé à un état inactif.|  
|tx_segments_dispatched|**bigint**|Nombre de segments vus par le contrôleur et distribués aux sérialiseurs. Segment est une partie contiguë du journal qui forme une unité de sérialisation. Il est actuellement dimensionné à 1 Mo, mais peut changer à l’avenir.|  
|segment_bytes_dispatched|**bigint**|Nombre total d’octets envoyés par le contrôleur aux sérialiseurs, depuis le redémarrage de la base de données.|  
|bytes_serialized|**bigint**|Nombre total d’octets sérialisés depuis le redémarrage de la base de données.|  
|serializer_user_time_in_ms|**bigint**|Temps passé par les sérialiseurs en mode utilisateur.|  
|serializer_kernel_time_in_ms|**bigint**|Temps passé par les sérialiseurs en mode noyau.|  
|xtp_log_bytes_consumed|**bigint**|Nombre total d’octets de journal consommés depuis le redémarrage de la base de données.|  
|checkpoints_closed|**bigint**|Nombre de points de contrôle fermés depuis le redémarrage de la base de données.|  
|last_closed_checkpoint_ts|**bigint**|Horodateur du dernier point de contrôle fermé.|  
|hardened_recovery_lsn|**numérique (38)**|La récupération va démarrer à partir de ce LSN.|  
|hardened_root_file_guid|**uniqueidentifier**|GUID du fichier racine qui a été renforcé suite au dernier point de contrôle terminé.|  
|hardened_root_file_watermark|**bigint**|**Interne uniquement**. Jusqu’à quel point il est valide pour lire le fichier racine (il s’agit d’un type qui est pertinent en interne uniquement-appelé BSN).|  
|hardened_truncation_lsn|**numérique (38)**|LSN du point de troncation.|  
|log_bytes_since_last_close|**bigint**|Octets de la dernière fermeture jusqu’à la fin actuelle du journal.|  
|time_since_last_close_in_ms|**bigint**|Heure depuis la dernière fermeture du point de contrôle.|  
|current_checkpoint_id|**bigint**|De nouveaux segments sont actuellement affectés à ce point de contrôle. Le système de point de contrôle est un pipeline. Le point de contrôle actuel est celui auquel les segments du journal sont affectés. Une fois la limite atteinte, le point de contrôle est libéré par le contrôleur et un nouveau est créé comme actuel.|  
|current_checkpoint_segment_count|**bigint**|Nombre de segments dans le point de contrôle actuel.|  
|recovery_lsn_candidate|**bigint**|En **interne uniquement**. Candidat à choisir comme recoverylsn quand current_checkpoint_id se ferme.|  
|outstanding_checkpoint_count|**bigint**|Nombre de points de contrôle dans le pipeline en attente de fermeture.|  
|closing_checkpoint_id|**bigint**|ID du point de contrôle de fermeture.<br /><br /> Les sérialiseurs fonctionnent en parallèle. ainsi, une fois qu’ils sont terminés, le point de contrôle est un candidat à fermer par le thread de fermeture. Mais le thread de fermeture ne peut se fermer qu’une seule fois et doit être dans l’ordre. le point de contrôle de fermeture est donc celui sur lequel le thread de fermeture travaille.|  
|recovery_checkpoint_id|**bigint**|ID du point de contrôle à utiliser pour la récupération.|  
|recovery_checkpoint_ts|**bigint**|Horodatage du point de contrôle de récupération.|  
|bootstrap_recovery_lsn|**numérique (38)**|LSN de récupération pour le bootstrap.|  
|bootstrap_root_file_guid|**uniqueidentifier**|GUID du fichier racine pour le bootstrap.|  
|internal_error_code|**bigint**|Erreur détectée par l’un des threads du contrôleur, du sérialiseur, de la fermeture et de la fusion.|
|bytes_of_large_data_serialized|**bigint**|Quantité de données qui a été sérialisée. |  
  
##  <a name="sssql14"></a><a name="bkmk_2014"></a> [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]  
 Le tableau suivant décrit les colonnes dans `sys.dm_db_xtp_checkpoint_stats` , pour **[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]** .  
  
|Nom de la colonne|Type|Description|  
|-----------------|----------|-----------------|  
|log_to_process_in_bytes|**bigint**|Nombre d'octets du journal entre le numéro séquentiel dans le journal (LSN) actuel du thread et la fin du journal.|  
|total_log_blocks_processed|**bigint**|Nombre total de blocs de journal traités depuis le démarrage du serveur.|  
|total_log_records_processed|**bigint**|Nombre total d'enregistrements de journal traités depuis le démarrage du serveur.|  
|xtp_log_records_processed|**bigint**|Nombre total d'enregistrements de journal de l'OLTP en mémoire traités depuis le démarrage du serveur.|  
|total_wait_time_in_ms|**bigint**|Durée cumulative d'attente en ms.|  
|waits_for_io|**bigint**|Nombre d'attentes pour les E/S du journal.|  
|io_wait_time_in_ms|**bigint**|Durée cumulative d'attente des E/S du journal.|  
|waits_for_new_log|**bigint**|Nombre d'attentes pour le nouveau journal à générer.|  
|new_log_wait_time_in_ms|**bigint**|Durée cumulative d'attente du nouveau journal.|  
|log_generated_since_last_checkpoint_in_bytes|**bigint**|Nombre d'entrées de journal générées depuis le dernier point de contrôle de l'OLTP en mémoire.|  
|ms_since_last_checkpoint|**bigint**|Durée en millisecondes depuis le dernier point de contrôle de l'OLTP en mémoire.|  
|checkpoint_lsn|**numérique (38)**|Numéro séquentiel dans le journal (LSN) de récupération associé au dernier point de vérification de l'OLTP en mémoire terminé.|  
|current_lsn|**numérique (38)**|Numéro séquentiel dans le journal de l'enregistrement de journal en cours de traitement.|  
|end_of_log_lsn|**numérique (38)**|Numéro séquentiel dans le journal à la fin du journal.|  
|task_address|**varbinary (8)**|Adresse de SOS_Task. Jointure à sys.dm_os_tasks pour rechercher des informations supplémentaires.|  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'autorisation `VIEW DATABASE STATE` sur le serveur.  
  
## <a name="see-also"></a>Voir aussi  
 [Vues de gestion dynamique des tables à mémoire optimisée &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/memory-optimized-table-dynamic-management-views-transact-sql.md)  
  
  
