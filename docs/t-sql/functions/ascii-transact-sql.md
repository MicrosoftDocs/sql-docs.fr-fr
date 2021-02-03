---
description: ASCII (Transact-SQL)
title: ASCII (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/14/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ASCII_TSQL
- ASCII
dev_langs:
- TSQL
helpviewer_keywords:
- ASCII function
- characters [SQL Server], ASCII
- code [SQL Server], ASCII
- leftmost character of expression
ms.assetid: 45c2044a-0593-4805-8bae-0fad4bde2e6b
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 57d1d99951784c4126accc3862bc86b5bc9dce7d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193479"
---
# <a name="ascii-transact-sql"></a>ASCII (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Retourne la valeur du code ASCII du caractère situé le plus à gauche dans une expression de caractères.
  
![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
ASCII ( character_expression )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
*expression_caractère*  
[Expression](../../t-sql/language-elements/expressions-transact-sql.md) de type **char** ou **varchar**.
  
## <a name="return-types"></a>Types de retour
 **int**  
  
## <a name="remarks"></a>Notes
ASCII est l’abréviation d’**A** merican **S** tandard **C** ode for **I** nformation **I** nterchange. Il s’agit d’une norme de codage de caractères pour les ordinateurs modernes. Consultez la section **Caractères imprimables** de la rubrique [ASCII](https://www.wikipedia.org/wiki/ASCII) pour obtenir la liste des caractères ASCII.

Le code ASCII est un jeu de caractères 7 bits. Le code ASCII étendu est un jeu de caractères 8 bits qui n’est pas géré par la fonction `ASCII`. 

## <a name="examples"></a>Exemples 

### <a name="a-this-example-assumes-an-ascii-character-set-and-returns-the-ascii-value-for-6-characters"></a>R. Cet exemple considère un jeu de caractères ASCII et retourne la valeur `ASCII` de 6 caractères.
  
```sql
SELECT ASCII('A') AS A, ASCII('B') AS B,   
ASCII('a') AS a, ASCII('b') AS b,  
ASCII(1) AS [1], ASCII(2) AS [2];  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
A           B           a           b           1           2  
----------- ----------- ----------- ----------- ----------- -----------  
65          66          97          98          49          50  
```  
  
### <a name="b-this-examples-shows-how-a-7-bit-ascii-value-is-returned-correctly-but-an-8-bit-extended-ascii-value-is-not-handled"></a>B. Cet exemple montre comment une valeur ASCII 7 bits est retournée correctement, tandis qu’une valeur ASCII étendue 8 bits n’est pas gérée.

```sql
SELECT ASCII('P') AS [ASCII], ASCII('æ') AS [Extended_ASCII];
```

[!INCLUDE[ssResult](../../includes/ssresult-md.md)]

```
ASCII       Extended_ASCII
----------- --------------
80          195
```

Pour vérifier si les résultats ci-dessus sont mappés au point de code de caractère correct, utilisez les valeurs de sortie avec la fonction `CHAR` ou `NCHAR` :

```sql
SELECT NCHAR(80) AS [CHARACTER], NCHAR(195) AS [CHARACTER];
```

[!INCLUDE[ssResult](../../includes/ssresult-md.md)]

```
CHARACTER CHARACTER
--------- ---------
P         Ã
```

À partir du résultat précédent, remarquez que le caractère du point de code 195 est **Ã** et non **æ**. Cela est dû au fait que la fonction `ASCII` est capable de lire le premier flux de 7 bits, mais pas le bit supplémentaire. Le point de code correct pour le caractère `æ` se trouve à l’aide de la fonction `UNICODE`, qui est capable de retourner le point de code de caractère correct :

```sql
SELECT UNICODE('æ') AS [Extended_ASCII], NCHAR(230) AS [CHARACTER];
```

[!INCLUDE[ssResult](../../includes/ssresult-md.md)]

```
Extended_ASCII CHARACTER
-------------- ---------
230            æ
```

## <a name="see-also"></a>Voir aussi
 [CHAR &#40;Transact-SQL&#41;](../../t-sql/functions/char-transact-sql.md)  
 [NCHAR &#40;Transact-SQL&#41;](../../t-sql/functions/nchar-transact-sql.md)  
 [UNICODE &#40;Transact-SQL&#41;](../../t-sql/functions/unicode-transact-sql.md)  
 [Fonctions de chaîne &#40;Transact-SQL&#41;](../../t-sql/functions/string-functions-transact-sql.md)
  
