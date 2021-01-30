---
description: sp_addpushsubscription_agent (Transact-SQL)
title: sp_addpushsubscription_agent (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/09/2020
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_addpushsubscription_agent_TSQL
- sp_addpushsubscription_agent
helpviewer_keywords:
- sp_addpushsubscription_agent
ms.assetid: 1fdd2052-50d8-4318-8aa7-fc635d5cad18
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 396e90bb76296270bf25d236a7fd20cd34a063dc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206672"
---
# <a name="sp_addpushsubscription_agent-transact-sql"></a>sp_addpushsubscription_agent (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Ajoute un nouveau travail de l'agent planifié utilisé pour synchroniser un abonnement envoyé avec une publication transactionnelle. Cette procédure stockée est exécutée sur le serveur de publication dans la base de données de publication.  
  
> [!IMPORTANT]  
>  Lors de la configuration d'un serveur de publication avec un serveur de distribution distant, les valeurs fournies pour tous les paramètres, y compris *job_login* et *job_password*, sont envoyées en texte brut au serveur de distribution. Vous devez chiffrer la connexion entre le serveur de publication et son serveur de distribution distant avant d'exécuter cette procédure stockée. Pour plus d’informations, consultez [Activer des connexions chiffrées dans le moteur de base de données &#40;Gestionnaire de configuration SQL Server&#41;](../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md).  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_addpushsubscription_agent [ @publication= ] 'publication'  
    [ , [ @subscriber = ] 'subscriber' ]  
    [ , [ @subscriber_db = ] 'subscriber_db' ]  
    [ , [ @subscriber_security_mode = ] subscriber_security_mode ]  
    [ , [ @subscriber_login = ] 'subscriber_login' ]  
    [ , [ @subscriber_password = ] 'subscriber_password' ]  
    [ , [ @job_login = ] 'job_login' ]   
    [ , [ @job_password = ] 'job_password' ]   
    [ , [ @job_name = ] 'job_name' ]   
    [ , [ @frequency_type = ] frequency_type ]  
    [ , [ @frequency_interval = ] frequency_interval ]  
    [ , [ @frequency_relative_interval = ] frequency_relative_interval ]  
    [ , [ @frequency_recurrence_factor = ] frequency_recurrence_factor ]  
    [ , [ @frequency_subday = ] frequency_subday ]  
    [ , [ @frequency_subday_interval = ] frequency_subday_interval ]  
    [ , [ @active_start_time_of_day = ] active_start_time_of_day ]  
    [ , [ @active_end_time_of_day = ] active_end_time_of_day ]  
    [ , [ @active_start_date = ] active_start_date ]  
    [ , [ @active_end_date = ] active_end_date ]  
    [ , [ @dts_package_name = ] 'dts_package_name' ]  
    [ , [ @dts_package_password = ] 'dts_package_password' ]  
    [ , [ @dts_package_location = ] 'dts_package_location' ]  
    [ , [ @enabled_for_syncmgr = ] 'enabled_for_syncmgr' ]  
    [ , [ @distribution_job_name = ] 'distribution_job_name' ]  
    [ , [ @publisher = ] 'publisher' ]  
    [ , [ @subscriber_provider = ] 'subscriber_provider' ]   
    [ , [ @subscriber_datasrc = ] 'subscriber_datasrc' ]   
    [ , [ @subscriber_location = ] 'subscriber_location' ]  
    [ , [ @subscriber_provider_string = ] 'subscriber_provider_string' ]   
    [ , [ @subscriber_catalog = ] 'subscriber_catalog' ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @publication = ] 'publication'` Nom de la publication. *publication* est de **type sysname**, sans valeur par défaut.  
  
`[ @subscriber = ] 'subscriber'` Nom de l’instance de l’abonné ou nom de l’écouteur GA si la base de données de l’abonné est un groupe de disponibilité. *Subscriber* est de **type sysname**, avec NULL comme valeur par défaut. 

> [!NOTE]
> Le nom du serveur peut être spécifié sous la forme `<Hostname>,<PortNumber>` . Vous devrez peut-être spécifier le numéro de port de votre connexion lorsque SQL Server est déployé sur Linux ou Windows avec un port personnalisé, et que le service Browser est désactivé.

`[ @subscriber_db = ] 'subscriber_db'` Nom de la base de données d’abonnement. *subscriber_db* est de **type sysname**, avec NULL comme valeur par défaut. Pour un abonné non-SQL Server, spécifiez la valeur **(destination par défaut)** pour *subscriber_db*.  
  
`[ @subscriber_security_mode = ] subscriber_security_mode` Mode de sécurité à utiliser lors de la connexion à un abonné au cours d’une synchronisation. *subscriber_security_mode* est de **type int**, avec 1 comme valeur par défaut. **0** spécifie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l’authentification. **1** spécifie l’authentification Windows.  
  
> [!IMPORTANT]  
>  Pour les abonnements mis à jour en attente, utilisez l'authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour les connexions aux abonnés et spécifiez un compte différent pour la connexion à chaque abonné. Pour tous les autres abonnements, utilisez l'authentification Windows.  
  
`[ @subscriber_login = ] 'subscriber_login'` Connexion de l’abonné à utiliser lors de la connexion à un abonné au cours d’une synchronisation. *subscriber_login* est de **type sysname**, avec NULL comme valeur par défaut.  
  
`[ @subscriber_password = ] 'subscriber_password'` Mot de passe de l’abonné. *subscriber_password* est requis si *subscriber_security_mode* a la valeur **0**. *subscriber_password* est de **type sysname**, avec NULL comme valeur par défaut. Si un mot de passe d'abonné est utilisé, il est automatiquement chiffré.  
  
> [!IMPORTANT]  
>  N'utilisez pas de mot de passe vide. Utilisez un mot de passe fort. Lorsque c'est possible, demande aux utilisateurs de fournir les informations d'identification au moment de l'exécution. Si vous devez enregistrer les informations d'identification dans un fichier de script, vous devez sécuriser le fichier pour empêcher un accès non autorisé.  
  
`[ @job_login = ] 'job_login'` Nom de connexion du compte sous lequel l’agent s’exécute. Sur Azure SQL Managed Instance utilisez un compte SQL Server. *job_login* est de type **nvarchar (257)**, avec NULL comme valeur par défaut. Ce compte Windows est toujours utilisé pour les connexions de l'Agent au serveur de distribution et pour les connexions à l'abonné utilisant l'authentification intégrée de Windows.  
  
`[ @job_password = ] 'job_password'` Mot de passe du compte sous lequel l’agent s’exécute. *job_password* est de **type sysname**, sans valeur par défaut.  
  
> [!IMPORTANT]  
>  Lorsque c'est possible, demande aux utilisateurs de fournir les informations d'identification au moment de l'exécution. Si vous devez enregistrer les informations d'identification dans un fichier de script, vous devez sécuriser le fichier pour empêcher un accès non autorisé.  
  
`[ @job_name = ] 'job_name'` Nom d’un travail d’agent existant. *job_name* est de **type sysname**, avec NULL comme valeur par défaut. Ce paramètre n'est indiqué que lorsque l'abonnement est synchronisé grâce à un travail existant plutôt qu'un nouveau travail (étant le comportement par défaut). Si vous n’êtes pas membre du rôle serveur fixe **sysadmin** , vous devez spécifier *job_login* et *job_password* lorsque vous spécifiez *job_name*.  
  
`[ @frequency_type = ] frequency_type` Fréquence de planification de l’Agent de distribution. *frequency_type* est de **type int** et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**1**|Ponctuelle|  
|**2**|À la demande|  
|**4**|Quotidien|  
|**8**|Hebdomadaire|  
|**16**|Mensuelle|  
|**32**|Mensuelle relative|  
|**64** (valeur par défaut)|Démarrage automatique|  
|**128**|Périodique|  
  
> [!NOTE]  
>  Si vous spécifiez la valeur **64** , le agent de distribution s’exécute en mode continu. Cela correspond à la définition du paramètre **-Continuous** pour l’agent. Pour plus d'informations, consultez [Replication Distribution Agent](../../relational-databases/replication/agents/replication-distribution-agent.md).  
  
`[ @frequency_interval = ] frequency_interval` Valeur à appliquer à la fréquence définie par *frequency_type*. *frequency_interval* est de **type int**, avec 1 comme valeur par défaut.  
  
`[ @frequency_relative_interval = ] frequency_relative_interval` Date de la Agent de distribution. Ce paramètre est utilisé lorsque *frequency_type* a la valeur **32** (mensuelle relative). *frequency_relative_interval* est de **type int** et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**1** (par défaut)|Premier|  
|**2**|Seconde|  
|**4**|Third|  
|**8**|Quatrième|  
|**16**|Dernier|  
  
`[ @frequency_recurrence_factor = ] frequency_recurrence_factor` Facteur de récurrence utilisé par *frequency_type*. *frequency_recurrence_factor* est de **type int**, avec 0 comme valeur par défaut.  
  
`[ @frequency_subday = ] frequency_subday` Fréquence de replanification au cours de la période définie. *frequency_subday* est de **type int** et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**1**|Une fois|  
|**2**|Seconde|  
|**4** (par défaut)|Minute|  
|**8**|Heure|  
  
`[ @frequency_subday_interval = ] frequency_subday_interval` Intervalle de *frequency_subday*. *frequency_subday_interval* est de **type int**, avec 5 comme valeur par défaut.  
  
`[ @active_start_time_of_day = ] active_start_time_of_day` Heure de la journée à laquelle le Agent de distribution est planifié pour la première fois, au format HHMMSS. *active_start_time_of_day* est de **type int**, avec 0 comme valeur par défaut.  
  
`[ @active_end_time_of_day = ] active_end_time_of_day` Heure de la journée à laquelle le Agent de distribution cesse d’être planifié, au format HHMMSS. *active_end_time_of_day* est de **type int**, avec 235959 comme valeur par défaut.  
  
`[ @active_start_date = ] active_start_date` Date à laquelle le Agent de distribution est planifié pour la première fois, au format AAAAMMJJ. *active_start_date* est de **type int**, avec 0 comme valeur par défaut.  
  
`[ @active_end_date = ] active_end_date` Date à laquelle le Agent de distribution cesse d’être planifié, au format AAAAMMJJ. *active_end_date* est de **type int**, avec 99991231 comme valeur par défaut.  
  
`[ @dts_package_name = ] 'dts_package_name'` Spécifie le nom du package DTS (Data Transformation Services). *dts_package_name* est de **type sysname** , avec NULL comme valeur par défaut. Par exemple, pour spécifier le nom de package `DTSPub_Package`, le paramètre est le suivant : `@dts_package_name = N'DTSPub_Package'`.  
  
`[ @dts_package_password = ] 'dts_package_password'` Spécifie le mot de passe requis pour exécuter le package. *dts_package_password* est de **type sysname** , avec NULL comme valeur par défaut.  
  
> [!NOTE]  
>  Vous devez spécifier un mot de passe si *dts_package_name* est spécifié.  
  
`[ @dts_package_location = ] 'dts_package_location'` Spécifie l’emplacement du package. *dts_package_location* est de type **nvarchar (12)**, avec Distributor comme valeur par défaut. L’emplacement du package peut être **Distributor** ou **Subscriber**.  
  
`[ @enabled_for_syncmgr = ] 'enabled_for_syncmgr'` Indique si l’abonnement peut être synchronisé via le [!INCLUDE[msCoName](../../includes/msconame-md.md)] Gestionnaire de synchronisation. *enabled_for_syncmgr* est de type **nvarchar (5)**, avec false comme valeur par défaut. Si la **valeur est false**, l’abonnement n’est pas inscrit auprès du gestionnaire de synchronisation. Si la **valeur est true**, l’abonnement est inscrit auprès du gestionnaire de synchronisation et peut être synchronisé sans démarrage [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] .  
  
`[ @distribution_job_name = ] 'distribution_job_name'` [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
`[ @publisher = ] 'publisher'` Nom du serveur de publication. *Publisher* est de **type sysname**, avec NULL comme valeur par défaut.  
  
`[ @subscriber_provider = ] 'subscriber_provider'` Identificateur programmatique unique (PROGID) avec lequel le fournisseur OLE DB de la source de données non- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est inscrit. *subscriber_provider* est de **type sysname**, avec NULL comme valeur par défaut. *subscriber_provider* doit être unique pour le fournisseur OLE DB installé sur le serveur de distribution. *subscriber_provider* n’est pris en charge que pour les abonnés non- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
`[ @subscriber_datasrc = ] 'subscriber_datasrc'` Nom de la source de données, tel qu’il est interprété par le fournisseur OLE DB. *subscriber_datasrc* est de type **nvarchar (4000)**, avec NULL comme valeur par défaut. *subscriber_datasrc* est passé comme DBPROP_INIT_DATASOURCE propriété pour initialiser le fournisseur de OLE DB. *subscriber_datasrc* n’est pris en charge que pour les abonnés non- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
`[ @subscriber_location = ] 'subscriber_location'` Est l’emplacement de la base de données tel qu’il est interprété par le fournisseur OLE DB. *subscriber_location* est de type **nvarchar (4000)**, avec NULL comme valeur par défaut. *subscriber_location* est passé comme DBPROP_INIT_LOCATION propriété pour initialiser le fournisseur de OLE DB. *subscriber_location* n’est pris en charge que pour les abonnés non- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
`[ @subscriber_provider_string = ] 'subscriber_provider_string'` Chaîne de connexion spécifique au fournisseur OLE DB qui identifie la source de données. *subscriber_provider_string* est de type **nvarchar (4000)**, avec NULL comme valeur par défaut. *subscriber_provider_string* est passé à IDataInitialize ou défini en tant que propriété DBPROP_INIT_PROVIDERSTRING pour initialiser le fournisseur OLE DB. *subscriber_provider_string* n’est pris en charge que pour les abonnés non- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
`[ @subscriber_catalog = ] 'subscriber_catalog'` Catalogue à utiliser lors de l’établissement d’une connexion au fournisseur OLE DB. *subscriber_catalog* est de **type sysname**, avec NULL comme valeur par défaut. *subscriber_catalog* est passé comme DBPROP_INIT_CATALOG propriété pour initialiser le fournisseur de OLE DB. *subscriber_catalog* n’est pris en charge que pour les abonnés non- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_addpushsubscription_agent** est utilisé dans la réplication d’instantané et la réplication transactionnelle.  
  
## <a name="example"></a>Exemple  
 [!code-sql[HowTo#sp_addtranpushsubscription_agent](../../relational-databases/replication/codesnippet/tsql/sp-addpushsubscription-a_1.sql)]  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** ou du rôle de base de données fixe **db_owner** peuvent exécuter **sp_addpushsubscription_agent**.  
  
## <a name="see-also"></a>Voir aussi  
 [Create a Push Subscription](../../relational-databases/replication/create-a-push-subscription.md)   
 [Créer un abonnement pour un abonné non-SQL Server](../../relational-databases/replication/create-a-subscription-for-a-non-sql-server-subscriber.md)   
 [S’abonner aux Publications](../../relational-databases/replication/subscribe-to-publications.md)   
 [Procédures stockées de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)   
 [sp_addsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md)   
 [sp_changesubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changesubscription-transact-sql.md)   
 [sp_dropsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropsubscription-transact-sql.md)   
 [sp_helpsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpsubscription-transact-sql.md)  
  
  
