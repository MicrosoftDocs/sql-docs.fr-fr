---
description: sysmail_help_profileaccount_sp (Transact-SQL)
title: sysmail_help_profileaccount_sp (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sysmail_help_profileaccount_sp_TSQL
- sysmail_help_profileaccount_sp
dev_langs:
- TSQL
helpviewer_keywords:
- sysmail_help_profileaccount_sp
ms.assetid: 3ea68271-0a6b-4d77-991c-4757f48f747a
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 45b542047875527a26d9c4854b223e626efda5ea
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181941"
---
# <a name="sysmail_help_profileaccount_sp-transact-sql"></a>sysmail_help_profileaccount_sp (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Affiche les comptes associés à un ou plusieurs profils de messagerie de base de données.  
    
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sysmail_help_profileaccount_sp  
   {   [ @profile_id = ] profile_id   
      | [ @profile_name = ] 'profile_name' }  
   [ , {   [ @account_id = ] account_id  
         | [ @account_name = ] 'account_name' } ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @profile_id = ] profile_id` ID du profil à répertorier. *profile_id* est de **type int**, avec NULL comme valeur par défaut. *Profile_id* ou *profile_name* doivent être spécifiés.  
  
`[ @profile_name = ] 'profile_name'` Nom du profil à répertorier. *profile_name* est de **type sysname**, avec NULL comme valeur par défaut. *Profile_id* ou *profile_name* doivent être spécifiés.  
  
`[ @account_id = ] account_id` ID de compte à répertorier. *account_id* est de **type int**, avec NULL comme valeur par défaut. Lorsque *account_id* et *account_name* ont tous deux la valeur null, répertorie tous les comptes du profil.  
  
`[ @account_name = ] 'account_name'` Nom du compte à répertorier. *account_name* est de **type sysname**, avec NULL comme valeur par défaut. Lorsque *account_id* et *account_name* ont tous deux la valeur null, répertorie tous les comptes du profil.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 Retourne un jeu de résultats comportant les colonnes suivantes.  
  
| Nom de la colonne | Type de données | Description |
| ----------- | --------- | ----------- |
|**profile_id**|**int**|ID de profil du profil.|  
|**profile_name**|**sysname**|Nom du profil.|  
|**account_id**|**int**|ID du compte.|  
|**account_name**|**sysname**|Nom du compte.|  
|**sequence_number**|**int**|Numéro de séquence du compte dans le profil.|  
  
## <a name="remarks"></a>Notes  
 Quand aucun *profile_id* ou *profile_name* n’est spécifié, cette procédure stockée retourne des informations pour chaque profil de l’instance.  
  
 La procédure stockée **sysmail_help_profileaccount_sp** se trouve dans la base de données **msdb** et appartient au schéma **dbo** . La procédure doit être exécutée avec un nom en trois parties si la base de données actuelle n’est pas **msdb**.  
  
## <a name="permissions"></a>Autorisations  
 Les autorisations d’exécution pour cette procédure sont octroyées par défaut aux membres du rôle serveur fixe **sysadmin** .  
  
## <a name="examples"></a>Exemples  
 **A. Affichage de la liste des comptes d'un profil spécifique, par nom**  
  
 L'exemple suivant affiche la liste des informations pour le profil `AdventureWorks Administrator` en spécifiant le nom du profil.  
  
```  
EXECUTE msdb.dbo.sysmail_help_profileaccount_sp  
   @profile_name = 'AdventureWorks Administrator';  
```  
  
 Voici un exemple d'ensemble de résultats, modifié pour la longueur de ligne :  
  
```  
profile_id  profile_name                 account_id  account_name         sequence_number  
----------- ---------------------------- ----------- -------------------- ---------------  
131         AdventureWorks Administrator 197         Admin-MainServer     1  
131         AdventureWorks Administrator 198         Admin-BackupServer   2  
```  
  
 **B. Affichage de la liste des comptes d'un profil spécifique, par ID de profil**  
  
 L'exemple suivant affiche une liste des informations pour le profil `AdventureWorks Administrator` en spécifiant l'ID du profil.  
  
```  
EXECUTE msdb.dbo.sysmail_help_profileaccount_sp  
    @profile_id = 131 ;  
```  
  
 Voici un exemple d'ensemble de résultats, modifié pour la longueur de ligne :  
  
```  
profile_id  profile_name                 account_id  account_name         sequence_number  
----------- ---------------------------- ----------- -------------------- ---------------  
131         AdventureWorks Administrator 197         Admin-MainServer     1  
131         AdventureWorks Administrator 198         Admin-BackupServer   2  
```  
  
 **C. Affichage de la liste des comptes de tous les profils**  
  
 L'exemple suivant affiche une liste des comptes de tous les profils de l'instance.  
  
```  
EXECUTE msdb.dbo.sysmail_help_profileaccount_sp;  
```  
  
 Voici un exemple d'ensemble de résultats, modifié pour la longueur de ligne :  
  
```  
profile_id  profile_name                 account_id  account_name         sequence_number  
----------- ---------------------------- ----------- -------------------- ---------------  
131         AdventureWorks Administrator 197         Admin-MainServer     1  
131         AdventureWorks Administrator 198         Admin-BackupServer   2  
106         AdventureWorks Operator      210         Operator-MainServer  1  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Messagerie de base de données](../../relational-databases/database-mail/database-mail.md)   
 [Créer un compte Database Mail](../../relational-databases/database-mail/create-a-database-mail-account.md)   
 [Objets de configuration Database Mail](../../relational-databases/database-mail/database-mail-configuration-objects.md)   
 [Database Mail des procédures stockées &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/database-mail-stored-procedures-transact-sql.md)  
  
  
