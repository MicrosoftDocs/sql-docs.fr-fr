---
description: sp_helpdbfixedrole (Transact-SQL)
title: sp_helpdbfixedrole (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sp_helpdbfixedrole
- sp_helpdbfixedrole_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_helpdbfixedrole
ms.assetid: ad87e9a0-b901-4e37-9950-aa517d680fc3
author: markingmyname
ms.author: maghan
ms.openlocfilehash: e0b72a64d8d895956273a7c76e21dbb88152b546
ms.sourcegitcommit: dd36d1cbe32cd5a65c6638e8f252b0bd8145e165
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/08/2020
ms.locfileid: "89538785"
---
# <a name="sp_helpdbfixedrole-transact-sql"></a>sp_helpdbfixedrole (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Retourne une liste des rôles de base de données fixes.  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_helpdbfixedrole [ [ @rolename = ] 'role' ]   
```  
  
## <a name="arguments"></a>Arguments  
`[ @rolename = ] 'role'` Nom d’un rôle de base de données fixe. *role* est de **type sysname**, avec NULL comme valeur par défaut. Si *role* est spécifié, seules les informations sur ce rôle sont retournées ; dans le cas contraire, une liste et une description de tous les rôles de base de données fixes sont retournées.  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**DbFixedRole**|**sysname**|nom du rôle de base de données fixe.|  
|**Description**|**nvarchar (70)**|Description de **DbFixedRole.**|  
  
## <a name="remarks"></a>Notes  
 Les rôles de base de données fixes, tels que répertoriés dans le tableau ci-dessous, sont définis au niveau de la base de données et possèdent les autorisations leur permettant d'effectuer des opérations d'administration spécifiques au niveau de la base de données. Il est impossible d'ajouter ou de supprimer des rôles de base de données fixes. Les autorisations accordées à un rôle de base de données fixe ne peuvent pas être modifiées.  
  
|Rôle de base de données fixe|Description|  
|-------------------------|-----------------|  
|**db_owner**|Propriétaires de base de données|  
|**db_accessadmin**|Administrateurs de l'accès aux bases de données|  
|**db_securityadmin**|Administrateurs de la sécurité des bases de données|  
|**db_ddladmin**|Administrateurs des DDL de base de données|  
|**db_backupoperator**|Opérateurs de sauvegarde de base de données|  
|**db_datareader**|Utilisateurs autorisés à lire les données des bases de données|  
|**db_datawriter**|Utilisateurs autorisés à écrire des données dans les bases de données|  
|**db_denydatareader**|Utilisateurs non autorisés à lire les données des bases de données|  
|**db_denydatawriter**|Utilisateurs non autorisés à écrire des données dans les bases de données|  
  
 Le tableau ci-dessous indique les procédures stockées utilisées pour modifier les rôles de base de données.  
  
|Procédure stockée|Action|  
|----------------------|------------|  
|**sp_addrolemember**|Ajoute un utilisateur de base de données à un rôle de base de données fixe|  
|**sp_helprole**|Affiche la liste des membres d'un rôle fixe de base de données.|  
|**sp_droprolemember**|Supprime un membre d'un rôle fixe de base de données|  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle **public** .  
  
 Les informations retournées sont sujettes à des restrictions d'accès aux métadonnées. Les entités sur lesquelles le principal ne possède pas d'autorisation n'apparaissent pas. Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="examples"></a>Exemples  
 Dans l'exemple ci-dessous, la liste de tous les rôles de base de données fixe est affichée.  
  
```  
EXEC sp_helpdbfixedrole;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées de sécurité &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [sp_addrolemember &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addrolemember-transact-sql.md)   
 [sp_dbfixedrolepermission &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbfixedrolepermission-transact-sql.md)   
 [sp_droprolemember &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-droprolemember-transact-sql.md)   
 [sp_helprole &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helprole-transact-sql.md)   
 [sp_helprolemember &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helprolemember-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
