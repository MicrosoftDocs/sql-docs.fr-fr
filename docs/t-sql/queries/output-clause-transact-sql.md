---
description: Clause OUTPUT (Transact-SQL)
title: OUTPUT, clause (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/14/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- OUTPUT_TSQL
- OUTPUT
dev_langs:
- TSQL
helpviewer_keywords:
- displaying updated rows
- INSERT statement [SQL Server], OUTPUT clause
- outputs [SQL Server]
- OUTPUT clause
- row additions [SQL Server], OUTPUT clause
- viewing updated rows
- row deletions [SQL Server], OUTPUT clause
- viewing deleted rows
- DELETE statement [SQL Server], OUTPUT clause
- row updates [SQL Server]
- displaying inserted rows
- viewing inserted rows
- displaying deleted rows
- UPDATE statement [SQL Server], OUTPUT clause
ms.assetid: 41b9962c-0c71-4227-80a0-08fdc19f5fe4
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: f248f7a26dee135cbb68d7d0660ea8e467f76d15
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207664"
---
# <a name="output-clause-transact-sql"></a>Clause OUTPUT (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Retourne des informations ou des expressions basées sur toutes les lignes affectées par une instruction INSERT, UPDATE, DELETE ou MERGE. Ces résultats peuvent être retournés à l'application en cours de traitement afin d'être utilisés notamment avec des messages de confirmation, des opérations d'archivage et d'autres spécifications d'application similaires. Il est également possible d'insérer ces résultats dans une table ou dans une variable de table. En outre, vous pouvez capturer les résultats d'une clause OUTPUT dans une instruction imbriquée INSERT, UPDATE, DELETE ou MERGE et les insérer dans une table ou une vue cible.  
  
> [!NOTE]  
>  Une instruction UPDATE, INSERT ou DELETE qui a une clause OUTPUT retourne des lignes au client même si l'instruction rencontre des erreurs et est annulée. Le résultat ne doit pas être utilisé si des erreurs se produisent lorsque vous exécutez l'instruction.  
  
 **Utilisation dans :**  
  
 [DELETE](../../t-sql/statements/delete-transact-sql.md)  
  
 [INSERT](../../t-sql/statements/insert-transact-sql.md)  
  
 [UPDATE](../../t-sql/queries/update-transact-sql.md)  
  
 [MERGE](../../t-sql/statements/merge-transact-sql.md)  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
<OUTPUT_CLAUSE> ::=  
{  
    [ OUTPUT <dml_select_list> INTO { @table_variable | output_table } [ ( column_list ) ] ]  
    [ OUTPUT <dml_select_list> ]  
}  
<dml_select_list> ::=  
{ <column_name> | scalar_expression } [ [AS] column_alias_identifier ]  
    [ ,...n ]  
  
<column_name> ::=  
{ DELETED | INSERTED | from_table_name } . { * | column_name }  
    | $action
```
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 \@*table_variable*  
 Spécifie une variable **table** dans laquelle les lignes retournées sont insérées au lieu d’être retournées à l’appelant. \@*table_variable* doit être déclaré avant les instructions INSERT, UPDATE, DELETE et MERGE.  
  
 Si *column_list* n’est pas spécifié, la variable **table** doit contenir le même nombre de colonnes que le jeu de résultats OUTPUT. Les exceptions sont des colonnes calculées et des colonnes d'identité qui doivent être ignorées. Si *column_list* est spécifié, toutes les colonnes ignorées doivent autoriser des valeurs NULL ou avoir des valeurs par défaut.  
  
 Pour plus d’informations sur les variables **table**, consultez [table &#40;Transact-SQL&#41;](../../t-sql/data-types/table-transact-sql.md).  
  
 *output_table*  
 Spécifie une table dans laquelle les lignes retournées sont insérées au lieu d'être retournées à l'appelant. *output_table* peut être une table temporaire.  
  
 Si *column_list* n’est pas spécifié, la table doit contenir le même nombre de colonnes que le jeu de résultats OUTPUT. Les exceptions sont des colonnes calculées et des colonnes d'identité. Elles doivent être ignorées. Si *column_list* est spécifié, toutes les colonnes ignorées doivent autoriser des valeurs NULL ou avoir des valeurs par défaut.  
  
 *output_table* ne peut pas :  
  
-   Avoir de déclencheurs activés.  
  
-   Participer aux côtés d'une contrainte FOREIGN KEY.  
  
-   Avoir des contraintes CHECK ou des règles activées.  
  
*column_list*  
 Liste facultative de noms de colonne sur la table cible de la clause INTO. Elle est analogue à la liste de colonnes autorisée dans l’instruction [INSERT](../../t-sql/statements/insert-transact-sql.md).  
  
 *scalar_expression*  
 Combinaison de symboles et d'opérateurs qui a pour résultat une seule valeur. Les fonctions d’agrégation ne sont pas autorisées dans *scalar_expression*.  
  
 Toute référence aux colonnes dans la table en cours de modification doit être qualifiée avec le préfixe INSERTED ou DELETED.  
  
 *column_alias_identifier*  
 Autre nom utilisé pour faire référence au nom de colonne.  
  
 DELETED  
 Préfixe de colonne qui spécifie la valeur supprimée par les opérations de mise à jour ou de suppression. Les colonnes dont le préfixe est DELETED reflètent la valeur avant la fin de l'instruction UPDATE, DELETE ou MERGE.  
  
 Le préfixe DELETED ne peut pas être utilisé avec la clause OUTPUT dans l'instruction INSERT.  
  
 INSERTED  
 Préfixe de colonne qui spécifie la valeur ajoutée par les opérations d'insertion ou de mise à jour. Les colonnes dont le préfixe est INSERTED reflètent la valeur après la fin de l'instruction UPDATE, INSERT ou MERGE mais avant l'exécution des déclencheurs.  
  
 Le préfixe INSERTED ne peut pas être utilisé avec la clause OUTPUT dans l'instruction DELETE.  
  
 *from_table_name*  
 Préfixe de colonne qui spécifie une table incluse dans la clause FROM d'une instruction DELETE, UPDATE ou MERGE qui est utilisée pour spécifier les lignes à mettre à jour ou à supprimer.  
  
 Si la table en cours de modification est également spécifiée dans la clause FROM, toutes les références aux colonnes de cette table doivent être qualifiées avec le préfixe INSERTED ou DELETED.  
  
 \*  
 Spécifie que toutes les colonnes affectées par les opérations de suppression, d'insertion ou de mise à jour sont retournées dans leur ordre d'apparition dans la table.  
  
 Par exemple, dans l'instruction DELETE qui suit, `OUTPUT DELETED.*` retourne toutes les colonnes supprimées de la table `ShoppingCartItem` :  
  
```sql
DELETE Sales.ShoppingCartItem
    OUTPUT DELETED.*;
```
  
 *column_name*  
 Référence de colonne explicite. Toutes les références à la table en cours de modification doivent être qualifiées correctement par le préfixe INSERTED ou DELETED, le cas échéant. Exemple : INSERTED **.** _column\_name_.  
  
 $action  
 Disponible uniquement pour l'instruction MERGE. Spécifie une colonne de type **nvarchar(10)** dans la clause OUTPUT d'une instruction MERGE qui retourne l'une des trois valeurs suivantes pour chaque ligne : « INSERT », « UPDATE » ou « DELETE », en fonction de l’action qui a été effectuée sur cette ligne.  
  
## <a name="remarks"></a>Notes  
 Les clauses OUTPUT \<dml_select_list> et OUTPUT \<dml_select_list> INTO { **\@** _variable\_table_ | _table\_sortie_ } peuvent être définies dans une seule instruction INSERT, UPDATE, DELETE ou MERGE.  
  
> [!NOTE]  
>  Sauf indication contraire, les références à la clause OUTPUT sont relatives aux clauses OUTPUT et OUTPUT INTO.  
  
 La clause OUTPUT est utile pour extraire la valeur d'identité ou les colonnes calculées après une opération INSERT ou UPDATE  
  
 Quand une colonne calculée est incluse dans \<dml_select_list>, la colonne correspondante dans la table de sortie ou la variable de table n’est pas une colonne calculée. Les valeurs de la nouvelle colonne sont les valeurs calculées lors de l'exécution de l'instruction.  
  
 Vous n'avez aucune garantie que l'ordre d'application des modifications dans la table et l'ordre d'insertion des lignes dans la table de sortie ou la variable de table correspondent.  
  
 Si les paramètres ou les variables sont modifiés dans le cadre d'une instruction UPDATE, la clause OUTPUT retourne toujours la valeur du paramètre ou de la variable, telle qu'elle était avant l'exécution de l'instruction et non après sa modification.  
  
 Vous pouvez utiliser l'instruction OUTPUT avec l'instruction UPDATE ou DELETE placée sur un curseur utilisant la syntaxe WHERE CURRENT OF.  
  
 La clause OUTPUT n'est pas prise en charge dans les instructions suivantes :  
  
-   Instructions DML faisant référence aux vues partitionnées locales, aux vues partitionnées distribuées ou aux tables distantes.  
  
-   Instructions INSERT contenant une instruction EXECUTE.  
  
-   Les prédicats de texte intégral ne sont pas autorisés dans la clause OUTPUT lorsque le niveau de compatibilité de la base de données a la valeur 100.  
  
-   La clause OUTPUT INTO ne peut pas être utilisée pour insérer une vue ou une fonction d'ensemble de lignes.  
  
-   Une fonction définie par l’utilisateur ne peut pas être créée si elle contient une clause OUTPUT INTO ayant une table comme cible.  
  
 Pour empêcher tout comportement non déterministe, la clause OUTPUT ne peut pas contenir les références suivantes :  
  
-   Les sous-requêtes ou les fonctions définies par l'utilisateur qui permettent d'accéder à des données utilisateur ou système, ou qui sont supposées permettre d'y accéder. Les fonctions définies par l'utilisateur sont supposées permettre d'accéder à des données si elles ne sont pas liées au schéma.  
  
-   Une colonne d'une vue ou fonction de table inline lorsque cette colonne est définie par l'une des méthodes suivantes :  
  
    -   Une sous-requête.  
  
    -   Une fonction définie par l'utilisateur qui offre un accès à des données utilisateur ou système, ou qui est supposée permettre d'y accéder.  
  
    -   Une colonne calculée qui contient une fonction définie par l’utilisateur qui effectue un accès aux données utilisateur ou système dans sa définition.  
  
     Quand [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] détecte une telle colonne dans la clause OUTPUT, l’erreur 4186 est générée.   
  
## <a name="inserting-data-returned-from-an-output-clause-into-a-table"></a>Insertion de données retournées à partir d'une clause OUTPUT dans une table  
 Lorsque vous capturez les résultats d'une clause OUTPUT dans une instruction INSERT, UPDATE, DELETE ou MERGE imbriquée et insérez ces résultats dans une table cible, n'oubliez pas les informations suivantes :  
  
-   L'opération entière est atomique. Soit l’instruction INSERT et l’instruction DML imbriquée qui contient la clause OUTPUT s’exécutent toutes les deux, soit l’instruction entière échoue.  
  
-   Les restrictions suivantes s'appliquent à la cible de l'instruction INSERT externe :  
  
    -   La cible ne peut pas être une expression de table commune, une vue ou une table distante.  
  
    -   La cible ne peut pas avoir de contrainte FOREIGN KEY ou être référencée par une contrainte FOREIGN KEY.  
  
    -   Il n’est pas possible de définir des déclencheurs sur la cible.  
  
    -   La cible ne peut pas participer à la réplication de fusion ou à des abonnements pouvant être mis à jour pour la réplication transactionnelle.  
  
-   Les restrictions suivantes s'appliquent à l'instruction DML imbriquée :  
  
    -   La cible ne peut pas être une table distante ou une vue partitionnée.  
  
    -   La source elle-même ne peut pas contenir de clause \<dml_table_source>.  
  
-   La clause OUTPUT INTO n’est pas prise en charge dans les instructions INSERT qui contiennent une clause \<dml_table_source>.  
  
-   \@\@ROWCOUNT retourne uniquement les lignes insérées par l'instruction INSERT externe.  
  
-   \@\@IDENTITY, SCOPE_IDENTITY et IDENT_CURRENT retournent les valeurs d'identité générées uniquement par l'instruction DML imbriquée, et non celles générées par l'instruction INSERT externe.  
  
-   Les notifications de requête traitent l'instruction comme une entité unique, et le type de tout message créé sera le type de l'instruction DML imbriquée, même si la modification significative provient de l'instruction INSERT externe elle-même.  
  
-   Dans la clause \<dml_table_source>, les clauses SELECT et WHERE ne peuvent pas inclure de sous-requêtes, de fonctions d’agrégation, de fonctions de classement, de prédicats de texte intégral, de fonctions définies par l’utilisateur pour l’accès aux données ou la fonction TEXTPTR.  

## <a name="parallelism"></a>Parallélisme
 Une clause OUTPUT qui retourne des résultats au client utilise toujours un plan en série.

Dans le contexte d’une base de données définie à un niveau de compatibilité de 130 ou supérieur, si une opération INSERT...SELECT utilise un indicateur WITH (TABLOCK) pour l’instruction SELECT et utilise également l’instruction OUTPUT...INTO pour insérer les résultats dans une table temporaire ou une table utilisateur, la table cible pour l’instruction INSERT...SELECT est éligible pour le parallélisme en fonction du coût des sous-arborescences.  La table cible référencée dans la clause OUTPUT INTO n’est pas éligible pour le parallélisme. 
 
## <a name="triggers"></a>Déclencheurs  
 Les colonnes retournées depuis l'instruction OUTPUT illustrent les données, telles qu'elles sont après la fin de l'instruction INSERT, UPDATE ou DELETE mais avant l'exécution des déclencheurs.  
  
 Pour les déclencheurs INSTEAD OF, les résultats retournés sont produits, comme si l'instruction INSERT, UPDATE ou DELETE avait eu lieu, même en cas d'absence de modifications suite à l'opération des déclencheurs. Si une instruction comprenant une clause OUTPUT est utilisée à l'intérieur du corps d'un déclencheur, les alias de table doivent être utilisés pour faire référence aux tables inserted et deleted du déclencheur, afin d'éviter des références de colonne en double avec les tables INSERTED et DELETED associées à OUTPUT.  
  
 Si la clause OUTPUT est spécifiée sans mot clé INTO, la cible de l'opération DML ne peut pas comporter de déclencheur activé pour l'action DML en question. Par exemple, si la clause OUTPUT est définie dans une instruction UPDATE, la table cible ne peut pas comporter de déclencheurs UPDATE activés.  
  
 Si l'option d'interdiction des résultats des déclencheurs sp_configure est définie, une clause OUTPUT sans clause INTO provoque l'échec de l'instruction lors de son appel à partir d'un déclencheur.  
  
## <a name="data-types"></a>Types de données  
 La clause OUTPUT prend en charge les types d’objets LOB suivants : **nvarchar(max)** , **varchar(max)** , **varbinary(max)** , **text**, **ntext**, **image** et **xml**. Quand vous utilisez la clause .WRITE dans l’instruction UPDATE pour modifier une colonne **nvarchar(max)** , **varchar(max)** ou **varbinary(max)** , les images complètes avant et après les valeurs sont retournées si elles sont référencées. La fonction TEXTPTR( ) ne peut pas être utilisée dans une expression sur une colonne **text**, **ntext** ou **image** dans la clause OUTPUT.  
  
## <a name="queues"></a>Files d’attente  
 Vous pouvez utiliser OUTPUT dans les applications utilisant les tables comme files d'attente ou garder les jeux de résultats intermédiaires. En d'autres termes, l'application ajoute ou supprime constamment des lignes de la table. L'exemple suivant utilise la clause OUTPUT dans une instruction DELETE afin de retourner la ligne supprimée à l'application appelante.  
  
```sql
USE AdventureWorks2012;
GO

DELETE TOP(1) dbo.DatabaseLog WITH (READPAST)  
OUTPUT deleted.*  
WHERE DatabaseLogID = 7;  
GO
```
  
 Cet exemple supprime une ligne d'une table utilisée comme file d'attente et retourne les valeurs supprimées à l'application de traitement en une seule fois. D'autres sémantiques peuvent également être mises en place, notamment l'utilisation d'une table pour implémenter une pile. Cependant, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne garantit pas l'ordre de traitement et de retour des lignes par les instructions DML à l'aide de la clause OUTPUT. C'est à l'application d'inclure une clause WHERE adéquate pour obtenir les sémantiques souhaitées ou de comprendre qu'aucun ordre n'est garanti lorsque plusieurs lignes sont qualifiées pour l'opération DML. L'exemple suivant utilise une sous-requête et suppose que le caractère unique est une caractéristique de la colonne `DatabaseLogID`, afin de mettre en place les sémantiques de classement souhaitées.  
  
```sql
USE tempdb;
GO

CREATE TABLE dbo.table1  
(  
    id INT,  
    employee VARCHAR(32)  
);  
GO  
  
INSERT INTO dbo.table1 VALUES   
      (1, 'Fred')  
     ,(2, 'Tom')  
     ,(3, 'Sally')  
     ,(4, 'Alice');  
GO  
  
DECLARE @MyTableVar TABLE  
(  
    id INT,  
    employee VARCHAR(32)  
);  
  
PRINT 'table1, before delete'   
SELECT * FROM dbo.table1;  
  
DELETE FROM dbo.table1  
OUTPUT DELETED.* INTO @MyTableVar  
WHERE id = 4 OR id = 2;  
  
PRINT 'table1, after delete'  
SELECT * FROM dbo.table1;  
  
PRINT '@MyTableVar, after delete'  
SELECT * FROM @MyTableVar;  
  
DROP TABLE dbo.table1;  
```

```
--Results  
--table1, before delete  
--id          employee  
------------- ------------------------------  
--1           Fred  
--2           Tom  
--3           Sally  
--4           Alice  
--  
--table1, after delete  
--id          employee  
------------- ------------------------------  
--1           Fred  
--3           Sally  
--@MyTableVar, after delete  
--id          employee  
------------- ------------------------------  
--2           Tom  
--4           Alice
```
  
> [!NOTE]  
>  Utilisez l'indicateur de table READPAST dans les instructions UPDATE et DELETE si votre scénario permet à plusieurs applications d'exécuter une lecture destructive à partir d'une table. Ce cas de figure empêche les problèmes de verrouillage qui peuvent survenir si une autre application est déjà entrain de lire le premier enregistrement dans la table.  
  
## <a name="permissions"></a>Autorisations  
 Les autorisations SELECT sont obligatoires sur toutes les colonnes récupérées par le biais de \<dml_select_list> et celles utilisées dans \<scalar_expression>.  
  
 Les autorisations INSERT sont obligatoires sur toutes les tables spécifiées dans \<output_table>.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-using-output-into-with-a-simple-insert-statement"></a>R. Utilisation de OUTPUT INTO avec une instruction INSERT simple  
 L’exemple suivant insère une ligne dans la table `ScrapReason` et utilise la clause `OUTPUT` pour retourner les résultats de l’instruction à la variable `@MyTableVar``table`. Étant donné que la colonne `ScrapReasonID` est définie avec une propriété IDENTITY, aucune valeur n'est spécifiée dans l'instruction `INSERT` pour cette colonne. Cependant, notez que la valeur générée par le [!INCLUDE[ssDE](../../includes/ssde-md.md)] pour cette colonne est retournée dans la clause `OUTPUT` de la colonne `inserted.ScrapReasonID`.  
  
```sql
USE AdventureWorks2012;
GO

DECLARE @MyTableVar TABLE (
    NewScrapReasonID SMALLINT,  
    Name VARCHAR(50),  
    ModifiedDate DATETIME); 
    
INSERT Production.ScrapReason  
    OUTPUT INSERTED.ScrapReasonID, INSERTED.Name, INSERTED.ModifiedDate  
        INTO @MyTableVar  
VALUES (N'Operator error', GETDATE());  
  
--Display the result set of the table variable.  
SELECT NewScrapReasonID, Name, ModifiedDate FROM @MyTableVar;  
--Display the result set of the table.  
SELECT ScrapReasonID, Name, ModifiedDate   
FROM Production.ScrapReason;  
GO
```
  
### <a name="b-using-output-with-a-delete-statement"></a>B. Utilisation de OUTPUT avec une instruction DELETE  
 L'exemple suivant supprime toutes les lignes de la table `ShoppingCartItem`. La clause `OUTPUT deleted.*` spécifie que les résultats de l'instruction `DELETE`, à savoir toutes les colonnes dans les lignes supprimées, sont retournés vers l'application appelante. L'instruction `SELECT` suivante vérifie les résultats de l'opération de suppression dans la table `ShoppingCartItem`.  
  
```sql
USE AdventureWorks2012;
GO

DELETE Sales.ShoppingCartItem  
OUTPUT DELETED.*   
WHERE ShoppingCartID = 20621;  
  
--Verify the rows in the table matching the WHERE clause have been deleted.  
SELECT COUNT(*) AS [Rows in Table] FROM Sales.ShoppingCartItem WHERE ShoppingCartID = 20621;  
GO
```
  
### <a name="c-using-output-into-with-an-update-statement"></a>C. Utilisation de OUTPUT INTO avec une instruction UPDATE  
 L'exemple suivant met à jour la colonne `VacationHours` à hauteur de 25 % pour 10 premières lignes aléatoires dans la table `Employee`. La clause `OUTPUT` retourne la valeur `VacationHours` qui existe avant l'application de l'instruction `UPDATE` dans la colonne `deleted.VacationHours` et la valeur mise à jour dans la colonne `inserted.VacationHours` sur la variable de table `@MyTableVar`.  
  
 Deux instructions `SELECT` suivent ; elles retournent les valeurs dans `@MyTableVar`, ainsi que les résultats de la mise à jour dans la table `Employee`.  
  
```sql
USE AdventureWorks2012;
GO
  
DECLARE @MyTableVar TABLE (  
    EmpID INT NOT NULL,  
    OldVacationHours INT,  
    NewVacationHours INT,  
    ModifiedDate DATETIME);  
  
UPDATE TOP (10) HumanResources.Employee  
SET VacationHours = VacationHours * 1.25,  
    ModifiedDate = GETDATE()   
OUTPUT inserted.BusinessEntityID,  
       deleted.VacationHours,  
       inserted.VacationHours,  
       inserted.ModifiedDate  
INTO @MyTableVar;  
  
--Display the result set of the table variable.  
SELECT EmpID, OldVacationHours, NewVacationHours, ModifiedDate  
FROM @MyTableVar;  
GO  
--Display the result set of the table.  
SELECT TOP (10) BusinessEntityID, VacationHours, ModifiedDate  
FROM HumanResources.Employee;  
GO  
```

### <a name="d-using-output-into-to-return-an-expression"></a>D. Utilisation de OUTPUT INTO pour retourner une expression  
 L'exemple suivant reprend l'exemple C en définissant une expression dans la clause `OUTPUT` comme étant la différence entre la valeur mise à jour `VacationHours` et la valeur `VacationHours` avant sa mise à jour. La valeur de cette expression est retournée à la variable `@MyTableVar``table` dans la colonne `VacationHoursDifference`.  
  
```sql
USE AdventureWorks2012;  
GO

DECLARE @MyTableVar TABLE (  
    EmpID INT NOT NULL,  
    OldVacationHours INT,  
    NewVacationHours INT,  
    VacationHoursDifference INT,  
    ModifiedDate DATETIME);  
  
UPDATE TOP (10) HumanResources.Employee  
SET VacationHours = VacationHours * 1.25,  
    ModifiedDate = GETDATE()  
OUTPUT inserted.BusinessEntityID,  
       deleted.VacationHours,  
       inserted.VacationHours,  
       inserted.VacationHours - deleted.VacationHours,  
       inserted.ModifiedDate  
INTO @MyTableVar;  
  
--Display the result set of the table variable.  
SELECT EmpID, OldVacationHours, NewVacationHours,   
    VacationHoursDifference, ModifiedDate  
FROM @MyTableVar;  
GO  
SELECT TOP (10) BusinessEntityID, VacationHours, ModifiedDate  
FROM HumanResources.Employee;  
GO
```

### <a name="e-using-output-into-with-from_table_name-in-an-update-statement"></a>E. Utilisation de OUTPUT INTO avec from_table_name dans une instruction UPDATE  
 L’exemple suivant met à jour la colonne `ScrapReasonID` dans la table `WorkOrder` pour toutes les commandes avec les valeurs `ProductID` et `ScrapReasonID` spécifiées. La clause `OUTPUT INTO` retourne les valeurs depuis la table en cours de mise à jour (`WorkOrder`) ainsi que de la table `Product`. La table `Product` est utilisée dans la clause `FROM` pour spécifier les lignes à mettre à jour. Étant donné qu'un déclencheur `WorkOrder` est défini sur la table `AFTER UPDATE`, le mot clé `INTO` est obligatoire.  
  
```sql
USE AdventureWorks2012;
GO

DECLARE @MyTestVar TABLE (  
    OldScrapReasonID INT NOT NULL,   
    NewScrapReasonID INT NOT NULL,   
    WorkOrderID INT NOT NULL,  
    ProductID INT NOT NULL,  
    ProductName NVARCHAR(50)NOT NULL);  
  
UPDATE Production.WorkOrder  
SET ScrapReasonID = 4  
OUTPUT deleted.ScrapReasonID,  
       inserted.ScrapReasonID,   
       inserted.WorkOrderID,  
       inserted.ProductID,  
       p.Name  
    INTO @MyTestVar  
FROM Production.WorkOrder AS wo  
    INNER JOIN Production.Product AS p   
    ON wo.ProductID = p.ProductID   
    AND wo.ScrapReasonID= 16  
    AND p.ProductID = 733;  
  
SELECT OldScrapReasonID, NewScrapReasonID, WorkOrderID,   
    ProductID, ProductName   
FROM @MyTestVar;  
GO
```

### <a name="f-using-output-into-with-from_table_name-in-a-delete-statement"></a>F. Utilisation de OUTPUT INTO avec from_table_name dans une instruction DELETE  
 L'exemple suivant supprime les lignes dans la table `ProductProductPhoto` en fonction des critères de recherche définis dans la clause `FROM` de l'instruction `DELETE`. La clause `OUTPUT` retourne les colonnes de la table en cours de suppression (`deleted.ProductID`, `deleted.ProductPhotoID`) ainsi que les colonnes de la table `Product`. Cette table est utilisée dans la clause `FROM` pour spécifier les lignes à supprimer.  
  
```sql
USE AdventureWorks2012;
GO

DECLARE @MyTableVar TABLE (  
    ProductID INT NOT NULL,   
    ProductName NVARCHAR(50)NOT NULL,  
    ProductModelID INT NOT NULL,   
    PhotoID INT NOT NULL);  
  
DELETE Production.ProductProductPhoto  
OUTPUT DELETED.ProductID,  
       p.Name,  
       p.ProductModelID,  
       DELETED.ProductPhotoID  
    INTO @MyTableVar  
FROM Production.ProductProductPhoto AS ph  
JOIN Production.Product as p   
    ON ph.ProductID = p.ProductID   
    WHERE p.ProductModelID BETWEEN 120 and 130;  
  
--Display the results of the table variable.  
SELECT ProductID, ProductName, ProductModelID, PhotoID   
FROM @MyTableVar  
ORDER BY ProductModelID;  
GO
```
  
### <a name="g-using-output-into-with-a-large-object-data-type"></a>G. Utilisation de OUTPUT INTO avec un type de données d'objets volumineux  
 L'exemple suivant met à jour une valeur partielle dans `DocumentSummary`, une colonne `nvarchar(max)` de la table `Production.Document`, à l'aide de la clause `.WRITE`. Le terme `components` est remplacé par le terme `features`, en spécifiant le terme de remplacement, la position de départ (décalage) du terme à remplacer dans les données existantes et le nombre de caractères à remplacer (longueur). L’exemple utilise la clause `OUTPUT` pour retourner les images avant et après de la colonne `DocumentSummary` à la variable `@MyTableVar``table`. Notez que les images complètes avant et après la colonne `DocumentSummary` sont retournées.  
  
```sql
USE AdventureWorks2012;
GO

DECLARE @MyTableVar TABLE (  
    SummaryBefore NVARCHAR(max),  
    SummaryAfter NVARCHAR(max));  
  
UPDATE Production.Document  
SET DocumentSummary .WRITE (N'features',28,10)  
OUTPUT deleted.DocumentSummary,   
       inserted.DocumentSummary   
    INTO @MyTableVar  
WHERE Title = N'Front Reflector Bracket Installation';  
  
SELECT SummaryBefore, SummaryAfter   
FROM @MyTableVar;  
GO
```
  
### <a name="h-using-output-in-an-instead-of-trigger"></a>H. Utilisation de OUTPUT dans un déclencheur INSTEAD OF  
 L'exemple suivant utilise la clause `OUTPUT` dans un déclencheur pour retourner les résultats de l'opération de celui-ci. Tout d'abord, une vue est créée sur la table `ScrapReason`, puis un déclencheur `INSTEAD OF INSERT` est défini sur la vue qui ne permet qu'à la colonne `Name` de la table de base d'être modifiée par l'utilisateur. Étant donné que la colonne `ScrapReasonID` est une colonne `IDENTITY` de la table de base, le déclencheur ignore la valeur fournie par l'utilisateur. Ceci permet au [!INCLUDE[ssDE](../../includes/ssde-md.md)] de générer automatiquement la valeur correcte. De plus, la valeur fournie par l'utilisateur pour `ModifiedDate` est ignorée et définie sur la date actuelle. La clause `OUTPUT` retourne les valeurs insérées dans la table `ScrapReason`.  
  
```sql
USE AdventureWorks2012;
GO

IF OBJECT_ID('dbo.vw_ScrapReason','V') IS NOT NULL  
    DROP VIEW dbo.vw_ScrapReason;  
GO  
CREATE VIEW dbo.vw_ScrapReason  
AS (SELECT ScrapReasonID, Name, ModifiedDate  
    FROM Production.ScrapReason);  
GO  
CREATE TRIGGER dbo.io_ScrapReason   
    ON dbo.vw_ScrapReason  
INSTEAD OF INSERT  
AS  
BEGIN  
--ScrapReasonID is not specified in the list of columns to be inserted   
--because it is an IDENTITY column.  
    INSERT INTO Production.ScrapReason (Name, ModifiedDate)  
        OUTPUT INSERTED.ScrapReasonID, INSERTED.Name,   
               INSERTED.ModifiedDate  
    SELECT Name, getdate()  
    FROM inserted;  
END  
GO  
INSERT vw_ScrapReason (ScrapReasonID, Name, ModifiedDate)  
VALUES (99, N'My scrap reason','20030404');  
GO
```
  
 Voici l'ensemble de résultats généré le 12 avril 2004 ('`2004-04-12'`). Notez que les colonnes `ScrapReasonIDActual` et `ModifiedDate` illustrent les valeurs générées par l'opération du déclencheur plutôt que les valeurs fournies dans l'instruction `INSERT`.  
  
 ```
 ScrapReasonID  Name             ModifiedDate  
 -------------  ---------------- -----------------------  
 17             My scrap reason  2004-04-12 16:23:33.050
 ```  
  
### <a name="i-using-output-into-with-identity-and-computed-columns"></a>I. Utilisation de OUTPUT INTO avec des colonnes d'identité et des colonnes calculées  
 L'exemple suivant crée la table `EmployeeSales`, puis y insère plusieurs lignes à l'aide d'une instruction `INSERT`, avec une instruction `SELECT` pour récupérer les données des tables sources. La table `EmployeeSales` contient une colonne d'identité (`EmployeeID`) et une colonne calculée (`ProjectedSales`).  
  
```sql
USE AdventureWorks2012;
GO

IF OBJECT_ID ('dbo.EmployeeSales', 'U') IS NOT NULL  
    DROP TABLE dbo.EmployeeSales;  
GO  
CREATE TABLE dbo.EmployeeSales  
( EmployeeID   INT IDENTITY (1,5) NOT NULL,  
  LastName     NVARCHAR(20) NOT NULL,  
  FirstName    NVARCHAR(20) NOT NULL,  
  CurrentSales MONEY NOT NULL,  
  ProjectedSales AS CurrentSales * 1.10   
);  
GO  
DECLARE @MyTableVar TABLE (  
  EmployeeID   INT NOT NULL,  
  LastName     NVARCHAR(20) NOT NULL,  
  FirstName    NVARCHAR(20) NOT NULL,  
  CurrentSales MONEY NOT NULL,  
  ProjectedSales MONEY NOT NULL  
  );  
  
INSERT INTO dbo.EmployeeSales (LastName, FirstName, CurrentSales)  
  OUTPUT INSERTED.EmployeeID,
         INSERTED.LastName,   
         INSERTED.FirstName,   
         INSERTED.CurrentSales,
         INSERTED.ProjectedSales
  INTO @MyTableVar  
    SELECT c.LastName, c.FirstName, sp.SalesYTD  
    FROM Sales.SalesPerson AS sp  
    INNER JOIN Person.Person AS c  
        ON sp.BusinessEntityID = c.BusinessEntityID  
    WHERE sp.BusinessEntityID LIKE '2%'  
    ORDER BY c.LastName, c.FirstName;  
  
SELECT EmployeeID, LastName, FirstName, CurrentSales, ProjectedSales  
FROM @MyTableVar;  
GO  
SELECT EmployeeID, LastName, FirstName, CurrentSales, ProjectedSales  
FROM dbo.EmployeeSales;  
GO
```
  
### <a name="j-using-output-and-output-into-in-a-single-statement"></a>J. Utilisation de OUTPUT et OUTPUT INTO dans une instruction unique  
 L'exemple suivant supprime les lignes dans la table `ProductProductPhoto` en fonction des critères de recherche définis dans la clause `FROM` de l'instruction `DELETE`. La clause `OUTPUT INTO` retourne les colonnes de la table en cours de suppression (`deleted.ProductID`, `deleted.ProductPhotoID`), ainsi que les colonnes de la table `Product` à la variable `@MyTableVar``table`. La table `Product` est utilisée dans la clause `FROM` pour spécifier les lignes à supprimer. La clause `OUTPUT` retourne à l'application appelante `deleted.ProductID`, les colonnes `deleted.ProductPhotoID`, la date et l'heure de suppression de la ligne de la table `ProductProductPhoto`.  
  
```sql
USE AdventureWorks2012;
GO

DECLARE @MyTableVar TABLE (  
    ProductID INT NOT NULL,   
    ProductName NVARCHAR(50)NOT NULL,  
    ProductModelID INT NOT NULL,   
    PhotoID INT NOT NULL);  
  
DELETE Production.ProductProductPhoto  
OUTPUT DELETED.ProductID,  
       p.Name,  
       p.ProductModelID,  
       DELETED.ProductPhotoID  
    INTO @MyTableVar  
OUTPUT DELETED.ProductID, DELETED.ProductPhotoID, GETDATE() AS DeletedDate   
FROM Production.ProductProductPhoto AS ph  
JOIN Production.Product as p   
    ON ph.ProductID = p.ProductID   
WHERE p.ProductID BETWEEN 800 and 810;  
  
--Display the results of the table variable.  
SELECT ProductID, ProductName, PhotoID, ProductModelID   
FROM @MyTableVar;  
GO
```
  
### <a name="k-inserting-data-returned-from-an-output-clause"></a>K. Insertion de données retournées à partir d'une clause OUTPUT  
 L'exemple suivant capture les données retournées par la clause `OUTPUT` d'une instruction `MERGE` et insère ces données dans une autre table. L'instruction `MERGE` met quotidiennement à jour la colonne `Quantity` de la table `ProductInventory`, en fonction des commandes traitées dans la table `SalesOrderDetail`. Elle supprime également les lignes correspondant aux produits dont le stock passe à `0` ou moins. Cet exemple capture les lignes supprimées et les insère dans une autre table, `ZeroInventory`, qui effectue le suivi des produits en rupture de stock.  
  
```sql
USE AdventureWorks2012;
GO

IF OBJECT_ID(N'Production.ZeroInventory', N'U') IS NOT NULL  
    DROP TABLE Production.ZeroInventory;  
GO  
--Create ZeroInventory table.  
CREATE TABLE Production.ZeroInventory (DeletedProductID int, RemovedOnDate DateTime);  
GO  
  
INSERT INTO Production.ZeroInventory (DeletedProductID, RemovedOnDate)  
SELECT ProductID, GETDATE()  
FROM  
(   MERGE Production.ProductInventory AS pi  
    USING (SELECT ProductID, SUM(OrderQty) FROM Sales.SalesOrderDetail AS sod  
           JOIN Sales.SalesOrderHeader AS soh  
           ON sod.SalesOrderID = soh.SalesOrderID  
           AND soh.OrderDate = '20070401'  
           GROUP BY ProductID) AS src (ProductID, OrderQty)  
    ON (pi.ProductID = src.ProductID)  
    WHEN MATCHED AND pi.Quantity - src.OrderQty <= 0  
        THEN DELETE  
    WHEN MATCHED  
        THEN UPDATE SET pi.Quantity = pi.Quantity - src.OrderQty  
    OUTPUT $action, deleted.ProductID) AS Changes (Action, ProductID)  
WHERE Action = 'DELETE';  
IF @@ROWCOUNT = 0  
PRINT 'Warning: No rows were inserted';  
GO  
SELECT DeletedProductID, RemovedOnDate
FROM Production.ZeroInventory;
GO
```
  
## <a name="see-also"></a>Voir aussi  
 [DELETE &#40;Transact-SQL&#41;](../../t-sql/statements/delete-transact-sql.md)   
 [INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/insert-transact-sql.md)   
 [UPDATE &#40;Transact-SQL&#41;](../../t-sql/queries/update-transact-sql.md)   
 [table &#40;Transact-SQL&#41;](../../t-sql/data-types/table-transact-sql.md)   
 [CREATE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/create-trigger-transact-sql.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)
