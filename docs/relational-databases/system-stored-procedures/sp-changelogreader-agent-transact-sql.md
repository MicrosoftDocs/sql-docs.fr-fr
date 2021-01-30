---
description: sp_changelogreader_agent (Transact-SQL)
title: sp_changelogreader_agent (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/15/2018
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_changelogreader_agent
- sp_changelogreader_agent_TSQL
helpviewer_keywords:
- sp_changelogreader_agent
ms.assetid: 929b2fa7-1267-41d0-8b69-e9ab26a62c0f
author: markingmyname
ms.author: maghan
ms.openlocfilehash: fef379d9ae9e103904dfb0ef1b765305daa6acc6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99158978"
---
# <a name="sp_changelogreader_agent-transact-sql"></a>sp_changelogreader_agent (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Modifie les propriétés de sécurité d'un Agent de lecture du journal. Cette procédure stockée est exécutée sur le serveur de publication dans la base de données de publication.  
  
> [!IMPORTANT]  
>  Lors de la configuration d'un serveur de publication avec un serveur de distribution distant, les valeurs fournies pour tous les paramètres, y compris *job_login* et *job_password*, sont envoyées en texte brut au serveur de distribution. Vous devez chiffrer la connexion entre le serveur de publication et son serveur de distribution distant avant d'exécuter cette procédure stockée. Pour plus d’informations, consultez [Activer des connexions chiffrées dans le moteur de base de données &#40;Gestionnaire de configuration SQL Server&#41;](../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md).  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_changelogreader_agent [ [ @job_login = ] 'job_login' ]  
    [ , [ @job_password = ] 'job_password' ]  
    [ , [ @publisher_security_mode = ] publisher_security_mode ]  
    [ , [ @publisher_login = ] 'publisher_login' ]  
    [ , [ @publisher_password = ] 'publisher_password' ]   
    [ , [ @publisher = ] 'publisher' ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @job_login = ] 'job_login'` Nom de connexion du compte sous lequel l’agent s’exécute. *job_login* est de type **nvarchar (257)**, avec NULL comme valeur par défaut. Sur Azure SQL Managed Instance, utilisez un compte SQL Server. *Cela ne peut pas être modifié pour un non-* [!INCLUDE[msCoName](../../includes/msconame-md.md)] serveur de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] *publication.*  
  
`[ @job_password = ] 'job_password'` Mot de passe du compte sous lequel l’agent s’exécute. *job_password* est de **type sysname**, avec NULL comme valeur par défaut.  
  
> [!IMPORTANT]  
>  Lorsque c'est possible, demande aux utilisateurs de fournir les informations d'identification au moment de l'exécution. Si vous devez enregistrer les informations d'identification dans un fichier de script, vous devez sécuriser le fichier pour empêcher un accès non autorisé.  
  
`[ @publisher_security_mode = ] publisher_security_mode` Mode de sécurité utilisé par l’agent lors de la connexion au serveur de publication. *publisher_security_mode* est de type **smallint**, avec NULL comme valeur par défaut. **0** spécifie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l’authentification et **1** spécifie l’authentification Windows.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteWinAuthentication](../../includes/ssnotewinauthentication-md.md)]  
  
`[ @publisher_login = ] 'publisher_login'` Nom de connexion utilisé lors de la connexion au serveur de publication. *publisher_login* est de **type sysname**, avec NULL comme valeur par défaut. *publisher_login* doit être spécifié lorsque *publisher_security_mode* a la **valeur 0**. Si *publisher_login* a la valeur null et que *publisher_security_mode* a la valeur **1**, le compte Windows spécifié dans *job_login* est utilisé lors de la connexion au serveur de publication.  
  
`[ @publisher_password = ] 'publisher_password'` Mot de passe utilisé lors de la connexion au serveur de publication. *publisher_password* est de **type sysname**, avec NULL comme valeur par défaut.  
  
> [!IMPORTANT]  
>  N'utilisez pas de mot de passe vide. Utilisez un mot de passe fort. Lorsque c'est possible, demande aux utilisateurs de fournir les informations d'identification au moment de l'exécution. Si vous devez enregistrer les informations d'identification dans un fichier de script, vous devez sécuriser le fichier pour empêcher un accès non autorisé.  
  
`[ @publisher = ] 'publisher'` Nom du serveur de publication. *Publisher* est de **type sysname**, avec NULL comme valeur par défaut. Ce paramètre est uniquement pris en charge pour les serveurs de publication non SQL Server.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_changelogreader_agent** est utilisé dans la réplication transactionnelle.  
  
 **sp_changelogreader_agent** est utilisé pour modifier le compte Windows sous lequel un agent de lecture du journal s’exécute. Vous pouvez changer le mot de passe d'une connexion Windows existante ou fournir une nouvelle connexion et un nouveau mot de passe Windows.  
  
 Après avoir modifié le nom de connexion ou le mot de passe d'un Agent, vous devez arrêter et redémarrer celui-ci avant que la modification prenne effet.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** ou du rôle de base de données fixe **db_owner** peuvent exécuter **sp_changelogreader_agent**.  
  
## <a name="see-also"></a>Voir aussi  
 [Afficher et modifier les paramètres de sécurité de la réplication](../../relational-databases/replication/security/view-and-modify-replication-security-settings.md)   
 [sp_helplogreader_agent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helplogreader-agent-transact-sql.md)   
 [sp_addlogreader_agent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addlogreader-agent-transact-sql.md)  
  
  
