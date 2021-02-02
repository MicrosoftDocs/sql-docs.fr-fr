---
description: sys.dm_db_index_physical_stats (Transact-SQL)
title: sys.dm_db_index_physical_stats (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_db_index_physical_stats
- sys.dm_db_index_physical_stats_TSQL
- sys.dm_db_index_physical_stats
- dm_db_index_physical_stats_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_index_physical_stats dynamic management function
- fragmentation [SQL Server]
ms.assetid: d294dd8e-82d5-4628-aa2d-e57702230613
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: b8957c55955ba24131f6cc5692e17b86cd2154a4
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/01/2021
ms.locfileid: "99236154"
---
# <a name="sysdm_db_index_physical_stats-transact-sql"></a>sys.dm_db_index_physical_stats (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Renvoie les informations de taille et de fragmentation pour les données et les index de la table ou de la vue spécifiée dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour un index, une ligne est retournée pour chaque niveau de l'arbre B (B-tree) dans chaque partition. Pour un segment de mémoire, une ligne est retournée pour l'unité d'allocation IN_ROW_DATA de chaque partition. Pour les données LOB, une ligne est retournée pour l'unité d'allocation LOB_DATA de chaque partition. S'il existe des données en dépassement de capacité des lignes dans la table, une ligne est retournée pour l'unité d'allocation ROW_OVERFLOW_DATA dans chaque partition. Ne retourne pas d'informations sur les index ColumnStore xVelocity avec mémoire optimisée.  
  
> [!IMPORTANT]
> Si vous interrogez des **sys.dm_db_index_physical_stats** sur une instance de serveur qui héberge un [réplica secondaire accessible en lecture](../../database-engine/availability-groups/windows/active-secondaries-readable-secondary-replicas-always-on-availability-groups.md)Always on, vous pouvez rencontrer un problème de blocage de rétablissement. Ce problème est dû au fait que la vue de gestion dynamique acquiert un verrou IS sur la table ou la vue utilisateur spécifiée qui peut bloquer les demandes d'un thread de phase de restauration par progression concernant un verrou X sur la table ou vue utilisateur en question.  
  
 **sys.dm_db_index_physical_stats** ne retourne pas d’informations sur les index optimisés en mémoire. Pour plus d’informations sur l’utilisation des index optimisés en mémoire, consultez [sys.dm_db_xtp_index_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-xtp-index-stats-transact-sql.md).  
  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sys.dm_db_index_physical_stats (   
    { database_id | NULL | 0 | DEFAULT }  
  , { object_id | NULL | 0 | DEFAULT }  
  , { index_id | NULL | 0 | -1 | DEFAULT }  
  , { partition_number | NULL | 0 | DEFAULT }  
  , { mode | NULL | DEFAULT }  
)  
```  
  
## <a name="arguments"></a>Arguments  
 *database_id* \| Valeur NULL \| 0 \| par défaut  
 ID de la base de données. *database_id* est de type **smallint**. Les entrées autorisées sont l'ID d'une base de données ou la valeur NULL, 0 ou DEFAULT. La valeur par défaut est 0. Les valeurs NULL, 0 et DEFAULT sont des valeurs équivalentes dans ce contexte.  
  
 Spécifiez NULL pour retourner des informations concernant toutes les bases de données de l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Si vous spécifiez NULL pour *database_id*, vous devez également spécifier null pour *object_id*, *index_id* et *partition_number*.  
  
 Vous pouvez spécifier la fonction intégrée [DB_ID](../../t-sql/functions/db-id-transact-sql.md). Si vous utilisez DB_ID sans spécifier de nom de base de données, le niveau de compatibilité de la base de données active doit être égal à 90 ou plus.  
  
 *object_id* \| Valeur NULL \| 0 \| par défaut  
 ID d’objet de la table ou de la vue sur laquelle l’index est activé. *object_id* est de **type int**.  
  
 Les entrées autorisées sont l'ID d'une table et d'une vue ou la valeur NULL, 0 ou DEFAULT. La valeur par défaut est 0. Les valeurs NULL, 0 et DEFAULT sont des valeurs équivalentes dans ce contexte. À partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] , les entrées valides incluent également le nom de la file d’attente Service Broker ou le nom de la table interne de la file d’attente. Lorsque les paramètres par défaut sont appliqués (c’est-à-dire tous les objets, tous les index, etc.), les informations de fragmentation de toutes les files d’attente sont incluses dans le jeu de résultats.  
  
 Spécifiez la valeur NULL pour retourner des informations sur toutes les tables et les vues de la base de données spécifiée. Si vous spécifiez NULL pour *object_id*, vous devez également spécifier null pour *index_id* et *partition_number*.  
  
 *index_id* \| 0 valeur \| null \| -1 \| par défaut  
 Identificateur de l’index. *index_id* est de **type int**. Les entrées valides sont le numéro d’ID d’un index, 0 si *object_id* est un segment de mémoire, NULL,-1 ou default. La valeur par défaut est -1. NULL,-1 et DEFAULT sont des valeurs équivalentes dans ce contexte.  
  
 Spécifiez la valeur NULL pour retourner des informations sur tous les index d'une table de base ou d'une vue. Si vous spécifiez NULL pour *index_id*, vous devez également spécifier null pour *partition_number*.  
  
 *partition_number* \| Valeur NULL \| 0 \| par défaut  
 Numéro de partition dans l'objet. *partition_number* est de **type int**. Les entrées valides sont le *partion_number* d’un index ou d’un segment de mémoire, NULL, 0 ou default. La valeur par défaut est 0. Les valeurs NULL, 0 et DEFAULT sont des valeurs équivalentes dans ce contexte.  
  
 Spécifiez NULL pour retourner des informations sur toutes les partitions de l'objet propriétaire.  
  
 *partition_number* est de base 1. Un index ou un segment de mémoire non partitionné a *partition_number* défini sur 1.  
  
 *mode* \| valeur \| par défaut null  
 Nom du mode. *mode* spécifie le niveau d’analyse utilisé pour obtenir des statistiques. *mode* est de **type sysname**. Les entrées autorisées sont DEFAULT, NULL, LIMITED, SAMPLED ou DETAILED. La valeur par défaut (NULL) est LIMITED.  
  
## <a name="table-returned"></a>Table retournée  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|database_id|**smallint**|ID de base de données de la table ou de la vue.|  
|object_id|**int**|ID d'objet de la table ou de la vue vers laquelle pointe l'index.|  
|index_id|**int**|ID d'index d'un index.<br /><br /> 0 = Segment de mémoire.|  
|partition_number|**int**|Numéro de partition de base 1 dans l'objet propriétaire : une table, une vue ou un index.<br /><br /> 1 = Index ou segment de mémoire non partitionné.|  
|index_type_desc|**nvarchar(60)**|Description du type d'index :<br /><br /> HEAP<br /><br /> CLUSTERED INDEX<br /><br /> NONCLUSTERED INDEX<br /><br /> PRIMARY XML INDEX<br /><br /> INDEX ÉTENDU<br /><br /> XML INDEX<br /><br /> INDEX de mappage COLUMNSTORE (interne)<br /><br /> INDEX COLUMNSTORE DELETEBUFFER (interne)<br /><br /> INDEX COLUMNSTORE DELETEBITMAP (interne)|  
|hobt_id|**bigint**|ID de segment de mémoire ou d’arbre B (B-Tree) de l’index ou de la partition.<br /><br /> Outre le renvoi de la hobt_id d’index définis par l’utilisateur, cela retourne également les hobt_id des index ColumnStore internes.|  
|alloc_unit_type_desc|**nvarchar(60)**|Description du type d'unité d'allocation :<br /><br /> IN_ROW_DATA<br /><br /> LOB_DATA<br /><br /> ROW_OVERFLOW_DATA<br /><br /> L’unité d’allocation LOB_DATA contient les données stockées dans des colonnes de type **Text**, **ntext**, **image**, **varchar (max)**, **nvarchar (max)**, **varbinary (max)** et **XML**. Pour plus d’informations, consultez [Types de données &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md).<br /><br /> L’unité d’allocation ROW_OVERFLOW_DATA contient les données stockées dans les colonnes de type **varchar (n)**, **nvarchar (n)**, **varbinary (n)** et **sql_variant** qui ont été envoyées hors ligne.|  
|index_depth|**tinyint**|Nombre de niveaux d'index.<br /><br /> 1 = Segment de mémoire ou unité d'allocation LOB_DATA ou ROW_OVERFLOW_DATA.|  
|index_level|**tinyint**|Niveau actuel de l'index.<br /><br /> 0 pour des index de niveau feuille, des segments de mémoire et des unités d'allocation LOB_DATA ou ROW_OVERFLOW_DATA.<br /><br /> Supérieur à 0 pour les index de niveaux non-feuille. *index_level* est le niveau le plus élevé au niveau racine d’un index.<br /><br /> Les niveaux non-feuille des index sont traités uniquement lorsque *mode* = detailed.|  
|avg_fragmentation_in_percent|**float**|Fragmentation logique des index ou fragmentation de l'étendue des segments de mémoire dans l'unité d'allocation IN_ROW_DATA.<br /><br /> La valeur est mesurée en pourcentage et prend en compte plusieurs fichiers. Pour les définitions de la fragmentation logique et de la fragmentation de l'étendue, consultez la section Notes.<br /><br /> 0 pour les unités d'allocation LOB_DATA et ROW_OVERFLOW_DATA.<br /><br /> NULL pour les segments de mémoire lorsque *mode* = sampled.|  
|fragment_count|**bigint**|Nombre de fragments dans le niveau feuille d'une unité d'allocation IN_ROW_DATA. Pour plus d'informations sur les fragments, consultez la section Notes.<br /><br /> NULL pour les niveaux non-feuille d'un index et les unités d'allocation LOB_DATA ou ROW_OVERFLOW_DATA.<br /><br /> NULL pour les segments de mémoire lorsque *mode* = sampled.|  
|avg_fragment_size_in_pages|**float**|Nombre moyen de pages dans un fragment dans le niveau feuille d'une unité d'allocation IN_ROW_DATA.<br /><br /> NULL pour les niveaux non-feuille d'un index et les unités d'allocation LOB_DATA ou ROW_OVERFLOW_DATA.<br /><br /> NULL pour les segments de mémoire lorsque *mode* = sampled.|  
|page_count|**bigint**|Nombre total d'index ou de pages de données.<br /><br /> Pour un index, il s'agit du nombre total de pages d'index au niveau actuel de l'arbre B (B-tree) dans l'unité d'allocation IN_ROW_DATA.<br /><br /> Pour un segment de mémoire, il s'agit du nombre total de pages de données dans l'unité d'allocation IN_ROW_DATA.<br /><br /> Pour les unités d'allocation LOB_DATA or ROW_OVERFLOW_DATA, il s'agit du nombre total de pages dans l'unité d'allocation.|  
|avg_page_space_used_in_percent|**float**|Pourcentage moyen d'espace de stockage disponible utilisé dans toutes les pages.<br /><br /> Pour un index, la moyenne s'applique au niveau actuel de l'arbre B (B-tree) dans l'unité d'allocation IN_ROW_DATA.<br /><br /> Pour un segment de mémoire, il s'agit de la moyenne de toutes les pages de données dans l'unité d'allocation IN_ROW_DATA.<br /><br /> Pour les unités d'allocation LOB_DATA or ROW_OVERFLOW_DATA, il s'agit de la moyenne de toutes les pages dans l'unité d'allocation.<br /><br /> NULL lorsque *mode* = Limited.|  
|record_count|**bigint**|Nombre total d'enregistrements.<br /><br /> Pour un index, le nombre total d'enregistrements s'applique au niveau actuel de l'arbre B (B-tree) dans l'unité d'allocation IN_ROW_DATA.<br /><br /> Pour un segment de mémoire, il s'agit du nombre total d'enregistrements dans l'unité d'allocation IN_ROW_DATA.<br /><br /> **Remarque :** Pour un segment de mémoire, le nombre d’enregistrements retournés par cette fonction peut ne pas correspondre au nombre de lignes retournées par l’exécution d’un SELECT COUNT ( \* ) sur le segment de mémoire. Cela est dû au fait qu'une ligne peut contenir plusieurs enregistrements. Par exemple, lors de certaines mises à jour, une ligne de segment unique peut comporter un enregistrement de transfert et un enregistrement transféré suite à l'opération de mise à jour. Par ailleurs, la plupart des lignes LOB de grande taille sont fractionnées en plusieurs enregistrements dans le stockage LOB_DATA.<br /><br /> Pour les unités d'allocation LOB_DATA or ROW_OVERFLOW_DATA, il s'agit du nombre total d'enregistrements dans toute l'unité d'allocation.<br /><br /> NULL lorsque *mode* = Limited.|  
|ghost_record_count|**bigint**|Nombre d'enregistrements fantômes prêts à être supprimés par la tâche de nettoyage des enregistrements fantômes dans l'unité d'allocation.<br /><br /> 0 pour les niveaux non-feuille d'un index dans l'unité d'allocation IN_ROW_DATA.<br /><br /> NULL lorsque *mode* = Limited.|  
|version_ghost_record_count|**bigint**|Nombre d'enregistrements fantômes retenus par une transaction d'isolation d'instantané en attente dans une unité d'allocation.<br /><br /> 0 pour les niveaux non-feuille d'un index dans l'unité d'allocation IN_ROW_DATA.<br /><br /> NULL lorsque *mode* = Limited.|  
|min_record_size_in_bytes|**int**|Taille minimale des enregistrements en octets.<br /><br /> Pour un index, la taille minimale des enregistrements s'applique au niveau actuel de l'arbre B (B-tree) dans l'unité d'allocation IN_ROW_DATA.<br /><br /> Pour un segment de mémoire, il s'agit de la taille minimale des enregistrements dans l'unité d'allocation IN_ROW_DATA.<br /><br /> Pour les unités d'allocation LOB_DATA ou ROW_OVERFLOW_DATA, il s'agit de la taille minimale des enregistrements dans toute l'unité d'allocation.<br /><br /> NULL lorsque *mode* = Limited.|  
|max_record_size_in_bytes|**int**|Taille maximale des enregistrements en octets.<br /><br /> Pour un index, la taille maximale des enregistrements s'applique au niveau actuel de l'arbre B (B-tree) dans l'unité d'allocation IN_ROW_DATA.<br /><br /> Pour un segment de mémoire, il s'agit de la taille maximale des enregistrements dans l'unité d'allocation IN_ROW_DATA.<br /><br /> Pour les unités d'allocation LOB_DATA ou ROW_OVERFLOW_DATA, il s'agit de la taille maximale des enregistrements dans toute l'unité d'allocation.<br /><br /> NULL lorsque *mode* = Limited.|  
|avg_record_size_in_bytes|**float**|Taille moyenne des enregistrements en octets.<br /><br /> Pour un index, la taille moyenne des enregistrements s'applique au niveau actuel de l'arbre B (B-tree) dans l'unité d'allocation IN_ROW_DATA.<br /><br /> Pour un segment de mémoire, il s'agit de la taille moyenne des enregistrements dans l'unité d'allocation IN_ROW_DATA.<br /><br /> Pour les unités d'allocation LOB_DATA ou ROW_OVERFLOW_DATA, il s'agit de la taille moyenne des enregistrements dans toute l'unité d'allocation.<br /><br /> NULL lorsque *mode* = Limited.|  
|forwarded_record_count|**bigint**|Nombre d'enregistrements d'un segment de mémoire qui contiennent des pointeurs avant vers un autre emplacement de données. (Cet état se produit pendant une mise à jour, lorsque l'espace disponible est insuffisant pour stocker la nouvelle ligne à l'emplacement d'origine.)<br /><br /> NULL pour toute unité d'allocation différente des unités d'allocation IN_ROW_DATA d'un segment de mémoire.<br /><br /> NULL pour les segments de mémoire lorsque *mode* = Limited.|  
|compressed_page_count|**bigint**|Nombre de pages compressées.<br /><br /> Pour les segments de mémoire, les pages allouées récemment ne sont pas compressées avec le mode PAGE. Un segment de mémoire est compressé avec le mode PAGE sous deux conditions spéciales : lorsque les données sont importées en bloc ou lorsqu'un segment de mémoire est reconstruit. Les opérations DML par défaut qui provoquent des allocations de page ne sont pas compressées avec le mode PAGE. Reconstruisez un segment de mémoire lorsque la valeur compressed_page_count dépasse le seuil que vous souhaitez.<br /><br /> Pour les tables qui ont un index cluster, la valeur compressed_page_count indique l'efficacité de la compression PAGE.|  
|hobt_id|bigint|Pour les index ColumnStore uniquement, il s’agit de l’ID d’un ensemble de lignes qui effectue le suivi des données ColumnStore internes pour une partition. Les ensembles de lignes sont stockés sous forme de tas de données ou d’arborescences binaires. Ils ont le même ID d’index que l’index ColumnStore parent. Pour plus d’informations, consultez [sys.internal_partitions &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-internal-partitions-transact-sql.md).<br /><br /> NULL si <br /><br /> **S’applique à**: SQL Server 2016 et versions ultérieures, Azure SQL Database, Managed instance SQL Azure|  
|column_store_delete_buffer_state|TINYINT| 0 = NOT_APPLICABLE<br /><br /> 1 = OPEN<br /><br /> 2 = DRAINAGE<br /><br /> 3 = VIDAGE<br /><br /> 4 = RETRAIT<br /><br /> 5 = PRÊT<br /><br />**S’applique à**: SQL Server 2016 et versions ultérieures, Azure SQL Database, Managed instance SQL Azure|  
|column_store_delete_buff_state_desc|| NON valide : l’index parent n’est pas un index ColumnStore.<br /><br /> Les scanneurs et les relecteurs ouverts utilisent cette.<br /><br /> DRAINAGE-les suppressions s’impriment, mais les scanneurs l’utilisent encore.<br /><br /> Le vidage du tampon est fermé et les lignes de la mémoire tampon sont écrites dans le bitmap de suppression.<br /><br /> Mise hors service : les lignes du tampon de suppression fermé ont été écrites dans le bitmap de suppression, mais la mémoire tampon n’a pas été tronquée, car les scanneurs l’utilisent encore. Les nouveaux scanneurs n’ont pas besoin d’utiliser la mémoire tampon de mise hors service, car la mémoire tampon ouverte est suffisante.<br /><br /> PRÊT : ce tampon de suppression est prêt à être utilisé. <br /><br /> **S’applique à**: SQL Server 2016 et versions ultérieures, Azure SQL Database, Managed instance SQL Azure|  
|version_record_count|**bigint**|Il s’agit du nombre d’enregistrements de version de ligne en cours de conservation dans cet index.  Ces versions de ligne sont gérées par la fonctionnalité [de récupération de base de données accélérée](../../relational-databases/accelerated-database-recovery-concepts.md) . <br /><br /> [!INCLUDE[SQL2019](../../includes/applies-to-version/sqlserver2019.md)], [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] |  
|inrow_version_record_count|**bigint**|Nombre d’enregistrements de version ADR conservés dans la ligne de données pour une récupération rapide. <br /><br />  [!INCLUDE[SQL2019](../../includes/applies-to-version/sqlserver2019.md)], [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] e|  
|inrow_diff_version_record_count|**bigint**| Nombre d’enregistrements de version ADR conservés sous la forme de différences par rapport à la version de base. <br /><br /> [!INCLUDE[SQL2019](../../includes/applies-to-version/sqlserver2019.md)], [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]|  
|total_inrow_version_payload_size_in_bytes|**bigint**|Taille totale en octets des enregistrements de version contrôles pour cet index. <br /><br /> [!INCLUDE[SQL2019](../../includes/applies-to-version/sqlserver2019.md)], [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]|  
|offrow_regular_version_record_count|**bigint**|Nombre d’enregistrements de version conservés en dehors de la ligne de données d’origine. <br /><br /> [!INCLUDE[SQL2019](../../includes/applies-to-version/sqlserver2019.md)], [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]|  
|offrow_long_term_version_record_count|**bigint**|Nombre d’enregistrements de version considérés à long terme. <br /><br /> [!INCLUDE[SQL2019](../../includes/applies-to-version/sqlserver2019.md)], [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] |  

## <a name="remarks"></a>Notes  
 La fonction de gestion dynamique sys.dm_db_index_physical_stats remplace l'instruction DBCC SHOWCONTIG.  
  
## <a name="scanning-modes"></a>Modes d'analyse  
 Le mode d'exécution de la fonction détermine le niveau de l'analyse effectuée pour obtenir les données statistiques utilisées par la fonction. le *mode* est spécifié sous la forme Limited, SAMPLED ou detailed. La fonction traverse les chaînes de pages des unités d'allocation qui composent les partitions spécifiées de la table ou de l'index. sys.dm_db_index_physical_stats requiert uniquement un verrou de table Intent-Shared (IS), quel que soit le mode dans lequel il s’exécute.  
  
 Le mode LIMITED est le mode plus rapide : il analyse le plus petit nombre de pages. Pour un index, seules les pages de niveau parent de l'arbre B (B-tree) (autrement dit, les pages au-dessus du niveau feuille) sont analysées. Pour un segment de mémoire, les pages PFS et IAM associées sont examinées et les pages de données d'un segment de mémoire sont analysées en mode LIMITED.  
  
 Avec le mode LIMITED, compressed_page_count a la valeur NULL, car le [!INCLUDE[ssDE](../../includes/ssde-md.md)] analyse uniquement les pages non-feuille de l’arborescence binaire (B-Tree) et les pages IAM et PFS du tas. Utilisez le mode SAMPLEd pour obtenir une valeur estimée pour compressed_page_count et utilisez le mode DETAILed pour obtenir la valeur réelle de compressed_page_count. Le mode SAMPLED retourne des statistiques basées sur 1 pour cent d'exemple de toutes les pages dans l'index ou le segment de mémoire. Les résultats en mode SAMPLED doivent être considérés comme étant approximatifs. Si l'index ou le segment de mémoire comporte moins de 10 000 pages, le mode DETAILED est utilisé à la place du mode SAMPLED.  
  
 Le mode DETAILED analyse toutes les pages et retourne toutes les statistiques.  
  
 Les modes sont graduellement plus lents de LIMITED à DETAILED, du fait qu'ils exécutent plus de travail. Pour évaluer rapidement la taille ou le niveau de fragmentation d'une table ou d'un index, utilisez le mode LIMITED. Il s'agit du mode le plus rapide qui ne retourne pas de ligne pour chaque niveau non-feuille dans l'unité d'allocation IN_ROW_DATA de l'index.  
  
## <a name="using-system-functions-to-specify-parameter-values"></a>Utilisation de fonctions système pour spécifier des valeurs de paramètres  
 Vous pouvez utiliser les [!INCLUDE[tsql](../../includes/tsql-md.md)] fonctions [DB_ID](../../t-sql/functions/db-id-transact-sql.md) et [object_id](../../t-sql/functions/object-id-transact-sql.md) pour spécifier une valeur pour les paramètres *database_id* et *object_id* . Toutefois, la transmission de valeurs non valides à ces fonctions peut entraîner des résultats imprévisibles. Par exemple, si le nom de la base de données ou de l'objet est introuvable car il n'existe pas ou il est mal orthographié, les deux fonctions retournent la valeur NULL. La fonction sys.dm_db_index_physical_stats interprète NULL comme une valeur générique qui spécifie toutes les bases de données ou tous les objets.  
  
 En outre, la fonction OBJECT_ID est traitée avant l’appel de la fonction sys.dm_db_index_physical_stats et est donc évaluée dans le contexte de la base de données active, et non dans la base de données spécifiée dans *database_id*. Il est possible que la fonction OBJECT_ID retourne une valeur NULL ; ou, si le nom d'objet existe dans le contexte de la base de données active et la base de données spécifiée, un message d'erreur peut être retourné. Les exemples suivants présentent ces résultats inattendus.  
  
```  
USE master;  
GO  
-- In this example, OBJECT_ID is evaluated in the context of the master database.   
-- Because Person.Address does not exist in master, the function returns NULL.  
-- When NULL is specified as an object_id, all objects in the database are returned.  
-- The same results are returned when an object that is not valid is specified.  
SELECT * FROM sys.dm_db_index_physical_stats  
    (DB_ID(N'AdventureWorks'), OBJECT_ID(N'Person.Address'), NULL, NULL , 'DETAILED');  
GO  
-- This example demonstrates the results of specifying a valid object name  
-- that exists in both the current database context and  
-- in the database specified in the database_id parameter of the   
-- sys.dm_db_index_physical_stats function.  
-- An error is returned because the ID value returned by OBJECT_ID does not  
-- match the ID value of the object in the specified database.  
CREATE DATABASE Test;  
GO  
USE Test;  
GO  
CREATE SCHEMA Person;  
GO  
CREATE Table Person.Address(c1 int);  
GO  
USE AdventureWorks2012;  
GO  
SELECT * FROM sys.dm_db_index_physical_stats  
    (DB_ID(N'Test'), OBJECT_ID(N'Person.Address'), NULL, NULL , 'DETAILED');  
GO  
-- Clean up temporary database.  
DROP DATABASE Test;  
GO  
```  
  
### <a name="best-practice"></a>Bonne pratique  
 Vérifiez systématiquement qu'un ID valide est retourné lorsque vous utilisez DB_ID ou OBJECT_ID. Par exemple, lorsque vous utilisez OBJECT_ID, spécifiez un nom en trois parties, tel que `OBJECT_ID(N'AdventureWorks2012.Person.Address')` , ou testez la valeur retournée par les fonctions avant de les utiliser dans la fonction sys.dm_db_index_physical_stats. Les exemples A et B qui suivent illustrent une méthode sûre pour déterminer des ID de bases de données et d'objets.  
  
## <a name="detecting-fragmentation"></a>Détection de la fragmentation  
 La fragmentation a lieu lors de la modification des données (instructions INSERT, UPDATE et DELETE) effectuées sur la table, et donc dans les index définis sur la table. Comme ces modifications ne sont généralement pas distribuées équitablement entre les lignes de la table et des index, le remplissage de chaque page peut varier dans le temps. Pour les requêtes qui analysent tout ou partie des index d'une table, ce type de fragmentation peut entraîner des lectures de pages supplémentaires. Cela perturbe l'analyse parallèle des données.  
  
 Le niveau de fragmentation d'un index ou d'un segment de mémoire est affiché dans la colonne avg_fragmentation_in_percent. Pour les segments de mémoire, cette valeur représente la fragmentation de l'étendue du segment. Pour les index, cette valeur représente la fragmentation logique de l'index. À la différence de DBCC SHOWCONTIG, les algorithmes de calcul de la fragmentation considèrent dans les deux cas l'espace de stockage qui englobe plusieurs fichiers : ils sont donc précis.  
  
 **Fragmentation logique**  
  
 Pourcentage de pages hors service dans les pages de feuilles d'un index. Une page non ordonnée est une page pour laquelle la page physique suivante allouée à l’index n’est pas la page désignée par le pointeur de pag *e* suivante dans la page feuille actuelle.  
  
 **Fragmentation de l'étendue**  
  
 Pourcentage d'étendues hors service dans les pages de feuilles d'un segment de mémoire. Une étendue est hors service lorsque l'étendue qui contient la page active d'un segment de mémoire n'est pas physiquement l'étendue suivant l'étendue contenant la page précédente.  
  
 La valeur de avg_fragmentation_in_percent doit être aussi proche que possible de zéro pour obtenir des performances maximales. Cependant, des valeurs comprises entre 0 et 10 % sont acceptables. Toutes les méthodes de réduction de la fragmentation (par exemple, la reconstruction, la réorganisation ou la recréation) peuvent s'utiliser pour diminuer ces valeurs. Pour plus d’informations sur l’analyse du degré de fragmentation d’un index, consultez [réorganiser et reconstruire des index](../../relational-databases/indexes/reorganize-and-rebuild-indexes.md).  
  
## <a name="reducing-fragmentation-in-an-index"></a>Réduction de la fragmentation d'un index  
 Lorsqu'un index est fragmenté de telle façon que la fragmentation nuit aux performances des requêtes, il existe trois possibilités de réduction de la fragmentation :  
  
-   Supprimer l'index cluster, puis le recréer.  
  
     Le fait de recréer un index cluster redistribue les données et produit des pages de données complètes. Vous pouvez configurer le niveau de remplissage à l'aide de l'option FILLFACTOR de l'instruction CREATE INDEX. Cette méthode présente deux inconvénients : l'index est en mode hors connexion pendant la suppression et la recréation, et l'opération est atomique. Si la création de l'index est interrompue, l'index n'est pas recréé. Pour plus d’informations, consultez [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md).  
  
-   Utilisez l'instruction ALTER INDEX REORGANIZE, qui remplace DBCC INDEXDEFRAG, pour réorganiser dans un ordre logique les pages de niveau feuille de l'index. Du fait qu'il s'agit d'une opération en ligne, l'index est disponible lorsque l'instruction est en cours d'exécution. Il est également possible d'interrompre l'opération sans perdre le travail déjà effectué. L'inconvénient de cette méthode est que la réorganisation des données est moins efficace que la reconstruction d'un index et qu'elle ne met pas à jour les statistiques.  
  
-   Utilisez l'instruction ALTER INDEX REBUILD, qui remplace DBCC DBREINDEX, pour reconstruire l'index en ligne ou hors ligne. Pour plus d’informations, consultez [ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md).  
  
 La fragmentation seule n'est pas une raison suffisante pour réorganiser ou reconstruire un index. Le principal effet de la fragmentation est le ralentissement de la lecture anticipée lors de l'analyse d'un index. Les temps de réponse sont donc plus longs. Si la charge de travail d'une requête sur une table ou un index fragmenté n'implique pas d'analyses car elle concerne essentiellement des recherches de singletons, la suppression de la fragmentation peut n'avoir aucun effet.
  
> [!NOTE]  
>  L'exécution de DBCC SHRINKFILE ou de DBCC SHRINKDATABASE peut fragmenter un index si celui-ci est partiellement ou totalement déplacé pendant le compactage. Par conséquent, si une opération de compactage doit être effectuée, vous devez l'exécuter avant la suppression de la fragmentation.  
  
## <a name="reducing-fragmentation-in-a-heap"></a>Réduction de la fragmentation d'un segment de mémoire  
 Pour réduire la fragmentation de l'étendue d'un segment de mémoire, créez un index cluster sur la table puis supprimez l'index. Cela redistribue les données pendant la création de l'index cluster. Si l'on considère l'espace disponible dans la base de données, l'organisation des données est également optimale. Lorsque l'index cluster est par la suite supprimé pour recréer le segment de mémoire, les données ne sont pas déplacées et restent en position optimale. Pour plus d'informations sur l'exécution de ces opérations, consultez [CREATE INDEX](../../t-sql/statements/create-index-transact-sql.md) et [DROP INDEX](../../t-sql/statements/drop-index-transact-sql.md).  
  
> [!CAUTION]  
>  La création et la suppression d’un index cluster sur une table reconstruit deux fois tous les index non cluster de cette table.  
  
## <a name="compacting-large-object-data"></a>Compactage des données LOB  
 Par défaut, l'instruction ALTER INDEX REORGANIZE compacte les pages qui contiennent des données LOB. Du fait que les pages LOB ne sont pas libérées lorsqu'elles sont vides, le compactage de ces données peut améliorer l'utilisation de l'espace disque si un grand nombre de données LOB ont été supprimées ou si une colonne LOB est supprimée.  
  
 La réorganisation d'un index cluster spécifié compacte toutes les colonnes LOB contenues dans l'index cluster. La réorganisation d'un index non cluster compacte toutes les colonnes LOB qui sont des colonnes non-clés (incluses) dans l'index. Lorsque l'argument ALL est spécifié dans l'instruction, tous les index associés à la table ou à la vue spécifiée sont réorganisés. De plus, toutes les colonnes LOB associées à l'index cluster, la table sous-jacente ou l'index non cluster avec des colonnes incluses sont compactés.  
  
## <a name="evaluating-disk-space-use"></a>Évaluation de l'utilisation de l'espace disque  
 La colonne avg_page_space_used_in_percent indique le remplissage des pages. Pour une utilisation optimale de l'espace disque, cette valeur doit être aussi proche que possible de 100 % pour un index qui n'aura pas beaucoup d'insertions aléatoires. Cependant, un index qui a beaucoup d'insertions aléatoires et des pages très remplies comportera un nombre plus élevé de fractionnements de pages. Cela implique une fragmentation plus importante. Par conséquent, pour réduire les fractionnements, la valeur doit être inférieure à 100 %. La reconstruction d'un index en spécifiant l'option FILLFACTOR permet de modifier le remplissage des pages pour ajuster le modèle de requête à l'index. Pour plus d’informations sur le facteur de remplissage, consultez [spécifier un facteur de remplissage pour un index](../../relational-databases/indexes/specify-fill-factor-for-an-index.md). L'instruction ALTER INDEX REORGANIZE compacte également un index en essayant de remplir les pages en fonction de la dernière valeur FILLFACTOR spécifiée. Cela augmente la valeur de avg_space_used_in_percent. Notez que ALTER INDEX REORGANIZE ne peut pas réduire le remplissage des pages. Au lieu de cela, une reconstruction de l'index doit avoir lieu.  
  
## <a name="evaluating-index-fragments"></a>Évaluation des fragments des index  
 Un fragment se compose de pages de feuilles physiquement contiguës dans le même fichier d'une unité d'allocation. Un index comporte au moins un fragment. Le nombre maximal de fragments d'un index est égal au nombre de pages du niveau feuille de l'index. Des fragments plus importants signifient que moins d'opérations d'entrées/sorties sur le disque sont nécessaires pour lire le même nombre de pages. Par conséquent, plus la valeur avg_fragment_size_in_pages est élevée, meilleures sont les performances d'analyse. Les valeurs avg_fragment_size_in_pages et avg_fragmentation_in_percent sont inversement proportionnelles. La reconstruction ou la réorganisation d'un index doit donc réduire la quantité de fragmentation et augmenter la taille des fragments.  
  
## <a name="limitations-and-restrictions"></a>Limitations et restrictions  
 Ne retourne pas de données pour les index columnstore cluster.  
  
## <a name="permissions"></a>Autorisations  
 Les autorisations suivantes sont nécessaires :  
  
-   autorisation CONTROL sur l'objet spécifié dans la base de données ;  
  
-   Autorisation VIEW DATABASE STATE pour renvoyer des informations sur tous les objets dans la base de données spécifiée, en utilisant l’objet générique @*object_id*= null.  
  
-   Autorisation VIEW SERVER STATE pour retourner des informations sur toutes les bases de données, à l’aide de la base de données générique @*database_id* = null.  
  
 L'octroi de l'autorisation VIEW DATABASE STATE autorise le renvoi de tous les objets de la base de données, quelles que soient les autorisations CONTROL refusées sur des objets spécifiques.  
  
 Le refus de l'autorisation VIEW DATABASE STATE interdit le retour de tous les objets de la base de données, quelles que soient les autorisations CONTROL accordées sur des objets spécifiques. En outre, lorsque la base de données générique @*database_id*= null est spécifiée, la base de données est omise.  
  
 Pour plus d’informations, consultez [fonctions et vues de gestion dynamique &#40;&#41;Transact-SQL ](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md).  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-returning-information-about-a-specified-table"></a>R. Retour d'informations sur une table spécifiée  
 L'exemple de code suivant retourne des statistiques de taille et de fragmentation sur tous les index et partitions de la table `Person.Address`. Le mode d'analyse est défini à `'LIMITED'` pour améliorer les performances et limiter les statistiques retournées. L'exécution de cette requête nécessite, au minimum, l'autorisation CONTROL sur la table `Person.Address`.  
  
```  
DECLARE @db_id SMALLINT;  
DECLARE @object_id INT;  
  
SET @db_id = DB_ID(N'AdventureWorks2012');  
SET @object_id = OBJECT_ID(N'AdventureWorks2012.Person.Address');  
  
IF @db_id IS NULL  
BEGIN;  
    PRINT N'Invalid database';  
END;  
ELSE IF @object_id IS NULL  
BEGIN;  
    PRINT N'Invalid object';  
END;  
ELSE  
BEGIN;  
    SELECT * FROM sys.dm_db_index_physical_stats(@db_id, @object_id, NULL, NULL , 'LIMITED');  
END;  
GO  
  
```  
  
### <a name="b-returning-information-about-a-heap"></a>B. Retour d'informations sur un segment de mémoire  
 Le code exemple suivant retourne toutes les statistiques sur le segment de mémoire `dbo.DatabaseLog` de la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. Comme la table contient des données LOB, une ligne est retournée pour l'unité d'allocation `LOB_DATA`, en plus de la ligne retournée pour l'unité d'allocation `IN_ROW_ALLOCATION_UNIT` qui stocke les pages de données du segment de mémoire. L'exécution de cette requête nécessite, au minimum, l'autorisation CONTROL sur la table `dbo.DatabaseLog`.  
  
```  
DECLARE @db_id SMALLINT;  
DECLARE @object_id INT;  
SET @db_id = DB_ID(N'AdventureWorks2012');  
SET @object_id = OBJECT_ID(N'AdventureWorks2012.dbo.DatabaseLog');  
IF @object_id IS NULL   
BEGIN;  
    PRINT N'Invalid object';  
END;  
ELSE  
BEGIN;  
    SELECT * FROM sys.dm_db_index_physical_stats(@db_id, @object_id, 0, NULL , 'DETAILED');  
END;  
GO  
  
```  
  
### <a name="c-returning-information-for-all-databases"></a>C. Retour d'informations sur toutes les bases de données  
 L'exemple suivant retourne toutes les statistiques de l'ensemble des tables et des index dans l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en spécifiant la valeur générique `NULL` pour tous les paramètres. L'exécution de cette requête nécessite l'autorisation VIEW SERVER STATE.  
  
```  
SELECT * FROM sys.dm_db_index_physical_stats (NULL, NULL, NULL, NULL, NULL);  
GO  
  
```  
  
### <a name="d-using-sysdm_db_index_physical_stats-in-a-script-to-rebuild-or-reorganize-indexes"></a>D. Utilisation de sys.dm_db_index_physical_stats dans un script pour reconstruire ou réorganiser des index  
 Le code exemple suivant réorganise ou reconstruit automatiquement toutes les partitions d'une base de données dont la fragmentation moyenne est supérieure à 10 %. L'exécution de cette requête nécessite l'autorisation VIEW DATABASE STATE. Cet exemple spécifie `DB_ID` en tant que premier paramètre sans fournir de nom de base de données. Une erreur sera générée si le niveau de compatibilité de la base de données active est inférieur ou égal à 80. Pour résoudre cette erreur, remplacez `DB_ID()` par un nom de base de données valide. Pour plus d’informations sur les niveaux de compatibilité de base de données, consultez [ALTER DATABASE Compatibility Level &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md).  
  
```  
-- Ensure a USE <databasename> statement has been executed first.  
SET NOCOUNT ON;  
DECLARE @objectid int;  
DECLARE @indexid int;  
DECLARE @partitioncount bigint;  
DECLARE @schemaname nvarchar(130);   
DECLARE @objectname nvarchar(130);   
DECLARE @indexname nvarchar(130);   
DECLARE @partitionnum bigint;  
DECLARE @partitions bigint;  
DECLARE @frag float;  
DECLARE @command nvarchar(4000);   
-- Conditionally select tables and indexes from the sys.dm_db_index_physical_stats function   
-- and convert object and index IDs to names.  
SELECT  
    object_id AS objectid,  
    index_id AS indexid,  
    partition_number AS partitionnum,  
    avg_fragmentation_in_percent AS frag  
INTO #work_to_do  
FROM sys.dm_db_index_physical_stats (DB_ID(), NULL, NULL , NULL, 'LIMITED')  
WHERE avg_fragmentation_in_percent > 10.0 AND index_id > 0;  
  
-- Declare the cursor for the list of partitions to be processed.  
DECLARE partitions CURSOR FOR SELECT * FROM #work_to_do;  
  
-- Open the cursor.  
OPEN partitions;  
  
-- Loop through the partitions.  
WHILE (1=1)  
    BEGIN;  
        FETCH NEXT  
           FROM partitions  
           INTO @objectid, @indexid, @partitionnum, @frag;  
        IF @@FETCH_STATUS < 0 BREAK;  
        SELECT @objectname = QUOTENAME(o.name), @schemaname = QUOTENAME(s.name)  
        FROM sys.objects AS o  
        JOIN sys.schemas as s ON s.schema_id = o.schema_id  
        WHERE o.object_id = @objectid;  
        SELECT @indexname = QUOTENAME(name)  
        FROM sys.indexes  
        WHERE  object_id = @objectid AND index_id = @indexid;  
        SELECT @partitioncount = count (*)  
        FROM sys.partitions  
        WHERE object_id = @objectid AND index_id = @indexid;  
  
-- 30 is an arbitrary decision point at which to switch between reorganizing and rebuilding.  
        IF @frag < 30.0  
            SET @command = N'ALTER INDEX ' + @indexname + N' ON ' + @schemaname + N'.' + @objectname + N' REORGANIZE';  
        IF @frag >= 30.0  
            SET @command = N'ALTER INDEX ' + @indexname + N' ON ' + @schemaname + N'.' + @objectname + N' REBUILD';  
        IF @partitioncount > 1  
            SET @command = @command + N' PARTITION=' + CAST(@partitionnum AS nvarchar(10));  
        EXEC (@command);  
        PRINT N'Executed: ' + @command;  
    END;  
  
-- Close and deallocate the cursor.  
CLOSE partitions;  
DEALLOCATE partitions;  
  
-- Drop the temporary table.  
DROP TABLE #work_to_do;  
GO  
  
```  
  
### <a name="e-using-sysdm_db_index_physical_stats-to-show-the-number-of-page-compressed-pages"></a>E. Utilisation de sys.dm_db_index_physical_stats pour afficher le nombre de pages compressées par page  
 L'exemple suivant montre comment afficher et comparer le nombre total de pages par rapport aux pages qui sont compressées par ligne et par page. Ces informations peuvent être utilisées pour déterminer l'avantage que procure la compression pour un index ou une table.  
  
```  
SELECT o.name,  
    ips.partition_number,  
    ips.index_type_desc,  
    ips.record_count, ips.avg_record_size_in_bytes,  
    ips.min_record_size_in_bytes,  
    ips.max_record_size_in_bytes,  
    ips.page_count, ips.compressed_page_count  
FROM sys.dm_db_index_physical_stats ( DB_ID(), NULL, NULL, NULL, 'DETAILED') ips  
JOIN sys.objects o on o.object_id = ips.object_id  
ORDER BY record_count DESC;  
```  
  
### <a name="f-using-sysdm_db_index_physical_stats-in-sampled-mode"></a>F. Utilisation de sys.dm_db_index_physical_stats en mode SAMPLED  
 L'exemple suivant montre comment le mode SAMPLED retourne un nombre approximatif qui est différent de celui résultant du mode DETAILED.  
  
```  
CREATE TABLE t3 (col1 int PRIMARY KEY, col2 varchar(500)) WITH(DATA_COMPRESSION = PAGE);  
GO  
BEGIN TRAN  
DECLARE @idx int = 0;  
WHILE @idx < 1000000  
BEGIN  
    INSERT INTO t3 (col1, col2)   
    VALUES (@idx,   
    REPLICATE ('a', 100) + CAST (@idx as varchar(10)) + REPLICATE ('a', 380))  
    SET @idx = @idx + 1  
END  
COMMIT;  
GO  
SELECT page_count, compressed_page_count, forwarded_record_count, *   
FROM sys.dm_db_index_physical_stats (db_id(),   
    object_id ('t3'), null, null, 'SAMPLED');  
SELECT page_count, compressed_page_count, forwarded_record_count, *   
FROM sys.dm_db_index_physical_stats (db_id(),   
    object_id ('t3'), null, null, 'DETAILED');  
```  
  
### <a name="g-querying-service-broker-queues-for-index-fragmentation"></a>G. Interrogation des files d’attente Service Broker pour la fragmentation de l’index  
  
||  
|-|  
|**S'applique à**: [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] jusqu'à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
  
 Les exemples suivants montrent comment interroger les files d’attente de service Broker pour la fragmentation.  
  
```  
--Using queue internal table name   
select * from sys.dm_db_index_physical_stats (db_id(), object_id ('sys.queue_messages_549576996'), default, default, default)   
  
--Using queue name directly  
select * from sys.dm_db_index_physical_stats (db_id(), object_id ('ExpenseQueue'), default, default, default)  
  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions et vues de gestion dynamique &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Fonctions et vues de gestion dynamique relatives aux index &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/index-related-dynamic-management-views-and-functions-transact-sql.md)   
 [sys.dm_db_index_operational_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-operational-stats-transact-sql.md)   
 [sys.dm_db_index_usage_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-usage-stats-transact-sql.md)   
 [sys.dm_db_partition_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-partition-stats-transact-sql.md)   
 [sys.allocation_units &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-allocation-units-transact-sql.md)   
 [Vues système &#40;&#41;Transact-SQL ](../../t-sql/language-reference.md)  
  
