---
title: sys.dm_exec_query_optimizer_memory_gateways (Transact-SQL)
description: Retourne l’état actuel des sémaphores de ressource utilisés pour limiter l’optimisation des requêtes simultanées
ms.custom: seo-dt-2019
ms.date: 04/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: wiassaf
ms.technology: performance
ms.topic: reference
f1_keywords:
- dm_exec_query_optimizer_memory_gateways_TSQL
- dm_exec_query_optimizer_memory_gateways
- sys.dm_exec_query_optimizer_memory_gateways_TSQL
- sys.dm_exec_query_optimizer_memory_gateways
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_query_optimizer_memory_gateways dynamic management view
author: josack
ms.author: josack
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 66e5d3e4509b8593a41a53348e466054ee01b4f4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342916"
---
# <a name="sysdm_exec_query_optimizer_memory_gateways-transact-sql"></a>sys.dm_exec_query_optimizer_memory_gateways (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

Retourne l’état actuel des sémaphores de ressource utilisés pour limiter l’optimisation des requêtes simultanées.

|Colonne|Type|Description|  
|----------|---------------|-----------------|  
|**pool_id**|**int**|ID du pool de ressources sous Resource Governor|  
|**name**|**sysname**|Nom de la porte de compilation (petite passerelle, passerelle moyenne, Big Gateway)|
|**max_count**|**int**|Nombre maximal configuré de compilations simultanées|
|**active_count**|**int**|Nombre de compilations actuellement actives dans cette porte|
|**waiter_count**|**int**|Nombre d’objets Waiter dans cette porte|
|**threshold_factor**|**bigint**|Facteur de seuil qui définit la partie de mémoire maximale utilisée par l’optimisation de requête.  Pour la petite passerelle, threshold_factor indique l’utilisation maximale de la mémoire de l’optimiseur en octets pour une requête avant qu’elle soit nécessaire pour obtenir un accès dans la petite passerelle.  Pour les passerelles moyenne et grande, threshold_factor affiche la partie de la mémoire totale du serveur disponible pour cette porte. Elle est utilisée comme diviseur lors du calcul du seuil d’utilisation de la mémoire pour la porte.|
|**threshold**|**bigint**|Mémoire de seuil suivante, en octets.  La requête est requise pour accéder à cette passerelle si sa consommation de mémoire atteint ce seuil.  « -1 » si la requête n’est pas requise pour accéder à cette passerelle.|
|**is_active**|**bit**|Indique si la requête est requise pour passer la porte active ou non.|


## <a name="permissions"></a>Autorisations  
SQL Server nécessite l’autorisation VIEW SERVER STATE sur le serveur.

Azure SQL Database nécessite l’autorisation VIEW DATABASE STATE dans la base de données.


## <a name="remarks"></a>Notes  
SQL Server utilise une approche de passerelle à plusieurs niveaux pour limiter le nombre de compilations simultanées autorisées.  Trois passerelles sont utilisées, y compris petite, moyenne et grande. Les passerelles permettent d’éviter l’épuisement des ressources mémoire globales par une plus grande mémoire de compilation nécessitant des consommateurs.

L’attente d’une passerelle entraîne une compilation différée. En plus des retards dans la compilation, les demandes limitées sont associées à une accumulation RESOURCE_SEMAPHORE_QUERY_COMPILE type d’attente. Le RESOURCE_SEMAPHORE_QUERY_COMPILE type d’attente peut indiquer que les requêtes utilisent une grande quantité de mémoire pour la compilation et que la mémoire est épuisée, ou que la mémoire disponible est insuffisante dans son ensemble, toutefois, les unités disponibles dans une passerelle spécifique sont épuisées. La sortie de **sys.dm_exec_query_optimizer_memory_gateways** peut être utilisée pour dépanner des scénarios où la mémoire était insuffisante pour compiler un plan d’exécution de requête.  

## <a name="examples"></a>Exemples  

### <a name="a-viewing-statistics-on-resource-semaphores"></a>R. Affichage des statistiques sur les sémaphores de ressources  
Quelles sont les statistiques actuelles de la passerelle mémoire de l’optimiseur pour cette instance de SQL Server ?

```  
SELECT [pool_id], [name], [max_count], [active_count],
       [waiter_count], [threshold_factor], [threshold],
       [is_active]
FROM sys.dm_exec_query_optimizer_memory_gateways;   

```  

## <a name="see-also"></a>Voir aussi  
 [Fonctions et vues de gestion dynamique &#40;Transact-SQL&#41;](./system-dynamic-management-views.md)   
 [Fonctions et vues de gestion dynamique relatives aux exécutions &#40;Transact-SQL&#41;](./execution-related-dynamic-management-views-and-functions-transact-sql.md)  
[Comment utiliser la commande DBCC MEMORYSTATUS pour surveiller l’utilisation de la mémoire sur SQL Server 2005](https://support.microsoft.com/help/907877/how-to-use-the-dbcc-memorystatus-command-to-monitor-memory-usage-on-sql-server-2005) 
 La [compilation de requêtes volumineuses attend sur RESOURCE_SEMAPHORE_QUERY_COMPILE dans SQL Server 2014](https://support.microsoft.com/help/3024815/large-query-compilation-waits-on-resource-semaphore-query-compile-in-sql-server-2014)
