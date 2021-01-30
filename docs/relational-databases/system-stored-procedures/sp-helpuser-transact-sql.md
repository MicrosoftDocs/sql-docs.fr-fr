---
description: sp_helpuser (Transact-SQL)
title: sp_helpuser (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_helpuser
- sp_helpuser_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_helpuser
ms.assetid: 9c70b41d-ef4c-43df-92da-bd534c287ca1
author: markingmyname
ms.author: maghan
ms.openlocfilehash: ce2bf386978412c6b1409b31e371f8caa15eccdb
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99103048"
---
# <a name="sp_helpuser-transact-sql"></a>sp_helpuser (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Signale des informations sur les principaux de niveau base de données dans la base de données en cours.  
  
> [!IMPORTANT]  
>  **sp_helpuser** ne retourne pas d’informations sur les éléments sécurisables introduits dans [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] . Utilisez à la place [sys.database_principals](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md) .  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_helpuser [ [ @name_in_db = ] 'security_account' ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @name_in_db = ] 'security_account'` Nom de l’utilisateur de base de données ou du rôle de base de données dans la base de données actuelle. *security_account* doit exister dans la base de données active. *security_account* est de **type sysname**, avec NULL comme valeur par défaut. Si *security_account* n’est pas spécifié, **sp_helpuser** retourne des informations sur tous les principaux de la base de données.  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 Le tableau suivant montre le jeu de résultats lorsque ni un compte d’utilisateur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , ni un utilisateur ou Windows n’est spécifié pour *security_account*.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**UserName**|**sysname**|Utilisateurs dans la base de données en cours.|  
|**RoleName**|**sysname**|Rôles auxquels appartient le **nom d’utilisateur** .|  
|**LoginName**|**sysname**|Connexion du **nom d’utilisateur**.|  
|**DefDBName**|**sysname**|Base de données par défaut du **nom d’utilisateur**.|  
|**DefSchemaName**|**sysname**|Schéma par défaut de l'utilisateur de la base de données.|  
|**l'UserId**|**smallint**|ID du **nom d’utilisateur** dans la base de données actuelle.|  
|**SID**|**smallint**|Numéro d'identification de sécurité (SID) de l'utilisateur.|  
  
 Le tableau ci-dessous indique l'ensemble de résultats obtenu, lorsqu'aucun compte d'utilisateur n'est spécifié et que des alias existent dans la base de données en cours.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**LoginName**|**sysname**|Connexions affectées comme alias aux utilisateurs de la base de données en cours.|  
|**UserNameAliasedTo**|**sysname**|Nom d'utilisateur dans la base de données en cours dont la connexion est affectée comme alias.|  
  
 Le tableau suivant montre le jeu de résultats lorsqu’un rôle est spécifié pour *security_account*.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**Role_name**|**sysname**|Nom du rôle dans la base de données en cours.|  
|**Role_id**|**smallint**|ID du rôle dans la base de données en cours.|  
|**Users_in_role**|**sysname**|Membre du rôle dans la base de données en cours.|  
|**IDutilisateur**|**smallint**|ID d'utilisateur du membre du rôle.|  
  
## <a name="remarks"></a>Notes  
 Pour afficher des informations sur l’appartenance aux rôles de base de données, utilisez [sys.database_role_members](../../relational-databases/system-catalog-views/sys-database-role-members-transact-sql.md). Pour afficher des informations sur les membres du rôle de serveur, utilisez [sys.server_role_members](../../relational-databases/system-catalog-views/sys-server-role-members-transact-sql.md)et pour afficher des informations sur les principaux au niveau du serveur, utilisez [sys.server_principals](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md).  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle **public** .  
  
 Les informations retournées sont sujettes à des restrictions d'accès aux métadonnées. Les entités sur lesquelles le principal ne possède pas d'autorisation n'apparaissent pas. Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-listing-all-users"></a>R. Création de la liste de tous les utilisateurs  
 L'exemple ci-dessous répertorie tous les utilisateurs dans la base de données en cours.  
  
```  
EXEC sp_helpuser;  
```  
  
### <a name="b-listing-information-for-a-single-user"></a>B. Création de la liste des informations sur un utilisateur unique  
 Dans l'exemple ci-dessous, des informations sont répertoriées sur le propriétaire de la base de données utilisateur (`dbo`).  
  
```  
EXEC sp_helpuser 'dbo';  
```  
  
### <a name="c-listing-information-for-a-database-role"></a>C. Création de la liste des informations sur un rôle de base de données  
 Dans l'exemple ci-dessous, des informations sont répertoriées sur le rôle de base de données fixe `db_securityadmin`.  
  
```  
EXEC sp_helpuser 'db_securityadmin';  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées de sécurité &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Principaux &#40;moteur de base de données&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [sys.database_principals &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md)   
 [sys.database_role_members &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-role-members-transact-sql.md)   
 [sys.server_principals &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md)   
 [sys.server_role_members &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-role-members-transact-sql.md)  
  
  
