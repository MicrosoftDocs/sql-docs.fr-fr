---
description: sp_vupgrade_mergeobjects (Transact-SQL)
title: sp_vupgrade_mergeobjects (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_vupgrade_mergeobjects
- sp_vupgrade_mergeobjects_TSQL
helpviewer_keywords:
- sp_vupgrade_mergeobjects
ms.assetid: 73257c2e-cc4c-48e7-9d66-7ef045bdd4f5
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 26e7405647b80d26c760bf88caf5e6aa786346ff
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201841"
---
# <a name="sp_vupgrade_mergeobjects-transact-sql"></a>sp_vupgrade_mergeobjects (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Régénère les affichages, les procédures stockées et les déclencheurs spécifiques aux articles qui sont utilisés pour suivre et appliquer les modifications de données pour la réplication de fusion. Exécutez cette procédure dans les cas de figure suivants :  
  
-   Si un objet requis par la réplication est accidentellement supprimé.  
  
-   Si vous appliquez une mise à jour, telle qu'un correctif, qui requiert une modification d'un ou de plusieurs des objets de réplication. Exécutez la procédure sur chaque nœud avant d'appliquer la mise à jour.  
  
 L'exécution de cette procédure stockée ne nécessite pas la réinitialisation des abonnements. Cette procédure n'est pas indispensable si vous installez un Service Pack ou une mise à niveau vers une nouvelle version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_vupgrade_mergeobjects [ [@login = ] 'login' ]  
    [ , [ @password = ] 'password' ]  
    [ , [ @security_mode = ] security_mode ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @login = ] 'login'` Connexion de l’administrateur système à utiliser lors de la création d’objets système dans la base de données de distribution. *login* est de type **sysname**, avec NULL comme valeur par défaut. Ce paramètre n’est pas requis si *security_mode* a la valeur **1**, ce qui correspond à l’authentification Windows.  
  
`[ @password = ] 'password'` Mot de passe de l’administrateur système à utiliser lors de la création d’objets système dans la base de données de distribution. *Password* est de **type sysname**, avec **«»** comme valeur par défaut (chaîne vide). Ce paramètre n’est pas requis si *security_mode* a la valeur **1**, ce qui correspond à l’authentification Windows.  
  
`[ @security_mode = ] 'security_mode'` Mode de sécurité de connexion à utiliser lors de la création d’objets système dans la base de données de distribution. *security_mode* est de **bits** avec **1** comme valeur par défaut. Si la **valeur est 0**, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l’authentification est utilisée. Si la condition est **1**, l’authentification Windows sera utilisée. [!INCLUDE[ssNoteWinAuthentication](../../includes/ssnotewinauthentication-md.md)]  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_vupgrade_mergeobjects** est utilisé uniquement pour la réplication de fusion.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle serveur fixe **sysadmin** .  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)   
 [Mettre à niveau des bases de données répliquées](../../database-engine/install-windows/upgrade-replicated-databases.md)  
  
  
