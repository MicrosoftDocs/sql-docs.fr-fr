---
description: datetime (Transact-SQL)
title: datetime (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/23/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- datetime_TSQL
- datetime
dev_langs:
- TSQL
helpviewer_keywords:
- time [SQL Server], data types
- date and time [SQL Server], datetime
- dates [SQL Server], data types
- datetime data type [SQL Server]
- data types [SQL Server], date and time
ms.assetid: 9bd1cc5b-227b-4032-95d6-7581ddcc9924
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: b7ce090d303fbdaeb5abacb0522c8ae4c6dc9f9c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99172563"
---
# <a name="datetime-transact-sql"></a>datetime (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Définit une date qui est associée à une heure de la journée avec des fractions de seconde qui se présente au format 24 heures.
  
> [!NOTE]  
>  Utilisez les types de données **time**, **date**, **datetime2** et **datetimeoffset** pour une nouvelle tâche. Ces types s'alignent sur la norme SQL. Ils sont plus portables. **time**, **datetime2** et **datetimeoffset** offrent une meilleure précision à la seconde. **datetimeoffset** fournit la prise en charge des fuseaux horaires pour les applications globalement déployées.  
  
## <a name="datetime-description"></a>Description de datetime  
  
|Propriété|Valeur|  
|---|---|
|Syntaxe|**datetime**|  
|Utilisation|DECLARE \@MyDatetime **datetime**<br /><br /> CREATE TABLE Table1 ( Column1 **datetime** )|  
|Formats de littéraux de chaîne par défaut<br /><br /> (utilisé pour le client de bas niveau)|Non applicable|  
|Plage de dates|Du 1er janvier 1753 au 31 décembre 9999|  
|Plage temporelle|00:00:00 à 23:59:59.997|  
|Plage de décalages de fuseau horaire|Aucun|  
|Plages d'éléments|AAAA comprend quatre chiffres, entre 1753 et 9999, qui représentent une année.<br /><br /> MM comprend deux chiffres, entre 01 et 12, qui représentent un mois de l’année spécifiée.<br /><br /> DD comprend deux chiffres, entre 01 et 31 selon le mois, qui représentent un jour du mois spécifié.<br /><br /> hh comprend deux chiffres, entre 00 et 23, qui représentent l'heure.<br /><br /> mm comprend deux chiffres, entre 00 et 59, qui représentent la minute.<br /><br /> ss comprend deux chiffres, entre 00 et 59, qui représentent la seconde.<br /><br /> n* comprend entre zéro et trois chiffres, entre 0 et 999, qui représentent les fractions de seconde.|  
|Longueur de caractère|19 positions au minimum et 23 au maximum|  
|Taille de stockage|8 octets|  
|Précision|Arrondi aux incréments de ,000, ,003 ou ,007 secondes|  
|Valeur par défaut|1900-01-01 00:00:00|  
|Calendrier|Grégorien (n’inclut pas la plage complète des années.)|  
|Précision à la fraction de seconde définie par l'utilisateur|Non|  
|Prise en charge et conservation du décalage de fuseau horaire|Non|  
|Prise en charge de l'heure d'été|Non|  
  
## <a name="supported-string-literal-formats-for-datetime"></a>Formats de littéraux de chaîne pris en charge pour datetime  
Les tableaux suivants répertorient les formats de littéraux de chaîne pris en charge pour **datetime**. Sauf pour ODBC, les littéraux de chaîne **datetime** sont encadrés par des guillemets simples ('), par exemple 'string_literaL'. Si l’environnement n’est pas **us_english**, les littéraux de type chaîne doivent être au format N'string_literaL'.
  
|Numérique|Description|  
|---|---|
|Formats de date :<br /><br /> [0]4/15/[19]96 -- (mja)<br /><br /> [0]4-15-[19]96 -- (mja)<br /><br /> [0]4.15.[19]96 -- (mja)<br /><br /> [0]4/[19]96/15 -- (maj)<br /><br /> 15/[0]4/[19]96 -- (jma)<br /><br /> 15/[19]96/[0]4 -- (jam)<br /><br /> [19]96/15/[0]4 -- (ajm)<br /><br /> [19]96/[0]4/15 -- (amj)<br /><br /> Formats d'heure :<br /><br /> 14:30<br /><br /> 14:30[:20:999]<br /><br /> 14:30[:20.9]<br /><br /> 4am<br /><br /> 16:00|Vous pouvez spécifier les données de date à l'aide d'un mois spécifié sous forme numérique. Par exemple, 5/20/97 correspond au vingtième jour du mois de mai 1997. Lorsque vous utilisez un format de date numérique, spécifiez l'année, le mois et le jour dans une chaîne qui utilise des barres obliques (/), des traits d'union (-) ou des points (.) comme séparateurs. Cette chaîne doit apparaître sous la forme suivante :<br /><br /> *nombre séparateur nombre séparateur nombre [heure] [heure]*<br /><br /> <br /><br /> Quand la langue sélectionnée est **français**, l’ordre par défaut pour la date est jma. Vous pouvez modifier l’ordre de la date à l’aide de l’instruction [SET DATEFORMAT](../../t-sql/statements/set-dateformat-transact-sql.md).<br /><br /> Le paramètre de SET DATEFORMAT détermine l'interprétation des valeurs de date. Si l’ordre ne correspond pas au paramètre, les valeurs ne sont pas interprétées comme des dates. Les dates en désordre peuvent être interprétées comme étant hors limites ou ayant des valeurs incorrectes. Par exemple, 12/10/08 peut être interprété comme 6 dates différentes, selon le paramétrage de DATEFORMAT. Une année en quatre parties est interprétée comme l'année.|  
  
|Alphabétique|Description|  
|---|---|
|Avr[il] [15][,] 1996<br /><br /> Avr[il] 15[,] [19]96<br /><br /> Avr[il] 1996 [15]<br /><br /> [15] Avr[il][,] 1996<br /><br /> 15 Avr[il][,][19]96<br /><br /> 15 [19]96 avr[il]<br /><br /> [15] 1996 avr[il]<br /><br /> 1996 AvR[IL] [15]<br /><br /> 1996 [15] AVR[IL]|Vous pouvez spécifier des données de date avec un mois spécifié comme nom complet du mois. Par exemple, Avril ou l'abréviation Avr spécifiée dans la langue actuelle ; les virgules sont facultatives et la mise en majuscules est ignorée.<br /><br /> Instructions relatives à l'utilisation des formats de date alphabétiques :<br /><br /> 1) Placez la date et l’heure entre des guillemets simples ('). Pour les langues autres que l'anglais, utilisez N'<br /><br /> 2) Les caractères placés entre crochets sont facultatifs.<br /><br /> 3) Si vous spécifiez uniquement les deux derniers chiffres de l’année, les valeurs inférieures aux deux derniers chiffres de la valeur de l’option de configuration [Configurer l’option de configuration de serveur two digit year cutoff](../../database-engine/configure-windows/configure-the-two-digit-year-cutoff-server-configuration-option.md) appartiennent au même siècle que l’année de coupure. Les valeurs supérieures ou égales à la valeur de cette option appartiennent au siècle qui précède l’année de coupure. Par exemple, si l’option **two digit year cutoff** a pour valeur 2050 (valeur par défaut), 25 est interprété comme 2025 et 50 comme 1950. Pour éviter toute ambiguïté, représentez les années à l'aide de quatre chiffres.<br /><br /> 4) Si le jour n’est pas précisé, le premier jour du mois est rajouté.<br /><br /> <br /><br /> Le paramètre de session SET DATEFORMAT n’est pas appliqué lorsque vous indiquez le mois sous forme alphabétique.|  
  
|ISO 8601|Description|  
|---|---|
|AAAA-MM-JJThh:mm:ss[.mmm]<br /><br /> AAAAMMJJ[ hh:mm:ss [.mmm]]|Exemples :<br /><br /> 1) 2004-05-23T14:25:10<br /><br /> 2) 2004-05-23T14:25:10.487<br /><br /> <br /><br /> Pour utiliser le format ISO 8601, vous devez spécifier chaque élément au format indiqué ci-dessus, notamment le **T**, les deux-points (:) et le point (.).<br /><br /> Les crochets indiquent que le composant fractions de seconde est facultatif. Le composant heure s'exprime au format 24 heures.<br /><br /> T indique le début de la partie heure de la valeur **datetime**.<br /><br /> Le format ISO 8601 présente un avantage de taille puisqu'il s'agit d'une norme internationale avec une spécification claire. Ce format n’est pas affecté par le paramètre SET DATEFORMAT ou [SET LANGUAGE](../../t-sql/statements/set-language-transact-sql.md).|  
  
|Non séparé|Description|  
|---|---|
|AAAAMMJJ hh:mm:ss [.mmm]||  
  
|ODBC|Description|  
|---|---|
|{ ts '1998-05-02 01:23:56.123' }<br /><br /> { d '1990-10-02' }<br /><br /> { t '13:33:41' }|L'API ODBC définit des séquences d'échappement pour représenter les valeurs de date et d'heure, ce qu'ODBC appelle des données horodateur. Ce format d’horodatage ODBC est également pris en charge par la définition de langage OLE DB (DBGUID-SQL) prise en charge par le fournisseur [!INCLUDE[msCoName](../../includes/msconame-md.md)] OLE DB pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Les applications qui utilisent les API basées sur ADO, OLE DB et ODBC peuvent utiliser ce format d'horodateur ODBC pour représenter les dates et les heures.<br /><br /> Les séquences d’échappement d’horodatage ODBC ont le format { *literal_type* '*constant_value*' }:<br /><br /> <br /><br /> - *literal_type* spécifie le type de séquence d’échappement. Les horodatages ont trois spécificateurs *literal_type* :<br />1) d = date uniquement<br />2) t = heure uniquement<br />3) ts = horodatage (heure + date)<br /><br /> <br /><br /> - '*constant_value*' est la valeur de la séquence d’échappement. *constant_value* doit respecter les formats suivants pour chaque *literal_type*.<br />d : aaaa-mm-jj<br />t : hh:mm:ss[.fff]<br />ts : aaaa-mm-jj hh:mm:ss[.fff]|  
  
## <a name="rounding-of-datetime-fractional-second-precision"></a>Arrondi de la précision à la fraction de seconde datetime  
Les valeurs **datetime** sont arrondies à des incréments de .000, .003 ou .007 secondes, comme cela est illustré dans le tableau suivant.
  
|Valeur spécifiée par l'utilisateur|Valeur stockée système|  
|---|---|
|01/01/98 23:59:59.999|1998-01-02 00:00:00.000|  
|01/01/98 23:59:59.995<br /><br /> 01/01/98 23:59:59.996<br /><br /> 01/01/98 23:59:59.997<br /><br /> 01/01/98 23:59:59.998|1998-01-01 23:59:59.997|  
|01/01/98 23:59:59.992<br /><br /> 01/01/98 23:59:59.993<br /><br /> 01/01/98 23:59:59.994|1998-01-01 23:59:59.993|  
|01/01/98 23:59:59.990<br /><br /> 01/01/98 23:59:59.991|1998-01-01 23:59:59.990|  
  
## <a name="ansi-and-iso-8601-compliance"></a>Conformité ANSI et ISO 8601  
**datetime** n’est pas conforme au format ANSI ou ISO 8601.
  
##  <a name="converting-date-and-time-data"></a><a name="_datetime"></a> Conversion de données date et time  
Lorsque vous effectuez une conversion vers des types de données date et heure, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] rejette toutes les valeurs qu’il ne reconnaît pas comme des dates ou des heures. Pour plus d’informations sur l’utilisation des fonctions CAST et CONVERT avec des données date et time, consultez [CAST et CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md).
  
### <a name="converting-other-date-and-time-types-to-the-datetime-data-type"></a>Conversion d’autres types date et time en type de données datetime 
Cette section décrit ce qui se produit quand d’autres types de données date et time sont convertis en type de données **datetime**.  
  
Lors d’une conversion à partir du format **date**, l’année, le mois et le jour sont copiés. Le composant heure est défini sur 00:00:00.000. Le code suivant montre les résultats de la conversion d'une valeur `date` en valeur `datetime`.  
  
```sql
DECLARE @date date = '12-21-16';  
DECLARE @datetime datetime = @date;  
  
SELECT @datetime AS '@datetime', @date AS '@date';  
  
--Result  
--@datetime               @date  
------------------------- ----------  
--2016-12-21 00:00:00.000 2016-12-21  
```  
  
Dans le cas d’une conversion à partir de **time(n)**, le composant heure est copié et le composant date est défini sur « 1900-01-01 ». Quand la précision de fraction de la valeur **time(n)** est supérieure à trois chiffres, la valeur est tronquée en conséquence. L'exemple suivant montre les résultats de la conversion d'une valeur `time(4)` en valeur `datetime`.  
  
```sql
DECLARE @time time(4) = '12:10:05.1237';  
DECLARE @datetime datetime = @time;  
  
SELECT @datetime AS '@datetime', @time AS '@time';  
  
--Result  
--@datetime               @time  
------------------------- -------------  
--1900-01-01 12:10:05.123 12:10:05.1237  
```  
  
Dans le cas d’une conversion à partir de **smalldatetime**, les heures et les minutes sont copiées. Les secondes et fractions de seconde sont définies sur 0. Le code suivant montre les résultats de la conversion d'une valeur `smalldatetime` en valeur `datetime`.  
  
```sql
DECLARE @smalldatetime smalldatetime = '12-01-16 12:32';  
DECLARE @datetime datetime = @smalldatetime;  
  
SELECT @datetime AS '@datetime', @smalldatetime AS '@smalldatetime';  
  
--Result  
--@datetime               @smalldatetime  
------------------------- -----------------------  
--2016-12-01 12:32:00.000 2016-12-01 12:32:00  
```  
  
Dans le cas d’une conversion à partir de **datetimeoffset(n)**, les composants date et heure sont copiés. Le fuseau horaire est tronqué. Quand la précision de fraction de la valeur **datetimeoffset(n)** est supérieure à trois chiffres, la valeur est tronquée. L'exemple suivant montre les résultats de la conversion d'une valeur `datetimeoffset(4)` en valeur `datetime`.  
  
```sql
DECLARE @datetimeoffset datetimeoffset(4) = '1968-10-23 12:45:37.1234 +10:0';  
DECLARE @datetime datetime = @datetimeoffset;  
  
SELECT @datetime AS '@datetime', @datetimeoffset AS '@datetimeoffset';  
  
--Result  
--@datetime               @datetimeoffset  
------------------------- ------------------------------  
--1968-10-23 12:45:37.123 1968-10-23 12:45:37.1237 +01:0   
```  
  
Dans le cas d’une conversion à partir de **datetime2(n)**, la date et l’heure sont copiées. Quand la précision de fraction de la valeur **datetime2(n)** est supérieure à trois chiffres, la valeur est tronquée. L'exemple suivant montre les résultats de la conversion d'une valeur `datetime2(4)` en valeur `datetime`.  
  
```sql
DECLARE @datetime2 datetime2(4) = '1968-10-23 12:45:37.1237';  
DECLARE @datetime datetime = @datetime2;  
  
SELECT @datetime AS '@datetime', @datetime2 AS '@datetime2';  
  
--Result  
--@datetime               @datetime2  
------------------------- ------------------------  
--1968-10-23 12:45:37.123 1968-10-23 12:45:37.1237  
```  
  
## <a name="examples"></a>Exemples  
L’exemple suivant compare les résultats de la conversion d’une chaîne en chaque type de données **date** et **time**.
  
```sql
SELECT   
     CAST('2007-05-08 12:35:29. 1234567 +12:15' AS time(7)) AS 'time'   
    ,CAST('2007-05-08 12:35:29. 1234567 +12:15' AS date) AS 'date'   
    ,CAST('2007-05-08 12:35:29.123' AS smalldatetime) AS   
        'smalldatetime'   
    ,CAST('2007-05-08 12:35:29.123' AS datetime) AS 'datetime'   
    ,CAST('2007-05-08 12:35:29. 1234567 +12:15' AS datetime2(7)) AS   
        'datetime2'  
    ,CAST('2007-05-08 12:35:29.1234567 +12:15' AS datetimeoffset(7)) AS   
        'datetimeoffset';  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
|Type de données|Output|  
|---|---|
|**time**|12:35:29. 1234567|  
|**date**|2007-05-08|  
|**smalldatetime**|2007-05-08 12:35:00|  
|**datetime**|2007-05-08 12:35:29.123|  
|**datetime2**|2007-05-08 12:35:29. 1234567|  
|**datetimeoffset**|2007-05-08 12:35:29.1234567 +12:15|  
  
## <a name="see-also"></a>Voir aussi
[CAST et CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)
  
  
