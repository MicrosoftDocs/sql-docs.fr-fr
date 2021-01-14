---
description: sys.dm_db_column_store_row_group_physical_stats (Transact-SQL)
title: sys.dm_db_column_store_row_group_physical_stats (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/05/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_db_column_store_row_group_physical_stats_TSQL
- sys.dm_db_column_store_row_group_physical_stats
- dm_db_column_store_row_group_physical_stats
- dm_db_column_store_row_group_physical_stats_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- dm_db_column_store_row_group_physical_stats
- sys.dm_db_column_store_row_group_physical_stats dynamic management view
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: b47d4bd97718bab9b25c6cb6d7772b08cd7251f2
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/13/2021
ms.locfileid: "98172121"
---
# <a name="sysdm_db_column_store_row_group_physical_stats-transact-sql"></a>sys.dm_db_column_store_row_group_physical_stats (Transact-SQL)


[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

Fournit les informations de niveau rowgroup actuelles sur tous les index ColumnStore dans la base de données actuelle.  

Cela étend l’affichage catalogue [sys.column_store_row_groups &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-column-store-row-groups-transact-sql.md).  

|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**object_id**|**int**|ID de la table sous-jacente.|  
|**index_id**|**int**|ID de cet index ColumnStore sur la table *object_id* .|  
|**partition_number**|**int**|ID de la partition de table qui contient *row_group_id*. Utilisez partition_number pour joindre cette vue DMV à sys.partitions.|  
|**row_group_id**|**int**|ID de ce groupe de lignes. Pour les tables partitionnées, la valeur est unique dans la partition.<br /><br /> -1 pour une fin en mémoire.|  
|**delta_store_hobt_id**|**bigint**|Hobt_id pour un groupe de lignes dans le magasin Delta.<br /><br /> NULL si le groupe de lignes ne se trouve pas dans le magasin Delta.<br /><br /> NULL pour la fin d’une table en mémoire.|  
|**state**|**tinyint**|Numéro d’identification associé *state_description*.<br /><br /> 0 = INVISIBLE<br /><br /> 1 = OPEN<br /><br /> 2 = CLOSED<br /><br /> 3 = COMPRESSED<br /><br /> 4 = OBJET TOMBSTONE<br /><br /> Compressed est le seul État qui s’applique aux tables en mémoire.|  
|**state_desc**|**nvarchar(60)**|Description de l’état du groupe de lignes :<br /><br /> 0-INVISIBLE-un groupe de lignes en cours de génération. Par exemple : <br />Un groupe de lignes dans le ColumnStore est INVISIBLE pendant la compression des données. Lorsque la compression est terminée, un changement de métadonnées modifie l’état du groupe de lignes ColumnStore de INVISIBLE à Compressed, et l’état du groupe de lignes deltastore de CLOSEd à Tombstone.<br /><br /> 1-ouvrir-un groupe de lignes deltastore qui accepte de nouvelles lignes. Un groupe de lignes ouvert est toujours au format rowstore et n'a pas été compressé au format columnstore.<br /><br /> 2-fermé : groupe de lignes dans le magasin Delta qui contient le nombre maximal de lignes et qui attend que le processus du moteur de tuple le compresse dans le ColumnStore.<br /><br /> 3-Compressed : groupe de lignes compressé avec compression ColumnStore et stocké dans le ColumnStore.<br /><br /> 4-tombstone : un groupe de lignes qui se trouvait auparavant dans deltastore et n’est plus utilisé.|  
|**total_rows**|**bigint**|Nombre de lignes physiquement stockées dans le groupe de lignes. Pour les groupes de lignes compressés. Comprend les lignes qui sont marquées comme supprimées.|  
|**deleted_rows**|**bigint**|Nombre de lignes physiquement stockées dans un groupe de lignes compressé marqué pour suppression.<br /><br /> 0 pour les groupes de lignes qui se trouvent dans le magasin delta.|  
|**size_in_bytes**|**bigint**|Taille combinée, en octets, de toutes les pages de ce groupe de lignes. Cette taille n’inclut pas la taille requise pour stocker les métadonnées ou les dictionnaires partagés.|  
|**trim_reason**|**tinyint**|Raison pour laquelle le groupe de lignes compressé a une valeur inférieure au nombre maximal de lignes.<br /><br /> 0-UNKNOWN_UPGRADED_FROM_PREVIOUS_VERSION<br /><br /> 1-NO_TRIM<br /><br /> 2-BULKLOAD<br /><br /> 3-REORG<br /><br /> 4-DICTIONARY_SIZE<br /><br /> 5-MEMORY_LIMITATION<br /><br /> 6-RESIDUAL_ROW_GROUP<br /><br /> 7-STATS_MISMATCH<br /><br /> 8-DÉBORDEMENT<br /><br /> 9-AUTO_MERGE|  
|**trim_reason_desc**|**nvarchar(60)**|Description de *trim_reason*.<br /><br /> 0-UNKNOWN_UPGRADED_FROM_PREVIOUS_VERSION : s’est produit lors de la mise à niveau à partir de la version précédente de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .<br /><br /> 1-NO_TRIM : le groupe de lignes n’a pas été tronqué. Le groupe de lignes a été compressé avec un maximum de 1 048 576 lignes.  Le nombre de lignes peut être inférieur si un sous-ensemble de lignes a été supprimé après la fermeture de rowgroup Delta<br /><br /> 2-BULKLOAD : la taille de lot de chargement en masse est limitée au nombre de lignes.<br /><br /> 3-REORG : compression forcée dans le cadre de la commande REORG.<br /><br /> 4-DICTIONARY_SIZE : taille du dictionnaire trop importante pour compresser toutes les lignes.<br /><br /> 5-MEMORY_LIMITATION : mémoire disponible insuffisante pour compresser toutes les lignes.<br /><br /> 6-RESIDUAL_ROW_GROUP : fermé dans le cadre du dernier groupe de lignes avec des lignes < 1 million lors de l’opération de génération d’index<br /><br /> 7-STATS_MISMATCH : uniquement pour ColumnStore sur une table en mémoire. Si les statistiques ne sont pas indiquées correctement >= 1 million lignes qualifiées dans la fin mais que nous en ayons trouvé moins, le rowgroup compressé aura < 1 million lignes<br /><br /> 8-débordement : uniquement pour ColumnStore sur une table en mémoire. Si tail a > 1 million lignes qualifiées, les dernières lignes de lot restantes sont compressées si le nombre est compris entre 100 Ko et 1 million<br /><br /> 9-AUTO_MERGE : une opération de fusion du moteur de tuple en cours d’exécution dans l’arrière-plan A consolidé un ou plusieurs RowGroups dans ce rowgroup.|  
|**transition_to_compressed_state**|TINYINT|Montre comment ce rowgroup est passé du deltastore à un État compressé dans le ColumnStore.<br /><br /> 1-NOT_APPLICABLE<br /><br /> 2-INDEX_BUILD<br /><br /> 3-TUPLE_MOVER<br /><br /> 4-REORG_NORMAL<br /><br /> 5-REORG_FORCED<br /><br /> 6-BULKLOAD<br /><br /> 7-FUSION|  
|**transition_to_compressed_state_desc**|nvarchar(60)| 1-NOT_APPLICABLE-l’opération ne s’applique pas au deltastore. Ou bien, le rowgroup a été compressé avant la mise à niveau vers, [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] auquel cas l’historique n’est pas conservé.<br /><br /> 2-INDEX_BUILD : une reconstruction d’index créée ou index a compressé rowgroup.<br /><br /> 3-TUPLE_MOVER-le moteur de TUPLE en cours d’exécution en arrière-plan a compressé rowgroup. Le moteur de tuple se produit après que le rowgroup a remplacé l’état ouvert par fermé.<br /><br /> 4-REORG_NORMAL-l’opération de réorganisation, ALTER INDEX... REORG, a déplacé le rowgroup fermé de deltastore vers ColumnStore. Cela s’est produit avant que le déposant de tuple ait le temps de déplacer le rowgroup.<br /><br /> 5-REORG_FORCED : ce rowgroup a été ouvert dans le deltastore et a été forcé dans le ColumnStore avant d’avoir un nombre complet de lignes.<br /><br /> 6-BULKLOAD : une opération de chargement en bloc A compressé directement les rowgroup sans utiliser deltastore.<br /><br /> 7-fusion-une opération de fusion A consolidé un ou plusieurs RowGroups dans ce rowgroup, puis A effectué la compression ColumnStore.|  
|**has_vertipaq_optimization**|bit|L’optimisation de VertiPaq améliore la compression ColumnStore en réorganisant l’ordre des lignes dans le rowgroup pour obtenir une compression supérieure. Cette optimisation se produit automatiquement dans la plupart des cas. Il existe deux cas où l’optimisation VertiPaq n’est pas utilisée :<br/>  a. Lorsqu’un rowgroup Delta se déplace dans le ColumnStore et qu’il y a un ou plusieurs index non cluster sur l’index ColumnStore, dans ce cas VertiPaq l’optimisation est ignorée pour réduire les modifications apportées à l’index de mappage.<br/> b. pour les index ColumnStore sur les tables optimisées en mémoire. <br /><br /> 0 = Non<br /><br /> 1 = Oui|  
|**toute**|bigint|Génération de groupe de lignes associée à ce groupe de lignes.|  
|**created_time**|datetime2|Heure à laquelle le rowgroup a été créé.<br /><br /> NULL : pour un index ColumnStore sur une table en mémoire.|  
|**closed_time**|datetime2|Heure à laquelle le rowgroup a été fermé.<br /><br /> NULL : pour un index ColumnStore sur une table en mémoire.|  
| &nbsp; | &nbsp; | &nbsp; |

## <a name="results"></a>Résultats  
 Retourne une ligne pour chaque rowgroup de la base de données active.  
  
## <a name="permissions"></a>Autorisations  
Nécessite `CONTROL` l’autorisation sur la table et l' `VIEW DATABASE STATE` autorisation sur la base de données.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-calculate-fragmentation-to-decide-when-to-reorganize-or-rebuild-a-columnstore-index"></a>R. Calculez la fragmentation pour décider quand réorganiser ou reconstruire un index ColumnStore.  
 Pour les index ColumnStore, le pourcentage de lignes supprimées est une bonne mesure pour la fragmentation dans un rowgroup. Lorsque la fragmentation est de 20% ou plus, supprimez les lignes supprimées. Pour obtenir plus d’exemples, consultez [réorganiser et reconstruire des index](~/relational-databases/indexes/reorganize-and-rebuild-indexes.md).  
  
 Cet exemple joint **sys.dm_db_column_store_row_group_physical_stats** avec d’autres tables système, puis calcule la `Fragmentation` colonne comme une estimation de l’efficacité de chaque groupe de lignes dans la base de données actuelle. Pour rechercher des informations sur une seule table, supprimez les traits d’Union de commentaire devant la clause **Where** et fournissez un nom de table.  
  
```sql  
SELECT i.object_id,   
    object_name(i.object_id) AS TableName,   
    i.name AS IndexName,   
    i.index_id,   
    i.type_desc,   
    CSRowGroups.*,  
    100*(ISNULL(deleted_rows,0))/NULLIF(total_rows,0) AS 'Fragmentation'
FROM sys.indexes AS i  
JOIN sys.dm_db_column_store_row_group_physical_stats AS CSRowGroups  
    ON i.object_id = CSRowGroups.object_id AND i.index_id = CSRowGroups.index_id   
-- WHERE object_name(i.object_id) = 'table_name'   
ORDER BY object_name(i.object_id), i.name, row_group_id;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Vues de catalogue d’objets &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [Affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)      
 [Index columnstore - Architecture](../../relational-databases/sql-server-index-design-guide.md#columnstore_index)         
 [Interrogation du SQL Server FAQ du catalogue système](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.md)   
 [sys.columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-columns-transact-sql.md)   
 [sys.all_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-all-columns-transact-sql.md)   
 [sys.computed_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-computed-columns-transact-sql.md)  
 [sys.column_store_dictionaries &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-column-store-dictionaries-transact-sql.md)   
 [sys.column_store_segments &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-column-store-segments-transact-sql.md)  
  
