---
description: ERROR_MESSAGE (Transact-SQL)
title: ERROR_MESSAGE (Transact-SQL)
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ERROR_MESSAGE_TSQL
- ERROR_MESSAGE
dev_langs:
- TSQL
helpviewer_keywords:
- ERROR_MESSAGE function
- errors [SQL Server], text of
- messages [SQL Server], text of
- TRY...CATCH [SQL Server]
- CATCH block
ms.assetid: f32877a6-5f17-418c-a32c-5a1a344b3c45
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 774a6ddf83d324c23da239a75fdad6f82d090a5b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99102676"
---
# <a name="error_message-transact-sql"></a>ERROR_MESSAGE (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Cette fonction retourne le texte du message de l’erreur qui a provoqué l’exécution du bloc CATCH d’une construction TRY...CATCH.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql  
ERROR_MESSAGE ( )   
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Types de retour
 **nvarchar(4000)**  
  
## <a name="return-value"></a>Valeur renvoyée  
Quand elle est appelée dans un bloc CATCH, la fonction `ERROR_MESSAGE` retourne le texte complet du message d’erreur qui a provoqué l’exécution du bloc `CATCH`. Le texte comprend les valeurs fournies pour tous les paramètres remplaçables (par exemple, les longueurs, les noms d’objets ou les heures).  
  
`ERROR_MESSAGE` retourne NULL quand l’appel a lieu en dehors de l’étendue d’un bloc CATCH.  
  
## <a name="remarks"></a>Notes  
`ERROR_MESSAGE` prend en charge les appels à partir de n’importe quel emplacement dans l’étendue d’un bloc CATCH.  
  
`ERROR_MESSAGE` retourne un message d’erreur pertinent, quel que soit le nombre de fois où elle s’exécute, ou l’emplacement de son exécution dans l’étendue du bloc `CATCH`. Ce comportement contraste avec celui d’une fonction comme @@ERROR, qui retourne uniquement un numéro d’erreur dans l’instruction immédiatement après celle qui a provoqué une erreur.  
  
Dans des blocs `CATCH` imbriqués, `ERROR_MESSAGE` retourne le message d’erreur spécifique à l’étendue du bloc `CATCH` qui a référencé ce bloc `CATCH`. Par exemple, le bloc `CATCH` d’une construction TRY...CATCH externe peut comporter une construction `TRY...CATCH` interne. À l’intérieur de ce bloc `CATCH` interne, `ERROR_MESSAGE` retourne le message de l’erreur qui a appelé le bloc `CATCH` interne. Si `ERROR_MESSAGE` s’exécute dans le bloc `CATCH` externe, elle retourne le message de l’erreur qui a appelé ce bloc `CATCH` externe.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-using-error_message-in-a-catch-block"></a>R. Utilisation de ERROR_MESSAGE dans un bloc CATCH  
L’exemple suivant montre une instruction `SELECT` qui génère une erreur de division par zéro. Le bloc `CATCH` retourne le message d’erreur.  
  
```sql   
BEGIN TRY  
    -- Generate a divide-by-zero error.  
    SELECT 1/0;  
END TRY  
BEGIN CATCH  
    SELECT ERROR_MESSAGE() AS ErrorMessage;  
END CATCH;  
GO  
```
[!INCLUDE[ssResult](../../includes/ssresult-md.md)] 
```
-----------

(0 row(s) affected)

ErrorMessage
----------------------------------
Divide by zero error encountered.

(1 row(s) affected)

```  
  
### <a name="b-using-error_message-in-a-catch-block-with-other-error-handling-tools"></a>B. Utilisation de ERROR_MESSAGE dans un bloc CATCH avec d'autres outils de traitement des erreurs  
L’exemple suivant montre une instruction `SELECT` qui génère une erreur de division par zéro. Outre le message de l’erreur, le bloc `CATCH` retourne des informations relatives à cette erreur.  
  
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
[!INCLUDE[ssResult](../../includes/ssresult-md.md)] 
```
-----------

(0 row(s) affected)

ErrorNumber ErrorSeverity ErrorState  ErrorProcedure  ErrorLine  ErrorMessage
----------- ------------- ----------- --------------- ---------- ----------------------------------
8134        16            1           NULL            4          Divide by zero error encountered.

(1 row(s) affected)

```
  
## <a name="see-also"></a>Voir aussi  
 [sys.messages &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/messages-for-errors-catalog-views-sys-messages.md)   
 [TRY...CATCH &#40;Transact-SQL&#41;](../../t-sql/language-elements/try-catch-transact-sql.md)   
 [ERROR_LINE &#40;Transact-SQL&#41;](../../t-sql/functions/error-line-transact-sql.md)   
 [ERROR_MESSAGE &#40;Transact-SQL&#41;](../../t-sql/functions/error-message-transact-sql.md)   
 [ERROR_PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/functions/error-procedure-transact-sql.md)   
 [ERROR_SEVERITY &#40;Transact-SQL&#41;](../../t-sql/functions/error-severity-transact-sql.md)   
 [ERROR_STATE &#40;Transact-SQL&#41;](../../t-sql/functions/error-state-transact-sql.md)   
 [RAISERROR &#40;Transact-SQL&#41;](../../t-sql/language-elements/raiserror-transact-sql.md)   
 [@@ERROR &#40;Transact-SQL&#41;](../../t-sql/functions/error-transact-sql.md)     
 [Guide de référence des erreurs et des événements &#40;Moteur de base de donnéese&#41;](../../relational-databases/errors-events/errors-and-events-reference-database-engine.md)     
  
    

