---
title: sp_addsubscription (Transact-SQL) | Microsoft Docs
description: Ajoute un abonnement à une publication et définit l'état de l'abonné. Cette procédure stockée s’exécute sur le serveur de publication de la base de données de publication.
ms.date: 06/09/2020
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.custom: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_addsubscription
- sp_addsubscription_TSQL
helpviewer_keywords:
- sp_addsubscription
ms.assetid: 61ddf287-1fa0-4c1a-8657-ced50cebf0e0
author: markingmyname
ms.author: maghan
ms.openlocfilehash: a4bb033d4817fcf08a7816f8c6598802e9ac3f8c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211960"
---
# <a name="sp_addsubscription-transact-sql"></a>sp_addsubscription (Transact-SQL)
[!INCLUDE[sql-asdb](../../includes/applies-to-version/sql-asdb.md)]

  Ajoute un abonnement à une publication et définit l'état de l'abonné. Cette procédure stockée est exécutée sur le serveur de publication dans la base de données de publication.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_addsubscription [ @publication = ] 'publication'  
    [ , [ @article = ] 'article']  
    [ , [ @subscriber = ] 'subscriber' ]  
    [ , [ @destination_db = ] 'destination_db' ]  
        [ , [ @sync_type = ] 'sync_type' ]  
    [ , [ @status = ] 'status'  
        [ , [ @subscription_type = ] 'subscription_type' ]  
    [ , [ @update_mode = ] 'update_mode' ]  
    [ , [ @loopback_detection = ] 'loopback_detection' ]  
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
    [ , [ @reserved = ] 'reserved' ]  
    [ , [ @enabled_for_syncmgr= ] 'enabled_for_syncmgr' ]  
    [ , [ @offloadagent= ] remote_agent_activation]  
    [ , [ @offloadserver= ] 'remote_agent_server_name' ]  
    [ , [ @dts_package_name= ] 'dts_package_name' ]  
    [ , [ @dts_package_password= ] 'dts_package_password' ]  
    [ , [ @dts_package_location= ] 'dts_package_location' ]  
    [ , [ @distribution_job_name= ] 'distribution_job_name' ]  
    [ , [ @publisher = ] 'publisher' ]  
    [ , [ @backupdevicetype = ] 'backupdevicetype' ]  
    [ , [ @backupdevicename = ] 'backupdevicename' ]  
    [ , [ @mediapassword = ] 'mediapassword' ]  
    [ , [ @password = ] 'password' ]  
    [ , [ @fileidhint = ] fileidhint ]  
    [ , [ @unload = ] unload ]  
    [ , [ @subscriptionlsn = ] subscriptionlsn ]  
    [ , [ @subscriptionstreams = ] subscriptionstreams ]  
    [ , [ @subscriber_type = ] subscriber_type ]  
    [ , [ @memory_optimized = ] memory_optimized ]  
```  
  
## <a name="arguments"></a>Arguments  
 [ @publication =] '*publication*'  
 Nom de la publication. *publication* est de **type sysname**, sans valeur par défaut.  
  
 [ @article =] '*article*'  
 Nom de l'article auquel la publication est abonnée. *article* est de **type sysname**, avec All comme valeur par défaut. Si la valeur définie est all (tous), l'abonnement s'ajoute à tous les articles de la publication. Seules les valeurs all ou NULL sont prises en charge par les serveurs de publication Oracle.  
  
 [ @subscriber =] «*abonné*»  
 Nom de l'Abonné. *Subscriber* est de **type sysname**, avec NULL comme valeur par défaut.  

> [!NOTE]
> Le nom du serveur peut être spécifié sous la forme `<Hostname>,<PortNumber>` . Vous devrez peut-être spécifier le numéro de port de votre connexion lorsque SQL Server est déployé sur Linux ou Windows avec un port personnalisé, et que le service Browser est désactivé.
  
 [ @destination_db =] '*destination_db*'  
 Nom de la base de données de destination dans laquelle les données répliquées seront placées. *destination_db* est de **type sysname**, avec NULL comme valeur par défaut. Si la valeur est NULL, *destination_db* est défini sur le nom de la base de données de publication. Pour les serveurs de publication Oracle, *destination_db* doit être spécifié. Pour un abonné non-SQL Server, spécifiez la valeur (destination par défaut) pour *destination_db*.  
  
 [ @sync_type =] '*sync_type*'  
 Type de synchronisation d'abonnement. *sync_type* est de type **nvarchar (255)** et peut prendre l’une des valeurs suivantes :  
  
|Valeur|Description|  
|-----------|-----------------|  
|aucun|L'abonnement dispose déjà du schéma et des données initiales destinées aux tables publiées.<br /><br /> Remarque : cette option est dépréciée. Utilisez plutôt la prise en charge de la réplication uniquement.|  
|automatic (valeur par défaut)|Le schéma et les données initiales des tables publiées sont transférés en premier lieu vers l'Abonné.|  
|replication support only|Fournit une génération automatique au niveau de l'Abonné des procédures stockées personnalisées de l'article et des déclencheurs qui prennent en charge les abonnements de mise à jour, le cas échéant. Considère que l'Abonné dispose déjà du schéma et des données initiales pour les tables publiées. Lors de la configuration d'une topologie de réplication transactionnelle d'égal à égal, veillez à ce que les données de tous les nœuds de la topologie soient identiques. Pour plus d'informations, consultez [Peer-to-Peer Transactional Replication](../../relational-databases/replication/transactional/peer-to-peer-transactional-replication.md).<br /><br /> *Non pris en charge pour les abonnements à des publications non-SQL Server.*|  
|initialize with backup|Le schéma et les données initiales destinées aux tables publiées proviennent d'une sauvegarde de la base de données de publication. L'abonné est censé avoir accès à une sauvegarde de la base de données de publication. L’emplacement de la sauvegarde et le type de support pour la sauvegarde sont spécifiés par *backupdevicename* et *BackupDeviceType*. Lors de l'utilisation de cette option, il n'est pas nécessaire de suspendre la topologie de réplication transactionnelle d'égal à égal pendant la configuration.<br /><br /> *Non pris en charge pour les abonnements à des publications non-SQL Server.*|  
|initialize from lsn|Utilisé lorsque vous ajoutez un nœud à une topologie de réplication transactionnelle d'égal à égal. Utilisé avec la propriété @subscriptionlsn pour vérifier que toutes les transactions appropriées sont répliquées sur le nouveau nœud. Considère que l'Abonné dispose déjà du schéma et des données initiales pour les tables publiées. Pour plus d'informations, consultez [Peer-to-Peer Transactional Replication](../../relational-databases/replication/transactional/peer-to-peer-transactional-replication.md).|  
  
> [!NOTE]  
>  Les données et les tables système sont toujours transférées.  
  
 [ @status =] '*État*'  
 État de l'abonnement. *Status* est de **type sysname**, avec NULL comme valeur par défaut. Lorsque ce paramètre n'est pas défini explicitement, la réplication lui donne automatiquement l'une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|active|L'abonnement est initialisé et prêt à accepter des modifications. Cette option est définie lorsque la valeur de *sync_type* est None, Initialize with backup ou Replication support only.|  
|subscribed|L'abonnement doit être initialisé. Cette option est définie lorsque la valeur de *sync_type* est automatique.|  
  
 [ @subscription_type =] '*subscription_type*'  
 Type d’abonnement. *subscription_type* est de type **nvarchar (4)**, avec Push comme valeur par défaut. Peut avoir la valeur push ou pull (émission ou extraction de données). Les Agents de distribution des abonnements par émission de données (push) résident sur le serveur de distribution, tandis que ceux des abonnements par extraction (pull) se trouvent au niveau de l'Abonné. *subscription_type* peut être pull pour créer un abonnement par extraction nommé connu du serveur de publication. Pour plus d’informations, consultez [S’abonner à des publications](../../relational-databases/replication/subscribe-to-publications.md).  
  
> [!NOTE]  
>  Les abonnements anonymes ne doivent pas utiliser cette procédure stockée.  
  
 [ @update_mode =] '*update_mode*'  
 Type de mise à jour. *update_mode* est de type **nvarchar (30)** et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|read only (valeur par défaut)|L'abonnement est en lecture seule. Les modifications effectuées chez l'abonné ne sont pas renvoyées au serveur de publication.|  
|sync tran|Active la prise en charge des abonnements de mise à jour immédiate. Non pris en charge pour les serveurs de publication Oracle.|  
|queued tran|Active l'abonnement pour la mise à jour en attente. Les modifications de données peuvent être effectuées chez l'abonné, stockées dans une file d'attente, puis propagées vers le serveur de publication. Non pris en charge pour les serveurs de publication Oracle.|  
|failover|Active l'abonnement pour la mise à jour immédiate avec mise à jour en attente sous forme de basculement. Les modifications de données peuvent être effectuées chez l'abonné, puis propagées immédiatement vers le serveur de publication. Si le serveur de publication et l'abonné ne sont pas connectés, il est possible de changer de mode de mise à jour afin que les modifications de données effectuées chez l'abonné soient stockées dans une file d'attente jusqu'à ce que l'abonné et le serveur de publication soient reconnectés. Non pris en charge pour les serveurs de publication Oracle.|  
|queued failover|Active l'abonnement en tant qu'abonnement de mise à jour en attente, avec possibilité de passer au mode de mise à jour immédiate. Les modifications de données peuvent être effectuées chez l'abonné et stockées dans une file d'attente, jusqu'à ce qu'une connexion soit établie entre l'abonné et le serveur de publication. Lorsqu'une connexion permanente est établie, il est possible de passer au mode de mise à jour immédiate. Non pris en charge pour les serveurs de publication Oracle.|  
  
 Notez que les valeurs synctran et Queued TRAN ne sont pas autorisées si la publication faisant l’objet d’un abonnement autorise les services DTS.  
  
 [ @loopback_detection =] '*loopback_detection*'  
 Indique si l'Agent de distribution envoie des transactions à un abonné qui en est l'auteur. *loopback_detection* est de type **nvarchar (5)** et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|true|L'Agent de distribution n'envoie pas à l'abonné ses propres transactions. Utilisé avec la réplication transactionnelle bidirectionnelle. Pour plus d’informations, voir [Bidirectional Transactional Replication](../../relational-databases/replication/transactional/bidirectional-transactional-replication.md).|  
|false|L'Agent de distribution renvoie à l'abonné ses propres transactions.|  
|NULL (par défaut)|Prend automatiquement la valeur true pour un Abonné [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et false pour un Abonné non-[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
  
 [ @frequency_type =] *frequency_type*  
 Fréquence de planification de la tâche de distribution. *frequency_type* est de type int et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|1|Ponctuelle|  
|2|À la demande|  
|4|Quotidien|  
|8|Hebdomadaire|  
|16|Mensuelle|  
|32|Mensuelle relative|  
|64 (valeur par défaut)|Démarrage automatique|  
|128|Périodique|  
  
 [ @frequency_interval =] *frequency_interval*  
 Valeur à appliquer à la fréquence définie par *frequency_type*. *frequency_interval* est de **type int**, avec NULL comme valeur par défaut.  
  
 [ @frequency_relative_interval =] *frequency_relative_interval*  
 Date de l'Agent de distribution. Ce paramètre est utilisé lorsque *frequency_type* a la valeur 32 (mensuelle relative). *frequency_relative_interval* est de **type int** et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|1|Premier|  
|2|Seconde|  
|4|Third|  
|8|Quatrième|  
|16|Dernier|  
|NULL (par défaut)||  
  
 [ @frequency_recurrence_factor =] *frequency_recurrence_factor*  
 Facteur de récurrence utilisé par *frequency_type*. *frequency_recurrence_factor* est de **type int**, avec NULL comme valeur par défaut.  
  
 [ @frequency_subday =] *frequency_subday*  
 Indique, en minutes, la fréquence de replanification pendant la période définie. *frequency_subday* est de **type int** et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|1|Une fois|  
|2|Seconde|  
|4|Minute|  
|8|Heure|  
|NULL||  
  
 [ @frequency_subday_interval =] *frequency_subday_interval*  
 Intervalle de *frequency_subday*. *frequency_subday_interval* est de **type int**, avec NULL comme valeur par défaut.  
  
 [ @active_start_time_of_day =] *active_start_time_of_day*  
 Heure à laquelle l’Agent de distribution est planifié pour la première fois, au format HHMMSS. *active_start_time_of_day* est de **type int**, avec NULL comme valeur par défaut.  
  
 [ @active_end_time_of_day =] *active_end_time_of_day*  
 Heure de la journée à laquelle le Agent de distribution cesse d’être planifié, au format HHMMSS. *active_end_time_of_day* est de **type int**, avec NULL comme valeur par défaut.  
  
 [ @active_start_date =] *active_start_date*  
 Date à laquelle l’Agent de distribution est planifié pour la première fois, au format AAAAMMJJ. *active_start_date* est de **type int**, avec NULL comme valeur par défaut.  
  
 [ @active_end_date =] *active_end_date*  
 Date à laquelle l’Agent de distribution cesse d'être planifié, au format AAAAMMJJ. *active_end_date* est de **type int**, avec NULL comme valeur par défaut.  
  
 [ @optional_command_line =] '*optional_command_line*'  
 Invite de commandes facultative à exécuter. *optional_command_line* est de type **nvarchar (4000)**, avec NULL comme valeur par défaut.  
  
 [ @reserved =] '*reserved*'  
 [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
 [ @enabled_for_syncmgr =] '*enabled_for_syncmgr*'  
 Indique si l’abonnement peut être synchronisé via le [!INCLUDE[msCoName](../../includes/msconame-md.md)] Gestionnaire de synchronisation Windows. *enabled_for_syncmgr* est de type **nvarchar (5)**, avec false comme valeur par défaut. Si la valeur est false, l'abonnement n'est pas enregistré par le Gestionnaire de synchronisation Windows. Si la valeur est true, l'abonnement est enregistré par le Gestionnaire de synchronisation Windows et il peut ensuite être synchronisé, sans qu'il soit nécessaire de démarrer [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Non pris en charge pour les serveurs de publication Oracle.  
  
 [ @offloadagent =] '*remote_agent_activation*'  
 Indique si l'Agent peut être activé à distance. *remote_agent_activation* est de **bits** avec 0 comme valeur par défaut.  
  
> [!NOTE]  
>  Ce paramètre est déconseillé et n'est conservé que pour la compatibilité descendante des scripts.  
  
 [ @offloadserver =] '*remote_agent_server_name*'  
 Indique le nom réseau du serveur à utiliser pour l'activation à distance. *remote_agent_server_name* est de **type sysname**, avec NULL comme valeur par défaut.  
  
 [ @dts_package_name =] '*dts_package_name*'  
 Spécifie le nom du package DTS (Data Transformation Services). *dts_package_name* est de **type sysname** , avec NULL comme valeur par défaut. Par exemple, pour spécifier un package de DTSPub_Package, le paramètre est : `@dts_package_name = N'DTSPub_Package'`. Ce paramètre est disponible avec les abonnements envoyés. Pour ajouter des informations de package DTS à un abonnement extrait, utilisez sp_addpullsubscription_agent.  
  
 [ @dts_package_password =] '*dts_package_password*'  
 Spécifie le mot de passe du package, s'il existe. *dts_package_password* est de **type sysname** , avec NULL comme valeur par défaut.  
  
> [!NOTE]  
>  Vous devez spécifier un mot de passe si *dts_package_name* est spécifié.  
  
 [ @dts_package_location =] '*dts_package_location*'  
 Spécifie l'emplacement du package. *dts_package_location* est de type **nvarchar (12)**, avec Distributor comme valeur par défaut. L'emplacement du package peut prendre la valeur distributor ou subscriber.  
  
 [ @distribution_job_name =] '*distribution_job_name*'  
 [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
 [ @publisher =] «*éditeur*»  
 Spécifie un serveur de publication non- [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . *Publisher* est de **type sysname**, avec NULL comme valeur par défaut.  
  
> [!NOTE]  
>  l' *éditeur* ne doit pas être spécifié pour un serveur de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] publication.  
  
 [ @backupdevicetype =] '*BackupDeviceType*'  
 Indique le type d'unité de sauvegarde utilisé lors de l'initialisation d'un Abonné à partir d'une sauvegarde. *BackupDeviceType* est de type **nvarchar (20)** et peut prendre l’une des valeurs suivantes :  
  
|Valeur|Description|  
|-----------|-----------------|  
|logical (valeur par défaut)|L'unité de sauvegarde est une unité logique.|  
|disk|L'unité de sauvegarde est un lecteur de disque.|  
|tape|L'unité de sauvegarde est un lecteur de bande.|  
  
 *BackupDeviceType* est utilisé uniquement lorsque *sync_method* est défini sur initialize_with_backup.  
  
 [ @backupdevicename =] '*backupdevicename*'  
 Indique le nom de l'unité utilisée lors de l'initialisation d'un Abonné à partir d'une sauvegarde. *backupdevicename* est de type **nvarchar (1000)**, avec NULL comme valeur par défaut.  
  
 [ @mediapassword =] '*MEDIAPASSWORD*'  
 Indique un mot de passe pour le support spécifié, si un mot de passe a été défini lors du formatage du support. *MEDIAPASSWORD* est de **type sysname**, avec NULL comme valeur par défaut.  
  
> [!NOTE]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]  
  
 [ @password =] '*mot_de_passe*'  
 Indique un mot de passe pour la sauvegarde, si un mot de passe a été défini lors de la création de celle-ci. *Password* est de **type sysname**, avec NULL comme valeur par défaut.  
  
 [ @fileidhint =] *fileidhint –*  
 Identifie une valeur ordinale du jeu de sauvegarde à restaurer. *fileidhint –* est de **type int**, avec NULL comme valeur par défaut.  
  
 [ @unload =] *décharger*  
 Indique si une unité de sauvegarde sur bande doit être déchargée une fois l'initialisation de la sauvegarde terminée. *Unload* est de **bits**, avec 1 comme valeur par défaut. 1 indique que la bande doit être déchargée. le *déchargement* est utilisé uniquement lorsque *BackupDeviceType* est une bande.  
  
 [ @subscriptionlsn =] *SubscriptionLSN*  
 Spécifie le numéro séquentiel dans le journal auquel un abonnement doit commencer à remettre des modifications à un nœud dans une topologie de réplication transactionnelle d'égal à égal. Utilisé avec la @sync_type valeur Initialize from LSN pour s’assurer que toutes les transactions pertinentes sont répliquées vers un nouveau nœud. Pour plus d'informations, consultez [Peer-to-Peer Transactional Replication](../../relational-databases/replication/transactional/peer-to-peer-transactional-replication.md).  
  
 [ @subscriptionstreams =] *SubscriptionStreams*  
 Nombre de connexions autorisées par l'Agent de distribution afin d'appliquer des lots de modifications en parallèle à un Abonné, tout en conservant bon nombre des caractéristiques transactionnelles présentes lors de l'utilisation d'un thread unique. *SubscriptionStreams* est de **type tinyint**, avec NULL comme valeur par défaut. Une plage de valeurs allant de 1 à 64 est prise en charge. Ce paramètre n’est pas pris en charge pour les abonnés non-, les serveurs de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] publication Oracle ou les abonnements d’égal à égal. Chaque fois que des flux d'abonnements sont utilisés, des lignes supplémentaires sont ajoutées au tableau msreplication_subscriptions (1 par flux) avec agent_id défini à NULL.  
  
> [!NOTE]  
>  Les flux d'abonnements ne fonctionnent pas pour les articles configurés pour fournir [!INCLUDE[tsql](../../includes/tsql-md.md)]. Pour utiliser les flux d'abonnements, configurez les articles afin qu'ils fournissent des appels de procédures stockées à la place.  
  
 [ @subscriber_type =] *subscriber_type*  
 Type d'abonné. *subscriber_type* est de **type tinyint** et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|0 (valeur par défaut)|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Côté|  
|1|Serveur de la source de données ODBC.|  
|2|Base de données [!INCLUDE[msCoName](../../includes/msconame-md.md)] Jet|  
|3|Fournisseur OLE DB|  
  
 [ @memory_optimized =] *memory_optimized*  
 Indique que l’abonnement prend en charge les tables optimisées en mémoire. *memory_optimized* est de **bits**, où 1 est égal à true (l’abonnement prend en charge les tables optimisées en mémoire).  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="remarks"></a>Notes  
 sp_addsubscription est utilisé lors des réplications d'instantané et transactionnelle.  
  
 Lorsque sp_addsubscription est exécuté par un membre du rôle serveur fixe sysadmin pour créer un abonnement envoyé, la tâche de l'Agent de distribution est implicitement créée et s'exécute sous le compte du service Agent SQL Server. Nous vous recommandons d’exécuter [sp_addpushsubscription_agent](../../relational-databases/system-stored-procedures/sp-addpushsubscription-agent-transact-sql.md) et de spécifier les informations d’identification d’un autre compte Windows spécifique à l’agent pour @job_login et @job_password . Pour plus d’informations, voir [Replication Agent Security Model](../../relational-databases/replication/security/replication-agent-security-model.md).  
  
 sp_addsubscription empêche les abonnés ODBC et OLE DB d'accéder aux publications qui :  
  
-   Ont été créés avec l' *sync_method* natif dans l’appel à [sp_addpublication](../../relational-databases/system-stored-procedures/sp-addpublication-transact-sql.md).  
  
-   Contiennent des articles qui ont été ajoutés à la publication avec la procédure stockée [sp_addarticle](../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md) qui avait une valeur de paramètre *pre_creation_cmd* de 3 (tronquer).  
  
-   Tentative de définition de *update_mode* pour la synchronisation TRAN.  
  
-   ont un article configuré pour utiliser des instructions paramétrables.  
  
 En outre, si l’option *allow_queued_tran* de la publication est définie sur true (ce qui permet la mise en file d’attente des modifications sur l’abonné jusqu’à ce qu’elles puissent être appliquées sur le serveur de publication), la colonne timestamp d’un article est remplacée par l' **horodateur** et les modifications sur cette colonne sont envoyées à l’abonné. L'abonné génère et met à jour la valeur de la colonne timestamp. Pour un abonné ODBC ou OLE DB, sp_addsubscription échoue si une tentative est effectuée pour s’abonner à une publication dont le *allow_queued_tran* a la valeur true et les articles contenant des colonnes timestamp.  
  
 Si un abonnement n’utilise pas de package DTS, il ne peut pas s’abonner à une publication qui est définie sur *allow_transformable_subscriptions*. Si la table issue de la publication doit être répliquée vers un abonnement DTS et un abonnement non-DTS, deux publications indépendantes doivent être créées : une pour chaque type d'abonnement.  
  
 Lors de la sélection des options **sync_type** , *replication support only*, *initialize with backup* ou *initialize from lsn*, l'Agent de lecture du journal doit s'exécuter après l'exécution de **sp_addsubscription**, afin que les scripts d'installation soient écrits dans la base de données de distribution. L'Agent de lecture du journal doit s'exécuter sous un compte membre du rôle serveur fixe **sysadmin** . Lorsque l'option **sync_type** a la valeur *Automatic*, aucune action particulière de l'Agent de lecture du journal n'est requise.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe sysadmin ou du rôle de base de données fixe db_owner peuvent exécuter sp_addsubscription. Pour les abonnements par extraction de données (pull), les utilisateurs ayant une connexion à la liste d'accès aux publications peuvent exécuter sp_addsubscription.  
  
## <a name="example"></a>Exemple  
 [!code-sql[HowTo#sp_addtranpushsubscription_agent](../../relational-databases/replication/codesnippet/tsql/sp-addsubscription-trans_1.sql)]  
  
## <a name="see-also"></a>Voir aussi  
 [Create a Push Subscription](../../relational-databases/replication/create-a-push-subscription.md)   
 [Créer un abonnement pour un abonné non-SQL Server](../../relational-databases/replication/create-a-subscription-for-a-non-sql-server-subscriber.md)   
 [S’abonner aux Publications](../../relational-databases/replication/subscribe-to-publications.md)   
 [sp_addpushsubscription_agent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addpushsubscription-agent-transact-sql.md)   
 [sp_changesubstatus &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changesubstatus-transact-sql.md)   
 [sp_dropsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropsubscription-transact-sql.md)   
 [sp_helpsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpsubscription-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
