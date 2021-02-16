---
title: CASE (Transact-SQL)
description: Référence Transact-SQL pour l’expression CASE. CASE évalue une liste de conditions pour retourner des résultats spécifiques.
ms.date: 01/26/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CASE_TSQL
- CASE
dev_langs:
- TSQL
helpviewer_keywords:
- CASE expression [Transact-SQL]
- simple CASE expression
- comparing expressions
- searched CASE expression
ms.assetid: 658039ec-8dc2-4251-bc82-30ea23708cee
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 5647234be5f758f1b4ff27f2f11e5338d67635da
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352386"
---
# <a name="case-transact-sql"></a>CASE (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Évalue une liste de conditions et retourne une expression de résultat parmi plusieurs possibilités.  
  
 L'expression CASE a deux formats :  
  
-   l'expression CASE simple détermine le résultat en comparant une expression à un jeu d'expressions simples ;  
  
-   l'expression CASE élaborée évalue un ensemble d'expressions booléennes pour déterminer le résultat.  
  
 Les deux formats prennent en charge un argument ELSE facultatif.  
  
 CASE peut être utilisé dans n'importe quelle instruction ou clause qui autorise une expression valide. Par exemple, vous pouvez utiliser CASE dans les instructions telles que SELECT, UPDATE, DELETE et SET, ainsi que dans les clauses telles que select_list, IN, WHERE, ORDER BY et HAVING.  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
-- Syntax for SQL Server, Azure SQL Database and Azure Synapse Analytics
  
--Simple CASE expression:   
CASE input_expression   
     WHEN when_expression THEN result_expression [ ...n ]   
     [ ELSE else_result_expression ]   
END   

--Searched CASE expression:  
CASE  
     WHEN Boolean_expression THEN result_expression [ ...n ]   
     [ ELSE else_result_expression ]   
END  
```  
  
```syntaxsql
-- Syntax for Parallel Data Warehouse  
  
CASE  
     WHEN when_expression THEN result_expression [ ...n ]   
     [ ELSE else_result_expression ]   
END
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments  
 *input_expression*  
 Expression évaluée à l'aide du format CASE simple. *input_expression* correspond à toute [expression](../../t-sql/language-elements/expressions-transact-sql.md) valide.  
  
 WHEN *when_expression*  
 Expression simple à laquelle *input_expression* est comparée quand le format CASE simple est utilisé. *when_expression* correspond à toute expression valide. Les types de données de *input_expression* et de chaque *when_expression* doivent être identiques ou correspondre à une conversion implicite.  
  
 THEN *result_expression*  
 Expression retournée quand *input_expression* égale à *when_expression* a la valeur TRUE, ou quand *Boolean_expression* a la valeur TRUE. *result_expression* correspond à toute [expression](../../t-sql/language-elements/expressions-transact-sql.md) valide.  
  
 ELSE *else_result_expression*  
 Expression retournée si aucune opération de comparaison n'a la valeur TRUE. Si cet argument est omis et si aucune opération de comparaison n'a la valeur TRUE, CASE retourne la valeur NULL. *else_result_expression* correspond à toute expression valide. Les types de données de *else_result_expression* et de toute *result_expression* doivent être identiques ou correspondre à une conversion implicite.  
  
 WHEN *Boolean_expression*  
 Expression booléenne évaluée lorsque la fonction CASE élaborée est utilisée. *Boolean_expression* correspond à toute expression booléenne valide.  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Types de retour
 Retourne le type de priorité la plus élevé de l’ensemble des types dans *result_expressions* et le paramètre facultatif *else_result_expression*. Pour plus d’informations, consultez [Priorités des types de données &#40;Transact-SQL&#41;](../../t-sql/data-types/data-type-precedence-transact-sql.md).  
  
### <a name="return-values"></a>Valeurs de retour  
 **Expression CASE simple :**  
  
 L'expression CASE simple fonctionne en comparant la première expression à l'expression contenue dans chaque clause WHEN pour déterminer son équivalence. Si ces expressions sont équivalentes, l'expression contenue dans la clause THEN est retournée.  
  
-   Autorise uniquement un contrôle d'égalité.  
  
-   Dans l’ordre spécifié, évalue input_expression = when_expression pour chaque clause WHEN.  
  
-   Retourne la valeur *result_expression* de la première *input_expression* = *when_expression* qui a la valeur TRUE.  
  
-   Si aucune *input_expression* = *when_expression* a la valeur TRUE, [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] retourne la valeur *else_result_expression* si une clause ELSE est spécifiée ou une valeur NULL si aucune clause ELSE n’est spécifiée.  
  
 **Expression CASE élaborée :**  
  
-   Évalue, dans l’ordre spécifié, *Boolean_expression* pour chaque clause WHEN.  
  
-   Retourne la valeur *result_expression* de la première *Boolean_expression* qui a la valeur TRUE.  
  
-   Si aucune *Boolean_expression* n’a la valeur TRUE, [!INCLUDE[ssDE](../../includes/ssde-md.md)] retourne la valeur *else_result_expression* si une clause ELSE est spécifiée ou une valeur NULL si aucune clause ELSE n’est spécifiée.  
  
## <a name="remarks"></a>Notes  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] autorise uniquement 10 niveaux d'imbrication dans les expressions CASE.  
  
 L'expression CASE ne peut pas être utilisée pour contrôler le flux d'exécution d'instructions, de blocs d'instructions, de fonctions définies par l'utilisateur et de procédures stockées Transact-SQL. Pour obtenir la liste des méthodes de contrôle de flux, consultez [Langage de contrôle de flux &#40;Transact-SQL&#41;](~/t-sql/language-elements/control-of-flow.md).  
  
 L’expression CASE évalue les conditions de manière séquentielle et s’arrête à la première condition remplie. Dans certains cas, une expression est évaluée avant qu’une expression CASE ne reçoive les résultats de l'expression en entrée. Des erreurs sont possibles lors de l'évaluation de ces expressions. Les expressions d’agrégation qui apparaissent dans les arguments WHEN d’une expression CASE sont évaluées en premier, puis fournies à l’expression CASE. Par exemple, la requête suivante génère une erreur de division par zéro lors de la production de la valeur de l'agrégat MAX. Cela se produit avant l'évaluation de l'expression CASE.  
  
```sql  
WITH Data (value) AS   
(   
SELECT 0   
UNION ALL   
SELECT 1   
)   
SELECT   
   CASE   
      WHEN MIN(value) <= 0 THEN 0   
      WHEN MAX(1/value) >= 100 THEN 1   
   END   
FROM Data ;  
```  
  
 Vous devez uniquement dépendre de l'ordre d'évaluation des conditions WHEN pour les expressions scalaires (notamment les sous-requêtes non corrélées qui retournent des expressions scalaires), et non pas pour les expressions d'agrégation.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-using-a-select-statement-with-a-simple-case-expression"></a>R. Utilisation d'une instruction SELECT avec une expression CASE simple  
 Dans une instruction `SELECT`, une expression `CASE` simple permet seulement de vérifier s'il y a égalité ; aucune autre comparaison n'est effectuée. L'exemple suivant utilise l'expression `CASE` pour modifier la présentation des catégories de gammes de produits pour en faciliter la lecture.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT   ProductNumber, Category =  
      CASE ProductLine  
         WHEN 'R' THEN 'Road'  
         WHEN 'M' THEN 'Mountain'  
         WHEN 'T' THEN 'Touring'  
         WHEN 'S' THEN 'Other sale items'  
         ELSE 'Not for sale'  
      END,  
   Name  
FROM Production.Product  
ORDER BY ProductNumber;  
GO  
```  
  
### <a name="b-using-a-select-statement-with-a-searched-case-expression"></a>B. Utilisation d'une instruction SELECT avec une expression CASE élaborée  
 Dans une instruction `SELECT`, l'expression `CASE` élaborée permet de remplacer des valeurs dans le jeu de résultats, en fonction des valeurs de comparaison. L'exemple suivant donne le prix sous la forme d'un texte de commentaire basé sur la fourchette de prix d'un produit.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT   ProductNumber, Name, "Price Range" =   
      CASE   
         WHEN ListPrice =  0 THEN 'Mfg item - not for resale'  
         WHEN ListPrice < 50 THEN 'Under $50'  
         WHEN ListPrice >= 50 and ListPrice < 250 THEN 'Under $250'  
         WHEN ListPrice >= 250 and ListPrice < 1000 THEN 'Under $1000'  
         ELSE 'Over $1000'  
      END  
FROM Production.Product  
ORDER BY ProductNumber ;  
GO  
```  
  
### <a name="c-using-case-in-an-order-by-clause"></a>C. Utilisation de CASE dans une clause ORDER BY  
 L'exemple suivant utilise l'expression CASE dans une clause ORDER BY pour déterminer l'ordre de tri des lignes d'après la valeur d'une colonne donnée. Dans le premier exemple, la valeur de la colonne `SalariedFlag` de la table `HumanResources.Employee` est évaluée. Les employés pour lesquels `SalariedFlag` a la valeur 1 sont retournés par ordre décroissant d'`BusinessEntityID`. Les employés pour lesquels `SalariedFlag` a la valeur 0 sont retournés par ordre croissant d'`BusinessEntityID`. Dans le deuxième exemple, le jeu de résultats est classé par la colonne `TerritoryName` lorsque la colonne `CountryRegionName` est égale à « United States » et par `CountryRegionName` pour toutes les autres lignes.  
  
```sql  
SELECT BusinessEntityID, SalariedFlag  
FROM HumanResources.Employee  
ORDER BY CASE SalariedFlag WHEN 1 THEN BusinessEntityID END DESC  
        ,CASE WHEN SalariedFlag = 0 THEN BusinessEntityID END;  
GO    
```  
  
```sql  
SELECT BusinessEntityID, LastName, TerritoryName, CountryRegionName  
FROM Sales.vSalesPerson  
WHERE TerritoryName IS NOT NULL  
ORDER BY CASE CountryRegionName WHEN 'United States' THEN TerritoryName  
         ELSE CountryRegionName END;  
```  
  
### <a name="d-using-case-in-an-update-statement"></a>D. Utilisation de CASE dans une instruction UPDATE  
 L'exemple suivant utilise l'expression CASE dans une instruction UPDATE afin de déterminer la valeur définie pour la colonne `VacationHours` des employés pour lesquels `SalariedFlag` a la valeur 0. En soustrayant 10 heures des résultats de `VacationHours` dans une valeur négative, `VacationHours` augmente de 40 heures ; sinon, `VacationHours` augmente de 20 heures. La clause OUTPUT est utilisée pour afficher les valeurs antérieures et postérieures aux congés.  
  
```sql  
USE AdventureWorks2012;  
GO  
UPDATE HumanResources.Employee  
SET VacationHours =   
    ( CASE  
         WHEN ((VacationHours - 10.00) < 0) THEN VacationHours + 40  
         ELSE (VacationHours + 20.00)  
       END  
    )  
OUTPUT Deleted.BusinessEntityID, Deleted.VacationHours AS BeforeValue,   
       Inserted.VacationHours AS AfterValue  
WHERE SalariedFlag = 0;   
```  
  
### <a name="e-using-case-in-a-set-statement"></a>E. Utilisation de CASE dans une instruction SET  
 L'exemple suivant utilise l'expression CASE dans une instruction SET au sein de la fonction table `dbo.GetContactInfo`. Dans la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)], toutes les données relatives aux personnes sont stockées dans la table `Person.Person`. Par exemple, la personne peut être un employé, un représentant du fournisseur ou un client. La fonction retourne le prénom et le nom d’un `BusinessEntityID` donné, ainsi que le type de contact correspondant à cette personne. L’expression CASE dans l’instruction SET détermine la valeur à afficher pour la colonne `ContactType` en fonction de l’existence de la colonne `BusinessEntityID` dans les tables `Employee`, `Vendor` ou `Customer`.  
  
```sql  
USE AdventureWorks2012;  
GO  
CREATE FUNCTION dbo.GetContactInformation(@BusinessEntityID INT)  
    RETURNS @retContactInformation TABLE   
(  
BusinessEntityID INT NOT NULL,  
FirstName NVARCHAR(50) NULL,  
LastName NVARCHAR(50) NULL,  
ContactType NVARCHAR(50) NULL,  
    PRIMARY KEY CLUSTERED (BusinessEntityID ASC)  
)   
AS   
-- Returns the first name, last name and contact type for the specified contact.  
BEGIN  
    DECLARE   
        @FirstName NVARCHAR(50),   
        @LastName NVARCHAR(50),   
        @ContactType NVARCHAR(50);  
  
    -- Get common contact information  
    SELECT   
        @BusinessEntityID = BusinessEntityID,   
@FirstName = FirstName,   
        @LastName = LastName  
    FROM Person.Person   
    WHERE BusinessEntityID = @BusinessEntityID;  
  
    SET @ContactType =   
        CASE   
            -- Check for employee  
            WHEN EXISTS(SELECT * FROM HumanResources.Employee AS e   
                WHERE e.BusinessEntityID = @BusinessEntityID)   
                THEN 'Employee'  
  
            -- Check for vendor  
            WHEN EXISTS(SELECT * FROM Person.BusinessEntityContact AS bec  
                WHERE bec.BusinessEntityID = @BusinessEntityID)   
                THEN 'Vendor'  
  
            -- Check for store  
            WHEN EXISTS(SELECT * FROM Purchasing.Vendor AS v            
                WHERE v.BusinessEntityID = @BusinessEntityID)   
                THEN 'Store Contact'  
  
            -- Check for individual consumer  
            WHEN EXISTS(SELECT * FROM Sales.Customer AS c   
                WHERE c.PersonID = @BusinessEntityID)   
                THEN 'Consumer'  
        END;  
  
    -- Return the information to the caller  
    IF @BusinessEntityID IS NOT NULL   
    BEGIN  
        INSERT @retContactInformation  
        SELECT @BusinessEntityID, @FirstName, @LastName, @ContactType;  
    END;  
  
    RETURN;  
END;  
GO  
  
SELECT BusinessEntityID, FirstName, LastName, ContactType  
FROM dbo.GetContactInformation(2200);  
GO  
SELECT BusinessEntityID, FirstName, LastName, ContactType  
FROM dbo.GetContactInformation(5);  
```  
  
### <a name="f-using-case-in-a-having-clause"></a>F. Utilisation de CASE dans une clause HAVING  
 L'exemple suivant utilise l'expression CASE dans une clause HAVING pour restreindre les lignes retournées par l'instruction SELECT. L’instruction retourne le taux horaire maximal de chaque poste indiqué dans la table `HumanResources.Employee`. La clause HAVING restreint les fonctions aux hommes ayant un taux de salaire maximal supérieur à 40 dollars ou aux femmes ayant un taux de salaire maximal supérieur à 42 dollars.  
  
```sql 
USE AdventureWorks2012;  
GO  
SELECT JobTitle, MAX(ph1.Rate)AS MaximumRate  
FROM HumanResources.Employee AS e  
JOIN HumanResources.EmployeePayHistory AS ph1 ON e.BusinessEntityID = ph1.BusinessEntityID  
GROUP BY JobTitle  
HAVING (MAX(CASE WHEN Gender = 'M'   
        THEN ph1.Rate   
        ELSE NULL END) > 40.00  
     OR MAX(CASE WHEN Gender  = 'F'   
        THEN ph1.Rate    
        ELSE NULL END) > 42.00)  
ORDER BY MaximumRate DESC;  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Exemples : [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="g-using-a-select-statement-with-a-case-expression"></a>G. Utilisation d’une instruction SELECT avec une expression CASE  
 Dans une instruction SELECT, l’expression CASE permet de remplacer des valeurs dans le jeu de résultats, en fonction de valeurs de comparaison. L’exemple suivant utilise l’expression CASE pour modifier la présentation des catégories de gammes de produits pour en faciliter la lecture. Quand une valeur n’existe pas, le texte « Not for sale » s’affiche.  
  
```sql 
-- Uses AdventureWorks  
  
SELECT   ProductAlternateKey, Category =  
      CASE ProductLine  
         WHEN 'R' THEN 'Road'  
         WHEN 'M' THEN 'Mountain'  
         WHEN 'T' THEN 'Touring'  
         WHEN 'S' THEN 'Other sale items'  
         ELSE 'Not for sale'  
      END,  
   EnglishProductName  
FROM dbo.DimProduct  
ORDER BY ProductKey;  
```  
  
### <a name="h-using-case-in-an-update-statement"></a>H. Utilisation de CASE dans une instruction UPDATE  
 L'exemple suivant utilise l'expression CASE dans une instruction UPDATE afin de déterminer la valeur définie pour la colonne `VacationHours` des employés pour lesquels `SalariedFlag` a la valeur 0. En soustrayant 10 heures des résultats de `VacationHours` dans une valeur négative, `VacationHours` augmente de 40 heures ; sinon, `VacationHours` augmente de 20 heures.  
  
```sql 
-- Uses AdventureWorks   
  
UPDATE dbo.DimEmployee  
SET VacationHours =   
    ( CASE  
         WHEN ((VacationHours - 10.00) < 0) THEN VacationHours + 40  
         ELSE (VacationHours + 20.00)   
       END  
    )   
WHERE SalariedFlag = 0;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Expressions &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md)   
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
 [COALESCE &#40;Transact-SQL&#41;](../../t-sql/language-elements/coalesce-transact-sql.md)   
 [IIF &#40;Transact-SQL&#41;](../../t-sql/functions/logical-functions-iif-transact-sql.md)   
 [CHOOSE &#40;Transact-SQL&#41;](../../t-sql/functions/logical-functions-choose-transact-sql.md)  
  
  



