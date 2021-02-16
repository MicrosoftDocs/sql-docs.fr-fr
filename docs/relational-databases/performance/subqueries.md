---
title: Sous-requêtes (SQL Server) | Microsoft Docs
description: Examinez un exemple de sous-requête, requête imbriquée dans une instruction SELECT, INSERT, UPDATE ou DELETE ou dans une autre sous-requête dans SQL Server.
ms.custom: ''
ms.date: 02/18/2018
ms.prod: sql
ms.technology: performance
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Subquery
- Subqueries
- subqueries [SQL Server], fundamentals
- subqueries [SQL Server], correlated
- subqueries [SQL Server], types
ms.assetid: bfc97432-c14c-4768-9dc5-a9c512f6b2bd
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 6ce8c74858311d125d8053272f0cff3e14917cf6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345536"
---
# <a name="subqueries-sql-server"></a>Sous-requêtes (SQL Server)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
 
Une sous-requête est une requête qui est imbriquée dans une instruction `SELECT`, `INSERT`, `UPDATE` ou `DELETE`, ou dans une autre sous-requête. Une sous-requête peut être utilisée partout où une expression est autorisée. Dans l’exemple suivant, une sous-requête est utilisée comme expression de colonne appelée MaxUnitPrice dans une instruction SELECT.

```sql
USE AdventureWorks2016;
GO
SELECT Ord.SalesOrderID, Ord.OrderDate,
    (SELECT MAX(OrdDet.UnitPrice)
     FROM Sales.SalesOrderDetail AS OrdDet
     WHERE Ord.SalesOrderID = OrdDet.SalesOrderID) AS MaxUnitPrice
FROM Sales.SalesOrderHeader AS Ord;
GO
```

## <a name="subquery-fundamentals"></a><a name="fundamentals"></a> Principes de base des sous-requêtes
Une sous-requête est également appelée « requête interne » ou « sélection interne » et l'instruction qui la contient est aussi appelée « requête externe » ou « sélection externe ».   

Il est souvent possible d'exprimer sous la forme d'une jointure une instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] qui contient une sous-requête. D'autres questions peuvent uniquement être posées par le biais de sous-requêtes. Dans [!INCLUDE[tsql](../../includes/tsql-md.md)], les performances ne varient généralement pas, que vous utilisiez une instruction comportant une sous-requête ou que vous ayez recours à une syntaxe sémantiquement équivalente qui n'en contient pas. Toutefois, lorsque vous devez vérifier l'existence, une jointure offre de meilleures performances. En effet, la requête imbriquée doit être traitée pour chaque résultat de la requête externe de façon à éliminer les doublons. Dans de tels cas, une jointure donnera de meilleurs résultats. L’exemple suivant présente une sous-requête `SELECT` et une jointure `SELECT` qui retournent le même jeu de résultats :

```sql
USE AdventureWorks2016;
GO

/* SELECT statement built using a subquery. */
SELECT Name
FROM Production.Product
WHERE ListPrice =
    (SELECT ListPrice
     FROM Production.Product
     WHERE Name = 'Chainring Bolts' );
GO

/* SELECT statement built using a join that returns
   the same result set. */
SELECT Prd1. Name
FROM Production.Product AS Prd1
     JOIN Production.Product AS Prd2
       ON (Prd1.ListPrice = Prd2.ListPrice)
WHERE Prd2. Name = 'Chainring Bolts';
GO
```

Une sous-requête imbriquée dans l'instruction SELECT externe comporte les éléments suivants :    
-   Une requête `SELECT` standard constituée des éléments standard d’une liste de sélection.   
-   Une clause `FROM` standard comportant les noms d’une ou de plusieurs tables ou vues.   
-   Une clause `WHERE` facultative.   
-   Une clause `GROUP BY` facultative.   
-   Une clause `HAVING` facultative.   

La requête SELECT d'une sous-requête se place toujours entre parenthèses. Elle ne peut pas contenir de clause `COMPUTE` ni `FOR BROWSE` et peut inclure une clause `ORDER BY` uniquement quand une clause TOP est également spécifiée.   

Une sous-requête peut être imbriquée dans la clause `WHERE` ou `HAVING` d’une instruction `SELECT`, `INSERT`, `UPDATE` ou `DELETE` externe, ou dans une autre sous-requête. Vous pouvez aller jusqu'à 32 niveaux d'imbrication mais cette limite dépend de la mémoire disponible et de la complexité des autres expressions constituant la requête. Les requêtes individuelles, pour leur part, n'acceptent pas toujours 32 niveaux d'imbrication. Une sous-requête peut apparaître à tout endroit où une expression est autorisée, à condition de ne retourner qu'une seule valeur.   

Si une table apparaît uniquement dans une sous-requête et pas dans la requête externe, les colonnes de cette table ne peuvent pas figurer dans les résultats (la liste SELECT de la requête externe).   

Les instructions contenant une sous-requête se présentent généralement sous une des formes suivantes :   
-   WHERE expression \[NOT] IN (subquery)
-   WHERE expression comparison_operator \[ANY | ALL] (subquery)
-   WHERE \[NOT] EXISTS (subquery)   

Dans certaines instructions [!INCLUDE[tsql](../../includes/tsql-md.md)], la sous-requête peut être évaluée comme s'il s'agissait d'une requête indépendante. Théoriquement, les résultats de la sous-requête viennent s’insérer dans la requête externe (bien que ce ne soit pas nécessairement comme cela que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] traite en réalité les instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] comportant des sous-requêtes).    

Il existe trois sous-requêtes de base : 
-   les sous-requêtes qui opèrent sur des listes, introduites par `IN` ou par un opérateur de comparaison modifié par `ANY` ou `ALL` ;
-   les sous-requêtes introduites par un opérateur de comparaison non modifié et qui doivent retourner une valeur unique ;
-   les sous-requêtes introduites par `EXISTS` qui constituent des tests d’existence.

## <a name="subquery-rules"></a><a name="rules"></a> Règles des sous-requêtes
Une sous-requête est soumise aux restrictions suivantes : 
-   La liste de sélection d’une sous-requête introduite par un opérateur de comparaison ne peut contenir qu’une seule expression ou qu’un seul nom de colonne (sauf quand `EXISTS` et `IN` opèrent respectivement sur `SELECT *` ou sur une liste).   
-   Si la clause `WHERE` d’une requête externe comprend un nom de colonne, ce dernier doit pouvoir être joint à la colonne spécifiée dans la liste de sélection de la sous-requête.   
-   Les types de données **ntext**, **text** et **image** ne peuvent pas être utilisés dans la liste de sélection des sous-requêtes.   
-   Puisqu’elles doivent retourner une seule valeur, les sous-requêtes introduites par un opérateur de comparaison non modifié (non suivi des mots clés ANY ou ALL) ne peuvent pas contenir les clauses `GROUP BY` et `HAVING`.   
-   Le mot clé `DISTINCT` ne peut pas être utilisé avec les sous-requêtes qui contiennent une clause GROUP BY.
-   Les clauses `COMPUTE` et `INTO` ne peuvent pas être spécifiées.   
-   `ORDER BY` peut être spécifiée uniquement quand `TOP` l’est aussi.   
-   Une vue créée à l'aide d'une sous-requête ne peut pas être mise à jour.   
-   Par convention, la liste de sélection d’une sous-requête introduite par `EXISTS` est dotée d’un astérisque (\*) au lieu d’un nom de colonne unique. Les règles à appliquer à une sous-requête introduite par `EXISTS` sont identiques à celles d’une liste de sélection standard. En effet, une sous-requête introduite par `EXISTS` crée un test d’existence et retourne les valeurs TRUE ou FALSE au lieu des données.   

## <a name="qualifying-column-names-in-subqueries"></a><a name="qualifying"></a> Qualification de noms de colonnes dans des sous-requêtes
Dans l’exemple suivant, la colonne *BusinessEntityID* indiquée dans la clause `WHERE` de la requête externe est implicitement qualifiée par le nom de table figurant dans la clause `FROM` de la requête externe, (*Sales.Store*). La référence à la colonne *CustomerID* dans la liste de sélection de la sous-requête est qualifiée par la clause `FROM` de la sous-requête, c’est-à-dire par la table *Sales.Customer*.

```sql
USE AdventureWorks2016;
GO
SELECT Name
FROM Sales.Store
WHERE BusinessEntityID NOT IN
    (SELECT CustomerID
     FROM Sales.Customer
     WHERE TerritoryID = 5);
GO
```

En règle générale, les noms de colonne précisés dans une instruction sont implicitement qualifiés par la table spécifiée dans la clause `FROM` appartenant au même niveau d’imbrication. Si une colonne n’existe pas dans la table référencée dans la clause `FROM` d’une sous-requête, elle est implicitement qualifiée par la table référencée dans la clause `FROM` de la requête externe.   

Voici comment se présente la requête lorsque ces qualifications implicites sont clairement spécifiées :

```sql
USE AdventureWorks2016;
GO
SELECT Name
FROM Sales.Store
WHERE Sales.Store.BusinessEntityID NOT IN
    (SELECT Sales.Customer.CustomerID
     FROM Sales.Customer
     WHERE TerritoryID = 5);
GO
```

Ce n'est pas une erreur d'exprimer explicitement le nom de table, et il est toujours possible d'annuler les qualifications implicites de noms de table avec des qualifications explicites.   

> [!IMPORTANT]
> Si une colonne est référencée dans une sous-requête qui n’existe pas dans la table référencée par la clause `FROM` de la sous-requête, mais qu’elle existe dans une table référencée par la clause `FROM` de la requête externe, la requête s’exécute sans erreur. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qualifie implicitement la colonne dans la sous-requête avec le nom de table indiqué dans la requête externe.   

## <a name="multiple-levels-of-nesting"></a><a name="nesting"></a> Niveaux d’imbrication multiples
Une sous-requête peut imbriquer une ou plusieurs autres sous-requêtes. Le nombre de sous-requêtes imbriquées dans une instruction est illimité.   

La requête ci-après recherche les noms des employés qui sont également vendeurs.   

```sql
USE AdventureWorks2016;
GO
SELECT LastName, FirstName
FROM Person.Person
WHERE BusinessEntityID IN
    (SELECT BusinessEntityID
     FROM HumanResources.Employee
     WHERE BusinessEntityID IN
        (SELECT BusinessEntityID
         FROM Sales.SalesPerson)
    );
GO
```

[!INCLUDE[ssResult](../../includes/ssresult-md.md)]

```
LastName                                           FirstName
-------------------------------------------------- -----------------------
Jiang                                              Stephen
Abbas                                              Syed
Alberts                                            Amy
Ansman-Wolfe                                       Pamela
Campbell                                           David
Carson                                             Jillian
Ito                                                Shu
Mitchell                                           Linda
Reiter                                             Tsvi
Saraiva                                            Jos
Vargas                                             Garrett
Varkey Chudukatil                                  Ranjit
Valdez                                             Rachel
Tsoflias                                           Lynn
Pak                                                Jae
Blythe                                             Michael
Mensa-Annan                                        Tete

(17 row(s) affected)
```

La requête la plus profonde retourne les numéros d'identification des vendeurs. La requête du niveau supérieur suivant est évaluée en fonction de ces numéros d'identification de vendeurs et retourne les numéros d'identification des employés. Enfin, la requête externe se sert des numéros d'identification des contacts pour rechercher les noms des employés.   

Cette requête peut également s'exprimer sous la forme d'une jointure :

```sql
USE AdventureWorks2016;
GO
SELECT LastName, FirstName
FROM Person.Person c
INNER JOIN HumanResources.Employee e
ON c.BusinessEntityID = e.BusinessEntityID
JOIN Sales.SalesPerson s 
ON e.BusinessEntityID = s.BusinessEntityID;
GO
```

## <a name="correlated-subqueries"></a><a name="correlated"></a> Sous-requêtes corrélées
De nombreuses requêtes peuvent être évaluées en exécutant une fois la sous-requête et en entrant la ou les valeurs obtenues dans la clause `WHERE` de la requête externe. Dans les requêtes qui contiennent une sous-requête en corrélation (aussi appelée sous-requête répétitive), la sous-requête dépend de la requête externe pour ses valeurs. Cela signifie que la sous-requête s'exécute de manière répétitive, une fois pour chaque ligne que la requête externe pourrait sélectionner.
Cette requête récupère une instance du prénom et du nom de chaque employé pour lequel la prime est égale à 5 000 dans la table *SalesPerson* et dont le numéro d’identification se trouve dans les tables *Employee* et *SalesPerson*.

```sql
USE AdventureWorks2016;
GO
SELECT DISTINCT c.LastName, c.FirstName, e.BusinessEntityID 
FROM Person.Person AS c JOIN HumanResources.Employee AS e
ON e.BusinessEntityID = c.BusinessEntityID 
WHERE 5000.00 IN
    (SELECT Bonus
    FROM Sales.SalesPerson sp
    WHERE e.BusinessEntityID = sp.BusinessEntityID) ;
GO
```

[!INCLUDE[ssResult](../../includes/ssresult-md.md)]

```
LastName FirstName BusinessEntityID
-------------------------- ---------- ------------
Ansman-Wolfe Pamela 280
Saraiva José 282

(2 row(s) affected)
```

La sous-requête précédente de cette instruction ne peut pas être évaluée indépendamment de la requête externe. Elle exige en effet une valeur pour *Employee.BusinessEntityID*, mais cette valeur change à mesure que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] examine les différentes lignes de la table *Employee*.   
Cette requête est évaluée de la manière suivante : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tente d’inclure chaque ligne de la table Employee dans les résultats en entrant la valeur de chaque ligne dans la requête interne.
Par exemple, si [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] examine tout d’abord la ligne de `Syed Abbas`, la variable *Employee.BusinessEntityID* prend la valeur 285, que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] entre ensuite dans la requête interne.

```sql
USE AdventureWorks2016;
GO
SELECT Bonus
FROM Sales.SalesPerson
WHERE BusinessEntityID = 285;
GO
```

Le résultat est 0 (`Syed Abbas` n’a pas reçu de prime parce qu’il n’est pas commercial), de sorte que la requête externe donne :

```sql
USE AdventureWorks2016;
GO
SELECT LastName, FirstName
FROM Person.Person AS c JOIN HumanResources.Employee AS e
ON e.BusinessEntityID = c.BusinessEntityID 
WHERE 5000 IN (0.00);
GO
```

Puisque la valeur est false, la ligne de `Syed Abbas` n’est pas incluse dans les résultats. Suivez la même procédure pour la ligne de `Pamela Ansman-Wolfe`. Vous constaterez qu'elle est incluse dans les résultats.     

Les sous-requêtes corrélées peuvent aussi inclure des fonctions table dans la clause `FROM` en référençant des colonnes d’une table dans la requête externe sous la forme d’argument de cette fonction table. Dans ce cas, pour chaque ligne de la requête externe, la fonction table est évaluée en fonction de la sous-requête.    
  
## <a name="subquery-types"></a><a name="types"></a> Types de sous-requête
Les sous-requêtes peuvent être spécifiées dans de nombreux endroits : 
-   Avec des alias. Pour plus d’informations, consultez [Sous-requêtes et alias](#aliases).
-   Avec `IN` ou `NOT IN`. Pour plus d’informations, consultez [Sous-requêtes introduites par IN](#in) et [Sous-requêtes introduites par NOT IN](#notin).
-   Dans les instructions `UPDATE`, `DELETE` et `INSERT`. Pour plus d’informations, consultez [Sous-requêtes dans les instructions UPDATE, DELETE et INSERT](#upsert).
-   Avec des opérateurs de comparaison. Pour plus d’informations, consultez [Sous-requêtes et opérateurs de comparaison](#comparison).
-   Avec `ANY`, `SOME` ou `ALL`. Pour plus d’informations, consultez [Opérateurs de comparaison modifiés par ANY, SOME ou ALL](#comparison_modified).
-   Avec `EXISTS` ou `NOT EXISTS`. Pour plus d’informations, consultez [Sous-requêtes introduites par EXISTS](#exists) et [Sous-requêtes introduites par NOT EXISTS](#notexists).
-   À la place d'une expression. Pour plus d’informations, consultez [Utilisation d’une sous-requête à la place d’une expression](#expression).

### <a name="subqueries-with-aliases"></a><a name="aliases"></a> Sous-requêtes et alias
De nombreuses instructions où la sous-requête et la requête externe portent sur la même table peuvent également être formulées sous forme de jointures réflexives (jointure d'une table à elle-même). Par exemple, vous pouvez rechercher les adresses d'employés dans un état donné à l'aide d'une sous-requête :   

```sql
USE AdventureWorks2016;
GO
SELECT StateProvinceID, AddressID
FROM Person.Address
WHERE AddressID IN
    (SELECT AddressID
     FROM Person.Address
     WHERE StateProvinceID = 39);
GO
```

[!INCLUDE[ssResult](../../includes/ssresult-md.md)]  

```
StateProvinceID AddressID
----------- -----------
39 942
39 955
39 972
39 22660

(4 row(s) affected)
```   

Vous pouvez aussi utiliser une auto-jointure :   

```sql
USE AdventureWorks2016;
GO
SELECT e1.StateProvinceID, e1.AddressID
FROM Person.Address AS e1
INNER JOIN Person.Address AS e2
ON e1.AddressID = e2.AddressID
AND e2.StateProvinceID = 39;
GO
```

Les alias de table sont requis car la table, jointe à elle-même, apparaît dans deux rôles différents. Les alias peuvent aussi s'employer dans des requêtes imbriquées qui portent sur la même table, qu'il s'agisse d'une requête interne ou externe.    

```sql
USE AdventureWorks2016;
GO
SELECT e1.StateProvinceID, e1.AddressID
FROM Person.Address AS e1
WHERE e1.AddressID IN
    (SELECT e2.AddressID
     FROM Person.Address AS e2
     WHERE e2.StateProvinceID = 39);
GO
```

Grâce aux noms d’alias explicites, il apparaît clairement qu’une référence à la table *Person.Address* dans la sous-requête est différente de la référence spécifiée dans la requête externe.   

### <a name="subqueries-with-in"></a><a name="in"></a> Sous-requêtes introduites par IN
Le résultat d’une sous-requête introduite par `IN` (ou par `NOT IN`) est une liste de valeurs zéro ou plus. Dès que la sous-requête retourne des résultats, la requête externe les utilise.    
La requête suivante recherche le nom de toutes les roues fabriquées par Adventure Works Cycles.     

```sql
USE AdventureWorks2016;
GO
SELECT Name
FROM Production.Product
WHERE ProductSubcategoryID IN
    (SELECT ProductSubcategoryID
     FROM Production.ProductSubcategory
     WHERE Name = 'Wheels');
GO
```

[!INCLUDE[ssResult](../../includes/ssresult-md.md)]     

```
Name
----------------------------
LL Mountain Front Wheel
ML Mountain Front Wheel
HL Mountain Front Wheel
LL Road Front Wheel
ML Road Front Wheel
HL Road Front Wheel
Touring Front Wheel
LL Mountain Rear Wheel
ML Mountain Rear Wheel
HL Mountain Rear Wheel
LL Road Rear Wheel
ML Road Rear Wheel
HL Road Rear Wheel
Touring Rear Wheel

(14 row(s) affected)
```

Cette instruction est évaluée en deux étapes. D'abord, la requête interne retourne le numéro d'identification de la sous-catégorie qui correspond au nom « Wheel » (roue) (17). Ensuite, cette valeur vient s’insérer dans la requête externe, qui recherche le nom des produits correspondant aux numéros d’identification de la sous-catégorie de Product.     

```sql
USE AdventureWorks2016;
GO
SELECT Name
FROM Production.Product
WHERE ProductSubcategoryID IN ('17');
GO
```

Dans la résolution de problèmes de ce type, une jointure se distingue d'une sous-requête en ce sens qu'elle vous permet d'afficher des colonnes provenant de plusieurs tables dans les résultats. Par exemple, si vous voulez inclure le nom de la sous-catégorie de produits dans le résultat, vous devez utiliser une jointure.    

```sql
USE AdventureWorks2016;
GO
SELECT p.Name, s.Name
FROM Production.Product p
INNER JOIN Production.ProductSubcategory s
ON p.ProductSubcategoryID = s.ProductSubcategoryID
AND s.Name = 'Wheels';
GO
```

[!INCLUDE[ssResult](../../includes/ssresult-md.md)]    

```
Name
LL Mountain Front Wheel Wheels
ML Mountain Front Wheel Wheels
HL Mountain Front Wheel Wheels
LL Road Front Wheel Wheels
ML Road Front Wheel Wheels
HL Road Front Wheel Wheels
Touring Front Wheel Wheels
LL Mountain Rear Wheel Wheels
ML Mountain Rear Wheel Wheels
HL Mountain Rear Wheel Wheels
LL Road Rear Wheel Wheels
ML Road Rear Wheel Wheels
HL Road Rear Wheel Wheels
Touring Rear Wheel Wheels

(14 row(s) affected)
```    

La requête suivante recherche le nom de tous les fournisseurs dont la cote de solvabilité est bonne, auprès desquels Adventure Works Cycles commande au moins 20 articles et dont le délai moyen de livraison est inférieur à 16 jours.    

```sql
USE AdventureWorks2016;
GO
SELECT Name
FROM Purchasing.Vendor
WHERE CreditRating = 1
AND BusinessEntityID IN
    (SELECT BusinessEntityID
     FROM Purchasing.ProductVendor
     WHERE MinOrderQty >= 20
     AND AverageLeadTime < 16);
GO
```

[!INCLUDE[ssResult](../../includes/ssresult-md.md)]

```
Name
--------------------------------------------------
Compete Enterprises, Inc
International Trek Center
First National Sport Co.
Comfort Road Bicycles
Circuit Cycles
First Rate Bicycles
Jeff's Sporting Goods
Competition Bike Training Systems
Electronic Bike Repair & Supplies
Crowley Sport
Expert Bike Co
Team Athletic Co.
Compete, Inc.   

(13 row(s) affected)
```   

La requête interne est évaluée, puis retourne les numéros d'identification des trois fournisseurs qui remplissent les critères de la sous-requête. La requête externe est évaluée ensuite. Notez qu'il est possible d'inclure plusieurs conditions dans la clause WHERE des requêtes interne et externe.   

En utilisant une jointure, la requête s'exprime de la façon suivante :

```sql
USE AdventureWorks2016;
GO
SELECT DISTINCT Name
FROM Purchasing.Vendor v
INNER JOIN Purchasing.ProductVendor p
ON v.BusinessEntityID = p.BusinessEntityID
WHERE CreditRating = 1
  AND MinOrderQty >= 20
  AND AverageLeadTime < 16;
GO
```

Une jointure peut toujours s'exprimer sous la forme d'une sous-requête. Une sous-requête peut souvent s'exprimer sous la forme d'une jointure, mais pas toujours. Les jointures étant symétriques, vous pouvez joindre une table A à une table B ou inversement et obtenir le même résultat. Ce n'est pas le cas lorsqu'on utilise une sous-requête.    

### <a name="subqueries-with-not-in"></a><a name="notin"></a> Sous-requêtes introduites par NOT IN
Les sous-requêtes introduites par le mot clé NOT IN retournent également une liste de valeurs zéro ou plus.   
La requête suivante trouve les noms des produits qui ne font pas partie de la sous-catégorie « produits finis ».   

```sql
USE AdventureWorks2016;
GO
SELECT Name
FROM Production.Product
WHERE ProductSubcategoryID NOT IN
    (SELECT ProductSubcategoryID
     FROM Production.ProductSubcategory
     WHERE Name = 'Mountain Bikes' 
        OR Name = 'Road Bikes'
        OR Name = 'Touring Bikes');
GO
```

Cette instruction ne peut pas être convertie en jointure. La jointure analogue « non égal à » revêt une autre signification : elle trouve les noms des produits qui se trouvent dans une sous-catégorie différente de « produits finis ».      

### <a name="subqueries-in-update-delete-and-insert-statements"></a><a name="upsert"></a> Sous-requêtes dans les instructions UPDATE, DELETE et INSERT
Les sous-requêtes peuvent être imbriquées dans les instructions de manipulation de données (DML, Data Manipulation Language) `UPDATE`, `DELETE`, `INSERT` et `SELECT`.    

L’exemple suivant double la valeur de la colonne *ListPrice* dans la table *Production.Product*. La sous-requête contenue dans la clause `WHERE` fait référence à la table *Purchasing.ProductVendor* pour limiter les lignes mises à jour dans la table *Product* à celles fournies par *BusinessEntity* 1540 uniquement.

```sql
USE AdventureWorks2016;
GO 
UPDATE Production.Product
SET ListPrice = ListPrice * 2
WHERE ProductID IN
    (SELECT ProductID 
     FROM Purchasing.ProductVendor
     WHERE BusinessEntityID = 1540);
GO
```

Voici une instruction `UPDATE` équivalente qui utilise une jointure :

```sql
USE AdventureWorks2016;
GO 
UPDATE Production.Product
SET ListPrice = ListPrice * 2
FROM Production.Product AS p
INNER JOIN Purchasing.ProductVendor AS pv
    ON p.ProductID = pv.ProductID AND BusinessEntityID = 1540;
GO   
```

### <a name="subqueries-with-comparison-operators"></a><a name="comparison"></a> Sous-requêtes et opérateurs de comparaison
Les sous-requêtes peuvent être introduites par l'un de ces opérateurs de comparaison (=, < >, >, > =, <, ! >, ! < ou < =).   

Une sous-requête introduite par un opérateur de comparaison non modifié (autrement dit, un opérateur de comparaison non suivi de `ANY` ou de `ALL`) ne doit retourner qu’une seule valeur et non une liste de valeurs, à l’instar des sous-requêtes introduites par `IN`. Si une sous-requête de ce type retourne plusieurs valeurs, SQL Server affiche un message d’erreur.    

Pour utiliser une sous-requête introduite par un opérateur de comparaison non modifié, et savoir si elle ne va retourner qu'une seule valeur, vous devez bien connaître vos données et la nature du problème.     

Par exemple, supposons que chaque commercial couvre uniquement un territoire commercial et que vous voulez trouver le nom des clients situés dans le territoire couvert par `Linda Mitchell`, vous pouvez écrire une instruction comportant une sous-requête introduite par le seul opérateur de comparaison = (signe égal).    

```sql
USE AdventureWorks2016;
GO
SELECT CustomerID
FROM Sales.Customer
WHERE TerritoryID =
    (SELECT TerritoryID
     FROM Sales.SalesPerson
     WHERE BusinessEntityID = 276);
GO
```

Toutefois, si `Linda Mitchell` couvre plusieurs territoires commerciaux, un message d’erreur s’affiche. À la place de l’opérateur de comparaison = (signe égal), vous pouvez utiliser une sous-requête introduite par `IN` (= ANY fonctionne aussi).    

Étant donné que les fonctions d'agrégation retournent une seule unique, elles figurent souvent dans les sous-requêtes introduites par un opérateur de comparaison non modifié. Par exemple, l'instruction suivante recherche le nom de tous les produits dont le tarif est supérieur au tarif moyen.     

```sql
USE AdventureWorks2016;
GO
SELECT Name
FROM Production.Product
WHERE ListPrice >
    (SELECT AVG (ListPrice)
     FROM Production.Product);
GO
```     

Puisque les sous-requêtes introduites par un opérateur de comparaison non modifié ne doivent retourner qu’une seule valeur, elles ne peuvent pas contenir de clauses `GROUP BY` ni `HAVING`, sauf si vous savez que celles-ci ne retournent qu’une seule valeur également. Par exemple, la requête suivante trouve les produits vendus plus chers que le produit le moins cher de la sous-catégorie 14.     

```sql
USE AdventureWorks2016;
GO
SELECT Name
FROM Production.Product
WHERE ListPrice >
    (SELECT MIN (ListPrice)
     FROM Production.Product
     GROUP BY ProductSubcategoryID
     HAVING ProductSubcategoryID = 14);
GO
```

### <a name="comparison-operators-modified-by-any-some-or-all"></a><a name="comparison_modified"></a> Opérateurs de comparaison modifiés par ANY, SOME ou ALL
Les opérateurs de comparaison qui introduisent une sous-requête peuvent être modifiés par les mots clés ALL ou ANY. SOME est un équivalent de `ANY` dans la norme ISO.     

Les sous-requêtes introduites par un opérateur de comparaison modifié retournent une liste contenant aucune ou plusieurs valeurs et peuvent contenir une clause `GROUP BY` ou `HAVING`. Ces sous-requêtes peuvent être reformulées avec `EXISTS`.     

En prenant comme exemple l’opérateur de comparaison >, `>ALL` signifie supérieur à toutes les valeurs. En d'autres termes, cela signifie supérieur à la valeur maximale. Par exemple, `>ALL (1, 2, 3)` signifie supérieur à 3. `>ANY` signifie supérieur à une valeur au moins, autrement dit supérieur à la valeur minimale. Par conséquent, `>ANY (1, 2, 3)` signifie supérieur à 1.
Pour qu’une ligne d’une sous-requête comportant `>ALL` réponde au critère défini dans la requête externe, la valeur de la colonne introduisant la sous-requête doit être supérieure à chaque valeur de la liste de valeurs retournée par la sous-requête.    

De même, `>ANY` signifie qu’afin qu’une ligne satisfasse au critère défini dans la requête externe, la valeur spécifiée dans la colonne qui introduit la sous-requête doit être supérieure à au moins une valeur de la liste retournée par la sous-requête.    

La requête suivante donne un exemple de sous-requête introduite par un opérateur de comparaison modifié par ANY. Elle recherche les produits dont les tarifs sont supérieurs ou égaux au tarif maximal de n'importe quelle sous-catégorie de produits.    

```sql
USE AdventureWorks2016;
GO
SELECT Name
FROM Production.Product
WHERE ListPrice >= ANY
    (SELECT MAX (ListPrice)
     FROM Production.Product
     GROUP BY ProductSubcategoryID);
GO
```    

Pour chaque sous-catégorie Product, la requête interne recherche le tarif maximal. Parmi toutes ces valeurs, la requête externe recherche les tarifs de produit particulier supérieurs ou égaux au tarif maximal de n'importe quelle sous-catégorie de produits. Si `ANY` est remplacé par `ALL`, la requête ne retourne que les produits dont le tarif est supérieur ou égal à tous les tarifs retournés dans la requête interne.    

Si la sous-requête ne retourne aucune valeur, la requête entière ne retourne aucune valeur non plus.    

L’opérateur `=ANY` est équivalent à `IN`. Par exemple, pour trouver le nom de toutes les roues fabriquées par Adventure Works Cycles, vous pouvez utiliser `IN` ou `=ANY`.

```sql
--Using =ANY
USE AdventureWorks2016;
GO
SELECT Name
FROM Production.Product
WHERE ProductSubcategoryID =ANY
    (SELECT ProductSubcategoryID
     FROM Production.ProductSubcategory
     WHERE Name = 'Wheels');
GO

--Using IN
USE AdventureWorks2016;
GO
SELECT Name
FROM Production.Product
WHERE ProductSubcategoryID IN
    (SELECT ProductSubcategoryID
     FROM Production.ProductSubcategory
     WHERE Name = 'Wheels');
GO
```

Voici l'ensemble de résultats pour ces deux requêtes :

```
Name
--------------------------------------------------
LL Mountain Front Wheel
ML Mountain Front Wheel
HL Mountain Front Wheel
LL Road Front Wheel
ML Road Front Wheel
HL Road Front Wheel
Touring Front Wheel
LL Mountain Rear Wheel
ML Mountain Rear Wheel
HL Mountain Rear Wheel
LL Road Rear Wheel
ML Road Rear Wheel
HL Road Rear Wheel
Touring Rear Wheel

(14 row(s) affected)
```    

Cependant, l’opérateur `<>ANY` diffère de `NOT IN` : `<>ANY` signifie not = a, ou not = b, ou not = c. `NOT IN` signifie not = a, et not = b, et not = c. `<>ALL` signifie la même chose que `NOT IN`.     

Par exemple, la requête suivante recherche les clients situés dans un secteur non couvert par un vendeur.     

```sql
USE AdventureWorks2016;
GO
SELECT CustomerID
FROM Sales.Customer
WHERE TerritoryID <> ANY
    (SELECT TerritoryID
     FROM Sales.SalesPerson);
GO
```    

Les résultats comprennent tous les clients, à l'exception de ceux dont le secteur géographique a pour valeur NULL, car chaque secteur affecté à un client est couvert par un vendeur. La requête interne recherche tous les secteurs géographiques couverts par les vendeurs puis, pour chaque secteur, la requête externe recherche les clients qui ne se trouvent dans aucun secteur.    

Pour la même raison, quand vous utilisez `NOT IN` dans cette requête, les résultats ne comprennent aucun des clients.      

Vous pouvez obtenir les mêmes résultats avec l’opérateur `<>ALL`, qui est l’équivalent de `NOT IN`.   

### <a name="subqueries-with-exists"></a><a name="exists"></a> Sous-requêtes introduites par EXISTS
Une sous-requête introduite par le mot clé `EXISTS` constitue un test d’existence. La clause `WHERE` de la requête externe teste l’existence des lignes retournées par la sous-requête. En réalité, la sous-requête ne génère pas de données, elle retourne la valeur TRUE ou FALSE.   

Une sous-requête introduite par EXISTS présente la syntaxe suivante :   

`WHERE [NOT] EXISTS (subquery)`    

La requête suivante recherche le nom de tous les produits de la sous-catégorie Wheels :    

```sql
USE AdventureWorks2016;
GO
SELECT Name
FROM Production.Product
WHERE EXISTS
    (SELECT * 
     FROM Production.ProductSubcategory
     WHERE ProductSubcategoryID = 
            Production.Product.ProductSubcategoryID
        AND Name = 'Wheels');
GO
```

[!INCLUDE[ssResult](../../includes/ssresult-md.md)]    

```
Name
--------------------------------------------------
LL Mountain Front Wheel
ML Mountain Front Wheel
HL Mountain Front Wheel
LL Road Front Wheel
ML Road Front Wheel
HL Road Front Wheel
Touring Front Wheel
LL Mountain Rear Wheel
ML Mountain Rear Wheel
HL Mountain Rear Wheel
LL Road Rear Wheel
ML Road Rear Wheel
HL Road Rear Wheel
Touring Rear Wheel

(14 row(s) affected)
```    

Pour comprendre les résultats de cette requête, considérez les noms de chaque produit un par un. La valeur provoque-t-elle le retour d'au moins une ligne par la sous-requête ? En d'autres termes, le test d'existence retourne-t-il la valeur TRUE ?   

Les sous-requêtes introduites par EXISTS sont légèrement différentes des autres sous-requêtes pour les raisons suivantes : 
-   Le mot clé `EXISTS` n’est pas précédé d’un nom de colonne, d’une constante ni de toute autre expression.     
-   La liste de sélection d’une sous-requête introduite par `EXISTS` se résume presque toujours à un astérisque (*). Il est inutile d’énumérer les noms de colonne puisque vous effectuez un simple test d’existence sur les lignes qui remplissent les conditions spécifiées dans la sous-requête.    

Le mot clé `EXISTS` est important, car il y a rarement une autre formulation sans sous-requêtes. Bien que certaines requêtes créées avec EXISTS ne puissent pas être exprimées autrement, de nombreuses requêtes peuvent utiliser IN ou un opérateur de comparaison modifié par `ANY` ou `ALL` pour obtenir des résultats semblables.     

Par exemple, la requête précédente peut être exprimée à l’aide de IN :   

```sql
USE AdventureWorks2016;
GO
SELECT Name
FROM Production.Product
WHERE ProductSubcategoryID IN
    (SELECT ProductSubcategoryID
     FROM Production.ProductSubcategory
     WHERE Name = 'Wheels');
GO
```   

### <a name="subqueries-with-not-exists"></a><a name="notexists"></a> Sous-requêtes introduites par NOT EXISTS
`NOT EXISTS` fonctionne comme `EXISTS`, sauf que la clause `WHERE` dans laquelle ce paramètre est utilisé est remplie si la sous-requête ne retourne aucune ligne.    

Par exemple, pour rechercher les noms de produits qui n'appartiennent pas à la sous-catégorie des roues (Wheels) :   

```sql
USE AdventureWorks2016;
GO
SELECT Name
FROM Production.Product
WHERE NOT EXISTS
    (SELECT * 
     FROM Production.ProductSubcategory
     WHERE ProductSubcategoryID = 
            Production.Product.ProductSubcategoryID
        AND Name = 'Wheels');
GO
```   

### <a name="subqueries-used-in-place-of-an-expression"></a><a name="expression"></a> Utilisation d’une sous-requête à la place d’une expression
Dans [!INCLUDE[tsql](../../includes/tsql-md.md)], une sous-requête peut remplacer une expression partout où celle-ci est autorisée dans des instructions `SELECT`, `UPDATE`, `INSERT` et `DELETE`, à l’exception d’une liste `ORDER BY`.    

L'exemple suivant illustre l'application de cette amélioration. Cette requête fournit le prix de tous les produits VTT, leur prix moyen, ainsi que la différence entre le prix de chaque VTT et le prix moyen.    

```sql
USE AdventureWorks2016;
GO
SELECT Name, ListPrice, 
(SELECT AVG(ListPrice) FROM Production.Product) AS Average, 
    ListPrice - (SELECT AVG(ListPrice) FROM Production.Product)
    AS Difference
FROM Production.Product
WHERE ProductSubcategoryID = 1;
GO
```   

## <a name="see-also"></a>Voir aussi  
[IN &#40;Transact-SQL&#41;](../../t-sql/language-elements/in-transact-sql.md)       
[EXISTS &#40;Transact-SQL&#41;](../../t-sql/language-elements/exists-transact-sql.md)     
[ALL &#40;Transact-SQL&#41;](../../t-sql/language-elements/all-transact-sql.md)     
[SOME | ANY &#40;Transact-SQL&#41;](../../t-sql/language-elements/some-any-transact-sql.md)     
[Jointures](../../relational-databases/performance/joins.md)    
[Opérateurs de comparaison &#40;Transact-SQL&#41;](../../t-sql/language-elements/comparison-operators-transact-sql.md)       
  
