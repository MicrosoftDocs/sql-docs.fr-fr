---
description: SELECT - GROUP BY- Transact-SQL
title: GROUP BY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- GROUP
- CUBE
- ROLLUP
- GROUPING_SETS_TSQL
- GROUP BY
- GROUPING SETS
- GROUP_BY_TSQL
- GROUP_TSQL
- CUBE_TSQL
- ROLLUP_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- GROUP BY clause, about GROUP BY clause
- dividing tables into groups
- GROUPING SETS
- GROUP BY clause
- table groups [SQL Server]
- groups [SQL Server], tables divided into groups
- summary values [SQL Server]
ms.assetid: 40075914-6385-4692-b4a5-62fe44ae6cb6
author: shkale-msft
ms.author: shkale
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: f4bbf0df92834b3bec1e7314edb8dfba305f734d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207576"
---
# <a name="select---group-by--transact-sql"></a>SELECT - GROUP BY- Transact-SQL
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Clause de l’instruction SELECT qui scinde le résultat de la requête en groupes de lignes, généralement pour effectuer ensuite une ou plusieurs agrégations sur chaque groupe. L’instruction SELECT retourne une ligne par groupe.  
  
## <a name="syntax"></a>Syntaxe  

 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL &#40;Transact-SQL&#41;](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database   
-- ISO-Compliant Syntax  
  
GROUP BY {
      column-expression  
    | ROLLUP ( <group_by_expression> [ ,...n ] )  
    | CUBE ( <group_by_expression> [ ,...n ] )  
    | GROUPING SETS ( <grouping_set> [ ,...n ]  )  
    | () --calculates the grand total 
} [ ,...n ] 
 
<group_by_expression> ::=  
      column-expression  
    | ( column-expression [ ,...n ] )    
   
<grouping_set> ::=  
      () --calculates the grand total  
    | <grouping_set_item>  
    | ( <grouping_set_item> [ ,...n ] )  
  
<grouping_set_item> ::=  
      <group_by_expression>  
    | ROLLUP ( <group_by_expression> [ ,...n ] )  
    | CUBE ( <group_by_expression> [ ,...n ] )  
  

-- For backward compatibility only.
-- Non-ISO-Compliant Syntax for SQL Server and Azure SQL Database 
  
GROUP BY 
      [ ALL ] column-expression [ ,...n ] 
    | column-expression [ ,...n ] [ WITH { CUBE | ROLLUP } ]   

```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics 
  
GROUP BY {
      column-name [ WITH (DISTRIBUTED_AGG) ]  
    | column-expression
    | ROLLUP ( <group_by_expression> [ ,...n ] ) 
} [ ,...n ]

```

```syntaxsql
-- Syntax for Parallel Data Warehouse  
  
GROUP BY {
      column-name [ WITH (DISTRIBUTED_AGG) ]  
    | column-expression
} [ ,...n ]

```  
    
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 
### <a name="column-expression"></a>*column-expression*  
Spécifie une colonne ou un calcul non agrégé sur une colonne. Il peut s’agir d’une colonne d’une table, d’une table dérivée ou d’une vue. La colonne doit figurer dans la clause FROM de l’instruction SELECT, mais elle n’est pas obligatoire dans la liste SELECT. 

Pour connaître les expressions valides, consultez [expression](~/t-sql/language-elements/expressions-transact-sql.md).    

La colonne doit figurer dans la clause FROM de l’instruction SELECT, mais elle n’est pas obligatoire dans la liste SELECT. Toutefois, chaque colonne de table ou de vue dans une expression non agrégée de la liste \<select> doit être dans la liste GROUP BY :  
  
Les instructions suivantes sont autorisées :  
  
```sql  
SELECT ColumnA, ColumnB FROM T GROUP BY ColumnA, ColumnB;  
SELECT ColumnA + ColumnB FROM T GROUP BY ColumnA, ColumnB;  
SELECT ColumnA + ColumnB FROM T GROUP BY ColumnA + ColumnB;  
SELECT ColumnA + ColumnB + constant FROM T GROUP BY ColumnA, ColumnB;  
```  
  
Les instructions suivantes ne sont pas autorisées :  
  
```sql  
SELECT ColumnA, ColumnB FROM T GROUP BY ColumnA + ColumnB;  
SELECT ColumnA + constant + ColumnB FROM T GROUP BY ColumnA + ColumnB;  
```  

L’expression de colonne ne peut pas contenir les éléments suivants :

- Un alias de colonne qui est défini dans la liste SELECT. Vous pouvez utiliser un alias de colonne pour une table dérivée qui est définie dans la clause FROM.
- Une colonne de type **text**, **ntext** ou **image**. Toutefois, vous pouvez utiliser une colonne text, ntext ou image comme argument d’une fonction qui retourne une valeur d’un type de données valide. Par exemple, l’expression peut utiliser SUBSTRING() et CAST(). Cela s’applique aussi aux expressions définies dans la clause HAVING.
- Des méthodes ayant le type de données xml. L’expression peut inclure une fonction définie par l’utilisateur qui utilise des méthodes de type de données xml. Elle peut inclure une colonne calculée qui utilise des méthodes de ce type. 
- Une sous-requête. L’erreur 144 est retournée. 
- Une colonne d’une vue indexée. 
 
### <a name="group-by-column-expression--n-"></a>GROUP BY *column-expression* [ ,...n ] 

Regroupe les résultats de l’instruction SELECT en fonction des valeurs dans une liste contenant une ou plusieurs expressions de colonne. 

Par exemple, cette requête crée une table Sales avec les colonnes Country, Region et Sales. Elle insère quatre lignes, dont deux ont des valeurs identiques dans les colonnes Country et Region.  

```sql
CREATE TABLE Sales ( Country VARCHAR(50), Region VARCHAR(50), Sales INT );

INSERT INTO sales VALUES (N'Canada', N'Alberta', 100);
INSERT INTO sales VALUES (N'Canada', N'British Columbia', 200);
INSERT INTO sales VALUES (N'Canada', N'British Columbia', 300);
INSERT INTO sales VALUES (N'United States', N'Montana', 100);
```
La table Sales contient les lignes suivantes :

| Country | Region | Ventes |
|---------|--------|-------|
| Canada | Alberta | 100 |
| Canada | British Columbia | 200 |
| Canada | British Columbia | 300 |
| États-Unis | Montana | 100 |

La requête suivante regroupe les résultats par pays (colonne Country) et par région (colonne Region), et retourne la somme agrégée de chaque combinaison de valeurs.  
 
```sql 
SELECT Country, Region, SUM(sales) AS TotalSales
FROM Sales
GROUP BY Country, Region;
```
La requête retourne trois lignes de résultats, car il y a trois combinaisons de valeurs pour les colonnes Country et Region. Le montant total des ventes (TotalSales) pour les pays Canada et British Columbia correspond à la somme de deux lignes. 

| Country | Région | TotalSales |
|---------|--------|-------|
| Canada | Alberta | 100 |
| Canada | British Columbia | 500 |
| États-Unis | Montana | 100 |

### <a name="group-by-rollup"></a>GROUP BY ROLLUP

Crée un groupe pour chaque combinaison d’expressions de colonne. De plus, l’instruction regroupe les résultats en sous-totaux et en totaux globaux. Pour cela, elle va de droite à gauche en diminuant le nombre d’expressions de colonne sur lesquelles elle crée des groupes et chaque agrégation. 

L'ordre des colonnes affecte la sortie de ROLLUP et peut affecter le nombre de lignes du jeu de résultats.  

Par exemple, `GROUP BY ROLLUP (col1, col2, col3, col4)` crée des groupes pour chaque combinaison d’expressions de colonne dans les listes suivantes.  

- col1, col2, col3, col4 
- col1, col2, col3, NULL
- col1, col2, NULL, NULL
- col1, NULL, NULL, NULL
- NULL, NULL, NULL, NULL (total global)

En utilisant la table de l’exemple précédent, ce code exécute une opération GROUP BY ROLLUP au lieu d’une opération GROUP BY simple.

```sql
SELECT Country, Region, SUM(Sales) AS TotalSales
FROM Sales
GROUP BY ROLLUP (Country, Region);
```

Le résultat de la requête a les mêmes agrégations que l’opération GROUP BY simple sans ROLLUP. De plus, la requête crée des sous-totaux pour chaque valeur Country. Enfin, elle donne un total global pour toutes les lignes. Le résultat ressemble à ceci :

| Country | Région | TotalSales |
| :------ | :----- | ---------: |
| Canada | Alberta | 100 |
| Canada | British Columbia | 500 |
| Canada | NULL | 600 |
| États-Unis | Montana | 100 |
| États-Unis | NULL | 100 |
| NULL | NULL | 700 |

### <a name="group-by-cube--"></a>GROUP BY CUBE ( )  

GROUP BY CUBE crée des groupes pour toutes les combinaisons possibles de colonnes. Pour GROUP BY CUBE (a, b), le résultat donne des groupes de valeurs uniques (a, b), (NULL, b), (a, NULL) et (NULL, NULL).

En utilisant la table des exemples précédents, ce code exécute une opération GROUP BY CUBE sur les colonnes Country et Region. 

```sql
SELECT Country, Region, SUM(Sales) AS TotalSales
FROM Sales
GROUP BY CUBE (Country, Region);
```

Le résultat de la requête donne des groupes de valeurs uniques pour (Country, Region), (NULL, Region), (Country, NULL) et (NULL, NULL). Le résultat ressemble à ceci :

| Country | Région | TotalSales |
|---------|--------|-------|
| Canada | Alberta | 100 |
| NULL | Alberta | 100 |
| Canada | British Columbia | 500 |
| NULL | British Columbia | 500 |
| États-Unis | Montana | 100 |
| NULL | Montana | 100 |
| NULL | NULL | 700
| Canada | NULL | 600 |
| États-Unis | NULL | 100 |
   
 ### <a name="group-by-grouping-sets--"></a>GROUP BY GROUPING SETS ( )  
 
L’option GROUPING SETS vous permet de combiner plusieurs clauses GROUP BY dans une seule clause GROUP BY. Les résultats sont l'équivalent de l'opération UNION ALL des groupes spécifiés. 

Par exemple, `GROUP BY ROLLUP (Country, Region)` et `GROUP BY GROUPING SETS ( ROLLUP (Country, Region) )` retournent les mêmes résultats. 

Quand GROUPING SETS contient plusieurs éléments, les résultats sont une union des éléments. Cet exemple retourne l’union des résultats ROLLUP et CUBE pour les colonnes Country et Region.

```sql
SELECT Country, Region, SUM(Sales) AS TotalSales
FROM Sales
GROUP BY GROUPING SETS ( ROLLUP (Country, Region), CUBE (Country, Region) );
```

Les résultats sont les mêmes que cette requête qui retourne une union de deux instructions GROUP BY.

```sql
SELECT Country, Region, SUM(Sales) AS TotalSales
FROM Sales
GROUP BY ROLLUP (Country, Region)
UNION ALL
SELECT Country, Region, SUM(Sales) AS TotalSales
FROM Sales
GROUP BY CUBE (Country, Region);
```

SQL ne consolide pas les groupes dupliqués qui sont générés pour une liste GROUPING SETS. Par exemple, dans `GROUP BY ( (), CUBE (Country, Region) )`, les deux éléments retournent une ligne pour le total global, et les deux lignes figurent dans les résultats. 

 ### <a name="group-by-"></a>GROUP BY ()  
Spécifie le groupe vide qui génère le total global. Il peut être utilisé comme élément d’un GROUPING SET. Par exemple, cette instruction donne le total des ventes pour chaque pays, puis le total global pour tous les pays.

```sql
SELECT Country, SUM(Sales) AS TotalSales
FROM Sales
GROUP BY GROUPING SETS ( Country, () );
```

### <a name="group-by--all--column-expression--n-"></a>GROUP BY [ ALL ] column-expression [ ,...n ] 

S’applique à : SQL Server et Azure SQL Database

REMARQUE :  Cette syntaxe est fournie uniquement pour la compatibilité descendante. Elle sera supprimée dans une version ultérieure. Évitez d’utiliser cette syntaxe dans de nouveaux travaux de développement et prévoyez de modifier les applications qui utilisent actuellement cette syntaxe.

Spécifie d’inclure tous les groupes dans les résultats, qu’ils remplissent ou non les critères de recherche définis dans la clause WHERE. Les groupes qui ne remplissent pas les critères de recherche ont la valeur NULL pour l’agrégation. 

GROUP BY ALL :
- N’est pas pris en charge dans les requêtes qui accèdent aux tables distantes et qui contiennent également une clause WHERE.
- Échoue sur les colonnes qui ont l’attribut FILESTREAM.
  
### <a name="with-distributed_agg"></a>WITH (DISTRIBUTED_AGG)
S’applique à : [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]

L’indicateur de requête DISTRIBUTED_AGG force le système MPP (Massively Parallel Processing) à redistribuer une table sur une colonne spécifique avant d’effectuer une agrégation. Une seule colonne dans la clause GROUP BY peut avoir un indicateur de requête DISTRIBUTED_AGG. À la fin de la requête, la table redistribuée est supprimée. La table d’origine n’est pas modifiée.  

REMARQUE :  L’indicateur de requête DISTRIBUTED_AGG est fourni à des fins de compatibilité descendante avec les versions de [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] antérieures et n’améliore pas les performances de la plupart des requêtes. Par défaut, MPP redistribue déjà les données de façon à améliorer les performances des agrégations. 
  
## <a name="general-remarks"></a>Remarques d'ordre général

### <a name="how-group-by-interacts-with-the-select-statement"></a>Interactions de GROUP BY avec l’instruction SELECT
Liste SELECT :
- Agrégations vectorielles. Si des fonctions d’agrégation sont incluses dans la liste SELECT, GROUP BY calcule une valeur récapitulative pour chaque groupe. Ces expressions sont dites agrégations vectorielles. 
- Agrégations distinctes. Les agrégations AVG (DISTINCT *column_name*), COUNT (DISTINCT *column_name*) et SUM (DISTINCT *column_name*) sont prises en charge avec ROLLUP, CUBE et GROUPING SETS.
  
Clause WHERE :
- SQL supprime les lignes qui ne remplissent pas les conditions définies dans la clause WHERE avant toute opération de regroupement.  
  
Clause HAVING :
- SQL utilise la clause HAVING pour filtrer les groupes dans le jeu de résultats. 
  
Clause ORDER BY :
- Utilisez la clause ORDER BY pour classer le jeu de résultats. La clause GROUP BY ne classe pas le jeu de résultats. 
  
Valeurs NULL :
- Si une colonne de regroupement contient des valeurs NULL, toutes les valeurs NULL sont considérées comme égales et sont collectées dans un même groupe.   
  
## <a name="limitations-and-restrictions"></a>Limitations et restrictions

S’applique à : SQL Server (à partir de 2008) et [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]

### <a name="maximum-capacity"></a>Capacité maximale

Pour une clause GROUP BY qui utilise ROLLUP, CUBE ou GROUPING SETS, le nombre maximal d’expressions est de 32. Le nombre maximal de groupes est de 4 096 (2<sup>12</sup>). Les exemples suivants échouent car la clause GROUP BY a plus de 4 096 groupes.  
 
-   L’exemple suivant échoue, car il génère 4 097 (2<sup>12</sup> + 1) jeux de regroupement.  
  
    ```sql
    GROUP BY GROUPING SETS( CUBE(a1, ..., a12), b )  
    ```  
  
-   L’exemple suivant échoue, car il génère 4 097 (2<sup>12</sup> + 1) groupes. `CUBE ()` et le jeu de regroupement `()` produisent une ligne de total global et les jeux de regroupement en double ne sont pas éliminés.  
  
    ```sql
    GROUP BY GROUPING SETS( CUBE(a1, ..., a12), ())  
    ```  

-   Cet exemple utilise la syntaxe à compatibilité descendante. Il échoue, car il génère 8 192 (2<sup>13</sup>) jeux de regroupement.  
  
    ```sql
    GROUP BY CUBE (a1, ..., a13)   
    GROUP BY a1, ..., a13 WITH CUBE   
    ```    
    Pour les clauses GROUP BY à compatibilité descendante qui ne contiennent pas CUBE ou ROLLUP, la limite du nombre d’éléments regroupés est déterminée par les tailles de colonne GROUP BY, les colonnes agrégées et les valeurs d’agrégation utilisées dans la requête. Cette limite provient de la limite de 8 060 octets de la table de travail intermédiaire requise pour stocker les résultats de requêtes intermédiaires. Un maximum de 12 expressions de regroupement est autorisé quand CUBE ou ROLLUP est spécifié.

### <a name="support-for-iso-and-ansi-sql-2006-group-by-features"></a>Prise en charge des fonctionnalités GROUP BY ISO et ANSI SQL-2006

La clause GROUP BY prend en charge toutes les fonctions GROUP BY incluses dans la norme SQL-2006, avec les exceptions de syntaxe suivantes :  
  
-   Les jeux de regroupement ne sont pas autorisés dans la clause GROUP BY, à moins de faire partie d'une liste GROUPING SETS explicite. Par exemple, `GROUP BY Column1, (Column2, ...ColumnN`) est autorisé dans la norme, mais pas dans Transact-SQL.  Transact-SQL prend en charge les syntaxes `GROUP BY C1, GROUPING SETS ((Column2, ...ColumnN))` et `GROUP BY Column1, Column2, ... ColumnN`, qui sont sémantiquement équivalentes. Ils sont sémantiquement équivalents à l'exemple `GROUP BY` précédent. Cela permet d’éviter que la syntaxe `GROUP BY Column1, (Column2, ...ColumnN`) soit interprétée de façon incorrecte comme la syntaxe `GROUP BY C1, GROUPING SETS ((Column2, ...ColumnN))`, ces deux syntaxes n’étant pas sémantiquement équivalentes.  
  
-   Les jeux de regroupement ne sont pas autorisés à l'intérieur des jeux de regroupement. Par exemple, `GROUP BY GROUPING SETS (A1, A2,...An, GROUPING SETS (C1, C2, ...Cn))` est autorisé dans la norme SQL-2006, mais pas dans Transact-SQL. Transact-SQL autorise les syntaxes `GROUP BY GROUPING SETS( A1, A2,...An, C1, C2, ...Cn )` et `GROUP BY GROUPING SETS( (A1), (A2), ... (An), (C1), (C2), ... (Cn) )`, qui sont sémantiquement équivalentes au premier exemple GROUP BY, tout en étant plus claires.  
  
-   GROUP BY [ALL/DISTINCT] est uniquement autorisée dans une clause GROUP BY simple qui contient des expressions de colonne. Elle n’est pas autorisée avec les constructions GROUPING SETS, ROLLUP, CUBE, WITH CUBE ou WITH ROLLUP. ALL est la valeur par défaut et est implicite. Elle est uniquement autorisée dans la syntaxe à compatibilité descendante.
  
### <a name="comparison-of-supported-group-by-features"></a>Comparaison des fonctionnalités GROUP BY prises en charge  
 Le tableau suivant décrit les fonctions GROUP BY prises en charge en fonction des versions de SQL et du niveau de compatibilité de la base de données.  
  
|Fonctionnalité|SQL Server Integration Services|Niveau de compatibilité SQL Server 100 ou supérieur|SQL Server 2008 ou version ultérieure avec niveau de compatibilité 90.|  
|-------------|-------------------------------------|--------------------------------------------------|-----------------------------------------------------------|  
|Agrégats DISTINCT|Non pris en charge pour WITH CUBE ou WITH ROLLUP.|Pris en charge pour WITH CUBE, WITH ROLLUP, GROUPING SETS, CUBE ou ROLLUP.|Identique au niveau de comptabilité 100.|  
|Fonction définie par l'utilisateur avec nom CUBE ou ROLLUP dans la clause GROUP BY|Les fonctions définies par l’utilisateur **dbo.cube(** _arg1_ **,** _...argN_ **)** ou **dbo.rollup(** _arg1_ **,** ..._argN_ **)** dans la clause GROUP BY sont autorisées.<br /><br /> Par exemple : `SELECT SUM (x) FROM T  GROUP BY dbo.cube(y);`|Les fonctions définies par l’utilisateur **dbo.cube (** _arg1_ **,** ...argN **)** ou **dbo.rollup(** arg1 **,** _...argN_ **)** dans la clause GROUP BY ne sont pas autorisées.<br /><br /> Par exemple : `SELECT SUM (x) FROM T  GROUP BY dbo.cube(y);`<br /><br /> Le message d’erreur suivant est retourné : « Syntaxe incorrecte près du mot clé 'cube'&#124;'rollup'. »<br /><br /> Pour éviter ce problème, remplacez `dbo.cube` par `[dbo].[cube]` ou `dbo.rollup` par `[dbo].[rollup]`.<br /><br /> L’exemple suivant est autorisé : `SELECT SUM (x) FROM T  GROUP BY [dbo].[cube](y);`|Les fonctions définies par l’utilisateur **dbo.cube (** _arg1_ **,** _...argN_) ou **dbo.rollup(** _arg1_ **,** _...argN_ **)** dans la clause GROUP BY sont autorisées<br /><br /> Par exemple : `SELECT SUM (x) FROM T  GROUP BY dbo.cube(y);`|  
|GROUPING SETS|Non pris en charge|Prise en charge|Prise en charge|  
|CUBE|Non pris en charge|Prise en charge|Non pris en charge|  
|ROLLUP|Non pris en charge|Prise en charge|Non pris en charge|  
|Total global, tel que GROUP BY ()|Non pris en charge|Prise en charge|Prise en charge|  
|GROUPING_ID, fonction|Non pris en charge|Prise en charge|Prise en charge|  
|GROUPING (fonction)|Prise en charge|Prise en charge|Prise en charge|  
|WITH CUBE|Prise en charge|Prise en charge|Prise en charge|  
|WITH ROLLUP|Prise en charge|Prise en charge|Prise en charge|  
|Suppression de regroupement WITH CUBE ou WITH ROLLUP « en double »|Prise en charge|Prise en charge|Prise en charge| 
 
  
## <a name="examples"></a>Exemples  
  
### <a name="a-use-a-simple-group-by-clause"></a>R. Utiliser une clause GROUP BY simple  
 L'exemple suivant extrait le total pour chaque `SalesOrderID` de la table `SalesOrderDetail`. Cet exemple utilise AdventureWorks.  
  
```sql  
SELECT SalesOrderID, SUM(LineTotal) AS SubTotal  
FROM Sales.SalesOrderDetail AS sod  
GROUP BY SalesOrderID  
ORDER BY SalesOrderID;  
```  
  
### <a name="b-use-a-group-by-clause-with-multiple-tables"></a>B. Utiliser une clause GROUP BY avec plusieurs tables  
 L'exemple suivant extrait le nombre d'employés pour chaque `City` de la table `Address` en conjonction avec la table `EmployeeAddress`. Cet exemple utilise AdventureWorks. 
  
```sql  
SELECT a.City, COUNT(bea.AddressID) EmployeeCount  
FROM Person.BusinessEntityAddress AS bea   
    INNER JOIN Person.Address AS a  
        ON bea.AddressID = a.AddressID  
GROUP BY a.City  
ORDER BY a.City;  
```  
  
### <a name="c-use-a-group-by-clause-with-an-expression"></a>C. Utiliser une clause GROUP BY avec une expression  
 L'exemple suivant récupère le total des ventes pour chaque année en utilisant la fonction `DATEPART`. La même expression doit être présente dans la liste `SELECT` et dans la clause `GROUP BY`.  
  
```sql  
SELECT DATEPART(yyyy,OrderDate) AS N'Year'  
    ,SUM(TotalDue) AS N'Total Order Amount'  
FROM Sales.SalesOrderHeader  
GROUP BY DATEPART(yyyy,OrderDate)  
ORDER BY DATEPART(yyyy,OrderDate);  
```  
  
### <a name="d-use-a-group-by-clause-with-a-having-clause"></a>D. Utiliser une clause GROUP BY avec une clause HAVING  
 L'exemple suivant utilise la clause `HAVING` pour spécifier lequel des groupes générés dans la clause `GROUP BY` doit être inclus dans le jeu de résultats.  
  
```sql  
SELECT DATEPART(yyyy,OrderDate) AS N'Year'  
    ,SUM(TotalDue) AS N'Total Order Amount'  
FROM Sales.SalesOrderHeader  
GROUP BY DATEPART(yyyy,OrderDate)  
HAVING DATEPART(yyyy,OrderDate) >= N'2003'  
ORDER BY DATEPART(yyyy,OrderDate);  
```  
  
## <a name="examples-azure-synapse-analytics-and-parallel-data-warehouse"></a>Exemples : Azure Synapse Analytics et Parallel Data Warehouse  
  
### <a name="e-basic-use-of-the-group-by-clause"></a>E. Utilisation de base de la clause GROUP BY  
 L’exemple suivant calcule le montant total des ventes réalisées par jour. Une seule ligne contenant la somme de toutes les ventes est retournée pour chaque jour.  
  
```sql  
-- Uses AdventureWorksDW  
  
SELECT OrderDateKey, SUM(SalesAmount) AS TotalSales FROM FactInternetSales  
GROUP BY OrderDateKey ORDER BY OrderDateKey;  
```  
  
### <a name="f-basic-use-of-the-distributed_agg-hint"></a>F. Utilisation de base de l’indicateur DISTRIBUTED_AGG  
 Cet exemple utilise l’indicateur de requête DISTRIBUTED_AGG pour forcer l’application à redistribuer la table sur la colonne `CustomerKey` avant d’effectuer l’agrégation.  
  
```sql  
-- Uses AdventureWorksDW  
  
SELECT CustomerKey, SUM(SalesAmount) AS sas  
FROM FactInternetSales  
GROUP BY CustomerKey WITH (DISTRIBUTED_AGG)  
ORDER BY CustomerKey DESC;  
```  
  
### <a name="g-syntax-variations-for-group-by"></a>G. Variantes de syntaxe pour GROUP BY  
 Quand la liste SELECT n’a pas d’agrégations, chaque colonne dans cette liste doit être incluse dans la liste GROUP BY. Les colonnes calculées dans la liste SELECT peuvent être incluses dans la liste GROUP BY, mais cela n’est pas obligatoire. Voici des exemples d’instructions SELECT avec une syntaxe valide :  
  
```sql  
-- Uses AdventureWorks  
  
SELECT LastName, FirstName FROM DimCustomer GROUP BY LastName, FirstName;  
SELECT NumberCarsOwned FROM DimCustomer GROUP BY YearlyIncome, NumberCarsOwned;  
SELECT (SalesAmount + TaxAmt + Freight) AS TotalCost FROM FactInternetSales GROUP BY SalesAmount, TaxAmt, Freight;  
SELECT SalesAmount, SalesAmount*1.10 SalesTax FROM FactInternetSales GROUP BY SalesAmount;  
SELECT SalesAmount FROM FactInternetSales GROUP BY SalesAmount, SalesAmount*1.10;  
```  
  
### <a name="h-using-a-group-by-with-multiple-group-by-expressions"></a>H. Utiliser GROUP BY avec plusieurs expressions GROUP BY  
 L’exemple suivant regroupe les résultats selon plusieurs critères `GROUP BY`. Si chaque groupe `OrderDateKey` contient des sous-groupes qui peuvent être différenciés par la valeur `DueDateKey`, un nouveau regroupement est défini pour le jeu de résultats.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT OrderDateKey, DueDateKey, SUM(SalesAmount) AS TotalSales   
FROM FactInternetSales
GROUP BY OrderDateKey, DueDateKey   
ORDER BY OrderDateKey;  
```  
  
### <a name="i-using-a-group-by-clause-with-a-having-clause"></a>I. Utilisation d'une clause GROUP BY avec une clause HAVING  
 L’exemple suivant utilise la clause `HAVING` pour spécifier quels groupes générés dans la clause `GROUP BY` doivent être inclus dans le jeu de résultats. Seuls les groupes ayant des dates de commande en 2004 ou après sont inclus dans les résultats.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT OrderDateKey, SUM(SalesAmount) AS TotalSales   
FROM FactInternetSales  
GROUP BY OrderDateKey   
HAVING OrderDateKey > 20040000   
ORDER BY OrderDateKey;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [GROUPING_ID &#40;Transact-SQL&#41;](~/t-sql/functions/grouping-id-transact-sql.md)   
 [GROUPING &#40;Transact-SQL&#41;](~/t-sql/functions/grouping-transact-sql.md)   
 [SELECT &#40;Transact-SQL&#41;](~/t-sql/queries/select-transact-sql.md)   
 [SELECT, clause &#40;Transact-SQL&#41;](~/t-sql/queries/select-clause-transact-sql.md)  
  
  




