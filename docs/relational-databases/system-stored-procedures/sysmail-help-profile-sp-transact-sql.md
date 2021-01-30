---
description: sysmail_help_profile_sp (Transact-SQL)
title: sysmail_help_profile_sp (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sysmail_help_profile_sp_TSQL
- sysmail_help_profile_sp
dev_langs:
- TSQL
helpviewer_keywords:
- sysmail_help_profile_sp
ms.assetid: d7169a8e-92b1-49eb-9124-3b2f69755ddb
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 865e871463d8ce15ef08db2d6431634e6e6bc08c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181949"
---
# <a name="sysmail_help_profile_sp-transact-sql"></a>sysmail_help_profile_sp (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Répertorie des informations sur un ou plusieurs profils de messagerie.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sysmail_help_profile_sp  [   [ @profile_id = ] profile_id | [ @profile_name = ] 'profile_name' ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @profile_id = ] profile_id` ID de profil pour lequel des informations doivent être retournées. *profile_id* est de **type int**, avec NULL comme valeur par défaut.  
  
`[ @profile_name = ] 'profile_name'` Nom du profil pour lequel des informations doivent être retournées. *profile_name* est de **type sysname**, avec NULL comme valeur par défaut.  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 Retourne un jeu de résultats comportant les colonnes suivantes.  
  
| Nom de la colonne | Type de données | Description |
| ----------- | --------- | ----------- |
|**profile_id**|**int**|ID de profil du profil.|  
|**name**|**sysname**|Nom de profil du profil.|  
|**description**|**nvarchar (256)**|Description du profil.|  
  
## <a name="remarks"></a>Notes  
 Lorsqu’un nom de profil ou un ID de profil est spécifié, **sysmail_help_profile_sp** retourne des informations sur ce profil. Dans le cas contraire, **sysmail_help_profile_sp** retourne des informations sur chaque profil de l' [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] instance.  
  
 La procédure stockée **sysmail_help_profile_sp** se trouve dans la base de données **msdb** et appartient au schéma **dbo** . La procédure doit être exécutée avec un nom en trois parties si la base de données actuelle n’est pas **msdb**.  
  
## <a name="permissions"></a>Autorisations  
 Les autorisations d’exécution pour cette procédure sont octroyées par défaut aux membres du rôle serveur fixe **sysadmin** .  
  
## <a name="examples"></a>Exemples  
 **A. Affichage de tous les profils**  
  
 Cet exemple montre comment afficher tous les profils de l'instance.  
  
```  
EXECUTE msdb.dbo.sysmail_help_profile_sp;  
```  
  
 Exemple d'un ensemble de résultats remis en forme au niveau de la longueur de ligne :  
  
```  
profile_id  name                          description  
----------- ----------------------------- ------------------------------  
56          AdventureWorks Administrator  Administrative mail profile.    
57          AdventureWorks Operator       Operator mail profile.          
```  
  
 **B. Affichage d'un profil spécifique**  
  
 Cet exemple montre comment afficher des informations pour le profil `AdventureWorks Administrator`.  
  
```  
EXECUTE msdb.dbo.sysmail_help_profile_sp  
    @profile_name = 'AdventureWorks Administrator' ;  
```  
  
 Exemple d'un ensemble de résultats remis en forme au niveau de la longueur de ligne :  
  
```  
profile_id  name                          description  
----------- ----------------------------- ------------------------------  
56          AdventureWorks Administrator  Administrative mail profile.    
```  
  
## <a name="see-also"></a>Voir aussi  
 [Messagerie de base de données](../../relational-databases/database-mail/database-mail.md)   
 [Database Mail des procédures stockées &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/database-mail-stored-procedures-transact-sql.md)  
  
  
