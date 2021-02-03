---
description: CHAR (Transact-SQL)
title: CHAR (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/19/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- char_TSQL
- char
dev_langs:
- TSQL
helpviewer_keywords:
- converting int ASCII code to character
- control characters
- tab
- ASCII conversions
- CHAR function
- carriage return
- inserting control characters
- characters [SQL Server], control
- line feed
- printing ASCII values
ms.assetid: 955afe94-539c-465d-af22-16ec45da432a
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: a0d517921ce5a54294c7ee5663b2e56045818c76
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202193"
---
# <a name="char-transact-sql"></a>CHAR (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Cette fonction convertit un code ASCII **int** en une valeur de caractère.
  
![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
CHAR ( integer_expression )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
*integer_expression*  
Entier compris entre 0 et 255. `CHAR` renvoie une valeur `NULL` pour les expressions d’entier en dehors de cette plage, ou lorsque l'entier exprime seulement le premier octet d'un caractère à deux octets.

> [!NOTE]
> Certains jeux de caractères non européens, tels que [Shift Japanese Industrial Standards](https://www.wikipedia.org/wiki/Shift_JIS), incluent des caractères qui peuvent être représentés dans un système de codage à un octet, mais nécessitent un codage multioctets. Pour plus d’informations sur les jeux de caractères, reportez-vous à [Jeux de caractères codés sur un octet et multioctets](/cpp/c-runtime-library/single-byte-and-multibyte-character-sets). 
  
## <a name="return-types"></a>Types de retour
**char(1)**
  
## <a name="remarks"></a>Remarques  
Utilisez `CHAR` pour insérer des caractères de contrôle dans des chaînes de caractères. Ce tableau répertorie quelques-uns des caractères de contrôle les plus utilisés.
  
|Caractère de contrôle|Value|  
|---|---|
|Onglet|**char(9)**|  
|Saut de ligne|**char(10)**|  
|Retour chariot|**char(13)**|  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-using-ascii-and-char-to-print-ascii-values-from-a-string"></a>R. Utilisation d'ASCII et CHAR pour imprimer les valeurs ASCII d'une chaîne  
Cet exemple imprime la valeur et le caractère ASCII de chacun des caractères de la chaîne `New Moon`.
  
```sql
SET TEXTSIZE 0;  
-- Create variables for the character string and for the current   
-- position in the string.  
DECLARE @position INT, @string CHAR(8);  
-- Initialize the current position and the string variables.  
SET @position = 1;  
SET @string = 'New Moon';  
WHILE @position <= DATALENGTH(@string)  
   BEGIN  
   SELECT ASCII(SUBSTRING(@string, @position, 1)),   
      CHAR(ASCII(SUBSTRING(@string, @position, 1)))  
   SET @position = @position + 1  
   END;  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
----------- -
78          N  
----------- -  
101         e  
----------- -  
119         w  
----------- -  
32  
----------- -  
77          M  
----------- -  
111         o  
----------- -  
111         o  
----------- - 
110         n  
```
  
### <a name="b-using-char-to-insert-a-control-character"></a>B. Utilisation de CHAR pour l'insertion d'un caractère de contrôle  
Cet exemple utilise `CHAR(13)` pour imprimer le nom et l’adresse e-mail d’un employé sur des lignes distinctes, quand la requête retourne ses résultats sous forme de texte. Cet exemple utilise la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].
  
```sql
SELECT p.FirstName + ' ' + p.LastName, + CHAR(13)  + pe.EmailAddress   
FROM Person.Person p 
INNER JOIN Person.EmailAddress pe ON p.BusinessEntityID = pe.BusinessEntityID  
  AND p.BusinessEntityID = 1;  
GO  
```
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
Ken Sanchez
ken0@adventure-works.com
  
(1 row(s) affected)
```
  
### <a name="c-using-ascii-and-char-to-print-ascii-values-from-a-string"></a>C. Utilisation d'ASCII et CHAR pour imprimer les valeurs ASCII d'une chaîne  
Cet exemple suppose un jeu de caractères ASCII. Il retourne la valeur de caractère pour six nombres de caractère ASCII différents.
  
```sql
SELECT CHAR(65) AS [65], CHAR(66) AS [66],   
CHAR(97) AS [97], CHAR(98) AS [98],   
CHAR(49) AS [49], CHAR(50) AS [50];  
```
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
65   66   97   98   49   50  
---- ---- ---- ---- ---- ----  
A    B    a    b    1    2  
```
  
### <a name="d-using-char-to-insert-a-control-character"></a>D. Utilisation de CHAR pour l'insertion d'un caractère de contrôle  
Cet exemple utilise `CHAR(13)` pour retourner des informations à partir de sys.databases sur des lignes distinctes, quand la requête retourne ses résultats sous forme de texte.
  
```sql
SELECT name, 'was created on ', create_date, CHAR(13), name, 'is currently ', state_desc   
FROM sys.databases;  
GO  
```
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
name                                      create_date               name                                  state_desc  
--------------------------------------------------------------------------------------------------------------------  
master                    was created on  2003-04-08 09:13:36.390   master                  is currently  ONLINE 
tempdb                    was created on  2014-01-10 17:24:24.023   tempdb                  is currently  ONLINE   
AdventureWorksPDW2012     was created on  2014-05-07 09:05:07.083   AdventureWorksPDW2012   is currently  ONLINE 
```

### <a name="e-using-char-to-return-single-byte-characters"></a>E. Utilisation de CHAR pour retourner les caractères codés sur un octet  
Cet exemple utilise les valeurs d’entier et hexadécimales dans la plage valide pour ASCII. La fonction CHAR peut sortir les caractères japonais codés sur un octet.
  
```sql
SELECT CHAR(188) AS single_byte_representing_complete_character, 
  CHAR(0xBC) AS single_byte_representing_complete_character;  
GO  
```
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
single_byte_representing_complete_character single_byte_representing_complete_character
------------------------------------------- -------------------------------------------
ｼ                                           ｼ                                         
```

### <a name="f-using-char-to-return-multibyte-characters"></a>F. Utilisation de CHAR pour retourner les caractères multioctets  
Cet exemple utilise les valeurs d’entier et hexadécimales dans la plage valide pour ASCII. Toutefois, la fonction CHAR retourne la valeur NULL, car le paramètre représente uniquement le premier octet d’un caractère multioctet.
  
```sql
SELECT CHAR(129) AS first_byte_of_double_byte_character, 
  CHAR(0x81) AS first_byte_of_double_byte_character;  
GO  
```
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
first_byte_of_double_byte_character first_byte_of_double_byte_character
----------------------------------- -----------------------------------
NULL                                NULL                                         
```
  
## <a name="see-also"></a>Voir aussi
 [ASCII &#40;Transact-SQL&#41;](../../t-sql/functions/ascii-transact-sql.md)  
 [NCHAR &#40;Transact-SQL&#41;](../../t-sql/functions/nchar-transact-sql.md)  
 [UNICODE &#40;Transact-SQL&#41;](../../t-sql/functions/unicode-transact-sql.md)  
 [+ &#40;Concaténation de chaînes&#41; &#40;Transact-SQL&#41;](../../t-sql/language-elements/string-concatenation-transact-sql.md)  
 [Fonctions de chaîne &#40;Transact-SQL&#41;](../../t-sql/functions/string-functions-transact-sql.md)
  
  

