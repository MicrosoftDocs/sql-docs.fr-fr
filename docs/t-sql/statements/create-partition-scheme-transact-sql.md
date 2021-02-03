---
description: CREATE PARTITION SCHEME (Transact-SQL)
title: CREATE PARTITION SCHEME (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 04/10/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CREATE PARTITION SCHEME
- SCHEME
- PARTITION SCHEME
- CREATE_PARTITION_SCHEME_TSQL
- SCHEME_TSQL
- PARTITION_SCHEME_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- partitioned indexes [SQL Server], schemes
- partitioned tables [SQL Server], schemes
- CREATE PARTITION SCHEME statement
- partition schemes [SQL Server], creating
- filegroups [SQL Server], partitions
- partitioned indexes [SQL Server], filegroups
- partitioned tables [SQL Server], filegroups
- mapping partitions [SQL Server]
ms.assetid: 5b21c53a-b4f4-4988-89a2-801f512126e4
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 079bd2ee428b706230e8d4283355603a09300d3d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99188556"
---
# <a name="create-partition-scheme-transact-sql"></a>CREATE PARTITION SCHEME (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Crée, dans la base de données active, un schéma qui mappe les partitions d'une table ou d'un index partitionné avec des groupes de fichiers. Le nombre et le domaine des partitions d'une table ou d'un index partitionné sont définis dans une fonction de partition. Vous devez d’abord créer une fonction de partition dans une instruction [CREATE PARTITION FUNCTION](../../t-sql/statements/create-partition-function-transact-sql.md) avant de créer un schéma de partition.  

>[!NOTE]
>Dans Azure SQL Database, seuls les groupes de fichiers primaires sont pris en charge.  

 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
CREATE PARTITION SCHEME partition_scheme_name  
AS PARTITION partition_function_name  
[ ALL ] TO ( { file_group_name | [ PRIMARY ] } [ ,...n ] )  
[ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *partition_scheme_name*  
 Nom du schéma de partition. Les noms de schéma de partition doivent être uniques dans la base de données et respecter les règles applicables aux [identificateurs](../../relational-databases/databases/database-identifiers.md).  
  
 *partition_function_name*  
 Nom de la fonction de partition qui utilise le schéma de partition. Les partitions créées par la fonction de partition sont mappées avec les groupes de fichiers spécifiés dans le schéma de partition. *partition_function_name* doit déjà exister dans la base de données. Une même partition ne peut pas contenir des groupes de fichiers FILESTREAM et non FILESTREAM.  
  
 ALL  
 Spécifie que toutes les partitions sont mappées au groupe de fichiers indiqué dans *file_group_name* ou au groupe de fichiers primaire si **[** PRIMARY **]** est spécifié. Si la valeur ALL est définie, un seul *file_group_name* peut être spécifié.  
  
 *file_group_name* |  **[** PRIMARY **]** [ **,** _...n_]  
 Spécifie les noms des groupes de fichiers devant contenir les partitions spécifiées par *partition_function_name*. *file_group_name* doit déjà exister dans la base de données.  
  
 Si **[** PRIMARY **]** est spécifié, la partition est stockée dans le groupe de fichiers primaire. Si la valeur ALL est définie, un seul *file_group_name* peut être spécifié. Les partitions sont affectées à des groupes de fichiers, en commençant par la partition 1, et dans l’ordre dans lequel les groupes de fichiers sont répertoriés dans [ **,** _...n_]. Un même *file_group_name* peut être spécifié plusieurs fois dans [ **,** _...n_]. Si *n* n’est pas suffisant pour contenir toutes les partitions indiquées dans *partition_function_name*, CREATE PARTITION SCHEME échoue avec une erreur.  
  
 Si *partition_function_name* génère moins de partitions que de groupes de fichiers, le premier groupe de fichiers non affecté est marqué comme NEXT USED et un message d’information s’affiche en indiquant le nom du groupe de fichiers NEXT USED. Si la valeur ALL est spécifié, l’unique *file_group_name* conserve sa propriété NEXT USED pour ce *partition_function_name*. Le groupe de fichiers NEXT USED recevra une partition supplémentaire si une instruction ALTER PARTITION FUNCTION en crée une. Utilisez ALTER PARTITION SCHEME pour créer de nouveaux groupes de fichiers non affectés qui contiendront de nouvelles partitions.  
  
 Quand vous spécifiez le groupe de fichiers primaire dans *file_group_name* [ 1 **,**_...n_], PRIMARY doit être délimité, comme dans **[** PRIMARY **]**, car il s’agit d’un mot clé.  
  
 Seul PRIMARY est pris en charge pour [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)]. Consultez l’exemple E ci-dessous. 
  
## <a name="permissions"></a>Autorisations  
 Les autorisations suivantes peuvent être utilisées pour exécuter la procédure CREATE PARTITION SCHEME :  
  
-   Autorisation ALTER ANY DATASPACE. Cette autorisation est attribuée par défaut aux membres du rôle de serveur fixe **sysadmin** et des rôles de base de données fixes **db_owner** et **db_ddladmin** .  
  
-   L'autorisation CONTROL ou ALTER sur la base de données dans laquelle le schéma de partition est créé.  
  
-   L'autorisation CONTROL SERVER ou ALTER ANY DATABASE sur le serveur de la base de données dans laquelle le schéma de partition est créé.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-creating-a-partition-scheme-that-maps-each-partition-to-a-different-filegroup"></a>R. Création d'un schéma de partition qui mappe chaque partition à un groupe de fichiers différent  
 L'exemple suivant crée une fonction de partition pour partitionner une table ou un index en quatre partitions. Un schéma de partition est ensuite créé ; il spécifie les groupes de fichiers qui contiennent chacune des quatre partitions. Cet exemple suppose que les groupes de fichiers existent déjà dans la base de données.  
  
```sql  
CREATE PARTITION FUNCTION myRangePF1 (INT)  
AS RANGE LEFT FOR VALUES (1, 100, 1000);  
GO  
CREATE PARTITION SCHEME myRangePS1  
AS PARTITION myRangePF1  
TO (test1fg, test2fg, test3fg, test4fg);  
```  
  
 Les partitions d’une table qui utilise la fonction de partition `myRangePF1` sur la colonne de partitionnement **col1** seraient affectées comme illustré dans le tableau suivant.  
  
||||||  
|-|-|-|-|-|  
|**Groupe de fichiers**|`test1fg`|`test2fg`|`test3fg`|`test4fg`|  
|**Partition**|1|2|3|4|  
|**Valeurs**|**col1** <= `1`|**col1** > `1` AND **col1** <= `100`|**col1** > `100` AND **col1** <= `1000`|**col1** > `1000`|  
  
### <a name="b-creating-a-partition-scheme-that-maps-multiple-partitions-to-the-same-filegroup"></a>B. Création d'un schéma de partition qui mappe plusieurs partitions à un même groupe de fichiers  
 Si toutes les partitions sont mappées avec le même groupe de fichiers, utilisez le mot clé ALL. En revanche, si plusieurs partitions (mais pas toutes) sont mappées avec le même groupe de fichiers, le nom de ce groupe doit être répété, comme illustré dans l'exemple suivant.  
  
```sql  
CREATE PARTITION FUNCTION myRangePF2 (INT)  
AS RANGE LEFT FOR VALUES (1, 100, 1000);  
GO  
CREATE PARTITION SCHEME myRangePS2  
AS PARTITION myRangePF2  
TO ( test1fg, test1fg, test1fg, test2fg );  
```  
  
 Les partitions d’une table qui utilise la fonction de partition `myRangePF2` sur la colonne de partitionnement **col1** seraient affectées comme illustré dans le tableau suivant.  
  
||||||  
|-|-|-|-|-|  
|**Groupe de fichiers**|`test1fg`|`test1fg`|`test1fg`|`test2fg`|  
|**Partition**|1|2|3|4|  
|**Valeurs**|**col1** <= `1`|**col1** > 1 AND **col1** <= `100`|**col1** > `100` AND **col1** <= `1000`|**col1** > `1000`|  
  
### <a name="c-creating-a-partition-scheme-that-maps-all-partitions-to-the-same-filegroup"></a>C. Création d'un schéma de partition qui mappe toutes les partitions à un même groupe de fichiers  
 L'exemple suivant crée la même fonction de partition que dans les exemples précédents. Le schéma de partition créé mappe toutes les partitions avec le même groupe de fichiers.  
  
```sql  
CREATE PARTITION FUNCTION myRangePF3 (INT)  
AS RANGE LEFT FOR VALUES (1, 100, 1000);  
GO  
CREATE PARTITION SCHEME myRangePS3  
AS PARTITION myRangePF3  
ALL TO ( test1fg );  
```  
  
### <a name="d-creating-a-partition-scheme-that-specifies-a-next-used-filegroup"></a>D. Création d'un schéma de partition qui spécifie un groupe de fichiers « NEXT USED »  
 L'exemple suivant crée la même fonction de partition que dans les exemples précédents. Le schéma de partition créé répertorie plus de groupe de fichiers que de partitions créées par la fonction de partition associée.  
  
```sql  
CREATE PARTITION FUNCTION myRangePF4 (INT)  
AS RANGE LEFT FOR VALUES (1, 100, 1000);  
GO  
CREATE PARTITION SCHEME myRangePS4  
AS PARTITION myRangePF4  
TO (test1fg, test2fg, test3fg, test4fg, test5fg)  
```  
  
 L'exécution de l'instruction renvoie le message suivant.  
  
Le schéma de partition 'myRangePS4' a été créé avec succès. 'test5fg' est marqué comme étant le prochain groupe de fichiers utilisé dans le schéma de partition 'myRangePS4'.  
  
  
 Si la fonction de partition `myRangePF4` est modifiée afin d'ajouter une partition, le groupe de fichiers `test5fg` reçoit la nouvelle partition.  

### <a name="e-creating-a-partition-scheme-only-on-primary"></a>E. Création d’un schéma de partition uniquement sur PRIMARY

 Dans Azure [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)], l’ajout de fichiers et de groupes de fichiers n’est pas pris en charge, mais le partitionnement de table est pris en charge en partitionnant dans le groupe de fichiers PRIMARY uniquement. 

L'exemple suivant crée une fonction de partition pour partitionner une table ou un index en quatre partitions. Un schéma de partition qui spécifie que toutes les partitions sont créées dans le groupe de fichiers PRIMARY est ensuite créé.  
  
```sql  
CREATE PARTITION FUNCTION myRangePF1 (INT)  
AS RANGE LEFT FOR VALUES (1, 100, 1000);  
GO  
CREATE PARTITION SCHEME myRangePS1  
AS PARTITION myRangePF1  
ALL TO ( [PRIMARY] );  
```
   
## <a name="see-also"></a>Voir aussi  
 [CREATE PARTITION FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/create-partition-function-transact-sql.md)   
 [ALTER PARTITION SCHEME &#40;Transact-SQL&#41;](../../t-sql/statements/alter-partition-scheme-transact-sql.md)   
 [DROP PARTITION SCHEME &#40;Transact-SQL&#41;](../../t-sql/statements/drop-partition-scheme-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)   
 [Créer des tables et des index partitionnés](../../relational-databases/partitions/create-partitioned-tables-and-indexes.md)   
 [sys.partition_schemes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-partition-schemes-transact-sql.md)   
 [sys.data_spaces &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-data-spaces-transact-sql.md)   
 [sys.destination_data_spaces &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-destination-data-spaces-transact-sql.md)   
 [sys.partitions &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-partitions-transact-sql.md)   
 [sys.tables &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-tables-transact-sql.md)   
 [sys.indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)   
 [sys.index_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-index-columns-transact-sql.md)  
  
  
