---
description: Fonctions numériques
title: Fonctions numériques | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- functions [ODBC], numeric functions
- numeric functions [ODBC]
ms.assetid: 4fa548dc-e8b0-4179-92ff-81d6a79d10c3
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: bef4a075eefffd0f9ec4b918189d40ae71ea78ec
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207126"
---
# <a name="numeric-functions"></a>Fonctions numériques
Le tableau suivant décrit les fonctions numériques incluses dans le jeu de fonctions scalaires ODBC. En appelant **SQLGetInfo** avec un *type d’informations* SQL_NUMERIC_FUNCTIONS, une application peut déterminer quelles fonctions numériques sont prises en charge par un pilote.  
  
 Toutes les fonctions numériques retournent des valeurs de type de données SQL_FLOAT à l’exception de ABS, ROUND, Truncate, SIGN, FLOOR et CEILING, qui retournent des valeurs du même type de données que les paramètres d’entrée.  
  
 Les arguments dénotés comme *numeric_exp* peuvent être le nom d’une colonne, le résultat d’une autre fonction scalaire ou *Numeric-literala*, où le type de données sous-jacent peut être représenté sous la forme SQL_NUMERIC, SQL_DECIMAL, SQL_TINYINT, SQL_SMALLINT, SQL_INTEGER, SQL_BIGINT, SQL_FLOAT, SQL_REAL ou SQL_DOUBLE.  
  
 Les arguments dénotés comme *float_exp* peuvent être le nom d’une colonne, le résultat d’une autre fonction scalaire ou un *littéral numérique*, où le type de données sous-jacent peut être représenté comme SQL_FLOAT.  
  
 Les arguments dénotés comme *integer_exp* peuvent être le nom d’une colonne, le résultat d’une autre fonction scalaire ou un *littéral numérique*, où le type de données sous-jacent peut être représenté sous la forme SQL_TINYINT, SQL_SMALLINT, SQL_INTEGER ou SQL_BIGINT.  
  
 Les fonctions scalaires CURRENT_DATE, CURRENT_TIME et CURRENT_TIMESTAMP ont été ajoutées dans ODBC 3,0 pour s’aligner sur SQL-92.  
  
|Fonction|Description|  
|--------------|-----------------|  
|**ABS (** _numeric_exp_ **)**  (ODBC 1,0)|Retourne la valeur absolue de *numeric_exp*.|  
|**ACOS (** _float_exp_ **)**  (ODBC 1,0)|Retourne l’arc cosinus de *float_exp* sous la forme d’un angle exprimé en radians.|  
|**ASIN (** _float_exp_ **)**  (ODBC 1,0)|Retourne l’arc sinus de *float_exp* en tant qu’angle exprimé en radians.|  
|**ATAN (** _float_exp_ **)**  (ODBC 1,0)|Retourne l’arc tangente de *float_exp* en tant qu’angle, exprimé en radians.|  
|**Atan2 (** _float_exp1_, _float_exp2_**)**  (ODBC 2,0)|Retourne l’arc tangente des coordonnées *x* et *y* , spécifié par *float_exp1* et *float_exp2*, respectivement, en tant qu’angle, exprimé en radians.|  
|**Ceiling (** _numeric_exp_ **)**  (ODBC 1,0)|Retourne le plus petit entier supérieur ou égal à *numeric_exp*. La valeur de retour est du même type que le paramètre d’entrée.|  
|**Cos (** _float_exp_ **)**  (ODBC 1,0)|Retourne le cosinus de *float_exp*, où *float_exp* est un angle exprimé en radians.|  
|**Cot (** _float_exp_ **)**  (ODBC 1,0)|Retourne la cotangente de *float_exp*, où *float_exp* est un angle exprimé en radians.|  
|**Degrés (** _numeric_exp_ **)**  (ODBC 2,0)|Retourne le nombre de degrés convertis à partir de *numeric_exp* radians.|  
|**Exp (** _float_exp_ **)**  (ODBC 1,0)|Retourne la valeur exponentielle de *float_exp*.|  
|**Floor (** _numeric_exp_ **)**  (ODBC 1,0)|Retourne le plus grand entier inférieur ou égal à *numeric_exp*. La valeur de retour est du même type que le paramètre d’entrée.|  
|**Log (** _float_exp_ **)**  (ODBC 1,0)|Retourne le logarithme népérien de *float_exp*.|  
|**Log10 (** _float_exp_ **)**  (ODBC 2,0)|Retourne le logarithme de base 10 de *float_exp*.|  
|**Mod (** _integer_exp1_, _integer_exp2_**)**  (ODBC 1,0)|Retourne le reste (modulo) de *integer_exp1* divisé par *integer_exp2*.|  
|**Pi ()**  (ODBC 1,0)|Retourne la valeur constante de pi sous la forme d’une valeur à virgule flottante.|  
|**Power (** _numeric_exp_, _integer_exp_**)**  (ODBC 2,0)|Retourne la valeur de *numeric_exp* à la puissance de *integer_exp*.|  
|**Radians (** _numeric_exp_ **)**  (ODBC 2,0)|Retourne le nombre de radians convertis à partir de *numeric_exp* degrés.|  
|**Rand (**[*integer_exp*]**)**  (ODBC 1,0)|Retourne une valeur à virgule flottante aléatoire à l’aide de *integer_exp* comme valeur de départ facultative.|  
|**Round (** _numeric_exp_, _integer_exp_**)**  (ODBC 2,0)|Retourne *numeric_exp* arrondi à *integer_exp* place à droite de la virgule décimale. Si *integer_exp* est négatif, *numeric_exp* est arrondi à &#124;*integer_exp*&#124; place à gauche de la virgule décimale.|  
|**Sign (** _numeric_exp_ **)**  (ODBC 1,0)|Retourne un indicateur du signe de *numeric_exp*. Si *numeric_exp* est inférieur à zéro,-1 est retourné. Si *numeric_exp* est égal à zéro, 0 est retourné. Si *numeric_exp* est supérieur à zéro, 1 est retourné.|  
|**Sin (** _float_exp_ **)**  (ODBC 1,0)|Retourne le sinus de *float_exp*, où *float_exp* est un angle exprimé en radians.|  
|**Sqrt (** _float_exp_ **)**  (ODBC 1,0)|Retourne la racine carrée de *float_exp*.|  
|**Tan (** _float_exp_ **)**  (ODBC 1,0)|Retourne la tangente de *float_exp*, où *float_exp* est un angle exprimé en radians.|  
|**Truncate (** _numeric_exp_, _integer_exp_**)**  (ODBC 2,0)|Retourne *numeric_exp* tronqué à *integer_exp* emplacements à droite de la virgule décimale. Si *integer_exp* est négatif, *numeric_exp* est tronqué en &#124;*integer_exp*&#124; place à gauche de la virgule décimale.|
