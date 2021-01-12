---
title: DATEPART (Transact-SQL) | Microsoft Docs
description: Référence Transact-SQL pour la fonction DATEPART. Cette fonction retourne un entier correspondant au datepart d’une date spécifiée.
ms.date: 07/29/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DATEPART_TSQL
- DATEPART
dev_langs:
- TSQL
helpviewer_keywords:
- dates [SQL Server], functions
- date and time [SQL Server], DATEPART
- dateparts [SQL Server]
- functions [SQL Server], time
- functions [SQL Server], date and time
- time [SQL Server], functions
- dateparts [SQL Server], datepart
- comparing dates times [SQL Server]
- DATEPART function [SQL Server]
- dates [SQL Server], dateparts
ms.assetid: 15f1a5bc-4c0c-4c48-848d-8ec03473e6c1
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: adae6be66b149f930bee51c636bf86d7614e3bf2
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093663"
---
# <a name="datepart-transact-sql"></a>DATEPART (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]


Cette fonction retourne un entier représentant la valeur *datepart* spécifiée de la *date* spécifiée.
  
Pour obtenir une vue d’ensemble de tous les types de données et fonctions de date et d’heure [!INCLUDE[tsql](../../includes/tsql-md.md)], consultez [Types de données et fonctions de date et d’heure &#40;Transact-SQL&#41;](../../t-sql/functions/date-and-time-data-types-and-functions-transact-sql.md).
  
![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
DATEPART ( datepart , date )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
*datepart*  
Partie spécifique de l’argument *date* pour lequel `DATEPART` retourne une valeur de type **integer**. Ce tableau répertorie tous les arguments *datepart* valides.

> [!NOTE]
> `DATEPART` n’accepte pas d’équivalents de variables définis par l’utilisateur pour les arguments *datepart*.
  
|*datepart*|Abréviations|  
|---|---|
|**year**|**yy**, **yyyy**|  
|**quarter**|**qq**, **q**|  
|**month**|**mm**, **m**|  
|**dayofyear**|**dy**, **y**|  
|**day**|**dd**, **d**|  
|**week**|**wk**, **ww**|  
|**weekday**|**dw**|  
|**hour**|**hh**|  
|**minute**|**mi, n**|  
|**second**|**ss**, **s**|  
|**millisecond**|**ms**|  
|**microsecond**|**mcs**|  
|**nanosecond**|**ns**|  
|**tzoffset**|**tz**|  
|**iso_week**|**isowk**, **isoww**|  
  
*date*  
Expression qui est résolue en l’un des types de données suivants : 

+ **date**
+ **datetime**
+ **datetimeoffset**
+ **datetime2** 
+ **smalldatetime**
+ **time**

Pour *date*, `DATEPART` accepte une expression de colonne, une expression, un littéral de chaîne ou une variable définie par l’utilisateur. Pour éviter toute ambiguïté, utilisez des années à quatre chiffres. Pour obtenir des informations sur les années à deux chiffres, consultez [Configurer l’option de configuration du serveur two digit year cutoff](../../database-engine/configure-windows/configure-the-two-digit-year-cutoff-server-configuration-option.md).
  
## <a name="return-type"></a>Type de retour  
 **int**  
  
## <a name="return-value"></a>Valeur de retour  
Chaque *datepart* et ses abréviations retournent la même valeur.
  
La valeur retournée dépend de l’environnement de langue défini à l’aide de [SET LANGUAGE](../../t-sql/statements/set-language-transact-sql.md) et de l’[option de configuration de serveur Configurer la langue par défaut](../../database-engine/configure-windows/configure-the-default-language-server-configuration-option.md) de la connexion. La valeur retournée dépend de [SET DATEFORMAT](../../t-sql/statements/set-dateformat-transact-sql.md) si *date* est un littéral de chaîne de certains formats. SET DATEFORMAT ne change pas la valeur de retour quand la date est une expression de colonne d’un type de données de date ou d’heure.
  
Le tableau suivant répertorie tous les arguments *datepart*, avec les valeurs retournées correspondantes, pour l’instruction `SELECT DATEPART(datepart,'2007-10-30 12:15:32.1234567 +05:10')`. L’argument *date* a le type de données **datetimeoffset(7)** . Les deux dernières positions de la valeur de retour **nanosecond** *datepart* sont toujours `00` et cette valeur a une échelle de 9 :

**.123456700**
  
|*datepart*|Valeur retournée|  
|---|---|
|**year, yyyy, yy**|2007|  
|**quarter, qq, q**|4|  
|**month, mm, m**|10|  
|**dayofyear, dy, y**|303|  
|**day, dd, d**|30|  
|**week, wk, ww**|44|  
|**weekday, dw**|3|  
|**hour, hh**|12|  
|**minute, n**|15|  
|**second, ss, s**|32|  
|**millisecond, ms**|123|  
|**microsecond, mcs**|123456|  
|**nanosecond, ns**|123456700|  
|**tzoffset, tz**|310|  
|**iso_week, isowk, isoww**|44|  
  
## <a name="week-and-weekday-datepart-arguments"></a>Arguments des parties de date semaine et jour ouvrable
Pour un argument *datepart* ayant la valeur **week** (**wk**, **ww**) ou **weekday** (**dw**), la valeur de retour `DATEPART` dépend de la valeur définie par [SET DATEFIRST](../../t-sql/statements/set-datefirst-transact-sql.md).
  
Le 1er janvier d’une année définit le numéro de départ de **week** _datepart_. Par exemple :

DATEPART (**wk**, 'Jan 1, *xxx* x') = 1

où *xxxx* représente une année.
  
Ce tableau montre la valeur de retour pour **week** et **weekday** *datepart* pour la valeur '2007-04-21' de chaque argument SET DATEFIRST. Le 1er janvier 2007 tombe un lundi. Le 21 avril 2007 tombe un samedi. Pour l’anglais des États-Unis,

`SET DATEFIRST 7 -- ( Sunday )`

est utilisée comme valeur par défaut. Après avoir défini DATEFIRST, utilisez l’instruction SQL suggérée suivante pour les valeurs de table datepart :

`SELECT DATEPART(week, '2007-04-21 '), DATEPART(weekday, '2007-04-21 ')`
  
|SET DATEFIRST<br /><br /> argument|week<br /><br /> retourné|weekday<br /><br /> retourné|  
|---|---|---|
|1|16|6|  
|2|17|5|  
|3|17|4|  
|4|17|3|  
|5|17|2|  
|6|17|1|  
|7|16|7|  
  
## <a name="year-month-and-day-datepart-arguments"></a>Arguments des parties de date année, mois et jour  
Les valeurs retournées pour DATEPART (**year**, *date*), DATEPART (**month**, *date*) et DATEPART (**day**, *date*) sont les mêmes que celles retournées par les fonctions [YEAR](../../t-sql/functions/year-transact-sql.md), [MONTH](../../t-sql/functions/month-transact-sql.md) et [DAY](../../t-sql/functions/day-transact-sql.md), respectivement.
  
## <a name="iso_week-datepart"></a>Partie de date iso_week  
ISO 8601 inclut le système semaine-date ISO, un système de numérotation pour les semaines. Chaque semaine est associée à l'année dans laquelle jeudi se produit. Par exemple, la semaine 1 de 2004 (2004W01) couvrait la période du lundi 29 décembre 2003 au dimanche 4 janvier 2004. En général, les pays européens utilisent ce style de numérotation, ce qui n’est normalement pas le cas des autres pays.

Remarque : Le numéro de semaine le plus élevé dans une année peut être 52 ou 53.
  
Les systèmes de numérotation des différents pays peuvent ne pas se conformer à la norme ISO. Le tableau suivant présente six possibilités :
  
|Premier jour de la semaine|La première semaine de l'année contient|Semaines assignées deux fois|Utilisé par/dans|  
|---|---|---|---|
|Dimanche|1er janvier,<br /><br /> Premier samedi,<br /><br /> 1 à 7 jours de l’année|Oui|États-Unis|  
|Lundi|1er janvier,<br /><br /> Premier dimanche,<br /><br /> 1 à 7 jours de l’année|Oui|Majorité de l'Europe et Royaume-Uni|  
|Lundi|4 janvier<br /><br /> Premier jeudi,<br /><br /> 4 à 7 jours de l’année|Non|ISO 8601, Norvège et Suède|  
|Lundi|7 janvier,<br /><br /> Premier lundi,<br /><br /> 7 jours de l'année|Non||  
|Mercredi|1er janvier,<br /><br /> Premier mardi,<br /><br /> 1 à 7 jours de l’année|Oui||  
|Samedi|1er janvier,<br /><br /> Premier vendredi,<br /><br /> 1 à 7 jours de l’année|Oui||  
  
## <a name="tzoffset"></a>tzoffset  
`DATEPART` retourne la valeur **tzoffset** (**tz**) sous la forme du nombre de minutes (signé). L’instruction suivante retourne un décalage de fuseau horaire de 310 minutes :
  
```sql
SELECT DATEPART (tzoffset, '2007-05-10  00:00:01.1234567 +05:10');  
```  
`DATEPART` restitue la valeur tzoffset comme suit :
- Pour datetimeoffset et datetime2, tzoffset retourne le décalage horaire en minutes, où le décalage pour datetime2 s’élève toujours à 0 minute.
- Pour les types de données qui peuvent implicitement être convertis en **datetimeoffset** ou **datetime2**, `DATEPART` retourne le décalage horaire en minutes. Exception : autre types de données de date/heure.
- Les paramètres de tous les autres types génèrent une erreur.
  
  
## <a name="smalldatetime-date-argument"></a>Argument date smalldatetime  
Pour une valeur *date* [smalldatetime](../../t-sql/data-types/smalldatetime-transact-sql.md), `DATEPART` retourne les secondes sous la forme 00.
  
## <a name="default-returned-for-a-datepart-that-is-not-in-a-date-argument"></a>Valeur par défaut retournée pour une partie de date qui ne figure pas dans un argument date  
Si le type de données de l’argument *date* n’a pas l’argument *datepart* spécifié, `DATEPART` retourne la valeur par défaut pour ce *datepart* uniquement lors de la spécification d’un littéral pour *date*.
  
Par exemple, la combinaison année-mois-jour par défaut pour tout type de données **date** est 1900-01-01. L’instruction suivante a des arguments de partie de date pour *datepart*, un argument d’heure pour *date*, et elle retourne `1900, 1, 1, 1, 2`.
  
```sql
SELECT DATEPART(year, '12:10:30.123')  
    ,DATEPART(month, '12:10:30.123')  
    ,DATEPART(day, '12:10:30.123')  
    ,DATEPART(dayofyear, '12:10:30.123')  
    ,DATEPART(weekday, '12:10:30.123');  
```  
  
Si l’argument *date* est spécifié comme variable ou colonne de table, et que cette variable ou colonne n’a pas l’argument *datepart* spécifié, `DATEPART` retourne l’erreur 9810. Dans l’exemple suivant, la variable *\@t* a le type de données **time**. L’exemple échoue car la partie année de la date n’est pas valide pour le type de données **time** :
  
```sql
DECLARE @t time = '12:10:30.123';   
SELECT DATEPART(year, @t);  
```  
  
## <a name="fractional-seconds"></a>Fractions de seconde
Les instructions suivantes montrent que `DATEPART` retourne des fractions de seconde :
  
```sql
SELECT DATEPART(millisecond, '00:00:01.1234567'); -- Returns 123  
SELECT DATEPART(microsecond, '00:00:01.1234567'); -- Returns 123456  
SELECT DATEPART(nanosecond,  '00:00:01.1234567'); -- Returns 123456700  
```  
  
## <a name="remarks"></a>Notes  
`DATEPART` peut être utilisé dans la liste de sélection et les clauses WHERE, HAVING, GROUP BY et ORDER BY.
  
DATEPART caste implicitement des littéraux de chaîne en type **datetime2** dans [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]. Cela signifie que DATENAME ne prend pas en charge le format YDM lorsque la date est transmise en tant que chaîne. Vous devez caster explicitement la chaîne en type **datetime** ou **smalldatetime** pour utiliser le format AJM.
  
## <a name="examples"></a>Exemples  
L’exemple suivant retourne l’année de base. L’année de base permet d’effectuer des calculs de date. Dans l’exemple, un nombre spécifie la date. Notez que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] interprète 0 comme représentant le 1er janvier 1900.
  
```sql
SELECT DATEPART(year, 0), DATEPART(month, 0), DATEPART(day, 0);  

-- Returns: 1900    1    1 
```  
  
L’exemple suivant retourne la partie jour de la date `12/20/1974`.
  
```sql
-- Uses AdventureWorks  
  
SELECT TOP(1) DATEPART (day,'12/20/1974') FROM dbo.DimCustomer;  

-- Returns: 20
```  
  
L’exemple suivant retourne la partie année de la date `12/20/1974`.
  
```sql
-- Uses AdventureWorks  
  
SELECT TOP(1) DATEPART (year,'12/20/1974') FROM dbo.DimCustomer;  

-- Returns: 1974
```  
  
## <a name="see-also"></a>Voir aussi
[CAST et CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)
  
  
