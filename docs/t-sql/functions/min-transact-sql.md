---
description: MIN (Transact-SQL)
title: MIN (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/13/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- MIN
- MIN_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- MIN function [Transact-SQL]
- minimum values [SQL Server]
- values [SQL Server], minimum
ms.assetid: 56cf6ec5-34f5-47e3-a402-7129039d4429
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 56d11998725a78b63d697aa898d11e7c280b4ade
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195951"
---
# <a name="min-transact-sql"></a>MIN (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Retourne la valeur minimale de l'expression. Cette fonction peut être suivie par la [clause OVER](../../t-sql/queries/select-over-clause-transact-sql.md).  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql  
-- Aggregation Function Syntax  
MIN ( [ ALL | DISTINCT ] expression )  
  
-- Analytic Function Syntax   
MIN ( [ ALL ] expression ) OVER ( [ <partition_by_clause> ] [ <order_by_clause> ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 **ALL**  
 Applique la fonction d'agrégation à toutes les valeurs. ALL est l'argument par défaut.  
  
 DISTINCT  
 Spécifie que chaque valeur unique est prise en considération. DISTINCT n'a pas d'effet avec MIN et n'est disponible que pour la compatibilité ISO.  
  
 *expression*  
 Constante, nom de colonne ou fonction, et toute combinaison d'opérateurs arithmétiques, de type chaîne ou binaire. La fonction MIN peut être utilisée avec des colonnes **numeric**, **char**, **varchar**, **uniqueidentifier** ou **datetime**, mais pas avec des colonnes **bit**. Les fonctions d'agrégation et les sous-requêtes ne sont pas autorisées.  
  
 Pour plus d’informations, consultez [Expressions &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md).  
  
 OVER **(** [ _partition\_by\_clause_ ] _order\_by\_clause_ **)**  
 *partition_by_clause* divise le jeu de résultats généré par la clause FROM en partitions auxquelles la fonction est appliquée. S'il n'est pas spécifié, la fonction gère toutes les lignes du jeu de résultats de la requête en un seul groupe. *order_by_clause* détermine l’ordre logique dans lequel l’opération est effectuée. *order_by_clause* est requis. Pour plus d’informations, consultez [Clause OVER &#40;Transact-SQL&#41;](../../t-sql/queries/select-over-clause-transact-sql.md).  
  
## <a name="return-types"></a>Types de retour  
 Retourne une valeur comme *expression*.  
  
## <a name="remarks"></a>Remarques  
 MIN ignore toutes les valeurs NULL.  
  
 Avec des colonnes de données caractères, MIN recherche la valeur la plus faible dans l'ordre de tri.  
  
 MIN est une fonction déterministe lorsqu'elle est utilisée sans les clauses OVER et ORDER BY. Elle n'est pas déterministe avec les clauses OVER et ORDER BY. Pour plus d’informations, consultez [Fonctions déterministes et non déterministes](../../relational-databases/user-defined-functions/deterministic-and-nondeterministic-functions.md).  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-simple-example"></a>R. Exemple simple  
 L'exemple suivant renvoie le taux d'imposition le plus bas (minimum). L'exemple utilise la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]  
  
```sql  
SELECT MIN(TaxRate)  
FROM Sales.SalesTaxRate;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
 -------------------
  
 5.00
  
 (1 row(s) affected)
 ```  
  
### <a name="b-using-the-over-clause"></a>B. Utilisation de la clause OVER  
 L'exemple suivant utilise les fonctions MIN, MAX, AVG et COUNT avec la clause OVER pour fournir des valeurs agrégées pour chaque service dans la table `HumanResources.Department` de la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
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
  
### <a name="c-using-min"></a>C. Utilisation de MIN  
 L’exemple suivant utilise la fonction d’agrégation MIN pour retourner le prix du produit (minimum) le moins coûteux dans un ensemble spécifié de commandes.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT DISTINCT MIN(UnitPrice)  
FROM dbo.FactResellerSales   
WHERE SalesOrderNumber IN (N'SO43659', N'SO43660', N'SO43664');  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
 ------  
 5.1865
 ```  
  
### <a name="d-using-min-with-over"></a>D. Utilisation de MIN avec OVER  
 Les exemples suivants utilisent la fonction analytique MIN OVER() pour retourner le prix du produit le moins coûteux dans chaque commande. Le jeu de résultats est partitionné par la colonne `SalesOrderID`.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT DISTINCT MIN(UnitPrice) OVER(PARTITION BY SalesOrderNumber) AS LeastExpensiveProduct,  
       SalesOrderNumber  
FROM dbo.FactResellerSales    
WHERE SalesOrderNumber IN (N'SO43659', N'SO43660', N'SO43664')  
ORDER BY SalesOrderNumber;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
LeastExpensiveProduct SalesOrderID  
--------------------- ----------  
5.1865                SO43659  
419.4589              SO43660  
28.8404               SO43664
```  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions d’agrégation &#40;Transact-SQL&#41;](../../t-sql/functions/aggregate-functions-transact-sql.md)   
 [MAX &#40;Transact-SQL&#41;](../../t-sql/functions/max-transact-sql.md)   
 [OVER, clause &#40;Transact-SQL&#41;](../../t-sql/queries/select-over-clause-transact-sql.md)  
  
  

