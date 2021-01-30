---
description: sys.dm_fts_index_keywords_position_by_document (Transact-SQL)
title: sys.dm_fts_index_keywords_position_by_document (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_fts_index_keywords_position_by_document_TSQL
- dm_fts_index_keywords_position_by_document_TSQL
- dm_fts_index_keywords_position_by_document
- sys.dm_fts_index_keywords_position_by_document
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_fts_index_keywords_position_by_document dynamic management view
ms.assetid: 0d70184f-baa2-411b-a32d-a4c5af890edd
author: pmasl
ms.author: pelopes
ms.openlocfilehash: 099574becb70e3812864c4acc7dc0a395f3169b9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211227"
---
# <a name="sysdm_fts_index_keywords_position_by_document-transact-sql"></a>sys.dm_fts_index_keywords_position_by_document (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Retourne des informations de position de mot clé dans les documents indexés.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
sys.dm_fts_index_keywords_position_by_document  
(   
    DB_ID('database_name'),   
OBJECT_ID('table_name')   
)  
```  
  
## <a name="arguments"></a>Arguments  
 db_id («*database_name*»)  
 Appel à la fonction [DB_ID ()](../../t-sql/functions/db-id-transact-sql.md) . Cette fonction accepte un nom de base de données et retourne l’ID de base de données, que sys.dm_fts_index_keywords_position_by_document utilise pour rechercher la base de données spécifiée.  
  
 object_id («*table_name*»)  
 Appel à la fonction [OBJECT_ID ()](../../t-sql/functions/object-id-transact-sql.md) . Cette fonction accepte un nom de table et retourne l'ID de la table contenant l'index de recherche en texte intégral à examiner.  
  
## <a name="table-returned"></a>Table retournée  
  
|Colonne|Type de données|Description|  
|------------|---------------|-----------------|  
|mot clé|**varbinary(128)**|Chaîne binaire représentant le mot clé.|  
|display_term|**nvarchar(4000)**|Format explicite du mot clé. Ce format est dérivé du format interne stocké dans l'index de recherche en texte intégral.|  
|column_id|**int**|ID de la colonne à partir de laquelle le mot clé actuel a été indexé en texte intégral.|  
|document_id|**bigint**|ID de la ligne ou du document à partir duquel le terme actuel a été indexé en texte intégral. Cet ID correspond à la valeur de clé de texte intégral de cette ligne ou de ce document.|  
|position|**int**|Position du mot clé dans le document.|  
  
## <a name="remarks"></a>Notes  
 Utilisez la DMV pour identifier l’emplacement des mots indexés dans les documents indexés. Cette vue de gestion dynamique peut être utilisée pour résoudre les problèmes lorsque **sys.dm_fts_index_keywords_by_document** indique que les mots se trouvent dans l’index de recherche en texte intégral, mais quand vous exécutez une requête à l’aide de ces mots, le document n’est pas renvoyé.  
  
## <a name="permissions"></a>Autorisations  
 Requiert l'autorisation SELECT sur les colonnes couvertes par l'index de recherche en texte intégral et les autorisations CREATE FULLTEXT CATALOG.  
  
## <a name="examples"></a>Exemples  
 L’exemple suivant retourne des mots clés à partir de l’index de recherche en texte intégral de la `Production.Document` table de l' `AdventureWorks` exemple de base de données.  
  
```  
USE AdventureWorks2012;  
GO   
  
SELECT * FROM sys.dm_fts_index_keywords_position_by_document  
(   
    DB_ID('AdventureWorks2012'),  
    OBJECT_ID('AdventureWorks2012.Production.Document')   
);   
GO  
```  
  
 Vous pouvez ajouter un prédicat sur l’autre columns_id comme dans l’exemple de requête suivant, afin d’isoler davantage les emplacements.  
  
```  
SELECT * FROM sys.dm_fts_index_keywords_position_by_document  
(   
    DB_ID('AdventureWorks2012'),  
    OBJECT_ID('AdventureWorks2012.Production.Document')   
)  
WHERE document_id = 7 AND display_term = 'performance';  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Recherche en texte intégral](../../relational-databases/search/full-text-search.md)   
 [Améliorer les performances des index Full-Text](../../relational-databases/search/improve-the-performance-of-full-text-indexes.md)   
 [Fonctions de recherche en texte intégral et de recherche sémantique &#40;Transact-SQL&#41;](../../relational-databases/system-functions/full-text-search-and-semantic-search-functions-transact-sql.md)   
 [Fonctions et vues de gestion dynamique de la recherche en texte intégral et de la recherche sémantique &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/full-text-and-semantic-search-dynamic-management-views-functions.md)   
 [Procédures stockées de recherche en texte intégral et de recherche sémantique &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/full-text-search-and-semantic-search-stored-procedures-transact-sql.md)   
 [Rechercher les propriétés du document à l’aide des listes de propriétés de recherche](../../relational-databases/search/search-document-properties-with-search-property-lists.md)   
 [sys.dm_fts_index_keywords_by_document &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-keywords-by-document-transact-sql.md)  
  
  
