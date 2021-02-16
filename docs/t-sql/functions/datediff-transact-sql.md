---
title: DATEDIFF (Transact-SQL) | Microsoft Docs
description: Référence Transact-SQL pour la fonction DATEDIFF. Retourne la différence numérique entre une date de début et de fin en fonction de datepart.
ms.date: 07/18/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DATEDIFF_TSQL
- DATEDIFF
dev_langs:
- TSQL
helpviewer_keywords:
- dates [SQL Server], functions
- DATEDIFF function [SQL Server]
- time [SQL Server], crossed boundaries
- differences in date and time [SQL Server]
- counting crossed date time boundaries [SQL Server]
- date and time [SQL Server], DATEDIFF
- dates [SQL Server], crossed boundaries
- boundary differences date and time [SQL Server]
- functions [SQL Server], time
- functions [SQL Server], date and time
- interval dates [SQL Server]
- time [SQL Server], functions
- crossing date time boundaries [SQL Server]
- calculating dates times [SQL Server]
ms.assetid: eba979f2-1a8d-4cce-9d75-b74f9b519b37
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 202134aeb21010a666eeb3b33fa54ee0143e3b63
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341666"
---
# <a name="datediff-transact-sql"></a>DATEDIFF (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Cette fonction retourne le nombre (valeur entière signée) de limites datepart spécifiées, traversées entre les valeurs *startdate* et *enddate* spécifiées.
  
Consultez [DATEDIFF_BIG &#40;Transact-SQL&#41;](../../t-sql/functions/datediff-big-transact-sql.md) pour obtenir une fonction qui gère des différences plus importantes entre les valeurs *startdate* et *enddate*. Pour obtenir une vue d’ensemble de tous les types de données et fonctions de date et d’heure [!INCLUDE[tsql](../../includes/tsql-md.md)], consultez [Types de données et fonctions de date et d’heure &#40;Transact-SQL&#41;](../../t-sql/functions/date-and-time-data-types-and-functions-transact-sql.md).
  
![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
DATEDIFF ( datepart , startdate , enddate )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments

*datepart*  
Unités dans lesquelles **DATEDIFF** signale la différence entre _startdate_ et _enddate_. Parmi les unités _datepart_ couramment utilisées, citons `month` et `second`.

La valeur _datepart_ ne peut pas être spécifiée dans une variable ni comme chaîne entre guillemets (par exemple, `'month'`).

Le tableau suivant liste toutes les valeurs _datepart_ valides. **DATEDIFF** accepte le nom complet de _datepart_ ou toute abréviation listée du nom complet.

|Nom *datepart*|Abréviation *datepart*|  
|-----------|------------|
|**year**|**yy, yyyy**|  
|**quarter**|**qq, q**|  
|**month**|**mm, m**|  
|**dayofyear**|**dy, y**|  
|**day**|**dd, d**|  
|**week**|**wk, ww**|  
|**hour**|**hh**|  
|**minute**|**mi, n**|  
|**second**|**ss, s**|  
|**millisecond**|**ms**|  
|**microsecond**|**mcs**|  
|**nanosecond**|**ns**|  
| &nbsp; | &nbsp; |

> [!NOTE]
> Chaque nom *datepart* spécifique et les abréviations pour ce nom *datepart* retournent la même valeur.

*startdate*  
Expression qui peut être résolue en valeur, parmi les suivantes :

+ **date**
+ **datetime**
+ **datetimeoffset**
+ **datetime2** 
+ **smalldatetime**
+ **time**
  
Pour éviter toute ambiguïté, utilisez des années à quatre chiffres. Pour obtenir des informations sur les années à deux chiffres, consultez [Configurer l’option de configuration du serveur two digit year cutoff](../../database-engine/configure-windows/configure-the-two-digit-year-cutoff-server-configuration-option.md).
  
*enddate*  
Consultez *startdate*.
  
## <a name="return-type"></a>Type de retour  
 **int**  
  
## <a name="return-value"></a>Valeur de retour  

Différence **int** entre *startdate* et *enddate*, exprimée dans le jeu de limites par *datepart*.
  
Par exemple, `SELECT DATEDIFF(day, '2036-03-01', '2036-02-28');` retourne -2, ce qui indique que 2036 doit être une année bissextile. Dans ce cas, si nous utilisons « 2036-03-01 » comme _startdate_ et comptons -2 jours, nous obtenons « 2036-02-28 » comme _enddate_.
  
Si une valeur de retour est hors limites pour **int** (-2 147 483 648 à +2 147 483 647), `DATEDIFF` retourne une erreur.  Pour **millisecond**, la différence maximale entre *startdate* et *enddate* est de 24 jours, 20 heures, 31 minutes et 23,647 secondes. Pour **seconde**, la différence maximale est de 68 ans, 19 jours, 3 heures, 14 minutes et 7 secondes.
  
Si *startdate* et *enddate* se voient tous les deux assigner uniquement une valeur d’heure et que *datepart* n’est pas un *datepart* d’heure, `DATEDIFF` retourne 0.
  
`DATEDIFF` utilise le composant de décalage de fuseau horaire de *startdate* ou *enddate* pour calculer la valeur de retour.
  
Dans la mesure où [smalldatetime](../../t-sql/data-types/smalldatetime-transact-sql.md) n’offre qu’une précision à la minute, les secondes et millisecondes ont toujours la valeur 0 dans la valeur de retour quand vous utilisez une valeur *smalldatetime* pour *startdate* ou **enddate**.
  
Si seule une valeur d’heure est affectée à une variable d’un type de données date, `DATEDIFF` définit la partie date manquante sur la valeur par défaut :`1900-01-01`. Si seule une valeur de date est affectée à une variable d’un type de données date ou heure, `DATEDIFF` définit la partie heure manquante sur la valeur par défaut : `00:00:00`. Si *startdate* ou *enddate* a uniquement une partie heure et que l’autre a uniquement une partie date, `DATEDIFF` affecte aux parties heure et date manquantes les valeurs par défaut.
  
Si *startdate* et *enddate* ont des types de données date différents et que l’un a plus de parties heure ou une meilleure précision en fractions de seconde que l’autre, `DATEDIFF` affecte aux parties manquantes de l’autre la valeur 0.
  
## <a name="_datepart_-boundaries"></a>Limites de _datepart_

Les instructions suivantes ont les mêmes valeurs *startdate* et *enddate*. Ces dates sont adjacentes et ont une différence d’une centaine de nanosecondes (0,0000001 seconde). La différence entre les *startdate* et *endate* dans chaque instruction traverse une limite d’heure ou de calendrier de son *datepart*. Chaque instruction retourne 1.
  
```sql
SELECT DATEDIFF(year,        '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
SELECT DATEDIFF(quarter,     '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
SELECT DATEDIFF(month,       '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
SELECT DATEDIFF(dayofyear,   '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
SELECT DATEDIFF(day,         '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
SELECT DATEDIFF(week,        '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
SELECT DATEDIFF(hour,        '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
SELECT DATEDIFF(minute,      '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
SELECT DATEDIFF(second,      '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
SELECT DATEDIFF(millisecond, '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
SELECT DATEDIFF(microsecond, '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
```

Si *startdate* et *enddate* ont des valeurs d’année différentes, mais les mêmes valeurs de semaine de calendrier, `DATEDIFF` retourne 0 pour *datepart* **week**.

## <a name="remarks"></a>Notes  
Utilisez `DATEDIFF` dans les clauses `SELECT <list>`, `WHERE`, `HAVING`, `GROUP BY` et `ORDER BY`.
  
`DATEDIFF` caste implicitement les littéraux de chaîne en type **datetime2**. Cela signifie que `DATEDIFF` ne prend pas en charge le format YDM quand la date est passée comme chaîne. Vous devez caster explicitement la chaîne en type **datetime** ou **smalldatetime** pour utiliser le format AJM.
  
La spécification de `SET DATEFIRST` n’a pas d’effet sur `DATEDIFF`. `DATEDIFF` utilise toujours Dimanche comme premier jour de la semaine pour que la fonction soit déterministe.

`DATEDIFF` peut dépasser la capacité avec une précision d’une **minute** ou plus si la différence entre *enddate* et *startdate* retourne une valeur qui est hors limites pour **int** .
  
## <a name="examples"></a>Exemples  
Ces exemples utilisent différents types d’expressions comme arguments pour les paramètres *startdate* et *enddate*.
  
### <a name="a-specifying-columns-for-startdate-and-enddate"></a>R. Spécification de colonnes pour les dates de début et de fin  
Cet exemple calcule le nombre de limites de jour qui sont traversées entre les dates de deux colonnes dans une table.
  
```sql
CREATE TABLE dbo.Duration  
    (startDate datetime2, endDate datetime2);  
    
INSERT INTO dbo.Duration(startDate, endDate)  
    VALUES ('2007-05-06 12:10:09', '2007-05-07 12:10:09');  
    
SELECT DATEDIFF(day, startDate, endDate) AS 'Duration'  
    FROM dbo.Duration;  
-- Returns: 1  
```  
  
### <a name="b-specifying-user-defined-variables-for-startdate-and-enddate"></a>B. Spécification de variables définies par l'utilisateur pour les dates de début et de fin  
Dans cet exemple, les variables définies par l’utilisateur font office d’arguments pour *startdate* et *enddate*.
  
```sql
DECLARE @startdate DATETIME2 = '2007-05-05 12:10:09.3312722';  
DECLARE @enddate   DATETIME2 = '2007-05-04 12:10:09.3312722';   
SELECT DATEDIFF(day, @startdate, @enddate);  
```  
  
### <a name="c-specifying-scalar-system-functions-for-startdate-and-enddate"></a>C. Spécification de fonctions système scalaires pour les dates de début et de fin  
Cet exemple utilise des fonctions système scalaires comme arguments pour *startdate* et *enddate*.
  
```sql
SELECT DATEDIFF(millisecond, GETDATE(), SYSDATETIME());  
```  
  
### <a name="d-specifying-scalar-subqueries-and-scalar-functions-for-startdate-and-enddate"></a>D. Spécification de sous-requêtes scalaires et de fonctions scalaires pour les dates de début et de fin  
Cet exemple utilise des sous-requêtes et des fonctions scalaires comme arguments pour *startdate* et *enddate*.
  
```sql
USE AdventureWorks2012;  
GO  
SELECT DATEDIFF(day,
    (SELECT MIN(OrderDate) FROM Sales.SalesOrderHeader),  
    (SELECT MAX(OrderDate) FROM Sales.SalesOrderHeader));  
```  
  
### <a name="e-specifying-constants-for-startdate-and-enddate"></a>E. Spécification de constantes pour les dates de début et de fin  
Cet exemple suivant des constantes à caractères comme arguments pour *startdate* et *enddate*.
  
```sql
SELECT DATEDIFF(day,
   '2007-05-07 09:53:01.0376635',
   '2007-05-08 09:53:01.0376635');  
```  
  
### <a name="f-specifying-numeric-expressions-and-scalar-system-functions-for-enddate"></a>F. Spécification d'expressions numériques et de fonctions système scalaires pour la date de fin  
Ce exemple utilise une expression numérique, `(GETDATE() + 1)` et des fonctions système scalaires, `GETDATE` et `SYSDATETIME`, comme arguments pour *enddate*.
  
```sql
USE AdventureWorks2012;  
GO  
SELECT DATEDIFF(day, '2007-05-07 09:53:01.0376635', GETDATE() + 1)
    AS NumberOfDays  
    FROM Sales.SalesOrderHeader;  
GO  
USE AdventureWorks2012;  
GO  
SELECT
    DATEDIFF(
            day,
            '2007-05-07 09:53:01.0376635',
            DATEADD(day, 1, SYSDATETIME())
        ) AS NumberOfDays  
    FROM Sales.SalesOrderHeader;  
GO  
```  
  
### <a name="g-specifying-ranking-functions-for-startdate"></a>G. Spécification de fonctions de classement pour la date de début  
Cet exemple utilise une fonction de classement comme argument pour *startdate*.
  
```sql
USE AdventureWorks2012;  
GO  
SELECT p.FirstName, p.LastName  
    ,DATEDIFF(day, ROW_NUMBER() OVER (ORDER BY   
        a.PostalCode), SYSDATETIME()) AS 'Row Number'  
FROM Sales.SalesPerson s   
    INNER JOIN Person.Person p   
        ON s.BusinessEntityID = p.BusinessEntityID  
    INNER JOIN Person.Address a   
        ON a.AddressID = p.BusinessEntityID  
WHERE TerritoryID IS NOT NULL   
    AND SalesYTD <> 0;  
```  
  
### <a name="h-specifying-an-aggregate-window-function-for-startdate"></a>H. Spécification d'une fonction d'agrégation pour la date de début  
Cet exemple utilise une fonction d’agrégation comme argument pour *startdate*.
  
```sql
USE AdventureWorks2012;  
GO  
SELECT soh.SalesOrderID, sod.ProductID, sod.OrderQty, soh.OrderDate,
    DATEDIFF(day, MIN(soh.OrderDate)   
        OVER(PARTITION BY soh.SalesOrderID), SYSDATETIME()) AS 'Total'  
FROM Sales.SalesOrderDetail sod  
    INNER JOIN Sales.SalesOrderHeader soh  
        ON sod.SalesOrderID = soh.SalesOrderID  
WHERE soh.SalesOrderID IN(43659, 58918);  
GO  
```  

### <a name="i-finding-difference-between-startdate-and-enddate-as-date-parts-strings"></a>I. Recherche de la différence entre des dates startdate et enddate sous forme de chaînes de parties de date

```sql
-- DOES NOT ACCOUNT FOR LEAP YEARS
DECLARE @date1 DATETIME, @date2 DATETIME, @result VARCHAR(100);
DECLARE @years INT, @months INT, @days INT,
    @hours INT, @minutes INT, @seconds INT, @milliseconds INT;

SET @date1 = '1900-01-01 00:00:00.000'
SET @date2 = '2018-12-12 07:08:01.123'

SELECT @years = DATEDIFF(yy, @date1, @date2)
IF DATEADD(yy, -@years, @date2) < @date1 
SELECT @years = @years-1
SET @date2 = DATEADD(yy, -@years, @date2)

SELECT @months = DATEDIFF(mm, @date1, @date2)
IF DATEADD(mm, -@months, @date2) < @date1 
SELECT @months=@months-1
SET @date2= DATEADD(mm, -@months, @date2)

SELECT @days=DATEDIFF(dd, @date1, @date2)
IF DATEADD(dd, -@days, @date2) < @date1 
SELECT @days=@days-1
SET @date2= DATEADD(dd, -@days, @date2)

SELECT @hours=DATEDIFF(hh, @date1, @date2)
IF DATEADD(hh, -@hours, @date2) < @date1 
SELECT @hours=@hours-1
SET @date2= DATEADD(hh, -@hours, @date2)

SELECT @minutes=DATEDIFF(mi, @date1, @date2)
IF DATEADD(mi, -@minutes, @date2) < @date1 
SELECT @minutes=@minutes-1
SET @date2= DATEADD(mi, -@minutes, @date2)

SELECT @seconds=DATEDIFF(s, @date1, @date2)
IF DATEADD(s, -@seconds, @date2) < @date1 
SELECT @seconds=@seconds-1
SET @date2= DATEADD(s, -@seconds, @date2)

SELECT @milliseconds=DATEDIFF(ms, @date1, @date2)

SELECT @result= ISNULL(CAST(NULLIF(@years,0) AS VARCHAR(10)) + ' years,','')
     + ISNULL(' ' + CAST(NULLIF(@months,0) AS VARCHAR(10)) + ' months,','')    
     + ISNULL(' ' + CAST(NULLIF(@days,0) AS VARCHAR(10)) + ' days,','')
     + ISNULL(' ' + CAST(NULLIF(@hours,0) AS VARCHAR(10)) + ' hours,','')
     + ISNULL(' ' + CAST(@minutes AS VARCHAR(10)) + ' minutes and','')
     + ISNULL(' ' + CAST(@seconds AS VARCHAR(10)) 
     + CASE
            WHEN @milliseconds > 0
                THEN '.' + CAST(@milliseconds AS VARCHAR(10)) 
            ELSE ''
       END 
     + ' seconds','')

SELECT @result
```

[!INCLUDE[ssResult](../../includes/ssresult-md.md)] 

```
118 years, 11 months, 11 days, 7 hours, 8 minutes and 1.123 seconds
```
  
## <a name="examples-sssdwfull-and-sspdw"></a>Exemples : [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
Ces exemples utilisent différents types d’expressions comme arguments pour les paramètres *startdate* et *enddate*.
  
### <a name="j-specifying-columns-for-startdate-and-enddate"></a>J. Spécification de colonnes pour les dates de début et de fin  
Cet exemple calcule le nombre de limites de jour qui sont traversées entre les dates de deux colonnes dans une table.
  
```sql
CREATE TABLE dbo.Duration 
    (startDate datetime2, endDate datetime2);
    
INSERT INTO dbo.Duration (startDate, endDate)  
    VALUES ('2007-05-06 12:10:09', '2007-05-07 12:10:09');  
    
SELECT TOP(1) DATEDIFF(day, startDate, endDate) AS Duration  
    FROM dbo.Duration;  
-- Returns: 1  
```  
  
### <a name="k-specifying-scalar-subqueries-and-scalar-functions-for-startdate-and-enddate"></a>K. Spécification de sous-requêtes scalaires et de fonctions scalaires pour les dates de début et de fin  
Cet exemple utilise des sous-requêtes et des fonctions scalaires comme arguments pour *startdate* et *enddate*.
  
```sql
-- Uses AdventureWorks  
  
SELECT TOP(1) DATEDIFF(day, (SELECT MIN(HireDate) FROM dbo.DimEmployee),  
    (SELECT MAX(HireDate) FROM dbo.DimEmployee))   
FROM dbo.DimEmployee;  
  
```  
  
### <a name="l-specifying-constants-for-startdate-and-enddate"></a>L. Spécification de constantes pour les dates de début et de fin  
Cet exemple suivant des constantes à caractères comme arguments pour *startdate* et *enddate*.
  
```sql
-- Uses AdventureWorks  
  
SELECT TOP(1) DATEDIFF(day,
    '2007-05-07 09:53:01.0376635',
    '2007-05-08 09:53:01.0376635') FROM DimCustomer;  
```  
  
### <a name="m-specifying-ranking-functions-for-startdate"></a>M. Spécification de fonctions de classement pour la date de début  
Cet exemple utilise une fonction de classement comme argument pour *startdate*.
  
```sql
-- Uses AdventureWorks  
  
SELECT FirstName, LastName,
    DATEDIFF(day, ROW_NUMBER() OVER (ORDER BY   
        DepartmentName), SYSDATETIME()) AS RowNumber  
FROM dbo.DimEmployee;  
```  
  
### <a name="n-specifying-an-aggregate-window-function-for-startdate"></a>N. Spécification d'une fonction d'agrégation pour la date de début  
Cet exemple utilise une fonction d’agrégation comme argument pour *startdate*.
  
```sql
-- Uses AdventureWorks  
  
SELECT FirstName, LastName, DepartmentName,
    DATEDIFF(year, MAX(HireDate)  
        OVER (PARTITION BY DepartmentName), SYSDATETIME()) AS SomeValue  
FROM dbo.DimEmployee  
```  
  
## <a name="see-also"></a>Voir aussi
[DATEDIFF_BIG &#40;Transact-SQL&#41;](../../t-sql/functions/datediff-big-transact-sql.md)  
[CAST et CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)
  
