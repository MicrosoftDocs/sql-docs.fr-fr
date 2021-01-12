---
description: sys.column_store_dictionaries (Transact-SQL)
title: sys.column_store_dictionaries (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.column_store_dictionaries_TSQL
- column_store_dictionaries
- column_store_dictionaries_TSQL
- sys.column_store_dictionaries
dev_langs:
- TSQL
helpviewer_keywords:
- sys.column_store_dictionaries catalog view
ms.assetid: 56efd563-2f72-4caf-94e3-8a182385c173
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 86c749ab8fdad447c477dd4f424cab3ab1d6b086
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092501"
---
# <a name="syscolumn_store_dictionaries-transact-sql"></a>sys.column_store_dictionaries (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contient une ligne pour chaque dictionnaire utilisé dans des index columnstore optimisés en mémoire xVelocity. Les dictionnaires sont utilisés pour encoder certains, mais pas tous les types de données, par conséquent, certaines colonnes d'un index columnstore n'ont pas de dictionnaires. Un dictionnaire peut exister en tant que dictionnaire principal (pour tous les segments) et éventuellement pour d'autres dictionnaires secondaires utilisés pour un sous-ensemble des segments de la colonne.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**hobt_id**|**bigint**|ID de l’index de segment de mémoire ou arbre B (B-tree) (HoBT) pour la table qui contient cet index ColumnStore.|  
|**column_id**|**int**|ID de la colonne ColumnStore à partir de 1. La première colonne a l’ID = 1, la seconde la colonne ID = 2, etc.|  
|**dictionary_id**|**int**|Il peut y avoir deux types de dictionnaires, globaux et locaux, associés à un segment de colonne. Une dictionary_id de 0 représente le dictionnaire global qui est partagé par tous les segments de colonne (un pour chaque groupe de lignes) de cette colonne.|  
|**version**|**int**|Version du format de dictionnaire.|  
|**type**|**int**|Type de dictionnaire :<br /><br /> 1-dictionnaire de hachage contenant des valeurs **int**<br /><br /> 2-non utilisé<br /><br /> 3-dictionnaire de hachage contenant des valeurs de chaîne<br /><br /> 4-dictionnaire de hachage contenant des valeurs **float**<br /><br /> Pour plus d’informations sur les dictionnaires, consultez le [Guide des index ColumnStore](~/relational-databases/indexes/columnstore-indexes-overview.md).|  
|**last_id**|**int**|Dernier ID de données dans le dictionnaire.|  
|**entry_count**|**bigint**|Nombre d'entrées dans le dictionnaire.|  
|**on_disk_size**|**bigint**|Taille du dictionnaire en octets.|  
|**partition_id**|**bigint**|Indique l'ID de partition. Unique dans une base de données.|  
  
## <a name="permissions"></a>Autorisations  
Nécessite l'autorisation `VIEW DEFINITION` sur la table. Les colonnes suivantes retournent la valeur null, sauf si l’utilisateur a également l' `SELECT` autorisation : Last_id, entry_count data_ptr.  
  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Vues de catalogue d’objets &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [Affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Interrogation du SQL Server FAQ du catalogue système](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.md)   
 [sys.columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-columns-transact-sql.md)   
 [sys.all_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-all-columns-transact-sql.md)   
 [sys.computed_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-computed-columns-transact-sql.md)   
 [Guide des index columnstore](~/relational-databases/indexes/columnstore-indexes-overview.md)   
 [Guide des index columnstore](~/relational-databases/indexes/columnstore-indexes-overview.md)   
 [sys.column_store_segments &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-column-store-segments-transact-sql.md)  
  
  

