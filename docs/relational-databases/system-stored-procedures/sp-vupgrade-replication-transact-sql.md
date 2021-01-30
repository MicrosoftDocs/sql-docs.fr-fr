---
description: sp_vupgrade_replication (Transact-SQL)
title: sp_vupgrade_replication (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_vupgrade_replication_TSQL
- sp_vupgrade_replication
helpviewer_keywords:
- sp_vupgrade_replication
ms.assetid: d2c0ed66-07d1-4adc-82e5-a654376879bc
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 7a47e06ee2d762763922974074927ea50e3944e4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201810"
---
# <a name="sp_vupgrade_replication-transact-sql"></a>sp_vupgrade_replication (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Activé par le programme d'installation lors de la mise à niveau d'un serveur de réplication. Met à niveau le schéma et les données système afin que la réplication soit prise en charge par la version actuelle du produit. Crée de nouveaux objets système de réplication dans les bases de données système et utilisateur. Cette procédure stockée est exécutée sur la machine sur laquelle la mise à niveau de la réplication doit intervenir.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_vupgrade_replication [ [@login=] 'login' ]  
    [ , [ @password= ] 'password' ]  
    [ , [ @ver_old= ] 'old_version' ]  
    [ , [ @force_remove= ] 'force_removal' ]  
    [ , [ @security_mode= ] security_mode ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @login = ] 'login'` Connexion de l’administrateur système à utiliser lors de la création d’objets système dans la base de données de distribution. *login* est de type **sysname**, avec NULL comme valeur par défaut. Ce paramètre n’est pas requis si *security_mode* a la valeur **1**, ce qui correspond à l’authentification Windows.  
  
> [!NOTE]  
>  Ce paramètre est ignoré lorsque vous effectuez une mise à niveau vers [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] et versions ultérieures.  
  
`[ @password = ] 'password'` Mot de passe de l’administrateur système à utiliser lors de la création d’objets système dans la base de données de distribution. *Password* est de **type sysname**, avec **«»** comme valeur par défaut (chaîne vide). Ce paramètre n’est pas requis si *security_mode* a la valeur **1**, ce qui correspond à l’authentification Windows.  
  
> [!NOTE]  
>  Ce paramètre est ignoré lorsque vous effectuez une mise à niveau vers SQL [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] et versions ultérieures.  
  
`[ @ver_old = ] 'old_version'` [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
 Cette procédure stockée est déconseillée et sera supprimée dans les prochaines versions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
`[ @force_remove = ] 'force_removal'` [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
`[ @security_mode = ] 'security_mode'` Mode de sécurité de connexion à utiliser lors de la création d’objets système dans la base de données de distribution. *security_mode* est de **bits** avec **0** comme valeur par défaut. Si la **valeur est 0**, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l’authentification est utilisée. Si la condition est **1**, l’authentification Windows sera utilisée.  
  
> [!NOTE]  
>  Ce paramètre est ignoré lorsque vous effectuez une mise à niveau vers [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] et versions ultérieures.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_vupgrade_replication** est utilisé lors de la mise à niveau de tous les types de réplication.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** peuvent exécuter **sp_vupgrade_replication**.  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)   
 [Valider des données répliquées](../../relational-databases/replication/validate-data-at-the-subscriber.md)  
  
  
