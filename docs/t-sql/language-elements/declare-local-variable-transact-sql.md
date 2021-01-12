---
title: DECLARE @local_variable (Transact-SQL) | Microsoft Docs
description: Référence Transact-SQL sur l’utilisation de DECLARE pour définir des variables locales à utiliser dans un lot ou une procédure.
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DECLARE
- DECLARE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- table-valued parameters
- variables [SQL Server], declaring
- DECLARE statement
- declaring variables
ms.assetid: d1635ebb-f751-4de1-8bbc-cae161f90821
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ce70338a29d271151a0431a157290e5c09241f42
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98102489"
---
# <a name="declare-local_variable-transact-sql"></a>DECLARE @local_variable (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Les variables sont déclarées au sein d'un lot ou d'une procédure avec l'instruction DECLARE, et l'instruction SET ou SELECT leur affecte des valeurs. Les variables curseur peuvent être déclarées avec cette instruction, puis utilisées avec d'autres instructions liées aux curseurs. Après la déclaration, toutes les variables sont initialisées avec la valeur NULL., à moins qu'une valeur ne soit fournie dans le cadre de la déclaration.  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql  
-- Syntax for SQL Server and Azure SQL Database  
  
DECLARE   
{   
    { @local_variable [AS] data_type  [ = value ] }  
  | { @cursor_variable_name CURSOR }  
} [,...n]   
| { @table_variable_name [AS] <table_type_definition> }   
  
<table_type_definition> ::=   
     TABLE ( { <column_definition> | <table_constraint> } [ ,...n] )   
  
<column_definition> ::=   
     column_name { scalar_data_type | AS computed_column_expression }  
     [ COLLATE collation_name ]   
     [ [ DEFAULT constant_expression ] | IDENTITY [ (seed ,increment ) ] ]   
     [ ROWGUIDCOL ]   
     [ <column_constraint> ]   
  
<column_constraint> ::=   
     { [ NULL | NOT NULL ]   
     | [ PRIMARY KEY | UNIQUE ]   
     | CHECK ( logical_expression )   
     | WITH ( <index_option > )  
     }   
  
<table_constraint> ::=   
     { { PRIMARY KEY | UNIQUE } ( column_name [ ,...n] )   
     | CHECK ( search_condition )   
     }   
  
<index_option> ::=  
See CREATE TABLE for index option syntax.  
  
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
DECLARE   
{{ @local_variable [AS] data_type } [ =value [ COLLATE <collation_name> ] ] } [,...n]  
  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
@*local_variable*  
 Nom d'une variable. Les noms de variables doivent commencer par le signe @. Les noms de variables locales doivent être conformes aux règles applicables aux [identificateurs](../../relational-databases/databases/database-identifiers.md).  
  
*data_type*  
 Il s'agit de tout type de table CLR (Common Language Runtime) défini par l'utilisateur, fourni par le système, ou type de données alias. Une variable ne peut pas être de type **text**, **ntext** ou **image**.  
  
 Pour plus d’informations sur les types de données système, consultez [Types de données &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md). Pour plus d’informations sur les types de données CLR définis par l’utilisateur ou les types de données alias, consultez [CREATE TYPE &#40;Transact-SQL&#41;](../../t-sql/statements/create-type-transact-sql.md).  
  
 =*value*  
 Attribue une valeur à la variable en ligne. La valeur peut être une constante ou une expression, mais elle doit soit correspondre au type de déclaration de la variable, soit être implicitement convertible vers ce type. Pour plus d’informations, consultez [Expressions &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md).  
  
@*cursor_variable_name*  
 Nom d’une variable de curseur. Les noms de variables curseur doivent commencer par le signe @ et être conformes aux règles des identificateurs.  
  
CURSOR  
 Indique que la variable est une variable curseur locale.  
  
@*table_variable_name*  
 Nom d’une variable de type **table**. Les noms de variables doivent commencer par le signe @ et être conformes aux règles des identificateurs.  
  
<table_type_definition>  
Définit le type de données **table**. La déclaration de table inclut des définitions de colonnes, des noms, des types de données et des contraintes. Les seuls types de contraintes autorisés sont PRIMARY KEY, UNIQUE, NULL et CHECK. Un type de données alias ne peut pas être utilisé comme type de données scalaire de colonne si une règle ou une définition par défaut est liée au type.
  
\<table_type_definiton> est un sous-ensemble d’informations utilisé pour définir une table dans CREATE TABLE. Les éléments et les définitions essentielles sont inclus ici. Pour plus d’informations, consultez [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md).  
  
 *n*  
 Espace réservé indiquant que plusieurs variables peuvent être spécifiées et que des valeurs peuvent leur être affectées. Lorsque vous déclarez des variables **table**, seules les variables **table** doivent être déclarées dans l’instruction DECLARE.  
  
 *column_name*  
 Nom de la colonne dans la table.  
  
 *scalar_data_type*  
 Spécifie que la colonne est un type de données scalaire.  
  
 *computed_column_expression*  
 Expression définissant la valeur d’une colonne calculée. Elle est calculée à partir d'une expression qui utilise d'autres colonnes de la même table. Par exemple, une colonne calculée peut avoir la définition **cost** AS **price \* qty**. L’expression peut être un nom de colonne non calculée, une constante, une fonction intégrée ou une combinaison de ces éléments connectés par un ou plusieurs opérateurs. L'expression ne peut pas être une sous-requête ou une fonction définie par l'utilisateur. L'expression ne peut pas faire référence à un type CLR défini par l'utilisateur.  
  
 [ COLLATE *collation_name*]  
 Indique le classement de la colonne. *collation_name* peut être soit un nom de classement Windows, soit un nom de classement SQL et s’applique uniquement aux colonnes des types de données **char**, **varchar**, **text**, **nchar**, **nvarchar** et **ntext**. Si cette valeur n'est pas spécifiée, la colonne reçoit le classement du type de données utilisateur (si la colonne est de type de données utilisateur), ou le classement de la base de données active.  
  
 Pour plus d’informations sur les noms de classements Windows et SQL, consultez [COLLATE &#40;Transact-SQL&#41;](~/t-sql/statements/collations.md).  
  
 DEFAULT  
 Spécifie la valeur fournie pour la colonne lorsque vous n'avez pas spécifié explicitement de valeur lors d'une insertion. Les définitions DEFAULT peuvent être appliquées à n’importe quelle colonne, sauf celles définies en tant que **timestamp** ou celles dotées de la propriété IDENTITY. Les définitions de valeurs par défaut sont supprimées lorsque la table est supprimée. Seule une valeur constante, telle qu'une chaîne de caractères, une fonction système comme SYSTEM_USER() ou la valeur NULL, peut être utilisée comme valeur par défaut. Pour maintenir la compatibilité avec les versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], un nom de contrainte peut être affecté à une définition DEFAULT.  
  
 *constant_expression*  
 Constante, valeur NULL ou fonction système utilisée comme valeur par défaut pour une colonne.  
  
 IDENTITY  
 Indique que la nouvelle colonne est une colonne d'identité. Lorsqu'une nouvelle ligne est ajoutée à la table, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournit une valeur incrémentielle unique pour la colonne. Les colonnes d'identité sont normalement utilisées avec les contraintes PRIMARY KEY comme identificateur unique de ligne pour la table. La propriété IDENTITY peut être affectée à des colonnes **tinyint**, **smallint**, **int**, **decimal(p,0)** ou **numeric(p,0)** . Une seule colonne d'identité peut être créée par table. Il n'est pas possible d'utiliser des valeurs par défaut liées et des contraintes DEFAULT avec une colonne d'identité. Vous devez spécifier à la fois la valeur initiale et l'incrément, ou bien aucun des deux. Si vous n'en spécifiez aucun, la valeur par défaut est (1,1).  
  
 *seed*  
 Valeur utilisée pour la toute première ligne chargée dans la table.  
  
 *increment*  
 Valeur d’incrément ajoutée à la valeur d’identité de la ligne précédemment chargée.  
  
 ROWGUIDCOL  
 Indique que la nouvelle colonne est une colonne d'identificateur unique global de ligne. Une seule colonne **uniqueidentifier** par table peut être désignée comme colonne ROWGUIDCOL. La propriété ROWGUIDCOL ne peut être affectée qu’à une colonne **uniqueidentifier**.  
  
 NULL | NOT NULL  
 Indique si NULL est autorisé dans la variable. La valeur par défaut est NULL.  
  
 PRIMARY KEY  
 Contrainte appliquant l'intégrité d'entité pour une ou plusieurs colonnes données via un index unique. Une seule contrainte PRIMARY KEY peut être créée par table.  
  
 UNIQUE  
 Contrainte fournissant l'intégrité d'entité pour une ou plusieurs colonnes données via un index unique. Une table peut comprendre plusieurs contraintes UNIQUE.  
  
 CHECK  
 Contrainte qui assure l'intégrité du domaine en limitant les valeurs possibles pouvant être entrées dans une ou plusieurs colonnes.  
  
 *logical_expression*  
 Expression logique qui retourne TRUE ou FALSE.  
  
## <a name="remarks"></a>Notes  
 Les variables locales sont souvent utilisées dans un traitement ou une procédure comme compteurs pour une boucle WHILE, LOOP ou pour un bloc IF...ELSE.  
  
 Les variables ne peuvent être utilisées que dans des expressions et pas la place de noms d'objets ou de mots clés. Pour créer des instructions dynamiques SQL, utilisez EXECUTE.  
  
 La portée d'une variable correspond au traitement dans lequel elle est déclarée.  
 
 Une variable de table ne réside pas nécessairement en mémoire. En cas de sollicitation de la mémoire, les pages appartenant à une variable de table peuvent être envoyées (push) à tempdb.
  
 Une variable curseur à laquelle un curseur a été affecté peut être référencée en tant que source dans les instructions suivantes :  
  
-   Instruction CLOSE.  
  
-   Instruction DEALLOCATE.  
  
-   Instruction FETCH.  
  
-   Instruction OPEN.  
  
-   Instruction positionnée DELETE ou UPDATE.  
  
-   Instruction variable (sur le côté droit) SET CURSOR.  
  
 Dans toutes ces instructions, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] génère une erreur si une variable curseur référencée existe et qu'un curseur ne lui est pas alloué. Si une variable curseur référencée n'existe pas, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] génère la même erreur que pour une variable non déclarée d'un autre type.  
  
 Une variable de curseur :  
  
-   peut être la cible d'une autre type de curseur ou d'une autre variable curseur ; Pour plus d’informations, consultez [SET @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/set-local-variable-transact-sql.md).  
  
-   peut être référencée en tant que cible d'un paramètre de curseur de sortie dans une instruction EXECUTE si aucun curseur ne lui est actuellement affecté ;  
  
-   doit être considérée comme un pointeur vers le curseur.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-using-declare"></a>R. Utilisation de DECLARE  
 L'exemple suivant utilise une variable locale nommée `@find` pour extraire les informations de contact de tous les noms commençant par `Man`.  
  
```sql  
USE AdventureWorks2012;  
GO  
DECLARE @find VARCHAR(30);   
/* Also allowed:   
DECLARE @find VARCHAR(30) = 'Man%';   
*/  
SET @find = 'Man%';   
SELECT p.LastName, p.FirstName, ph.PhoneNumber  
FROM Person.Person AS p   
JOIN Person.PersonPhone AS ph ON p.BusinessEntityID = ph.BusinessEntityID  
WHERE LastName LIKE @find;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
LastName            FirstName               Phone
------------------- ----------------------- -------------------------
Manchepalli         Ajay                    1 (11) 500 555-0174
Manek               Parul                   1 (11) 500 555-0146
Manzanares          Tomas                   1 (11) 500 555-0178
  
(3 row(s) affected)
```  
  
### <a name="b-using-declare-with-two-variables"></a>B. Utilisation de DECLARE avec deux variables  
 L'exemple suivant extrait les noms des vendeurs de [!INCLUDE[ssSampleDBCoFull](../../includes/sssampledbcofull-md.md)] qui se trouvent sur le secteur de vente North American et qui génèrent un chiffre d'affaires annuel minimum de 2 000 000 de dollars.  
  
```sql  
USE AdventureWorks2012;  
GO  
SET NOCOUNT ON;  
GO  
DECLARE @Group nvarchar(50), @Sales MONEY;  
SET @Group = N'North America';  
SET @Sales = 2000000;  
SET NOCOUNT OFF;  
SELECT FirstName, LastName, SalesYTD  
FROM Sales.vSalesPerson  
WHERE TerritoryGroup = @Group and SalesYTD >= @Sales;  
```  
  
### <a name="c-declaring-a-variable-of-type-table"></a>C. Déclaration d'une variable de type table  
 L'exemple suivant crée une variable `table` qui stocke les valeurs définies dans la clause OUTPUT de l'instruction UPDATE. Deux instructions `SELECT` suivent ; elles retournent les valeurs dans `@MyTableVar`, ainsi que les résultats de la mise à jour dans la table `Employee`. Notez que les résultats dans la colonne `INSERTED.ModifiedDate` sont différents des valeurs de la colonne `ModifiedDate` dans la table `Employee`. Ceci s'explique par le fait que le déclencheur `AFTER UPDATE`, qui met à jour la valeur de `ModifiedDate` en fonction de la date actuelle, est défini sur la table `Employee`. Toutefois, les colonnes renvoyées par `OUTPUT` reflètent les données avant l'activation des déclencheurs. Pour plus d’informations, consultez [Clause OUTPUT &#40;Transact-SQL&#41;](../../t-sql/queries/output-clause-transact-sql.md).  
  
```sql  
USE AdventureWorks2012;  
GO  
DECLARE @MyTableVar TABLE(  
    EmpID INT NOT NULL,  
    OldVacationHours INT,  
    NewVacationHours INT,  
    ModifiedDate DATETIME);  
UPDATE TOP (10) HumanResources.Employee  
SET VacationHours = VacationHours * 1.25   
OUTPUT INSERTED.BusinessEntityID,  
       DELETED.VacationHours,  
       INSERTED.VacationHours,  
       INSERTED.ModifiedDate  
INTO @MyTableVar;  
--Display the result set of the table variable.  
SELECT EmpID, OldVacationHours, NewVacationHours, ModifiedDate  
FROM @MyTableVar;  
GO  
--Display the result set of the table.  
--Note that ModifiedDate reflects the value generated by an  
--AFTER UPDATE trigger.  
SELECT TOP (10) BusinessEntityID, VacationHours, ModifiedDate  
FROM HumanResources.Employee;  
GO  
```  
  
### <a name="d-declaring-a-variable-of-user-defined-table-type"></a>D. Déclaration d'une variable de type table défini par l'utilisateur  
 L'exemple suivant crée un paramètre table ou une variable de table portant le nom `@LocationTVP`. Cela nécessite un type de table défini par l'utilisateur correspondant appelé `LocationTableType`. Pour plus d’informations sur la création d’un type de table défini par l’utilisateur, consultez [CREATE TYPE &#40;Transact-SQL&#41;](../../t-sql/statements/create-type-transact-sql.md). Pour plus d’informations sur les paramètres table, consultez [Utiliser les paramètres table &#40;moteur de base de données&#41;](../../relational-databases/tables/use-table-valued-parameters-database-engine.md).  
  
```sql  
DECLARE @LocationTVP   
AS LocationTableType;  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Exemples : [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="e-using-declare"></a>E. Utilisation de DECLARE  
 L'exemple suivant utilise une variable locale nommée `@find` pour extraire les informations de contact de tous les noms commençant par `Walt`.  
  
```sql  
-- Uses AdventureWorks  
  
DECLARE @find VARCHAR(30);  
/* Also allowed:   
DECLARE @find VARCHAR(30) = 'Man%';  
*/  
SET @find = 'Walt%';  
  
SELECT LastName, FirstName, Phone  
FROM DimEmployee   
WHERE LastName LIKE @find;  
```  
  
### <a name="f-using-declare-with-two-variables"></a>F. Utilisation de DECLARE avec deux variables  
 L’exemple suivant récupère des variables pour spécifier les prénoms et noms des employés dans la table `DimEmployee`.  
  
```sql  
-- Uses AdventureWorks  
  
DECLARE @lastName VARCHAR(30), @firstName VARCHAR(30);  
  
SET @lastName = 'Walt%';  
SET @firstName = 'Bryan';  
  
SELECT LastName, FirstName, Phone  
FROM DimEmployee   
WHERE LastName LIKE @lastName AND FirstName LIKE @firstName;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [EXECUTE &#40;Transact-SQL&#41;](../../t-sql/language-elements/execute-transact-sql.md)   
 [Fonctions intégrées &#40;Transact-SQL&#41;](~/t-sql/functions/functions.md)   
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
 [table &#40;Transact-SQL&#41;](../../t-sql/data-types/table-transact-sql.md)   
 [Comparer du XML typé et du XML non typé](../../relational-databases/xml/compare-typed-xml-to-untyped-xml.md)  
  
  




