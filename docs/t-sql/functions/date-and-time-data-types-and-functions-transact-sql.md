---
title: Types de données et fonctions de date et d’heure
description: Liens vers des articles sur les types de données et les fonctions de date et d’heure.
titleSuffix: SQL Server (Transact-SQL)
ms.custom: seo-lt-2019
ms.date: 09/01/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- dates [SQL Server], functions
- dates [SQL Server]
- date and time [SQL Server], all data types and functions
- date and time [SQL Server]
- functions [SQL Server], time
- functions [SQL Server], date and time
- time [SQL Server], functions
ms.assetid: 83e378a2-6e89-4c80-bc4f-644958d9e0a9
author: cawrites
ms.author: chadam
monikerRange: = azure-sqldw-latest||= azuresqldb-current || >= sql-server-2016 || >= sql-server-linux-2017
ms.openlocfilehash: 461b4db55574fcad2fb5ff7250ce88f508999fa6
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98102613"
---
# <a name="date-and-time-data-types-and-functions-transact-sql"></a>Types de données et fonctions de date et d'heure (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

Ces sections dans cette rubrique couvrent tous les types de données et fonctions de date et d’heure [!INCLUDE[tsql](../../includes/tsql-md.md)].

- [Types de données de date et d’heure](#DateandTimeDataTypes)
- [Fonctions de date et d’heure](#DateandTimeFunctions)
  - [Fonctions permettant de retourner des valeurs système de date et d’heure](#GetSystemDateandTimeValues)
  - [Fonctions permettant de retourner des parties de date et d’heure](#GetDateandTimeParts)
  - [Fonctions permettant de retourner des valeurs de date et d’heure à partir de leurs parties](#fromParts)
  - [Fonctions permettant de retourner des valeurs de différence de date et d’heure](#GetDateandTimeDifference)
  - [Fonctions permettant de modifier les valeurs de date et d’heure](#ModifyDateandTimeValues)
  - [Fonctions permettant de définir ou de retourner des fonctions de format de session](#SetorGetSessionFormatFunctions)
  - [Fonctions permettant de valider les valeurs de date et d’heure](#ValidateDateandTimeValues)
- [Rubriques relatives à la date et à l’heure](#DateandTimeRelatedTopics)

## <a name="date-and-time-data-types"></a><a name="DateandTimeDataTypes"></a> Types de données de date et d’heure

Les types de données de date et d’heure [!INCLUDE[tsql](../../includes/tsql-md.md)] sont répertoriés dans le tableau suivant :

|Type de données|Format|Plage|Précision|Taille de stockage (en octets)|Précision à la fraction de seconde définie par l'utilisateur|Décalage de fuseau horaire|
|---|---|---|---|---|---|---|
|[time](../../t-sql/data-types/time-transact-sql.md)|hh:mm:ss [.nnnnnnn]|00:00:00.0000000 à 23:59:59.9999999|100 nanosecondes|De 3 à 5|Oui|Non|
|[date](../../t-sql/data-types/date-transact-sql.md)|AAAA-MM-JJ|0001-01-01 à 9999-12-31|1 jour|3|Non|Non|
|[smalldatetime](../../t-sql/data-types/smalldatetime-transact-sql.md)|AAAA-MM-JJ hh:mm:ss|1900-01-01 à 2079-06-06|1 minute|4|Non|Non|
|[datetime](../../t-sql/data-types/datetime-transact-sql.md)|AAAA-MM-JJ hh:mm:ss[.nnn]|1753-01-01 à 9999-12-31|0,00333 seconde|8|Non|Non|
|[datetime2](../../t-sql/data-types/datetime2-transact-sql.md)|AAAA-MM-JJ hh:mm:ss[.nnnnnnn]|0001-01-01 00:00:00.0000000 à 9999-12-31 23:59:59.9999999|100 nanosecondes|6 à 8|Oui|Non|
|[datetimeoffset](../../t-sql/data-types/datetimeoffset-transact-sql.md)|AAAA-MM-JJ hh:mm:ss[.nnnnnnn] [+&#124;-]hh:mm|0001-01-01 00:00:00,0000000 à 9999-12-31 23:59:59,9999999 (au format UTC)|100 nanosecondes|8 à 10|Oui|Oui|

> [!NOTE]
> Avec [!INCLUDE[tsql](../../includes/tsql-md.md)], le type de données [rowversion](../../t-sql/data-types/rowversion-transact-sql.md) n’est pas un type de données de date et d’heure. **timestamp** est un synonyme déprécié de **rowversion**.

## <a name="date-and-time-functions"></a><a name="DateandTimeFunctions"></a> Fonctions de date et heure

Les tableaux suivants répertorient les fonctions de date et d’heure [!INCLUDE[tsql](../../includes/tsql-md.md)]. Pour plus d’informations sur le déterminisme, consultez [Fonctions déterministes et non déterministes](../../relational-databases/user-defined-functions/deterministic-and-nondeterministic-functions.md).

### <a name="function-that-return-system-date-and-time-values"></a><a name="GetSystemDateandTimeValues"></a> Fonctions permettant de retourner des valeurs système de date et d’heure

[!INCLUDE[tsql](../../includes/tsql-md.md)] dérive toutes les valeurs système de date et d’heure du système d’exploitation de l’ordinateur sur lequel s’exécute l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

#### <a name="higher-precision-system-date-and-time-functions"></a>Fonctions système de date et d'heure de plus grande précision

[!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] dérive les valeurs de date et d’heure en utilisant l’API Windows GetSystemTimeAsFileTime(). La précision dépend des composants matériels de l’ordinateur et de la version de Windows sur laquelle s’exécute l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Cette API a une précision fixée à 100 nanosecondes. Utilisez l’API Windows GetSystemTimeAdjustment() pour déterminer la précision.

|Fonction|Syntaxe|Valeur retournée|Type de données de retour|Propriété de déterminisme|
|---|---|---|---|---|
|[SYSDATETIME](../../t-sql/functions/sysdatetime-transact-sql.md)|SYSDATETIME ()|Retourne une valeur **datetime2(7)** contenant la date et l’heure de l’ordinateur sur lequel s’exécute l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. La valeur retournée n’inclut pas le décalage de fuseau horaire.|**datetime2(7)**|Non déterministe|
|[SYSDATETIMEOFFSET](../../t-sql/functions/sysdatetimeoffset-transact-sql.md)|SYSDATETIMEOFFSET ()|Retourne une valeur **datetimeoffset(7)** contenant la date et l’heure de l’ordinateur sur lequel s’exécute l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. La valeur retournée inclut le décalage de fuseau horaire.|**datetimeoffset(7)**|Non déterministe|
|[SYSUTCDATETIME](../../t-sql/functions/sysutcdatetime-transact-sql.md)|SYSUTCDATETIME ( )|Retourne une valeur **datetime2(7)** contenant la date et l’heure de l’ordinateur sur lequel s’exécute l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. La fonction retourne les valeurs de date et d’heure sous forme d’heure UTC (temps universel coordonné).|**datetime2(7)**|Non déterministe|

#### <a name="lower-precision-system-date-and-time-functions"></a>Fonctions système de date et d'heure de moindre précision

|Fonction|Syntaxe|Valeur retournée|Type de données de retour|Propriété de déterminisme|
|---|---|---|---|---|
|[CURRENT_TIMESTAMP](../../t-sql/functions/current-timestamp-transact-sql.md)|CURRENT_TIMESTAMP|Retourne une valeur **datetime** contenant la date et l’heure de l’ordinateur sur lequel s’exécute l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. La valeur retournée n’inclut pas le décalage de fuseau horaire.|**datetime**|Non déterministe|
|[GETDATE](../../t-sql/functions/getdate-transact-sql.md)|GETDATE ( )|Retourne une valeur **datetime** contenant la date et l’heure de l’ordinateur sur lequel s’exécute l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. La valeur retournée n’inclut pas le décalage de fuseau horaire.|**datetime**|Non déterministe|
|[GETUTCDATE](../../t-sql/functions/getutcdate-transact-sql.md)|GETUTCDATE ( )|Retourne une valeur **datetime** contenant la date et l’heure de l’ordinateur sur lequel s’exécute l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. La fonction retourne les valeurs de date et d’heure sous forme d’heure UTC (temps universel coordonné).|**datetime**|Non déterministe|

### <a name="functions-that-return-date-and-time-parts"></a><a name="GetDateandTimeParts"></a> Fonctions permettant de retourner des parties de date et d’heure

|Fonction|Syntaxe|Valeur retournée|Type de données de retour|Propriété de déterminisme|
|--------------|------------|------------------|----------------------|-----------------|
|[DATENAME](../../t-sql/functions/datename-transact-sql.md)|DATENAME ( *datepart* , *date* )|Retourne une chaîne de caractères représentant la partie date (*datepart*) spécifiée de la date spécifiée.|**nvarchar**|Non déterministe|
|[DATEPART](../../t-sql/functions/datepart-transact-sql.md)|DATEPART ( *datepart* , *date* )|Retourne un entier représentant la partie date (*datepart*) spécifiée de la *date* spécifiée.|**int**|Non déterministe|
|[DAY](../../t-sql/functions/day-transact-sql.md)|DAY ( *date* )|Retourne un entier représentant la partie jour de la *date* spécifiée.|**int**|Déterministe|
|[MONTH](../../t-sql/functions/month-transact-sql.md)|MONTH ( *date* )|Retourne un entier représentant la partie mois d’une *date* spécifiée.|**int**|Déterministe|
|[YEAR](../../t-sql/functions/year-transact-sql.md)|YEAR ( *date* )|Retourne un entier représentant la partie année d’une *date* spécifiée.|**int**|Déterministe|

### <a name="functions-that-return-date-and-time-values-from-their-parts"></a><a name="fromParts"></a> Fonctions permettant de retourner des valeurs de date et d’heure à partir de leurs parties

|Fonction|Syntaxe|Valeur retournée|Type de données de retour|Propriété de déterminisme|
|---|---|---|---|---|
|[DATEFROMPARTS](../../t-sql/functions/datefromparts-transact-sql.md)|DATEFROMPARTS ( *year*, *month*, *day* )|Renvoie une valeur **date** pour l’année, le mois et le jour spécifiés.|**date**|Déterministe|
|[DATETIME2FROMPARTS](../../t-sql/functions/datetime2fromparts-transact-sql.md)|DATETIME2FROMPARTS ( *year*, *month*, *day*, *hour*, *minute*, *seconds*, *fractions*, *precision*)|Retourne une valeur **datetime2** pour la date et l’heure spécifiées, avec la précision spécifiée.|**datetime2(** _precision_ **)**|Déterministe|
|[DATETIMEFROMPARTS](../../t-sql/functions/datetimefromparts-transact-sql.md)|DATETIMEFROMPARTS ( *year*, *month*, *day*, *hour*, *minute*, *seconds*, *milliseconds*)|Renvoie une valeur **datetime** pour la date et l’heure spécifiées.|**datetime**|Déterministe|
|[DATETIMEOFFSETFROMPARTS](../../t-sql/functions/datetimeoffsetfromparts-transact-sql.md)|DATETIMEOFFSETFROMPARTS ( *year*, *month*, *day*, *hour*, *minute*, *seconds*, *fractions*, *hour_offset*, *minute_offset*, *precision*)|Retourne une valeur **datetimeoffset** pour la date et l’heure spécifiées, avec la précision et les décalages spécifiés.|**datetimeoffset(** _precision_ **)**|Déterministe|
|[SMALLDATETIMEFROMPARTS](../../t-sql/functions/smalldatetimefromparts-transact-sql.md)|SMALLDATETIMEFROMPARTS ( *year*, *month*, *day*, *hour*, *minute* )|Renvoie une valeur **smalldatetime** pour la date et l’heure spécifiées.|**smalldatetime**|Déterministe|
|[TIMEFROMPARTS](../../t-sql/functions/timefromparts-transact-sql.md)|TIMEFROMPARTS ( *hour*, *minute*, *seconds*, *fractions*, *precision* )|Retourne une valeur **time** pour l’heure spécifiée, avec la précision spécifiée.|**time(** _precision_ **)**|Déterministe|

### <a name="functions-that-return-date-and-time-difference-values"></a><a name="GetDateandTimeDifference"></a> Fonctions permettant de retourner des valeurs de différence de date et d’heure

|Fonction|Syntaxe|Valeur retournée|Type de données de retour|Propriété de déterminisme|
|---|---|---|---|---|
|[DATEDIFF](../../t-sql/functions/datediff-transact-sql.md)|DATEDIFF ( *datepart* , *startdate* , *enddate* )|Retourne le nombre de limites *datepart* de date ou d’heure traversées entre deux dates spécifiées.|**int**|Déterministe|
|[DATEDIFF_BIG](../../t-sql/functions/datediff-big-transact-sql.md)|DATEDIFF_BIG ( *datepart* , *startdate* , *enddate* )|Retourne le nombre de limites *datepart* de date ou d’heure traversées entre deux dates spécifiées.|**bigint**|Déterministe|

### <a name="functions-that-modify-date-and-time-values"></a><a name="ModifyDateandTimeValues"></a> Fonctions permettant de modifier les valeurs de date et d’heure

|Fonction|Syntaxe|Valeur retournée|Type de données de retour|Propriété de déterminisme|
|---|---|---|---|---|
|[DATEADD](../../t-sql/functions/dateadd-transact-sql.md)|DATEADD (*datepart* , *number* , *date* )|Retourne une nouvelle valeur **datetime** en ajoutant un intervalle au *datepart* spécifié de la *date* spécifiée.|Type de données de l’argument *date*.|Déterministe|
|[EOMONTH](../../t-sql/functions/eomonth-transact-sql.md)|EOMONTH ( *start_date* [, *month_to_add* ] )|Retourne le dernier jour du mois contenant la date spécifiée, avec un décalage facultatif.|Le type de retour est le type de l’argument *start_date* ou le type de données **date**.|Déterministe|
|[SWITCHOFFSET](../../t-sql/functions/switchoffset-transact-sql.md)|SWITCHOFFSET (*DATETIMEOFFSET* , *time_zone*)|SWITCHOFFSET change le décalage de fuseau horaire d’une valeur DATETIMEOFFSET et préserve la valeur UTC.|**datetimeoffset** avec la précision fractionnelle de *DATETIMEOFFSET*.|Déterministe|
|[TODATETIMEOFFSET](../../t-sql/functions/todatetimeoffset-transact-sql.md)|TODATETIMEOFFSET (*expression* , *time_zone*)|TODATETIMEOFFSET transforme une valeur datetime2 en une valeur datetimeoffset. *TODATETIMEOFFSET* interprète la valeur datetime2 en heure locale, pour le time_zone spécifié.|**datetimeoffset** avec la précision fractionnelle de l’argument *datetime*|Déterministe|

### <a name="functions-that-set-or-return-session-format-functions"></a><a name="SetorGetSessionFormatFunctions"></a> Fonctions permettant de définir ou de retourner des fonctions de format de session

|Fonction|Syntaxe|Valeur retournée|Type de données de retour|Propriété de déterminisme|
|---|---|---|---|---|
|[@@DATEFIRST](../../t-sql/functions/datefirst-transact-sql.md)|@@DATEFIRST|Retourne la valeur actuelle, pour la session, de SET DATEFIRST.|**tinyint**|Non déterministe|
|[SET DATEFIRST](../../t-sql/statements/set-datefirst-transact-sql.md)|SET DATEFIRST { *number* &#124; **\@** _number_var_ }|Affecte au premier jour de la semaine un nombre allant de 1 à 7.|Non applicable|Non applicable|
|[SET DATEFORMAT](../../t-sql/statements/set-dateformat-transact-sql.md)|SET DATEFORMAT { *format* &#124; **@** _format_var_ }|Définit l’ordre des éléments de date (jour/mois/année) pour enregistrer des données de type **datetime** ou **smalldatetime**.|Non applicable|Non applicable|
|[@@LANGUAGE](../../t-sql/functions/language-transact-sql.md)|@@LANGUAGE|Retourne le nom de la langue actuellement utilisée. @@LANGUAGE n’est pas une fonction de date ou d’heure. Toutefois, le paramètre de langue peut affecter la sortie de fonctions de date.|Non applicable|Non applicable|
|[SET LANGUAGE](../../t-sql/statements/set-language-transact-sql.md)|SET LANGUAGE { [ N ] **'** _language_ **'** &#124; **\@** _language_var_ }|Définit l'environnement de la langue pour la session et les messages système. SET LANGUAGE n'est pas une fonction de date ou d'heure. Toutefois, le paramètre de langue affecte la sortie de fonctions de date.|Non applicable|Non applicable|
|[sp_helplanguage](../../relational-databases/system-stored-procedures/sp-helplanguage-transact-sql.md)|**sp_helplanguage** [ [ **@language =** ] **'** _language_ **'** ]|Retourne des informations sur les formats de date de toutes les langues prises en charge. **sp_helplanguage** n’est pas une procédure stockée de date ou d’heure. Toutefois, le paramètre de langue affecte la sortie de fonctions de date.|Non applicable|Non applicable|

### <a name="functions-that-validate-date-and-time-values"></a><a name="ValidateDateandTimeValues"></a> Fonctions permettant de valider les valeurs de date et d’heure

|Fonction|Syntaxe|Valeur retournée|Type de données de retour|Propriété de déterminisme|
|---|---|---|---|---|
|[ISDATE](../../t-sql/functions/isdate-transact-sql.md)|ISDATE ( *expression* )|Détermine si une expression d’entrée **datetime** ou **smalldatetime** a une valeur de date ou d’heure valide.|**int**|ISDATE est déterministe uniquement si elle est utilisée avec la fonction CONVERT, quand le paramètre de style CONVERT est spécifié et quand le style est différent de 0, 100, 9 ou 109.|

## <a name="date-and-time-related-topics"></a><a name="DateandTimeRelatedTopics"></a> Rubriques relatives à la date et à l’heure

|Rubrique|Description|
|-----------|-----------------|
|[FORMAT](../../t-sql/functions/format-transact-sql.md)|Retourne une valeur mise en forme avec la culture facultative et le format spécifiés. Utilisez la fonction FORMAT pour la mise en forme comme chaînes de valeurs de date/heure et de valeurs numériques compatibles avec les paramètres régionaux.|
|[CAST et CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)|Fournit des informations sur la conversion des valeurs de date et d’heure depuis et vers des littéraux de chaîne et d’autres formats de date et d’heure.|
|[Rédiger des instructions Transact-SQL internationales](../../relational-databases/collations/write-international-transact-sql-statements.md)|Fournit des directives relatives à la portabilité des bases de données et applications de bases de données qui utilisent des instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] d’une langue à l’autre ou qui prennent en charge plusieurs langues.|
|[Fonctions scalaires ODBC &#40;Transact-SQL&#41;](../../t-sql/functions/odbc-scalar-functions-transact-sql.md)|Fournit des informations sur les fonctions scalaires ODBC qui peuvent être utilisées dans les instructions [!INCLUDE[tsql](../../includes/tsql-md.md)]. Cela inclut les fonctions de date et d’heure ODBC.|
|[AT TIME ZONE &#40;Transact-SQL&#41;](../../t-sql/queries/at-time-zone-transact-sql.md)|Fournit la conversion de fuseau horaire.|

## <a name="see-also"></a>Voir aussi

- [Fonctions](../../t-sql/functions/functions.md)
- [Types de données &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)
