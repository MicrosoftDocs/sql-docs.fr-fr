---
description: sysmail_update_profileaccount_sp (Transact-SQL)
title: sysmail_update_profileaccount_sp (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sysmail_update_profileaccount_sp_TSQL
- sysmail_update_profileaccount_sp
dev_langs:
- TSQL
helpviewer_keywords:
- sysmail_update_profileaccount_sp
ms.assetid: 92ca7488-29db-414e-8e36-08b0a8f542bb
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 140256e73adee3593c620b17f81ac10c163030f3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181851"
---
# <a name="sysmail_update_profileaccount_sp-transact-sql"></a>sysmail_update_profileaccount_sp (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Met à jour le numéro de séquence d'un compte dans un profil de messagerie de base de données.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sysmail_update_profileaccount_sp  { [ @profile_id = ] profile_id   
| [ @profile_name = ] 'profile_name' } ,  
    { [ @account_id = ] account_id | [ @account_name = ] 'account_name' } ,  
    [ @sequence_number = ] sequence_number  
```  
  
## <a name="arguments"></a>Arguments  
`[ @profile_id = ] profile_id` ID de profil du profil à mettre à jour. *profile_id* est de **type int**, avec NULL comme valeur par défaut. Le *profile_id* ou le *profile_name* doit être spécifié.  
  
`[ @profile_name = ] 'profile_name'` Nom de profil du profil à mettre à jour. *profile_name* est de **type sysname**, avec NULL comme valeur par défaut. Le *profile_id* ou le *profile_name* doit être spécifié.  
  
`[ @account_id = ] account_id` ID de compte à mettre à jour. *account_id* est de **type int**, avec NULL comme valeur par défaut. Le *account_id* ou le *account_name* doit être spécifié.  
  
`[ @account_name = ] 'account_name'` Nom du compte à mettre à jour. *account_name* est de **type sysname**, avec NULL comme valeur par défaut. Le *account_id* ou le *account_name* doit être spécifié.  
  
`[ @sequence_number = ] sequence_number` Nouveau numéro de séquence du compte. *sequence_number* est de **type int**, sans valeur par défaut. Le numéro de séquence détermine l'ordre dans lequel les comptes sont utilisés dans le profil.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 None  
  
## <a name="remarks"></a>Notes  
 Renvoie une erreur si le compte spécifié n'est pas associé au profil spécifié.  
  
 Le numéro de séquence détermine l'ordre d'utilisation des comptes de messagerie de base de données dans le profil. Pour un nouveau message électronique, la messagerie de base de données démarre avec le compte dont le numéro de séquence est le plus petit. Si ce compte échoue, la messagerie de base de données utilise le compte qui possède le numéro de séquence plus élevé suivant, et ainsi de suite jusqu'à ce qu'elle envoie le message correctement ou que le compte qui possède le numéro de séquence le plus élevé échoue. En cas d'échec du compte avec le numéro de séquence le plus élevé, le message échoue.  
  
 Si plusieurs comptes possèdent le même numéro de séquence, la messagerie de base de données utilise uniquement l'un d'eux pour un message électronique donné. Dans ce cas, la messagerie de base de données exclut toute garantie en ce qui concerne le compte utilisé pour ce numéro de séquence ou l'utilisation du même compte d'un message à un autre.  
  
 La procédure stockée **sysmail_update_profileaccount_sp** se trouve dans la base de données **msdb** et appartient au schéma **dbo** . La procédure doit être exécutée avec un nom en trois parties si la base de données actuelle n’est pas **msdb**.  
  
## <a name="permissions"></a>Autorisations  
 Les autorisations d’exécution pour cette procédure sont octroyées par défaut aux membres du rôle serveur fixe **sysadmin** .  
  
## <a name="examples"></a>Exemples  
 L’exemple suivant modifie le numéro de séquence du compte dans le `Admin-BackupServer` profil `AdventureWorks Administrator` dans la base de données **msdb** . Une fois ce code exécuté, le numéro de séquence du compte est `3`. Cela indique qu'il sera utilisé si les deux premiers comptes échouent.  
  
```  
EXECUTE msdb.dbo.sysmail_update_profileaccount_sp  
    @profile_name = 'AdventureWorks Administrator'  
    ,@account_name = 'Admin-BackupServer',  
    ,@sequence_number = 3;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Messagerie de base de données](../../relational-databases/database-mail/database-mail.md)   
 [Créer un compte Database Mail](../../relational-databases/database-mail/create-a-database-mail-account.md)   
 [Objets de configuration Database Mail](../../relational-databases/database-mail/database-mail-configuration-objects.md)   
 [Database Mail des procédures stockées &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/database-mail-stored-procedures-transact-sql.md)  
  
  
