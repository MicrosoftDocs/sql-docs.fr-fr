---
description: datetime2 (Transact-SQL)
title: datetime2 (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/21/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- datetime2
- datetime2_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- time [SQL Server], data types
- dates [SQL Server], data types
- date and time [SQL Server], datetime2
- data types [SQL Server], date and time
- datetime2 data type [SQL Server]
ms.assetid: 868017f3-214f-43ef-8536-cc1632a2288f
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 0bdacc09933666e1deb2d238f867441a8cbc8f6c
ms.sourcegitcommit: c83c17e44b5e1e3e2a3b5933c2a1c4afb98eb772
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/15/2021
ms.locfileid: "100525184"
---
# <a name="datetime2-transact-sql"></a>datetime2 (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Définit une date qui est associée à une heure de la journée au format 24 heures. **datetime2** peut être considéré comme une extension du type **datetime** existant qui a une plus grande plage de dates, une plus grande précision de fraction par défaut et une précision spécifiée par l’utilisateur facultative.
  
## <a name="datetime2-description"></a>Description de datetime2
  
|Property|Valeur|  
|--------------|-----------|  
|Syntaxe|**datetime2** [ (*précision à la fraction de seconde*) ]|  
|Utilisation|DECLARE \@MyDatetime2 **datetime2(7)**<br /><br /> CREATE TABLE Table1 ( Column1 **datetime2(7)** )|  
|Format de littéral de chaîne par défaut<br /><br /> (utilisé pour le client de bas niveau)|AAAA-MM-JJ hh:mm:ss[.fractions de seconde]<br /><br /> Pour plus d’informations, consultez la section « Compatibilité descendante pour les clients de bas niveau » ci-dessous.|  
|Plage de dates|0001-01-01 à 9999-12-31<br /><br /> Du 1er janvier de l’an 1 au 31 décembre 9999|  
|Plage temporelle|00:00:00 à 23:59:59.9999999|  
|Plage de décalages de fuseau horaire|Aucun|  
|Plages d'éléments|AAAA est un nombre de quatre chiffres, entre 0001 et 9999, qui représente une année.<br /><br /> MM est un nombre à deux chiffres, entre 01 et 12, qui représente un mois dans l'année spécifiée.<br /><br /> DD est un nombre à deux chiffres, entre 01 et 31 selon le mois, qui représente un jour du mois spécifié.<br /><br /> hh est un nombre à deux chiffres, entre 00 et 23, qui représente l'heure.<br /><br /> mm est un nombre à deux chiffres, entre 00 et 59, qui représente la minute.<br /><br /> ss est un nombre à deux chiffres, entre 00 et 59, qui représente la seconde.<br /><br /> n* est un nombre qui comprend entre zéro et sept chiffres, entre 0 et 9999999, qui représente les fractions de seconde. Dans Informatica, les fractions de seconde sont tronquées quand n > 3.|  
|Longueur de caractère|19 positions au minimum (AAAA-MM-JJ hh:mm:ss) et 27 au maximum (AAAA-MM-JJ hh:mm:ss.0000000)|  
|Précision, échelle|De 0 à 7 chiffres, avec une précision de 100 ns. La précision par défaut est de 7 chiffres.|  
|Taille de stockage <sup>1</sup>|6 octets pour une précision inférieure à 3.<br/>7 octets pour une précision de 3 ou 4.<br/>Toutes les autres précisions nécessitent 8 octets.<sup>2</sup>|  
|Précision|100 nanosecondes|  
|Valeur par défaut|1900-01-01 00:00:00|  
|Calendrier|Grégorien|  
|Précision à la fraction de seconde définie par l'utilisateur|Oui|  
|Prise en charge et conservation du décalage de fuseau horaire|Non|  
|Prise en charge de l'heure d'été|Non|  

<sup>1</sup> Les valeurs fournies sont destinées à un rowstore non compressé. L’utilisation de la [compression de données](../../relational-databases/data-compression/data-compression.md) ou de [columnstore](../../relational-databases/indexes/columnstore-indexes-overview.md) peut altérer la taille de stockage pour chaque précision. De plus, la taille de stockage sur disque et en mémoire peut être différente. Par exemple, les valeurs **datetime2** nécessitent toujours 8 octets en mémoire quand le mode batch est utilisé.

<sup>2</sup> Quand une valeur **datetime2** est castée en valeur **varbinary**, un octet supplémentaire est ajouté à la valeur **varbinary** pour stocker la précision.

Pour afficher les métadonnées de type de données, consultez [sys.systypes &#40;Transact-SQL&#41;](../../relational-databases/system-compatibility-views/sys-systypes-transact-sql.md) ou [TYPEPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/typeproperty-transact-sql.md). La précision et l'échelle sont variables pour certains types de données de date et d'heure. Pour obtenir la précision et l’échelle d’une colonne, consultez [COLUMNPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/columnproperty-transact-sql.md), [COL_LENGTH &#40;Transact-SQL&#41;](../../t-sql/functions/col-length-transact-sql.md) ou [sys.columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-columns-transact-sql.md).
  
## <a name="supported-string-literal-formats-for-datetime2"></a>Formats de littéraux de chaîne pris en charge pour datetime2
Les tableaux suivants répertorient les formats de littéraux de chaîne ISO 8601 et ODBC pris en charge pour **datetime2**. Pour plus d’informations sur les formats alphabétique, numérique, non séparé et d’heure pour les parties de date et d’heure de **datetime2**, consultez [date &#40;Transact-SQL&#41;](../../t-sql/data-types/date-transact-sql.md) et [time &#40;Transact-SQL&#41;](../../t-sql/data-types/time-transact-sql.md).
  
|ISO 8601|Descriptions|  
|---|---|
|AAAA-MM-JJThh:mm:ss[.nnnnnnn]<br /><br /> AAAA-MM-JJThh:mm:ss[.nnnnnnn]|Ce format n'est pas affecté par les paramètres régionaux de session SET LANGUAGE et SET DATEFORMAT. **T**, le signe deux-points (:) et le point (.) sont inclus dans le littéral de chaîne, par exemple « 2007-05-02T19:58:47.1234567 ».|  
  
|ODBC|Description|  
|---|---|
|{ ts 'aaaa-mm-jj hh:mm:ss[.fractions de seconde]' }|Spécifique à l'API ODBC :<br /><br /> Le nombre de chiffres à droite de la virgule décimale, qui représente les fractions de seconde, peut être spécifié de 0 jusqu'à 7 (100 nanosecondes).|  
  
## <a name="ansi-and-iso-8601-compliance"></a>Conformité ANSI et ISO 8601  
La conformité ANSI et ISO 8601 de [date](../../t-sql/data-types/date-transact-sql.md) et [time](../../t-sql/data-types/time-transact-sql.md) s’applique à **datetime2**.
  
##  <a name="backward-compatibility-for-down-level-clients"></a>Compatibilité descendante pour les clients de bas niveau  
Certains clients de bas niveau ne prennent pas en charge les types de données **time**, **date**, **datetime2** et **datetimeoffset**. Le tableau suivant présente le type de mappage entre une instance de haut niveau de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et des clients de bas niveau.
  
|Type de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|Format de littéral de chaîne par défaut passé au client de bas niveau|ODBC de bas niveau|OLEDB de bas niveau|JDBC de bas niveau|SQLCLIENT de bas niveau|  
| --- | --- | --- | --- | --- | --- |
|**time**|hh:mm:ss [.nnnnnnn]|SQL_WVARCHAR ou SQL_VARCHAR|DBTYPE_WSTR ou DBTYPE_STR|Java.sql.String|String ou SqString|  
|**date**|AAAA-MM-JJ|SQL_WVARCHAR ou SQL_VARCHAR|DBTYPE_WSTR ou DBTYPE_STR|Java.sql.String|String ou SqString|  
|**datetime2**|AAAA-MM-JJ hh:mm:ss[.nnnnnnn]|SQL_WVARCHAR ou SQL_VARCHAR|DBTYPE_WSTR ou DBTYPE_STR|Java.sql.String|String ou SqString|  
|**datetimeoffset**|AAAA-MM-JJ hh:mm:ss[.nnnnnnn] [+&#124;-]hh:mm|SQL_WVARCHAR ou SQL_VARCHAR|DBTYPE_WSTR ou DBTYPE_STR|Java.sql.String|String ou SqString|  
  
## <a name="converting-date-and-time-data"></a>Conversion de données date et time
Lorsque vous effectuez une conversion vers des types de données date et heure, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] rejette toutes les valeurs qu'il ne peut identifier comme dates ou heures. Pour plus d’informations sur l’utilisation des fonctions CAST et CONVERT avec des données date et time, consultez [CAST et CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)
  
### <a name="converting-other-date-and-time-types-to-the-datetime2-data-type"></a>Conversion d’autres types date et time en type de données datetime2
Cette section décrit ce qui se produit quand d’autres types de données date et time sont convertis en type de données **datetime2**.  
  
Dans le cas d’une conversion à partir de **date**, l’année, le mois et le jour sont copiés.  Le composant heure est défini sur 00:00:00.0000000.  Le code suivant montre les résultats de la conversion d'une valeur `date` en valeur `datetime2`.  
  
```sql
DECLARE @date date = '12-21-16';
DECLARE @datetime2 datetime2 = @date;

SELECT @datetime2 AS '@datetime2', @date AS '@date';
  
--Result  
--@datetime2                  @date
----------------------------- ----------
--2016-12-21 00:00:00.0000000 2016-12-21
```  
  
Dans le cas d’une conversion à partir de **time(n)**, le composant heure est copié et le composant date est défini sur « 1900-01-01 ». L'exemple suivant montre les résultats de la conversion d'une valeur `time(7)` en valeur `datetime2`.  
  
```sql
DECLARE @time time(7) = '12:10:16.1234567';
DECLARE @datetime2 datetime2 = @time;

SELECT @datetime2 AS '@datetime2', @time AS '@time';
  
--Result  
--@datetime2                  @time
----------------------------- ----------------
--1900-01-01 12:10:16.1234567 12:10:16.1234567
```  
  
Dans le cas d’une conversion à partir de **smalldatetime**, les heures et les minutes sont copiées. Les secondes et fractions de seconde sont définies sur 0. Le code suivant montre les résultats de la conversion d'une valeur `smalldatetime` en valeur `datetime2`.  
  
```sql
DECLARE @smalldatetime smalldatetime = '12-01-16 12:32';
DECLARE @datetime2 datetime2 = @smalldatetime;

SELECT @datetime2 AS '@datetime2', @smalldatetime AS '@smalldatetime'; 
  
--Result  
--@datetime2                  @smalldatetime
----------------------------- -----------------------
--2016-12-01 12:32:00.0000000 2016-12-01 12:32:00 
```  
  
Dans le cas d’une conversion à partir de **datetimeoffset(n)**, les composants date et heure sont copiés. Le fuseau horaire est tronqué. L'exemple suivant montre les résultats de la conversion d'une valeur `datetimeoffset(7)` en valeur `datetime2`.  
  
```sql
DECLARE @datetimeoffset datetimeoffset(7) = '2016-10-23 12:45:37.1234567 +10:0';
DECLARE @datetime2 datetime2 = @datetimeoffset;

SELECT @datetime2 AS '@datetime2', @datetimeoffset AS '@datetimeoffset'; 
  
--Result  
--@datetime2                  @datetimeoffset
----------------------------- ----------------------------------
--2016-10-23 12:45:37.1234567 2016-10-23 12:45:37.1234567 +10:00
```  

Dans le cas d’une conversion à partir de **datetime**, la date et l’heure sont copiées. La précision de fraction est étendue à 7 chiffres. L'exemple suivant montre les résultats de la conversion d'une valeur `datetime` en valeur `datetime2`.

```sql
DECLARE @datetime datetime = '2016-10-23 12:45:37.333';
DECLARE @datetime2 datetime2 = @datetime;

SELECT @datetime2 AS '@datetime2', @datetime AS '@datetime';
   
--Result  
--@datetime2                  @datetime
------------------------- ---------------------------
--2016-10-23 12:45:37.3333333 2016-10-23 12:45:37.333
```  

> [!NOTE]
> Sous le niveau de compatibilité de base de données 130, les conversions implicites des types de données datetime en datetime2 offrent une meilleure précision en prenant en compte les fractions de milliseconde, ce qui génère différentes valeurs converties, comme illustré dans l’exemple ci-dessus. Utilisez un transtypage explicite vers le type de données datetime2 chaque fois qu’il existe un scénario de comparaison mixte entre les types de données datetime et datetime2. Pour plus d’informations, consultez cet [article du Support technique Microsoft](https://support.microsoft.com/help/4010261).

### <a name="converting-string-literals-to-datetime2"></a>Conversion de littéraux de chaîne en datetime2  
Les conversions de littéraux de chaîne en types de date et d'heure sont autorisées si toutes les parties des chaînes sont dans des formats valides. Sinon, une erreur d'exécution est déclenchée. Les conversions implicites ou explicites qui ne spécifient pas de style à partir de types de date et d'heure en littéraux de chaîne seront au format par défaut de la session active. Le tableau suivant montre les règles de conversion d’un littéral de chaîne en type de données **datetime2**.
  
|Littéral de chaîne d'entrée|**datetime2(n)**|  
|---|---|
|ODBC DATE|Les littéraux de chaîne ODBC sont mappés au type de données **datetime**. Toute opération d’affectation de littéraux ODBC DATETIME dans des types **datetime2** provoque une conversion implicite entre **datetime** et ce type, comme défini par les règles de conversion.|  
|ODBC TIME|Consultez la règle DATE ODBC précédente.|  
|ODBC DATETIME|Consultez la règle DATE ODBC précédente.|  
|DATE uniquement|La partie TIME a pour valeur par défaut 00:00:00.|  
|TIME uniquement|La partie DATE a pour valeur par défaut 1900-1-1.|  
|TIMEZONE uniquement|Les valeurs par défaut sont fournies.|  
|DATE + TIME|Simple|  
|DATE + TIMEZONE|Non autorisé.|  
|TIME + TIMEZONE|La partie DATE a pour valeur par défaut 1900-1-1. L'entrée TIMEZONE est ignorée.|  
|DATE + TIME + TIMEZONE|Le DATETIME local sera utilisé.|  
  
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
|**time**|12:35:29.1234567|  
|**date**|2007-05-08|  
|**smalldatetime**|2007-05-08 12:35:00|  
|**datetime**|2007-05-08 12:35:29.123|  
|**datetime2**|2007-05-08 12:35:29.1234567|  
|**datetimeoffset**|2007-05-08 12:35:29.1234567 +12:15|  
  
## <a name="see-also"></a>Voir aussi
[CAST et CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)
  
  
