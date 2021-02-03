---
description: CREATE SEARCH PROPERTY LIST (Transact-SQL)
title: CREATE SEARCH PROPERTY LIST (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 04/10/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CREATE_SEARCH_PROPERTY_LIST_TSQL
- CREATE SEARCH
- CREATE_SEARCH_TSQL
- CREATE_SEARCH_PROPERTY_TSQL
- CREATE SEARCH PROPERTY
- CREATE SEARCH PROPERTY LIST
- sql13.swb.spl.newsearchpropertylist.f1
dev_langs:
- TSQL
helpviewer_keywords:
- full-text search [SQL Server], search property lists
- search property lists [SQL Server], creating
- CREATE SEARCH PROPERTY LIST statement
ms.assetid: 5440cbb8-3403-4d27-a2f9-8e1f5a1bc12b
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 31e938250b543fbb6bb3e3443138677f8ee3befd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192696"
---
# <a name="create-search-property-list-transact-sql"></a>CREATE SEARCH PROPERTY LIST (Transact-SQL)
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

  Crée une liste de propriétés de recherche. Une liste de propriétés de recherche permet de spécifier une ou plusieurs propriétés de recherche que vous souhaitez inclure dans un index de recherche en texte intégral.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql  
CREATE SEARCH PROPERTY LIST new_list_name  
   [ FROM [ database_name. ] source_list_name ]  
   [ AUTHORIZATION owner_name ]  
;  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *new_list_name*  
 Nom de la nouvelle liste des propriétés de recherche. *new_list_name* est un identificateur avec un maximum de 128 caractères. *new_list_name* doit être unique parmi toutes les listes de propriétés dans la base de données active et respecter les règles applicables aux identificateurs. *new_list_name* est utilisé une fois que l’index de recherche en texte intégral est créé.  
  
 *database_name*  
 Nom de la base de données dans laquelle se trouve la liste de propriétés spécifiée par *source_list_name*. Si aucun nom n’est spécifié, *database_name* correspond par défaut à la base de données actuelle.  
  
 *database_name* doit spécifier le nom d’une base de données existante. Le compte de la connexion actuelle doit être associé à un ID d’utilisateur existant de la base de données spécifiée par *database_name*. Vous devez également disposer des [autorisations](#Permissions) exigées sur la base de données.  
  
 *source_list_name*  
 Spécifie que la nouvelle liste de propriétés est créée en copiant une liste de propriétés existante à partir de *database_name*. Si *source_list_name* n’existe pas, CREATE SEARCH PROPERTY LIST échoue avec une erreur. Les propriétés de recherche dans *source_list_name* sont héritées par *new_list_name*.  
  
 AUTHORIZATION *owner_name*  
 Spécifie le nom d'un utilisateur ou d'un rôle pour détenir la liste de propriétés. *owner_name* doit être le nom d’un rôle dont l’utilisateur actuel est membre, ou l’utilisateur actuel doit disposer de l’autorisation IMPERSONATE sur *owner_name*. En l'absence de spécification, la propriété revient à l'utilisateur actuel.  
  
> [!NOTE]  
>  Il est possible de changer le propriétaire à l’aide de l’instruction [ALTER AUTHORIZATION](../../t-sql/statements/alter-authorization-transact-sql.md)[!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
## <a name="remarks"></a>Notes  
  
> [!NOTE]  
>  Pour obtenir des informations générales sur les listes de propriétés, consultez [Rechercher les propriétés du document à l’aide des listes de propriétés de recherche](../../relational-databases/search/search-document-properties-with-search-property-lists.md).  
  
 Par défaut, une nouvelle liste des propriétés de recherche est vide et vous devez la modifier pour ajouter une ou plusieurs propriétés de recherche manuellement. Vous pouvez également copier une liste de propriétés de recherche existante. Dans ce cas, la nouvelle liste hérite des propriétés de recherche de sa source, mais vous pouvez modifier la nouvelle liste pour ajouter ou supprimer des propriétés de recherche. Toutes les propriétés figurant dans la liste des propriétés de recherche au moment du remplissage complet suivant sont incluses dans l'index de recherche en texte intégral.  
  
 Une instruction CREATE SEARCH PROPERTY LIST échoue si l'une des conditions suivantes se présente :  
  
-   Si la base de données spécifiée par *database_name* n’existe pas.  
  
-   Si la liste spécifiée par *source_list_name* n’existe pas.  
  
-   Si vous ne possédez pas les autorisations appropriées.  
  
 **Pour ajouter ou supprimer des propriétés dans une liste**  
  
-   [ALTER SEARCH PROPERTY LIST &#40;Transact-SQL&#41;](../../t-sql/statements/alter-search-property-list-transact-sql.md)  
  
-   **Pour supprimer une liste de propriétés**  
  
-   [DROP SEARCH PROPERTY LIST &#40;Transact-SQL&#41;](../../t-sql/statements/drop-search-property-list-transact-sql.md)  
  
##  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 Requiert des autorisations CREATE FULLTEXT CATALOG dans la base de données actuelle et des autorisations REFERENCES sur toute base de données depuis laquelle vous copiez une liste de propriétés source.  
  
> [!NOTE]  
>  L'autorisation REFERENCES est obligatoire pour associer la liste à un index de recherche en texte intégral. L'autorisation CONTROL est obligatoire pour ajouter et supprimer des propriétés ou supprimer la liste. Le propriétaire d'une liste de propriétés peut accorder des autorisations REFERENCES ou CONTROL sur la liste. Les utilisateurs disposant de l'autorisation CONTROL peuvent également accorder l'autorisation REFERENCES à d'autres utilisateurs.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-creating-an-empty-property-list-and-associating-it-with-an-index"></a>R. Création d'une liste de propriétés vide et association à un index  
 L'exemple suivant crée une nouvelle liste de propriétés de recherche nommée `DocumentPropertyList`. L’exemple utilise ensuite une instruction [ALTER FULLTEXT INDEX](../../t-sql/statements/alter-fulltext-index-transact-sql.md) pour associer la nouvelle liste de propriétés à l’index de recherche en texte intégral de la table `Production.Document` dans la base de données `AdventureWorks`, sans démarrer un remplissage.  
  
> [!NOTE]  
>  Pour obtenir un exemple qui ajoute plusieurs propriétés de recherche prédéfinies bien connues à cette liste de propriétés de recherche, consultez [ALTER SEARCH PROPERTY LIST &#40;Transact-SQL&#41;](../../t-sql/statements/alter-search-property-list-transact-sql.md). Après avoir ajouté des propriétés de recherche à la liste, l'administrateur de base de données devra utiliser une autre instruction ALTER FULLTEXT INDEX avec la clause START FULL POPULATION.  
  
```sql 
CREATE SEARCH PROPERTY LIST DocumentPropertyList;  
GO  
USE AdventureWorks2012;  
ALTER FULLTEXT INDEX ON Production.Document   
   SET SEARCH PROPERTY LIST DocumentPropertyList  
   WITH NO POPULATION;   
GO   
```  
  
### <a name="b-creating-a-property-list-from-an-existing-one"></a>B. Création d'une liste de propriétés à partir d'une liste existante  
 L'exemple suivant crée une nouvelle liste de propriétés de recherche, `JobCandidateProperties`, à partir de la liste créée par l'exemple A, `DocumentPropertyList`, associée à un index de recherche en texte intégral dans la base de données `AdventureWorks2012`. L'exemple utilise ensuite une instruction ALTER FULLTEXT INDEX pour associer la nouvelle liste de propriétés à l'index de recherche en texte intégral de la table `HumanResources.JobCandidate` dans la base de données `AdventureWorks2012`. Cette instruction ALTER FULLTEXT INDEX entraîne un remplissage complet, ce qui correspond au comportement par défaut de la clause SET SEARCH PROPERTY LIST.  
  
```sql  
CREATE SEARCH PROPERTY LIST JobCandidateProperties 
FROM AdventureWorks2012.DocumentPropertyList;  
GO  
ALTER FULLTEXT INDEX ON HumanResources.JobCandidate   
   SET SEARCH PROPERTY LIST JobCandidateProperties;  
GO
```  
  
## <a name="see-also"></a>Voir aussi  
 [ALTER SEARCH PROPERTY LIST &#40;Transact-SQL&#41;](../../t-sql/statements/alter-search-property-list-transact-sql.md)   
 [DROP SEARCH PROPERTY LIST &#40;Transact-SQL&#41;](../../t-sql/statements/drop-search-property-list-transact-sql.md)   
 [sys.registered_search_properties &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-registered-search-properties-transact-sql.md)   
 [sys.registered_search_property_lists &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-registered-search-property-lists-transact-sql.md)   
 [sys.dm_fts_index_keywords_by_property &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-keywords-by-property-transact-sql.md)   
 [Rechercher les propriétés du document à l’aide des listes de propriétés de recherche](../../relational-databases/search/search-document-properties-with-search-property-lists.md)   
 [Recherche des GUID du jeu de propriétés et des ID d'entier de propriétés pour les propriétés de recherche](../../relational-databases/search/find-property-set-guids-and-property-integer-ids-for-search-properties.md)  
  
  
