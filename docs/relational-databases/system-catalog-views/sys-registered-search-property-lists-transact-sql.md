---
description: sys.registered_search_property_lists (Transact-SQL)
title: sys.registered_search_property_lists (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- registered_search_property_lists_TSQL
- sys.registered_search_property_lists
- registered_search_property_lists
- sys.registered_search_property_lists_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- full-text search [SQL Server], search property lists
- sys.registered_search_property_lists catalog view
- search property lists [SQL Server], viewing
ms.assetid: 630d4caa-9bea-4cd3-a5b1-01098b0855fc
author: pmasl
ms.author: pelopes
ms.reviewer: mikeray
ms.openlocfilehash: 6504ed677747548d940de7858ceade9a6a0ac75a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206860"
---
# <a name="sysregistered_search_property_lists-transact-sql"></a>sys.registered_search_property_lists (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contient une ligne pour chaque liste de propriétés de recherche sur la base de données actuelle.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**property_list_id**|**int**|ID de la liste de propriétés.|  
|**name**|**sysname**|Nom de la liste de propriétés.|  
|**create_date**|**datetime**|Date de création de la liste de propriétés.|  
|**modify_date**|**datetime**|Date de dernière modification de la liste de propriétés au moyen d'une instruction ALTER.|  
|**principal_id**|**int**|Propriétaire de la liste de propriétés.|  
  
## <a name="remarks"></a>Notes  
 Pour plus d’informations, consultez [Rechercher les propriétés du document à l’aide des listes de propriétés de recherche](../../relational-databases/search/search-document-properties-with-search-property-lists.md).  
  
## <a name="permissions"></a>Autorisations  
 La visibilité des métadonnées dans les listes de propriétés de recherche est limitée à celles dont vous êtes propriétaire ou sur lesquelles une autorisation REFERENCE vous a été accordée.  
  
> [!NOTE]  
>  Le propriétaire d'une liste de propriétés de recherche peut accorder des autorisations REFERENCE ou CONTROL dans la liste. Les utilisateurs disposant de l'autorisation CONTROL peuvent également accorder l'autorisation REFERENCE à d'autres utilisateurs.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant affiche l'ID et le nom des listes de propriétés de recherche dans la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```  
USE AdventureWorks2012;  
GO  
SELECT property_list_id, name FROM sys.registered_search_property_lists;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [ALTER FULLTEXT INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-fulltext-index-transact-sql.md)   
 [sys.fulltext_indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-fulltext-indexes-transact-sql.md)  
  
  
