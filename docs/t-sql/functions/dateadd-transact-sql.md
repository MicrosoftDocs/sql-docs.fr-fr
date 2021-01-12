---
title: DATEADD (Transact-SQL) | Microsoft Docs
description: Référence Transact-SQL pour la fonction DATEADD. Cette fonction retourne une date qui a été modifiée par le datepart spécifié.
ms.date: 07/29/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DATEADD
- DATEADD_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- dates [SQL Server], functions
- add interval to date or time [SQL Server]
- subtract interval from date or time [SQL Server]
- functions [SQL Server], time
- functions [SQL Server], date and time
- time [SQL Server], functions
- dates [SQL Server], intervals
- date and time [SQL Server], DATEADD
- DATEADD function [SQL Server]
ms.assetid: 89c5ae32-89c6-47e1-979e-15d97908b9f1
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 4edc9ff57e9f8d1bd875df6d62814b3317c7ac45
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093677"
---
# <a name="dateadd-transact-sql"></a>DATEADD (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Cette fonction ajoute une valeur *number* spécifiée (entier signé) au *datepart* spécifié d’une valeur d’entrée *date*, puis retourne la valeur modifiée.
  
Pour obtenir une vue d’ensemble de tous les types de données et fonctions de date et d’heure [!INCLUDE[tsql](../../includes/tsql-md.md)], consultez [Types de données et fonctions de date et d’heure &#40;Transact-SQL&#41;](../../t-sql/functions/date-and-time-data-types-and-functions-transact-sql.md).
  
![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntaxe  
  
```sql
DATEADD (datepart , number , date )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
*datepart*  
Partie de *date* à laquelle `DATEADD` ajoute un *number* de type **integer**. Ce tableau répertorie tous les arguments *datepart* valides. 

> [!NOTE]
> `DATEADD` n’accepte pas d’équivalents de variables définis par l’utilisateur pour les arguments *datepart*. 
  
|*datepart*|Abréviations|  
|---|---|
|**year**|**yy**, **yyyy**|  
|**quarter**|**qq**, **q**|  
|**month**|**mm**, **m**|  
|**dayofyear**|**dy**, **y**|  
|**day**|**dd**, **d**|  
|**week**|**wk**, **ww**|  
|**weekday**|**dw**, **w**|  
|**hour**|**hh**|  
|**minute**|**mi**, **n**|  
|**second**|**ss**, **s**|  
|**millisecond**|**ms**|  
|**microsecond**|**mcs**|  
|**nanosecond**|**ns**|  
  
*number*  
Expression qui peut être résolue en [int](../../t-sql/data-types/int-bigint-smallint-and-tinyint-transact-sql.md) ajouté par `DATEADD` au *datepart* de *date*. `DATEADD` accepte les valeurs des variables définies par l’utilisateur pour *number*. `DATEADD` tronque une valeur *number* spécifiée ayant une fraction décimale. Il n’arrondit pas la valeur *number* dans cette situation.
  
*date*  
Expression qui peut être résolue en valeur, parmi les suivantes : 

+ **date**
+ **datetime**
+ **datetimeoffset**
+ **datetime2** 
+ **smalldatetime**
+ **time**

Pour *date*, `DATEADD` accepte une expression de colonne, une expression, un littéral de chaîne ou une variable définie par l’utilisateur. Une valeur de littéral de chaîne doit être résolue en **datetime**. Pour éviter toute ambiguïté, utilisez des années à quatre chiffres. Pour obtenir des informations sur les années à deux chiffres, consultez [Configurer l’option de configuration du serveur two digit year cutoff](../../database-engine/configure-windows/configure-the-two-digit-year-cutoff-server-configuration-option.md).
  
## <a name="return-types"></a>Types de retour

Le type de données de la valeur de retour pour cette méthode est dynamique. Le type de retour dépend de l’argument fourni pour `date`. Si la valeur de `date` est un littéral de chaîne de date, `DATEADD` retourne une valeur **datetime**. Si un autre type de données d’entrée valide est fourni pour `date`, `DATEADD` retourne le même type de données. `DATEADD` lève une erreur si l’échelle en secondes du littéral de chaîne compte plus de trois décimales (.nnn) ou si le littéral de chaîne contient la partie de décalage du fuseau horaire.
  
## <a name="return-value"></a>Valeur de retour  
  
## <a name="datepart-argument"></a>Argument datepart  
**dayofyear**, **day** et **weekday** renvoient la même valeur.
  
Chaque *datepart* et ses abréviations retournent la même valeur.
  
Si les conditions suivantes sont remplies :

+ *datepart* est **month**.
+ Le mois de *date* a plus de jours que le mois retourné.
+ Le jour *date* n’existe pas dans le mois retourné.

`DATEADD` retourne alors le dernier jour du mois retourné. Par exemple, comme le mois de septembre compte 30 jours, les deux instructions suivantes retournent 2006-09-30 00:00:00.000 :
  
```sql
SELECT DATEADD(month, 1, '20060830');
SELECT DATEADD(month, 1, '20060831');
```
  
## <a name="number-argument"></a>Argument number  
L’argument *number* ne peut pas dépasser la plage des **int**. Dans les instructions suivantes, l’argument pour *number* dépasse la plage des **int** de 1. Ces deux instructions retournent le message d’erreur suivant : `Msg 8115, Level 16, State 2, Line 1. Arithmetic overflow error converting expression to data type int."`
  
```sql
SELECT DATEADD(year,2147483648, '20060731');  
SELECT DATEADD(year,-2147483649, '20060731');  
```  
  
## <a name="date-argument"></a>Argument date  
`DATEADD` n’accepte pas d’argument *date* incrémenté à une valeur située hors de la plage de son type de données. Dans les instructions suivantes, la valeur *number* ajoutée à la valeur *date* dépasse la plage du type de données *date*. `DATEADD` retourne le message d’erreur suivant : `Msg 517, Level 16, State 1, Line 1 Adding a value to a 'datetime' column caused overflow`
  
```sql
SELECT DATEADD(year,2147483647, '20060731');  
SELECT DATEADD(year,-2147483647, '20060731');  
```  
  
## <a name="return-values-for-a-smalldatetime-date-and-a-second-or-fractional-seconds-datepart"></a>Valeurs retournées pour une date smalldatetime et une partie de date seconde ou fractions de seconde  
La partie des secondes d’une valeur [smalldatetime](../../t-sql/data-types/smalldatetime-transact-sql.md) est toujours 00. Pour une valeur *date* **smalldatetime**, les remarques suivantes s’appliquent : 

-   Si *datepart* est **second** et que la valeur de *number* est comprise entre -30 et +29, `DATEADD` ne change rien.  
-   Si *datepart* est **second** et que la valeur de *number* est inférieure à -30 ou supérieure à +29, `DATEADD` effectue son ajout en commençant à une minute.  
-   Si *datepart* est **millisecond** et que la valeur de *number* est comprise entre -30001 et +29998, `DATEADD` ne change rien.  
-   Si *datepart* est **millisecond** et que la valeur de *number* est inférieure à -30001 ou supérieure à +29998, `DATEADD` effectue son ajout en commençant à une minute.  
  
## <a name="remarks"></a>Notes  
Utilisez `DATEADD` dans les clauses suivantes :

+ GROUP BY
+ HAVING
+ ORDER BY
+ SELECT \<list>
+ WHERE
  
## <a name="fractional-seconds-precision"></a>Précision en fractions de seconde
`DATEADD` n’autorise aucun ajout si *datepart* est **microsecond** ou **nanosecond** pour les types de données *date***smalldatetime**, **date** et **datetime**.
  
Les millisecondes ont une échelle de 3 (0,123), les microsecondes une échelle de 6 (0,123456) et les nanosecondes une échelle de 9 (0,123456789). Les types de données **time**, **datetime2** et **datetimeoffset** ont une échelle maximale de 7 (.1234567). Si *datepart* est **nanosecond**, *number* doit être 100 avant que les fractions de seconde de *date* augmentent. Une valeur *number* comprise entre 1 et 49 est arrondie à 0, et une valeur comprise entre 50 et 99 est arrondie à 100.
  
Ces instructions ajoutent une valeur *datepart* égale à **millisecond**, **microsecond** ou **nanosecond**.
  
```sql
DECLARE @datetime2 datetime2 = '2007-01-01 13:10:10.1111111';  
SELECT '1 millisecond', DATEADD(millisecond,1,@datetime2)  
UNION ALL  
SELECT '2 milliseconds', DATEADD(millisecond,2,@datetime2)  
UNION ALL  
SELECT '1 microsecond', DATEADD(microsecond,1,@datetime2)  
UNION ALL  
SELECT '2 microseconds', DATEADD(microsecond,2,@datetime2)  
UNION ALL  
SELECT '49 nanoseconds', DATEADD(nanosecond,49,@datetime2)  
UNION ALL  
SELECT '50 nanoseconds', DATEADD(nanosecond,50,@datetime2)  
UNION ALL  
SELECT '150 nanoseconds', DATEADD(nanosecond,150,@datetime2);  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
1 millisecond     2007-01-01 13:10:10.1121111  
2 milliseconds    2007-01-01 13:10:10.1131111  
1 microsecond     2007-01-01 13:10:10.1111121  
2 microseconds    2007-01-01 13:10:10.1111131  
49 nanoseconds    2007-01-01 13:10:10.1111111  
50 nanoseconds    2007-01-01 13:10:10.1111112  
150 nanoseconds   2007-01-01 13:10:10.1111113  
```  
  
## <a name="time-zone-offset"></a>Décalage de fuseau horaire
`DATEADD` n’autorise aucun ajout pour le décalage de fuseau horaire.
  
## <a name="examples"></a>Exemples  

### <a name="a-incrementing-datepart-by-an-interval-of-1"></a>R. Incrémentation d'une partie de date d'un intervalle de 1  
Chacune de ces instructions incrémente *datepart* d’un intervalle de 1 :
  
```sql
DECLARE @datetime2 datetime2 = '2007-01-01 13:10:10.1111111';  
SELECT 'year', DATEADD(year,1,@datetime2)  
UNION ALL  
SELECT 'quarter',DATEADD(quarter,1,@datetime2)  
UNION ALL  
SELECT 'month',DATEADD(month,1,@datetime2)  
UNION ALL  
SELECT 'dayofyear',DATEADD(dayofyear,1,@datetime2)  
UNION ALL  
SELECT 'day',DATEADD(day,1,@datetime2)  
UNION ALL  
SELECT 'week',DATEADD(week,1,@datetime2)  
UNION ALL  
SELECT 'weekday',DATEADD(weekday,1,@datetime2)  
UNION ALL  
SELECT 'hour',DATEADD(hour,1,@datetime2)  
UNION ALL  
SELECT 'minute',DATEADD(minute,1,@datetime2)  
UNION ALL  
SELECT 'second',DATEADD(second,1,@datetime2)  
UNION ALL  
SELECT 'millisecond',DATEADD(millisecond,1,@datetime2)  
UNION ALL  
SELECT 'microsecond',DATEADD(microsecond,1,@datetime2)  
UNION ALL  
SELECT 'nanosecond',DATEADD(nanosecond,1,@datetime2);  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
Year         2008-01-01 13:10:10.1111111  
quarter      2007-04-01 13:10:10.1111111  
month        2007-02-01 13:10:10.1111111  
dayofyear    2007-01-02 13:10:10.1111111  
day          2007-01-02 13:10:10.1111111  
week         2007-01-08 13:10:10.1111111  
weekday      2007-01-02 13:10:10.1111111  
hour         2007-01-01 14:10:10.1111111  
minute       2007-01-01 13:11:10.1111111  
second       2007-01-01 13:10:11.1111111  
millisecond  2007-01-01 13:10:10.1121111  
microsecond  2007-01-01 13:10:10.1111121  
nanosecond   2007-01-01 13:10:10.1111111  
```  
  
### <a name="b-incrementing-more-than-one-level-of-datepart-in-one-statement"></a>B. Incrémentation de plusieurs niveaux d'une partie de date dans une instruction  
Chacune de ces instructions incrémente *datepart* d’une valeur *number* assez grande pour incrémenter également la valeur *datepart* immédiatement supérieure de *date* :
  
```sql
DECLARE @datetime2 datetime2;  
SET @datetime2 = '2007-01-01 01:01:01.1111111';  
--Statement                                 Result     
-------------------------------------------------------------------   
SELECT DATEADD(quarter,4,@datetime2);     --2008-01-01 01:01:01.1111111  
SELECT DATEADD(month,13,@datetime2);      --2008-02-01 01:01:01.1111111  
SELECT DATEADD(dayofyear,365,@datetime2); --2008-01-01 01:01:01.1111111  
SELECT DATEADD(day,365,@datetime2);       --2008-01-01 01:01:01.1111111  
SELECT DATEADD(week,5,@datetime2);        --2007-02-05 01:01:01.1111111  
SELECT DATEADD(weekday,31,@datetime2);    --2007-02-01 01:01:01.1111111  
SELECT DATEADD(hour,23,@datetime2);       --2007-01-02 00:01:01.1111111  
SELECT DATEADD(minute,59,@datetime2);     --2007-01-01 02:00:01.1111111  
SELECT DATEADD(second,59,@datetime2);     --2007-01-01 01:02:00.1111111  
SELECT DATEADD(millisecond,1,@datetime2); --2007-01-01 01:01:01.1121111  
```  
  
### <a name="c-using-expressions-as-arguments-for-the-number-and-date-parameters"></a>C. Utilisation d'expressions comme arguments pour les paramètres de date et numériques  
Ces exemples utilisent différents types d’expressions comme arguments pour les paramètres *number* et *date*. Ces exemples utilisent la base de données AdventureWorks.
  
#### <a name="specifying-a-column-as-date"></a>Spécification d'une colonne comme date  
Cet exemple ajoute `2` (deux) jours à chaque valeur dans la colonne `OrderDate` pour dériver une nouvelle colonne nommée `PromisedShipDate` :
  
```sql
SELECT SalesOrderID  
    ,OrderDate   
    ,DATEADD(day,2,OrderDate) AS PromisedShipDate  
FROM Sales.SalesOrderHeader;  
```  
  
Jeu de résultats partiel :
  
```
SalesOrderID OrderDate               PromisedShipDate  
------------ ----------------------- -----------------------  
43659        2005-07-01 00:00:00.000 2005-07-03 00:00:00.000  
43660        2005-07-01 00:00:00.000 2005-07-03 00:00:00.000  
43661        2005-07-01 00:00:00.000 2005-07-03 00:00:00.000  
...  
43702        2005-07-02 00:00:00.000 2005-07-04 00:00:00.000  
43703        2005-07-02 00:00:00.000 2005-07-04 00:00:00.000  
43704        2005-07-02 00:00:00.000 2005-07-04 00:00:00.000  
43705        2005-07-02 00:00:00.000 2005-07-04 00:00:00.000  
43706        2005-07-03 00:00:00.000 2005-07-05 00:00:00.000  
...  
43711        2005-07-04 00:00:00.000 2005-07-06 00:00:00.000  
43712        2005-07-04 00:00:00.000 2005-07-06 00:00:00.000  
...  
43740        2005-07-11 00:00:00.000 2005-07-13 00:00:00.000  
43741        2005-07-12 00:00:00.000 2005-07-14 00:00:00.000  
  
```  
  
#### <a name="specifying-user-defined-variables-as-number-and-date"></a>Spécification de variables définies par l'utilisateur comme nombre et date  
Cet exemple spécifie des variables définies par l’utilisateur comme arguments pour *number* et *date* :
  
```sql
DECLARE @days INT = 365,   
        @datetime DATETIME = '2000-01-01 01:01:01.111'; /* 2000 was a leap year */;  
SELECT DATEADD(day, @days, @datetime);  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
-----------------------  
2000-12-31 01:01:01.110  
  
(1 row(s) affected)  
```  
  
#### <a name="specifying-scalar-system-function-as-date"></a>Spécification de la fonction système scalaire comme date  
Cet exemple spécifie `SYSDATETIME` pour *date*. La valeur exacte retournée varie selon le jour et l’heure de l’exécution de l’instruction :
  
```sql
SELECT DATEADD(month, 1, SYSDATETIME());  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
---------------------------  
2013-02-06 14:29:59.6727944  
  
(1 row(s) affected)  
```  
  
#### <a name="specifying-scalar-subqueries-and-scalar-functions-as-number-and-date"></a>Spécification de sous-requêtes scalaires et de fonctions scalaires comme nombre et date  
Cet exemple utilise des sous-requêtes scalaires, `MAX(ModifiedDate)`, comme arguments pour *number* et *date*. `(SELECT TOP 1 BusinessEntityID FROM Person.Person)` sert d’argument artificiel pour le paramètre number pour illustrer la sélection d’un argument *number* dans une liste de valeurs.
  
```sql
SELECT DATEADD(month,(SELECT TOP 1 BusinessEntityID FROM Person.Person),  
    (SELECT MAX(ModifiedDate) FROM Person.Person));  
```  
  
#### <a name="specifying-numeric-expressions-and-scalar-system-functions-as-number-and-date"></a>Spécification d'expressions numériques et de fonctions système scalaires comme nombre et date  
Cet exemple utilise une expression numérique (-`(10/2))`, des [opérateurs unaires](../../mdx/unary-operators.md) (`-`), un [opérateur arithmétique](../../mdx/arithmetic-operators.md) (`/`) et des fonctions système scalaires (`SYSDATETIME`) comme arguments pour *number* et *date*.
  
```sql
SELECT DATEADD(month,-(10/2), SYSDATETIME());  
```  
  
#### <a name="specifying-ranking-functions-as-number"></a>Spécification de fonctions de classement comme nombre  
Cet exemple utilise une fonction de classement comme argument pour *number*.
  
```sql
SELECT p.FirstName, p.LastName  
    ,DATEADD(day,ROW_NUMBER() OVER (ORDER BY  
        a.PostalCode),SYSDATETIME()) AS 'Row Number'  
FROM Sales.SalesPerson AS s   
    INNER JOIN Person.Person AS p   
        ON s.BusinessEntityID = p.BusinessEntityID  
    INNER JOIN Person.Address AS a   
        ON a.AddressID = p.BusinessEntityID  
WHERE TerritoryID IS NOT NULL   
    AND SalesYTD <> 0;  
```  
  
#### <a name="specifying-an-aggregate-window-function-as-number"></a>Spécification d'une fonction d'agrégation comme nombre  
Cet exemple utilise une fonction d’agrégation comme argument pour *number*.
  
```sql
SELECT SalesOrderID, ProductID, OrderQty  
    ,DATEADD(day,SUM(OrderQty)   
        OVER(PARTITION BY SalesOrderID),SYSDATETIME()) AS 'Total'  
FROM Sales.SalesOrderDetail   
WHERE SalesOrderID IN(43659,43664);  
GO  
```  
  
  
## <a name="see-also"></a>Voir aussi
[CAST et CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)
  
  

