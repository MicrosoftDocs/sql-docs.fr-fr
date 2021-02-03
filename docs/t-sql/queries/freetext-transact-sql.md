---
description: FREETEXT (Transact-SQL)
title: FREETEXT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/23/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- FREETEXT
- FREETEXT_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- full-text search [SQL Server], meaning matches
- meaning matches [full-text search]
- FREETEXT predicate (Transact-SQL)
- words in predicate [full-text search]
- column searches [full-text search]
ms.assetid: 2f199d3c-440e-4bcf-bdb5-82bb3994005d
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 927f11ff77a50a3a7b7a04983fa197510e88232f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207749"
---
# <a name="freetext-transact-sql"></a>FREETEXT (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Prédicat utilisé dans la [clause WHERE](../../t-sql/queries/where-transact-sql.md) [!INCLUDE[tsql](../../includes/tsql-md.md)] d’une instruction SELECT [!INCLUDE[tsql](../../includes/tsql-md.md)] pour effectuer une recherche en texte intégral [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans des colonnes indexées en texte intégral qui contiennent des types de données caractères. Ce prédicat recherche des valeurs qui correspondent à la signification, et non pas seulement au libellé exact des mots dans la condition de recherche. Quand FREETEXT est utilisé, le moteur de requête de texte intégral effectue les actions suivantes en interne sur la chaîne *freetext_string*, assigne une pondération à chaque terme, puis recherche les correspondances :  
  
-   Il scinde la chaîne en mots en se basant sur les limites de mot (coupure de mots).  
  
-   Il génère des formes fléchies des mots (extraction de la racine).  
  
-   Il identifie une liste d'extensions ou de substituts en fonction des correspondances fournies par le dictionnaire des synonymes.  
  
> [!NOTE]  
>  Pour plus d’informations sur les sortes de recherches en texte intégral prises en charge par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consultez [Exécuter une requête avec une recherche en texte intégral](../../relational-databases/search/query-with-full-text-search.md).  
  
**S’applique à**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (de [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] à la [version actuelle](/troubleshoot/sql/general/determine-version-edition-update-level)).
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
FREETEXT ( { column_name | (column_list) | * }   
          , 'freetext_string' [ , LANGUAGE language_term ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *column_name*  
 Nom d'une ou de plusieurs colonnes de texte intégral indexées de la table spécifiée dans la clause FROM. Les colonnes peuvent être de type **char**, **varchar**, **nchar**, **nvarchar**, **text**, **ntext**, **image**, **xml**, **varbinary** ou **varbinary(max)** .  
  
 *column_list*  
 Indique que plusieurs colonnes, délimitées par des virgules, peuvent être spécifiées. *column_list* doit être mis entre parenthèses. Une seule et même langue doit être utilisée dans toutes les colonnes de *column_list*, sauf si *language_term* est spécifié.  
  
 \*  
 Spécifie que la chaîne *freetext_string* à trouver doit être recherchée dans toutes les colonnes répertoriées pour la recherche en texte intégral. Si la clause FROM contient plusieurs tables, \* doit être qualifié par le nom de la table. Une seule et même langue doit être utilisée dans toutes les colonnes de la table, sauf si *language_term* est spécifié.  
  
 *freetext_string*  
 Texte à rechercher dans *column_name*. Tout texte, y compris des mots, expressions et phrases peuvent être entrés. Des correspondances sont générées si l'un des termes ou les formes de quelque terme que ce soit sont trouvés dans l'index de texte intégral.  
  
 À la différence des conditions de recherche CONTAINS et CONTAINSTABLE où AND est un mot clé, le mot « and » utilisé dans *freetext_string* est considéré comme un mot parasite (ou [mot vide](../../relational-databases/search/configure-and-manage-stopwords-and-stoplists-for-full-text-search.md)) et est ignoré.  
  
 L'utilisation de WEIGHT, de FORMSOF, de caractères génériques, de NEAR et d'autres syntaxes n'est pas autorisée. La chaîne *freetext_string* est découpée en mots en vue de la recherche des radicaux et de l’analyse dans le dictionnaire des synonymes.  
  
 *freetext_string* est une chaîne de type **nvarchar**. Une conversion implicite se produit lorsqu'un autre type de données character est utilisé comme entrée. Les chaînes longues de types varchar(max) et nvarchar(max) ne peuvent pas être utilisées. Dans l'exemple suivant, la variable `@SearchWord`, à laquelle est attribuée la valeur `varchar(30)`, provoque une conversion implicite dans le prédicat `FREETEXT`.  
  
```sql
USE AdventureWorks2012;  
GO  
DECLARE @SearchWord VARCHAR(30)  
SET @SearchWord ='performance'  
SELECT Description   
FROM Production.ProductDescription   
WHERE FREETEXT(Description, @SearchWord);  
  
```  
  
 Étant donné que la détection des paramètres ne fonctionne pas lors de la conversion, utilisez **nvarchar** pour obtenir de meilleures performances. Dans l’exemple, déclarez `@SearchWord` en tant que `nvarchar(30)`.  
  
```sql  
USE AdventureWorks2012;  
GO  
DECLARE @SearchWord NVARCHAR(30)  
SET @SearchWord = N'performance'  
SELECT Description   
FROM Production.ProductDescription   
WHERE FREETEXT(Description, @SearchWord);  
  
```  
  
 Vous pouvez également utiliser l’indicateur de requête OPTIMIZE FOR quand un plan non optimal est généré.  
  
 LANGUAGE *language_term*  
 Langue dont les ressources seront utilisées pour l'analyse lexicale, la recherche de radical, l'utilisation du dictionnaire de synonymes et la suppression de mots vides dans la requête. Ce paramètre est facultatif et peut être spécifié sous la forme d'une chaîne, d'un entier ou d'une valeur hexadécimale correspondant à l'identificateur de paramètres régionaux (LCID) d'une langue. Si une langue est définie avec *language_term*, elle est appliquée à tous les éléments de la condition de recherche. Si aucune valeur n'est définie, la langue du texte intégral de la colonne est utilisée.  
  
 Si des documents de langues différentes sont stockés ensemble en tant qu'objets blob dans une colonne unique, l'identificateur de paramètres régionaux (LCID) d'un document donné détermine la langue utilisée pour l'indexation de son contenu. Quand une requête est effectuée sur la colonne, la spécification de LANGUAGE *language_term* augmente la probabilité d’une meilleure correspondance.  
  
 Quand il est spécifié en tant que chaîne, *language_term* correspond à la valeur de colonne **alias** dans l’affichage de compatibilité [sys.syslanguages &#40;Transact-SQL&#41;](../../relational-databases/system-compatibility-views/sys-syslanguages-transact-sql.md).  La chaîne doit être placée entre guillemets simples, comme dans '*language_term*'. Quand il est spécifié sous la forme d’un entier, *language_term* est le LCID qui identifie la langue. Quand il est spécifié sous la forme d’une valeur hexadécimale, *language_term* est 0x suivi de la valeur hexadécimale du LCID. La valeur hexadécimale ne doit pas dépasser huit caractères, y compris les zéros non significatifs.  
  
 Si la valeur est au format DBCS (jeu de caractères codés sur deux octets), [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] la convertit en Unicode.  
  
 Si la langue spécifiée n’est pas valide ou si aucune ressource correspondant à cette langue n’est installée, [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] retourne une erreur. Pour utiliser des ressources linguistiques neutres, indiquez 0x0 pour *language_term*.  
  
## <a name="general-remarks"></a>Remarques d'ordre général  
 Les prédicats et les fonctions de texte intégral s'appliquent à une table unique, ce qui est implicite dans le prédicat FROM. Pour effectuer des recherches sur plusieurs tables, utilisez une table jointe dans votre clause FROM afin de baser votre recherche sur un jeu de résultats qui est le produit de deux tables ou plus.  
  
Les requêtes de texte intégral qui utilisent FREETEXT sont moins précises que celles qui utilisent CONTAINS. Le moteur de recherche en texte intégral de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] identifie les expressions et les mots importants. Aucune signification particulière n’est donnée aux mots clés réservés ni aux caractères génériques qui ont généralement un sens quand ils sont précisés dans le paramètre \<contains_search_condition> du prédicat CONTAINS.
  
 Les prédicats de texte intégral ne sont pas autorisés dans la [clause OUTPUT](../../t-sql/queries/output-clause-transact-sql.md) quand le niveau de compatibilité de la base de données a la valeur 100.  
  
> [!NOTE]  
>  La fonction FREETEXTTABLE est utile pour les mêmes types de correspondance que le prédicat FREETEXT. Vous pouvez référencer cette fonction comme un nom de table standard dans la [clause FROM](../../t-sql/queries/from-transact-sql.md) d’une instruction SELECT. Pour plus d’informations, consultez [FREETEXTTABLE &#40;Transact-SQL&#41;](../../relational-databases/system-functions/freetexttable-transact-sql.md).  
  
## <a name="querying-remote-servers"></a>Interrogation de serveurs distants  
 Vous pouvez utiliser un nom en quatre parties dans le prédicat [CONTAINS](../../t-sql/queries/contains-transact-sql.md) ou FREETEXT pour faire des requêtes sur les colonnes indexées en texte intégral dans les tables cibles d’un serveur lié. Pour préparer un serveur distant à recevoir des requêtes de texte intégral, créez un index de recherche en texte intégral sur les tables et colonnes cibles du serveur distant, puis ajoutez le serveur distant comme serveur lié.  
  
## <a name="comparison-of-like-to-full-text-search"></a>Comparaison entre LIKE et la recherche en texte intégral  
 Contrairement à la recherche en texte intégral, le prédicat [LIKE](../../t-sql/language-elements/like-transact-sql.md)[!INCLUDE[tsql](../../includes/tsql-md.md)] fonctionne uniquement sur les modèles de caractères. En outre, vous ne pouvez pas utiliser le prédicat LIKE pour interroger des données binaires mises en forme. De plus, une requête LIKE portant sur un important volume de données de texte non structurées est beaucoup plus lente qu'une requête de texte intégral équivalente exécutée sur les mêmes données. Une requête LIKE portant sur des millions de lignes de données de texte peut prendre plusieurs minutes pour retourner un résultat alors qu'une requête de texte intégral retourne en quelques secondes à peine le même résultat, en fonction du nombre de lignes retournées.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-using-freetext-to-search-for-words-containing-specified-character-values"></a>R. Utilisation de FREETEXT pour rechercher des mots contenant les valeurs de caractère spécifiées  
 L'exemple suivant recherche tous les documents contenant les mots liés à « vital », « safety » et « components ».  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT Title  
FROM Production.Document  
WHERE FREETEXT (Document, 'vital safety components' );  
GO  
```  
  
### <a name="b-using-freetext-with-variables"></a>B. Utilisation de FREETEXT avec des variables  
 L'exemple ci-dessous utilise une variable à la place d'un terme de recherche spécifique.  
  
```sql  
USE AdventureWorks2012;  
GO  
DECLARE @SearchWord NVARCHAR(30);  
SET @SearchWord = N'high-performance';  
SELECT Description   
FROM Production.ProductDescription   
WHERE FREETEXT(Description, @SearchWord);  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Commencer à utiliser la recherche en texte intégral](../../relational-databases/search/get-started-with-full-text-search.md)   
 [Créer et gérer des catalogues de texte intégral](../../relational-databases/search/create-and-manage-full-text-catalogs.md)   
 [CREATE FULLTEXT CATALOG &#40;Transact-SQL&#41;](../../t-sql/statements/create-fulltext-catalog-transact-sql.md)   
 [CREATE FULLTEXT INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-fulltext-index-transact-sql.md)   
 [Créer et gérer des index de recherche en texte intégral](../../relational-databases/search/create-and-manage-full-text-indexes.md)   
 [Exécuter une requête avec une recherche en texte intégral](../../relational-databases/search/query-with-full-text-search.md)   
 [Créer des requêtes de recherche en texte intégral &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/create-full-text-search-queries-visual-database-tools.md)   
 [CONTAINS &#40;Transact-SQL&#41;](../../t-sql/queries/contains-transact-sql.md)   
 [CONTAINSTABLE &#40;Transact-SQL&#41;](../../relational-databases/system-functions/containstable-transact-sql.md)   
 [Types de données &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)   
 [FREETEXTTABLE &#40;Transact-SQL&#41;](../../relational-databases/system-functions/freetexttable-transact-sql.md)   
 [WHERE &#40;Transact-SQL&#41;](../../t-sql/queries/where-transact-sql.md)  
  
