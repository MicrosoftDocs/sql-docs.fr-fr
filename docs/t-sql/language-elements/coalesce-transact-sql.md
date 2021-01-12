---
title: COALESCE (Transact-SQL) | Microsoft Docs
description: Référence Transact-SQL pour COALESCE qui retourne la valeur de la première expression dont la valeur n’est pas NULL.
ms.date: 08/30/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- COALESCE
- COALESCE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- expressions [SQL Server], nonnull
- COALESCE function
- first nonnull expressions [SQL Server]
- nonnull expressions
ms.assetid: fafc0dba-f8a8-4aad-9b7f-908e34b74d88
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 041f5b1b87a5d6ee92cd67fe17c6fd45f682dd95
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093586"
---
# <a name="coalesce-transact-sql"></a>COALESCE (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Évalue les arguments dans l’ordre et retourne la valeur actuelle de la première expression qui ne prend pas initialement la valeur `NULL`. Par exemple, `SELECT COALESCE(NULL, NULL, 'third_value', 'fourth_value');` retourne la troisième valeur, car c’est la première valeur qui n’est pas Null. 
  
![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
COALESCE ( expression [ ,...n ] )   
```  
  
## <a name="arguments"></a>Arguments  
_expression_  
[Expression](../../t-sql/language-elements/expressions-transact-sql.md) de tout type.  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Types de retour
Retourne le type de données de l’_expression_ dont la priorité est la plus élevée. Si aucune des expressions n'acceptent les valeurs NULL, le résultat est typé comme n'acceptant pas les valeurs NULL.  
  
## <a name="remarks"></a>Notes  
Si tous les arguments ont la valeur `NULL`, `COALESCE` retourne `NULL`. Au moins une des valeurs Null doit être une valeur `NULL` typée.  
  
## <a name="comparing-coalesce-and-case"></a>Comparaison de COALESCE et de CASE  
L’expression `COALESCE` est un raccourci syntaxique de l’expression `CASE`.  Autrement dit, le code `COALESCE`(_expression1_, _...n_) est réécrit par l’optimiseur de requête sous la forme de l’expression `CASE` suivante :  
  
```sql  
CASE  
WHEN (expression1 IS NOT NULL) THEN expression1  
WHEN (expression2 IS NOT NULL) THEN expression2  
...  
ELSE expressionN  
END  
```  
  
Cela signifie que les valeurs d’entrée (_expression1_, _expression2_, _expressionN_, etc.) sont évaluées plusieurs fois. Une expression de valeur contenant une sous-requête est considérée comme non déterministe et la sous-requête est évaluée deux fois. Ce résultat est conforme à la norme SQL. Dans l’un ou l’autre cas, les résultats retournés peuvent être différents entre la première évaluation et les suivantes.  
  
Par exemple, lorsque le code `COALESCE((subquery), 1)` est exécuté, la sous-requête est évaluée deux fois. Par conséquent, vous pouvez obtenir des résultats différents selon le niveau d'isolement de la requête. Par exemple, le code peut retourner la valeur `NULL` avec le niveau d’isolement `READ COMMITTED` dans un environnement multi-utilisateurs. Pour garantir des résultats stables, utilisez le niveau d’isolement `SNAPSHOT ISOLATION` ou remplacez `COALESCE` par la fonction `ISNULL`. Vous pouvez également réécrire la requête pour envoyer (push) la sous-requête dans une sous-sélection, comme le montre l’exemple suivant :  
  
```sql  
SELECT CASE WHEN x IS NOT NULL THEN x ELSE 1 END  
FROM  
(  
SELECT (SELECT Nullable FROM Demo WHERE SomeCol = 1) AS x  
) AS T;  
  
```  
  
## <a name="comparing-coalesce-and-isnull"></a>Comparaison de COALESCE et de ISNULL  
La fonction `ISNULL` et l’expression `COALESCE` ont un objectif similaire, mais peuvent se comporter différemment.  
  
1.  `ISNULL` étant une fonction, elle est évaluée une seule fois.  Comme décrit ci-dessus, les valeurs d’entrée pour l’expression `COALESCE` peuvent être évaluées plusieurs fois.  
  
2.  La détermination du type de données de l'expression obtenue est différente. `ISNULL` utilise le type de données du premier paramètre, `COALESCE` suit les règles de l’expression `CASE` et retourne le type de données de la valeur ayant la priorité la plus élevée.  
  
3.  La possibilité de valeurs Null de l’expression de résultat est différente pour `ISNULL` et `COALESCE`. La valeur `ISNULL` renvoyée est toujours considérée comme n’acceptant PAS la valeur NULL (en supposant que la valeur renvoyée est non-NULL). En revanche, la valeur `COALESCE` avec des paramètres non null est considérée comme `NULL`. Bien qu’elles soient égales, les expressions `ISNULL(NULL, 1)` et `COALESCE(NULL, 1)` ont des possibilités de valeur NULL différentes. Ces valeurs font une différence si vous utilisez ces expressions dans des colonnes calculées, si vous créez des contraintes de clé ou si vous rendez déterministe la valeur renvoyée par une fonction définie par l’utilisateur scalaire afin qu’elle puisse être indexée, comme le montre l’exemple suivant :  
  
    ```sql  
    USE tempdb;  
    GO  
    -- This statement fails because the PRIMARY KEY cannot accept NULL values  
    -- and the nullability of the COALESCE expression for col2   
    -- evaluates to NULL.  
    CREATE TABLE #Demo   
    (   
      col1 INTEGER NULL,   
      col2 AS COALESCE(col1, 0) PRIMARY KEY,   
      col3 AS ISNULL(col1, 0)   
    );   
  
    -- This statement succeeds because the nullability of the   
    -- ISNULL function evaluates AS NOT NULL.  
  
    CREATE TABLE #Demo   
    (   
      col1 INTEGER NULL,   
      col2 AS COALESCE(col1, 0),   
      col3 AS ISNULL(col1, 0) PRIMARY KEY   
    );  
    ```  
  
4.  Les validations pour `ISNULL` et `COALESCE` sont également différentes. Par exemple, la valeur `NULL` de `ISNULL` est convertie en type **int**, tandis que pour `COALESCE`, vous devez fournir un type de données.  
  
5.  `ISNULL` n’accepte que deux paramètres. En revanche, `COALESCE` accepte un nombre variable de paramètres.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-running-a-simple-example"></a>R. Exécution d'un exemple simple  
L'exemple suivant montre comment `COALESCE` sélectionne les données de la première colonne qui a une valeur non Null. Cet exemple utilise la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```sql  
SELECT Name, Class, Color, ProductNumber,  
COALESCE(Class, Color, ProductNumber) AS FirstNotNull  
FROM Production.Product;  
```  
  
### <a name="b-running-a-complex-example"></a>B. Exécution d'un exemple complexe  
Dans l'exemple suivant, la table `wages` comporte trois colonnes qui contiennent des informations sur les salaires annuels des employés : salaire horaire, salaire et commission. Cependant, chaque employé ne perçoit qu'un seul type de salaire. Pour déterminer le montant total versé à tous les employés, utilisez `COALESCE` afin de recevoir seulement la valeur non NULL trouvée dans `hourly_wage`, `salary` et `commission`.  
  
```sql  
SET NOCOUNT ON;  
GO  
USE tempdb;  
IF OBJECT_ID('dbo.wages') IS NOT NULL  
    DROP TABLE wages;  
GO  
CREATE TABLE dbo.wages  
(  
    emp_id        TINYINT   IDENTITY,  
    hourly_wage   DECIMAL   NULL,  
    salary        DECIMAL   NULL,  
    commission    DECIMAL   NULL,  
    num_sales     TINYINT   NULL  
);  
GO  
INSERT dbo.wages (hourly_wage, salary, commission, num_sales)  
VALUES  
    (10.00, NULL, NULL, NULL),  
    (20.00, NULL, NULL, NULL),  
    (30.00, NULL, NULL, NULL),  
    (40.00, NULL, NULL, NULL),  
    (NULL, 10000.00, NULL, NULL),  
    (NULL, 20000.00, NULL, NULL),  
    (NULL, 30000.00, NULL, NULL),  
    (NULL, 40000.00, NULL, NULL),  
    (NULL, NULL, 15000, 3),  
    (NULL, NULL, 25000, 2),  
    (NULL, NULL, 20000, 6),  
    (NULL, NULL, 14000, 4);  
GO  
SET NOCOUNT OFF;  
GO  
SELECT CAST(COALESCE(hourly_wage * 40 * 52,   
   salary,   
   commission * num_sales) AS money) AS 'Total Salary'   
FROM dbo.wages  
ORDER BY 'Total Salary';  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```   
Total Salary  
------------  
10000.00  
20000.00  
20800.00  
30000.00  
40000.00  
41600.00  
45000.00  
50000.00  
56000.00  
62400.00  
83200.00  
120000.00  
  
(12 row(s) affected)
```  
  
### <a name="c-simple-example"></a>C : Exemple simple  
L’exemple suivant montre comment `COALESCE` sélectionne les données de la première colonne qui a une valeur non-Null. Cet exemple suppose que la table `Products` contient ces données :  
  
```  
Name         Color      ProductNumber  
------------ ---------- -------------  
Socks, Mens  NULL       PN1278  
Socks, Mens  Blue       PN1965  
NULL         White      PN9876
```  
 
Nous exécutons ensuite la requête COALESCE suivante :  
  
```sql  
SELECT Name, Color, ProductNumber, COALESCE(Color, ProductNumber) AS FirstNotNull   
FROM Products ;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Name         Color      ProductNumber  FirstNotNull  
------------ ---------- -------------  ------------  
Socks, Mens  NULL       PN1278         PN1278  
Socks, Mens  Blue       PN1965         Blue  
NULL         White      PN9876         White
```  
  
Notez que dans la première ligne, la valeur `FirstNotNull` est `PN1278`, pas `Socks, Mens`. Cette valeur est celle-ci car la colonne `Name` n’a pas été spécifiée en tant que paramètre de `COALESCE` dans l’exemple.  
  
### <a name="d-complex-example"></a>D : Exemple complexe  
L’exemple suivant utilise `COALESCE` pour comparer les valeurs de trois colonnes et retourner uniquement la valeur non-Null trouvée dans les colonnes.  
  
```sql  
CREATE TABLE dbo.wages  
(  
    emp_id        TINYINT   NULL,  
    hourly_wage   DECIMAL   NULL,  
    salary        DECIMAL   NULL,  
    commission    DECIMAL   NULL,  
    num_sales     TINYINT   NULL  
);  
INSERT INTO dbo.wages (emp_id, hourly_wage, salary, commission, num_sales)  
VALUES (1, 10.00, NULL, NULL, NULL);  
  
INSERT INTO dbo.wages (emp_id, hourly_wage, salary, commission, num_sales)  
VALUES (2, 20.00, NULL, NULL, NULL);  
  
INSERT INTO dbo.wages (emp_id, hourly_wage, salary, commission, num_sales)  
VALUES (3, 30.00, NULL, NULL, NULL);  
  
INSERT INTO dbo.wages (emp_id, hourly_wage, salary, commission, num_sales)  
VALUES (4, 40.00, NULL, NULL, NULL);  
  
INSERT INTO dbo.wages (emp_id, hourly_wage, salary, commission, num_sales)  
VALUES (5, NULL, 10000.00, NULL, NULL);  
  
INSERT INTO dbo.wages (emp_id, hourly_wage, salary, commission, num_sales)  
VALUES (6, NULL, 20000.00, NULL, NULL);  
  
INSERT INTO dbo.wages (emp_id, hourly_wage, salary, commission, num_sales)  
VALUES (7, NULL, 30000.00, NULL, NULL);  
  
INSERT INTO dbo.wages (emp_id, hourly_wage, salary, commission, num_sales)  
VALUES (8, NULL, 40000.00, NULL, NULL);  
  
INSERT INTO dbo.wages (emp_id, hourly_wage, salary, commission, num_sales)  
VALUES (9, NULL, NULL, 15000, 3);  
  
INSERT INTO dbo.wages (emp_id, hourly_wage, salary, commission, num_sales)  
VALUES (10,NULL, NULL, 25000, 2);  
  
INSERT INTO dbo.wages (emp_id, hourly_wage, salary, commission, num_sales)  
VALUES (11, NULL, NULL, 20000, 6);  
  
INSERT INTO dbo.wages (emp_id, hourly_wage, salary, commission, num_sales)  
VALUES (12, NULL, NULL, 14000, 4);  
  
SELECT CAST(COALESCE(hourly_wage * 40 * 52,   
   salary,   
   commission * num_sales) AS DECIMAL(10,2)) AS TotalSalary   
FROM dbo.wages  
ORDER BY TotalSalary;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```
Total Salary  
------------  
10000.00  
20000.00  
20800.00  
30000.00  
40000.00  
41600.00  
45000.00  
50000.00  
56000.00  
62400.00  
83200.00  
120000.00
```  
  
## <a name="see-also"></a>Voir aussi  
[ISNULL &#40;Transact-SQL&#41;](../../t-sql/functions/isnull-transact-sql.md)   
[CASE &#40;Transact-SQL&#41;](../../t-sql/language-elements/case-transact-sql.md)  
  
  
