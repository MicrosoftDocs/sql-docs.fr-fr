---
title: Utiliser des objets SQL Server | Microsoft Docs
description: Découvrez les objets et compteurs SQL Server qu’utilise le Moniteur système pour superviser l’activité des ordinateurs exécutant une instance de SQL Server.
ms.custom: ''
ms.date: 03/17/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- server performance [SQL Server], objects for monitoring
- database monitoring [SQL Server], objects for monitoring
- charts [SQL Server]
- System Monitor [SQL Server], counters
- counters [SQL Server], listed
- objects [SQL Server], performance monitoring
- objects [SQL Server], Windows System Monitor
- performance counters [SQL Server], about performance counters
- System Monitor [SQL Server], objects
- performance counters [SQL Server]
- counters [SQL Server], about performance counters
- tuning databases [SQL Server], objects for monitoring
- database performance [SQL Server], objects for monitoring
- SQL Server, objects
- monitoring performance [SQL Server], objects for monitoring
- Windows System Monitor [SQL Server], objects
- Windows System Monitor [SQL Server], counters
- counters [SQL Server]
- performance counters [SQL Server], listed
ms.assetid: bcd731b1-3c4e-4086-b58a-af7a3af904ad
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: e293b33b0b20ab7545744c7d4e3f47351f8e0c29
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/13/2021
ms.locfileid: "98172191"
---
# <a name="use-sql-server-objects"></a>Utiliser des objets SQL Server
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournit des objets et des compteurs qui peuvent être utilisés par le Moniteur système pour surveiller l'activité des ordinateurs exécutant une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Un objet peut être n'importe quelle ressource [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , telle qu'un verrou [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou un processus Windows. Chaque objet contient un ou plusieurs compteurs qui déterminent divers aspects de l'objet à surveiller. Par exemple, l’objet **SQL Server Locks** contient des compteurs appelés **Nombre d’interblocages/s** et **Dépassement du délai d’attente des verrous/s**.  
  
 Certains objets disposent de plusieurs instances si plusieurs ressources d'un type donné sont présentes sur l'ordinateur. Par exemple, le type d'objet **Processor** possède plusieurs instances si le système est multiprocesseur. Le type d'objet **Databases** dispose d'une instance pour chaque base de données sur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Certains types d’objets (l’objet **Memory Manager** , par exemple) ne disposent que d’une seule instance. Si un type d'objet dispose de plusieurs instances, vous pouvez ajouter des compteurs pour suivre les statistiques de chaque instance ou, le plus souvent, de toutes les instances à la fois. Les compteurs de l'instance par défaut apparaissent au format **SQLServer:** _\<object name>_ . Les compteurs pour les instances nommées apparaissent au format **MSSQL$** _\<instance name>_ **:** _\<counter name>_ ou **SQLAgent$** _\<instance name>_ **:** _\<counter name>_ .  
  
Les valeurs du compteur de performances [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sont générées à l’aide du moteur Compteur de performances Windows (WPC). Certaines valeurs de compteur ne sont pas calculées directement par le [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournit des valeurs de base au moteur WPC, qui effectue alors les calculs nécessaires (tels que des pourcentages). La vue de gestion dynamique [sys.dm_os_performance_counters &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-performance-counters-transact-sql.md) fournit la valeur d’origine générée par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à tous les compteurs. La colonne `cntr_type` indique le type du compteur. La façon dont le moteur WPC traite les valeurs de compteur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dépend de ce type. Pour plus d’informations sur les types de compteurs de performances, consultez la [documentation WMI](/windows/win32/wmisdk/wmi-performance-counter-types).
  
 En ajoutant ou en supprimant des compteurs du graphique et en enregistrant les valeurs du graphique, vous pouvez spécifier les objets et compteurs de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] surveillés lors du démarrage du Moniteur système.  
  
 Vous pouvez configurer le Moniteur système pour qu'il affiche des statistiques provenant de n'importe quel compteur de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . De plus, vous pouvez fixer une valeur seuil pour n'importe quel compteur de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et générer ensuite une alerte quand le compteur dépasse ce seuil. Pour plus d’informations sur la définition d’une alerte, consultez [Créer une alerte de base de données SQL Server](../../relational-databases/performance-monitor/create-a-sql-server-database-alert.md).  
    
  
> [!NOTE]  
>  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne sont affichées que lorsqu'une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est installée. Si vous arrêtez puis relancez une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], l'affichage des statistiques est interrompu, puis reprend automatiquement. Remarquez également que vous pouvez voir des compteurs [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans le composant logiciel enfichable Moniteur système même si [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] n'est pas exécuté. Sur une instance cluster, les compteurs de performances ne fonctionnent sur le nœud que lorsque [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est exécuté.  
  
 Cette rubrique contient les sections suivantes :  
  
-   [Objets de performance de l'Agent SQL Server](#SQLServerAgentPOs)  
  
-   [Objets de performance de Service Broker](#ServiceBrokerPOs)  
  
-   [Objets de performance de SQL Server](#SQLServerPOs)  
  
-   [Objets de performance de la réplication de SQL Server](#SQLServerReplicationPOs)  
  
-   [Compteurs de pipeline SSIS](#SsisPipelineCounters)  
  
-   [Autorisations requises](#RequiredPermissions)  
  
##  <a name="sql-server-agent-performance-objects"></a><a name="SQLServerAgentPOs"></a> Objets de performance de l'Agent SQL Server  
 Le tableau suivant répertorie les objets de performance fournis pour l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] :  
  
|Objet de performance|Description|  
|------------------------|-----------------|  
|[SQLAgent:Alerts](../../relational-databases/performance-monitor/sql-server-agent-alerts-object.md)|Fournit des informations sur les alertes de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|  
|[SQLAgent:Jobs](../../relational-databases/performance-monitor/sql-server-agent-jobs-object.md)|Fournit des informations sur les travaux de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|  
|[SQLAgent:JobSteps](../../relational-databases/performance-monitor/sql-server-agent-jobsteps-object.md)|Fournit des informations sur les étapes de travail de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|  
|[SQLAgent:Statistics](../../relational-databases/performance-monitor/sql-server-agent-statistics-object.md)|Fournit des informations générales sur l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|  
  
##  <a name="service-broker-performance-objects"></a><a name="ServiceBrokerPOs"></a> Objets de performance de Service Broker  
 Le tableau suivant répertorie les objets de performance fournis pour [!INCLUDE[ssSB](../../includes/sssb-md.md)].  
  
|Objet de performance|Description|  
|------------------------|-----------------|  
|[SQLServer:Broker Activation](../../relational-databases/performance-monitor/sql-server-broker-activation-object.md)|Fournit des informations sur les tâches activées par [!INCLUDE[ssSB](../../includes/sssb-md.md)].|  
|[SQLServer:Broker Statistics](../../relational-databases/performance-monitor/sql-server-broker-statistics-object.md)|Fournit des informations générales sur [!INCLUDE[ssSB](../../includes/sssb-md.md)] .|  
|[SQLServer:Broker Transport](../../relational-databases/performance-monitor/sql-server-broker-dbm-transport-object.md)|Fournit des informations sur le réseau [!INCLUDE[ssSB](../../includes/sssb-md.md)] .|  
  
##  <a name="sql-server-performance-objects"></a><a name="SQLServerPOs"></a> Objets de performance de SQL Server  
 Le tableau ci-dessous décrit les objets [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
|Objet de performance|Description|  
|------------------------|-----------------|  
|[SQLServer:Access Methods](../../relational-databases/performance-monitor/sql-server-access-methods-object.md)|Recherche et mesure l'allocation des objets de bases de données de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (par exemple, le nombre de recherches d'index ou de pages allouées aux index et aux données).|  
|[SQLServer:Backup Device](../../relational-databases/performance-monitor/sql-server-backup-device-object.md)|Fournit des informations sur les unités de sauvegarde utilisées pour les opérations de sauvegarde et de restauration, comme le débit de l'unité de sauvegarde.|  
|[SQLServer:Batch Resp Statistics](../../relational-databases/performance-monitor/sql-server-batch-resp-statistics-object.md)|Compteurs pour suivre le temps de réponse par lot SQL.| 
|[SQLServer:Buffer Manager](../../relational-databases/performance-monitor/sql-server-buffer-manager-object.md)|Fournit des informations sur les mémoires tampon utilisées par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], comme les **pages libres** et le **taux d'accès au cache des tampons**.|  
|[SQL Server:Buffer Node](../../relational-databases/performance-monitor/sql-server-buffer-node.md)|Fournit des informations sur la fréquence à laquelle [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] effectue des demandes et accède aux pages libres.|  
|[SQLServer:Catalog Metadata](../../relational-databases/performance-monitor/sql-server-catalog-metadata-object.md)|Ceci définit un objet de gestionnaire de métadonnées de catalogue pour SQL Server.| 
|[SQLServer:CLR](../../relational-databases/performance-monitor/sql-server-clr-object.md)|Fournit des informations à propos du common language runtime (CLR).|  
|[SQLServer:Columnstore](../../relational-databases/performance-monitor/sql-server-columnstore-object.md)|**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] et versions ultérieures).<br /><br /> Fournit des informations sur les rowgroups et les segments des index columnstore.|  
|[SQLServer:Cursor Manager by Type](../../relational-databases/performance-monitor/sql-server-cursor-manager-by-type-object.md)|Fournit des informations sur les curseurs.|  
|[SQLServer:Cursor Manager Total](../../relational-databases/performance-monitor/sql-server-cursor-manager-total-object.md)|Fournit des informations sur les curseurs.|  
|[SQLServer:Database Mirroring](../../relational-databases/performance-monitor/sql-server-database-mirroring-object.md)|Fournit des informations sur la mise en miroir de bases de données.|  
|[SQLServer:Databases](../../relational-databases/performance-monitor/sql-server-databases-object.md)|Fournit des informations sur une base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , comme la quantité d'espace journal disponible ou le nombre de transactions actives dans la base de données. Cet objet peut avoir plusieurs instances.|  
|[SQL Server : Fonctionnalités déconseillées](../../relational-databases/performance-monitor/sql-server-deprecated-features-object.md)|Compte le nombre d'utilisations de fonctions déconseillées.|  
|[SQLServer:Exec Statistics](../../relational-databases/performance-monitor/sql-server-execstatistics-object.md)|Fournit des informations sur les statistiques d'exécution.|  
|[SQL Server:External Scripts](../../relational-databases/performance-monitor/sql-server-external-scripts-object.md)|**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] et versions ultérieures).<br /><br /> Fournit des informations sur l’exécution de scripts externes.|  
|[SQLServer:FileTable](../../relational-databases/performance-monitor/sql-server-filetable-object.md)|Statistiques associées à FileTable et aux accès non transactionnels.|  
|[SQLServer:General Statistics](../../relational-databases/performance-monitor/sql-server-general-statistics-object.md)|Fournit des informations sur l'activité générale à l'échelle du serveur, comme le nombre d'utilisateurs connectés à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|[SQL Server:HADR Availability Replica](../../relational-databases/performance-monitor/sql-server-availability-replica.md)|Fournit des informations sur les alertes de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)][!INCLUDE[ssHADR](../../includes/sshadr-md.md)] .|  
|[SQL Server:HADR Database Replica](../../relational-databases/performance-monitor/sql-server-database-replica.md)|Fournit des informations sur les réplicas de bases de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)][!INCLUDE[ssHADR](../../includes/sshadr-md.md)] .|  
|[SQL Server : stockage HTTP](../../relational-databases/performance-monitor/sql-server-http-storage-object.md)|Fournit des informations pour surveiller un compte de stockage Microsoft Azure lors de l’utilisation de [fichiers de données SQL Server dans Microsoft Azure](../../relational-databases/databases/sql-server-data-files-in-microsoft-azure.md)|  
|[SQLServer:Latches](../../relational-databases/performance-monitor/sql-server-latches-object.md)|Fournit des informations sur les verrous de ressources internes, comme les pages de bases de données, utilisés par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|[SQLServer:Locks](../../relational-databases/performance-monitor/sql-server-locks-object.md)|Fournit des informations sur les demandes de verrous individuelles émises par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], comme les dépassements du délai d'attente des verrous et les interblocages. Cet objet peut avoir plusieurs instances.|  
|[SQLServer:LogPool FreePool](../../relational-databases/performance-monitor/sql-server-logpool-freepool-object.md)|Décrit des statistiques pour le pool libre dans le pool du journal.|
|[SQLServer:Memory Broker Clerks](../../relational-databases/performance-monitor/sql-server-memory-broker-clerks-object.md)|Statistiques relatives aux régisseurs de gestionnaire d’allocation mémoire.|
|[SQLServer:Memory Manager](../../relational-databases/performance-monitor/sql-server-memory-manager-object.md)|Fournit des informations sur l'utilisation de la mémoire de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , comme le nombre total de structures de verrous actuellement allouées.|  
|[SQLServer:Plan Cache](../../relational-databases/performance-monitor/sql-server-plan-cache-object.md)|Fournit des informations sur le cache de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilisé pour stocker des objets tels que les procédures stockées, les déclencheurs et les plans de requête.|  
|[SQLServer : Magasin des requêtes](../../relational-databases/performance-monitor/sql-server-query-store-object.md)|Fournit des informations sur le magasin de requêtes.|  
|[SQLServer : Statistiques des pools de ressources](../../relational-databases/performance-monitor/sql-server-resource-pool-stats-object.md)|Fournit des informations à propos des statistiques du pool de ressources de Resource Governor.|  
|[SQLServer:SQL Errors](../../relational-databases/performance-monitor/sql-server-sql-errors-object.md)|Fournit des informations sur les erreurs [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|  
|[SQLServer:SQL Statistics](../../relational-databases/performance-monitor/sql-server-sql-statistics-object.md)|Fournit des informations sur les aspects des requêtes [!INCLUDE[tsql](../../includes/tsql-md.md)] , comme le nombre de lots d'instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] reçus par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|[SQLServer:Transactions](../../relational-databases/performance-monitor/sql-server-transactions-object.md)|Fournit des informations sur les transactions actives dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], telles que le nombre total de transactions et le nombre de transactions d'instantané.|  
|[SQLServer:User Settable](../../relational-databases/performance-monitor/sql-server-user-settable-object.md)|Réalise une surveillance personnalisée. Chaque compteur peut être une procédure stockée personnalisée ou toute instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] qui renvoie une valeur à surveiller.|  
|[SQLServer : Statistiques d’attente](../../relational-databases/performance-monitor/sql-server-wait-statistics-object.md)|Fournit des informations sur les attentes.|  
|[SQLServer : Statistiques des groupes de charges de travail](../../relational-databases/performance-monitor/sql-server-workload-group-stats-object.md)|Fournit des informations à propos des statistiques du groupe de charges de travail de Resource Governor.|  
  
##  <a name="sql-server-replication-performance-objects"></a><a name="SQLServerReplicationPOs"></a> Objets de performance de la réplication de SQL Server  
 Le tableau suivant répertorie les objets de performance fournis pour la réplication [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] :  
  
|Objet de performance|Description|  
|------------------------|-----------------|  
|**SQLServer:Replication Agents**<br /><br /> **SQLServer:Replication Snapshot**<br /><br /> **SQLServer:Replication Logreader**<br /><br /> **SQLServer:Replication Dist.**<br /><br /> **SQLServer:Replication Merge**<br /><br /> Pour plus d’informations, voir [Monitoring Replication with System Monitor](../../relational-databases/replication/monitor/monitoring-replication-with-system-monitor.md).|Fournit des informations sur l'activité de l'agent de réplication.|  
  
##  <a name="ssis-pipeline-counters"></a><a name="SsisPipelineCounters"></a> Compteurs de pipeline SSIS  
 Pour le compteur **Pipeline SSIS** , consultez [Compteur de performances](../../integration-services/performance/performance-counters.md).  
  
##  <a name="required-permissions"></a><a name="RequiredPermissions"></a> Autorisations requises  
 L'utilisation des objets [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dépend des autorisations Windows, sauf **SQLAgent:Alerts**. Pour utiliser **SQLAgent:Alerts** , les utilisateurs doivent être membres du rôle de serveur fixe **sysadmin**.  
  
## <a name="see-also"></a>Voir aussi  
 [Utiliser des objets de performance](../../ssms/agent/use-performance-objects.md)   
 [sys.dm_os_performance_counters &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-performance-counters-transact-sql.md)  
  
