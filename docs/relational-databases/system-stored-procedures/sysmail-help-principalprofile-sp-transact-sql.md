---
description: sysmail_help_principalprofile_sp (Transact-SQL)
title: sysmail_help_principalprofile_sp (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/02/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sysmail_help_principalprofile_sp_TSQL
- sysmail_help_principalprofile_sp
dev_langs:
- TSQL
helpviewer_keywords:
- sysmail_help_principalprofile_sp
ms.assetid: 0cfd6464-09c7-4f03-9d25-58001c096a9e
author: markingmyname
ms.author: maghan
ms.openlocfilehash: efb4b5cd655bf4a401d530e4c9bd52cbc9660a65
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181955"
---
# <a name="sysmail_help_principalprofile_sp-transact-sql"></a>sysmail_help_principalprofile_sp (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Répertorie des informations sur les associations entre les profils de messagerie de la base de données et les principaux de la base de données.  
  
 
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sysmail_help_principalprofile_sp [ {   [ @principal_id = ] principal_id | [ @principal_name = ] 'principal_name' } ]  
    [ [ , ] {   [ @profile_id = ] profile_id | [ @profile_name = ] 'profile_name' } ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @principal_id = ] principal_id` ID de l’utilisateur ou du rôle de base de données dans la base de données **msdb** pour l’Association à répertorier. *principal_id* est de **type int**, avec NULL comme valeur par défaut. *Principal_id* ou *principal_name* peuvent être spécifiés.  
  
`[ @principal_name = ] 'principal_name'` Nom de l’utilisateur ou du rôle de base de données dans la base de données **msdb** pour l’Association à répertorier. *principal_name* est de **type sysname**, avec NULL comme valeur par défaut. *Principal_id* ou *principal_name* peuvent être spécifiés.  
  
`[ @profile_id = ] profile_id` ID du profil pour l’Association à répertorier. *profile_id* est de **type int**, avec NULL comme valeur par défaut. *Profile_id* ou *profile_name* peuvent être spécifiés.  
  
`[ @profile_name = ] 'profile_name'` Nom du profil pour l’Association à répertorier. *profile_name* est de **type sysname**, avec NULL comme valeur par défaut. *Profile_id* ou *profile_name* peuvent être spécifiés.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 Retourne un jeu de résultats qui contient les colonnes répertoriées dans le tableau ci-dessous.  
  
| Nom de la colonne | Type de données | Description |
| ----------- | --------- | ----------- |
|**principal_id**|**int**|Identificateur de l'utilisateur de la base de données.|  
|**principal_name**|**sysname**|Nom de l’utilisateur de base de données.|  
|**profile_id**|**int**|Numéro d'identification du profil de messagerie de la base de données.|  
|**profile_name**|**sysname**|Nom du profil de messagerie de la base de données.|  
|**is_default**|**bit**|Indicateur signalant s'il s'agit du profil par défaut de l'utilisateur.|  
  
## <a name="remarks"></a>Notes  
 Si **sysmail_help_principalprofile_sp** est appelée sans paramètres, le jeu de résultats retourné répertorie toutes les associations dans l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Sinon, l'ensemble de résultats contient des informations pour les associations qui correspondent aux paramètres fournis. Par exemple, la procédure répertorie toutes les associations d'un profil lorsque le nom de ce dernier est fourni.  
  
 **sysmail_help_principalprofile_sp** se trouve dans la base de données **msdb** et appartient au schéma **dbo** . La procédure doit être exécutée avec un nom en trois parties si la base de données actuelle n’est pas **msdb**.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle serveur fixe **sysadmin** .  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-listing-information-for-a-specific-association"></a>R. Affichage d'une liste d'informations pour une association spécifique  
 L'exemple suivant illustre l'affichage d'une liste d'informations sur toutes les associations entre le profil `AdventureWorks Administrator` et le principal `ApplicationLogin` de la base de données `msdb`.  
  
```  
EXECUTE msdb.dbo.sysmail_help_principalprofile_sp  
    @principal_name = 'danw',  
    @profile_name = 'AdventureWorks Administrator' ;  
```  
  
 Exemple d'un ensemble de résultats remis en forme au niveau de la longueur de ligne.  
  
```  
principal_id principal_name     profile_id  profile_name                   is_default  
------------ ------------------ ----------- ------------------------------ ----------  
5            danw               9           AdventureWorks Administrator   1  
```  
  
### <a name="b-listing-information-for-all-associations"></a>B. Affichage d'une liste d'informations pour toutes les associations  
 L'exemple suivant illustre l'affichage d'une liste d'informations sur toutes les associations de l'instance.  
  
```  
EXECUTE msdb.dbo.sysmail_help_principalprofile_sp ;  
```  
  
 Exemple d'un ensemble de résultats remis en forme au niveau de la longueur de ligne.  
  
```  
principal_id principal_name     profile_id  profile_name                   is_default  
------------ ------------------ ----------- ------------------------------ ----------  
6            terrid             3           Product Update Profile         1  
5            danw               9           AdventureWorks Administrator   1  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Messagerie de base de données](../../relational-databases/database-mail/database-mail.md)   
 [Database Mail des procédures stockées &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/database-mail-stored-procedures-transact-sql.md)  
  
  
