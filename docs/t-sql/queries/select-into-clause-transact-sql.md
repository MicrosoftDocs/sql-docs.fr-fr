---
description: SELECT - Clause INTO (Transact-SQL)
title: INTO, clause (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/23/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- INTO_TSQL
- INSERT_INTO_TSQL
- INSERT INTO
- INTO
- INTO clause
- INTO_clause_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- copying data [SQL Server], into a new table
- INTO clause
- moving data, to a new table
- table creation [SQL Server], INTO clause
- SELECT INTO statement
- inserting rows
- clauses [SQL Server], INTO
- row additions [SQL Server], INTO clause
ms.assetid: b48d69e8-5a00-48bf-b2f3-19278a72dd88
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 3ce1740def1f7d2ebd70ad611f3b7b489ca04f99
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/01/2021
ms.locfileid: "99235855"
---
# <a name="select---into-clause-transact-sql"></a>SELECT - Clause INTO (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

SELECT...INTO crée une table dans le groupe de fichiers par défaut et y insère les lignes résultantes de la requête. Pour afficher la syntaxe SELECT complète, consultez [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md).  
  
![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
[ INTO new_table ]
[ ON filegroup ]
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *nouvelle_table*   
 Spécifie le nom d'une table à créer en fonction des colonnes de la liste de sélection et des lignes choisies à partir de la source de données.  
 
 Le format de *new_table* est déterminé par l’évaluation des expressions de la liste de sélection. Les colonnes de *new_table* sont créées dans l’ordre spécifié par la liste de sélection. Chaque colonne de *new_table* a le même nom, le même type de données, la même possibilité de valeur Null et la même valeur que l’expression correspondante dans la liste de sélection. La propriété IDENTITY d'une colonne est transférée sauf dans les conditions définies dans « Utilisation des colonnes d'identité » dans la section Remarques.  
  
 Pour créer la table dans une autre base de données de la même instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], spécifiez *new_table* comme nom complet sous la forme *database.schema.table_name*.  
  
 Vous ne pouvez pas créer *new_table* sur un serveur distant, mais vous pouvez remplir *new_table* à partir d’une source de données distante. Pour créer *new_table* à partir d’une table source distante, spécifiez la table source par un nom en quatre parties sous la forme *linked_server*.*catalog*.*schema*.*object* dans la clause FROM de l’instruction SELECT. Vous pouvez aussi utiliser la fonction [OPENQUERY](../../t-sql/functions/openquery-transact-sql.md) ou la fonction [OPENDATASOURCE](../../t-sql/functions/opendatasource-transact-sql.md) dans la clause FROM pour spécifier la source de données distante.  
 
 *groupe_fichiers*    
 Spécifie le nom du groupe de fichiers dans lequel créer la table. Si le groupe de fichiers spécifié n’existe pas dans la base de données, le moteur SQL Server lève une erreur.   
 
 **S’applique à :** [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP2 et versions ultérieures.
  
## <a name="data-types"></a>Types de données  
 L'attribut FILESTREAM n'est pas transféré dans la nouvelle table. Les objets BLOB FILESTREAM sont copiés et stockés dans la nouvelle table en tant qu’objets BLOB **varbinary(max)** . Sans l’attribut FILESTREAM, le type de données **varbinary(max)** est limité à 2 Go. Si un objet BLOB FILESTREAM dépasse cette valeur, l'erreur 7119 se déclenche et l'instruction s'arrête.  
  
 Lorsque vous sélectionnez une colonne d'identité existante dans une nouvelle table, la nouvelle colonne hérite de la propriété IDENTITY sauf si l'une des conditions suivantes est vraie :  
  
-   L'instruction SELECT contient une jointure.  
  
-   Plusieurs instructions SELECT sont reliées par UNION.  
  
-   La colonne d'identité est répertoriée plus d'une fois dans la liste de sélection.  
  
-   La colonne d'identité fait partie d'une expression.  
  
-   La colonne d'identité fait partie d'une source de données distante.  
  
Si l'une de ces conditions est vérifiée, la colonne est créée avec l'attribut NOT NULL au lieu d'hériter de la propriété IDENTITY. Si une colonne d'identité est requise dans la nouvelle table et si ce type de colonne n'est pas disponible, ou si vous voulez une valeur initiale ou une valeur d'incrément différente de la colonne d'identité source, définissez la colonne dans la liste de sélection à l'aide de la fonction IDENTITY. Consultez « Création d'une colonne d'identité à l'aide de la fonction IDENTITY » dans la section Exemples ci-dessous.  

## <a name="remarks"></a>Notes  
L’instruction `SELECT...INTO` s’exécute en deux temps : la nouvelle table est créée, puis les lignes sont insérées.  Cela signifie que, si les insertions échouent, elles sont toutes annulées, mais la nouvelle table (vide) est conservée.  Si l’opération doit réussir ou échouer dans sa globalité, utilisez une [transaction explicite](../language-elements/begin-transaction-transact-sql.md).
  
## <a name="limitations-and-restrictions"></a>Limitations et restrictions  
 Vous ne pouvez pas spécifier une variable de table ou un paramètre table en tant que nouvelle table.  
  
 Vous ne pouvez pas utiliser `SELECT...INTO` pour créer une table partitionnée, même quand la table source est partitionnée. `SELECT...INTO` n’utilise pas le schéma de partition de la table source ; à la place, la nouvelle table est créée dans le groupe de fichiers par défaut. Pour insérer des lignes dans une table partitionnée, vous devez d’abord créer la table partitionnée, puis utiliser l’instruction `INSERT INTO...SELECT...FROM`.  
  
 Les index, contraintes et déclencheurs définis dans la table source ne sont pas transférés dans la nouvelle table ; ils ne peuvent pas non plus être spécifiés dans l’instruction `SELECT...INTO`. Si ces objets sont nécessaires, vous pouvez les créer après avoir exécuté l’instruction `SELECT...INTO`.  
  
 La spécification d’une clause `ORDER BY` ne garantit pas que les lignes soient insérées dans l’ordre spécifié.  
  
 Lorsqu'une colonne éparse est comprise dans la liste de sélection, la propriété de colonne éparse n'est pas transférée à la colonne de la nouvelle table. Si cette propriété est obligatoire dans la nouvelle table, modifiez la définition de colonne après avoir exécuté l'instruction SELECT...INTO afin d'inclure cette propriété.  
  
 Lorsqu'une colonne calculée est comprise dans la liste de sélection, la colonne correspondante de la nouvelle table n'est pas une colonne calculée. Les valeurs de la nouvelle colonne sont les valeurs calculées au moment de l’exécution de l’instruction `SELECT...INTO`.  
  
## <a name="logging-behavior"></a>Comportement de journalisation  
 La quantité d’informations journalisées pour `SELECT...INTO` dépend du mode de récupération en vigueur pour la base de données. En mode de récupération simple ou en mode de récupération utilisant les journaux de transactions, les opérations de chargement en masse font l'objet d'une journalisation minimale. Avec une journalisation minimale, l’utilisation de l’instruction `SELECT...INTO` peut s’avérer plus efficace que la création d’une table et son remplissage avec une instruction INSERT. Pour plus d'informations, consultez [Journal des transactions &#40;SQL Server&#41;](../../relational-databases/logs/the-transaction-log-sql-server.md).
 
Les instructions `SELECT...INTO` qui contiennent des fonctions définies par l’utilisateur (UDF) sont des opérations entièrement journalisées. Si les fonctions définies par l’utilisateur utilisées dans l’instruction `SELECT...INTO` n’effectuent aucune opération d’accès aux données, vous pouvez spécifier la clause SCHEMABINDING pour les fonctions définies par l’utilisateur, qui définira la propriété UserDataAccess dérivée de ces fonctions définies par l’utilisateur sur 0. Après cette modification, les instructions `SELECT...INTO` sont journalisées de façon minimale. Si l’instruction `SELECT...INTO` fait toujours référence à au moins une fonction définie par l’utilisateur dont la propriété a la valeur 1, l’opération est entièrement journalisée.
  
## <a name="permissions"></a>Autorisations  
 Requiert l'autorisation CREATE TABLE dans la base de données de destination.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-creating-a-table-by-specifying-columns-from-multiple-sources"></a>R. Création d'une table en spécifiant des colonnes provenant de plusieurs sources  
 L'exemple suivant crée la table `dbo.EmployeeAddresses` dans la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] en sélectionnant sept colonnes de diverses tables liées aux salariés et aux adresses.  
  
```sql  
SELECT c.FirstName, c.LastName, e.JobTitle, a.AddressLine1, a.City,   
    sp.Name AS [State/Province], a.PostalCode  
INTO dbo.EmployeeAddresses  
FROM Person.Person AS c  
    JOIN HumanResources.Employee AS e   
    ON e.BusinessEntityID = c.BusinessEntityID  
    JOIN Person.BusinessEntityAddress AS bea  
    ON e.BusinessEntityID = bea.BusinessEntityID  
    JOIN Person.Address AS a  
    ON bea.AddressID = a.AddressID  
    JOIN Person.StateProvince as sp   
    ON sp.StateProvinceID = a.StateProvinceID;  
GO  
```  
  
### <a name="b-inserting-rows-using-minimal-logging"></a>B. Insertion de lignes en utilisant une journalisation minimale  
 L'exemple suivant crée la table `dbo.NewProducts` et insère des lignes provenant de la table `Production.Product`. L'exemple suppose que le mode de récupération de la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] a la valeur FULL. Pour garantir une journalisation minimale, le mode de récupération de la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] a la valeur BULK_LOGGED avant l'insertion des lignes ; il reprend ensuite la valeur FULL après l'utilisation de l'instruction SELECT...INTO. Ce processus permet de garantir que l'instruction SELECT...INTO utilise un espace minimal dans le journal des transactions et qu'elle s'exécute de manière efficace.  
  
```sql  
ALTER DATABASE AdventureWorks2012 SET RECOVERY BULK_LOGGED;  
GO  
  
SELECT * INTO dbo.NewProducts  
FROM Production.Product  
WHERE ListPrice > $25   
AND ListPrice < $100;  
GO  
ALTER DATABASE AdventureWorks2012 SET RECOVERY FULL;  
GO  
```  
  
### <a name="c-creating-an-identity-column-using-the-identity-function"></a>C. Création d'une colonne d'identité à l'aide de la fonction IDENTITY  
 L'exemple suivant utilise la fonction IDENTITY pour créer une colonne d'identité dans la nouvelle table `Person.USAddress` de la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. Cela est nécessaire, car l'instruction SELECT qui définit la table contient une jointure qui empêche le transfert de la propriété IDENTITY vers la nouvelle table. Notez que la valeur initiale et la valeur d'incrément spécifiées dans la fonction IDENTITY sont différentes de celles de la colonne `AddressID` dans la table source `Person.Address`.  
  
```sql  
-- Determine the IDENTITY status of the source column AddressID.  
SELECT OBJECT_NAME(object_id) AS TableName, name AS column_name, 
  is_identity, seed_value, increment_value  
FROM sys.identity_columns  
WHERE name = 'AddressID';  
  
-- Create a new table with columns from the existing table Person.Address. 
-- A new IDENTITY column is created by using the IDENTITY function.  
SELECT IDENTITY (int, 100, 5) AS AddressID,   
       a.AddressLine1, a.City, b.Name AS State, a.PostalCode  
INTO Person.USAddress   
FROM Person.Address AS a  
INNER JOIN Person.StateProvince AS b 
  ON a.StateProvinceID = b.StateProvinceID  
WHERE b.CountryRegionCode = N'US';   
  
-- Verify the IDENTITY status of the AddressID columns in both tables.  
SELECT OBJECT_NAME(object_id) AS TableName, name AS column_name, 
  is_identity, seed_value, increment_value  
FROM sys.identity_columns  
WHERE name = 'AddressID';  
```  
  
### <a name="d-creating-a-table-by-specifying-columns-from-a-remote-data-source"></a>D. Création d'une table en spécifiant des colonnes provenant d'une source de données distante  
 L'exemple suivant illustre l'utilisation de trois méthodes de création d'une table sur le serveur local à partir d'une source de données distante. L'exemple commence par créer un lien vers la source de données distante. Le nom du serveur lié, `MyLinkServer,`, est ensuite spécifié dans la clause FROM de la première instruction SELECT...INTO, ainsi que dans la fonction OPENQUERY de la deuxième instruction SELECT...INTO. La troisième instruction SELECT...INTO utilise la fonction OPENDATASOURCE, qui spécifie directement la source de données distante au lieu d'utiliser le nom du serveur lié.  
  
 **S’applique à :** [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures  
  
```sql
USE master;  
GO  
-- Create a link to the remote data source.   
-- Specify a valid server name for @datasrc as 'server_name' 
-- or 'server_name\instance_name'.  
EXEC sp_addlinkedserver @server = N'MyLinkServer',  
    @srvproduct = N' ',  
    @provider = N'SQLNCLI',   
    @datasrc = N'server_name',  
    @catalog = N'AdventureWorks2012';  
GO  

USE AdventureWorks2012;  
GO  
-- Specify the remote data source in the FROM clause using a four-part name   
-- in the form linked_server.catalog.schema.object.  
SELECT DepartmentID, Name, GroupName, ModifiedDate  
INTO dbo.Departments  
FROM MyLinkServer.AdventureWorks2012.HumanResources.Department  
GO  
-- Use the OPENQUERY function to access the remote data source.  
SELECT DepartmentID, Name, GroupName, ModifiedDate  
INTO dbo.DepartmentsUsingOpenQuery  
FROM OPENQUERY(MyLinkServer, 'SELECT *  
               FROM AdventureWorks2012.HumanResources.Department');   
GO  
-- Use the OPENDATASOURCE function to specify the remote data source.  
-- Specify a valid server name for Data Source using the format 
-- server_name or server_name\instance_name.  
SELECT DepartmentID, Name, GroupName, ModifiedDate  
INTO dbo.DepartmentsUsingOpenDataSource  
FROM OPENDATASOURCE('SQLNCLI',  
    'Data Source=server_name;Integrated Security=SSPI')  
    .AdventureWorks2012.HumanResources.Department;  
GO  
```  
  
### <a name="e-import-from-an-external-table-created-with-polybase"></a>E. Importer à partir d’une table externe créée avec PolyBase  
 Importez des données de Hadoop ou d’Azure Storage dans SQL Server à des fins de stockage permanent. Utilisez `SELECT INTO` pour importer des données référencées par une table externe en vue de leur stockage permanent dans SQL Server. Créez une table relationnelle à la volée, puis créez un index column-store en plus de la table.  
  
 **S’applique à :** [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)].  
  
```sql
-- Import data for car drivers into SQL Server to do more in-depth analysis.  
SELECT DISTINCT   
        Insured_Customers.FirstName, Insured_Customers.LastName,   
        Insured_Customers.YearlyIncome, Insured_Customers.MaritalStatus  
INTO Fast_Customers from Insured_Customers INNER JOIN   
(  
        SELECT * FROM CarSensor_Data where Speed > 35   
) AS SensorD  
ON Insured_Customers.CustomerKey = SensorD.CustomerKey  
ORDER BY YearlyIncome;  
```  
### <a name="f-copying-the-data-from-one-table-to-another-and-create-the-new-table-on-a-specified-filegroup"></a>F. Copier les données d’une table vers une autre et créer la nouvelle table sur un groupe de fichiers spécifié
L’exemple suivant illustre la création d’une table en tant que copie d’une autre table et son chargement dans un autre groupe de fichiers que le groupe de fichiers par défaut de l’utilisateur.

 **S’applique à :** [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP2 et versions ultérieures.

```sql
ALTER DATABASE [AdventureWorksDW2016] ADD FILEGROUP FG2;
ALTER DATABASE [AdventureWorksDW2016]
ADD FILE
(
NAME='FG2_Data',
FILENAME = '/var/opt/mssql/data/AdventureWorksDW2016_Data1.mdf'
)
TO FILEGROUP FG2;
GO
SELECT * INTO [dbo].[FactResellerSalesXL] ON FG2 FROM [dbo].[FactResellerSales];
```
  
## <a name="see-also"></a>Voir aussi  
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
 [Exemples SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-examples-transact-sql.md)   
 [INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/insert-transact-sql.md)   
 [IDENTITY &#40;Function&#41; &#40;Transact-SQL&#41;](../../t-sql/functions/identity-function-transact-sql.md)  
  
  
