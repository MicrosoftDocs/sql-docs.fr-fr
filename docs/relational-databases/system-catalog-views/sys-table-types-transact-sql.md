---
description: sys.table_types (Transact-SQL)
title: sys.table_types (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- table_types_TSQL
- sys.table_types
- sys.table_types_TSQL
- table_types
dev_langs:
- TSQL
helpviewer_keywords:
- table types [SQL Server]
- table-valued parameters, sys.table_types
- sys.table_types
- UDTT
ms.assetid: c05fd873-aff2-4a89-9936-a54c2ea09996
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: c920317b40f0fbcd922c1fe0c440c1a210e0a984
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094441"
---
# <a name="systable_types-transact-sql"></a>sys.table_types (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Affiche les propriétés des types de tables définis par l'utilisateur dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Un type de table est un type à partir duquel il est possible de déclarer des variables de table ou des paramètres table. Chaque type de table a une **type_table_object_id** qui est une clé étrangère dans l’affichage catalogue [sys. Objects](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md) . Vous pouvez utiliser cette colonne ID pour interroger différents affichages catalogue, de manière similaire à une **object_id** colonne d’une table normale, pour découvrir la structure du type de table, par exemple ses colonnes et contraintes.    
 
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|*\<inherited columns>*||Pour obtenir la liste des colonnes héritées par cette vue, consultez [sys. types &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-types-transact-sql.md).|  
|**type_table_object_id**|**int**|Numéro d'identification de l'objet. Ce numéro est unique dans la base de données.|  
|**is_memory_optimized**|**bit**|**S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.<br /><br /> Les valeurs possibles sont les suivantes :<br /><br /> 0 = Non optimisé en mémoire<br /><br /> 1 = Optimisé en mémoire<br /><br /> La valeur 0 est la valeur par défaut.<br /><br /> Les types de tables sont toujours créés avec DURABILITY = SCHEMA_ONLY. Seul le schéma est rendu persistant sur le disque.|  
  
## <a name="permissions"></a>Autorisations  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Vues de catalogue d’objets &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [Utilisez les paramètres de Table-Valued &#40;Moteur de base de données&#41;](../../relational-databases/tables/use-table-valued-parameters-database-engine.md)   
 [OLTP en mémoire &#40;Optimisation en mémoire&#41;](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md)  
  
  
