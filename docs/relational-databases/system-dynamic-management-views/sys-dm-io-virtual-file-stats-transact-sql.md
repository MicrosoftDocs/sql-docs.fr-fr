---
description: sys.dm_io_virtual_file_stats (Transact-SQL)
title: sys.dm_io_virtual_file_stats (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/11/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_io_virtual_file_stats
- sys.dm_io_virtual_file_stats_TSQL
- sys.dm_io_virtual_file_stats
- dm_io_virtual_file_stats_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_io_virtual_file_stats dynamic management function
ms.assetid: fa3e321f-6fe5-45ff-b397-02a0dd3d6b7d
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 48292ef07266a6dff28479a4f08e09d0be824d93
ms.sourcegitcommit: e40e75055c1435c5e3f9b6e3246be55526807b4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/13/2021
ms.locfileid: "98151294"
---
# <a name="sysdm_io_virtual_file_stats-transact-sql"></a>sys.dm_io_virtual_file_stats (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

  Renvoie des statistiques d'E/S sur les fichiers de données et les journaux. Cette fonction de gestion dynamique remplace la fonction [fn_virtualfilestats](../../relational-databases/system-functions/sys-fn-virtualfilestats-transact-sql.md) .  
  
> [!NOTE]  
>  Pour appeler ce à partir de [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , utilisez le nom **sys.dm_pdw_nodes_io_virtual_file_stats**. 

## <a name="syntax"></a>Syntaxe  
  
```  
-- Syntax for SQL Server and Azure SQL Database

sys.dm_io_virtual_file_stats (   
    { database_id | NULL },  
    { file_id | NULL }  
)  
```  

```  
-- Syntax for Azure Synapse Analytics

sys.dm_pdw_nodes_io_virtual_file_stats
```
  
## <a name="arguments"></a>Arguments  


 *database_id* | NUL

 **S’APPLIQUE À :** SQL Server (à partir de 2008), Azure SQL Database

 ID de la base de données. *database_id* est de type int, sans valeur par défaut. Les entrées autorisées sont l'ID d'une base de données ou la valeur NULL. Lorsque vous spécifiez la valeur NULL, toutes les bases de données de l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sont renvoyées.  
  
 Vous pouvez spécifier la fonction intégrée [DB_ID](../../t-sql/functions/db-id-transact-sql.md).  
  
*file_id* | NUL

**S’APPLIQUE À :** SQL Server (à partir de 2008), Azure SQL Database
 
ID du fichier. *file_id* est de type int, sans valeur par défaut. Les entrées autorisées sont l'ID d'un fichier ou la valeur NULL. Lorsque vous spécifiez la valeur NULL, tous les fichiers de la base de données sont renvoyés.  
  
 La fonction intégrée [FILE_IDEX](../../t-sql/functions/file-idex-transact-sql.md) peut être spécifiée et fait référence à un fichier dans la base de données active.  
  
## <a name="table-returned"></a>Table retournée  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**database_name**|**sysname**|**Ne s’applique pas à :**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .<br /><br /> Nom de la base de données.</br></br>Pour Azure Synapse Analytics, il s’agit du nom de la base de données stockée sur le nœud qui est identifiée par pdw_node_id. Chaque nœud possède une base de données tempdb qui contient 13 fichiers. Chaque nœud possède également une base de données par distribution, et chaque base de données de distribution contient 5 fichiers. Par exemple, si chaque nœud contient 4 distributions, les résultats affichent 20 fichiers de base de données de distribution par pdw_node_id. 
|**database_id**|**smallint**|ID de la base de données.|  
|**file_id**|**smallint**|ID du fichier.|  
|**sample_ms**|**bigint**|Nombre de millisecondes écoulées depuis le démarrage de l'ordinateur. Cette colonne peut être utilisée pour comparer différents résultats de cette fonction.</br></br>Le type de données est **int** pour [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] jusqu’à [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]|  
|**num_of_reads**|**bigint**|Nombre de lectures effectuées sur le fichier.|  
|**num_of_bytes_read**|**bigint**|Nombre total d'octets lus sur ce fichier.|  
|**io_stall_read_ms**|**bigint**|Durée totale (en millisecondes) d'attente des utilisateurs pour les lectures effectuées sur le fichier.|  
|**num_of_writes**|**bigint**|Nombre d'écritures effectuées sur ce fichier.|  
|**num_of_bytes_written**|**bigint**|Nombre total d'octets écrits dans le fichier.|  
|**io_stall_write_ms**|**bigint**|Durée totale (en millisecondes) d'attente des utilisateurs pour les écritures effectuées sur le fichier.|  
|**io_stall**|**bigint**|Durée totale (en millisecondes) d'attente des utilisateurs pour les entrées/sorties effectuées sur le fichier.|  
|**size_on_disk_bytes**|**bigint**|Nombre d'octets utilisés sur le disque pour ce fichier. Pour les fichiers partiellement alloués, ce nombre est le nombre réel d'octets utilisés sur le disque pour les instantanés de la base de données.|  
|**file_handle**|**varbinary**|Descripteur de fichier Windows pour ce fichier.|  
|**io_stall_queued_read_ms**|**bigint**|**Ne s’applique pas à :**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] à [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] .<br /><br /> Latence totale d'E/S introduite par la gouvernance des ressources d'E/S pour les lectures. N'accepte pas la valeur NULL. Pour plus d’informations, consultez [sys.dm_resource_governor_resource_pools &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-resource-pools-transact-sql.md).|  
|**io_stall_queued_write_ms**|**bigint**|**Ne s’applique pas à :**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] à [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] .<br /><br />  Latence totale d'E/S introduite par la gouvernance des ressources d'E/S pour les écritures. N'accepte pas la valeur NULL.|
|**pdw_node_id**|**int**|**S’applique à :** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)]</br></br>Identificateur du nœud de la distribution.
 
## <a name="remarks"></a>Notes
Les compteurs sont initialisés à vide chaque fois que le service SQL Server (MSSQLSERVER) est démarré.
  
## <a name="permissions"></a>Autorisations  
 Requiert l'autorisation VIEW SERVER STATE. Pour plus d’informations, consultez [fonctions et vues de gestion dynamique &#40;&#41;Transact-SQL ](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md).  
  
## <a name="examples"></a>Exemples  

### <a name="a-return-statistics-for-a-log-file"></a>R. Renvoyer les statistiques d’un fichier journal

**S’applique à :** SQL Server (à partir de 2008), Azure SQL Database

 Le code exemple suivant retourne les statistiques pour le fichier journal de la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```sql  
SELECT * FROM sys.dm_io_virtual_file_stats(DB_ID(N'AdventureWorks2012'), 2);  
GO  
```  
  
### <a name="b-return-statistics-for-file-in-tempdb"></a>B. Statistiques de retour pour le fichier dans tempdb

**S’applique à :** Azure Synapse Analytics

```sql
SELECT * FROM sys.dm_pdw_nodes_io_virtual_file_stats 
WHERE database_name = 'tempdb' AND file_id = 2;

```

## <a name="see-also"></a>Voir aussi  
 [Fonctions et vues de gestion dynamique &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [I O fonctions et vues de gestion dynamique associées &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/i-o-related-dynamic-management-views-and-functions-transact-sql.md)   
 [sys.database_files &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md)   
 [sys.master_files &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-master-files-transact-sql.md)  
  
  

