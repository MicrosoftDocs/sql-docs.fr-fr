---
description: sp_adduser (Transact-SQL)
title: sp_adduser (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_adduser
- sp_adduser_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_adduser
ms.assetid: 61a40eb4-573f-460c-9164-bd1bbfaf8b25
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: c250837ad65e0b14fc5612eae69be6f93d8f23e6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198364"
---
# <a name="sp_adduser-transact-sql"></a>sp_adduser (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Ajoute un nouvel utilisateur dans la base de données active.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Utilisez à la place [Create User](../../t-sql/statements/create-user-transact-sql.md) .  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_adduser [ @loginame = ] 'login'   
    [ , [ @name_in_db = ] 'user' ]   
    [ , [ @grpname = ] 'role' ]   
```  
  
## <a name="arguments"></a>Arguments  
`[ @loginame = ] 'login'` Nom de la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] connexion ou de la connexion Windows. *login* est de **type sysname** et n’a pas de valeur par défaut. la *connexion* doit être une connexion existante ou une connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Windows.  
  
`[ @name_in_db = ] 'user'` Nom du nouvel utilisateur de base de données. *User* est de **type sysname**, avec NULL comme valeur par défaut. Si *User* n’est pas spécifié, le nom du nouvel utilisateur de base de données est défini par défaut sur le nom de *connexion* . La spécification de l' *utilisateur* donne au nouvel utilisateur un nom dans la base de données différent du nom de connexion au niveau du serveur.  
  
`[ @grpname = ] 'role'` Rôle de base de données dont le nouvel utilisateur devient membre. *role* est de **type sysname**, avec NULL comme valeur par défaut. le *rôle* doit être un rôle de base de données valide dans la base de données actuelle.  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_adduser** créera également un schéma qui porte le nom de l’utilisateur.  
  
 Une fois qu'un utilisateur a été ajouté, utilisez les instructions GRANT, DENY et REVOKE afin de définir les autorisations contrôlant les activités effectuées par l'utilisateur.  
  
 Utilisez **sys.server_principals** pour afficher la liste des noms de connexion valides.  
  
 Utilisez **sp_helprole** pour afficher la liste des noms de rôle valides. Lorsque vous spécifiez un rôle, l'utilisateur obtient automatiquement les autorisations définies pour ce rôle. Si aucun rôle n’est spécifié, l’utilisateur obtient les autorisations accordées au rôle **public** par défaut. Pour ajouter un utilisateur à un rôle, une valeur doit être fournie pour le *nom d’utilisateur* . (le *nom d’utilisateur* peut être le même que *login_id*.)  
  
 L' **invité** de l’utilisateur existe déjà dans chaque base de données. L’ajout de l’utilisateur **invité** activera cet utilisateur, s’il a été précédemment désactivé. Par défaut, l’utilisateur **invité** est désactivé dans les nouvelles bases de données.  
  
 **sp_adduser** ne peut pas être exécutée dans une transaction définie par l’utilisateur.  
  
 Vous ne pouvez pas ajouter un utilisateur **invité** , car un utilisateur **invité** existe déjà dans chaque base de données. Pour activer l’utilisateur **invité** , accordez l’autorisation Connect **invitée** comme indiqué ci-dessous :  
  
```  
GRANT CONNECT TO guest;  
GO  
```  
  
## <a name="permissions"></a>Autorisations  
 Il faut être propriétaire de la base de données.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-adding-a-database-user"></a>R. Ajout d'un utilisateur de base de données  
 L'exemple suivant ajoute l'utilisateur de base de données `Vidur` au rôle `Recruiting` existant dans la base de données active, en utilisant la connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]`Vidur` existante.  
  
```  
EXEC sp_adduser 'Vidur', 'Vidur', 'Recruiting';  
```  
  
### <a name="b-adding-a-database-user-with-the-same-login-id"></a>B. Ajout d'un utilisateur de base de données avec le même ID de connexion  
 L'exemple suivant ajoute l'utilisateur `Arvind` à la base de données active pour la connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]`Arvind`. Cet utilisateur appartient au rôle **public** par défaut.  
  
```  
EXEC sp_adduser 'Arvind';  
```  
  
### <a name="c-adding-a-database-user-with-a-different-name-than-its-server-level-login"></a>C. Ajout d'un utilisateur de base de données avec un nom différent de sa connexion de niveau serveur  
 L'exemple suivant ajoute la connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]`BjornR` à la base de données active, qui a un nom d'utilisateur `Bjorn`, et ajoute l'utilisateur `Bjorn` dans le rôle de base de données `Production`.  
  
```  
EXEC sp_adduser 'BjornR', 'Bjorn', 'Production';  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées de sécurité &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [sys.server_principals &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md)   
 [sp_addrole &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addrole-transact-sql.md)   
 [CREATE USER &#40;Transact-SQL&#41;](../../t-sql/statements/create-user-transact-sql.md)   
 [sp_dropuser &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropuser-transact-sql.md)   
 [sp_grantdbaccess &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-grantdbaccess-transact-sql.md)   
 [sp_grantlogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-grantlogin-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
