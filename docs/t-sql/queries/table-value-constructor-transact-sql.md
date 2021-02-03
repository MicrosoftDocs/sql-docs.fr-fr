---
description: Constructeur de valeurs de table (Transact-SQL)
title: Constructeur de valeurs de table (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/23/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- inserting multiple rows
- row value expression
- row constructor [SQL Server]
- table value constructor [SQL Server]
ms.assetid: e57cd31d-140e-422f-8178-2761c27b9deb
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 32074a8c0229ace53d636f8ef9bb31b8caf729b8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200433"
---
# <a name="table-value-constructor-transact-sql"></a>Constructeur de valeurs de table (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Spécifie un ensemble d'expressions de valeurs de ligne à créer dans une table. Le constructeur de valeurs de table [!INCLUDE[tsql](../../includes/tsql-md.md)] permet de spécifier plusieurs lignes de données dans une seule instruction DML. Le constructeur de valeurs de table peut être spécifié en tant que clause VALUES d’une instruction INSERT ... VALUES ou que table dérivée dans la clause USING de l’instruction MERGE ou dans la clause FROM.
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
VALUES ( <row value expression list> ) [ ,...n ]   
  
<row value expression list> ::=  
    {<row value expression> } [ ,...n ]  
  
<row value expression> ::=  
    { DEFAULT | NULL | expression }  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 VALUES  
 Introduit les listes d'expressions de valeurs de ligne. Chaque liste doit être placée entre parenthèses et séparée par une virgule.  
  
 Le nombre de valeurs spécifiées doit être identique dans chaque liste et les valeurs doivent être dans le même ordre que les colonnes de la table. Une valeur doit être spécifiée pour chaque colonne de la table ou la liste de colonnes doit spécifier explicitement les colonnes pour chaque valeur entrante.  
  
 DEFAULT  
 Force le [!INCLUDE[ssDE](../../includes/ssde-md.md)] à insérer la valeur par défaut définie pour une colonne. S'il n'existe pas de valeur par défaut pour la colonne et si celle-ci autorise les valeurs NULL, NULL est inséré. DEFAULT n'est pas valide pour une colonne d'identité. Lorsqu'il est spécifié dans un constructeur de valeurs de table, DEFAULT est autorisé uniquement dans une instruction INSERT.  
  
 *expression*  
 Constante, variable ou expression. L'expression ne peut pas contenir d'instruction EXECUTE.  
  
## <a name="limitations-and-restrictions"></a>Limitations et restrictions  
 Dans le cadre d’une table dérivée, aucune limite ne s’applique au nombre de lignes.  
 
 Dans le cadre de la clause VALUES d’une instruction INSERT ... VALUES, il existe une limite de 1 000 lignes. L’erreur 10738 est retournée si le nombre de lignes dépasse la valeur maximale. Pour insérer plus de 1 000 lignes, utilisez une des méthodes suivantes :  
  
- Créer plusieurs instructions INSERT  
  
- Utiliser une table dérivée  
  
- Importer les données en bloc à l’aide de l’[utilitaire **bcp**](../../tools/bcp-utility.md), de la [classe SqlBulkCopy](/dotnet/api/system.data.sqlclient.sqlbulkcopy) .NET, d’[OPENROWSET (BULK ...)](../../t-sql/functions/openrowset-transact-sql.md) ou de l’instruction [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md)  
  
 Seules les valeurs scalaires uniques sont autorisées en tant qu'expression de valeurs de ligne. Une sous-requête qui implique plusieurs colonnes n'est pas autorisée en tant qu'expression de valeurs de ligne. Par exemple, le code suivant génère une erreur de syntaxe car la troisième liste d'expressions de valeurs de ligne contient une sous-requête avec plusieurs colonnes.  
  
```sql
USE AdventureWorks2012;  
GO  
CREATE TABLE dbo.MyProducts (Name VARCHAR(50), ListPrice MONEY);  
GO  
-- This statement fails because the third values list contains multiple columns in the subquery.  
INSERT INTO dbo.MyProducts (Name, ListPrice)  
VALUES ('Helmet', 25.50),  
       ('Wheel', 30.00),  
       (SELECT Name, ListPrice FROM Production.Product WHERE ProductID = 720);  
GO  
```  
  
 Toutefois, l'instruction peut être réécrite en spécifiant séparément chaque colonne dans la sous-requête. L'exemple suivant insère correctement trois lignes dans la table `MyProducts`.  
  
```sql
INSERT INTO dbo.MyProducts (Name, ListPrice)  
VALUES ('Helmet', 25.50),  
       ('Wheel', 30.00),  
       ((SELECT Name FROM Production.Product WHERE ProductID = 720),  
        (SELECT ListPrice FROM Production.Product WHERE ProductID = 720));  
GO  
```  
  
## <a name="data-types"></a>Types de données  
 Les valeurs spécifiées dans une instruction INSERT portant sur plusieurs lignes respectent les propriétés de conversion de type de données de la syntaxe UNION ALL. Par conséquent, les types incompatibles sont convertis implicitement vers le type ayant la [précédence](../../t-sql/data-types/data-type-precedence-transact-sql.md) la plus élevée. Si la conversion n'est pas prise en charge en tant que conversion implicite, une erreur est renvoyée. Par exemple, l’instruction suivante insère un nombre entier et un caractère dans une colonne de type **char**.  
  
```sql
CREATE TABLE dbo.t (a INT, b CHAR);  
GO  
INSERT INTO dbo.t VALUES (1,'a'), (2, 1);  
GO  
```  
  
 Lorsque l'instruction INSERT est exécutée, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tente de convertir « a » en un nombre entier, car la précédence des types de données indique qu'un nombre entier est prioritaire par rapport à un caractère. La conversion échoue et une erreur est retournée. Vous pouvez éviter cette erreur en utilisant la conversion explicite des valeurs. Par exemple, l'instruction précédente peut être écrite de la façon suivante :  
  
```sql
INSERT INTO dbo.t VALUES (1,'a'), (2, CONVERT(CHAR,1));  
```  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-inserting-multiple-rows-of-data"></a>R. Insertion de plusieurs lignes de données  
 L'exemple suivant crée la table `dbo.Departments`, puis utilise le constructeur de valeurs de table pour insérer cinq lignes dans la table. Étant donné que les valeurs de toutes les colonnes sont fournies et qu'elles sont répertoriées dans le même ordre que les colonnes de la table, il n'est pas nécessaire de spécifier les noms de colonnes dans la liste de colonnes.  
  
```sql
USE AdventureWorks2012;  
GO  
INSERT INTO Production.UnitMeasure  
VALUES (N'FT2', N'Square Feet ', '20080923'), (N'Y', N'Yards', '20080923'),
       (N'Y3', N'Cubic Yards', '20080923');  
GO  
```  
  
### <a name="b-inserting-multiple-rows-with-default-and-null-values"></a>B. Insertion de plusieurs lignes avec les valeurs DEFAULT et NULL  
 L'exemple suivant illustre la spécification de DEFAULT et de NULL lors de l'utilisation du constructeur de valeurs de table pour insérer des lignes dans une table.  
  
```sql
USE AdventureWorks2012;  
GO  
CREATE TABLE Sales.MySalesReason(  
SalesReasonID int IDENTITY(1,1) NOT NULL,  
Name dbo.Name NULL ,  
ReasonType dbo.Name NOT NULL DEFAULT 'Not Applicable' );  
GO  
INSERT INTO Sales.MySalesReason   
VALUES ('Recommendation','Other'), ('Advertisement', DEFAULT), (NULL, 'Promotion');  
  
SELECT * FROM Sales.MySalesReason;  
```  
  
### <a name="c-specifying-multiple-values-as-a-derived-table-in-a-from-clause"></a>C. Spécification de plusieurs valeurs sous forme de table dérivée dans une clause FROM  
 Les exemples suivants utilisent le constructeur de valeurs de table pour spécifier plusieurs valeurs dans la clause FROM d’une instruction SELECT.  
  
```sql
SELECT a, b FROM (VALUES (1, 2), (3, 4), (5, 6), (7, 8), (9, 10) ) AS MyTable(a, b);  
GO  
-- Used in an inner join to specify values to return.  
SELECT ProductID, a.Name, Color  
FROM Production.Product AS a  
INNER JOIN (VALUES ('Blade'), ('Crown Race'), ('AWC Logo Cap')) AS b(Name)   
ON a.Name = b.Name;  
```  
  
### <a name="d-specifying-multiple-values-as-a-derived-source-table-in-a-merge-statement"></a>D. Spécification de plusieurs valeurs sous forme de table source dérivée dans une instruction MERGE  
 L'exemple suivant utilise l'instruction MERGE pour modifier la table `SalesReason` en mettant à jour ou en insérant des lignes. Lorsque la valeur de `NewName` dans la table source correspond à une valeur de la colonne `Name` dans la table cible, (`SalesReason`), la colonne `ReasonType` est mise à jour dans la table cible. Lorsque la valeur de `NewName` ne correspond à aucune autre valeur, la ligne source est insérée dans la table cible. La table source est une table dérivée qui utilise le constructeur de valeurs de table [!INCLUDE[tsql](../../includes/tsql-md.md)] afin de spécifier plusieurs lignes pour la table source.  
  
```sql
USE AdventureWorks2012;  
GO  
-- Create a temporary table variable to hold the output actions.  
DECLARE @SummaryOfChanges TABLE(Change VARCHAR(20));  
  
MERGE INTO Sales.SalesReason AS Target  
USING (VALUES ('Recommendation','Other'), ('Review', 'Marketing'), ('Internet', 'Promotion'))  
       AS Source (NewName, NewReasonType)  
ON Target.Name = Source.NewName  
WHEN MATCHED THEN  
UPDATE SET ReasonType = Source.NewReasonType  
WHEN NOT MATCHED BY TARGET THEN  
INSERT (Name, ReasonType) VALUES (NewName, NewReasonType)  
OUTPUT $action INTO @SummaryOfChanges;  
  
-- Query the results of the table variable.  
SELECT Change, COUNT(*) AS CountPerChange  
FROM @SummaryOfChanges  
GROUP BY Change;  
```  
  
### <a name="e-inserting-more-than-1000-rows"></a>E. Insertion de plus de 1 000 lignes
  L’exemple suivant illustre l’utilisation du constructeur de valeurs de table en tant que table dérivée. Il est ainsi possible d’insérer plus de 1 000 lignes à partir d’un seul constructeur de valeurs de table.
  
```sql
CREATE TABLE dbo.Test ([Value] INT);  
  
INSERT INTO dbo.Test ([Value])  
  SELECT drvd.[NewVal]
  FROM   (VALUES (0), (1), (2), (3), ..., (5000)) drvd([NewVal]);
```  


## <a name="see-also"></a>Voir aussi  
 [INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/insert-transact-sql.md)   
 [MERGE &#40;Transact-SQL&#41;](../../t-sql/statements/merge-transact-sql.md)   
 [FROM &#40;Transact-SQL&#41;](../../t-sql/queries/from-transact-sql.md)  
  
  
