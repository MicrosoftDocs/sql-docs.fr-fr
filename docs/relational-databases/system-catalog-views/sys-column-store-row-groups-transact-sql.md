---
description: sys.column_store_row_groups (Transact-SQL)
title: sys.column_store_row_groups (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/28/2020
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.column_store_row_groups_TSQL
- column_store_row_groups
- sys.column_store_row_groups
- column_store_row_groups_TSQL
- deleted bitmap
dev_langs:
- TSQL
helpviewer_keywords:
- sys.column_store_row_groups catalog view
ms.assetid: 76e7fef2-d1a4-4272-a2bb-5f5dcd84aedc
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: c6e99ae69d27bbaebbd0fa8bd720820f14064d9b
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095619"
---
# <a name="syscolumn_store_row_groups-transact-sql"></a>sys.column_store_row_groups (Transact-SQL)
[!INCLUDE[sqlserver](../../includes/applies-to-version/sqlserver.md)]

  Fournit des informations sur les index cluster columnstore pour chaque segment afin d'aider l'administrateur à prendre des décisions de gestion du système. **sys.column_store_row_groups** contient une colonne pour le nombre total de lignes stockées physiquement (y compris celles marquées comme supprimées) et une colonne pour le nombre de lignes marquées comme supprimées. Utilisez **sys.column_store_row_groups** pour déterminer les groupes de lignes qui ont un pourcentage élevé de lignes supprimées et doivent être reconstruites.  
   
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**object_id**|int|ID de la table sur laquelle cet index est défini.|  
|**index_id**|int|ID de l'index de la table qui contient cet index columnstore.|  
|**partition_number**|int|ID de la partition de table qui contient le row_group_id de groupe de lignes. Utilisez partition_number pour joindre cette vue DMV à sys.partitions.|  
|**row_group_id**|int|Numéro de groupe de lignes associé à ce groupe de lignes. Cet ID est unique dans la partition.<br /><br /> -1 = fin d’une table en mémoire.|  
|* * delta_store_hobt_id|bigint * *|Hobt_id pour le groupe de lignes ouvert dans le magasin Delta.<br /><br /> NULL si le groupe de lignes n’est pas dans le magasin Delta.<br /><br /> Valeur NULL pour la fin d’une table en mémoire.|  
|**state**|TINYINT|Numéro d'ID associé à state_description.<br /><br /> 0 = INVISIBLE<br /><br /> 1 = OPEN<br /><br /> 2 = CLOSED<br /><br /> 3 = COMPRESSED <br /><br /> 4 = OBJET TOMBSTONE|  
|**state_description**|nvarchar(60)|Description de l'état permanent du groupe de lignes :<br /><br /> INVISIBLE : segment compressé masqué dans le processus de génération à partir des données d’un magasin Delta. Les actions de lecture utiliseront la banque delta jusqu'à ce que le segment compressé masqué soit terminé. Ensuite, le nouveau le segment est rendu visible, et la banque delta source est supprimée.<br /><br /> OUVRIR : groupe de lignes en lecture/écriture qui accepte les nouveaux enregistrements. Un groupe de lignes ouvert est toujours au format rowstore et n'a pas été compressé au format columnstore.<br /><br /> CLOSEd : groupe de lignes qui a été rempli, mais pas encore compressé par le processus du moteur de Tuple.<br /><br /> Compressed : groupe de lignes qui a été rempli et compressé.|  
|**total_rows**|bigint|Nombre total de lignes stockées physiquement dans le groupe de lignes. Certaines peuvent avoir été supprimées, mais elles sont toujours stockées. Le nombre maximal de lignes d'un groupe de lignes est 1 048 576 (hexadécimal FFFFF).|  
|**deleted_rows**|bigint|Nombre total de lignes marquées comme étant supprimées dans le groupe de lignes. Cette valeur est toujours 0 pour les groupes de lignes DELTA.|  
|**size_in_bytes**|bigint|Taille en octets de toutes les données dans ce groupe de lignes (sans les métadonnées ou les dictionnaires partagés), pour les rowgroups DELTA et COLUMNSTORE.|  
  
## <a name="remarks"></a>Notes  
 Retourne une ligne pour chaque groupe de lignes columnstore pour chaque table ayant un index columnstore cluster ou non cluster.  
  
 Utilisez **sys.column_store_row_groups** pour déterminer le nombre de lignes incluses dans le groupe de lignes et la taille du groupe de lignes.  
  
 Lorsque le nombre de lignes supprimées dans un groupe de lignes atteint un fort pourcentage du nombre total de lignes, la table est moins efficace. Reconstruisez l'index columnstore pour réduire la taille de la table, ce qui réduit les E/S disque nécessaires pour lire la table. Pour reconstruire l’index ColumnStore, utilisez l’option **Rebuild** de l’instruction **ALTER index** .  
  
 Le ColumnStore pouvant être mis à jour insère d’abord de nouvelles données dans un rowgroup **ouvert** , qui est au format rowstore, et est également parfois appelé table Delta.  Une fois qu’un rowgroup ouvert est plein, son état passe à **fermé**. Un rowgroup fermé est compressé au format ColumnStore par le moteur de tuple et l’état passe à **compressé**.  Le moteur de tuple est un processus en arrière-plan qui se réveille régulièrement et vérifie s'il existe des rowgroups fermés prêts à être compressés dans un rowgroup columnstore.  Le moteur de tuple libère également les rowgroups dans lesquels chaque ligne a été supprimée. Les RowGroups désalloués sont marqués comme **Tombstone**. Pour exécuter immédiatement le moteur de tuple, utilisez l’option **REorganize** de l’instruction **ALTER index** .  
  
 Lorsqu'un groupe de lignes columnstore est rempli, il est compressé, et cesse de recevoir de nouvelles lignes. Lorsque vous supprimez des lignes d'un groupe compressé, elles sont conservées mais sont marquées comme étant supprimées. Les mises à jour dans un groupe compressé sont implémentées comme une suppression du groupe compressé, et une insertion dans un groupe ouvert.  
  
## <a name="permissions"></a>Autorisations  
 Retourne des informations pour une table si l’utilisateur a l' `VIEW DEFINITION` autorisation sur la table.  
  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="examples"></a>Exemples  
 L’exemple suivant joint la table **sys.column_store_row_groups** à d’autres tables système pour retourner des informations sur des tables spécifiques. La colonne calculée `PercentFull` est une estimation de l'efficacité du groupe de lignes. Pour rechercher des informations sur une seule table, supprimez les traits d’Union de commentaire devant la clause **Where** et fournissez un nom de table.  
  
```sql  
SELECT i.object_id, object_name(i.object_id) AS TableName,   
i.name AS IndexName, i.index_id, i.type_desc,   
CSRowGroups.*,   
100*(total_rows - ISNULL(deleted_rows,0))/total_rows AS PercentFull    
FROM sys.indexes AS i  
JOIN sys.column_store_row_groups AS CSRowGroups  
    ON i.object_id = CSRowGroups.object_id  
AND i.index_id = CSRowGroups.index_id   
--WHERE object_name(i.object_id) = '<table_name>'   
ORDER BY object_name(i.object_id), i.name, row_group_id;  
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
 [sys.column_store_segments &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-column-store-segments-transact-sql.md)  
  
  

