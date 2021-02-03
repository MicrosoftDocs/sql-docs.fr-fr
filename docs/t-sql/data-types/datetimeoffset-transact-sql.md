---
description: datetimeoffset (Transact-SQL)
title: datetimeoffset (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/23/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- datetimeoffset_TSQL
- datetimeoffset
dev_langs:
- TSQL
helpviewer_keywords:
- time [SQL Server], data types
- date and time [SQL Server], datetimeoffset
- datetimeoffset data type [SQL Server]
- dates [SQL Server], data types
- data types [SQL Server], date and time
- time zones [SQL Server]
ms.assetid: a0455b71-ca25-476e-a7a8-0770f1860bb7
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 76913dfeded63354a8597027380e7e966ff73f31
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99172574"
---
# <a name="datetimeoffset-transact-sql"></a>datetimeoffset (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Définit une date qui est associée à une heure de la journée qui prend en charge les fuseaux horaires et se présente au format 24 heures.

## <a name="datetimeoffset-description"></a>Description de datetimeoffset
  
|Propriété|Valeur|  
|---|---|
|Syntaxe|**datetimeoffset** [ (*précision à la fraction de seconde*) ]|  
|Utilisation|DECLARE \@MyDatetimeoffset **datetimeoffset(7)**<br /><br /> CREATE TABLE Table1 ( Column1 **datetimeoffset(7)** )|  
|Formats de littéraux de chaîne par défaut (utilisés pour le client de bas niveau)|AAAA-MM-JJ hh:mm:ss[.nnnnnnn] [{+&#124;-}hh:mm]<br /><br /> Pour plus d’informations, consultez la section « Compatibilité descendante pour les clients de bas niveau » ci-dessous.|  
|Plage de dates|0001-01-01 à 9999-12-31<br /><br /> Du 1er janvier de l’an 1 au 31 décembre 9999|  
|Plage temporelle|00:00:00 à 23:59:59.9999999|  
|Plage de décalages de fuseau horaire|-14:00 à +14:00|  
|Plages d'éléments|AAAA comprend quatre chiffres, entre 0001 et 9999, qui représentent une année.<br /><br /> MM comprend deux chiffres, entre 01 et 12, qui représentent un mois de l’année spécifiée.<br /><br /> DD comprend deux chiffres, entre 01 et 31 selon le mois, qui représentent un jour du mois spécifié.<br /><br /> hh comprend deux chiffres, entre 00 et 23, qui représentent l'heure.<br /><br /> mm comprend deux chiffres, entre 00 et 59, qui représentent la minute.<br /><br /> ss comprend deux chiffres, entre 00 et 59, qui représentent la seconde.<br /><br /> n* comprend entre zéro et sept chiffres, entre 0 et 9999999, qui représentent les fractions de seconde.<br /><br /> hh comprend deux chiffres, entre -14 et +14. <br /><br /> mm comprend deux chiffres, entre 00 et 59.|  
|Longueur de caractère|26 positions au minimum (AAAA-MM-JJ hh:mm:ss {+&#124;-}hh:mm) et 34 au maximum (AAAA-MM-JJ hh:mm:ss.nnnnnnn {+&#124;-}hh:mm)|  
|Précision, échelle|Consultez le tableau ci-dessous.|  
|Taille de stockage|10 octets, fixes, sont la valeur par défaut avec une précision à la fraction de seconde de 100 ns par défaut.|  
|Précision|100 nanosecondes|  
|Valeur par défaut|1900-01-01 00:00:00 00:00|  
|Calendrier|Grégorien|  
|Précision à la fraction de seconde définie par l'utilisateur|Oui|  
|Prise en charge et conservation du décalage de fuseau horaire|Oui|  
|Prise en charge de l'heure d'été|Non|  
  
|Échelle spécifiée|Résultat (précision, échelle)|Longueur de colonne (octets)|Précision en fractions de seconde|  
|---|---|---|---|
|**datetimeoffset**|(34,7)|10|7|  
|**datetimeoffset(0)**|(26,0)|8|0-2|  
|**datetimeoffset(1)**|(28,1)|8|0-2|  
|**datetimeoffset(2)**|(29,2)|8|0-2|  
|**datetimeoffset(3)**|(30,3)|9|3-4|  
|**datetimeoffset(4)**|(31,4)|9|3-4|  
|**datetimeoffset(5)**|(32,5)|10|5-7|  
|**datetimeoffset(6)**|(33,6)|10|5-7|  
|**datetimeoffset(7)**|(34,7)|10|5-7|  
  
## <a name="supported-string-literal-formats-for-datetimeoffset"></a>Formats de littéraux de chaîne pris en charge pour datetimeoffset
Le tableau suivant répertorie les formats de littéraux de chaîne ISO 8601 pris en charge pour **datetimeoffset**. Pour plus d’informations sur les formats alphabétique, numérique, non séparé et d’heure pour les parties de date et d’heure de **datetimeoffset**, consultez [date &#40;Transact-SQL&#41;](../../t-sql/data-types/date-transact-sql.md) et [time &#40;Transact-SQL&#41;](../../t-sql/data-types/time-transact-sql.md).
  
|ISO 8601|Description|  
|---|---|
|AAAA-MM-JJThh:mm:ss[.nnnnnnn][{+&#124;-}hh:mm]|Ces deux formats ne sont pas affectés par les paramètres régionaux de session SET LANGUAGE et SET DATEFORMAT. Les espaces ne sont pas autorisés entre les éléments **datetimeoffset** et **datetime**.|  
|AAAA-MM-JJThh:mm:ss[.nnnnnnn]Z (UTC)|Ce format par définition ISO indique que la partie **datetime** doit être exprimée dans le fuseau horaire UTC. Par exemple, 1999-12-12 12:30:30.12345 -07:00 doit être représenté sous la forme 1999-12-12 19:30:30.12345Z.|  
  
## <a name="time-zone-offset"></a>Décalage de fuseau horaire
Un décalage de fuseau horaire spécifie le décalage de la zone de fuseau horaire UTC pour une valeur **time** ou **datetime**. Le décalage de fuseau horaire peut être représenté sous la forme [+|-] hh:mm:
-   hh comprend deux chiffres, entre 00 et 14, qui représentent le nombre d'heures dans le décalage de fuseau horaire.  
-   mm comprend deux chiffres, entre 00 et 59, qui représentent le nombre de minutes supplémentaires dans le décalage de fuseau horaire.  
-   \+ (plus) ou - (moins) est le signe obligatoire pour un décalage de fuseau horaire. Cela indique si le décalage de fuseau horaire est ajouté au temps UTC ou soustrait de celui-ci pour obtenir l’heure locale. La plage valide du décalage de fuseau horaire se situe entre -14:00 et +14:00.  
  
La plage des décalages de fuseau horaire respecte la norme XML W3C pour la définition de schéma XSD et est légèrement différente de la définition de la norme SQL 2003, 12:59 à +14:00.
  
Le paramètre de type facultatif *précision à la fraction de seconde* spécifie le nombre de chiffres pour la partie fractionnaire des secondes. Cette valeur peut être un entier avec 0 à 7 chiffres (100 nanosecondes). La *précision à la fraction de seconde* par défaut est 100 ns (sept chiffres pour la partie fractionnaire des secondes).
  
Les données sont stockées dans la base de données, puis traitées, comparées, triées et indexées sur le serveur comme au format UTC. Le décalage de fuseau horaire sera conservé dans la base de données pour récupération.
  
Le décalage de fuseau horaire donné sera supposé prendre en charge l’heure d’été et être réglé pour tout **datetime** donné situé dans la période d’observation de l’heure d’été.
  
Pour le type **datetimeoffset**, les valeurs **datetime** UTC et locale (pour le décalage de fuseau horaire persistant ou préservé) seront validées lors des opérations d’insertion, de mise à jour, arithmétique, de conversion ou d’attribution. La détection de toute valeur **datetime** UTC ou locale non valide (pour le décalage de fuseau horaire persistant ou préservé) déclenchera une erreur de valeur non valide. Par exemple, 9999-12-31 10:10:00 est valide au format UTC, mais déborde en heure locale sur le décalage de fuseau horaire +13:50.
  
Pour convertir une date en valeur **datetimeoffset** correspondante dans un fuseau horaire cible, consultez [AT TIME ZONE &#40;Transact-SQL&#41;](../../t-sql/queries/at-time-zone-transact-sql.md).
  
## <a name="ansi-and-iso-8601-compliance"></a>Conformité ANSI et ISO 8601  
Les sections relatives à la conformité ANSI et ISO 8601 des rubriques [date](../../t-sql/data-types/date-transact-sql.md) et [time](../../t-sql/data-types/time-transact-sql.md) s’appliquent à **datetimeoffset**.
  
## <a name="backward-compatibility-for-down-level-clients"></a>Compatibilité descendante pour les clients de bas niveau
Certains clients de bas niveau ne prennent pas en charge les types de données **time**, **date**, **datetime2** et **datetimeoffset**. Le tableau suivant présente le type de mappage entre une instance de haut niveau de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et des clients de bas niveau.
  
|Type de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|Format de littéral de chaîne par défaut passé au client de bas niveau|ODBC de bas niveau|OLEDB de bas niveau|JDBC de bas niveau|SQLCLIENT de bas niveau|  
|---|---|---|---|---|---|
|**time**|hh:mm:ss [.nnnnnnn]|SQL_WVARCHAR ou SQL_VARCHAR|DBTYPE_WSTR ou DBTYPE_STR|Java.sql.String|String ou SqString|  
|**date**|AAAA-MM-JJ|SQL_WVARCHAR ou SQL_VARCHAR|DBTYPE_WSTR ou DBTYPE_STR|Java.sql.String|String ou SqString|  
|**datetime2**|AAAA-MM-JJ hh:mm:ss[.nnnnnnn]|SQL_WVARCHAR ou SQL_VARCHAR|DBTYPE_WSTR ou DBTYPE_STR|Java.sql.String|String ou SqString|  
|**datetimeoffset**|AAAA-MM-JJ hh:mm:ss[.nnnnnnn] [+&#124;-]hh:mm|SQL_WVARCHAR ou SQL_VARCHAR|DBTYPE_WSTR ou DBTYPE_STR|Java.sql.String|String ou SqString|  
  
## <a name="converting-date-and-time-data"></a>Conversion de données date et time
Lorsque vous effectuez une conversion vers des types de données date et heure, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] rejette toutes les valeurs qu'il ne peut identifier comme dates ou heures. Pour plus d’informations sur l’utilisation des fonctions CAST et CONVERT avec des données date et time, consultez [CAST et CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)
  
### <a name="converting-datetimeoffset-data-type-to-other-date-and-time-types"></a>Conversion du type de données datetimeoffset en d’autres types date et time
Cette section décrit ce qui se produit quand un type de données **datetimeoffset** est converti en d’autres types de données date et time.
  
Dans le cas d’une conversion en **date**, l’année, le mois et le jour sont copiés. Le code suivant montre les résultats de la conversion d'une valeur `datetimeoffset(4)` en valeur `date`.  
  
```sql
DECLARE @datetimeoffset datetimeoffset(4) = '12-10-25 12:32:10 +01:00';  
DECLARE @date date= @datetimeoffset;  
  
SELECT @datetimeoffset AS '@datetimeoffset ', @date AS 'date';  
  
--Result  
--@datetimeoffset                date  
-------------------------------- ----------  
--2025-12-10 12:32:10.0000 +01:0 2025-12-10  
--  
--(1 row(s) affected)  
  
```  
  
Dans le cas d’une conversion en **time(n)**, l’heure, la minute, la seconde et les fractions de seconde sont copiées. La valeur de fuseau horaire est tronquée. Quand la précision de la valeur **datetimeoffset(n)** est supérieure à la précision de la valeur **time(n)**, la valeur est arrondie. Le code suivant montre les résultats de la conversion d'une valeur `datetimeoffset(4)` en valeur `time(3)`.
  
```sql
DECLARE @datetimeoffset datetimeoffset(4) = '12-10-25 12:32:10.1237 +01:0';  
DECLARE @time time(3) = @datetimeoffset;  
  
SELECT @datetimeoffset AS '@datetimeoffset ', @time AS 'time';  
  
--Result  
--@datetimeoffset                time  
-------------------------------- ------------  
-- 2025-12-10 12:32:10.1237 +01:00    12:32:10.124  
  
--  
--(1 row(s) affected)  
  
```  
  
Dans le cas d’une conversion en **datetime**, les valeurs de date et heure sont copiées et le fuseau horaire est tronqué. Quand la précision de fraction de la valeur **datetimeoffset(n)** est supérieure à trois chiffres, la valeur est tronquée. Le code suivant montre les résultats de la conversion d'une valeur `datetimeoffset(4)` en valeur `datetime`.
  
```sql
DECLARE @datetimeoffset datetimeoffset(4) = '12-10-25 12:32:10.1237 +01:0';  
DECLARE @datetime datetime = @datetimeoffset;  
  
SELECT @datetimeoffset AS '@datetimeoffset ', @datetime AS 'datetime';  
  
--Result  
--@datetimeoffset                datetime  
-------------------------------- -----------------------  
--2025-12-10 12:32:10.1237 +01:0 2025-12-10 12:32:10.123  
--  
--(1 row(s) affected)  
```  
  
Dans le cas d’une conversion en **smalldatetime**, la date et les heures sont copiées. Les minutes sont arrondies selon la valeur des secondes et les secondes sont définies sur 0. Le code suivant montre les résultats de la conversion d'une valeur `datetimeoffset(3)` en valeur `smalldatetime`.  
  
```sql
DECLARE @datetimeoffset datetimeoffset(3) = '1912-10-25 12:24:32 +10:0';  
DECLARE @smalldatetime smalldatetime = @datetimeoffset;  
  
SELECT @datetimeoffset AS '@datetimeoffset', @smalldatetime AS '@smalldatetime';  
  
--Result  
--@datetimeoffset                @smalldatetime  
-------------------------------- -----------------------  
--1912-10-25 12:24:32.000 +10:00 1912-10-25 12:25:00  
--  
--(1 row(s) affected)  
```  
  
Dans le cas d’une conversion en **datetime2(n)**, la date et l’heure sont copiées dans la valeur **datetime2** et le fuseau horaire est tronqué. Quand la précision de la valeur **datetime2(n)** est supérieure à la précision de la valeur **datetimeoffset(n)**, les fractions de seconde sont tronquées en conséquence. Le code suivant montre les résultats de la conversion d'une valeur `datetimeoffset(4)` en valeur `datetime2(3)`.
  
```sql
DECLARE @datetimeoffset datetimeoffset(4) = '1912-10-25 12:24:32.1277 +10:0';  
DECLARE @datetime2 datetime2(3)=@datetimeoffset;  
  
SELECT @datetimeoffset AS '@datetimeoffset', @datetime2 AS '@datetime2';  
  
--Result  
@datetimeoffset                    @datetime2  
---------------------------------- ----------------------  
1912-10-25 12:24:32.1277 +10:00    1912-10-25 12:24:32.12  
  
--(1 row(s) affected)  
```  
  
### <a name="converting-string-literals-to-datetimeoffset"></a>Conversion de littéraux de chaîne en datetimeoffset
Les conversions de littéraux de chaîne en types de date et d'heure sont autorisées si toutes les parties des chaînes sont dans des formats valides. Sinon, une erreur d'exécution est déclenchée. Les conversions implicites ou explicites qui ne spécifient pas de style à partir de types de date et d'heure en littéraux de chaîne seront au format par défaut de la session active. Le tableau suivant montre les règles de conversion d’un littéral de chaîne en type de données **datetimeoffset**.
  
|Littéral de chaîne d'entrée|**datetimeoffset(n)**|  
|---|---|
|ODBC DATE|Les littéraux de chaîne ODBC sont mappés au type de données **datetime**. Toute opération d’affectation de littéraux ODBC DATETIME dans des types **datetimeoffset** provoque une conversion implicite entre **datetime** et ce type, comme défini par les règles de conversion.|  
|ODBC TIME|Consultez la règle DATE ODBC précédente.|  
|ODBC DATETIME|Consultez la règle DATE ODBC précédente.|  
|DATE uniquement|La partie TIME a pour valeur par défaut 00:00:00. TIMEZONE a pour valeur par défaut +00:00.|  
|TIME uniquement|La partie DATE a pour valeur par défaut 1900-1-1. TIMEZONE aura pour valeur par défaut +00:00.|  
|TIMEZONE uniquement|Les valeurs par défaut sont fournies|  
|DATE + TIME|TIMEZONE a pour valeur par défaut +00:00.|  
|DATE + TIMEZONE|Non autorisé|  
|TIME + TIMEZONE|La partie DATE a pour valeur par défaut 1900-1-1.|  
|DATE + TIME + TIMEZONE|Simple|  
  
## <a name="examples"></a>Exemples  
L’exemple suivant compare les résultats de la conversion d’une chaîne en chaque type de données **date** et **time**.
  
```sql
SELECT   
     CAST('2007-05-08 12:35:29. 1234567 +12:15' AS time(7)) AS 'time'   
    ,CAST('2007-05-08 12:35:29. 1234567 +12:15' AS date) AS 'date'   
    ,CAST('2007-05-08 12:35:29.123' AS smalldatetime) AS   
        'smalldatetime'   
    ,CAST('2007-05-08 12:35:29.123' AS datetime) AS 'datetime'   
    ,CAST('2007-05-08 12:35:29.1234567+12:15' AS datetime2(7)) AS   
        'datetime2'  
    ,CAST('2007-05-08 12:35:29.1234567 +12:15' AS datetimeoffset(7)) AS   
        'datetimeoffset'  
    ,CAST('2007-05-08 12:35:29.1234567+12:15' AS datetimeoffset(7)) AS  
        'datetimeoffset IS08601';  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
|Type de données|Output|  
|---|---|
|**Time**|12:35:29. 1234567|  
|**Date**|2007-05-08|  
|**Smalldatetime**|2007-05-08 12:35:00|  
|**Datetime**|2007-05-08 12:35:29.123|  
|**datetime2**|2007-05-08 12:35:29. 1234567|  
|**Datetimeoffset**|2007-05-08 12:35:29.1234567 +12:15|  
  
## <a name="see-also"></a>Voir aussi
[CAST et CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)  
[AT TIME ZONE &#40;Transact-SQL&#41;](../../t-sql/queries/at-time-zone-transact-sql.md)
  
  
