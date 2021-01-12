---
description: Opérateurs composés (Transact-SQL)
title: Opérateurs composés (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- compound operators
- compound operators, described
ms.assetid: 5072fe91-02d3-42a7-831f-756eff714a17
author: cawrites
ms.author: chadam
ms.openlocfilehash: 2666168b9123d5656c2e9822cb8c5bfc328e9a0f
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092189"
---
# <a name="compound-operators-transact-sql"></a>Opérateurs composés (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Les opérateurs composés exécutent une opération particulière et affectent une valeur d'origine au résultat de l'opération. Par exemple, si une variable @x est égale à 35, alors @x += 2 prend la valeur d’origine de @x, ajoute 2 et affecte à @x cette nouvelle valeur (37).  
  
 [!INCLUDE[tsql](../../includes/tsql-md.md)] fournit les types d'opérateurs composés suivants :  
  
|Opérateur|Lien vers des informations  supplémentaires|Action|  
|--------------|------------------------------|------------|  
|+=|[+= &#40;Affectation après addition&#41; &#40;Transact-SQL&#41;](../../t-sql/language-elements/add-equals-transact-sql.md)|Ajoute un montant à la valeur d'origine et affecte la valeur d'origine au résultat.|  
|-=|[-= &#40;Affectation après soustraction&#41; &#40;Transact-SQL&#41;](../../t-sql/language-elements/subtract-equals-transact-sql.md)|Soustrait un montant de la valeur d'origine et affecte la valeur d'origine au résultat.|  
|*=|[&#42;= &#40;Affectation après multiplication&#41; &#40;Transact-SQL&#41;](../../t-sql/language-elements/multiply-equals-transact-sql.md)|Multiplie par un montant et affecte la valeur d'origine au résultat.|  
|/=|[&#40;Affectation après division&#41; &#40;Transact-SQL&#41;](../../t-sql/language-elements/divide-equals-transact-sql.md)|Divise par un montant et affecte la valeur d'origine au résultat.|  
|%=|[Affectation du reste &#40;Transact-SQL&#41;](../../t-sql/language-elements/modulo-equals-transact-sql.md)|Divise par un montant et affecte la valeur d'origine au modulo.|  
|&=|[&= &#40;Affectation après AND au niveau du bit&#41; &#40;Transact-SQL&#41;](../../t-sql/language-elements/bitwise-and-equals-transact-sql.md)|Effectue une opération de bits AND et affecte la valeur d'origine au résultat.|  
|^=|[^= &#40;Affectation après OR exclusif au niveau du bit&#41; &#40;Transact-SQL&#41;](../../t-sql/language-elements/bitwise-exclusive-or-equals-transact-sql.md)|Effectue une opération de bits OR exclusive et affecte la valeur d'origine au résultat.|  
|&#124;=|[&#124;= &#40;Affectation après OR au niveau du bit&#41; &#40;Transact-SQL&#41;](../../t-sql/language-elements/bitwise-or-equals-transact-sql.md)|Effectue une opération de bits OR et affecte la valeur d'origine au résultat.|  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
expression operator expression  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *expression*  
 Toute [expression](../../t-sql/language-elements/expressions-transact-sql.md) valide de tout type de données de la catégorie numérique.  
  
## <a name="result-types"></a>Types des résultats  
 Retourne le type de données de l'argument ayant la priorité la plus élevée. Pour plus d’informations, consultez [Priorités des types de données &#40;Transact-SQL&#41;](../../t-sql/data-types/data-type-precedence-transact-sql.md).  
  
## <a name="remarks"></a>Notes  
 Pour plus d'informations, consultez les rubriques relatives à chaque opérateur.  
  
## <a name="examples"></a>Exemples  
 Les exemples suivants illustrent des opérations composées.  
  
```sql  
DECLARE @x1 INT = 27;  
SET @x1 += 2 ;  
SELECT @x1 AS Added_2;  
  
DECLARE @x2 INT = 27;  
SET @x2 -= 2 ;  
SELECT @x2 AS Subtracted_2;  
  
DECLARE @x3 INT = 27;  
SET @x3 *= 2 ;  
SELECT @x3 AS Multiplied_by_2;  
  
DECLARE @x4 INT = 27;  
SET @x4 /= 2 ;  
SELECT @x4 AS Divided_by_2;  
  
DECLARE @x5 INT = 27;  
SET @x5 %= 2 ;  
SELECT @x5 AS Modulo_of_27_divided_by_2;  
  
DECLARE @x6 INT = 9;  
SET @x6 &= 13 ;  
SELECT @x6 AS Bitwise_AND;  
  
DECLARE @x7 INT = 27;  
SET @x7 ^= 2 ;  
SELECT @x7 AS Bitwise_Exclusive_OR;  
  
DECLARE @x8 INT = 27;  
SET @x8 |= 2 ;  
SELECT @x8 AS Bitwise_OR;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Opérateurs &#40;Transact-SQL&#41;](../../t-sql/language-elements/operators-transact-sql.md)   
 [Opérateurs au niveau du bit &#40;Transact-SQL&#41;](../../t-sql/language-elements/bitwise-operators-transact-sql.md)  
  
  
