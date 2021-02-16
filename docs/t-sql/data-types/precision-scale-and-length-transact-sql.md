---
description: Précision, échelle et longueur (Transact-SQL)
title: Précision, échelle et longueur (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/22/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- data types [SQL Server], length
- data types [SQL Server], scale
- precision [SQL Server], data types
- lengths [SQL Server], data types
- number of digits
- scale [SQL Server]
- scale [SQL Server], data types
- data types [SQL Server], precision
ms.assetid: fbc9ad2c-0d3b-4e98-8fdd-4d912328e40a
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: da816b6e55bc1578a820a53d6e1036b685086713
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338042"
---
# <a name="precision-scale-and-length-transact-sql"></a>Précision, échelle et longueur (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

La précision est le nombre de chiffres qui composent un nombre. L'échelle est le nombre de chiffres à droite du séparateur décimal dans un nombre. Par exemple, le nombre 123,45 a une précision de 5 et une échelle de 2.
  
Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], la précision maximale par défaut des types de données **numeric** et **decimal** est 38. Dans les versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], elle est de 28.
  
La longueur d'un type de données numérique est le nombre d'octets utilisés pour stocker le nombre. Pour les types varchar et char, la longueur d’une chaîne de caractères est le nombre d’octets. Pour les types nvarchar et nchar, la longueur d’une chaîne de caractères est le nombre de paires d’octets. La longueur des types de données **binary**, **varbinary** et **image** est le nombre d’octets. Par exemple, un type de données **int** peut contenir 10 chiffres, est stocké sur 4 octets et n’accepte pas de virgule décimale. La précision du type **int** est 10, sa longueur est 4 et son échelle est 0.
  
Quand deux expressions de type **char**, **varchar**, **binary** ou **varbinary** sont concaténées, la longueur de l’expression obtenue est la somme des longueurs des deux expressions sources, dans la limite de 8 000 octets.
  
Quand deux expressions de type **nchar** ou **nvarchar** sont concaténées, la longueur de l’expression obtenue est la somme des longueurs des deux expressions sources, dans la limite de 4 000 paires d’octets.
  
Lors de la comparaison de deux expressions du même type de données, mais de longueur différente, à l’aide des fonctions UNION, EXCEPT ou INTERSECT, la longueur obtenue est la plus longue des deux expressions.
  
La précision et l’échelle des types de données numériques sont constantes, sauf pour le type **decimal**. Quand un opérateur arithmétique agit sur deux expressions du même type, le résultat est du même type de données, avec la précision et l’échelle définies pour ce type. Si un opérateur agit sur deux expressions de types de données numériques différents, les règles de priorité des types de données déterminent le type du résultat. Le résultat a la précision et l'échelle définies pour son type de données.
  
Le tableau suivant montre le calcul de la précision et de l’échelle quand le résultat d’une opération est du type **decimal**. Le résultat est de type **decimal** quand :
-   Les deux expressions sont de type **decimal**.  
-   Une expression est de type **decimal** et l’autre est d’un type de données avec une priorité moins élevée que **decimal**.  
  
Les expressions des opérandes sont notées e1 et e2, avec respectivement les précisions p1 et p2 et les échelles s1 et s2. La précision et l’échelle d’une expression d’un type autre que **decimal** sont celles définies pour le type de données de l’expression. La fonction max(a,b) signifie ceci : prendre la valeur la plus grande entre « a » et « b ». De même, min(a,b) indique de prendre la valeur la plus petite entre « a » et « b ».
  
|Opération|Précision du résultat|Échelle du résultat *|  
|---|---|---|
|e1 + e2|max(s1, s2) + max(p1-s1, p2-s2) + 1|max(s1, s2)|  
|e1 - e2|max(s1, s2) + max(p1-s1, p2-s2) + 1|max(s1, s2)|  
|e1 * e2|p1 + p2 + 1|s1 + s2|  
|e1 / e2|p1 - s1 + s2 + max(6, s1 + p2 + 1)|max(6, s1 + p2 + 1)|  
|e1 { UNION &#124; EXCEPT &#124; INTERSECT } e2|max(s1, s2) + max(p1-s1, p2-s2)|max(s1, s2)|  
|e1 % e2|min(p1-s1, p2 -s2) + max( s1,s2 )|max(s1, s2)|  
  
\* La précision et l’échelle du résultat ne peuvent pas être supérieures à 38. Si la précision d’un résultat est supérieure à 38, elle est ramenée à 38 et l’échelle correspondante est réduite pour tenter d’empêcher la troncation de la partie entière du résultat. Dans certains cas, tels que la multiplication ou la division, le facteur d’échelle n’est pas réduit pour assurer une précision décimale, bien que l’erreur de dépassement puisse être générée.

Dans les opérations d’addition et de soustraction, nous devons avoir `max(p1 - s1, p2 - s2)` pour stocker la partie entière du nombre décimal. Si l’espace est insuffisant pour cela, autrement dit `max(p1 - s1, p2 - s2) < min(38, precision) - scale`, l’échelle est réduite afin de fournir suffisamment d’espace pour la partie entière. L’échelle obtenue étant `MIN(precision, 38) - max(p1 - s1, p2 - s2)`, la partie fractionnaire peut être arrondie pour y contenir.

Dans les opérations de multiplication et de division, nous devons avoir `precision - scale` pour stocker la partie entière du résultat. L’échelle peut être réduite à l’aide des règles suivantes :
1.  L’échelle obtenue est réduite à `min(scale, 38 - (precision-scale))` si la partie entière est inférieure à 32, car elle ne peut pas être supérieure à `38 - (precision-scale)`. Le résultat peut être arrondi dans ce cas.
1. L’échelle ne change pas si elle est inférieure à 6 et si la partie entière est supérieure à 32. Dans ce cas, une erreur de dépassement peut être générée si elle ne peut pas tenir dans decimal(38, scale) 
1. L’échelle est fixée à 6 si elle est supérieure à 6 et si la partie entière est supérieure à 32. Dans ce cas, la partie entière et l’échelle sont toutes deux réduites et le type obtenu est decimal(38,6). Le résultat peut être arrondi à 6 décimales ou l’erreur de dépassement est générée si la partie entière ne peut pas tenir dans 32 chiffres.

## <a name="examples"></a>Exemples
L’expression suivante retourne le résultat `0.00000090000000000` sans arrondi, car le résultat peut tenir dans `decimal(38,17)` :
```sql
select cast(0.0000009000 as decimal(30,20)) * cast(1.0000000000 as decimal(30,20)) [decimal 38,17]
```
Dans ce cas, la précision est égale à 61 et l’échelle à 40.
La partie entière (précision-échelle = 21) étant inférieure à 32, il s’agit du cas (1) dans les règles de multiplication et l’échelle est calculée comme `min(scale, 38 - (precision-scale)) = min(40, 38 - (61-40)) = 17`. Le type de résultat est `decimal(38,17)`.

L’expression suivante retourne le résultat `0.000001` pour tenir dans `decimal(38,6)` :
```sql
SELECT CAST(0.0000009000 AS DECIMAL(30,10)) * CAST(1.0000000000 AS DECIMAL(30,10)) [decimal(38, 6)]
```
Dans ce cas, la précision est égale à 61 et l’échelle à 20.
L’échelle est supérieure à 6 et la partie entière (`precision-scale = 41`) est supérieure à 32. Il s’agit du cas (3) dans les règles de multiplication et le type de résultat est `decimal(38,6)`.

## <a name="see-also"></a>Voir aussi
[Expressions &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md)  
[Types de données &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)
  
