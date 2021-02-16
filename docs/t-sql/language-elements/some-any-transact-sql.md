---
description: SOME | ANY (Transact-SQL)
title: SOME | ANY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SOME
- SOME_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- scalar values
- comparing scalar with single-column set
- ANY operator
- SOME | ANY keyword
- single-column set of values [SQL Server]
ms.assetid: 1f717ad6-f67b-4980-9397-577ecb0e5789
author: cawrites
ms.author: chadam
ms.openlocfilehash: 5f25709500f370b6083584b1321937b0484310bf
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/13/2021
ms.locfileid: "100342579"
---
# <a name="some--any-transact-sql"></a>SOME | ANY (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Compare une valeur scalaire avec un ensemble de valeurs appartenant à une seule colonne. SOME et ANY sont équivalents.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
scalar_expression { = | < > | ! = | > | > = | ! > | < | < = | ! < }   
     { SOME | ANY } ( subquery )   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *scalar_expression*  
 Toute [expression](../../t-sql/language-elements/expressions-transact-sql.md) valide.  
  
 { = \| <> \| != \| > \| >= \| !> \| < \| <= \| !< }  
 Tout opérateur de comparaison valide.  
  
 SOME | ANY  
 Spécifie qu'il convient d'effectuer une comparaison.  
  
 *subquery*  
 Sous-requête avec un jeu de résultats d'une colonne. Le type de données de la colonne renvoyée doit correspondre à celui de *scalar_expression*.  
  
## <a name="result-types"></a>Types des résultats  
 **Booléen**  
  
## <a name="result-value"></a>Valeur des résultats  
 SOME ou ANY retourne la valeur **TRUE** quand la comparaison spécifiée a la valeur TRUE pour une paire (_scalar_expression_ **,** _x_) où *x* est une valeur du jeu de valeurs sur une seule colonne ; dans le cas contraire, la valeur **FALSE** est retournée.  
  
## <a name="remarks"></a>Notes  
 SOME nécessite que *scalar_expression* corresponde à au moins une valeur retournée par la sous-requête. Pour les instructions qui nécessitent que l’argument *scalar_expression* corresponde à toutes les valeurs retournées par la sous-requête, consultez [ALL &#40;Transact-SQL&#41;](../../t-sql/language-elements/all-transact-sql.md). Par exemple, si la sous-requête retourne les valeurs 2 et 3, *scalar_expression* = SOME (sous-requête) prend la valeur TRUE pour une *scalar_expression* égale à 2. Si la sous-requête retourne les valeurs 2 et 3, l’instruction *scalar_expression* = ALL (sous-requête) donne FALSE, étant donné que certaines des valeurs de la sous-requête (à savoir 3) ne répondent pas aux critères de l’expression.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-running-a-simple-example"></a>R. Exécution d'un exemple simple  
 Les instructions suivantes créent une table simple et ajoutent les valeurs `1`, `2`, `3` et `4` à la colonne `ID`.  
  
```sql  
CREATE TABLE T1  
(ID INT) ;  
GO  
INSERT T1 VALUES (1) ;  
INSERT T1 VALUES (2) ;  
INSERT T1 VALUES (3) ;  
INSERT T1 VALUES (4) ;  
```  
  
 La requête suivante renvoie `TRUE` car `3` est inférieur à certaines des valeurs de la table.  
  
```sql  
IF 3 < SOME (SELECT ID FROM T1)  
PRINT 'TRUE'   
ELSE  
PRINT 'FALSE' ;  
```  
  
 La requête suivante retourne `FALSE`, car `3` n'est pas inférieur à toutes les valeurs de la table.  
  
```sql  
IF 3 < ALL (SELECT ID FROM T1)  
PRINT 'TRUE'   
ELSE  
PRINT 'FALSE' ;  
```  
  
### <a name="b-running-a-practical-example"></a>B. Exécution d'un exemple pratique  
 L’exemple suivant crée une procédure stockée qui détermine si tous les composants d’un `SalesOrderID` spécifié dans la base de données `AdventureWorks2012` peuvent être fabriqués dans le délai du nombre de jours spécifié. L'exemple utilise une sous-requête pour créer une liste du nombre de `DaysToManufacture` pour tous les composants du `SalesOrderID` spécifié, puis vérifie si parmi les valeurs retournées par la sous-requête certaines sont supérieures au nombre de jours spécifié. Si chaque valeur retournée pour `DaysToManufacture` est inférieure au nombre fourni, la condition a la valeur TRUE et le premier message est imprimé.  
  
```sql  
-- Uses AdventureWorks  
  
CREATE PROCEDURE ManyDaysToComplete @OrderID INT, @NumberOfDays INT  
AS  
IF   
@NumberOfDays < SOME  
   (  
    SELECT DaysToManufacture  
    FROM Sales.SalesOrderDetail  
    JOIN Production.Product   
    ON Sales.SalesOrderDetail.ProductID = Production.Product.ProductID   
    WHERE SalesOrderID = @OrderID  
   )  
PRINT 'At least one item for this order can''t be manufactured in specified number of days.'
ELSE   
PRINT 'All items for this order can be manufactured in the specified number of days or less.' ;  
  
```  
  
 Pour tester la procédure, exécutez-la en utilisant le `SalesOrderID``49080` dont l’un des composants demande `2` jours de fabrication, tandis que les 2 autres en demandent 0. La première instruction remplit les critères, Ce n’est pas le cas de la deuxième requête.  
  
```sql  
EXECUTE ManyDaysToComplete 49080, 2 ;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 `All items for this order can be manufactured in the specified number of days or less.`  
  
```  
EXECUTE ManyDaysToComplete 49080, 1 ;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 `At least one item for this order can't be manufactured in specified number of days.`  
  
## <a name="see-also"></a>Voir aussi  
 [ALL &#40;Transact-SQL&#41;](../../t-sql/language-elements/all-transact-sql.md)   
 [CASE &#40;Transact-SQL&#41;](../../t-sql/language-elements/case-transact-sql.md)   
 [Fonctions intégrées &#40;Transact-SQL&#41;](~/t-sql/functions/functions.md)   
 [Opérateurs &#40;Transact-SQL&#41;](../../t-sql/language-elements/operators-transact-sql.md)   
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
 [WHERE &#40;Transact-SQL&#41;](../../t-sql/queries/where-transact-sql.md)   
 [IN &#40;Transact-SQL&#41;](../../t-sql/language-elements/in-transact-sql.md)  
  
