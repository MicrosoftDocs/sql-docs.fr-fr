---
description: ALTER SERVER CONFIGURATION (Transact-SQL)
title: ALTER SERVER CONFIGURATION (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/22/2019
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER SERVER CONFIGURATION
- ALTER_SERVER_CONFIGURATION_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- SERVER CONFIGURATION, ALTER
- process affinity, setting
- ALTER SERVER CONFIGURATION statement
- setting process affinity
ms.assetid: f3059e42-5f6f-4a64-903c-86dca212a4b4
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 7a61ed11e141376c0b0c769f961fad4e9520c271
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237046"
---
# <a name="alter-server-configuration-transact-sql"></a>ALTER SERVER CONFIGURATION (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Modifie les options de configuration générales pour le serveur actif dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  

```syntaxsql
ALTER SERVER CONFIGURATION  
SET <optionspec>   
[;]  
  
<optionspec> ::=  
{  
     <process_affinity>  
   | <diagnostic_log>  
   | <failover_cluster_property>  
   | <hadr_cluster_context>  
   | <buffer_pool_extension>  
   | <soft_numa>  
   | <memory_optimized>
}  
  
<process_affinity> ::=   
   PROCESS AFFINITY   
   {  
     CPU = { AUTO | <CPU_range_spec> }   
   | NUMANODE = <NUMA_node_range_spec>   
   }  
   <CPU_range_spec> ::=   
      { CPU_ID | CPU_ID  TO CPU_ID } [ ,...n ]   
  
   <NUMA_node_range_spec> ::=   
      { NUMA_node_ID | NUMA_node_ID TO NUMA_node_ID } [ ,...n ]  
  
<diagnostic_log> ::=   
   DIAGNOSTICS LOG   
   {   
     ON    
   | OFF    
   | PATH = { 'os_file_path' | DEFAULT }    
   | MAX_SIZE = { 'log_max_size' MB | DEFAULT }    
   | MAX_FILES = { 'max_file_count' | DEFAULT }    
   }  
  
<failover_cluster_property> ::=   
   FAILOVER CLUSTER PROPERTY <resource_property>  
   <resource_property> ::=  
      {  
        VerboseLogging = { 'logging_detail' | DEFAULT }    
      | SqlDumperDumpFlags = { 'dump_file_type' | DEFAULT }  
      | SqlDumperDumpPath = { 'os_file_path'| DEFAULT }  
      | SqlDumperDumpTimeOut = { 'dump_time-out' | DEFAULT }  
      | FailureConditionLevel = { 'failure_condition_level' | DEFAULT }  
      | HealthCheckTimeout = { 'health_check_time-out' | DEFAULT }  
      }  
  
<hadr_cluster_context> ::=  
   HADR CLUSTER CONTEXT = { 'remote_windows_cluster' | LOCAL }  
  
<buffer_pool_extension>::=  
    BUFFER POOL EXTENSION   
    { ON ( FILENAME = 'os_file_path_and_name' , SIZE = <size_spec> )   
    | OFF }  
  
    <size_spec> ::=  
        { size [ KB | MB | GB ] }  
  
<soft_numa> ::=  
    SOFTNUMA  
    { ON | OFF }  

<memory-optimized> ::=   
   MEMORY_OPTIMIZED   
   {   
     ON 
   | OFF
   | [ TEMPDB_METADATA = { ON [(RESOURCE_POOL='resource_pool_name')] | OFF }
   | [ HYBRID_BUFFER_POOL = { ON | OFF }
   }  
```  
  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
**\<process_affinity> ::=**  
  
PROCESS AFFINITY  
Active des threads de matériel à associer aux unités centrales.  
  
UC = {AUTO | \<CPU_range_spec> }  
Distribue des threads de travail [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à chaque UC dans la plage spécifiée. Les unités centrales situées à l'extérieur de la plage spécifiée ne se verront attribuer aucun thread.  
  
AUTO  
Spécifie qu'aucun thread n'est associé à une unité centrale. Le système d'exploitation peut déplacer librement des threads entre les unités centrales, selon la charge de travail du serveur. Il s’agit du paramètre par défaut recommandé.  
  
\<CPU_range_spec> ::=  
Spécifie l'unité centrale ou la plage d'unités centrales à laquelle affecter des threads.  
  
{ CPU_ID | CPU_ID  TO  CPU_ID } [ ,...n ]  
Liste d'une ou de plusieurs unités centrales. Les ID d’unités centrales commencent à 0 et sont des valeurs de type **entier**.  
  
NUMANODE = \<NUMA_node_range_spec>  
Attribue des threads à toutes les unités centrales qui appartiennent au nœud NUMA spécifié ou à la page de nœuds.  
  
\<NUMA_node_range_spec> ::=  
Spécifie le nœud NUMA ou la plage de nœuds NUMA.  
  
{ NUMA_node_ID | NUMA_node_ID  TO NUMA_node_ID } [ ,...n ]  
Liste d'un ou de plusieurs nœuds NUMA. Les ID de nœud NUMA commencent par 0 et sont des valeurs de type **entier**.  
  
**\<diagnostic_log> ::=**  
  
**S’applique à :** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]).  

  
DIAGNOSTICS LOG  
Démarre ou arrête la journalisation des données de diagnostic que la procédure sp_server_diagnostics capture. Cet argument définit également les paramètres de configuration de journal SQLDIAG comme le nombre de substitutions du fichier journal, la taille du fichier journal et l’emplacement du fichier. Pour plus d’informations, consultez [Afficher et lire le journal de diagnostic de l’instance de cluster de basculement](../../sql-server/failover-clusters/windows/view-and-read-failover-cluster-instance-diagnostics-log.md).  
  
ACTIVÉ  
Démarre la journalisation [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] des données de diagnostics à l'emplacement spécifié dans l'option de fichier PATH. Il s’agit de l’argument par défaut.  
  
OFF  
Cesse la journalisation des données de diagnostics.  
  
PATH = { 'os_file_path' | DEFAULT }  
Chemin d'accès qui indique l'emplacement des journaux de diagnostics. L’emplacement par défaut est \<\MSSQL\Log> dans le dossier d’installation de l’instance du cluster de basculement [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
MAX_SIZE = { 'log_max_size' MB | DEFAULT }  
Taille maximale, en mégaoctets, que peut atteindre chaque journal de diagnostics. La valeur par défaut est 100 MB.  
  
MAX_FILES = { 'max_file_count' | DEFAULT }  
Nombre maximal de fichiers journaux de diagnostics qui peuvent être stockés sur l'ordinateur avant qu'ils ne soient recyclés pour créer de nouveaux journaux de diagnostics.  
  
**\<failover_cluster_property> ::=**  
  
**S’applique à :** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]).    
  
FAILOVER CLUSTER PROPERTY  
Modifie les propriétés de cluster de basculement privées de ressources SQL Server.  
  
VERBOSE LOGGING = { 'logging_detail' | DEFAULT }  
Définit le niveau de journalisation pour le clustering de basculement SQL Server. Il peut être activé pour fournir des détails supplémentaires dans les journaux des erreurs à des fins de dépannage.  
  
-   0 - La journalisation est désactivée (valeur par défaut)  
  
-   1 - Erreurs uniquement  
  
-   2 - Erreurs et avertissements  
  
Dans les scénarios de basculement de ressources, la DLL de ressource SQL Server peut obtenir un fichier dump avant le basculement. Cela s’applique à la fois aux technologies d’instance de cluster de basculement et de groupe de disponibilité. Lorsque la DLL de ressource SQL Server détermine qu’une ressource SQL Server a échoué, elle utilise l’utilitaire Sqldumper.exe pour obtenir un fichier dump du processus SQL Server. Pour vous assurer que l’utilitaire Sqldumper.exe génère correctement le fichier dump lors du basculement de ressources, vous devez définir les trois propriétés suivantes comme prérequis : SqlDumperDumpTimeOut, SqlDumperDumpPath, SqlDumperDumpFlags.

SQLDUMPEREDUMPFLAGS  
Détermine le type de fichiers dump généré par l'utilitaire SQL Server SQLDumper. Le paramètre par défaut est 0. Des valeurs décimales et non hexadécimales sont utilisées pour ce paramètre. Pour un minidump, utilisez 288 ; pour un minidump avec mémoire indirecte, utilisez 296 ; pour un dump filtré, utilisez 33024. Pour plus d’informations, consultez [l’article de la base de connaissances consacré à l’utilitaire SQL Server Dumper](/troubleshoot/sql/tools/use-sqldumper-generate-dump-file).  
  
SQLDUMPERDUMPPATH = { 'os_file_path' | DEFAULT }  
Emplacement de stockage des fichiers dump par l'utilitaire SQLDumper. Pour plus d’informations, consultez [l’article de la base de connaissances consacré à l’utilitaire SQL Server Dumper](/troubleshoot/sql/tools/use-sqldumper-generate-dump-file).  
  
SQLDUMPERDUMPTIMEOUT = { 'dump_time-out' | DEFAULT }  
Valeur du délai d'attente, en millisecondes, de l'utilitaire SQLDumper pour générer un vidage en cas d'échec de SQL Server. La valeur par défaut est 0, ce qui signifie qu'il n'existe aucune limite de temps pour procéder au vidage. Pour plus d’informations, consultez [l’article de la base de connaissances consacré à l’utilitaire SQL Server Dumper](/troubleshoot/sql/tools/use-sqldumper-generate-dump-file).  
  
 FAILURECONDITIONLEVEL = { 'failure_condition_level' | DEFAULT }  
 Conditions dans lesquelles l’instance de cluster de basculement SQL Server doit basculer ou redémarrer. La valeur par défaut est 3, ce qui signifie que la ressource SQL Server procèdera au basculement ou au redémarrage lors d'erreurs de serveur critiques. Pour plus d’informations sur ce niveau de condition d’échec et sur d’autres niveaux, consultez [Configurer les paramètres de propriété FailureConditionLevel](../../sql-server/failover-clusters/windows/configure-failureconditionlevel-property-settings.md).  
  
HEALTHCHECKTIMEOUT = { 'health_check_time-out' | DEFAULT }  
Valeur du délai d'attente qui définit la durée pendant laquelle la DLL de ressource du moteur de base de données SQL Server doit attendre les informations d'intégrité du serveur avant de considérer que l'instance de SQL Server ne répond pas. Valeur de délai d'attente, exprimée en millisecondes. La valeur par défaut est 60 000 millisecondes (60 secondes).  
  
**\<hadr_cluster_context> ::=**  
  
**S’applique à :** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]).   
  
HADR CLUSTER CONTEXT **=** { **'** _remote\_windows\_cluster_ **'** | LOCAL }  
Remplace le contexte de cluster HADR de l'instance de serveur par le cluster de basculement Windows Server (WSFC) spécifié. Le *contexte de cluster HADR* détermine le cluster de basculement Windows Server (WSFC) qui gère les métadonnées pour les réplicas de disponibilité hébergés par l’instance de serveur. N'utilisez l'option SET HADR CLUSTER CONTEXT que pendant une migration entre clusters de [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] vers une instance de [!INCLUDE[ssSQL11SP1](../../includes/sssql11sp1-md.md)], ou d'une version supérieure, sur un nouveau cluster WSFC.  
  
Vous pouvez basculer le contexte de cluster HADR uniquement du cluster WSFC local vers un cluster WSFC distant. Ensuite, vous pouvez choisir de basculer du cluster WSFC distant vers le cluster WSFC local. Il est possible de basculer le contexte de cluster HADR vers un cluster distant uniquement lorsque l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] n'héberge pas un réplica de disponibilité.  
  
Un environnement de cluster HADR distant peut être basculé vers le cluster local à tout moment. Toutefois, le contexte ne peut pas être rebasculé tant que l'instance de serveur héberge des réplicas de disponibilité. 
  
Pour identifier le cluster de destination, spécifiez l'une des valeurs suivantes :  
  
*windows_cluster*  
Nom réseau d’un cluster WSFC. Vous pouvez spécifier le nom court ou le nom de domaine complet. Pour rechercher l'adresse IP cible d'un nom court, ALTER SERVER CONFIGURATION utilise la résolution DNS. Dans certains cas, un nom court peut entraîner quelques confusions, et DNS peut retourner une adresse IP incorrecte. Nous vous recommandons de spécifier le nom de domaine complet.  
  
> [!NOTE] 
> La migration entre clusters à l’aide de ce paramètre n’est plus prise en charge. Pour effectuer une migration entre clusters, utilisez un groupe de disponibilité distribué ou toute autre méthode, par exemple une copie des journaux de transaction. 
  
LOCAL  
Cluster WSFC local.  
  
Pour plus d’informations, consultez [Changer le contexte de cluster HADR de l’instance de serveur &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/change-the-hadr-cluster-context-of-server-instance-sql-server.md).  
  
**\<buffer_pool_extension>::=**  
  
**S’applique à :** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]).    
  
ACTIVÉ  
Active l'option d'extension du pool de mémoires tampons. Cette option augmente la taille de pool de mémoires tampons en utilisant le stockage non volatile. Le stockage non volatile comme les disques SSD conserve les pages de données propres dans le pool. Pour plus d’informations sur cette fonctionnalité, consultez [Extension du pool de mémoires tampons](../../database-engine/configure-windows/buffer-pool-extension.md). L’extension du pool de mémoires tampons n’est pas disponible dans toutes les éditions de SQL Server. Pour plus d’informations, consultez [Fonctionnalités prises en charge par les éditions de SQL Server 2016](../../sql-server/editions-and-components-of-sql-server-2016.md).  
  
FILENAME = 'os_file_path_and_name'  
Définit le chemin d'accès au répertoire et le nom du fichier du cache d'extension du pool de mémoires tampons. L'extension de fichier doit être spécifiée comme .BPE. Désactivez BUFFER POOL EXTENSION avant de modifier FILENAME.  
  
SIZE = *size* [ **KB** | MB | GB ]  
Définit la taille du cache. La spécification de la taille par défaut est en Ko. La taille maximale est la taille de Max Server Memory. La limite est 32 fois la taille de Max Server Memory. Pour plus d’informations sur Max Server Memory, consultez [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md).  
  
Désactivez BUFFER POOL EXTENSION avant de modifier la taille du fichier. Pour spécifier une taille inférieure à la taille actuelle, l'instance de SQL Server doit être redémarrer afin de récupérer de la mémoire. Sinon, la taille spécifiée doit être supérieure ou égale à la taille actuelle.  
  
OFF  
Désactive l'option d'extension du pool de mémoires tampons. Désactivez l'option d'extension du pool de mémoires tampons avant de modifier les paramètres associés tels que la taille ou le nom du fichier. Lorsque cette option est désactivée, toutes les informations de configuration associées sont supprimées du Registre.  
  
> [!WARNING]  
>  Désactiver l'extension du pool de mémoires tampons peut avoir un impact négatif sur les performances du serveur car le pool de mémoires tampons a une taille sensiblement réduite.  
  
**\<soft_numa>**  

**S’applique à :** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]).  
  
ACTIVÉ  
Active le partitionnement automatique afin de fractionner les nœuds NUMA matériels volumineux en nœuds NUMA plus petits. La modification de la valeur active nécessite un redémarrage du moteur de base de données.  
  
OFF  
Désactive le partitionnement logiciel automatique des nœuds NUMA matériels volumineux en nœuds NUMA plus petits. La modification de la valeur active nécessite un redémarrage du moteur de base de données.  

> [!WARNING]
> Il existe des problèmes connus avec le comportement de l’instruction ALTER SERVER CONFIGURATION avec l’option SOFT NUMA et l’Agent SQL Server.  Voici la séquence d’opérations recommandée :  
> 1) Arrêtez l’instance de SQL Server Agent.  
> 2) Exécutez votre option ALTER SERVER CONFIGURATION  SOFT NUMA.  
> 3) Redémarrez l’instance de SQL Server.  
> 4) Démarrez l’instance de SQL Server Agent.  
  
**Informations supplémentaires :** Si vous exécutez ALTER SERVER CONFIGURATION avec SET SOFTNUMA avant que le service SQL Server redémarre, alors, quand le service SQL Server Agent s’arrête, il exécute une commande T-SQL RECONFIGURE qui restaure les paramètres SOFTNUMA en vigueur avant l’exécution d’ALTER SERVER CONFIGURATION. 

**\<memory_optimized> ::=**

**S’applique à :** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]).

ACTIVÉ <br>
Active toutes les fonctionnalités de niveau instance qui font partie de la famille de fonctionnalités [Base de données en mémoire](../../relational-databases/in-memory-database.md). Cela inclut les [métadonnées tempdb à mémoire optimisée](../../relational-databases/databases/tempdb-database.md#memory-optimized-tempdb-metadata) et le [pool de mémoires tampons hybride](../../database-engine/configure-windows/hybrid-buffer-pool.md). Nécessite un redémarrage.

OFF <br>
Désactive toutes les fonctionnalités de niveau instance qui font partie de la famille de fonctionnalités Base de données en mémoire. Nécessite un redémarrage.

TEMPDB_METADATA = ON | OFF <br>
Active ou désactive uniquement les métadonnées tempdb à mémoire optimisée. Nécessite un redémarrage. 

RESOURCE_POOL='resource_pool_name' <br>
Quand il est combiné avec TEMPDB_METADATA = ON, spécifie le pool de ressources défini par l’utilisateur qui doit être utilisé pour tempdb. S’il n’est pas spécifié, tempdb utilise le pool par défaut. Le pool doit déjà exister. Si le pool n’est pas disponible quand le service est redémarré, tempdb utilise le pool par défaut.


HYBRID_BUFFER_POOL = ON | OFF <br>
Active ou désactive le pool de mémoires tampons hybride au niveau de l’instance. Nécessite un redémarrage.


## <a name="general-remarks"></a>Remarques d'ordre général  
Cette instruction ne nécessite pas un redémarrage de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], sauf spécification contraire. Dans le cas d'une instance du cluster de basculement [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], elle ne requiert pas un redémarrage de la ressource de cluster [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="limitations-and-restrictions"></a>Limitations et restrictions  
Cette instruction ne prend pas en charge les déclencheurs DDL.  
  
## <a name="permissions"></a>Autorisations  
Nécessite :
- Autorisations `ALTER SETTINGS` pour l'option d'affinité de processus.
- Autorisations `ALTER SETTINGS` et `VIEW SERVER STATE` pour les options de propriété de cluster de basculement et de journal de diagnostic.
- Autorisation `CONTROL SERVER` pour l’option de contexte de cluster HADR.  
- Autorisation `ALTER SERVER STATE` pour l’option d’extension du pool de mémoires tampons.  
  
La DLL de ressource du [!INCLUDE[ssDE](../../includes/ssde-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] s’exécute sous le compte Système local. Par conséquent, le compte Système local doit disposer d'un accès en lecture et en écriture au chemin d'accès spécifié dans l'option de journal de diagnostics.  
  
## <a name="examples"></a>Exemples  
  
|Category|Éléments syntaxiques proposés|  
|--------------|------------------------------|  
|[Définition de l’affinité de processus](#Affinity)|CPU • NUMANODE • AUTO|  
|[Définition des options du journal de diagnostics](#Diagnostic)|ON • OFF • PATH • MAX_SIZE|  
|[Définition des propriétés de cluster de basculement](#Failover)|HealthCheckTimeout|  
|[Modification du contexte de cluster d’un réplica de disponibilité](#ChangeClusterContextExample)|**'** *windows_cluster* **'**|  
|[Définition de l’extension du pool de mémoires tampons](#BufferPoolExtension)|BUFFER POOL EXTENSION| 
|[Définition des options de base de données en mémoire](#MemoryOptimized)|MEMORY_OPTIMIZED|

  
###  <a name="setting-process-affinity"></a><a name="Affinity"></a> Définition de l’affinité de processus  
Les exemples de cette section montrent comment définir l'affinité de processus sur les unités centrales et les nœuds NUMA. Dans les exemples suivants, on part de l'hypothèse que le serveur contient 256 unités centrales qui sont réparties dans quatre groupes de 16 nœuds NUMA chacun. Les threads ne sont attribués à aucun nœud NUMA ou UC.  
  
-   Groupe 0 : nœuds NUMA 0 à 3, UC 0 à 63  
-   Groupe 1 : nœuds NUMA 4 à 7, UC 64 à 127  
-   Groupe 2 : nœuds NUMA 8 à 12, UC 128 à 191  
-   Groupe 3 : nœuds NUMA 13 à 16, UC 192 à 255  
  
#### <a name="a-setting-affinity-to-all-cpus-in-groups-0-and-2"></a>R. Définition de l'affinité sur toutes les unités centrales dans les groupes 0 et 2  
L'exemple suivant définit l'affinité sur toutes les unités centrales dans les groupes 0 et 2.  
  
```sql  
ALTER SERVER CONFIGURATION   
SET PROCESS AFFINITY CPU=0 TO 63, 128 TO 191;  
```  
  
#### <a name="b-setting-affinity-to-all-cpus-in-numa-nodes-0-and-7"></a>B. Définition de l'affinité sur toutes les unités centrales dans les nœuds NUMA 0 et 7  
L'exemple suivant définit l'affinité UC sur les nœuds `0` et `7` uniquement.  
  
```sql  
ALTER SERVER CONFIGURATION   
SET PROCESS AFFINITY NUMANODE=0, 7;  
```  
  
#### <a name="c-setting-affinity-to-cpus-60-through-200"></a>C. Définition de l'affinité sur les unités centrales entre 60 et 200  
L'exemple suivant définit l'affinité sur les unités centrales entre 60 et 200.  
  
```sql  
ALTER SERVER CONFIGURATION   
SET PROCESS AFFINITY CPU=60 TO 200;  
```  
  
#### <a name="d-setting-affinity-to-cpu-0-on-a-system-that-has-two-cpus"></a>D. Définition de l'affinité sur l'UC 0 sur un système qui compte deux unités centrales  
L'exemple suivant définit l'affinité sur `CPU=0` sur un ordinateur qui compte deux unités centrales. Avant l'exécution de l'instruction suivante, le masque de bits d'affinité interne est de 00.  
  
```sql  
ALTER SERVER CONFIGURATION SET PROCESS AFFINITY CPU=0;  
```  
  
#### <a name="e-setting-affinity-to-auto"></a>E. Définition de l'affinité sur AUTO  
L'exemple suivant définit l'affinité sur la valeur `AUTO`.  
  
```sql  
ALTER SERVER CONFIGURATION  
SET PROCESS AFFINITY CPU=AUTO;  
```  
  
###  <a name="setting-diagnostic-log-options"></a><a name="Diagnostic"></a> Setting diagnostic log options  
  
**S’applique à :** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]).    
  
Les exemples de cette section montrent comment définir les valeurs de l'option de journal de diagnostics.  
  
#### <a name="a-starting-diagnostic-logging"></a>R. Début de la journalisation des diagnostics  
L'exemple suivant démarre la journalisation de données de diagnostics.  
  
```sql  
ALTER SERVER CONFIGURATION SET DIAGNOSTICS LOG ON;  
```  
  
#### <a name="b-stopping-diagnostic-logging"></a>B. Fin de la journalisation des diagnostics  
L'exemple suivant met fin à la journalisation des données de diagnostics.  
  
```sql  
ALTER SERVER CONFIGURATION SET DIAGNOSTICS LOG OFF;  
```  
  
#### <a name="c-specifying-the-location-of-the-diagnostic-logs"></a>C. Spécification de l'emplacement des journaux de diagnostics  
L'exemple suivant définit l'emplacement des journaux de diagnostics sur le chemin d'accès au fichier spécifié.  
  
```sql  
ALTER SERVER CONFIGURATION  
SET DIAGNOSTICS LOG PATH = 'C:\logs';  
```  
  
#### <a name="d-specifying-the-maximum-size-of-each-diagnostic-log"></a>D. Spécification de la taille maximale de chaque journal de diagnostics  
L'exemple suivant définit la taille maximale de chaque journal de diagnostics sur 10 mégaoctets.  
  
```sql  
ALTER SERVER CONFIGURATION   
SET DIAGNOSTICS LOG MAX_SIZE = 10 MB;  
```  
  
###  <a name="setting-failover-cluster-properties"></a><a name="Failover"></a> Définition des propriétés de cluster de basculement  
  
**S’applique à :** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]).   
  
L'exemple suivant illustre la définition des valeurs des propriétés de ressource de cluster de basculement [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
#### <a name="a-specifying-the-value-for-the-healthchecktimeout-property"></a>R. Spécification de la valeur de la propriété HealthCheckTimeout  
L'exemple suivant définit l'option `HealthCheckTimeout` sur 15 000 millisecondes (15 secondes).  
  
```sql  
ALTER SERVER CONFIGURATION   
SET FAILOVER CLUSTER PROPERTY HealthCheckTimeout = 15000;  
```  
  
###  <a name="b-changing-the-cluster-context-of-an-availability-replica"></a><a name="ChangeClusterContextExample"></a> B. Modification du contexte de cluster d'un réplica de disponibilité  
L'exemple qui suit remplace le contexte de cluster HADR de l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour spécifier le cluster WSFC de destination, `clus01`, l'exemple spécifie le nom d'objet cluster complet, `clus01.xyz.com`.  
  
```sql  
ALTER SERVER CONFIGURATION SET HADR CLUSTER CONTEXT = 'clus01.xyz.com';  
```  
  
### <a name="setting-buffer-pool-extension-options"></a>Définition des options d'extension du pool de mémoires tampons  
  
####  <a name="a-setting-the-buffer-pool-extension-option"></a><a name="BufferPoolExtension"></a> A. Définition de l'option d'extension du pool de mémoires tampons  
  
**S’applique à :** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]).    
  
L'exemple suivant active l'option d'extension du pool de mémoires tampons et spécifie un nom de fichier et une taille.  
  
```sql  
ALTER SERVER CONFIGURATION   
SET BUFFER POOL EXTENSION ON  
    (FILENAME = 'F:\SSDCACHE\Example.BPE', SIZE = 50 GB);  
```  
  
#### <a name="b-modifying-buffer-pool-extension-parameters"></a>B. Modifier les paramètres de l'extension du pool de mémoires tampons  
L'exemple suivant modifie la taille du fichier d'extension du pool de mémoires tampons. L'option d'extension du pool de mémoires tampons doit être désactivée avant de modifier les paramètres.  
  
```sql  
ALTER SERVER CONFIGURATION   
SET BUFFER POOL EXTENSION OFF;  
GO  
EXEC sp_configure 'max server memory (MB)', 12000;  
GO  
RECONFIGURE;  
GO  
ALTER SERVER CONFIGURATION  
SET BUFFER POOL EXTENSION ON  
    (FILENAME = 'F:\SSDCACHE\Example.BPE', SIZE = 60 GB);  
GO   
```  

### <a name="setting-in-memory-database-options"></a><a name="MemoryOptimized"></a> Définition des options de base de données en mémoire

**S’applique à :** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]).

#### <a name="a-enable-all-in-memory-database-features-with-default-options"></a>R. Activer toutes les fonctionnalités de base de données en mémoire avec les options par défaut

```sql
ALTER SERVER CONFIGURATION SET MEMORY_OPTIMIZED ON;
GO
```

#### <a name="b-enable-memory-optimized-tempdb-metadata-using-the-default-resource-pool"></a>B. Activer les métadonnées tempdb à mémoire optimisée à l’aide du pool de ressources par défaut

```sql
ALTER SERVER CONFIGURATION SET MEMORY_OPTIMIZED TEMPDB_METADATA = ON;
GO
```

#### <a name="c-enable-memory-optimized-tempdb-metadata-with-a-user-defined-resource-pool"></a>C. Activer les métadonnées tempdb à mémoire optimisée à l’aide du pool de ressources défini par l’utilisateur

```sql
ALTER SERVER CONFIGURATION SET MEMORY_OPTIMIZED TEMPDB_METADATA = ON (RESOURCE_POOL = 'pool_name');
GO
```

#### <a name="d-enable-hybrid-buffer-pool"></a>D. Activer le pool de mémoires tampons hybride

```sql
ALTER SERVER CONFIGURATION SET MEMORY_OPTIMIZED HYBRID_BUFFER_POOL = ON;
GO
```

## <a name="see-also"></a>Voir aussi  
[Soft-NUMA &#40;SQL Server&#41;](../../database-engine/configure-windows/soft-numa-sql-server.md)   
[Changer le contexte de cluster HADR de l’instance de serveur &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/change-the-hadr-cluster-context-of-server-instance-sql-server.md)   
[sys.dm_os_schedulers &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-schedulers-transact-sql.md)   
[sys.dm_os_memory_nodes &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-memory-nodes-transact-sql.md)   
[sys.dm_os_buffer_pool_extension_configuration &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-buffer-pool-extension-configuration-transact-sql.md)   
[Extension du pool de mémoires tampons](../../database-engine/configure-windows/buffer-pool-extension.md)  
