---
description: sp_addpullsubscription_agent (Transact-SQL)
title: sp_addpullsubscription_agent (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/09/2020
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_addpullsubscription_agent
- sp_addpullsubscription_agent_TSQL
helpviewer_keywords:
- sp_addpullsubscription_agent
ms.assetid: b9c2eaed-6d2d-4b78-ae9b-73633133180b
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 031911ae4e9f6f81e82ec9d0b4fb20c73d94a2cd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206689"
---
# <a name="sp_addpullsubscription_agent-transact-sql"></a>sp_addpullsubscription_agent (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
 
  Ajoute un nouveau travail de l'Agent planifié, utilisé pour synchroniser un abonnement par extraction de données (pull) avec une publication transactionnelle. Cette procédure stockée est exécutée sur la base de données d'abonnement de l'Abonné.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_addpullsubscription_agent [ @publisher = ] 'publisher'  
    [ , [ @publisher_db = ] 'publisher_db' ]          , [ @publication = ] 'publication'  
    [ , [ @subscriber = ] 'subscriber' ]  
    [ , [ @subscriber_db = ] 'subscriber_db' ]  
    [ , [ @subscriber_security_mode = ] subscriber_security_mode ]  
    [ , [ @subscriber_login = ] 'subscriber_login' ]  
    [ , [ @subscriber_password = ] 'subscriber_password' ]  
    [ , [ @distributor = ] 'distributor' ]  
    [ , [ @distribution_db = ] 'distribution_db' ]  
    [ , [ @distributor_security_mode = ] distributor_security_mode ]  
    [ , [ @distributor_login = ] 'distributor_login' ]  
    [ , [ @distributor_password = ] 'distributor_password' ]  
    [ , [ @optional_command_line = ] 'optional_command_line' ]  
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
    [ , [ @distribution_jobid = ] distribution_jobid OUTPUT ]  
    [ , [ @encrypted_distributor_password = ] encrypted_distributor_password ]  
    [ , [ @enabled_for_syncmgr = ] 'enabled_for_syncmgr' ]  
    [ , [ @ftp_address = ] 'ftp_address' ]  
    [ , [ @ftp_port = ] ftp_port ]  
    [ , [ @ftp_login = ] 'ftp_login' ]  
    [ , [ @ftp_password = ] 'ftp_password' ]  
    [ , [ @alt_snapshot_folder = ] 'alternate_snapshot_folder' ]  
    [ , [ @working_directory = ] 'working_directory' ]  
    [ , [ @use_ftp = ] 'use_ftp' ]  
    [ , [ @publication_type = ] publication_type ]  
    [ , [ @dts_package_name = ] 'dts_package_name' ]  
    [ , [ @dts_package_password = ] 'dts_package_password' ]  
    [ , [ @dts_package_location = ] 'dts_package_location' ]  
    [ , [ @reserved = ] 'reserved' ]  
    [ , [ @offloadagent = ] 'remote_agent_activation' ]  
    [ , [ @offloadserver = ] 'remote_agent_server_name']  
    [ , [ @job_name = ] 'job_name' ]  
    [ , [ @job_login = ] 'job_login' ]   
    [ , [ @job_password = ] 'job_password' ]   
```  
  
## <a name="arguments"></a>Arguments  
`[ @publisher = ] 'publisher'` Nom du serveur de publication. *Publisher* est de **type sysname**, sans valeur par défaut.  

> [!NOTE]
> Le nom du serveur peut être spécifié sous la forme `<Hostname>,<PortNumber>` . Vous devrez peut-être spécifier le numéro de port de votre connexion lorsque SQL Server est déployé sur Linux ou Windows avec un port personnalisé, et que le service Browser est désactivé.
  
`[ @publisher_db = ] 'publisher_db'_` Nom de la base de données du serveur de publication. *publisher_db* est de **type sysname**, avec NULL comme valeur par défaut. *publisher_db* est ignorée par les serveurs de publication Oracle.  
  
`[ @publication = ] 'publication'` Nom de la publication. *publication* est de **type sysname**, sans valeur par défaut.  
  
`[ @subscriber = ] 'subscriber'` Nom de l’instance de l’abonné ou nom de l’écouteur GA si la base de données de l’abonné se trouve dans un groupe de disponibilité. *Subscriber* est de **type sysname**, avec NULL comme valeur par défaut.  
  
> [!NOTE]  
>  Ce paramètre est déconseillé et n'est maintenu que dans un but de compatibilité ascendante avec les scripts déjà établis.  
  
`[ @subscriber_db = ] 'subscriber_db'` Nom de la base de données d’abonnement. *subscriber_db* est de **type sysname**, avec NULL comme valeur par défaut.  
  
> [!NOTE]  
>  Ce paramètre est déconseillé et n'est maintenu que dans un but de compatibilité ascendante avec les scripts déjà établis.  
  
`[ @subscriber_security_mode = ] subscriber_security_mode` Mode de sécurité à utiliser lors de la connexion à un abonné au cours d’une synchronisation. *subscriber_security_mode* est de **type int,** avec NULL comme valeur par défaut. **0** spécifie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l’authentification. **1** spécifie l’authentification Windows.  
  
> [!NOTE]  
>  Ce paramètre est déconseillé et n'est maintenu que dans un but de compatibilité ascendante avec les scripts déjà établis. L'Agent de distribution se connecte toujours à l'Abonné local à l'aide de l'authentification Windows. Si une valeur autre que NULL ou **1** est spécifiée pour ce paramètre, un message d’avertissement est retourné.  
  
`[ @subscriber_login = ] 'subscriber_login'` Connexion de l’abonné à utiliser lors de la connexion à un abonné au cours d’une synchronisation. *subscriber_login* est de **type sysname**, avec NULL comme valeur par défaut.  
  
> [!NOTE]  
>  Ce paramètre est déconseillé et n'est maintenu que dans un but de compatibilité ascendante avec les scripts déjà établis. Si une valeur est précisée pour ce paramètre, un message d'avertissement est retourné mais la valeur reste ignorée.  
  
`[ @subscriber_password = ] 'subscriber_password'` Mot de passe de l’abonné. *subscriber_password* est requis si *subscriber_security_mode* a la valeur **0**. *subscriber_password* est de **type sysname**, avec NULL comme valeur par défaut. Si un mot de passe d'abonné est utilisé, il est automatiquement chiffré.  
  
> [!NOTE]  
>  Ce paramètre est déconseillé et n'est maintenu que dans un but de compatibilité ascendante avec les scripts déjà établis. Si une valeur est précisée pour ce paramètre, un message d'avertissement est retourné mais la valeur reste ignorée.  
  
`[ @distributor = ] 'distributor'` Nom du serveur de distribution. *Distributor* est de **type sysname**, avec la valeur par défaut spécifiée par le serveur de *publication*.  
  
`[ @distribution_db = ] 'distribution_db'` Nom de la base de données de distribution. *distribution_db* est de **type sysname**, avec NULL comme valeur par défaut.  
  
`[ @distributor_security_mode = ] distributor_security_mode` Mode de sécurité à utiliser lors de la connexion à un serveur de distribution lors de la synchronisation. *distributor_security_mode* est de **type int**, avec **1** comme valeur par défaut. **0** spécifie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l’authentification. **1** spécifie l’authentification Windows.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteWinAuthentication](../../includes/ssnotewinauthentication-md.md)]  
  
`[ @distributor_login = ] 'distributor_login'` Connexion du serveur de distribution à utiliser lors de la connexion à un serveur de distribution lors de la synchronisation. *distributor_login* est requis si *distributor_security_mode* a la valeur **0**. *distributor_login* est de **type sysname**, avec NULL comme valeur par défaut.  
  
`[ @distributor_password = ] 'distributor_password'` Est le mot de passe du serveur de distribution. *distributor_password* est requis si *distributor_security_mode* a la valeur **0**. *distributor_password* est de **type sysname**, avec NULL comme valeur par défaut.  
  
> [!IMPORTANT]  
>  N'utilisez pas de mot de passe vide. Utilisez un mot de passe fort. Lorsque c'est possible, demande aux utilisateurs de fournir les informations d'identification au moment de l'exécution. Si vous devez enregistrer les informations d'identification dans un fichier de script, vous devez sécuriser le fichier pour empêcher un accès non autorisé.  
  
`[ @optional_command_line = ] 'optional_command_line'` Est une invite de commandes facultative fournie au Agent de distribution. Par exemple, **-DefinitionFile** C:\Distdef.txt ou **-CommitBatchSize** 10. *optional_command_line* est de type **nvarchar (4000)**, avec une chaîne vide comme valeur par défaut.  
  
`[ @frequency_type = ] frequency_type` Fréquence de planification de l’Agent de distribution. *frequency_type* est de **type int** et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**1**|Ponctuelle|  
|**2** (par défaut)|À la demande|  
|**4**|Quotidien|  
|**8**|Hebdomadaire|  
|**16**|Mensuelle|  
|**32**|Mensuelle relative|  
|**64**|Démarrage automatique|  
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
  
`[ @frequency_recurrence_factor = ] frequency_recurrence_factor` Facteur de récurrence utilisé par *frequency_type*. *frequency_recurrence_factor* est de **type int**, avec **1** comme valeur par défaut.  
  
`[ @frequency_subday = ] frequency_subday` Fréquence de replanification au cours de la période définie. *frequency_subday* est de **type int** et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**1** (par défaut)|Une fois|  
|**2**|Seconde|  
|**4**|Minute|  
|**8**|Heure|  
  
`[ @frequency_subday_interval = ] frequency_subday_interval` Intervalle de *frequency_subday*. *frequency_subday_interval* est de **type int**, avec **1** comme valeur par défaut.  
  
`[ @active_start_time_of_day = ] active_start_time_of_day` Heure de la journée à laquelle le Agent de distribution est planifié pour la première fois, au format HHMMSS. *active_start_time_of_day* est de **type int**, avec **0** comme valeur par défaut.  
  
`[ @active_end_time_of_day = ] active_end_time_of_day` Heure de la journée à laquelle le Agent de distribution cesse d’être planifié, au format HHMMSS. *active_end_time_of_day* est de **type int**, avec **0** comme valeur par défaut.  
  
`[ @active_start_date = ] active_start_date` Date à laquelle le Agent de distribution est planifié pour la première fois, au format AAAAMMJJ. *active_start_date* est de **type int**, avec **0** comme valeur par défaut.  
  
`[ @active_end_date = ] active_end_date` Date à laquelle le Agent de distribution cesse d’être planifié, au format AAAAMMJJ. *active_end_date* est de **type int**, avec **0** comme valeur par défaut.  
  
`[ @distribution_jobid = ] _distribution_jobidOUTPUT` ID de la Agent de distribution pour ce travail. *distribution_jobid* est de **type Binary (16)**, avec NULL comme valeur par défaut, et il s’agit d’un paramètre de sortie.  
  
`[ @encrypted_distributor_password = ] encrypted_distributor_password` La définition de *encrypted_distributor_password* n’est plus prise en charge. Si vous tentez de définir ce paramètre de **bit** sur **1** , une erreur se produit.  
  
`[ @enabled_for_syncmgr = ] 'enabled_for_syncmgr'` Indique si l’abonnement peut être synchronisé via le [!INCLUDE[msCoName](../../includes/msconame-md.md)] Gestionnaire de synchronisation. *enabled_for_syncmgr* est de type **nvarchar (5)**, avec false comme valeur par défaut. Si la **valeur est false**, l’abonnement n’est pas inscrit auprès du gestionnaire de synchronisation. Si la **valeur est true**, l’abonnement est inscrit auprès du gestionnaire de synchronisation et peut être synchronisé sans démarrage [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] .  
  
`[ @ftp_address = ] 'ftp_address'` À des fins de compatibilité descendante uniquement.  
  
`[ @ftp_port = ] ftp_port` À des fins de compatibilité descendante uniquement.  
  
`[ @ftp_login = ] 'ftp_login'` À des fins de compatibilité descendante uniquement.  
  
`[ @ftp_password = ] 'ftp_password'` À des fins de compatibilité descendante uniquement.  
  
`[ @alt_snapshot_folder = ] 'alternate_snapshot_folder'_` Spécifie l’emplacement du dossier de remplacement pour l’instantané. *alternate_snapshot_folder* est de type **nvarchar (255)**, avec NULL comme valeur par défaut.  
  
`[ @working_directory = ] 'working_director'` Nom du répertoire de travail utilisé pour stocker les fichiers de données et de schéma de la publication. *working_directory* est de type **nvarchar (255)**, avec NULL comme valeur par défaut. Le nom doit être indiqué au format UNC.  
  
`[ @use_ftp = ] 'use_ftp'` Spécifie l’utilisation de FTP au lieu du protocole normal pour récupérer des instantanés. *use_ftp* est de type **nvarchar (5)**, avec false comme valeur par défaut.  
  
`[ @publication_type = ] publication_type` Spécifie le type de réplication de la publication. *publication_type* est de **type tinyint** , avec **0** comme valeur par défaut. Si la **valeur est 0**, la publication est un type de transaction. Si la variable est **1**, la publication est de type instantané. Si la condition est **2**, la publication est de type fusion.  
  
`[ @dts_package_name = ] 'dts_package_name'` Spécifie le nom du package DTS. *dts_package_name* est de **type sysname** , avec NULL comme valeur par défaut. Par exemple, pour spécifier un package nommé `DTSPub_Package`, le paramètre est le suivant : `@dts_package_name = N'DTSPub_Package'`.  
  
`[ @dts_package_password = ] 'dts_package_password'` Spécifie le mot de passe du package, le cas échéant. *dts_package_password* est de **type sysname** , avec NULL comme valeur par défaut, ce qui signifie qu’aucun mot de passe n’est présent sur le package.  
  
> [!NOTE]  
>  Vous devez spécifier un mot de passe si *dts_package_name* est spécifié.  
  
`[ @dts_package_location = ] 'dts_package_location'` Spécifie l’emplacement du package. *dts_package_location* est de type **nvarchar (12)**, avec l' **abonné** comme valeur par défaut. L’emplacement du package peut être **Distributor** ou **Subscriber**.  
  
`[ @reserved = ] 'reserved'` [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
`[ @offloadagent = ] 'remote_agent_activation'`
 > [!NOTE]  
>  L'activation d'agent distant est déconseillée et n'est plus prise en charge. Ce paramètre est uniquement pris en charge pour assurer la compatibilité descendante des scripts. Si vous affectez à *remote_agent_activation* une valeur autre que **false** , une erreur est générée.  
  
`[ @offloadserver = ] 'remote_agent_server_name'`
 > [!NOTE]  
>  L'activation d'agent distant est déconseillée et n'est plus prise en charge. Ce paramètre est uniquement pris en charge pour assurer la compatibilité descendante des scripts. Si vous définissez *remote_agent_server_name* sur une valeur non null, une erreur est générée.  
  
`[ @job_name = ] 'job_name'` Nom d’un travail d’agent existant. *job_name* est de **type sysname**, avec NULL comme valeur par défaut. Ce paramètre n'est indiqué que lorsque l'abonnement est synchronisé grâce à un travail existant plutôt qu'un nouveau travail (étant le comportement par défaut). Si vous n’êtes pas membre du rôle serveur fixe **sysadmin** , vous devez spécifier *job_login* et *job_password* lorsque vous spécifiez *job_name*.  
  
`[ @job_login = ] 'job_login'` Nom de connexion du compte Windows sous lequel l’agent s’exécute. *job_login* est de type **nvarchar (257)**, sans valeur par défaut. C'est ce compte Windows qui est destiné à toujours être utilisé pour les connexions des Agents à l'Abonné.  
  
`[ @job_password = ] 'job_password'` Mot de passe du compte Windows sous lequel l’agent s’exécute. *job_password* est de **type sysname**, sans valeur par défaut.  
  
> [!IMPORTANT]  
>  Lorsque c'est possible, demande aux utilisateurs de fournir les informations d'identification au moment de l'exécution. Si vous devez enregistrer les informations d'identification dans un fichier de script, vous devez sécuriser le fichier pour empêcher un accès non autorisé.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_addpullsubscription_agent** est utilisé dans la réplication d’instantané et la réplication transactionnelle.  
  
## <a name="example"></a>Exemple  
 [!code-sql[HowTo#sp_addtranpullsubscriptionagent](../../relational-databases/replication/codesnippet/tsql/sp-addpullsubscription-a_1.sql)]  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** ou du rôle de base de données fixe **db_owner** peuvent exécuter **sp_addpullsubscription_agent**.  
  
## <a name="see-also"></a>Voir aussi  
 [Create a Pull Subscription](../../relational-databases/replication/create-a-pull-subscription.md)   
 [S’abonner aux Publications](../../relational-databases/replication/subscribe-to-publications.md)   
 [sp_addpullsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addpullsubscription-transact-sql.md)   
 [sp_change_subscription_properties &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-change-subscription-properties-transact-sql.md)   
 [sp_droppullsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-droppullsubscription-transact-sql.md)   
 [sp_helppullsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helppullsubscription-transact-sql.md)   
 [sp_helpsubscription_properties &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpsubscription-properties-transact-sql.md)  
  
  
