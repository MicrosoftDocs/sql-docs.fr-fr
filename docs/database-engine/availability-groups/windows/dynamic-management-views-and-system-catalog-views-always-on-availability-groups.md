---
title: Vues de gestion dynamique et vues de catalogue système pour les groupes de disponibilité
description: Collection de vues de gestion dynamique et de vues de catalogue système vous permettant de superviser et diagnostiquer l’intégrité d’un groupe de disponibilité Always On.
ms.custom: seo-lt-2019
ms.date: 06/13/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: troubleshooting
ms.assetid: 4320a4a4-6183-462b-8bda-e7424e7cb706
author: rothja
ms.author: jroth
ms.openlocfilehash: 6d1a45cf3572c1d533714ff7ebf96fe0b4f236fe
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349298"
---
# <a name="dynamic-management-views-and-system-catalog-views-always-on-availability-groups"></a>Vues de gestion dynamique et affichages catalogue système (groupes de disponibilité Always On)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Cette rubrique présente quelques-unes des requêtes courantes sur les DMV (vues de gestion dynamique) Always On que vous pouvez utiliser pour monitorer les groupes de disponibilité et résoudre les problèmes liés à ceux-ci.  
  
> [!TIP]  
>  Dans le tableau de bord Always On, vous pouvez facilement configurer l’interface graphique utilisateur de manière à afficher la plupart de ces DMV pour les réplicas et bases de données de disponibilité en cliquant sur le bouton droit sur l’en-tête du tableau respectif et en sélectionnant la DMV à afficher ou à masquer.  
  
 Pour plus d’informations sur les DMV de groupe de disponibilité, consultez [Vues et fonctions de gestion dynamiques de groupes de disponibilité Always On &#40;Transact-SQL &#41;](~/relational-databases/system-dynamic-management-views/always-on-availability-groups-dynamic-management-views-functions.md). Pour plus d’informations sur les vues de catalogue des groupes de disponibilité, consultez [Vues de catalogue des groupes de disponibilité Always On &#40;Transact-SQL &#41;](~/relational-databases/system-catalog-views/always-on-availability-groups-catalog-views-transact-sql.md).  
  
## <a name="check-the-wsfc-cluster-node-configuration"></a>Vérifier la configuration du nœud de cluster WSFC  
 La requête T-SQL (Transact-SQL) suivante récupère l’état de tous les nœuds du cluster WSFC (clustering de basculement Windows Server) actif.  
  
```sql  
use master  
go  
select * from sys.dm_hadr_cluster_members  
go  
```  
  
 Ce jeu de résultats fournit l’état de chaque nœud membre du cluster WSFC actif. Si le quorum est défini comme **Nœud et partage de fichiers majoritaires**, même le partage de fichiers est signalé. Vous pouvez voir l’état de chaque nœud, notamment le poids du vote de chaque nœud (valeur [number_of_quorum_votes](~/relational-databases/system-dynamic-management-views/sys-dm-hadr-cluster-members-transact-sql.md)).  
  
## <a name="explore-the-cluster-network"></a>Explorer le réseau en cluster  
 La requête suivante récupère la configuration réseau du cluster WSFC actif.  
  
```sql  
select * from sys.dm_hadr_cluster_networks  
```  
  
 Le jeu de résultats contient une ligne pour chaque carte réseau du cluster WSFC. Par exemple, dans un cluster à deux nœuds contenant deux cartes réseau par nœud, cette requête retourne quatre lignes.  
  
## <a name="explore-the-availability-groups"></a>Explorer les groupes de disponibilité  
 La requête suivante récupère des informations sur un groupe de disponibilité.  
  
```sql  
select primary_replica, primary_recovery_health_desc, synchronization_health_desc from sys.dm_hadr_availability_group_states  
go  
select * from sys.availability_groups  
go  
select * from sys.availability_groups_cluster  
go  
```  
  
 Les DMV [sys.dm_hadr_availability_group_states &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-group-states-transact-sql.md), [sys.availability_groups &#40;Transact-SQL&#41;](~/relational-databases/system-catalog-views/sys-availability-groups-transact-sql.md) et [sys.availability_groups_cluster](~/relational-databases/system-catalog-views/sys-availability-groups-cluster-transact-sql.md) retournent des informations sur les groupes de disponibilité dans le cluster WSFC actif. En fait, [sys.availability_groups &#40;Transact-SQL&#41;](~/relational-databases/system-catalog-views/sys-availability-groups-transact-sql.md) et [sys.availability_groups_cluster](~/relational-databases/system-catalog-views/sys-availability-groups-cluster-transact-sql.md) semblent retourner des informations identiques.  
  
 Toutefois, [sys.availability_groups_cluster](~/relational-databases/system-catalog-views/sys-availability-groups-cluster-transact-sql.md) fournit les métadonnées de groupe de disponibilité stockées dans le cluster WSFC, tandis que [sys.availability_groups &#40;Transact-SQL&#41;](~/relational-databases/system-catalog-views/sys-availability-groups-transact-sql.md) fournit les métadonnées de groupe de disponibilité mises en cache dans l’espace de processus SQL Server. Par ailleurs, ces deux DMV fournissent des informations de configuration alors que [sys.dm_hadr_availability_group_states &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-group-states-transact-sql.md) fournit les états d’intégrité actuels des groupes de disponibilité.  
  
> [!IMPORTANT]  
>  Cette nomenclature est transmise avec les DMV qui documentent les réplicas et bases de données de disponibilité.  
  
## <a name="explore-the-availability-replicas"></a>Explorer les réplicas de disponibilité  
 La requête suivante récupère des informations sur les réplicas de disponibilité définis dans vos groupes de disponibilité.  
  
```sql  
select replica_id, role_desc, connected_state_desc, synchronization_health_desc from sys.dm_hadr_availability_replica_states  
go  
select replica_server_name, replica_id, availability_mode_desc, endpoint_url from sys.availability_replicas  
go  
select replica_server_name, join_state_desc from sys.dm_hadr_availability_replica_cluster_states  
go  
```  
  
 Comme pour les groupes de disponibilité, les informations sur les réplicas de disponibilité sont présentées dans trois DMV. [sys.dm_hadr_availability_replica_states](~/relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-replica-states-transact-sql.md) fournit des informations d’état sur les réplicas de disponibilité (mises en cache localement dans SQL Server), et [sys.dm_hadr_availability_replica_cluster_states](~/relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-replica-cluster-states-transact-sql.md) fournit des informations d’état sur les réplicas de disponibilité (à partir du cluster WSFC). Enfin, [sys.availability_replicas](~/relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-replica-cluster-states-transact-sql.md) fournit des données de configuration sur les réplicas de disponibilité (données mises en cache localement dans SQL Server).  
  
## <a name="explore-availability-replica-health"></a>Explorer l’intégrité du réplica de disponibilité  
 La requête suivante récupère les informations d’intégrité actuelles sur les réplicas de disponibilité.  
  
```sql  
select replica_id, role_desc, recovery_health_desc, synchronization_health_desc from sys.dm_hadr_availability_replica_states  
go  
```  
  
 Comparez les résultats de la requête sur les réplicas principal et secondaire. Sur le réplica secondaire, notez que les informations d’intégrité signalées ne s’appliquent qu’à ce réplica (et non à d’autres réplicas du groupe de disponibilité).  
  
## <a name="explore-the-availability-databases"></a>Explorer les bases de données de disponibilité  
 La requête suivante récupère des informations sur les réplicas de disponibilité définis dans vos groupes de disponibilité. Vous pouvez observer le changement dans les résultats de la requête avant et après la suspension du déplacement de données sur une base de données de disponibilité.  
  
```sql
select * from sys.availability_databases_cluster  
go  
select group_database_id, database_name, is_failover_ready  from sys.dm_hadr_database_replica_cluster_states  
go  
select database_id, synchronization_state_desc, synchronization_health_desc, last_hardened_lsn, redo_queue_size, log_send_queue_size from sys.dm_hadr_database_replica_states  
go  
```  
  
 Ici encore, trois DMV Always On fournissent des informations sur les bases de données de disponibilité. [sys.availability_databases_cluster](~/relational-databases/system-catalog-views/sys-availability-databases-cluster-transact-sql.md) fournit des informations de configuration sur les bases de données de disponibilité du cluster WSFC. [sys.dm_hadr_database_replica_cluster_states](~/relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-cluster-states-transact-sql.md) fournit des informations d’état sur les réplicas de base de données (mises en cache localement dans SQL Server). Elle contient des informations d’état importantes, comme la disponibilité de basculement du réplica de disponibilité. Enfin, [sys.dm_hadr_database_replica_states](~/relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-states-transact-sql.md) est un jeu de résultats très détaillé qui fournit des informations d’identité et d’état sur chaque base de données de disponibilité, notamment les informations de progression LSN pour les journaux des réplicas de base de données primaire et secondaire.  
  
## <a name="explore-availability-database-health"></a>Explorer l’intégrité des bases de données de disponibilité  
 La requête suivante récupère des informations sur l’intégrité de chaque base de données de disponibilité sur les réplicas. Vous pouvez observer le changement dans les résultats de la requête avant et après la suspension du déplacement de données sur une base de données de disponibilité.  
  
```sql  
select dc.database_name, dr.database_id, dr.synchronization_state_desc,   
dr.suspend_reason_desc, dr.synchronization_health_desc  
from sys.dm_hadr_database_replica_states dr  join sys.availability_databases_cluster dc  
on dr.group_database_id=dc.group_database_id   
where is_local=1  
go  
```  
  
  
