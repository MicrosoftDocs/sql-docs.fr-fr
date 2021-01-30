---
description: sys.server_event_sessions (Transact-SQL)
title: sys.server_event_sessions (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- server_event_sessions
- server_event_sessions_TSQL
- sys.server_event_sessions_TSQL
- sys.server_event_sessions
dev_langs:
- TSQL
helpviewer_keywords:
- sys.server_event_sessions catalog view
- xe
ms.assetid: 796f3093-6a3e-4d67-8da6-b9810ae9ef5b
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 4eaa06349d7266fa44cecd69cbc7c56733e1a44f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211651"
---
# <a name="sysserver_event_sessions-transact-sql"></a>sys.server_event_sessions (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Répertorie toutes les définitions de session d'événements qui existent dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|event_session_id|**int**|ID unique de la session d'événements. N'accepte pas la valeur NULL.|  
|name|**sysname**|Nom défini par l'utilisateur pour identifier la session d'événements. le nom est unique. N'accepte pas la valeur NULL.|  
|event_retention_mode|**nchar(1)**|Détermine comment est gérée la perte d'événements. La valeur par défaut est S. Elle n'accepte pas la valeur Null. Prend l'une des valeurs suivantes :<br /><br /> S. Correspond à event_retention_mode_desc = ALLOW_SINGLE_EVENT_LOSS<br /><br /> M. Correspond à event_retention_mode_desc = ALLOW_MULTIPLE_EVENT_LOSS<br /><br /> N. Correspond à event_retention_mode_desc = NO_EVENT_LOSS|  
|event_retention_mode_desc|**sysname**|Décrit comment est gérée la perte d'événements. La valeur par défaut est ALLOW_SINGLE_EVENT_LOSS. N'accepte pas la valeur NULL. Prend l'une des valeurs suivantes :<br /><br /> ALLOW_SINGLE_EVENT_LOSS. Il est possible de perdre des événements de la session. Les événements uniques ne sont abandonnés uniquement lorsque toutes les mémoires tampons d'événements sont saturées. La perte d'événements uniques lorsque les mémoires tampons d'événements sont saturées permet d'obtenir des caractéristiques de performance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] acceptables, tout en réduisant la perte de données du flux d'événements traité.<br /><br /> ALLOW_MULTIPLE_EVENT_LOSS. Il est possible de perdre des mémoires tampons d'événements saturées de la session. Le nombre d'événements perdus dépend de la taille de la mémoire allouée à la session, du partitionnement de la mémoire et de la taille des événements dans la mémoire tampon. Cette option atténue l'impact sur les performances du serveur lorsque les mémoires tampons d'événements sont rapidement remplies. Toutefois, il est possible de perdre un grand nombre d'événements de la session.<br /><br /> NO_EVENT_LOSS. Aucune perte d'événements n'est autorisée. Cette option garantit que tous les événements déclenchés sont conservés. Elle force toutes les tâches qui déclenchent des événements à attendre que de l'espace se libère dans une mémoire tampon d'événements. Cette option peut entraîner des problèmes de performance détectables pendant que la session d'événements est active.|  
|max_dispatch_latency|**int**|Durée (en millisecondes) pendant laquelle les événements sont mis en mémoire tampon avant d'être distribués aux cibles de la session. Les valeurs valides sont comprises entre 0 et 2147483648, et 0. La valeur 0 indique que la latence de répartition est infinie. Autorise la valeur NULL.|  
|max_memory|**int**|Quantité de mémoire allouée à la session pour la mise en mémoire tampon d'événements. La valeur par défaut est 4 Mo. Autorise la valeur NULL.|  
|max_event_size|**int**|Quantité de mémoire réservée aux événements pour lesquels les mémoires tampons de session d'événements ne suffisent pas. Si max_event_size dépasse la taille de mémoire tampon calculée, deux mémoires tampons supplémentaires de max_event_size sont allouées à la session d'événements. Autorise la valeur NULL.|  
|memory_partition_mode|**nchar(1)**|Emplacement dans la mémoire où les mémoires tampons d'événements sont créées. Le mode de partition par défaut est G. N'accepte pas la valeur Null. memory_partition_mode est l’un des éléments suivants :<br /><br /> G - NONE<br /><br /> C - PER_CPU<br /><br /> N - PER_NODE|  
|memory_partition_mode_desc|**sysname**|La valeur par défaut est NONE. N'accepte pas la valeur NULL. Prend l'une des valeurs suivantes :<br /><br /> NONE. Un ensemble unique de mémoires tampons est créé dans une instance de SQL Server.<br /><br /> PER_CPU. Un ensemble de mémoires tampons est créé pour chaque UC.<br /><br /> PER_NODE. Un jeu de mémoires tampons est créé pour chaque nœud NUMA (Non-Uniform Memory Access).|  
|track_causality|**bit**|Activer ou désactiver le suivi de causalité. Si cette option a la valeur 1 (ON), le suivi est activé et les événements associés de différentes connexions au serveur peuvent être corrélés. Le paramètre par défaut est 0 (OFF). N'accepte pas la valeur NULL.|  
|startup_state|**bit**|Cette valeur détermine si la session est lancée automatiquement au démarrage du serveur. La valeur par défaut est 0. N'accepte pas la valeur NULL. Les valeurs suivantes sont possibles :<br /><br /> 0 (OFF). La session ne se lance pas au démarrage du serveur.<br /><br /> 1 (ON). La session d'événements se lance au démarrage du serveur.|  
  
## <a name="permissions"></a>Autorisations  
 requièrent l'autorisation VIEW SERVER STATE sur le serveur.  
  
## <a name="see-also"></a>Voir aussi  
 [Affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Affichages catalogue des événements étendus &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/extended-events-catalog-views-transact-sql.md)   
 [Événements étendus](../../relational-databases/extended-events/extended-events.md)  
  
  
