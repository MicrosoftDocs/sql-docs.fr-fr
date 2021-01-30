---
description: sp_addmergepullsubscription_agent (Transact-SQL)
title: sp_addmergepullsubscription_agent (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_addmergepullsubscription_agent
- sp_addmergepullsubscription_agent_TSQL
helpviewer_keywords:
- sp_addmergepullsubscription_agent
ms.assetid: a2f4b086-078d-49b5-8971-8a1e3f6a6feb
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 4ecc9b2fded678671bc608235fa3080f2ed31573
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209423"
---
# <a name="sp_addmergepullsubscription_agent-transact-sql"></a>sp_addmergepullsubscription_agent (Transact-SQL)

[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Ajoute un nouveau travail d'Agent permettant de planifier la synchronisation d'un abonnement par extraction de données (pull) à une publication de fusion. Cette procédure stockée est exécutée sur la base de données d'abonnement de l'Abonné.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_addmergepullsubscription_agent [ [ @name = ] 'name' ]   
        , [ @publisher = ] 'publisher'   
        , [ @publisher_db = ] 'publisher_db'  
        , [ @publication =] 'publication'   
    [ , [ @publisher_security_mod e= ] publisher_security_mode ]   
    [ , [ @publisher_login = ] 'publisher_login' ]   
    [ , [ @publisher_password = ] 'publisher_password' ]   
    [ , [ @publisher_encrypted_password = ] publisher_encrypted_password ]   
    [ , [ @subscriber = ] 'subscriber' ]   
    [ , [ @subscriber_db = ] 'subscriber_db' ]   
    [ , [ @subscriber_security_mode = ] subscriber_security_mode ]   
    [ , [ @subscriber_login = ] 'subscriber_login' ]   
    [ , [ @subscriber_password= ] 'subscriber_password' ]   
    [ , [ @distributor = ] 'distributor' ]   
    [ , [ @distributor_security_mode = ] distributor_security_mode ]   
    [ , [ @distributor_login = ] 'distributor_login' ]   
    [ , [ @distributor_password = ] 'distributor_password' ]   
    [ , [ @encrypted_password = ] encrypted_password ]   
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
    [ , [ @optional_command_line = ] 'optional_command_line' ]   
    [ , [ @merge_jobid = ] merge_jobid ]   
    [ , [ @enabled_for_syncmgr = ] 'enabled_for_syncmgr' ]   
    [ , [ @ftp_address = ] 'ftp_address' ]   
    [ , [ @ftp_port = ] ftp_port ]   
    [ , [ @ftp_login = ] 'ftp_login' ]   
    [ , [ @ftp_password = ] 'ftp_password' ]    
    [ , [ @alt_snapshot_folder = ] 'alternate_snapshot_folder' ]   
    [ , [ @working_directory = ] 'working_directory' ]   
    [ , [ @use_ftp = ] 'use_ftp' ]   
    [ , [ @reserved = ] 'reserved' ]   
    [ , [ @use_interactive_resolver = ] 'use_interactive_resolver' ]   
    [ , [ @offloadagent = ] 'remote_agent_activation' ]   
    [ , [ @offloadserver = ] 'remote_agent_server_name']   
    [ , [ @job_name = ] 'job_name' ]   
    [ , [ @dynamic_snapshot_location = ] 'dynamic_snapshot_location' ]  
    [ , [ @use_web_sync = ] use_web_sync ]  
        [ , [ @internet_url = ] 'internet_url' ]  
    [ , [ @internet_login = ] 'internet_login' ]  
        [ , [ @internet_password = ] 'internet_password' ]  
    [ , [ @internet_security_mode = ] internet_security_mode ]  
        [ , [ @internet_timeout = ] internet_timeout ]  
    [ , [ @hostname = ] 'hostname' ]  
        [ , [ @job_login = ] 'job_login' ]   
    [ , [ @job_password = ] 'job_password' ]   
```  
  
## <a name="arguments"></a>Arguments  
`[ @name = ] 'name'` Nom de l’agent. *Name* est de **type sysname**, avec NULL comme valeur par défaut.  
  
`[ @publisher = ] 'publisher'` Est le nom du serveur de publication. *Publisher* est de **type sysname**, sans valeur par défaut.  
  
`[ @publisher_db = ] 'publisher_db'` Nom de la base de données du serveur de publication. *publisher_db* est de **type sysname**, sans valeur par défaut.  
  
`[ @publication = ] 'publication'` Nom de la publication. *publication* est de **type sysname**, sans valeur par défaut.  
  
`[ @publisher_security_mode = ] publisher_security_mode` Mode de sécurité à utiliser lors de la connexion à un serveur de publication lors de la synchronisation. *publisher_security_mode* est de **type int**, avec 1 comme valeur par défaut. Si la **valeur est 0**, spécifie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l’authentification. Si **1**, spécifie l’authentification Windows.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteWinAuthentication](../../includes/ssnotewinauthentication-md.md)]  
  
`[ @publisher_login = ] 'publisher_login'` Nom de connexion à utiliser lors de la connexion à un serveur de publication lors de la synchronisation. *publisher_login* est de **type sysname**, avec NULL comme valeur par défaut.  
  
`[ @publisher_password = ] 'publisher_password'` Mot de passe utilisé lors de la connexion au serveur de publication. *publisher_password* est de **type sysname**, avec NULL comme valeur par défaut.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteStrongPass](../../includes/ssnotestrongpass-md.md)] Si possible, invitez les utilisateurs à entrer leurs informations d'identification au moment de l'exécution. Si vous devez enregistrer les informations d'identification dans un fichier de script, vous devez sécuriser le fichier pour empêcher un accès non autorisé.  
  
`[ @publisher_encrypted_password = ]publisher_encrypted_password` La définition de *publisher_encrypted_password* n’est plus prise en charge. Si vous tentez de définir ce paramètre de **bit** sur **1** , une erreur se produit.  
  
`[ @subscriber = ] 'subscriber'` Nom de l’abonné. *Subscriber* est de **type sysname**, avec NULL comme valeur par défaut.  
  
`[ @subscriber_db = ] 'subscriber_db'` Nom de la base de données d’abonnement. *subscriber_db* est de **type sysname**, avec NULL comme valeur par défaut.  
  
`[ @subscriber_security_mode = ] subscriber_security_mode` Mode de sécurité à utiliser lors de la connexion à un abonné au cours d’une synchronisation. *subscriber_security_mode* est de **type int**, avec 1 comme valeur par défaut. Si la **valeur est 0**, spécifie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l’authentification. Si **1**, spécifie l’authentification Windows.  
  
> [!NOTE]  
>  Ce paramètre est déconseillé et n'est maintenu que dans un but de compatibilité ascendante avec les scripts déjà établis. L'Agent de fusion se connecte toujours à l'Abonné local à l'aide de l'authentification Windows. Si une valeur est spécifiée pour ce paramètre, un message d'avertissement est retourné, mais la valeur est ignorée.  
  
`[ @subscriber_login = ] 'subscriber_login'` Connexion de l’abonné à utiliser lors de la connexion à un abonné au cours d’une synchronisation. *subscriber_login* est requis si *subscriber_security_mode* a la valeur **0**. *subscriber_login* est de **type sysname**, avec NULL comme valeur par défaut.  
  
> [!NOTE]  
>  Ce paramètre est déconseillé et n'est maintenu que dans un but de compatibilité ascendante avec les scripts déjà établis. Si une valeur est spécifiée pour ce paramètre, un message d'avertissement est retourné, mais la valeur est ignorée.  
  
`[ @subscriber_password = ] 'subscriber_password'` Mot de passe de l’abonné pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l’authentification. *subscriber_password* est requis si *subscriber_security_mode* a la valeur **0**. *subscriber_password* est de **type sysname**, avec NULL comme valeur par défaut.  
  
> [!NOTE]  
>  Ce paramètre est déconseillé et n'est maintenu que dans un but de compatibilité ascendante avec les scripts déjà établis. Si une valeur est spécifiée pour ce paramètre, un message d'avertissement est retourné, mais la valeur est ignorée.  
  
`[ @distributor = ] 'distributor'` Nom du serveur de distribution. *Distributor* est de **type sysname**, avec *Publisher* comme valeur par défaut. autrement dit, le serveur de publication est également le serveur de distribution.  
  
`[ @distributor_security_mode = ] distributor_security_mode` Mode de sécurité à utiliser lors de la connexion à un serveur de distribution lors de la synchronisation. *distributor_security_mode* est de **type int**, avec 0 comme valeur par défaut. **0** spécifie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l’authentification. **1** spécifie l’authentification Windows.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteWinAuthentication](../../includes/ssnotewinauthentication-md.md)]  
  
`[ @distributor_login = ] 'distributor_login'` Connexion du serveur de distribution à utiliser lors de la connexion à un serveur de distribution lors de la synchronisation. *distributor_login* est requis si *distributor_security_mode* a la valeur **0**. *distributor_login* est de **type sysname**, avec NULL comme valeur par défaut.  
  
`[ @distributor_password = ] 'distributor_password'` Est le mot de passe du serveur de distribution. *distributor_password* est requis si *distributor_security_mode* a la valeur **0**. *distributor_password* est de **type sysname**, avec NULL comme valeur par défaut.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteStrongPass](../../includes/ssnotestrongpass-md.md)] Si possible, invitez les utilisateurs à entrer leurs informations d'identification au moment de l'exécution. Si vous devez enregistrer les informations d'identification dans un fichier de script, vous devez sécuriser le fichier pour empêcher un accès non autorisé.  
  
`[ @encrypted_password = ] encrypted_password` La définition de *encrypted_password* n’est plus prise en charge. Si vous tentez de définir ce paramètre de **bit** sur **1** , une erreur se produit.  
  
`[ @frequency_type = ] frequency_type` Fréquence de planification de l’Agent de fusion. *frequency_type* est de **type int** et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**1**|Ponctuelle|  
|**2**|À la demande|  
|**4**|Quotidien|  
|**8**|Hebdomadaire|  
|**16**|Mensuelle|  
|**32**|Mensuelle relative|  
|**64**|Démarrage automatique|  
|**128**|Périodique|  
|NULL (par défaut)||  
  
> [!NOTE]  
>  Si vous spécifiez la valeur **64** , le agent de fusion s’exécute en mode continu. Cela correspond à la définition du paramètre **-Continuous** pour l’agent. Pour plus d’informations, voir [Replication Merge Agent](../../relational-databases/replication/agents/replication-merge-agent.md).  
  
`[ @frequency_interval = ] frequency_interval` Jour ou jours d’exécution de l’Agent de fusion. *frequency_interval* est de **type int** et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**1**|Dimanche|  
|**2**|Lundi|  
|**3**|Mardi|  
|**4**|Mercredi|  
|**5**|Jeudi|  
|**6**|Vendredi|  
|**7**|Samedi|  
|**8**|Jour|  
|**9**|Jours de la semaine|  
|**10**|Jours de week-end|  
|NULL (par défaut)||  
  
`[ @frequency_relative_interval = ] frequency_relative_interval` Date de la Agent de fusion. Ce paramètre est utilisé lorsque *frequency_type* a la valeur **32** (mensuelle relative). *frequency_relative_interval* est de **type int** et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**1**|Premier|  
|**2**|Seconde|  
|**4**|Third|  
|**8**|Quatrième|  
|**16**|Dernier|  
|NULL (par défaut)||  
  
`[ @frequency_recurrence_factor = ] frequency_recurrence_factor` Facteur de récurrence utilisé par *frequency_type*. *frequency_recurrence_factor* est de **type int**, avec NULL comme valeur par défaut.  
  
`[ @frequency_subday = ] frequency_subday` Fréquence de replanification au cours de la période définie. *frequency_subday* est de **type int** et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**1**|Une fois|  
|**2**|Seconde|  
|**4**|Minute|  
|**8**|Heure|  
|NULL (par défaut)||  
  
`[ @frequency_subday_interval = ] frequency_subday_interval` Intervalle de *frequency_subday*. *frequency_subday_interval* est de **type int**, avec NULL comme valeur par défaut.  
  
`[ @active_start_time_of_day = ] active_start_time_of_day` Heure de la journée à laquelle le Agent de fusion est planifié pour la première fois, au format HHMMSS. *active_start_time_of_day* est de **type int**, avec NULL comme valeur par défaut.  
  
`[ @active_end_time_of_day = ] active_end_time_of_day` Heure de la journée à laquelle le Agent de fusion cesse d’être planifié, au format HHMMSS. *active_end_time_of_day* est de **type int**, avec NULL comme valeur par défaut.  
  
`[ @active_start_date = ] active_start_date` Date à laquelle le Agent de fusion est planifié pour la première fois, au format AAAAMMJJ. *active_start_date* est de **type int**, avec NULL comme valeur par défaut.  
  
`[ @active_end_date = ] active_end_date` Date à laquelle le Agent de fusion cesse d’être planifié, au format AAAAMMJJ. *active_end_date* est de **type int**, avec NULL comme valeur par défaut.  
  
`[ @optional_command_line = ] 'optional_command_line'` Est une invite de commandes facultative qui est fournie au Agent de fusion. *optional_command_line* est de type **nvarchar (255)**, avec «» comme valeur par défaut. Permet de fournir des paramètres supplémentaires à l'Agent de fusion, comme dans l'exemple suivant où le délai d'expiration par défaut de la requête est augmenté jusqu'à `600` secondes :  
  
```  
@optional_command_line = N'-QueryTimeOut 600'  
```  
  
`[ @merge_jobid = ] merge_jobid` Paramètre de sortie de l’ID de travail. *merge_jobid* est de **type Binary (16)**, avec NULL comme valeur par défaut.  
  
`[ @enabled_for_syncmgr = ] 'enabled_for_syncmgr'` Spécifie si l’abonnement peut être synchronisé via le gestionnaire de synchronisation Windows. *enabled_for_syncmgr* est de type **nvarchar (5)**, avec false comme valeur par défaut. Si la **valeur est false**, l’abonnement n’est pas inscrit auprès du gestionnaire de synchronisation. Si la **valeur est true**, l’abonnement est inscrit auprès du gestionnaire de synchronisation et peut être synchronisé sans démarrage [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] .  
  
`[ @ftp_address = ] 'ftp_address'` À des fins de compatibilité descendante uniquement.  
  
`[ @ftp_port = ] ftp_port` À des fins de compatibilité descendante uniquement.  
  
`[ @ftp_login = ] 'ftp_login'` À des fins de compatibilité descendante uniquement.  
  
`[ @ftp_password = ] 'ftp_password'` À des fins de compatibilité descendante uniquement.  
  
`[ @alt_snapshot_folder = ] 'alternate_snapshot_folder'` Spécifie l’emplacement à partir duquel récupérer les fichiers d’instantanés. *alternate_snapshot_folder* est de type **nvarchar (255)**, avec NULL comme valeur par défaut. Si la valeur est NULL, les fichiers d'instantané sont extraits à partir de l'emplacement par défaut spécifié par le serveur de publication.  
  
`[ @working_directory = ] 'working_directory'` Nom du répertoire de travail utilisé pour stocker temporairement les fichiers de données et de schéma de la publication lorsque le protocole FTP est utilisé pour transférer des fichiers d’instantanés. *working_directory* est de type **nvarchar (255)**, avec NULL comme valeur par défaut.  
  
`[ @use_ftp = ] 'use_ftp'` Spécifie l’utilisation de FTP au lieu du protocole standard pour récupérer des instantanés. *use_ftp* est de type **nvarchar (5)**, avec false comme valeur par défaut.  
  
`[ @reserved = ] 'reserved'` [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
`[ @use_interactive_resolver = ] 'use_interactive_resolver' ]` Utilise le programme de résolution interactif pour résoudre les conflits pour tous les articles qui autorisent la résolution interactive. *use_interactive_resolver* est de type **nvarchar (5)**, avec false comme valeur par défaut.  
  
`[ @offloadagent = ] 'remote_agent_activation'`
 > [!NOTE]  
>  L'activation d'agent distant est déconseillée et n'est plus prise en charge. Ce paramètre est uniquement pris en charge pour assurer la compatibilité descendante des scripts. Si vous affectez à *remote_agent_activation* une valeur autre que **false** , une erreur est générée.  
  
`[ @offloadserver = ] 'remote_agent_server_name'`
 > [!NOTE]  
>  L'activation d'agent distant est déconseillée et n'est plus prise en charge. Ce paramètre est uniquement pris en charge pour assurer la compatibilité descendante des scripts. Si vous définissez *remote_agent_server_name* sur une valeur non null, une erreur est générée.  
  
`[ @job_name = ] 'job_name' ]` Nom d’un travail d’agent existant. *job_name* est de **type sysname**, avec NULL comme valeur par défaut. Ce paramètre n'est indiqué que lorsque l'abonnement est synchronisé grâce à un travail existant plutôt qu'un nouveau travail (étant le comportement par défaut). Si vous n’êtes pas membre du rôle serveur fixe **sysadmin** , vous devez spécifier *job_login* et *job_password* lorsque vous spécifiez *job_name*.  
  
`[ @dynamic_snapshot_location = ] 'dynamic_snapshot_location' ]` Chemin d’accès au dossier dans lequel les fichiers d’instantanés seront lus si un instantané de données filtrées doit être utilisé. *dynamic_snapshot_location* est de type **nvarchar (260)**, avec NULL comme valeur par défaut. Pour plus d'informations, voir [Parameterized Row Filters](../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md).  
  
`[ @use_web_sync = ] use_web_sync` Indique que la synchronisation Web est activée. *use_web_sync* est de **bit**, avec 0 comme valeur par défaut. **1** indique que l’abonnement par extraction peut être synchronisé via Internet à l’aide du protocole http.  
  
`[ @internet_url = ] 'internet_url'` Est l’emplacement de l’écouteur de réplication (REPLISAPI.DLL) pour la synchronisation Web. *internet_url* est de type **nvarchar (260)**, avec NULL comme valeur par défaut. *internet_url* est une URL complète, au format `http://server.domain.com/directory/replisapi.dll` . Si le serveur est configuré de manière à être à l'écoute sur un port autre que le port 80, le numéro de port doit également être fourni sous la forme `http://server.domain.com:portnumber/directory/replisapi.dll`, où `portnumber` représente le port.  
  
`[ @internet_login = ] 'internet_login'` Nom de connexion utilisé par le Agent de fusion lors de la connexion au serveur Web qui héberge la synchronisation Web à l’aide de l’authentification HTTP de base. *internet_login* est de **type sysname**, avec NULL comme valeur par défaut.  
  
`[ @internet_password = ] 'internet_password'` Mot de passe utilisé par le Agent de fusion lors de la connexion au serveur Web qui héberge la synchronisation Web à l’aide de l’authentification HTTP de base. *internet_password* est de type **nvarchar (524)**, avec NULL comme valeur par défaut.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteStrongPass](../../includes/ssnotestrongpass-md.md)]  
  
`[ @internet_security_mode = ] internet_security_mode` Méthode d’authentification utilisée par le Agent de fusion lors de la connexion au serveur Web pendant la synchronisation Web à l’aide du protocole HTTPs. *internet_security_mode* est de **type int** et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**0**|L'authentification de base est utilisée.|  
|**1** (par défaut)|L'authentification intégrée de Windows est utilisée.|  
  
> [!NOTE]  
>  Il est recommandé d'utiliser l'authentification de base pour la synchronisation Web. Pour utiliser la synchronisation Web, vous devez établir une connexion TLS au serveur Web. Pour plus d’informations, consultez [Configurer la synchronisation Web](../../relational-databases/replication/configure-web-synchronization.md).  
  
`[ @internet_timeout = ] internet_timeout` Durée, en secondes, avant l’expiration d’une demande de synchronisation Web. *internet_timeout* est de **type int**, avec une valeur par défaut de **300** secondes.  
  
`[ @hostname = ] 'hostname'` Remplace la valeur de HOST_NAME () lorsque cette fonction est utilisée dans la clause WHERE d’un filtre paramétré. *hostname* est de **type sysname**, avec NULL comme valeur par défaut.  
  
`[ @job_login = ] 'job_login'` Nom de connexion du compte Windows sous lequel l’agent s’exécute. *job_login* est de type **nvarchar (257)**, sans valeur par défaut. Ce compte Windows est toujours utilisé pour les connexions d'Agent à l'Abonné et pour les connexions au serveur de distribution et au serveur de publication lors de l'utilisation de l'authentification intégrée de Windows.  
  
`[ @job_password = ] 'job_password'` Mot de passe du compte Windows sous lequel l’agent s’exécute. *job_password* est de **type sysname**, sans valeur par défaut.  
  
> [!IMPORTANT]  
>  Ne stockez pas les informations d'authentification dans des fichiers de script. Pour une sécurité optimale, les noms de connexion et les mots de passe doivent être fournis au moment de l'exécution.  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_addmergepullsubscription_agent** est utilisé dans la réplication de fusion et utilise des fonctionnalités similaires à [sp_addpullsubscription_agent](../../relational-databases/system-stored-procedures/sp-addpullsubscription-agent-transact-sql.md).  
  
 Pour obtenir un exemple de la façon de spécifier correctement les paramètres de sécurité lors de l’exécution de **sp_addmergepullsubscription_agent**, consultez [créer un abonnement par extraction](../../relational-databases/replication/create-a-pull-subscription.md).  
  
## <a name="example"></a>Exemple  
 [!code-sql[HowTo#sp_addmergepullsubscriptionagent](../../relational-databases/replication/codesnippet/tsql/sp-addmergepullsubscript_1_1.sql)]  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** ou du rôle de base de données fixe **db_owner** peuvent exécuter **sp_addmergepullsubscription_agent**.  
  
## <a name="see-also"></a>Voir aussi  
 [Create a Pull Subscription](../../relational-databases/replication/create-a-pull-subscription.md)   
 [S’abonner aux Publications](../../relational-databases/replication/subscribe-to-publications.md)   
 [sp_addmergepullsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergepullsubscription-transact-sql.md)   
 [sp_changemergepullsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changemergepullsubscription-transact-sql.md)   
 [sp_dropmergepullsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropmergepullsubscription-transact-sql.md)   
 [sp_helpmergepullsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpmergepullsubscription-transact-sql.md)   
 [sp_helpsubscription_properties &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpsubscription-properties-transact-sql.md)  
  
  
