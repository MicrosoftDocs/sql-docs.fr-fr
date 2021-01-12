---
description: DATEDIFF_BIG (Transact-SQL)
title: DATEDIFF_BIG (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/18/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DATEDIFF_BIG
- DATEDIFF_BIG_TSQL
helpviewer_keywords:
- DATEDIFF_BIG function [SQL Server]
- dates [SQL Server]. functions
- date and time [SQL Server], DATEDIFF_BIG
- functions [SQL Server], time
- functions [SQL Server], date and time
- time [SQL Server], functions
ms.assetid: 19ac1693-3cfa-400d-bf83-20a9cb46599a
author: cawrites
ms.author: chadam
ms.openlocfilehash: ea087da6532c43493fd10f647788297d98f35f72
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093652"
---
# <a name="datediff_big-transact-sql"></a>DATEDIFF_BIG (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

Cette fonction retourne le nombre (valeur entière très grande signée) de limites *datepart* spécifiées, traversées entre les valeurs *startdate* et *enddate* spécifiées.
  
Pour obtenir une vue d’ensemble de tous les types de données et fonctions de date et d’heure [!INCLUDE[tsql](../../includes/tsql-md.md)], consultez [Types de données et fonctions de date et d’heure &#40;Transact-SQL&#41;](../../t-sql/functions/date-and-time-data-types-and-functions-transact-sql.md).
  
![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
DATEDIFF_BIG ( datepart , startdate , enddate )  
```  

## <a name="arguments"></a>Arguments
*datepart*  
Partie de *startdate* et *enddate* qui spécifie le type de limite traversée.

> [!NOTE]
> `DATEDIFF_BIG` n’accepte pas les valeurs *datepart* de variables définies par l’utilisateur ou comme chaînes entre guillemets.

Ce tableau liste l’ensemble des noms et des abréviations *datepart* valides.
  
|Nom *datepart*| Abréviation *datepart*|  
|---|---|
|**year**|**yy, yyyy**|  
|**quarter**|**qq, q**|  
|**month**|**mm, m**|  
|**dayofyear**|**dy, y**|  
|**day**|**dd, d**|  
|**week**|**wk, ww**|  
|**hour**|**hh**|  
|**minute**|**mi, n**|  
|**second**|**ss, s**|  
|**millisecond**|**ms**|  
|**microsecond**|**mcs**|  
|**nanosecond**|**ns**|  

> [!NOTE]
> Chaque nom *datepart* spécifique et les abréviations pour ce nom *datepart* retournent la même valeur.

*startdate*  
Expression qui peut être résolue en valeur, parmi les suivantes :

+ **date**
+ **datetime**
+ **datetimeoffset**
+ **datetime2** 
+ **smalldatetime**
+ **time**

Pour *date*, `DATEDIFF_BIG` accepte une expression de colonne, une expression, un littéral de chaîne ou une variable définie par l’utilisateur. Une valeur de littéral de chaîne doit être résolue en **datetime**. Pour éviter toute ambiguïté, utilisez des années à quatre chiffres. `DATEDIFF_BIG` soustrait *startdate* de *enddate*. Pour éviter toute ambiguïté, représentez les années à l'aide de quatre chiffres. Pour obtenir des informations sur les années à deux chiffres, consultez [Configurer l’option de configuration du serveur two digit year cutoff](../../database-engine/configure-windows/configure-the-two-digit-year-cutoff-server-configuration-option.md).
  
*enddate*  
Consultez *startdate*.
  
## <a name="return-type"></a>Type de retour  
**bigint** signé  
  
## <a name="return-value"></a>Valeur de retour  
Retourne la différence **bigint** entre *startdate* et *enddate*, exprimée dans le jeu de limites par *datepart*.
  
Si une valeur de retour est hors limites pour **bigint** (-9 223 372 036 854 775 808 à +9 223 372 036 854 775 807), `DATEDIFF_BIG` retourne une erreur. Contrairement à `DATEDIFF`, qui retourne un **int** et qui par conséquent, peut dépasser la capacité avec une précision d’une **minute** ou plus, `DATEDIFF_BIG` peut dépasser la capacité seulement si vous utilisez une précision d’une **nanoseconde**, où la différence entre *enddate* et *startdate* est supérieure à 292 années, 3 mois, 10 jours, 23 heures, 47 minutes et 16,8547758 secondes.
  
Si *startdate* et *enddate* se voient tous les deux assigner uniquement une valeur d’heure et que *datepart* n’est pas un *datepart* d’heure, `DATEDIFF_BIG` retourne 0.
  
`DATEDIFF_BIG` n’utilise pas un composant de décalage de fuseau horaire de *startdate* ou *enddate* pour calculer la valeur de retour.
  
Quand vous utilisez une valeur **smalldatetime** pour *startdate* ou *enddate*, `DATEDIFF_BIG` définit toujours les secondes et millisecondes avec la valeur 0 dans la valeur de retour dans la mesure où [smalldatetime](../../t-sql/data-types/smalldatetime-transact-sql.md) n’offre qu’une précision à la minute.
  
Si seule une valeur d’heure est affectée à une variable d’un type de données date, `DATEDIFF_BIG` définit la partie date manquante sur la valeur par défaut :`1900-01-01`. Si seule une valeur de date est affectée à une variable d’un type de données date ou heure, `DATEDIFF_BIG` définit la partie heure manquante sur la valeur par défaut : `00:00:00`. Si *startdate* ou *enddate* a uniquement une partie heure et que l’autre a uniquement une partie date, `DATEDIFF_BIG` affecte aux parties heure et date manquantes les valeurs par défaut.
  
Si *startdate* et *enddate* ont des types de données date différents et que l’un a plus de parties heure ou une meilleure précision en fractions de seconde que l’autre, `DATEDIFF_BIG` affecte aux parties manquantes de l’autre la valeur 0.
  
## <a name="datepart-boundaries"></a>Limites de datepart
Les instructions suivantes ont les mêmes valeurs *startdate* et *enddate*. Ces dates sont adjacentes et ont une différence d’une microseconde (0,0000001 seconde). La différence entre les *startdate* et *endate* dans chaque instruction traverse une limite d’heure ou de calendrier de son *datepart*. Chaque instruction retourne 1. Si *startdate* et *enddate* ont des valeurs d’année différentes, mais les mêmes valeurs de semaine de calendrier, `DATEDIFF_BIG` retourne 0 pour *datepart* **week**.

```sql
SELECT DATEDIFF_BIG(year,        '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
SELECT DATEDIFF_BIG(quarter,     '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
SELECT DATEDIFF_BIG(month,       '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
SELECT DATEDIFF_BIG(dayofyear,   '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
SELECT DATEDIFF_BIG(day,         '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
SELECT DATEDIFF_BIG(week,        '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
SELECT DATEDIFF_BIG(hour,        '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
SELECT DATEDIFF_BIG(minute,      '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
SELECT DATEDIFF_BIG(second,      '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
SELECT DATEDIFF_BIG(millisecond, '2005-12-31 23:59:59.9999999', '2006-01-01 00:00:00.0000000');
```
  
## <a name="remarks"></a>Notes  
Utilisez `DATEDIFF_BIG` dans les clauses `SELECT <list>`, `WHERE`, `HAVING`, `GROUP BY` et `ORDER BY`.
  
`DATEDIFF_BIG` caste implicitement les littéraux de chaîne en type **datetime2**. Cela signifie que `DATEDIFF_BIG` ne prend pas en charge le format YDM quand la date est passée comme chaîne. Vous devez caster explicitement la chaîne en type **datetime** ou **smalldatetime** pour utiliser le format AJM.
  
La spécification de `SET DATEFIRST` n’a pas d’effet sur `DATEDIFF_BIG`. `DATEDIFF_BIG` utilise toujours Dimanche comme premier jour de la semaine pour que la fonction soit déterministe.

`DATEDIFF_BIG` peut dépasser la capacité avec une précision d’une **nanoseconde** ou plus si la différence entre *enddate* et *startdate* retourne une valeur qui est hors limites pour **bigint**.
  
## <a name="examples"></a>Exemples 
  
### <a name="specifying-columns-for-startdate-and-enddate"></a>Spécification de colonnes pour les dates de début et de fin  
Cet exemple utilise différents types d’expressions comme arguments pour les paramètres *startdate* et *enddate*. Il calcule le nombre de limites de jour qui sont traversées entre les dates de deux colonnes d’une table.
  
```sql
CREATE TABLE dbo.Duration  
    (startDate datetime2, endDate datetime2);  
    
INSERT INTO dbo.Duration(startDate,endDate)  
    VALUES('2007-05-06 12:10:09', '2007-05-07 12:10:09');  
    
SELECT DATEDIFF_BIG(day, startDate, endDate) AS 'Duration'  
    FROM dbo.Duration;  
-- Returns: 1  
```  

### <a name="finding-difference-between-startdate-and-enddate-as-date-parts-strings"></a>Recherche de la différence entre des dates startdate et enddate sous forme de chaînes de parties de date

```sql
DECLARE @date1 DATETIME2, @date2 DATETIME2, @result VARCHAR(100)
DECLARE @years BIGINT, @months BIGINT, @days BIGINT, @hours BIGINT, @minutes BIGINT, @seconds BIGINT, @milliseconds BIGINT

SET @date1 = '0001-01-01 00:00:00.00000000'
SET @date2 = '2018-12-12 07:08:01.12345678'

SELECT @years = DATEDIFF(yy, @date1, @date2)
IF DATEADD(yy, -@years, @date2) < @date1 
SELECT @years = @years-1
SET @date2 = DATEADD(yy, -@years, @date2)

SELECT @months = DATEDIFF(mm, @date1, @date2)
IF DATEADD(mm, -@months, @date2) < @date1 
SELECT @months=@months-1
SET @date2= DATEADD(mm, -@months, @date2)

SELECT @days=DATEDIFF(dd, @date1, @date2)
IF DATEADD(dd, -@days, @date2) < @date1 
SELECT @days=@days-1
SET @date2= DATEADD(dd, -@days, @date2)

SELECT @hours=DATEDIFF(hh, @date1, @date2)
IF DATEADD(hh, -@hours, @date2) < @date1 
SELECT @hours=@hours-1
SET @date2= DATEADD(hh, -@hours, @date2)

SELECT @minutes=DATEDIFF(mi, @date1, @date2)
IF DATEADD(mi, -@minutes, @date2) < @date1 
SELECT @minutes=@minutes-1
SET @date2= DATEADD(mi, -@minutes, @date2)

SELECT @seconds=DATEDIFF(s, @date1, @date2)
IF DATEADD(s, -@seconds, @date2) < @date1 
SELECT @seconds=@seconds-1
SET @date2= DATEADD(s, -@seconds, @date2)

SELECT @milliseconds=DATEDIFF(ms, @date1, @date2)

SELECT @result= ISNULL(CAST(NULLIF(@years,0) AS VARCHAR(10)) + ' years,','')
     + ISNULL(' ' + CAST(NULLIF(@months,0) AS VARCHAR(10)) + ' months,','')    
     + ISNULL(' ' + CAST(NULLIF(@days,0) AS VARCHAR(10)) + ' days,','')
     + ISNULL(' ' + CAST(NULLIF(@hours,0) AS VARCHAR(10)) + ' hours,','')
     + ISNULL(' ' + CAST(@minutes AS VARCHAR(10)) + ' minutes and','')
     + ISNULL(' ' + CAST(@seconds AS VARCHAR(10)) 
          + CASE WHEN @milliseconds > 0 THEN '.' + CAST(@milliseconds AS VARCHAR(10)) 
               ELSE '' END 
          + ' seconds','')

SELECT @result
```

[!INCLUDE[ssResult](../../includes/ssresult-md.md)] 

```
2017 years, 11 months, 11 days, 7 hours, 8 minutes and 1.123 seconds
```   

Pour obtenir des exemples plus étroitement liés, consultez [DATEDIFF &#40;Transact-SQL&#41;](../../t-sql/functions/datediff-transact-sql.md).
  
## <a name="see-also"></a>Voir aussi
[CAST et CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)  
[DATEDIFF &#40;Transact-SQL&#41;](../../t-sql/functions/datediff-transact-sql.md)
  
  
