---
description: '% (Reste) (Transact-SQL)'
title: '% (Reste) (Transact-SQL) | Microsoft Docs'
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- modulo
- modulus
- '% (Modulo)'
- '% (Modulus)'
- MOD_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- '% (modulo operator)'
- '% (modulus operator)'
- remainder of division operation
- modulo operator (%)
- modulus operator (%)
ms.assetid: f93c662e-f405-486e-bf23-a2d03907b5bd
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 34b80f69ef53e5cfa3c6c52edbdf3a78c8058e3c
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98083701"
---
# <a name="-modulus-transact-sql"></a>% (Reste) (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Renvoie le reste d'un nombre divisé par un autre.  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql  
dividend % divisor  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *dividend*  
 Expression numérique à diviser. *dividend* doit être une [expression](../../t-sql/language-elements/expressions-transact-sql.md) valide de l’un des types de données des catégories entier et monétaire, ou bien du type de données **numeric**.  
  
 *divisor*  
 Expression numérique par laquelle diviser le dividende. *divisor* doit être une expression valide de l’un des types de données des catégories entier et monétaire, ou bien du type de données **numeric**.  
  
## <a name="result-types"></a>Types des résultats  
 Déterminés par les types de données des deux arguments.  
  
## <a name="remarks"></a>Remarques  
 Vous pouvez utiliser l’opérateur arithmétique modulo dans la liste de sélection de l’instruction SELECT, avec toute combinaison de noms de colonnes, de constantes numériques ou toute expression valide de l’un des types de données des catégories entier et monétaire, ou bien du type **numeric**.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-simple-example"></a>R. Exemple simple  
 L'exemple suivant divise le nombre 38 par 5. Cette opération produit le nombre 7 comme partie entière du résultat, et montre comment le modulo retourne le reste 3.  
  
```sql  
SELECT 38 / 5 AS Integer, 38 % 5 AS Remainder;
```  
  
### <a name="b-example-using-columns-in-a-table"></a>B. Exemple utilisant des colonnes dans une table  
 L'exemple suivant renvoie le numéro d'identification et le prix unitaire du produit, ainsi que le reste (modulo) de la division du prix de chaque produit, converti en valeur entière, par le nombre de produits commandés.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT TOP(100)ProductID, UnitPrice, OrderQty,  
   CAST((UnitPrice) AS INT) % OrderQty AS Modulo  
FROM Sales.SalesOrderDetail;  
GO  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Exemples : [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="c-simple-example"></a>C. Exemple simple  
 L’exemple suivant montre les résultats de l’opérateur `%` lors d’une division de 3 par 2.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT TOP(1) 3%2 FROM dimEmployee;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
---------   
1         
```  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions intégrées &#40;Transact-SQL&#41;](~/t-sql/functions/functions.md)   
 [LIKE &#40;Transact-SQL&#41;](../../t-sql/language-elements/like-transact-sql.md)   
 [Opérateurs &#40;Transact-SQL&#41;](../../t-sql/language-elements/operators-transact-sql.md)   
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
 [%= &#40;Affectation du reste&#41; &#40;Transact-SQL&#41;](../../t-sql/language-elements/modulo-equals-transact-sql.md)   
 [Opérateurs composés &#40;Transact-SQL&#41;](../../t-sql/language-elements/compound-operators-transact-sql.md)  
  
  


