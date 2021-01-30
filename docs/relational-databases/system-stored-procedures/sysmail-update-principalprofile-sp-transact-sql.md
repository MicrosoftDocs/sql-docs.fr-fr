---
description: sysmail_update_principalprofile_sp (Transact-SQL)
title: sysmail_update_principalprofile_sp (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sysmail_update_principalprofile_sp
- sysmail_update_principalprofile_sp_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysmail_update_principalprofile_sp
ms.assetid: 9fe96e9a-4758-4e4a-baee-3e1217c4426c
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 85bab8892a9459ee5de954596ffea2be2a5aed92
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181866"
---
# <a name="sysmail_update_principalprofile_sp-transact-sql"></a>sysmail_update_principalprofile_sp (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Met à jour les informations d'une association entre un principal et un profil.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sysmail_update_principalprofile_sp { @principal_id = principal_id | @principal_name = 'principal_name' } ,  
    { [ @profile_id = ] profile_id | [ @profile_name = ] 'profile_name' } ,  
    [ @is_default = ] 'is_default'  
```  
  
## <a name="arguments"></a>Arguments  
`[ @principal_id = ] principal_id` ID de l’utilisateur ou du rôle de base de données dans la base de données **msdb** pour l’Association à modifier. *principal_id* est de **type int**, avec NULL comme valeur par défaut. *Principal_id* ou *principal_name* doivent être spécifiés.  
  
`[ @principal_name = ] 'principal_name'` Nom de l’utilisateur ou du rôle de base de données dans la base de données **msdb** pour l’Association à mettre à jour. *principal_name* est de **type sysname**, avec NULL comme valeur par défaut. *Principal_id* ou *principal_name* peuvent être spécifiés.  
  
`[ @profile_id = ] profile_id` ID du profil pour l’Association à modifier. *profile_id* est de **type int**, avec NULL comme valeur par défaut. *Profile_id* ou *profile_name* doivent être spécifiés.  
  
`[ @profile_name = ] 'profile_name'` Nom du profil pour l’Association à modifier. *profile_name* est de **type sysname**, avec NULL comme valeur par défaut. *Profile_id* ou *profile_name* doivent être spécifiés.  
  
`[ @is_default = ] 'is_default'` Indique si ce profil est le profil par défaut de l’utilisateur de base de données. Un utilisateur de base de données ne peut avoir plus d'un seul profil par défaut. *is_default* est de **bits**, sans valeur par défaut.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 None  
  
## <a name="remarks"></a>Notes  
 Cette procédure stockée est modifiée si le profil spécifié est ou non le profil par défaut pour l'utilisateur de la base de données. Un utilisateur de base de données peut avoir uniquement un profil privé par défaut.  
  
 Lorsque le nom principal de l’Association est **public** ou que l’ID du principal de l’Association est **0**, cette procédure stockée modifie le profil public. Il ne peut y avoir qu'un seul profil public par défaut.  
  
 Lorsque **\@ is_default** a la valeur «**1**» et que le principal est associé à plusieurs profils, le profil spécifié devient le profil par défaut pour le principal. Le précédent profil par défaut est toujours associé au principal, mais il ne représente plus le profil par défaut.  
  
 La procédure stockée **sysmail_update_principalprofile_sp** se trouve dans la base de données **msdb** et appartient au schéma **dbo** . La procédure doit être exécutée avec un nom en trois parties si la base de données actuelle n’est pas **msdb**.  
  
## <a name="permissions"></a>Autorisations  
 Les autorisations d’exécution pour cette procédure sont octroyées par défaut aux membres du rôle serveur fixe **sysadmin** .  
  
## <a name="examples"></a>Exemples  
 **A. Définition du profil public par défaut d'une base de données**  
  
 L’exemple suivant définit le profil comme `General Use Profile` profil public par défaut pour les utilisateurs de la base de données **msdb** .  
  
```  
EXECUTE msdb.dbo.sysmail_update_principalprofile_sp  
    @principal_name = 'public',  
    @profile_name = 'General Use Profile',  
    @is_default = '1';  
```  
  
 **B. Définition du profil privé par défaut d'un utilisateur**  
  
 L’exemple suivant définit le profil comme `AdventureWorks Administrator` profil par défaut pour le principal `ApplicationUser` dans la base de données **msdb** . Le profil doit déjà être associé au principal. Le précédent profil par défaut est toujours associé au principal, mais il ne représente plus le profil par défaut.  
  
```  
EXECUTE msdb.dbo.sysmail_update_principalprofile_sp  
    @principal_name = 'ApplicationUser',  
    @profile_name = 'AdventureWorks Administrator',  
    @is_default = '1' ;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Messagerie de base de données](../../relational-databases/database-mail/database-mail.md)   
 [Objets de configuration Database Mail](../../relational-databases/database-mail/database-mail-configuration-objects.md)   
 [Database Mail des procédures stockées &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/database-mail-stored-procedures-transact-sql.md)  
  
  
