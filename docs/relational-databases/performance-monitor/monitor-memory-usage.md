---
title: Surveiller l'utilisation de la mémoire
description: 'Supervisez une instance de SQL Server pour vous assurer que l’utilisation de la mémoire reste dans des limites normales. Utilisez les compteurs Mémoire : Octets disponibles et Mémoire : Pages/s.'
ms.custom: ''
ms.date: 01/20/2021
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- tuning databases [SQL Server], memory
- monitoring server performance [SQL Server], memory usage
- isolating memory [SQL Server]
- paging rate [SQL Server]
- memory [SQL Server], monitoring usage
- monitoring [SQL Server], memory usage
- low-memory conditions
- database monitoring [SQL Server], memory usage
- available memory [SQL Server]
- page faults [SQL Server]
- monitoring performance [SQL Server], memory usage
- server performance [SQL Server], memory
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 8954573b0c32eef45ec655b27d26e03e72be96ed
ms.sourcegitcommit: 713e5a709e45711e18dae1e5ffc190c7918d52e7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/22/2021
ms.locfileid: "98688758"
---
# <a name="monitor-memory-usage"></a>Surveiller l’utilisation de la mémoire
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Surveillez périodiquement une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour vous assurer que l'utilisation de la mémoire reste dans des limites normales. 

## <a name="configuring-ssnoversion-max-memory"></a>Configuration de la mémoire maximale de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

Par défaut, une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut, au fil du temps, consommer la majeure partie de la mémoire disponible du système d’exploitation Windows dans le serveur. Une fois la mémoire acquise, elle n’est libérée que si une sollicitation de la mémoire est détectée. Ce comportement est normal et n’est pas l’indication d’une fuite de mémoire dans le processus de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Utilisez [l’option **max server memory**](../../database-engine/configure-windows/server-memory-server-configuration-options.md) pour limiter la quantité de mémoire que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est autorisé à acquérir pour la plupart de ses utilisations. Pour plus d’informations, consultez le [guide d’architecture de gestion de la mémoire](../../relational-databases/memory-management-architecture-guide.md#changes-to-memory-management-starting-with-).

Dans SQL Server sur Linux, [définissez la limite de la mémoire](../../linux/sql-server-linux-performance-best-practices.md#advanced-configuration) avec l’outil mssql-conf et le [paramètre memory.memorylimitmb](../../linux/sql-server-linux-configure-mssql-conf.md#memorylimit).  

## <a name="monitor-operating-system-memory"></a>Surveiller la mémoire du système d’exploitation   
 Pour surveiller une condition de mémoire insuffisante, utilisez les compteurs de serveur Windows suivants. Utilisez les vues de gestion dynamique [sys.dm_os_process_memory](../system-dynamic-management-views/sys-dm-os-process-memory-transact-sql.md) et [sys.dm_os_sys_memory](../system-dynamic-management-views/sys-dm-os-sys-memory-transact-sql.md) pour interroger de nombreux compteurs de mémoire de système d’exploitation.

-   **Mémoire : Octets disponibles**  
Ce compteur indique combien d’octets de mémoire sont actuellement disponibles pour les processus. Un compteur **Octets disponibles** avec des valeurs faibles peut indiquer une pénurie globale de la mémoire du système d’exploitation. Vous pouvez interroger cette valeur en T-SQL avec [sys.dm_os_sys_memory](../system-dynamic-management-views/sys-dm-os-sys-memory-transact-sql.md).available_physical_memory_kb.

-   **Mémoire : Pages/sec**  
Ce compteur indique le nombre de pages qui ont été extraites du disque en raison de défauts de page ou écrites sur le disque pour libérer de l’espace dans la plage de travail en raison de défauts de page. Une valeur élevée du compteur **Pages/s** peut indiquer une pagination excessive.  

-   **Mémoire : Défauts de page/s** Ce compteur indique le taux de défauts de page pour tous les processus, y compris les processus système. Même si l’ordinateur a beaucoup de mémoire à sa disposition, il est normal d’avoir un taux de pagination sur disque faible, mais différent de zéro (d’où des défauts de page). Le gestionnaire de mémoire virtuelle de Microsoft Windows soustrait des pages à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et à d'autres processus quand il réduit la taille des parties actives de ces processus. Son activité a tendance à provoquer des défauts de page.  

-   **Processus : Défauts de page/s** Ce compteur indique le taux de défauts de page pour un processus utilisateur donné. Surveillez **Processus : Défauts de page/s** pour déterminer si l’activité du disque est due à la pagination par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour déterminer si [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou un autre processus est la cause d’une pagination excessive, surveillez le compteur **Processus : Défauts de page/s** pour l’instance de processus [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Pour plus d’informations sur la résolution d’une pagination excessive, consultez la documentation du système d’exploitation.  
  
## <a name="isolating-memory-used-by-ssnoversion"></a>Isolement de la mémoire utilisée par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 

 Pour surveiller l’utilisation de la mémoire de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , utilisez les [compteurs d’objets SQL Server](use-sql-server-objects.md). Utilisez les vues de gestion dynamique [sys.dm_os_performance_counters](../system-dynamic-management-views/sys-dm-os-performance-counters-transact-sql.md) et [sys.dm_os_process_memory](../system-dynamic-management-views/sys-dm-os-process-memory-transact-sql.md) pour interroger de nombreux compteurs d’objets SQL Server.

 Par défaut, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] gère ses besoins de mémoire de façon dynamique en fonction des ressources système disponibles. Si [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a besoin de plus de mémoire, il demande au système d'exploitation de déterminer si de la mémoire physique libre est disponible. Dans l'affirmative, il l'utilise. Si la mémoire disponible est insuffisante pour le système d’exploitation, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] libère de la mémoire sur le système d’exploitation jusqu’à ce que la condition de mémoire insuffisante soit atténuée ou que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] atteigne la limite **min server memory**. Vous pouvez cependant ignorer cette option pour utiliser dynamiquement de la mémoire au moyen des options de configuration du serveur suivantes : **min server memory** et **max server memory**. Pour plus d'informations, consultez [Options de mémoire du serveur](../../database-engine/configure-windows/server-memory-server-configuration-options.md).  
  
 Pour surveiller la mémoire utilisée par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , examinez les compteurs de performances suivants :  
  
-   **SQL Server : Memory Manager : Mémoire totale du serveur (Ko)**  
Ce compteur indique la quantité de mémoire du système d’exploitation que le gestionnaire de mémoire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a actuellement alloué à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Ce nombre doit augmenter en fonction des besoins de l’activité réelle et augmente après le démarrage de SQL Server. Interrogez ce compteur à l’aide de la vue de gestion dynamique [sys.dm_os_sys_info](../system-dynamic-management-views/sys-dm-os-sys-info-transact-sql.md) et observez la colonne **committed_kb**.

-   **SQL Server : Memory Manager : Mémoire du serveur cible (Ko)**  
Ce compteur indique la quantité de mémoire idéale que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut consommer, en fonction de la charge de travail récente. Comparez-le à la **Mémoire totale du serveur** après une période de fonctionnement normal pour déterminer si la quantité de mémoire allouée à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] convient. Après une période de fonctionnement normal, **Mémoire totale du serveur** et **Mémoire du serveur cible** doivent être similaires. Si la **Mémoire totale du serveur** est nettement inférieure à la **Mémoire du serveur cible**, l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut faire l’objet d’une sollicitation de la mémoire. Pendant un certain temps après le démarrage de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], la **Mémoire totale du serveur** est censée être inférieure à la **Mémoire du serveur cible** dans la mesure où la **Mémoire totale du serveur** augmente. Interrogez ce compteur à l’aide de la vue de gestion dynamique [sys.dm_os_sys_info](../system-dynamic-management-views/sys-dm-os-sys-info-transact-sql.md) et observez la colonne **committed_target_kb**. Pour obtenir plus d’informations et les bonnes pratiques relatives à la configuration de la mémoire, consultez les [options de configuration de la mémoire du serveur](../../database-engine/configure-windows/server-memory-server-configuration-options.md#manually).

-   **Processus : Plage de travail**  
Ce compteur indique la quantité de mémoire physique actuellement utilisée par un processus, selon le système d’exploitation. Observez l’instance sqlservr.exe de ce compteur. Interrogez ce compteur à l’aide de la vue de gestion dynamique [sys.dm_os_process_memory](../system-dynamic-management-views/sys-dm-os-process-memory-transact-sql.md) et observez la colonne **physical_memory_in_use_kb**.

-   **Processus : Octets privés**  
Ce compteur indique la quantité de mémoire demandée par un processus au système d’exploitation pour son propre usage. Observez l’instance sqlservr.exe de ce compteur. Étant donné que ce compteur inclut toutes les allocations de mémoire demandées par sqlservr.exe, notamment celles non limitées par [l’option **max server memory**](../../database-engine/configure-windows/server-memory-server-configuration-options.md), ce compteur peut signaler des valeurs supérieures à [l’option **max server memory**](../../database-engine/configure-windows/server-memory-server-configuration-options.md).

-   **SQL Server : Buffer Manager : Pages de base de données**  
Ce compteur indique le nombre de pages dans le pool de mémoires tampons avec le contenu de la base de données. N’inclut pas la mémoire des pools sans tampon au sein du processus SQL Server. Utilisez la vue de gestion dynamique [sys.dm_os_performance_counters](../system-dynamic-management-views/sys-dm-os-performance-counters-transact-sql.md) pour interroger ce compteur.

-   **SQL Server : Buffer Manager : Taux d’accès au cache**  
Ce compteur est spécifique à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Un ratio de 90 ou plus est souhaitable. Une valeur supérieure à 90 indique que plus de 90 % de toutes les requêtes ont été satisfaites à partir de la mémoire cache sans avoir à lire sur le disque. Pour plus d’informations sur l’objet SQL Server Buffer Manager, consultez [SQL Server, objet Buffer Manager](sql-server-buffer-manager-object.md). Utilisez la vue de gestion dynamique [sys.dm_os_performance_counters](../system-dynamic-management-views/sys-dm-os-performance-counters-transact-sql.md) pour interroger ce compteur.  
 
-   **SQL Server : Buffer Manager : Espérance de vie d’une page**  
Ce compteur mesure la durée en secondes pendant laquelle la page la plus ancienne reste dans le pool de mémoires tampons. Pour les systèmes qui utilisent une architecture NUMA, il s’agit de la moyenne sur tous les nœuds NUMA. Une valeur élevée et croissante est préférable. Une chute soudaine indique une attrition importante des données dans et hors du pool de mémoires tampons, ce qui signifie que la charge de travail n’a pas pu tirer pleinement parti des données déjà en mémoire. Chaque nœud NUMA a son propre nœud du pool de mémoires tampons. Sur les serveurs contenant plusieurs nœuds NUMA, vous pouvez voir l’espérance de vie d’une page de chaque nœud du pool de mémoires tampons avec **SQL Server : Buffer Node : Espérance de vie d’une page**. Utilisez la vue de gestion dynamique [sys.dm_os_performance_counters](../system-dynamic-management-views/sys-dm-os-performance-counters-transact-sql.md) pour interroger ce compteur.

  
## <a name="examples"></a>Exemples 

### <a name="determining-current-memory-allocation"></a>Détermination de l’allocation de mémoire actuelle  
 Les requêtes suivantes retournent des informations sur la mémoire actuellement allouée.  
  
```  
SELECT
(total_physical_memory_kb/1024) AS Total_OS_Memory_MB,
(available_physical_memory_kb/1024)  AS Available_OS_Memory_MB
FROM sys.dm_os_sys_memory;

SELECT  
(physical_memory_in_use_kb/1024) AS Memory_used_by_Sqlserver_MB,  
(locked_page_allocations_kb/1024) AS Locked_pages_used_by_Sqlserver_MB,  
(total_virtual_address_space_kb/1024) AS Total_VAS_in_MB,
process_physical_memory_low,  
process_virtual_memory_low  
FROM sys.dm_os_process_memory;  
```  

### <a name="determining-current-sql-server-memory-utilization"></a>Détermination de l’utilisation actuelle de la mémoire par SQL Server   
 La requête suivante retourne des informations sur l’utilisation actuelle de la mémoire par SQL Server.  
```  
SELECT
sqlserver_start_time,
(committed_kb/1024) AS Total_Server_Memory_MB,
(committed_target_kb/1024)  AS Target_Server_Memory_MB
FROM sys.dm_os_sys_info;
```   

### <a name="determining-page-life-expectancy"></a>Détermination de l’espérance de vie d’une page
 La requête suivante utilise **sys.dm_os_performance_counters** pour observer la valeur actuelle d’**espérance de vie d’une page** de l’instance SQL Server au niveau du gestionnaire de tampons global et au niveau de chaque nœud NUMA.
```
SELECT
CASE instance_name WHEN '' THEN 'Overall' ELSE instance_name END AS NUMA_Node, cntr_value AS PLE_s
FROM sys.dm_os_performance_counters    
WHERE counter_name = 'Page life expectancy';
```

## <a name="see-also"></a>Voir aussi
- [sys.dm_os_sys_memory (Transact-SQL)](../system-dynamic-management-views/sys-dm-os-sys-memory-transact-sql.md)
- [sys.dm_os_process_memory (Transact-SQL)](../system-dynamic-management-views/sys-dm-os-process-memory-transact-sql.md)
- [sys.dm_os_sys_info (Transact-SQL)](../system-dynamic-management-views/sys-dm-os-sys-info-transact-sql.md)
- [sys.dm_os_performance_counters (Transact-SQL)](../system-dynamic-management-views/sys-dm-os-performance-counters-transact-sql.md)
- [SQL Server, objet Memory Manager](sql-server-memory-manager-object.md)
- [SQL Server, objet Buffer Manager](sql-server-buffer-manager-object.md)   
- [Options de configuration de la mémoire du serveur](../../database-engine/configure-windows/server-memory-server-configuration-options.md)
- [Guide d’architecture de gestion de la mémoire](../../relational-databases/memory-management-architecture-guide.md)
