---
title: CHARINDEX (Transact-SQL) | Microsoft Docs
description: Référence Transact-SQL pour la fonction CHARINDEX.
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CHARINDEX
- CHARINDEX_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- expressions [SQL Server], pattern searching
- CHARINDEX function
- pattern searching [SQL Server]
- starting point of expression in character string
ms.assetid: 78c10341-8373-4b30-b404-3db20e1a3ac4
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 43c10d7f6f65d0171204869be1a5897ac00d22bf
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193452"
---
# <a name="charindex-transact-sql"></a>CHARINDEX (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Cette fonction recherche une expression de caractères à l’intérieur d’une deuxième expression de caractères, retournant la position de départ de la première expression si elle est trouvée.
  
![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
CHARINDEX ( expressionToFind , expressionToSearch [ , start_location ] )   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
*expressionToFind*  
[Expression](../../t-sql/language-elements/expressions-transact-sql.md) de caractères contenant la séquence à rechercher. *expressionToFind* a une limite de 8 000 caractères.
  
*expressionToSearch*  
Expression de caractères à rechercher.
  
*start_location*  
Expression de type **integer** ou **bigint** à laquelle la recherche commence. Si *start_location* n’est pas spécifiée, a une valeur négative ou est égale à 0, la recherche commence au début de *expressionToSearch*.
  
## <a name="return-types"></a>Types de retour
**bigint** si *expressionToSearch* a un type de données **nvarchar(max)** , **varbinary(max)** ou **varchar(max)** ; sinon, **int**.
  
## <a name="remarks"></a>Notes  
Si l’expression *expressionToFind* ou *expressionToSearch* a un type de données Unicode (**nchar** ou **nvarchar**) et que ce n’est pas le cas de l’autre expression, la fonction CHARINDEX convertit cette autre expression en un type de données Unicode. Vous ne pouvez pas utiliser CHARINDEX avec les types de données **text**, **ntext** ou **image**.
  
Si l’expression *expressionToFind* ou *expressionToSearch* a une valeur NULL, CHARINDEX retourne NULL.
  
Si CHARINDEX ne trouve pas *expressionToFind* dans *expressionToSearch*, CHARINDEX retourne 0.
  
CHARINDEX effectue des comparaisons basées sur le classement de l’entrée. Pour effectuer une comparaison dans un classement spécifié, utilisez COLLATE pour appliquer un classement explicite à l’entrée.
  
La position de départ retournée est basée sur la valeur 1, et non sur la valeur 0.
  
0x0000 (**char(0)** ) est un caractère non défini dans les classements Windows et ne peut pas être inclus dans CHARINDEX.
  
## <a name="supplementary-characters-surrogate-pairs"></a>Caractères supplémentaires (paires de substitution)  
Lors de l’utilisation de classements SC, *start_location* et la valeur renvoyée comptent les paires de substitution comme s’il s’agissait d’un seul caractère et non de deux. Pour plus d’informations, consultez [Prise en charge d’Unicode et du classement](../../relational-databases/collations/collation-and-unicode-support.md).
  
## <a name="examples"></a>Exemples  
  
### <a name="a-returning-the-starting-position-of-an-expression"></a>R. Retour de la position de départ d'une expression  
Cet exemple recherche `bicycle` dans la variable avec une valeur de chaîne `@document` où la recherche est effectuée.
  
```sql
DECLARE @document VARCHAR(64);  
SELECT @document = 'Reflectors are vital safety' +  
                   ' components of your bicycle.';  
SELECT CHARINDEX('bicycle', @document);  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
-----------   
48            
```  
  
### <a name="b-searching-from-a-specific-position"></a>B. Recherche à partir d'une position spécifique  
L’exemple suivant utilise le paramètre facultatif *start_location* pour commencer la recherche de `vital` à partir du cinquième caractère de variable avec une valeur de chaîne `@document` où la recherche est effectuée.
  
```sql
DECLARE @document VARCHAR(64);  
  
SELECT @document = 'Reflectors are vital safety' +  
                   ' components of your bicycle.';  
SELECT CHARINDEX('vital', @document, 5);  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
-----------   
16            
  
(1 row(s) affected)  
```  
  
### <a name="c-searching-for-a-nonexistent-expression"></a>C. Recherche d'une expression inexistante  
Cet exemple montre le jeu de résultats quand CHARINDEX ne trouve pas *expressionToFind* dans *expressionToSearch*.
  
```sql
DECLARE @document VARCHAR(64);  
  
SELECT @document = 'Reflectors are vital safety' +  
                   ' components of your bicycle.';  
SELECT CHARINDEX('bike', @document);  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
-----------
0
  
(1 row(s) affected)
```
  
### <a name="d-performing-a-case-sensitive-search"></a>D. Recherche respectant la casse  
Cet exemple montre une recherche avec respect de la casse de la chaîne `'TEST'` dans `'This is a Test``'`.
  
```sql
USE tempdb;  
GO  
--perform a case sensitive search  
SELECT CHARINDEX ( 'TEST',  
       'This is a Test'  
       COLLATE Latin1_General_CS_AS);  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
-----------
0
```  
  
Cet exemple montre une recherche avec respect de la casse de la chaîne `'Test'` dans `'This is a Test'`.
  
```sql
  
USE tempdb;  
GO  
SELECT CHARINDEX ( 'Test',  
       'This is a Test'  
       COLLATE Latin1_General_CS_AS);  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
-----------
11
```  
  
### <a name="e-performing-a-case-insensitive-search"></a>E. Recherche ne respectant pas la casse  
Cet exemple montre une recherche sans respect de la casse de la chaîne `'TEST'` dans `'This is a Test'`.
  
```sql
USE tempdb;  
GO  
SELECT CHARINDEX ( 'TEST',  
       'This is a Test'  
       COLLATE Latin1_General_CI_AS);  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
-----------
11
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Exemples : [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="f-searching-from-the-start-of-a-string-expression"></a>F. Recherche à partir du début d’une expression de chaîne  
Cet exemple retourne le premier emplacement de la chaîne `is` dans `This is a string`, en commençant à la position 1 (le premier caractère) de `This is a string`.
  
```sql
SELECT CHARINDEX('is', 'This is a string');  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
---------
3
```  
  
### <a name="g-searching-from-a-position-other-than-the-first-position"></a>G. Recherche à partir d’une position autre que la première position  
Cet exemple retourne le premier emplacement de la chaîne `is` dans `This is a string`, en commençant à la position 4 (le quatrième caractère).
  
```sql
SELECT CHARINDEX('is', 'This is a string', 4);  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
---------
 6
 ```  
  
### <a name="h-results-when-the-string-is-not-found"></a>H. Résultats quand la chaîne est introuvable  
Cet exemple montre la valeur de retour quand CHARINDEX ne trouve pas la chaîne *string_pattern* dans la chaîne où a lieu la recherche.
  
```sql
SELECT TOP(1) CHARINDEX('at', 'This is a string') FROM dbo.DimCustomer;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
---------
0
```  
  
## <a name="see-also"></a>Voir aussi
 [LEN &#40;Transact-SQL&#41;](../../t-sql/functions/len-transact-sql.md)  
 [PATINDEX &#40;Transact-SQL&#41;](../../t-sql/functions/patindex-transact-sql.md)  
 [Fonctions de chaîne &#40;Transact-SQL&#41;](../../t-sql/functions/string-functions-transact-sql.md)  
 [+ &#40;Concaténation de chaînes&#41; &#40;Transact-SQL&#41;](../../t-sql/language-elements/string-concatenation-transact-sql.md)  
 [Prise en charge d'Unicode et du classement](../../relational-databases/collations/collation-and-unicode-support.md)  
  
  


