---
description: sys.dm_os_wait_stats (Transact-SQL)
title: sys.dm_os_wait_stats (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/19/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_os_wait_stats_TSQL
- dm_os_wait_stats
- sys.dm_os_wait_stats
- sys.dm_os_wait_stats_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_wait_stats dynamic management view
ms.assetid: 568d89ed-2c96-4795-8a0c-2f3e375081da
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 16226ba2a759e0455078330a7437a99c263ce49a
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097619"
---
# <a name="sysdm_os_wait_stats-transact-sql"></a>sys.dm_os_wait_stats (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Retourne des informations sur toutes les attentes subies par les threads qui se sont exécutés. Vous pouvez utiliser cette vue agrégée pour diagnostiquer les problèmes de performance liés à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et également liés à des requêtes et des traitements spécifiques. [sys.dm_exec_session_wait_stats &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-session-wait-stats-transact-sql.md) fournit des informations similaires par session.  
  
> [!NOTE] 
> Pour appeler cette valeur à partir de **[!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] ou**, utilisez le nom **sys.dm_pdw_nodes_os_wait_stats**.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|wait_type|**nvarchar(60)**|Nom du type d'attente. Pour plus d'informations, consultez [Types d'attentes](#WaitTypes), plus loin dans cette rubrique.|  
|waiting_tasks_count|**bigint**|Nombre d'attentes sur ce type d'attente. Ce compteur est incrémenté au début de chaque attente.|  
|wait_time_ms|**bigint**|Temps d'attente total en millisecondes pour ce type d'attente. Ce temps comprend signal_wait_time_ms.|  
|max_wait_time_ms|**bigint**|Temps d'attente maximal sur ce type d'attente.|  
|signal_wait_time_ms|**bigint**|Différence entre le moment où le thread qui attend a été signalé et le moment où il a commencé à s'exécuter.|  
|pdw_node_id|**int**|Identificateur du nœud sur lequel cette distribution se trouve. <br/> **S’applique à**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] |  
  
## <a name="permissions"></a>Autorisations

Sur [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] , requiert l' `VIEW SERVER STATE` autorisation.   
Sur SQL Database objectifs de service de base, S0 et S1, et pour les bases de données dans des pools élastiques, le `Server admin` ou un `Azure Active Directory admin` compte est requis. Pour tous les autres SQL Database objectifs de service, l' `VIEW DATABASE STATE` autorisation est requise dans la base de données.   

##  <a name="types-of-waits"></a><a name="WaitTypes"></a> Types d’attente  
 Les **attentes de ressource** se produisent lorsqu’un Worker demande l’accès à une ressource qui n’est pas disponible parce que la ressource est utilisée par un autre travail ou qu’elle n’est pas encore disponible. Ces attentes sont par exemple des attentes de verrous, de verrous internes, de réseau et d'E/S de disque. Les attentes de verrous et de verrous internes sont des attentes sur des objets de synchronisation.  
  
Les **attentes de file d’attente** se produisent lorsqu’un thread de travail est inactif, en attente d’une affectation de travail. Ces attentes se produisent le plus souvent avec des tâches système en arrière-plan, telles que la surveillance des interblocages et le nettoyage des enregistrements supprimés. Ces tâches attendent que les demandes de travail soient placées dans une file d'attente. Les attentes de file d'attente peuvent aussi devenir actives même si aucun nouveau paquet n'a été placé dans la file d'attente.  
  
 Les **attentes externes** se produisent lorsqu’un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] thread de travail attend un événement externe, tel qu’un appel de procédure stockée étendue ou une requête de serveur lié, pour se terminer. Lorsque vous diagnostiquez des problèmes de blocage, souvenez-vous que les attentes externes n'impliquent pas forcément que le thread de travail est inactif, parce qu'il est peut être en train d'exécuter du code externe.  
  
 `sys.dm_os_wait_stats` indique la durée des attentes qui se sont achevées. Cette vue de gestion dynamique n'affiche pas les attentes actuelles.  
  
 Un thread de travail [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] n'est pas considéré comme étant en train d'attendre si l'une de ces conditions est vraie :  
  
-   Une ressource devient disponible.  
  
-   Une file d'attente n'est pas vide.  
  
-   Un processus externe se termine.  
  
 Bien que le thread ne soit plus en train d'attendre, il n'a pas à redémarrer immédiatement. En effet, ce type de thread est d'abord placé dans la file d'attente des travaux pouvant s'exécuter et doit attendre qu'un quantum s'exécute sur le planificateur.  
  
 Dans, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] les compteurs de temps d’attente sont des valeurs **bigint** et, par conséquent, ne sont pas aussi sujets à la substitution de compteur que les compteurs équivalents dans les versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 Des types spécifiques de temps d'attente pendant l'exécution des requêtes peuvent indiquer des goulots d'étranglement ou des points de blocage dans la requête. De la même façon, des temps d'attente élevés, ou des nombres d'attente à l'échelle du serveur peuvent indiquer des goulots d'étranglement ou des zones réactives en interaction avec l'instance du serveur. Par exemple, des attentes de verrou indiquent une contention de données par les requêtes ; des attentes de verrou interne d'E/S de page indiquent des temps de réponse E/S lents ; des attentes de mise à jour de verrous internes de page indiquent une mise en page de fichier incorrecte.  
  
 Le contenu de cette vue de gestion dynamique peut être réinitialisé en exécutant la commande suivante :  
  
```sql  
DBCC SQLPERF ('sys.dm_os_wait_stats', CLEAR);  
GO  
```  
  
Cette commande remet tous les compteurs à 0.  
  
> [!NOTE]
> Ces statistiques ne sont pas préservées d'un redémarrage de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à l'autre et toutes les données s'accumulent depuis la dernière réinitialisation des statistiques ou le dernier démarrage du serveur.  
  
 Le tableau suivant récapitule les types d'attente que rencontrent les tâches.  

|type |Description| 
|-------------------------- |--------------------------| 
|ABR |Identifié à titre d'information uniquement. Non pris en charge. La compatibilité future n'est pas garantie.| | 
|AM_INDBUILD_ALLOCATION |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|AM_SCHEMAMGR_UNSHARED_CACHE |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|ASSEMBLY_FILTER_HASHTABLE |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] et versions ultérieures.| 
|ASSEMBLY_LOAD |Se produit pendant l'accès exclusif au chargement d'un assembly.| 
|ASYNC_DISKPOOL_LOCK |Se produit lors d'une tentative de synchronisation de threads parallèles qui effectuent des tâches telles que la création ou l'initialisation d'un fichier.| 
|ASYNC_IO_COMPLETION |Se produit lorsqu'une tâche attend la fin d'E/S.| 
|ASYNC_NETWORK_IO |Se produit sur des écritures réseau lorsque la tâche est bloquée derrière le réseau. Vérifiez que le client traite les données du serveur.| 
|ASYNC_OP_COMPLETION |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|ASYNC_OP_CONTEXT_READ |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|ASYNC_OP_CONTEXT_WRITE |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|ASYNC_SOCKETDUP_IO |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] et versions ultérieures.| 
|AUDIT_GROUPCACHE_LOCK |Se produit lorsqu'il y a une attente sur un verrou qui contrôle l'accès à un cache spécial. Le cache contient des informations sur les audits utilisés pour auditer chaque groupe d'actions d'audit.| 
|AUDIT_LOGINCACHE_LOCK |Se produit lorsqu'il y a une attente sur un verrou qui contrôle l'accès à un cache spécial. Le cache contient des informations sur les audits utilisés pour auditer des groupes d'actions d'audit de connexion.| 
|AUDIT_ON_DEMAND_TARGET_LOCK |Se produit lorsqu'il y a une attente sur un verrou utilisé pour garantir une initialisation unique des cibles d'événements étendus liées à l'audit.| 
|AUDIT_XE_SESSION_MGR |Se produit lorsqu'il y a une attente sur un verrou utilisé pour synchroniser le démarrage et l'arrêt des sessions d'événements étendus liées à l'audit.| 
|BACKUP |Se produit lorsqu'une tâche est bloquée dans un traitement de sauvegarde.| 
|BACKUP_OPERATOR |Se produit lorsqu'une tâche attend le montage d'une bande. Pour voir l'état de la bande, interrogez sys.dm_io_backup_tapes. S'il n'y a pas d'opération de montage en cours, ce type d'attente peut indiquer un problème matériel au niveau du lecteur de bande.| 
|BACKUPBUFFER |Se produit lorsqu'une tâche de sauvegarde attend des données ou une mémoire tampon pour y stocker les données. Ce type d'attente n'est pas typique, sauf lorsqu'une tâche attend qu'une bande monte.| 
|BACKUPIO |Se produit lorsqu'une tâche de sauvegarde attend des données ou une mémoire tampon pour y stocker les données. Ce type d'attente n'est pas typique, sauf lorsqu'une tâche attend qu'une bande monte.| 
|BACKUPTHREAD |Se produit lorsqu'une tâche attend la fin d'une tâche de sauvegarde. Les temps d'attente peuvent être longs, de plusieurs minutes à plusieurs heures. Si la tâche concernée est un processus d'E/S, ce type n'indique pas de problème.| 
|BAD_PAGE_PROCESS |Se produit lorsque le journal de pages suspectes en arrière-plan tente d'éviter une exécution plus que toutes les cinq secondes. Un nombre excessif de pages suspectes entraîne une exécution fréquente du journal.| 
|BLOB_METADATA |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] et versions ultérieures.| 
|BMPALLOCATION |Se produit avec des plans en mode batch parallèles lors de la synchronisation de l’allocation d’un filtre Bitmap de grande taille. Si l'attente est excessive et ne peut pas être réduite en ajustant la requête (en ajoutant des index, par exemple), pensez à affiner le seuil de coût pour le parallélisme ou à baisser le degré de parallélisme.<br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|BMPBUILD |Se produit avec des plans en mode batch parallèles lors de la synchronisation de la génération d’un filtre Bitmap de grande taille. Si l'attente est excessive et ne peut pas être réduite en ajustant la requête (en ajoutant des index, par exemple), pensez à affiner le seuil de coût pour le parallélisme ou à baisser le degré de parallélisme. <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|BMPREPARTITION |Se produit avec des plans en mode batch parallèle lors de la synchronisation du repartitionnement d’un filtre Bitmap de grande taille. Si l'attente est excessive et ne peut pas être réduite en ajustant la requête (en ajoutant des index, par exemple), pensez à affiner le seuil de coût pour le parallélisme ou à baisser le degré de parallélisme. <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|BMPREPLICATION |Se produit avec des plans en mode batch parallèle lors de la synchronisation de la réplication d’un filtre Bitmap volumineux entre les threads de travail. Si l'attente est excessive et ne peut pas être réduite en ajustant la requête (en ajoutant des index, par exemple), pensez à affiner le seuil de coût pour le parallélisme ou à baisser le degré de parallélisme. <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|BPSORT |Se produit avec des plans en mode batch parallèles lors de la synchronisation du tri d’un DataSet sur plusieurs threads. Si l'attente est excessive et ne peut pas être réduite en ajustant la requête (en ajoutant des index, par exemple), pensez à affiner le seuil de coût pour le parallélisme ou à baisser le degré de parallélisme. <br /><br /> **S’applique à** : [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] et versions ultérieures.| 
|BROKER_CONNECTION_RECEIVE_TASK |Se produit lorsque vous attendez l'autorisation de recevoir un message sur un point de terminaison de connexion. L'accès au point de terminaison accordé est sérialisé.| 
|BROKER_DISPATCHER |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|BROKER_ENDPOINT_STATE_MUTEX |Se produit en cas de contention pour accéder à l’état d’un point de terminaison de connexion Service Broker. L'accès à l'état pour procéder à des modifications est sérialisé.| 
|BROKER_EVENTHANDLER |Se produit lorsqu’une tâche attend dans le gestionnaire d’événements principal de l’Service Broker. Ce doit être très bref.| 
|BROKER_FORWARDER |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|BROKER_INIT |Se produit lors de l’initialisation de Service Broker dans chaque base de données active. Cela ne doit pas arriver souvent.| 
|BROKER_MASTERSTART |Se produit lorsqu’une tâche attend le début du gestionnaire d’événements principal de l’Service Broker. Ce doit être très bref.| 
|BROKER_RECEIVE_WAITFOR |Se produit lorsque RECEIVE WAITFOR attend. Cela peut signifier qu’aucun message n’est prêt à être reçu dans la file d’attente ou qu’une contention de verrouillage l’empêche de recevoir des messages de la file d’attente.| 
|BROKER_REGISTERALLENDPOINTS |Se produit pendant l’initialisation d’un point de terminaison de connexion Service Broker. Ce doit être très bref.| 
|BROKER_SERVICE |Se produit lorsque la liste de destination Service Broker qui est associée à un service cible est mise à jour ou classée de nouveau par ordre de priorité.| 
|BROKER_SHUTDOWN |Se produit en cas d’arrêt planifié d’Service Broker. Ce doit être très bref, voire inexistant.| 
|BROKER_START |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] et versions ultérieures.| 
|BROKER_TASK_SHUTDOWN |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|BROKER_TASK_STOP |Se produit lorsque le gestionnaire de tâches de file d’attente Service Broker tente d’arrêter la tâche. Le contrôle d'état est sérialisé et doit être au préalable dans un état d'exécution.| 
|BROKER_TASK_SUBMIT |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|BROKER_TO_FLUSH |Se produit lorsque l’Service Broker le vidage différé vide les objets de transmission en mémoire dans une table de travail.| 
|BROKER_TRANSMISSION_OBJECT |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|BROKER_TRANSMISSION_TABLE |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|BROKER_TRANSMISSION_WORK |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|BROKER_TRANSMITTER |Se produit lorsque l’émetteur de Service Broker est en attente de travail. Service Broker a un composant appelé émetteur qui planifie l’envoi des messages de plusieurs boîtes de dialogue sur un ou plusieurs points de terminaison de connexion. L’émetteur a 2 threads dédiés à cet effet. Ce type d’attente est facturé lorsque ces threads émetteurs attendent que des messages de boîte de dialogue soient envoyés à l’aide des connexions de transport. Des valeurs élevées de waiting_tasks_count pour ce type d’attente fonctionnent par intermittence pour ces threads émetteurs et ne sont pas des indications de problèmes de performances. Si Service Broker n’est pas utilisé du tout, waiting_tasks_count doit être 2 (pour les 2 threads émetteurs) et wait_time_ms doit être le double de la durée depuis le démarrage de l’instance. Consultez les [statistiques d’attente de service Broker](/archive/blogs/sql_service_broker/service-broker-wait-types).|
|BUILTIN_HASHKEY_MUTEX |Peut se produire après le démarrage d'une instance, pendant l'initialisation des structures de données internes. Ne se reproduira plus lorsque les structures de données auront été initialisées.| 
|CHANGE_TRACKING_WAITFORCHANGES |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|CHECK_PRINT_RECORD |Identifié à titre d'information uniquement. Non pris en charge. La compatibilité future n'est pas garantie.| 
|CHECK_SCANNER_MUTEX |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] et versions ultérieures.| 
|CHECK_TABLES_INITIALIZATION |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] et versions ultérieures.| 
|CHECK_TABLES_SINGLE_SCAN |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] et versions ultérieures.| 
|CHECK_TABLES_THREAD_BARRIER |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] et versions ultérieures.| 
|CHECKPOINT_QUEUE |Se produit lorsque la tâche de point de vérification attend la demande de point de vérification suivante.| 
|CHKPT |Se produit au démarrage du serveur pour signaler au thread du point de contrôle qu'il peut démarrer.| 
|CLEAR_DB |Se produit lors d'opérations qui modifient l'état d'une base de données, telles que l'ouverture ou la fermeture d'une base de données.| 
|CLR_AUTO_EVENT |Se produit lorsqu'une tâche est en train d'exécuter du CLR (Common Language Runtime) et qu'elle attend le début d'un événement automatique particulier. De longues attentes sont classiques et elles n'indiquent pas la présence d'un problème.| 
|CLR_CRST |Se produit lorsqu'une tâche est en train d'exécuter du CLR et qu'elle attend avant de passer à une phase essentielle qui est actuellement utilisée par une autre tâche.| 
|CLR_JOIN |Se produit lorsqu'une tâche est en train d'exécuter du CLR et qu'elle attend la fin d'une autre tâche. Cet état d'attente se produit lorsqu'il y a une jointure entre des tâches.| 
|CLR_MANUAL_EVENT |Se produit lorsqu'une tâche est en train d'exécuter du CLR et qu'elle attend le début d'un événement manuel particulier.| 
|CLR_MEMORY_SPY |Se produit pendant une attente sur une acquisition de verrou pour une structure de données utilisée pour enregistrer toutes les allocations de mémoire virtuelle qui viennent du CLR. La structure de données est verrouillée pour maintenir son intégrité en cas d'accès parallèle.| 
|CLR_MONITOR |Se produit lorsqu'une tâche est en train d'exécuter du CLR et qu'elle attend d'avoir obtenu un verrou sur le moniteur.| 
|CLR_RWLOCK_READER |Se produit lorsqu'une tâche est en train d'exécuter du CLR et qu'elle attend un verrou de lecteur.| 
|CLR_RWLOCK_WRITER |Se produit lorsqu'une tâche est en train d'exécuter du CLR et qu'elle attend un verrou d'écriture.| 
|CLR_SEMAPHORE |Se produit lorsqu'une tâche est en train d'exécuter du CLR et qu'elle attend un sémaphore.| 
|CLR_TASK_START |Se produit pendant l'attente du démarrage d'une tâche CLR.| 
|CLRHOST_STATE_ACCESS |Se produit lorsqu'il y a une attente pour l'acquisition d'un accès exclusif aux structures de données d'hébergement CLR. Ce type d'attente se produit lors de la configuration ou de la destruction du runtime CLR.| 
|CMEMPARTITIONED |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|CMEMTHREAD |Se produit lorsqu'une tâche attend un objet mémoire thread-safe. Le temps d'attente peut augmenter en cas de contention liée au fait que plusieurs tâches essaient d'allouer de la mémoire à partir du même objet mémoire.| 
|COLUMNSTORE_BUILD_THROTTLE |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|COLUMNSTORE_COLUMNDATASET_SESSION_LIST |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] et versions ultérieures.| 
|COMMIT_TABLE |À usage interne uniquement| 
|CONNECTION_ENDPOINT_LOCK |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] et versions ultérieures.| 
|COUNTRECOVERYMGR |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|CREATE_DATINISERVICE |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|CXCONSUMER <a name="cxconsumer"></a>|Se produit avec des plans de requête parallèles lorsqu’un thread de consommateur (parent) attend qu’un thread producteur envoie des lignes. Les attentes CXCONSUMER sont provoquées par un itérateur Exchange qui ne dispose plus de lignes de son thread producteur. Il s’agit d’une partie normale de l’exécution des requêtes parallèles. <br /><br /> **S’applique à**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à partir de [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] SP2, [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] , [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]|
|CXPACKET <a name="cxpacket"></a>|Se produit avec des plans de requête parallèles lors de l’attente de la synchronisation de l' [itérateur d’échange](../../relational-databases/showplan-logical-and-physical-operators-reference.md)du processeur de requêtes, et lors de la production et de la consommation de lignes. Si l’attente est excessive et ne peut pas être réduite par le paramétrage de la requête (par exemple, l’ajout d’index), envisagez d’ajuster le seuil de coût pour le parallélisme ou de réduire le degré maximal de parallélisme (MaxDOP). <br /><br /> **Remarque :** À compter de [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] SP2 et [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3, CXPACKET fait uniquement référence à l’attente de la synchronisation de l’itérateur Exchange et à la production de lignes. Les threads consommant des lignes sont suivis séparément dans le type d’attente CXCONSUMER. Si les threads de consommateur sont trop lents, la mémoire tampon de l’itérateur Exchange peut devenir pleine et provoquer l’attente de CXPACKET. <br /><br /> **Remarque :** Dans [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] et [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)] , CXPACKET fait uniquement référence à l’attente de threads produisant des lignes. La synchronisation de l’itérateur Exchange est suivie séparément dans le CXSYNC_PORT et CXSYNC_CONSUMER les types d’attente. Les threads consommant des lignes sont suivis séparément dans le type d’attente CXCONSUMER.<br /> | 
|CXSYNC_PORT|Se produit avec des plans de requête parallèles lors de l’attente de l’ouverture, de la fermeture et de la synchronisation des ports d' [itérateurs Exchange](../../relational-databases/showplan-logical-and-physical-operators-reference.md) entre les threads producteur et consommateur. Par exemple, si un plan de requête a une longue opération de tri, CXSYNC_PORT attentes peuvent être plus élevées, car le tri doit se terminer avant que le port d’itérateur Exchange puisse être synchronisé. <br /><br /> **S’applique à**: [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] , [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]| 
|CXSYNC_CONSUMER|Se produit avec des plans de requête parallèles lorsqu’ils attendent d’atteindre un point de synchronisation d' [itérateur Exchange](../../relational-databases/showplan-logical-and-physical-operators-reference.md) parmi tous les threads consommateurs. <br /><br /> **S’applique à**: [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] , [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]| 
|CXROWSET_SYNC |Se produit pendant une analyse de plage parallèle.| 
|DAC_INIT |Se produit alors que la connexion administrateur dédiée est en cours d'initialisation.| 
|DBCC_SCALE_OUT_EXPR_CACHE |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|DBMIRROR_DBM_EVENT |Identifié à titre d'information uniquement. Non pris en charge. La compatibilité future n'est pas garantie.| 
|DBMIRROR_DBM_MUTEX |Identifié à titre d'information uniquement. Non pris en charge. La compatibilité future n'est pas garantie.| 
|DBMIRROR_EVENTS_QUEUE |Se produit lorsque la mise en miroir de base de données attend le traitement des événements.| 
|DBMIRROR_SEND |Se produit lorsqu'une tâche attend que le Backlog des communications au niveau de la couche réseau soit vidé pour pouvoir envoyer des messages. Indique que la couche des communications est proche de la saturation et que cela affecte le débit des données pour la mise en miroir de bases de données.| 
|DBMIRROR_WORKER_QUEUE |Indique que la tâche de travail de mise en miroir de base de données attend plus de travail.| 
|DBMIRRORING_CMD |Se produit lorsqu'une tâche attend que les enregistrements de journal soient vidés sur le disque. Cet état d'attente dure généralement assez longtemps.| 
|DBSEEDING_FLOWCONTROL |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|DBSEEDING_OPERATION |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|DEADLOCK_ENUM_MUTEX |Se produit lorsque le moniteur de blocage et sys.dm_os_waiting_tasks essaient de vérifier que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] n'effectue pas plusieurs recherches de blocage en même temps.| 
|DEADLOCK_TASK_SEARCH |Une durée d'attente importante pour cette ressource indique que le serveur exécute des requêtes par-dessus sys.dm_os_waiting_tasks, et que ces dernières empêchent l'analyseur de blocages d'exécuter une recherche de blocage. Ce type d'attente est utilisé uniquement par l'analyseur d'interblocages. Les requêtes exécutées par-dessus sys.dm_os_waiting_tasks utilisent DEADLOCK_ENUM_MUTEX.| 
|DEBUG |Se produit pendant le débogage Transact-SQL et CLR pour la synchronisation interne.| 
|DIRECTLOGCONSUMER_LIST |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] et versions ultérieures.| 
|DIRTY_PAGE_POLL |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|DIRTY_PAGE_SYNC |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|DIRTY_PAGE_TABLE_LOCK |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] et versions ultérieures.| 
|DISABLE_VERSIONING |Se produit lorsque [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sonde le gestionnaire de transactions de versions pour voir si le cachet temporel de la plus ancienne transaction active est postérieur au cachet temporel indiquant le début de changement d'état. Si c'est le cas, toutes les transactions d'instantané qui avaient démarré avant l'exécution de l'instruction ALTER DATABASE sont terminées. Cet état d'attente est utilisé lorsque [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] désactive le contrôle de version à l'aide de l'instruction ALTER DATABASE.| 
|DISKIO_SUSPEND |Se produit lorsqu'une tâche attend pour accéder à un fichier lorsqu'une sauvegarde externe est en cours. Ce type d'attente est signalé pour chaque processus utilisateur en attente. Un nombre supérieur à cinq par processus utilisateur peut indiquer que la sauvegarde externe prend trop de temps.| 
|DISPATCHER_PRIORITY_QUEUE_SEMAPHORE |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|DISPATCHER_QUEUE_SEMAPHORE |Se produit lorsqu'un thread du pool de répartiteurs attend de traiter d'autres travaux. Le temps d'attente pour ce type d'attente est supposé augmenter lorsque le répartiteur est inactif.| 
|DLL_LOADING_MUTEX |Se produit une fois pendant l'attente du chargement de la DLL de l'analyseur XML.| 
|DPT_ENTRY_LOCK |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] et versions ultérieures.| 
|DROP_DATABASE_TIMER_TASK |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|DROPTEMP |Se produit entre les tentatives de suppression d'un objet temporaire si la tentative précédente a échoué. La durée d'attente augmente de manière exponentielle au fur et à mesure que les différentes tentatives de suppression échouent.| 
|DTC |Se produit lorsqu'une tâche s'occupe d'un événement qui est utilisé pour gérer une transition d'état. Cet État contrôle le moment où la récupération des transactions Microsoft Distributed Transaction Coordinator (MS DTC) se produit après la réception d’une [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] notification indiquant que le service MS DTC est devenu indisponible.| 
|DTC_ABORT_REQUEST |Se produit dans une session de travail MS DTC lorsque la session attend de prendre possession d'une transaction MS DTC. Une fois que MS DTC est propriétaire de la transaction, la session peut la restaurer. En général, la session attendra une autre session qui utilise la transaction.| 
|DTC_RESOLVE |Se produit lorsqu'une tâche de récupération attend la base de données master dans une transaction entre bases de données, afin de pouvoir interroger le résultat de la transaction.| 
|DTC_STATE |Se produit lorsqu'une tâche s'occupe d'un événement qui protège les modifications effectuées sur l'objet d'état global MS DTC interne. Cet état doit être maintenu pendant des périodes très courtes.| 
|DTC_TMDOWN_REQUEST |Se produit dans une session de travail MS DTC lorsque [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] reçoit une notification selon laquelle le service MS DTC n'est pas disponible. Tout d'abord, le thread de travail attend que le processus de récupération MS DTC commence. Ensuite, il attend d'avoir obtenu le résultat de la transaction distribuée sur laquelle il travaille. Cela peut continuer jusqu'à ce que la connexion avec le service MS DTC ait été rétablie.| 
|DTC_WAITFOR_OUTCOME |Se produit lorsque des tâches de récupération attendent que MS DTC s'active pour permettre la résolution des transactions préparées.| 
|DTCNEW_ENLIST |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] et versions ultérieures.| 
|DTCNEW_PREPARE |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] et versions ultérieures.| 
|DTCNEW_RECOVERY |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] et versions ultérieures.| 
|DTCNEW_TM |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] et versions ultérieures.| 
|DTCNEW_TRANSACTION_ENLISTMENT |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] et versions ultérieures.| 
|DTCPNTSYNC |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|DUMP_LOG_COORDINATOR |Se produit lorsqu'une tâche principale attend qu'une tâche secondaire ait produit des données. En principe cet état n'arrive pas. Une longue attente indique un blocage inattendu. Il faut examiner la tâche secondaire.| 
|DUMP_LOG_COORDINATOR_QUEUE |À usage interne uniquement| 
|DUMPTRIGGER |Identifié à titre d'information uniquement. Non pris en charge. La compatibilité future n'est pas garantie.| 
|EC |Identifié à titre d'information uniquement. Non pris en charge. La compatibilité future n'est pas garantie.| 
|EE_PMOLOCK |Se produit durant la synchronisation de certains types d'allocations de mémoire durant l'exécution d'une instruction.| 
|EE_SPECPROC_MAP_INIT |Se produit durant la synchronisation de la création de la table de hachage de procédure interne. Cette attente ne peut se produire que lors de l'accès initial à la table de hachage après le démarrage de l'instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].| 
|ENABLE_EMPTY_VERSIONING |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|ENABLE_VERSIONING |Se produit lorsque [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] attend que toutes les transactions de mise à jour de la base de données soient terminées avant de déclarer que la base de données est prête à passer à l'état autorisé d'isolement d'instantané. Cet état est utilisé lorsque [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] active l'isolement d'instantané à l'aide de l'instruction ALTER DATABASE.| 
|ERROR_REPORTING_MANAGER |Se produit durant la synchronisation de plusieurs initialisations simultanées de journaux d'erreurs.| 
|EXCHANGE |Se produit durant la synchronisation dans l'itérateur d'échange du processeur de requêtes au cours de requêtes parallèles.| 
|EXECSYNC |Se produit au cours de requêtes parallèles, lors de la synchronisation dans le processeur de requêtes, dans des zones qui ne sont pas liées à l'itérateur d'échange. Ces zones sont par exemple des images bitmap, des objets binaires volumineux et l'itérateur de spouleurs. Les objets LOB peuvent utiliser souvent cet état d'attente.| 
|EXECUTION_PIPE_EVENT_INTERNAL |Se produit durant la synchronisation entre les parties producteur et consommateur d'exécution de lot soumises via le contexte de connexion.| 
|EXTERNAL_RG_UPDATE |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] et versions ultérieures.| 
|EXTERNAL_SCRIPT_NETWORK_IO |À usage interne uniquement <br /><br /> **S’applique à**: [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] en cours.| 
|EXTERNAL_SCRIPT_PREPARE_SERVICE |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] et versions ultérieures.| 
|EXTERNAL_SCRIPT_SHUTDOWN |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] et versions ultérieures.| 
|EXTERNAL_WAIT_ON_LAUNCHER, |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] et versions ultérieures.| 
|FABRIC_HADR_TRANSPORT_CONNECTION |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|FABRIC_REPLICA_CONTROLLER_LIST |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|FABRIC_REPLICA_CONTROLLER_STATE_AND_CONFIG |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|FABRIC_REPLICA_PUBLISHER_EVENT_PUBLISH |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|FABRIC_REPLICA_PUBLISHER_SUBSCRIBER_LIST |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|FABRIC_WAIT_FOR_BUILD_REPLICA_EVENT_PROCESSING |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|FAILPOINT |Identifié à titre d'information uniquement. Non pris en charge. La compatibilité future n'est pas garantie.| 
|FCB_REPLICA_READ |Se produit lorsque les lectures du fichier partiellement alloué d'un instantané (ou d'un instantané temporaire créée par DBCC) sont synchronisées.| 
|FCB_REPLICA_WRITE |Se produit lorsque des émissions ou extractions d'une page dans un fichier partiellement alloué d'instantané (ou un instantané temporaire créé par DBCC) sont synchronisées.| 
|FEATURE_SWITCHES_UPDATE |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|FFT_NSO_DB_KILL_FLAG |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|FFT_NSO_DB_LIST |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|FFT_NSO_FCB |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|FFT_NSO_FCB_FIND |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|FFT_NSO_FCB_PARENT |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|FFT_NSO_FCB_RELEASE_CACHED_ENTRIES |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|FFT_NSO_FCB_STATE |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|FFT_NSO_FILEOBJECT |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|FFT_NSO_TABLE_LIST |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|FFT_NTFS_STORE |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|FFT_RECOVERY |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|FFT_RSFX_COMM |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|FFT_RSFX_WAIT_FOR_MEMORY |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|FFT_STARTUP_SHUTDOWN |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|FFT_STORE_DB |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|FFT_STORE_ROWSET_LIST |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|FFT_STORE_TABLE |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|FILE_VALIDATION_THREADS |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] et versions ultérieures.| 
|FILESTREAM_CACHE |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|FILESTREAM_CHUNKER |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|FILESTREAM_CHUNKER_INIT |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|FILESTREAM_FCB |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|FILESTREAM_FILE_OBJECT |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|FILESTREAM_WORKITEM_QUEUE |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|FILETABLE_SHUTDOWN |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|FOREIGN_REDO |À usage interne uniquement <br /><br /> **S’applique à**: [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] en cours.| 
|FORWARDER_TRANSITION |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] et versions ultérieures.| 
|FS_FC_RWLOCK |Se produit lorsqu'il y a une attente par le garbage collector FILESTREAM pour effectuer l'une des opérations suivantes :| 
|FS_GARBAGE_COLLECTOR_SHUTDOWN |Se produit lorsque le garbage collector FILESTREAM attend l'exécution de tâches de nettoyage.| 
|FS_HEADER_RWLOCK |Se produit lorsqu'il y a une attente d'obtention de l'accès à l'en-tête FILESTREAM d'un conteneur de données FILESTREAM pour la lecture ou la mise à jour du contenu du fichier d'en-tête FILESTREAM (Filestream.hdr).| 
|FS_LOGTRUNC_RWLOCK |Se produit lorsqu'il y a une attente d'obtention de l'accès à la troncation de journaux FILESTREAM pour effectuer l'une des opérations suivantes :| 
|FSA_FORCE_OWN_XACT |Se produit lorsqu'une opération d'E/S de fichier FILESTREAM doit se lier à la transaction associée, alors que la transaction appartient actuellement à une autre session.| 
|FSAGENT |Se produit lorsqu'une opération d'E/S de fichier FILESTREAM attend une ressource de l'agent FILESTREAM utilisée par une autre opération d'E/S de fichier.| 
|FSTR_CONFIG_MUTEX |Se produit lorsqu'il y a une attente d'une autre reconfiguration de fonctionnalité FILESTREAM.| 
|FSTR_CONFIG_RWLOCK |Se produit lorsqu'il y a une attente de sérialisation de l'accès aux paramètres de configuration FILESTREAM.| 
|FT_COMPROWSET_RWLOCK |Le texte intégral attend une opération des métadonnées du fragment. Documenté à titre d'information uniquement. Non pris en charge. La compatibilité future n'est pas garantie.| 
|FT_IFTS_RWLOCK |Le texte intégral attend une synchronisation interne. Documenté à titre d'information uniquement. Non pris en charge. La compatibilité future n'est pas garantie.| 
|FT_IFTS_SCHEDULER_IDLE_WAIT |Type d'attente en état de veille du planificateur de texte intégral. Le planificateur est inactif.| 
|FT_IFTSHC_MUTEX |Le texte intégral attend une opération de contrôle fdhost. Documenté à titre d'information uniquement. Non pris en charge. La compatibilité future n'est pas garantie.| 
|FT_IFTSISM_MUTEX |Le texte intégral attend une opération de communication. Documenté à titre d'information uniquement. Non pris en charge. La compatibilité future n'est pas garantie.| 
|FT_MASTER_MERGE |Le texte intégral attend une opération de fusion principale. Documenté à titre d'information uniquement. Non pris en charge. La compatibilité future n'est pas garantie.| 
|FT_MASTER_MERGE_COORDINATOR |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|FT_METADATA_MUTEX |Documenté à titre d'information uniquement. Non pris en charge. La compatibilité future n'est pas garantie.| 
|FT_PROPERTYLIST_CACHE |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|FT_RESTART_CRAWL |Se produit lorsqu'une analyse de texte intégral doit redémarrer à partir du dernier point de référence connu et fiable pour une récupération faisant suite à une panne transitoire. Grâce à cette attente, les tâches travaillant actuellement sur ce remplissage peuvent se terminer ou quitter l'étape en cours.| 
|FULLTEXT GATHERER |Se produit durant la synchronisation d'opérations de texte intégral.| 
|GDMA_GET_RESOURCE_OWNER |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|GHOSTCLEANUP_UPDATE_STATS |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] et versions ultérieures.| 
|GHOSTCLEANUPSYNCMGR |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|GLOBAL_QUERY_CANCEL |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] et versions ultérieures.| 
|GLOBAL_QUERY_CLOSE |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] et versions ultérieures.| 
|GLOBAL_QUERY_CONSUMER |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] et versions ultérieures.| 
|GLOBAL_QUERY_PRODUCER |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] et versions ultérieures.| 
|GLOBAL_TRAN_CREATE |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] et versions ultérieures.| 
|GLOBAL_TRAN_UCS_SESSION |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] et versions ultérieures.| 
|GUARDIAN |Identifié à titre d'information uniquement. Non pris en charge. La compatibilité future n'est pas garantie.| 
|HADR_AG_MUTEX |Se produit lorsqu’une instruction DDL Always On ou une commande de clustering de basculement Windows Server attend un accès exclusif en lecture/écriture à la configuration d’un groupe de disponibilité. <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|HADR_AR_CRITICAL_SECTION_ENTRY |Se produit lorsqu’une instruction DDL Always On ou une commande de clustering de basculement Windows Server attend un accès exclusif en lecture/écriture à l’état d’exécution du réplica local du groupe de disponibilité associé. <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|HADR_AR_MANAGER_MUTEX |Se produit lorsque un réplica de disponibilité à l'arrêt attend d'être redémarré ou qu'un réplica de disponibilité en cours de démarrage attend d'être arrêté. À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|HADR_AR_UNLOAD_COMPLETED |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|HADR_ARCONTROLLER_NOTIFICATIONS_SUBSCRIBER_LIST |Le serveur de publication d'un événement de réplica de disponibilité (comme un changement d'état ou de configuration) attend l'accès exclusif en lecture/écriture à la liste des abonnés à un événement. À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|HADR_BACKUP_BULK_LOCK |La Always On base de données primaire a reçu une demande de sauvegarde d’une base de données secondaire et attend que le thread d’arrière-plan termine le traitement de la requête lors de l’acquisition ou de la libération du verrou BulkOp. <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|HADR_BACKUP_QUEUE |Le thread d’arrière-plan de sauvegarde de la base de données primaire Always On attend une nouvelle demande de travail de la base de données secondaire. (En général, cela se produit lorsque la base de données primaire détient le journal BulkOp et attend que la base de données secondaire indique que la base de données primaire peut libérer le verrou.) <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|HADR_CLUSAPI_CALL |Un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] thread attend de passer du mode non préemptif (planifié par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ) au mode préemptif (planifié par le système d’exploitation) afin d’appeler les API de clustering de basculement Windows Server. <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|HADR_COMPRESSED_CACHE_SYNC |En attente de l'accès au cache des blocs de journal compressés qui est utilisé pour éviter une compression redondante des blocs de journal envoyés à plusieurs bases de données secondaires. <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|HADR_CONNECTIVITY_INFO |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|HADR_DATABASE_FLOW_CONTROL |En attente de messages à envoyer au partenaire lorsque le nombre maximal de messages en file d'attente a été atteint. Indique que les analyses de journal s'exécutent plus rapidement que la vitesse d'envoi par le réseau. Ceci pose problème uniquement si le réseau envoie les données plus lentement que prévu. <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|HADR_DATABASE_VERSIONING_STATE |Se produit lors de la modification de l’état du contrôle de version d’une base de données secondaire Always On. Cette attente concerne les structures de données internes et est généralement très brève sans effet direct sur l’accès aux données. <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|HADR_DATABASE_WAIT_FOR_RECOVERY |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] et versions ultérieures.| 
|HADR_DATABASE_WAIT_FOR_RESTART |En attente de redémarrage de la base de données sous Always On contrôle des groupes de disponibilité. Dans des conditions normales, ceci ne pose aucun problème à l'utilisateur car les attentes sont prévues dans ce cas. <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|HADR_DATABASE_WAIT_FOR_TRANSITION_TO_VERSIONING |Une requête sur un ou plusieurs objets dans une base de données secondaire accessible en lecture d’un groupe de disponibilité Always On est bloquée sur le contrôle de version de ligne lors de l’attente de la validation ou de la restauration de toutes les transactions qui étaient en cours lorsque le réplica secondaire a été activé pour les charges de travail en lecture. Ce type d'attente garantit que les versions de ligne sont disponibles avant l'exécution d'une requête sous isolement d'instantané. <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|HADR_DB_COMMAND |En attente de réponses aux messages de conversation (qui nécessitent une réponse explicite de l’autre côté, à l’aide de l’infrastructure de message de conversation Always On). Un certain nombre de types de message différents utilise ce type d'attente. <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|HADR_DB_OP_COMPLETION_SYNC |En attente de réponses aux messages de conversation (qui nécessitent une réponse explicite de l’autre côté, à l’aide de l’infrastructure de message de conversation Always On). Un certain nombre de types de message différents utilise ce type d'attente. <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|HADR_DB_OP_START_SYNC |Une instruction DDL Always On ou une commande de clustering de basculement Windows Server attend l’accès sérialisé à une base de données de disponibilité et son état d’exécution. <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|HADR_DBR_SUBSCRIBER |Le serveur de publication d'un événement de réplica de disponibilité (comme un changement d'état ou de configuration) attend l'accès exclusif en lecture/écriture à l'état d'exécution d'un abonné à un événement qui correspond à une base de données de disponibilité. À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|HADR_DBR_SUBSCRIBER_FILTER_LIST |Le serveur de publication d'un événement de réplica de disponibilité (comme un changement d'état ou de configuration) attend l'accès exclusif en lecture/écriture à la liste des abonnés à un événement qui correspondent à des bases de données de disponibilité. À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|HADR_DBSEEDING |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|HADR_DBSEEDING_LIST |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|HADR_DBSTATECHANGE_SYNC |Attente de contrôle de concurrence pour la mise à jour de l'état interne du réplica de base de données. <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|HADR_FABRIC_CALLBACK |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|HADR_FILESTREAM_BLOCK_FLUSH |Le gestionnaire de transport FILESTREAM Always On attend jusqu’à ce que le traitement d’un bloc de journal soit terminé. <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|HADR_FILESTREAM_FILE_CLOSE |Le gestionnaire de transport FILESTREAM Always On attend que le prochain fichier FILESTREAM soit traité et que son handle soit fermé. <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|HADR_FILESTREAM_FILE_REQUEST |Un réplica secondaire Always On attend que le réplica principal envoie tous les fichiers FILESTREAM demandés pendant l’annulation. <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|HADR_FILESTREAM_IOMGR |Le gestionnaire de transport FILESTREAM Always On attend un verrou R/W qui protège le FILESTREAM Always On gestionnaire d’e/s lors du démarrage ou de l’arrêt. <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|HADR_FILESTREAM_IOMGR_IOCOMPLETION |Le gestionnaire d’e/s FILESTREAM Always On attend la fin de l’e/s. <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|HADR_FILESTREAM_MANAGER |Le gestionnaire de transport FILESTREAM Always On attend le verrou R/W qui protège le gestionnaire de transport FILESTREAM Always On au cours du démarrage ou de l’arrêt. <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|HADR_FILESTREAM_PREPROC |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] et versions ultérieures.| 
|HADR_GROUP_COMMIT |Le traitement de validation de la transaction attend d'autoriser la validation d'un groupe afin que plusieurs enregistrements de journal de validation puissent être placés dans un bloc de journal unique. Cette attente est une condition prévue qui optimise les opérations d'E/S de journal, de capture et d'envoi. <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|HADR_LOGCAPTURE_SYNC |Contrôle de concurrence autour de la capture du journal ou de l'objet d'application lors de la création ou la destruction d'analyses. Il s'agit d'une attente prévue lorsque des serveurs partenaires changent d'état ou de statut de connexion. <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|HADR_LOGCAPTURE_WAIT |En attente de la mise à disposition d'enregistrements de journal. Peut se produire en cas d'attente de la génération de nouveaux enregistrements de journal par des connexions ou de la réalisation d'opérations d'E/S lors de la lecture du journal ne figurant pas dans le cache. Il s'agit d'une attente prévue si l'analyse du journal est mise à niveau avec la fin du journal ou procède à une lecture depuis le disque. <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|HADR_LOGPROGRESS_SYNC |Attente de contrôle de concurrence lors de la mise à jour de l'état de progression du journal des réplicas de bases de données. <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|HADR_NOTIFICATION_DEQUEUE |Une tâche en arrière-plan qui traite les notifications de clustering de basculement Windows Server attend la notification suivante. À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|HADR_NOTIFICATION_WORKER_EXCLUSIVE_ACCESS |Le gestionnaire de réplicas de disponibilité Always On attend un accès sérialisé à l’état d’exécution d’une tâche en arrière-plan qui traite les notifications de clustering de basculement Windows Server. À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|HADR_NOTIFICATION_WORKER_STARTUP_SYNC |Une tâche en arrière-plan attend la fin du démarrage d'une tâche en arrière-plan qui traite les notifications de clustering de basculement Windows Server. À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|HADR_NOTIFICATION_WORKER_TERMINATION_SYNC |Une tâche en arrière-plan attend la fin d'une tâche en arrière-plan qui traite les notifications de clustering de basculement Windows Server. À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|HADR_PARTNER_SYNC |Attente de contrôle de concurrence sur la liste des serveurs partenaires. <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|HADR_READ_ALL_NETWORKS |En attente de l'obtention de l'accès en lecture ou en écriture à la liste des réseaux WSFC. À usage interne uniquement Remarque : le moteur conserve une liste de réseaux WSFC utilisés dans les vues de gestion dynamique (par exemple sys.dm_hadr_cluster_networks) ou pour valider Always On instructions Transact-SQL qui font référence à des informations réseau WSFC. Cette liste est mise à jour lors du démarrage du moteur, des notifications liées à WSFC et du redémarrage interne du Always On (par exemple, la perte et la réobtention du quorum WSFC). Les tâches sont généralement bloquées lorsqu'une mise à jour est en cours dans cette liste. <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|HADR_RECOVERY_WAIT_FOR_CONNECTION |En attente de la connexion de la base de données secondaire à la base de données primaire avant d'effectuer la récupération. Il s'agit d'une attente prévue, qui peut se prolonger si la connexion à la base de données primaire est lente à établir. <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|HADR_RECOVERY_WAIT_FOR_UNDO |La récupération de base de données attend que la base de données secondaire termine la phase de rétablissement et d'initialisation afin de la ramener au point de journal commun avec la base de données primaire. Il s'agit d'une attente prévue à la suite d'un basculement. La progression de l'annulation peut faire l'objet d'un suivi au moyen du Moniteur système Windows (perfmon.exe) et des vues de gestion dynamique. <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|HADR_REPLICAINFO_SYNC |En attente d'un contrôle de concurrence pour mettre à jour l'état de réplica actuel. <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|HADR_SEEDING_CANCELLATION |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] et versions ultérieures.| 
|HADR_SEEDING_FILE_LIST |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] et versions ultérieures.| 
|HADR_SEEDING_LIMIT_BACKUPS |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] et versions ultérieures.| 
|HADR_SEEDING_SYNC_COMPLETION |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] et versions ultérieures.| 
|HADR_SEEDING_TIMEOUT_TASK |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] et versions ultérieures.| 
|HADR_SEEDING_WAIT_FOR_COMPLETION |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] et versions ultérieures.| 
|HADR_SYNC_COMMIT |En attente du traitement de validation de la transaction pour que les bases de données secondaires synchronisées renforcent le journal. Cette attente est également reflétée par le compteur de performances Délai de transaction. Ce type d'attente est prévu pour les groupes de disponibilité synchronisés et indique l'heure à laquelle envoyer, écrire et accepter le journal dans les bases de données secondaires. <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|HADR_SYNCHRONIZING_THROTTLE |En attente du traitement de validation de la transaction pour permettre à une base de données secondaire en cours de synchronisation de se mettre au niveau de la fin du journal de la base de données primaire afin d'assurer la transition vers l'état synchronisé. Il s'agit d'une attente prévue lors du rattrapage d'une base de données secondaire. <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|HADR_TDS_LISTENER_SYNC |Le système de Always On interne ou le cluster WSFC demandera que les écouteurs soient démarrés ou arrêtés. Le traitement de cette demande est toujours asynchrone et il existe un mécanisme pour supprimer les demandes redondantes. Parfois, ce processus est interrompu en raison de modifications de la configuration. Toutes les attentes en rapport avec ce mécanisme de synchronisation des écouteurs font appel à ce type d'attente. À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|HADR_TDS_LISTENER_SYNC_PROCESSING |Utilisé à la fin d’une instruction Transact-SQL Always On qui nécessite le démarrage et/ou l’arrêt d’un écouteur de groupe de disponibilité. Étant donné que l'opération de démarrage/d'arrêt s'effectue de manière asynchrone, le thread utilisateur procède à un blocage à l'aide de ce type d'attente jusqu'à ce que la situation de l'écouteur soit connue. <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|HADR_THROTTLE_LOG_RATE_GOVERNOR |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] et versions ultérieures.| 
|HADR_THROTTLE_LOG_RATE_MISMATCHED_SLO | Se produit lorsqu’une base de données secondaire de géo-réplication est configurée avec une taille de calcul inférieure (SLO inférieure) par rapport à la base de données primaire. Une base de données primaire est limitée en raison d’une consommation de journal retardée par la base de données secondaire. Cela est dû au fait que la base de données secondaire dispose d’une capacité de calcul insuffisante pour suivre le taux de modification de la base de données primaire. <br /><br /> **S’applique à**: Azure SQL Database| 
|HADR_THROTTLE_LOG_RATE_LOG_SIZE |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] et versions ultérieures.| 
|HADR_THROTTLE_LOG_RATE_SEEDING |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] et versions ultérieures.| 
|HADR_THROTTLE_LOG_RATE_SEND_RECV_QUEUE_SIZE |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] et versions ultérieures.| 
|HADR_TIMER_TASK |En attente de l'obtention du verrou sur l'objet de tâche du minuteur ; également utilisé pour les attentes réelles entre l'exécution de travaux. Par exemple, pour une tâche qui s’exécute toutes les 10 secondes, après une exécution, Always On groupes de disponibilité attendent environ 10 secondes de replanifier la tâche, et l’attente est incluse ici. <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|HADR_TRANSPORT_DBRLIST |En attente de l'accès à la liste des réplicas de bases de données de la couche de transport. Utilisé pour la boucle qui accorde l'accès à cette liste. <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|HADR_TRANSPORT_FLOW_CONTROL |En attente lorsque le nombre de messages Always On sans accusé de réception en attente est dépassé le seuil de contrôle de dépassement de capacité. Il s'agit d'une attente variant d'un réplica de disponibilité à l'autre (et non d'une base de données à l'autre). <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|HADR_TRANSPORT_SESSION |Always On groupes de disponibilité sont en attente lors de la modification ou de l’accès à l’état de transport sous-jacent. <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|HADR_WORK_POOL |Attente du contrôle d’accès concurrentiel sur le Always On objet de tâche de travail en arrière-plan des groupes de disponibilité. <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|HADR_WORK_QUEUE |Always On le thread de travail en arrière-plan des groupes de disponibilité en attente d’affectation d’un nouveau travail. Il s'agit d'une attente prévue lorsque des threads de travail sont prêts à accepter un nouveau travail, ce qui correspond à l'état normal. <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|HADR_XRF_STACK_ACCESS |Accès (Rechercher, ajouter et supprimer) la pile de branchements de récupération étendus pour une base de données de disponibilité Always On. <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|HCCO_CACHE |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] et versions ultérieures.| 
|HK_RESTORE_FILEMAP |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] et versions ultérieures.| 
|HKCS_PARALLEL_MIGRATION |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] et versions ultérieures.| 
|HKCS_PARALLEL_RECOVERY |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] et versions ultérieures.| 
|HTBUILD |Se produit avec des plans en mode batch parallèles lors de la synchronisation de la génération de la table de hachage du côté entrée d’une agrégation/jointure de hachage. Si l'attente est excessive et ne peut pas être réduite en ajustant la requête (en ajoutant des index, par exemple), pensez à affiner le seuil de coût pour le parallélisme ou à baisser le degré de parallélisme. <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|HTDELETE |Se produit avec des plans en mode batch parallèles lors de la synchronisation à la fin d’une agrégation/jointure de hachage. Si l'attente est excessive et ne peut pas être réduite en ajustant la requête (en ajoutant des index, par exemple), pensez à affiner le seuil de coût pour le parallélisme ou à baisser le degré de parallélisme. <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|HTMEMO |Se produit avec des plans en mode batch parallèles lors de la synchronisation avant l’analyse de la table de hachage pour la sortie des correspondances/non-correspondances dans la jointure/agrégation de hachage. Si l'attente est excessive et ne peut pas être réduite en ajustant la requête (en ajoutant des index, par exemple), pensez à affiner le seuil de coût pour le parallélisme ou à baisser le degré de parallélisme. <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|HTREINIT |Se produit avec des plans en mode batch parallèles lors de la synchronisation avant la réinitialisation d’une association/agrégation de hachage pour la jointure partielle suivante. Si l'attente est excessive et ne peut pas être réduite en ajustant la requête (en ajoutant des index, par exemple), pensez à affiner le seuil de coût pour le parallélisme ou à baisser le degré de parallélisme. <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|HTREPARTITION |Se produit avec des plans en mode batch parallèles lors de la synchronisation du repartitionnement de la table de hachage du côté entrée d’une agrégation/jointure de hachage. Si l'attente est excessive et ne peut pas être réduite en ajustant la requête (en ajoutant des index, par exemple), pensez à affiner le seuil de coût pour le parallélisme ou à baisser le degré de parallélisme.<br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|HTTP_ENUMERATION |Se produit au démarrage pour énumérer les points de terminaison HTTP pour lancer HTTP.| 
|HTTP_START |Se produit lorsqu'une connexion attend que le protocole HTTP termine l'initialisation.| 
|HTTP_STORAGE_CONNECTION |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|IMPPROV_IOWAIT |Se produit lorsque [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] attend la fin d'une E/S de chargement en masse.| 
|INSTANCE_LOG_RATE_GOVERNOR |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] et versions ultérieures.| 
|INTERNAL_TESTING |Identifié à titre d'information uniquement. Non pris en charge. La compatibilité future n'est pas garantie.| 
|IO_AUDIT_MUTEX |Se produit durant la synchronisation des mémoires tampons d'événements de trace.| 
|IO_COMPLETION |Se produit durant l'attente de l'exécution des opérations d'E/S. Ce type d'attente représente en général des entrées/sorties de page qui ne sont pas des données. Les attentes d’exécution d’e/s de page de données apparaissent en tant qu' \_ \* attentes PAGEIOLATCH.| 
|IO_QUEUE_LIMIT |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] et versions ultérieures.| 
|IO_RETRY |Se produit lorsqu'une opération d'E/S telle qu'une lecture ou une écriture sur disque échoue en raison de ressources insuffisantes, puis est retentée.| 
|IOAFF_RANGE_QUEUE |Identifié à titre d'information uniquement. Non pris en charge. La compatibilité future n'est pas garantie.| 
|KSOURCE_WAKEUP |Utilisé par la tâche de contrôle de service pour les demandes émanant du Gestionnaire de contrôle des services. De longues attentes sont prévisibles, et elles n'indiquent pas la présence d'un problème.| 
|KTM_ENLISTMENT |Identifié à titre d'information uniquement. Non pris en charge. La compatibilité future n'est pas garantie.| 
|KTM_RECOVERY_MANAGER |Identifié à titre d'information uniquement. Non pris en charge. La compatibilité future n'est pas garantie.| 
|KTM_RECOVERY_RESOLUTION |Identifié à titre d'information uniquement. Non pris en charge. La compatibilité future n'est pas garantie.| 
|LATCH_DT |Se produit pendant l'attente d'un verrou en mode de destruction. Cela n'inclut pas les verrous internes de tampons ni les verrous internes de marque de transaction. La liste des attentes de VERROUs \_ \* est disponible dans sys.dm_os_latch_stats. Notez que sys.dm_os_latch_stats regroupe les attentes LATCH_NL, LATCH_SH, LATCH_UP, LATCH_EX et LATCH_DT.| 
|LATCH_EX |Se produit pendant l'attente d'un verrou exclusif. Cela n'inclut pas les verrous internes de tampons ni les verrous internes de marque de transaction. La liste des attentes de VERROUs \_ \* est disponible dans sys.dm_os_latch_stats. Notez que sys.dm_os_latch_stats regroupe les attentes LATCH_NL, LATCH_SH, LATCH_UP, LATCH_EX et LATCH_DT.| 
|LATCH_KP |Se produit pendant l'attente d'un verrou de maintien. Cela n'inclut pas les verrous internes de tampons ni les verrous internes de marque de transaction. La liste des attentes de VERROUs \_ \* est disponible dans sys.dm_os_latch_stats. Notez que sys.dm_os_latch_stats regroupe les attentes LATCH_NL, LATCH_SH, LATCH_UP, LATCH_EX et LATCH_DT.| 
|LATCH_NL |Identifié à titre d'information uniquement. Non pris en charge. La compatibilité future n'est pas garantie.| 
|LATCH_SH |Se produit pendant l'attente d'un verrou de partage. Cela n'inclut pas les verrous internes de tampons ni les verrous internes de marque de transaction. La liste des attentes de VERROUs \_ \* est disponible dans sys.dm_os_latch_stats. Notez que sys.dm_os_latch_stats regroupe les attentes LATCH_NL, LATCH_SH, LATCH_UP, LATCH_EX et LATCH_DT.| 
|LATCH_UP |Se produit pendant l'attente d'un verrou de mise à jour. Cela n'inclut pas les verrous internes de tampons ni les verrous internes de marque de transaction. La liste des attentes de VERROUs \_ \* est disponible dans sys.dm_os_latch_stats. Notez que sys.dm_os_latch_stats regroupe les attentes LATCH_NL, LATCH_SH, LATCH_UP, LATCH_EX et LATCH_DT.| 
|LAZYWRITER_SLEEP |Se produit lorsque des tâches d’écriture différée sont suspendues. Il s'agit d'une mesure de la durée consacrée aux tâches en arrière-plan qui attendent. Ne considérez pas cet état lorsque vous cherchez des blocages d'utilisateur.| 
|LCK_M_BU |Se produit lorsqu'une tâche attend pour acquérir un verrou de mise à jour en bloc.| 
|LCK_M_BU_ABORT_BLOCKERS |Se produit lorsqu'une tâche attend pour acquérir un verrou de mise à jour en bloc avec des blocages d'abandon. (En relation avec l’option d’attente basse priorité de ALTER TABLE et ALTER INDEX.), <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|LCK_M_BU_LOW_PRIORITY |Se produit lorsqu'une tâche attend pour acquérir un verrou de mise à jour en bloc avec une priorité basse. (En relation avec l’option d’attente basse priorité de ALTER TABLE et ALTER INDEX.), <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|LCK_M_IS |Se produit lorsqu'une tâche attend pour acquérir un verrou intentionnel partagé.| 
|LCK_M_IS_ABORT_BLOCKERS |Se produit lorsqu'une tâche attend pour acquérir un verrou intentionnel partagé (IS) avec des blocages d'abandon. (En relation avec l’option d’attente basse priorité de ALTER TABLE et ALTER INDEX.), <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|LCK_M_IS_LOW_PRIORITY |Se produit lorsqu'une tâche attend pour acquérir un verrou intentionnel partagé (IS) avec une priorité basse. (En relation avec l’option d’attente basse priorité de ALTER TABLE et ALTER INDEX.), <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|LCK_M_IU |Se produit lorsqu'une tâche attend pour acquérir un verrou intentionnel de mise à jour.| 
|LCK_M_IU_ABORT_BLOCKERS |Se produit lorsqu'une tâche attend pour acquérir un verrou intentionnel de mise à jour (IU) avec des blocages d'abandon. (En relation avec l’option d’attente basse priorité de ALTER TABLE et ALTER INDEX.), <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|LCK_M_IU_LOW_PRIORITY |Se produit lorsqu'une tâche attend pour acquérir un verrou intentionnel de mise à jour (IU) avec une priorité basse. (En relation avec l’option d’attente basse priorité de ALTER TABLE et ALTER INDEX.), <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|LCK_M_IX |Se produit lorsqu'une tâche attend pour acquérir un verrou intentionnel exclusif.| 
|LCK_M_IX_ABORT_BLOCKERS |Se produit lorsqu'une tâche attend pour acquérir un verrou intentionnel exclusif (IX) avec des blocages d'abandon. (En relation avec l’option d’attente basse priorité de ALTER TABLE et ALTER INDEX.), <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|LCK_M_IX_LOW_PRIORITY |Se produit lorsqu'une tâche attend pour acquérir un verrou intentionnel exclusif (IX) avec une priorité basse. (En relation avec l’option d’attente basse priorité de ALTER TABLE et ALTER INDEX.), <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|LCK_M_RIn_NL |Se produit lorsqu'une tâche attend pour acquérir un verrou NULL sur la valeur de clé actuelle, et un verrou de groupes d'insertions entre la clé actuelle et la clé précédente. Un verrou NULL sur la clé est un verrou de libération instantanée.| 
|LCK_M_RIn_NL_ABORT_BLOCKERS |Se produit lorsqu'une tâche attend pour acquérir un verrou NULL avec des blocages d'abandon sur la valeur de clé actuelle, et un verrou de plage d'insertion avec des blocages d'abandon entre la clé actuelle et la clé précédente. Un verrou NULL sur la clé est un verrou de libération instantanée. (En relation avec l’option d’attente basse priorité de ALTER TABLE et ALTER INDEX.), <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|LCK_M_RIn_NL_LOW_PRIORITY |Se produit lorsqu'une tâche attend pour acquérir un verrou NULL avec une priorité base sur la valeur de clé actuelle, et un verrou de plage d'insertion avec une priorité basse entre la clé actuelle et la clé précédente. Un verrou NULL sur la clé est un verrou de libération instantanée. (En relation avec l’option d’attente basse priorité de ALTER TABLE et ALTER INDEX.), <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|LCK_M_RIn_S |Se produit lorsqu'une tâche attend pour acquérir un verrou partagé sur la valeur de clé actuelle, et un verrou de groupes d'insertions entre la clé actuelle et la clé précédente.| 
|LCK_M_RIn_S_ABORT_BLOCKERS |Se produit lorsqu'une tâche attend pour acquérir un verrou partagé avec des blocages d'abandon sur la valeur de clé actuelle, et un verrou de plage d'insertion avec des blocages d'abandon entre la clé actuelle et la clé précédente. (En relation avec l’option d’attente basse priorité de ALTER TABLE et ALTER INDEX.), <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|LCK_M_RIn_S_LOW_PRIORITY |Se produit lorsqu'une tâche attend pour acquérir un verrou partagé avec une priorité base sur la valeur de clé actuelle, et un verrou de plage d'insertion avec une priorité basse entre la clé actuelle et la clé précédente. (En relation avec l’option d’attente basse priorité de ALTER TABLE et ALTER INDEX.), <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|LCK_M_RIn_U |La tâche attend pour acquérir un verrou de mise à jour sur la valeur de clé actuelle, et un verrou de groupes d'insertions entre la clé actuelle et la clé précédente.| 
|LCK_M_RIn_U_ABORT_BLOCKERS |La tâche attend d'acquérir un verrou de mise à jour avec des blocages d'abandon sur la valeur de clé actuelle, et un verrou de plage d'insertion avec des blocages d'abandon entre la clé actuelle et la clé précédente. (En relation avec l’option d’attente basse priorité de ALTER TABLE et ALTER INDEX.), <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|LCK_M_RIn_U_LOW_PRIORITY |La tâche attend d'acquérir un verrou de mise à jour avec une priorité base sur la valeur de clé actuelle, et un verrou de plage d'insertion avec une priorité basse entre la clé actuelle et la clé précédente. (En relation avec l’option d’attente basse priorité de ALTER TABLE et ALTER INDEX.), <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|LCK_M_RIn_X |Se produit lorsqu'une tâche attend pour acquérir un verrou exclusif sur la valeur de clé actuelle, et un verrou de groupes d'insertions entre la clé actuelle et la clé précédente.| 
|LCK_M_RIn_X_ABORT_BLOCKERS |Se produit lorsqu'une tâche attend pour acquérir un verrou exclusif avec des blocages d'abandon sur la valeur de clé actuelle, et un verrou de plage d'insertion avec des blocages d'abandon entre la clé actuelle et la clé précédente. (En relation avec l’option d’attente basse priorité de ALTER TABLE et ALTER INDEX.), <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|LCK_M_RIn_X_LOW_PRIORITY |Se produit lorsqu'une tâche attend pour acquérir un verrou exclusif avec une priorité base sur la valeur de clé actuelle, et un verrou de plage d'insertion avec une priorité basse entre la clé actuelle et la clé précédente. (En relation avec l’option d’attente basse priorité de ALTER TABLE et ALTER INDEX.), <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|LCK_M_RS_S |Se produit lorsqu'une tâche attend pour acquérir un verrou partagé sur la valeur de clé actuelle, et un verrou de groupes partagés entre la clé actuelle et la clé précédente.| 
|LCK_M_RS_S_ABORT_BLOCKERS |Se produit lorsqu'une tâche attend pour acquérir un verrou partagé avec des blocages d'abandon sur la valeur de clé actuelle, et un verrou de plage partagée avec des blocages d'abandon entre la clé actuelle et la clé précédente. (En relation avec l’option d’attente basse priorité de ALTER TABLE et ALTER INDEX.), <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|LCK_M_RS_S_LOW_PRIORITY |Se produit lorsqu'une tâche attend pour acquérir un verrou partagé avec une priorité base sur la valeur de clé actuelle, et un verrou de plage partagée avec une priorité basse entre la clé actuelle et la clé précédente. (En relation avec l’option d’attente basse priorité de ALTER TABLE et ALTER INDEX.), <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|LCK_M_RS_U |Se produit lorsqu'une tâche attend pour acquérir un verrou de mise à jour sur la valeur de clé actuelle, et un verrou de groupes de mises à jour entre la clé actuelle et la clé précédente.| 
|LCK_M_RS_U_ABORT_BLOCKERS |Se produit lorsqu'une tâche attend pour acquérir un verrou de mise à jour avec des blocages d'abandon sur la valeur de clé actuelle, et un verrou de plage de mise à jour avec des blocages d'abandon entre la clé actuelle et la clé précédente. (En relation avec l’option d’attente basse priorité de ALTER TABLE et ALTER INDEX.), <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|LCK_M_RS_U_LOW_PRIORITY |Se produit lorsqu'une tâche attend pour acquérir un verrou de mise à jour avec une priorité base sur la valeur de clé actuelle, et un verrou de plage de mise à jour avec une priorité basse entre la clé actuelle et la clé précédente. (En relation avec l’option d’attente basse priorité de ALTER TABLE et ALTER INDEX.), <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|LCK_M_RX_S |Se produit lorsqu'une tâche attend pour acquérir un verrou partagé sur la valeur de clé actuelle, et un verrou de groupes exclusifs entre la clé actuelle et la clé précédente.| 
|LCK_M_RX_S_ABORT_BLOCKERS |Se produit lorsqu'une tâche attend pour acquérir un verrou de partage avec des blocages d'abandon sur la valeur de clé actuelle, et un verrou de plage exclusive avec des blocages d'abandon entre la clé actuelle et la clé précédente. (En relation avec l’option d’attente basse priorité de ALTER TABLE et ALTER INDEX.), <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|LCK_M_RX_S_LOW_PRIORITY |Se produit lorsqu'une tâche attend pour acquérir un verrou partagé avec une priorité base sur la valeur de clé actuelle, et un verrou de plage exclusive avec une priorité basse entre la clé actuelle et la clé précédente. (En relation avec l’option d’attente basse priorité de ALTER TABLE et ALTER INDEX.), <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|LCK_M_RX_U |Se produit lorsqu'une tâche attend pour acquérir un verrou de mise à jour sur la valeur de clé actuelle, et un verrou de groupes exclusifs entre la clé actuelle et la clé précédente.| 
|LCK_M_RX_U_ABORT_BLOCKERS |Se produit lorsqu'une tâche attend pour acquérir un verrou de mise à jour avec des blocages d'abandon sur la valeur de clé actuelle, et un verrou de plage exclusive avec des blocages d'abandon entre la clé actuelle et la clé précédente. (En relation avec l’option d’attente basse priorité de ALTER TABLE et ALTER INDEX.), <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|LCK_M_RX_U_LOW_PRIORITY |Se produit lorsqu'une tâche attend pour acquérir un verrou de mise à jour avec une priorité base sur la valeur de clé actuelle, et un verrou de plage exclusive avec une priorité basse entre la clé actuelle et la clé précédente. (En relation avec l’option d’attente basse priorité de ALTER TABLE et ALTER INDEX.), <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|LCK_M_RX_X |Se produit lorsqu'une tâche attend pour acquérir un verrou exclusif sur la valeur de clé actuelle, et un verrou de groupes exclusifs entre la clé actuelle et la clé précédente.| 
|LCK_M_RX_X_ABORT_BLOCKERS |Se produit lorsqu'une tâche attend pour acquérir un verrou exclusif avec des blocages d'abandon sur la valeur de clé actuelle, et un verrou de plage exclusive avec des blocages d'abandon entre la clé actuelle et la clé précédente. (En relation avec l’option d’attente basse priorité de ALTER TABLE et ALTER INDEX.), <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|LCK_M_RX_X_LOW_PRIORITY |Se produit lorsqu'une tâche attend pour acquérir un verrou exclusif avec une priorité base sur la valeur de clé actuelle, et un verrou de plage exclusive avec une priorité basse entre la clé actuelle et la clé précédente. (En relation avec l’option d’attente basse priorité de ALTER TABLE et ALTER INDEX.), <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|LCK_M_S |Se produit lorsqu'une tâche attend pour acquérir un verrou partagé.| 
|LCK_M_S_ABORT_BLOCKERS |Se produit lorsqu'une tâche attend pour acquérir un verrou partagé avec des blocages d'abandon. (En relation avec l’option d’attente basse priorité de ALTER TABLE et ALTER INDEX.), <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|LCK_M_S_LOW_PRIORITY |Se produit lorsqu'une tâche attend pour acquérir un verrou partagé avec une priorité basse. (En relation avec l’option d’attente basse priorité de ALTER TABLE et ALTER INDEX.), <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|LCK_M_SCH_M |Se produit lorsqu'une tâche attend pour acquérir un verrou de modification de schéma.| 
|LCK_M_SCH_M_ABORT_BLOCKERS |Se produit lorsqu'une tâche attend pour acquérir un verrou de modification de schéma avec des blocages d'abandon. (En relation avec l’option d’attente basse priorité de ALTER TABLE et ALTER INDEX.), <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|LCK_M_SCH_M_LOW_PRIORITY |Se produit lorsqu'une tâche attend pour acquérir un verrou de modification de schéma avec une priorité basse. (En relation avec l’option d’attente basse priorité de ALTER TABLE et ALTER INDEX.), <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|LCK_M_SCH_S |Se produit lorsqu'une tâche attend pour acquérir un verrou de partage de schéma.| 
|LCK_M_SCH_S_ABORT_BLOCKERS |Se produit lorsqu'une tâche attend pour acquérir un verrou de partage de schéma avec des blocages d'abandon. (En relation avec l’option d’attente basse priorité de ALTER TABLE et ALTER INDEX.), <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|LCK_M_SCH_S_LOW_PRIORITY |Se produit lorsqu'une tâche attend pour acquérir un verrou de partage de schéma avec une priorité basse. (En relation avec l’option d’attente basse priorité de ALTER TABLE et ALTER INDEX.), <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|LCK_M_SIU |Se produit lorsqu'une tâche attend pour acquérir un verrou intentionnel de mise à jour partagé.| 
|LCK_M_SIU_ABORT_BLOCKERS |Se produit lorsqu'une tâche attend pour acquérir un verrou intentionnel de mise à jour partagé avec des blocages d'abandon. (En relation avec l’option d’attente basse priorité de ALTER TABLE et ALTER INDEX.), <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|LCK_M_SIU_LOW_PRIORITY |Se produit lorsqu'une tâche attend pour acquérir un verrou intentionnel de mise à jour partagé avec une priorité basse. (En relation avec l’option d’attente basse priorité de ALTER TABLE et ALTER INDEX.), <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|LCK_M_SIX |Se produit lorsqu'une tâche attend pour acquérir un verrou intentionnel exclusif partagé.| 
|LCK_M_SIX_ABORT_BLOCKERS |Se produit lorsqu'une tâche attend pour acquérir un verrou intentionnel exclusif partagé avec des blocages d'abandon. (En relation avec l’option d’attente basse priorité de ALTER TABLE et ALTER INDEX.), <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|LCK_M_SIX_LOW_PRIORITY |Se produit lorsqu'une tâche attend pour acquérir un verrou intentionnel exclusif partagé avec une priorité basse. (En relation avec l’option d’attente basse priorité de ALTER TABLE et ALTER INDEX.), <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|LCK_M_U |Se produit lorsqu'une tâche attend pour acquérir un verrou de mise à jour.| 
|LCK_M_U_ABORT_BLOCKERS |Se produit lorsqu'une tâche attend pour acquérir un verrou de mise à jour avec des blocages d'abandon. (En relation avec l’option d’attente basse priorité de ALTER TABLE et ALTER INDEX.), <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|LCK_M_U_LOW_PRIORITY |Se produit lorsqu'une tâche attend pour acquérir un verrou de mise à jour avec une priorité basse. (En relation avec l’option d’attente basse priorité de ALTER TABLE et ALTER INDEX.), <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|LCK_M_UIX |Se produit lorsqu'une tâche attend pour acquérir un verrou intentionnel exclusif de mise à jour.| 
|LCK_M_UIX_ABORT_BLOCKERS |Se produit lorsqu'une tâche attend pour acquérir un verrou intentionnel exclusif de mise à jour avec des blocages d'abandon. (En relation avec l’option d’attente basse priorité de ALTER TABLE et ALTER INDEX.), <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|LCK_M_UIX_LOW_PRIORITY |Se produit lorsqu'une tâche attend pour acquérir un verrou intentionnel exclusif de mise à jour avec une priorité basse. (En relation avec l’option d’attente basse priorité de ALTER TABLE et ALTER INDEX.), <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|LCK_M_X |Se produit lorsqu'une tâche attend pour acquérir un verrou exclusif.| 
|LCK_M_X_ABORT_BLOCKERS |Se produit lorsqu'une tâche attend pour acquérir un verrou exclusif avec des blocages d'abandon. (En relation avec l’option d’attente basse priorité de ALTER TABLE et ALTER INDEX.), <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|LCK_M_X_LOW_PRIORITY |Se produit lorsqu'une tâche attend pour acquérir un verrou exclusif avec une priorité basse. (En relation avec l’option d’attente basse priorité de ALTER TABLE et ALTER INDEX.), <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|LOG_POOL_SCAN |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] et versions ultérieures.| 
|LOG_RATE_GOVERNOR |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] et versions ultérieures.| 
|LOGBUFFER |Se produit lorsqu'une tâche attend de l'espace dans le tampon de journal pour stocker un enregistrement de journal. Des valeurs élevées fréquentes peuvent indiquer que les unités de journaux ne parviennent pas à gérer la quantité d'entrées de journaux produites par le serveur.| 
|LOGCAPTURE_LOGPOOLTRUNCPOINT |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|LOGGENERATION |Identifié à titre d'information uniquement. Non pris en charge. La compatibilité future n'est pas garantie.| 
|LOGMGR |Se produit lorsqu'une tâche attend la fin des E/S de journal en cours avant d'arrêter le journal lors de la fermeture de la base de données.| 
|LOGMGR_FLUSH |Identifié à titre d'information uniquement. Non pris en charge. La compatibilité future n'est pas garantie.| 
|LOGMGR_PMM_LOG |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] et versions ultérieures.| 
|LOGMGR_QUEUE |Se produit lorsque la tâche d'écriture du journal attend des demandes de travail.| 
|LOGMGR_RESERVE_APPEND |Se produit lorsqu'une tâche attend de voir si la troncature du journal libère de l'espace pour lui permettre d'écrire un nouvel enregistrement dans le journal. Vous pouvez éventuellement augmenter la taille des fichiers journaux correspondant à la base de données concernée pour réduire cette attente.| 
|LOGPOOL_CACHESIZE |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|LOGPOOL_CONSUMER |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|LOGPOOL_CONSUMERSET |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|LOGPOOL_FREEPOOLS |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|LOGPOOL_MGRSET |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|LOGPOOL_REPLACEMENTSET |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|LOGPOOLREFCOUNTEDOBJECT_REFDONE |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|LOWFAIL_MEMMGR_QUEUE |Se produit lorsque vous attendez que la mémoire soit disponible afin d'être utilisée.| 
|MD_AGENT_YIELD |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|MD_LAZYCACHE_RWLOCK |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|MEMORY_ALLOCATION_EXT |Se produit lors de l’allocation de mémoire à partir du [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pool de mémoire interne ou du système d’exploitation. <br /><br /> **S’applique à** : [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] et versions ultérieures.| 
|MEMORY_GRANT_UPDATE |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] et versions ultérieures.| 
|METADATA_LAZYCACHE_RWLOCK |À usage interne uniquement <br /><br /> **S’applique à**: [!INCLUDE[ssKilimanjaro_md](../../includes/sskilimanjaro-md.md)] uniquement. |  
|MIGRATIONBUFFER |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] et versions ultérieures.| 
|MISCELLANEOUS |Identifié à titre d'information uniquement. Non pris en charge. La compatibilité future n'est pas garantie.| 
|MISCELLANEOUS |Identifié à titre d'information uniquement. Non pris en charge. La compatibilité future n'est pas garantie.| 
|MSQL_DQ |Se produit lorsqu'une tâche attend la fin d'une opération de requête distribuée. Permet de détecter d'éventuels interblocages d'application MARS (Multiple Active Result Set). L'attente se termine à la fin de l'appel de requête distribuée.| 
|MSQL_XACT_MGR_MUTEX |Se produit lorsqu'une tâche attend d'avoir obtenu la propriété du gestionnaire de transactions de la session pour effectuer une opération de transaction au niveau de la session.| 
|MSQL_XACT_MUTEX |Se produit durant la synchronisation de l'utilisation de la transaction. Une demande doit d'abord obtenir l'exclusion mutuelle pour pouvoir utiliser la transaction.| 
|MSQL_XP |Se produit lorsqu'une tâche attend la fin d'une procédure stockée étendue. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilise cet état d'attente pour détecter d'éventuels blocages d'application MARS. L'attente se termine à la fin de l'appel de procédure stockée étendue.| 
|MSSEARCH |Se produit durant des appels de recherche en texte intégral. Cette attente se termine lorsque l'opération de texte intégral prend fin. Ces informations n'indiquent pas des contentions, mais plutôt la durée des opérations de texte intégral.| 
|NET_WAITFOR_PACKET |Se produit lorsqu'une connexion attend un paquet réseau durant une lecture sur le réseau.| 
|NETWORKSXMLMGRLOAD |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] et versions ultérieures.| 
|NODE_CACHE_MUTEX |À usage interne uniquement| 
|OLEDB |Se produit lorsque [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] appelle le fournisseur SNAC OLE DB (SQLNCLI) ou le pilote de OLE DB Microsoft pour SQL Server (MSOLEDBSQL). Ce type d'attente n'est pas utilisé pour la synchronisation. Par contre, il indique la durée des appels émis vers le fournisseur OLE DB.| 
|ONDEMAND_TASK_QUEUE |Se produit lorsqu'une tâche en arrière-plan attend des demandes de tâches système à priorité élevée. De longues attentes indiquent qu'il n'existe aucune demande à priorité élevée à traiter, et elles ne révèlent pas l'existence d'un problème.| 
|PAGEIOLATCH_DT |Se produit lorsqu'une tâche attend sur un verrou interne un tampon qui est une demande d'E/S. La demande de verrou interne est en mode de destruction. De longues attentes peuvent indiquer l'existence de problèmes au niveau du sous-système de disque.| 
|PAGEIOLATCH_EX |Se produit lorsqu'une tâche attend sur un verrou interne un tampon qui est une demande d'E/S. La demande de verrou interne est en mode exclusif. De longues attentes peuvent indiquer l'existence de problèmes au niveau du sous-système de disque.| 
|PAGEIOLATCH_KP |Se produit lorsqu'une tâche attend sur un verrou interne un tampon qui est une demande d'E/S. La demande de verrou interne est en mode de conservation. De longues attentes peuvent indiquer l'existence de problèmes au niveau du sous-système de disque.| 
|PAGEIOLATCH_NL |Identifié à titre d'information uniquement. Non pris en charge. La compatibilité future n'est pas garantie.| 
|PAGEIOLATCH_SH |Se produit lorsqu'une tâche attend sur un verrou interne un tampon qui est une demande d'E/S. La demande de verrou interne est en mode partagé. De longues attentes peuvent indiquer l'existence de problèmes au niveau du sous-système de disque.| 
|PAGEIOLATCH_UP |Se produit lorsqu'une tâche attend sur un verrou interne un tampon qui est une demande d'E/S. La demande de verrou interne est en mode de mise à jour. De longues attentes peuvent indiquer l'existence de problèmes au niveau du sous-système de disque.| 
|PAGELATCH_DT |Se produit lorsqu'une tâche attend sur un verrou interne un tampon qui n'est pas une demande d'E/S. La demande de verrou interne est en mode de destruction.| 
|PAGELATCH_EX |Se produit lorsqu'une tâche attend sur un verrou interne un tampon qui n'est pas une demande d'E/S. La demande de verrou interne est en mode exclusif.| 
|PAGELATCH_KP |Se produit lorsqu'une tâche attend sur un verrou interne un tampon qui n'est pas une demande d'E/S. La demande de verrou interne est en mode de conservation.| 
|PAGELATCH_NL |Identifié à titre d'information uniquement. Non pris en charge. La compatibilité future n'est pas garantie.| 
|PAGELATCH_SH |Se produit lorsqu'une tâche attend sur un verrou interne un tampon qui n'est pas une demande d'E/S. La demande de verrou interne est en mode partagé.| 
|PAGELATCH_UP |Se produit lorsqu'une tâche attend sur un verrou interne un tampon qui n'est pas une demande d'E/S. La demande de verrou interne est en mode de mise à jour.| 
|PARALLEL_BACKUP_QUEUE |Se produit lors de la sérialisation de la sortie générée par RESTORE HEADERONLY, RESTORE FILELISTONLY ou RESTORE LABELONLY.| 
|PARALLEL_REDO_DRAIN_WORKER |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] et versions ultérieures.| 
|PARALLEL_REDO_FLOW_CONTROL |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] et versions ultérieures.| 
|PARALLEL_REDO_LOG_CACHE |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] et versions ultérieures.| 
|PARALLEL_REDO_TRAN_LIST |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] et versions ultérieures.| 
|PARALLEL_REDO_TRAN_TURN |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] et versions ultérieures.| 
|PARALLEL_REDO_WORKER_SYNC |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] et versions ultérieures.| 
|PARALLEL_REDO_WORKER_WAIT_WORK |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] et versions ultérieures.| 
|PERFORMANCE_COUNTERS_RWLOCK |À usage interne uniquement| 
|PHYSICAL_SEEDING_DMV |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|POOL_LOG_RATE_GOVERNOR |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] et versions ultérieures.| 
|PREEMPTIVE_ABR |Identifié à titre d'information uniquement. Non pris en charge. La compatibilité future n'est pas garantie.| 
|PREEMPTIVE_AUDIT_ACCESS_EVENTLOG |Se produit lorsque le planificateur du système d'exploitation [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] (SQLOS) bascule en mode préemptif pour écrire un événement d'audit dans le journal des événements Windows. <br /><br /> **S’applique à**: [!INCLUDE[ssKilimanjaro_md](../../includes/sskilimanjaro-md.md)] uniquement. |  
|PREEMPTIVE_AUDIT_ACCESS_SECLOG |Se produit lorsque le planificateur du système d'exploitation (SQLOS) bascule en mode préemptif pour écrire un événement d'audit dans le journal de sécurité Windows. <br /><br /> **S’applique à**: [!INCLUDE[ssKilimanjaro_md](../../includes/sskilimanjaro-md.md)] uniquement. |  
|PREEMPTIVE_CLOSEBACKUPMEDIA |Se produit lorsque le planificateur du système d'exploitation (SQLOS) bascule en mode préemptif pour fermer le support de sauvegarde.| 
|PREEMPTIVE_CLOSEBACKUPTAPE |Se produit lorsque le planificateur du système d'exploitation (SQLOS) bascule en mode préemptif pour fermer une unité de sauvegarde sur bande.| 
|PREEMPTIVE_CLOSEBACKUPVDIDEVICE |Se produit lorsque le planificateur du système d'exploitation (SQLOS) bascule en mode préemptif pour fermer une unité de sauvegarde virtuelle.| 
|PREEMPTIVE_CLUSAPI_CLUSTERRESOURCECONTROL |Se produit lorsque le planificateur du système d'exploitation (SQLOS) bascule en mode préemptif pour effectuer des opérations de cluster de basculement Windows.| 
|PREEMPTIVE_COM_COCREATEINSTANCE |Se produit lorsque le planificateur du système d'exploitation (SQLOS) bascule en mode préemptif pour créer un objet COM.| 
|PREEMPTIVE_COM_COGETCLASSOBJECT |À usage interne uniquement| 
|PREEMPTIVE_COM_CREATEACCESSOR |À usage interne uniquement| 
|PREEMPTIVE_COM_DELETEROWS |À usage interne uniquement| 
|PREEMPTIVE_COM_GETCOMMANDTEXT |À usage interne uniquement| 
|PREEMPTIVE_COM_GETDATA |À usage interne uniquement| 
|PREEMPTIVE_COM_GETNEXTROWS |À usage interne uniquement| 
|PREEMPTIVE_COM_GETRESULT |À usage interne uniquement| 
|PREEMPTIVE_COM_GETROWSBYBOOKMARK |À usage interne uniquement| 
|PREEMPTIVE_COM_LBFLUSH |À usage interne uniquement| 
|PREEMPTIVE_COM_LBLOCKREGION |À usage interne uniquement| 
|PREEMPTIVE_COM_LBREADAT |À usage interne uniquement| 
|PREEMPTIVE_COM_LBSETSIZE |À usage interne uniquement| 
|PREEMPTIVE_COM_LBSTAT |À usage interne uniquement| 
|PREEMPTIVE_COM_LBUNLOCKREGION |À usage interne uniquement| 
|PREEMPTIVE_COM_LBWRITEAT |À usage interne uniquement| 
|PREEMPTIVE_COM_QUERYINTERFACE |À usage interne uniquement| 
|PREEMPTIVE_COM_RELEASE |À usage interne uniquement| 
|PREEMPTIVE_COM_RELEASEACCESSOR |À usage interne uniquement| 
|PREEMPTIVE_COM_RELEASEROWS |À usage interne uniquement| 
|PREEMPTIVE_COM_RELEASESESSION |À usage interne uniquement| 
|PREEMPTIVE_COM_RESTARTPOSITION |À usage interne uniquement| 
|PREEMPTIVE_COM_SEQSTRMREAD |À usage interne uniquement| 
|PREEMPTIVE_COM_SEQSTRMREADANDWRITE |À usage interne uniquement| 
|PREEMPTIVE_COM_SETDATAFAILURE |À usage interne uniquement| 
|PREEMPTIVE_COM_SETPARAMETERINFO |À usage interne uniquement| 
|PREEMPTIVE_COM_SETPARAMETERPROPERTIES |À usage interne uniquement| 
|PREEMPTIVE_COM_STRMLOCKREGION |À usage interne uniquement| 
|PREEMPTIVE_COM_STRMSEEKANDREAD |À usage interne uniquement| 
|PREEMPTIVE_COM_STRMSEEKANDWRITE |À usage interne uniquement| 
|PREEMPTIVE_COM_STRMSETSIZE |À usage interne uniquement| 
|PREEMPTIVE_COM_STRMSTAT |À usage interne uniquement| 
|PREEMPTIVE_COM_STRMUNLOCKREGION |À usage interne uniquement| 
|PREEMPTIVE_CONSOLEWRITE |À usage interne uniquement| 
|PREEMPTIVE_CREATEPARAM |À usage interne uniquement| 
|PREEMPTIVE_DEBUG |À usage interne uniquement| 
|PREEMPTIVE_DFSADDLINK |À usage interne uniquement| 
|PREEMPTIVE_DFSLINKEXISTCHECK |À usage interne uniquement| 
|PREEMPTIVE_DFSLINKHEALTHCHECK |À usage interne uniquement| 
|PREEMPTIVE_DFSREMOVELINK |À usage interne uniquement| 
|PREEMPTIVE_DFSREMOVEROOT |À usage interne uniquement| 
|PREEMPTIVE_DFSROOTFOLDERCHECK |À usage interne uniquement| 
|PREEMPTIVE_DFSROOTINIT |À usage interne uniquement| 
|PREEMPTIVE_DFSROOTSHARECHECK |À usage interne uniquement| 
|PREEMPTIVE_DTC_ABORT |À usage interne uniquement| 
|PREEMPTIVE_DTC_ABORTREQUESTDONE |À usage interne uniquement| 
|PREEMPTIVE_DTC_BEGINTRANSACTION |À usage interne uniquement| 
|PREEMPTIVE_DTC_COMMITREQUESTDONE |À usage interne uniquement| 
|PREEMPTIVE_DTC_ENLIST |À usage interne uniquement| 
|PREEMPTIVE_DTC_PREPAREREQUESTDONE |À usage interne uniquement| 
|PREEMPTIVE_FILESIZEGET |À usage interne uniquement| 
|PREEMPTIVE_FSAOLEDB_ABORTTRANSACTION |À usage interne uniquement| 
|PREEMPTIVE_FSAOLEDB_COMMITTRANSACTION |À usage interne uniquement| 
|PREEMPTIVE_FSAOLEDB_STARTTRANSACTION |À usage interne uniquement| 
|PREEMPTIVE_FSRECOVER_UNCONDITIONALUNDO |À usage interne uniquement| 
|PREEMPTIVE_GETRMINFO |À usage interne uniquement| 
|PREEMPTIVE_HADR_LEASE_MECHANISM |Always On la planification du gestionnaire de bail du groupe de disponibilité pour les diagnostics Support Microsoft. <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|PREEMPTIVE_HTTP_EVENT_WAIT |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] et versions ultérieures.| 
|PREEMPTIVE_HTTP_REQUEST |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] et versions ultérieures.| 
|PREEMPTIVE_LOCKMONITOR |À usage interne uniquement| 
|PREEMPTIVE_MSS_RELEASE |À usage interne uniquement| 
|PREEMPTIVE_ODBCOPS |À usage interne uniquement| 
|PREEMPTIVE_OLE_UNINIT |À usage interne uniquement| 
|PREEMPTIVE_OLEDB_ABORTORCOMMITTRAN |À usage interne uniquement| 
|PREEMPTIVE_OLEDB_ABORTTRAN |À usage interne uniquement| 
|PREEMPTIVE_OLEDB_GETDATASOURCE |À usage interne uniquement| 
|PREEMPTIVE_OLEDB_GETLITERALINFO |À usage interne uniquement| 
|PREEMPTIVE_OLEDB_GETPROPERTIES |À usage interne uniquement| 
|PREEMPTIVE_OLEDB_GETPROPERTYINFO |À usage interne uniquement| 
|PREEMPTIVE_OLEDB_GETSCHEMALOCK |À usage interne uniquement| 
|PREEMPTIVE_OLEDB_JOINTRANSACTION |À usage interne uniquement| 
|PREEMPTIVE_OLEDB_RELEASE |À usage interne uniquement| 
|PREEMPTIVE_OLEDB_SETPROPERTIES |À usage interne uniquement| 
|PREEMPTIVE_OLEDBOPS |À usage interne uniquement| 
|PREEMPTIVE_OS_ACCEPTSECURITYCONTEXT |À usage interne uniquement| 
|PREEMPTIVE_OS_ACQUIRECREDENTIALSHANDLE |À usage interne uniquement| 
|PREEMPTIVE_OS_AUTHENTICATIONOPS |À usage interne uniquement| 
|PREEMPTIVE_OS_AUTHORIZATIONOPS |À usage interne uniquement| 
|PREEMPTIVE_OS_AUTHZGETINFORMATIONFROMCONTEXT |À usage interne uniquement| 
|PREEMPTIVE_OS_AUTHZINITIALIZECONTEXTFROMSID |À usage interne uniquement| 
|PREEMPTIVE_OS_AUTHZINITIALIZERESOURCEMANAGER |À usage interne uniquement| 
|PREEMPTIVE_OS_BACKUPREAD |À usage interne uniquement| 
|PREEMPTIVE_OS_CLOSEHANDLE |À usage interne uniquement| 
|PREEMPTIVE_OS_CLUSTEROPS |À usage interne uniquement| 
|PREEMPTIVE_OS_COMOPS |À usage interne uniquement| 
|PREEMPTIVE_OS_COMPLETEAUTHTOKEN |À usage interne uniquement| 
|PREEMPTIVE_OS_COPYFILE |À usage interne uniquement| 
|PREEMPTIVE_OS_CREATEDIRECTORY |À usage interne uniquement| 
|PREEMPTIVE_OS_CREATEFILE |À usage interne uniquement| 
|PREEMPTIVE_OS_CRYPTACQUIRECONTEXT |À usage interne uniquement| 
|PREEMPTIVE_OS_CRYPTIMPORTKEY |À usage interne uniquement| 
|PREEMPTIVE_OS_CRYPTOPS |À usage interne uniquement| 
|PREEMPTIVE_OS_DECRYPTMESSAGE |À usage interne uniquement| 
|PREEMPTIVE_OS_DELETEFILE |À usage interne uniquement| 
|PREEMPTIVE_OS_DELETESECURITYCONTEXT |À usage interne uniquement| 
|PREEMPTIVE_OS_DEVICEIOCONTROL |À usage interne uniquement| 
|PREEMPTIVE_OS_DEVICEOPS |À usage interne uniquement| 
|PREEMPTIVE_OS_DIRSVC_NETWORKOPS |À usage interne uniquement| 
|PREEMPTIVE_OS_DISCONNECTNAMEDPIPE |À usage interne uniquement| 
|PREEMPTIVE_OS_DOMAINSERVICESOPS |À usage interne uniquement| 
|PREEMPTIVE_OS_DSGETDCNAME |À usage interne uniquement| 
|PREEMPTIVE_OS_DTCOPS |À usage interne uniquement| 
|PREEMPTIVE_OS_ENCRYPTMESSAGE |À usage interne uniquement| 
|PREEMPTIVE_OS_FILEOPS |À usage interne uniquement| 
|PREEMPTIVE_OS_FINDFILE |À usage interne uniquement| 
|PREEMPTIVE_OS_FLUSHFILEBUFFERS |À usage interne uniquement| 
|PREEMPTIVE_OS_FORMATMESSAGE |À usage interne uniquement| 
|PREEMPTIVE_OS_FREECREDENTIALSHANDLE |À usage interne uniquement| 
|PREEMPTIVE_OS_FREELIBRARY |À usage interne uniquement| 
|PREEMPTIVE_OS_GENERICOPS |À usage interne uniquement| 
|PREEMPTIVE_OS_GETADDRINFO |À usage interne uniquement| 
|PREEMPTIVE_OS_GETCOMPRESSEDFILESIZE |À usage interne uniquement| 
|PREEMPTIVE_OS_GETDISKFREESPACE |À usage interne uniquement| 
|PREEMPTIVE_OS_GETFILEATTRIBUTES |À usage interne uniquement| 
|PREEMPTIVE_OS_GETFILESIZE |À usage interne uniquement| 
|PREEMPTIVE_OS_GETFINALFILEPATHBYHANDLE |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] et versions ultérieures.| 
|PREEMPTIVE_OS_GETLONGPATHNAME |À usage interne uniquement| 
|PREEMPTIVE_OS_GETPROCADDRESS |À usage interne uniquement| 
|PREEMPTIVE_OS_GETVOLUMENAMEFORVOLUMEMOUNTPOINT |À usage interne uniquement| 
|PREEMPTIVE_OS_GETVOLUMEPATHNAME |À usage interne uniquement| 
|PREEMPTIVE_OS_INITIALIZESECURITYCONTEXT |À usage interne uniquement| 
|PREEMPTIVE_OS_LIBRARYOPS |À usage interne uniquement| 
|PREEMPTIVE_OS_LOADLIBRARY |À usage interne uniquement| 
|PREEMPTIVE_OS_LOGONUSER |À usage interne uniquement| 
|PREEMPTIVE_OS_LOOKUPACCOUNTSID |À usage interne uniquement| 
|PREEMPTIVE_OS_MESSAGEQUEUEOPS |À usage interne uniquement| 
|PREEMPTIVE_OS_MOVEFILE |À usage interne uniquement| 
|PREEMPTIVE_OS_NETGROUPGETUSERS |À usage interne uniquement| 
|PREEMPTIVE_OS_NETLOCALGROUPGETMEMBERS |À usage interne uniquement| 
|PREEMPTIVE_OS_NETUSERGETGROUPS |À usage interne uniquement| 
|PREEMPTIVE_OS_NETUSERGETLOCALGROUPS |À usage interne uniquement| 
|PREEMPTIVE_OS_NETUSERMODALSGET |À usage interne uniquement| 
|PREEMPTIVE_OS_NETVALIDATEPASSWORDPOLICY |À usage interne uniquement| 
|PREEMPTIVE_OS_NETVALIDATEPASSWORDPOLICYFREE |À usage interne uniquement| 
|PREEMPTIVE_OS_OPENDIRECTORY |À usage interne uniquement| 
|PREEMPTIVE_OS_PDH_WMI_INIT |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|PREEMPTIVE_OS_PIPEOPS |À usage interne uniquement| 
|PREEMPTIVE_OS_PROCESSOPS |À usage interne uniquement| 
|PREEMPTIVE_OS_QUERYCONTEXTATTRIBUTES |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|PREEMPTIVE_OS_QUERYREGISTRY |À usage interne uniquement| 
|PREEMPTIVE_OS_QUERYSECURITYCONTEXTTOKEN |À usage interne uniquement| 
|PREEMPTIVE_OS_REMOVEDIRECTORY |À usage interne uniquement| 
|PREEMPTIVE_OS_REPORTEVENT |À usage interne uniquement| 
|PREEMPTIVE_OS_REVERTTOSELF |À usage interne uniquement| 
|PREEMPTIVE_OS_RSFXDEVICEOPS |À usage interne uniquement| 
|PREEMPTIVE_OS_SECURITYOPS |À usage interne uniquement| 
|PREEMPTIVE_OS_SERVICEOPS |À usage interne uniquement| 
|PREEMPTIVE_OS_SETENDOFFILE |À usage interne uniquement| 
|PREEMPTIVE_OS_SETFILEPOINTER |À usage interne uniquement| 
|PREEMPTIVE_OS_SETFILEVALIDDATA |À usage interne uniquement| 
|PREEMPTIVE_OS_SETNAMEDSECURITYINFO |À usage interne uniquement| 
|PREEMPTIVE_OS_SQLCLROPS |À usage interne uniquement| 
|PREEMPTIVE_OS_SQMLAUNCH |À usage interne uniquement <br /><br /> **S'applique à**: [!INCLUDE[ssKilimanjaro_md](../../includes/sskilimanjaro-md.md)] jusqu'à [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)]. |  
|PREEMPTIVE_OS_VERIFYSIGNATURE |À usage interne uniquement| 
|PREEMPTIVE_OS_VERIFYTRUST |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] et versions ultérieures.| 
|PREEMPTIVE_OS_VSSOPS |À usage interne uniquement| 
|PREEMPTIVE_OS_WAITFORSINGLEOBJECT |À usage interne uniquement| 
|PREEMPTIVE_OS_WINSOCKOPS |À usage interne uniquement| 
|PREEMPTIVE_OS_WRITEFILE |À usage interne uniquement| 
|PREEMPTIVE_OS_WRITEFILEGATHER |À usage interne uniquement| 
|PREEMPTIVE_OS_WSASETLASTERROR |À usage interne uniquement| 
|PREEMPTIVE_REENLIST |À usage interne uniquement| 
|PREEMPTIVE_RESIZELOG |À usage interne uniquement| 
|PREEMPTIVE_ROLLFORWARDREDO |À usage interne uniquement| 
|PREEMPTIVE_ROLLFORWARDUNDO |À usage interne uniquement| 
|PREEMPTIVE_SB_STOPENDPOINT |À usage interne uniquement| 
|PREEMPTIVE_SERVER_STARTUP |À usage interne uniquement| 
|PREEMPTIVE_SETRMINFO |À usage interne uniquement| 
|PREEMPTIVE_SHAREDMEM_GETDATA |À usage interne uniquement| 
|PREEMPTIVE_SNIOPEN |À usage interne uniquement| 
|PREEMPTIVE_SOSHOST |À usage interne uniquement| 
|PREEMPTIVE_SOSTESTING |Identifié à titre d'information uniquement. Non pris en charge. La compatibilité future n'est pas garantie.| 
|PREEMPTIVE_SP_SERVER_DIAGNOSTICS |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|PREEMPTIVE_STARTRM |À usage interne uniquement| 
|PREEMPTIVE_STREAMFCB_CHECKPOINT |À usage interne uniquement| 
|PREEMPTIVE_STREAMFCB_RECOVER |À usage interne uniquement| 
|PREEMPTIVE_STRESSDRIVER |Identifié à titre d'information uniquement. Non pris en charge. La compatibilité future n'est pas garantie.| 
|PREEMPTIVE_TESTING |Identifié à titre d'information uniquement. Non pris en charge. La compatibilité future n'est pas garantie.| 
|PREEMPTIVE_TRANSIMPORT |À usage interne uniquement| 
|PREEMPTIVE_UNMARSHALPROPAGATIONTOKEN |À usage interne uniquement| 
|PREEMPTIVE_VSS_CREATESNAPSHOT |À usage interne uniquement| 
|PREEMPTIVE_VSS_CREATEVOLUMESNAPSHOT |À usage interne uniquement| 
|PREEMPTIVE_XE_CALLBACKEXECUTE |À usage interne uniquement| 
|PREEMPTIVE_XE_CX_FILE_OPEN |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|PREEMPTIVE_XE_CX_HTTP_CALL |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|PREEMPTIVE_XE_DISPATCHER |À usage interne uniquement| 
|PREEMPTIVE_XE_ENGINEINIT |À usage interne uniquement| 
|PREEMPTIVE_XE_GETTARGETSTATE |À usage interne uniquement| 
|PREEMPTIVE_XE_SESSIONCOMMIT |À usage interne uniquement| 
|PREEMPTIVE_XE_TARGETFINALIZE |À usage interne uniquement| 
|PREEMPTIVE_XE_TARGETINIT |À usage interne uniquement| 
|PREEMPTIVE_XE_TIMERRUN |À usage interne uniquement| 
|PREEMPTIVE_XETESTING |Identifié à titre d'information uniquement. Non pris en charge. La compatibilité future n'est pas garantie.| 
|PRINT_ROLLBACK_PROGRESS |Utilisé pour attendre que des processus utilisateur se terminent dans une base de données qui a subi un changement d'état suite à l'utilisation de la clause de terminaison ALTER DATABASE. Pour plus d’informations, consultez ALTER DATABASE (Transact-SQL).| 
|PRU_ROLLBACK_DEFERRED |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|PWAIT_ALL_COMPONENTS_INITIALIZED |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|PWAIT_COOP_SCAN |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|PWAIT_DIRECTLOGCONSUMER_GETNEXT |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] et versions ultérieures.| 
|PWAIT_EVENT_SESSION_INIT_MUTEX |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|PWAIT_FABRIC_REPLICA_CONTROLLER_DATA_LOSS |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] et versions ultérieures.| 
|PWAIT_HADR_ACTION_COMPLETED |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|PWAIT_HADR_CHANGE_NOTIFIER_TERMINATION_SYNC |Se produit lorsqu'une tâche en arrière-plan attend la fin de la tâche en arrière-plan qui reçoit (par interrogation) des notifications de clustering de basculement Windows Server. <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|PWAIT_HADR_CLUSTER_INTEGRATION |Une opération d’ajout, de remplacement et/ou de suppression attend de saisir un verrou d’écriture sur une liste interne Always On (par exemple, une liste de réseaux, d’adresses réseau ou d’écouteurs de groupe de disponibilité). Usage interne uniquement, <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|PWAIT_HADR_FAILOVER_COMPLETED |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|PWAIT_HADR_JOIN |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|PWAIT_HADR_OFFLINE_COMPLETED |Une opération Drop Availability Group Always On attend que le groupe de disponibilité cible se déconnecte avant de détruire les objets de clustering de basculement Windows Server. <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|PWAIT_HADR_ONLINE_COMPLETED |Une opération de création ou de basculement de groupe de disponibilité Always On attend que le groupe de disponibilité cible soit mis en ligne. <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|PWAIT_HADR_POST_ONLINE_COMPLETED |Une opération Drop Availability Group Always On attend l’arrêt d’une tâche en arrière-plan qui était planifiée dans le cadre d’une commande précédente. Par exemple, une tâche en arrière-plan peut effectuer la transition de bases de données de disponibilité vers le rôle principal. La DLL DROP AVAILABILITY GROUP doit attendre la fin de cette tâche en arrière-plan afin d'éviter des conditions de concurrence. <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|PWAIT_HADR_SERVER_READY_CONNECTIONS |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|PWAIT_HADR_WORKITEM_COMPLETED |Attente interne par un thread attendant la fin d'une tâche de travail asynchrone. Il s'agit d'une attente prévue, à l'usage de CSS. <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|PWAIT_HADRSIM |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] et versions ultérieures.| 
|PWAIT_LOG_CONSOLIDATION_IO |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|PWAIT_LOG_CONSOLIDATION_POLL |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|PWAIT_MD_LOGIN_STATS |Se produit durant la synchronisation interne dans les métadonnées des statistiques de connexion. <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|PWAIT_MD_RELATION_CACHE |Se produit durant la synchronisation interne dans les métadonnées de la table ou de l'index. <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|PWAIT_MD_SERVER_CACHE |Se produit durant la synchronisation interne dans les métadonnées sur les serveurs liés. <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|PWAIT_MD_UPGRADE_CONFIG |Se produit durant la synchronisation interne lors de la mise à niveau des configurations à l'échelle du serveur. <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|PWAIT_PREEMPTIVE_APP_USAGE_TIMER |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] et versions ultérieures.| 
|PWAIT_PREEMPTIVE_AUDIT_ACCESS_WINDOWSLOG |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|PWAIT_QRY_BPMEMORY |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|PWAIT_REPLICA_ONLINE_INIT_MUTEX |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|PWAIT_RESOURCE_SEMAPHORE_FT_PARALLEL_QUERY_SYNC |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|PWAIT_SBS_FILE_OPERATION |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] et versions ultérieures.| 
|PWAIT_XTP_FSSTORAGE_MAINTENANCE |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|PWAIT_XTP_HOST_STORAGE_WAIT |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|QDS_ASYNC_CHECK_CONSISTENCY_TASK |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|QDS_ASYNC_PERSIST_TASK |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|QDS_ASYNC_PERSIST_TASK_START |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|QDS_ASYNC_QUEUE |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] et versions ultérieures.| 
|QDS_BCKG_TASK |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|QDS_BLOOM_FILTER |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] et versions ultérieures.| 
|QDS_CLEANUP_STALE_QUERIES_TASK_MAIN_LOOP_SLEEP |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|QDS_CTXS |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|QDS_DB_DISK |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|QDS_DYN_VECTOR |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|QDS_EXCLUSIVE_ACCESS |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] et versions ultérieures.| 
|QDS_HOST_INIT |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] et versions ultérieures.| 
|QDS_LOADDB |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|QDS_PERSIST_TASK_MAIN_LOOP_SLEEP |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|QDS_QDS_CAPTURE_INIT |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] et versions ultérieures.| 
|QDS_SHUTDOWN_QUEUE |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|QDS_STMT |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|QDS_STMT_DISK |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|QDS_TASK_SHUTDOWN |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|QDS_TASK_START |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|QE_WARN_LIST_SYNC |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|QPJOB_KILL |Indique qu'une mise à jour asynchrone des statistiques automatiques a été annulée par un appel à KILL alors que la mise à jour commençait. Le thread de terminaison est suspendu, en attendant qu'il commence à écouter les commandes KILL. Une valeur idéale est inférieure à une seconde.| 
|QPJOB_WAITFOR_ABORT |Indique qu'une mise à jour asynchrone des statistiques automatiques a été annulée par un appel à KILL lors de son exécution. La mise à jour est maintenant terminée, mais elle est suspendue jusqu'à ce que la coordination du message de fin de thread soit achevée. Il s'agit d'un état ordinaire, mais rare, qui doit être très bref. Une valeur idéale est inférieure à une seconde.| 
|QRY_MEM_GRANT_INFO_MUTEX |Se produit lorsque la gestion de la mémoire pour l'exécution des requêtes essaie de contrôler l'accès à la liste d'informations d'octroi statique. Cet état énumère les informations sur les demandes de mémoire actuellement accordées et en attente. Il s'agit d'un simple état de contrôle d'accès. Il ne devrait jamais y avoir de longue attente sur cet état. Si ce mutex n'est pas libéré, toutes les nouvelles requêtes d'utilisation de mémoire cesseront de répondre.| 
|QRY_PARALLEL_THREAD_MUTEX |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|QRY_PROFILE_LIST_MUTEX |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] et versions ultérieures.| 
|QUERY_ERRHDL_SERVICE_DONE |Identifié à titre d'information uniquement. Non pris en charge. <br /><br /> **S’applique à**: [!INCLUDE[ssKilimanjaro_md](../../includes/sskilimanjaro-md.md)] uniquement. |  
|QUERY_WAIT_ERRHDL_SERVICE |Identifié à titre d'information uniquement.  Non pris en charge. <br /><br /> **S’applique à**: [!INCLUDE[ssKilimanjaro_md](../../includes/sskilimanjaro-md.md)] uniquement.  |  
|QUERY_EXECUTION_INDEX_SORT_EVENT_OPEN |Se produit dans certains cas lorsqu'une construction d'index hors connexion est exécutée en parallèle, et les différents threads de travail qui effectuent le tri synchronisent l'accès aux fichiers de tri.| 
|QUERY_NOTIFICATION_MGR_MUTEX |Se produit durant la synchronisation de la file d'attente de nettoyage de la mémoire dans le gestionnaire de notification de requête.| 
|QUERY_NOTIFICATION_SUBSCRIPTION_MUTEX |Se produit durant la synchronisation de l'état des transactions dans les notifications de requêtes.| 
|QUERY_NOTIFICATION_TABLE_MGR_MUTEX |Se produit durant la synchronisation interne dans le gestionnaire de notification de requête.| 
|QUERY_NOTIFICATION_UNITTEST_MUTEX |Identifié à titre d'information uniquement. Non pris en charge. La compatibilité future n'est pas garantie.| 
|QUERY_OPTIMIZER_PRINT_MUTEX |Se produit durant la synchronisation de la production de la sortie de diagnostics de l'optimiseur de requête. Ce type d’attente se produit uniquement si les paramètres de diagnostic ont été activés sous la direction du support technique Microsoft.| 
|QUERY_TASK_ENQUEUE_MUTEX |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|QUERY_TRACEOUT |Identifié à titre d'information uniquement. Non pris en charge. La compatibilité future n'est pas garantie.| 
|RBIO_WAIT_VLF |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] et versions ultérieures.| 
|RBIO_RG_STORAGE |Se produit lorsqu’un nœud de calcul de base de données hyperscale est limité en raison d’une consommation de journal retardée sur le ou les serveurs de pages. <br /><br /> **S’applique à**: Azure SQL Database hyperscale.|
|RBIO_RG_DESTAGE |Se produit lorsqu’un nœud de calcul de base de données hyperscale est limité en raison d’une consommation de journal retardée par le stockage du journal à long terme. <br /><br /> **S’applique à**: Azure SQL Database hyperscale.|
|RBIO_RG_REPLICA |Se produit lorsqu’un nœud de calcul de base de données hyperscale est limité en raison d’une consommation de journal retardée par le ou les nœuds de réplica secondaires accessibles en lecture. <br /><br /> **S’applique à**: Azure SQL Database hyperscale.|
|RBIO_RG_LOCALDESTAGE |Se produit lorsqu’un nœud de calcul de base de données hyperscale est limité en raison d’une consommation de journal retardée par le service de journalisation. <br /><br /> **S’applique à**: Azure SQL Database hyperscale.|
|RECOVER_CHANGEDB |Se produit durant la synchronisation de l'état de la base de données dans une base de données en mode secours semi-automatique.| 
|RECOVERY_MGR_LOCK |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] et versions ultérieures.| 
|REDO_THREAD_PENDING_WORK |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|REDO_THREAD_SYNC |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|REMOTE_BLOCK_IO |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] et versions ultérieures.| 
|REMOTE_DATA_ARCHIVE_MIGRATION_DMV |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] et versions ultérieures.| 
|REMOTE_DATA_ARCHIVE_SCHEMA_DMV |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] et versions ultérieures.| 
|REMOTE_DATA_ARCHIVE_SCHEMA_TASK_QUEUE |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] et versions ultérieures.| 
|REPL_CACHE_ACCESS |Se produit durant la synchronisation sur un cache des articles de réplication. Lors de ces attentes, l'utilitaire de lecture du journal des réplications se bloque et les instructions de langage de définition de données (DDL - Data Definition Language) sur une table publiée sont bloquées.| 
|REPL_HISTORYCACHE_ACCESS |À usage interne uniquement| 
|REPL_SCHEMA_ACCESS |Se produit durant la synchronisation des informations de version du schéma de réplication. Cet état existe lorsque des instructions DDL sont exécutées sur l'objet répliqué et lorsque l'utilitaire de lecture du journal crée ou exploite le schéma avec version reposant sur une occurrence de DDL. La contention peut être consultée sur ce type d’attente si vous avez de nombreuses bases de données publiées sur un seul serveur de publication avec la réplication transactionnelle et que les bases de données publiées sont très actives.| 
|REPL_TRANFSINFO_ACCESS |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|REPL_TRANHASHTABLE_ACCESS |À usage interne uniquement| 
|REPL_TRANTEXTINFO_ACCESS |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|REPLICA_WRITES |Se produit lorsqu'une tâche attend la fin des écritures de page dans des instantanés de base de données ou des réplicas DBCC.| 
|REQUEST_DISPENSER_PAUSE |Se produit lorsqu'une tâche attend la fin de toutes les E/S en suspens, afin que les E/S puissent être figées dans un fichier pour une sauvegarde des instantanés.| 
|REQUEST_FOR_DEADLOCK_SEARCH |Se produit lorsque l'analyseur de blocages attend pour démarrer la recherche de blocage suivante. Cette attente est normale entre les détections de blocages, et une importante durée d'attente totale sur cette ressource n'indique pas la présence d'un problème.| 
|RESERVED_MEMORY_ALLOCATION_EXT |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] et versions ultérieures.| 
|RESMGR_THROTTLED |Se produit lorsqu'une nouvelle demande arrive et qu'elle est accélérée en fonction du paramètre GROUP_MAX_REQUESTS.| 
|RESOURCE_GOVERNOR_IDLE |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|RESOURCE_QUEUE |Se produit durant la synchronisation de diverses files d'attente de ressources internes.| 
|RESOURCE_SEMAPHORE |Se produit lorsqu'une demande de mémoire de requête ne peut pas être accordée immédiatement en raison d'autres requêtes simultanées. Des temps d'attente élevés peuvent indiquer un trop grand nombre de requêtes simultanées ou des quantités de demande de mémoire trop importantes.| 
|RESOURCE_SEMAPHORE_MUTEX |Se produit lorsqu'une requête attend que sa demande de réservation de thread soit exécutée. Se produit également lors de la synchronisation des demandes d'allocation de mémoire et de compilation de requête.| 
|RESOURCE_SEMAPHORE_QUERY_COMPILE |Se produit lorsque le nombre de compilations de requêtes simultanées atteint une limite. Les attentes et les temps d’attente élevés peuvent indiquer des compilations, des recompilations ou des plans sans mise en cache excessifs.| 
|RESOURCE_SEMAPHORE_SMALL_QUERY |Se produit lorsqu'une demande de mémoire émanant d'une petite requête ne peut pas être accordée immédiatement en raison d'autres requêtes simultanées. Le temps d'attente ne doit pas excéder quelques secondes, car le serveur transfère la demande au pool de mémoire de requête principal s'il ne parvient pas à accorder la mémoire demandée dans les secondes qui suivent. Des temps d'attente élevés peuvent indiquer un nombre excessif de petites requêtes simultanées alors que le pool de mémoire principal est bloqué par les requêtes en attente. <br /><br /> **S’applique à**: [!INCLUDE[ssKilimanjaro_md](../../includes/sskilimanjaro-md.md)] uniquement. |  
|RESTORE_FILEHANDLECACHE_ENTRYLOCK |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] et versions ultérieures.| 
|RESTORE_FILEHANDLECACHE_LOCK |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] et versions ultérieures.| 
|RG_RECONFIG |À usage interne uniquement| 
|ROWGROUP_OP_STATS |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] et versions ultérieures.| 
|ROWGROUP_VERSION |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] et versions ultérieures.| 
|RTDATA_LIST |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|SATELLITE_CARGO |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] et versions ultérieures.| 
|SATELLITE_SERVICE_SETUP |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] et versions ultérieures.| 
|SATELLITE_TASK |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] et versions ultérieures.| 
|SBS_DISPATCH |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] et versions ultérieures.| 
|SBS_RECEIVE_TRANSPORT |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] et versions ultérieures.| 
|SBS_TRANSPORT |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] et versions ultérieures.| 
|SCAN_CHAR_HASH_ARRAY_INITIALIZATION |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|SEC_DROP_TEMP_KEY |Se produit après l'échec d'une tentative de suppression d'une clé de sécurité temporaire avant une nouvelle tentative.| 
|SECURITY_CNG_PROVIDER_MUTEX |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] et versions ultérieures.| 
|SECURITY_CRYPTO_CONTEXT_MUTEX |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|SECURITY_DBE_STATE_MUTEX |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] et versions ultérieures.| 
|SECURITY_KEYRING_RWLOCK |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|SECURITY_MUTEX |Se produit lorsqu'il y a une attente de mutex qui contrôlent l'accès à la liste globale de fournisseurs de services de chiffrement EKM (Gestion de clés extensible) et la liste au niveau de la session des sessions EKM.| 
|SECURITY_RULETABLE_MUTEX |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|SEMPLAT_DSI_BUILD |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|SEQUENCE_GENERATION |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|SEQUENTIAL_GUID |Se produit lorsqu'un nouveau GUID séquentiel est obtenu.| 
|SERVER_IDLE_CHECK |Se produit durant la synchronisation de l'état d'inactivité d'une instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] lorsqu'un moniteur de ressources essaie de déclarer une instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] comme étant inactive ou sur le point de se réactiver.| 
|SERVER_RECONFIGURE |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|SESSION_WAIT_STATS_CHILDREN |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] et versions ultérieures.| 
|SHARED_DELTASTORE_CREATION |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] et versions ultérieures.| 
|SHUTDOWN |Se produit lorsqu'une instruction d'arrêt attend que les connexions actives soient coupées.| 
|SLEEP_BPOOL_FLUSH |Se produit lorsqu'un point de vérification limite l'émission des nouvelles E/S afin de ne pas saturer le sous-système de disque.| 
|SLEEP_BUFFERPOOL_HELPLW |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] et versions ultérieures.| 
|SLEEP_DBSTARTUP |Se produit au démarrage de la base de données lors de l'attente de la récupération de toutes les bases de données.| 
|SLEEP_DCOMSTARTUP |Se produit une fois au maximum durant le démarrage d'une instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en attendant la fin de l'initialisation DCOM.| 
|SLEEP_MASTERDBREADY |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|SLEEP_MASTERMDREADY |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|SLEEP_MASTERUPGRADED |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|SLEEP_MEMORYPOOL_ALLOCATEPAGES |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] et versions ultérieures.| 
|SLEEP_MSDBSTARTUP |Se produit lorsque Trace SQL attend la fin du démarrage de la base de données msdb.| 
|SLEEP_RETRY_VIRTUALALLOC |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] et versions ultérieures.| 
|SLEEP_SYSTEMTASK |Se produit lors du démarrage d'une tâche en arrière-plan pendant l'attente du démarrage de tempdb.| 
|SLEEP_TASK |Se produit lorsqu'une tâche est en état de veille en attendant qu'un événement générique survienne.| 
|SLEEP_TEMPDBSTARTUP |Se produit lorsqu'une tâche attend la fin du démarrage de tempdb.| 
|SLEEP_WORKSPACE_ALLOCATEPAGE |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] et versions ultérieures.| 
|SLO_UPDATE |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|SMSYNC |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] et versions ultérieures.| 
|SNI_CONN_DUP |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|SNI_CRITICAL_SECTION |Se produit durant la synchronisation interne au sein des composants réseau de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].| 
|SNI_HTTP_WAITFOR_0_DISCON |Se produit durant l'arrêt de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], pendant l'attente de la fermeture des connexions HTTP en suspens.| 
|SNI_LISTENER_ACCESS |Se produit pendant l'attente de mise à jour de la modification d'état des nœuds NUMA (Non-Uniform Memory Access). L'accès à la modification d'état est sérialisé.| 
|SNI_TASK_COMPLETION |Se produit lorsqu'il y a une attente de fin de toutes les tâches pendant la modification d'état d'un nœud NUMA.| 
|SNI_WRITE_ASYNC |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] et versions ultérieures.| 
|SOAP_READ |Se produit durant l'attente de l'exécution d'une lecture sur le réseau HTTP.| 
|SOAP_WRITE |Se produit durant l'attente de l'exécution d'une écriture sur le réseau HTTP.| 
|SOCKETDUPLICATEQUEUE_CLEANUP |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] et versions ultérieures.| 
|SOS_CALLBACK_REMOVAL |Se produit lors de la synchronisation sur une liste de rappels afin de supprimer un rappel. Ce compteur ne change pas à la fin de l'initialisation du serveur.| 
|SOS_DISPATCHER_MUTEX |Se produit durant la synchronisation interne du pool de répartiteurs. Cela inclut l'ajustement du pool.| 
|SOS_LOCALALLOCATORLIST |Se produit durant la synchronisation interne dans le gestionnaire de mémoire [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)]. <br /><br /> **S’applique à**: [!INCLUDE[ssKilimanjaro_md](../../includes/sskilimanjaro-md.md)] uniquement. |  
|SOS_MEMORY_TOPLEVELBLOCKALLOCATOR |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|SOS_MEMORY_USAGE_ADJUSTMENT |Se produit lorsque l'utilisation de la mémoire est répartie entre des pools.| 
|SOS_OBJECT_STORE_DESTROY_MUTEX |Se produit durant la synchronisation interne dans les pools de mémoire lorsque des objets du pool sont détruits.| 
|SOS_PHYS_PAGE_CACHE |Tient compte de la durée d'attente d'un thread pour acquérir le mutex qu'il doit obtenir avant de pouvoir allouer des pages physiques ou avant de retourner ces pages au système d'exploitation. Les attentes sur ce type s’affichent uniquement si l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilise la mémoire AWE. <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|SOS_PROCESS_AFFINITY_MUTEX |Se produit durant la synchronisation de l'accès pour traiter les paramètres d'affinité.| 
|SOS_RESERVEDMEMBLOCKLIST |Se produit pendant la synchronisation interne dans le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Gestionnaire de mémoire. <br /><br /> **S’applique à**: [!INCLUDE[ssKilimanjaro_md](../../includes/sskilimanjaro-md.md)] uniquement. |  
|SOS_SCHEDULER_YIELD |Se produit lorsqu'une tâche abandonne volontairement le planificateur pour d'autres tâches à exécuter. Durant cette attente, la tâche attend le renouvellement de son quantum.| 
|SOS_SMALL_PAGE_ALLOC |Se produit pendant l'allocation et la libération de la mémoire gérée par quelques objets mémoire.| 
|SOS_STACKSTORE_INIT_MUTEX |Se produit durant la synchronisation de l'initialisation de stockage interne.| 
|SOS_SYNC_TASK_ENQUEUE_EVENT |Se produit lorsqu'une tâche est démarrée de manière synchrone. La plupart des tâches dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sont démarrées de manière asynchrone ; c'est-à-dire que le contrôle est renvoyé à l'élément initial dès que la tâche a été placée dans la file d'attente de travail.| 
|SOS_VIRTUALMEMORY_LOW |Se produit lorsqu’une allocation de mémoire attend qu’un Gestionnaire des ressources libère de la mémoire virtuelle.| 
|SOSHOST_EVENT |Se produit lorsqu'un composant hébergé, tel que CLR, attend sur un objet de synchronisation d'événement [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].| 
|SOSHOST_INTERNAL |Se produit durant la synchronisation des rappels du gestionnaire de mémoire utilisés par des composants hébergés, tels que CLR.| 
|SOSHOST_MUTEX |Se produit lorsqu'un composant hébergé, tel que CLR, attend sur un objet de synchronisation d'exclusion mutuelle (mutex) [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].| 
|SOSHOST_RWLOCK |Se produit lorsqu'un composant hébergé, tel que CLR, attend sur un objet de synchronisation de lecture/écriture [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].| 
|SOSHOST_SEMAPHORE |Se produit lorsqu'un composant hébergé, tel que CLR, attend sur un objet de synchronisation de sémaphore [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].| 
|SOSHOST_SLEEP |Se produit lorsqu'une tâche hébergée est en veille en attendant qu'un événement générique survienne. Les tâches hébergées sont utilisées par les composants hébergés tels que CLR.| 
|SOSHOST_TRACELOCK |Se produit durant la synchronisation de l'accès aux flux de trace.| 
|SOSHOST_WAITFORDONE |Se produit lorsqu'un composant hébergé, tel que CLR, attend la fin d'une tâche.| 
|SP_PREEMPTIVE_SERVER_DIAGNOSTICS_SLEEP |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|SP_SERVER_DIAGNOSTICS_BUFFER_ACCESS |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|SP_SERVER_DIAGNOSTICS_INIT_MUTEX |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|SP_SERVER_DIAGNOSTICS_SLEEP |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|SQLCLR_APPDOMAIN |Se produit lorsque CLR attend la fin du démarrage d'un domaine d'application.| 
|SQLCLR_ASSEMBLY |Se produit durant l'attente de l'accès à la liste des assembly chargés dans le domaine d'application.| 
|SQLCLR_DEADLOCK_DETECTION |Se produit lorsque CLR attend la fin d'une détection d'interblocage.| 
|SQLCLR_QUANTUM_PUNISHMENT |Se produit lorsqu'une tâche CLR est accélérée car elle a dépassé son quantum d'exécution. Cette accélération est opérée afin de limiter l'incidence de cette tâche consommant une grande quantité de ressources sur les autres tâches.| 
|SQLSORT_NORMMUTEX |Se produit durant la synchronisation interne, lors de l'initialisation des structures de tri internes.| 
|SQLSORT_SORTMUTEX |Se produit durant la synchronisation interne, lors de l'initialisation des structures de tri internes.| 
|SQLTRACE_BUFFER_FLUSH |Se produit lorsqu'une tâche attend qu'une tâche en arrière-plan vide les tampons de traçage sur le disque toutes les quatre secondes. <br /><br /> **S’applique à**: [!INCLUDE[ssKilimanjaro_md](../../includes/sskilimanjaro-md.md)] uniquement. |  
|SQLTRACE_FILE_BUFFER |Se produit durant la synchronisation des tampons de trace lors d'un suivi de fichier. <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|SQLTRACE_FILE_READ_IO_COMPLETION |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|SQLTRACE_FILE_WRITE_IO_COMPLETION |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|SQLTRACE_INCREMENTAL_FLUSH_SLEEP |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|SQLTRACE_LOCK |À usage interne uniquement <br /><br /> **S’applique à**: [!INCLUDE[ssKilimanjaro_md](../../includes/sskilimanjaro-md.md)] uniquement. |  
|SQLTRACE_PENDING_BUFFER_WRITERS |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|SQLTRACE_SHUTDOWN |Se produit pendant que l'arrêt de la trace attend la fin des événements de trace en suspens.| 
|SQLTRACE_WAIT_ENTRIES |Se produit lorsqu'une file d'attente d'événements Trace SQL attend l'arrivée de paquets dans la file d'attente.| 
|SRVPROC_SHUTDOWN |Se produit lorsque le processus d'arrêt attend la libération des ressources internes pour que l'arrêt s'effectue correctement.| 
|STARTUP_DEPENDENCY_MANAGER |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|TDS_BANDWIDTH_STATE |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] et versions ultérieures.| 
|TDS_INIT |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] et versions ultérieures.| 
|TDS_PROXY_CONTAINER |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] et versions ultérieures.| 
|TEMPOBJ |Se produit lorsque des suppressions d'objets temporaires sont synchronisées. Cette attente est rare ; elle survient uniquement si une tâche a demandé un accès exclusif pour les suppressions de tables temp.| 
|TEMPORAL_BACKGROUND_PROCEED_CLEANUP |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] et versions ultérieures.| 
|TERMINATE_LISTENER |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|THREADPOOL |Se produit lorsqu'une tâche attend un thread de travail pour l'exécution. Ceci peut indiquer qu'un paramètre de thread de travail maximal est trop bas ou que les exécutions de traitement sont exceptionnellement longues, ce qui réduit le nombre de threads de travail disponibles pour répondre aux autres lots.| 
|TIMEPRIV_TIMEPERIOD |Se produit durant la synchronisation interne du minuteur d'événements étendus.| 
|TRACE_EVTNOTIF |À usage interne uniquement| 
|TRACEWRITE |Se produit lorsque le fournisseur de traces d'ensemble de lignes Trace SQL attend le traitement d'un tampon libre ou d'un tampon avec des événements.| 
|TRAN_MARKLATCH_DT |Se produit lors de l'attente d'un verrou en mode de destruction sur un verrou interne de marque de transaction. Les verrous internes de marque de transaction sont utilisés pour la synchronisation des validations avec des transactions marquées.| 
|TRAN_MARKLATCH_EX |Se produit lors de l'attente d'un verrou en mode exclusif sur une transaction marquée. Les verrous internes de marque de transaction sont utilisés pour la synchronisation des validations avec des transactions marquées.| 
|TRAN_MARKLATCH_KP |Se produit lors de l'attente d'un verrou en mode de conservation sur une transaction marquée. Les verrous internes de marque de transaction sont utilisés pour la synchronisation des validations avec des transactions marquées.| 
|TRAN_MARKLATCH_NL |Identifié à titre d'information uniquement. Non pris en charge. La compatibilité future n'est pas garantie.| 
|TRAN_MARKLATCH_SH |Se produit lors de l'attente d'un verrou en mode partagé sur une transaction marquée. Les verrous internes de marque de transaction sont utilisés pour la synchronisation des validations avec des transactions marquées.| 
|TRAN_MARKLATCH_UP |Se produit lors de l'attente d'un verrou en mode de mise à jour sur une transaction marquée. Les verrous internes de marque de transaction sont utilisés pour la synchronisation des validations avec des transactions marquées.| 
|TRANSACTION_MUTEX |Se produit durant la synchronisation de l'accès à une transaction par plusieurs traitements.| 
|UCS_ENDPOINT_CHANGE |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|UCS_MANAGER |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|UCS_MEMORY_NOTIFICATION |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|UCS_SESSION_REGISTRATION |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|UCS_TRANSPORT |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|UCS_TRANSPORT_STREAM_CHANGE |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|UTIL_PAGE_ALLOC |Se produit lorsque les analyses des journaux des transactions attendent que de la mémoire soit disponible lors de la sollicitation de la mémoire.| 
|VDI_CLIENT_COMPLETECOMMAND |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|VDI_CLIENT_GETCOMMAND |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|VDI_CLIENT_OPERATION |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|VDI_CLIENT_OTHER |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|VERSIONING_COMMITTING |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|VIA_ACCEPT |Se produit lorsqu'une connexion du fournisseur VIA (Virtual Interface Adapter) est terminée au cours du démarrage.| 
|VIEW_DEFINITION_MUTEX |Se produit durant la synchronisation d'accès aux définitions des vues mises en cache.| 
|WAIT_FOR_RESULTS |Se produit durant l'attente du déclenchement d'une notification de requête.| 
|WAIT_ON_SYNC_STATISTICS_REFRESH |Se produit lors de l’attente de la fin de la mise à jour des statistiques synchrones avant que la compilation et l’exécution des requêtes puissent reprendre.<br /><br /> **S’applique à** : À compter de [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)]|
|WAIT_SCRIPTDEPLOYMENT_REQUEST |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|WAIT_SCRIPTDEPLOYMENT_WORKER |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|WAIT_XLOGREAD_SIGNAL |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] et versions ultérieures.| 
|WAIT_XTP_ASYNC_TX_COMPLETION |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|WAIT_XTP_CKPT_AGENT_WAKEUP |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|WAIT_XTP_CKPT_CLOSE |Se produit lors de l'attente d'un point de contrôle. <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|WAIT_XTP_CKPT_ENABLED |Se produit lorsque les points de contrôle sont désactivés, et lors de l'attente de l'activation des points de contrôle. <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|WAIT_XTP_CKPT_STATE_LOCK |Se produit lors de la synchronisation de la vérification de l'état du point de contrôle. <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|WAIT_XTP_COMPILE_WAIT |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] et versions ultérieures.| 
|WAIT_XTP_GUEST |Se produit lorsque l'allocateur de mémoire de base de données doit cesser de recevoir des notifications de mémoire insuffisante. <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|WAIT_XTP_HOST_WAIT |Se produit lorsque des attentes sont déclenchées par le moteur de base de données et implémentées par l'hôte. <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|WAIT_XTP_OFFLINE_CKPT_BEFORE_REDO |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|WAIT_XTP_OFFLINE_CKPT_LOG_IO |Se produit lorsqu'un point de contrôle hors connexion attend la fin d'une E/S de lecture du journal. <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|WAIT_XTP_OFFLINE_CKPT_NEW_LOG |Se produit lorsqu'un point de contrôle hors connexion attend de nouveaux enregistrements du journal à analyser. <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|WAIT_XTP_PROCEDURE_ENTRY |Se produit lorsqu'une procédure de suppression attend que toutes les exécutions en cours de cette procédure soient terminées. <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|WAIT_XTP_RECOVERY |Se produit lorsque la récupération de base de données attend la fin de la récupération des objets mémoire optimisés. <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|WAIT_XTP_SERIAL_RECOVERY |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] et versions ultérieures.| 
|WAIT_XTP_SWITCH_TO_INACTIVE |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] et versions ultérieures.| 
|WAIT_XTP_TASK_SHUTDOWN |Se produit lors de l'attente de l'exécution d'un thread OLTP en mémoire. <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|WAIT_XTP_TRAN_DEPENDENCY |Se produit lors de l'attente des dépendances de transaction. <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|WAITFOR |Se produit suite à une instruction WAITFOR Transact-SQL. La durée de l'attente est déterminée par les paramètres de l'instruction. Il s'agit d'une attente initialisée par l'utilisateur.| 
|WAITFOR_PER_QUEUE |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|WAITFOR_TASKSHUTDOWN |Identifié à titre d'information uniquement. Non pris en charge. La compatibilité future n'est pas garantie.| 
|WAITSTAT_MUTEX |Se produit durant la synchronisation d'accès à la collection de statistiques utilisées pour remplir sys.dm_os_wait_stats.| 
|WCC |Identifié à titre d'information uniquement. Non pris en charge. La compatibilité future n'est pas garantie.| 
|WINDOW_AGGREGATES_MULTIPASS |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] et versions ultérieures.| 
|WINFAB_API_CALL |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|WINFAB_REPLICA_BUILD_OPERATION |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|WINFAB_REPORT_FAULT |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] et versions ultérieures.| 
|WORKTBL_DROP |Se produit lors d'une suspension avant une nouvelle tentative, suite à l'échec de la suppression d'une table de travail.| 
|WRITE_COMPLETION |Se produit lorsqu'une opération d'écriture est en cours.| 
|WRITELOG |Se produit lors de l'attente d'un vidage du journal. Les opérations courantes qui provoquent des vidages du journal sont les points de vérification et les validations des transactions.| 
|XACT_OWN_TRANSACTION |Se produit pendant l'attente de l'obtention de la propriété d'une transaction.| 
|XACT_RECLAIM_SESSION |Se produit lorsque vous attendez que le propriétaire actuel d'une session libère la propriété de la session.| 
|XACTLOCKINFO |Se produit durant la synchronisation de l'accès à la liste des verrous d'une transaction. Outre la transaction proprement dite, la liste des verrous est accessible par le biais d'opérations telles que la détection d'interblocages et la migration des verrous durant les fractionnements de pages.| 
|XACTWORKSPACE_MUTEX |Se produit durant la synchronisation des désinscriptions à partir d'une transaction et du nombre de verrous de base de données entre les membres d'inscription d'une transaction.| 
|XDB_CONN_DUP_HASH |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] et versions ultérieures.| 
|XDES_HISTORY |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|XDES_OUT_OF_ORDER_LIST |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|XDES_SNAPSHOT |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|XDESTSVERMGR |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|XE_BUFFERMGR_ALLPROCESSED_EVENT |Se produit lorsque les mémoires tampons de session d'Événements étendus sont vidées sur les cibles. Cette attente se produit dans un thread d'arrière-plan.| 
|XE_BUFFERMGR_FREEBUF_EVENT |Se produit lorsqu'une des conditions suivantes est remplie :| 
|XE_CALLBACK_LIST |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|XE_CX_FILE_READ |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|XE_DISPATCHER_CONFIG_SESSION_LIST |Se produit lorsqu'une session Événements étendus qui utilise des cibles asynchrones est démarrée ou arrêtée. Cette attente signifie que :| 
|XE_DISPATCHER_JOIN |Se produit lorsqu'un thread d'arrière-plan utilisé pour les sessions Événements étendus est arrêté.| 
|XE_DISPATCHER_WAIT |Se produit lorsqu'un thread d'arrière-plan utilisé pour les sessions Événements étendus attend le traitement des tampons d'événements.| 
|XE_FILE_TARGET_TVF |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] et versions ultérieures.| 
|XE_LIVE_TARGET_TVF |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.| 
|XE_MODULEMGR_SYNC |Identifié à titre d'information uniquement. Non pris en charge. La compatibilité future n'est pas garantie.| 
|XE_OLS_LOCK |Identifié à titre d'information uniquement. Non pris en charge. La compatibilité future n'est pas garantie.| 
|XE_PACKAGE_LOCK_BACKOFF |Identifié à titre d'information uniquement. Non pris en charge. <br /><br /> **S’applique à**: [!INCLUDE[ssKilimanjaro_md](../../includes/sskilimanjaro-md.md)] uniquement. |  
|XE_SERVICES_EVENTMANUAL |À usage interne uniquement| 
|XE_SERVICES_MUTEX |À usage interne uniquement| 
|XE_SERVICES_RWLOCK |À usage interne uniquement| 
|XE_SESSION_CREATE_SYNC |À usage interne uniquement| 
|XE_SESSION_FLUSH |À usage interne uniquement| 
|XE_SESSION_SYNC |À usage interne uniquement| 
|XE_STM_CREATE |À usage interne uniquement| 
|XE_TIMER_EVENT |À usage interne uniquement| 
|XE_TIMER_MUTEX |À usage interne uniquement| 
|XE_TIMER_TASK_DONE |À usage interne uniquement| 
|XIO_CREDENTIAL_MGR_RWLOCK |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] et versions ultérieures.| 
|XIO_CREDENTIAL_RWLOCK |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] et versions ultérieures.| 
|XIO_EDS_MGR_RWLOCK |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] et versions ultérieures.| 
|XIO_EDS_RWLOCK |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] et versions ultérieures.| 
|XIO_IOSTATS_BLOBLIST_RWLOCK |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] et versions ultérieures.| 
|XIO_IOSTATS_FCBLIST_RWLOCK |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] et versions ultérieures.| 
|XIO_LEASE_RENEW_MGR_RWLOCK |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] et versions ultérieures.| 
|XTP_HOST_DB_COLLECTION |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|XTP_HOST_LOG_ACTIVITY |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|XTP_HOST_PARALLEL_RECOVERY |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] et versions ultérieures.| 
|XTP_PREEMPTIVE_TASK |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] et versions ultérieures.| 
|XTP_TRUNCATION_LSN |À usage interne uniquement <br /><br /> **S’applique à** : [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] et versions ultérieures.| 
|XTPPROC_CACHE_ACCESS |Se produit lors de l'accès à tous les objets cache de procédure stockée compilée en mode natif. <br /><br /> **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures.| 
|XTPPROC_PARTITIONED_STACK_CREATE |Se produit lors de l'allocation des structures cache de procédure stockée compilée en mode natif par nœud NUMA (doit être un thread unique) pour une procédure donnée. <br /><br /> **S’applique à** : [!INCLUDE[ssSQL12](../../includes/sssql11-md.md)] et versions ultérieures.|

  
 Les XEvents suivants sont liés au **commutateur** de partition et à la reconstruction d’index en ligne. Pour plus d’informations sur la syntaxe, consultez [ALTER TABLE &#40;Transact-sql&#41;](../../t-sql/statements/alter-table-transact-sql.md) et [ALTER index &#40;transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md).  
  
-   lock_request_priority_state  
  
-   process_killed_by_abort_blockers  
  
-   ddl_with_wait_at_low_priority  
  
 Pour obtenir une matrice de compatibilité des verrous, consultez [sys.dm_tran_locks &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md).  
  
## <a name="see-also"></a>Voir aussi  
    
 [SQL Server vues de gestion dynamique liées au système d’exploitation &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)   
 [sys.dm_exec_session_wait_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-session-wait-stats-transact-sql.md)   
 [sys.dm_db_wait_stats &#40;Azure SQL Database&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-wait-stats-azure-sql-database.md)  
  
