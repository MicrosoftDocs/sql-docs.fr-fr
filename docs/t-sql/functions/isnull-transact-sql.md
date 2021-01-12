---
description: ISNULL (Transact-SQL)
title: ISNULL (Transact-SQL)
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ISNULL
- ISNULL_TSQL
- IFNULL_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- replacing null values
- null values [SQL Server], ISNULL
- null values [SQL Server], replacement values
- ISNULL function
ms.assetid: 6f3e5802-864b-4e77-9862-657bb5430b68
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 1a3a5b4a1f22a84435c0658c3e0cfc8a14c6f086
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100469"
---
# <a name="isnull-transact-sql"></a>ISNULL (Transact-SQL) 

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Remplace NULL par la valeur de remplacement spécifiée.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
ISNULL ( check_expression , replacement_value )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *check_expression*  
 [Expression](../../t-sql/language-elements/expressions-transact-sql.md) dans laquelle la valeur NULL est recherchée. *check_expression* peut être de n’importe quel type.  
  
 *replacement_value*  
 Expression à renvoyer si *check_expression* a la valeur NULL. *replacement_value* doit être d’un type pouvant être implicitement converti vers le type de *check_expression*.  
  
## <a name="return-types"></a>Types de retour  
 Renvoie le même type que *check_expression*. Si une valeur littérale NULL est fournie en tant que *check_expression*, renvoie le type de données de *replacement_value*. Si une valeur littérale NULL est fournie en tant que *check_expression* et qu’aucun argument *replacement_value* n’est fourni, renvoie un type **int**.  
  
## <a name="remarks"></a>Notes  
 La valeur de *check_expression* est renvoyée si elle n’est pas NULL ; sinon, l’argument *replacement_value* est renvoyé après avoir été implicitement converti vers le type de *check_expression*, si les types sont différents. *replacement_value* peut être tronqué si *replacement_value* dépasse *check_expression*.  
  
> [!NOTE]  
>  Utilisez [COALESCE &#40;Transact-SQL&#41;](../../t-sql/language-elements/coalesce-transact-sql.md) pour renvoyer la première valeur non-NULL.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-using-isnull-with-avg"></a>R. Utilisation de ISNULL avec AVG  
 L'exemple suivant recherche la moyenne du poids de tous les produits. La valeur `50` se substitue à toutes les entrées NULL de la colonne `Weight` de la table `Product`.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT AVG(ISNULL(Weight, 50))  
FROM Production.Product;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
 -------------------------- 
59.79  
  
 (1 row(s) affected)
 ```  
  
### <a name="b-using-isnull"></a>B. Utilisation de ISNULL  
 Dans l'exemple suivant, la description (« Description »), le pourcentage de remise (« DiscountPct »), la quantité minimale (« MinQty ») et la quantité maximale (« Max Quantity ») sont sélectionnés pour toutes les offres spéciales figurant dans `AdventureWorks2012`. Si la quantité maximale associée à une offre spéciale déterminée est NULL, `MaxQty` présente la valeur `0.00` dans l'ensemble de résultats.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT Description, DiscountPct, MinQty, ISNULL(MaxQty, 0.00) AS 'Max Quantity'  
FROM Sales.SpecialOffer;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
|  Description       |  DiscountPct    |   MinQty    |   Quantité maximale       |
|  ---------------   |  -------------  |   --------  |   ---------------    |
|  No Discount       |  0,00           |   0         |   0                  |
|  Volume Discount   |  0,02           |   11        |   14                 |
|  Volume Discount   |  0.05           |   15        |   4                  |
|  Volume Discount   |  0.10           |   25        |   0                  |
|  Volume Discount   |  0.15           |   41        |   0                  |
|  Volume Discount   |  0.20           |   61        |   0                  |
|  Mountain-100 Cl   |  0,35           |   0         |   0                  |
|  Sport Helmet Di   |  0.10           |   0         |   0                  |
|  Road-650 Overst   |  0.30           |   0         |   0                  |
|  Mountain Tire S   |  0.50           |   0         |   0                  |
|  Sport Helmet Di   |  0.15           |   0         |   0                  |
|  LL Road Frame S   |  0,35           |   0         |   0                  |
|  Touring-3000 Pr   |  0.15           |   0         |   0                  |
|  Touring-1000 Pr   |  0.20           |   0         |   0                  |
|  Half-Price Peda   |  0.50           |   0         |   0                  |
|  Mountain-500 Si   |  0,40           |   0         |   0                  |

 `(16 row(s) affected)`  
  
### <a name="c-testing-for-null-in-a-where-clause"></a>C. Test de valeur NULL dans une clause WHERE  
 N'utilisez pas ISNULL pour rechercher des valeurs NULL. Utilisez IS NULL à la place. L'exemple suivant recherche tous les produits qui comportent `NULL` dans la colonne weight. Notez l'espace entre `IS` et `NULL`.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT Name, Weight  
FROM Production.Product  
WHERE Weight IS NULL;  
GO  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Exemples : [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="d-using-isnull-with-avg"></a>D. Utilisation de ISNULL avec AVG  
 L’exemple suivant recherche la moyenne du poids de tous les produits dans un exemple de table. La valeur `50` se substitue à toutes les entrées NULL de la colonne `Weight` de la table `Product`.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT AVG(ISNULL(Weight, 50))  
FROM dbo.DimProduct;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
--------------------------   
52.88   
```  
  
### <a name="e-using-isnull"></a>E. Utilisation de ISNULL  
 L’exemple suivant utilise ISNULL pour tester les valeurs NULL dans la colonne `MinPaymentAmount` et afficher la valeur `0.00` pour les lignes.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT ResellerName,   
       ISNULL(MinPaymentAmount,0) AS MinimumPayment  
FROM dbo.DimReseller  
ORDER BY ResellerName;  
  
```  
  
 Voici un jeu de résultats partiel.  
  
|  ResellerName                |  MinimumPayment    |
|  -------------------------   |  --------------    |
|  A Bicycle Association       |     0,0000         |
|  A Bike Store                |     0,0000         |
|  A Cycle Shop                |     0,0000         |
|  A Great Bicycle Company     |     0,0000         |
|  A Typical Bike Shop         |   200,0000         |
|  Acceptable Sales & Service  |     0,0000         |
  
### <a name="f-using-is-null-to-test-for-null-in-a-where-clause"></a>F. Utilisation de la fonction IS NULL pour tester les valeurs NULL dans une clause WHERE  
 L’exemple suivant recherche tous les produits qui comportent `NULL` dans la colonne `Weight`. Notez l'espace entre `IS` et `NULL`.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT EnglishProductName, Weight  
FROM dbo.DimProduct  
WHERE Weight IS NULL;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Expressions &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md)   
 [IS NULL &#40;Transact-SQL&#41;](../../t-sql/queries/is-null-transact-sql.md)   
 [Fonctions système &#40;Transact-SQL&#41;](../../relational-databases/system-functions/system-functions-category-transact-sql.md)   
 [WHERE &#40;Transact-SQL&#41;](../../t-sql/queries/where-transact-sql.md)   
 [COALESCE &#40;Transact-SQL&#41;](../../t-sql/language-elements/coalesce-transact-sql.md)  
  
  

