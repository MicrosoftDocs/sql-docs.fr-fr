---
description: sysmail_add_profileaccount_sp (Transact-SQL)
title: sysmail_add_profileaccount_sp (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sysmail_add_profileaccount_sp
- sysmail_add_profileaccount_sp_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysmail_add_profileaccount_sp
ms.assetid: 7cbf430f-1997-45ea-9707-0086184de744
author: markingmyname
ms.author: maghan
ms.openlocfilehash: c01a72073b36aeba37cf28b5be8175fc283a07df
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99188765"
---
# <a name="sysmail_add_profileaccount_sp-transact-sql"></a>sysmail_add_profileaccount_sp (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Ajoute un compte de messagerie de base de données à un profil de messagerie de base de données. Exécutez **sysmail_add_profileaccount_sp** une fois qu’un compte de base de données a été créé avec [sysmail_add_account_sp &#40;transact-SQL&#41;](../../relational-databases/system-stored-procedures/sysmail-add-account-sp-transact-sql.md), et qu’un profil de base de données est créé avec [sysmail_add_profile_sp &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sysmail-add-profile-sp-transact-sql.md).  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sysmail_add_profileaccount_sp { [ @profile_id = ] profile_id | [ @profile_name = ] 'profile_name' } ,  
    { [ @account_id = ] account_id | [ @account_name = ] 'account_name' }  
    [ , [ @sequence_number = ] sequence_number ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @profile_id = ] profile_id` ID de profil auquel ajouter le compte. *profile_id* est de **type int**, avec NULL comme valeur par défaut. Le *profile_id* ou le *profile_name* doit être spécifié.  
  
`[ @profile_name = ] 'profile_name'` Nom du profil auquel ajouter le compte. *profile_name* est de **type sysname**, avec NULL comme valeur par défaut. Le *profile_id* ou le *profile_name* doit être spécifié.  
  
`[ @account_id = ] account_id` ID de compte à ajouter au profil. *account_id* est de **type int**, avec NULL comme valeur par défaut. Le *account_id* ou le *account_name* doit être spécifié.  
  
`[ @account_name = ] 'account_name'` Nom du compte à ajouter au profil. *account_name* est de **type sysname**, avec NULL comme valeur par défaut. Le *account_id* ou le *account_name* doit être spécifié.  
  
`[ @sequence_number = ] sequence_number` Numéro de séquence du compte dans le profil. *sequence_number* est de **type int**, sans valeur par défaut. Le numéro de séquence détermine l'ordre dans lequel les comptes sont utilisés dans le profil.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 Le profil et le compte doivent déjà exister. Sinon, la procédure stockée retourne une erreur.  
  
 Notez que cette procédure stockée ne modifie pas le numéro de séquence d'un compte qui est déjà associé au profil spécifié. Pour plus d’informations sur la mise à jour du numéro de séquence d’un compte, consultez [sysmail_update_profileaccount_sp &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sysmail-update-profileaccount-sp-transact-sql.md).  
  
 Le numéro de séquence détermine l'ordre d'utilisation des comptes de messagerie de base de données dans le profil. Pour un nouveau message électronique, la messagerie de base de données démarre avec le compte dont le numéro de séquence est le plus petit. Si ce compte échoue, la messagerie de base de données utilise le compte qui possède le numéro de séquence plus élevé suivant, et ainsi de suite jusqu'à ce qu'elle envoie le message correctement ou que le compte qui possède le numéro de séquence le plus élevé échoue. Si le compte qui a le numéro de séquence le plus élevé échoue, la messagerie de base de données interrompt les tentatives d’envoi du courrier électronique pour la durée configurée dans le paramètre *AccountRetryDelay* de **sysmail_configure_sp**, puis relance le processus de tentative d’envoi du courrier électronique en commençant à partir du numéro de séquence le moins élevé. Utilisez le paramètre *AccountRetryAttempts* de **sysmail_configure_sp** pour configurer le nombre de fois que le processus de messagerie externe tente d’envoyer le message électronique à l’aide de chaque compte du profil spécifié.  
  
 Si plusieurs comptes ont le même numéro de séquence, la messagerie de base de données utilisera uniquement l'un de ces comptes pour un message électronique donné. Dans ce cas, la messagerie de base de données exclut toute garantie en ce qui concerne le compte utilisé pour ce numéro de séquence ou l'utilisation du même compte d'un message à un autre.  
  
 La procédure stockée **sysmail_add_profileaccount_sp** se trouve dans la base de données **msdb** et appartient au schéma **dbo** . La procédure doit être exécutée avec un nom en trois parties si la base de données actuelle n’est pas **msdb**.  
  
## <a name="permissions"></a>Autorisations  
 Les autorisations d’exécution pour cette procédure sont octroyées par défaut aux membres du rôle serveur fixe **sysadmin** .  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant associe le profil `AdventureWorks Administrator` au compte `Audit Account`. Le numéro de séquence de ce compte est 1.  
  
```  
EXECUTE msdb.dbo.sysmail_add_profileaccount_sp  
    @profile_name = 'AdventureWorks Administrator',  
    @account_name = 'Audit Account',  
    @sequence_number = 1 ;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Messagerie de base de données](../../relational-databases/database-mail/database-mail.md)   
 [Créer un compte Database Mail](../../relational-databases/database-mail/create-a-database-mail-account.md)   
 [Objets de configuration Database Mail](../../relational-databases/database-mail/database-mail-configuration-objects.md)   
 [Database Mail des procédures stockées &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/database-mail-stored-procedures-transact-sql.md)  
  
  
