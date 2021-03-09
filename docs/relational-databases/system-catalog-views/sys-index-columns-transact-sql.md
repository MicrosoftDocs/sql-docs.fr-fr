---
description: sys.index_columns (Transact-SQL)
title: sys.index_columns (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/03/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.index_columns
- sys.index_columns_TSQL
- index_columns
- index_columns_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.index_columns catalog view
ms.assetid: 211471aa-558a-475c-9b94-5913c143ed12
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 9d0f9ed720df11627d59ecb773f294e12ec98346
ms.sourcegitcommit: 15c7cd187dcff9fc91f2daf0056b12ed3f0403f0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/08/2021
ms.locfileid: "102465057"
---
# <a name="sysindex_columns-transact-sql"></a>sys.index_columns (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Contient une ligne par colonne qui fait partie d’un index **sys. Indexes** ou d’une table non ordonnée (segment de mémoire).  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**object_id**|**int**|ID de l'objet pour lequel l'index est défini.|  
|**index_id**|**int**|Identificateur de l'index où la colonne est définie.|  
|**index_column_id**|**int**|Identificateur de l'index de colonne. **index_column_id** n’est unique que dans **index_id**.|  
|**column_id**|**int**|ID de la colonne dans **object_id**.<br /><br /> 0 = Identificateur de ligne (RID) dans un index non-cluster.<br /><br /> **column_id** n’est unique que dans **object_id**.|  
|**key_ordinal**|**tinyint**|Valeur ordinale (basée sur la valeur 1) dans l'ensemble de colonnes clés.<br /><br /> 0 = N'est pas une colonne clé, ou est un index XML, un index columnstore ou un index spatial.<br /><br /> Remarque : un index XML ou spatial ne peut pas être une clé, car les colonnes sous-jacentes ne sont pas comparables, ce qui signifie que leurs valeurs ne peuvent pas être triées.|  
|**partition_ordinal**|**tinyint**|Valeur ordinale (basée sur la valeur 1) dans l'ensemble de colonnes de partitionnement. Un index cluster columnstore peut avoir au plus une colonne de partitionnement.<br /><br /> 0 = N'est pas une colonne de partitionnement.|  
|**is_descending_key**|**bit**|1 = Colonne clé d'index avec un ordre de tri descendant.<br /><br /> 0 = Colonne clé d'index avec un ordre de tri croissant, ou il s'agit d'une colonne qui fait partie d'un index de hachage.|  
|**is_included_column**|**bit**|1 = la colonne est une colonne non-clé ajoutée à l’index à l’aide de la clause CREATe INDEX INCLUDe, ou la colonne fait partie d’un index ColumnStore.<br /><br /> 0 = Colonne non incluse.<br /><br /> Les colonnes ajoutées implicitement parce qu’elles font partie de la clé de clustering ne sont pas répertoriées dans **sys.index_columns**.<br /><br /> Les colonnes ajoutées implicitement car il s'agit de colonnes de partitionnement sont retournées avec la valeur 0.| 
|**column_store_order_ordinal**</br> S’applique à : Azure Synapse Analytics (version préliminaire)|**tinyint**|Ordinal (de base 1) au sein d’un ensemble de colonnes de commande dans un index ColumnStore ordonné ordonné en clusters.|
  
## <a name="permissions"></a>Autorisations

 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="examples"></a>Exemples

 L'exemple suivant retourne tous les index et les colonnes d'index de la table `Production.BillOfMaterials`.  
  
```sql
USE AdventureWorks2012;  
GO  
SELECT i.name AS index_name  
    ,COL_NAME(ic.object_id,ic.column_id) AS column_name  
    ,ic.index_column_id  
    ,ic.key_ordinal  
,ic.is_included_column  
FROM sys.indexes AS i  
INNER JOIN sys.index_columns AS ic
    ON i.object_id = ic.object_id AND i.index_id = ic.index_id  
WHERE i.object_id = OBJECT_ID('Production.BillOfMaterials');  
  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```
  
index_name                                                 column_name        index_column_id key_ordinal is_included_column  
---------------------------------------------------------- -----------------  --------------- ----------- -------------  
AK_BillOfMaterials_ProductAssemblyID_ComponentID_StartDate ProductAssemblyID  1               1           0  
AK_BillOfMaterials_ProductAssemblyID_ComponentID_StartDate ComponentID        2               2           0  
AK_BillOfMaterials_ProductAssemblyID_ComponentID_StartDate StartDate          3               3           0  
PK_BillOfMaterials_BillOfMaterialsID                       BillOfMaterialsID  1               1           0  
IX_BillOfMaterials_UnitMeasureCode                         UnitMeasureCode    1               1           0  
  
(5 row(s) affected)  
  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Vues de catalogue d’objets &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [Affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [sys.indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)   
 [sys.objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)   
 [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md)   
 [sys.columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-columns-transact-sql.md)   
 [Questions fréquentes sur l'interrogation des catalogues système de SQL Server](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.yml)  
  
  
