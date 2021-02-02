---
description: RIGHT (Transact-SQL)
title: RIGHT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/13/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- RIGHT_TSQL
- RIGHT
dev_langs:
- TSQL
helpviewer_keywords:
- rightmost character of expression
- RIGHT function
- character strings [SQL Server], RIGHT
ms.assetid: 43f1fe1f-aa18-47e3-ba20-e03e32254a6d
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: da0e2e1224cce5dfe13da57eb0efb7959ae8c4ec
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99101582"
---
# <a name="right-transact-sql"></a>RIGHT (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Retourne la partie de droite d'une chaîne de caractères avec le nombre spécifié de caractères.  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
RIGHT ( character_expression , integer_expression )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *expression_caractère*  
 [Expression](../../t-sql/language-elements/expressions-transact-sql.md) de données binaires ou caractères. *character_expression* peut être une constante, une variable ou une colonne. *character_expression* peut être de n’importe quel type de données, à l’exception de **text** et **ntext**, qui peut être converti implicitement en **varchar** ou **nvarchar**. Sinon, utilisez la fonction [CAST](../../t-sql/functions/cast-and-convert-transact-sql.md) pour convertir explicitement *character_expression*.  
   
> [!NOTE]  
> Si *string_expression* est de type **fixe** ou **variable**, RIGHT effectuera une conversion implicite vers **varchar**, et ne préservera donc pas l'entrée fixe.  
  
 *integer_expression*  
 Entier positif qui spécifie le nombre de caractères de *character_expression* à retourner. Si *integer_expression* est négatif, une erreur est retournée. Si *integer_expression* est de type **bigint** et contient une valeur de grande taille, *character_expression* doit être d’un type de données de grande taille, tel que **varchar(max)** .  
  
## <a name="return-types"></a>Types de retour  
 Retourne **varchar** quand *character_expression* est un type de données caractères non-Unicode.  
  
 Retourne **nvarchar** quand *character_expression* est un type de données caractères Unicode.  
  
## <a name="supplementary-characters-surrogate-pairs"></a>Caractères supplémentaires (paires de substitution)  
 Lors de l'utilisation de classements SC, la fonction RIGHT compte une paire de substitution UTF-16 comme un caractère unique. Pour plus d’informations, consultez [Prise en charge d’Unicode et du classement](../../relational-databases/collations/collation-and-unicode-support.md).  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-using-right-with-a-column"></a>A : Utilisation de RIGHT avec une colonne  
 L'exemple suivant retourne les cinq derniers caractères les plus à droite du prénom de chaque personne dans la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```sql  
SELECT RIGHT(FirstName, 5) AS 'First Name'  
FROM Person.Person  
WHERE BusinessEntityID < 5  
ORDER BY FirstName;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
First Name  
----------  
Ken  
Terri  
berto  
Rob  
  
(4 row(s) affected)  
  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Exemples : [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="b-using-right-with-a-column"></a>B. Utilisation de RIGHT avec une colonne  
 L’exemple suivant retourne les cinq caractères les plus à droite de chaque nom de famille dans la table `DimEmployee`.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT RIGHT(LastName, 5) AS Name  
FROM dbo.DimEmployee  
ORDER BY EmployeeKey;  
```  
  
 Voici un jeu de résultats partiel.  
  
 ```
Name
-----
lbert
Brown
rello
lters
 ```  
  
### <a name="c-using-right-with-a-character-string"></a>C. Utilisation de RIGHT avec une chaîne de caractères  
 L’exemple suivant utilise `RIGHT` pour retourner les deux caractères les plus à droite de la chaîne de caractères `abcdefg`.  
  
```sql  
SELECT RIGHT('abcdefg', 2); 
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```
-------  
fg
```  
  
## <a name="see-also"></a>Voir aussi  
 [LEFT &#40;Transact-SQL&#41;](../../t-sql/functions/left-transact-sql.md)  
 [LTRIM &#40;Transact-SQL&#41;](../../t-sql/functions/ltrim-transact-sql.md)  
 [RTRIM &#40;Transact-SQL&#41;](../../t-sql/functions/rtrim-transact-sql.md)  
 [STRING_SPLIT &#40;Transact-SQL&#41;](../../t-sql/functions/string-split-transact-sql.md)  
 [SUBSTRING &#40;Transact-SQL&#41;](../../t-sql/functions/substring-transact-sql.md)  
 [TRIM &#40;Transact-SQL&#41;](../../t-sql/functions/trim-transact-sql.md)  
 [CAST et CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)   
 [Types de données &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)   
 [Fonctions de chaîne &#40;Transact-SQL&#41;](../../t-sql/functions/string-functions-transact-sql.md)  
  
  

