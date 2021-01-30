---
description: sp_dbfixedrolepermission (Transact-SQL)
title: sp_dbfixedrolepermission (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_dbfixedrolepermission
- sp_dbfixedrolepermission_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_dbfixedrolepermission
ms.assetid: b8c30191-f532-49cd-83f3-c271f63ce572
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 78f1381d05c5841ce48e395f8cc1cd477b820e01
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201275"
---
# <a name="sp_dbfixedrolepermission-transact-sql"></a>sp_dbfixedrolepermission (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Affiche les autorisations d'un rôle de base de données fixe. **sp_dbfixedrolepermission** retourne des informations correctes dans [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] . La sortie ne reflète pas les modifications apportées à la hiérarchie des autorisations qui ont été implémentées dans [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] . Pour plus d’informations, consultez [rôles au niveau de la base de données](../../relational-databases/security/authentication-access/database-level-roles.md#fixed-database-roles), qui affiche une liste de rôles de base de données fixes et les autorisations correspondantes.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_dbfixedrolepermission [ [ @rolename = ] 'role' ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @rolename = ] 'role'` Nom d’un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] rôle de base de données fixe valide. *role* est de **type sysname**, avec NULL comme valeur par défaut. Si le *rôle* n’est pas spécifié, les autorisations de tous les rôles de base de données fixes sont affichées.  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**DbFixedRole**|**sysname**|Nom du rôle de base de données fixe|  
|**Permission**|**nvarchar (70)**|Autorisations associées à **DbFixedRole**|  
  
## <a name="remarks"></a>Notes  
 Pour afficher la liste des rôles de base de données fixes, exécutez **sp_helpdbfixedrole**. Le tableau suivant présente les rôles de base de données fixes.  
  
|Rôle de base de données fixe|Description|  
|-------------------------|-----------------|  
|**db_owner**|Propriétaires de base de données|  
|**db_accessadmin**|Administrateurs de l'accès aux bases de données|  
|**db_securityadmin**|Administrateurs de la sécurité des bases de données|  
|**db_ddladmin**|Administrateurs du langage de définition de données (DDL - Data Definition Language)|  
|**db_backupoperator**|Opérateurs de sauvegarde de base de données|  
|**db_datareader**|Utilisateurs autorisés à lire les données des bases de données|  
|**db_datawriter**|Utilisateurs autorisés à écrire des données dans les bases de données|  
|**db_denydatareader**|Utilisateurs non autorisés à lire les données des bases de données|  
|**db_denydatawriter**|Utilisateurs non autorisés à écrire des données dans les bases de données|  
  
 Les membres du rôle de base de données fixe **db_owner** disposent des autorisations de tous les autres rôles de base de données fixes. Pour afficher les autorisations pour les rôles serveur fixes, exécutez **sp_srvrolepermission**.  
  
 L'ensemble des résultats comprend les instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] qu'il est possible d'exécuter ainsi que d'autres activités spéciales que les membres du rôle de base de données peuvent effectuer.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle **public** .  
  
## <a name="examples"></a>Exemples  
 La requête suivante renvoie les autorisations de tous les rôles de base de données fixes du fait qu'elle ne spécifie pas un rôle précis.  
  
```  
EXEC sp_dbfixedrolepermission;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées de sécurité &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [sp_addrolemember &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addrolemember-transact-sql.md)   
 [sp_droprolemember &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-droprolemember-transact-sql.md)   
 [sp_helpdbfixedrole &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpdbfixedrole-transact-sql.md)   
 [sp_srvrolepermission &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-srvrolepermission-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
