---
description: NTILE (Transact-SQL)
title: NTILE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- NTILE_TSQL
- NTILE
dev_langs:
- TSQL
helpviewer_keywords:
- distributing rows
- groups [SQL Server], row distribution
- row distribution [SQL Server]
- NTILE function
ms.assetid: 1c364511-d72a-4789-8efa-3cf2a1f6b791
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: d0386795164484da0932ff7b6963c68c6eaf414a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189943"
---
# <a name="ntile-transact-sql"></a>NTILE (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Distribue les lignes d'une partition triée dans un nombre spécifié de groupes. Les groupes sont numérotés à partir de un. Pour chaque ligne, NTILE retourne le numéro du groupe auquel la ligne appartient.  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
NTILE (integer_expression) OVER ( [ <partition_by_clause> ] < order_by_clause > )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *integer_expression*  
 Expression dont la valeur est un entier positif qui spécifie le nombre de groupes utilisés pour diviser chaque partition. *integer_expression* peut être de type **int** ou **bigint**.  
  
 \<partition_by_clause>  
 Divise le jeu de résultats généré par la clause [FROM](../../t-sql/queries/from-transact-sql.md) en partitions auxquelles la fonction est appliquée. Pour la syntaxe de PARTITION BY, consultez [OVER, clause &#40;Transact-SQL&#41;](../../t-sql/queries/select-over-clause-transact-sql.md).  
  
 \<order_by_clause>  
 Détermine l'ordre dans lequel les valeurs de NTILE sont assignées aux lignes d'une partition. Un entier ne peut pas représenter une colonne lorsque \<order_by_clause> est utilisé dans une fonction de classement.  
  
## <a name="return-types"></a>Types de retour  
 **bigint**  
  
## <a name="remarks"></a>Notes  
 Si le nombre de lignes d’une partition n’est pas divisible par *integer_expression*, vous obtiendrez des groupes de deux tailles différentes qui diffèrent d’un membre. Les groupes plus grands viennent avant les groupes plus petits dans l'ordre spécifié par la clause OVER. Par exemple, si le nombre total de lignes est égal à 53 et qu'il existe 5 groupes, les trois premiers groupes contiendront 11 lignes et les deux derniers, 10 lignes. En revanche, si le nombre total de lignes est divisible par le nombre de groupes, les lignes seront réparties équitablement entre les différents groupes. Par exemple, si le nombre total de lignes est égal à 50 et qu'il existe 5 groupes, chaque compartiment contiendra 10 lignes.  
  
 NTILE n'est pas déterministe. Pour plus d’informations, consultez [Fonctions déterministes et non déterministes](../../relational-databases/user-defined-functions/deterministic-and-nondeterministic-functions.md).  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-dividing-rows-into-groups"></a>R. Division des lignes en groupes  
 L'exemple suivant divise les lignes en quatre groupes d'employés en fonction de leurs ventes de l'année. Dans la mesure où le nombre total de lignes n'est pas divisible par le nombre de groupes, les deux premiers groupes contiennent quatre lignes tandis que les autres en possèdent trois chacun.  
  
```sql  
USE AdventureWorks2012;   
GO  
SELECT p.FirstName, p.LastName  
    ,NTILE(4) OVER(ORDER BY SalesYTD DESC) AS Quartile  
    ,CONVERT(NVARCHAR(20),s.SalesYTD,1) AS SalesYTD  
    , a.PostalCode  
FROM Sales.SalesPerson AS s   
INNER JOIN Person.Person AS p   
    ON s.BusinessEntityID = p.BusinessEntityID  
INNER JOIN Person.Address AS a   
    ON a.AddressID = p.BusinessEntityID  
WHERE TerritoryID IS NOT NULL   
    AND SalesYTD <> 0;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
  
FirstName      LastName              Quartile  SalesYTD       PostalCode  
-------------  --------------------- --------- -------------- ----------  
Linda          Mitchell              1         4,251,368.55   98027  
Jae            Pak                   1         4,116,871.23   98055  
Michael        Blythe                1         3,763,178.18   98027  
Jillian        Carson                1         3,189,418.37   98027  
Ranjit         Varkey Chudukatil     2         3,121,616.32   98055  
José           Saraiva               2         2,604,540.72   98055  
Shu            Ito                   2         2,458,535.62   98055  
Tsvi           Reiter                2         2,315,185.61   98027  
Rachel         Valdez                3         1,827,066.71   98055  
Tete           Mensa-Annan           3         1,576,562.20   98055  
David          Campbell              3         1,573,012.94   98055  
Garrett        Vargas                4         1,453,719.47   98027  
Lynn           Tsoflias              4         1,421,810.92   98055  
Pamela         Ansman-Wolfe          4         1,352,577.13   98027  

(14 row(s) affected)  
```  
  
### <a name="b-dividing-the-result-set-by-using-partition-by"></a>B. Division du jeu de résultats à l'aide de PARTITION BY  
 L'exemple suivant ajoute l'argument `PARTITION BY` au code de l'exemple A. Les lignes sont d'abord partitionnées par `PostalCode`, puis divisées en 4 groupes à l'intérieur de chaque `PostalCode`. L’exemple déclare également une variable `@NTILE_Var` et utilise cette variable pour spécifier la valeur du paramètre *integer_expression*.  
  
```sql  
USE AdventureWorks2012;  
GO  
DECLARE @NTILE_Var INT = 4;  
  
SELECT p.FirstName, p.LastName  
    ,NTILE(@NTILE_Var) OVER(PARTITION BY PostalCode ORDER BY SalesYTD DESC) AS Quartile  
    ,CONVERT(NVARCHAR(20),s.SalesYTD,1) AS SalesYTD  
    ,a.PostalCode  
FROM Sales.SalesPerson AS s   
INNER JOIN Person.Person AS p   
    ON s.BusinessEntityID = p.BusinessEntityID  
INNER JOIN Person.Address AS a   
    ON a.AddressID = p.BusinessEntityID  
WHERE TerritoryID IS NOT NULL   
    AND SalesYTD <> 0;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
FirstName    LastName             Quartile SalesYTD      PostalCode  
------------ -------------------- -------- ------------  ----------  
Linda        Mitchell             1        4,251,368.55  98027  
Michael      Blythe               1        3,763,178.18  98027  
Jillian      Carson               2        3,189,418.37  98027  
Tsvi         Reiter               2        2,315,185.61  98027  
Garrett      Vargas               3        1,453,719.47  98027  
Pamela       Ansman-Wolfe         4        1,352,577.13  98027  
Jae          Pak                  1        4,116,871.23  98055  
Ranjit       Varkey Chudukatil    1        3,121,616.32  98055  
José         Saraiva              2        2,604,540.72  98055  
Shu          Ito                  2        2,458,535.62  98055  
Rachel       Valdez               3        1,827,066.71  98055  
Tete         Mensa-Annan          3        1,576,562.20  98055  
David        Campbell             4        1,573,012.94  98055  
Lynn         Tsoflias             4        1,421,810.92  98055  
  
(14 row(s) affected)  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Exemples : [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="c-dividing-rows-into-groups"></a>C. Division des lignes en groupes  
 L’exemple suivant utilise la fonction NTILE pour diviser un ensemble de vendeurs en quatre groupes selon leurs quotas de ventes attribués pour l’année 2003. Dans la mesure où le nombre total de lignes n’est pas divisible par le nombre de groupes, le premier groupe contient 5 lignes, tandis que les autres en possèdent 4 chacun.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT e.LastName, NTILE(4) OVER(ORDER BY SUM(SalesAmountQuota) DESC) AS Quartile,  
       CONVERT (VARCHAR(13), SUM(SalesAmountQuota), 1) AS SalesQuota  
FROM dbo.DimEmployee AS e   
INNER JOIN dbo.FactSalesQuota AS sq   
    ON e.EmployeeKey = sq.EmployeeKey  
WHERE sq.CalendarYear = 2003  
    AND SalesTerritoryKey IS NOT NULL AND SalesAmountQuota <> 0  
GROUP BY e.LastName  
ORDER BY Quartile, e.LastName;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
LastName          Quartile SalesYTD  
----------------- -------- ------------`  
Blythe            1        4,716,000.00  
Carson            1        4,350,000.00  
Mitchell          1        4,682,000.00  
Pak               1        5,142,000.00  
Varkey Chudukatil 1        2,940,000.00  
Ito               2        2,644,000.00  
Saraiva           2        2,293,000.00  
Vargas            2        1,617,000.00  
Ansman-Wolfe      3        1,183,000.00  
Campbell          3        1,438,000.00  
Mensa-Annan       3        1,481,000.00  
Valdez            3        1,294,000.00  
Abbas             4          172,000.00  
Albert            4          651,000.00  
Jiang             4          544,000.00  
Tsoflias          4          867,000.00
```  
  
### <a name="d-dividing-the-result-set-by-using-partition-by"></a>D. Division du jeu de résultats à l'aide de PARTITION BY  
 L’exemple suivant ajoute l’argument PARTITION BY au code de l’exemple A. Les lignes sont d’abord partitionnées par `SalesTerritoryCountry`, puis divisées en deux groupes à l’intérieur de chaque `SalesTerritoryCountry`. Notez que la clause ORDER BY dans la clause OVER trie NTILE et que la clause ORDER BY de l’instruction SELECT trie les résultats.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT e.LastName, NTILE(2) OVER(PARTITION BY e.SalesTerritoryKey ORDER BY SUM(SalesAmountQuota) DESC) AS Quartile,  
       CONVERT (VARCHAR(13), SUM(SalesAmountQuota), 1) AS SalesQuota  
   ,st.SalesTerritoryCountry  
FROM dbo.DimEmployee AS e   
INNER JOIN dbo.FactSalesQuota AS sq   
    ON e.EmployeeKey = sq.EmployeeKey  
INNER JOIN dbo.DimSalesTerritory AS st  
    ON e.SalesTerritoryKey = st.SalesTerritoryKey  
WHERE sq.CalendarYear = 2003  
GROUP BY e.LastName,e.SalesTerritoryKey,st.SalesTerritoryCountry  
ORDER BY st.SalesTerritoryCountry, Quartile;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
LastName          Quartile SalesYTD       SalesTerritoryCountry  
----------------- -------- -------------- ------------------  
Tsoflias          1         867,000.00     Australia  
Saraiva           1        2,293,000.00    Canada  
Varkey Chudukatil 1        2,940,000.00    France  
Valdez            1        1,294,000.00    Germany  
Alberts           1          651,000.00    NA  
Jiang             1          544,000.00    NA  
Pak               1        5,142,000.00    United Kingdom  
Mensa-Annan       1        1,481,000.00    United States  
Campbell          1        1,438,000.00    United States  
Reiter            1        2,768,000.00    United States  
Blythe            1        4,716,000.00    United States  
Carson            1        4,350,000.00     United States  
Mitchell          1        4,682,000.00     United States  
Vargas            2        1,617,000.00     Canada  
Abbas             2          172,000.00     NA  
Ito               2        2,644,000.00     United States  
Ansman-Wolfe      2        1,183,000.00     United States
```  
  
## <a name="see-also"></a>Voir aussi  
 [RANK &#40;Transact-SQL&#41;](../../t-sql/functions/rank-transact-sql.md)   
 [DENSE_RANK &#40;Transact-SQL&#41;](../../t-sql/functions/dense-rank-transact-sql.md)   
 [ROW_NUMBER &#40;Transact-SQL&#41;](../../t-sql/functions/row-number-transact-sql.md)   
 [Fonctions de classement &#40;Transact-SQL&#41;](../../t-sql/functions/ranking-functions-transact-sql.md)   
 [Fonctions intégrées &#40;Transact-SQL&#41;](~/t-sql/functions/functions.md)  
  
  


