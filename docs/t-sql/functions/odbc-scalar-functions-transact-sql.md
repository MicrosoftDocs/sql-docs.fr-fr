---
description: Fonctions scalaires ODBC (Transact-SQL)
title: Fonctions scalaires ODBC (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- CURDATE ODBC function
- DAYOFMONTH ODBC function
- WEEK ODBC function
- functions, ODBC CONCAT
- SECOND ODBC function
- functions, ODBC CURRENT_TIME
- functions, ODBC HOUR
- functions, ODBC QUARTER
- TRUNCATE ODBC function
- functions, ODBC SECOND
- DAYOFWEEK ODBC function
- CURRENT_DATE ODBC function
- DAYNAME ODBC function
- CURTIME ODBC function
- functions, ODBC CURRENT_DATE
- MINUTE ODBC function
- functions, ODBC BIT_LENGTH
- functions, ODBC OCTET_LENGTH
- CONCAT ODBC function
- MONTHNAME ODBC function
- functions, ODBC DAYOFYEAR
- OCTET_LENGTH ODBC function
- functions [SQL Server], ODBC scalar functions
- BIT_LENGTH ODBC function
- functions, ODBC DAYOFMONTH
- CURRENT_TIME ODBC function
- functions, ODBC CURDATE
- functions, ODBC CURTIME
- functions, ODBC TRUNCATE
- functions, ODBC MONTHNAME
- functions, ODBC DAYNAME
- ODBC scalar functions
- functions, ODBC MINUTE
- functions, ODBC DAYOFWEEK
- QUARTER ODBC function
- DAYOFYEAR ODBC function
- functions, ODBC WEEK
- HOUR ODBC function
ms.assetid: a0df1ac2-6699-4ac0-8f79-f362f23496f1
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: fb9cc0e305334fa15bdbdb8b61eafc82f8360c3b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340665"
---
# <a name="odbc-scalar-functions-transact-sql"></a>Fonctions scalaires ODBC (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Vous pouvez utiliser les [fonctions scalaires ODBC](../../odbc/reference/appendixes/appendix-e-scalar-functions.md) dans les instructions [!INCLUDE[tsql](../../includes/tsql-md.md)]. Ces instructions sont interprétées par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Elles peuvent être utilisées dans les procédures stockées et les fonctions définies par l'utilisateur. Celles-ci incluent les fonctions de chaîne, numériques, d'heure, de date, d'intervalle et système.  
  
## <a name="usage"></a>Utilisation  
 ```syntaxsql
 SELECT {fn <function_name> [ (<argument>,....n) ] }
 ```
  
## <a name="functions"></a>Fonctions  
 Les tableaux suivants listent les fonctions scalaires ODBC qui ne sont pas dupliquées dans [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
### <a name="string-functions"></a>Fonctions de chaîne  
  
|Fonction|Description|  
|--------------|-----------------|  
|BIT_LENGTH( string_exp ) (ODBC 3.0)|Retourne la longueur en bits de l'expression de chaîne.<br /><br /> Retourne la taille interne du type de données indiqué, sans convertir string_exp en chaîne.|  
|CONCAT( string_exp1,string_exp2) (ODBC 1.0)|Retourne une chaîne de caractères qui est le résultat de la concaténation de string_exp2 à string_exp1. La chaîne résultante dépend de SGBD. Par exemple, si la colonne représentée par string_exp1 contenait une valeur NULL, DB2 retournerait NULL, mais [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] retournerait la chaîne non NULL.|  
|OCTET_LENGTH( string_exp ) (ODBC 3.0)|Retourne la longueur en octets de l'expression de chaîne. Le résultat est le plus petit entier qui n'est pas inférieur au nombre de bits divisé par 8.<br /><br /> Retourne la taille interne du type de données indiqué, sans convertir string_exp en chaîne.|  
  
### <a name="numeric-function"></a>Fonction numérique  
  
|Fonction|Description|  
|--------------|-----------------|  
|TRUNCATE( numeric_exp, integer_exp) (ODBC 2.0)|Retourne les positions numeric_exp tronquées en integer_exp à droite de la virgule décimale. Si integer_exp est négatif, les positions numeric_exp sont tronquées en &#124;integer_exp&#124; à gauche de la virgule décimale.|  
  
### <a name="time-date-and-interval-functions"></a>Fonctions d'heure, de date et d'intervalle  
  
|Fonction|Description|  
|--------------|-----------------|  
|CURRENT_DATE( ) (ODBC 3.0)|Retourne la date du jour.|  
|CURDATE( ) (ODBC 3.0)|Retourne la date du jour.|  
|CURRENT_TIME`[( time-precision )]` (ODBC 3.0)|Retourne l'heure locale actuelle. L'argument time-precision détermine la précision en secondes de la valeur retournée|  
|CURTIME() (ODBC 3.0)|Retourne l'heure locale actuelle.|  
|DAYNAME( date_exp ) (ODBC 2.0)|Retourne une chaîne de caractères qui contient le nom spécifique à la source de données du jour pour la partie jour de date_exp. Par exemple, le nom est dimanche à samedi ou dim à sam pour une source de données qui utilise le français. Le nom est Sonntag à Samstag pour une source de données qui utilise l’allemand.|
|DAYOFMONTH( date_exp ) (ODBC 1.0)|Retourne le jour du mois basé sur le champ du mois dans date_exp comme valeur entière. La valeur de retour est comprise dans la plage 1 à 31.|  
|DAYOFWEEK( date_exp ) (ODBC 1.0)|Retourne le jour de la semaine basé sur le champ de la semaine dans date_exp comme valeur entière. La valeur de retour est comprise dans la plage 1 à 7, où 1 représente dimanche.|  
|HOUR( time_exp ) (ODBC 1.0)|Retourne l’heure basée sur le champ de l’heure dans time_exp comme valeur entière dans la plage 0 à 23.|  
|MINUTE( time_exp ) (ODBC 1.0)|Retourne la minute basée sur le champ de minute dans time_exp comme valeur entière dans la plage 0 à 59.|  
|SECOND( time_exp ) (ODBC 1.0)|Retourne la seconde basée sur le champ de seconde dans time_exp comme valeur entière dans la plage 0 à 59.|  
|MONTHNAME( date_exp ) (ODBC 2.0)|Retourne une chaîne de caractères qui contient le nom spécifique à la source de données du mois pour la partie mois de date_exp. Par exemple, le nom est janvier à décembre ou jan à déc pour une source de données qui utilise le français. Le nom est Januar à Dezember pour une source de données qui utilise l’allemand.|  
|QUARTER( date_exp ) (ODBC 1.0)|Retourne le trimestre dans date_exp comme valeur entière dans la plage 1 à 4, où 1 représente la période du 1er janvier au 31 mars.|  
|WEEK( date_exp ) (ODBC 1.0)|Retourne la semaine de l’année basée sur le champ de la semaine dans date_exp comme valeur entière dans la plage 1 à 53.|  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-using-an-odbc-function-in-a-stored-procedure"></a>R. Utilisation d'une fonction ODBC dans une procédure stockée  
 L'exemple ci-dessous utilise une fonction ODBC dans une procédure stockée.  
  

```sql 
CREATE PROCEDURE dbo.ODBCprocedure  
(  
    @string_exp NVARCHAR(4000)  
)  
AS  
SELECT {fn OCTET_LENGTH( @string_exp )};  
```  
  
### <a name="b-using-an-odbc-function-in-a-user-defined-function"></a>B. Utilisation d'une fonction ODBC dans une fonction définie par l'utilisateur  
 L'exemple ci-dessous utilise une fonction ODBC dans une fonction définie par l'utilisateur :  
  
```sql  
CREATE FUNCTION dbo.ODBCudf  
(  
    @string_exp NVARCHAR(4000)  
)  
RETURNS INT  
AS  
BEGIN  
DECLARE @len INT  
SET @len = (SELECT {fn OCTET_LENGTH( @string_exp )})  
RETURN(@len)  
END ;  
  
SELECT dbo.ODBCudf('Returns the length.');  
--Returns 38  
```  
  
### <a name="c-using-an-odbc-functions-in-select-statements"></a>C. Utilisation de fonctions ODBC dans des instructions SELECT  
 Les instructions SELECT suivantes utilisent des fonctions ODBC :  
  
```sql 
DECLARE @string_exp NVARCHAR(4000) = 'Returns the length.';  
SELECT {fn BIT_LENGTH( @string_exp )};  
-- Returns 304  
SELECT {fn OCTET_LENGTH( @string_exp )};  
-- Returns 38  
  
SELECT {fn CONCAT( 'CONCAT ','returns a character string')};  
-- Returns CONCAT returns a character string  
SELECT {fn TRUNCATE( 100.123456, 4)};  
-- Returns 100.123400  
SELECT {fn CURRENT_DATE( )};  
-- Returns 2007-04-20  
SELECT {fn CURRENT_TIME(6)};  
-- Returns 10:27:11.973000  
  
DECLARE @date_exp NVARCHAR(30) = '2007-04-21 01:01:01.1234567';  
SELECT {fn DAYNAME( @date_exp )};  
-- Returns Saturday  
SELECT {fn DAYOFMONTH( @date_exp )};  
-- Returns 21  
SELECT {fn DAYOFWEEK( @date_exp )};  
-- Returns 7  
SELECT {fn HOUR( @date_exp)};  
-- Returns 1   
SELECT {fn MINUTE( @date_exp )};  
-- Returns 1  
SELECT {fn SECOND( @date_exp )};  
-- Returns 1  
SELECT {fn MONTHNAME( @date_exp )};  
-- Returns April  
SELECT {fn QUARTER( @date_exp )};  
-- Returns 2  
SELECT {fn WEEK( @date_exp )};  
-- Returns 16  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Exemples : [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="d-using-an-odbc-function-in-a-stored-procedure"></a>D. Utilisation d'une fonction ODBC dans une procédure stockée  
 L'exemple ci-dessous utilise une fonction ODBC dans une procédure stockée.  
  
```sql  
CREATE PROCEDURE dbo.ODBCprocedure  
(  
    @string_exp NVARCHAR(4000)  
)  
AS  
SELECT {fn BIT_LENGTH( @string_exp )};  
```  
  
### <a name="e-using-an-odbc-function-in-a-user-defined-function"></a>E. Utilisation d'une fonction ODBC dans une fonction définie par l'utilisateur  
 L'exemple ci-dessous utilise une fonction ODBC dans une fonction définie par l'utilisateur :  
  
```sql  
CREATE FUNCTION dbo.ODBCudf  
(  
    @string_exp NVARCHAR(4000)  
)  
RETURNS INT  
AS  
BEGIN  
DECLARE @len INT  
SET @len = (SELECT {fn BIT_LENGTH( @string_exp )})  
RETURN(@len)  
END ;  
  
SELECT dbo.ODBCudf('Returns the length in bits.');  
--Returns 432  
```  
  
### <a name="f-using-an-odbc-functions-in-select-statements"></a>F. Utilisation de fonctions ODBC dans des instructions SELECT  
 Les instructions SELECT suivantes utilisent des fonctions ODBC :  
  
```sql  
DECLARE @string_exp NVARCHAR(4000) = 'Returns the length.';  
SELECT {fn BIT_LENGTH( @string_exp )};  
-- Returns 304  
  
SELECT {fn CONCAT( 'CONCAT ','returns a character string')};  
-- Returns CONCAT returns a character string  
SELECT {fn CURRENT_DATE( )};  
-- Returns today's date  
SELECT {fn CURRENT_TIME(6)};  
-- Returns the time  
  
DECLARE @date_exp NVARCHAR(30) = '2007-04-21 01:01:01.1234567';  
SELECT {fn DAYNAME( @date_exp )};  
-- Returns Saturday  
SELECT {fn DAYOFMONTH( @date_exp )};  
-- Returns 21  
SELECT {fn DAYOFWEEK( @date_exp )};  
-- Returns 7  
SELECT {fn HOUR( @date_exp)};  
-- Returns 1   
SELECT {fn MINUTE( @date_exp )};  
-- Returns 1  
SELECT {fn SECOND( @date_exp )};  
-- Returns 1  
SELECT {fn MONTHNAME( @date_exp )};  
-- Returns April  
SELECT {fn QUARTER( @date_exp )};  
-- Returns 2  
SELECT {fn WEEK( @date_exp )};  
-- Returns 16  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions intégrées &#40;Transact-SQL&#41;](~/t-sql/functions/functions.md)