---
description: sp_grant_login_to_proxy (Transact-SQL)
title: sp_grant_login_to_proxy (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_grant_login_to_proxy
- sp_grant_login_to_proxy_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_grant_login_to_proxy
ms.assetid: 90e1a6d5-a692-4462-a163-4b0709d83150
ms.author: vanto
author: VanMSFT
ms.openlocfilehash: c59f8b19e63f491757386ba5562ea9e0106e92a8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159805"
---
# <a name="sp_grant_login_to_proxy-transact-sql"></a>sp_grant_login_to_proxy (Transact-SQL)

[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Accorde à un principal de sécurité les droits d'accès à un proxy.  

  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
sp_grant_login_to_proxy   
     { [ @login_name = ] 'login_name'   
     | [ @fixed_server_role = ] 'fixed_server_role'   
     | [ @msdb_role = ] 'msdb_role' } ,   
     { [ @proxy_id = ] id | [ @proxy_name = ] 'proxy_name' }  
```  
  
## <a name="arguments"></a>Arguments  
`[ @login_name = ] 'login_name'` Nom de connexion auquel accorder l’accès. Le *login_name* est de type **nvarchar (256)**, avec NULL comme valeur par défaut. L’une des **\@ login_name**, **\@ fixed_server_role** ou **\@ msdb_role** doit être spécifiée, sinon la procédure stockée échoue.  
  
`[ @fixed_server_role = ] 'fixed_server_role'` Rôle serveur fixe auquel accorder l’accès. Le *fixed_server_role* est de type **nvarchar (256)**, avec NULL comme valeur par défaut. L’une des **\@ login_name**, **\@ fixed_server_role** ou **\@ msdb_role** doit être spécifiée, sinon la procédure stockée échoue.  
  
`[ @msdb_role = ] 'msdb_role'` Rôle de base de données dans la base de données **msdb** à laquelle accorder l’accès. Le *msdb_role* est de type **nvarchar (256)**, avec NULL comme valeur par défaut. L’une des **\@ login_name**, **\@ fixed_server_role** ou **\@ msdb_role** doit être spécifiée, sinon la procédure stockée échoue.  
  
`[ @proxy_id = ] id` Identificateur du proxy pour lequel l’accès doit être accordé. L' *ID* est de **type int**, avec NULL comme valeur par défaut. L’une des **\@ proxy_id** ou **\@ proxy_name** doit être spécifiée, sinon la procédure stockée échoue.  
  
`[ @proxy_name = ] 'proxy_name'` Nom du proxy pour lequel l’accès doit être accordé. Le *proxy_name* est de type **nvarchar (256)**, avec NULL comme valeur par défaut. L’une des **\@ proxy_id** ou **\@ proxy_name** doit être spécifiée, sinon la procédure stockée échoue.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_grant_login_to_proxy** doit être exécuté à partir de la base de données **msdb** .  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** peuvent exécuter **sp_grant_login_to_proxy**.  
  
## <a name="examples"></a>Exemples  
 L’exemple suivant permet à la connexion `adventure-works\terrid` d’utiliser le proxy `Catalog application proxy` .  
  
```sql
USE msdb ;  
GO  
  
EXEC dbo.sp_grant_login_to_proxy  
    @login_name = N'adventure-works\terrid',  
    @proxy_name = N'Catalog application proxy' ;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [CREATE LOGIN &#40;Transact-SQL&#41;](../../t-sql/statements/create-login-transact-sql.md)   
 [sp_add_proxy &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-proxy-transact-sql.md)   
 [sp_revoke_login_from_proxy &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-revoke-login-from-proxy-transact-sql.md)  
  
  
