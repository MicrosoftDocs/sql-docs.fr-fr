---
description: LEFT (Transact-SQL)
title: LEFT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/13/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- LEFT
- LEFT_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- character strings [SQL Server], LEFT
- characters [SQL Server], leftmost
- LEFT function
- leftmost character of expression
ms.assetid: 44a8c71b-63d8-458b-8b5d-99d570067c3c
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 4a30e01ddb227bafeac5913750945d7c610252e9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99188642"
---
# <a name="left-transact-sql"></a>LEFT (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Retourne la partie de gauche d'une chaîne de caractères avec le nombre spécifié de caractères.  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
LEFT ( character_expression , integer_expression )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *expression_caractère*  
 [Expression](../../t-sql/language-elements/expressions-transact-sql.md) de données binaires ou caractères. *character_expression* peut être une constante, une variable ou une colonne. *character_expression* peut être de n’importe quel type de données, à l’exception de **text** et **ntext**, qui peut être converti implicitement en **varchar** ou **nvarchar**. Sinon, utilisez la fonction [CAST](../../t-sql/functions/cast-and-convert-transact-sql.md) pour convertir explicitement *character_expression*.  
 
> [!NOTE]  
> Si *string_expression* est de type **binary** ou **varbinary**, LEFT effectuera une conversion implicite vers **varchar**, et ne préservera donc pas l’entrée binaire.  
  
 *integer_expression*  
 Entier positif qui spécifie le nombre de caractères de *character_expression* à renvoyer. Si *integer_expression* est négatif, une erreur est retournée. Si *integer_expression* est de type **bigint** et contient une valeur de grande taille, *character_expression* doit être d’un type de données de grande taille, tel que **varchar(max)** .  
  
 Le paramètre *integer_expression* compte un caractère de substitution UTF-16 comme un caractère.  
  
## <a name="return-types"></a>Types de retour  
 Retourne **varchar** quand *character_expression* est un type de données caractères non-Unicode.  
  
 Retourne **nvarchar** quand *character_expression* est un type de données caractères Unicode.  
  
## <a name="remarks"></a>Notes  
 Lors de l’utilisation de classements SC, le paramètre *integer_expression* compte une paire de substitution UTF-16 comme un caractère. Pour plus d’informations, consultez [Prise en charge d’Unicode et du classement](../../relational-databases/collations/collation-and-unicode-support.md).  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-using-left-with-a-column"></a>R. Utilisation de LEFT avec une colonne  
 L'exemple suivant retourne les cinq caractères les plus à gauche de chaque nom de produit dans la table `Product` de la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```sql  
SELECT LEFT(Name, 5)   
FROM Production.Product  
ORDER BY ProductID;  
GO  
```  
  
### <a name="b-using-left-with-a-character-string"></a>B. Utilisation de LEFT avec une chaîne de caractères  
 L'exemple suivant utilise `LEFT` pour retourner les deux caractères les plus à gauche de la chaîne de caractères `abcdefg`.  
  
```sql  
SELECT LEFT('abcdefg',2);  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
--   
ab   
  
(1 row(s) affected)  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Exemples : [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="c-using-left-with-a-column"></a>C. Utilisation de LEFT avec une colonne  
 L'exemple suivant retourne les cinq caractères les plus à gauche du nom de chaque produit.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT LEFT(EnglishProductName, 5)   
FROM dbo.DimProduct  
ORDER BY ProductKey;  
```  
  
### <a name="d-using-left-with-a-character-string"></a>D. Utilisation de LEFT avec une chaîne de caractères  
 L'exemple suivant utilise `LEFT` pour retourner les deux caractères les plus à gauche de la chaîne de caractères `abcdefg`.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT LEFT('abcdefg',2) FROM dbo.DimProduct;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
--   
ab  
```  
  
## <a name="see-also"></a>Voir aussi  
 [LTRIM &#40;Transact-SQL&#41;](../../t-sql/functions/ltrim-transact-sql.md)  
 [RIGHT &#40;Transact-SQL&#41;](../../t-sql/functions/right-transact-sql.md)  
 [RTRIM &#40;Transact-SQL&#41;](../../t-sql/functions/rtrim-transact-sql.md)  
 [STRING_SPLIT &#40;Transact-SQL&#41;](../../t-sql/functions/string-split-transact-sql.md)  
 [SUBSTRING &#40;Transact-SQL&#41;](../../t-sql/functions/substring-transact-sql.md)  
 [TRIM &#40;Transact-SQL&#41;](../../t-sql/functions/trim-transact-sql.md)  
 [CAST et CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)   
 [Types de données &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)   
 [Fonctions de chaîne &#40;Transact-SQL&#41;](../../t-sql/functions/string-functions-transact-sql.md)  
  
  

