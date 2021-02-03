---
description: ~ (NOT exclusif au niveau du bit) (Transact-SQL)
title: ~ (NOT au niveau du bit) (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/10/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ~_TSQL
- bitwise
- NOT
- "~"
- Bitwise NOT
dev_langs:
- TSQL
helpviewer_keywords:
- NOT keyword
- bitwise NOT (~)
- ~ (bitwise NOT)
ms.assetid: 02da8016-f6c0-41ae-8d59-33eaa02bfc95
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 473ef65d1b2149268e5e3b0b8bd3e76e2c65a20b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199997"
---
# <a name="-bitwise-not-transact-sql"></a>~ (NOT exclusif au niveau du bit) (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Exécute une opération logique NOT au niveau du bit sur une valeur entière.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql  
~ expression  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *expression*  
 Toute [expression](../../t-sql/language-elements/expressions-transact-sql.md) valide de tout type de données de la catégorie entier, ou de type **bit**, **binary** ou **varbinary**. L’*expression* est traitée comme un nombre binaire pour l’opération au niveau du bit.  
  
> [!NOTE]  
>  Une seule *expression* peut être de type **binary** ou **varbinary** dans une opération au niveau du bit.  
  
## <a name="result-types"></a>Types des résultats  
 **int** si les valeurs d’entrée sont **int**.  
  
 **smallint** si les valeurs d’entrée sont **smallint**.  
  
 **tinyint** si les valeurs d’entrée sont **tinyint**.  
  
 **bit** si les valeurs d’entrée sont **bit**.  
  
## <a name="remarks"></a>Notes  
 L’opérateur au niveau du bit **~** exécute une opération NOT logique au niveau du bit sur cette *expression*, en évaluant chaque bit. Si l’*expression* a la valeur 0, les bits du jeu de résultats prennent la valeur 1 ; sinon, le bit résultant est mis à 0. En d'autres termes, les uns sont changés en zéros et les zéros sont changés en uns.  
  
> [!IMPORTANT]  
>  Quelle que soit l'opération au niveau du bit que vous effectuez, la longueur d'enregistrement de l'expression sur laquelle porte l'opération est importante. Il est recommandé d'utiliser le même nombre d'octets lors du stockage des valeurs. Par exemple, le stockage de la valeur décimale 5 en tant que **tinyint**, **smallint** ou **int** produit une valeur stockée avec des nombres d’octets différents : **tinyint** stocke les données sur un 1 octet, **smallint** sur 2 octets et **int** sur 4 octets. En conséquence, l’exécution d’une opération au niveau du bit sur une valeur décimale **int** peut produire des résultats différents de ceux de la même opération sur la même valeur traduite directement en binaire ou en hexadécimal, en particulier avec l’opérateur **~** (NOT au niveau du bit). L'opération NOT au niveau du bit peut se produire sur une variable plus courte. Dans ce cas, lorsque la variable plus courte est convertie en un type de données plus long, les bits des 8 bits de gauche risquent d'avoir une valeur imprévue. Nous vous recommandons de convertir d'abord la variable de type de données courte dans le type de données plus long, et d'exécuter ensuite l'opération NOT sur le résultat.  
  
## <a name="examples"></a>Exemples  
 Cet exemple crée une table en utilisant le type de données **int** pour stocker les valeurs et insère les deux valeurs dans une ligne.  
  
```sql  
CREATE TABLE bitwise (  
  a_int_value INT NOT NULL,  
  b_int_value INT NOT NULL); 
GO  
INSERT bitwise VALUES (170, 75);  
GO  
```  
  
 La requête suivante exécute l'opération NOT au niveau du bit sur les colonnes `a_int_value` et `b_int_value`.  
  
```sql  
SELECT ~ a_int_value, ~ b_int_value  
FROM bitwise;  
```  
  
 Voici l'ensemble de résultats obtenu :  
  
```  
--- ---   
-171  -76   
  
(1 row(s) affected)  
```  
  
 La représentation binaire de 170 (`a_int_value` ou `A`) est `0000 0000 1010 1010`. L'exécution de l'opération NOT au niveau du bit sur cette valeur produit le résultat binaire `1111 1111 0101 0101`, qui est la valeur décimale -171. La représentation binaire pour 75 est `0000 0000 0100 1011`. L'exécution de l'opération NOT au niveau du bit produit `1111 1111 1011 0100`, qui est -76 en notation décimale.  
  
```  
 (~A)     
         0000 0000 1010 1010  
         -------------------  
         1111 1111 0101 0101  
(~B)     
         0000 0000 0100 1011  
         -------------------  
         1111 1111 1011 0100  
```  
  
 
## <a name="see-also"></a>Voir aussi  
 [Expressions &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md)   
 [Opérateurs &#40;Transact-SQL&#41;](../../t-sql/language-elements/operators-transact-sql.md)   
 [Opérateurs au niveau du bit &#40;Transact-SQL&#41;](../../t-sql/language-elements/bitwise-operators-transact-sql.md)  
  
  


