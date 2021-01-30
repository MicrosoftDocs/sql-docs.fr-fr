---
description: sp_addsrvrolemember (Transact-SQL)
title: sp_addsrvrolemember (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/20/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_addsrvrolemember
- sp_addsrvrolemember_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_addsrvrolemember
ms.assetid: 777f0e09-8ee5-4cb2-a3ac-939d02c3cd22
author: markingmyname
ms.author: maghan
ms.openlocfilehash: a8de69bfd8e0fee825bd0e3fb914acddee0cb419
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207091"
---
# <a name="sp_addsrvrolemember-transact-sql"></a>sp_addsrvrolemember (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Ajoute une connexion à un membre d'un rôle serveur fixe.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Utilisez à la place [ALTER Server Role](../../t-sql/statements/alter-server-role-transact-sql.md) .  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_addsrvrolemember [ @loginame= ] 'login'   
    , [ @rolename = ] 'role'  
```  
  
## <a name="arguments"></a>Arguments  
 [ @loginame **=** ] **«**_connexion_**»**  
 Nom de la connexion ajoutée au rôle serveur fixe. *login* est de **type sysname**, sans valeur par défaut. la *connexion* peut être une connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou une connexion Windows. Si la connexion Windows n'a pas encore été autorisée à accéder à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], elle l'est automatiquement.  
  
 [ @rolename **=** ] **«**_role_**»**  
 Nom du rôle serveur fixe auquel est ajoutée la connexion. *role* est de **type sysname**, avec NULL comme valeur par défaut et doit avoir l’une des valeurs suivantes :  
  
-   administrateur système  
  
-   securityadmin  
  
-   serveradmin  
  
-   setupadmin  
  
-   processadmin  
  
-   diskadmin  
  
-   dbcreator  
  
-   bulkadmin  

## <a name="return-code-values"></a>Codet de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="remarks"></a>Notes  
 Lorsque vous ajoutez une connexion à un rôle serveur fixe, la connexion obtient les autorisations associées à ce rôle.  
  
 L'appartenance au rôle de la connexion sa et public ne peut pas être modifiée.  
  
 Utilisez sp_addrolemember pour ajouter un membre à un rôle de base de données fixe ou à un rôle défini par l'utilisateur.  
  
 La procédure sp_addsrvrolemember ne peut pas être exécutée dans une transaction définie par l'utilisateur.  
  
## <a name="permissions"></a>Autorisations  
 Il faut appartenir au rôle auquel le nouveau membre est ajouté.  
  
## <a name="examples"></a>Exemples  
 L’exemple suivant ajoute la connexion Windows `Corporate\HelenS` au `sysadmin` rôle serveur fixe.  
  
```  
EXEC sp_addsrvrolemember 'Corporate\HelenS', 'sysadmin';  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées de sécurité &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [sp_addrolemember &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addrolemember-transact-sql.md)   
 [sp_dropsrvrolemember &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropsrvrolemember-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Fonctions de sécurité &#40;Transact-SQL&#41;](../../t-sql/functions/security-functions-transact-sql.md)   
 [CREATE SERVER ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-server-role-transact-sql.md)   
 [DROP SERVER ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-server-role-transact-sql.md)  
  
  
