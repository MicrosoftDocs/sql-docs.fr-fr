---
description: Fonctions mathématiques (Transact-SQL)
title: Fonctions mathématiques (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/06/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- calculations [SQL Server]
- mathematical functions [SQL Server]
- functions [SQL Server], mathematical
ms.assetid: 46495a2e-81d0-4677-9d72-9db083cd1023
author: cawrites
ms.author: chadam
ms.openlocfilehash: 2529117d5087a083fc32da30689129dd49733907
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195989"
---
# <a name="mathematical-functions-transact-sql"></a>Fonctions mathématiques (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Les fonctions scalaires suivantes effectuent un calcul, généralement basé sur les valeurs d'entrée fournies en tant qu'arguments, et retournent une valeur numérique :  
  
- [ABS](../../t-sql/functions/abs-transact-sql.md)
- [ACOS](../../t-sql/functions/acos-transact-sql.md)
- [ASIN](../../t-sql/functions/asin-transact-sql.md)
- [ATAN](../../t-sql/functions/atan-transact-sql.md)
- [ATN2](../../t-sql/functions/atn2-transact-sql.md)
- [CEILING](../../t-sql/functions/ceiling-transact-sql.md)
- [COS](../../t-sql/functions/cos-transact-sql.md)
- [COT](../../t-sql/functions/cot-transact-sql.md)
- [DEGREES](../../t-sql/functions/degrees-transact-sql.md)
- [EXP](../../t-sql/functions/exp-transact-sql.md)
- [FLOOR](../../t-sql/functions/floor-transact-sql.md)
- [LOG](../../t-sql/functions/log-transact-sql.md)
- [LOG10](../../t-sql/functions/log10-transact-sql.md)
- [PI](../../t-sql/functions/pi-transact-sql.md)
- [POWER](../../t-sql/functions/power-transact-sql.md)
- [RADIANS](../../t-sql/functions/radians-transact-sql.md)  
- [RAND](../../t-sql/functions/rand-transact-sql.md)  
- [ROUND](../../t-sql/functions/round-transact-sql.md)  
- [SIGN](../../t-sql/functions/sign-transact-sql.md)  
- [SIN](../../t-sql/functions/sin-transact-sql.md)  
- [SQRT](../../t-sql/functions/sqrt-transact-sql.md)  
- [SQUARE](../../t-sql/functions/square-transact-sql.md)  
- [TAN](../../t-sql/functions/tan-transact-sql.md)
  
> [!NOTE]  
> Les fonctions arithmétiques telles que ABS, CEILING, DEGREES, FLOOR, POWER, RADIANS et SIGN retournent une valeur du même type que la valeur d'entrée. Les fonctions trigonométriques et autres, y compris EXP, LOG, LOG10, SQUARE et SQRT, convertissent leurs valeurs d’entrée en valeurs **float** et renvoient une valeur **float**.  
  
Toutes les fonctions mathématiques, à l’exception de RAND, sont des fonctions déterministes. Cela signifie qu’elles retournent les mêmes résultats chaque fois qu’elles sont appelées avec un ensemble spécifique de valeurs d’entrée. RAND est déterministe uniquement lorsqu'une valeur de départ est spécifiée. Pour plus d’informations sur le déterminisme des fonctions, consultez [Fonctions déterministes et non déterministes](../../relational-databases/user-defined-functions/deterministic-and-nondeterministic-functions.md).  
  
## <a name="see-also"></a>Voir aussi

- [Opérateurs arithmétiques &#40;Transact-SQL&#41;](../../t-sql/language-elements/arithmetic-operators-transact-sql.md)
- [Fonctions intégrées &#40;Transact-SQL&#41;](~/t-sql/functions/functions.md)  
