---
description: ORIGINAL_LOGIN (Transact-SQL)
title: ORIGINAL_LOGIN (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ORIGINAL_LOGIN_TSQL
- ORIGINAL_LOGIN
dev_langs:
- TSQL
helpviewer_keywords:
- logins [SQL Server], context switches
- context switching [SQL Server], login names
- original login names [SQL Server]
- ORIGINAL_LOGIN function
- names [SQL Server], logins
ms.assetid: ddfb0991-cde3-4b97-a5b7-ee450133f160
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: b7409baa8bcdac97746f8da2751a16e323c77850
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183929"
---
# <a name="original_login-transact-sql"></a>ORIGINAL_LOGIN (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Renvoie le nom de la connexion qui s'est connectée à l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Vous pouvez utiliser cette fonction pour renvoyer l'identité de la connexion d'origine dans les sessions où il y a un grand nombre de changements de contexte implicites ou explicites.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
ORIGINAL_LOGIN( )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Types de retour
 **sysname**  
  
## <a name="remarks"></a>Notes  
 Cette fonction peut s'avérer utile pour auditer l'identité du contexte de connexion d'origine. Alors que les fonctions telles que [SESSION_USER](../../t-sql/functions/session-user-transact-sql.md) et [CURRENT_USER](../../t-sql/functions/current-user-transact-sql.md) renvoient le contexte d’exécution actuel, ORIGINAL_LOGIN renvoie l’identité de la première connexion à l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans la session.  
 
  
## <a name="examples"></a>Exemples  
 L'exemple suivant change le contexte d'exécution de la session actuelle de l'appelant des instructions vers `login1`. Les fonctions `SUSER_SNAME` et `ORIGINAL_LOGIN` sont utilisées pour renvoyer l'utilisateur de la session actuelle (l'utilisateur vers lequel le contexte a été basculé) et le compte de connexion d'origine. 
 
  >[!NOTE]
  > Bien que la fonction ORIGINAL_LOGIN soit prise en charge sur Azure SQL Database, le script suivant échoue, car *Execute as LOGIN* n’est pas pris en charge sur Azure SQL Database. 
  
```sql  
USE AdventureWorks2012;  
GO  
--Create a temporary login and user.  
CREATE LOGIN login1 WITH PASSWORD = 'J345#$)thb';  
CREATE USER user1 FOR LOGIN login1;  
GO  
--Execute a context switch to the temporary login account.  
DECLARE @original_login sysname;  
DECLARE @current_context sysname;  
EXECUTE AS LOGIN = 'login1';  
SET @original_login = ORIGINAL_LOGIN();  
SET @current_context = SUSER_SNAME();  
SELECT 'The current executing context is: '+ @current_context;  
SELECT 'The original login in this session was: '+ @original_login  
GO  
-- Return to the original execution context  
-- and remove the temporary principal.  
REVERT;  
GO  
DROP LOGIN login1;  
DROP USER user1;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [EXECUTE AS &#40;Transact-SQL&#41;](../../t-sql/statements/execute-as-transact-sql.md)   
 [REVERT &#40;Transact-SQL&#41;](../../t-sql/statements/revert-transact-sql.md)  
  
  
