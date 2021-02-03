---
description: SELECT - Clause ORDER BY (Transact-SQL)
title: ORDER BY, clause (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 12/24/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ORDER_TSQL
- BY
- ORDER_BY_TSQL
- BY_TSQL
- ORDER
- ORDER BY
dev_langs:
- TSQL
helpviewer_keywords:
- ad-hoc query paging
- OFFSET clause
- SELECT statement [SQL Server], FETCH clause
- clauses [SQL Server], ORDER BY
- SELECT statement [SQL Server], limiting the rows returned
- data [SQL Server], limiting the rows returned
- data [SQL Server], ad-hoc query paging
- sort orders [SQL Server]
- SELECT statement [SQL Server], OFFSET clause
- ORDER BY clause [Transact-SQL]
- LIMIT
- limiting result sets
- SELECT statement [SQL Server], ORDER BY clause
- query paging
- data [SQL Server], sorting
- limiting data returned in a query
- sort orders [SQL Server], ORDER BY clause
- FETCH clause
ms.assetid: bb394abe-cae6-4905-b5c6-8daaded77742
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 2aadc41cd8b4cf40a6e00ef9e33fd18248780df4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205426"
---
# <a name="select---order-by-clause-transact-sql"></a>SELECT - Clause ORDER BY (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Trie les données retournées par une requête dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Utilisez cette clause pour effectuer les opérations suivantes :  
  
-   Trier le jeu de résultats d'une requête par la liste de colonnes spécifiée et, éventuellement, limiter les lignes retournées à une plage spécifiée. L'ordre dans lequel les lignes sont retournées dans un jeu de résultats n'est pas garanti sauf si une clause ORDER BY a été spécifiée.  
  
-   Déterminer l’ordre dans lequel les valeurs de [fonction de classement](../../t-sql/functions/ranking-functions-transact-sql.md) sont appliquées au jeu de résultats.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
> [!NOTE]  
>  ORDER BY n’est pas pris en charge dans les instructions SELECT/INTO ou CREATE TABLE AS SELECT (CTAS) dans [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] ou [!INCLUDE[ssPDW](../../includes/sspdw-md.md)].

## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
ORDER BY order_by_expression  
    [ COLLATE collation_name ]   
    [ ASC | DESC ]   
    [ ,...n ]   
[ <offset_fetch> ]  
  
<offset_fetch> ::=  
{   
    OFFSET { integer_constant | offset_row_count_expression } { ROW | ROWS }  
    [  
      FETCH { FIRST | NEXT } {integer_constant | fetch_row_count_expression } { ROW | ROWS } ONLY  
    ]  
}  
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
[ ORDER BY   
    {  
    order_by_expression   
    [ ASC | DESC ]   
    } [ ,...n ]   
]   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *order_by_expression*  
 Spécifie une colonne ou une expression sur laquelle trier le jeu de résultats de la requête. Une colonne de tri peut être définie comme un nom ou alias de colonne ou comme un nombre entier non négatif représentant la position de la colonne dans la liste de sélection.  
  
 Il est possible de définir plusieurs colonnes de tri. Les noms de colonne doivent être uniques. La séquence des colonnes de tri de la clause ORDER BY définit la structure du jeu de résultats trié. Autrement dit, le jeu de résultats est trié par la première colonne puis, cette liste triée est triée par la deuxième colonne, et ainsi de suite.  
  
 Les noms de colonnes référencés dans la clause ORDER BY doivent correspondre à une colonne ou à un alias de colonne de la liste de sélection, ou à une colonne définie dans une table spécifiée dans la clause FROM, sans ambiguïté. Si la clause ORDER BY référence un alias de colonne de la liste de sélection, l’alias de colonne doit être utilisé de manière autonome, et non dans le cadre d’une expression dans la clause ORDER BY, par exemple :
 
```sql
SELECT SCHEMA_NAME(schema_id) AS SchemaName FROM sys.objects 
ORDER BY SchemaName; -- correct 
SELECT SCHEMA_NAME(schema_id) AS SchemaName FROM sys.objects 
ORDER BY SchemaName + ''; -- wrong
```
  
 COLLATE *collation_name*  
 Spécifie que l’opération ORDER BY doit être exécutée conformément au classement spécifié dans *collation_name*, et pas selon le classement de la colonne défini dans la table ou l’affichage. *collation_name* peut être un nom de classement Windows ou SQL. Pour plus d’informations, consultez [Prise en charge d’Unicode et du classement](../../relational-databases/collations/collation-and-unicode-support.md). COLLATE est applicable uniquement aux colonnes de types **char**, **varchar**, **nchar** et **nvarchar**.  
  
 **ASC** | DESC  
 Spécifie que les valeurs dans la colonne spécifiée doivent être triées par ordre croissant ou décroissant. ASC effectue le tri de la valeur la plus faible à la valeur la plus élevée. DESC effectue le tri de la valeur la plus élevée à la valeur la plus faible. ASC correspond à l'ordre de tri par défaut. Les valeurs NULL sont traitées comme les plus petites valeurs possibles.  
  
 OFFSET { *integer_constant* | *offset_row_count_expression* } { ROW | ROWS }  
 Spécifie le nombre de lignes à ignorer avant de retourner des lignes à partir de l'expression de requête. La valeur peut être une constante entière ou une expression supérieure ou égale à zéro.  
  
**S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et versions ultérieures et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].  
  
 *offset_row_count_expression* peut être une variable, un paramètre ou une sous-requête scalaire constante. Lorsqu'une sous-requête est utilisée, elle ne peut pas référencer de colonnes définies dans l'étendue de requête externe. Autrement dit, elle ne peut pas être mise en corrélation avec la requête externe.  
  
 ROW et ROWS sont synonymes et sont fournis pour la compatibilité ANSI.  
  
 Dans les plans d’exécution de requêtes, la valeur du nombre de lignes du décalage est affichée dans l’attribut **Offset** de l’opérateur de requête TOP.  
  
 FETCH { FIRST \| NEXT } { *integer_constant* \| *fetch_row_count_expression* } { ROW \| ROWS } ONLY  
 Spécifie le nombre de lignes à retourner une fois la clause OFFSET traitée. La valeur peut être une constante entière ou une expression supérieure ou égale à un.  
  
**S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et versions ultérieures et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].  
  
 *fetch_row_count_expression* peut être une variable, un paramètre ou une sous-requête scalaire constante. Lorsqu'une sous-requête est utilisée, elle ne peut pas référencer de colonnes définies dans l'étendue de requête externe. Autrement dit, elle ne peut pas être mise en corrélation avec la requête externe.  
  
 FIRST et NEXT sont synonymes et sont fournis pour la compatibilité ANSI.  
  
 ROW et ROWS sont synonymes et sont fournis pour la compatibilité ANSI.  
  
 Dans les plans d’exécution de requêtes, la valeur du nombre de lignes du décalage est affichée dans l’attribut **Rows** ou **Top** de l’opérateur de requête TOP.  
  
## <a name="best-practices"></a>Bonnes pratiques  
 Évitez de spécifier des entiers dans la clause ORDER BY comme représentations positionnelles des colonnes dans la liste de sélection. Par exemple, bien qu'une instruction telle que `SELECT ProductID, Name FROM Production.Production ORDER BY 2` soit valide, l'instruction n'est pas comprise aussi facilement que celles spécifiant le nom de colonne réel. De plus, si vous modifiez la liste de sélection, par exemple en changeant l’ordre des colonnes ou en ajoutant de nouvelles colonnes, vous devez modifier la clause ORDER BY pour éviter des résultats inattendus.  
  
 Dans une instruction SELECT TOP (*N*), utilisez toujours une clause ORDER BY. Il s'agit de la seule méthode permettant d'indiquer de manière prévisible les lignes qui sont affectées par TOP. Pour plus d’informations, consultez [TOP &#40;Transact-SQL&#41;](../../t-sql/queries/top-transact-sql.md).  
  
## <a name="interoperability"></a>Interopérabilité  
 Quand elle est utilisée avec une instruction SELECT...INTO ou INSERT...SELECT qui insère des lignes provenant d’une autre source, la clause ORDER BY ne garantit pas l’insertion des lignes dans l’ordre spécifié.  
  
 L'utilisation d'OFFSET et de FETCH dans une vue ne modifie pas la propriété Updateability de la vue.  
  
## <a name="limitations-and-restrictions"></a>Limitations et restrictions  
 Il n'existe aucune limite quant au nombre de colonnes dans la clause ORDER BY ; toutefois, la taille totale des colonnes spécifiée dans une clause ORDER BY ne peut pas dépasser 8 060 octets.  
  
 Les colonnes de types **ntext**, **text**, **image**, **geography**, **geometry**, et **xml** ne sont pas autorisées dans une clause ORDER BY.  
  
 Il n’est pas possible de spécifier un entier ou une constante quand l’argument *order_by_expression* est utilisé dans une fonction de classement. Pour plus d’informations, consultez [Clause OVER &#40;Transact-SQL&#41;](../../t-sql/queries/select-over-clause-transact-sql.md).  
  
 Si un nom de table possède un alias dans la clause FROM, seul le nom de l'alias peut être utilisé pour qualifier ses colonnes dans la clause ORDER BY.  
  
 Les noms de colonne et les alias spécifiés dans la clause ORDER BY doivent être définis dans la liste de sélection si l'instruction SELECT contient l'un des clauses ou opérateurs suivants :  
  
-   opérateur UNION  
  
-   opérateur EXCEPT  
  
-   opérateur INTERSECT  
  
-   SELECT DISTINCT  
  
 De plus, quand une instruction SELECT comprend un opérateur UNION, EXCEPT ou INTERSECT, les noms ou alias de colonne doivent correspondre à ceux spécifiés dans la liste de sélection de la première requête (côté gauche).  
  
 Dans une requête qui utilise des opérateurs UNION, EXCEPT ou INTERSECT, la clause ORDER BY est autorisée uniquement à la fin de l'instruction. Cette restriction ne s’applique qu’en cas de spécification des opérateurs UNION, EXCEPT et INTERSECT dans une requête de niveau supérieur, et non dans une sous-requête. Consultez la section Exemples qui suit.  
  
 La clause ORDER BY n'est pas valide dans les vues, les fonctions incluses, les tables dérivées et les sous-requêtes, sauf si les clauses TOP ou OFFSET et FETCH sont également spécifiées. Lorsque ORDER BY est utilisé dans ces objets, la clause est utilisée uniquement pour déterminer les lignes retournées par la clause TOP ou les clauses OFFSET et FETCH. La clause ORDER BY ne garantit pas des résultats classés lorsque ces constructions sont interrogées, sauf si ORDER BY est également spécifié dans la requête proprement dite.  
  
 OFFSET et FETCH ne sont pas prises en charge dans les vues indexées ou dans une vue définie à l'aide de la clause CHECK OPTION.  
  
 OFFSET et FETCH peuvent être utilisés dans toute requête qui autorise TOP et ORDER BY, avec les limitations suivantes :  
  
-   La clause OVER ne prend pas en charge OFFSET ni FETCH.  
  
-   OFFSET et FETCH ne peuvent pas être spécifiées directement dans des instructions INSERT, UPDATE, MERGE et DELETE, mais peuvent l'être dans une sous-requête définie dans ces instructions. Par exemple, dans l'instruction INSERT INTO SELECT, OFFSET et FETCH peuvent être spécifiées dans l'instruction SELECT.  
  
-   Dans une requête qui utilise des opérateurs UNION, EXCEPT ou INTERSECT, OFFSET et FETCH peuvent être spécifiées uniquement dans la dernière requête qui spécifie l'ordre des résultats de la requête.  
  
-   TOP ne peut pas être combiné avec OFFSET et FETCH dans la même expression de requête (dans la même étendue de requête).  
  
## <a name="using-offset-and-fetch-to-limit-the-rows-returned"></a>Utilisation d'OFFSET et de FETCH pour limiter les lignes retournées  
 Nous vous recommandons d'utiliser les clauses OFFSET et FETCH au lieu de la clause TOP pour implémenter une solution de pagination de requête et limiter le nombre de lignes envoyées à une application cliente.  
  
 L'utilisation d'OFFSET et de FETCH comme une solution de pagination requiert l'exécution de la requête une fois pour chaque « page » de données retournée à l'application cliente. Par exemple, pour retourner les résultats d'une requête par incréments de 10 lignes, vous devez exécuter la requête une fois pour retourner les lignes 1 à 10 et exécuter, puis une nouvelle fois pour retourner les lignes 11 à 20 et ainsi de suite. Chaque requête est indépendante et sans rapport les unes avec les autres. Cela signifie que, contrairement à l'utilisation d'un curseur dans lequel la requête est exécutée une fois et l'état est géré sur le serveur, l'application cliente est chargée du suivi de l'état. Pour obtenir des résultats stables entre des requêtes d'interrogation à l'aide d'OFFSET et de FETCH, les conditions suivantes doivent être réunies :  
  
1.  Les données sous-jacentes utilisées par la requête ne doivent pas changer. Autrement dit, soit les lignes touchées par la requête ne sont pas mises à jour, soit toutes les demandes pour les pages de la requête sont exécutées dans une transaction unique à l'aide de l'isolement des transactions instantané ou sérialisable. Pour plus d’informations sur ces niveaux d’isolement, consultez [SET TRANSACTION ISOLATION LEVEL &#40;Transact-SQL&#41;](../../t-sql/statements/set-transaction-isolation-level-transact-sql.md).  
  
2.  La clause ORDER BY contient une colonne ou une combinaison de colonnes dont l'unicité est garantie.  
  
 Consultez l'exemple « Exécution de plusieurs requêtes dans une transaction unique » dans la section Exemples dans la suite de cette rubrique.  
  
 Si des plans d'exécution cohérents sont importants dans votre solution de pagination, envisagez d'utiliser l'indicateur de requête OPTIMIZE FOR pour les paramètres OFFSET et FETCH. Consultez « Spécification d'expressions pour les valeurs OFFSET et FETCH » dans la section Exemples dans la suite de cette rubrique. Pour plus d’informations sur OPTIMZE FOR, consultez [Indicateurs de requête &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-query.md).  
  
## <a name="examples"></a>Exemples  
  
|Category|Éléments syntaxiques proposés|  
|--------------|------------------------------|  
|[Syntaxe de base](#BasicSyntax)|ORDER BY|  
|[Spécification de l’ordre croissant et décroissant](#SortOrder)|DESC • ASC|  
|[Spécification d’un classement](#Collation)|COLLATE|  
|[Spécification d’un ordre conditionnel](#Case)|Expression CASE|  
|[Utilisation de la clause ORDER BY dans une fonction de classement](#Rank)|Fonctions de classement|  
|[Limitation du nombre de lignes retournées](#Offset)|OFFSET • FETCH|  
|[Utilisation de la clause ORDER BY avec UNION, EXCEPT et INTERSECT](#Union)|UNION|  
  
###  <a name="basic-syntax"></a><a name="BasicSyntax"></a> Syntaxe de base  
 Les exemples fournis dans cette section présentent les fonctionnalités de base de la clause ORDER BY en utilisant la syntaxe minimale requise.  
  
#### <a name="a-specifying-a-single-column-defined-in-the-select-list"></a>R. Spécification d'une colonne unique définie dans la liste de sélection  
 L'exemple suivant classe le jeu de résultats selon la colonne `ProductID` numérique. Étant donné qu'aucun ordre de tri spécifique n'est spécifié, la valeur par défaut (ordre croissant) est utilisée.  
  
```sql
USE AdventureWorks2012;  
GO  
SELECT ProductID, Name FROM Production.Product  
WHERE Name LIKE 'Lock Washer%'  
ORDER BY ProductID;  
```  
  
#### <a name="b-specifying-a-column-that-is-not-defined-in-the-select-list"></a>B. Spécification d'une colonne qui n'est pas définie dans la liste de sélection  
 L'exemple suivant classe le jeu de résultats selon une colonne qui n'est pas incluse dans la liste de sélection, mais est définie dans la table spécifiée dans la clause FROM.  
  
```sql
USE AdventureWorks2012;  
GO  
SELECT ProductID, Name, Color  
FROM Production.Product  
ORDER BY ListPrice;  
```  
  
#### <a name="c-specifying-an-alias-as-the-sort-column"></a>C. Spécification d'un alias comme colonne de tri  
 L'exemple suivant spécifie l'alias de colonne `SchemaName` comme colonne d'ordre de tri.  
  
```sql
USE AdventureWorks2012;  
GO  
SELECT name, SCHEMA_NAME(schema_id) AS SchemaName  
FROM sys.objects  
WHERE type = 'U'  
ORDER BY SchemaName;  
```  
  
#### <a name="d-specifying-an-expression-as-the-sort-column"></a>D. Spécification d'une expression comme colonne de tri  
 L'exemple suivant utilise une expression comme colonne de tri. L'expression est définie en utilisant la fonction DATEPART pour trier le jeu de résultats selon l'année au cours de laquelle les employés ont été embauchés.  
  
```sql
USE AdventureWorks2012;  
GO  
SELECT BusinessEntityID, JobTitle, HireDate  
FROM HumanResources.Employee  
ORDER BY DATEPART(year, HireDate);  
```  
  
###  <a name="specifying-ascending-and-descending-sort-order"></a><a name="SortOrder"></a> Spécification de l’ordre de tri croissant et décroissant  
  
#### <a name="a-specifying-a-descending-order"></a>R. Spécification d'un ordre de tri décroissant  
 L'exemple suivant classe le jeu de résultats selon la colonne `ProductID` numérique par ordre décroissant.  
  
```sql
USE AdventureWorks2012;  
GO  
SELECT ProductID, Name FROM Production.Product  
WHERE Name LIKE 'Lock Washer%'  
ORDER BY ProductID DESC;  
```  
  
#### <a name="b-specifying-an-ascending-order"></a>B. Spécification d’un ordre croissant  
 L'exemple suivant classe le jeu de résultats selon la colonne `Name` dans l'ordre croissant. Les caractères sont triés par ordre alphabétique, et non par ordre numérique. Autrement dit, 10 arrive avant 2.  
  
```sql
USE AdventureWorks2012;  
GO  
SELECT ProductID, Name FROM Production.Product  
WHERE Name LIKE 'Lock Washer%'  
ORDER BY Name ASC ;  
```  
  
#### <a name="c-specifying-both-ascending-and-descending-order"></a>C. Spécification d'un ordre de tri croissant et décroissant  
 L'exemple suivant classe le jeu de résultats selon deux colonnes. Le jeu de résultats de la requête fait d'abord l'objet d'un tri par ordre croissant selon la colonne `FirstName`, suivi d'un second tri par ordre décroissant selon la colonne `LastName`.  
  
```sql
USE AdventureWorks2012;  
GO  
SELECT LastName, FirstName FROM Person.Person  
WHERE LastName LIKE 'R%'  
ORDER BY FirstName ASC, LastName DESC ;  
```  
  
###  <a name="specifying-a-collation"></a><a name="Collation"></a> Spécification d’un classement  
 L'exemple suivant indique comment la spécification d'un classement dans la clause ORDER BY peut modifier l'ordre dans lequel les résultats de la requête sont retournés. Une table contenant une colonne définie à l'aide d'un classement non sensible à la casse et ne tenant pas compte des accents est créée. Les valeurs sont insérées selon différentes variantes d'accent et de casse. Étant donné qu'aucun classement n'est spécifié dans la clause ORDER BY, la première requête utilise le classement de la colonne lors du tri des valeurs. Dans la deuxième requête, un classement sensible à la casse et tenant compte des accents est spécifié dans la clause ORDER BY, ce qui modifie l'ordre dans lequel les lignes sont retournées.  
  
```sql
USE tempdb;  
GO  
CREATE TABLE #t1 (name NVARCHAR(15) COLLATE Latin1_General_CI_AI)  
GO  
INSERT INTO #t1 VALUES(N'Sánchez'),(N'Sanchez'),(N'sánchez'),(N'sanchez');  
  
-- This query uses the collation specified for the column 'name' for sorting.  
SELECT name  
FROM #t1  
ORDER BY name;  
-- This query uses the collation specified in the ORDER BY clause for sorting.  
SELECT name  
FROM #t1  
ORDER BY name COLLATE Latin1_General_CS_AS;  
```  
  
###  <a name="specifying-a-conditional-order"></a><a name="Case"></a> Spécification d’un ordre conditionnel  
 Les exemples suivants utilisent l’expression CASE dans une clause ORDER BY pour déterminer de façon conditionnelle l’ordre de tri des lignes d’après la valeur d’une colonne donnée. Dans le premier exemple, la valeur de la colonne `SalariedFlag` de la table `HumanResources.Employee` est évaluée. Les employés pour lesquels `SalariedFlag` a la valeur 1 sont retournés par ordre décroissant d'`BusinessEntityID`. Les employés pour lesquels `SalariedFlag` a la valeur 0 sont retournés par ordre croissant d'`BusinessEntityID`. Dans le deuxième exemple, le jeu de résultats est classé par la colonne `TerritoryName` lorsque la colonne `CountryRegionName` est égale à « United States » et par `CountryRegionName` pour toutes les autres lignes.  
  
```sql
SELECT BusinessEntityID, SalariedFlag  
FROM HumanResources.Employee  
ORDER BY CASE SalariedFlag WHEN 1 THEN BusinessEntityID END DESC  
        ,CASE WHEN SalariedFlag = 0 THEN BusinessEntityID END;  
GO  
```  
  
```sql
SELECT BusinessEntityID, LastName, TerritoryName, CountryRegionName  
FROM Sales.vSalesPerson  
WHERE TerritoryName IS NOT NULL  
ORDER BY CASE CountryRegionName WHEN 'United States' THEN TerritoryName  
         ELSE CountryRegionName END;  
```  
  
###  <a name="using-order-by-in-a-ranking-function"></a><a name="Rank"></a> Utilisation de la clause ORDER BY dans une fonction de classement  
 L'exemple suivant utilise la clause ORDER BY dans les fonctions de classement ROW_NUMBER, RANK, DENSE_RANK et NTILE.  
  
```sql
USE AdventureWorks2012;  
GO  
SELECT p.FirstName, p.LastName  
    ,ROW_NUMBER() OVER (ORDER BY a.PostalCode) AS "Row Number"  
    ,RANK() OVER (ORDER BY a.PostalCode) AS "Rank"  
    ,DENSE_RANK() OVER (ORDER BY a.PostalCode) AS "Dense Rank"  
    ,NTILE(4) OVER (ORDER BY a.PostalCode) AS "Quartile"  
    ,s.SalesYTD, a.PostalCode  
FROM Sales.SalesPerson AS s   
    INNER JOIN Person.Person AS p   
        ON s.BusinessEntityID = p.BusinessEntityID  
    INNER JOIN Person.Address AS a   
        ON a.AddressID = p.BusinessEntityID  
WHERE TerritoryID IS NOT NULL AND SalesYTD <> 0;  
```  
  
###  <a name="limiting-the-number-of-rows-returned"></a><a name="Offset"></a> Limitation du nombre de lignes retournées  
 Les exemples suivants utilisent OFFSET et FETCH pour limiter le nombre de lignes retournées par une requête.  
  
**S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et versions ultérieures et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].  
  
#### <a name="a-specifying-integer-constants-for-offset-and-fetch-values"></a>R. Spécification de constantes entières pour les valeurs OFFSET et FETCH  
 L'exemple suivant spécifie une constante entière comme valeur pour les clauses OFFSET et FETCH. La première requête retourne toutes les lignes triées selon la colonne `DepartmentID`. Comparez les résultats retournés par cette requête avec les résultats des deux requêtes qui la suivent. La requête suivante utilise la clause `OFFSET 5 ROWS` pour ignorer les 5 premières lignes et retourner toutes les lignes restantes. La dernière requête utilise la clause `OFFSET 0 ROWS` pour démarrer avec la première ligne, puis utilise `FETCH NEXT 10 ROWS ONLY` pour limiter les lignes retournées à 10 depuis le jeu de résultats trié.  
  
```sql
USE AdventureWorks2012;  
GO  
-- Return all rows sorted by the column DepartmentID.  
SELECT DepartmentID, Name, GroupName  
FROM HumanResources.Department  
ORDER BY DepartmentID;  
  
-- Skip the first 5 rows from the sorted result set and return all remaining rows.  
SELECT DepartmentID, Name, GroupName  
FROM HumanResources.Department  
ORDER BY DepartmentID OFFSET 5 ROWS;  
  
-- Skip 0 rows and return only the first 10 rows from the sorted result set.  
SELECT DepartmentID, Name, GroupName  
FROM HumanResources.Department  
ORDER BY DepartmentID   
    OFFSET 0 ROWS  
    FETCH NEXT 10 ROWS ONLY;  
```  
  
#### <a name="b-specifying-variables-for-offset-and-fetch-values"></a>B. Spécification de variables pour les valeurs OFFSET et FETCH  
 L'exemple suivant déclare les variables `@RowsToSkip` et `@FetchRows`, puis spécifie ces variables dans les clauses OFFSET et FETCH.  
  
```sql
USE AdventureWorks2012;  
GO  
-- Specifying variables for OFFSET and FETCH values    
DECLARE @RowsToSkip TINYINT = 2
      , @FetchRows TINYINT = 8;  
SELECT DepartmentID, Name, GroupName  
FROM HumanResources.Department  
ORDER BY DepartmentID ASC   
    OFFSET @RowsToSkip ROWS   
    FETCH NEXT @FetchRows ROWS ONLY;  
```  
  
#### <a name="c-specifying-expressions-for-offset-and-fetch-values"></a>C. Spécification d'expressions pour les valeurs OFFSET et FETCH  
 L'exemple suivant utilise l'expression `@StartingRowNumber - 1` pour spécifier la valeur OFFSET et l'expression `@EndingRowNumber - @StartingRowNumber + 1` pour spécifier la valeur FETCH. De plus, l'indicateur de requête, OPTIMIZE FOR, est spécifié. Cet indicateur permet d'attribuer à une variable locale une valeur déterminée lors de la compilation et de l'optimisation de la requête. Cette valeur n'est utilisée que pendant l'optimisation de la requête, et non pas lors de son exécution. Pour plus d’informations, consultez [Indicateurs de requête &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-query.md).  
  
```sql
USE AdventureWorks2012;  
GO  
  
-- Specifying expressions for OFFSET and FETCH values      
DECLARE @StartingRowNumber TINYINT = 1  
      , @EndingRowNumber TINYINT = 8;  
SELECT DepartmentID, Name, GroupName  
FROM HumanResources.Department  
ORDER BY DepartmentID ASC   
    OFFSET @StartingRowNumber - 1 ROWS   
    FETCH NEXT @EndingRowNumber - @StartingRowNumber + 1 ROWS ONLY  
OPTION ( OPTIMIZE FOR (@StartingRowNumber = 1, @EndingRowNumber = 20) );  
```  
  
#### <a name="d-specifying-a-constant-scalar-subquery-for-offset-and-fetch-values"></a>D. Spécification d'une sous-requête scalaire constante pour les valeurs OFFSET et FETCH  
 L'exemple suivant utilise une sous-requête scalaire constante pour définir la valeur de la clause FETCH. Le sous-requête retourne une valeur unique de la colonne `PageSize` dans la table `dbo.AppSettings`.  
  
```sql
-- Specifying a constant scalar subquery  
USE AdventureWorks2012;  
GO  
CREATE TABLE dbo.AppSettings (AppSettingID INT NOT NULL, PageSize INT NOT NULL);  
GO  
INSERT INTO dbo.AppSettings VALUES(1, 10);  
GO  
DECLARE @StartingRowNumber TINYINT = 1;  
SELECT DepartmentID, Name, GroupName  
FROM HumanResources.Department  
ORDER BY DepartmentID ASC   
    OFFSET @StartingRowNumber ROWS   
    FETCH NEXT (SELECT PageSize FROM dbo.AppSettings WHERE AppSettingID = 1) ROWS ONLY;  
```  
  
#### <a name="e-running-multiple-queries-in-a-single-transaction"></a>E. Exécution de plusieurs requêtes dans une transaction unique  
 L'exemple suivant affiche une méthode d'implémentation d'une solution de pagination qui garantit le retour de résultats stables dans toutes les demandes émanant de la requête. La requête est exécutée dans une transaction unique à l'aide du niveau d'isolement d'instantané, et la colonne spécifiée dans la clause ORDER BY garantit l'unicité de colonne.  
  
```sql
USE AdventureWorks2012;  
GO  
  
-- Ensure the database can support the snapshot isolation level set for the query.  
IF (SELECT snapshot_isolation_state FROM sys.databases WHERE name = N'AdventureWorks2012') = 0  
    ALTER DATABASE AdventureWorks2012 SET ALLOW_SNAPSHOT_ISOLATION ON;  
GO  
  
-- Set the transaction isolation level  to SNAPSHOT for this query.  
SET TRANSACTION ISOLATION LEVEL SNAPSHOT;  
GO  
  
-- Beginning the transaction.
BEGIN TRANSACTION;  
GO  
-- Declare and set the variables for the OFFSET and FETCH values.  
DECLARE @StartingRowNumber INT = 1  
      , @RowCountPerPage INT = 3;  
  
-- Create the condition to stop the transaction after all rows have been returned.  
WHILE (SELECT COUNT(*) FROM HumanResources.Department) >= @StartingRowNumber  
BEGIN  
  
-- Run the query until the stop condition is met.  
SELECT DepartmentID, Name, GroupName  
FROM HumanResources.Department  
ORDER BY DepartmentID ASC   
    OFFSET @StartingRowNumber - 1 ROWS   
    FETCH NEXT @RowCountPerPage ROWS ONLY;  
  
-- Increment @StartingRowNumber value.  
SET @StartingRowNumber = @StartingRowNumber + @RowCountPerPage;  
CONTINUE  
END;  
GO  
COMMIT TRANSACTION;  
GO  
```  
  
###  <a name="using-order-by-with-union-except-and-intersect"></a><a name="Union"></a> Utilisation de la clause ORDER BY avec UNION, EXCEPT et INTERSECT  
 Lorsqu'une requête utilise les opérateurs UNION, EXCEPT ou INTERSECT, la clause ORDER BY doit être spécifiée à la fin de l'instruction et les résultats des requêtes combinées sont triés. L'exemple suivant retourne tous les produits qui sont rouges ou jaunes et effectue le tri de cette liste combinée selon la colonne `ListPrice`.  
  
```sql
USE AdventureWorks2012;  
GO  
SELECT Name, Color, ListPrice  
FROM Production.Product  
WHERE Color = 'Red'  
-- ORDER BY cannot be specified here.  
UNION ALL  
SELECT Name, Color, ListPrice  
FROM Production.Product  
WHERE Color = 'Yellow'  
ORDER BY ListPrice ASC;  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Exemples : [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
 L’exemple suivant illustre le tri d’un jeu de résultats par ordre croissant selon la colonne `EmployeeKey` numérique.  
  
```sql
-- Uses AdventureWorks  
  
SELECT EmployeeKey, FirstName, LastName FROM DimEmployee  
WHERE LastName LIKE 'A%'  
ORDER BY EmployeeKey;  
```  
  
 L’exemple suivant trie un jeu de résultats par ordre décroissant selon la colonne `EmployeeKey` numérique.  
  
```sql
-- Uses AdventureWorks  
  
SELECT EmployeeKey, FirstName, LastName FROM DimEmployee  
WHERE LastName LIKE 'A%'  
ORDER BY EmployeeKey DESC;  
```  
  
 L’exemple suivant trie un jeu de résultats sur la colonne `LastName`.  
  
```sql
-- Uses AdventureWorks  
  
SELECT EmployeeKey, FirstName, LastName FROM DimEmployee  
WHERE LastName LIKE 'A%'  
ORDER BY LastName;  
```  
  
 L’exemple suivant trie un jeu de résultats sur deux colonnes. Cette requête effectue un premier tri par ordre croissant selon la colonne `FirstName`, puis elle trie les valeurs `FirstName` communes par ordre décroissant selon la colonne `LastName`.  
  
```sql
-- Uses AdventureWorks  
  
SELECT EmployeeKey, FirstName, LastName FROM DimEmployee  
WHERE LastName LIKE 'A%'  
ORDER BY LastName, FirstName;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Expressions &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md)   
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
 [FROM &#40;Transact-SQL&#41;](../../t-sql/queries/from-transact-sql.md)   
 [Fonctions de classement &#40;Transact-SQL&#41;](../../t-sql/functions/ranking-functions-transact-sql.md)   
 [TOP &#40;Transact-SQL&#41;](../../t-sql/queries/top-transact-sql.md)   
 [Indicateurs de requête &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-query.md)   
 [EXCEPT et INTERSECT &#40;Transact-SQL&#41;](../../t-sql/language-elements/set-operators-except-and-intersect-transact-sql.md)   
 [UNION &#40;Transact-SQL&#41;](../../t-sql/language-elements/set-operators-union-transact-sql.md)   
 [CASE &#40;Transact-SQL&#41;](../../t-sql/language-elements/case-transact-sql.md)  
  
  

