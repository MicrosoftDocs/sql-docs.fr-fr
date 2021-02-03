---
description: ERROR_LINE (Transact-SQL)
title: ERROR_LINE (Transact-SQL)
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ERROR_LINE
- ERROR_LINE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- errors [SQL Server], line number
- messages [SQL Server], line number
- TRY...CATCH [SQL Server]
- line number of error [SQL Server]
- ERROR_LINE function
- CATCH block
ms.assetid: 47335734-0baf-45a6-8b3b-6c4fd80d2cb8
author: cawrites
ms.author: chadam
ms.openlocfilehash: 8ec5cf07de8e7a32c52101b195ca32ff1caf2a12
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199526"
---
# <a name="error_line-transact-sql"></a>ERROR_LINE (Transact-SQL)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Cette fonction retourne le numéro de ligne de l’occurrence d’une erreur qui a provoqué l’exécution du bloc CATCH d’une construction TRY...CATCH.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe

```syntaxsql
ERROR_LINE ( )
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-type"></a>Type de retour
**int**

## <a name="return-value"></a>Valeur de retour  
Quand elle est appelée dans un bloc CATCH, `ERROR_LINE` retourne  
  
-   le numéro de la ligne où l’erreur s’est produite ;    
-   le numéro de la ligne dans un sous-programme si l’erreur s’est produite dans une procédure stockée ou un déclencheur ;  
-   NULL si l’appel a lieu en dehors de l’étendue d’un bloc CATCH.  
  
## <a name="remarks"></a>Remarques  
Un appel à `ERROR_LINE` peut se produire n’importe où dans l’étendue d’un bloc CATCH.  
  
`ERROR_LINE` retourne le numéro de la ligne de survenue de l’erreur. Cela se produit quel que soit l’emplacement de l’appel à `ERROR_LINE` dans l’étendue du bloc CATCH et quel que soit le nombre d’appels à `ERROR_LINE`. Ce comportement contraste avec celui de fonctions comme @@ERROR. @@ERROR retourne un numéro d’erreur dans l’instruction immédiatement après celle qui a provoqué une erreur ou dans la première instruction d’un bloc CATCH.  
  
Dans les blocs CATCH imbriqués, `ERROR_LINE` retourne le numéro de ligne de l’erreur spécifique à l’étendue du bloc CATCH référencé. Par exemple, le bloc CATCH d’une construction TRY...CATCH peut contenir une construction TRY...CATCH imbriquée. Dans un bloc CATCH imbriqué, `ERROR_LINE` retourne le numéro de ligne de l’erreur qui a appelé le bloc CATCH imbriqué. Si `ERROR_LINE` s’exécute dans le bloc CATCH externe, elle retourne le numéro de ligne de l’erreur qui a appelé ce bloc CATCH spécifique.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-using-error_line-in-a-catch-block"></a>R. Utilisation de ERROR_LINE dans un bloc CATCH  
L’exemple de code suivant présente une instruction `SELECT` qui génère une erreur de division par zéro. `ERROR_LINE` retourne le numéro de la ligne où l’erreur s’est produite.  
  
```sql  
BEGIN TRY  
    -- Generate a divide-by-zero error.  
    SELECT 1/0;  
END TRY  
BEGIN CATCH  
    SELECT ERROR_LINE() AS ErrorLine;  
END CATCH;  
GO  
```  
  
### <a name="b-using-error_line-in-a-catch-block-with-a-stored-procedure"></a>B. Utilisation de ERROR_LINE dans un bloc CATCH avec une procédure stockée  
L’exemple suivant illustre une procédure stockée qui génère une erreur de division par zéro. `ERROR_LINE` retourne le numéro de la ligne où l’erreur s’est produite.  
  
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
    SELECT ERROR_LINE() AS ErrorLine;  
END CATCH;  
GO  
``` 


### <a name="c-using-error_line-in-a-catch-block-with-other-error-handling-tools"></a>C. Utilisation de ERROR_LINE dans un bloc CATCH avec d'autres outils de traitement des erreurs  
L’exemple de code suivant présente une instruction `SELECT` qui génère une erreur de division par zéro. `ERROR_LINE` retourne le numéro de la ligne où l’erreur s’est produite et les informations relatives à l’erreur elle-même.  
  
```sql  
BEGIN TRY  
    -- Generate a divide-by-zero error.  
    SELECT 1/0;  
END TRY  
BEGIN CATCH  
    SELECT  
        ERROR_NUMBER() AS ErrorNumber,  
        ERROR_SEVERITY() AS ErrorSeverity,  
        ERROR_STATE() AS ErrorState,  
        ERROR_PROCEDURE() AS ErrorProcedure,  
        ERROR_LINE() AS ErrorLine,  
        ERROR_MESSAGE() AS ErrorMessage;  
END CATCH;  
GO  
``` 
  
## <a name="see-also"></a>Voir aussi  
 [TRY...CATCH &#40;Transact-SQL&#41;](../../t-sql/language-elements/try-catch-transact-sql.md)   
 [sys.messages &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/messages-for-errors-catalog-views-sys-messages.md)   
 [ERROR_NUMBER &#40;Transact-SQL&#41;](../../t-sql/functions/error-number-transact-sql.md)   
 [ERROR_MESSAGE &#40;Transact-SQL&#41;](../../t-sql/functions/error-message-transact-sql.md)   
 [ERROR_PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/functions/error-procedure-transact-sql.md)   
 [ERROR_SEVERITY &#40;Transact-SQL&#41;](../../t-sql/functions/error-severity-transact-sql.md)   
 [ERROR_STATE &#40;Transact-SQL&#41;](../../t-sql/functions/error-state-transact-sql.md)   
 [RAISERROR &#40;Transact-SQL&#41;](../../t-sql/language-elements/raiserror-transact-sql.md)   
 [@@ERROR &#40;Transact-SQL&#41;](../../t-sql/functions/error-transact-sql.md)  
  
  
