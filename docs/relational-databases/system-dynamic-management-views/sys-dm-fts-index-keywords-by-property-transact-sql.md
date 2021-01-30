---
description: sys.dm_fts_index_keywords_by_property (Transact-SQL)
title: sys.dm_fts_index_keywords_by_property (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_fts_index_keywords_by_property
- dm_fts_index_keywords_by_property_TSQL
- sys.dm_fts_index_keywords_by_property
- sys.dm_fts_index_keywords_by_property_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- full-text search [SQL Server], troubleshooting
- search property lists [SQL Server], viewing keywords by property
- full-text search [SQL Server], viewing keywords
- sys.dm_fts_index_keywords_by_property dynamic management view
ms.assetid: fa41e052-a79a-4194-9b1a-2885f7828500
author: pmasl
ms.author: pelopes
ms.openlocfilehash: 9052c7d64ab78c2e6eca1388ed08903365b6e129
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196260"
---
# <a name="sysdm_fts_index_keywords_by_property-transact-sql"></a>sys.dm_fts_index_keywords_by_property (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Retourne tout le contenu en rapport avec les propriétés dans l'index de recherche en texte intégral d'une table donnée. Cela inclut toutes les données qui appartiennent à une propriété inscrite par la liste de propriétés de recherche associée à cet index de recherche en texte intégral.  
  
 sys.dm_fts_index_keywords_by_property est une fonction de gestion dynamique qui vous permet de voir les propriétés inscrites qui ont été émises par les IFilters au moment de l’indexation, ainsi que le contenu exact de chaque propriété dans chaque document indexé.  
  
 **Pour consulter tout le contenu au niveau du document (notamment le contenu en rapport avec les propriétés)**  
  
-   [sys.dm_fts_index_keywords_by_document &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-keywords-by-document-transact-sql.md)  
  
 **Pour afficher des informations sur l'index de recherche en texte intégral de niveau supérieur**  
  
-   [sys.dm_fts_index_keywords &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-keywords-transact-sql.md)  
  
> [!NOTE]  
>  Pour plus d’informations sur les listes de propriétés de recherche, consultez [Rechercher des propriétés de document avec des listes de propriétés de recherche](../../relational-databases/search/search-document-properties-with-search-property-lists.md).  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sys.dm_fts_index_keywords_by_property  
(   
    DB_ID('database_name'),   
OBJECT_ID('table_name')   
)  
```  
  
## <a name="arguments"></a>Arguments  
 db_id («*database_name*»)  
 Appel à la fonction [DB_ID ()](../../t-sql/functions/db-id-transact-sql.md) . Cette fonction accepte un nom de base de données et retourne l’ID de base de données, que sys.dm_fts_index_keywords_by_property utilise pour rechercher la base de données spécifiée. Si *database_name* est omis, la fonction retourne l’ID de la base de données active.  
  
 object_id («*table_name*»)  
 Appel à la fonction [OBJECT_ID ()](../../t-sql/functions/object-id-transact-sql.md) . Cette fonction accepte un nom de table et retourne l'ID de la table contenant l'index de recherche en texte intégral à examiner.  
  
## <a name="table-returned"></a>Table retournée  
  
|Colonne|Type de données|Description|  
|------------|---------------|-----------------|  
|mot clé|**nvarchar(4000)**|Représentation hexadécimale du mot clé stocké dans l'index de recherche en texte intégral.<br /><br /> Remarque : OxFF représente le caractère spécial qui indique la fin d’un fichier ou d’un jeu de données.|  
|display_term|**nvarchar(4000)**|Format explicite du mot clé. Ce format est dérivé du format interne stocké dans l'index de recherche en texte intégral.<br /><br /> Remarque : OxFF représente le caractère spécial qui indique la fin d’un fichier ou d’un jeu de données.|  
|column_id|**int**|ID de la colonne à partir de laquelle le mot clé actuel a été indexé en texte intégral.|  
|document_id|**int**|ID de la ligne ou du document à partir duquel le terme actuel a été indexé en texte intégral. Cet ID correspond à la valeur de clé de texte intégral de cette ligne ou de ce document.|  
|property_id|**int**|ID de propriété interne de la propriété de recherche dans l’index de recherche en texte intégral de la table que vous avez spécifiée dans le paramètre OBJECT_ID ('*table_name*').<br /><br /> Lorsqu'une propriété donnée est ajoutée à une liste de propriétés de recherche, le moteur d'indexation et de recherche en texte intégral inscrit la propriété et lui affecte un ID de propriété interne qui est spécifique à cette liste de propriétés. L'ID de propriété interne, qui est un entier, est unique à une liste de propriétés de recherche donnée. Si une propriété donnée est enregistrée pour plusieurs listes de propriétés de recherche, un ID de propriété interne différent peut être affecté pour chaque liste de propriétés de recherche.<br /><br /> Remarque : l’ID de propriété interne est différent de l’identificateur entier de propriété qui est spécifié lors de l’ajout de la propriété à la liste de propriétés de recherche. Pour plus d’informations, consultez [Rechercher les propriétés du document à l’aide des listes de propriétés de recherche](../../relational-databases/search/search-document-properties-with-search-property-lists.md).<br /><br /> Pour afficher l’association entre property_id et le nom de la propriété :<br />                    [sys.registered_search_properties &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-registered-search-properties-transact-sql.md)|  
  
## <a name="remarks"></a>Notes  
 Cette vue de gestion dynamique peut répondre aux questions telles que celles-ci :  
  
-   Quel contenu est stocké sur une propriété donnée pour un DocID donné ?  
  
-   Quels sont les éléments communs d'une propriété donnée parmi les documents indexés ?  
  
-   Quels documents contiennent réellement une propriété donnée ? C'est utile si l'interrogation sur une propriété de recherche donnée ne retourne pas le document que vous vous attendiez à trouver.  
  
 Lorsque la colonne clé de texte intégral est un type de données Integer, comme cela est recommandé, document_id est directement mappé à la valeur de la clé de texte intégral dans la table de base.  
  
 En revanche, lorsque la colonne clé de texte intégral fait appel à un type de données non entier, document_id ne représente pas la clé de texte intégral dans la table de base. Dans ce cas, pour identifier la ligne de la table de base qui est retournée par dm_fts_index_keywords_by_property, vous devez joindre cette vue avec les résultats retournés par [sp_fulltext_keymappings](../../relational-databases/system-stored-procedures/sp-fulltext-keymappings-transact-sql.md). Avant de pouvoir les joindre, vous devez stocker la sortie de la procédure stockée dans une table temp. Vous pouvez ensuite joindre la colonne document_id de dm_fts_index_keywords_by_property à la colonne ID de retour qui est retournée par cette procédure stockée. Notez qu’une colonne **timestamp** ne peut pas recevoir de valeurs au moment de l’insertion, car elles sont générées automatiquement par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Par conséquent, la colonne **timestamp** doit être convertie en colonnes **varbinary (8)** . L'exemple suivant affiche ces étapes. Dans cet exemple, *table_id* est l’ID de votre table, *database_name* est le nom de votre base de données et *table_name* est le nom de votre table.  
  
```  
USE database_name;  
GO  
CREATE TABLE #MyTempTable   
   (  
      docid INT PRIMARY KEY ,  
      [key] INT NOT NULL  
   );  
DECLARE @db_id int = db_id(N'database_name');  
DECLARE @table_id int = OBJECT_ID(N'table_name');  
INSERT INTO #MyTempTable EXEC sp_fulltext_keymappings @table_id;  
SELECT * FROM sys.dm_fts_index_keywords_by_property   
   ( @db_id, @table_id ) kbd  
   INNER JOIN #MyTempTable tt ON tt.[docid]=kbd.document_id;  
GO  
  
```  
  
## <a name="permissions"></a>Autorisations  
 Requiert l'autorisation SELECT sur les colonnes couvertes par l'index de recherche en texte intégral et les autorisations CREATE FULLTEXT CATALOG.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant retourne des mots clés de la propriété `Author` dans l'index de recherche en texte intégral de la table `Production.Document` de l'exemple de base de données `AdventureWorks`. L’exemple utilise l’alias `KWBPOP` pour la table retournée par **sys.dm_fts_index_keywords_by_property**. L’exemple utilise des jointures internes pour combiner des colonnes à partir de [sys.registered_search_properties](../../relational-databases/system-catalog-views/sys-registered-search-properties-transact-sql.md) et [sys.fulltext_indexes](../../relational-databases/system-catalog-views/sys-fulltext-indexes-transact-sql.md).  
  
```  
-- Once the full-text index is configured to support property searching  
-- on the Author property, return any keywords indexed for this property.  
USE AdventureWorks2012;  
GO   
SELECT KWBPOP.* FROM   
   sys.dm_fts_index_keywords_by_property( DB_ID(),   
   object_id('Production.Document') ) AS KWBPOP  
   INNER JOIN  
      sys.registered_search_properties AS RSP ON(   
         (KWBPOP.property_id = RSP.property_id)   
         AND (RSP.property_name = 'Author') )  
   INNER JOIN  
      sys.fulltext_indexes AS FTI ON(   
         (FTI.[object_id] = object_id('Production.Document'))   
         AND (RSP.property_list_id = FTI.property_list_id) );  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Recherche en texte intégral](../../relational-databases/search/full-text-search.md)   
 [Améliorer les performances des index Full-Text](../../relational-databases/search/improve-the-performance-of-full-text-indexes.md)   
 [sp_fulltext_keymappings &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-fulltext-keymappings-transact-sql.md)   
 [sys.dm_fts_index_keywords_by_document &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-keywords-by-document-transact-sql.md)   
 [sys.dm_fts_index_keywords &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-keywords-transact-sql.md)   
 [sys.registered_search_properties &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-registered-search-properties-transact-sql.md)   
 [sys.registered_search_property_lists &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-registered-search-property-lists-transact-sql.md)   
 [Rechercher les propriétés du document à l’aide des listes des propriétés de recherche](../../relational-databases/search/search-document-properties-with-search-property-lists.md)  
  
  
