---
description: sys.column_store_segments (Transact-SQL)
title: sys.column_store_segments (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/24/2020
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- column_store_segments
- sys.column_store_segments_TSQL
- sys.column_store_segments
- column_store_segments_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.column_store_segments catalog view
ms.assetid: 1253448c-2ec9-4900-ae9f-461d6b51b2ea
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 189a5cca5cfac0cce6437ccc256d461140f3c40b
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095604"
---
# <a name="syscolumn_store_segments-transact-sql"></a>sys.column_store_segments (Transact-SQL)
[!INCLUDE[sqlserver](../../includes/applies-to-version/sqlserver.md)]

Retourne une ligne pour chaque segment de colonne dans un index ColumnStore. Il y a un segment de colonne par colonne par rowgroup. Par exemple, une table contenant 10 colonnes RowGroups et 34 renvoie 340 lignes. 
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**partition_id**|**bigint**|Indique l'ID de partition. Unique dans une base de données.|  
|**hobt_id**|**bigint**|ID de l’index de segment de mémoire ou arbre B (B-tree) (HoBT) pour la table qui contient cet index ColumnStore.|  
|**column_id**|**int**|ID de la colonne columnstore.|  
|**segment_id**|**int**|ID du rowgroup. Pour la compatibilité descendante, le nom de colonne continue d’être appelé segment_id même s’il s’agit de l’ID rowgroup. Vous pouvez identifier un segment de manière unique à l’aide \<hobt_id, partition_id, column_id> de, <segment_id>.|  
|**version**|**int**|Version du format de segment de colonne.|  
|**encoding_type**|**int**|Type d’encodage utilisé pour ce segment :<br /><br /> 1 = VALUE_BASED non chaîne/binaire sans dictionnaire (semblable à 4 avec certaines variations internes)<br /><br /> 2 = VALUE_HASH_BASED-colonne non chaîne/binaire avec des valeurs communes dans le dictionnaire<br /><br /> 3 = STRING_HASH_BASED-chaîne/colonne binaire avec des valeurs communes dans le dictionnaire<br /><br /> 4 = STORE_BY_VALUE_BASED-non-chaîne/binaire sans dictionnaire<br /><br /> 5 = STRING_STORE_BY_VALUE_BASED chaîne/binaire sans dictionnaire<br /><br /> Pour plus d’informations, consultez la section [Notes](#remarks) .|  
|**row_count**|**int**|Nombre de lignes dans le groupe de lignes.|  
|**has_nulls**|**int**|1 si le segment de colonne a des valeurs NULL.|  
|**base_id**|**bigint**|ID de la valeur de base si le type d’encodage 1 est utilisé. Si le type d’encodage 1 n’est pas utilisé, base_id a la valeur-1.|  
|**magnitude**|**float**|Magnitude si le type d’encodage 1 est utilisé. Si le type d’encodage 1 n’est pas utilisé, l’argument magnitude a la valeur-1.|  
|**primary_dictionary_id**|**int**|La valeur 0 représente le dictionnaire global. La valeur-1 indique qu’il n’existe aucun dictionnaire global créé pour cette colonne.|  
|**secondary_dictionary_id**|**int**|Une valeur différente de zéro pointe vers le dictionnaire local de cette colonne dans le segment actuel (c’est-à-dire, rowgroup). La valeur-1 indique qu’il n’existe aucun dictionnaire local pour ce segment.|  
|**min_data_id**|**bigint**|ID de données minimal dans le segment de colonne.|  
|**max_data_id**|**bigint**|ID de données maximal dans le segment de colonne.|  
|**null_value**|**bigint**|Valeur utilisée pour représenter les valeurs NULL.|  
|**on_disk_size**|**bigint**|Taille de segment en octets.|  
  
## <a name="remarks"></a>Notes  
Le type d’encodage de segment ColumnStore est sélectionné par le [!INCLUDE[ssde_md](../../includes/ssde_md.md)] , avec l’objectif d’obtenir le coût de stockage le plus bas, en analysant les données de segment. Si les données sont essentiellement distinctes, [!INCLUDE[ssde_md](../../includes/ssde_md.md)] utilise l’encodage basé sur des valeurs. Si les données ne sont généralement pas distinctes, le [!INCLUDE[ssde_md](../../includes/ssde_md.md)] utilise l’encodage basé sur le hachage. Le choix entre l’encodage basé sur une chaîne et sur la valeur est lié au type de données stockées, qu’il s’agisse de données de type chaîne ou binaires. Tous les encodages tirent parti de l’encodage de bits et de la longueur d’exécution lorsque cela est possible.
 
## <a name="permissions"></a>Autorisations  
 Toutes les colonnes requièrent au moins `VIEW DEFINITION` l’autorisation sur la table. Les colonnes suivantes retournent la valeur null, sauf si l’utilisateur a également l' `SELECT` autorisation : `has_nulls` ,,,, `base_id` `magnitude` `min_data_id` `max_data_id` et `null_value` .  
  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  

## <a name="examples"></a>Exemples

### <a name="the-following-query-returns-information-about-segments-of-a-columnstore-index"></a>La requête suivante retourne des informations sur les segments d'un index columnstore.  
  
```sql  
SELECT i.name, p.object_id, p.index_id, i.type_desc,   
    COUNT(*) AS number_of_segments  
FROM sys.column_store_segments AS s   
INNER JOIN sys.partitions AS p   
    ON s.hobt_id = p.hobt_id   
INNER JOIN sys.indexes AS i   
    ON p.object_id = i.object_id  
WHERE i.type = 5 OR i.type = 6  
GROUP BY i.name, p.object_id, p.index_id, i.type_desc ;  
GO  
```  

## <a name="see-also"></a>Voir aussi  
 [Vues de catalogue d’objets &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [Affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Interrogation du SQL Server FAQ du catalogue système](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.md)   
 [sys.columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-columns-transact-sql.md)   
 [sys.all_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-all-columns-transact-sql.md)   
 [sys.computed_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-computed-columns-transact-sql.md)   
 [Guide des index columnstore](~/relational-databases/indexes/columnstore-indexes-overview.md)    
 [sys.column_store_dictionaries &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-column-store-dictionaries-transact-sql.md)  
  
 
