---
description: sys.dm_hadr_availability_replica_states (Transact-SQL)
title: sys.dm_hadr_availability_replica_states (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/16/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_hadr_availability_replica_states
- sys.dm_hadr_availability_replica_states_TSQL
- sys.dm_hadr_availability_replica_states
- dm_hadr_availability_replica_states_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
- sys.dm_hadr_availability_replica_states dynamic management view
ms.assetid: d2e678bb-51e8-4a61-b223-5c0b8d08b8b1
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 549c37fdcc04d16eb2163fc7cca7e2ffddf9ce3e
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092779"
---
# <a name="sysdm_hadr_availability_replica_states-transact-sql"></a>sys.dm_hadr_availability_replica_states (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Retourne une ligne pour chaque réplica local et une ligne pour chaque réplica distant dans le même groupe de disponibilité Always On qu'un réplica local. Chaque ligne contient des informations sur l'état d'un réplica donné.  
  
> [!IMPORTANT]  
>  Pour obtenir des informations sur chaque réplica d’un groupe de disponibilité donné, interrogez **sys.dm_hadr_availability_replica_states** sur l’instance de serveur qui héberge le réplica principal. En cas d'interrogation sur une instance de serveur qui héberge un réplica secondaire d'un groupe de disponibilité, cette vue de gestion dynamique retourne uniquement les informations locales pour le groupe de disponibilité.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**replica_id**|**uniqueidentifier**|Identificateur unique du réplica.|  
|**group_id**|**uniqueidentifier**|Identificateur unique du groupe de disponibilité.|  
|**is_local**|**bit**|Si le réplica est local, l’un des éléments suivants :<br /><br /> 0 = Indique un réplica secondaire distant dans un groupe de disponibilité dont le réplica principal est hébergé par l'instance de serveur local. Cette valeur est présente uniquement sur l'emplacement de réplica principal.<br /><br /> 1 = indique un réplica local. Sur les réplicas secondaires, il s'agit de la seule valeur disponible pour le groupe de disponibilité auquel le réplica appartient.|  
|**role**|**tinyint**|[!INCLUDE[ssHADR](../../includes/sshadr-md.md)]Rôle actuel d’un réplica local ou d’un réplica distant connecté, parmi les suivants :<br /><br /> 0 = Résolution<br /><br /> 1 = Principal<br /><br /> 2 = Secondaire<br /><br /> Pour plus d’informations sur les rôles des [!INCLUDE[ssHADR](../../includes/sshadr-md.md)], consultez [Vue d’ensemble des groupes de disponibilité Always On &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md).|  
|**role_desc**|**nvarchar(60)**|Description du **rôle**, parmi les suivants :<br /><br /> RESOLVING<br /><br /> PRIMARY<br /><br /> SECONDARY|  
|**operational_state**|**tinyint**|État opérationnel actuel du réplica, parmi les suivants :<br /><br /> 0 = Basculement en attente<br /><br /> 1 = en attente<br /><br /> 2 = en ligne<br /><br /> 3 = hors connexion<br /><br /> 4 = échec<br /><br /> 5 = Échec, aucun quorum<br /><br /> NULL = Le réplica n'est pas local.<br /><br /> Pour plus d’informations, consultez [rôles et États opérationnels](#RolesAndOperationalStates), plus loin dans cette rubrique.|  
|**Description de l' \_ état opérationnel \_**|**nvarchar(60)**|Description de **l' \_ état opérationnel**, parmi les suivants :<br /><br /> PENDING_FAILOVER<br /><br /> PENDING<br /><br /> ONLINE<br /><br /> OFFLINE<br /><br /> FAILED<br /><br /> FAILED_NO_QUORUM<br /><br /> NULL|  
|**intégrité de la récupération \_**|**tinyint**|Cumul de la **colonne \_ État** de la base de données de la vue de gestion dynamique [sys.dm_hadr_database_replica_states](../../relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-states-transact-sql.md) . Voici les valeurs possibles et leurs descriptions.<br /><br /> 0 : en cours.  Au moins une base de données jointe a un état de base de données autre que en ligne (l'**\_ État de la base de données** n’est pas 0).<br /><br /> 1 : en ligne. Toutes les bases de données jointes ont un état de base de données en ligne (**database_state** a la valeur 0).<br /><br /> NULL : **is_local** = 0|  
|**recovery_health_desc**|**nvarchar(60)**|Description de **recovery_health**, parmi :<br /><br /> ONLINE_IN_PROGRESS<br /><br /> ONLINE<br /><br /> NULL|  
|**intégrité de la synchronisation \_**|**tinyint**|Reflète un cumul de l’état de synchronisation de base de données (**synchronization_state**) de toutes les bases de données de disponibilité jointes (également appelées *réplicas*) et le mode de disponibilité du réplica (mode de validation synchrone ou asynchrone). Le cumul reflète l’État accumulé le moins sain des bases de données sur le réplica. Vous trouverez ci-dessous les valeurs possibles et leurs descriptions.<br /><br /> 0 : non intègre.   Au moins une base de données jointe est dans un état NOT SYNCHRONIZING.<br /><br /> 1 : partiellement sain. Certains réplicas ne sont pas dans l'état de synchronisation cible : les réplicas avec validation synchrone doivent être synchronisés, et les réplicas avec validation asynchrone doivent être en cours de synchronisation.<br /><br /> 2 : intègre. Tous les réplicas sont dans l'état de synchronisation cible : les réplicas avec validation synchrone sont synchronisés, et les réplicas avec validation asynchrone sont en cours de synchronisation.|  
|**synchronization_health_desc**|**nvarchar(60)**|Description de **synchronization_health**, parmi :<br /><br /> NOT_HEALTHY<br /><br /> PARTIALLY_HEALTHY<br /><br /> HEALTHY|  
|**connected_state**|**tinyint**|Indique si un réplica secondaire est actuellement connecté au réplica principal. Les valeurs possibles sont indiquées ci-dessous avec leurs descriptions.<br /><br /> 0 : déconnecté. La réponse d’un réplica de disponibilité à l’État Disconnected dépend de son rôle : sur le réplica principal, si un réplica secondaire est déconnecté, ses bases de données secondaires sont marquées comme non SYNCHRONISÉes sur le réplica principal, qui attend la reconnexion de la base de données secondaire ; Sur un réplica secondaire, lorsqu’il détecte qu’il est déconnecté, le réplica secondaire tente de se reconnecter au réplica principal.<br /><br /> 1 : connecté.<br /><br /> Chaque réplica principal suit l'état de la connexion pour chaque réplica secondaire dans le même groupe de disponibilité. Les réplicas secondaires suivent l'état de connexion du réplica principal uniquement.|  
|**connected_state_desc**|**nvarchar(60)**|Description de **connection_state**, parmi :<br /><br /> DISCONNECTED<br /><br /> CONNECTED|  
|**last_connect_error_number**|**int**|Numéro de la dernière erreur de connexion.|  
|**last_connect_error_description**|**nvarchar(1024)**|Texte du message de **last_connect_error_number** .|  
|**last_connect_error_timestamp**|**datetime**|Horodateur de la date et de l’heure indiquant le moment où l' **last_connect_error_number** erreur s’est produite.|  
  
##  <a name="roles-and-operational-states"></a><a name="RolesAndOperationalStates"></a> Rôles et États opérationnels  
 Le rôle, le **rôle**, reflète l’état d’un réplica de disponibilité donné et l’état opérationnel, **operational_state**, décrit si le réplica est prêt à traiter les demandes des clients pour l’ensemble de la base de données du réplica de disponibilité. Voici un résumé des États opérationnels qui sont possibles pour chaque rôle : résolution, principal et secondaire.  
  
 **Résolution :** Lorsqu’un réplica de disponibilité se trouve dans le rôle de résolution, les États opérationnels possibles sont répertoriés dans le tableau suivant.  
  
|État de fonctionnement|Description|  
|-----------------------|-----------------|  
|PENDING_FAILOVER|Une commande de basculement est traitée pour le groupe de disponibilité.|  
|OFFLINE|Toutes les données de configuration du réplica de disponibilité ont été mises à jour sur le cluster WSFC, ainsi que dans les métadonnées locales, mais le groupe de disponibilité ne dispose pas actuellement de réplica principal.|  
|FAILED|Un échec de lecture s'est produit pendant une tentative de récupération des informations du cluster WSFC.|  
|FAILED_NO_QUORUM|Le nœud local WSFC n'a pas de quorum. Il s'agit d'un état déduit.|  
  
 **Principal :** Lorsqu’un réplica de disponibilité effectue le rôle principal, il est actuellement le réplica principal. Les États opérationnels possibles sont répertoriés dans le tableau suivant.  
  
|État de fonctionnement|Description|  
|-----------------------|-----------------|  
|PENDING|Il s'agit d'un état temporaire, mais un réplica principal peut être bloqué dans cet état si les processus de travail ne sont pas disponibles pour traiter les demandes.|  
|ONLINE|La ressource du groupe de disponibilité est en ligne, et tous les threads de travail de base de données ont été sélectionnés.|  
|FAILED|Le réplica de disponibilité ne peut pas lire sur le cluster WSFC et/ou écrire à partir de celui-ci.|  
  
 **Secondaire :** Lorsqu’un réplica de disponibilité joue le rôle secondaire, il s’agit actuellement d’un réplica secondaire. Les États opérationnels possibles sont répertoriés dans le tableau ci-dessous.  
  
|État de fonctionnement|Description|  
|-----------------------|-----------------|  
|ONLINE|Le réplica secondaire local n'est pas connecté au réplica principal.|  
|FAILED|Le réplica secondaire local ne peut pas lire sur le cluster WSFC et/ou écrire à partir de celui-ci.|  
|NULL|Sur un réplica principal, cette valeur est retournée lorsque la ligne est liée à un réplica secondaire.|  
  
## <a name="security"></a>Sécurité  
  
### <a name="permissions"></a>Autorisations  
 requièrent l'autorisation VIEW SERVER STATE sur le serveur.  
  
## <a name="see-also"></a>Voir aussi  
 [Vue d’ensemble des groupes de disponibilité Always On &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Surveiller des groupes de disponibilité &#40;Transact-SQL&#41;](../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)  
  
  
