---
description: COUNT (Transact-SQL)
title: COUNT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- COUNT_TSQL
- COUNT
dev_langs:
- TSQL
helpviewer_keywords:
- totals [SQL Server], COUNT function
- totals [SQL Server]
- counting items in group
- groups [SQL Server], number of items in
- number of group items
- COUNT function [Transact-SQL]
ms.assetid: 28d39da6-bc2e-46c7-858c-b1721c938830
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ac784713b7a74eb08acdfa9478b519c0b80940c1
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101281"
---
# <a name="count-transact-sql"></a>COUNT (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Cette fonction retourne le nombre d’éléments figurant dans un groupe. `COUNT` fonctionne comme la fonction [COUNT_BIG](../../t-sql/functions/count-big-transact-sql.md). Ces fonctions diffèrent uniquement par les types de données des valeurs qu’elles retournent. `COUNT` retourne toujours une valeur de type de données **int**. `COUNT_BIG` retourne toujours une valeur de type de données **bigint**.
  
![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql

-- Aggregation Function Syntax  
COUNT ( { [ [ ALL | DISTINCT ] expression ] | * } )  

-- Analytic Function Syntax  
COUNT ( [ ALL ]  { expression | * } ) OVER ( [ <partition_by_clause> ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
**ALL**  
Applique la fonction d'agrégation à toutes les valeurs. La valeur ALL est utilisée par défaut.
  
DISTINCT  
Précise que la fonction `COUNT` doit renvoyer le nombre de valeurs non nulles uniques.
  
*expression*  
[Expression](../../t-sql/language-elements/expressions-transact-sql.md) de tout type, sauf **image**, **ntext** ou **text**. Notez que `COUNT` ne prend pas en charge les fonctions d’agrégation ou sous-requêtes dans une expression.
  
\*  
Spécifie que `COUNT` doit compter toutes les lignes pour déterminer le nombre total de lignes de la table à retourner. `COUNT(*)` ne sélectionne aucun paramètre et ne prend pas en charge l’utilisation de DISTINCT. `COUNT(*)` ne nécessite pas de paramètre *expression* puisque, par définition, elle n’utilise les données d’aucune colonne en particulier. `COUNT(*)` retourne le nombre de lignes de la table spécifiée et conserve les doublons. Il compte chaque ligne séparément, y compris les lignes qui contiennent des valeurs NULL.
  
OVER **(** [ *partition_by_clause* ] [ *order_by_clause* ] [ *ROW_or_RANGE_clause* ] **)**  
*partition_by_clause* divise le jeu de résultats généré par la clause `FROM` en partitions auxquelles la fonction `COUNT` est appliquée. S'il n'est pas spécifié, la fonction gère toutes les lignes du jeu de résultats de la requête en un seul groupe. *order_by_clause* détermine l’ordre logique de l’opération. Consultez [Clause OVER &#40;Transact-SQL&#41;](../../t-sql/queries/select-over-clause-transact-sql.md) pour plus d’informations. 

## <a name="return-types"></a>Types de retour
 **int**  
  
## <a name="remarks"></a>Notes  
COUNT(\*) renvoie le nombre d'éléments figurant dans un groupe. y compris les valeurs NULL et les doublons.
  
COUNT(ALL *expression*) évalue l’argument *expression* pour chaque ligne d’un groupe et renvoie le nombre de valeurs non-NULL.
  
COUNT(DISTINCT *expression*) évalue l’argument *expression* pour chaque ligne d’un groupe et renvoie le nombre de valeurs non-NULL uniques.
  
Pour les valeurs de retour supérieures à 2^31-1, `COUNT` renvoie une erreur. Dans ce cas, utilisez plutôt `COUNT_BIG`.
  
`COUNT` est une fonction déterministe quand elle est utilisée ***sans** _ les clauses OVER et ORDER BY. Elle n’est pas déterministe quand elle est utilisée _*_avec_*_ les clauses OVER et ORDER BY. Consultez [Fonctions déterministes et non déterministes](../../relational-databases/user-defined-functions/deterministic-and-nondeterministic-functions.md) pour plus d’informations.
  
## <a name="examples"></a>Exemples  
  
### <a name="a-using-count-and-distinct"></a>R. Utilisation de COUNT et DISTINCT  
Cet exemple retourne le nombre de titres différents qu’un employé [!INCLUDE[ssSampleDBCoFull](../../includes/sssampledbcofull-md.md)] peut porter.
  
```sql
SELECT COUNT(DISTINCT Title)  
FROM HumanResources.Employee;  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
-----------
67  
  
(1 row(s) affected)
```
  
### <a name="b-using-count_"></a>B. Utilisation de COUNT(\_)  
Cet exemple retourne le nombre total d’employés [!INCLUDE[ssSampleDBCoFull](../../includes/sssampledbcofull-md.md)].
  
```sql
SELECT COUNT(*)  
FROM HumanResources.Employee;  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
-----------
290  
  
(1 row(s) affected)
```
  
### <a name="c-using-count-with-other-aggregates"></a>C. Utilisation de COUNT(\*) avec d'autres fonctions d'agrégation  
Cet exemple montre que `COUNT(*)` fonctionne avec d’autres fonctions d’agrégation dans la liste `SELECT`. L'exemple utilise la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].
  
```sql
SELECT COUNT(*), AVG(Bonus)  
FROM Sales.SalesPerson  
WHERE SalesQuota > 25000;  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
----------- ---------------------
14            3472.1428
  
(1 row(s) affected)
```
  
### <a name="d-using-the-over-clause"></a>D. Utilisation de la clause OVER  
Cet exemple utilise les fonctions `MIN`, `MAX`, `AVG` et `COUNT` avec la clause `OVER` afin de retourner des valeurs agrégées pour chaque service dans la table `HumanResources.Department` de la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].
  
```sql
SELECT DISTINCT Name  
       , MIN(Rate) OVER (PARTITION BY edh.DepartmentID) AS MinSalary  
       , MAX(Rate) OVER (PARTITION BY edh.DepartmentID) AS MaxSalary  
       , AVG(Rate) OVER (PARTITION BY edh.DepartmentID) AS AvgSalary  
       ,COUNT(edh.BusinessEntityID) OVER (PARTITION BY edh.DepartmentID) AS EmployeesPerDept  
FROM HumanResources.EmployeePayHistory AS eph  
JOIN HumanResources.EmployeeDepartmentHistory AS edh  
     ON eph.BusinessEntityID = edh.BusinessEntityID  
JOIN HumanResources.Department AS d  
ON d.DepartmentID = edh.DepartmentID
WHERE edh.EndDate IS NULL  
ORDER BY Name;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
Name                          MinSalary             MaxSalary             AvgSalary             EmployeesPerDept  
----------------------------- --------------------- --------------------- --------------------- ----------------  
Document Control              10.25                 17.7885               14.3884               5  
Engineering                   32.6923               63.4615               40.1442               6  
Executive                     39.06                 125.50                68.3034               4  
Facilities and Maintenance    9.25                  24.0385               13.0316               7  
Finance                       13.4615               43.2692               23.935                10  
Human Resources               13.9423               27.1394               18.0248               6  
Information Services          27.4038               50.4808               34.1586               10  
Marketing                     13.4615               37.50                 18.4318               11  
Production                    6.50                  84.1346               13.5537               195  
Production Control            8.62                  24.5192               16.7746               8  
Purchasing                    9.86                  30.00                 18.0202               14  
Quality Assurance             10.5769               28.8462               15.4647               6  
Research and Development      40.8654               50.4808               43.6731               4  
Sales                         23.0769               72.1154               29.9719               18  
Shipping and Receiving        9.00                  19.2308               10.8718               6  
Tool Design                   8.62                  29.8462               23.5054               6  
  
(16 row(s) affected)
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Exemples : [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="e-using-count-and-distinct"></a>E. Utilisation de COUNT et DISTINCT  
Cet exemple retourne le nombre de titres différents qu’un employé d’une société spécifique peut porter.
  
```sql
USE ssawPDW;  
  
SELECT COUNT(DISTINCT Title)  
FROM dbo.DimEmployee;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
-----------
67
```  
  
### <a name="f-using-count"></a>F. Utilisation de COUNT(\*)  
Cet exemple renvoie le nombre total de lignes dans la table `dbo.DimEmployee`.
  
```sql
USE ssawPDW;  
  
SELECT COUNT(*)  
FROM dbo.DimEmployee;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
-------------
296
```  
  
### <a name="g-using-count-with-other-aggregates"></a>G. Utilisation de COUNT(\*) avec d'autres fonctions d'agrégation  
Cet exemple associe `COUNT(*)` et d’autres fonctions d’agrégation dans la liste `SELECT`. Il renvoie le nombre de représentants commerciaux avec un quota de ventes annuel supérieur à 500 000 $ et le quota de ventes moyen.
  
```sql
USE ssawPDW;  
  
SELECT COUNT(EmployeeKey) AS TotalCount, AVG(SalesAmountQuota) AS [Average Sales Quota]  
FROM dbo.FactSalesQuota  
WHERE SalesAmountQuota > 500000 AND CalendarYear = 2001;  
  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
TotalCount  Average Sales Quota
----------  -------------------
10          683800.0000
```
  
### <a name="h-using-count-with-having"></a>H. Utilisation de COUNT avec HAVING  
Cet exemple utilise `COUNT` avec la clause `HAVING` pour retourner les services d’une société, chacun d’eux employant plus de 15 personnes.
  
```sql
USE ssawPDW;  
  
SELECT DepartmentName,   
       COUNT(EmployeeKey)AS EmployeesInDept  
FROM dbo.DimEmployee  
GROUP BY DepartmentName  
HAVING COUNT(EmployeeKey) > 15;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
DepartmentName  EmployeesInDept
--------------  ---------------
Sales           18
Production      179
```
  
### <a name="i-using-count-with-over"></a>I. Utilisation de COUNT avec OVER  
Cet exemple suivant utilise `COUNT` avec la clause `OVER` pour renvoyer le nombre de produits contenus dans chacune des commandes client spécifiées.
  
```sql
USE ssawPDW;  
  
SELECT DISTINCT COUNT(ProductKey) OVER(PARTITION BY SalesOrderNumber) AS ProductCount  
    ,SalesOrderNumber  
FROM dbo.FactInternetSales  
WHERE SalesOrderNumber IN (N'SO53115',N'SO55981');  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
ProductCount   SalesOrderID`
------------   -----------------
3              SO53115
1              SO55981
```
  
## <a name="see-also"></a>Voir aussi
[Fonctions d’agrégation &#40;Transact-SQL&#41;](../../t-sql/functions/aggregate-functions-transact-sql.md)  
[COUNT_BIG &#40;Transact-SQL&#41;](../../t-sql/functions/count-big-transact-sql.md)  
[OVER, clause &#40;Transact-SQL&#41;](../../t-sql/queries/select-over-clause-transact-sql.md)
  
  


