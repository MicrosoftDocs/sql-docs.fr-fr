---
description: sp_addpublication (Transact-SQL)
title: sp_addpublication (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- sp_addpublication_TSQL
- sp_addpublication
helpviewer_keywords:
- sp_addpublication
ms.assetid: c7167ed1-2b7e-4824-b82b-65f4667c4407
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 9aa65ff9b5b9c14441ac555d40788e92082f01c5
ms.sourcegitcommit: 7791bd2ba339edc5cd2078a6537c8f6bfe72a19b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/19/2021
ms.locfileid: "98564452"
---
# <a name="sp_addpublication-transact-sql"></a>sp_addpublication (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Crée une publication d'instantané ou transactionnelle. Cette procédure stockée est exécutée sur le serveur de publication dans la base de données de publication.  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_addpublication [ @publication = ] 'publication'  
    [ , [ @taskid = ] tasked ]  
    [ , [ @restricted = ] 'restricted' ]  
    [ , [ @sync_method = ] 'sync_method' ]  
    [ , [ @repl_freq = ] 'repl_freq' ]  
    [ , [ @description = ] 'description' ]  
    [ , [ @status = ] 'status' ]  
    [ , [ @independent_agent = ] 'independent_agent' ]  
    [ , [ @immediate_sync = ] 'immediate_sync' ]  
    [ , [ @enabled_for_internet = ] 'enabled_for_internet' ]  
    [ , [ @allow_push = ] 'allow_push'  
    [ , [ @allow_pull = ] 'allow_pull' ]  
    [ , [ @allow_anonymous = ] 'allow_anonymous' ]  
    [ , [ @allow_sync_tran = ] 'allow_sync_tran' ]  
    [ , [ @autogen_sync_procs = ] 'autogen_sync_procs' ]  
    [ , [ @retention = ] retention ]  
    [ , [ @allow_queued_tran= ] 'allow_queued_updating' ]  
    [ , [ @snapshot_in_defaultfolder= ] 'snapshot_in_default_folder' ]  
    [ , [ @alt_snapshot_folder= ] 'alternate_snapshot_folder' ]  
    [ , [ @pre_snapshot_script= ] 'pre_snapshot_script' ]  
    [ , [ @post_snapshot_script= ] 'post_snapshot_script' ]  
    [ , [ @compress_snapshot= ] 'compress_snapshot' ]  
    [ , [ @ftp_address = ] 'ftp_address' ]  
    [ , [ @ftp_port= ] ftp_port ]  
    [ , [ @ftp_subdirectory = ] 'ftp_subdirectory' ]  
    [ , [ @ftp_login = ] 'ftp_login' ]  
    [ , [ @ftp_password = ] 'ftp_password' ]  
    [ , [ @allow_dts = ] 'allow_dts' ]  
    [ , [ @allow_subscription_copy = ] 'allow_subscription_copy' ]  
    [ , [ @conflict_policy = ] 'conflict_policy' ]  
    [ , [ @centralized_conflicts = ] 'centralized_conflicts' ]   
    [ , [ @conflict_retention = ] conflict_retention ]  
    [ , [ @queue_type = ] 'queue_type' ]  
    [ , [ @add_to_active_directory = ] 'add_to_active_directory' ]  
    [ , [ @logreader_job_name = ] 'logreader_agent_name' ]  
    [ , [ @qreader_job_name = ] 'queue_reader_agent_name' ]  
    [ , [ @publisher = ] 'publisher' ]   
    [ , [ @allow_initialize_from_backup = ] 'allow_initialize_from_backup' ]  
    [ , [ @replicate_ddl = ] replicate_ddl ]  
    [ , [ @enabled_for_p2p = ] 'enabled_for_p2p' ]  
    [ , [ @publish_local_changes_only = ] 'publish_local_changes_only' ]  
    [ , [ @enabled_for_het_sub = ] 'enabled_for_het_sub' ]  
    [ , [ @p2p_conflictdetection = ] 'p2p_conflictdetection' ]  
    [ , [ @p2p_originator_id = ] p2p_originator_id  
    [ , [ @p2p_continue_onconflict = ] 'p2p_continue_onconflict'  
    [ , [ @allow_partition_switch = ] 'allow_partition_switch'  
    [ , [ @replicate_partition_switch = ]'replicate_partition_switch'  
```  
  
## <a name="arguments"></a>Arguments  
`[ \@publication = ] 'publication'` Nom de la publication à créer. *publication* est de **type sysname**, sans valeur par défaut. Le nom doit être unique au sein de la base de données.  
  
`[ \@taskid = ] taskid` Pris en charge pour la compatibilité descendante uniquement ; Utilisez [sp_addpublication_snapshot &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addpublication-snapshot-transact-sql.md).  
  
`[ \@restricted = ] 'restricted'` Pris en charge pour la compatibilité descendante uniquement ; Utilisez *default_access*.  
  
`[ \@sync_method = ] _'sync_method'` Mode de synchronisation. *sync_method* est de type **nvarchar (13)** et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**native**|Produit une copie par bloc en mode natif de toutes les tables. *Non pris en charge pour les serveurs de publication Oracle*.|  
|**character**|Produit une copie par bloc en mode caractère de toutes les tables. _Pour un serveur de publication Oracle,_ le **caractère** _est valide uniquement pour la réplication d’instantané_.|  
|**concurrence**|Produit une copie en bloc en mode natif de toutes les tables, mais ne verrouille pas les tables au cours de l'instantané. Pris en charge uniquement pour les publications transactionnelles. *Non pris en charge pour les serveurs de publication Oracle*.|  
|**concurrent_c**|Produit une copie en bloc en mode caractère de toutes les tables, mais ne verrouille pas les tables au cours de l'instantané. Pris en charge uniquement pour les publications transactionnelles.|  
|**instantané de base de données**|Produit une copie en bloc en mode natif de toutes les tables à partir d'un instantané de base de données. Les instantanés de base de données ne sont pas disponibles dans toutes les éditions de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Pour obtenir la liste des fonctionnalités prises en charge par les éditions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consultez [Fonctionnalités prise en charge par les éditions de SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md).|  
|**database snapshot character**|Produit une copie en bloc en mode caractère de toutes les tables à partir d'un instantané de base de données. Les instantanés de base de données ne sont pas disponibles dans toutes les éditions de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Pour obtenir la liste des fonctionnalités prises en charge par les éditions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consultez [Fonctionnalités prise en charge par les éditions de SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md).|  
|NULL (par défaut)|La valeur par défaut est **Native** pour les serveurs de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] publication. Pour les serveurs de publication non- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , la valeur par défaut est **caractère** lorsque la valeur de *repl_freq* est **snapshot** et **concurrent_c** pour tous les autres cas.|  
  
`[ \@repl_freq = ] 'repl_freq'` Est le type de fréquence de réplication, *repl_freq* est **nvarchar (10)** et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**continu** (par défaut)|Le serveur de publication fournit la sortie de toutes les transactions enregistrées dans le journal. Pour les serveurs de publication non- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , cela nécessite que *sync_method* soit défini sur **concurrent_c**.|  
|**instantané**|Le serveur de publication fournit uniquement les événements de synchronisation planifiés. Pour les serveurs de publication non- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , cela nécessite que *sync_method* soit définie sur **caractère**.|  
  
`[ \@description = ] 'description'` Description facultative de la publication. *Description* est de type **nvarchar (255)**, avec NULL comme valeur par défaut.  
  
`[ \@status = ] 'status'` Spécifie si les données de publication sont disponibles. *Status* est de type **nvarchar (8)** et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**active**|Les données de publication sont disponibles immédiatement pour les Abonnés.|  
|**inactif** (par défaut)|Les données de la publication ne sont pas disponibles pour les Abonnés lors de la première création de la publication (ils peuvent s'abonner, mais les abonnements ne sont pas traités).|  
  
 *Non pris en charge pour les serveurs de publication Oracle*.  
  
`[ \@independent_agent = ] 'independent_agent'` Spécifie s’il existe un Agent de distribution autonome pour cette publication. *independent_agent* est de type **nvarchar (5)**, avec false comme valeur par défaut. Si la **valeur est true**, il existe un agent de distribution autonome pour cette publication. Si la **valeur est false**, la publication utilise un agent de distribution partagé, et chaque paire base de données du serveur de publication/base de données de l’abonné possède un seul agent partagé.  
  
`[ \@immediate_sync = ] 'immediate_synchronization'` Spécifie si les fichiers de synchronisation de la publication sont créés chaque fois que le Agent d’instantané s’exécute. *immediate_synchronization* est de type **nvarchar (5)**, avec false comme valeur par défaut. Si la **valeur est true**, les fichiers de synchronisation sont créés ou recréés à chaque exécution de la agent d’instantané. Les Abonnés peuvent obtenir les fichiers de synchronisation immédiatement si l'Agent d'instantané a fini de s'exécuter avant la création de l'abonnement. Les nouveaux abonnements obtiennent les fichiers de synchronisation les plus récents, générés lors de la dernière exécution de l'Agent d'instantané. *independent_agent* doit avoir la **valeur true** pour que *immediate_synchronization* ait la **valeur true**. Si la **valeur est false**, les fichiers de synchronisation sont créés uniquement s’il existe de nouveaux abonnements. Vous devez appeler [sp_addsubscription](../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md) pour chaque abonnement lorsque vous ajoutez de manière incrémentielle un nouvel article à une publication existante. Les abonnés ne peuvent recevoir les fichiers de synchronisation après s'être abonnés qu'après le lancement et l'exécution des Agents d'instantané.  
  
`[ \@enabled_for_internet = ] 'enabled_for_internet'` Spécifie si la publication est activée pour Internet et détermine si le protocole FTP (File Transfer Protocol) peut être utilisé pour transférer les fichiers d’instantanés à un abonné. *enabled_for_internet* est de type **nvarchar (5)**, avec false comme valeur par défaut. Si la **valeur est true**, les fichiers de synchronisation de la publication sont placés dans le répertoire C:\Program Files\Microsoft SQL Server\MSSQL\MSSQL.x\Repldata\Ftp. L'utilisateur doit créer le répertoire FTP.  
  
`[ \@allow_push = ] 'allow_push'` Spécifie si des abonnements par émission de données peuvent être créés pour la publication donnée. *allow_push* est de type **nvarchar (5)**, avec true comme valeur par défaut, ce qui permet l’envoi d’abonnements à la publication.  
  
`[ \@allow_pull = ] 'allow_pull'` Spécifie si des abonnements par extraction de données peuvent être créés pour la publication donnée. *allow_pull* est de type **nvarchar (5)**, avec false comme valeur par défaut. Si la **valeur est false**, les abonnements par extraction ne sont pas autorisés dans la publication.  
  
`[ \@allow_anonymous = ] 'allow_anonymous'` Spécifie si des abonnements anonymes peuvent être créés pour la publication donnée. *allow_anonymous* est de type **nvarchar (5)**, avec false comme valeur par défaut. Si la **valeur est true**, *immediate_synchronization* doit également avoir la valeur **true**. Si la **valeur est false**, les abonnements anonymes ne sont pas autorisés sur la publication.  
  
`[ \@allow_sync_tran = ] 'allow_sync_tran'` Spécifie si les abonnements avec mise à jour immédiate sont autorisés pour la publication. *allow_sync_tran* est de type **nvarchar (5)**, avec false comme valeur par défaut. la **valeur true** n’est *pas prise en charge pour les serveurs de publication Oracle*.  
  
`[ \@autogen_sync_procs = ] 'autogen_sync_procs'` Spécifie si la procédure stockée de synchronisation pour la mise à jour des abonnements est générée sur le serveur de publication. *autogen_sync_procs* est de type **nvarchar (5)** et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**true**|Défini automatiquement lorsque la mise à jour des abonnements est activée.|  
|**false**|Défini automatiquement lorsque la mise à jour des abonnements n'est pas activée, ou pour les serveurs de publication Oracle.|  
|NULL (par défaut)|La valeur par défaut est **true** lorsque la mise à jour des abonnements est activée et la **valeur false** lorsque la mise à jour des abonnements n’est pas activée.|  
  
> [!NOTE]  
>  La valeur fournie par l’utilisateur pour *autogen_sync_procs* est remplacée en fonction des valeurs spécifiées pour *allow_queued_tran* et *allow_sync_tran*.  
  
`[ \@retention = ] retention` Période de rétention, en heures, pour l’activité d’abonnement. la *rétention* est de **type int**, avec une valeur par défaut de 336 heures. Si un abonnement reste inactif durant la période de rétention, il arrive à expiration et est supprimé. La valeur peut être supérieure à la période de rétention maximale de la base de données de distribution utilisée par le serveur de publication. Si la **valeur est 0**, les abonnements bien connus à la publication n’expirent jamais et sont supprimés par l’agent de nettoyage de l’abonnement expiré.  
  
`[ \@allow_queued_tran = ] 'allow_queued_updating'` Active ou désactive la mise en file d’attente des modifications sur l’abonné jusqu’à ce qu’elles puissent être appliquées sur le serveur de publication. *allow_queued_updating* est de type **nvarchar (5),** avec false comme valeur par défaut. Si la **valeur est false**, les modifications au niveau de l’abonné ne sont pas mises en file d’attente. la **valeur true** n’est *pas prise en charge pour les serveurs de publication Oracle*.  
  
`[ \@snapshot_in_defaultfolder = ] 'snapshot_in_default_folder'` Spécifie si les fichiers d’instantanés sont stockés dans le dossier par défaut. *snapshot_in_default_folder* est de type **nvarchar (5)** et sa valeur par défaut est true. Si la **valeur est true**, les fichiers d’instantanés se trouvent dans le dossier par défaut. Si la **valeur est false**, les fichiers d’instantanés ont été stockés à l’emplacement secondaire spécifié par *alternate_snapshot_folder*. Les emplacements de remplacement peuvent se trouver sur un autre serveur, un lecteur réseau ou un support amovible (tel qu'un CD-ROM ou des disques amovibles). Vous pouvez également enregistrer les fichiers d'instantané sur un site FTP, pour qu'ils soient récupérés ultérieurement par l'Abonné. Notez que ce paramètre peut avoir la valeur true et qu’il a toujours un emplacement dans le paramètre **\@ alt_snapshot_folder** . Cette combinaison indique que les fichiers d'instantané sont stockés dans les emplacements par défaut et secondaires.  
  
`[ \@alt_snapshot_folder = ] 'alternate_snapshot_folder'` Spécifie l’emplacement du dossier de remplacement pour l’instantané. *alternate_snapshot_folder* est de type **nvarchar (255),** avec NULL comme valeur par défaut.  
  
`[ \@pre_snapshot_script = ] 'pre_snapshot_script'` Spécifie un pointeur vers un emplacement de fichier **. SQL** . *pre_snapshot_script* est de type **nvarchar (255)**, avec NULL comme valeur par défaut. L'Agent de distribution exécute le script de pré-instantané avant l'exécution des scripts d'objet répliqué, lors de l'application d'un instantané sur un Abonné. Le script est exécuté dans le contexte de sécurité utilisé par l'Agent de distribution lors de sa connexion à la base de données d'abonnement.  
  
`[ \@post_snapshot_script = ] 'post_snapshot_script'` Spécifie un pointeur vers un emplacement de fichier **. SQL** . *post_snapshot_script* est de type **nvarchar (255)**, avec NULL comme valeur par défaut. L'Agent de distribution exécute le script de post-instantané après que tous les autres scripts et données d'objet répliqué ont été appliqués lors d'une synchronisation initiale. Le script est exécuté dans le contexte de sécurité utilisé par l'Agent de distribution lors de sa connexion à la base de données d'abonnement.  
  
`[ \@compress_snapshot = ] 'compress_snapshot'`Spécifie que l’instantané écrit dans l’emplacement de **\@ alt_snapshot_folder** doit être compressé au [!INCLUDE[msCoName](../../includes/msconame-md.md)] format cab. *compress_snapshot* est de type **nvarchar (5)**, avec false comme valeur par défaut. **false** spécifie que l’instantané ne sera pas compressé ; **true** spécifie que l’instantané sera compressé. Les fichiers d'instantané de plus de 2 Go (gigaoctets) ne peuvent pas être compressés. Les fichiers d'instantané compressés sont décompressés là où s'exécute l'Agent de distribution ; les abonnements par extraction de données (pull) sont en général utilisés avec des instantanés compressés et les fichiers sont décompressés sur l'Abonné. L'instantané se trouvant dans le dossier par défaut ne peut pas être compressé.  
  
`[ \@ftp_address = ] 'ftp_address'` Adresse réseau du service FTP du serveur de distribution. *ftp_address* est de **type sysname**, avec NULL comme valeur par défaut. Indique l'emplacement à partir duquel l'Agent de distribution ou l'Agent de fusion d'un abonné peut extraire les fichiers d'instantané de la publication. Étant donné que cette propriété est stockée pour chaque publication, chaque publication peut avoir une *ftp_address* différente. La publication doit prendre en charge la propagation des instantanés à l'aide du protocole FTP.  
  
`[ \@ftp_port = ] ftp_port` Numéro de port du service FTP du serveur de distribution. *ftp_port* est de **type int**, avec 21 comme valeur par défaut. Spécifie l'emplacement à partir duquel l'Agent de distribution ou l'Agent de fusion d'un Abonné peut extraire les fichiers d'instantané de la publication. Étant donné que cette propriété est stockée pour chaque publication, chaque publication peut avoir sa propre *ftp_port*.  
  
`[ \@ftp_subdirectory = ] 'ftp_subdirectory'` Spécifie l’emplacement où les fichiers d’instantanés seront disponibles pour la Agent de distribution ou Agent de fusion de l’abonné à récupérer si la publication prend en charge la propagation d’instantanés à l’aide de FTP. *ftp_subdirectory* est de type **nvarchar (255)**, avec NULL comme valeur par défaut. Étant donné que cette propriété est stockée pour chaque publication, chaque publication peut avoir sa propre *ftp_subdirctory* ou choisir de n’avoir aucun sous-répertoire, indiqué par une valeur null.  
  
`[ \@ftp_login = ] 'ftp_login'` Nom d’utilisateur utilisé pour la connexion au service FTP. *ftp_login* est de **type sysname**, avec Anonymous comme valeur par défaut.  
  
`[ \@ftp_password = ] 'ftp_password'` Mot de passe de l’utilisateur utilisé pour la connexion au service FTP. *ftp_password* est de **type sysname**, avec NULL comme valeur par défaut.  
  
`[ \@allow_dts = ] 'allow_dts'` Spécifie que la publication autorise les transformations de données. Vous pouvez spécifier un package DTS lors de la création d'un abonnement. *allow_transformable_subscriptions* est de type **nvarchar (5)** et sa valeur par défaut est false, ce qui n’autorise pas les transformations DTS. Lorsque *allow_dts* a la valeur true, *sync_method* doit être défini sur **caractère** ou **concurrent_c**.  
  
 la **valeur true** n’est *pas prise en charge pour les serveurs de publication Oracle*.  
  
`[ \@allow_subscription_copy = ] 'allow_subscription_copy'` Active ou désactive la possibilité de copier les bases de données d’abonnement qui s’abonnent à cette publication. *allow_subscription_copy* est de type **nvarchar (5)**, avec false comme valeur par défaut.  
  
`[ \@conflict_policy = ] 'conflict_policy'` Spécifie la stratégie de résolution des conflits suivie lorsque l’option d’abonné avec mise à jour en attente est utilisée. *conflict_policy* est de type **nvarchar (100)** avec NULL comme valeur par défaut et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**pub wins**|Le serveur de publication remporte le conflit.|  
|**sub reinit**|Réinitialisez l'abonnement.|  
|**sub wins**|L'Abonné remporte le conflit.|  
|NULL (par défaut)|Si la valeur est NULL et que la publication est une publication d’instantané, la stratégie par défaut devient **Sub-init**. Si la valeur est NULL et que la publication n’est pas une publication d’instantané, la valeur par défaut est **pub WINS**.|  
  
 *Non pris en charge pour les serveurs de publication Oracle*.  
  
`[ \@centralized_conflicts = ] 'centralized_conflicts'` Spécifie si les enregistrements en conflit sont stockés sur le serveur de publication. *centralized_conflicts* est de type **nvarchar (5)**, avec true comme valeur par défaut. Si la **valeur est true**, les enregistrements en conflit sont stockés sur le serveur de publication. Si la **valeur est false**, les enregistrements en conflit sont stockés sur le serveur de publication et sur l’abonné à l’origine du conflit. *Non pris en charge pour les serveurs de publication Oracle*.  
  
`[ \@conflict_retention = ] conflict_retention` Spécifie la période de rétention des conflits, en jours. Il s'agit de la période pendant laquelle les métadonnées en conflit sont stockées pour la réplication transactionnelle d'égal à égal et les abonnements mis à jour en attente. *conflict_retention* est de **type int**, avec 14 comme valeur par défaut. *Non pris en charge pour les serveurs de publication Oracle*.  
  
`[ \@queue_type = ] 'queue_type'` Spécifie le type de file d’attente utilisé. *queue_type* est de type **nvarchar (10)**, avec NULL comme valeur par défaut et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**sql**|Utilise [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour stocker les transactions.|  
|NULL (par défaut)|La valeur par défaut est **SQL**, qui spécifie d’utiliser [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour stocker des transactions.|  
  
> [!NOTE]  
>  La prise en charge de l'utilisation de [!INCLUDE[msCoName](../../includes/msconame-md.md)] Message Queuing a été arrêtée. Si vous spécifiez la valeur **MSMQ** , vous obtenez un avertissement, et la réplication définit automatiquement la valeur sur **SQL**.  
  
 *Non pris en charge pour les serveurs de publication Oracle*.  
  
`[ \@add_to_active_directory = ] 'add\to_active_directory'` Ce paramètre est déconseillé et n’est pris en charge que pour la compatibilité descendante des scripts. Vous ne pouvez plus ajouter d'informations de publication à [!INCLUDE[msCoName](../../includes/msconame-md.md)] Active Directory.  
  
`[ \@logreader_job_name = ] 'logreader_agent_name'` Nom d’un travail d’agent existant. *logreader_agent_name* est de **type sysname**, avec NULL comme valeur par défaut. Ce paramètre n'est spécifié que lorsque l'Agent de lecture du journal doit utiliser un travail existant et non pas un travail nouvellement créé.  
  
`[ \@qreader_job_name = ] 'queue_reader_agent_name'` Nom d’un travail d’agent existant. *queue_reader_agent_name* est de **type sysname**, avec NULL comme valeur par défaut. Ce paramètre n'est spécifié que lorsque l'Agent de lecture de file d'attente doit utiliser un travail existant et non pas un travail nouvellement créé.  
  
`[ \@publisher = ] 'publisher'` Spécifie un serveur de publication non- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . *Publisher* est de **type sysname**, avec NULL comme valeur par défaut.  
  
> [!NOTE]  
>  l' *éditeur* ne doit pas être utilisé lors de l’ajout d’une publication à un serveur de publication [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
`[ \@allow_initialize_from_backup = ] 'allow_initialize_from_backup'` Indique si les abonnés peuvent initialiser un abonnement à cette publication à partir d’une sauvegarde plutôt que d’un instantané initial. *allow_initialize_from_backup* est de type **nvarchar (5)** et peut prendre l’une des valeurs suivantes :  
  
|Valeur|Description|  
|-----------|-----------------|  
|**true**|Autorise l'initialisation à partir d'une sauvegarde.|  
|**false**|Interdit l'initialisation à partir d'une sauvegarde.|  
|NULL (par défaut)|La valeur par défaut est **true** pour une publication dans une topologie de réplication d’égal à égal et **false** pour toutes les autres publications.|  
  
 Pour plus d’informations, consultez [Initialiser un abonnement transactionnel sans instantané](../../relational-databases/replication/initialize-a-transactional-subscription-without-a-snapshot.md).  
  
> [!WARNING]  
>  Afin d'éviter les données d'abonnés manquantes, lorsque vous utilisez **sp_addpublication** avec `@allow_initialize_from_backup = N'true'`, utilisez toujours `@immediate_sync = N'true'`.  
  
`[ \@replicate_ddl = ] replicate_ddl` Indique si la réplication de schéma est prise en charge pour la publication. *replicate_ddl* est de **type int**, avec **1** comme valeur par défaut pour les serveurs de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] publication et **0** pour les serveurs de publication non- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . **1** indique que les instructions DDL (Data Definition Language) exécutées sur le serveur de publication sont répliquées, et **0** indique que les instructions DDL ne sont pas répliquées. *La réplication de schéma n'est pas prise en charge pour les serveurs de publication Oracle.* Pour plus d’informations, consultez [Modifier le schéma dans les bases de données de publication](../../relational-databases/replication/publish/make-schema-changes-on-publication-databases.md).  
  
 Le paramètre *\@ replicate_ddl* est respecté lorsqu’une instruction DDL ajoute une colonne. Le paramètre *\@ replicate_ddl* est ignoré lorsqu’une instruction DDL modifie ou supprime une colonne pour les raisons suivantes.  
  
-   Lorsqu'une colonne est supprimée, sysarticlecolumns doit être mis à jour pour empêcher de nouvelles instructions DML d'inclure la colonne supprimée, ce qui provoquerait l'échec de l'agent de distribution. Le paramètre *\@ replicate_ddl* est ignoré, car la réplication doit toujours répliquer la modification de schéma.  
  
-   Lorsqu'une colonne est modifiée, le type de données source ou la possibilité d'une valeur NULL peuvent avoir changé et les instructions DML peuvent contenir une valeur non compatible avec la table sur l'abonné. Ces instructions DML peuvent entraîner l'échec de l'agent de distribution. Le paramètre *\@ replicate_ddl* est ignoré, car la réplication doit toujours répliquer la modification de schéma.  
  
-   Lorsqu'une instruction DDL ajoute une nouvelle colonne, sysarticlecolumns n'inclut pas la nouvelle colonne. Les instructions DML n'essayeront pas de répliquer les données pour la nouvelle colonne. Le paramètre est respecté parce que la réplication ou la réplication DDL est acceptable.  
  
`[ \@enabled_for_p2p = ] 'enabled_for_p2p'` Permet d’utiliser la publication dans une topologie de réplication d’égal à égal. *enabled_for_p2p* est de type **nvarchar (5)**, avec false comme valeur par défaut. la **valeur true** indique que la publication prend en charge la réplication d’égal à égal. Lorsque vous affectez la **valeur true** à *enabled_for_p2p* , les restrictions suivantes s’appliquent :  
  
-   *allow_anonymous* doit avoir la **valeur false**.  
  
-   *allow_dts* doit avoir la **valeur false**.  
  
-   *allow_initialize_from_backup* doit avoir la **valeur true**.  
  
-   *allow_queued_tran* doit avoir la **valeur false**.  
  
-   *allow_sync_tran* doit avoir la **valeur false**.  
  
-   *conflict_policy* doit avoir la **valeur false**.  
  
-   *independent_agent* doit avoir la **valeur true**.  
  
-   les *repl_freq* doivent être **continus**.  
  
-   *replicate_ddl* doit être **1**.  
  
 Pour plus d'informations, consultez [Peer-to-Peer Transactional Replication](../../relational-databases/replication/transactional/peer-to-peer-transactional-replication.md).  
  
`[ \@publish_local_changes_only = ] 'publish_local_changes_only'` [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
`[ \@enabled_for_het_sub = ] 'enabled_for_het_sub'` Permet à la publication de prendre en charge les abonnés non- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . *enabled_for_het_sub* est de type **nvarchar (5),** avec false comme valeur par défaut. La valeur **true** signifie que la publication prend en charge les [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] abonnés non-. Lorsque *enabled_for_het_sub* a la **valeur true**, les restrictions suivantes s’appliquent :  
  
-   *allow_initialize_from_backup* doit avoir la **valeur false**.  
  
-   *allow_push* doit avoir la **valeur true**.  
  
-   *allow_queued_tran* doit avoir la **valeur false**.  
  
-   *allow_subscription_copy* doit avoir la **valeur false**.  
  
-   *allow_sync_tran* doit avoir la **valeur false**.  
  
-   *autogen_sync_procs* doit avoir la **valeur false**.  
  
-   *conflict_policy* doit avoir la valeur null.  
  
-   *enabled_for_internet* doit avoir la **valeur false**.  
  
-   *enabled_for_p2p* doit avoir la **valeur false**.  
  
-   *ftp_address* doit avoir la valeur null.  
  
-   *ftp_subdirectory* doit avoir la valeur null.  
  
-   *ftp_password* doit avoir la valeur null.  
  
-   *pre_snapshot_script* doit avoir la valeur null.  
  
-   *post_snapshot_script* doit avoir la valeur null.  
  
-   *replicate_ddl* doit avoir la valeur 0.  
  
-   *qreader_job_name* doit avoir la valeur null.  
  
-   *queue_type* doit avoir la valeur null.  
  
-   les *sync_method* ne peuvent pas être **natives** ou **simultanées**.  
  
 Pour plus d’informations, consultez [Non-SQL Server Subscribers](../../relational-databases/replication/non-sql/non-sql-server-subscribers.md).  
  
`[ \@p2p_conflictdetection = ] 'p2p_conflictdetection'` Permet à l’Agent de distribution de détecter des conflits si la publication est activée pour la réplication d’égal à égal. *p2p_conflictdetection* est de type **nvarchar (5)** et sa valeur par défaut est true. Pour plus d’informations, voir [Conflict Detection in Peer-to-Peer Replication](../../relational-databases/replication/transactional/peer-to-peer-conflict-detection-in-peer-to-peer-replication.md).  
  
`[ \@p2p_originator_id = ] p2p_originator_id` Spécifie un ID pour un nœud dans une topologie d’égal à égal. *p2p_originator_id* est de **type int**, avec NULL comme valeur par défaut. Cet ID est utilisé pour la détection de conflit si *p2p_conflictdetection* a la valeur true. Spécifiez un ID positif différent de zéro qui n’a jamais été utilisé dans la topologie. Pour obtenir la liste des ID qui ont déjà été utilisés, exécutez [sp_help_peerconflictdetection](../../relational-databases/system-stored-procedures/sp-help-peerconflictdetection-transact-sql.md).  
  
`[ \@p2p_continue_onconflict = ] 'p2p_continue_onconflict'` Détermine si le Agent de distribution continue à traiter les modifications après la détection d’un conflit. *p2p_continue_onconflict* est de type **nvarchar (5),** avec false comme valeur par défaut.  
  
> [!CAUTION]  
>  Nous vous recommandons de conserver la valeur par défaut FALSE. Lorsque cette option a la valeur TRUE, l'Agent de distribution tente de converger les données dans la topologie en appliquant la ligne en conflit du nœud doté de l'ID d'appelant le plus élevé. Cette méthode ne garantit pas la convergence. Vous devez vous assurer que la topologie est cohérente après la détection d'un conflit. Pour plus d'informations, consultez « Gestion des conflits » dans [Conflict Detection in Peer-to-Peer Replication](../../relational-databases/replication/transactional/peer-to-peer-conflict-detection-in-peer-to-peer-replication.md).  
  
  
`[ \@allow_partition_switch = ] 'allow_partition_switch'` Spécifie si ALTER TABLE... Les instructions SWITCH peuvent être exécutées sur la base de données publiée. *allow_partition_switch* est de type **nvarchar (5),** avec false comme valeur par défaut. Pour plus d’informations, consultez [Répliquer des tables et des index partitionnés](../../relational-databases/replication/publish/replicate-partitioned-tables-and-indexes.md).  
  
`[ \@replicate_partition_switch = ] 'replicate_partition_switch'` Spécifie si ALTER TABLE... Les instructions SWITCH exécutées sur la base de données publiée doivent être répliquées sur les abonnés. *replicate_partition_switch* est de type **nvarchar (5),** avec false comme valeur par défaut. Cette option est valide uniquement si *allow_partition_switch* a la valeur true.  

## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Remarques  
 **sp_addpublication** est utilisé dans la réplication d’instantané et la réplication transactionnelle.  
  
 S’il existe plusieurs publications qui publient le même objet de base de données, seules les publications dont la valeur de *replicate_ddl* est **1** RÉPLIQUENT les instructions ALTER TABLE, ALTER VIEW, ALTER PROCEDURE, ALTER FUNCTION et ALTER TRIGGER DDL. Cependant, une instruction ALTER TABLE DROP COLUMN DDL sera répliquée par toutes les publications publiant la colonne supprimée.  
  
 Si la réplication DDL est activée (*replicate_ddl*  =  **1**) pour une publication, afin d’effectuer des modifications DDL non répliquées dans la publication, [sp_changepublication](../../relational-databases/system-stored-procedures/sp-changepublication-transact-sql.md) devez d’abord exécuter pour définir *replicate_ddl* sur **0**. Après l’émission des instructions DDL non répliquées, [sp_changepublication](../../relational-databases/system-stored-procedures/sp-changepublication-transact-sql.md) peut être réexécutée pour réactiver la réplication DDL.  
  
## <a name="example"></a>Exemple  
 [!code-sql[HowTo#sp_AddTranPub](../../relational-databases/replication/codesnippet/tsql/sp-addpublication-transa_1.sql)]  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** ou du rôle de base de données fixe **db_owner** peuvent exécuter **sp_addpublication**. Les connexions d'authentification Windows doivent disposer d'un compte d'utilisateur dans la base de données, représentant leur compte d'utilisateur Windows. Un compte d'utilisateur représentant un groupe Windows n'est pas suffisant.  
  
## <a name="see-also"></a>Voir aussi  
 [sp_addlogreader_agent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addlogreader-agent-transact-sql.md)   
 [sp_addpublication_snapshot &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addpublication-snapshot-transact-sql.md)   
 [sp_changepublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changepublication-transact-sql.md)   
 [sp_droppublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-droppublication-transact-sql.md)   
 [sp_helppublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helppublication-transact-sql.md)   
 [sp_replicationdboption &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-replicationdboption-transact-sql.md)   
 [Publier des données et des objets de base de données](../../relational-databases/replication/publish/publish-data-and-database-objects.md)   
 [Procédures stockées de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
