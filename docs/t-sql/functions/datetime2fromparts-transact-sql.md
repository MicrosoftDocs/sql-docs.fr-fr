---
description: DATETIME2FROMPARTS (Transact-SQL)
title: DATETIME2FROMPARTS (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/29/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DATETIME2FROMPARTS_TSQL
- DATETIME2FROMPARTS
dev_langs:
- TSQL
helpviewer_keywords:
- DATETIME2FROMPARTS function
ms.assetid: 632b757d-d2d1-43a5-b870-792a779ae204
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: eed7298a5f4ca77192baba346232cd42bb433ab6
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101233"
---
# <a name="datetime2fromparts-transact-sql"></a>DATETIME2FROMPARTS (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Cette fonction retourne une valeur **datetime2** pour les arguments de date et d’heure spécifiés. La valeur retournée a une précision spécifiée par l’argument de précision.
  
![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
DATETIME2FROMPARTS ( year, month, day, hour, minute, seconds, fractions, precision )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
*year*  
Expression entière qui spécifie une année.
  
*month*  
Expression entière qui spécifie un mois.
  
*day*  
Expression entière qui spécifie un jour.
  
*hour*  
Expression entière qui spécifie les heures.
  
*minute*  
Expression entière qui spécifie les minutes.
  
*secondes*  
Expression entière qui spécifie les secondes.
  
*fractions*  
Expression entière qui spécifie une valeur de fraction de secondes.
  
*precision*  
Expression entière qui spécifie la précision de la valeur **datetime2** que `DATETIME2FROMPARTS` retourne.
  
## <a name="return-types"></a>Types de retour
**datetime2(** *precision* **)**
  
## <a name="remarks"></a>Remarques  
`DATETIME2FROMPARTS` retourne une valeur **datetime2** entièrement initialisée. `DATETIME2FROMPARTS` génère une erreur si au moins un argument obligatoire a une valeur non valide. `DATETIME2FROMPARTS` retourne une valeur Null si au moins un argument obligatoire a une valeur Null. Toutefois, si l’argument *precision* a une valeur Null, `DATETIME2FROMPARTS` génère une erreur.

L’argument *fractions* dépend de l’argument *precision*. Par exemple, pour la valeur *precision* 7, chaque fraction représente 100 nanosecondes ; pour la valeur *precision* 3, chaque fraction représente une milliseconde. Pour la valeur *precision* zéro, la valeur de *fractions* doit également être zéro ; sinon, `DATETIME2FROMPARTS` génère une erreur.
  
Cette fonction prend en charge la communication à distance vers des serveurs [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] et versions ultérieures. Elle ne prend pas en charge la communication à distance vers des serveurs dont la version est antérieure à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].
  
## <a name="examples"></a>Exemples  
  
### <a name="a-an-example-without-fractions-of-a-second"></a>R. Exemple sans fractions de seconde  
  
```sql
SELECT DATETIME2FROMPARTS ( 2010, 12, 31, 23, 59, 59, 0, 0 ) AS Result;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
Result  
---------------------------  
2010-12-31 23:59:59.0000000  
  
(1 row(s) affected)  
```  
  
### <a name="b-example-with-fractions-of-a-second"></a>B. Exemple avec fractions de seconde  
L’exemple suivant illustre l’utilisation des paramètres *fractions* et *precision* :
  
1.  Quand *fractions* a la valeur 5 et *precision* la valeur 1, la valeur de *fractions* représente 5/10 de seconde.  
  
2.  Quand *fractions* a la valeur 50 et *precision* la valeur 2, la valeur de *fractions* représente 50/100 de seconde.  
  
3.  Quand *fractions* a la valeur 500 et *precision* la valeur 3, la valeur de *fractions* représente 500/1000 de seconde.  
  
```sql
SELECT DATETIME2FROMPARTS ( 2011, 8, 15, 14, 23, 44, 5, 1 );  
SELECT DATETIME2FROMPARTS ( 2011, 8, 15, 14, 23, 44, 50, 2 );  
SELECT DATETIME2FROMPARTS ( 2011, 8, 15, 14, 23, 44, 500, 3 );  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
----------------------  
2011-08-15 14:23:44.5  
  
(1 row(s) affected)  
  
----------------------  
2011-08-15 14:23:44.50  
  
(1 row(s) affected)  
  
----------------------  
2011-08-15 14:23:44.500  
  
(1 row(s) affected)  
```  
  
## <a name="see-also"></a>Voir aussi
[datetime2 &#40;Transact-SQL&#41;](../../t-sql/data-types/datetime2-transact-sql.md)
  
  

