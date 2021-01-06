---
description: Créer des fonctions définies par l'utilisateur (moteur de base de données)
title: Créer des fonctions définies par l’utilisateur (moteur de base de données) | Microsoft Docs
ms.custom: ''
ms.date: 11/09/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- SCHEMABINDING clause
- schema-bound functions [SQL Server]
- user-defined functions [SQL Server], creating
- CREATE FUNCTION statement
- valid statements [SQL Server]
- UDF
- TVF
ms.assetid: f0d5dd10-73fd-4e05-9177-07f56552bdf7
author: rothja
ms.author: jroth
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 3303df6ac71b7a6c92d4a1c65a8b7b4b3229cd6e
ms.sourcegitcommit: 4419e99d77ee2c73f9da1559c7944f7702f2de30
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/23/2020
ms.locfileid: "97743676"
---
# <a name="create-user-defined-functions-database-engine"></a>Créer des fonctions définies par l'utilisateur (moteur de base de données)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  Cette rubrique décrit comment créer une fonction définie par l’utilisateur (UDF) dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à l’aide de [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitations et restrictions  
  
-   Les fonctions définies par l'utilisateur ne permettent pas d'exécuter des actions qui modifient l'état des bases de données.  
  
-   Les fonctions définies par l’utilisateur ne peuvent pas contenir de clause `OUTPUT INTO` avec une table comme cible.  
  
-   Les fonctions définies par l'utilisateur ne peuvent pas renvoyer plusieurs jeux de résultats. Utilisez une procédure stockée si vous devez renvoyer plusieurs jeux de résultats.  
  
-   La gestion des erreurs est restreinte dans une fonction définie par l'utilisateur. Une fonction UDF ne prend pas en charge `TRY...CATCH`, `@ERROR` ou `RAISERROR`.  
  
-   Les fonctions définies par l'utilisateur ne peuvent pas appeler une procédure stockée, mais elles peuvent appeler une procédure stockée étendue.  
  
-   Les fonctions définies par l'utilisateur ne peuvent pas utiliser des tables SQL dynamiques ou temporaires. Les variables de table sont autorisées.  
  
-   Les instructions `SET` ne sont pas autorisées dans une fonction définie par l’utilisateur.  
  
-   La clause `FOR XML` n’est pas autorisée.  
  
-   Les fonctions définies par l'utilisateur peuvent être imbriquées ; en d'autres termes, une fonction définie par l'utilisateur peut en appeler une autre. Le niveau d'imbrication est incrémenté lorsque la fonction appelée commence à s'exécuter, et décrémenté lorsque l'exécution est terminée. Les fonctions définies par l'utilisateur peuvent être imbriquées jusqu'à 32 niveaux. Le dépassement des niveaux d'imbrication maximum autorisés, provoque l'échec de la totalité de la chaîne de fonctions appelantes. Toute référence à du code managé depuis une fonction Transact-SQL définie par l'utilisateur compte pour un niveau parmi les 32 niveaux d'imbrication possibles. Les méthodes appelées à partir du code managé ne comptent pas par rapport à cette limite.  
  
-   Les instructions Service Broker suivantes **ne peuvent pas être incluses** dans la définition d’une fonction [!INCLUDE[tsql](../../includes/tsql-md.md)] définie par l’utilisateur :  
  
    -   `BEGIN DIALOG CONVERSATION`  
  
    -   `END CONVERSATION`  
  
    -   `GET CONVERSATION GROUP`  
  
    -   `MOVE CONVERSATION`  
  
    -   `RECEIVE`  
  
    -   `SEND`  
  
###  <a name="permissions"></a><a name="Security"></a> Autorisations 

Nécessite l’autorisation `CREATE FUNCTION` dans la base de données et l’autorisation `ALTER` sur le schéma dans lequel la fonction est créée. Si la fonction spécifie un type défini par l’utilisateur, elle nécessite l’autorisation `EXECUTE` sur le type.  
  
##  <a name="scalar-functions"></a><a name="Scalar"></a> Fonctions scalaires  
 L’exemple suivant illustre la création d’une **fonction scalaire (fonction UDF scalaire)** à instructions multiples dans la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. À partir d'une valeur d'entrée unique ( `ProductID`), la fonction retourne une valeur de donnée unique, en l'occurrence, la quantité totale du produit spécifié en stock.  
  
```sql  
IF OBJECT_ID (N'dbo.ufnGetInventoryStock', N'FN') IS NOT NULL  
    DROP FUNCTION ufnGetInventoryStock;  
GO  
CREATE FUNCTION dbo.ufnGetInventoryStock(@ProductID int)  
RETURNS int   
AS   
-- Returns the stock level for the product.  
BEGIN  
    DECLARE @ret int;  
    SELECT @ret = SUM(p.Quantity)   
    FROM Production.ProductInventory p   
    WHERE p.ProductID = @ProductID   
        AND p.LocationID = '6';  
     IF (@ret IS NULL)   
        SET @ret = 0;  
    RETURN @ret;  
END; 
```  
  
 Dans l'exemple suivant, la fonction `ufnGetInventoryStock` est utilisée pour déterminer la quantité en stock des produits dont la valeur `ProductModelID` est comprise entre 75 et 80.  
  
```sql  
SELECT ProductModelID, Name, dbo.ufnGetInventoryStock(ProductID)AS CurrentSupply  
FROM Production.Product  
WHERE ProductModelID BETWEEN 75 and 80;  
```  

> [!NOTE]  
> Pour obtenir des informations complémentaires et des exemples de fonctions scalaires, consultez [CREATE FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/create-function-transact-sql.md). 

##  <a name="table-valued-functions"></a><a name="TVF"></a> Fonctions table  
L’exemple suivant crée une **fonction table incluse** dans la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. À partir d'un paramètre d'entrée unique (storeID), la fonction retourne les colonnes `ProductID`, `Name`, ainsi que le total cumulé des ventes ( `YTD Total` ) pour chaque produit vendu au magasin du client.  
  
```sql  
IF OBJECT_ID (N'Sales.ufn_SalesByStore', N'IF') IS NOT NULL  
    DROP FUNCTION Sales.ufn_SalesByStore;  
GO  
CREATE FUNCTION Sales.ufn_SalesByStore (@storeid int)  
RETURNS TABLE  
AS  
RETURN   
(  
    SELECT P.ProductID, P.Name, SUM(SD.LineTotal) AS 'Total'  
    FROM Production.Product AS P   
    JOIN Sales.SalesOrderDetail AS SD ON SD.ProductID = P.ProductID  
    JOIN Sales.SalesOrderHeader AS SH ON SH.SalesOrderID = SD.SalesOrderID  
    JOIN Sales.Customer AS C ON SH.CustomerID = C.CustomerID  
    WHERE C.StoreID = @storeid  
    GROUP BY P.ProductID, P.Name  
);  
```  
  
L'exemple suivant appelle la fonction et spécifie l'ID client 602.  
  
```sql  
SELECT * FROM Sales.ufn_SalesByStore (602);  
```  
  
L’exemple suivant crée une **fonction table à instructions multiples** dans la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. À partir d'un paramètre d'entrée unique, `EmployeeID` , la fonction retourne la liste de tous les employés qui sont sous la responsabilité directe ou indirecte de l'employé spécifié. La fonction est ensuite appelée en spécifiant l'ID d'employé 19.  
  
```sql  
IF OBJECT_ID (N'dbo.ufn_FindReports', N'TF') IS NOT NULL  
    DROP FUNCTION dbo.ufn_FindReports;  
GO  
CREATE FUNCTION dbo.ufn_FindReports (@InEmpID INTEGER)  
RETURNS @retFindReports TABLE   
(  
    EmployeeID int primary key NOT NULL,  
    FirstName nvarchar(255) NOT NULL,  
    LastName nvarchar(255) NOT NULL,  
    JobTitle nvarchar(50) NOT NULL,  
    RecursionLevel int NOT NULL  
)  
--Returns a result set that lists all the employees who report to the   
--specific employee directly or indirectly.*/  
AS  
BEGIN  
WITH EMP_cte(EmployeeID, OrganizationNode, FirstName, LastName, JobTitle, RecursionLevel) -- CTE name and columns  
    AS (  
        SELECT e.BusinessEntityID, e.OrganizationNode, p.FirstName, p.LastName, e.JobTitle, 0 -- Get the initial list of Employees for Manager n  
        FROM HumanResources.Employee e   
INNER JOIN Person.Person p   
ON p.BusinessEntityID = e.BusinessEntityID  
        WHERE e.BusinessEntityID = @InEmpID  
        UNION ALL  
        SELECT e.BusinessEntityID, e.OrganizationNode, p.FirstName, p.LastName, e.JobTitle, RecursionLevel + 1 -- Join recursive member to anchor  
        FROM HumanResources.Employee e   
            INNER JOIN EMP_cte  
            ON e.OrganizationNode.GetAncestor(1) = EMP_cte.OrganizationNode  
INNER JOIN Person.Person p   
ON p.BusinessEntityID = e.BusinessEntityID  
        )  
-- copy the required columns to the result of the function   
   INSERT @retFindReports  
   SELECT EmployeeID, FirstName, LastName, JobTitle, RecursionLevel  
   FROM EMP_cte   
   RETURN  
END;  
GO  
```  
  
L’exemple suivant appelle la fonction et spécifie l’ID d’employé 1.  
  
```sql  
SELECT EmployeeID, FirstName, LastName, JobTitle, RecursionLevel  
FROM dbo.ufn_FindReports(1);  
```  

> [!NOTE]  
> Pour obtenir des informations complémentaires et des exemples de fonctions table incluses et de fonctions table à instructions multiples, consultez [CREATE FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/create-function-transact-sql.md). 

## <a name="best-practices"></a>Bonnes pratiques  
Si une fonction définie par l’utilisateur (UDF) n’est pas créée avec la clause `SCHEMABINDING`, les modifications apportées aux objets sous-jacents peuvent affecter la définition de la fonction et produire des résultats inattendus quand elle est appelée. Nous vous recommandons d'implémenter l'une des méthodes suivantes pour vous assurer que la fonction ne devient pas obsolète en raison des modifications apportées à ses objets sous-jacents :  
  
-   Spécifiez la clause `WITH SCHEMABINDING` au moment de créer la fonction UDF. Vous vous assurez ainsi que les objets référencés dans la définition de la fonction ne peuvent pas être modifiés sauf si la fonction est également modifiée.  
  
-   Exécutez la procédure stockée [sp_refreshsqlmodule](../../relational-databases/system-stored-procedures/sp-refreshsqlmodule-transact-sql.md) après avoir modifié tout objet spécifié dans la définition de la fonction UDF.  

Si la fonction UDF que vous créez n’accède pas à des données, spécifiez l’option `SCHEMABINDING`. Elle empêche l’optimiseur de requête de générer des opérateurs de spool inutiles pour les plans de requête impliquant ces fonctions UDF. Pour plus d’informations sur les spools, consultez le [Guide de référence des opérateurs Showplan logiques et physiques](../../relational-databases/showplan-logical-and-physical-operators-reference.md). Pour plus d’informations sur la création d’une fonction liée à un schéma, consultez [Fonctions liées à un schéma](../../relational-databases/user-defined-functions/user-defined-functions.md#SchemaBound).

Il est possible de se joindre à une fonction table à instructions multiples dans une clause `FROM`, mais cela peut nuire aux performances. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne peut pas employer toutes les techniques optimisées sur certaines instructions pouvant être incluses dans une fonction table à instructions multiples. De ce fait, le plan de requête obtenu n’est pas optimal. Pour obtenir de meilleures performances, quand cela est possible, utilisez des jointures entre les tables de base au lieu des fonctions.  

> [!IMPORTANT]
> Les fonctions table à instructions multiples ont une estimation de cardinalité fixe égale à 100 à compter de [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)], et égale à 1 pour les versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].    
> Depuis [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)], l’optimisation d’un plan d’exécution qui utilise des fonctions table à instructions multiples peut tirer parti de l’exécution entrelacée. Résultat : la cardinalité réelle est utilisée à la place de l’approche heuristique ci-dessus.     
> Pour plus d’informations, consultez [Exécution entrelacée pour les fonctions table à instructions multiples](../../relational-databases/performance/intelligent-query-processing.md#interleaved-execution-for-mstvfs).

> [!NOTE]  
> ANSI_WARNINGS n'est pas honoré lorsque vous transmettez des paramètres dans une procédure stockée, dans une fonction définie par l'utilisateur ou lorsque vous déclarez et définissez des variables dans une instruction par lot. Par exemple, si une variable est définie comme **char(3)** et qu’une valeur de plus de trois caractères lui est attribuée, les données sont tronquées en fonction de la taille définie, et l’instruction `INSERT` ou `UPDATE` réussit.

## <a name="see-also"></a>Voir aussi  
 [Fonctions définies par l’utilisateur](../../relational-databases/user-defined-functions/user-defined-functions.md)     
 [CREATE FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/create-function-transact-sql.md)    
 [ALTER FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-function-transact-sql.md)    
 [DROP FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/drop-function-transact-sql.md)     
 [DROP PARTITION FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/drop-partition-function-transact-sql.md)    
 Plus d’exemples dans la [communauté](https://www.bing.com/search?q=user%20defined%20function%20%22sql%20server%202016%22%20examples&qs=n&form=QBRE&pq=user%20defined%20function%20%22sql%20server%202016%22%20examples&sc=0-48&sp=-1&sk=&cvid=C3AD337125A840AD9EEFA3AAC36A3712)   
  
