---
description: DATENAME (Transact-SQL)
title: DATENAME (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/29/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DATENAME_TSQL
- DATENAME
dev_langs:
- TSQL
helpviewer_keywords:
- dates [SQL Server], functions
- DATENAME function [SQL Server
- functions [SQL Server], time
- functions [SQL Server], date and time
- dateparts [SQL Server], datename
- date and time [SQL Server], DATENAME
- comparing dates times [SQL Server]
- dates [SQL Server], dateparts
ms.assetid: 11855b56-c554-495d-aad4-ba446990153b
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: f9279883c9f032574e32056b2360a5947ffba022
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/25/2021
ms.locfileid: "98766205"
---
# <a name="datename-transact-sql"></a>DATENAME (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Cette fonction retourne une chaîne de caractères représentant la valeur *datepart* spécifiée de l’argument *date* spécifié.

Pour obtenir une vue d’ensemble de tous les types de données et fonctions de date et d’heure [!INCLUDE[tsql](../../includes/tsql-md.md)], consultez [Types de données et fonctions de date et d’heure &#40;Transact-SQL&#41;](../../t-sql/functions/date-and-time-data-types-and-functions-transact-sql.md).
  
![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
DATENAME ( datepart , date )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
*datepart*  
Partie spécifique de l’argument *date* que `DATENAME` retourne. Ce tableau répertorie tous les arguments *datepart* valides.

> [!NOTE]
> `DATENAME` n’accepte pas d’équivalents de variables définis par l’utilisateur pour les arguments *datepart*.
  
|*datepart*|Abréviations|  
|---|---|
|**year**|**yy, yyyy**|  
|**quarter**|**qq, q**|  
|**month**|**mm, m**|  
|**dayofyear**|**dy, y**|  
|**day**|**dd, d**|  
|**week**|**wk, ww**|  
|**weekday**|**dw, w**|  
|**hour**|**hh**|  
|**minute**|**mi, n**|  
|**second**|**ss, s**|  
|**millisecond**|**ms**|  
|**microsecond**|**mcs**|  
|**nanosecond**|**ns**|  
|**TZoffset**|**tz**|  
|**ISO_WEEK**|**ISOWK, ISOWW**|  
  
*date*  

Expression qui peut être résolue en l’un des types de données suivants : 

+ **date**
+ **datetime**
+ **datetimeoffset**
+ **datetime2** 
+ **smalldatetime**
+ **time**

Pour *date*, `DATENAME` accepte une expression de colonne, une expression, un littéral de chaîne ou une variable définie par l’utilisateur. Pour éviter toute ambiguïté, utilisez des années à quatre chiffres. Pour obtenir des informations sur les années à deux chiffres, consultez [Configurer l’option de configuration du serveur two digit year cutoff](../../database-engine/configure-windows/configure-the-two-digit-year-cutoff-server-configuration-option.md).
  
## <a name="return-type"></a>Type de retour  
**nvarchar**
  
## <a name="return-value"></a>Valeur de retour  
  
-   Chaque *datepart* et ses abréviations retournent la même valeur.  
  
La valeur retournée dépend de l’environnement de langue défini à l’aide de [SET LANGUAGE](../../t-sql/statements/set-language-transact-sql.md) et de l’[option de configuration de serveur Configurer la langue par défaut](../../database-engine/configure-windows/configure-the-default-language-server-configuration-option.md) de la connexion. La valeur retournée dépend de [SET DATEFORMAT](../../t-sql/statements/set-dateformat-transact-sql.md) si *date* est un littéral de chaîne de certains formats. SET DATEFORMAT ne change pas la valeur de retour quand la date est une expression de colonne d’un type de données de date ou d’heure.
  
Quand le paramètre *date* a un argument de type de données **date**, la valeur retournée dépend du paramètre spécifié par [SET DATEFIRST](../../t-sql/statements/set-datefirst-transact-sql.md).
  
## <a name="tzoffset-datepart-argument"></a>Argument de la partie de date TZoffset  
Si l’argument *datepart* est de type **TZoffset** (**tz**) et que l’argument *date* n’a aucun décalage de fuseau horaire, `DATEADD` retourne la valeur 0.
  
## <a name="smalldatetime-date-argument"></a>Argument date smalldatetime  
Quand *date* a la valeur [smalldatetime](../../t-sql/data-types/smalldatetime-transact-sql.md), `DATENAME` retourne les secondes sous la forme 00.
  
## <a name="default-returned-for-a-datepart-that-is-not-in-the-date-argument"></a>Valeur par défaut retournée pour une partie de date qui ne figure pas dans l'argument date  
Si le type de données de l’argument *date* n’a pas l’argument *datepart* spécifié, `DATENAME` retourne la valeur par défaut pour cet argument *datepart* uniquement si *date* a un littéral.
  
Par exemple, la combinaison année-mois-jour par défaut pour tout type de données **date** est 1900-01-01. L’instruction suivante a des arguments de partie de date pour *datepart*, un argument d’heure pour *date*, et `DATENAME` retourne `1900, January, 1, 1, Monday`.
  
```sql
SELECT DATENAME(year, '12:10:30.123')  
    ,DATENAME(month, '12:10:30.123')  
    ,DATENAME(day, '12:10:30.123')  
    ,DATENAME(dayofyear, '12:10:30.123')  
    ,DATENAME(weekday, '12:10:30.123');  
```  
  
Si l’argument *date* est spécifié comme variable ou colonne de table, et que cette variable ou colonne n’a pas l’argument *datepart* spécifié, `DATENAME` retourne l’erreur 9810. Dans l’exemple suivant, la variable *\@t* a le type de données **time**. L’exemple échoue car la partie année de la date n’est pas valide pour le type de données **time** :
  
```sql
DECLARE @t time = '12:10:30.123';   
SELECT DATENAME(year, @t);  
```  
  
## <a name="remarks"></a>Notes  

Utilisez `DATENAME` dans les clauses suivantes :

+ GROUP BY
+ HAVING
+ ORDER BY
+ SELECT \<list>
+ WHERE
  
Dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)], DATENAME convertit des littéraux de chaîne implicitement en type **datetime2**. En d’autres termes, `DATENAME` ne prend pas en charge le format YDM quand la date est passée sous forme de chaîne. Vous devez caster explicitement la chaîne en type **datetime** ou **smalldatetime** pour utiliser le format AJM.
  
## <a name="examples"></a>Exemples  
L’exemple suivant retourne les parties de la date spécifiée. Remplacez une valeur *datepart* de la table pour l’argument `datepart` dans l’instruction SELECT :
  
`SELECT DATENAME(datepart,'2007-10-30 12:15:32.1234567 +05:10');`
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
|*datepart*|Valeur retournée|  
|---|---|
|**year, yyyy, yy**|2007|  
|**quarter, qq, q**|4|  
|**month, mm, m**|Octobre|  
|**dayofyear, dy, y**|303|  
|**day, dd, d**|30|  
|**week, wk, ww**|44|  
|**weekday, dw**|Mardi|  
|**hour, hh**|12|  
|**minute, n**|15|  
|**second, ss, s**|32|  
|**millisecond, ms**|123|  
|**microsecond, mcs**|123456|  
|**nanosecond, ns**|123456700|  
|**TZoffset, tz**|+05:10|  
|**ISO_WEEK, ISOWK, ISOWW**|44|  
  
[!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]

L’exemple suivant retourne les parties de la date spécifiée. Remplacez une valeur *datepart* de la table pour l’argument `datepart` dans l’instruction SELECT :
  
```sql
SELECT DATENAME(datepart,'2007-10-30 12:15:32.1234567 +05:10');  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
|*datepart*|Valeur retournée|  
|---|---|
|**year, yyyy, yy**|2007|  
|**quarter, qq, q**|4|  
|**month, mm, m**|Octobre|  
|**dayofyear, dy, y**|303|  
|**day, dd, d**|30|  
|**week, wk, ww**|44|  
|**weekday, dw**|Mardi|  
|**hour, hh**|12|  
|**minute, n**|15|  
|**second, ss, s**|32|  
|**millisecond, ms**|123|  
|**microsecond, mcs**|123456|  
|**nanosecond, ns**|123456700|  
|**TZoffset, tz**|+05:10|  
|**ISO_WEEK, ISOWK, ISOWW**|44|  
  
## <a name="see-also"></a>Voir aussi
[CAST et CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)
  
  

