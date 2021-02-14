---
description: sys.dm_os_job_object (Azure SQL Database)
title: sys.dm_os_job_object (Azure SQL Database) | Microsoft Docs
ms.custom: ''
ms.date: 06/03/2020
ms.service: sql-database
ms.reviewer: ''
ms.topic: reference
f1_keywords:
- sys.dm_db_resource_stats
- sys.dm_db_resource_stats_TSQL
- dm_db_resource_stats
- dm_db_resource_stats_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_resource_stats
- dm_db_resource_stats
ms.assetid: 6e76b39f-236e-4bbf-b0b5-38be190d81e8
author: julieMSFT
ms.author: jrasnick
monikerRange: = azuresqldb-current
ms.openlocfilehash: c9dbbe97b010e5fb47dd58f60f5863a63a5a36fd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342146"
---
# <a name="sysdm_os_job_object-azure-sql-database"></a>sys.dm_os_job_object (Azure SQL Database)
[!INCLUDE[Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/asdb-asdbmi.md)]

Retourne une ligne unique décrivant la configuration de l’objet de traitement qui gère le processus de SQL Server, ainsi que certaines statistiques de consommation des ressources au niveau de l’objet du travail. Retourne un jeu vide si SQL Server n’est pas en cours d’exécution dans un objet de traitement.

Un objet de traitement est une construction Windows qui implémente la gouvernance des ressources du processeur, de la mémoire et de l’e/s au niveau du système d’exploitation. Pour plus d’informations sur les objets de traitement, consultez [objets de traitement](/windows/desktop/ProcThread/job-objects).
  
|Colonnes|Type de données|Description|  
|-------------|---------------|-----------------|  
|cpu_rate|**int**|Spécifie la partie des cycles de processeur que SQL Server threads peut utiliser pendant chaque intervalle de planification. La valeur est indiquée sous la forme d’un pourcentage de cycles disponibles dans un intervalle de planification du cycle de 10000, multiplié par le nombre d’UC logiques. Par exemple, la valeur 800 sur une instance de SQL Server avec 8 UC logiques signifie que les threads peuvent utiliser des processeurs comme leur capacité totale.|
|cpu_affinity_mask|**bigint**|Masque de bits décrivant les processeurs logiques que le processus de SQL Server peut utiliser dans le groupe de processeurs. Par exemple, cpu_affinity_mask 255 (1111 1111 en binaire) signifie que les huit premiers processeurs logiques peuvent être utilisés. <br /><br />Cette colonne est fournie à des fins de compatibilité descendante. Il ne signale pas le groupe de processeurs, et la valeur signalée peut être incorrecte lorsqu’un groupe de processeurs contient plus de 64 processeurs logiques. Utilisez la `process_physical_affinity` colonne pour déterminer l’affinité du processeur à la place.|
|cpu_affinity_group|**int**|Numéro du groupe de processeurs utilisé par SQL Server.|
|memory_limit_mb|**bigint**|Quantité maximale de mémoire allouée, en Mo, que tous les processus de l’objet de traitement, y compris les SQL Server, peuvent utiliser de façon cumulative.| 
|process_memory_limit_mb |**bigint**|Quantité maximale de mémoire allouée, en Mo, qu’un seul processus de l’objet de traitement, tel que SQL Server, peut utiliser.|
|workingset_limit_mb |**bigint**|Quantité maximale de mémoire, en Mo, que peut utiliser la plage de travail du SQL Server.|
|non_sos_mem_gap_mb|**bigint**|Quantité de mémoire, en Mo, réservée aux piles de threads, dll et autres allocations de mémoire non SOS. La mémoire cible SOS correspond à la différence entre `process_memory_limit_mb` et `non_sos_mem_gap_mb` .| 
|low_mem_signal_threshold_mb|**bigint**|Seuil de mémoire, en Mo. Lorsque la quantité de mémoire disponible pour l’objet de traitement est inférieure à ce seuil, un signal de notification de mémoire insuffisante est envoyé au processus de SQL Server. |
|total_user_time|**bigint**|Nombre total de battements de 100 ns que les threads de l’objet de traitement ont passé en mode utilisateur, depuis la création de l’objet de traitement. |
|total_kernel_time |**bigint**|Nombre total de battements de 100 ns que les threads de l’objet de traitement ont passé en mode noyau, depuis la création de l’objet de traitement. |
|write_operation_count |**bigint**|Nombre total d’opérations d’e/s en écriture sur les disques locaux émises par SQL Server depuis la création de l’objet de traitement. |
|read_operation_count |**bigint**|Nombre total d’opérations d’e/s de lecture sur les disques locaux émis par SQL Server depuis la création de l’objet de traitement. |
|peak_process_memory_used_mb|**bigint**|La quantité de mémoire maximale, en MÉGAOCTETs, qu’un seul processus de l’objet de traitement, tel que SQL Server, a été utilisé depuis la création de l’objet de traitement.| 
|peak_job_memory_used_mb|**bigint**|Quantité de mémoire maximale, en Mo, que tous les processus de l’objet de traitement ont utilisé de façon cumulative depuis la création de l’objet de traitement.|
|process_physical_affinity|**nvarchar (3072)**|Masques de bits décrivant les processeurs logiques que le processus de SQL Server peut utiliser dans chaque groupe de processeurs. La valeur de cette colonne est formée d’une ou plusieurs paires de valeurs, chacune placée entre accolades. Dans chaque paire, la première valeur est le numéro de groupe de processeurs, et la deuxième valeur est le masque de bits d’affinité pour ce groupe de processeurs. Par exemple, la valeur `{{0,a}{1,2}}` signifie que le masque d’affinité pour le groupe de processeurs `0` est `a` ( `1010` en binaire, indiquant que les processeurs 2 et 4 sont utilisés) et que le masque d’affinité pour le groupe de processeurs `1` est `2` ( `10` en binaire, indiquant que le processeur 2 est utilisé).|
  
## <a name="permissions"></a>Autorisations  
Sur SQL Managed Instance, requiert l' `VIEW SERVER STATE` autorisation. Sur SQL Database, requiert l’autorisation `VIEW DATABASE STATE` dans la base de données.  
 
## <a name="see-also"></a>Voir aussi  

Pour plus d’informations sur les instances gérées, consultez [SQL Managed instance](/azure/sql-database/sql-database-managed-instance).
