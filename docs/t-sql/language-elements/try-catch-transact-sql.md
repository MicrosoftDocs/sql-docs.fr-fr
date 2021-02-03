---
description: TRY...CATCH (Transact-SQL)
title: TRY...CATCH (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- BEGIN_TRY_TSQL
- BEGIN_CATCH_TSQL
- TRY
- BEGIN TRY
- TRY_TSQL
- BEGIN CATCH
dev_langs:
- TSQL
helpviewer_keywords:
- BEGIN CATCH statement
- uncommittable transactions
- errors [SQL Server], TRY...CATCH
- TRY block [SQL Server]
- XACT_STATE function
- TRY...CATCH [SQL Server]
- BEGIN TRY statement
- CATCH block
ms.assetid: 248df62a-7334-4bca-8262-235a28f4b07f
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 7e694b4366ac0b0382fb9a29f155bc7fe346653b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99176374"
---
# <a name="trycatch-transact-sql"></a>TRY...CATCH (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]


  Implémente la gestion des erreurs pour [!INCLUDE[tsql](../../includes/tsql-md.md)], similaire à la gestion des exceptions dans les langages [!INCLUDE[msCoName](../../includes/msconame-md.md)] Visual C# et [!INCLUDE[msCoName](../../includes/msconame-md.md)] Visual C++. Un groupe d'instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] peut être contenu dans un bloc TRY. Si une erreur survient dans le bloc TRY, le contrôle est passé à un autre groupe d'instructions contenues dans un bloc CATCH.  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
BEGIN TRY  
     { sql_statement | statement_block }  
END TRY  
BEGIN CATCH  
     [ { sql_statement | statement_block } ]  
END CATCH  
[ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *sql_statement*  
 Toute instruction [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 *statement_block*  
 Tout groupe d’instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] dans un lot ou contenues dans un bloc BEGIN...END.  
  
## <a name="remarks"></a>Notes  
 Une construction TRY...CATCH intercepte toutes les erreurs d’exécution dont la gravité est supérieure à 10 et qui ne ferment pas la connexion de la base de données.  
  
 Un bloc TRY doit être suivi immédiatement d'un bloc CATCH associé. L'inclusion d'une autre instruction entre les instructions END TRY et BEGIN CATCH génère une erreur de syntaxe.  
  
 Une construction TRY...CATCH ne peut pas s’étendre sur plusieurs lots. Une construction TRY...CATCH ne peut pas s’étendre sur plusieurs blocs d’instructions [!INCLUDE[tsql](../../includes/tsql-md.md)]. Par exemple, une construction TRY...CATCH ne peut pas s’étendre sur deux blocs BEGIN...END d’instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] et ne peut pas s’étendre sur une construction IF...ELSE.  
  
 Si le code inclus dans un bloc TRY ne contient aucune erreur, lorsque l'exécution de la dernière instruction du bloc TRY est terminée, le contrôle est passé à l'instruction qui se trouve immédiatement après l'instruction END CATCH associée.
 
 S'il existe une erreur dans le code inclus dans le bloc TRY, le contrôle est passé à la première instruction du bloc CATCH associé. Lorsque le code du bloc CATCH est terminé, le contrôle est passé à l'instruction qui se trouve immédiatement après l'instruction END CATCH. 
 
 > [!NOTE] 
 > Si l'instruction END CATCH se trouve dans la dernière instruction d'une procédure stockée ou d'un déclencheur, le contrôle revient à l'instruction qui a appelé la procédure stockée ou activé le déclencheur. 
 
 Les erreurs interceptées par un bloc CATCH ne sont pas renvoyées à l'application appelante. Si une partie des informations sur les erreurs doivent être renvoyées à l'application, le code dans le bloc CATCH doit pour ce faire utiliser des mécanismes tels que les jeux de résultats SELECT ou les instructions RAISERROR et PRINT.  
  
 Les constructions TRY...CATCH peuvent être imbriquées. Un bloc TRY ou CATCH peut contenir des constructions imbriquées TRY...CATCH. Par exemple, un bloc CATCH peut contenir une construction TRY...CATCH imbriquée pour gérer les erreurs rencontrées par le code CATCH.  
  
 Les erreurs rencontrées dans un bloc CATCH sont traitées comme les erreurs générées à n'importe quel autre emplacement. Quand un bloc CATCH contient une construction TRY...CATCH imbriquée, toute erreur dans le bloc imbriqué TRY passe le contrôle au bloc CATCH imbriqué. S’il n’existe pas de construction TRY...CATCH imbriquée, l’erreur est renvoyée à l’appelant.  
  
 Les constructions TRY...CATCH interceptent les erreurs non gérées provenant de procédures stockées ou de déclencheurs exécutés par le code dans le bloc TRY. Les procédures stockées ou les déclencheurs peuvent également contenir leurs propres constructions TRY...CATCH pour gérer les erreurs générées par leur code. Par exemple, lorsqu'un bloc TRY exécute une procédure stockée et qu'une erreur survient dans la procédure stockée, l'erreur peut être gérée des manières suivantes :  
  
-   Si la procédure stockée ne contient pas sa propre construction TRY...CATCH, l’erreur retourne le contrôle au bloc CATCH associé au bloc TRY qui contient l’instruction EXECUTE.  
  
-   Si la procédure stockée contient une construction TRY...CATCH, l’erreur transfère le contrôle au bloc CATCH dans la procédure stockée. Lorsque le code du bloc CATCH est terminé, le contrôle est renvoyé à l'instruction qui se trouve immédiatement après l'instruction EXECUTE qui a appelé la procédure stockée.  
  
 Les instructions GOTO ne peuvent pas être utilisées pour entrer un bloc TRY ou CATCH. Les instructions GOTO peuvent être utilisées pour accéder directement à une étiquette dans le même bloc TRY ou CATCH ou pour quitter un bloc TRY ou CATCH.  
  
 La construction TRY...CATCH ne peut pas être utilisée dans une fonction définie par l’utilisateur.  
  
## <a name="retrieving-error-information"></a>Extraction des informations sur les erreurs  
 Dans la portée d'un bloc CATCH, les fonctions système suivantes peuvent être utilisées pour obtenir des informations sur l'erreur qui a entraîné l'exécution du bloc CATCH :  
  
-   [ERROR_NUMBER()](../../t-sql/functions/error-number-transact-sql.md) renvoie le numéro de l’erreur.  
  
-   [ERROR_SEVERITY()](../../t-sql/functions/error-severity-transact-sql.md) renvoie la gravité de l’erreur.  
  
-   [ERROR_STATE()](../../t-sql/functions/error-state-transact-sql.md) renvoie le numéro d’état de l’erreur.  
  
-   [ERROR_PROCEDURE()](../../t-sql/functions/error-procedure-transact-sql.md) renvoie le nom de la procédure stockée ou du déclencheur où s’est produite l’erreur.  
  
-   [ERROR_LINE()](../../t-sql/functions/error-line-transact-sql.md) renvoie le numéro de ligne au sein de la routine qui a entraîné l’erreur.  
  
-   [ERROR_MESSAGE()](../../t-sql/functions/error-message-transact-sql.md) renvoie le texte complet du message d’erreur. Le texte comprend les valeurs fournies pour tous les paramètres remplaçables, tels que les longueurs, les noms d'objet ou les heures.  
  
Ces fonctions renvoient la valeur NULL si elles sont appelées en dehors de l'étendue du bloc CATCH. Les informations sur les erreurs peuvent être récupérées à l'aide de ces fonctions à partir de n'importe quel emplacement dans l'étendue du bloc CATCH. Par exemple, le script suivant montre une procédure stockée contenant des fonctions de gestion des erreurs : dans le bloc `CATCH` d'une construction `TRY...CATCH`, la procédure stockée est appelée et les informations sur l'erreur sont retournées.  
  
```sql  
-- Verify that the stored procedure does not already exist.  
IF OBJECT_ID ( 'usp_GetErrorInfo', 'P' ) IS NOT NULL   
    DROP PROCEDURE usp_GetErrorInfo;  
GO  
  
-- Create procedure to retrieve error information.  
CREATE PROCEDURE usp_GetErrorInfo  
AS  
SELECT  
    ERROR_NUMBER() AS ErrorNumber  
    ,ERROR_SEVERITY() AS ErrorSeverity  
    ,ERROR_STATE() AS ErrorState  
    ,ERROR_PROCEDURE() AS ErrorProcedure  
    ,ERROR_LINE() AS ErrorLine  
    ,ERROR_MESSAGE() AS ErrorMessage;  
GO  
  
BEGIN TRY  
    -- Generate divide-by-zero error.  
    SELECT 1/0;  
END TRY  
BEGIN CATCH  
    -- Execute error retrieval routine.  
    EXECUTE usp_GetErrorInfo;  
END CATCH;   
```  
  
 Les fonctions ERROR\_\* fonctionnent également dans un bloc `CATCH` à l’intérieur d’une [procédure stockée compilée en mode natif](../../relational-databases/in-memory-oltp/a-guide-to-query-processing-for-memory-optimized-tables.md).  
  
## <a name="errors-unaffected-by-a-trycatch-construct"></a>Erreurs non affectées par une construction TRY...CATCH  
 Les constructions TRY...CATCH n’interceptent pas les conditions suivantes :  
  
-   Les avertissements ou messages d'information dont la gravité est inférieure ou égale à 10.  
  
-   Les erreurs dont le niveau de gravité est supérieur ou égal à 20 interrompent le traitement des tâches du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] pour la session. Si une erreur dont le niveau de gravité est supérieur ou égal à 20 survient et que la connexion à la base de données n’est pas interrompue, TRY...CATCH gère l’erreur.  
  
-   Un événement d'avertissement, tel qu'une requête d'interruption par le client ou une rupture de connexion avec le client, se produit.  
  
-   Cela survient lorsque la session est terminée par un administrateur système utilisant l'instruction KILL.  
  
Les types d’erreurs suivants ne sont pas gérés par un bloc CATCH quand elles surviennent au même niveau d’exécution que la construction TRY...CATCH :  
  
-   Les erreurs de compilation, telles que les erreurs de syntaxe, qui empêchent un traitement de s'exécuter.  
  
-   Les erreurs qui se produisent pendant une recompilation de niveau instruction, telles que les erreurs de résolution de nom d’objet qui surviennent après la compilation en raison d’une résolution de nom différée.  
-   Erreurs de résolution de noms d’objets   

  
Ces erreurs sont renvoyées au niveau qui a exécuté le traitement, la procédure stockée ou le déclencheur.  
  
Si une erreur survient pendant la compilation ou la recompilation au niveau de l’instruction à un niveau d’exécution inférieur (par exemple lors de l’exécution de sp_executesql ou d’une procédure stockée définie par l’utilisateur) à l’intérieur du bloc TRY, l’erreur survient à un niveau inférieur à la construction TRY...CATCH et est gérée par le bloc CATCH associé.  
  
L'exemple suivant montre comment une erreur de résolution de noms d'objets générée par une instruction `SELECT` n'est pas interceptée par la construction `TRY...CATCH`, mais par le bloc `CATCH` lorsque la même instruction `SELECT` est exécutée au sein d'une procédure stockée.  
  
```sql  
BEGIN TRY  
    -- Table does not exist; object name resolution  
    -- error not caught.  
    SELECT * FROM NonexistentTable;  
END TRY  
BEGIN CATCH  
    SELECT   
        ERROR_NUMBER() AS ErrorNumber  
       ,ERROR_MESSAGE() AS ErrorMessage;  
END CATCH  
```  
  
 L'erreur n'est pas interceptée et le contrôle est passé de la construction `TRY...CATCH` au niveau supérieur suivant.  
  
 L'exécution de l'instruction `SELECT` au sein d'une procédure stockée entraîne l'occurrence de l'erreur à un niveau inférieur à celui du bloc `TRY`. L'erreur sera gérée par la construction `TRY...CATCH`.  
  
```sql  
-- Verify that the stored procedure does not exist.  
IF OBJECT_ID ( N'usp_ExampleProc', N'P' ) IS NOT NULL   
    DROP PROCEDURE usp_ExampleProc;  
GO  
  
-- Create a stored procedure that will cause an   
-- object resolution error.  
CREATE PROCEDURE usp_ExampleProc  
AS  
    SELECT * FROM NonexistentTable;  
GO  
  
BEGIN TRY  
    EXECUTE usp_ExampleProc;  
END TRY  
BEGIN CATCH  
    SELECT   
        ERROR_NUMBER() AS ErrorNumber  
        ,ERROR_MESSAGE() AS ErrorMessage;  
END CATCH;  
```  
  
## <a name="uncommittable-transactions-and-xact_state"></a>Transactions non validables et XACT_STATE  
 Si une erreur générée dans un bloc TRY entraîne l'invalidation de la transaction actuelle, elle est classifiée comme transaction non validable. Une erreur qui termine normalement une transaction en dehors d'un bloc TRY fait entrer la transaction dans un état non validable lorsqu'elle survient au sein d'un bloc TRY. Une transaction non validable ne peut effectuer que des opérations de lecture ou ROLLBACK TRANSACTION. La transaction ne peut exécuter aucune instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] qui générerait une opération d'écriture ou COMMIT TRANSACTION. La fonction XACT_STATE renvoie une valeur de -1 si une transaction a été classifiée comme non validable. Lorsqu'un traitement est terminé, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] restaure automatiquement toutes les transactions non validables actives. Si aucun message d'erreur n'a été envoyé lorsque la transaction est passée dans un état non validable, une erreur est envoyée à l'application cliente lorsque le traitement se termine. Cela indique qu'une transaction non validable a été détectée et annulée.  
  
 Pour plus d’informations sur les transactions non validables et la fonction XACT_STATE, consultez [XACT_STATE &#40;Transact-SQL&#41;](../../t-sql/functions/xact-state-transact-sql.md).  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-using-trycatch"></a>R. Utilisation de TRY...CATCH  
 L'exemple suivant illustre une instruction `SELECT` qui génère une erreur de division par zéro. L'erreur entraîne le saut de l'exécution vers le bloc `CATCH` associé.  
  
```sql  
BEGIN TRY  
    -- Generate a divide-by-zero error.  
    SELECT 1/0;  
END TRY  
BEGIN CATCH  
    SELECT  
        ERROR_NUMBER() AS ErrorNumber  
        ,ERROR_SEVERITY() AS ErrorSeverity  
        ,ERROR_STATE() AS ErrorState  
        ,ERROR_PROCEDURE() AS ErrorProcedure  
        ,ERROR_LINE() AS ErrorLine  
        ,ERROR_MESSAGE() AS ErrorMessage;  
END CATCH;  
GO  
```  
  
### <a name="b-using-trycatch-in-a-transaction"></a>B. Utilisation de TRY...CATCH dans une transaction  
 L'exemple suivant montre comment un bloc `TRY...CATCH` fonctionne dans une transaction. L'instruction dans le bloc `TRY` génère une erreur de violation de contrainte.  
  
```sql  
BEGIN TRANSACTION;  
  
BEGIN TRY  
    -- Generate a constraint violation error.  
    DELETE FROM Production.Product  
    WHERE ProductID = 980;  
END TRY  
BEGIN CATCH  
    SELECT   
        ERROR_NUMBER() AS ErrorNumber  
        ,ERROR_SEVERITY() AS ErrorSeverity  
        ,ERROR_STATE() AS ErrorState  
        ,ERROR_PROCEDURE() AS ErrorProcedure  
        ,ERROR_LINE() AS ErrorLine  
        ,ERROR_MESSAGE() AS ErrorMessage;  
  
    IF @@TRANCOUNT > 0  
        ROLLBACK TRANSACTION;  
END CATCH;  
  
IF @@TRANCOUNT > 0  
    COMMIT TRANSACTION;  
GO  
```  
  
### <a name="c-using-trycatch-with-xact_state"></a>C. Utilisation de TRY...CATCH avec XACT_STATE  
 L'exemple suivant montre comment utiliser la construction `TRY...CATCH` pour gérer les erreurs qui surviennent dans une transaction. La fonction `XACT_STATE` détermine si la transaction doit être validée ou annulée. Dans cet exemple, `SET XACT_ABORT` est `ON`. Cela rend la transaction non validable lorsque l'erreur de violation de contrainte se produit.  
  
```sql  
-- Check to see whether this stored procedure exists.  
IF OBJECT_ID (N'usp_GetErrorInfo', N'P') IS NOT NULL  
    DROP PROCEDURE usp_GetErrorInfo;  
GO  
  
-- Create procedure to retrieve error information.  
CREATE PROCEDURE usp_GetErrorInfo  
AS  
    SELECT   
         ERROR_NUMBER() AS ErrorNumber  
        ,ERROR_SEVERITY() AS ErrorSeverity  
        ,ERROR_STATE() AS ErrorState  
        ,ERROR_LINE () AS ErrorLine  
        ,ERROR_PROCEDURE() AS ErrorProcedure  
        ,ERROR_MESSAGE() AS ErrorMessage;  
GO  
  
-- SET XACT_ABORT ON will cause the transaction to be uncommittable  
-- when the constraint violation occurs.   
SET XACT_ABORT ON;  
  
BEGIN TRY  
    BEGIN TRANSACTION;  
        -- A FOREIGN KEY constraint exists on this table. This   
        -- statement will generate a constraint violation error.  
        DELETE FROM Production.Product  
            WHERE ProductID = 980;  
  
    -- If the DELETE statement succeeds, commit the transaction.  
    COMMIT TRANSACTION;  
END TRY  
BEGIN CATCH  
    -- Execute error retrieval routine.  
    EXECUTE usp_GetErrorInfo;  
  
    -- Test XACT_STATE:  
        -- If 1, the transaction is committable.  
        -- If -1, the transaction is uncommittable and should   
        --     be rolled back.  
        -- XACT_STATE = 0 means that there is no transaction and  
        --     a commit or rollback operation would generate an error.  
  
    -- Test whether the transaction is uncommittable.  
    IF (XACT_STATE()) = -1  
    BEGIN  
        PRINT  
            N'The transaction is in an uncommittable state.' +  
            'Rolling back transaction.'  
        ROLLBACK TRANSACTION;  
    END;  
  
    -- Test whether the transaction is committable.
    -- You may want to commit a transaction in a catch block if you want to commit changes to statements that ran prior to the error.
    IF (XACT_STATE()) = 1  
    BEGIN  
        PRINT  
            N'The transaction is committable.' +  
            'Committing transaction.'  
        COMMIT TRANSACTION;     
    END;  
END CATCH;  
GO  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Exemples : [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="d-using-trycatch"></a>D. Utilisation de TRY...CATCH  
 L'exemple suivant illustre une instruction `SELECT` qui génère une erreur de division par zéro. L'erreur entraîne le saut de l'exécution vers le bloc `CATCH` associé.  
  
```sql  
BEGIN TRY  
    -- Generate a divide-by-zero error.  
    SELECT 1/0;  
END TRY  
BEGIN CATCH  
    SELECT  
        ERROR_NUMBER() AS ErrorNumber  
        ,ERROR_SEVERITY() AS ErrorSeverity  
        ,ERROR_STATE() AS ErrorState  
        ,ERROR_PROCEDURE() AS ErrorProcedure  
        ,ERROR_MESSAGE() AS ErrorMessage;  
END CATCH;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [THROW &#40;Transact-SQL&#41;](../../t-sql/language-elements/throw-transact-sql.md)   
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
  
