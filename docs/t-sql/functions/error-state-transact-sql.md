---
description: ERROR_STATE (Transact-SQL)
title: ERROR_STATE (Transact-SQL)
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: sql-data-warehouse, pdw, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ERROR_STATE_TSQL
- ERROR_STATE
dev_langs:
- TSQL
helpviewer_keywords:
- messages [SQL Server], state
- ERROR_STATE function
- errors [SQL Server], state
- TRY...CATCH [SQL Server]
- CATCH block
- states [SQL Server], error numbers
ms.assetid: 6059af00-83fe-409f-ab7c-daad111bc671
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 9cef172dff8e103a8ece7a8d704cdd7b009c08d4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99158745"
---
# <a name="error_state-transact-sql"></a>ERROR_STATE (Transact-SQL)

[!INCLUDE [sql-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdbmi-asa-pdw.md)]

  Retourne le numéro d’état de l’erreur qui a provoqué l’exécution du bloc CATCH d’une construction TRY...CATCH.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql  
ERROR_STATE ( )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Types de retour
 **int**  
  
## <a name="return-value"></a>Valeur de retour  
 Lorsqu'elle est appelée dans un bloc CATCH, elle renvoie le numéro d'état du message d'erreur à l'origine de l'exécution du bloc CATCH.  
  
 Retourne NULL si l'appel a lieu en dehors de l'étendue d'un bloc CATCH.  
  
## <a name="remarks"></a>Remarques  
 Certains messages d'erreur peuvent apparaître en de multiples endroits du code du [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssDE](../../includes/ssde-md.md)]. L'erreur « 1105 » par exemple, peut être provoquée par différentes conditions. Chaque condition qui génère l'erreur attribue un code d'état unique.  
  
 Lorsque vous consultez des bases de données de problèmes connus, telles que la Base de connaissances [!INCLUDE[msCoName](../../includes/msconame-md.md)], vous pouvez utiliser le numéro d'état pour voir si le problème enregistré est identique à l'erreur que vous avez rencontrée. Par exemple, si un article de la Base de connaissances traite de l'erreur 1105 avec un état 2 et que l'erreur 1105 reçue était associée à un état 3, votre erreur avait probablement une autre cause que celle mentionnée dans l'article.  
  
 Un ingénieur technique de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut également utiliser le code d'état pour déterminer l'emplacement du code d'erreur dans le code source et réaliser ainsi plus facilement un diagnostic.  
  
 ERROR_STATE peut être appelée depuis n'importe quel emplacement dans le champ d'un bloc CATCH.  
  
 ERROR_STATE renvoie l'état de l'erreur, indépendamment du nombre d'exécutions ou de l'endroit où elle est exécutée dans le champ du bloc CATCH. En revanche, des fonctions telles que @@ERROR renvoient uniquement le numéro d’erreur dans l’instruction qui suit celle à l’origine d’une erreur ou dans la première instruction d’un bloc CATCH.  
  
 Dans les blocs CATCH imbriqués, ERROR_STATE renvoie l'état d'erreur propre au champ du bloc CATCH dans lequel elle est référencée. Par exemple, le bloc CATCH d'une construction TRY...CATCH externe peut comporter une construction TRY...CATCH imbriquée. Dans un bloc CATCH imbriqué, ERROR_STATE renvoie l'état depuis l'erreur qui a appelé ce bloc. Si ERROR_STATE est exécutée dans le bloc CATCH externe, elle renvoie l'état à partir de l'erreur qui a appelé ce bloc CATCH.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-using-error_state-in-a-catch-block"></a>R. Utilisation d'ERROR_STATE dans un bloc CATCH  
 L’exemple suivant montre une instruction `SELECT` qui génère une erreur de division par zéro. L'état de l'erreur est renvoyé.  
  
```sql  
BEGIN TRY  
    -- Generate a divide by zero error  
    SELECT 1/0;  
END TRY  
BEGIN CATCH  
    SELECT ERROR_STATE() AS ErrorState;  
END CATCH;  
GO  
```  
  
### <a name="b-using-error_state-in-a-catch-block-with-other-error-handling-tools"></a>B. Utilisation d'ERROR_STATE dans un bloc CATCH avec d'autres outils de gestion des erreurs  
 L’exemple suivant montre une instruction `SELECT` qui génère une erreur de division par zéro. Outre l'état de l'erreur, la procédure renvoie également les informations relatives à l'erreur.  
  
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
  
## <a name="examples-sssdwfull-and-sspdw"></a>Exemples : [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="c-using-error_state-in-a-catch-block-with-other-error-handling-tools"></a>C. Utilisation d'ERROR_STATE dans un bloc CATCH avec d'autres outils de gestion des erreurs  
 L’exemple suivant montre une instruction `SELECT` qui génère une erreur de division par zéro. Outre l'état de l'erreur, la procédure renvoie également les informations relatives à l'erreur.  
  
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
        ERROR_MESSAGE() AS ErrorMessage;  
END CATCH;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [sys.messages &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/messages-for-errors-catalog-views-sys-messages.md)   
 [TRY...CATCH &#40;Transact-SQL&#41;](../../t-sql/language-elements/try-catch-transact-sql.md)   
 [ERROR_LINE &#40;Transact-SQL&#41;](../../t-sql/functions/error-line-transact-sql.md)   
 [ERROR_MESSAGE &#40;Transact-SQL&#41;](../../t-sql/functions/error-message-transact-sql.md)   
 [ERROR_NUMBER &#40;Transact-SQL&#41;](../../t-sql/functions/error-number-transact-sql.md)   
 [ERROR_PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/functions/error-procedure-transact-sql.md)   
 [ERROR_SEVERITY &#40;Transact-SQL&#41;](../../t-sql/functions/error-severity-transact-sql.md)   
 [RAISERROR &#40;Transact-SQL&#41;](../../t-sql/language-elements/raiserror-transact-sql.md)   
 [@@ERROR &#40;Transact-SQL&#41;](../../t-sql/functions/error-transact-sql.md)    
 [Guide de référence des erreurs et des événements &#40;Moteur de base de donnéese&#41;](../../relational-databases/errors-events/errors-and-events-reference-database-engine.md)     
  
    

