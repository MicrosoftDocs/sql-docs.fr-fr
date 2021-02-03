---
description: DATETIMEOFFSETFROMPARTS (Transact-SQL)
title: DATETIMEOFFSETFROMPARTS (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/29/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DATETIMEOFFSETFROMPARTS_TSQL
- DATETIMEOFFSETFROMPARTS
dev_langs:
- TSQL
helpviewer_keywords:
- DATETIMEOFFSETFROMPARTS function
ms.assetid: 463da1f4-b4b6-45a3-9a95-ea1f99575542
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 9f8ce697a414d095e91214626b4ae4f8e1e2a83b
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237922"
---
# <a name="datetimeoffsetfromparts-transact-sql"></a>DATETIMEOFFSETFROMPARTS (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Retourne une valeur **datetimeoffset** pour les arguments de date et d’heure indiqués. Cette valeur a une précision spécifiée par l’argument precision et un décalage déterminé par les arguments offset.  
  
![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
DATETIMEOFFSETFROMPARTS ( year, month, day, hour, minute, seconds, fractions, hour_offset, minute_offset, precision )  
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
  
*hour_offset*  
Expression entière qui spécifie la partie heure du décalage de fuseau horaire.  
  
*minute_offset*  
Expression entière qui spécifie la partie minute du décalage de fuseau horaire.  
  
*precision*  
Valeur littérale entière qui spécifie la précision de la valeur **datetimeoffset** retournée par `DATETIMEOFFSETFROMPARTS`.  
  
## <a name="return-types"></a>Types de retour
**datetimeoffset(** *precision* **)**  
  
## <a name="remarks"></a>Remarques  

`DATETIMEOFFSETFROMPARTS` retourne un type de données **datetimeoffset** entièrement initialisé. Les arguments offset représentent le décalage de fuseau horaire. S’ils sont omis, `DATETIMEOFFSETFROMPARTS` suppose un décalage de fuseau horaire de `00:00`, soit aucun décalage de fuseau horaire. S’ils sont spécifiés, `DATETIMEOFFSETFROMPARTS` attend des valeurs, toutes deux positives ou négatives, pour les deux arguments. Si *minute_offset* a une valeur et *hour_offset* non, `DATETIMEOFFSETFROMPARTS` génère une erreur. `DATETIMEOFFSETFROMPARTS` génère également une erreur si les autres arguments ont des valeurs non valides. Si au moins un des arguments requis a une valeur `NULL`, `DATETIMEOFFSETFROMPARTS` retourne `NULL`. En revanche, si l’argument *precision* a une valeur `NULL`, `DATETIMEOFFSETFROMPARTS` génère une erreur.  
  
L’argument *fractions* dépend de l’argument precision. Par exemple, pour une valeur precision égale à 7, chaque fraction représente 100 nanosecondes ; si elle est de 3, chaque fraction représente une milliseconde. Lorsque precision a la valeur zéro, la valeur de fractions doit également être zéro ; sinon, `DATETIMEOFFSETFROMPARTS` génère une erreur.  
  
Cette fonction peut être exécutée à distance sur des serveurs [!INCLUDE[sssql11-md](../../includes/sssql11-md.md)] et versions ultérieures. Elle ne peut pas être exécutée à distance sur des serveurs dont la version est antérieure à [!INCLUDE[sssql11-md](../../includes/sssql11-md.md)].  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-an-example-without-fractions-of-a-second"></a>R. Exemple sans fractions de seconde  
  
```sql
SELECT DATETIMEOFFSETFROMPARTS ( 2010, 12, 31, 14, 23, 23, 0, 12, 0, 7 ) AS Result;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
Result  
----------------------------------
2010-12-31 14:23:23.0000000 +12:00  
  
(1 row(s) affected)  
```  
  
### <a name="b-example-with-fractions-of-a-second"></a>B. Exemple avec fractions de seconde  

L’exemple suivant illustre l’utilisation des paramètres *fractions* et *precision* :  

1. Quand *fractions* a la valeur 5 et *precision* la valeur 1, la valeur de *fractions* représente 5/10 de seconde.  

2. Quand *fractions* a la valeur 50 et *precision* la valeur 2, la valeur de *fractions* représente 50/100 de seconde.  

3. Quand *fractions* a la valeur 500 et *precision* la valeur 3, la valeur de *fractions* représente 500/1000 de seconde.  
  
```sql
SELECT DATETIMEOFFSETFROMPARTS ( 2011, 8, 15, 14, 30, 00, 5, 12, 30, 1 );  
SELECT DATETIMEOFFSETFROMPARTS ( 2011, 8, 15, 14, 30, 00, 50, 12, 30, 2 );  
SELECT DATETIMEOFFSETFROMPARTS ( 2011, 8, 15, 14, 30, 00, 500, 12, 30, 3 );  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
----------------------------------  
2011-08-15 14:30:00.5 +12:30  
  
(1 row(s) affected)  
  
----------------------------------  
2011-08-15 14:30:00.50 +12:30  
  
(1 row(s) affected)  
  
----------------------------------  
2011-08-15 14:30:00.500 +12:30  
  
(1 row(s) affected)  
```  
  
## <a name="see-also"></a>Voir aussi
[datetimeoffset &#40;Transact-SQL&#41;](../../t-sql/data-types/datetimeoffset-transact-sql.md)  
[AT TIME ZONE &#40;Transact-SQL&#41;](../../t-sql/queries/at-time-zone-transact-sql.md)
  
  


