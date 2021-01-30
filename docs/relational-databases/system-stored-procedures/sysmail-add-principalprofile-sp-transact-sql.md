---
description: sysmail_add_principalprofile_sp (Transact-SQL)
title: sysmail_add_principalprofile_sp (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sysmail_add_principalprofile_sp_TSQL
- sysmail_add_principalprofile_sp
dev_langs:
- TSQL
helpviewer_keywords:
- sysmail_add_principalprofile_sp
ms.assetid: b2a0b313-abb9-4c23-8511-db77ca8172b3
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 0252e299f072144999e3c2db1b2404de9aa9b6a8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183029"
---
# <a name="sysmail_add_principalprofile_sp-transact-sql"></a>sysmail_add_principalprofile_sp (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Accorde l’autorisation à un principal de base de données msdb d’utiliser un profil de Database Mail. Le principal de la base de données doit être mappé à un utilisateur d’authentification SQL Server, un utilisateur Windows ou un groupe Windows.
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sysmail_add_principalprofile_sp  { [ @principal_id = ] principal_id | [ @principal_name = ] 'principal_name' } ,  
    { [ @profile_id = ] profile_id | [ @profile_name = ] 'profile_name' }  
    [ , [ @is_default ] = 'is_default' ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @principal_id = ] principal_id` ID de l’utilisateur ou du rôle de base de données dans la base de données **msdb** pour l’Association. *principal_id* est de **type int**, avec NULL comme valeur par défaut. *Principal_id* ou *principal_name* doivent être spécifiés. Une *principal_id* de **0** fait de ce profil un profil public, en accordant l’accès à tous les principaux de la base de données.  
  
`[ @principal_name = ] 'principal_name'` Nom de l’utilisateur ou du rôle de base de données dans la base de données **msdb** pour l’Association. *principal_name* est de **type sysname**, avec NULL comme valeur par défaut. *Principal_id* ou *principal_name* doivent être spécifiés. Une *principal_name* de **« public »** fait de ce profil un profil public, en accordant l’accès à tous les principaux de la base de données.  
  
`[ @profile_id = ] profile_id` ID du profil pour l’Association. *profile_id* est de **type int**, avec NULL comme valeur par défaut. *Profile_id* ou *profile_name* doivent être spécifiés.  
  
`[ @profile_name = ] 'profile_name'` Nom du profil pour l’Association. *profile_name* est de **type sysname**, sans valeur par défaut. *Profile_id* ou *profile_name* doivent être spécifiés.  
  
`[ @is_default = ] is_default` Spécifie si ce profil est le profil par défaut pour le principal. Un principal ne peut avoir qu'un seul profil par défaut. *is_default* est de **bits**, sans valeur par défaut.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 Pour rendre un profil public, spécifiez un **\@ principal_id** de **0** ou un **\@ principal_name** de **public**. Un profil public est disponible pour tous les utilisateurs de la base de données **msdb** , bien que les utilisateurs doivent également être membres de **DatabaseMailUserRole** pour exécuter **sp_send_dbmail**.  
  
 Un utilisateur de base de données ne peut avoir plus d'un seul profil par défaut. Lorsque **\@ is_default** a la valeur «**1**» et que l’utilisateur est déjà associé à un ou plusieurs profils, le profil spécifié devient le profil par défaut de l’utilisateur. L'ancien profil par défaut est toujours associé à l'utilisateur, mais n'est plus le profil par défaut.  
  
 Lorsque **\@ is_default** a la **valeur « 0**» et qu’aucune autre association n’existe, la procédure stockée retourne une erreur.  
  
 La procédure stockée **sysmail_add_principalprofile_sp** se trouve dans la base de données **msdb** et appartient au schéma **dbo** . La procédure doit être exécutée avec un nom en trois parties si la base de données actuelle n’est pas **msdb**.  
  
## <a name="permissions"></a>Autorisations  
 Les autorisations d’exécution pour cette procédure sont octroyées par défaut aux membres du rôle serveur fixe **sysadmin** .  
  
## <a name="examples"></a>Exemples  
 **A. Création d'une association et définition du profil par défaut**  
  
 L’exemple suivant crée une association entre le profil nommé `AdventureWorks Administrator Profile` et l’utilisateur de base de données **msdb** `ApplicationUser` . Le profil est le profil par défaut de l'utilisateur.  
  
```  
EXECUTE msdb.dbo.sysmail_add_principalprofile_sp  
    @principal_name = 'ApplicationUser',  
    @profile_name = 'AdventureWorks Administrator Profile',  
    @is_default = 1 ;  
```  
  
 **B. Modification d'un profil en profil public par défaut**  
  
 L’exemple suivant définit le profil comme `AdventureWorks Public Profile` profil public par défaut pour les utilisateurs de la base de données **msdb** .  
  
```  
EXECUTE msdb.dbo.sysmail_add_principalprofile_sp  
    @principal_name = 'public',  
    @profile_name = 'AdventureWorks Public Profile',  
    @is_default = 1 ;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Messagerie de base de données](../../relational-databases/database-mail/database-mail.md)   
 [Objets de configuration Database Mail](../../relational-databases/database-mail/database-mail-configuration-objects.md)   
 [Database Mail des procédures stockées &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/database-mail-stored-procedures-transact-sql.md)  
  
  
