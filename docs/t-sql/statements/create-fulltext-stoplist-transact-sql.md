---
description: CREATE FULLTEXT STOPLIST (Transact-SQL)
title: CREATE FULLTEXT STOPLIST (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- STOPLIST_TSQL
- FULLTEXT STOPLIST
- STOPLIST
- FULLTEXT_STOPLIST_TSQL
- CREATE FULLTEXT STOPLIST
- CREATE_FULLTEXT_STOPLIST_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- stoplists [full-text search]
- CREATE FULLTEXT STOPLIST statement
- full-text search [SQL Server], stoplists
- full-text search [SQL Server], stopwords
- stopwords [full-text search]
ms.assetid: 0669b1d0-46cc-4fac-8df7-5f7fa7af5db4
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 40668fb2b4deecdbe64df50f68e6d318e639f717
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095829"
---
# <a name="create-fulltext-stoplist-transact-sql"></a>CREATE FULLTEXT STOPLIST (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Crée une nouvelle liste de mots vides de texte intégral dans la base de données actuelle.  
  
 Les mots vides sont gérés dans des bases de données à l’aide d’objets appelés des *liste de mots vides*. Une liste de mots vides est une liste qui, associée à un index de texte intégral, s'applique aux requêtes de texte intégral sur cet index. Pour plus d’informations, consultez [Configurer et gérer les mots vides et listes de mots vides pour la recherche en texte intégral](../../relational-databases/search/configure-and-manage-stopwords-and-stoplists-for-full-text-search.md).  
  
> [!IMPORTANT]  
>  CREATE FULLTEXT STOPLIST, ALTER FULLTEXT STOPLIST, and DROP FULLTEXT STOPLIST sont uniquement pris en charge avec le niveau de compatibilité 100. Elles ne le sont pas avec un niveau de compatibilité égal à 80 ou 90. Toutefois, quel que soit le niveau de compatibilité, la liste de mots vides système est automatiquement associée aux nouveaux index de recherche en texte intégral.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
  
CREATE FULLTEXT STOPLIST stoplist_name  
[ FROM { [ database_name.]source_stoplist_name } | SYSTEM STOPLIST ]  
[ AUTHORIZATION owner_name ]  
;  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *stoplist_name*  
 Nom de la liste de mots vides. *stoplist_name* peut avoir un maximum de 128 caractères. *stoplist_name* doit être unique parmi toutes les listes de mots vides de la base de données active et doit respecter les règles applicables aux identificateurs.  
  
 *stoplist_name* est utilisé une fois que l’index de recherche en texte intégral est créé.  
  
 *database_name*  
 Nom de la base de données dans laquelle se trouve la liste de mots vides spécifiée par *source_stoplist_name*. Si aucun nom n’est spécifié, *database_name* correspond par défaut à la base de données actuelle.  
  
 *source_stoplist_name*  
 Spécifie que la nouvelle liste de mots vides est créée en copiant une liste de mots vides existante. Si *source_stoplist_name* n’existe pas ou si l’utilisateur de la base de données ne dispose pas des autorisations appropriées, CREATE FULLTEXT STOPLIST échoue avec une erreur. Si des langues spécifiées dans les mots vides de la liste de mots vides source ne sont pas inscrits dans la base de données actuelle, l'exécution de CREATE FULLTEXT STOPLIST réussit, mais des avertissements sont retournés et les mots vides correspondants ne sont pas ajoutés.  
  
 SYSTEM STOPLIST  
 Spécifie que la nouvelle liste de mots vides est créée à partir de la liste de mots vides qui existe par défaut dans la [base de données Resource](../../relational-databases/databases/resource-database.md).  
  
 AUTHORIZATION *owner_name*  
 Spécifie le nom d'un principal de base de données comme propriétaire de la liste de mots vides. *owner_name* doit être le nom d’un principal dont l’utilisateur actuel est membre ou l’utilisateur actuel doit avoir l’autorisation IMPERSONATE sur *owner_name*. En l'absence de spécification, la propriété revient à l'utilisateur actuel.  
  
## <a name="remarks"></a>Notes  
 Le créateur d'une liste de mots vides est son propriétaire.  
  
## <a name="permissions"></a>Autorisations  
 La création d'une liste de mots vides requiert les autorisations CREATE FULLTEXT CATALOG. Le propriétaire d'une liste de mots vides peut accorder explicitement l'autorisation CONTROL sur une liste de mots vides pour autoriser les utilisateurs à ajouter et supprimer des mots et à supprimer la liste de mots vides.  
  
> [!NOTE]  
>  L'utilisation d'une liste de mots vides avec un index de texte intégral requiert l'autorisation REFERENCE.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-creating-a-new-full-text-stoplist"></a>R. Création d'une nouvelle liste de mots vides de texte intégral  
 L'exemple ci-dessous crée une nouvelle liste de mots vides de texte intégral nommée `myStoplist`.  
  
```sql  
CREATE FULLTEXT STOPLIST myStoplist;  
GO  
```  
  
### <a name="b-copying-a-full-text-stoplist-from-an-existing-full-text-stoplist"></a>B. Copie d'une liste de mots vides de texte intégral à partir d'une liste de mots vides de texte intégral existante  
 L'exemple ci-dessous crée une nouvelle liste de mots vides de texte intégral nommée `myStoplist2` en copiant une liste de mots vides AdventureWorks existante, nommée `Customers.otherStoplist`.  
  
```sql  
CREATE FULLTEXT STOPLIST myStoplist2 FROM AdventureWorks.otherStoplist;  
GO  
```  
  
### <a name="c-copying-a-full-text-stoplist-from-the-system-full-text-stoplist"></a>C. Copie d'une liste de mots vides de texte intégral à partir de la liste de mots vides de texte intégral système  
 L'exemple ci-dessous crée une nouvelle liste de mots vides de texte intégral nommée `myStoplist3` en copiant à partir de la liste de mots vides système.  
  
```sql  
CREATE FULLTEXT STOPLIST myStoplist3 FROM SYSTEM STOPLIST;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [ALTER FULLTEXT STOPLIST &#40;Transact-SQL&#41;](../../t-sql/statements/alter-fulltext-stoplist-transact-sql.md)   
 [DROP FULLTEXT STOPLIST &#40;Transact-SQL&#41;](../../t-sql/statements/drop-fulltext-stoplist-transact-sql.md)   
 [Configurer et gérer les mots vides et listes de mots vides pour la recherche en texte intégral](../../relational-databases/search/configure-and-manage-stopwords-and-stoplists-for-full-text-search.md)   
 [sys.fulltext_stoplists &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-fulltext-stoplists-transact-sql.md)   
 [sys.fulltext_stopwords &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-fulltext-stopwords-transact-sql.md)   
 [Configurer et gérer les mots vides et listes de mots vides pour la recherche en texte intégral](../../relational-databases/search/configure-and-manage-stopwords-and-stoplists-for-full-text-search.md)  
  
  
