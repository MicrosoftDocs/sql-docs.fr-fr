---
description: sysmail_delete_profileaccount_sp (Transact-SQL)
title: sysmail_delete_profileaccount_sp (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sysmail_delete_profileaccount_sp
- sysmail_delete_profileaccount_sp_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysmail_delete_profileaccount_sp
ms.assetid: b58d06f2-d6c9-4c8e-95bd-027c50f4621a
author: markingmyname
ms.author: maghan
ms.openlocfilehash: d364dfb4de4408da2f424185e6cad4f62b0c54cf
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181986"
---
# <a name="sysmail_delete_profileaccount_sp-transact-sql"></a>sysmail_delete_profileaccount_sp (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Supprime un compte d'un profil de messagerie de base de données.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sysmail_delete_profileaccount_sp  {   [ @profile_id = ] profile_id | [ @profile_name = ] 'profile_name' } ,  
    {   [ @account_id = ] account_id | [ @account_name = ] 'account_name' }  
```  
  
## <a name="arguments"></a>Arguments  
`[ @profile_id = ] profile_id` ID de profil du profil à supprimer. *profile_id* est de **type int**, avec NULL comme valeur par défaut. La *profile_id* ou la *profile_name* peut être spécifiée.  
  
`[ @profile_name = ] 'profile_name'` Nom de profil du profil à supprimer. *profile_name* est de **type sysname**, avec NULL comme valeur par défaut. La *profile_id* ou la *profile_name* peut être spécifiée.  
  
`[ @account_id = ] account_id` ID de compte à supprimer. *account_id* est de **type int**, avec NULL comme valeur par défaut. La *account_id* ou la *account_name* peut être spécifiée.  
  
`[ @account_name = ] 'account_name'` Nom du compte à supprimer. *account_name* est de **type sysname**, avec NULL comme valeur par défaut. La *account_id* ou la *account_name* peut être spécifiée.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 None  
  
## <a name="remarks"></a>Notes  
 Renvoie une erreur si le compte spécifié n'est pas associé au profil spécifié.  
  
 Lorsqu'un compte est spécifié mais pas un profil, cette procédure stockée supprime le compte spécifié de tous les profils. Par exemple, pour fermer un serveur SMTP existant, supprimez de tous les profils les comptes qui utilisent le serveur SMTP en question au lieu de supprimer chaque compte de chaque profil.  
  
 Lorsqu'un profil est spécifié mais pas un compte, cette procédure stockée supprime tous les comptes du profil spécifié. Par exemple, pour modifier les serveurs SMTP utilisés par un profil, il peut être pratique de supprimer tous les comptes du profil et d'ajouter ensuite les nouveaux comptes.  
  
 La procédure stockée **sysmail_delete_profileaccount_sp** se trouve dans la base de données **msdb** et appartient au schéma **dbo** . La procédure doit être exécutée avec un nom en trois parties si la base de données actuelle n’est pas **msdb**.  
  
## <a name="permissions"></a>Autorisations  
 Les autorisations d’exécution pour cette procédure sont octroyées par défaut aux membres du rôle serveur fixe **sysadmin** .  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant montre la suppression du compte `Audit Account` du profil `AdventureWorks Administrator`.  
  
```  
EXECUTE msdb.dbo.sysmail_delete_profileaccount_sp  
    @profile_name = 'AdventureWorks Administrator',  
    @account_name = 'Audit Account' ;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Messagerie de base de données](../../relational-databases/database-mail/database-mail.md)   
 [Créer un compte Database Mail](../../relational-databases/database-mail/create-a-database-mail-account.md)   
 [Objets de configuration Database Mail](../../relational-databases/database-mail/database-mail-configuration-objects.md)   
 [Database Mail des procédures stockées &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/database-mail-stored-procedures-transact-sql.md)  
  
  
