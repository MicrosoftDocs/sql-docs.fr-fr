---
description: '&amp; (AND au niveau du bit) (Transact-SQL)'
title: '&amp; (AND au niveau du bit) (Transact-SQL) | Microsoft Docs'
ms.custom: ''
ms.date: 01/10/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- bitwise
- '&'
- '&_TSQL'
dev_langs:
- TSQL
helpviewer_keywords:
- AND, bitwise AND
- '& (bitwise AND)'
- bitwise AND (&)
ms.assetid: 20275755-4fa7-47b1-a9be-ac85606d63b0
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: a6d75027d1ccd171a8b860bfa536da0b51fb457b
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095977"
---
# <a name="amp-bitwise-and-transact-sql"></a>&amp; (AND au niveau du bit) (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Effectue une opération AND logique au niveau du bit avec deux valeurs entières.  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
expression & expression  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *expression*  
 Toute [expression](../../t-sql/language-elements/expressions-transact-sql.md) valide de tout type de données de la catégorie entier, ou de type **bit**, **binary** ou **varbinary**. L’*expression* est traitée comme un nombre binaire pour l’opération au niveau du bit.  
  
> [!NOTE]  
>  Dans une opération au niveau du bit, une seule *expression* peut être de type **binary** ou **varbinary**.  
  
## <a name="result-types"></a>Types des résultats  
 **int** si les valeurs d’entrée sont **int**.  
  
 **smallint** si les valeurs d’entrée sont **smallint**.  
  
 **tinyint** si les valeurs d’entrée sont **tinyint** ou **bit**.  
  
## <a name="remarks"></a>Notes  
 L’opérateur au niveau du bit **&** effectue une opération AND logique au niveau du bit entre les deux expressions, en prenant chaque bit correspondant dans chacune des expressions. Dans le résultat, les bits prennent la valeur 1 si et seulement si les deux bits correspondants (pour la position en cours d'évaluation) dans les deux expressions d'entrées ont une valeur de 1 ; si tel n'est pas le cas, le bit résultant est mis à 0.  
  
 Si les expressions de droite et de gauche ont des types de données integer (entier) différents (par exemple, l’*expression* de gauche est de type **smallint** et l’*expression* de droite est de type **int**), l’argument du type de données le plus petit est converti dans le type de données plus grand. Dans cet exemple, l’**expression** _smallint_ est convertie en un **int**.  
  
## <a name="examples"></a>Exemples  
 L’exemple suivant crée une table avec le type de données **int** pour le stockage des valeurs et place deux valeurs dans une seule ligne.  
  
```sql
CREATE TABLE bitwise (   
  a_int_value INT NOT NULL,  
  b_int_value INT NOT NULL);  
GO  
INSERT bitwise VALUES (170, 75);  
GO  
```  
  
 Cette requête exécute le AND au niveau du bit entre les deux colonnes `a_int_value` et `b_int_value`.  
  
```sql  
SELECT a_int_value & b_int_value  
FROM bitwise;  
GO  
```  
  
 Voici l'ensemble de résultats obtenu :  
  
```  
-----------   
10            
  
(1 row(s) affected)  
```  
  
 La représentation binaire de 170 (`a_int_value` ou `A`) est `0000 0000 1010 1010`. La représentation binaire de 75 (`b_int_value` ou `B`) est `0000 0000 0100 1011`. L'exécution de l'opération AND au niveau du bit sur ces deux valeurs produit le résultat binaire `0000 0000 0000 1010`, qui est la valeur décimale 10.  
  
```  
(A & B)  
0000 0000 1010 1010  
0000 0000 0100 1011  
-------------------  
0000 0000 0000 1010  
```  
  
  
## <a name="see-also"></a>Voir aussi  
 [Expressions &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md)   
 [Opérateurs &#40;Transact-SQL&#41;](../../t-sql/language-elements/operators-transact-sql.md)   
 [Opérateurs au niveau du bit &#40;Transact-SQL&#41;](../../t-sql/language-elements/bitwise-operators-transact-sql.md)   
 [&= &#40;Affectation après AND au niveau du bit&#41; &#40;Transact-SQL&#41;](../../t-sql/language-elements/bitwise-and-equals-transact-sql.md)   
 [Opérateurs composés &#40;Transact-SQL&#41;](../../t-sql/language-elements/compound-operators-transact-sql.md)  
  
  


