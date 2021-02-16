---
title: OVER, clause (Transact-SQL) | Microsoft Docs
description: Référence Transact-SQL pour la clause OVER qui définit un ensemble de lignes spécifié par l’utilisateur dans un jeu de résultats de requête.
ms.date: 08/11/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- OVER_TSQL
- OVER
dev_langs:
- TSQL
helpviewer_keywords:
- order of rowsets [SQL Server]
- rowsets [SQL Server], windowing
- window function
- partitions [SQL Server], rowsets
- clauses [SQL Server], OVER
- rowsets [SQL Server], partitioning
- rowsets [SQL Server], ordering
- OVER clause
ms.assetid: ddcef3a6-0341-43e0-ae73-630484b7b398
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 3b78f603517e6f754039d66572c88ea77b81098a
ms.sourcegitcommit: c83c17e44b5e1e3e2a3b5933c2a1c4afb98eb772
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/15/2021
ms.locfileid: "100525225"
---
# <a name="select---over-clause-transact-sql"></a>SELECT - Clause OVER (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Détermine le partitionnement et l'ordre d'un ensemble de lignes avant l'application de la fonction de fenêtre associée. Autrement dit, la clause OVER définit une fenêtre ou un ensemble de lignes spécifié par l'utilisateur dans un jeu de résultats de requête. Une fonction de fenêtre calcule ensuite une valeur pour chaque ligne dans la fenêtre. Vous pouvez utiliser la clause OVER avec des fonctions pour calculer des valeurs agrégées telles que les moyennes mobiles, les agrégats cumulatifs, des cumuls ou les N premières lignes par groupe de résultats.  
  
-   [Fonctions de classement](../../t-sql/functions/ranking-functions-transact-sql.md)  
  
-   [Fonctions d’agrégation](../../t-sql/functions/aggregate-functions-transact-sql.md)  
  
-   [Fonctions analytiques](../../t-sql/functions/analytic-functions-transact-sql.md)  
  
-   [Fonction NEXT VALUE FOR](../../t-sql/functions/next-value-for-transact-sql.md)  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
-- Syntax for SQL Server, Azure SQL Database, and Azure Synapse Analytics  
  
OVER (   
       [ <PARTITION BY clause> ]  
       [ <ORDER BY clause> ]   
       [ <ROW or RANGE clause> ]  
      )  
  
<PARTITION BY clause> ::=  
PARTITION BY value_expression , ... [ n ]  
  
<ORDER BY clause> ::=  
ORDER BY order_by_expression  
    [ COLLATE collation_name ]   
    [ ASC | DESC ]   
    [ ,...n ]  
  
<ROW or RANGE clause> ::=  
{ ROWS | RANGE } <window frame extent>  
  
<window frame extent> ::=   
{   <window frame preceding>  
  | <window frame between>  
}  
<window frame between> ::=   
  BETWEEN <window frame bound> AND <window frame bound>  
  
<window frame bound> ::=   
{   <window frame preceding>  
  | <window frame following>  
}  
  
<window frame preceding> ::=   
{  
    UNBOUNDED PRECEDING  
  | <unsigned_value_specification> PRECEDING  
  | CURRENT ROW  
}  
  
<window frame following> ::=   
{  
    UNBOUNDED FOLLOWING  
  | <unsigned_value_specification> FOLLOWING  
  | CURRENT ROW  
}  
  
<unsigned value specification> ::=   
{  <unsigned integer literal> }  
  
```  
  
```syntaxsql
-- Syntax for Parallel Data Warehouse  
  
OVER ( [ PARTITION BY value_expression ] [ order_by_clause ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments

Les fonctions de fenêtre peuvent avoir les arguments suivants dans leur clause `OVER` :
- [PARTITION BY](#partition-by) qui divise le jeu de résultats de la requête en partitions.
- [ORDER BY](#order-by) qui définit l’ordre logique des lignes dans chaque partition du jeu de résultats. 
- [ROWS/RANGE](#rows-or-range) qui limite davantage les lignes dans la partition en spécifiant les points de départ et de terminaison dans la partition. Elle requiert l’argument `ORDER BY` et la valeur par défaut est du début de la partition à l’élément actuel si l’argument `ORDER BY` est spécifié.

Si vous ne spécifiez pas d’argument, les fonctions de fenêtre seront appliquées à l’ensemble du jeu de résultats.
```sql
select 
      object_id
    , [min] = min(object_id) over()
    , [max] = max(object_id) over()
from sys.objects
```
 
|object_id | minute(s) | max |
|---|---|---|
| 3 | 3 | 2139154666 |
| 5 | 3 | 2139154666 |
| ... | ... | ... |
| 2123154609 |  3 | 2139154666 |
| 2139154666 |  3 | 2139154666 |

### <a name="partition-by"></a>PARTITION BY  
 Divise le jeu de résultats de la requête en partitions. La fonction de fenêtre est appliquée à chaque partition séparément et le calcul redémarre pour chaque partition.  

```syntaxsql
PARTITION BY *value_expression* 
```
 
 Si PARTITION BY n’est pas spécifié, la fonction gère toutes les lignes du jeu de résultats de la requête en une seule partition.
La fonction sera appliquée à toutes les lignes de la partition si vous ne spécifiez pas clause `ORDER BY`.
  
#### <a name="partition-by-value_expression"></a>PARTITION BY *value_expression*  
 Spécifie la colonne par laquelle l'ensemble de lignes est partitionné. *value_expression* peut uniquement référencer des colonnes mises à disposition par la clause FROM. *value_expression* ne peut pas référencer des expressions ou des alias dans la liste de sélection. *value_expression* peut être une expression de colonne, une sous-requête scalaire, une fonction scalaire ou une variable définie par l’utilisateur. 
 
 ```sql
 select 
      object_id, type
    , [min] = min(object_id) over(partition by type)
    , [max] = max(object_id) over(partition by type)
from sys.objects
```

|object_id | type | minute(s) | max |
|---|---|---|---|
| 68195293  | PK    | 68195293  | 711673583 |
| 631673298 | PK    | 68195293  | 711673583 |
| 711673583 | PK    | 68195293  | 711673583 |
| ... | ... | ... |
| 3 | S | 3 | 98 |
| 5 | S |   3   | 98 |
| ... | ... | ... |
| 98    | S |   3   | 98 |
| ... | ... | ... |
  
### <a name="order-by"></a>ORDER BY  

```syntaxsql
ORDER BY *order_by_expression* [COLLATE *collation_name*] [ASC|DESC]  
```

 Définit l'ordre logique des lignes dans chaque partition du jeu de résultats. Autrement dit, il spécifie l’ordre logique dans lequel le calcul de la fonction de la fenêtre est effectué. 
 - Si elle n’est pas spécifiée, l’ordre par défaut est `ASC` et la fonction de fenêtre utilise toutes les lignes de la partition.
 - S’il est spécifié mais pas ROWS/RANGE, la valeur `RANGE UNBOUNDED PRECEDING AND CURRENT ROW` est utilisée par défaut pour le cadre de fenêtre par les fonctions qui peuvent accepter une spécification ROWS/RANGE facultative (par exemple `min` ou `max`). 
 
```sql
select 
      object_id, type
    , [min] = min(object_id) over(partition by type order by object_id)
    , [max] = max(object_id) over(partition by type order by object_id)
from sys.objects
```

|object_id | type | minute(s) | max |
|---|---|---|---|
| 68195293  | PK    | 68195293  | 68195293 |
| 631673298 | PK    | 68195293  | 631673298 |
| 711673583 | PK    | 68195293  | 711673583 |
| ... | ... | ... |
| 3 | S | 3 | 3 |
| 5 | S |   3 | 5 |
| 6 | S |   3 | 6 |
| ... | ... | ... |
| 97    | S |   3 | 97 |
| 98    | S |   3 | 98 |
| ... | ... | ... |


 *order_by_expression*  
 Spécifie une colonne ou une expression dans lesquelles trier. *order_by_expression* peut uniquement référencer des colonnes mises à disposition par la clause FROM. Un entier ne peut pas être spécifié pour représenter un nom de colonne ou un alias.  
  
 COLLATE *collation_name*  
 Spécifie que l’opération ORDER BY doit être exécutée selon le classement spécifié dans *collation_name*. *collation_name* peut être un nom de classement Windows ou SQL. Pour plus d’informations, consultez [Prise en charge d’Unicode et du classement](../../relational-databases/collations/collation-and-unicode-support.md). COLLATE est applicable uniquement aux colonnes de types **char**, **varchar**, **nchar** et **nvarchar**.  
  
 **ASC** | DESC  
 Spécifie que les valeurs dans la colonne spécifiée doivent être triées par ordre croissant ou décroissant. ASC correspond à l'ordre de tri par défaut. Les valeurs NULL sont traitées comme les plus petites valeurs possibles.  
  
### <a name="rows-or-range"></a>ROWS ou RANGE  
**S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et versions ultérieures. 
  
 Limite davantage les lignes dans la partition en spécifiant les points de départ et de terminaison dans la partition. Cette opération s'effectue en spécifiant une plage de lignes par rapport à la ligne actuelle par association logique ou association physique. L'association physique est réalisée en utilisant la clause ROWS.  
  
 La clause ROWS limite les lignes dans une partition en spécifiant un nombre fixe de lignes précédant ou suivant la ligne actuelle. Également, la clause RANGE limite logiquement les lignes dans une partition en spécifiant une plage de valeurs par rapport à la valeur de la ligne actuelle. Les lignes précédentes et suivantes sont définies en fonction de l'organisation dans la clause ORDER BY. Le frame de fenêtre « RANGE ... CURRENT ROW ... » inclut toutes les lignes qui ont les mêmes valeurs dans l’expression ORDER BY que la ligne actuelle. Par exemple, ROWS BETWEEN 2 PRECEDING AND CURRENT ROW signifie que la fenêtre de lignes traitées par la fonction comprend trois lignes, en commençant par les deux lignes qui précèdent la ligne actuelle (ligne actuelle comprise).  
 
```sql
select
      object_id
    , [preceding]   = count(*) over(order by object_id ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW )
    , [central] = count(*) over(order by object_id ROWS BETWEEN 2 PRECEDING AND 2 FOLLOWING )
    , [following]   = count(*) over(order by object_id ROWS BETWEEN CURRENT ROW AND UNBOUNDED FOLLOWING)
from sys.objects
order by object_id asc
```

|object_id | précédent | central | suivant |
|---|---|---|---|
| 3 | 1 | 3 | 156 |
| 5 | 2 | 4 | 155 |
| 6 | 3 | 5 | 154 |
| 7 | 4 | 5 | 153 |
| 8 | 5 | 5 | 152 |
| ...   | ...   | ...   | ... |
| 2112726579    | 153   | 5 | 4 |
| 2119678599    | 154   | 5 | 3 |
| 2123154609    | 155   | 4 | 2 |
| 2139154666    | 156   | 3 | 1 | 
  
> [!NOTE]  
>  ROWS ou RANGE requièrent que la clause ORDER BY soit spécifiée. Si ORDER BY contient plusieurs expressions d'ordre, CURRENT ROW FOR RANGE prend en compte toutes les colonnes dans la liste ORDER BY lors de la détermination de la ligne actuelle.  
  
#### <a name="unbounded-preceding"></a>UNBOUNDED PRECEDING  
**S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et versions ultérieures.  
  
 Spécifie que la fenêtre commence à la première ligne de la partition. UNBOUNDED PRECEDING peut être spécifié comme point de départ de la fenêtre.  
  
 \<unsigned value specification> PRECEDING  
 Spécifié avec \<unsigned value specification> pour indiquer le nombre de lignes ou de valeurs qui précèdent la ligne actuelle. Cette spécification n'est pas autorisée pour RANGE.  
  
#### <a name="current-row"></a>CURRENT ROW  
**S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et versions ultérieures. 
  
 Spécifie que la fenêtre commence ou se termine à la ligne actuelle en cas d'utilisation avec ROWS ou à la valeur actuelle en cas de utilisation avec RANGE. CURRENT ROW peut être spécifié comme point de départ et de fin.  
  
#### <a name="between-and"></a>BETWEEN AND  
**S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et versions ultérieures. 
  
```syntaxsql
BETWEEN <window frame bound > AND <window frame bound >  
```
 Utilisé avec ROWS ou RANGE pour spécifier les points limite inférieurs (départ) et supérieurs (fin) de la fenêtre. \<window frame bound> définit le point de départ limite et \<window frame bound> définit le point de fin limite. La limite supérieure ne peut pas être inférieure à la limite inférieure.  
  
#### <a name="unbounded-following"></a>UNBOUNDED FOLLOWING  
**S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et versions ultérieures. 
  
 Spécifie que la fenêtre se termine à la dernière ligne de la partition. FOLLOWING UNBOUNDED peut être spécifié comme point de fin de fenêtre. Par exemple RANGE BETWEEN CURRENT ROW AND UNBOUNDED FOLLOWING définit une fenêtre qui commence par la ligne actuelle et se termine à la dernière ligne de la partition.  
  
 \<unsigned value specification> FOLLOWING  
 Spécifié avec \<unsigned value specification> pour indiquer le nombre de lignes ou de valeurs qui suivent la ligne actuelle. Lorsque \<unsigned value specification> FOLLOWING est spécifié comme point de départ de la fenêtre, le point de fin doit être \<unsigned value specification> FOLLOWING. Par exemple, ROWS BETWEEN 2 FOLLOWING AND 10 FOLLOWING définit une fenêtre qui commence avec la deuxième ligne qui suit la ligne actuelle et se termine par la dixième ligne qui suit la ligne actuelle. Cette spécification n'est pas autorisée pour RANGE.  
  
 littéral entier non signé  
**S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et versions ultérieures.  
  
 Est un littéral entier positif (comprenant 0) qui spécifie le nombre de lignes ou de valeurs qui précèdent ou suivent la ligne ou la valeur actuelle. Cette condition est valide uniquement pour ROWS.  
  
## <a name="general-remarks"></a>Remarques d'ordre général  
 Vous pouvez utiliser plusieurs fonctions de fenêtre dans une seule requête avec une seule clause FROM. La clause OVER de chaque fonction peut être différente en termes de partitionnement et de tri.  
  
 Si PARTITION BY n'est pas spécifié, la fonction gère toutes les lignes du jeu de résultats de la requête en un seul groupe. 
 
### <a name="important"></a>Important !

Si ROWS/RANGE est spécifié et que \<window frame preceding> est utilisé pour \<window frame extent> (syntaxe courte), cette spécification est utilisée comme point de départ limite et CURRENT ROW comme point de fin limite du frame de la fenêtre. Par exemple « ROWS 5 PRECEDING » est égal à « ROWS BETWEEN 5 PRECEDING AND CURRENT ROW ».  
  
> [!NOTE]
> Si ORDER BY n'est pas spécifié, la partition entière est utilisée pour un cadre de fenêtre. Cela s'applique uniquement aux fonctions qui ne nécessitent pas la clause ORDER BY. Si ROWS/RANGE n'est pas spécifié mais ORDER BY est spécifié, RANGE UNBOUNDED PRECEDING AND CURRENT ROW est utilisé comme valeur par défaut pour le cadre de fenêtre. Cela s'applique uniquement aux fonctions qui acceptent la spécification facultative ROWS/RANGE. Par exemple, les fonctions de classement n'acceptent pas ROWS/RANGE, par conséquent ce cadre de fenêtre n'est pas appliqué même si ORDER BY est présent et ROWS/RANGE ne l'est pas.  
    
## <a name="limitations-and-restrictions"></a>Limitations et restrictions  
 La clause OVER ne peut pas être utilisée avec la fonction d'agrégation CHECKSUM.  
  
 RANGE ne peut pas être utilisée avec \<unsigned value specification> PRECEDING ou \<unsigned value specification> FOLLOWING.  
  
 Selon la fonction de classement, d’agrégation ou analytique utilisée avec la clause OVER, \<ORDER BY clause> et/ou \<ROWS and RANGE clause> peuvent ne pas être pris en charge.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-using-the-over-clause-with-the-row_number-function"></a>R. Utilisation de la clause OVER avec la fonction ROW_NUMBER  
 L'exemple suivant montre comment utiliser la clause OVER avec la fonction ROW_NUMBER pour afficher un numéro de ligne pour chaque ligne dans une partition. La clause ORDER BY spécifiée dans la clause OVER trie les lignes dans chaque partition par la colonne `SalesYTD`. La clause ORDER BY dans l'instruction SELECT détermine l'ordre dans lequel la totalité du jeu de résultats de la requête est retournée.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT ROW_NUMBER() OVER(PARTITION BY PostalCode ORDER BY SalesYTD DESC) AS "Row Number",   
    p.LastName, s.SalesYTD, a.PostalCode  
FROM Sales.SalesPerson AS s   
    INNER JOIN Person.Person AS p   
        ON s.BusinessEntityID = p.BusinessEntityID  
    INNER JOIN Person.Address AS a   
        ON a.AddressID = p.BusinessEntityID  
WHERE TerritoryID IS NOT NULL   
    AND SalesYTD <> 0  
ORDER BY PostalCode;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
 Row Number      LastName                SalesYTD              PostalCode 
 --------------- ----------------------- --------------------- ---------- 
 1               Mitchell                4251368.5497          98027 
 2               Blythe                  3763178.1787          98027 
 3               Carson                  3189418.3662          98027 
 4               Reiter                  2315185.611           98027 
 5               Vargas                  1453719.4653          98027  
 6               Ansman-Wolfe            1352577.1325          98027  
 1               Pak                     4116871.2277          98055  
 2               Varkey Chudukatil       3121616.3202          98055  
 3               Saraiva                 2604540.7172          98055  
 4               Ito                     2458535.6169          98055  
 5               Valdez                  1827066.7118          98055  
 6               Mensa-Annan             1576562.1966          98055  
 7               Campbell                1573012.9383          98055  
 8               Tsoflias                1421810.9242          98055
 ```  
  
### <a name="b-using-the-over-clause-with-aggregate-functions"></a>B. Utilisation de la clause OVER avec des fonctions d'agrégation  
 L'exemple suivant utilise la clause `OVER` avec des fonctions d'agrégation sur toutes les lignes retournées par la requête. Dans cet exemple, l'utilisation de la clause `OVER` est plus efficace que l'utilisation de sous-requêtes pour dériver les valeurs d'agrégation.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT SalesOrderID, ProductID, OrderQty  
    ,SUM(OrderQty) OVER(PARTITION BY SalesOrderID) AS Total  
    ,AVG(OrderQty) OVER(PARTITION BY SalesOrderID) AS "Avg"  
    ,COUNT(OrderQty) OVER(PARTITION BY SalesOrderID) AS "Count"  
    ,MIN(OrderQty) OVER(PARTITION BY SalesOrderID) AS "Min"  
    ,MAX(OrderQty) OVER(PARTITION BY SalesOrderID) AS "Max"  
FROM Sales.SalesOrderDetail   
WHERE SalesOrderID IN(43659,43664);  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
SalesOrderID ProductID   OrderQty Total       Avg         Count       Min    Max  
------------ ----------- -------- ----------- ----------- ----------- ------ ------  
43659        776         1        26          2           12          1      6  
43659        777         3        26          2           12          1      6  
43659        778         1        26          2           12          1      6  
43659        771         1        26          2           12          1      6  
43659        772         1        26          2           12          1      6  
43659        773         2        26          2           12          1      6  
43659        774         1        26          2           12          1      6  
43659        714         3        26          2           12          1      6  
43659        716         1        26          2           12          1      6  
43659        709         6        26          2           12          1      6  
43659        712         2        26          2           12          1      6  
43659        711         4        26          2           12          1      6  
43664        772         1        14          1           8           1      4  
43664        775         4        14          1           8           1      4  
43664        714         1        14          1           8           1      4  
43664        716         1        14          1           8           1      4  
43664        777         2        14          1           8           1      4  
43664        771         3        14          1           8           1      4  
43664        773         1        14          1           8           1      4  
43664        778         1        14          1           8           1      4  
```  
  
 L'exemple suivant illustre l'utilisation de la clause `OVER` avec une fonction d'agrégation dans une valeur calculée.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT SalesOrderID, ProductID, OrderQty  
    ,SUM(OrderQty) OVER(PARTITION BY SalesOrderID) AS Total  
    ,CAST(1. * OrderQty / SUM(OrderQty) OVER(PARTITION BY SalesOrderID)   
        *100 AS DECIMAL(5,2))AS "Percent by ProductID"  
FROM Sales.SalesOrderDetail   
WHERE SalesOrderID IN(43659,43664);  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)] Notez que les agrégations sont calculées par `SalesOrderID` et que le pourcentage `Percent by ProductID` est calculé pour chaque ligne de `SalesOrderID`.  
  
```  
SalesOrderID ProductID   OrderQty Total       Percent by ProductID  
------------ ----------- -------- ----------- ---------------------------------------  
43659        776         1        26          3.85  
43659        777         3        26          11.54  
43659        778         1        26          3.85  
43659        771         1        26          3.85  
43659        772         1        26          3.85  
43659        773         2        26          7.69  
43659        774         1        26          3.85  
43659        714         3        26          11.54  
43659        716         1        26          3.85  
43659        709         6        26          23.08  
43659        712         2        26          7.69  
43659        711         4        26          15.38  
43664        772         1        14          7.14  
43664        775         4        14          28.57  
43664        714         1        14          7.14  
43664        716         1        14          7.14  
43664        777         2        14          14.29  
43664        771         3        14          21.4  
43664        773         1        14          7.14  
43664        778         1        14          7.14  
  
 (20 row(s) affected)  
```  
  
### <a name="c-producing-a-moving-average-and-cumulative-total"></a>C. Production d'une moyenne mobile et d'un total cumulé  
 L'exemple suivant utilise les fonctions AVG et SUM avec la clause OVER pour fournir une moyenne mobile et un total cumulé des ventes annuelles pour chaque secteur dans la table `Sales.SalesPerson`. Les données sont partitionnées par `TerritoryID` et classées logiquement par `SalesYTD`. Cela signifie que la fonction AVG est calculée pour chaque secteur selon l'année de vente. Notez que pour `TerritoryID` 1, il y a deux lignes pour l'année 2005 représentant les deux vendeurs avec leurs ventes de l'année. Les ventes moyennes pour ces deux lignes sont calculées et la troisième ligne représentant les ventes de l'année 2006 est incluse dans le calcul.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT BusinessEntityID, TerritoryID   
   ,DATEPART(yy,ModifiedDate) AS SalesYear  
   ,CONVERT(VARCHAR(20),SalesYTD,1) AS  SalesYTD  
   ,CONVERT(VARCHAR(20),AVG(SalesYTD) OVER (PARTITION BY TerritoryID   
                                            ORDER BY DATEPART(yy,ModifiedDate)   
                                           ),1) AS MovingAvg  
   ,CONVERT(VARCHAR(20),SUM(SalesYTD) OVER (PARTITION BY TerritoryID   
                                            ORDER BY DATEPART(yy,ModifiedDate)   
                                            ),1) AS CumulativeTotal  
FROM Sales.SalesPerson  
WHERE TerritoryID IS NULL OR TerritoryID < 5  
ORDER BY TerritoryID,SalesYear;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
BusinessEntityID TerritoryID SalesYear   SalesYTD             MovingAvg            CumulativeTotal  
---------------- ----------- ----------- -------------------- -------------------- --------------------  
274              NULL        2005        559,697.56           559,697.56           559,697.56  
287              NULL        2006        519,905.93           539,801.75           1,079,603.50  
285              NULL        2007        172,524.45           417,375.98           1,252,127.95  
283              1           2005        1,573,012.94         1,462,795.04         2,925,590.07  
280              1           2005        1,352,577.13         1,462,795.04         2,925,590.07  
284              1           2006        1,576,562.20         1,500,717.42         4,502,152.27  
275              2           2005        3,763,178.18         3,763,178.18         3,763,178.18  
277              3           2005        3,189,418.37         3,189,418.37         3,189,418.37  
276              4           2005        4,251,368.55         3,354,952.08         6,709,904.17  
281              4           2005        2,458,535.62         3,354,952.08         6,709,904.17  
  
(10 row(s) affected)  
  
```  
  
 Dans cet exemple, la clause OVER n'inclut pas PARTITION BY. Cela signifie que la fonction sera appliquée à toutes les lignes retournées par la requête. La clause ORDER BY spécifiée dans la clause OVER détermine l'ordre logique selon lequel la fonction AVG est appliquée. La requête retourne une moyenne mobile des ventes par année pour tous les secteurs de vente spécifiés dans la clause WHERE. La clause ORDER BY spécifiée dans l'instruction SELECT détermine l'ordre dans lequel les lignes de la requête sont affichées.  
  
```sql  
SELECT BusinessEntityID, TerritoryID   
   ,DATEPART(yy,ModifiedDate) AS SalesYear  
   ,CONVERT(VARCHAR(20),SalesYTD,1) AS SalesYTD  
   ,CONVERT(VARCHAR(20),AVG(SalesYTD) OVER (ORDER BY DATEPART(yy,ModifiedDate)   
                                            ),1) AS MovingAvg  
   ,CONVERT(VARCHAR(20),SUM(SalesYTD) OVER (ORDER BY DATEPART(yy,ModifiedDate)   
                                            ),1) AS CumulativeTotal  
FROM Sales.SalesPerson  
WHERE TerritoryID IS NULL OR TerritoryID < 5  
ORDER BY SalesYear;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
BusinessEntityID TerritoryID SalesYear   SalesYTD             MovingAvg            CumulativeTotal  
---------------- ----------- ----------- -------------------- -------------------- --------------------  
274              NULL        2005        559,697.56           2,449,684.05         17,147,788.35  
275              2           2005        3,763,178.18         2,449,684.05         17,147,788.35  
276              4           2005        4,251,368.55         2,449,684.05         17,147,788.35  
277              3           2005        3,189,418.37         2,449,684.05         17,147,788.35  
280              1           2005        1,352,577.13         2,449,684.05         17,147,788.35  
281              4           2005        2,458,535.62         2,449,684.05         17,147,788.35  
283              1           2005        1,573,012.94         2,449,684.05         17,147,788.35  
284              1           2006        1,576,562.20         2,138,250.72         19,244,256.47  
287              NULL        2006        519,905.93           2,138,250.72         19,244,256.47  
285              NULL        2007        172,524.45           1,941,678.09         19,416,780.93  
(10 row(s) affected)  
```  
  
### <a name="d-specifying-the-rows-clause"></a>D. Spécification de la clause ROWS  
  
**S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et versions ultérieures.  
  
 L’exemple suivant utilise la clause ROWS pour définir une fenêtre de calcul des lignes comprenant la ligne actuelle et les *N* lignes qui suivent (une seule ligne dans cet exemple).  
  
```sql  
SELECT BusinessEntityID, TerritoryID   
    ,CONVERT(VARCHAR(20),SalesYTD,1) AS SalesYTD  
    ,DATEPART(yy,ModifiedDate) AS SalesYear  
    ,CONVERT(VARCHAR(20),SUM(SalesYTD) OVER (PARTITION BY TerritoryID   
                                             ORDER BY DATEPART(yy,ModifiedDate)   
                                             ROWS BETWEEN CURRENT ROW AND 1 FOLLOWING ),1) AS CumulativeTotal  
FROM Sales.SalesPerson  
WHERE TerritoryID IS NULL OR TerritoryID < 5;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
BusinessEntityID TerritoryID SalesYTD             SalesYear   CumulativeTotal  
---------------- ----------- -------------------- ----------- --------------------  
274              NULL        559,697.56           2005        1,079,603.50  
287              NULL        519,905.93           2006        692,430.38  
285              NULL        172,524.45           2007        172,524.45  
283              1           1,573,012.94         2005        2,925,590.07  
280              1           1,352,577.13         2005        2,929,139.33  
284              1           1,576,562.20         2006        1,576,562.20  
275              2           3,763,178.18         2005        3,763,178.18  
277              3           3,189,418.37         2005        3,189,418.37  
276              4           4,251,368.55         2005        6,709,904.17  
281              4           2,458,535.62         2005        2,458,535.62  
```  
  
 Dans l'exemple suivant, la clause ROWS est spécifiée avec UNBOUNDED PRECEDING. Le résultat est que la fenêtre commence à la première ligne de la partition.  
  
```sql  
SELECT BusinessEntityID, TerritoryID   
    ,CONVERT(VARCHAR(20),SalesYTD,1) AS SalesYTD  
    ,DATEPART(yy,ModifiedDate) AS SalesYear  
    ,CONVERT(VARCHAR(20),SUM(SalesYTD) OVER (PARTITION BY TerritoryID   
                                             ORDER BY DATEPART(yy,ModifiedDate)   
                                             ROWS UNBOUNDED PRECEDING),1) AS CumulativeTotal  
FROM Sales.SalesPerson  
WHERE TerritoryID IS NULL OR TerritoryID < 5;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
BusinessEntityID TerritoryID SalesYTD             SalesYear   CumulativeTotal  
---------------- ----------- -------------------- ----------- --------------------  
274              NULL        559,697.56           2005        559,697.56  
287              NULL        519,905.93           2006        1,079,603.50  
285              NULL        172,524.45           2007        1,252,127.95  
283              1           1,573,012.94         2005        1,573,012.94  
280              1           1,352,577.13         2005        2,925,590.07  
284              1           1,576,562.20         2006        4,502,152.27  
275              2           3,763,178.18         2005        3,763,178.18  
277              3           3,189,418.37         2005        3,189,418.37  
276              4           4,251,368.55         2005        4,251,368.55  
281              4           2,458,535.62         2005        6,709,904.17  
  
```  
  
## <a name="examples-sspdw"></a>Exemples : [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="e-using-the-over-clause-with-the-row_number-function"></a>E. Utilisation de la clause OVER avec la fonction ROW_NUMBER  
 L’exemple suivant retourne le ROW_NUMBER des représentants commerciaux en fonction de leur quota de ventes assigné.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT ROW_NUMBER() OVER(ORDER BY SUM(SalesAmountQuota) DESC) AS RowNumber,  
    FirstName, LastName,   
CONVERT(VARCHAR(13), SUM(SalesAmountQuota),1) AS SalesQuota   
FROM dbo.DimEmployee AS e  
INNER JOIN dbo.FactSalesQuota AS sq  
    ON e.EmployeeKey = sq.EmployeeKey  
WHERE e.SalesPersonFlag = 1  
GROUP BY LastName, FirstName;  
```  
  
 Voici un jeu de résultats partiel.  
  
 ```
 RowNumber  FirstName  LastName            SalesQuota  
 ---------  ---------  ------------------  -------------  
 1          Jillian    Carson              12,198,000.00  
 2          Linda      Mitchell            11,786,000.00  
 3          Michael    Blythe              11,162,000.00  
 4          Jae        Pak                 10,514,000.00  
 ```
 
### <a name="f-using-the-over-clause-with-aggregate-functions"></a>F. Utilisation de la clause OVER avec des fonctions d'agrégation  
 Les exemples ci-dessous illustrent l’utilisation de la clause OVER avec des fonctions d’agrégation. Dans cet exemple, l’utilisation de la clause OVER s’avère plus efficace que d’utiliser des sous-requêtes.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT SalesOrderNumber AS OrderNumber, ProductKey,   
       OrderQuantity AS Qty,   
       SUM(OrderQuantity) OVER(PARTITION BY SalesOrderNumber) AS Total,  
       AVG(OrderQuantity) OVER(PARTITION BY SalesOrderNumber) AS Avg,  
       COUNT(OrderQuantity) OVER(PARTITION BY SalesOrderNumber) AS Count,  
       MIN(OrderQuantity) OVER(PARTITION BY SalesOrderNumber) AS Min,  
       MAX(OrderQuantity) OVER(PARTITION BY SalesOrderNumber) AS Max  
FROM dbo.FactResellerSales   
WHERE SalesOrderNumber IN(N'SO43659',N'SO43664') AND  
      ProductKey LIKE '2%'  
ORDER BY SalesOrderNumber,ProductKey;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
 OrderNumber  Product  Qty  Total  Avg  Count  Min  Max  
 -----------  -------  ---  -----  ---  -----  ---  ---  
 SO43659      218      6    16     3    5      1    6  
 SO43659      220      4    16     3    5      1    6  
 SO43659      223      2    16     3    5      1    6  
 SO43659      229      3    16     3    5      1    6  
 SO43659      235      1    16     3    5      1    6  
 SO43664      229      1     2     1    2      1    1  
 SO43664      235      1     2     1    2      1    1  
 ```
 
 L’exemple suivant illustre l’utilisation de la clause OVER avec une fonction d’agrégation dans une valeur calculée. Notez que les agrégations sont calculées par `SalesOrderNumber` et que le pourcentage des commandes totales est calculé pour chaque ligne de chaque `SalesOrderNumber`.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT SalesOrderNumber AS OrderNumber, ProductKey AS Product,   
       OrderQuantity AS Qty,   
       SUM(OrderQuantity) OVER(PARTITION BY SalesOrderNumber) AS Total,  
       CAST(1. * OrderQuantity / SUM(OrderQuantity)   
        OVER(PARTITION BY SalesOrderNumber)   
            *100 AS DECIMAL(5,2)) AS PctByProduct  
FROM dbo.FactResellerSales   
WHERE SalesOrderNumber IN(N'SO43659',N'SO43664') AND  
      ProductKey LIKE '2%'  
ORDER BY SalesOrderNumber,ProductKey;  
```  
  
 Le début de ce jeu de résultats est le suivant :  
  
 ```
 OrderNumber  Product  Qty  Total  PctByProduct  
 -----------  -------  ---  -----  ------------  
 SO43659      218      6    16     37.50  
 SO43659      220      4    16     25.00  
 SO43659      223      2    16     12.50  
 SO43659      229      2    16     18.75  
 ```
 
## <a name="see-also"></a>Voir aussi  
 [Fonctions d’agrégation &#40;Transact-SQL&#41;](../../t-sql/functions/aggregate-functions-transact-sql.md)   
 [Fonctions analytiques &#40;Transact-SQL&#41;](../../t-sql/functions/analytic-functions-transact-sql.md)   
 [Excellent billet de blog sur les fonctions de fenêtre et OVER publié sur sqlmag.com par Itzik Ben-Gan](https://www.itprotoday.com/sql-server/how-use-microsoft-sql-server-2012s-window-functions-part-1)  
  
  
