---
description: '&lt;= (Inférieur ou égal à) (Transact-SQL)'
title: '&lt;= (Inférieur ou égal à) (Transact-SQL) | Microsoft Docs'
ms.custom: ''
ms.date: 03/13/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- <=
- Less
- Equal To
- <=_TSQL
- Less Than
- Equal
dev_langs:
- TSQL
helpviewer_keywords:
- <= (less than or equal to operator)
- less than or equal to operator (<=)
ms.assetid: 1f05474c-0377-48cb-b567-9d85d0c40479
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 069c1400b5e49be2376994eee8981ab23327a6f6
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98086170"
---
# <a name="lt-less-than-or-equal-to-transact-sql"></a>&lt;= (Inférieur ou égal à) (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Compare deux expressions (opérateur de comparaison). Lorsque vous comparez des expressions non nulles, le résultat est TRUE si l'opérande de gauche a une valeur inférieure ou égale à celui de droite ; sinon le résultat est FALSE.  
  
 Contrairement à l’opérateur de comparaison = (égalité), le résultat de la comparaison >= de deux valeurs NULL ne dépend pas du paramètre ANSI_NULLS.  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql  
expression <= expression  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *expression*  
 Toute [expression](../../t-sql/language-elements/expressions-transact-sql.md) valide. Les deux expressions doivent avoir des types de données implicitement convertibles. La conversion dépend des règles de [priorité des types de données](../../t-sql/data-types/data-type-precedence-transact-sql.md).  
  
## <a name="result-types"></a>Types des résultats  
 **Booléen**  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-using--in-a-simple-query"></a>R. Utilisation de <= dans une requête simple  
 L'exemple suivant retourne toutes les lignes de la table `HumanResources.Department` qui ont une valeur dans `DepartmentID` inférieure ou égale à la valeur 3.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT DepartmentID, Name  
FROM HumanResources.Department  
WHERE DepartmentID <= 3  
ORDER BY DepartmentID;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
DepartmentID Name  
------------ --------------------------------------------------  
1            Engineering  
2            Tool Design  
3            Sales  
  
(3 row(s) affected)  
  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Types de données &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)   
 [Opérateurs &#40;Transact-SQL&#41;](../../t-sql/language-elements/operators-transact-sql.md)  
  
  
