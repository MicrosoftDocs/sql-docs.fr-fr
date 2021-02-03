---
description: ALL (Transact-SQL)
title: ALL (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALL_TSQL
- ALL
dev_langs:
- TSQL
helpviewer_keywords:
- single-column set of values [SQL Server]
- ALL (Transact-SQL)
ms.assetid: 4b0c002e-1ffd-4425-a980-11fdc1f24af7
author: cawrites
ms.author: chadam
ms.openlocfilehash: f315735e5ad8b1c8fd9d5a54d581d66fe7e820b7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99176367"
---
# <a name="all-transact-sql"></a>ALL (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Compare une valeur scalaire avec un ensemble de valeurs appartenant à une seule colonne.  
  
 ![Icône Lien d’article](../../database-engine/configure-windows/media/topic-link.gif "Icône Lien d’article") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
scalar_expression { = | <> | != | > | >= | !> | < | <= | !< } ALL ( subquery )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *scalar_expression*  
 Toute [expression](../../t-sql/language-elements/expressions-transact-sql.md) valide.  
  
 { = \| <> \| != \| > \| >= \| !> \| < \| <= \| !< }  
 Opérateur de comparaison.  
  
 *subquery*  
 Sous-requête dont le jeu de résultats est une colonne. Le type de données de la colonne retournée doit être le même que celui de *scalar_expression*.  
  
 Instruction SELECT limitée, dans laquelle la clause ORDER BY et le mot clé INTO ne sont pas autorisés.  
  
## <a name="result-types"></a>Types des résultats  
 **Booléen**  
  
## <a name="result-value"></a>Valeur des résultats  
 Retourne la valeur TRUE quand la comparaison spécifiée a la valeur TRUE pour toutes les paires (_scalar_expression_ **,** _x)_ , quand *x* est une valeur du jeu de la colonne. Sinon, retourne la valeur FALSE.  
  
## <a name="remarks"></a>Notes  
 ALL requiert que l’argument *scalar_expression* corresponde à toutes les valeurs retournées par la sous-requête. Par exemple, si la sous-requête retourne les valeurs 2 et 3, *scalar_expression* < = ALL (sous-requête) a la valeur TRUE pour une *scalar_expression* égale à 2. Si la sous-requête retourne les valeurs 2 et 3, l’instruction *scalar_expression* = ALL (sous-requête) donne FALSE, étant donné que certaines des valeurs de la sous-requête (à savoir 3) ne répondent pas aux critères de l’expression.  
  
 Pour les instructions qui demandent que l’argument *scalar_expression* corresponde à seulement une des valeurs retournées par la sous-requête, consultez [SOME &#124; ANY &#40;Transact-SQL&#41;](../../t-sql/language-elements/some-any-transact-sql.md).  
  
 Cet article fait référence à l’instruction ALL lorsqu’elle est utilisée avec une sous-requête. ALL peut aussi être utilisé avec [UNION](../../t-sql/language-elements/set-operators-union-transact-sql.md) et [SELECT](../../t-sql/queries/select-transact-sql.md).  
  
## <a name="examples"></a>Exemples  
 L’exemple suivant crée une procédure stockée qui détermine si tous les composants d’un `SalesOrderID` spécifié dans la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] peuvent être fabriqués dans le délai du nombre de jours spécifié. L’exemple utilise une sous-requête pour créer une liste du nombre de valeurs `DaysToManufacture` pour tous les composants du `SalesOrderID` spécifié, puis vérifie que toutes les valeurs `DaysToManufacture` sont inférieures ou égales au nombre de jours spécifié.  
  
```sql  
-- Uses AdventureWorks  
  
CREATE PROCEDURE DaysToBuild @OrderID INT, @NumberOfDays INT  
AS  
IF   
@NumberOfDays >= ALL  
   (  
    SELECT DaysToManufacture  
    FROM Sales.SalesOrderDetail  
    JOIN Production.Product   
    ON Sales.SalesOrderDetail.ProductID = Production.Product.ProductID   
    WHERE SalesOrderID = @OrderID  
   )  
PRINT 'All items for this order can be manufactured in specified number of days or less.'  
ELSE   
PRINT 'Some items for this order can''t be manufactured in specified number of days or less.' ;  
```  
  
 Pour tester la procédure, exécutez-la en utilisant le `SalesOrderID 49080` dont l'un des composants demande `2` jours de fabrication, tandis que les 2 autres en demandent 0. La première instruction ci-dessous remplit les critères, Ce n’est pas le cas de la deuxième requête.  
  
```sql  
EXECUTE DaysToBuild 49080, 2 ;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 `All items for this order can be manufactured in specified number of days or less.`  
  
```sql  
EXECUTE DaysToBuild 49080, 1 ;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 `Some items for this order can't be manufactured in specified number of days or less.`  
  
## <a name="see-also"></a>Voir aussi  
 [CASE &#40;Transact-SQL&#41;](../../t-sql/language-elements/case-transact-sql.md)   
 [Expressions &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md)   
 [Fonctions intégrées &#40;Transact-SQL&#41;](~/t-sql/functions/functions.md)   
 [LIKE &#40;Transact-SQL&#41;](../../t-sql/language-elements/like-transact-sql.md)   
 [Opérateurs &#40;Transact-SQL&#41;](../../t-sql/language-elements/operators-transact-sql.md)   
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
 [WHERE &#40;Transact-SQL&#41;](../../t-sql/queries/where-transact-sql.md)   
 [IN &#40;Transact-SQL&#41;](../../t-sql/language-elements/in-transact-sql.md)  
  
