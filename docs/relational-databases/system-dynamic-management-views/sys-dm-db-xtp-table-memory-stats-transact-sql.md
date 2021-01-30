---
description: sys.dm_db_xtp_table_memory_stats (Transact-SQL)
title: sys.dm_db_xtp_table_memory_stats (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/29/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_db_xtp_table_memory_stats_TSQL
- dm_db_xtp_table_memory_stats
- sys.dm_db_xtp_table_memory_stats
- dm_db_xtp_table_memory_stats_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_xtp_table_memory_stats
- dm_db_xtp_table_memory_stats
ms.assetid: ad0efc06-3d9c-4861-9dfa-a7a87822d0c8
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 6521cc342b94ba120271215b98970c05eed7fd62
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160111"
---
# <a name="sysdm_db_xtp_table_memory_stats-transact-sql"></a>sys.dm_db_xtp_table_memory_stats (Transact-SQL)
[!INCLUDE[sql-asdb-asdbmi](../../includes/applies-to-version/sql-asdb-asdbmi.md)]

  Fournit les statistiques d'utilisation de la mémoire pour chaque table [!INCLUDE[hek_2](../../includes/hek-2-md.md)] (utilisateur et système) dans la base de données active. Les tables système ont des ID d'objet négatifs et sont utilisées pour stocker les informations d'exécution du moteur [!INCLUDE[hek_2](../../includes/hek-2-md.md)]. Contrairement aux objets utilisateur, les tables système sont internes et existent uniquement en mémoire, par conséquent, elles ne sont pas visibles via les affichages catalogue. Les tables système sont utilisées pour stocker des informations telles que des métadonnées pour tous les fichiers de données/delta dans le stockage, les demandes de fusion, les filigranes des fichiers delta pour filtrer les lignes, les tables supprimées, et les informations pertinentes pour la récupération et les sauvegardes. Étant donné que le moteur [!INCLUDE[hek_2](../../includes/hek-2-md.md)] peut avoir jusqu'à 4 096 paires de fichiers de données et delta, pour les bases de données en mémoire volumineuses, la mémoire allouée aux tables système peut être de quelques mégaoctets.  
  
 Pour plus d’informations, consultez [OLTP en mémoire &#40;optimisation en mémoire&#41;](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md).  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|object_id|**int**|ID d'objet de la table. NULL pour les tables système de l'OLTP en mémoire.|  
|memory_allocated_for_table_kb|**bigint**|Mémoire allouée pour cette table.|  
|memory_used_by_table_kb|**bigint**|Mémoire utilisée par la table, y compris les versions de ligne.|  
|memory_allocated_for_indexes_kb|**bigint**|Mémoire allouée aux index sur cette table.|  
|memory_used_by_indexes_kb|**bigint**|Mémoire consommée pour les index sur cette table.|  
  
## <a name="permissions"></a>Autorisations  
 Toutes les lignes sont retournées si vous avez l'autorisation VIEW DATABASE STATE sur la base de données active. Sinon, un ensemble de lignes vide est retourné.  
  
 Si vous n'avez pas l'autorisation VIEW DATABASE, toutes les colonnes seront retournées pour les lignes dans les tables sur lesquelles vous avez l'autorisation SELECT.  
  
 Les tables système sont retournées uniquement pour les utilisateurs qui ont l'autorisation VIEW DATABASE STATE.  
  
## <a name="examples"></a>Exemples  
 Vous pouvez interroger la vue de gestion dynamique suivante pour obtenir la mémoire allouée aux tables et aux index de la base de données :  
  
```  
-- finding memory for objects  
SELECT OBJECT_NAME(object_id), *   
FROM sys.dm_db_xtp_table_memory_stats;  
```  
  
 Pour rechercher la mémoire de tous les objets de la base de données :  
  
```  
SELECT SUM( memory_allocated_for_indexes_kb + memory_allocated_for_table_kb) AS  
 memoryallocated_objects_in_kb   
FROM sys.dm_db_xtp_table_memory_stats;  
```  
  
## <a name="user-scenario"></a>Scénario d'utilisateur  
 Commencez par créer les tables suivantes dans une base de données appelée HkDb1.  
  
```  
-- set max server memory to 4 GB  
EXEC sp_configure 'max server memory (MB)', 4048  
go  
  
RECONFIGURE  
go  
  
-- create a resource pool for database with memory-optimized objects  
CREATE RESOURCE POOL PoolHkDb1 WITH (MAX_MEMORY_PERCENT = 50);  
ALTER RESOURCE GOVERNOR RECONFIGURE;  
go  
  
--bind the pool to the database  
EXEC sp_xtp_bind_db_resource_pool 'HkDb1', 'PoolHkdb1'  
go  
  
-- take database offline/online to associate the pool  
use master  
go  
  
alter database HkDb1 set offline  
go  
alter database HkDb1 set online  
go  
  
USE HkDb1  
go  
  
CREATE TABLE dbo.t1 (  
       c1 int NOT NULL,  
       c2 char(40) NOT NULL,  
       c3 char(8000) NOT NULL,  
  
       CONSTRAINT [pk_t1_c1] PRIMARY KEY NONCLUSTERED HASH (c1) WITH (BUCKET_COUNT = 100000)  
) WITH (MEMORY_OPTIMIZED = ON, DURABILITY = SCHEMA_AND_DATA)  
go  
  
CREATE TABLE dbo.t2 (  
       c1 int NOT NULL,  
       c2 char(40) NOT NULL,  
       c3 char(8000) NOT NULL,  
  
       CONSTRAINT [pk_t2_c1] PRIMARY KEY NONCLUSTERED HASH (c1) WITH (BUCKET_COUNT = 100000)  
) WITH (MEMORY_OPTIMIZED = ON, DURABILITY = SCHEMA_AND_DATA)  
go  
  
CREATE TABLE dbo.t3 (  
       c1 int NOT NULL,  
       c2 char(40) NOT NULL,  
       c3 char(8000) NOT NULL,  
  
       CONSTRAINT [pk_t3_c1] PRIMARY KEY NONCLUSTERED HASH (c1) WITH (BUCKET_COUNT = 1000000)  
) WITH (MEMORY_OPTIMIZED = ON, DURABILITY = SCHEMA_AND_DATA)  
go  
  
-- load 150K rows  
declare @i int = 0  
while (@i <= 150000)  
begin  
       insert t1 values (@i, 'a', replicate ('b', 8000))  
       set @i += 1;  
end  
go  
```  
  
 Lorsque des données sont chargées dans une table, vous voyez les tables définies par l'utilisateur et la quantité de stockage utilisé. Par exemple, chaque ligne d'une table peut contenir environ 8 070 octets : la taille d'allocation est 8 Ko (8 192 octets). Vous pouvez voir les index par table et la quantité de stockage utilisée. Par exemple, 1 Mo correspond à 100 000 entrées arrondies à la puissance suivante de 2 (2**17) = 131072 de 8 octets chacune. Une table peut ne pas avoir d'index, auquel cas elle affiche l'allocation de mémoire pour l'index. D'autres lignes peuvent représenter des tables système  
  
```  
select convert(char(10), object_name(object_id)) as Name,*   
from sys.dm_db_xtp_table_memory_stats  
```  
  
 Voici la sortie, en deux parties :  
  
```  
Name       object_id   memory_allocated_for_table_kb memory_used_by_table_kb  
---------- ----------- ----------------------------- -----------------------  
t3         629577281   0                             0  
t1         565577053   1372928                       1202351  
t2         597577167   0                             0  
NULL       -6          0                             0  
NULL       -5          0                             0  
NULL       -4          0                             0  
NULL       -3          0                             0  
NULL       -2          192                           25  
  
memory_allocated_for_indexes_kb memory_used_by_indexes_kb  
------------------------------- -------------------------  
8192                            8192  
1024                            1024  
8192                            8192  
2                               2  
24                              24  
2                               2  
2                               2  
16                              16  
```  
  
 Le résultat de  
  
```  
select  sum(allocated_bytes)/(1024*1024) as total_allocated_MB,   
       sum(used_bytes)/(1024*1024) as total_used_MB  
from sys.dm_db_xtp_memory_consumers  
```  
  
 est  
  
```  
total_allocated_MB   total_used_MB  
-------------------- --------------------  
1357                 1191  
```  
  
 Regardons à présent le résultat du pool de ressources. Notez que la mémoire utilisée depuis le pool s'élève à 1 356 Mo  
  
```  
select pool_id,convert(char(10), name) as Name, min_memory_percent, max_memory_percent,   
   max_memory_kb/1024 as max_memory_mb  
from sys.dm_resource_governor_resource_pools  
  
select used_memory_kb/1024 as used_memory_mb ,target_memory_kb/1024 as target_memory_mb  
from sys.dm_resource_governor_resource_pools  
```  
  
 voici le résultat :  
  
```  
pool_id     Name       min_memory_percent max_memory_percent max_memory_mb  
----------- ---------- ------------------ ------------------ --------------------  
1           internal   0                  100                3845  
2           default    0                  100                3845  
259         PoolHkDb1  0                  100                3845  
  
used_memory_mb       target_memory_mb  
-------------------- --------------------  
125                  3845  
32                   3845  
1356                 3845  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Vues de gestion dynamique des tables à mémoire optimisée &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/memory-optimized-table-dynamic-management-views-transact-sql.md)  
  
  
