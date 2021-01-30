---
description: sysmail_delete_principalprofile_sp (Transact-SQL)
title: sysmail_delete_principalprofile_sp (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sysmail_delete_principalprofile_sp_TSQL
- sysmail_delete_principalprofile_sp
dev_langs:
- TSQL
helpviewer_keywords:
- sysmail_delete_principalprofile_sp
ms.assetid: 8fc14700-e17a-4073-9a96-7fc23e775c69
author: markingmyname
ms.author: maghan
ms.openlocfilehash: a80485aca8c7f0adb6a539f57540952ccd758a31
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182022"
---
# <a name="sysmail_delete_principalprofile_sp-transact-sql"></a>sysmail_delete_principalprofile_sp (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Supprime l'autorisation d'utilisation d'un profil de messagerie de base de données public ou privé pour un utilisateur de base de données ou un rôle de base de données.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sysmail_delete_principalprofile_sp  { [ @principal_id = ] principal_id | [ @principal_name = ] 'principal_name' } ,  
    { [ @profile_id = ] profile_id | [ @profile_name = ] 'profile_name' }  
```  
  
## <a name="arguments"></a>Arguments  
`[ @principal_id = ] principal_id` ID de l’utilisateur ou du rôle de base de données dans la base de données **msdb** pour l’Association à supprimer. *principal_id* est de **type int**, avec NULL comme valeur par défaut. Pour créer un profil public dans un profil privé, indiquez l’ID principal **0** ou le nom principal **« public »**. *Principal_id* ou *principal_name* doivent être spécifiés.  
  
`[ @principal_name = ] 'principal_name'` Nom de l’utilisateur ou du rôle de base de données dans la base de données **msdb** pour l’Association à supprimer. *principal_name* est de **type sysname**, avec NULL comme valeur par défaut. Pour créer un profil public dans un profil privé, indiquez l’ID principal **0** ou le nom principal **« public »**. *Principal_id* ou *principal_name* doivent être spécifiés.  
  
`[ @profile_id = ] profile_id` ID du profil pour l’Association à supprimer. *profile_id* est de **type int**, avec NULL comme valeur par défaut. *Profile_id* ou *profile_name* doivent être spécifiés.  
  
`[ @profile_name = ] 'profile_name'` Nom du profil pour l’Association à supprimer. *profile_name* est de **type sysname**, avec NULL comme valeur par défaut. *Profile_id* ou *profile_name* doivent être spécifiés.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 Pour créer un profil public dans un profil privé, indiquez **« public »** comme nom de principal ou **0** pour l’ID de principal.  
  
 Soyez prudent lorsque vous supprimez des autorisations pour le profil privé par défaut d'un utilisateur ou pour le profil public par défaut. Quand aucun profil par défaut n’est disponible, **sp_send_dbmail** requiert le nom d’un profil comme argument. Par conséquent, la suppression d’un profil par défaut peut entraîner l’échec des appels à **sp_send_dbmail** . Pour plus d’informations, consultez [sp_send_dbmail &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-send-dbmail-transact-sql.md).  
  
 La procédure stockée **sysmail_delete_principalprofile_sp** se trouve dans la base de données **msdb** et appartient au schéma **dbo** . La procédure doit être exécutée avec un nom en trois parties si la base de données actuelle n’est pas **msdb**.  
  
## <a name="permissions"></a>Autorisations  
 Les autorisations d’exécution pour cette procédure sont octroyées par défaut aux membres du rôle serveur fixe **sysadmin** .  
  
## <a name="examples"></a>Exemples  
 L’exemple suivant illustre la suppression de l’association entre le profil **administrateur AdventureWorks** et le **ApplicationUser** de connexion dans la base de données **msdb** .  
  
```  
EXECUTE msdb.dbo.sysmail_delete_principalprofile_sp  
    @principal_name = 'ApplicationUser',  
    @profile_name = 'AdventureWorks Administrator' ;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Messagerie de base de données](../../relational-databases/database-mail/database-mail.md)   
 [Objets de configuration Database Mail](../../relational-databases/database-mail/database-mail-configuration-objects.md)   
 [Database Mail des procédures stockées &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/database-mail-stored-procedures-transact-sql.md)  
  
  
