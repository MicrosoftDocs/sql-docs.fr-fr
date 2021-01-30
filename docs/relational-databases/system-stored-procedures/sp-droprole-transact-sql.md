---
description: sp_droprole (Transact-SQL)
title: sp_droprole (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_droprole
- sp_droprole_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_droprole
ms.assetid: 889ee074-00f8-40a9-bddb-d7d3ef0cbc19
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 58832ba9f1d5b508f8125dc137805e59d3977f9e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187019"
---
# <a name="sp_droprole-transact-sql"></a>sp_droprole (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Supprime un rôle de base de données de la base de données actuelle.  
  
> [!IMPORTANT]  
>  Dans [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] , **sp_droprole** a été remplacé par l’instruction DROP ROLE. **sp_droprole** est inclus uniquement pour la compatibilité avec les versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et peut ne pas être pris en charge dans une version ultérieure.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_droprole [ @rolename= ] 'role'  
```  
  
## <a name="arguments"></a>Arguments  
`[ @rolename = ] 'role'` Nom du rôle de base de données à supprimer de la base de données actuelle. *role* est de **type sysname** et n’a pas de valeur par défaut. le *rôle* doit déjà exister dans la base de données actuelle.  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="remarks"></a>Notes  
 Seuls les rôles de base de données peuvent être supprimés à l’aide de **sp_droprole**.  
  
 Il est impossible de supprimer un rôle de base de données s'il possède des membres. Il faut supprimer tous les membres d'un rôle de base de données avant de supprimer ce rôle. Pour supprimer des utilisateurs d’un rôle, utilisez **sp_droprolemember**. Si des utilisateurs sont toujours membres du rôle, **sp_droprole** affiche ces membres.  
  
 Les rôles fixes et le rôle **public** ne peuvent pas être supprimés.  
  
 Il n'est pas possible de supprimer un rôle s'il est propriétaire d'éléments sécurisables. Avant de supprimer un rôle d'application propriétaire d'éléments sécurisables, vous devez transférer la propriété de ces éléments sécurisables ou les supprimer. Utilisez ALTER AUTHORIZATION pour changer le propriétaire des objets qui ne doivent pas être supprimés.  
  
 **sp_droprole** ne peut pas être exécutée dans une transaction définie par l’utilisateur.  
  
## <a name="permissions"></a>Autorisations  
 Il faut avoir l'autorisation CONTROL sur le rôle.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant supprime le rôle d'application `Sales`.  
  
```  
EXEC sp_droprole 'Sales';  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées de sécurité &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [sp_addrole &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addrole-transact-sql.md)   
 [DROP ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-role-transact-sql.md)   
 [ALTER AUTHORIZATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-authorization-transact-sql.md)   
 [sp_dropapprole &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropapprole-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
