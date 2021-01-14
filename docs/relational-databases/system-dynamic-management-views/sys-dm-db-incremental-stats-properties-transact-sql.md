---
description: sys.dm_db_incremental_stats_properties (Transact-SQL)
title: sys.dm_db_incremental_stats_properties (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 12/18/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_db_incremental_stats_properties
- sys.dm_db_incremental_stats_properties_TSQL
- dm_db_incremental_stats_properties
- dm_db_incremental_stats_properties_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_incremental_stats_properties
ms.assetid: aa0db893-34d1-419c-b008-224852e71307
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 27585efd99f537e6b2f6d3082c341533e8bbea3b
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/13/2021
ms.locfileid: "98172091"
---
# <a name="sysdm_db_incremental_stats_properties-transact-sql"></a>sys.dm_db_incremental_stats_properties (Transact-SQL)
[!INCLUDE[sqlserver](../../includes/applies-to-version/sqlserver.md)]

  Retourne les propriétés de statistiques incrémentielles de l’objet de base de données spécifié (table) dans la base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] active. L’utilisation de `sys.dm_db_incremental_stats_properties` (qui contient un numéro de partition) est similaire à celle de `sys.dm_db_stats_properties` qui est utilisé pour les statistiques non incrémentielles. 
  
  Cette fonction a été introduite dans [!INCLUDE[ssSQL14_md](../../includes/sssql14-md.md)] Service Pack 2 et [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] Service Pack 1.
  
## <a name="syntax"></a>Syntaxe  
  
```  
sys.dm_db_incremental_stats_properties (object_id, stats_id)  
```  
  
## <a name="arguments"></a>Arguments  
 *object_id*  
 ID de l’objet dans la base de données active dont les propriétés d’une de ses statistiques incrémentielles sont demandées. *object_id* est de **type int**.  
  
 *stats_id*  
 ID des statistiques pour *l’object_id*. spécifié. L’ID des statistiques peut être obtenu à partir de la vue de gestion dynamique [sys.stats](../../relational-databases/system-catalog-views/sys-stats-transact-sql.md) . *stats_id* correspond à **int**.  
  
## <a name="table-returned"></a>Table retournée  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|object_id|**int**|ID de l’objet (table) pour lequel retourner les propriétés de l’objet de statistiques.|  
|stats_id|**int**|ID de l'objet de statistiques. Est unique dans la table. Pour plus d’informations, consultez [sys.stats &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-stats-transact-sql.md).|
|partition_number|**int**|Numéro de la partition contenant la partie de la table.|  
|last_updated|**datetime2**|Date et heure de la dernière mise à jour de l'objet de statistiques. Pour plus d’informations, consultez la section [Notes](#Remarks) dans cette page.|  
|rows|**bigint**|Nombre total de lignes dans la table au moment de la dernière mise à jour des statistiques. Si les statistiques sont filtrées ou correspondent à un index filtré, le nombre de lignes peut être inférieur à celui de la table.|  
|rows_sampled|**bigint**|Nombre total de lignes échantillonnées pour le calcul des statistiques.|  
|steps|**int**|Nombre d'étapes dans l'histogramme. Pour plus d’informations, consultez [DBCC SHOW_STATISTICS &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-show-statistics-transact-sql.md).|  
|unfiltered_rows|**bigint**|Nombre total de lignes dans la table avant l'application de l'expression de filtre (pour les statistiques filtrées). Si les statistiques ne sont pas filtrées, unfiltered_rows est égal à la valeur retournée dans la colonne rows.|  
|modification_counter|**bigint**|Nombre total de modifications de la première colonne de statistiques (la colonne sur laquelle l'histogramme est construit) depuis la dernière mise à jour des statistiques.<br /><br /> Cette colonne ne contient pas d'informations pour les tables mémoire optimisées.|  
  
## <a name="remarks"></a><a name="Remarks"></a> Notes  
 `sys.dm_db_incremental_stats_properties` retourne un ensemble de lignes vide sous chacune des conditions suivantes :  
  
-   `object_id` ou `stats_id` est NULL.   
-   L’objet spécifié est introuvable ou ne correspond à aucune table avec des statistiques incrémentielles.  
-   L'ID de statistiques spécifié ne correspond pas à des statistiques existantes pour l'ID d'objet spécifié.  
-   L'utilisateur actuel n'est pas autorisé à afficher l'objet de statistiques.
 
 Ce comportement permet d’utiliser `sys.dm_db_incremental_stats_properties` en toute sécurité lorsqu’un croisement est appliqué aux lignes dans les vues telles que `sys.objects` et `sys.stats`. Cette méthode peut retourner des propriétés pour les statistiques qui correspondent à chaque partition. Pour afficher les propriétés pour les statistiques fusionnées combinées sur toutes les partitions, utilisez sys.dm_db_stats_properties à la place. 

La date de mise à jour des statistiques est stockée dans l’[objet blob de statistiques](../../relational-databases/statistics/statistics.md#DefinitionQOStatistics) avec l’[histogramme](../../relational-databases/statistics/statistics.md#histogram) et le [vecteur de densité](../../relational-databases/statistics/statistics.md#density), et non dans les métadonnées. Quand aucune donnée n’est lue pour générer des données de statistiques, l’objet blob de statistiques n’est pas créé, la date n’est pas disponible et la colonne *last_updated* a la valeur null. C’est le cas pour les statistiques filtrées pour lesquelles le prédicat ne renvoie aucune ligne, ou pour les nouvelles tables vides.

## <a name="permissions"></a>Autorisations  
 L'utilisateur doit avoir sélectionné des autorisations sur les colonnes de statistiques, ou bien il doit être le propriétaire de la table, ou encore il doit être membre du rôle serveur fixe `sysadmin`, du rôle de base de données fixe `db_owner` ou du rôle de base de données fixe `db_ddladmin`.  
  
## <a name="examples"></a>Exemples  

### <a name="a-simple-example"></a>R. Exemple simple
L’exemple suivant retourne les statistiques pour la table `PartitionTable` décrite dans la rubrique [Créer des tables et des index partitionnés](../../relational-databases/partitions/create-partitioned-tables-and-indexes.md).

```sql
SELECT * FROM sys.dm_db_incremental_stats_properties (object_id('PartitionTable'), 1);
``` 

Pour des suggestions d’utilisation supplémentaires, consultez  [sys.dm_db_stats_properties](../../relational-databases/system-dynamic-management-views/sys-dm-db-stats-properties-transact-sql.md).
  
## <a name="see-also"></a>Voir aussi  
 [DBCC SHOW_STATISTICS &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-show-statistics-transact-sql.md)   
 [sys.stats &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-stats-transact-sql.md)   
 [Fonctions et vues de gestion dynamique liées aux objets &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/object-related-dynamic-management-views-and-functions-transact-sql.md)   
 [Fonctions et vues de gestion dynamique &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)  
 [sys.dm_db_stats_properties](../../relational-databases/system-dynamic-management-views/sys-dm-db-stats-properties-transact-sql.md)   
 [sys.dm_db_stats_histogram (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-db-stats-histogram-transact-sql.md) 
