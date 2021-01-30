---
description: sp_msx_set_account (Transact-SQL)
title: sp_msx_set_account (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_msx_set_account
- sp_msx_set_account_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_msx_set_account
ms.assetid: 314ec720-3a37-48f7-bb6b-8d5b894bf843
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 29a92920e81f65beabdc0837b85d19484d2e35f0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192006"
---
# <a name="sp_msx_set_account-transact-sql"></a>sp_msx_set_account (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Définit le nom du compte et le mot de passe du serveur maître de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent sur le serveur cible.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_msx_set_account [ @credential_name = ] 'credential_name'  | [ @credential_id = ] credential_id  
```  
  
## <a name="arguments"></a>Arguments  
`[ @credential_name = ] 'credential_name'` Nom des informations d’identification à utiliser pour se connecter au serveur maître. Ce nom doit être celui d'informations d'identification existantes. *Credential_name* ou *credential_id* doivent être spécifiés.  
  
`[ @credential_id = ] credential_id` Identificateur des informations d’identification à utiliser pour se connecter au serveur maître. Il doit désigner des informations d'identification existantes. *Credential_name* ou *credential_id* doivent être spécifiés.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 Aucun.  
  
## <a name="remarks"></a>Notes  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilise des informations d'identification pour stocker le nom d'utilisateur et le mot de passe qu'un serveur cible utilise pour se connecter à un serveur maître. Cette procédure définit les informations d'identification utilisées par l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de ce serveur cible pour se connecter au serveur maître.  
  
 Il doit s'agir d'informations d'identification existantes. Pour plus d’informations sur la création d’informations d’identification, consultez [Create credential &#40;Transact-SQL&#41;](../../t-sql/statements/create-credential-transact-sql.md).  
  
## <a name="permissions"></a>Autorisations  
 Les autorisations d’exécution pour **sp_msx_set_account** par défaut aux membres du rôle serveur fixe **sysadmin** .  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant configure ce serveur afin qu'il utilise les informations d'identification `MsxAccount` pour la connexion au serveur maître.  
  
```  
USE msdb ;  
GO  
  
EXECUTE dbo.sp_msx_set_account @credential_name = MsxAccount ;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [SQL Server Agent des procédures stockées &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sql-server-agent-stored-procedures-transact-sql.md)   
 [CREATE CREDENTIAL &#40;Transact-SQL&#41;](../../t-sql/statements/create-credential-transact-sql.md)   
 [sp_msx_get_account &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-msx-get-account-transact-sql.md)  
  
  
