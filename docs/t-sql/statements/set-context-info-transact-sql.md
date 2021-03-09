---
description: SET CONTEXT_INFO (Transact-SQL)
title: SET CONTEXT_INFO (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/26/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SET_CONTEXT_INFO_TSQL
- SET CONTEXT_INFO
dev_langs:
- TSQL
helpviewer_keywords:
- context information [SQL Server]
- CONTEXT_INFO option
- SET CONTEXT_INFO statement
ms.assetid: a0b7b9f3-dbda-4350-a274-bd9ecd5c0a74
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 271cf281b2e3dc5f015dd02bc1778fab24be0794
ms.sourcegitcommit: 15c7cd187dcff9fc91f2daf0056b12ed3f0403f0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/08/2021
ms.locfileid: "102464147"
---
# <a name="set-context_info-transact-sql"></a>SET CONTEXT_INFO (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Associe jusqu'à 128 octets d'informations binaires à la session ou à la connexion actuelle.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
  
SET CONTEXT_INFO { binary_str | @binary_var }  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *binary_str*  
 Représente une constante **binary** ou une constante implicitement convertible en **binary** à associer à la session ou à la connexion active.  
  
 **@** *binary_var*  
 Représente une variable **varbinary** ou **binary** contenant une valeur de contexte à associer à la session ou à la connexion actuelle.  
  
## <a name="remarks"></a>Remarques  
 La méthode privilégiée pour récupérer les informations de contexte pour la session actuelle consiste à utiliser la fonction CONTEXT_INFO. Les informations relatives au contexte de la session sont stockées dans les colonnes **context_info** des vues système suivantes :  
  
-   **sys.dm_exec_requests**  
  
-   **sys.dm_exec_sessions**  
  
-   **sys.sysprocesses**  
  
 SET CONTEXT_INFO ne peut pas être spécifié dans une fonction définie par l'utilisateur. Vous ne pouvez pas fournir une valeur NULL dans SET CONTEXT_INFO, car les vues contenant les valeurs n'autorisent pas ce type de valeurs.  
  
 SET CONTEXT_INFO n'accepte pas les expressions autres que les constantes ou les noms de variables. Pour définir les informations de contexte associées au résultat d’un appel de fonction, vous devez d’abord inclure le résultat de l’appel de la fonction dans une variable **binary** ou **varbinary**.  
  
 Lorsque vous spécifiez SET CONTEXT_INFO dans une procédure stockée ou un déclencheur, contrairement aux autres instructions SET, la nouvelle valeur définie pour les informations de contexte est conservée à l'issue de l'exécution de la procédure stockée ou du déclencheur.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-setting-context-information-by-using-a-constant"></a>R. Définition des informations de contexte à l'aide d'une constante  
 L'exemple ci-dessous illustre `SET CONTEXT_INFO` par la définition de la valeur et l'affichage des résultats. Notez que l'interrogation de `sys.dm_exec_sessions` nécessite les autorisations SELECT et VIEW SERVER STATE, ce qui n'est pas le cas pour la fonction CONTEXT_INFO.  
  
```sql
SET CONTEXT_INFO 0x01010101;  
GO  
SELECT context_info   
FROM sys.dm_exec_sessions  
WHERE session_id = @@SPID;  
GO  
```  
  
### <a name="b-setting-context-information-by-using-a-function"></a>B. Définition des informations de contexte à l'aide d'une fonction  
 L’exemple ci-dessous illustre l’utilisation du résultat d’une fonction pour définir la valeur de contexte, où la valeur de la fonction doit être placée d’abord dans une variable **binary**.  
  
```sql
DECLARE @BinVar varbinary(128);  
SET @BinVar = CAST(REPLICATE( 0x20, 128 ) AS varbinary(128) );  
SET CONTEXT_INFO @BinVar;  
  
SELECT CONTEXT_INFO() AS MyContextInfo;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Sécurité au niveau des lignes](../../relational-databases/security/row-level-security.md) [Instructions SET &#40;Transact-SQL&#41;](../../t-sql/statements/set-statements-transact-sql.md)   
 [CONTEXT_INFO  &#40;Transact-SQL&#41;](../../t-sql/functions/context-info-transact-sql.md)  
 [SESSION_CONTEXT  &#40;Transact-SQL&#41;](../../t-sql/functions/session-context-transact-sql.md)  
 [sys.dm_exec_requests &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)   
 [sys.dm_exec_sessions &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sessions-transact-sql.md)   
 [sp_set_session_context  &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-set-session-context-transact-sql.md)  
  
