---
description: sys.fulltext_indexes (Transact-SQL)
title: sys.fulltext_indexes (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- fulltext_indexes
- fulltext_indexes_TSQL
- sys.fulltext_indexes_TSQL
- sys.fulltext_indexes
dev_langs:
- TSQL
helpviewer_keywords:
- sys.fulltext_indexes catalog view
- full-text indexes [SQL Server], properties
ms.assetid: 7fc10fdc-370f-4927-bba0-b76108a7508e
author: pmasl
ms.author: pelopes
ms.reviewer: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 46b65b9a9662df54a9945adfc78ca4f38d9343d1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193879"
---
# <a name="sysfulltext_indexes-transact-sql"></a>sys.fulltext_indexes (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Contient une ligne par index de recherche en texte intégral d'un objet tabulaire.  

|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**object_id**|**int**|ID de l'objet auquel cet index de texte intégral appartient.|  
|**unique_index_id**|**int**|ID de l'index de texte non intégral unique correspondant utilisé pour associer l'index de texte intégral aux lignes.|  
|**fulltext_catalog_id**|**int**|ID du catalogue de texte intégral dans lequel réside l'index de texte intégral.|  
|**is_enabled**|**bit**|1 = l'index de texte intégral est actuellement activé.|  
|**change_tracking_state**|**char(1)**|État du suivi des modifications.<br /><br /> M = manuel<br /><br /> A = automatique<br /><br /> O = désactivé|  
|**change_tracking_state_desc**|**nvarchar(60)**|Description de l'état du suivi des modifications.<br /><br /> MANUAL<br /><br /> AUTO<br /><br /> OFF|  
|**has_crawl_completed**|**bit**|Dernière analyse (remplissage) réalisée par l'index de texte intégral.|  
|**crawl_type**|**char(1)**|Type de l'analyse actuelle ou de la dernière analyse.<br /><br /> F = analyse complète<br /><br /> I = analyse incrémentielle basée sur un horodatage<br /><br /> U = analyse de mise à jour, basée sur des notifications<br /><br /> P = l'analyse complète est suspendue.|  
|**crawl_type_desc**|**nvarchar(60)**|Description de l'analyse actuelle ou de la dernière analyse.<br /><br /> FULL_CRAWL<br /><br /> INCREMENTAL_CRAWL<br /><br /> UPDATE_CRAWL<br /><br /> PAUSED_FULL_CRAWL|  
|**crawl_start_date**|**datetime**|Démarrage de l'analyse actuelle ou de la dernière analyse.<br /><br /> NULL = aucune.|  
|**crawl_end_date**|**datetime**|Fin de l'analyse actuelle ou de la dernière analyse.<br /><br /> NULL = aucune.|  
|**incremental_timestamp**|**Binary(8**|Valeur d'horodatage à utiliser pour l'analyse incrémentielle suivante.<br /><br /> NULL = aucune.|  
|**stoplist_id**|**int**|ID de la [STOPLIST](../../relational-databases/search/configure-and-manage-stopwords-and-stoplists-for-full-text-search.md) qui est associée à cet index de recherche en texte intégral.|  
|**data_space_id**|**int**|Groupe de fichiers où réside cet index de recherche en texte intégral.|  
|**property_list_id**|**int**|ID de la liste de propriétés de recherche associée à cet index de recherche en texte intégral. NULL indique qu'aucune liste de propriétés de recherche n'est associée à l'index de recherche en texte intégral. Pour obtenir plus d’informations sur cette liste de propriétés de recherche, utilisez l’affichage catalogue [sys.registered_search_property_lists &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-registered-search-property-lists-transact-sql.md) .|  
  
## <a name="permissions"></a>Autorisations  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)]  
  
## <a name="examples"></a>Exemples  
 L'exemple ci-après utilise un index de recherche en texte intégral sur la table `HumanResources.JobCandidate` de l'exemple de base de données `AdventureWorks2012`. L'exemple retourne l'ID d'objet de la table, l'ID de la liste de propriétés de recherche et l'ID de la liste de mots vides utilisée par l'index de recherche en texte intégral.  
  
> [!NOTE]  
>  Pour obtenir l’exemple de code qui crée cet index de recherche en texte intégral, consultez la section « Exemples » de [CREATE FULLTEXT index &#40;Transact-SQL&#41;](../../t-sql/statements/create-fulltext-index-transact-sql.md).  
  
```  
USE AdventureWorks2012;  
GO  
SELECT object_id, property_list_id, stoplist_id FROM sys.fulltext_indexes  
    where object_id = object_id('HumanResources.JobCandidate');   
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [sys.fulltext_index_fragments &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-fulltext-index-fragments-transact-sql.md)   
 [sys.fulltext_index_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-fulltext-index-columns-transact-sql.md)   
 [sys.fulltext_index_catalog_usages &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-fulltext-index-catalog-usages-transact-sql.md)   
 [Vues de catalogue d’objets &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [Affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Créer et gérer des index de recherche en texte intégral](../../relational-databases/search/create-and-manage-full-text-indexes.md)   
 [DROP FULLTEXT INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/drop-fulltext-index-transact-sql.md)   
 [CREATE FULLTEXT INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-fulltext-index-transact-sql.md)   
 [ALTER FULLTEXT INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-fulltext-index-transact-sql.md)  
  
  
