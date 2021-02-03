---
description: RETURN (Transact-SQL)
title: RETURN (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- RETURN_TSQL
- RETURN
dev_langs:
- TSQL
helpviewer_keywords:
- unconditionally exiting program
- stored procedures [SQL Server], exiting
- batches [SQL Server], exiting
- statement blocks [SQL Server]
- queries [SQL Server], exiting
- exiting queries [SQL Server]
- exiting procedures [SQL Server]
- RETURN statement
ms.assetid: 1d9c8247-fd89-4544-be9c-01c95b745db0
author: cawrites
ms.author: chadam
ms.openlocfilehash: aec79684b2d0325381c3a1d70b976e4281007f61
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194320"
---
# <a name="return-transact-sql"></a>RETURN (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Permet de quitter sans condition une requête ou une procédure. RETURN a une action immédiate et complète, et elle est utilisable à tout moment pour quitter une procédure, un traitement ou un bloc d'instructions. Les instructions qui suivent RETURN ne sont pas exécutées.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
RETURN [ integer_expression ]   
```  
  
## <a name="arguments"></a>Arguments  
 *integer_expression*  
 Valeur entière qui est retournée. Les procédures stockées peuvent retourner une valeur entière à une procédure appelante ou à une application.  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Types de retour
 Retourne éventuellement **int**.  
  
> [!NOTE]  
>  Sauf spécification contraire, toutes les procédures stockées système retournent la valeur 0. Celle-ci indique la réussite d'une procédure ; une valeur différente de zéro indique un échec.  
  
## <a name="remarks"></a>Remarques  
 Utilisée avec une procédure stockée, l'instruction RETURN ne peut pas retourner de valeur Null. Si une procédure tente de retourner une valeur Null (par exemple à l'aide de RETURN @status alors que @status est NULL), un avertissement est généré et une valeur de 0 est retournée.  
  
 Il est possible d'inclure la valeur de l'état retourné dans des instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] suivantes du lot ou de la procédure ayant exécuté la procédure en cours, mais il convient dans ce cas de respecter la forme suivante : `EXECUTE @return_status = <procedure_name>`.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-returning-from-a-procedure"></a>R. Sortie d'une procédure  
 Cet exemple montre que si aucun nom d'utilisateur n'est fourni en tant que paramètre lors de l'exécution de `findjobs`, `RETURN` met un terme à la procédure après envoi d'un message à l'utilisateur. Si en revanche un nom d'utilisateur est fourni, les noms de tous les objets créés par celui-ci dans la base de données active sont récupérés dans les tables système adéquates.  
  
```sql  
CREATE PROCEDURE findjobs @nm sysname = NULL  
AS   
IF @nm IS NULL  
    BEGIN  
        PRINT 'You must give a user name'  
        RETURN  
    END  
ELSE  
    BEGIN  
        SELECT o.name, o.id, o.uid  
        FROM sysobjects o INNER JOIN master..syslogins l  
            ON o.uid = l.sid  
        WHERE l.name = @nm  
    END;  
```  
  
### <a name="b-returning-status-codes"></a>B. Renvoi des codes d'état  
 L'exemple suivant vérifie l'état de résidence aux États-Unis pour l'ID d'un contact spécifié. Si l'état est Washington (`WA`), une valeur de `1` est retournée. Sinon, le code `2` est retourné pour toute autre condition (valeur différente de `WA` pour `StateProvince` ou `ContactID` qui ne correspond pas à une ligne).  
  
```sql  
USE AdventureWorks2012;  
GO  
CREATE PROCEDURE checkstate @param VARCHAR(11)  
AS  
IF (SELECT StateProvince FROM Person.vAdditionalContactInfo WHERE ContactID = @param) = 'WA'  
    RETURN 1  
ELSE  
    RETURN 2;  
GO  
```  
  
 Les exemples suivants indiquent le code d'état retourné à la suite de l'exécution de `checkstate`. Le premier montre un contact à Washington ; le second, un contact qui n'est pas à Washington ; et le troisième, un contact qui n'est pas valide. La variable locale `@return_status` doit être déclarée avant d'être utilisée.  
  
```sql  
DECLARE @return_status INT;  
EXEC @return_status = checkstate '2';  
SELECT 'Return Status' = @return_status;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
 Return Status 
  
 ------------- 
  
 1
 ```  
  
 Exécutez à nouveau la requête, en spécifiant un numéro de contact différent.  
  
```sql  
DECLARE @return_status INT;  
EXEC @return_status = checkstate '6';  
SELECT 'Return Status' = @return_status;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
 Return Status  
 -------------  
  
 2
 ```  
  
 Exécutez à nouveau la requête, en spécifiant un autre numéro de contact.  
  
```sql  
DECLARE @return_status INT  
EXEC @return_status = checkstate '12345678901';  
SELECT 'Return Status' = @return_status;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
 Return Status  
 -------------  
  
 2
 ```  
  
## <a name="see-also"></a>Voir aussi  
 [ALTER PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-procedure-transact-sql.md)   
 [CREATE PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/statements/create-procedure-transact-sql.md)   
 [DECLARE @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/declare-local-variable-transact-sql.md)   
 [EXECUTE &#40;Transact-SQL&#41;](../../t-sql/language-elements/execute-transact-sql.md)   
 [SET @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/set-local-variable-transact-sql.md)   
 [THROW &#40;Transact-SQL&#41;](../../t-sql/language-elements/throw-transact-sql.md)  
  
  
