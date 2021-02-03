---
description: THROW (Transact-SQL)
title: THROW (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- THROW_TSQL
- THROW
dev_langs:
- TSQL
helpviewer_keywords:
- THROW statement
ms.assetid: 43661b89-8f13-4480-ad53-70306cbb14c5
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: a9be1b4243009bbc4ca99fe9bdf9bfcd94d72c45
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182307"
---
# <a name="throw-transact-sql"></a>THROW (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Lève une exception et transfère l’exécution à un bloc CATCH d’une construction TRY...CATCH dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)].  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
THROW [ { error_number | @local_variable },  
        { message | @local_variable },  
        { state | @local_variable } ]   
[ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *error_number*  
 Constante ou variable qui représente l'exception. *error_number* est **int** et doit être supérieur ou égal à 50000 et inférieur ou égal à 2147483647.  
  
 *message*  
 Chaîne ou variable qui décrit l’exception. *message* est **nvarchar(2048)** .  
  
 *state*  
 Constante ou variable comprise entre 0 et 255 qui indique l'état à associer au message. *state* est **tinyint**.  
  
## <a name="remarks"></a>Notes  
 L'instruction qui précède l'instruction THROW doit être suivie du terminateur d'instruction, le point-virgule (;).  
  
 Si une construction TRY...CATCH n’est pas disponible, le lot d’instructions se termine. Le numéro de ligne et la procédure où l'exception est levée sont définis. La gravité est définie sur 16.  
  
 Si l'instruction THROW est spécifiée sans paramètres, elle doit apparaître à l'intérieur d'un bloc CATCH. Cela provoque la levée de l'exception interceptée. Toute erreur qui se produit dans une instruction THROW entraîne la fin du lot d’instructions.  
  
 % est un caractère réservé dans le texte du message d'une instruction THROW et doit être placé dans une séquence d'échappement. Doublez le caractère % pour retourner % en tant que partie du texte du message, par exemple « L'augmentation a dépassé de 15 %% la valeur d'origine. »  
  
## <a name="differences-between-raiserror-and-throw"></a>Différences entre RAISERROR et THROW  
 Le tableau suivant répertorie quelques-unes des différences entre les instructions RAISERROR et THROW.  
  
|RAISERROR (instruction)|Instruction THROW|  
|-------------------------|---------------------|  
|Si un *msg_id* est passé à RAISERROR, l’ID doit être défini dans sys.messages.|Le paramètre *error_number* ne doit pas être défini dans sys.messages.|  
|Le paramètre *msg_str* peut contenir des styles de mise en forme **printf**.|Le paramètre *message* n’accepte pas la mise en forme du style **printf**.|  
|Le paramètre *severity* spécifie la gravité de l'exception.|Il n’y a pas de paramètre *severity*. Quand THROW est utilisé pour initier l’exception, la gravité est toujours définie à 16. Cependant, quand THROW est utilisé pour lever à nouveau une exception existante, la gravité correspond au niveau de gravité de l’exception.|  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-using-throw-to-raise-an-exception"></a>R. Utilisation de THROW pour lever une exception  
 L’exemple suivant montre comment utiliser l’instruction `THROW` pour lever une exception.  
  
```sql  
THROW 51000, 'The record does not exist.', 1;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
 Msg 51000, Level 16, State 1, Line 1  
  
 The record does not exist.
 ```  
  
### <a name="b-using-throw-to-raise-an-exception-again"></a>B. Utilisation de THROW pour lever à nouveau une exception  
 L'exemple suivant montre comment utiliser l'instruction `THROW` pour lever une nouvelle fois la dernière exception levée.  
  
```sql  
USE tempdb;  
GO  
CREATE TABLE dbo.TestRethrow  
(    ID INT PRIMARY KEY  
);  
BEGIN TRY  
    INSERT dbo.TestRethrow(ID) VALUES(1);  
--  Force error 2627, Violation of PRIMARY KEY constraint to be raised.  
    INSERT dbo.TestRethrow(ID) VALUES(1);  
END TRY  
BEGIN CATCH  
  
    PRINT 'In catch block.';  
    THROW;  
END CATCH;  
  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
 In catch block. 
 Msg 2627, Level 14, State 1, Line 1  
 Violation of PRIMARY KEY constraint 'PK__TestReth__3214EC272E3BD7D3'. Cannot insert duplicate key in object 'dbo.TestRethrow'.  
 The statement has been terminated.
 ```  
  
### <a name="c-using-formatmessage-with-throw"></a>C. Utilisation de FORMATMESSAGE avec THROW  
 L'exemple suivant indique comment utiliser la fonction `FORMATMESSAGE` avec `THROW` pour générer un message d'erreur personnalisé. L'exemple commence par créer un message d'erreur défini par l'utilisateur à l'aide de `sp_addmessage`. Étant donné que l’instruction THROW n’accepte pas les paramètres de substitution dans le paramètre *message* comme le fait RAISERROR, la fonction FORMATMESSAGE permet de passer les trois valeurs de paramètre attendues par le message d’erreur 60000.  
  
```sql  
EXEC sys.sp_addmessage  
     @msgnum   = 60000  
    ,@severity = 16  
    ,@msgtext  = N'This is a test message with one numeric parameter (%d), one string parameter (%s), and another string parameter (%s).'  
    ,@lang = 'us_english';   
GO  
  
DECLARE @msg NVARCHAR(2048) = FORMATMESSAGE(60000, 500, N'First string', N'second string');   
  
THROW 60000, @msg, 1;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
 Msg 60000, Level 16, State 1, Line 2  
 This is a test message with one numeric parameter (500), one string parameter (First string), and another string parameter (second string).
 ```  
  
## <a name="see-also"></a>Voir aussi  
 [FORMATMESSAGE &#40;Transact-SQL&#41;](../../t-sql/functions/formatmessage-transact-sql.md)   
 [Niveaux de gravité des erreurs du moteur de base de données](../../relational-databases/errors-events/database-engine-error-severities.md)   
 [ERROR_LINE &#40;Transact-SQL&#41;](../../t-sql/functions/error-line-transact-sql.md)   
 [ERROR_MESSAGE &#40;Transact-SQL&#41;](../../t-sql/functions/error-message-transact-sql.md)   
 [ERROR_NUMBER &#40;Transact-SQL&#41;](../../t-sql/functions/error-number-transact-sql.md)   
 [ERROR_PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/functions/error-procedure-transact-sql.md)   
 [ERROR_SEVERITY &#40;Transact-SQL&#41;](../../t-sql/functions/error-severity-transact-sql.md)   
 [ERROR_STATE &#40;Transact-SQL&#41;](../../t-sql/functions/error-state-transact-sql.md)   
 [RAISERROR &#40;Transact-SQL&#41;](../../t-sql/language-elements/raiserror-transact-sql.md)   
 [@@ERROR &#40;Transact-SQL&#41;](../../t-sql/functions/error-transact-sql.md)   
 [GOTO &#40;Transact-SQL&#41;](../../t-sql/language-elements/goto-transact-sql.md)   
 [BEGIN...END &#40;Transact-SQL&#41;](../../t-sql/language-elements/begin-end-transact-sql.md)   
 [XACT_STATE &#40;Transact-SQL&#41;](../../t-sql/functions/xact-state-transact-sql.md)   
 [SET XACT_ABORT &#40;Transact-SQL&#41;](../../t-sql/statements/set-xact-abort-transact-sql.md)  
  
  

