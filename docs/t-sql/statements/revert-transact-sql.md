---
description: REVERT (Transact-SQL)
title: REVERT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/26/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- REVERT_TSQL
- REVERT
dev_langs:
- TSQL
helpviewer_keywords:
- REVERT statement
- context switching [SQL Server], reverting
- reverting execution context
- REVERT WITH COOKIE statement
- execution context [SQL Server]
- COOKIE clause
ms.assetid: 4688b17a-dfd1-4f03-8db4-273a401f879f
author: VanMSFT
ms.author: vanto
monikerRange: = azuresqldb-current || = azuresqldb-mi-current || >= sql-server-2016 || >= sql-server-linux-2017||=azure-sqldw-latest
ms.openlocfilehash: f8966a7c1204ec61f2271d4cd44a559a593473a2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209974"
---
# <a name="revert-transact-sql"></a>REVERT (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]   

  Restaure le contexte d'exécution de l'appelant de la dernière instruction EXECUTE AS.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
REVERT  
    [ WITH COOKIE = @varbinary_variable ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 WITH COOKIE = @*varbinary_variable*  
 Spécifie le cookie créé dans une instruction [EXECUTE AS](../../t-sql/statements/execute-as-transact-sql.md) autonome correspondante. *\@varbinary_variable* est **varbinary(100)** .  
  
## <a name="remarks"></a>Remarques  
 REVERT peut figurer dans un module tel qu'une procédure stockée ou une fonction définie par l'utilisateur, mais aussi en tant qu'instruction autonome. À l'intérieur d'un module, REVERT s'applique uniquement aux instructions EXECUTE AS définies dans le module. Par exemple, la procédure stockée suivante exécute une instruction `EXECUTE AS` suivie d'une instruction `REVERT`.  
  
```sql  
CREATE PROCEDURE dbo.usp_myproc   
  WITH EXECUTE AS CALLER  
AS   
    SELECT SUSER_NAME(), USER_NAME();  
    EXECUTE AS USER = 'guest';  
    SELECT SUSER_NAME(), USER_NAME();  
    REVERT;  
    SELECT SUSER_NAME(), USER_NAME();  
GO  
```  
  
 Supposons que dans la session où la procédure stockée est exécutée, le contexte d'exécution de la session est explicitement modifié en `login1`, comme dans l'exemple ci-dessous.  
  
```sql 
  -- Sets the execution context of the session to 'login1'.  
EXECUTE AS LOGIN = 'login1';  
GO  
EXECUTE dbo.usp_myproc;   
```  
  
 L’instruction `REVERT` définie dans `usp_myproc` change le contexte d’exécution défini dans le module, mais elle n’affecte pas le contexte d’exécution défini à l’extérieur du module. Autrement dit, le contexte d'exécution de la session reste `login1`.  
  
 Lorsque REVERT est une instruction autonome, elle s'applique aux instructions EXECUTE AS définies dans un traitement ou une session. REVERT n'a aucun effet si l'instruction EXECUTE AS correspondante contient la clause WITH NO REVERT. Dans ce cas, le contexte d'exécution reste en vigueur jusqu'à la suppression de la session.  
  
## <a name="using-revert-with-cookie"></a>Utilisation de REVERT WITH COOKIE  
 L’instruction EXECUTE AS utilisée pour définir le contexte d’exécution d’une session peut comprendre la clause facultative WITH NO REVERT COOKIE = @*varbinary_variable*. Quand cette instruction est exécutée, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] passe le cookie à @*varbinary_variable*. Le contexte d’exécution défini par cette instruction ne peut être restauré vers le contexte précédent que si l’instruction REVERT WITH COOKIE = @*varbinary_variable* appelante contient la valeur *\@varbinary_variable* correcte.  
  
 Ce mécanisme est utile dans un environnement utilisant le groupement de connexions. Le groupement de connexions est la maintenance d'un groupe de connexions de base de données en vue de leur réutilisation par plusieurs utilisateurs finaux. Comme la valeur passée à *\@varbinary_variable* n’est connue que de l’appelant de l’instruction EXECUTE AS (en l’occurrence, l’application), celui-ci peut garantir que le contexte d’exécution qu’il établit ne sera pas modifié par l’utilisateur final qui appelle l’application. Après restauration du contexte d'exécution, l'application peut changer de contexte au profit d'un autre principal.  
  
## <a name="permissions"></a>Autorisations  
 Aucune autorisation n'est requise.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-using-execute-as-and-revert-to-switch-context"></a>R. Utilisation de EXECUTE AS et REVERT pour changer de contexte  
 L'exemple ci-dessous crée une pile de contextes d'exécution à l'aide de plusieurs principaux. L'instruction REVERT y est ensuite utilisée pour rendre le contexte d'exécution à l'appelant précédent. L'instruction REVERT est exécutée plusieurs fois en remontant dans la pile, jusqu'à ce que le contexte d'exécution revienne à l'appelant d'origine.  
  
```sql  
USE AdventureWorks2012;  
GO  
-- Create two temporary principals.  
CREATE LOGIN login1 WITH PASSWORD = 'J345#$)thb';  
CREATE LOGIN login2 WITH PASSWORD = 'Uor80$23b';  
GO  
CREATE USER user1 FOR LOGIN login1;  
CREATE USER user2 FOR LOGIN login2;  
GO  
-- Give IMPERSONATE permissions on user2 to user1  
-- so that user1 can successfully set the execution context to user2.  
GRANT IMPERSONATE ON USER:: user2 TO user1;  
GO  
-- Display current execution context.  
SELECT SUSER_NAME(), USER_NAME();  
-- Set the execution context to login1.   
EXECUTE AS LOGIN = 'login1';  
-- Verify that the execution context is now login1.  
SELECT SUSER_NAME(), USER_NAME();  
-- Login1 sets the execution context to login2.  
EXECUTE AS USER = 'user2';  
-- Display current execution context.  
SELECT SUSER_NAME(), USER_NAME();  
-- The execution context stack now has three principals: the originating caller, login1, and login2.  
-- The following REVERT statements will reset the execution context to the previous context.  
REVERT;  
-- Display the current execution context.  
SELECT SUSER_NAME(), USER_NAME();  
REVERT;  
-- Display the current execution context.  
SELECT SUSER_NAME(), USER_NAME();  
  
-- Remove the temporary principals.  
DROP LOGIN login1;  
DROP LOGIN login2;  
DROP USER user1;  
DROP USER user2;  
GO  
```  
  
### <a name="b-using-the-with-cookie-clause"></a>B. Utilisation de la clause WITH COOKIE  
 L’exemple suivant définit le contexte d’exécution d’une session sur un utilisateur spécifié et précise la clause WITH NO REVERT COOKIE = @*varbinary_variable*. L'instruction `REVERT` doit spécifier la valeur passée à la variable `@cookie` dans l'instruction `EXECUTE AS` pour ramener le contexte à l'appelant. Pour exécuter cet exemple, la connexion `login1` et l'utilisateur `user1` créés dans l'exemple A doivent exister.  
  
```sql 
DECLARE @cookie VARBINARY(100);  
EXECUTE AS USER = 'user1' WITH COOKIE INTO @cookie;  
-- Store the cookie somewhere safe in your application.  
-- Verify the context switch.  
SELECT SUSER_NAME(), USER_NAME();  
--Display the cookie value.  
SELECT @cookie;  
GO  
-- Use the cookie in the REVERT statement.  
DECLARE @cookie VARBINARY(100);  
-- Set the cookie value to the one from the SELECT @cookie statement.  
SET @cookie = <value from the SELECT @cookie statement>;  
REVERT WITH COOKIE = @cookie;  
-- Verify the context switch reverted.  
SELECT SUSER_NAME(), USER_NAME();  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [EXECUTE AS &#40;Transact-SQL&#41;](../../t-sql/statements/execute-as-transact-sql.md)   
 [Clause EXECUTE AS &#40;Transact-SQL&#41;](../../t-sql/statements/execute-as-clause-transact-sql.md)   
 [EXECUTE &#40;Transact-SQL&#41;](../../t-sql/language-elements/execute-transact-sql.md)   
 [SUSER_NAME &#40;Transact-SQL&#41;](../../t-sql/functions/suser-name-transact-sql.md)   
 [USER_NAME &#40;Transact-SQL&#41;](../../t-sql/functions/user-name-transact-sql.md)   
 [CREATE LOGIN &#40;Transact-SQL&#41;](../../t-sql/statements/create-login-transact-sql.md)   
 [CREATE USER &#40;Transact-SQL&#41;](../../t-sql/statements/create-user-transact-sql.md)  
  
  
