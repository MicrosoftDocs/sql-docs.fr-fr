---
description: sys.indexes (Transact-SQL)
title: sys. Indexes (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 02/12/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.indexes
- indexes
- sys.indexes_TSQL
- indexes_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.indexes catalog view
ms.assetid: 066bd9ac-6554-4297-88fe-d740de1f94a8
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: a2828149b22fedab245231a5bc620729c3ddd02a
ms.sourcegitcommit: 15c7cd187dcff9fc91f2daf0056b12ed3f0403f0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/08/2021
ms.locfileid: "102464568"
---
# <a name="sysindexes-transact-sql"></a>sys.indexes (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Contient une ligne par index ou segment d'un objet tabulaire, comme une table, une vue, ou une fonction table.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**object_id**|**int**|ID de l'objet auquel appartient cet index.|  
|**name**|**sysname**|Nom de l'index. le **nom** est unique dans l’objet.<br /><br /> NULL = Segment|  
|**index_id**|**int**|Identificateur de l'index. **index_id** n’est unique que dans l’objet.<br /><br /> 0 = Segment de mémoire<br /><br /> 1 ou 5 = index cluster (B Tree, ColumnStore)<br /><br /> > 1 et <> 5 = index non cluster|  
|**type**|**tinyint**|Type de l'index :<br /><br /> 0 = Segment de mémoire<br /><br /> 1 = Clustered<br /><br /> 2 = Non cluster<br /><br /> 3 = XML<br /><br /> 4 = Spatial<br /><br /> 5 = index cluster ColumnStore. **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.<br /><br /> 6 = index ColumnStore non cluster. **S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et versions ultérieures.<br /><br /> 7 = index de hachage non cluster. **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.|  
|**type_desc**|**nvarchar(60)**|Description du type d'index :<br /><br /> HEAP<br /><br /> CLUSTERED<br /><br /> NONCLUSTERED<br /><br /> XML<br /><br /> SPATIAL<br /><br /> CLUSTERed COLUMNSTORE : **s’applique à**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.<br /><br /> COLUMNSTORE non CLUSTER : **s’applique à**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et versions ultérieures.<br /><br /> HACHAGE non-CLUSTER : les index de hachage non CLUSTER sont pris en charge uniquement sur les tables optimisées en mémoire. La vue sys.hash_indexes indique les index de hachage actuels et les propriétés de hachage. Pour plus d’informations, consultez [sys.hash_indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-hash-indexes-transact-sql.md). **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.|  
|**is_unique**|**bit**|1 = L'index est unique.<br /><br /> 0 = L'index n'est pas unique.<br /><br /> Toujours 0 pour des index columnstore cluster.|  
|**data_space_id**|**int**|ID de l'espace de données de cet index. L'espace de données est soit un groupe de fichiers, soit un schéma de partition.<br /><br /> 0 = **object_id** est une fonction table ou un index en mémoire.|  
|**ignore_dup_key**|**bit**|1 = IGNORE_DUP_KEY est ON.<br /><br /> 0 = IGNORE_DUP_KEY est OFF.|  
|**is_primary_key**|**bit**|1 = L'index fait partie d'une contrainte PRIMARY KEY.<br /><br /> Toujours 0 pour des index columnstore cluster.|  
|**is_unique_constraint**|**bit**|1 = L'index fait partie d'une contrainte UNIQUE.<br /><br /> Toujours 0 pour des index columnstore cluster.|  
|**fill_factor**|**tinyint**|> 0 = pourcentage de FILLFACTOR utilisé lors de la création ou de la reconstruction de l’index.<br /><br /> 0 = Valeur par défaut<br /><br /> Toujours 0 pour des index columnstore cluster.|  
|**is_padded**|**bit**|1 = PADINDEX est ON.<br /><br /> 0 = PADINDEX est OFF.<br /><br /> Toujours 0 pour des index columnstore cluster.|  
|**is_disabled**|**bit**|1 = L'index est désactivé.<br /><br /> 0 = L'index n'est pas désactivé.|  
|**is_hypothetical**|**bit**|1 = L'index est hypothétique et ne peut être utilisé directement comme un chemin d'accès aux données. Les index hypothétiques conservent des statistiques au niveau des colonnes.<br /><br /> 0 = L'index n'est pas hypothétique.|  
|**allow_row_locks**|**bit**|1 = Index autorisant les verrous de ligne<br /><br /> 0 = Index n'autorisant pas les verrous de ligne<br /><br /> Toujours 0 pour des index columnstore cluster.|  
|**allow_page_locks**|**bit**|1 = Index autorisant les verrous de page<br /><br /> 0 = Index n'autorisant pas les verrous de page<br /><br /> Toujours 0 pour des index columnstore cluster.|  
|**has_filter**|**bit**|1 = Index disposant d'un filtre et contenant uniquement les lignes qui satisfont la définition du filtre.<br /><br /> 0 = Index ne disposant pas de filtre.|  
|**filter_definition**|**nvarchar(max)**|Expression pour le sous-ensemble de lignes inclus dans l'index filtré.<br /><br /> NULL pour un segment de mémoire, un index non filtré ou des autorisations insuffisantes sur la table.|  
|**auto_created**|**bit**|1 = l’index a été créé par le paramétrage automatique.<br /><br />0 = l’index a été créé par l’utilisateur.
|**optimize_for_sequential_key**|**bit**|1 = l’optimisation de l’insertion de la dernière page est activée pour l’index.<br><br>0 = valeur par défaut. L’optimisation de l’insertion de la dernière page de l’index a été désactivée.|

> [!NOTE]
> Le bit de **optimize_for_sequential_key** est pris en charge uniquement dans les versions SQL Server 2019 CTP 3,1 et ultérieur.
  
## <a name="permissions"></a>Autorisations  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="examples"></a>Exemples  
 L’exemple suivant retourne tous les index de la table `Production.Product` dans la [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] base de données.  
  
```  
  
SELECT i.name AS index_name  
    ,i.type_desc  
    ,is_unique  
    ,ds.type_desc AS filegroup_or_partition_scheme  
    ,ds.name AS filegroup_or_partition_scheme_name  
    ,ignore_dup_key  
    ,is_primary_key  
    ,is_unique_constraint  
    ,fill_factor  
    ,is_padded  
    ,is_disabled  
    ,allow_row_locks  
    ,allow_page_locks  
FROM sys.indexes AS i  
INNER JOIN sys.data_spaces AS ds ON i.data_space_id = ds.data_space_id  
WHERE is_hypothetical = 0 AND i.index_id <> 0   
AND i.object_id = OBJECT_ID('Production.Product');  
GO  
  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Vues de catalogue d’objets &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [Affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [sys.index_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-index-columns-transact-sql.md)   
 [sys.xml_indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-xml-indexes-transact-sql.md)   
 [sys.objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)   
 [sys.key_constraints &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-key-constraints-transact-sql.md)   
 [sys.filegroups &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-filegroups-transact-sql.md)   
 [sys.partition_schemes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-partition-schemes-transact-sql.md)   
 [Interrogation du SQL Server FAQ du catalogue système](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.yml)   
 [OLTP en mémoire &#40;Optimisation en mémoire&#41;](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md)  
  
  
