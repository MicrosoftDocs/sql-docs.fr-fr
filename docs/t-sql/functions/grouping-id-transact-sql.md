---
description: GROUPING_ID (Transact-SQL)
title: GROUPING_ID (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- GROUPING_ID_TSQL
- GROUPING_ID
dev_langs:
- TSQL
helpviewer_keywords:
- GROUP BY clause, GROUPING_ID
- GROUPING_ID function
ms.assetid: c1050658-b19f-42ee-9a05-ecd6a73b896c
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: e071cc947160ef741486e8e9eaea988687d4af9e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182388"
---
# <a name="grouping_id-transact-sql"></a>GROUPING_ID (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Fonction qui calcule le niveau de regroupement. GROUPING_ID peut être utilisé uniquement dans les clauses SELECT \<select> list, HAVING et ORDER BY lorsque GROUP BY est spécifié.  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
GROUPING_ID ( <column_expression>[ ,...n ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 \<column_expression>  
 *column_expression* dans une clause [GROUP BY](../../t-sql/queries/select-group-by-transact-sql.md).  
  
## <a name="return-type"></a>Type de retour  
 **int**  
  
## <a name="remarks"></a>Notes  
 Le GROUPING_ID \<column_expression> doit correspondre exactement à l’expression dans la liste GROUP BY. Par exemple, si vous groupez par DATEPART (yyyy, \<*column name*>), utilisez GROUPING_ID (DATEPART (yyyy, \<*column name*>)) ; ou si vous groupez par \<*column name*>, utilisez GROUPING_ID (\<*column name*>).  
  
## <a name="comparing-grouping_id--to-grouping-"></a>Comparaison de GROUPING_ID () à GROUPING ()  
 GROUPING_ID (\<column_expression> [ **,** ...*n* ]) entre l'équivalent du retour de GROUPING (\<column_expression>) pour chaque colonne dans sa liste de colonnes dans chaque ligne de sortie en tant que chaîne de uns et de zéros. GROUPING_ID interprète cette chaîne comme nombre de base 2 et retourne l'entier équivalent. Par exemple, considérez l’instruction suivante : `SELECT a, b, c, SUM(d),``GROUPING_ID(a,b,c)``FROM T GROUP BY <group by list>`. Le tableau suivant indique les valeurs d'entrée et de sortie de GROUPING_ID ().  
  
|Colonnes agrégées|Entrée GROUPING_ID (a, b, c) = GROUPING(a) + GROUPING(b) + GROUPING(c)|Sortie GROUPING_ID ()|  
|------------------------|---------------------------------------------------------------------------------------|------------------------------|  
|`a`|`100`|`4`|  
|`b`|`010`|`2`|  
|`c`|`001`|`1`|  
|`ab`|`110`|`6`|  
|`ac`|`101`|`5`|  
|`bc`|`011`|`3`|  
|`abc`|`111`|`7`|  
  
## <a name="technical-definition-of-grouping_id-"></a>Définition technique de GROUPING_ID ()  
 Chaque argument GROUPING_ID doit être un élément de la liste GROUP BY. GROUPING_ID () retourne une bitmap **integer** dont les N bits les plus bas peuvent être activés. Un **bit** activé indique que l’argument correspondant n’est pas une colonne de regroupement pour la ligne de sortie donnée. Le **bit** d’ordre le plus bas correspond à l’argument N, et le **bit** d’ordre le plus bas N-1 <sup>e</sup> correspond à l’argument 1.  
  
## <a name="grouping_id--equivalents"></a>Équivalents de GROUPING_ID ()  
 Pour une requête de regroupement unique, GROUPING (\<column_expression>) est équivalent à GROUPING_ID (\<column_expression>), et les deux fonctions retournent 0.  
  
 Par exemple, les instructions suivantes sont équivalentes :  
  
 Déclaration A :  
  
```sql  
SELECT GROUPING_ID(A,B)  
FROM T   
GROUP BY CUBE(A,B)   
```  
  
 Déclaration B :  
  
```sql  
SELECT 3 FROM T GROUP BY ()  
UNION ALL  
SELECT 1 FROM T GROUP BY A  
UNION ALL  
SELECT 2 FROM T GROUP BY B  
UNION ALL  
SELECT 0 FROM T GROUP BY A,B  
```  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-using-grouping_id-to-identify-grouping-levels"></a>R. Utilisation de GROUPING_ID pour identifier des niveaux de regroupement  
 L'exemple suivant retourne le nombre d'employés par `Name` et `Title`, `Name,` et le total de la société dans la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. `GROUPING_ID()` est utilisé pour créer une valeur pour chaque ligne de la colonne `Title` afin d'identifier le niveau de regroupement.  
  
```sql  
SELECT D.Name  
    ,CASE   
    WHEN GROUPING_ID(D.Name, E.JobTitle) = 0 THEN E.JobTitle  
    WHEN GROUPING_ID(D.Name, E.JobTitle) = 1 THEN N'Total: ' + D.Name   
    WHEN GROUPING_ID(D.Name, E.JobTitle) = 3 THEN N'Company Total:'  
        ELSE N'Unknown'  
    END AS N'Job Title'  
    ,COUNT(E.BusinessEntityID) AS N'Employee Count'  
FROM HumanResources.Employee E  
    INNER JOIN HumanResources.EmployeeDepartmentHistory DH  
        ON E.BusinessEntityID = DH.BusinessEntityID  
    INNER JOIN HumanResources.Department D  
        ON D.DepartmentID = DH.DepartmentID       
WHERE DH.EndDate IS NULL  
    AND D.DepartmentID IN (12,14)  
GROUP BY ROLLUP(D.Name, E.JobTitle);  
```  
  
### <a name="b-using-grouping_id-to-filter-a-result-set"></a>B. Utilisation de GROUPING_ID pour filtrer un jeu de résultats  
  
#### <a name="simple-example"></a>Exemple simple  
 Dans le code suivant, pour retourner uniquement les lignes qui ont un nombre d'employés par titre, supprimez les caractères de commentaire de `HAVING GROUPING_ID(D.Name, E.JobTitle); = 0` dans la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. Pour retourner uniquement des lignes avec un nombre d'employés par département, supprimez les caractères de commentaire de `HAVING GROUPING_ID(D.Name, E.JobTitle) = 1;`.  
  
```sql  
SELECT D.Name  
    ,E.JobTitle  
    ,GROUPING_ID(D.Name, E.JobTitle) AS 'Grouping Level'  
    ,COUNT(E.BusinessEntityID) AS N'Employee Count'  
FROM HumanResources.Employee AS E  
    INNER JOIN HumanResources.EmployeeDepartmentHistory AS DH  
        ON E.BusinessEntityID = DH.BusinessEntityID  
    INNER JOIN HumanResources.Department AS D  
        ON D.DepartmentID = DH.DepartmentID       
WHERE DH.EndDate IS NULL  
    AND D.DepartmentID IN (12,14)  
GROUP BY ROLLUP(D.Name, E.JobTitle)  
--HAVING GROUPING_ID(D.Name, E.JobTitle) = 0; --All titles  
--HAVING GROUPING_ID(D.Name, E.JobTitle) = 1; --Group by Name;  
```  
  
 Voici le jeu de résultats non filtré.  
  
|Nom|Intitulé|Grouping Level|Employee Count|Nom|  
|----------|-----------|--------------------|--------------------|----------|  
|Document Control|Control Specialist|0|2|Document Control|  
|Document Control|Document Control Assistant|0|2|Document Control|  
|Document Control|Document Control Manager|0|1|Document Control|  
|Document Control|NULL|1|5|Document Control|  
|Facilities and Maintenance|Facilities Administrative Assistant|0|1|Facilities and Maintenance|  
|Facilities and Maintenance|Facilities Manager|0|1|Facilities and Maintenance|  
|Facilities and Maintenance|Janitor|0|4|Facilities and Maintenance|  
|Facilities and Maintenance|Maintenance Supervisor|0|1|Facilities and Maintenance|  
|Facilities and Maintenance|NULL|1|7|Facilities and Maintenance|  
|NULL|NULL|3|12|NULL|  
  
#### <a name="complex-example"></a>Exemple complexe  
 L'exemple suivant utilise `GROUPING_ID()` pour filtrer un jeu de résultats qui contient plusieurs niveaux de regroupement par niveau de regroupement. Du code semblable peut être utilisé pour créer une vue qui a plusieurs niveaux de regroupement et une procédure stockée qui appellent la vue en passant un paramètre qui filtre la vue par niveau de regroupement. L'exemple utilise la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```sql  
DECLARE @Grouping NVARCHAR(50);  
DECLARE @GroupingLevel SMALLINT;  
SET @Grouping = N'CountryRegionCode Total';  
  
SELECT @GroupingLevel = (  
    CASE @Grouping  
        WHEN N'Grand Total'             THEN 15  
        WHEN N'SalesPerson Total'       THEN 14  
        WHEN N'Store Total'             THEN 13  
        WHEN N'Store SalesPerson Total' THEN 12  
        WHEN N'CountryRegionCode Total' THEN 11  
        WHEN N'Group Total'             THEN 7  
        ELSE N'Unknown'  
    END);  
  
SELECT   
    T.[Group]  
    ,T.CountryRegionCode  
    ,S.Name AS N'Store'  
    ,(SELECT P.FirstName + ' ' + P.LastName   
        FROM Person.Person AS P   
        WHERE P.BusinessEntityID = H.SalesPersonID)  
        AS N'Sales Person'  
    ,SUM(TotalDue)AS N'TotalSold'  
    ,CAST(GROUPING(T.[Group])AS char(1)) +   
        CAST(GROUPING(T.CountryRegionCode)AS char(1)) +   
        CAST(GROUPING(S.Name)AS char(1)) +   
        CAST(GROUPING(H.SalesPersonID)AS char(1))   
        AS N'GROUPING base-2'  
    ,GROUPING_ID((T.[Group])  
        ,(T.CountryRegionCode),(S.Name),(H.SalesPersonID)  
        ) AS N'GROUPING_ID'  
    ,CASE   
        WHEN GROUPING_ID(  
            (T.[Group]),(T.CountryRegionCode)  
            ,(S.Name),(H.SalesPersonID)  
            ) = 15 THEN N'Grand Total'  
        WHEN GROUPING_ID(  
            (T.[Group]),(T.CountryRegionCode)  
            ,(S.Name),(H.SalesPersonID)  
            ) = 14 THEN N'SalesPerson Total'  
        WHEN GROUPING_ID(  
            (T.[Group]),(T.CountryRegionCode)  
            ,(S.Name),(H.SalesPersonID)  
            ) = 13 THEN N'Store Total'  
        WHEN GROUPING_ID(  
            (T.[Group]),(T.CountryRegionCode)  
            ,(S.Name),(H.SalesPersonID)  
            ) = 12 THEN N'Store SalesPerson Total'  
        WHEN GROUPING_ID(  
            (T.[Group]),(T.CountryRegionCode)  
            ,(S.Name),(H.SalesPersonID)  
            ) = 11 THEN N'CountryRegionCode Total'  
        WHEN GROUPING_ID(  
            (T.[Group]),(T.CountryRegionCode)  
            ,(S.Name),(H.SalesPersonID)  
            ) =  7 THEN N'Group Total'  
        ELSE N'Error'  
        END AS N'Level'  
FROM Sales.Customer AS C  
    INNER JOIN Sales.Store AS S  
        ON C.StoreID  = S.BusinessEntityID   
    INNER JOIN Sales.SalesTerritory AS T  
        ON C.TerritoryID  = T.TerritoryID   
    INNER JOIN Sales.SalesOrderHeader AS H  
        ON C.CustomerID = H.CustomerID  
GROUP BY GROUPING SETS ((S.Name,H.SalesPersonID)  
    ,(H.SalesPersonID),(S.Name)  
    ,(T.[Group]),(T.CountryRegionCode),()  
    )  
HAVING GROUPING_ID(  
    (T.[Group]),(T.CountryRegionCode),(S.Name),(H.SalesPersonID)  
    ) = @GroupingLevel  
ORDER BY   
    GROUPING_ID(S.Name,H.SalesPersonID),GROUPING_ID((T.[Group])  
    ,(T.CountryRegionCode)  
    ,(S.Name)  
    ,(H.SalesPersonID))ASC;  
```  
  
### <a name="c-using-grouping_id--with-rollup-and-cube-to-identify-grouping-levels"></a>C. Utilisation de GROUPING_ID () avec ROLLUP et CUBE pour identifier des niveaux de regroupement  
 Le code des exemples suivants illustre l'utilisation de `GROUPING()` pour calculer la colonne `Bit Vector(base-2)`. `GROUPING_ID()` est utilisé pour calculer la colonne `Integer Equivalent` correspondante. L'ordre des colonnes dans la fonction `GROUPING_ID()` est l'inverse de celui des colonnes concaténées par la fonction `GROUPING()`.  
  
 Dans ces exemples, `GROUPING_ID()` est utilisé pour créer une valeur pour chaque ligne dans la colonne `Grouping Level` afin d'identifier le niveau de regroupement. Les niveaux de regroupement ne sont pas toujours une liste consécutive d’entiers commençant à 1 (0, 1, 2,...*n*).  
  
> [!NOTE]  
>  GROUPING et GROUPING_ID peuvent être utilisés dans une clause HAVING pour filtrer un jeu de résultats.  
  
#### <a name="rollup-example"></a>Exemple ROLLUP  
 Dans cet exemple, tous les niveaux de regroupement n'apparaissent pas comme dans l'exemple CUBE suivant. Si l'ordre des colonnes dans la liste `ROLLUP` est modifié, les niveaux de valeurs dans la colonne `Grouping Level` devront également être modifiés. L'exemple utilise la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```sql  
SELECT DATEPART(yyyy,OrderDate) AS N'Year'  
    ,DATEPART(mm,OrderDate) AS N'Month'  
    ,DATEPART(dd,OrderDate) AS N'Day'  
    ,SUM(TotalDue) AS N'Total Due'  
    ,CAST(GROUPING(DATEPART(dd,OrderDate)) AS CHAR(1)) +   
        CAST(GROUPING(DATEPART(mm,OrderDate)) AS CHAR(1)) +   
        CAST(GROUPING(DATEPART(yyyy,OrderDate)) AS CHAR(1))   
     AS N'Bit Vector(base-2)'  
    ,GROUPING_ID(DATEPART(yyyy,OrderDate)  
        ,DATEPART(mm,OrderDate)  
        ,DATEPART(dd,OrderDate))   
        AS N'Integer Equivalent'  
    ,CASE  
        WHEN GROUPING_ID(DATEPART(yyyy,OrderDate)  
            ,DATEPART(mm,OrderDate),DATEPART(dd,OrderDate)  
            ) = 0 THEN N'Year Month Day'  
        WHEN GROUPING_ID(DATEPART(yyyy,OrderDate)  
            ,DATEPART(mm,OrderDate),DATEPART(dd,OrderDate)  
            ) = 1 THEN N'Year Month'  
        WHEN GROUPING_ID(DATEPART(yyyy,OrderDate)  
            ,DATEPART(mm,OrderDate),DATEPART(dd,OrderDate)  
            ) = 2 THEN N'not used'  
        WHEN GROUPING_ID(DATEPART(yyyy,OrderDate)  
            ,DATEPART(mm,OrderDate),DATEPART(dd,OrderDate)  
            ) = 3 THEN N'Year'  
        WHEN GROUPING_ID(DATEPART(yyyy,OrderDate)  
            ,DATEPART(mm,OrderDate),DATEPART(dd,OrderDate)  
            ) = 4 THEN N'not used'  
        WHEN GROUPING_ID(DATEPART(yyyy,OrderDate)  
            ,DATEPART(mm,OrderDate),DATEPART(dd,OrderDate)  
            ) = 5 THEN N'not used'  
        WHEN GROUPING_ID(DATEPART(yyyy,OrderDate)  
            ,DATEPART(mm,OrderDate),DATEPART(dd,OrderDate)  
            ) = 6 THEN N'not used'  
        WHEN GROUPING_ID(DATEPART(yyyy,OrderDate)  
            ,DATEPART(mm,OrderDate),DATEPART(dd,OrderDate)  
            ) = 7 THEN N'Grand Total'  
    ELSE N'Error'  
    END AS N'Grouping Level'  
FROM Sales.SalesOrderHeader  
WHERE DATEPART(yyyy,OrderDate) IN(N'2007',N'2008')  
    AND DATEPART(mm,OrderDate) IN(1,2)  
    AND DATEPART(dd,OrderDate) IN(1,2)  
GROUP BY ROLLUP(DATEPART(yyyy,OrderDate)  
        ,DATEPART(mm,OrderDate)  
        ,DATEPART(dd,OrderDate))  
ORDER BY GROUPING_ID(DATEPART(mm,OrderDate)  
    ,DATEPART(yyyy,OrderDate)  
    ,DATEPART(dd,OrderDate)  
    )  
    ,DATEPART(yyyy,OrderDate)  
    ,DATEPART(mm,OrderDate)  
    ,DATEPART(dd,OrderDate);  
```  
  
 Voici un jeu de résultats partiel.  
  
|Year|Month|jour|Total Due|Bit Vector (base-2)|Integer Equivalent|Grouping Level|  
|----------|-----------|---------|---------------|----------------------------|------------------------|--------------------|  
|2007|1|1|1497452,6066|000|0|Year Month Day|  
|2007|1|2|21772,3494|000|0|Year Month Day|  
|2007|2|1|2705653,5913|000|0|Year Month Day|  
|2007|2|2|21684,4068|000|0|Year Month Day|  
|2008|1|1|1908122,0967|000|0|Year Month Day|  
|2008|1|2|46458,0691|000|0|Year Month Day|  
|2008|2|1|3108771,9729|000|0|Year Month Day|  
|2008|2|2|54598,5488|000|0|Year Month Day|  
|2007|1|NULL|1519224,956|100|1|Year Month|  
|2007|2|NULL|2727337,9981|100|1|Year Month|  
|2008|1|NULL|1954580,1658|100|1|Year Month|  
|2008|2|NULL|3163370,5217|100|1|Year Month|  
|2007|NULL|NULL|4246562,9541|110|3|Year|  
|2008|NULL|NULL|5117950,6875|110|3|Year|  
|NULL|NULL|NULL|9364513,6416|111|7|Total général|  
  
#### <a name="cube-example"></a>Exemple CUBE  
 Dans cet exemple, la fonction `GROUPING_ID()` est utilisée pour créer une valeur pour chaque ligne dans la colonne `Grouping Level` afin d'identifier le niveau de regroupement.  
  
 Contrairement à `ROLLUP` dans l'exemple précédent, `CUBE` sort tous les niveaux de regroupement. Si l'ordre des colonnes dans la liste `CUBE` est modifié, les niveaux de valeurs dans la colonne `Grouping Level` devront également être modifiés. L'exemple utilise la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]  
  
```sql  
SELECT DATEPART(yyyy,OrderDate) AS N'Year'  
    ,DATEPART(mm,OrderDate) AS N'Month'  
    ,DATEPART(dd,OrderDate) AS N'Day'  
    ,SUM(TotalDue) AS N'Total Due'  
    ,CAST(GROUPING(DATEPART(dd,OrderDate)) AS CHAR(1)) +   
        CAST(GROUPING(DATEPART(mm,OrderDate)) AS CHAR(1)) +   
        CAST(GROUPING(DATEPART(yyyy,OrderDate)) AS CHAR(1))   
        AS N'Bit Vector(base-2)'  
    ,GROUPING_ID(DATEPART(yyyy,OrderDate)  
        ,DATEPART(mm,OrderDate)  
        ,DATEPART(dd,OrderDate))   
        AS N'Integer Equivalent'  
    ,CASE  
        WHEN GROUPING_ID(DATEPART(yyyy,OrderDate)  
            ,DATEPART(mm,OrderDate),DATEPART(dd,OrderDate)  
            ) = 0 THEN N'Year Month Day'  
        WHEN GROUPING_ID(DATEPART(yyyy,OrderDate)   
            ,DATEPART(mm,OrderDate),DATEPART(dd,OrderDate)  
            ) = 1 THEN N'Year Month'  
        WHEN GROUPING_ID(DATEPART(yyyy,OrderDate)   
            ,DATEPART(mm,OrderDate),DATEPART(dd,OrderDate)  
            ) = 2 THEN N'Year Day'  
        WHEN GROUPING_ID(DATEPART(yyyy,OrderDate)   
            ,DATEPART(mm,OrderDate),DATEPART(dd,OrderDate)  
            ) = 3 THEN N'Year'  
        WHEN GROUPING_ID(DATEPART(yyyy,OrderDate)   
            ,DATEPART(mm,OrderDate),DATEPART(dd,OrderDate)  
            ) = 4 THEN N'Month Day'  
        WHEN GROUPING_ID(DATEPART(yyyy,OrderDate)   
            ,DATEPART(mm,OrderDate),DATEPART(dd,OrderDate)  
            ) = 5 THEN N'Month'  
        WHEN GROUPING_ID(DATEPART(yyyy,OrderDate)   
            ,DATEPART(mm,OrderDate),DATEPART(dd,OrderDate)  
            ) = 6 THEN N'Day'  
        WHEN GROUPING_ID(DATEPART(yyyy,OrderDate)   
            ,DATEPART(mm,OrderDate),DATEPART(dd,OrderDate)  
            ) = 7 THEN N'Grand Total'  
    ELSE N'Error'  
    END AS N'Grouping Level'  
FROM Sales.SalesOrderHeader  
WHERE DATEPART(yyyy,OrderDate) IN(N'2007',N'2008')  
    AND DATEPART(mm,OrderDate) IN(1,2)  
    AND DATEPART(dd,OrderDate) IN(1,2)  
GROUP BY CUBE(DATEPART(yyyy,OrderDate)  
    ,DATEPART(mm,OrderDate)  
    ,DATEPART(dd,OrderDate))  
ORDER BY GROUPING_ID(DATEPART(yyyy,OrderDate)  
    ,DATEPART(mm,OrderDate)  
    ,DATEPART(dd,OrderDate)  
    )  
    ,DATEPART(yyyy,OrderDate)  
    ,DATEPART(mm,OrderDate)  
    ,DATEPART(dd,OrderDate);  
```  
  
 Voici un jeu de résultats partiel.  
  
|Year|Month|jour|Total Due|Bit Vector (base-2)|Integer Equivalent|Grouping Level|  
|----------|-----------|---------|---------------|----------------------------|------------------------|--------------------|  
|2007|1|1|1497452,6066|000|0|Year Month Day|  
|2007|1|2|21772,3494|000|0|Year Month Day|  
|2007|2|1|2705653,5913|000|0|Year Month Day|  
|2007|2|2|21684,4068|000|0|Year Month Day|  
|2008|1|1|1908122,0967|000|0|Year Month Day|  
|2008|1|2|46458,0691|000|0|Year Month Day|  
|2008|2|1|3108771,9729|000|0|Year Month Day|  
|2008|2|2|54598,5488|000|0|Year Month Day|  
|2007|1|NULL|1519224,956|100|1|Year Month|  
|2007|2|NULL|2727337,9981|100|1|Year Month|  
|2008|1|NULL|1954580,1658|100|1|Year Month|  
|2008|2|NULL|3163370,5217|100|1|Year Month|  
|2007|NULL|1|4203106,1979|010|2|Year Day|  
|2007|NULL|2|43456,7562|010|2|Year Day|  
|2008|NULL|1|5016894,0696|010|2|Year Day|  
|2008|NULL|2|101056,6179|010|2|Year Day|  
|2007|NULL|NULL|4246562,9541|110|3|Year|  
|2008|NULL|NULL|5117950,6875|110|3|Year|  
|NULL|1|1|3405574,7033|001|4|Month Day|  
|NULL|1|2|68230,4185|001|4|Month Day|  
|NULL|2|1|5814425,5642|001|4|Month Day|  
|NULL|2|2|76282,9556|001|4|Month Day|  
|NULL|1|NULL|3473805,1218|101|5|Month|  
|NULL|2|NULL|5890708,5198|101|5|Month|  
|NULL|NULL|1|9220000,2675|011|6|jour|  
|NULL|NULL|2|144513,3741|011|6|jour|  
|NULL|NULL|NULL|9364513,6416|111|7|Total général|  
  
## <a name="see-also"></a>Voir aussi  
 [GROUPING &#40;Transact-SQL&#41;](../../t-sql/functions/grouping-transact-sql.md)   
 [GROUP BY &#40;Transact-SQL&#41;](../../t-sql/queries/select-group-by-transact-sql.md)  
  
  
