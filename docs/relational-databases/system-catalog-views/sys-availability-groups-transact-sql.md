---
description: sys.availability_groups (Transact-SQL)
title: sys.availability_groups (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.availability_groups_TSQL
- availability_groups_TSQL
- sys.availability_groups
- availability_groups
dev_langs:
- TSQL
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
- sys.availability_groups catalog view
ms.assetid: da7fa55f-c008-45d9-bcfc-3513b02d9e71
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 2808ab7ce8d64ed5615a2bf2d2b6500a68d6e88f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210055"
---
# <a name="sysavailability_groups-transact-sql"></a>sys.availability_groups (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Retourne une ligne pour chaque groupe de disponibilité pour lequel l'instance locale de SQL Server héberge un réplica de disponibilité. Chaque ligne contient une copie mise en cache des métadonnées du groupe de disponibilité.  
   
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**group_id**|**uniqueidentifier**|Identificateur unique (GUID) du groupe de disponibilité.|  
|**name**|**sysname**|Nom du groupe de disponibilité. Il s'agit d'un nom spécifié par l'utilisateur qui doit être unique dans le cluster de basculement Windows Server (WSFC).|  
|**resource_id**|**nvarchar(40)**|ID de ressource pour la ressource de cluster WSFC.|  
|**resource_group_id**|**nvarchar(40)**|ID du groupe de ressources pour le groupe de ressources du cluster WSFC du groupe de disponibilité.|  
|**failure_condition_level**|**int**|Niveau de condition d’échec défini par l’utilisateur sous lequel un basculement automatique doit être déclenché, l’une des valeurs entières indiquées dans le tableau situé immédiatement en dessous de cette table.<br /><br /> Les niveaux de condition d’échec (1-5) s’étendent du moins restrictif, le niveau 1, au plus restrictif, le niveau 5. Un niveau de condition donné comprend tous les niveaux moins restrictifs. Par conséquent, le niveau de condition le plus strict, le niveau 5, inclut les quatre niveaux de condition moins restrictifs (1 à 4), le niveau 4 inclut les niveaux 1 à 3, et ainsi de suite.<br /><br /> Pour modifier cette valeur, utilisez l’option FAILURE_CONDITION_LEVEL de l’instruction [ALTER Availability Group](../../t-sql/statements/alter-availability-group-transact-sql.md) [!INCLUDE[tsql](../../includes/tsql-md.md)] .|  
|**health_check_timeout**|**int**|Délai d’attente (en millisecondes) pour la procédure stockée système [sp_server_diagnostics](../../relational-databases/system-stored-procedures/sp-server-diagnostics-transact-sql.md) pour retourner les informations sur l’intégrité du serveur, avant que l’instance de serveur soit considérée comme lente ou ne répond pas. La valeur par défaut est 30 000 millisecondes (ou 30 secondes).<br /><br /> Pour modifier cette valeur, utilisez l’option HEALTH_CHECK_TIMEOUT de l’instruction [ALTER Availability Group](../../t-sql/statements/alter-availability-group-transact-sql.md) [!INCLUDE[tsql](../../includes/tsql-md.md)] .|  
|**automated_backup_preference**|**tinyint**|Emplacement par défaut des sauvegardes effectuées sur des bases de données de disponibilité dans ce groupe de disponibilité. Voici les valeurs possibles et leurs descriptions.<br /><br /> <br /><br /> 0 : principal. Les sauvegardes doivent toujours avoir lieu sur le réplica principal.<br /><br /> 1 : secondaire uniquement. Les sauvegardes sur un réplica secondaire sont préférables.<br /><br /> 2 : préférer le réplica secondaire. Les sauvegardes sur un réplica secondaire sont préférables, mais celles sur le réplica principal sont acceptables si aucun réplica secondaire n'est disponible pour les opérations de sauvegarde. Il s'agit du comportement par défaut.<br /><br /> 3 : tout réplica. Aucune préférence : les sauvegardes sont effectuées sur le réplica principal ou sur un réplica secondaire.<br /><br /> <br /><br /> Pour plus d’informations, consultez [Secondaires actifs : Sauvegarde sur des réplicas secondaires &#40;groupes de disponibilité Always On&#41;](../../database-engine/availability-groups/windows/active-secondaries-backup-on-secondary-replicas-always-on-availability-groups.md).|  
|**automated_backup_preference_desc**|**nvarchar(60)**|Description de **automated_backup_preference**, parmi :<br /><br /> PRIMARY<br /><br /> SECONDARY_ONLY<br /><br /> SECONDARY<br /><br /> Aucune|  
|**version**|**smallint**|Version des métadonnées du groupe de disponibilité stockées dans le cluster de basculement Windows. Ce numéro de version est incrémenté lors de l’ajout de nouvelles fonctionnalités.|  
|**basic_features**|**bit**|Spécifie s’il s’agit d’un groupe de disponibilité de base. Pour plus d’informations, consultez [Groupes de disponibilité de base &#40;groupes de disponibilité Always On&#41;](../../database-engine/availability-groups/windows/basic-availability-groups-always-on-availability-groups.md).|  
|**dtc_support**|**bit**|Spécifie si la prise en charge de DTC a été activée pour ce groupe de disponibilité. L’option **DTC_SUPPORT** de **Create Availability Group** contrôle ce paramètre.|  
|**db_failover**|**bit**|Spécifie si le groupe de disponibilité prend en charge le basculement des conditions d’intégrité de la base de données. L’option **DB_FAILOVER** de **Create Availability Group** contrôle ce paramètre.|  
|**is_distributed**|**bit**|Spécifie s’il s’agit d’un groupe de disponibilité distribué. Pour plus d’informations, consultez [Groupes de disponibilité distribués &#40;groupes de disponibilité Always On&#41;](../../database-engine/availability-groups/windows/distributed-availability-groups.md).|
|**cluster_type**|**tinyint**|0 : cluster de basculement Windows Server <br/><br/>1 : cluster externe (par exemple, le stimulateur Linux)<br/><br/>2 : aucun|
|**cluster_type_desc**|**nvarchar(60)**|Description textuelle du type de cluster|
|**required_synchronized_secondaries_to_commit**|**int**| Nombre de réplicas secondaires qui doivent se trouver dans un état synchronisé pour qu’une validation soit terminée|
|**sequence_number**|**bigint**|Identifie la séquence de configuration du groupe de disponibilité. Augmente de façon incrémentielle chaque fois que le réplica principal du groupe de disponibilité met à jour la configuration du groupe.|
|**is_contained**|**bit**|1 : instance principale du cluster Big Data configurée pour la haute disponibilité. <br/><br/> 0 : tous les autres.|
  
## <a name="failure-condition-level--values"></a>Valeurs de niveau de condition d’échec  
 Le tableau suivant décrit les niveaux de condition d’échec possibles pour la colonne **failure_condition_level** .  
  
|Valeur|Condition d’échec|  
|-----------|-----------------------|  
|1|Spécifie qu'un basculement automatique doit être initialisé lorsque l'une des conditions suivantes se produit :<br /><br /> <br /><br /> -Le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] service est inactif.<br /><br /> -Le bail du groupe de disponibilité pour la connexion au cluster de basculement WSFC expire car aucun accusé de réception n’est reçu de l’instance de serveur. Pour plus d’informations, consultez [How It Works: SQL Server Always On Lease Timeout](https://techcommunity.microsoft.com/t5/sql-server-support/how-it-works-sql-server-alwayson-lease-timeout/ba-p/317268).|  
|2|Spécifie qu'un basculement automatique doit être initialisé lorsque l'une des conditions suivantes se produit :<br /><br /> <br /><br /> -L’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne se connecte pas au cluster et le seuil de **health_check_timeout** spécifié par l’utilisateur du groupe de disponibilité est dépassé.<br /><br /> -Le réplica de disponibilité est dans un état d’échec.|  
|3|Spécifie qu'un basculement automatique doit être initialisé sur les erreurs internes critiques [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], telles que les verrouillages spinlock orphelins, les violations graves d'accès en écriture, ou en cas de vidages trop importants.<br /><br /> Il s’agit de la valeur par défaut.|  
|4|Spécifie qu'un basculement automatique doit être initialisé sur les erreurs internes modérées [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], telles qu'une condition persistante de mémoire insuffisante dans le pool de ressources interne [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|5|Spécifie qu'un basculement automatique doit être initialisé sur tous les états d'échec qualifiés, notamment :<br /><br /> <br /><br /> -Épuisement des threads de travail du moteur SQL.<br /><br /> -Détection d’un blocage insoluble.|  
  
## <a name="security"></a>Sécurité  
  
### <a name="permissions"></a>Autorisations  
 Nécessite l'autorisation VIEW ANY DEFINITION sur l'instance de serveur.  
  
## <a name="see-also"></a>Voir aussi  
 [sys.availability_replicas &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-availability-replicas-transact-sql.md)   
 [Groupes de disponibilité Always On &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)   
 [Surveiller les groupes de disponibilité &#40;Transact-SQL&#41;](../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)   
 [Surveiller des groupes de disponibilité &#40;Transact-SQL&#41;](../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)  
  
