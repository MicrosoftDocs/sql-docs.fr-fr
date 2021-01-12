---
description: sys.dm_db_wait_stats (Azure SQL Database)
title: sys.dm_db_wait_stats (Azure SQL Database) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.service: sql-database
ms.reviewer: ''
ms.topic: language-reference
f1_keywords:
- dm_db_wait_stats_TSQL
- dm_db_wait_stats
- sys.dm_db_wait_stats
- sys.dm_db_wait_stats_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_wait_stats dynamic management view
- dm_db_wait_stats
ms.assetid: 00abd0a5-bae0-4d71-b173-f7a14cddf795
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: = azuresqldb-current
ms.openlocfilehash: 3d711b3eb0cb99c65582d8feacf8678fbca264c7
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092918"
---
# <a name="sysdm_db_wait_stats-azure-sql-database"></a>sys.dm_db_wait_stats (Azure SQL Database)
[!INCLUDE[Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/asdb-asdbmi.md)]

  Retourne des informations sur toutes les attentes subies par les threads qui se sont exécutés pendant l'opération. Vous pouvez utiliser cette vue agrégée pour diagnostiquer les problèmes de performance liés à [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] et également liés à des requêtes et des traitements spécifiques.  
  
 Des types spécifiques de temps d'attente pendant l'exécution des requêtes peuvent indiquer des goulots d'étranglement ou des points de blocage dans la requête. De la même façon, des temps d'attente élevés, ou des nombres d'attente à l'échelle du serveur peuvent indiquer des goulots d'étranglement ou des zones réactives en interaction avec l'instance du serveur. Par exemple, des attentes de verrou indiquent une contention de données par les requêtes ; des attentes de verrou interne d'E/S de page indiquent des temps de réponse E/S lents ; des attentes de mise à jour de verrous internes de page indiquent une mise en page de fichier incorrecte.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|wait_type|**nvarchar(60)**|Nom du type d'attente. Pour plus d'informations, consultez [Types d'attentes](#WaitTypes), plus loin dans cette rubrique.|  
|waiting_tasks_count|**bigint**|Nombre d'attentes sur ce type d'attente. Ce compteur est incrémenté au début de chaque attente.|  
|wait_time_ms|**bigint**|Temps d'attente total en millisecondes pour ce type d'attente. Ce temps comprend signal_wait_time_ms.|  
|max_wait_time_ms|**bigint**|Temps d'attente maximal sur ce type d'attente.|  
|signal_wait_time_ms|**bigint**|Différence entre le moment où le thread qui attend a été signalé et le moment où il a commencé à s'exécuter.|  
  
## <a name="remarks"></a>Notes  
  
-   Cette vue de gestion dynamique affiche des données uniquement pour la base de données active.  
  
-   Cette vue de gestion dynamique montre la durée des attentes qui se sont achevées. Elle n'affiche pas les attentes en cours.  
  
-   Les compteurs sont remis à zéro lorsque la base de données est déplacée ou est hors connexion.  
  
-   Un thread de travail SQL Server n'est pas considéré comme en attente si l'une de ces conditions est remplie :  
  
    -   Une ressource devient disponible.  
  
    -   Une file d'attente n'est pas vide.  
  
    -   Un processus externe se termine.  
  
-   Ces statistiques ne sont pas préservées d'un événement de basculement SQL Database à l'autre et toutes les données s'accumulent depuis la dernière réinitialisation des statistiques.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'autorisation VIEW DATABASE STATE sur le serveur.  
  
##  <a name="types-of-waits"></a><a name="WaitTypes"></a> Types d’attente  
 Resource waits  
 Les attentes de ressource se produisent lorsque des demandes de travail accèdent à une ressource qui n'est pas disponible parce qu'elle est en cours d'utilisation par un autre travail ou qu'elle n'est pas encore disponible. Ces attentes sont par exemple des attentes de verrous, de verrous internes, de réseau et d'E/S de disque. Les attentes de verrou (interne ou externe) sont des attentes sur des objets de synchronisation.  
  
 Queue waits  
 Les attentes de file d'attente se produisent lorsqu'un thread de travail est inactif, en attente d'une affectation de travail. Ces attentes se produisent le plus souvent avec des tâches système en arrière-plan, telles que la surveillance des interblocages et le nettoyage des enregistrements supprimés. Ces tâches attendent que les demandes de travail soient placées dans une file d'attente. Les attentes de file d'attente peuvent aussi devenir actives même si aucun nouveau paquet n'a été placé dans la file d'attente.  
  
 External waits  
 Les attentes externes se produisent lorsqu'un thread de travail [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] attend un événement externe, par exemple un appel de procédure stockée étendue ou une requête de serveur lié, pour se terminer. Lorsque vous diagnostiquez des problèmes de blocage, souvenez-vous que les attentes externes n'impliquent pas forcément que le thread de travail est inactif, parce qu'il est peut être en train d'exécuter du code externe.  
  
 Bien que le thread ne soit plus en train d'attendre, il n'a pas à redémarrer immédiatement. En effet, ce type de thread est d'abord placé dans la file d'attente des travaux pouvant s'exécuter et doit attendre qu'un quantum s'exécute sur le planificateur.  
  
 Dans, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] les compteurs de temps d’attente sont des valeurs **bigint** et, par conséquent, ne sont pas aussi sujets à la substitution de compteur que les compteurs équivalents dans les versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 Le tableau suivant récapitule les types d'attente que rencontrent les tâches.  
  
|Type d’attente|Description|  
|---------------|-----------------|  
|ABR|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|ASSEMBLY_LOAD|Se produit pendant l'accès exclusif au chargement d'un assembly.|  
|ASYNC_DISKPOOL_LOCK|Se produit lors d'une tentative de synchronisation de threads parallèles qui effectuent des tâches telles que la création ou l'initialisation d'un fichier.|  
|ASYNC_IO_COMPLETION|Se produit lorsqu'une tâche attend la fin d'E/S.|  
|ASYNC_NETWORK_IO|Se produit sur des écritures réseau lorsque la tâche est bloquée derrière le réseau. Vérifiez que le client traite les données du serveur.|  
|AUDIT_GROUPCACHE_LOCK|Se produit lorsqu'il y a une attente sur un verrou qui contrôle l'accès à un cache spécial. Le cache contient des informations sur les audits utilisés pour auditer chaque groupe d'actions d'audit.|  
|AUDIT_LOGINCACHE_LOCK|Se produit lorsqu'il y a une attente sur un verrou qui contrôle l'accès à un cache spécial. Le cache contient des informations sur les audits utilisés pour auditer des groupes d'actions d'audit de connexion.|  
|AUDIT_ON_DEMAND_TARGET_LOCK|Se produit lorsqu'il y a une attente sur un verrou utilisé pour garantir une initialisation unique des cibles d'événements étendus liées à l'audit.|  
|AUDIT_XE_SESSION_MGR|Se produit lorsqu'il y a une attente sur un verrou utilisé pour synchroniser le démarrage et l'arrêt des sessions d'événements étendus liées à l'audit.|  
|BACKUP|Se produit lorsqu'une tâche est bloquée dans un traitement de sauvegarde.|  
|BACKUP_OPERATOR|Se produit lorsqu'une tâche attend le montage d'une bande. Pour voir l'état de la bande, interrogez [sys.dm_io_backup_tapes](../../relational-databases/system-dynamic-management-views/sys-dm-io-backup-tapes-transact-sql.md). S'il n'y a pas d'opération de montage en cours, ce type d'attente peut indiquer un problème matériel au niveau du lecteur de bande.|  
|BACKUPBUFFER|Se produit lorsqu'une tâche de sauvegarde attend des données ou une mémoire tampon pour y stocker les données. Ce type d'attente n'est pas typique, sauf lorsqu'une tâche attend qu'une bande monte.|  
|BACKUPIO|Se produit lorsqu'une tâche de sauvegarde attend des données ou une mémoire tampon pour y stocker les données. Ce type d'attente n'est pas typique, sauf lorsqu'une tâche attend qu'une bande monte.|  
|BACKUPTHREAD|Se produit lorsqu'une tâche attend la fin d'une tâche de sauvegarde. Les temps d'attente peuvent être longs, de plusieurs minutes à plusieurs heures. Si la tâche concernée est un processus d'E/S, ce type n'indique pas de problème.|  
|BAD_PAGE_PROCESS|Se produit lorsque le journal de pages suspectes en arrière-plan tente d'éviter une exécution plus que toutes les cinq secondes. Un nombre excessif de pages suspectes entraîne une exécution fréquente du journal.|  
|BROKER_CONNECTION_RECEIVE_TASK|Se produit lorsque vous attendez l'autorisation de recevoir un message sur un point de terminaison de connexion. L'accès au point de terminaison accordé est sérialisé.|  
|BROKER_ENDPOINT_STATE_MUTEX|Se produit en cas de contention pour accéder à l'état d'un point de terminaison de connexion de [!INCLUDE[ssSB](../../includes/sssb-md.md)]. L'accès à l'état pour procéder à des modifications est sérialisé.|  
|BROKER_EVENTHANDLER|Se produit lorsqu'une tâche attend dans le gestionnaire d'événements principal de [!INCLUDE[ssSB](../../includes/sssb-md.md)]. Ce doit être très bref.|  
|BROKER_INIT|Se produit lors de l'initialisation de [!INCLUDE[ssSB](../../includes/sssb-md.md)] dans chaque base de données active. Cela ne doit pas arriver souvent.|  
|BROKER_MASTERSTART|Se produit lorsqu'une tâche attend le début du gestionnaire d'événements principal de [!INCLUDE[ssSB](../../includes/sssb-md.md)]. Ce doit être très bref.|  
|BROKER_RECEIVE_WAITFOR|Se produit lorsque RECEIVE WAITFOR attend. Typique si aucun message n'est prêt à être reçu.|  
|BROKER_REGISTERALLENDPOINTS|Se produit lors de l'initialisation d'un point de terminaison de connexion de [!INCLUDE[ssSB](../../includes/sssb-md.md)]. Ce doit être très bref.|  
|BROKER_SERVICE|Se produit lorsque la liste de destinations [!INCLUDE[ssSB](../../includes/sssb-md.md)] associée à un service cible est mise à jour ou retriée selon sa priorité.|  
|BROKER_SHUTDOWN|Se produit lorsqu'il y a un arrêt programmé de [!INCLUDE[ssSB](../../includes/sssb-md.md)]. Ce doit être très bref, voire inexistant.|  
|BROKER_TASK_STOP|Se produit lorsque le gestionnaire de tâches de file d'attente [!INCLUDE[ssSB](../../includes/sssb-md.md)] essaie d'arrêter la tâche. Le contrôle d'état est sérialisé et doit être au préalable dans un état d'exécution.|  
|BROKER_TO_FLUSH|Se produit lorsque le vidage « paresseux » [!INCLUDE[ssSB](../../includes/sssb-md.md)] vide les objets de transmission en mémoire dans une table de travail.|  
|BROKER_TRANSMITTER|Se produit lorsque l'émetteur [!INCLUDE[ssSB](../../includes/sssb-md.md)] est en attente de travail.|  
|BUILTIN_HASHKEY_MUTEX|Peut se produire après le démarrage d'une instance, pendant l'initialisation des structures de données internes. Ne se reproduira plus lorsque les structures de données auront été initialisées.|  
|CHECK_PRINT_RECORD|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|CHECKPOINT_QUEUE|Se produit lorsque la tâche de point de vérification attend la demande de point de vérification suivante.|  
|CHKPT|Se produit au démarrage du serveur pour signaler au thread du point de contrôle qu'il peut démarrer.|  
|CLEAR_DB|Se produit lors d'opérations qui modifient l'état d'une base de données, telles que l'ouverture ou la fermeture d'une base de données.|  
|CLR_AUTO_EVENT|Se produit lorsqu'une tâche est en train d'exécuter du CLR (Common Language Runtime) et qu'elle attend le début d'un événement automatique particulier. De longues attentes sont classiques et elles n'indiquent pas la présence d'un problème.|  
|CLR_CRST|Se produit lorsqu'une tâche est en train d'exécuter du CLR et qu'elle attend avant de passer à une phase essentielle qui est actuellement utilisée par une autre tâche.|  
|CLR_JOIN|Se produit lorsqu'une tâche est en train d'exécuter du CLR et qu'elle attend la fin d'une autre tâche. Cet état d'attente se produit lorsqu'il y a une jointure entre des tâches.|  
|CLR_MANUAL_EVENT|Se produit lorsqu'une tâche est en train d'exécuter du CLR et qu'elle attend le début d'un événement manuel particulier.|  
|CLR_MEMORY_SPY|Se produit pendant une attente sur une acquisition de verrou pour une structure de données utilisée pour enregistrer toutes les allocations de mémoire virtuelle qui viennent du CLR. La structure de données est verrouillée pour maintenir son intégrité en cas d'accès parallèle.|  
|CLR_MONITOR|Se produit lorsqu'une tâche est en train d'exécuter du CLR et qu'elle attend d'avoir obtenu un verrou sur le moniteur.|  
|CLR_RWLOCK_READER|Se produit lorsqu'une tâche est en train d'exécuter du CLR et qu'elle attend un verrou de lecteur.|  
|CLR_RWLOCK_WRITER|Se produit lorsqu'une tâche est en train d'exécuter du CLR et qu'elle attend un verrou d'écriture.|  
|CLR_SEMAPHORE|Se produit lorsqu'une tâche est en train d'exécuter du CLR et qu'elle attend un sémaphore.|  
|CLR_TASK_START|Se produit pendant l'attente du démarrage d'une tâche CLR.|  
|CLRHOST_STATE_ACCESS|Se produit lorsqu'il y a une attente pour l'acquisition d'un accès exclusif aux structures de données d'hébergement CLR. Ce type d'attente se produit lors de la configuration ou de la destruction du runtime CLR.|  
|CMEMTHREAD|Se produit lorsqu'une tâche attend un objet mémoire thread-safe. Le temps d'attente peut augmenter en cas de contention liée au fait que plusieurs tâches essaient d'allouer de la mémoire à partir du même objet mémoire.|  
|CXPACKET|Se produit lors d'une tentative de synchronisation de l'itérateur d'échange du processeur de requêtes. Vous pouvez réduire le degré de parallélisme si la contention devient un problème sur ce type d'attente.|  
|CXROWSET_SYNC|Se produit pendant une analyse de plage parallèle.|  
|DAC_INIT|Se produit alors que la connexion administrateur dédiée est en cours d'initialisation.|  
|DBMIRROR_DBM_EVENT|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|DBMIRROR_DBM_MUTEX|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|DBMIRROR_EVENTS_QUEUE|Se produit lorsque la mise en miroir de base de données attend le traitement des événements.|  
|DBMIRROR_SEND|Se produit lorsqu'une tâche attend que le Backlog des communications au niveau de la couche réseau soit vidé pour pouvoir envoyer des messages. Indique que la couche des communications est proche de la saturation et que cela affecte le débit des données pour la mise en miroir de bases de données.|  
|DBMIRROR_WORKER_QUEUE|Indique que la tâche de travail de mise en miroir de base de données attend plus de travail.|  
|DBMIRRORING_CMD|Se produit lorsqu'une tâche attend que les enregistrements de journal soient vidés sur le disque. Cet état d'attente dure généralement assez longtemps.|  
|DEADLOCK_ENUM_MUTEX|Se produit lorsque le moniteur de blocage et sys.dm_os_waiting_tasks essaient de vérifier que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] n'effectue pas plusieurs recherches de blocage en même temps.|  
|DEADLOCK_TASK_SEARCH|Une durée d'attente importante pour cette ressource indique que le serveur exécute des requêtes par-dessus sys.dm_os_waiting_tasks, et que ces dernières empêchent l'analyseur de blocages d'exécuter une recherche de blocage. Ce type d'attente est utilisé uniquement par l'analyseur d'interblocages. Les requêtes exécutées par-dessus sys.dm_os_waiting_tasks utilisent DEADLOCK_ENUM_MUTEX.|  
|DEBUG|Se produit lors du débogage CLR et [!INCLUDE[tsql](../../includes/tsql-md.md)] pour la synchronisation interne.|  
|DISABLE_VERSIONING|Se produit lorsque [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sonde le gestionnaire de transactions de versions pour voir si le cachet temporel de la plus ancienne transaction active est postérieur au cachet temporel indiquant le début de changement d'état. Si c'est le cas, toutes les transactions d'instantané qui avaient démarré avant l'exécution de l'instruction ALTER DATABASE sont terminées. Cet état d'attente est utilisé lorsque [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] désactive le contrôle de version à l'aide de l'instruction ALTER DATABASE.|  
|DISKIO_SUSPEND|Se produit lorsqu'une tâche attend pour accéder à un fichier lorsqu'une sauvegarde externe est en cours. Ce type d'attente est signalé pour chaque processus utilisateur en attente. Un nombre supérieur à cinq par processus utilisateur peut indiquer que la sauvegarde externe prend trop de temps.|  
|DISPATCHER_QUEUE_SEMAPHORE|Se produit lorsqu'un thread du pool de répartiteurs attend de traiter d'autres travaux. Le temps d'attente pour ce type d'attente est supposé augmenter lorsque le répartiteur est inactif.|  
|DLL_LOADING_MUTEX|Se produit une fois pendant l'attente du chargement de la DLL de l'analyseur XML.|  
|DROPTEMP|Se produit entre les tentatives de suppression d'un objet temporaire si la tentative précédente a échoué. La durée d'attente augmente de manière exponentielle au fur et à mesure que les différentes tentatives de suppression échouent.|  
|DTC|Se produit lorsqu'une tâche s'occupe d'un événement qui est utilisé pour gérer une transition d'état. Cet état contrôle le moment où a lieu la récupération des transactions MS DTC (Distributed Transaction Coordinator) de [!INCLUDE[msCoName](../../includes/msconame-md.md)] une fois que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a reçu la notification selon laquelle le service MS DTC n'est plus disponible.<br /><br /> Cet état décrit aussi une tâche qui attend lorsqu'une validation d'une transaction MS DTC est lancée par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] attend la fin de cette validation MS DTC.|  
|DTC_ABORT_REQUEST|Se produit dans une session de travail MS DTC lorsque la session attend de prendre possession d'une transaction MS DTC. Une fois que MS DTC est propriétaire de la transaction, la session peut la restaurer. En général, la session attendra une autre session qui utilise la transaction.|  
|DTC_RESOLVE|Se produit lorsqu'une tâche de récupération attend la base de données master dans une transaction entre bases de données, afin de pouvoir interroger le résultat de la transaction.|  
|DTC_STATE|Se produit lorsqu'une tâche s'occupe d'un événement qui protège les modifications effectuées sur l'objet d'état global MS DTC interne. Cet état doit être maintenu pendant des périodes très courtes.|  
|DTC_TMDOWN_REQUEST|Se produit dans une session de travail MS DTC lorsque [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] reçoit une notification selon laquelle le service MS DTC n'est pas disponible. Tout d'abord, le thread de travail attend que le processus de récupération MS DTC commence. Ensuite, il attend d'avoir obtenu le résultat de la transaction distribuée sur laquelle il travaille. Cela peut continuer jusqu'à ce que la connexion avec le service MS DTC ait été rétablie.|  
|DTC_WAITFOR_OUTCOME|Se produit lorsque des tâches de récupération attendent que MS DTC s'active pour permettre la résolution des transactions préparées.|  
|DUMP_LOG_COORDINATOR|Se produit lorsqu'une tâche principale attend qu'une tâche secondaire ait produit des données. En principe cet état n'arrive pas. Une longue attente indique un blocage inattendu. Il faut examiner la tâche secondaire.|  
|DUMPTRIGGER|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|EC|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|EE_PMOLOCK|Se produit durant la synchronisation de certains types d'allocations de mémoire durant l'exécution d'une instruction.|  
|EE_SPECPROC_MAP_INIT|Se produit durant la synchronisation de la création de la table de hachage de procédure interne. Cette attente ne peut se produire que lors de l'accès initial à la table de hachage après le démarrage de l'instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|ENABLE_VERSIONING|Se produit lorsque [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] attend que toutes les transactions de mise à jour de la base de données soient terminées avant de déclarer que la base de données est prête à passer à l'état autorisé d'isolement d'instantané. Cet état est utilisé lorsque [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] active l'isolement d'instantané à l'aide de l'instruction ALTER DATABASE.|  
|ERROR_REPORTING_MANAGER|Se produit durant la synchronisation de plusieurs initialisations simultanées de journaux d'erreurs.|  
|EXCHANGE|Se produit durant la synchronisation dans l'itérateur d'échange du processeur de requêtes au cours de requêtes parallèles.|  
|EXECSYNC|Se produit au cours de requêtes parallèles, lors de la synchronisation dans le processeur de requêtes, dans des zones qui ne sont pas liées à l'itérateur d'échange. Ces zones sont par exemple des images bitmap, des objets binaires volumineux et l'itérateur de spouleurs. Les objets LOB peuvent utiliser souvent cet état d'attente.|  
|EXECUTION_PIPE_EVENT_INTERNAL|Se produit durant la synchronisation entre les parties producteur et consommateur d'exécution de lot soumises via le contexte de connexion.|  
|FAILPOINT|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|FCB_REPLICA_READ|Se produit lorsque les lectures du fichier partiellement alloué d'un instantané (ou d'un instantané temporaire créée par DBCC) sont synchronisées.|  
|FCB_REPLICA_WRITE|Se produit lorsque des émissions ou extractions d'une page dans un fichier partiellement alloué d'instantané (ou un instantané temporaire créé par DBCC) sont synchronisées.|  
|FS_FC_RWLOCK|Se produit lorsqu'il y a une attente par le garbage collector FILESTREAM pour effectuer l'une des opérations suivantes :<br /><br /> Désactivation du garbage collection (utilisé par la sauvegarde et la restauration).<br /><br /> Exécution d'un cycle du garbage collector FILESTREAM.|  
|FS_GARBAGE_COLLECTOR_SHUTDOWN|Se produit lorsque le garbage collector FILESTREAM attend l'exécution de tâches de nettoyage.|  
|FS_HEADER_RWLOCK|Se produit lorsqu'il y a une attente d'obtention de l'accès à l'en-tête FILESTREAM d'un conteneur de données FILESTREAM pour la lecture ou la mise à jour du contenu du fichier d'en-tête FILESTREAM (Filestream.hdr).|  
|FS_LOGTRUNC_RWLOCK|Se produit lorsqu'il y a une attente d'obtention de l'accès à la troncation de journaux FILESTREAM pour effectuer l'une des opérations suivantes :<br /><br /> Désactivation temporaire de la troncation (FSLOG) de journaux FILESTREAM (utilisée par la sauvegarde et la restauration).<br /><br /> Exécution d'un cycle de troncation FSLOG.|  
|FSA_FORCE_OWN_XACT|Se produit lorsqu'une opération d'E/S de fichier FILESTREAM doit se lier à la transaction associée, alors que la transaction appartient actuellement à une autre session.|  
|FSAGENT|Se produit lorsqu'une opération d'E/S de fichier FILESTREAM attend une ressource de l'agent FILESTREAM utilisée par une autre opération d'E/S de fichier.|  
|FSTR_CONFIG_MUTEX|Se produit lorsqu'il y a une attente d'une autre reconfiguration de fonctionnalité FILESTREAM.|  
|FSTR_CONFIG_RWLOCK|Se produit lorsqu'il y a une attente de sérialisation de l'accès aux paramètres de configuration FILESTREAM.|  
|FT_METADATA_MUTEX|Documenté à titre d'information uniquement. Non pris en charge. La compatibilité future n'est pas garantie.|  
|FT_RESTART_CRAWL|Se produit lorsqu'une analyse de texte intégral doit redémarrer à partir du dernier point de référence connu et fiable pour une récupération faisant suite à une panne transitoire. Grâce à cette attente, les tâches travaillant actuellement sur ce remplissage peuvent se terminer ou quitter l'étape en cours.|  
|FULLTEXT GATHERER|Se produit durant la synchronisation d'opérations de texte intégral.|  
|GUARDIAN|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|HTTP_ENUMERATION|Se produit au démarrage pour énumérer les points de terminaison HTTP pour lancer HTTP.|  
|HTTP_START|Se produit lorsqu'une connexion attend que le protocole HTTP termine l'initialisation.|  
|IMPPROV_IOWAIT|Se produit lorsque [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] attend la fin d'une E/S de chargement en masse.|  
|INTERNAL_TESTING|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|IO_AUDIT_MUTEX|Se produit durant la synchronisation des mémoires tampons d'événements de trace.|  
|IO_COMPLETION|Se produit durant l'attente de l'exécution des opérations d'E/S. Ce type d'attente représente en général des entrées/sorties de page qui ne sont pas des données. Les attentes d'exécution des entrées/sorties de pages de données apparaissent sous la forme d'attentes PAGEIOLATCH_*.|  
|IO_QUEUE_LIMIT|Se produit quand la file d'attente des E/S asynchrones pour la [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] a un trop grand nombre d'E/S en attente. Les tâches qui essaient d'émettre une autre E/S sont bloquées sur ce type d'attente jusqu'à ce que le nombre d'E/S en attente passe en dessous du seuil. Le seuil est proportionnel aux DTU affectées à la base de données.|  
|IO_RETRY|Se produit lorsqu'une opération d'E/S telle qu'une lecture ou une écriture sur disque échoue en raison de ressources insuffisantes, puis est retentée.|  
|IOAFF_RANGE_QUEUE|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|KSOURCE_WAKEUP|Utilisé par la tâche de contrôle de service pour les demandes émanant du Gestionnaire de contrôle des services. De longues attentes sont prévisibles, et elles n'indiquent pas la présence d'un problème.|  
|KTM_ENLISTMENT|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|KTM_RECOVERY_MANAGER|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|KTM_RECOVERY_RESOLUTION|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|LATCH_DT|Se produit pendant l'attente d'un verrou en mode de destruction. Cela n'inclut pas les verrous internes de tampons ni les verrous internes de marque de transaction. La liste des attentes LATCH_* est disponible dans la section sys.dm_os_latch_stats. Notez que sys.dm_os_latch_stats regroupe les attentes LATCH_NL, LATCH_SH, LATCH_UP, LATCH_EX et LATCH_DT.|  
|LATCH_EX|Se produit pendant l'attente d'un verrou exclusif. Cela n'inclut pas les verrous internes de tampons ni les verrous internes de marque de transaction. La liste des attentes LATCH_* est disponible dans la section sys.dm_os_latch_stats. Notez que sys.dm_os_latch_stats regroupe les attentes LATCH_NL, LATCH_SH, LATCH_UP, LATCH_EX et LATCH_DT.|  
|LATCH_KP|Se produit pendant l'attente d'un verrou de maintien. Cela n'inclut pas les verrous internes de tampons ni les verrous internes de marque de transaction. La liste des attentes LATCH_* est disponible dans la section sys.dm_os_latch_stats. Notez que sys.dm_os_latch_stats regroupe les attentes LATCH_NL, LATCH_SH, LATCH_UP, LATCH_EX et LATCH_DT.|  
|LATCH_NL|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|LATCH_SH|Se produit pendant l'attente d'un verrou de partage. Cela n'inclut pas les verrous internes de tampons ni les verrous internes de marque de transaction. La liste des attentes LATCH_* est disponible dans la section sys.dm_os_latch_stats. Notez que sys.dm_os_latch_stats regroupe les attentes LATCH_NL, LATCH_SH, LATCH_UP, LATCH_EX et LATCH_DT.|  
|LATCH_UP|Se produit pendant l'attente d'un verrou de mise à jour. Cela n'inclut pas les verrous internes de tampons ni les verrous internes de marque de transaction. La liste des attentes LATCH_* est disponible dans la section sys.dm_os_latch_stats. Notez que sys.dm_os_latch_stats regroupe les attentes LATCH_NL, LATCH_SH, LATCH_UP, LATCH_EX et LATCH_DT.|  
|LAZYWRITER_SLEEP|Se produit lorsque des tâches d'écriture différée sont suspendues. Il s'agit d'une mesure de la durée consacrée aux tâches en arrière-plan qui attendent. Ne considérez pas cet état lorsque vous cherchez des blocages d'utilisateur.|  
|LCK_M_BU|Se produit lorsqu'une tâche attend pour acquérir un verrou de mise à jour en bloc. Pour obtenir une matrice de compatibilité des verrous, consultez [sys.dm_tran_locks &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md).|  
|LCK_M_IS|Se produit lorsqu'une tâche attend pour acquérir un verrou intentionnel partagé. Pour obtenir une matrice de compatibilité des verrous, consultez [sys.dm_tran_locks &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md).|  
|LCK_M_IU|Se produit lorsqu'une tâche attend pour acquérir un verrou intentionnel de mise à jour. Pour obtenir une matrice de compatibilité des verrous, consultez [sys.dm_tran_locks &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md).|  
|LCK_M_IX|Se produit lorsqu'une tâche attend pour acquérir un verrou intentionnel exclusif. Pour obtenir une matrice de compatibilité des verrous, consultez [sys.dm_tran_locks &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md).|  
|LCK_M_RIn_NL|Se produit lorsqu'une tâche attend pour acquérir un verrou NULL sur la valeur de clé actuelle, et un verrou de groupes d'insertions entre la clé actuelle et la clé précédente. Un verrou NULL sur la clé est un verrou de libération instantanée. Pour obtenir une matrice de compatibilité des verrous, consultez [sys.dm_tran_locks &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md).|  
|LCK_M_RIn_S|Se produit lorsqu'une tâche attend pour acquérir un verrou partagé sur la valeur de clé actuelle, et un verrou de groupes d'insertions entre la clé actuelle et la clé précédente. Pour obtenir une matrice de compatibilité des verrous, consultez [sys.dm_tran_locks &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md).|  
|LCK_M_RIn_U|La tâche attend pour acquérir un verrou de mise à jour sur la valeur de clé actuelle, et un verrou de groupes d'insertions entre la clé actuelle et la clé précédente. Pour obtenir une matrice de compatibilité des verrous, consultez [sys.dm_tran_locks &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md).|  
|LCK_M_RIn_X|Se produit lorsqu'une tâche attend pour acquérir un verrou exclusif sur la valeur de clé actuelle, et un verrou de groupes d'insertions entre la clé actuelle et la clé précédente. Pour obtenir une matrice de compatibilité des verrous, consultez [sys.dm_tran_locks &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md).|  
|LCK_M_RS_S|Se produit lorsqu'une tâche attend pour acquérir un verrou partagé sur la valeur de clé actuelle, et un verrou de groupes partagés entre la clé actuelle et la clé précédente. Pour obtenir une matrice de compatibilité des verrous, consultez [sys.dm_tran_locks &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md).|  
|LCK_M_RS_U|Se produit lorsqu'une tâche attend pour acquérir un verrou de mise à jour sur la valeur de clé actuelle, et un verrou de groupes de mises à jour entre la clé actuelle et la clé précédente. Pour obtenir une matrice de compatibilité des verrous, consultez [sys.dm_tran_locks &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md).|  
|LCK_M_RX_S|Se produit lorsqu'une tâche attend pour acquérir un verrou partagé sur la valeur de clé actuelle, et un verrou de groupes exclusifs entre la clé actuelle et la clé précédente. Pour obtenir une matrice de compatibilité des verrous, consultez [sys.dm_tran_locks &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md).|  
|LCK_M_RX_U|Se produit lorsqu'une tâche attend pour acquérir un verrou de mise à jour sur la valeur de clé actuelle, et un verrou de groupes exclusifs entre la clé actuelle et la clé précédente. Pour obtenir une matrice de compatibilité des verrous, consultez [sys.dm_tran_locks &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md).|  
|LCK_M_RX_X|Se produit lorsqu'une tâche attend pour acquérir un verrou exclusif sur la valeur de clé actuelle, et un verrou de groupes exclusifs entre la clé actuelle et la clé précédente. Pour obtenir une matrice de compatibilité des verrous, consultez [sys.dm_tran_locks &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md).|  
|LCK_M_S|Se produit lorsqu'une tâche attend pour acquérir un verrou partagé. Pour obtenir une matrice de compatibilité des verrous, consultez [sys.dm_tran_locks &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md).|  
|LCK_M_SCH_M|Se produit lorsqu'une tâche attend pour acquérir un verrou de modification de schéma. Pour obtenir une matrice de compatibilité des verrous, consultez [sys.dm_tran_locks &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md).|  
|LCK_M_SCH_S|Se produit lorsqu'une tâche attend pour acquérir un verrou de partage de schéma. Pour obtenir une matrice de compatibilité des verrous, consultez [sys.dm_tran_locks &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md).|  
|LCK_M_SIU|Se produit lorsqu'une tâche attend pour acquérir un verrou intentionnel de mise à jour partagé. Pour obtenir une matrice de compatibilité des verrous, consultez [sys.dm_tran_locks &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md).|  
|LCK_M_SIX|Se produit lorsqu'une tâche attend pour acquérir un verrou intentionnel exclusif partagé. Pour obtenir une matrice de compatibilité des verrous, consultez [sys.dm_tran_locks &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md).|  
|LCK_M_U|Se produit lorsqu'une tâche attend pour acquérir un verrou de mise à jour. Pour obtenir une matrice de compatibilité des verrous, consultez [sys.dm_tran_locks &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md).|  
|LCK_M_UIX|Se produit lorsqu'une tâche attend pour acquérir un verrou intentionnel exclusif de mise à jour. Pour obtenir une matrice de compatibilité des verrous, consultez [sys.dm_tran_locks &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md).|  
|LCK_M_X|Se produit lorsqu'une tâche attend pour acquérir un verrou exclusif. Pour obtenir une matrice de compatibilité des verrous, consultez [sys.dm_tran_locks &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md).|  
|LOG_RATE_GOVERNOR|Se produit lorsque la base de données attend des quotas à écrire dans le journal.|  
|LOGBUFFER|Se produit lorsqu'une tâche attend de l'espace dans le tampon de journal pour stocker un enregistrement de journal. Des valeurs élevées fréquentes peuvent indiquer que les unités de journaux ne parviennent pas à gérer la quantité d'entrées de journaux produites par le serveur.|  
|LOGGENERATION|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|LOGMGR|Se produit lorsqu'une tâche attend la fin des E/S de journal en cours avant d'arrêter le journal lors de la fermeture de la base de données.|  
|LOGMGR_FLUSH|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|LOGMGR_QUEUE|Se produit lorsque la tâche d'écriture du journal attend des demandes de travail.|  
|LOGMGR_RESERVE_APPEND|Se produit lorsqu'une tâche attend de voir si la troncature du journal libère de l'espace pour lui permettre d'écrire un nouvel enregistrement dans le journal. Vous pouvez éventuellement augmenter la taille des fichiers journaux correspondant à la base de données concernée pour réduire cette attente.|  
|LOWFAIL_MEMMGR_QUEUE|Se produit lorsque vous attendez que la mémoire soit disponible afin d'être utilisée.|  
|MSQL_DQ|Se produit lorsqu'une tâche attend la fin d'une opération de requête distribuée. Permet de détecter d'éventuels interblocages d'application MARS (Multiple Active Result Set). L'attente se termine à la fin de l'appel de requête distribuée.|  
|MSQL_XACT_MGR_MUTEX|Se produit lorsqu'une tâche attend d'avoir obtenu la propriété du gestionnaire de transactions de la session pour effectuer une opération de transaction au niveau de la session.|  
|MSQL_XACT_MUTEX|Se produit durant la synchronisation de l'utilisation de la transaction. Une demande doit d'abord obtenir l'exclusion mutuelle pour pouvoir utiliser la transaction.|  
|MSQL_XP|Se produit lorsqu'une tâche attend la fin d'une procédure stockée étendue. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilise cet état d'attente pour détecter d'éventuels blocages d'application MARS. L'attente se termine à la fin de l'appel de procédure stockée étendue.|  
|MSSEARCH|Se produit durant des appels de recherche en texte intégral. Cette attente se termine lorsque l'opération de texte intégral prend fin. Ces informations n'indiquent pas des contentions, mais plutôt la durée des opérations de texte intégral.|  
|NET_WAITFOR_PACKET|Se produit lorsqu'une connexion attend un paquet réseau durant une lecture sur le réseau.|  
|OLEDB|Se produit lorsque [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] appelle le fournisseur OLE DB [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client. Ce type d'attente n'est pas utilisé pour la synchronisation. Par contre, il indique la durée des appels émis vers le fournisseur OLE DB.|  
|ONDEMAND_TASK_QUEUE|Se produit lorsqu'une tâche en arrière-plan attend des demandes de tâches système à priorité élevée. De longues attentes indiquent qu'il n'existe aucune demande à priorité élevée à traiter, et elles ne révèlent pas l'existence d'un problème.|  
|PAGEIOLATCH_DT|Se produit lorsqu'une tâche attend sur un verrou interne un tampon qui est une demande d'E/S. La demande de verrou interne est en mode de destruction. De longues attentes peuvent indiquer l'existence de problèmes au niveau du sous-système de disque.|  
|PAGEIOLATCH_EX|Se produit lorsqu'une tâche attend sur un verrou interne un tampon qui est une demande d'E/S. La demande de verrou interne est en mode exclusif. De longues attentes peuvent indiquer l'existence de problèmes au niveau du sous-système de disque.|  
|PAGEIOLATCH_KP|Se produit lorsqu'une tâche attend sur un verrou interne un tampon qui est une demande d'E/S. La demande de verrou interne est en mode de conservation. De longues attentes peuvent indiquer l'existence de problèmes au niveau du sous-système de disque.|  
|PAGEIOLATCH_NL|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|PAGEIOLATCH_SH|Se produit lorsqu'une tâche attend sur un verrou interne un tampon qui est une demande d'E/S. La demande de verrou interne est en mode partagé. De longues attentes peuvent indiquer l'existence de problèmes au niveau du sous-système de disque.|  
|PAGEIOLATCH_UP|Se produit lorsqu'une tâche attend sur un verrou interne un tampon qui est une demande d'E/S. La demande de verrou interne est en mode de mise à jour. De longues attentes peuvent indiquer l'existence de problèmes au niveau du sous-système de disque.|  
|PAGELATCH_DT|Se produit lorsqu'une tâche attend sur un verrou interne un tampon qui n'est pas une demande d'E/S. La demande de verrou interne est en mode de destruction.|  
|PAGELATCH_EX|Se produit lorsqu'une tâche attend sur un verrou interne un tampon qui n'est pas une demande d'E/S. La demande de verrou interne est en mode exclusif.|  
|PAGELATCH_KP|Se produit lorsqu'une tâche attend sur un verrou interne un tampon qui n'est pas une demande d'E/S. La demande de verrou interne est en mode de conservation.|  
|PAGELATCH_NL|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|PAGELATCH_SH|Se produit lorsqu'une tâche attend sur un verrou interne un tampon qui n'est pas une demande d'E/S. La demande de verrou interne est en mode partagé.|  
|PAGELATCH_UP|Se produit lorsqu'une tâche attend sur un verrou interne un tampon qui n'est pas une demande d'E/S. La demande de verrou interne est en mode de mise à jour.|  
|PARALLEL_BACKUP_QUEUE|Se produit lors de la sérialisation de la sortie générée par RESTORE HEADERONLY, RESTORE FILELISTONLY ou RESTORE LABELONLY.|  
|PREEMPTIVE_ABR|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|PREEMPTIVE_AUDIT_ACCESS_EVENTLOG|Se produit lorsque le planificateur du système d'exploitation [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (SQLOS) bascule en mode préemptif pour écrire un événement d'audit dans le journal des événements Windows.|  
|PREEMPTIVE_AUDIT_ACCESS_SECLOG|Se produit lorsque le planificateur du système d'exploitation (SQLOS) bascule en mode préemptif pour écrire un événement d'audit dans le journal de sécurité Windows.|  
|PREEMPTIVE_CLOSEBACKUPMEDIA|Se produit lorsque le planificateur du système d'exploitation (SQLOS) bascule en mode préemptif pour fermer le support de sauvegarde.|  
|PREEMPTIVE_CLOSEBACKUPTAPE|Se produit lorsque le planificateur du système d'exploitation (SQLOS) bascule en mode préemptif pour fermer une unité de sauvegarde sur bande.|  
|PREEMPTIVE_CLOSEBACKUPVDIDEVICE|Se produit lorsque le planificateur du système d'exploitation (SQLOS) bascule en mode préemptif pour fermer une unité de sauvegarde virtuelle.|  
|PREEMPTIVE_CLUSAPI_CLUSTERRESOURCECONTROL|Se produit lorsque le planificateur du système d'exploitation (SQLOS) bascule en mode préemptif pour effectuer des opérations de cluster de basculement Windows.|  
|PREEMPTIVE_COM_COCREATEINSTANCE|Se produit lorsque le planificateur du système d'exploitation (SQLOS) bascule en mode préemptif pour créer un objet COM.|  
|PREEMPTIVE_HADR_LEASE_MECHANISM|Always On la planification du gestionnaire de bail du groupe de disponibilité pour les diagnostics CSS.|  
|PREEMPTIVE_SOSTESTING|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|PREEMPTIVE_STRESSDRIVER|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|PREEMPTIVE_TESTING|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|PREEMPTIVE_XETESTING|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|PRINT_ROLLBACK_PROGRESS|Utilisé pour attendre que des processus utilisateur se terminent dans une base de données qui a subi un changement d'état suite à l'utilisation de la clause de terminaison ALTER DATABASE. Pour plus d’informations, consultez [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md).|  
|PWAIT_HADR_CHANGE_NOTIFIER_TERMINATION_SYNC|Se produit lorsqu'une tâche en arrière-plan attend la fin de la tâche en arrière-plan qui reçoit (par interrogation) des notifications de clustering de basculement Windows Server.  À usage interne uniquement|  
|PWAIT_HADR_CLUSTER_INTEGRATION|Une opération d’ajout, de remplacement et/ou de suppression attend de saisir un verrou d’écriture sur une liste interne Always On (par exemple, une liste de réseaux, d’adresses réseau ou d’écouteurs de groupe de disponibilité).  À usage interne uniquement|  
|PWAIT_HADR_OFFLINE_COMPLETED|Une opération Drop Availability Group Always On attend que le groupe de disponibilité cible se déconnecte avant de détruire les objets de clustering de basculement Windows Server.|  
|PWAIT_HADR_ONLINE_COMPLETED|Une opération de création ou de basculement de groupe de disponibilité Always On attend que le groupe de disponibilité cible soit mis en ligne.|  
|PWAIT_HADR_POST_ONLINE_COMPLETED|Une opération Drop Availability Group Always On attend l’arrêt d’une tâche en arrière-plan qui était planifiée dans le cadre d’une commande précédente. Par exemple, une tâche en arrière-plan peut effectuer la transition de bases de données de disponibilité vers le rôle principal. La DLL DROP AVAILABILITY GROUP doit attendre la fin de cette tâche en arrière-plan afin d'éviter des conditions de concurrence.|  
|PWAIT_HADR_WORKITEM_COMPLETED|Attente interne par un thread attendant la fin d'une tâche de travail asynchrone. Il s'agit d'une attente prévue, à l'usage de CSS.|  
|PWAIT_MD_LOGIN_STATS|Se produit durant la synchronisation interne dans les métadonnées des statistiques de connexion.|  
|PWAIT_MD_RELATION_CACHE|Se produit durant la synchronisation interne dans les métadonnées de la table ou de l'index.|  
|PWAIT_MD_SERVER_CACHE|Se produit durant la synchronisation interne dans les métadonnées sur les serveurs liés.|  
|PWAIT_MD_UPGRADE_CONFIG|Se produit durant la synchronisation interne lors de la mise à niveau des configurations à l'échelle du serveur.|  
|PWAIT_METADATA_LAZYCACHE_RWLOCk|Se produit durant la synchronisation interne dans le cache de métadonnées avec l'index d'itération ou les statistiques dans une table.|  
|QPJOB_KILL|Indique qu'une mise à jour asynchrone des statistiques automatiques a été annulée par un appel à KILL alors que la mise à jour commençait. Le thread de terminaison est suspendu, en attendant qu'il commence à écouter les commandes KILL. Une valeur idéale est inférieure à une seconde.|  
|QPJOB_WAITFOR_ABORT|Indique qu'une mise à jour asynchrone des statistiques automatiques a été annulée par un appel à KILL lors de son exécution. La mise à jour est maintenant terminée, mais elle est suspendue jusqu'à ce que la coordination du message de fin de thread soit achevée. Il s'agit d'un état ordinaire, mais rare, qui doit être très bref. Une valeur idéale est inférieure à une seconde.|  
|QRY_MEM_GRANT_INFO_MUTEX|Se produit lorsque la gestion de la mémoire pour l'exécution des requêtes essaie de contrôler l'accès à la liste d'informations d'octroi statique. Cet état énumère les informations sur les demandes de mémoire actuellement accordées et en attente. Il s'agit d'un simple état de contrôle d'accès. Il ne devrait jamais y avoir de longue attente sur cet état. Si ce mutex n'est pas libéré, toutes les nouvelles requêtes d'utilisation de mémoire cesseront de répondre.|  
|QUERY_ERRHDL_SERVICE_DONE|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|QUERY_EXECUTION_INDEX_SORT_EVENT_OPEN|Se produit dans certains cas lorsqu'une construction d'index hors connexion est exécutée en parallèle, et les différents threads de travail qui effectuent le tri synchronisent l'accès aux fichiers de tri.|  
|QUERY_NOTIFICATION_MGR_MUTEX|Se produit durant la synchronisation de la file d'attente de nettoyage de la mémoire dans le gestionnaire de notification de requête.|  
|QUERY_NOTIFICATION_SUBSCRIPTION_MUTEX|Se produit durant la synchronisation de l'état des transactions dans les notifications de requêtes.|  
|QUERY_NOTIFICATION_TABLE_MGR_MUTEX|Se produit durant la synchronisation interne dans le gestionnaire de notification de requête.|  
|QUERY_NOTIFICATION_UNITTEST_MUTEX|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|QUERY_OPTIMIZER_PRINT_MUTEX|Se produit durant la synchronisation de la production de la sortie de diagnostics de l'optimiseur de requête. Ce type d'attente intervient si les paramètres de diagnostic ont été activés sous la direction du Support technique [!INCLUDE[msCoName](../../includes/msconame-md.md)].|  
|QUERY_TRACEOUT|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|QUERY_WAIT_ERRHDL_SERVICE|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|RECOVER_CHANGEDB|Se produit durant la synchronisation de l'état de la base de données dans une base de données en mode secours semi-automatique.|  
|REPL_CACHE_ACCESS|Se produit durant la synchronisation sur un cache des articles de réplication. Lors de ces attentes, l'utilitaire de lecture du journal des réplications se bloque et les instructions de langage de définition de données (DDL - Data Definition Language) sur une table publiée sont bloquées.|  
|REPL_SCHEMA_ACCESS|Se produit durant la synchronisation des informations de version du schéma de réplication. Cet état existe lorsque des instructions DDL sont exécutées sur l'objet répliqué et lorsque l'utilitaire de lecture du journal crée ou exploite le schéma avec version reposant sur une occurrence de DDL.|  
|REPLICA_WRITES|Se produit lorsqu'une tâche attend la fin des écritures de page dans des instantanés de base de données ou des réplicas DBCC.|  
|REQUEST_DISPENSER_PAUSE|Se produit lorsqu'une tâche attend la fin de toutes les E/S en suspens, afin que les E/S puissent être figées dans un fichier pour une sauvegarde des instantanés.|  
|REQUEST_FOR_DEADLOCK_SEARCH|Se produit lorsque l'analyseur de blocages attend pour démarrer la recherche de blocage suivante. Cette attente est normale entre les détections de blocages, et une importante durée d'attente totale sur cette ressource n'indique pas la présence d'un problème.|  
|RESMGR_THROTTLED|Se produit lorsqu'une nouvelle demande arrive et qu'elle est accélérée en fonction du paramètre GROUP_MAX_REQUESTS.|  
|RESOURCE_QUEUE|Se produit durant la synchronisation de diverses files d'attente de ressources internes.|  
|RESOURCE_SEMAPHORE|Se produit lorsqu'une demande de mémoire de requête ne peut pas être accordée immédiatement en raison d'autres requêtes simultanées. Des temps d'attente élevés peuvent indiquer un trop grand nombre de requêtes simultanées ou des quantités de demande de mémoire trop importantes.|  
|RESOURCE_SEMAPHORE_MUTEX|Se produit lorsqu'une requête attend que sa demande de réservation de thread soit exécutée. Se produit également lors de la synchronisation des demandes d'allocation de mémoire et de compilation de requête.|  
|RESOURCE_SEMAPHORE_QUERY_COMPILE|Se produit lorsque le nombre de compilations de requêtes simultanées atteint une limite. Des temps d'attente élevés peuvent indiquer des compilations et des recompilations excessives, ou encore des plans ne pouvant pas être mis en cache.|  
|RESOURCE_SEMAPHORE_SMALL_QUERY|Se produit lorsqu'une demande de mémoire émanant d'une petite requête ne peut pas être accordée immédiatement en raison d'autres requêtes simultanées. Le temps d'attente ne doit pas excéder quelques secondes, car le serveur transfère la demande au pool de mémoire de requête principal s'il ne parvient pas à accorder la mémoire demandée dans les secondes qui suivent. Des temps d'attente élevés peuvent indiquer un nombre excessif de petites requêtes simultanées alors que le pool de mémoire principal est bloqué par les requêtes en attente.|  
|SE_REPL_CATCHUP_THROTTLE|Se produit lorsque la transaction attend la progression de l'un des serveurs de base de données secondaires.|  
|SE_REPL_COMMIT_ACK|Se produit lorsque la transaction attend un accusé de réception de la validation du quorum de la part des réplicas secondaires.|  
|SE_REPL_COMMIT_TURN|Se produit lorsque la transaction attend la validation après avoir reçu les accusés de réception de la validation du quorum.|  
|SE_REPL_ROLLBACK_ACK|Se produit lorsque la transaction attend un accusé de réception de la restauration du quorum de la part des réplicas secondaires.|  
|SE_REPL_SLOW_SECONDARY_THROTTLE|Se produit lorsqu'un thread attend un des réplicas de base de données secondaires.|  
|SEC_DROP_TEMP_KEY|Se produit après l'échec d'une tentative de suppression d'une clé de sécurité temporaire avant une nouvelle tentative.|  
|SECURITY_MUTEX|Se produit lorsqu'il y a une attente de mutex qui contrôlent l'accès à la liste globale de fournisseurs de services de chiffrement EKM (Gestion de clés extensible) et la liste au niveau de la session des sessions EKM.|  
|SEQUENTIAL_GUID|Se produit lorsqu'un nouveau GUID séquentiel est obtenu.|  
|SERVER_IDLE_CHECK|Se produit durant la synchronisation de l'état d'inactivité d'une instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] lorsqu'un moniteur de ressources essaie de déclarer une instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] comme étant inactive ou sur le point de se réactiver.|  
|SHUTDOWN|Se produit lorsqu'une instruction d'arrêt attend que les connexions actives soient coupées.|  
|SLEEP_BPOOL_FLUSH|Se produit lorsqu'un point de vérification limite l'émission des nouvelles E/S afin de ne pas saturer le sous-système de disque.|  
|SLEEP_DBSTARTUP|Se produit au démarrage de la base de données lors de l'attente de la récupération de toutes les bases de données.|  
|SLEEP_DCOMSTARTUP|Se produit une fois au maximum durant le démarrage d'une instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en attendant la fin de l'initialisation DCOM.|  
|SLEEP_MSDBSTARTUP|Se produit lorsque Trace SQL attend la fin du démarrage de la base de données msdb.|  
|SLEEP_SYSTEMTASK|Se produit lors du démarrage d'une tâche en arrière-plan pendant l'attente du démarrage de tempdb.|  
|SLEEP_TASK|Se produit lorsqu'une tâche est en état de veille en attendant qu'un événement générique survienne.|  
|SLEEP_TEMPDBSTARTUP|Se produit lorsqu'une tâche attend la fin du démarrage de tempdb.|  
|SNI_CRITICAL_SECTION|Se produit durant la synchronisation interne au sein des composants réseau de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|SNI_HTTP_WAITFOR_0_DISCON|Se produit durant l'arrêt de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], pendant l'attente de la fermeture des connexions HTTP en suspens.|  
|SNI_LISTENER_ACCESS|Se produit pendant l'attente de mise à jour de la modification d'état des nœuds NUMA (Non-Uniform Memory Access). L'accès à la modification d'état est sérialisé.|  
|SNI_TASK_COMPLETION|Se produit lorsqu'il y a une attente de fin de toutes les tâches pendant la modification d'état d'un nœud NUMA.|  
|SOAP_READ|Se produit durant l'attente de l'exécution d'une lecture sur le réseau HTTP.|  
|SOAP_WRITE|Se produit durant l'attente de l'exécution d'une écriture sur le réseau HTTP.|  
|SOS_CALLBACK_REMOVAL|Se produit lors de la synchronisation sur une liste de rappels afin de supprimer un rappel. Ce compteur ne change pas à la fin de l'initialisation du serveur.|  
|SOS_DISPATCHER_MUTEX|Se produit durant la synchronisation interne du pool de répartiteurs. Cela inclut l'ajustement du pool.|  
|SOS_LOCALALLOCATORLIST|Se produit durant la synchronisation interne dans le gestionnaire de mémoire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|SOS_MEMORY_USAGE_ADJUSTMENT|Se produit lorsque l'utilisation de la mémoire est répartie entre des pools.|  
|SOS_OBJECT_STORE_DESTROY_MUTEX|Se produit durant la synchronisation interne dans les pools de mémoire lorsque des objets du pool sont détruits.|  
|SOS_PROCESS_AFFINITY_MUTEX|Se produit durant la synchronisation de l'accès pour traiter les paramètres d'affinité.|  
|SOS_RESERVEDMEMBLOCKLIST|Se produit durant la synchronisation interne dans le gestionnaire de mémoire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|SOS_SCHEDULER_YIELD|Se produit lorsqu'une tâche abandonne volontairement le planificateur pour d'autres tâches à exécuter. Durant cette attente, la tâche attend le renouvellement de son quantum.|  
|SOS_SMALL_PAGE_ALLOC|Se produit pendant l'allocation et la libération de la mémoire gérée par quelques objets mémoire.|  
|SOS_STACKSTORE_INIT_MUTEX|Se produit durant la synchronisation de l'initialisation de stockage interne.|  
|SOS_SYNC_TASK_ENQUEUE_EVENT|Se produit lorsqu'une tâche est démarrée de manière synchrone. La plupart des tâches dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sont démarrées de manière asynchrone ; c'est-à-dire que le contrôle est renvoyé à l'élément initial dès que la tâche a été placée dans la file d'attente de travail.|  
|SOS_VIRTUALMEMORY_LOW|Se produit lorsqu'une allocation de mémoire attend qu'un gestionnaire de ressources libère de la mémoire virtuelle.|  
|SOSHOST_EVENT|Se produit lorsqu'un composant hébergé, tel que CLR, attend sur un objet de synchronisation d'événement [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|SOSHOST_INTERNAL|Se produit durant la synchronisation des rappels du gestionnaire de mémoire utilisés par des composants hébergés, tels que CLR.|  
|SOSHOST_MUTEX|Se produit lorsqu'un composant hébergé, tel que CLR, attend sur un objet de synchronisation d'exclusion mutuelle (mutex) [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|SOSHOST_RWLOCK|Se produit lorsqu'un composant hébergé, tel que CLR, attend sur un objet de synchronisation de lecture/écriture [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|SOSHOST_SEMAPHORE|Se produit lorsqu'un composant hébergé, tel que CLR, attend sur un objet de synchronisation de sémaphore [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|SOSHOST_SLEEP|Se produit lorsqu'une tâche hébergée est en veille en attendant qu'un événement générique survienne. Les tâches hébergées sont utilisées par les composants hébergés tels que CLR.|  
|SOSHOST_TRACELOCK|Se produit durant la synchronisation de l'accès aux flux de trace.|  
|SOSHOST_WAITFORDONE|Se produit lorsqu'un composant hébergé, tel que CLR, attend la fin d'une tâche.|  
|SQLCLR_APPDOMAIN|Se produit lorsque CLR attend la fin du démarrage d'un domaine d'application.|  
|SQLCLR_ASSEMBLY|Se produit durant l'attente de l'accès à la liste des assembly chargés dans le domaine d'application.|  
|SQLCLR_DEADLOCK_DETECTION|Se produit lorsque CLR attend la fin d'une détection d'interblocage.|  
|SQLCLR_QUANTUM_PUNISHMENT|Se produit lorsqu'une tâche CLR est accélérée car elle a dépassé son quantum d'exécution. Cette accélération est opérée afin de limiter l'incidence de cette tâche consommant une grande quantité de ressources sur les autres tâches.|  
|SQLSORT_NORMMUTEX|Se produit durant la synchronisation interne, lors de l'initialisation des structures de tri internes.|  
|SQLSORT_SORTMUTEX|Se produit durant la synchronisation interne, lors de l'initialisation des structures de tri internes.|  
|SQLTRACE_BUFFER_FLUSH|Se produit lorsqu'une tâche attend qu'une tâche en arrière-plan vide les tampons de traçage sur le disque toutes les quatre secondes.|  
|SQLTRACE_LOCK|Se produit durant la synchronisation des tampons de trace lors d'un suivi de fichier.|  
|SQLTRACE_SHUTDOWN|Se produit pendant que l'arrêt de la trace attend la fin des événements de trace en suspens.|  
|SQLTRACE_WAIT_ENTRIES|Se produit lorsqu'une file d'attente d'événements Trace SQL attend l'arrivée de paquets dans la file d'attente.|  
|SRVPROC_SHUTDOWN|Se produit lorsque le processus d'arrêt attend la libération des ressources internes pour que l'arrêt s'effectue correctement.|  
|TEMPOBJ|Se produit lorsque des suppressions d'objets temporaires sont synchronisées. Cette attente est rare ; elle survient uniquement si une tâche a demandé un accès exclusif pour les suppressions de tables temp.|  
|THREADPOOL|Se produit lorsqu'une tâche attend un thread de travail pour l'exécution. Ceci peut indiquer qu'un paramètre de thread de travail maximal est trop bas ou que les exécutions de traitement sont exceptionnellement longues, ce qui réduit le nombre de threads de travail disponibles pour répondre aux autres lots.|  
|TIMEPRIV_TIMEPERIOD|Se produit durant la synchronisation interne du minuteur d'événements étendus.|  
|TRACEWRITE|Se produit lorsque le fournisseur de traces d'ensemble de lignes Trace SQL attend le traitement d'un tampon libre ou d'un tampon avec des événements.|  
|TRAN_MARKLATCH_DT|Se produit lors de l'attente d'un verrou en mode de destruction sur un verrou interne de marque de transaction. Les verrous internes de marque de transaction sont utilisés pour la synchronisation des validations avec des transactions marquées.|  
|TRAN_MARKLATCH_EX|Se produit lors de l'attente d'un verrou en mode exclusif sur une transaction marquée. Les verrous internes de marque de transaction sont utilisés pour la synchronisation des validations avec des transactions marquées.|  
|TRAN_MARKLATCH_KP|Se produit lors de l'attente d'un verrou en mode de conservation sur une transaction marquée. Les verrous internes de marque de transaction sont utilisés pour la synchronisation des validations avec des transactions marquées.|  
|TRAN_MARKLATCH_NL|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|TRAN_MARKLATCH_SH|Se produit lors de l'attente d'un verrou en mode partagé sur une transaction marquée. Les verrous internes de marque de transaction sont utilisés pour la synchronisation des validations avec des transactions marquées.|  
|TRAN_MARKLATCH_UP|Se produit lors de l'attente d'un verrou en mode de mise à jour sur une transaction marquée. Les verrous internes de marque de transaction sont utilisés pour la synchronisation des validations avec des transactions marquées.|  
|TRANSACTION_MUTEX|Se produit durant la synchronisation de l'accès à une transaction par plusieurs traitements.|  
|UTIL_PAGE_ALLOC|Se produit lorsque les analyses des journaux des transactions attendent que de la mémoire soit disponible lors de la sollicitation de la mémoire.|  
|VIA_ACCEPT|Se produit lorsqu'une connexion du fournisseur VIA (Virtual Interface Adapter) est terminée au cours du démarrage.|  
|VIEW_DEFINITION_MUTEX|Se produit durant la synchronisation d'accès aux définitions des vues mises en cache.|  
|WAIT_FOR_RESULTS|Se produit durant l'attente du déclenchement d'une notification de requête.|  
|WAITFOR|Se produit suite à une instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] WAITFOR. La durée de l'attente est déterminée par les paramètres de l'instruction. Il s'agit d'une attente initialisée par l'utilisateur.|  
|WAITFOR_TASKSHUTDOWN|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|WAITSTAT_MUTEX|Se produit durant la synchronisation d'accès à la collection de statistiques utilisées pour remplir sys.dm_os_wait_stats.|  
|WCC|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|WORKTBL_DROP|Se produit lors d'une suspension avant une nouvelle tentative, suite à l'échec de la suppression d'une table de travail.|  
|WRITE_COMPLETION|Se produit lorsqu'une opération d'écriture est en cours.|  
|WRITELOG|Se produit lors de l'attente d'un vidage du journal. Les opérations courantes qui provoquent des vidages du journal sont les points de vérification et les validations des transactions.|  
|XACT_OWN_TRANSACTION|Se produit pendant l'attente de l'obtention de la propriété d'une transaction.|  
|XACT_RECLAIM_SESSION|Se produit lorsque vous attendez que le propriétaire actuel d'une session libère la propriété de la session.|  
|XACTLOCKINFO|Se produit durant la synchronisation de l'accès à la liste des verrous d'une transaction. Outre la transaction proprement dite, la liste des verrous est accessible par le biais d'opérations telles que la détection d'interblocages et la migration des verrous durant les fractionnements de pages.|  
|XACTWORKSPACE_MUTEX|Se produit durant la synchronisation des désinscriptions à partir d'une transaction et du nombre de verrous de base de données entre les membres d'inscription d'une transaction.|  
|XE_BUFFERMGR_ALLPROCESSED_EVENT|Se produit lorsque les mémoires tampons de session d'Événements étendus sont vidées sur les cibles. Cette attente se produit dans un thread d'arrière-plan.|  
|XE_BUFFERMGR_FREEBUF_EVENT|Se produit lorsqu'une des conditions suivantes est remplie :<br /><br /> Une session Événements étendus est configurée sans perte d'événement et tous les tampons de la session sont actuellement pleins. Cela peut indiquer que tous les tampons pour une session Événements étendus sont trop petits ou qu'ils doivent être partitionnés.<br /><br /> Les audits subissent un délai. Cela peut indiquer un goulot d'étranglement du disque sur le lecteur sur lequel les audits sont écrits.|  
|XE_DISPATCHER_CONFIG_SESSION_LIST|Se produit lorsqu'une session Événements étendus qui utilise des cibles asynchrones est démarrée ou arrêtée. Cette attente signifie que :<br /><br /> Une session Événements étendus est inscrite avec un pool de threads d'arrière-plan.<br /><br /> Le pool de threads d'arrière-plan calcule le nombre de threads requis en fonction de la charge actuelle.|  
|XE_DISPATCHER_JOIN|Se produit lorsqu'un thread d'arrière-plan utilisé pour les sessions Événements étendus est arrêté.|  
|XE_DISPATCHER_WAIT|Se produit lorsqu'un thread d'arrière-plan utilisé pour les sessions Événements étendus attend le traitement des tampons d'événements.|  
|XE_MODULEMGR_SYNC|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|XE_OLS_LOCK|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|XE_PACKAGE_LOCK_BACKOFF|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|FT_COMPROWSET_RWLOCK|Le texte intégral attend une opération des métadonnées du fragment. Documenté à titre d'information uniquement. Non pris en charge. La compatibilité future n'est pas garantie.|  
|FT_IFTS_RWLOCK|Le texte intégral attend une synchronisation interne. Documenté à titre d'information uniquement. Non pris en charge. La compatibilité future n'est pas garantie.|  
|FT_IFTS_SCHEDULER_IDLE_WAIT|Type d'attente en état de veille du planificateur de texte intégral. Le planificateur est inactif.|  
|FT_IFTSHC_MUTEX|Le texte intégral attend une opération de contrôle fdhost. Documenté à titre d'information uniquement. Non pris en charge. La compatibilité future n'est pas garantie.|  
|FT_IFTSISM_MUTEX|Le texte intégral attend une opération de communication. Documenté à titre d'information uniquement. Non pris en charge. La compatibilité future n'est pas garantie.|  
|FT_MASTER_MERGE|Le texte intégral attend une opération de fusion principale. Documenté à titre d'information uniquement. Non pris en charge. La compatibilité future n'est pas garantie.|  
  
  
