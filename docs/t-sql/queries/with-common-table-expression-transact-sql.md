---
title: WITH common_table_expression (Transact-SQL) | Microsoft Docs
description: Référence Transact-SQL sur l’utilisation d’expressions de table communes (CTE) dans les requêtes.
ms.date: 08/09/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- WITH common_table_expression
- WITH_TSQL
- WITH
- common table expression
dev_langs:
- TSQL
helpviewer_keywords:
- WITH common_table_expression clause
- CTEs
- hierarchical queries [SQL Server], WITH common_table_expression
- recursive CTEs [SQL Server]
- recursive queries [SQL Server]
- common table expressions
- MAXRECURSION hint
- clauses [SQL Server], WITH common_table_expression
ms.assetid: 27cfb819-3e8d-4274-8bbe-cbbe4d9c2e23
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 1f2455ad3601ac5139766e732046d9436513185c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192804"
---
# <a name="with-common_table_expression-transact-sql"></a>WITH common_table_expression (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Spécifie un jeu de résultats nommé temporaire, désigné par le terme d'expression de table commune (CTE, Common Table Expression). Cette clause est dérivée d’une requête simple et définie au sein de l’étendue d’exécution d’une seule instruction SELECT, INSERT, UPDATE, DELETE ou MERGE. Cette clause peut également être utilisée dans une instruction CREATE VIEW comme faisant partie de l'instruction SELECT qui la définit. Une expression de table commune peut inclure des références à elle-même. Dans ce cas, elle est désignée en tant qu'expression de table commune récursive.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
[ WITH <common_table_expression> [ ,...n ] ]  
  
<common_table_expression>::=  
    expression_name [ ( column_name [ ,...n ] ) ]  
    AS  
    ( CTE_query_definition )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *expression_name*  
Identificateur valide pour l’expression de table commune. *expression_name* doit être différent du nom de toutes les autres expressions de table communes définies dans la même clause WITH \<common_table_expression>, mais *expression_name* peut être identique au nom d’une table de base ou d’une vue. Toute référence à *expression_name* dans la requête utilise l’expression de table commune à la place de l’objet de base.
  
 *column_name*  
 Spécifie un nom de colonne dans l'expression de table commune. Les noms dupliqués ne sont pas autorisés au sein d'une définition d'expression de table commune unique (CTE, Common Table Expression). Le nombre de noms de colonnes spécifiés doit correspondre au nombre de colonnes dans le jeu de résultats de *CTE_query_definition*. La liste des noms de colonnes n'est facultative que si des noms distincts pour toutes les colonnes résultants sont fournis dans la définition de la requête.  
  
 *CTE_query_definition*  
 Spécifie une instruction SELECT dont le jeu de résultats remplit l'expression de table commune. L’instruction SELECT de *CTE_query_definition* doit remplir les mêmes conditions que celles requises pour la création d’une vue, mis à part qu’une expression de table commune ne peut pas en définir une autre. Pour plus d’informations, consultez la section Notes et [CREATE VIEW &#40;Transact-SQL&#41;](../../t-sql/statements/create-view-transact-sql.md).  
  
 Si plusieurs *CTE_query_definition* sont définis, les définitions de requêtes doivent être jointes par l’un des opérateurs de jeu ci-après : UNION ALL, UNION, EXCEPT ou INTERSECT.  
  
## <a name="remarks"></a>Notes  
  
## <a name="guidelines-for-creating-and-using-common-table-expressions"></a>Principes de création et d'utilisation des expressions de table communes  
Les principes suivants s'appliquent à des expressions de table communes non récursives. Pour obtenir les instructions qui s’appliquent à des expressions de table communes récursives, consultez [Principes de définition et d’utilisation des expressions de table communes récursives](#guidelines-for-defining-and-using-recursive-common-table-expressions) ci-dessous.  
  
-   Une expression de table commune (CTE) doit être suivie d’une seule instruction `SELECT`, `INSERT`, `UPDATE` ou `DELETE` qui fait référence à certaines ou à toutes les colonnes de l’expression de table commune. Une expression de table commune peut également être spécifiée dans une instruction `CREATE VIEW` comme faisant partie de l’instruction `SELECT` de définition de la vue.  
  
-   Des définitions de requête d'expression de table commune multiples peuvent être définies dans une expression de table commune non récursive. Les définitions doivent être associées par l’un des opérateurs de jeu ci-après : `UNION ALL`, `UNION`, `INTERSECT` ou `EXCEPT`.  
  
-   Une expression de table commune peut faire référence à elle-même ou à des expressions de table communes définies précédemment dans la même clause `WITH`. Les références à des éléments ultérieurs ne sont pas autorisées.  
  
-   La spécification de plusieurs clauses WITH dans une expression de table commune n'est pas autorisée. Par exemple, si un argument *CTE_query_definition* contient une sous-requête, celle-ci ne peut pas contenir de clause WITH imbriquée qui définit une autre expression de table commune.  
  
-   Les clauses suivantes ne peuvent pas être utilisées dans l’argument *CTE_query_definition* :  
  
    -   `ORDER BY` (sauf si une clause `TOP` est spécifiée)  
  
    -   `INTO`  
  
    -   Clause `OPTION` avec des indicateurs de requête  
  
    -   `FOR BROWSE`  
  
-   Lorsqu'une expression de table commune est utilisée dans une instruction faisant partie d'un traitement, l'instruction qui la précède doit être suivie d'un point-virgule.  
  
-   Une requête faisant référence à une expression de table commune peut être utilisée pour définir le curseur.  
  
-   Les tables sur des serveurs distants peuvent être référencées dans l’expression de table commune.  
  
-   Lors de l'exécution d'une expression de table commune, tous les indicateurs faisant référence à une expression de table commune peuvent entrer en conflit avec d'autres indicateurs découverts lorsque l'expression de table commune accède à ses tables sous-jacentes, de la même manière que les indicateurs qui font référence à des vues dans des requêtes. Lorsque cela se produit, la requête retourne une erreur.  
  
## <a name="guidelines-for-defining-and-using-recursive-common-table-expressions"></a>Principes de définition et d'utilisation des expressions de table communes récursives  
 Les principes suivants s'appliquent à la définition d'une expression de table commune récursive :  
  
-   La définition de l'expression de table commune récursive doit contenir au moins deux définitions de requête d'expression de table commune, un membre d'ancrage et un membre récursif. Plusieurs membres d'ancrage et membres récursifs peuvent être définis ; toutefois, toutes les définitions de requêtes de membres d'ancrage doivent être placées avant la première définition de membre récursif. Toutes les définitions de requête d'expression de table commune sont des membres d'ancrage à moins qu'ils ne fassent référence à l'expression de table commune elle-même.  
  
-   Les membres d’ancrage doivent être associés par l’un des opérateurs de jeu ci-après : UNION ALL, UNION, INTERSECT ou EXCEPT. UNION ALL est le seul opérateur défini autorisé entre le dernier membre d'ancrage et le premier membre récursif, ainsi que lors de la combinaison de plusieurs membres récursifs.  
  
-   Le nombre de colonnes des membres d'ancrage et récursifs doivent être identiques.  
  
-   Le type de données d'une colonne du membre récursif doit également être identique au type de données de la colonne correspondante du membre d'ancrage.  
  
-   La clause FROM d’un membre récursif doit référencer une seule fois l’argument *expression_name* de l’expression de table commune.  
  
-   Les éléments suivants ne sont pas autorisés dans l’argument *CTE_query_definition* d’un membre récursif :  
  
    -   `SELECT DISTINCT`  
  
    -   `GROUP BY`  
  
    -   `PIVOT` (quand le niveau de compatibilité de la base de données est de 110 ou supérieur. Consultez [Changements essentiels dans les fonctionnalités du moteur de base de données de SQL Server 2016](../../database-engine/breaking-changes-to-database-engine-features-in-sql-server-2016.md).)  
  
    -   `HAVING`  
  
    -   Agrégation scalaire  
  
    -   `TOP`  
  
    -   `LEFT`, `RIGHT`, `OUTER JOIN` (`INNER JOIN` est autorisé)  
  
    -   Sous-requêtes  
  
    -   Indicateur appliqué à une référence récursive à une expression de table commune à l’intérieur d’un argument *CTE_query_definition*.  
  
 Les principes suivants s'appliquent à l'utilisation d'une expression de table commune récursive :  
  
-   Toutes les colonnes retournées par une expression de table commune récursive peuvent prendre la valeur NULL que les colonnes retournées par les instructions `SELECT` participantes puissent prendre la valeur NULL ou non.  
  
-   Une expression de table commune récursive incorrectement composée peut entraîner une boucle infinie. Par exemple, si la définition de requête du membre récursif renvoie les mêmes valeurs pour les colonnes parent et enfant, une boucle infinie est créée. Pour éviter une boucle infinie, vous pouvez limiter le nombre de niveaux de récursivité autorisés pour une instruction spécifique à l’aide de l’indicateur `MAXRECURSION` et une valeur comprise entre 0 et 32 767 dans la clause OPTION de l’instruction `INSERT`, `UPDATE`, `DELETE` ou `SELECT`. Cela vous permet de contrôler l'exécution de l'instruction jusqu'à ce que vous résolviez le problème de code qui crée la boucle. La valeur par défaut à l'échelle du serveur est 100. Lorsque 0 est spécifié, aucune limite n'est appliquée. Une seule valeur `MAXRECURSION` peut être spécifiée par instruction. Pour plus d’informations, consultez [Indicateurs de requête &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-query.md).  
  
-   Une vue contenant une expression de table commune récursive ne peut pas être utilisée pour mettre à jour des données.  
  
-   Les curseurs peuvent être définis sur des requêtes à l'aide d'expressions de table communes. L’expression de table commune est l’argument *select_statement* qui définit le jeu de résultats du curseur. Seuls les curseurs d'avance rapide uniquement et statiques (instantané) sont autorisés pour les expressions de table communes récursives. Si un autre type de curseur est spécifié dans une expression de table commune récursive, ce type est converti en statique.  
  
-   Les tables de serveurs distants peuvent être référencées dans l'expression de table commune. Si le serveur distant est référencé dans le membre récursif de l'expression de table commune récursive, un spouleur est créé pour chaque table distante afin que les tables soient accessibles localement de manière répétée. S’il s’agit d’une requête d’expression de table commune, Index Spool/Lazy Spools est affiché dans le plan de requête avec le prédicat `WITH STACK` supplémentaire. Il s'agit de l'une des méthodes permettant de confirmer la récursivité appropriée.  
  
-   Les fonctions analytiques et d'agrégation dans la partie récursive de l'expression CTE sont appliquées à l'ensemble du niveau de récursivité actuel et non à l'ensemble de l'expression CTE. Les fonctions telles que `ROW_NUMBER` s’appliquent uniquement au sous-ensemble de données qui leur est transmis par le niveau de récursivité actuel, et non à l’ensemble entier de données transmis à la partie récursive de l’expression CTE. Pour plus d’informations, consultez « K. Utilisation de fonctions analytiques dans une expression de table commune récursive », plus loin dans cet article.  
  
## <a name="features-and-limitations-of-common-table-expressions-in-sssdw-and-sspdw"></a>Fonctionnalités et limitations des expressions de table commune dans [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
 L’implémentation actuelle des expressions de table communes dans [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] présente les fonctionnalités et limitations suivantes :  
  
-   Une expression de table commune peut être spécifiée dans une instruction `SELECT`.  
  
-   Une expression de table commune peut être spécifiée dans une instruction `CREATE VIEW`.  
  
-   Une expression de table commune peut être spécifiée dans une instruction `CREATE TABLE AS SELECT` (CTAS).  
  
-   Une expression de table commune peut être spécifiée dans une instruction `CREATE REMOTE TABLE AS SELECT` (CRTAS).  
  
-   Une expression de table commune peut être spécifiée dans une instruction `CREATE EXTERNAL TABLE AS SELECT` (CETAS).  
  
-   Une table distante peut être référencée à partir d’une expression de table commune.  
  
-   Une table externe peut être référencée à partir d’une expression de table commune.  
  
-   Plusieurs définitions de requête d’expression de table commune peuvent être définies dans une même expression de table commune.  
  
-   Une expression de table commune doit être suivie d’une seule instruction `SELECT`. Les instructions `INSERT`, `UPDATE`, `DELETE` et `MERGE` ne sont pas prises en charge.  
  
-   Une expression de table commune qui contient des références à elle-même (expression de table commune récursive) n’est pas prise en charge.  
  
-   La spécification de plusieurs clauses `WITH` dans une expression de table commune n’est pas autorisée. Par exemple, si une définition de requête d’expression de table commune contient une sous-requête, cette dernière ne peut pas contenir de clause `WITH` imbriquée qui définit une autre expression de table commune.  
  
-   Une clause `ORDER BY` ne peut pas être utilisée dans l’argument CTE_query_definition, sauf si une clause `TOP` est spécifiée.  
  
-   Lorsqu'une expression de table commune est utilisée dans une instruction faisant partie d'un traitement, l'instruction qui la précède doit être suivie d'un point-virgule.  
  
-   Quand elles sont utilisées dans des instructions préparées par `sp_prepare`, les expressions de table communes se comportent de la même façon que les autres instructions `SELECT` dans PDW. Toutefois, si elles sont utilisées dans des instructions CETAS préparées par `sp_prepare`, leur comportement peut différer de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et d’autres instructions PDW en raison de la façon dont la liaison est implémentée pour `sp_prepare`. Si l’instruction `SELECT` qui référence une expression de table commune utilise une colonne incorrecte qui n’existe pas dans cette expression, `sp_prepare` est passé sans que l’ erreur soit détectée, mais cette dernière est levée pendant `sp_execute` à la place.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-creating-a-simple-common-table-expression"></a>R. Création d'une expression de table commune simple  
 L'exemple suivant affiche le nombre total de commandes client par année pour chaque commercial chez [!INCLUDE[ssSampleDBCoFull](../../includes/sssampledbcofull-md.md)].  
  
```sql   
-- Define the CTE expression name and column list.  
WITH Sales_CTE (SalesPersonID, SalesOrderID, SalesYear)  
AS  
-- Define the CTE query.  
(  
    SELECT SalesPersonID, SalesOrderID, YEAR(OrderDate) AS SalesYear  
    FROM Sales.SalesOrderHeader  
    WHERE SalesPersonID IS NOT NULL  
)  
-- Define the outer query referencing the CTE name.  
SELECT SalesPersonID, COUNT(SalesOrderID) AS TotalSales, SalesYear  
FROM Sales_CTE  
GROUP BY SalesYear, SalesPersonID  
ORDER BY SalesPersonID, SalesYear;  
```  
  
### <a name="b-using-a-common-table-expression-to-limit-counts-and-report-averages"></a>B. Utilisation d'une expression de table commune pour limiter les nombres et les moyennes de rapports  
 L'exemple suivant affiche le nombre moyen de commandes client de toutes les années pour les commerciaux.  
  
```sql  
WITH Sales_CTE (SalesPersonID, NumberOfOrders)  
AS  
(  
    SELECT SalesPersonID, COUNT(*)  
    FROM Sales.SalesOrderHeader  
    WHERE SalesPersonID IS NOT NULL  
    GROUP BY SalesPersonID  
)  
SELECT AVG(NumberOfOrders) AS "Average Sales Per Person"  
FROM Sales_CTE;  
```  
  
### <a name="c-using-multiple-cte-definitions-in-a-single-query"></a>C. Utilisation de plusieurs définitions d'expression de table commune dans une seule requête  
 L'exemple ci-dessous montre comment définir plus d'une expression de table commune dans une seule requête. Notez qu'une virgule sépare les définitions de requête d'expression de table commune. La fonction FORMAT, utilisée pour afficher les montants monétaires dans un format monétaire, est disponible dans SQL Server 2012 et version ultérieure.  
  
```sql  
WITH Sales_CTE (SalesPersonID, TotalSales, SalesYear)  
AS  
-- Define the first CTE query.  
(  
    SELECT SalesPersonID, SUM(TotalDue) AS TotalSales, YEAR(OrderDate) AS SalesYear  
    FROM Sales.SalesOrderHeader  
    WHERE SalesPersonID IS NOT NULL  
       GROUP BY SalesPersonID, YEAR(OrderDate)  
  
)  
,   -- Use a comma to separate multiple CTE definitions.  
  
-- Define the second CTE query, which returns sales quota data by year for each sales person.  
Sales_Quota_CTE (BusinessEntityID, SalesQuota, SalesQuotaYear)  
AS  
(  
       SELECT BusinessEntityID, SUM(SalesQuota)AS SalesQuota, YEAR(QuotaDate) AS SalesQuotaYear  
       FROM Sales.SalesPersonQuotaHistory  
       GROUP BY BusinessEntityID, YEAR(QuotaDate)  
)  
  
-- Define the outer query by referencing columns from both CTEs.  
SELECT SalesPersonID  
  , SalesYear  
  , FORMAT(TotalSales,'C','en-us') AS TotalSales  
  , SalesQuotaYear  
  , FORMAT (SalesQuota,'C','en-us') AS SalesQuota  
  , FORMAT (TotalSales -SalesQuota, 'C','en-us') AS Amt_Above_or_Below_Quota  
FROM Sales_CTE  
JOIN Sales_Quota_CTE ON Sales_Quota_CTE.BusinessEntityID = Sales_CTE.SalesPersonID  
                    AND Sales_CTE.SalesYear = Sales_Quota_CTE.SalesQuotaYear  
ORDER BY SalesPersonID, SalesYear;    
```  
  
Voici un jeu de résultats partiel.  
  
```  
SalesPersonID SalesYear   TotalSales    SalesQuotaYear SalesQuota  Amt_Above_or_Below_Quota  
------------- ---------   -----------   -------------- ---------- ----------------------------------   
274           2005        $32,567.92    2005           $35,000.00  ($2,432.08)  
274           2006        $406,620.07   2006           $455,000.00 ($48,379.93)  
274           2007        $515,622.91   2007           $544,000.00 ($28,377.09)  
274           2008        $281,123.55   2008           $271,000.00  $10,123.55  
```  
  
### <a name="d-using-a-recursive-common-table-expression-to-display-multiple-levels-of-recursion"></a>D. Utilisation d'une expression de table commune récursive pour afficher plusieurs niveaux de récursivité  
 L'exemple suivant affiche la liste hiérarchique des responsables et des employés sous leurs ordres. L'exemple commence en créant et en remplissant la table `dbo.MyEmployees`.  
  
```sql  
-- Create an Employee table.  
CREATE TABLE dbo.MyEmployees  
(  
EmployeeID SMALLINT NOT NULL,  
FirstName NVARCHAR(30)  NOT NULL,  
LastName  NVARCHAR(40) NOT NULL,  
Title NVARCHAR(50) NOT NULL,  
DeptID SMALLINT NOT NULL,  
ManagerID INT NULL,  
 CONSTRAINT PK_EmployeeID PRIMARY KEY CLUSTERED (EmployeeID ASC)   
);  
-- Populate the table with values.  
INSERT INTO dbo.MyEmployees VALUES   
 (1, N'Ken', N'Sánchez', N'Chief Executive Officer',16,NULL)  
,(273, N'Brian', N'Welcker', N'Vice President of Sales',3,1)  
,(274, N'Stephen', N'Jiang', N'North American Sales Manager',3,273)  
,(275, N'Michael', N'Blythe', N'Sales Representative',3,274)  
,(276, N'Linda', N'Mitchell', N'Sales Representative',3,274)  
,(285, N'Syed', N'Abbas', N'Pacific Sales Manager',3,273)  
,(286, N'Lynn', N'Tsoflias', N'Sales Representative',3,285)  
,(16,  N'David',N'Bradley', N'Marketing Manager', 4, 273)  
,(23,  N'Mary', N'Gibson', N'Marketing Specialist', 4, 16);  
```  
  
```sql
WITH DirectReports(ManagerID, EmployeeID, Title, EmployeeLevel) AS   
(  
    SELECT ManagerID, EmployeeID, Title, 0 AS EmployeeLevel  
    FROM dbo.MyEmployees   
    WHERE ManagerID IS NULL  
    UNION ALL  
    SELECT e.ManagerID, e.EmployeeID, e.Title, EmployeeLevel + 1  
    FROM dbo.MyEmployees AS e  
        INNER JOIN DirectReports AS d  
        ON e.ManagerID = d.EmployeeID   
)  
SELECT ManagerID, EmployeeID, Title, EmployeeLevel   
FROM DirectReports  
ORDER BY ManagerID;   
```  
  
#### <a name="using-a-recursive-common-table-expression-to-display-two-levels-of-recursion"></a>Utilisation d'une expression de table commune récursive pour afficher deux niveaux de récursivité  
 L'exemple suivant affiche les responsables et les employés sous leurs ordres. Le nombre de niveaux retournés est limité à deux.  
  
```sql
WITH DirectReports(ManagerID, EmployeeID, Title, EmployeeLevel) AS   
(  
    SELECT ManagerID, EmployeeID, Title, 0 AS EmployeeLevel  
    FROM dbo.MyEmployees   
    WHERE ManagerID IS NULL  
    UNION ALL  
    SELECT e.ManagerID, e.EmployeeID, e.Title, EmployeeLevel + 1  
    FROM dbo.MyEmployees AS e  
        INNER JOIN DirectReports AS d  
        ON e.ManagerID = d.EmployeeID   
)  
SELECT ManagerID, EmployeeID, Title, EmployeeLevel   
FROM DirectReports  
WHERE EmployeeLevel <= 2 ;  
```  
  
#### <a name="using-a-recursive-common-table-expression-to-display-a-hierarchical-list"></a>Utilisation d'une expression de table commune récursive pour afficher une liste hiérarchique  
 L’exemple suivant ajoute les noms du responsable et des employés, ainsi que leurs titres respectifs. La hiérarchie des responsables et des employés est mise en évidence par l'indentation de chaque niveau.  
  
```sql
WITH DirectReports(Name, Title, EmployeeID, EmployeeLevel, Sort)  
AS (SELECT CONVERT(VARCHAR(255), e.FirstName + ' ' + e.LastName),  
        e.Title,  
        e.EmployeeID,  
        1,  
        CONVERT(VARCHAR(255), e.FirstName + ' ' + e.LastName)  
    FROM dbo.MyEmployees AS e  
    WHERE e.ManagerID IS NULL  
    UNION ALL  
    SELECT CONVERT(VARCHAR(255), REPLICATE ('|    ' , EmployeeLevel) +  
        e.FirstName + ' ' + e.LastName),  
        e.Title,  
        e.EmployeeID,  
        EmployeeLevel + 1,  
        CONVERT (VARCHAR(255), RTRIM(Sort) + '|    ' + FirstName + ' ' +   
                 LastName)  
    FROM dbo.MyEmployees AS e  
    JOIN DirectReports AS d ON e.ManagerID = d.EmployeeID  
    )  
SELECT EmployeeID, Name, Title, EmployeeLevel  
FROM DirectReports   
ORDER BY Sort;  
```  
  
#### <a name="using-maxrecursion-to-cancel-a-statement"></a>Utilisation de MAXRECURSION pour annuler une instruction  
 `MAXRECURSION` peut être utilisé pour empêcher une expression de table commune récursive mal rédigée d'entrer dans une boucle infinie. L'exemple suivant créée intentionnellement une boucle infinie et utilise l'indicateur `MAXRECURSION` pour limiter le nombre de niveaux de récursivité à deux.  
  
```sql
--Creates an infinite loop  
WITH cte (EmployeeID, ManagerID, Title) AS  
(  
    SELECT EmployeeID, ManagerID, Title  
    FROM dbo.MyEmployees  
    WHERE ManagerID IS NOT NULL  
  UNION ALL  
    SELECT cte.EmployeeID, cte.ManagerID, cte.Title  
    FROM cte   
    JOIN  dbo.MyEmployees AS e   
        ON cte.ManagerID = e.EmployeeID  
)  
--Uses MAXRECURSION to limit the recursive levels to 2  
SELECT EmployeeID, ManagerID, Title  
FROM cte  
OPTION (MAXRECURSION 2);  
```  
  
 Une fois l'erreur de codage corrigée, MAXRECURSION n'est plus nécessaire. L'exemple suivant montre le code corrigé.  
  
```sql
WITH cte (EmployeeID, ManagerID, Title)  
AS  
(  
    SELECT EmployeeID, ManagerID, Title  
    FROM dbo.MyEmployees  
    WHERE ManagerID IS NOT NULL  
  UNION ALL  
    SELECT  e.EmployeeID, e.ManagerID, e.Title  
    FROM dbo.MyEmployees AS e  
    JOIN cte ON e.ManagerID = cte.EmployeeID  
)  
SELECT EmployeeID, ManagerID, Title  
FROM cte;  
```  
  
### <a name="e-using-a-common-table-expression-to-selectively-step-through-a-recursive-relationship-in-a-select-statement"></a>E. Utilisation d'une expression de table commune pour parcourir de façon sélective une relation récursive dans une instruction SELECT  
 L'exemple suivant montre la hiérarchie des composants et assemblys de produits nécessaires pour construire la bicyclette pour `ProductAssemblyID = 800`.  
  
```sql  
USE AdventureWorks2012;  
GO  
WITH Parts(AssemblyID, ComponentID, PerAssemblyQty, EndDate, ComponentLevel) AS  
(  
    SELECT b.ProductAssemblyID, b.ComponentID, b.PerAssemblyQty,  
        b.EndDate, 0 AS ComponentLevel  
    FROM Production.BillOfMaterials AS b  
    WHERE b.ProductAssemblyID = 800  
          AND b.EndDate IS NULL  
    UNION ALL  
    SELECT bom.ProductAssemblyID, bom.ComponentID, p.PerAssemblyQty,  
        bom.EndDate, ComponentLevel + 1  
    FROM Production.BillOfMaterials AS bom   
        INNER JOIN Parts AS p  
        ON bom.ProductAssemblyID = p.ComponentID  
        AND bom.EndDate IS NULL  
)  
SELECT AssemblyID, ComponentID, Name, PerAssemblyQty, EndDate,  
        ComponentLevel   
FROM Parts AS p  
    INNER JOIN Production.Product AS pr  
    ON p.ComponentID = pr.ProductID  
ORDER BY ComponentLevel, AssemblyID, ComponentID;  
```  
  
### <a name="f-using-a-recursive-cte-in-an-update-statement"></a>F. Utilisation d'une expression de table commune récursive dans une instruction UPDATE  
 L’exemple suivant met à jour la valeur `PerAssemblyQty` pour tous les composants du produit 'Road-550-W Yellow, 44' `(ProductAssemblyID``800`). L'expression de table commune retourne une liste hiérarchique des parties utilisées pour générer `ProductAssemblyID 800` et des composants utilisés pour créer ces parties, et ainsi de suite. Seules les lignes renvoyées par l'expression de table commune récursive sont modifiées.  
  
```sql  
USE AdventureWorks2012;  
GO  
WITH Parts(AssemblyID, ComponentID, PerAssemblyQty, EndDate, ComponentLevel) AS  
(  
    SELECT b.ProductAssemblyID, b.ComponentID, b.PerAssemblyQty,  
        b.EndDate, 0 AS ComponentLevel  
    FROM Production.BillOfMaterials AS b  
    WHERE b.ProductAssemblyID = 800  
          AND b.EndDate IS NULL  
    UNION ALL  
    SELECT bom.ProductAssemblyID, bom.ComponentID, p.PerAssemblyQty,  
        bom.EndDate, ComponentLevel + 1  
    FROM Production.BillOfMaterials AS bom   
        INNER JOIN Parts AS p  
        ON bom.ProductAssemblyID = p.ComponentID  
        AND bom.EndDate IS NULL  
)  
UPDATE Production.BillOfMaterials  
SET PerAssemblyQty = c.PerAssemblyQty * 2  
FROM Production.BillOfMaterials AS c  
JOIN Parts AS d ON c.ProductAssemblyID = d.AssemblyID  
WHERE d.ComponentLevel = 0;  
```  
  
### <a name="h-using-multiple-anchor-and-recursive-members"></a>H. Utilisation de plusieurs membres d'ancrage et récursifs  
 L'exemple suivant utilise plusieurs membres d'ancrage et récursifs pour retourner tous les ancêtres d'une personne donnée. Une table est créée et les valeurs insérées pour établir la généalogie familiale renvoyée par l'expression de table commune récursive.  
  
```sql  
-- Genealogy table  
IF OBJECT_ID('dbo.Person','U') IS NOT NULL DROP TABLE dbo.Person;  
GO  
CREATE TABLE dbo.Person(ID int, Name VARCHAR(30), Mother INT, Father INT);  
GO  
INSERT dbo.Person   
VALUES(1, 'Sue', NULL, NULL)  
      ,(2, 'Ed', NULL, NULL)  
      ,(3, 'Emma', 1, 2)  
      ,(4, 'Jack', 1, 2)  
      ,(5, 'Jane', NULL, NULL)  
      ,(6, 'Bonnie', 5, 4)  
      ,(7, 'Bill', 5, 4);  
GO  
-- Create the recursive CTE to find all of Bonnie's ancestors.  
WITH Generation (ID) AS  
(  
-- First anchor member returns Bonnie's mother.  
    SELECT Mother   
    FROM dbo.Person  
    WHERE Name = 'Bonnie'  
UNION  
-- Second anchor member returns Bonnie's father.  
    SELECT Father   
    FROM dbo.Person  
    WHERE Name = 'Bonnie'  
UNION ALL  
-- First recursive member returns male ancestors of the previous generation.  
    SELECT Person.Father  
    FROM Generation, Person  
    WHERE Generation.ID=Person.ID  
UNION ALL  
-- Second recursive member returns female ancestors of the previous generation.  
    SELECT Person.Mother  
    FROM Generation, dbo.Person  
    WHERE Generation.ID=Person.ID  
)  
SELECT Person.ID, Person.Name, Person.Mother, Person.Father  
FROM Generation, dbo.Person  
WHERE Generation.ID = Person.ID;  
GO  
```  
  
###  <a name="i-using-analytical-functions-in-a-recursive-cte"></a><a name="bkmkUsingAnalyticalFunctionsInARecursiveCTE"></a> I. Utilisation de fonctions analytiques dans une expression de table commune récursive  
 L'exemple suivant montre un piège qui peut se produire lors de l'utilisation d'une fonction analytique ou d'agrégation dans la partie récursive d'une expression CTE.  
  
```sql  
DECLARE @t1 TABLE (itmID INT, itmIDComp INT);  
INSERT @t1 VALUES (1,10), (2,10);   
  
DECLARE @t2 TABLE (itmID INT, itmIDComp INT);   
INSERT @t2 VALUES (3,10), (4,10);   
  
WITH vw AS  
 (  
    SELECT itmIDComp, itmID  
    FROM @t1  
  
    UNION ALL  
  
    SELECT itmIDComp, itmID  
    FROM @t2  
)   
,r AS  
 (  
    SELECT t.itmID AS itmIDComp  
           , NULL AS itmID  
           ,CAST(0 AS BITING) AS N  
           ,1 AS Lvl  
    FROM (SELECT 1 UNION ALL SELECT 2 UNION ALL SELECT 3 UNION ALL SELECT 4) AS t (itmID)   
  
UNION ALL  
  
SELECT t.itmIDComp  
    , t.itmID  
    , ROW_NUMBER() OVER(PARTITION BY t.itmIDComp ORDER BY t.itmIDComp, t.itmID) AS N  
    , Lvl + 1  
FROM r   
    JOIN vw AS t ON t.itmID = r.itmIDComp  
)   
  
SELECT Lvl, N FROM r;  
```  
  
Les résultats suivants sont ceux attendus pour la requête.  
  
```  
Lvl  N  
1    0  
1    0  
1    0  
1    0  
2    4  
2    3  
2    2  
2    1  
```  
  
Les résultats suivants sont les résultats réels de la requête.  
  
```  
Lvl  N  
1    0  
1    0  
1    0  
1    0  
2    1  
2    1  
2    1  
2    1  
```  
  
 `N` retourne la valeur 1 à chaque passage de la partie récursive de l'expression CTE, car seul le sous-ensemble de données de ce niveau de récursivité est transmis à `ROWNUMBER`. Pour chacune des itérations de la partie récursive de la requête, une seule ligne est passée à `ROWNUMBER`.  
  
## <a name="examples-sssdw-and-sspdw"></a>Exemples : [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="j-using-a-common-table-expression-within-a-ctas-statement"></a>J. Utilisation d’une expression de table commune dans une instruction CTAS  
 L’exemple suivant crée une table qui contient le nombre total de commandes client réalisées par chaque représentant commercial chez [!INCLUDE[ssSampleDBCoFull](../../includes/sssampledbcofull-md.md)].  
  
```sql  
USE AdventureWorks2012;  
GO   
CREATE TABLE SalesOrdersPerYear  
WITH  
(  
    DISTRIBUTION = HASH(SalesPersonID)  
)  
AS  
    -- Define the CTE expression name and column list.  
    WITH Sales_CTE (SalesPersonID, SalesOrderID, SalesYear)  
    AS  
    -- Define the CTE query.  
    (  
        SELECT SalesPersonID, SalesOrderID, YEAR(OrderDate) AS SalesYear  
        FROM Sales.SalesOrderHeader  
        WHERE SalesPersonID IS NOT NULL  
    )  
    -- Define the outer query referencing the CTE name.  
    SELECT SalesPersonID, COUNT(SalesOrderID) AS TotalSales, SalesYear  
    FROM Sales_CTE  
    GROUP BY SalesYear, SalesPersonID  
    ORDER BY SalesPersonID, SalesYear;  
GO  
```  
  
### <a name="k-using-a-common-table-expression-within-a-cetas-statement"></a>K. Utilisation d’une expression de table commune dans une instruction CETAS  
 L’exemple suivant crée une table externe qui contient le nombre total de commandes client réalisées par chaque représentant commercial chez [!INCLUDE[ssSampleDBCoFull](../../includes/sssampledbcofull-md.md)].  
  
```sql  
USE AdventureWorks2012;  
GO    
CREATE EXTERNAL TABLE SalesOrdersPerYear  
WITH  
(  
    LOCATION = 'hdfs://xxx.xxx.xxx.xxx:5000/files/Customer',  
    FORMAT_OPTIONS ( FIELD_TERMINATOR = '|' )   
)  
AS  
    -- Define the CTE expression name and column list.  
    WITH Sales_CTE (SalesPersonID, SalesOrderID, SalesYear)  
    AS  
    -- Define the CTE query.  
    (  
        SELECT SalesPersonID, SalesOrderID, YEAR(OrderDate) AS SalesYear  
        FROM Sales.SalesOrderHeader  
        WHERE SalesPersonID IS NOT NULL  
    )  
    -- Define the outer query referencing the CTE name.  
    SELECT SalesPersonID, COUNT(SalesOrderID) AS TotalSales, SalesYear  
    FROM Sales_CTE  
    GROUP BY SalesYear, SalesPersonID  
    ORDER BY SalesPersonID, SalesYear;  
GO  
```  
  
### <a name="l-using-multiple-comma-separated-ctes-in-a-statement"></a>L. Utilisation de plusieurs expressions de table communes, séparées par des virgules, dans une instruction  
 L’exemple suivant montre comment inclure deux expressions de table communes dans la même instruction. Les expressions de table communes ne peuvent pas être imbriquées (pas de récursivité).  
  
```sql  
WITH   
 CountDate (TotalCount, TableName) AS  
    (  
     SELECT COUNT(datekey), 'DimDate' FROM DimDate  
    ) ,  
 CountCustomer (TotalAvg, TableName) AS  
    (  
     SELECT COUNT(CustomerKey), 'DimCustomer' FROM DimCustomer  
    )  
SELECT TableName, TotalCount FROM CountDate  
UNION ALL  
SELECT TableName, TotalAvg FROM CountCustomer;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [CREATE VIEW &#40;Transact-SQL&#41;](../../t-sql/statements/create-view-transact-sql.md)   
 [DELETE &#40;Transact-SQL&#41;](../../t-sql/statements/delete-transact-sql.md)   
 [EXCEPT et INTERSECT &#40;Transact-SQL&#41;](../../t-sql/language-elements/set-operators-except-and-intersect-transact-sql.md)   
 [INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/insert-transact-sql.md)   
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
 [UPDATE &#40;Transact-SQL&#41;](../../t-sql/queries/update-transact-sql.md)  
  
 
