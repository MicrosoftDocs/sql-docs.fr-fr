---
description: sys.dm_db_xtp_memory_consumers (Transact-SQL)
title: sys.dm_db_xtp_memory_consumers (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_db_xtp_memory_consumers
- dm_db_xtp_memory_consumers
- dm_db_xtp_memory_consumers_TSQL
- sys.dm_db_xtp_memory_consumers_stats_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_xtp_memory_consumers dynamic management view
ms.assetid: f7ab2eaf-e627-464d-91fe-0e170b3f37bc
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 28968656d3efbb5a9cc658168944b75e93350d44
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193616"
---
# <a name="sysdm_db_xtp_memory_consumers-transact-sql"></a>sys.dm_db_xtp_memory_consumers (Transact-SQL)
[!INCLUDE[sql-asdb-asdbmi](../../includes/applies-to-version/sql-asdb-asdbmi.md)]

  Enregistre les consommateurs de mémoire au niveau du moteur de base de données [!INCLUDE[hek_2](../../includes/hek-2-md.md)]. La vue retourne une ligne pour chaque consommateur de mémoire que le moteur de base de données utilise. Utilisez cette vue de gestion dynamique pour voir comment la mémoire est répartie entre différents objets internes.  
  
 Pour plus d’informations, consultez [OLTP en mémoire &#40;optimisation en mémoire&#41;](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md).  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|memory_consumer_id|**bigint**|ID (interne) du consommateur de mémoire.|  
|memory_consumer_type|**int**|Type de consommateur de mémoire :<br /><br /> 0 = Agrégation. (Regroupe l'utilisation de la mémoire de deux consommateurs ou plus. Ne doit pas être affiché.)<br /><br /> 2=VARHEAP (Suit la consommation de mémoire pour un segment de longueur variable.)<br /><br /> 3=HASH (Suit la consommation de mémoire pour un index.)<br /><br /> 5=DB PAGE POOL (Suit la consommation de mémoire d'un pool de pages de base de données pour les opérations runtime.) Par exemple, les variables de table et certaines analyses sérialisables. Il existe un seul consommateur de mémoire de ce type par base de données.)|  
|memory_consumer_type_desc|**nvarchar (64)**|Type de consommateur de mémoire : VARHEAP, HASH ou PGPOOL.<br /><br /> 0-(il ne doit pas être affiché).<br /><br /> 2 - VARHEAP<br /><br /> 3 - HASH<br /><br /> 5 - PGPOOL|  
|memory_consumer_desc|**nvarchar (64)**|Description de l'instance de consommateur de mémoire :<br /><br /> VARHEAP <br />Segment de mémoire de la base de données. Utilisé pour allouer des données utilisateur à une base de données (lignes).<br />Segment de mémoire de la base de données système. Utilisé pour allouer les données de la base de données qui seront incluses dans les vidages de mémoire et qui n'incluent pas les données utilisateur.<br />Segment de mémoire de l'index de plage. Segment de mémoire privé utilisé par l'index de plage pour allouer les pages BW.<br /><br /> HASH : aucune description puisque le object_id indique la table et le index_id l’index de hachage lui-même.<br /><br /> PGPOOL : pour la base de données, il n’existe qu’un seul pool de pages de base de données de pools de pages.|  
|object_id|**bigint**|ID de l'objet auquel la mémoire allouée est attribuée. Valeur négative pour les objets système.|  
|xtp_object_id|**bigint**|ID d’objet pour la table optimisée en mémoire.|  
|index_id|**int**|ID d'index du consommateur (le cas échéant). NULL pour les tables de base.|  
|allocated_bytes|**bigint**|Nombre d'octets réservés pour ce consommateur.|  
|used_bytes|**bigint**|Octets utilisés par ce consommateur. S'applique uniquement à varheap.|  
|allocation_count|**int**|Nombre d'allocations.|  
|partition_count|**int**|À usage interne uniquement|  
|sizeclass_count|**int**|À usage interne uniquement|  
|min_sizeclass|**int**|À usage interne uniquement|  
|max_sizeclass|**int**|À usage interne uniquement|  
|memory_consumer_address|**varbinary**|Adresse interne du consommateur. À usage interne uniquement.|  
|xtp_object_id|**bigint**|ID d’objet OLTP en mémoire qui correspond à la table optimisée en mémoire.|  
  
## <a name="remarks"></a>Notes  
 Dans le résultat, les allocateurs au niveau de la base de données font référence aux tables utilisateur, aux index, et aux tables système. VARHEAP avec object_id = NULL fait référence à la mémoire allouée aux tables contenant des colonnes de longueur variable.  
  
## <a name="permissions"></a>Autorisations  
 Toutes les lignes sont retournées si vous avez l'autorisation VIEW DATABASE STATE sur la base de données active. Sinon, un ensemble de lignes vide est retourné.  
  
 Si vous n'avez pas l'autorisation VIEW DATABASE, toutes les colonnes seront retournées pour les lignes dans les tables sur lesquelles vous avez l'autorisation SELECT.  
  
 Les tables système sont retournées uniquement pour les utilisateurs qui ont l'autorisation VIEW DATABASE STATE.  
  
## <a name="general-remarks"></a>Remarques d'ordre général  
 Lorsqu’une table mémoire optimisée a un index ColumnStore, le système utilise des tables internes, qui consomment de la mémoire, pour suivre les données de l’index ColumnStore. Pour plus d’informations sur ces tables internes et des exemples de requêtes indiquant leur consommation de mémoire, consultez [sys.memory_optimized_tables_internal_attributes (Transact-SQL)](../../relational-databases/system-catalog-views/sys-memory-optimized-tables-internal-attributes-transact-sql.md).
 
  
## <a name="examples"></a>Exemples  
  
```  
-- memory consumers (database level)  
SELECT OBJECT_NAME(object_id), *   
FROM sys.dm_db_xtp_memory_consumers;  
```  
  
## <a name="user-scenario"></a>Scénario d'utilisateur  
  
```  
-- memory consumers (database level)  
  
select  convert(char(10), object_name(object_id)) as Name,   
convert(char(10),memory_consumer_type_desc ) as memory_consumer_type_desc, object_id,index_id, allocated_bytes,  used_bytes   
from sys.dm_db_xtp_memory_consumers  
```  
  
 Voici le résultat avec un sous-ensemble de colonnes. Les allocateurs aux niveaux de la base de données font référence à des tables utilisateur, des index et des tables système. Le VARHEAP avec object_id = NULL (dernière ligne) fait référence à de la mémoire allouée aux lignes de données des tables (dans cet exemple, il s'agit de t1). Les octets alloués, une fois convertis en Mo, se montent à 1 340 Mo.  
  
```  
Name       memory_consumer_type_desc object_id   index_id    allocated_bytes      used_bytes  
---------- ------------------------- ----------- ----------- -------------------- --------------------  
t3         HASH                      629577281   2           8388608              8388608  
t2         HASH                      597577167   2           8388608              8388608  
t1         HASH                      565577053   2           1048576              1048576  
NULL       HASH                      -6          1           2048                 2048  
NULL       VARHEAP                   -6          NULL        0                    0  
NULL       HASH                      -5          3           8192                 8192  
NULL       HASH                      -5          2           8192                 8192  
NULL       HASH                      -5          1           8192                 8192  
NULL       HASH                      -4          1           2048                 2048  
NULL       VARHEAP                   -4          NULL        0                    0  
NULL       HASH                      -3          1           2048                 2048  
NULL       HASH                      -2          2           8192                 8192  
NULL       HASH                      -2          1           8192                 8192  
NULL       VARHEAP                   -2          NULL        196608               26496  
NULL       HASH                      0           1           2048                 2048  
NULL       PGPOOL                    0           NULL        0                    0  
NULL       VARHEAP                   NULL        NULL        1405943808           1231220560  
  
(17 row(s) affected)  
```  
  
 La mémoire totale allouée et utilisée à partir de cette vue de gestion dynamique est identique au niveau objet dans [sys.dm_db_xtp_table_memory_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-xtp-table-memory-stats-transact-sql.md).  
  
```  
select  sum(allocated_bytes)/(1024*1024) as total_allocated_MB,   
        sum(used_bytes)/(1024*1024) as total_used_MB  
from sys.dm_db_xtp_memory_consumers  
  
total_allocated_MB   total_used_MB  
-------------------- --------------------  
1358                 1191  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Vues de gestion dynamique des tables à mémoire optimisée &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/memory-optimized-table-dynamic-management-views-transact-sql.md)  
  
  
