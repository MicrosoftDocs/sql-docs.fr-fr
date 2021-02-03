---
description: time (Transact-SQL)
title: time (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/07/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- time_TSQL
- time
dev_langs:
- TSQL
helpviewer_keywords:
- time [SQL Server], data types
- time [SQL Server]
- date and time [SQL Server], time
- data types [SQL Server], date and time
- time data type [SQL Server]
ms.assetid: 30a6c681-8190-48e4-94d0-78182290a402
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 7425622f808eccb983b73670b57ec3295de15fb3
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237345"
---
# <a name="time-transact-sql"></a>time (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Définit une heure d'un jour. L'heure ne prend pas en charge les fuseaux horaires et se présente au format 24 heures.  
  
  > [!NOTE]  
  > Des informations sur Informatica sont fournies pour les clients PDW utilisant le connecteur Informatica. 
  
## <a name="time-description"></a>Description de time  
  
|Property|Valeur|  
|--------------|-----------|  
|Syntaxe|**time** [ (*échelle de fractions de seconde*) ]|  
|Utilisation|DECLARE \@MyTime **time(7)**<br /><br /> CREATE TABLE Table1 ( Column1 **time(7)** )|  
|*échelle de fractions de seconde*|Spécifie le nombre de chiffres pour la partie fractionnaire des secondes.<br /><br /> Il peut s'agir d'un entier compris entre 0 et 7. Pour Informatica, il peut s’agir d’un entier compris entre 0 et 3.<br /><br /> L’échelle de fractions par défaut est 7 (100 ns).|  
|Format de littéral de chaîne par défaut<br /><br /> (utilisé pour le client de bas niveau)|hh:mm:ss[.nnnnnnn] pour Informatica)<br /><br /> Pour plus d’informations, consultez la section [Compatibilité descendante pour les clients de bas niveau](#BackwardCompatibilityforDownlevelClients).|  
|Plage|00:00:00.0000000 à 23:59:59.9999999 (00:00:00.000 à 23:59:59.999 pour Informatica)|  
|Plages d'éléments|hh comprend deux chiffres, entre 0 et 23, qui représentent l'heure.<br /><br /> mm comprend deux chiffres, entre 0 et 59, qui représentent la minute.<br /><br /> ss comprend deux chiffres, entre 0 et 59, qui représentent la seconde.<br /><br /> n\* comprend entre zéro et sept chiffres, entre 0 et 9999999, qui représentent les fractions de seconde. Pour Informatica, n\* comprend entre zéro et trois chiffres, entre 0 et 999.|  
|Longueur de caractère|8 positions au minimum (hh:mm:ss) à 16 au maximum (hh:mm:ss.nnnnnnn). Pour Informatica, 12 au maximum (hh:mm:ss.nnn).|  
|Précision, échelle<br /><br /> (l'utilisateur spécifie l'échelle uniquement)|Consultez le tableau ci-dessous.|  
|Taille de stockage|5 octets, fixes, sont la valeur par défaut avec une précision à la fraction de seconde de 100 ns par défaut. Dans Informatica, 4 octets, fixes, sont la valeur par défaut avec une précision à la fraction de seconde de 1 ms par défaut.|  
|Précision|100 nanosecondes (1 milliseconde dans Informatica)|  
|Valeur par défaut|00:00:00<br /><br /> Cette valeur est utilisée pour la partie heure ajoutée pour la conversion implicite de **date** en **datetime2** ou **datetimeoffset**.|  
|Précision à la fraction de seconde définie par l'utilisateur|Oui|  
|Prise en charge et conservation du décalage de fuseau horaire|Non|  
|Prise en charge de l'heure d'été|Non|  
  
|Échelle spécifiée|Résultat (précision, échelle)|Longueur de colonne (octets)|Fraction<br /><br /> secondes<br /><br /> précision|  
|---------------------|---------------------------------|-----------------------------|------------------------------------------|  
|**time**|(16,7) [(12,3) dans Informatica]|5 (4 dans Informatica)|7 (3 dans Informatica)|  
|**time(0)**|(8,0)|3|0-2|  
|**time(1)**|(10,1)|3|0-2|  
|**time(2)**|(11,2)|3|0-2|  
|**time(3)**|(12,3)|4|3-4|  
|**time(4)**<br /><br /> Pas de prise en charge dans Informatica.|(13,4)|4|3-4|  
|**time(5)**<br /><br /> Pas de prise en charge dans Informatica.|(14,5)|5|5-7|  
|**time(6)**<br /><br /> Pas de prise en charge dans Informatica.|(15,6)|5|5-7|  
|**time(7)**<br /><br /> Pas de prise en charge dans Informatica.|(16,7)|5|5-7|  
  
## <a name="supported-string-literal-formats-for-time"></a>Formats de littéraux de chaîne pris en charge pour l'heure  
 Le tableau suivant affiche les formats de littéraux de chaîne valides pour le type de données **time**.  
  
|SQL Server|Description|  
|----------------|-----------------|  
|hh:mm[:ss][:fractions de seconde][AM][PM]<br /><br /> hh:mm[:ss][.fractions de seconde][AM][PM]<br /><br /> hhAM[PM]<br /><br /> hh AM[PM]|La valeur d'heure 0 représente l'heure après minuit (AM), que l'élément AM soit ou non spécifié. PM ne peut pas être spécifié quand l'heure est égale à 0.<br /><br /> Les valeurs d'heure comprises entre 01 et 11 représentent les heures avant midi si ni AM ni PM n'est spécifié. Elles représentent les heures avant midi si AM est spécifié et les heures après midi si PM est spécifié.<br /><br /> La valeur 12 pour les heures représente l'heure qui démarre à midi si ni AM ni PM n'est spécifié. Elle représente l'heure qui démarre à minuit si AM est spécifié et l'heure qui démarre à midi si PM est spécifié. Par exemple, 12:01 correspond à 1 minute après midi, comme 12:01 PM, alors que 12:01 AM équivaut à 1 minute après minuit. La spécification de 12:01 AM équivaut à 00:01 ou 00:01 AM.<br /><br /> Les heures situées entre 13 et 23 représentent les heures après midi si ni AM ni PM n'est spécifié. Les valeurs représentent également les heures après midi si PM est spécifié. Vous ne pouvez pas spécifier AM lorsque la valeur d'heure est comprise entre 13 et 23.<br /><br /> Une valeur d'heure de 24 n'est pas valide. Pour représenter minuit, utilisez 12:00 AM ou 00:00.<br /><br /> Les millisecondes peuvent être précédées du signe deux-points (:) ou d'un point (.). Si le signe deux-points est utilisé, il s'agit de millièmes de secondes. Si le point est utilisé, un chiffre unique représente un dixième de seconde, deux chiffres un centième et trois chiffres un millième. Par exemple, 12:30:20:1 indique 20 secondes et un millième après 12:30 ; 12:30:20.1 indique 20 secondes et un dixième après 12:30.|  
  
|ISO 8601|Notes|  
|--------------|-----------|  
|hh:mm:ss<br /><br /> hh:mm[:ss][.fractions de seconde]|hh comprend deux chiffres, entre 0 et 23, qui représentent le nombre d'heures dans le décalage de fuseau horaire.<br /><br /> mm comprend deux chiffres, entre 0 et 59, qui représentent le nombre de minutes supplémentaires dans le décalage de fuseau horaire.|  
  
|ODBC|Notes|  
|----------|-----------|  
|{t 'hh:mm:ss[.fractions de seconde]'}|Spécifique à l'API ODBC.|  
  
## <a name="compliance-with-ansi-and-iso-8601-standards"></a>Conformité avec les normes ANSI et ISO 8601  
 L'utilisation de l'heure 24 pour représenter minuit et du saut de seconde au-delà de 59 comme défini par ISO 8601 (5.3.2 et 5.3) n'est pas prise en charge comme étant à compatibilité descendante et cohérente avec les types de date et d'heure existants.  
  
 Le format de littéral de chaîne par défaut (utilisé pour le client de bas niveau) s'alignera avec le format standard SQL qui est défini comme hh:mm:ss[.nnnnnnn]. Ce format ressemble à la définition ISO 8601 pour TIME à l'exclusion des fractions de seconde.  
  
##  <a name="backward-compatibility-for-down-level-clients"></a><a name="BackwardCompatibilityforDownlevelClients"></a> Compatibilité descendante pour les clients de bas niveau  
 Certains clients de bas niveau ne prennent pas en charge les types de données **time**, **date**, **datetime2** et **datetimeoffset**. Le tableau suivant présente le type de mappage entre une instance de haut niveau de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et des clients de bas niveau.  
  
|Type de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|Format de littéral de chaîne par défaut passé au client de bas niveau|ODBC de bas niveau|OLEDB de bas niveau|JDBC de bas niveau|SQLCLIENT de bas niveau|  
|-----------------------------------------|----------------------------------------------------------------|----------------------|-----------------------|----------------------|---------------------------|  
|**time**|hh:mm:ss [.nnnnnnn]|SQL_WVARCHAR ou SQL_VARCHAR|DBTYPE_WSTR ou DBTYPE_STR|Java.sql.String|String ou SqString|  
|**date**|AAAA-MM-JJ|SQL_WVARCHAR ou SQL_VARCHAR|DBTYPE_WSTR ou DBTYPE_STR|Java.sql.String|String ou SqString|  
|**datetime2**|AAAA-MM-JJ hh:mm:ss[.nnnnnnn]|SQL_WVARCHAR ou SQL_VARCHAR|DBTYPE_WSTR ou DBTYPE_STR|Java.sql.String|String ou SqString|  
|**datetimeoffset**|AAAA-MM-JJ hh:mm:ss[.nnnnnnn] [+&#124;-]hh:mm|SQL_WVARCHAR ou SQL_VARCHAR|DBTYPE_WSTR ou DBTYPE_STR|Java.sql.String|String ou SqString|  
  
## <a name="converting-date-and-time-data"></a>Conversion de données de date et d'heure  
 Lorsque vous effectuez une conversion vers des types de données date et heure, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] rejette toutes les valeurs qu'il ne peut identifier comme dates ou heures. Pour plus d’informations sur l’utilisation des fonctions CAST et CONVERT avec des données date et time, consultez [CAST et CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)  
  
### <a name="converting-timen-data-type-to-other-date-and-time-types"></a>Conversion de types de données time(n) vers d'autres types de date et d'heure  
 Cette section décrit ce qui se produit quand un type de données **time** est converti en d’autres types de données date et time.  
  
 Dans le cas d’une conversion en **time(n)**, les heures, minutes et secondes sont copiées. Lorsque la précision de destination est inférieure à la précision source, les fractions de seconde sont arrondies selon la précision de destination. L'exemple suivant montre les résultats de la conversion d'une valeur `time(4)` en valeur `time(3)`.  
  
```sql
DECLARE @timeFrom time(4) = '12:34:54.1237';  
DECLARE @timeTo time(3) = @timeFrom;  
  
SELECT @timeTo AS 'time(3)', @timeFrom AS 'time(4)';  
  
--Results  
--time(3)      time(4)  
-------------- -------------  
--12:34:54.124 12:34:54.1237  
--  
--(1 row(s) affected)  
```  
  
 Dans le cas d’une conversion en **date**, la conversion échoue et le message d’erreur 206 est généré : « Conflit de types d’opérandes : date est incompatible avec time ».  
  
 Dans le cas d’une conversion en **datetime**, les valeurs d’heures, de minutes et de secondes sont copiées et le composant date est défini sur « 1900-01-01 ». Quand la précision à la fraction de seconde de la valeur **time(n)** est supérieure à trois chiffres, le résultat **datetime** est tronqué. Le code suivant montre les résultats de la conversion d'une valeur `time(4)` en valeur `datetime`.  
  
```sql
DECLARE @time time(4) = '12:15:04.1237';  
DECLARE @datetime datetime= @time;  
SELECT @time AS '@time', @datetime AS '@datetime';  
  
--Result  
--@time         @datetime  
--------------- -----------------------  
--12:15:04.1237 1900-01-01 12:15:04.123  
--  
--(1 row(s) affected)  
  
```  
  
 Dans le cas d’une conversion en **smalldatetime**, la date est définie sur « 1900-01-01 » et les valeurs d’heures et de minutes sont arrondies. Les secondes et fractions de seconde sont définies sur 0. Le code suivant montre les résultats de la conversion d'une valeur `time(4)` en valeur `smalldatetime`.  
  
```sql
-- Shows rounding up of the minute value.  
DECLARE @time time(4) = '12:15:59.9999';   
DECLARE @smalldatetime smalldatetime= @time;    
SELECT @time AS '@time', @smalldatetime AS '@smalldatetime';   
  
--Result  
@time            @smalldatetime  
---------------- -----------------------  
12:15:59.9999    1900-01-01 12:16:00--  
--(1 row(s) affected)  
  
-- Shows rounding up of the hour value.  
DECLARE @time time(4) = '12:59:59.9999';   
DECLARE @smalldatetime smalldatetime= @time;    
  
SELECT @time AS '@time', @smalldatetime AS '@smalldatetime';  
@time            @smalldatetime  
---------------- -----------------------  
12:59:59.9999    1900-01-01 13:00:00  
  
(1 row(s) affected)  
  
```  
  
 Dans le cas d’une conversion en **datetimeoffset(n)**, la date est définie sur « 1900-01-01 » et l’heure est copiée. Le décalage de fuseau horaire est défini sur +00:00. Quand la précision à la fraction de seconde de la valeur **time(n)** est supérieure à la précision de la valeur **datetimeoffset(n)**, la valeur est arrondie en conséquence. L'exemple suivant montre les résultats de la conversion d'une valeur `time(4)` en type `datetimeoffset(3)`.  
  
```sql
DECLARE @time time(4) = '12:15:04.1237';  
DECLARE @datetimeoffset datetimeoffset(3) = @time;  
  
SELECT @time AS '@time', @datetimeoffset AS '@datetimeoffset';  
  
--Result  
--@time         @datetimeoffset  
--------------- ------------------------------  
--12:15:04.1237 1900-01-01 12:15:04.124 +00:00  
--  
--(1 row(s) affected)  
  
```  
  
 Dans le cas d’une conversion en **datetime2(n)**, la date est définie sur « 1900-01-01 », le composant heure est copié et le décalage de fuseau horaire est défini sur 00:00. Quand la précision à la fraction de seconde de la valeur **datetime2(n)** est supérieure à la valeur **time(n)**, la valeur est arrondie en conséquence.  L'exemple suivant montre les résultats de la conversion d'une valeur `time(4)` en valeur `datetime2(2)`.  
  
```sql
DECLARE @time time(4) = '12:15:04.1237';  
DECLARE @datetime2 datetime2(3) = @time;  
  
SELECT @datetime2 AS '@datetime2', @time AS '@time';  
  
--Result  
--@datetime2              @time  
------------------------- -------------  
--1900-01-01 12:15:04.124 12:15:04.1237  
--  
--(1 row(s) affected)  
```  
  
### <a name="converting-string-literals-to-timen"></a>Conversion de littéraux de chaîne en time(n)  
 Les conversions de littéraux de chaîne en types de date et d'heure sont autorisées si toutes les parties des chaînes sont dans des formats valides. Sinon, une erreur d'exécution est déclenchée. Les conversions implicites ou explicites qui ne spécifient pas de style à partir de types de date et d'heure en littéraux de chaîne seront au format par défaut de la session active. Le tableau suivant montre les règles de conversion d’un littéral de chaîne en type de données **time**.  
  
|Littéral de chaîne d'entrée|Règle de conversion|  
|--------------------------|---------------------|  
|ODBC DATE|Les littéraux de chaîne ODBC sont mappés au type de données **datetime**. Toute opération d’affectation de littéraux ODBC DATETIME dans des types **time** provoque une conversion implicite entre **datetime** et ce type, comme défini par les règles de conversion.|  
|ODBC TIME|Voir la règle DATE ODBC plus haut.|  
|ODBC DATETIME|Voir la règle DATE ODBC plus haut.|  
|DATE uniquement|Les valeurs par défaut sont fournies.|  
|TIME uniquement|Simple|  
|TIMEZONE uniquement|Les valeurs par défaut sont fournies.|  
|DATE + TIME|La partie TIME de la chaîne d'entrée est utilisée.|  
|DATE + TIMEZONE|Non autorisé.|  
|TIME + TIMEZONE|La partie TIME de la chaîne d'entrée est utilisée.|  
|DATE + TIME + TIMEZONE|La partie TIME du DATETIME local sera utilisée.|  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-comparing-date-and-time-data-types"></a>R. Comparaison de types de données de date et d'heure  
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
  
|Type de données|Output|  
|---------------|------------|  
|**time**|12:35:29. 1234567|  
|**date**|2007-05-08|  
|**smalldatetime**|2007-05-08 12:35:00|  
|**datetime**|2007-05-08 12:35:29.123|  
|**datetime2**|2007-05-08 12:35:29. 1234567|  
|**datetimeoffset**|2007-05-08 12:35:29.1234567 +12:15|  
  
###  <a name="b-inserting-valid-time-string-literals-into-a-time7-column"></a><a name="ExampleB"></a> B. Insertion de littéraux de chaîne d'heure valides dans une colonne time(7)  
 Le tableau suivant répertorie différents littéraux de chaîne qui peuvent être insérés dans une colonne de type de données **time(7)** avec les valeurs qui sont ensuite stockées dans cette colonne.  
  
|Type de format du littéral de chaîne|Littéral de chaîne inséré|Valeur time(7) qui est stockée|Description|  
|--------------------------------|-----------------------------|------------------------------------|-----------------|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|'01:01:01:123AM'|01:01:01.1230000|Lorsqu'un signe deux-points (:) précède la précision en fractions de seconde, l'échelle ne peut pas dépasser trois positions ou une erreur sera déclenchée.|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|'01:01:01.1234567 AM'|01:01:01.1234567|Lorsque l'élément AM ou PM est spécifié, l'heure est stockée au format 24 heures sans le littéral AM ou PM|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|'01:01:01.1234567 PM'|13:01:01.1234567|Lorsque l'élément AM ou PM est spécifié, l'heure est stockée au format 24 heures sans le littéral AM ou PM|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|'01:01:01.1234567PM'|13:01:01.1234567|Un espace avant AM ou PM est facultatif.|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|'01AM'|01:00:00.0000000|Lorsque seule l'heure est spécifiée, toutes les autres valeurs sont égales à 0.|  
|SQL Server|'01 AM'|01:00:00.0000000|Un espace avant AM ou PM est facultatif.|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|'01:01:01'|01:01:01.0000000|Lorsque la précision en fractions de seconde n'est pas spécifiée, chaque position qui est définie par le type de données est égale à 0.|  
|ISO 8601|'01:01:01.1234567'|01:01:01.1234567|Pour la conformité avec ISO 8601, utilisez le format 24 heures, et non AM ou PM.|  
|ISO 8601|'01:01:01.1234567 +01:01'|01:01:01.1234567|La différence de fuseau horaire facultative (TZD) est autorisée dans l'entrée, mais n'est pas stockée.|  
  
### <a name="c-inserting-time-string-literal-into-columns-of-each-date-and-time-date-type"></a>C. Insertion du littéral de chaîne d'heure dans les colonnes de chaque type de données de date et d'heure  
 Dans le tableau suivant, la première colonne affiche un littéral de chaîne d'heure à insérer dans une colonne de table de base de données du type de données de date et d'heure indiqué dans la deuxième colonne. La troisième colonne affiche la valeur qui sera stockée dans la colonne de table de base de données.  
  
|Littéral de chaîne inséré|Type de données de colonne|Valeur qui est stockée dans la colonne|Description|  
|-----------------------------|----------------------|------------------------------------|-----------------|  
|'12:12:12.1234567'|**time(7)**|12:12:12.1234567|Si la précision en fractions de seconde dépasse la valeur spécifiée pour la colonne, la chaîne sera tronquée sans erreur.|  
|'2007-05-07'|**date**|NULL|Toute valeur d'heure provoquera l'échec de l'instruction INSERT.|  
|'12:12:12'|**smalldatetime**|1900-01-01 12:12:00|Toute valeur de précision en fractions de seconde provoquera l'échec de l'instruction INSERT.|  
|'12:12:12.123'|**datetime**|1900-01-01 12:12:12.123|Toute précision de seconde supérieure à trois positions provoquera l'échec de l'instruction INSERT.|  
|'12:12:12.1234567'|**datetime2(7)**|1900-01-01 12:12:12.1234567|Si la précision en fractions de seconde dépasse la valeur spécifiée pour la colonne, la chaîne sera tronquée sans erreur.|  
|'12:12:12.1234567'|**datetimeoffset(7)**|1900-01-01 12:12:12.1234567 +00:00|Si la précision en fractions de seconde dépasse la valeur spécifiée pour la colonne, la chaîne sera tronquée sans erreur.|  
  
## <a name="see-also"></a>Voir aussi  
 [CAST et CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)  
  
  
