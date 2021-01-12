---
description: EXP (Transact-SQL)
title: EXP (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- EXP_TSQL
- EXP
dev_langs:
- TSQL
helpviewer_keywords:
- exponential functions
- EXP function
ms.assetid: 5a9b8c52-6fb6-4e33-8b02-a878785b2f51
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: b3c9429ab47de08584fbc995fa8dfa9c8182ee40
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101212"
---
# <a name="exp-transact-sql"></a>EXP (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Renvoie la valeur exponentielle de l’expression **float** spécifiée.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
EXP ( float_expression )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *float_expression*  
 [Expression](../../t-sql/language-elements/expressions-transact-sql.md) de type **float** ou dont le type peut être implicitement converti en type **float**.  
  
## <a name="return-types"></a>Types de retour  
 **float**  
  
## <a name="remarks"></a>Remarques  
 La constante **e** (2,718281…), est la base des logarithmes naturels.  
  
 L'exposant d'un nombre correspond à la constante **e** élevée à la puissance du nombre. Par exemple, EXP(1,0) = e^1,0 = 2,71828182845905 et EXP(10) = e^10 = 22026,4657948067.  
  
 La valeur exponentielle du logarithme népérien d’un nombre est le nombre lui-même : EXP (LOG (*n*)) = *n*. De même, le logarithme népérien de la valeur exponentielle d’un nombre est le nombre lui-même : LOG (EXP (*n*)) = *n*.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-finding-the-exponent-of-a-number"></a>R. Calcul de l'exposant d'un nombre  
 L'exemple suivant déclare une variable et renvoie la valeur exponentielle de cette dernière (`10`), accompagnées d'un texte descriptif.  
  
```sql  
DECLARE @var FLOAT  
SET @var = 10  
SELECT 'The EXP of the variable is: ' + CONVERT(VARCHAR, EXP(@var))  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
----------------------------------------------------------  
The EXP of the variable is: 22026.5  
(1 row(s) affected)  
```  
  
### <a name="b-finding-exponentials-and-natural-logarithms"></a>B. Calcul des valeurs exponentielles et logarithmes naturels  
 L'exemple suivant renvoie la valeur exponentielle du logarithme naturel de `20` et le logarithme naturel de la valeur exponentielle de `20`. Comme ces fonctions sont l'inverse l'une de l'autre, la valeur `20` est renvoyée dans les deux cas.  
  
```sql  
SELECT EXP(LOG(20)), LOG(EXP(20))  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
---------------------- ----------------------  
20                     20  
  
(1 row(s) affected)  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Exemples : [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="c-finding-the-exponent-of-a-number"></a>C. Calcul de l'exposant d'un nombre  
 L’exemple suivant renvoie la valeur exponentielle de la valeur spécifiée (`10`).  
  
```sql  
SELECT EXP(10);  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
----------  
22026.4657948067  
```  
  
### <a name="d-finding-exponential-values-and-natural-logarithms"></a>D. Calcul des valeurs exponentielles et des logarithmes népériens  
 L'exemple suivant renvoie la valeur exponentielle du logarithme naturel de `20` et le logarithme naturel de la valeur exponentielle de `20`. Comme ces fonctions sont l'inverse l'une de l'autre, la valeur `20` est renvoyée dans les deux cas.  
  
```sql  
SELECT EXP( LOG(20)), LOG( EXP(20));  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
-------------- -----------------  
20                  20  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions mathématiques &#40;Transact-SQL&#41;](../../t-sql/functions/mathematical-functions-transact-sql.md)   
 [LOG &#40;Transact-SQL&#41;](../../t-sql/functions/log-transact-sql.md)   
 [LOG10 &#40;Transact-SQL&#41;](../../t-sql/functions/log10-transact-sql.md)  
  
  

