---
description: ERROR_PROCEDURE (Transact-SQL)
title: ERROR_PROCEDURE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ERROR_PROCEDURE_TSQL
- ERROR_PROCEDURE
dev_langs:
- TSQL
helpviewer_keywords:
- ERROR_PROCEDURE function
- messages [SQL Server], trigger where occurred
- errors [SQL Server], stored procedure where occurred
- TRY...CATCH [SQL Server]
- CATCH block
- messages [SQL Server], stored procedure where occurred
- errors [SQL Server], trigger where occurred
ms.assetid: b81edbf0-856a-498f-ba87-48ff1426d980
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 05ab2a8c6f9b278fb548eb215653e0434232b2ae
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97643755"
---
# <a name="error_procedure-transact-sql"></a>ERROR_PROCEDURE (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]  

Cette fonction retourne le nom de la procédure stockée ou du déclencheur où une erreur se produit, si cette erreur a entraîné l’exécution du bloc CATCH d’une construction TRY...CATCH. 
- SQL Server 2017 jusqu’à [version actuelle](../../sql-server/what-s-new-in-sql-server-ver15.md) retourne schema_name.stored_procedure_name
- SQL Server 2016 retourne stored_procedure_name

 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql  
ERROR_PROCEDURE ( )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Types de retour
**nvarchar(128)**  
  
## <a name="return-value"></a>Valeur de retour  
Si elle est appelée dans un bloc CATCH, `ERROR_PROCEDURE` retourne le nom de la procédure stockée ou du déclencheur d’où provient l’erreur.
  
`ERROR_PROCEDURE` retourne la valeur NULL si l’erreur ne s’est pas produite dans une procédure stockée ou un déclencheur.  
  
`ERROR_PROCEDURE` retourne NULL quand l’appel a lieu en dehors de l’étendue d’un bloc CATCH.  
  
## <a name="remarks"></a>Notes  
`ERROR_PROCEDURE` prend en charge les appels à partir de n’importe quel emplacement dans l’étendue d’un bloc CATCH.  
  
`ERROR_PROCEDURE` retourne le nom de la procédure stockée ou du déclencheur où une erreur se produit, quel que soit le nombre de fois où il/elle s’exécute ou l’emplacement de son exécution dans l’étendue du bloc `CATCH`. Ce comportement contraste avec celui d’une fonction comme @@ERROR, qui retourne uniquement un numéro d’erreur dans l’instruction immédiatement après celle qui a provoqué une erreur.  
   
## <a name="examples-sssdwfull-and-sspdw"></a>Exemples : [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]
  
### <a name="a-using-error_procedure-in-a-catch-block"></a>R. Utilisation d'ERROR_PROCEDURE dans un bloc CATCH  
L’exemple suivant illustre une procédure stockée qui génère une erreur de division par zéro. `ERROR_PROCEDURE` retourne le nom de la procédure stockée où l’erreur s’est produite.  
  
```sql  
-- Verify that the stored procedure does not already exist.  
IF OBJECT_ID ( 'usp_ExampleProc', 'P' ) IS NOT NULL   
    DROP PROCEDURE usp_ExampleProc;  
GO  
  
-- Create a stored procedure that   
-- generates a divide-by-zero error.  
CREATE PROCEDURE usp_ExampleProc  
AS  
    SELECT 1/0;  
GO  
  
BEGIN TRY  
    -- Execute the stored procedure inside the TRY block.  
    EXECUTE usp_ExampleProc;  
END TRY  
BEGIN CATCH  
    SELECT ERROR_PROCEDURE() AS ErrorProcedure;  
END CATCH;  
GO  
```  

[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
```  
-----------

(0 row(s) affected)

ErrorProcedure
--------------------
usp_ExampleProc

(1 row(s) affected)
```  

  
### <a name="b-using-error_procedure-in-a-catch-block-with-other-error-handling-tools"></a>B. Utilisation d'ERROR_PROCEDURE dans un bloc CATCH avec d'autres outils de gestion des erreurs  
L’exemple suivant illustre une procédure stockée qui génère une erreur de division par zéro. Outre le nom de la procédure stockée où l’erreur s’est produite, la procédure stockée retourne des informations sur cette erreur.  
  
```sql
-- Verify that the stored procedure does not already exist.  
IF OBJECT_ID ( 'usp_ExampleProc', 'P' ) IS NOT NULL   
    DROP PROCEDURE usp_ExampleProc;  
GO  
  
-- Create a stored procedure that   
-- generates a divide-by-zero error.  
CREATE PROCEDURE usp_ExampleProc  
AS  
    SELECT 1/0;  
GO  
  
BEGIN TRY  
    -- Execute the stored procedure inside the TRY block.  
    EXECUTE usp_ExampleProc;  
END TRY  
BEGIN CATCH  
    SELECT   
        ERROR_NUMBER() AS ErrorNumber,  
        ERROR_SEVERITY() AS ErrorSeverity,  
        ERROR_STATE() AS ErrorState,  
        ERROR_PROCEDURE() AS ErrorProcedure,  
        ERROR_MESSAGE() AS ErrorMessage,  
        ERROR_LINE() AS ErrorLine;  
        END CATCH;  
GO
``` 

[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
``` 
-----------

(0 row(s) affected)

ErrorNumber ErrorSeverity ErrorState  ErrorProcedure   ErrorMessage                       ErrorLine
----------- ------------- ----------- ---------------- ---------------------------------- -----------
8134        16            1           usp_ExampleProc  Divide by zero error encountered.  6

(1 row(s) affected)

``` 

  
## <a name="see-also"></a>Voir aussi  
 [sys.messages &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/messages-for-errors-catalog-views-sys-messages.md)   
 [TRY...CATCH &#40;Transact-SQL&#41;](../../t-sql/language-elements/try-catch-transact-sql.md)   
 [ERROR_LINE &#40;Transact-SQL&#41;](../../t-sql/functions/error-line-transact-sql.md)   
 [ERROR_MESSAGE &#40;Transact-SQL&#41;](../../t-sql/functions/error-message-transact-sql.md)   
 [ERROR_NUMBER &#40;Transact-SQL&#41;](../../t-sql/functions/error-number-transact-sql.md)   
 [ERROR_SEVERITY &#40;Transact-SQL&#41;](../../t-sql/functions/error-severity-transact-sql.md)   
 [ERROR_STATE &#40;Transact-SQL&#41;](../../t-sql/functions/error-state-transact-sql.md)   
 [RAISERROR &#40;Transact-SQL&#41;](../../t-sql/language-elements/raiserror-transact-sql.md)   
 [@@ERROR &#40;Transact-SQL&#41;](../../t-sql/functions/error-transact-sql.md)  
  
  

