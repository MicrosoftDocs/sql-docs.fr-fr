---
description: sys.internal_partitions (Transact-SQL)
title: sys.internal_partitions (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/26/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: 0262df2b-5ba7-4715-b17b-3d9ce470a38e
author: ronortloff
ms.author: rortloff
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 402e8ff5793ad0ea5abd11cad60f93b6ae963df2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191404"
---
# <a name="sysinternal_partitions-transact-sql"></a>sys.internal_partitions (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

  Retourne une ligne pour chaque ensemble de lignes qui effectue le suivi des données internes pour les index ColumnStore sur les tables sur disque. Ces ensembles de lignes sont internes aux index ColumnStore et suivent les lignes supprimées, les mappages rowgroup et le magasin Delta RowGroups. Ils effectuent le suivi des données pour chaque partition de table ; chaque table possède au moins une partition. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] recrée les ensembles de lignes chaque fois qu’il reconstruit l’index ColumnStore.   
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|partition_id|**bigint**|ID de partition pour cette partition. Unique dans la base de données.|  
|object_id|**int**|ID d’objet de la table qui contient la partition.|  
|index_id|**int**|ID d’index de l’index ColumnStore défini sur la table.<br /><br /> 1 = index cluster ColumnStore<br /><br /> 2 = index ColumnStore non cluster|  
|partition_number|**int**|Numéro de partition.<br /><br /> 1 = première partition d’une table partitionnée, ou partition unique d’une table non partitionnée.<br /><br /> 2 = deuxième partition, et ainsi de suite.|  
|internal_object_type|**tinyint**|Objets rowset qui effectuent le suivi des données internes pour l’index ColumnStore.<br /><br /> 2 = COLUMN_STORE_DELETE_BITMAP<br /><br /> 3 = COLUMN_STORE_DELTA_STORE<br /><br /> 4 = COLUMN_STORE_DELETE_BUFFER<br /><br /> 5 = COLUMN_STORE_MAPPING_INDEX|  
|internal_object_type_desc|**nvarchar(60)**|COLUMN_STORE_DELETE_BITMAP : cet index bitmap effectue le suivi des lignes marquées comme supprimées du ColumnStore. La bitmap est pour chaque rowgroup, car les partitions peuvent avoir des lignes dans plusieurs RowGroups. Les lignes sont toujours présentes physiquement et occupent de l’espace dans le ColumnStore.<br /><br /> COLUMN_STORE_DELTA_STORE : stocke les groupes de lignes, appelés RowGroups, qui n’ont pas été compressés dans un stockage en colonnes. Chaque partition de table peut avoir zéro ou plusieurs deltastore RowGroups.<br /><br /> COLUMN_STORE_DELETE_BUFFER : pour conserver les suppressions des index ColumnStore non cluster actualisables. Quand une requête supprime une ligne de la table rowstore sous-jacente, le tampon de suppression effectue le suivi de la suppression à partir du ColumnStore. Lorsque le nombre de lignes supprimées dépasse 1048576, elles sont refusionnées dans le bitmap de suppression par le thread du moteur de tuple de l’arrière-plan ou par une commande REORGANIZE explicite.  À un moment donné, l’Union de l’image bitmap de suppression et du tampon de suppression représente toutes les lignes supprimées.<br /><br /> COLUMN_STORE_MAPPING_INDEX-utilisé uniquement lorsque l’index cluster ColumnStore possède un index non cluster secondaire. Cela mappe les clés d’index non cluster à la rowgroup et à l’ID de ligne corrects dans le ColumnStore. Il stocke uniquement les clés pour les lignes qui se déplacent vers un autre rowgroup ; Cela se produit lorsqu’un rowgroup Delta est compressé dans le ColumnStore, et lorsqu’une opération de fusion fusionne des lignes à partir de deux RowGroups différents.|  
|Row_group_id|**int**|ID du rowgroup deltastore. Chaque partition de table peut avoir zéro ou plusieurs deltastore RowGroups.|  
|hobt_id|**bigint**|ID de l’objet d’ensemble de lignes interne (HoBT). Il s’agit d’une bonne clé pour la jointure avec d’autres DMV pour obtenir plus d’informations sur les caractéristiques physiques de l’ensemble de lignes interne.|  
|rows|**bigint**|Nombre approximatif de lignes dans cette partition.|  
|data_compression|**tinyint**|État de compression de l’ensemble de lignes :<br /><br /> 0 = AUCUN<br /><br /> 1 = LIGNE<br /><br /> 2 = PAGE|  
|data_compression_desc|**nvarchar(60)**|État de la compression pour chaque partition. Les valeurs possibles pour les tables rowstore sont AUCUN, LIGNE et PAGE. Les valeurs possibles pour les tables columnstore sont COLUMNSTORE et COLUMNSTORE_ARCHIVE.|  
|optimize_for_sequential_key|**bit**|1 = l’optimisation de l’insertion de la dernière page a été activée pour la partition.<br><br>0 = valeur par défaut. L’optimisation de l’insertion de la dernière page de la partition est désactivée.|
  
## <a name="permissions"></a>Autorisations  
 Nécessite l’appartenance au rôle `public`. Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="general-remarks"></a>Remarques d'ordre général  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] recrée de nouveaux index internes ColumnStore chaque fois qu’il crée ou reconstruit un index ColumnStore.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-view-all-of-the-internal-rowsets-for-a-table"></a>R. Afficher tous les ensembles de lignes internes d’une table  
 Cet exemple retourne tous les ensembles de lignes ColumnStore internes pour une table. Vous pouvez également utiliser le hobt_id pour obtenir plus d’informations sur l’ensemble de lignes spécifique.  
  
```sql  
SELECT i.object_id, i.index_id, i.name, p.hobt_id, p.internal_object_type_id, p.internal_object_type_desc  
FROM sys.internal_partitions AS p  
JOIN sys.indexes AS i  
on i.object_id = p.object_id  
WHERE p.object_id = OBJECT_ID ( '<table name' ) ;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Vues de catalogue d’objets &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [Affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Questions fréquentes sur l'interrogation des catalogues système de SQL Server](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.md)  
  
  
