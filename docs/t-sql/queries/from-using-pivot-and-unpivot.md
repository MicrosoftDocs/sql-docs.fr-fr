---
title: Utilisation des opérateurs PIVOT et UNPIVOT | Microsoft Docs
description: Référence Transact-SQL pour les opérateurs relationnels PIVOT et UNPIVOT. Utilisez ces opérateurs sur des instructions SELECT pour transformer une expression table en une autre table.
ms.date: 10/14/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- PIVOT_TSQL
helpviewer_keywords:
- FROM clause, UNPIVOT operator
- unpivoting tables
- table pivoting [SQL Server]
- UNPIVOT operator
- crosstab query
- PIVOT operator
- rotating table-valued expressions
- pivoting tables
- FROM clause, PIVOT operator
- rotating columns
ms.assetid: 24ba54fc-98f7-4d35-8881-b5158aac1d66
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 21df23cf904c561d295553fb5fb79ebe2cd7702a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207738"
---
# <a name="from---using-pivot-and-unpivot"></a>FROM - Utilisation des opérateurs PIVOT et UNPIVOT

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Vous pouvez utiliser les opérateurs de relation `PIVOT` et `UNPIVOT` pour modifier une expression table dans une autre table. `PIVOT` fait pivoter une expression de table en activant les valeurs uniques d’une colonne de l’expression en plusieurs colonnes dans la sortie. Et `PIVOT` exécute des agrégations là où elles sont requises sur les valeurs de colonnes restantes qui doivent figurer dans la sortie finale. L’opérateur `UNPIVOT` effectue l’opération inverse : il transforme les colonnes d’une expression table en valeurs de colonne.  
  
L’opérateur `PIVOT` a une syntaxe plus simple et plus lisible qu’une série complexe d’instructions `SELECT...CASE`. Pour obtenir la description complète de la syntaxe de `PIVOT`, consultez [FROM (Transact-SQL)](../../t-sql/queries/from-transact-sql.md).  
  
## <a name="syntax"></a>Syntaxe  
La syntaxe suivante récapitule comment utiliser l’opérateur `PIVOT`.  
  
```syntaxsql  
SELECT <non-pivoted column>,  
    [first pivoted column] AS <column name>,  
    [second pivoted column] AS <column name>,  
    ...  
    [last pivoted column] AS <column name>  
FROM  
    (<SELECT query that produces the data>)   
    AS <alias for the source query>  
PIVOT  
(  
    <aggregation function>(<column being aggregated>)  
FOR   
[<column that contains the values that will become column headers>]   
    IN ( [first pivoted column], [second pivoted column],  
    ... [last pivoted column])  
) AS <alias for the pivot table>  
<optional ORDER BY clause>;  
```  

## <a name="remarks"></a>Notes  
Les identificateurs de colonne dans la clause `UNPIVOT` suivent le classement de catalogue. Pour [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)], le classement est toujours `SQL_Latin1_General_CP1_CI_AS`. Pour les bases de données partiellement autonomes [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)], le classement est toujours `Latin1_General_100_CI_AS_KS_WS_SC`. Si la colonne est combinée avec d’autres colonnes, une clause Collate (`COLLATE DATABASE_DEFAULT`) doit être ajoutée pour éviter les conflits.  

  
## <a name="basic-pivot-example"></a>Exemple PIVOT de base  
L'exemple de code suivant produit un tableau à deux colonnes et quatre lignes.  
  
```sql
USE AdventureWorks2014 ;  
GO  
SELECT DaysToManufacture, AVG(StandardCost) AS AverageCost   
FROM Production.Product  
GROUP BY DaysToManufacture;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```
DaysToManufacture AverageCost
----------------- -----------
0                 5.0885
1                 223.88
2                 359.1082
4                 949.4105
```
  
Aucun produit n'est défini avec trois `DaysToManufacture`.  
  
Le code suivant affiche le même résultat, croisé dynamiquement pour que les valeurs `DaysToManufacture` deviennent les en-têtes de colonne. Une colonne est fournie pour trois `[3]` jours même si les résultats sont `NULL`.  
  
```sql
-- Pivot table with one row and five columns  
SELECT 'AverageCost' AS Cost_Sorted_By_Production_Days,   
[0], [1], [2], [3], [4]  
FROM  
(SELECT DaysToManufacture, StandardCost   
    FROM Production.Product) AS SourceTable  
PIVOT  
(  
AVG(StandardCost)  
FOR DaysToManufacture IN ([0], [1], [2], [3], [4])  
) AS PivotTable;  
  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```
Cost_Sorted_By_Production_Days 0           1           2           3           4         
------------------------------ ----------- ----------- ----------- ----------- -----------
AverageCost                    5.0885      223.88      359.1082    NULL        949.4105
```
  
## <a name="complex-pivot-example"></a>Exemple PIVOT complexe  
Un scénario classique consiste à utiliser l'opérateur `PIVOT` pour générer des rapports à tabulation croisée afin de synthétiser des données. Par exemple, supposons que vous souhaitiez interroger la table `PurchaseOrderHeader` de l'exemple de base de données `AdventureWorks2014` pour déterminer le nombre de commandes traitées par certains employés. La requête suivante fournit ce rapport, ventilé par fournisseur.  
  
```sql
USE AdventureWorks2014;  
GO  
SELECT VendorID, [250] AS Emp1, [251] AS Emp2, [256] AS Emp3, [257] AS Emp4, [260] AS Emp5  
FROM   
(SELECT PurchaseOrderID, EmployeeID, VendorID  
FROM Purchasing.PurchaseOrderHeader) p  
PIVOT  
(  
COUNT (PurchaseOrderID)  
FOR EmployeeID IN  
( [250], [251], [256], [257], [260] )  
) AS pvt  
ORDER BY pvt.VendorID;  
```  
  
Voici un jeu de résultats partiel.  
  
```
VendorID    Emp1        Emp2        Emp3        Emp4        Emp5  
----------- ----------- ----------- ----------- ----------- -----------
1492        2           5           4           4           4
1494        2           5           4           5           4
1496        2           4           4           5           5
1498        2           5           4           4           4
1500        3           4           4           5           4
```
  
Les résultats retournés par cette instruction de sous-sélection sont croisés dynamiquement sur la colonne `EmployeeID`.  
  
```sql
SELECT PurchaseOrderID, EmployeeID, VendorID  
FROM PurchaseOrderHeader;  
```  
  
Les valeurs uniques retournées par la colonne `EmployeeID` deviennent des champs dans l'ensemble de résultats final. Par conséquent, il existe une colonne pour chaque numéro `EmployeeID` spécifié dans la clause PIVOT : dans ce cas les employés `250`, `251`, `256`, `257` et `260`. La colonne `PurchaseOrderID` sert de colonne de valeur par rapport à laquelle les colonnes retournées dans la sortie finale (colonnes de regroupement) sont regroupées. Dans ce cas, les colonnes de regroupement sont agrégées par la fonction `COUNT`. Un message d'avertissement apparaît indiquant qu'aucune valeur NULL figurant dans la colonne `PurchaseOrderID` n'a été prise en compte pour le calcul de la valeur `COUNT` de chaque employé.  
  
> [!IMPORTANT]  
>  Quand vous utilisez des fonctions d’agrégation avec l’opérateur `PIVOT`, les valeurs NULL présentes dans la colonne de valeurs ne sont pas prises en compte lors du calcul d’une agrégation.  

## <a name="unpivot-example"></a>Exemple UNPIVOT
  
L’opérateur `UNPIVOT` effectue pratiquement l’opération inverse de l’opérateur `PIVOT`, en transformant des colonnes en lignes. Supposons que la table générée dans l'exemple précédent soit stockée dans la base de données sous le nom `pvt` et que vous souhaitiez transformer les identificateurs de colonne `Emp1`, `Emp2`, `Emp3`, `Emp4` et `Emp5` en valeurs de ligne correspondant à un fournisseur particulier. Cela signifie que vous devez identifier deux colonnes supplémentaires. La colonne qui doit contenir les valeurs de colonne transformées (`Emp1`, `Emp2`,...) est la colonne `Employee` tandis que la colonne destinée à contenir les valeurs existant actuellement dans les colonnes subissant la transformation est la colonne `Orders`. Ces colonnes correspondent respectivement aux paramètres *pivot_column* et *value_column* dans la définition [!INCLUDE[tsql](../../includes/tsql-md.md)]. La requête est la suivante.  
  
```sql
-- Create the table and insert values as portrayed in the previous example.  
CREATE TABLE pvt (VendorID INT, Emp1 INT, Emp2 INT,  
    Emp3 INT, Emp4 INT, Emp5 INT);  
GO  
INSERT INTO pvt VALUES (1,4,3,5,4,4);  
INSERT INTO pvt VALUES (2,4,1,5,5,5);  
INSERT INTO pvt VALUES (3,4,3,5,4,4);  
INSERT INTO pvt VALUES (4,4,2,5,5,4);  
INSERT INTO pvt VALUES (5,5,1,5,5,5);  
GO  
-- Unpivot the table.  
SELECT VendorID, Employee, Orders  
FROM   
   (SELECT VendorID, Emp1, Emp2, Emp3, Emp4, Emp5  
   FROM pvt) p  
UNPIVOT  
   (Orders FOR Employee IN   
      (Emp1, Emp2, Emp3, Emp4, Emp5)  
)AS unpvt;  
GO  
```  
  
Voici un jeu de résultats partiel.  
  
```
VendorID    Employee    Orders
----------- ----------- ------
1            Emp1       4
1            Emp2       3 
1            Emp3       5
1            Emp4       4
1            Emp5       4
2            Emp1       4
2            Emp2       1
2            Emp3       5
2            Emp4       5
2            Emp5       5
...
```
  
L’opérateur `UNPIVOT` n’est pas l’exact opposé de l’opérateur `PIVOT`. L’opérateur `PIVOT` effectue une agrégation et fusionne plusieurs lignes possibles en une ligne unique dans la sortie. L’opérateur `UNPIVOT` ne regénère pas le résultat de l’expression table d’origine après la fusion des lignes. En outre, les valeurs null dans l’entrée de `UNPIVOT` disparaissent dans la sortie. Lorsque les valeurs disparaissent, cela indique qu’il y avait peut-être des valeurs null d’origine dans l’entrée avant l’opération `PIVOT`.  
  
L’affichage `Sales.vSalesPersonSalesByFiscalYears` de l’exemple de base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] utilise l’opérateur `PIVOT` pour retourner le total des ventes de chaque vendeur, par exercice comptable. Pour générer le script de l’affichage dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], dans **l’Explorateur d’objets**, recherchez l’affichage dans le dossier **Vues** de la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]. Cliquez avec le bouton droit sur le nom de l’affichage, puis sélectionnez **Générer un script de la vue en tant que**.  
  
## <a name="see-also"></a>Voir aussi  
[FROM (Transact-SQL)](../../t-sql/queries/from-transact-sql.md)   
[CASE (Transact-SQL)](../../t-sql/language-elements/case-transact-sql.md)  
  
