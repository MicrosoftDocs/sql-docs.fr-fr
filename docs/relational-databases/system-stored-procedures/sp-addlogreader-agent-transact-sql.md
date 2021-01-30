---
description: sp_addlogreader_agent (Transact-SQL)
title: sp_addlogreader_agent (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_addlogreader_agent
- sp_addlogreader_agent_TSQL
helpviewer_keywords:
- sp_addlogreader_agent
ms.assetid: d83096b9-96ee-4789-bde0-940d4765b9ed
author: mashamsft
ms.author: mathoma
ms.openlocfilehash: a511c4f47f84a8f083dbab86bf1d851762142d90
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202363"
---
# <a name="sp_addlogreader_agent-transact-sql"></a>sp_addlogreader_agent (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Ajoute un Agent de lecture du journal pour une base de données. Cette procédure stockée est exécutée sur le serveur de publication dans la base de données de publication.  
  
> [!IMPORTANT]  
>  Lors de la configuration d'un serveur de publication avec un serveur de distribution distant, les valeurs fournies pour tous les paramètres, y compris *job_login* et *job_password*, sont envoyées en texte brut au serveur de distribution. Vous devez chiffrer la connexion entre le serveur de publication et son serveur de distribution distant avant d'exécuter cette procédure stockée. Pour plus d’informations, consultez [Activer des connexions chiffrées dans le moteur de base de données &#40;Gestionnaire de configuration SQL Server&#41;](../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md).  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_addlogreader_agent [ @job_login = ] 'job_login'  
        , [ @job_password = ] 'job_password'  
    [ , [ @job_name = ] 'job_name' ]  
    [ , [ @publisher_security_mode = ] publisher_security_mode ]  
    [ , [ @publisher_login = ] 'publisher_login' ]  
    [ , [ @publisher_password = ] 'publisher_password' ]   
    [ , [ @publisher = ] 'publisher' ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @job_login = ] 'job_login'` Nom de connexion du [!INCLUDE[msCoName](../../includes/msconame-md.md)] compte Windows sous lequel l’agent s’exécute. *job_login* est de type **nvarchar (257)**, avec NULL comme valeur par défaut. Ce compte Windows est toujours utilisé pour les connexions des agents au serveur de distribution.  
  
> [!NOTE]
>  Pour les serveurs de publication non- [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , il doit s’agir de la même connexion que celle spécifiée dans [sp_adddistpublisher &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-adddistpublisher-transact-sql.md).  
  
`[ @job_password = ] 'job_password'` Mot de passe du compte Windows sous lequel l’agent s’exécute. *job_password* est de **type sysname**, avec NULL comme valeur par défaut.  
  
> [!IMPORTANT]  
>  Ne stockez pas les informations d'authentification dans des fichiers de script. Pour une sécurité optimale, les noms de connexion et les mots de passe doivent être fournis au moment de l'exécution.  
  
`[ @job_name = ] 'job_name'` Nom d’un travail d’agent existant. *job_name* est de **type sysname**, avec NULL comme valeur par défaut. Ce paramètre n'est défini que lorsque l'Agent est démarré avec un travail existant au lieu d'un nouveau travail (la valeur par défaut).  
  
`[ @publisher_security_mode = ] publisher_security_mode` Mode de sécurité utilisé par l’agent lors de la connexion au serveur de publication. *publisher_security_mode* est de type **smallint**, avec **1** comme valeur par défaut. **0** spécifie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l’authentification et **1** spécifie l’authentification Windows. La valeur **0** doit être spécifiée pour les serveurs de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] publication non-.  
  
`[ @publisher_login = ] 'publisher_login'` Nom de connexion utilisé lors de la connexion au serveur de publication. *publisher_login* est de **type sysname**, avec NULL comme valeur par défaut. *publisher_login* doit être spécifié lorsque *publisher_security_mode* a la **valeur 0**. Si *publisher_login* a la valeur null et que *publisher_security_mode* a la valeur **1**, le compte Windows spécifié dans *job_login* sera utilisé lors de la connexion au serveur de publication.  
  
`[ @publisher_password = ] 'publisher_password'` Mot de passe utilisé lors de la connexion au serveur de publication. *publisher_password* est de **type sysname**, avec NULL comme valeur par défaut.  
  
> [!IMPORTANT]  
>  Ne stockez pas les informations d'authentification dans des fichiers de script. Pour une sécurité optimale, les noms de connexion et les mots de passe doivent être fournis au moment de l'exécution.  
  
`[ @publisher = ] 'publisher'` Nom du serveur de publication non- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . *Publisher* est de **type sysname**, avec NULL comme valeur par défaut.  
  
> [!NOTE]  
>  Ne spécifiez pas ce paramètre pour un serveur de publication [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_addlogreader_agent** est utilisé dans la réplication transactionnelle.  
  
 Vous devez exécuter **sp_addlogreader_agent** pour ajouter un agent de lecture du journal si vous avez mis à niveau une base de données qui a été activée pour la réplication vers cette version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] avant la création d’une publication qui utilisait la base de données.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** ou du rôle de base de données fixe **db_owner** peuvent exécuter **sp_addlogreader_agent**.  
  
## <a name="example"></a>Exemple  
 [!code-sql[HowTo#sp_AddTranPub](../../relational-databases/replication/codesnippet/tsql/sp-addlogreader-agent-tr_1.sql)]  
  
## <a name="see-also"></a>Voir aussi  
 [Create a Publication](../../relational-databases/replication/publish/create-a-publication.md)   
 [sp_addpublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addpublication-transact-sql.md)   
 [sp_changelogreader_agent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changelogreader-agent-transact-sql.md)   
 [Procédures stockées de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
