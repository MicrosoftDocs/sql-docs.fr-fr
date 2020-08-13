---
title: sys. dm_resource_governor_external_resource_pools (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/24/2019
ms.prod: sql
ms.technology: machine-learning-services
ms.reviewer: ''
ms.topic: language-reference
f1_keywords:
- sys.dm_resource_governor_external_resource_pools_TSQL
- sys.dm_resource_governor_external_resource_pools
- dm_resource_governor_external_resource_pools
- dm_resource_governor_external_resource_pools_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- dm_resource_governor_external_resource_pools
- sys.dm_resource_governor_external_resource_pools
author: dphansen
ms.author: davidph
manager: cgronlun
monikerRange: '>=sql-server-2016||=sqlallproducts-allversions'
ms.openlocfilehash: ee42a9a7b4fe026df8e9a424ed25224e7a7edb7b
ms.sourcegitcommit: 9b41725d6db9957dd7928a3620fe4db41eb51c6e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/13/2020
ms.locfileid: "88170988"
---
# <a name="sysdm_resource_governor_external_resource_pools-transact-sql"></a>sys. dm_resource_governor_external_resource_pools (Transact-SQL)
[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)]

Retourne des informations sur l’état actuel du pool de ressources externes, la configuration actuelle des pools de ressources et les statistiques de pool de ressources. 
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).  
  
|Nom de Colmn      |Type de données      |Description|  
|----------------|---------------|-----------------| 
| external_pool_id|**int**|ID du pool de ressources. N'accepte pas la valeur NULL. |
| name|**sysname**|Nom du pool de ressources. N'accepte pas la valeur NULL. 
| pool_version|**int**|Numéro de version interne.|
| max_cpu_percent|**int**|Configuration actuelle de la bande passante processeur moyenne maximale pour toutes les demandes dans le pool de ressources en cas de contention du processeur. N'accepte pas la valeur NULL. |
| max_processes|**int**|Nombre maximal de processus externes simultanés. La valeur par défaut 0 spécifie qu'il n'y a pas de limite. N'accepte pas la valeur NULL.|
| max_memory_percent|**int**|Configuration actuelle du pourcentage de la mémoire totale du serveur qui peut être utilisé par les demandes dans ce pool de ressources. N'accepte pas la valeur NULL. |
| statistics_start_time|**datetime**|Heure à laquelle les statistiques ont été réinitialisées pour ce pool. N'accepte pas la valeur NULL. 
| peak_memory_kb|**bigint**|Quantité maximale de mémoire utilisée, en kilo-octets, pour le pool de ressources. N'accepte pas la valeur NULL. |
| write_io_count|**int**|Total des entrées/sorties d'écriture émises depuis que les statistiques du gouverneur de ressources ont été réinitialisées. N'accepte pas la valeur NULL. |
| read_io_count|**int**|Total d'entrées/sorties de lecture émises depuis que les statistiques du gouverneur de ressources ont été réinitialisées. N'accepte pas la valeur NULL. |
| total_cpu_kernel_ms|**bigint**|Temps total du noyau utilisateur de l’UC, en millisecondes, depuis la réinitialisation des statistiques de du gouverneur de ressources. N'accepte pas la valeur NULL. |
| total_cpu_user_ms|**bigint**|Temps total d’utilisation de l’UC, en millisecondes, depuis la réinitialisation des statistiques de du gouverneur de ressources. N'accepte pas la valeur NULL. |
| active_processes_count|**int**|Nombre de processus externes en cours d’exécution au moment de la requête. N'accepte pas la valeur NULL. |

 
## <a name="permissions"></a>Autorisations

Nécessite l'autorisation `VIEW SERVER STATE`.

## <a name="see-also"></a>Voir aussi  
 [sys.dm_resource_governor_external_resource_pool_affinity &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-external-resource-pool-affinity-transact-sql.md)  
  
  
