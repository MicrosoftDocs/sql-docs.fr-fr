---
description: sp_helprolemember (Transact-SQL)
title: sp_helprolemember (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_helprolemember_TSQL
- sp_helprolemember
dev_langs:
- TSQL
helpviewer_keywords:
- sp_helprolemember
ms.assetid: 42797510-aa5d-4564-85ac-27418419af9c
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 223d0fe0a72ed72f312e1aba5402ee9c14e9db63
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195084"
---
# <a name="sp_helprolemember-transact-sql"></a>sp_helprolemember (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Retourne des informations sur les membres directs d'un rôle dans la base de données active.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_helprolemember [ [ @rolename = ] 'role' ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @rolename = ] ' role '` Nom d’un rôle dans la base de données active. *role* est de **type sysname**, avec NULL comme valeur par défaut. le *rôle* doit exister dans la base de données actuelle. Si le *rôle* n’est pas spécifié, tous les rôles qui contiennent au moins un membre de la base de données actuelle sont retournés.  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**DbRole**|**sysname**|Nom du rôle dans la base de données en cours.|  
|**MemberName**|**sysname**|Nom d’un membre de **DbRole.**|  
|**MemberSID**|**varbinary(85)**|Identificateur de sécurité de **MemberName.**|  
  
## <a name="remarks"></a>Notes  
 Si la base de données contient des rôles imbriqués, **MemberName** peut être le nom d’un rôle. **sp_helprolemember** n’affiche pas l’appartenance obtenue via des rôles imbriqués. Par exemple si User1 est membre de Role1, et Role1 est membre de Role2, `EXEC sp_helprolemember 'Role2'` ; retourne Role1, mais pas les membres de Role1 (User1 dans cet exemple). Pour retourner des appartenances imbriquées, vous devez exécuter **sp_helprolemember** à plusieurs reprises pour chaque rôle imbriqué.  
  
 Utilisez **sp_helpsrvrolemember** pour afficher les membres d’un rôle serveur fixe.  
  
 Utilisez [IS_ROLEMEMBER &#40;&#41;Transact-SQL ](../../t-sql/functions/is-rolemember-transact-sql.md) pour vérifier l’appartenance à un rôle pour un utilisateur spécifié.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle **public** .  
  
## <a name="examples"></a>Exemples  
 Cet exemple affiche les membres du rôle `Sales`.  
  
```  
EXEC sp_helprolemember 'Sales';  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées de sécurité &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [sp_addrolemember &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addrolemember-transact-sql.md)   
 [sp_droprolemember &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-droprolemember-transact-sql.md)   
 [sp_helprole &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helprole-transact-sql.md)   
 [sp_helpsrvrolemember &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpsrvrolemember-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
