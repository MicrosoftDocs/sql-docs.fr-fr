---
description: sp_password (Transact-SQL)
title: sp_password (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_password
- sp_password_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_password
ms.assetid: 0ecbec81-e637-44a9-a61e-11bf060ef084
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: f154661d7cd03edfde83dd7774685e73e79aeb69
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195424"
---
# <a name="sp_password-transact-sql"></a>sp_password (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Ajoute ou modifie un mot de passe pour une [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] connexion.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Utilisez à la place [ALTER LOGIN](../../t-sql/statements/alter-login-transact-sql.md) .  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_password [ [ @old = ] 'old_password' , ]  
     { [ @new =] 'new_password' }  
     [ , [ @loginame = ] 'login' ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @old = ] 'old_password'` Est l’ancien mot de passe. *OLD_PASSWORD* est de **type sysname**, avec NULL comme valeur par défaut.  
  
`[ @new = ] 'new_password'` Nouveau mot de passe. *new_password* est de **type sysname**, sans valeur par défaut. *OLD_PASSWORD* doit être spécifié si les paramètres nommés ne sont pas utilisés.  
  
> [!IMPORTANT]  
>  N'utilisez pas de mot de passe NULL, Utilisez un mot de passe fort. Pour plus d’informations, consultez [Strong Passwords](../../relational-databases/security/strong-passwords.md).  
  
`[ @loginame = ] 'login'` Nom de la connexion affectée par la modification du mot de passe. *login* est de type **sysname**, avec NULL comme valeur par défaut. la *connexion* doit déjà exister et ne peut être spécifiée que par les membres des rôles serveur fixes **sysadmin** ou **securityadmin** .  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_password** appelle ALTER LOGIN. Cette instruction prend en charge d'autres options. Pour plus d’informations sur la modification des mots de passe, consultez [ALTER LOGIN &#40;Transact-SQL&#41;](../../t-sql/statements/alter-login-transact-sql.md).  
  
 **sp_password** ne peut pas être exécutée dans une transaction définie par l’utilisateur.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'autorisation ALTER ANY LOGIN. Nécessite également l'autorisation CONTROL SERVER pour réinitialiser un mot de passe sans fournir l'ancien mot de passe ou si la connexion en cours de modification détient l'autorisation CONTROL SERVER.  
  
 Un principal peut modifier son propre mot de passe.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-changing-the-password-of-a-login-without-knowing-the-old-password"></a>R. Modification du mot de passe d'une connexion sans disposer de l'ancien  
 L'exemple suivant montre l'utilisation de `ALTER LOGIN` pour remplacer le mot de passe de la connexion `Victoria` par `B3r1000d#2-36`. Ceci est la méthode privilégiée. L'utilisateur qui exécute cette commande doit avoir l'autorisation CONTROL SERVER.  
  
```  
ALTER LOGIN Victoria WITH PASSWORD = 'B3r1000d#2-36';  
GO  
```  
  
### <a name="b-changing-a-password"></a>B. Modification d'un mot de passe  
 L'exemple suivant montre l'utilisation de `ALTER LOGIN` pour changer le mot de passe de la connexion `Victoria` de `B3r1000d#2-36` en `V1cteAmanti55imE`. Ceci est la méthode privilégiée. L'utilisateur `Victoria` peut exécuter cette commande sans autorisations supplémentaires. Les autres utilisateurs ont besoin de l'autorisation ALTER ANY LOGIN.  
  
```  
ALTER LOGIN Victoria WITH   
     PASSWORD = 'V1cteAmanti55imE'   
     OLD_PASSWORD = 'B3r1000d#2-36';  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées de sécurité &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [ALTER LOGIN &#40;Transact-SQL&#41;](../../t-sql/statements/alter-login-transact-sql.md)   
 [CREATE LOGIN &#40;Transact-SQL&#41;](../../t-sql/statements/create-login-transact-sql.md)   
 [sp_addlogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addlogin-transact-sql.md)   
 [sp_adduser &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-adduser-transact-sql.md)   
 [sp_grantlogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-grantlogin-transact-sql.md)   
 [sp_revokelogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-revokelogin-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
