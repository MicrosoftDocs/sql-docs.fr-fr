---
description: QUOTENAME (Transact-SQL)
title: QUOTENAME (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- QUOTENAME_TSQL
- QUOTENAME
dev_langs:
- TSQL
helpviewer_keywords:
- delimited identifiers [SQL Server]
- input strings [SQL Server]
- Unicode [SQL Server], delimited identifiers
- QUOTENAME function
- valid identifiers [SQL Server]
ms.assetid: 34d47f1e-2ac7-4890-8c9c-5f60f115e076
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 7043fd4eaa083c288eb2d1949c60b00d5ce7c41d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182370"
---
# <a name="quotename-transact-sql"></a>QUOTENAME (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Retourne une chaîne Unicode avec les délimiteurs ajoutés afin que la chaîne d'entrée soit un identificateur délimité [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] valide.  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
QUOTENAME ( 'character_string' [ , 'quote_character' ] )   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 '*character_string*'  
 Représente une chaîne de caractères au format Unicode. *character_string* est de type **sysname** et est limité à 128 caractères. Les entrées de plus de 128 caractères retournent une valeur NULL.  
  
 '*quote_character*'  
 Représente une chaîne d'un seul caractère à utiliser en tant que délimiteur. Il peut s’agir d’une apostrophe ( **'** ), d’un crochet droit ou gauche ( **[]** ), d’un guillemet double ( **"** ), d’une parenthèse gauche ou droite ( **()** ), d’un signe inférieur ou supérieur à ( **><** ), d’une accolade gauche ou droite ( **{}** ) ou d’un accent grave ( **\`** ). La valeur NULL est retournée si un caractère non autorisé est fourni. Si *quote_character* n’est pas spécifié, les crochets sont utilisés.  
  
## <a name="return-types"></a>Types de retour  
 **nvarchar(258)**  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant prend la chaîne de caractères `abc[]def` et utilise les caractères `[``]` pour créer un identificateur délimité [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] valide.  
  
```sql
SELECT QUOTENAME('abc[]def');
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
[abc[]]def]
  
(1 row(s) affected)  
```  
  
 Vous remarquerez que dans la chaîne `abc[]def`, le crochet de droite est doublé pour indiquer un caractère d'échappement.  
 
 L’exemple suivant prépare une chaîne entre guillemets à utiliser pour nommer une colonne.  
  
```sql
DECLARE @columnName NVARCHAR(255)='user''s "custom" name'
DECLARE @sql NVARCHAR(MAX) = 'SELECT FirstName AS ' + QUOTENAME(@columnName) + ' FROM dbo.DimCustomer'

EXEC sp_executesql @sql
```
  
## <a name="examples-sssdwfull-and-sspdw"></a>Exemples : [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
 L'exemple suivant prend la chaîne de caractères `abc def` et utilise les caractères `[``]` pour créer un identificateur délimité [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] valide.  
  
```sql
SELECT QUOTENAME('abc def');   
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
[abc def]  
  
(1 row(s) affected)  
```  
  
## <a name="see-also"></a>Voir aussi  
 [PARSENAME &#40;Transact-SQL&#41;](../../t-sql/functions/parsename-transact-sql.md)  
 [CONCAT &#40;Transact-SQL&#41;](../../t-sql/functions/concat-transact-sql.md)  
 [CONCAT_WS &#40;Transact-SQL&#41;](../../t-sql/functions/concat-ws-transact-sql.md)  
 [FORMATMESSAGE &#40;Transact-SQL&#41;](../../t-sql/functions/formatmessage-transact-sql.md)  
 [REPLACE &#40;Transact-SQL&#41;](../../t-sql/functions/replace-transact-sql.md)  
 [REVERSE &#40;Transact-SQL&#41;](../../t-sql/functions/reverse-transact-sql.md)  
 [STRING_AGG &#40;Transact-SQL&#41;](../../t-sql/functions/string-agg-transact-sql.md)  
 [STRING_ESCAPE &#40;Transact-SQL&#41;](../../t-sql/functions/string-escape-transact-sql.md)  
 [STUFF &#40;Transact-SQL&#41;](../../t-sql/functions/stuff-transact-sql.md)  
 [TRANSLATE &#40;Transact-SQL&#41;](../../t-sql/functions/translate-transact-sql.md)  
 [Fonctions de chaîne &#40;Transact-SQL&#41;](../../t-sql/functions/string-functions-transact-sql.md)  
