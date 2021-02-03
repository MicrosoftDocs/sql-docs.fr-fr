---
description: PRINT (Transact-SQL)
title: PRINT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- PRINT_TSQL
- PRINT
dev_langs:
- TSQL
helpviewer_keywords:
- PRINT statement
- user-defined messages [SQL Server]
- messages [SQL Server], PRINT statement
- displaying user-defined messages
- viewing user-defined messages
- conditionally returning messages [SQL Server]
ms.assetid: 32ba0729-c4b5-4cfb-a5aa-e8b9402be028
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 472c7c66d39c32ee87200603d2ff39b230a206aa
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210189"
---
# <a name="print-transact-sql"></a>PRINT (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Retourne un message défini par l'utilisateur au client.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql  
PRINT msg_str | @local_variable | string_expr  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *msg_str*  
 Chaîne de caractères ou constante de chaîne Unicode. Pour plus d’informations, consultez [Constantes &#40;Transact-SQL&#41;](../../t-sql/data-types/constants-transact-sql.md).  
  
 **@** *local_variable*  
 Variable de tout type de données caractères valide. **@** _local\_variable_ doit être de type **char**, **nchar**, **varchar** ou **nvarchar**, ou il doit pouvoir être implicitement converti dans ces types de données.  
  
 *string_expr*  
 Expression qui retourne une chaîne. Cet argument peut inclure des valeurs littérales concaténées, des fonctions et des variables. Pour plus d’informations, consultez [Expressions &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md).  
  
## <a name="remarks"></a>Remarques  
 Une chaîne de message peut contenir jusqu'à 8 000 caractères s'il s'agit d'une chaîne non-Unicode, et 4 000 caractères s'il s'agit d'une chaîne Unicode. Les chaînes plus longues sont tronquées. Les types de données **varchar(max)** et **nvarchar(max)** sont tronqués en types de données qui ne dépassent pas **varchar(8000)** et **nvarchar(4000)**.  
  
 L'instruction RAISERROR peut également être utilisée pour retourner des messages. Elle présente trois avantages par rapport à l'instruction PRINT :  
  
-   RAISERROR prend en charge la substitution des arguments dans un message d'erreur, grâce un mécanisme semblable à la fonction printf de la bibliothèque standard en langage C.  
  
-   RAISERROR peut spécifier un numéro d'erreur unique, une gravité et un code d'état en plus du message textuel.  
  
-   RAISERROR peut être utilisée pour retourner des messages définis par l'utilisateur, tels qu'ils sont créés par la procédure stockée système sp_addmessage.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-conditionally-executing-print-if-exists"></a>R. Impression sous condition (IF EXISTS)  
 L'exemple suivant utilise l'instruction `PRINT` pour retourner un message sous condition.  
  
```sql  
IF @@OPTIONS & 512 <> 0  
    PRINT N'This user has SET NOCOUNT turned ON.';  
ELSE  
    PRINT N'This user has SET NOCOUNT turned OFF.';  
GO  
```  
  
### <a name="b-building-and-displaying-a-string"></a>B. Construction et affichage d'une chaîne  
 L'exemple suivant convertit les résultats de la fonction `GETDATE` dans le type `nvarchar` et le concatène avec un texte littéral à retourner à l'aide de `PRINT`.  
  
```sql  
-- Build the message text by concatenating  
-- strings and expressions.  
PRINT N'This message was printed on '  
    + RTRIM(CAST(GETDATE() AS NVARCHAR(30)))  
    + N'.';  
GO  
-- This example shows building the message text  
-- in a variable and then passing it to PRINT.  
-- This was required in SQL Server 7.0 or earlier.  
DECLARE @PrintMessage NVARCHAR(50);  
SET @PrintMessage = N'This message was printed on '  
    + RTRIM(CAST(GETDATE() AS NVARCHAR(30)))  
    + N'.';  
PRINT @PrintMessage;  
GO  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Exemples : [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="c-conditionally-executing-print"></a>C. Impression sous condition  
 L'exemple suivant utilise l'instruction `PRINT` pour retourner un message sous condition.  
  
```sql  
IF DB_ID() = 1  
    PRINT N'The current database is master.';  
ELSE  
    PRINT N'The current database is not master.';  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Types de données &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)   
 [DECLARE @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/declare-local-variable-transact-sql.md)   
 [RAISERROR &#40;Transact-SQL&#41;](../../t-sql/language-elements/raiserror-transact-sql.md)  
  
  

