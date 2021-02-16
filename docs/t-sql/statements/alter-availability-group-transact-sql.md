---
description: ALTER AVAILABILITY GROUP (Transact-SQL)
title: ALTER AVAILABILITY GROUP (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/02/2018
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER_AVAILABILITY_GROUP_TSQL
- ALTER_AVAILABILITY_TSQL
- ALTER AVAILABILITY GROUP
- ALTER AVAILABILITY
dev_langs:
- TSQL
helpviewer_keywords:
- Availability Groups [SQL Server], availability replicas
- ALTER AVAILABILITY GROUP statement
- Availability Groups [SQL Server], configuring
- Availability Groups [SQL Server], Transact-SQL statements
ms.assetid: f039d0de-ade7-4aaf-8b7b-d207deb3371a
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 7dfa4ce102b0d53987ad782f6ab8883b2ff0037c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348557"
---
# <a name="alter-availability-group-transact-sql"></a>ALTER AVAILABILITY GROUP (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Modifie un groupe de disponibilité AlwaysOn existant dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. La plupart des arguments ALTER AVAILABILITY GROUP ne sont pris en charge que par le réplica principal actuel. Toutefois, les arguments JOIN, FAILOVER et FORCE_FAILOVER_ALLOW_DATA_LOSS, ne sont pris en charge que sur les réplicas secondaires.  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
  
ALTER AVAILABILITY GROUP group_name   
  {  
     SET ( <set_option_spec> )   
   | ADD DATABASE database_name   
   | REMOVE DATABASE database_name  
   | ADD REPLICA ON <add_replica_spec>   
   | MODIFY REPLICA ON <modify_replica_spec>  
   | REMOVE REPLICA ON <server_instance>  
   | JOIN  
   | JOIN AVAILABILITY GROUP ON <add_availability_group_spec> [ ,...2 ]  
   | MODIFY AVAILABILITY GROUP ON <modify_availability_group_spec> [ ,...2 ]  
   | GRANT CREATE ANY DATABASE  
   | DENY CREATE ANY DATABASE  
   | FAILOVER  
   | FORCE_FAILOVER_ALLOW_DATA_LOSS   
   | ADD LISTENER 'dns_name' ( <add_listener_option> )  
   | MODIFY LISTENER 'dns_name' ( <modify_listener_option> )  
   | RESTART LISTENER 'dns_name'  
   | REMOVE LISTENER 'dns_name'  
   | OFFLINE  
  }  
[ ; ]  
  
<set_option_spec> ::=   
    AUTOMATED_BACKUP_PREFERENCE = { PRIMARY | SECONDARY_ONLY| SECONDARY | NONE }  
  | FAILURE_CONDITION_LEVEL  = { 1 | 2 | 3 | 4 | 5 }   
  | HEALTH_CHECK_TIMEOUT = milliseconds  
  | DB_FAILOVER  = { ON | OFF }   
  | DTC_SUPPORT  = { PER_DB | NONE }  
  | REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT = { integer }
  
<server_instance> ::=   
 { 'system_name[\instance_name]' | 'FCI_network_name[\instance_name]' }  
  
<add_replica_spec>::=  
  <server_instance> WITH  
    (  
       ENDPOINT_URL = 'TCP://system-address:port',  
       AVAILABILITY_MODE = { SYNCHRONOUS_COMMIT | ASYNCHRONOUS_COMMIT | CONFIGURATION_ONLY },  
       FAILOVER_MODE = { AUTOMATIC | MANUAL }   
       [ , <add_replica_option> [ ,...n ] ]  
    )   
  
  <add_replica_option>::=  
       SEEDING_MODE = { AUTOMATIC | MANUAL }  
     | BACKUP_PRIORITY = n  
     | SECONDARY_ROLE ( {   
            [ ALLOW_CONNECTIONS = { NO | READ_ONLY | ALL } ]   
        [,] [ READ_ONLY_ROUTING_URL = 'TCP://system-address:port' ]  
     } )  
     | PRIMARY_ROLE ( {   
            [ ALLOW_CONNECTIONS = { READ_WRITE | ALL } ]   
        [,] [ READ_ONLY_ROUTING_LIST = { ( '<server_instance>' [ ,...n ] ) | NONE } ]  
        [,] [ READ_WRITE_ROUTING_URL = { ( '<server_instance>' ) ] 
     } )  
     | SESSION_TIMEOUT = integer
  
<modify_replica_spec>::=  
  <server_instance> WITH  
    (    
       ENDPOINT_URL = 'TCP://system-address:port'   
     | AVAILABILITY_MODE = { SYNCHRONOUS_COMMIT | ASYNCHRONOUS_COMMIT }   
     | FAILOVER_MODE = { AUTOMATIC | MANUAL }   
     | SEEDING_MODE = { AUTOMATIC | MANUAL }   
     | BACKUP_PRIORITY = n  
     | SECONDARY_ROLE ( {   
          ALLOW_CONNECTIONS = { NO | READ_ONLY | ALL }   
        | READ_ONLY_ROUTING_URL = 'TCP://system-address:port'   
          } )  
     | PRIMARY_ROLE ( {   
          ALLOW_CONNECTIONS = { READ_WRITE | ALL }   
        | READ_ONLY_ROUTING_LIST = { ( '<server_instance>' [ ,...n ] ) | NONE }   
          } )  
     | SESSION_TIMEOUT = seconds  
    )   
  
<add_availability_group_spec>::=  
 <ag_name> WITH  
    (  
       LISTENER_URL = 'TCP://system-address:port',  
       AVAILABILITY_MODE = { SYNCHRONOUS_COMMIT | ASYNCHRONOUS_COMMIT },  
       FAILOVER_MODE = MANUAL,  
       SEEDING_MODE = { AUTOMATIC | MANUAL }  
    )  
  
<modify_availability_group_spec>::=  
 <ag_name> WITH  
    (  
       LISTENER = 'TCP://system-address:port'  
       | AVAILABILITY_MODE = { SYNCHRONOUS_COMMIT | ASYNCHRONOUS_COMMIT }  
       | SEEDING_MODE = { AUTOMATIC | MANUAL }  
    )  
  
<add_listener_option> ::=  
   {  
      WITH DHCP [ ON ( <network_subnet_option> ) ]  
    | WITH IP ( { ( <ip_address_option> ) } [ , ...n ] ) [ , PORT = listener_port ]  
   }  
  
  <network_subnet_option> ::=  
     'ipv4_address', 'ipv4_mask'    
  
  <ip_address_option> ::=  
     {   
        'four_part_ipv4_address', 'four_part_ipv4_mask'  
      | 'ipv6_address'  
     }  
  
<modify_listener_option>::=  
    {  
       ADD IP ( <ip_address_option> )   
     | PORT = listener_port  
    }  
  
```  
  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *group_name*  
 Spécifie le nom du nouveau groupe de disponibilité. *group_name* doit être un identificateur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] valide et doit être unique parmi tous les groupes de disponibilité du cluster WSFC.  
  
 AUTOMATED_BACKUP_PREFERENCE **=** { PRIMARY \| SECONDARY_ONLY \| SECONDARY \| NONE }  
 Spécifie une préférence sur la manière dont un travail de sauvegarde doit évaluer le réplica principal lorsqu'on choisit où effectuer des sauvegardes. Vous pouvez écrire un travail de sauvegarde donné pour prendre en compte la préférence de sauvegarde automatisée. Il est important de comprendre que la préférence n’est pas appliquée par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Elle n’a donc aucune incidence sur les sauvegardes ad hoc.  
  
 Pris en charge uniquement sur le réplica principal.  
  
 Les valeurs sont les suivantes :  
  
 PRIMARY  
 Spécifie que les sauvegardes doivent toujours avoir lieu sur le réplica principal. Cette option est utile si vous avez besoin de fonctionnalités de sauvegarde, telles que la création de sauvegardes différentielles, qui ne sont pas prises en charge lorsque la sauvegarde est exécutée sur un réplica secondaire.  
  
> [!IMPORTANT]  
>  Si vous envisagez d’utiliser la copie de journaux de transaction pour préparer des bases de données secondaires pour un groupe de disponibilité, définissez la préférence de sauvegarde automatisée sur **Principal** jusqu’à ce que toutes les bases de données secondaires aient été préparées et attachées au groupe de disponibilité.  
  
 SECONDARY_ONLY  
 Spécifie que les sauvegardes ne doivent jamais être effectuées sur le réplica principal. Si le réplica principal est le seul réplica en ligne, la sauvegarde ne doit pas avoir lieu.  
  
 SECONDARY  
 Spécifie que les sauvegardes doivent se produire sur un réplica secondaire sauf lorsque le réplica principal est le seul réplica en ligne. Dans ce cas, la sauvegarde doit se produire sur le réplica principal. Il s'agit du comportement par défaut.  
  
 Aucune  
 Spécifie que vous préférez que les travaux de sauvegarde ignorent le rôle des réplicas de disponibilité lorsque vous choisissez le réplica pour effectuer les sauvegardes. Notez que les travaux de sauvegarde peuvent évaluer d'autres facteurs tels que la priorité de sauvegarde de chaque réplica de disponibilité en association avec son état opérationnel et son état connecté.  
  
> [!IMPORTANT]  
>  Il n'y a aucune contrainte du paramètre AUTOMATED_BACKUP_PREFERENCE. La traduction de cette préférence dépend de la logique, le cas échéant, que vous avez écrite dans les travaux de sauvegarde pour les bases de données dans un groupe de disponibilité donné. Le paramètre de préférence de sauvegarde automatisée n’a aucun impact sur les sauvegardes ad-hoc. Pour plus d’informations, consultez [Configurer la sauvegarde sur des réplicas de disponibilité &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/configure-backup-on-availability-replicas-sql-server.md).  
  
> [!NOTE]  
>  Pour voir la préférence de sauvegarde automatisée d’un groupe de disponibilité existant, sélectionnez la colonne **automated_backup_preference** ou **automated_backup_preference_desc** de la vue de catalogue [sys.availability_groups](../../relational-databases/system-catalog-views/sys-availability-groups-transact-sql.md). De plus, vous pouvez utiliser [sys.fn_hadr_backup_is_preferred_replica  &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-hadr-backup-is-preferred-replica-transact-sql.md) pour déterminer le réplica de sauvegarde par défaut.  Cette fonction renverra toujours 1 pour au moins l'un des réplicas, même avec `AUTOMATED_BACKUP_PREFERENCE = NONE`.  
  
 FAILURE_CONDITION_LEVEL **=** { 1 \| 2 \| **3** \| 4 \| 5 }  
 Spécifie les conditions d'échec qui déclencheront un basculement automatique pour ce groupe de disponibilité. FAILURE_CONDITION_LEVEL est défini au niveau du groupe, mais s’applique uniquement aux réplicas de disponibilité configurés pour le mode de disponibilité de validation synchrone (AVAILABILITY_MODE **=** SYNCHRONOUS_COMMIT). En outre, les conditions d’échec ne peuvent déclencher un basculement automatique que si les réplicas principaux et secondaires sont configurés pour le mode de basculement automatique (FAILOVER_MODE **=** AUTOMATIC), et si le réplica secondaire est actuellement synchronisé avec le réplica principal.  
  
 Pris en charge uniquement sur le réplica principal.  
  
 Les niveaux de condition d’échec (1-5) s’étendent du moins restrictif, le niveau 1, au plus restrictif, le niveau 5. Un niveau de condition donné comprend tous les niveaux moins restrictifs. Par conséquent, le niveau de condition le plus strict, le niveau 5, inclut les quatre niveaux de condition moins restrictifs (1 à 4), le niveau 4 inclut les niveaux 1 à 3, et ainsi de suite. Le tableau suivant décrit la condition d'échec qui correspond à chaque niveau.  
  
|Level|Condition d'échec|  
|-----------|-----------------------|  
|1|Spécifie qu'un basculement automatique doit être initialisé lorsque l'une des conditions suivantes se produit :<br /><br /> Le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est fermé.<br /><br /> Le bail du groupe de disponibilité pour la connexion au cluster WSFC expire car aucun accusé de réception n'est reçu de l'instance de serveur. Pour plus d’informations, consultez [How It Works: SQL Server Always On Lease Timeout](https://techcommunity.microsoft.com/t5/sql-server-support/how-it-works-sql-server-alwayson-lease-timeout/ba-p/317268).|  
|2|Spécifie qu'un basculement automatique doit être initialisé lorsque l'une des conditions suivantes se produit :<br /><br /> L'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne se connecte pas au cluster et le seuil du groupe de disponibilité HEALTH_CHECK_TIMEOUT spécifié par l'utilisateur est dépassé.<br /><br /> Le réplica de disponibilité est dans un état d'échec.|  
|3|Spécifie qu'un basculement automatique doit être initialisé sur les erreurs internes critiques [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], telles que les verrouillages spinlock orphelins, les violations graves d'accès en écriture, ou en cas de vidages trop importants.<br /><br /> Il s'agit du comportement par défaut.|  
|4|Spécifie qu'un basculement automatique doit être initialisé sur les erreurs internes modérées [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], telles qu'une condition persistante de mémoire insuffisante dans le pool de ressources interne [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|5|Spécifie qu'un basculement automatique doit être initialisé sur tous les états d'échec qualifiés, notamment :<br /><br /> Insuffisance des threads de travail du moteur SQL.<br /><br /> Détection d'un blocage insoluble.|  
  
> [!NOTE]  
>  L’absence de réponse par une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] aux requêtes des clients ne dépend pas des groupes de disponibilité.  
  
 Les valeurs de FAILURE_CONDITION_LEVEL et de HEALTH_CHECK_TIMEOUT définissent une *stratégie de basculement souple* pour un groupe donné. Cette stratégie de basculement souple vous offre un contrôle granulaire sur les conditions qui doivent entraîner un basculement automatique. Pour plus d’informations, consultez [Stratégie flexible pour le basculement automatique d’un groupe de disponibilité &#40;Transact-SQL&#41;](../../database-engine/availability-groups/windows/configure-flexible-automatic-failover-policy.md).  
  
 HEALTH_CHECK_TIMEOUT **=** *millisecondes*  
 Spécifie le temps d’attente (en millisecondes) nécessaire pour que la procédure stockée système [sp_server_diagnostics](../../relational-databases/system-stored-procedures/sp-server-diagnostics-transact-sql.md) retourne les informations d’intégrité du serveur avant que le cluster WSFC ne suppose que l’instance de serveur est lente ou ne répond pas. HEALTH_CHECK_TIMEOUT est défini au niveau du groupe, mais s’applique uniquement aux réplicas de disponibilité configurés pour le mode de disponibilité de validation synchrone avec basculement automatique (AVAILABILITY_MODE **=** SYNCHRONOUS_COMMIT).  De plus, l’expiration d’un délai de contrôle d’intégrité peut déclencher un basculement automatique uniquement si les réplicas principaux et secondaires sont configurés pour le mode de basculement automatique (FAILOVER_MODE **=** AUTOMATIC) et si le réplica secondaire est synchronisé avec le réplica principal.  
  
 La valeur par défaut de HEALTH_CHECK_TIMEOUT est de 30 000 millisecondes (30 secondes). La valeur minimale est 15 000 millisecondes (15 secondes), et la valeur maximale est 4 294 967 295 millisecondes.  
  
 Pris en charge uniquement sur le réplica principal.  
  
> [!IMPORTANT]  
>  **sp_server_diagnostics** n’exécute pas de vérifications d’intégrité au niveau de la base de données.  
  
 DB_FAILOVER  **=** { ON | OFF }  
 Spécifie la réponse à prendre lorsqu’une base de données sur le réplica principal est hors connexion. Quand cette option est définie sur ON, n’importe quel état autre qu’ONLINE d’une base de données du groupe de disponibilité déclenche un basculement automatique. Quand cette option est définie sur OFF, seule l’intégrité de l’instance est utilisée pour déclencher un basculement automatique.  
 
 Pour plus d’informations sur ce paramètre, consultez [Option de détection de l’état d’intégrité au niveau base de données](../../database-engine/availability-groups/windows/sql-server-always-on-database-health-detection-failover-option.md) 

DTC_SUPPORT  **=** { PER_DB | NONE }  
Spécifie si les transactions distribuées sont activées pour ce groupe de disponibilité. Les transactions distribuées sont uniquement prises en charge pour les bases de données de groupe de disponibilité à compter de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], et les transactions entre bases de données sont uniquement prises en charge dans [!INCLUDE[ssSQL16](../../includes/sssql16-md.md)] SP2. `PER_DB` crée le groupe de disponibilité avec prise en charge de ces transactions, et promeut automatiquement les transactions entre bases de données impliquant des bases de données dans le groupe de disponibilité dans des transactions distribuées. `NONE` empêche la promotion automatique des transactions entre bases de données en transactions distribuées, et n’inscrit pas la base de données avec un RMID stable dans DTC. Les transactions distribuées ne sont pas bloquées quand le paramètre `NONE` est utilisé, mais le basculement et la récupération automatique de base de données risquent d’échouer dans certaines circonstances. Pour plus d’informations, consultez [Transactions entre bases de données et transactions distribuées pour des groupes de disponibilité Always On et la mise en miroir de bases de données &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/transactions-always-on-availability-and-database-mirroring.md). 
 
> [!NOTE]
> La prise en charge de la modification du paramètre DTC_SUPPORT d’un groupe de disponibilité a été introduite dans [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] Service Pack 2. Vous ne pouvez pas utiliser cette option avec les versions antérieures. Pour modifier ce paramètre dans les versions antérieures de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)], vous devez supprimer puis recréer le groupe de disponibilité.
 
 REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT   
 Introduite dans SQL Server 2017. Utilisé pour définir un nombre minimal de réplicas secondaires synchrones nécessaires à la validation avant que le réplica principal ne valide une transaction. Garantit que la transaction SQL Server attend que les journaux des transactions soient mis à jour avec le nombre minimal de réplicas secondaires. La valeur par défaut est 0, ce qui donne le même comportement que SQL Server 2016. La valeur minimale est 0. La valeur maximale est le nombre de réplicas moins 1. Cette option se rapporte aux réplicas en mode de validation synchrone. Quand les réplicas sont en mode de validation synchrone, les écritures sur le réplica principal attend que les écritures sur les réplicas secondaires synchrones soient validées dans le journal des transactions de la base de données de réplica. Si un serveur SQL Server qui héberge un réplica synchrone secondaire cesse de répondre, le serveur SQL qui héberge le réplica principal marque ce réplica secondaire comme NOT SYNCHRONIZED et poursuit. Quand la base de données qui ne répond pas revient en ligne, elle est dans un état « non synchronisé » et le réplica est marqué comme non sain tant que le réplica principal ne l’a pas rendu à nouveau synchrone. Ce paramètre garantit que le réplica principal attend que le nombre minimal de réplicas aient validé chaque transaction. Si le nombre minimal de réplicas n’est pas disponible, les validations sur le réplica principal échouent. Pour le type de cluster `EXTERNAL`, le paramètre est modifié quand le groupe de disponibilité est ajouté à une ressource de cluster. Consultez [Haute disponibilité et protection des données pour les configurations des groupes de disponibilité](../../linux/sql-server-linux-availability-group-ha.md).
  
 ADD DATABASE *database_name*  
 Spécifie une liste d'une ou de plusieurs bases de données utilisateur que vous souhaitez ajouter au groupe de disponibilité. Ces bases de données doivent résider sur l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui héberge le réplica principal actuel. Vous pouvez spécifier plusieurs bases de données pour un groupe de disponibilité, mais chaque base de données ne peut appartenir qu'à un seul groupe de disponibilité. Pour plus d’informations sur le type de bases de données que les groupes de disponibilité peuvent prendre en charge, consultez [Prérequis, restrictions et recommandations pour les groupes de disponibilité Always On &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md). Pour connaître les bases de données locales qui appartiennent déjà à un groupe de disponibilité, consultez la colonne **replica_id** dans l’affichage catalogue [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md).  
  
 Pris en charge uniquement sur le réplica principal.  
  
> [!NOTE]  
>  Après avoir créé le groupe de disponibilité, vous devez vous connecter à chaque instance de serveur qui héberge un réplica secondaire, puis préparer chaque base de données secondaire et la joindre au groupe de disponibilité. Pour plus d’informations, consultez [Démarrer un mouvement de données sur une base de données secondaire Always On &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/start-data-movement-on-an-always-on-secondary-database-sql-server.md).  
  
 REMOVE DATABASE *database_name*  
 Supprime la base de données primaire spécifiée et les bases de données secondaires correspondantes du groupe de disponibilité. Pris en charge uniquement sur le réplica principal.  
  
 Pour plus d’informations sur le suivi recommandé après la suppression d’une base de données de disponibilité dans un groupe de disponibilité, consultez [Supprimer une base de données primaire d’un groupe de disponibilité &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/remove-a-primary-database-from-an-availability-group-sql-server.md).  
  
 ADD REPLICA ON  
 Spécifie de une à huit instances SQL Server pour l'hébergement des réplicas secondaires dans un groupe de disponibilité.  Chaque réplica est spécifié par l’adresse de son instance de serveur suivie d’une clause WITH (...).  
  
 Pris en charge uniquement sur le réplica principal.  
  
 Vous devez joindre chaque nouveau réplica secondaire au groupe de disponibilité. Pour plus d'informations, consultez la description de l'option JOIN ultérieurement dans cette section.  
  
 \<server_instance>  
 Spécifie l’adresse de l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui est l’hôte d’un réplica. Le format de l'adresse varie selon que l'instance est l'instance par défaut ou une instance nommée et s'il s'agit d'une instance autonome ou d'une instance de cluster de basculement (FCI). La syntaxe est la suivante :  
  
 { '*nom_système*[\\*nom_instance*]' | '*nom_réseau_FCI*[\\*nom_instance*]' }  
  
 Les composants de cette adresse sont les suivants :  
  
 *nom_système*  
 Nom NetBIOS du système informatique sur lequel réside l'instance cible de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Cet ordinateur doit être un nœud WSFC.  
  
 *nom_réseau_FCI*  
 Nom réseau utilisé pour accéder à un cluster de basculement [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Utilisez ce nom si l'instance de serveur participe en tant que serveur partenaire de basculement [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. L’exécution de SELECT [@@SERVERNAME](../../t-sql/functions/servername-transact-sql.md) sur une instance de serveur FCI retourne l’intégralité de sa chaîne '*FCI_network_name*[\\*instance_name*]' (laquelle correspond au nom complet du réplica).  
  
 *instance_name*  
 Nom d’une instance d’un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] hébergé par *system_name* ou *FCI_network_name* et où AlwaysOn est activé. Pour une instance de serveur par défaut, *nom_instance* est facultatif. Les noms d'instance ne respectent pas la casse. Sur une instance de serveur autonome, ce nom de valeur est le même que la valeur retournée en exécutant SELECT [@@SERVERNAME](../../t-sql/functions/servername-transact-sql.md).  
  
 \  
 Est un séparateur utilisé uniquement lors de la spécification de *instance_name* pour la séparer de *system_name* ou de *FCI_network_name*.  
  
 Pour plus d’informations sur les prérequis pour les nœuds WSFC et les instances de serveur, consultez [Prérequis, restrictions et recommandations pour les groupes de disponibilité Always On &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md).  
  
 ENDPOINT_URL ='TCP://*system-address*:*port*'  
 Spécifie le chemin d’URL pour le [point de terminaison de mise en miroir de bases de données](../../database-engine/database-mirroring/the-database-mirroring-endpoint-sql-server.md) sur l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui va héberger le réplica de disponibilité que vous ajoutez ou modifiez.  
  
 ENDPOINT_URL est requis dans la clause ADD REPLICA ON et est facultatif dans la clause MODIFY REPLICA ON.  Pour plus d’informations, consultez [Spécifier l’URL de point de terminaison lors de l’ajout ou lors de la modification d’un réplica de disponibilité &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/specify-endpoint-url-adding-or-modifying-availability-replica.md).  
  
 **'** TCP **://** _system-address_ **:** _port_ **'**  
 Spécifie une URL pour spécifier une URL de point de terminaison ou l'URL de routage en lecture seule. Les paramètres d'URL sont les suivants :  
  
 *system-address*  
 Chaîne, telle qu'un nom de système, un nom de domaine complet ou une adresse IP, qui identifie de manière unique l'ordinateur de destination.  
  
 *port*  
 Numéro de port associé au point de terminaison de mise en miroir de l'instance de serveur (pour l'option ENDPOINT_URL) ou numéro de port utilisé par le [!INCLUDE[ssDE](../../includes/ssde-md.md)] de l'instance de serveur (pour l'option READ_ONLY_ROUTING_URL).  
  
 AVAILABILITY_MODE **=** { SYNCHRONOUS_COMMIT | ASYNCHRONOUS_COMMIT | CONFIGURATION_ONLY }  
 Spécifie si le réplica principal doit attendre le réplica secondaire pour accepter la sécurisation renforcée (écriture) des enregistrements de journal sur le disque avant que le réplica principal puisse valider la transaction sur une base de données principale donnée. Les transactions sur des bases de données différentes sur le même réplica principal peuvent être validées indépendamment.  
  
 SYNCHRONOUS_COMMIT  
 Spécifie que le réplica principal attendra que la sécurité soit renforcée sur les transactions de ce réplica secondaire (mode de validation synchrone) avant de les valider. Vous pouvez spécifier SYNCHRONOUS_COMMIT pour trois réplicas au maximum, y compris le réplica principal.  
  
 ASYNCHRONOUS_COMMIT  
 Spécifie que le réplica principal valide les transactions sans attendre que la sécurité du journal de ce réplica secondaire soit renforcée (mode de disponibilité de validation synchrone). Vous pouvez spécifier ASYNCHRONOUS_COMMIT pour cinq réplicas de disponibilité au maximum, y compris le réplica principal.  

 CONFIGURATION_ONLY spécifie que le réplica principal valide de façon synchrone les métadonnées de configuration de groupe de disponibilité dans la base de données master de ce réplica. Le réplica ne contient aucune donnée utilisateur. Cette option :

- Peut être hébergée sur n’importe quelle édition de SQL Server, y compris l’édition Express.
- Exige que le point de terminaison de mise en miroir des données du réplica CONFIGURATION_ONLY soit de type `WITNESS`.
- Ne peut pas être altérée.
- N’est pas valide quand `CLUSTER_TYPE = WSFC`. 

   Pour plus d’informations, consultez [Réplica en configuration seule](../../linux/sql-server-linux-availability-group-ha.md).
    
 AVAILABILITY_MODE est requis dans la clause ADD REPLICA ON et est facultatif dans la clause MODIFY REPLICA ON. Pour plus d’informations, consultez [Modes de disponibilité &#40;groupes de disponibilité Always On&#41;](../../database-engine/availability-groups/windows/availability-modes-always-on-availability-groups.md).  
  
 FAILOVER_MODE **=** { AUTOMATIC | MANUAL }  
 Spécifie le mode de basculement du réplica de disponibilité que vous définissez.  
  
 AUTOMATIC  
 Active le basculement automatique. AUTOMATIC n'est pris en charge que si vous spécifiez également AVAILABILITY_MODE = SYNCHRONOUS_COMMIT. Vous pouvez spécifier AUTOMATIC pour trois réplicas de disponibilité, y compris le réplica principal.  
  
> [!NOTE]  
>  Avant SQL Server 2016, vous étiez limité à deux réplicas de basculement automatique, y compris le réplica principal.
  
> [!NOTE]  
>  Les instances de cluster de basculement (FCI) SQL Server ne prennent pas en charge le basculement automatique par les groupes de disponibilité. Par conséquent, tout réplica de disponibilité hébergé par une instance de cluster de basculement ne peut être configuré que pour un basculement manuel.  
  
 MANUAL  
 Active le basculement manuel ou le basculement manuel forcé (*basculement forcé*) par l’administrateur de base de données.  
  
 FAILOVER_MODE est requis dans la clause ADD REPLICA ON et est facultatif dans la clause MODIFY REPLICA ON. Deux types de basculement manuel existent, le basculement manuel sans perte de données et le basculement forcé (avec perte de données possible) et sont pris en charge dans différentes conditions.  Pour plus d’informations, consultez [Basculement et modes de basculement &#40;groupes de disponibilité Always On&#41;](../../database-engine/availability-groups/windows/failover-and-failover-modes-always-on-availability-groups.md).  
  
 SEEDING_MODE **=** { AUTOMATIC | MANUAL }  
 Spécifie comment le réplica secondaire est initialement amorcé.  
  
 AUTOMATIC  
 Permet l’amorçage direct. Cette méthode amorce le réplica secondaire sur le réseau. Cette méthode ne vous oblige pas à sauvegarder et à restaurer une copie de la base de données principal sur le réplica.  
  
> [!NOTE]  
>  Pour l’amorçage direct, vous devez autoriser la création de la base de données sur chaque réplica secondaire en appelant **ALTER AVAILABILITY GROUP** avec l’option **GRANT CREATE ANY DATABASE**.  
  
 MANUAL  
 Spécifie l’amorçage manuel (par défaut). Cette méthode vous oblige à créer une sauvegarde de la base de données sur le réplica principal et à restaurer manuellement cette sauvegarde sur le réplica secondaire.  
  
 BACKUP_PRIORITY **=** _n_  
 Spécifie la priorité d'exécution des sauvegardes sur ce réplica par rapport aux autres réplicas dans le même groupe de disponibilité. La valeur est un entier compris entre 0 et 100. Ces valeurs ont les significations suivantes :  
  
-   1..100 indique que le réplica de disponibilité peut être choisi pour effectuer des sauvegardes. 1 indique la priorité la plus faible, 100 la priorité la plus élevée. Si BACKUP_PRIORITY = 1, le réplica de disponibilité est choisi pour l'exécution des sauvegardes uniquement si aucun réplica de disponibilité ayant une priorité plus élevée n'est actuellement disponible.  
  
-   0 indique que ce réplica de disponibilité ne sera jamais choisi pour effectuer des sauvegardes. Cela est utile, par exemple, pour un réplica de disponibilité distant sur lequel vous ne souhaitez jamais basculer de sauvegardes.  
  
 Pour plus d’informations, consultez [Secondaires actifs : Sauvegarde sur des réplicas secondaires &#40;groupes de disponibilité Always On&#41;](../../database-engine/availability-groups/windows/active-secondaries-backup-on-secondary-replicas-always-on-availability-groups.md).  
  
 SECONDARY_ROLE **(** ... **)**  
 Spécifie des paramètres spécifiques au rôle qui entreront en vigueur si ce réplica de disponibilité a actuellement le rôle secondaire (autrement dit, chaque fois qu'il s'agit d'un réplica secondaire). Entre parenthèses, spécifiez le l'une ou l'autre, ou les deux options de rôle secondaire. Si vous spécifiez les deux, utilisez une liste séparée par des virgules.  
  
 Les options de rôle secondaire sont les suivantes :  
  
 ALLOW_CONNECTIONS **=** { NO | READ_ONLY | ALL }  
 Spécifie si les bases de données d'un réplica de disponibilité donné qui joue le rôle secondaire (autrement dit, qui joue le rôle d'un réplica secondaire) peuvent accepter de clients, dont :  
  
 Non  
 Aucune connexion utilisateur n'est autorisée aux bases de données secondaires de ce réplica. Elles ne sont pas disponibles pour l'accès en lecture. Il s'agit du comportement par défaut.  
  
 READ_ONLY  
 Seules sont autorisées les connexions aux bases de données dans le réplica secondaire où la propriété d’intention de l’application est définie sur **ReadOnly**. Pour plus d'informations sur cette propriété, consultez [Using Connection String Keywords with SQL Server Native Client](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md).  
  
 ALL  
 Toutes les connexions sont autorisées aux bases de données dans le réplica secondaire pour un accès en lecture seule.  
  
 Pour plus d’informations, consultez [Secondaires actifs : Réplicas secondaires accessibles en lecture &#40;groupes de disponibilité AlwaysOn&#41;](../../database-engine/availability-groups/windows/active-secondaries-readable-secondary-replicas-always-on-availability-groups.md).  
  
 READ_ONLY_ROUTING_URL **='** TCP **://** _system-address_ **:** _port_ **'**  
 Spécifie l'URL à utiliser pour le routage des demandes de connexion de tentative de lecture à ce réplica de disponibilité. Il s'agit de l'URL sur laquelle le moteur de base de données SQL Server écoute. En général, l'instance par défaut du moteur de base de données SQL Server écoute le port TCP 1433.  
  
 Pour une instance nommée, vous pouvez obtenir le numéro de port en interrogeant les colonnes **port** et **type_desc** de la vue de gestion dynamique [sys.dm_tcp_listener_states](../../relational-databases/system-dynamic-management-views/sys-dm-tcp-listener-states-transact-sql.md). L’instance de serveur utilise l’écouteur Transact-SQL (**type_desc='TSQL'** ).  
  
 Pour plus d’informations sur le calcul de l’URL de routage en lecture seule pour un réplica de disponibilité, consultez [Calcul de l’URL de routage en lecture seule pour AlwaysOn](/archive/blogs/mattn/calculating-read_only_routing_url-for-alwayson).  
  
> [!NOTE]  
>  Pour une instance nommée de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], l'écouteur Transact-SQL doit être configuré pour utiliser un port spécifique. Pour plus d’informations, consultez [Configurer un serveur pour écouter un port TCP spécifique &#40;Gestionnaire de configuration SQL Server&#41;](../../database-engine/configure-windows/configure-a-server-to-listen-on-a-specific-tcp-port.md).  
  
 PRIMARY_ROLE **(** ... **)**  
 Spécifie des paramètres spécifiques au rôle qui entreront en vigueur si ce réplica de disponibilité a actuellement le rôle principal (autrement dit, chaque fois qu'il s'agit d'un réplica principal). Entre parenthèses, spécifiez le l'une ou l'autre, ou les deux options de rôle principal. Si vous spécifiez les deux, utilisez une liste séparée par des virgules.  
  
 Les options de rôle principal sont les suivantes :  
  
 ALLOW_CONNECTIONS **=** { READ_WRITE | ALL }  
 Spécifie le type de connexion que les bases de données d'un réplica de disponibilité donné qui joue le rôle principal (autrement dit, qui joue le rôle d'un réplica principal) peuvent accepter de clients, dont :  
  
 READ_WRITE  
 Les connexions où la propriété de connexion d’intention de l’application a la valeur **ReadOnly** ne sont pas autorisées.  Lorsque la propriété d'intention de l'application a la valeur **ReadWrite** ou si cette propriété n'est pas définie, la connexion est autorisée. Pour plus d'informations sur la propriété de connexion d'intention de l'application, consultez [Using Connection String Keywords with SQL Server Native Client](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md).  
  
 ALL  
 Toutes les connexions aux bases de données sont autorisées dans le réplica principal. Il s'agit du comportement par défaut.  
  
 READ_ONLY_ROUTING_LIST **=** { **('** \<server_instance> **'** [ **,** ...*n*] **)** | NONE }  
 Spécifie une liste séparée par des virgules des instances de serveur qui hébergent les réplicas de disponibilité pour ce groupe de disponibilité qui répondent aux conditions suivantes lors de l'exécution sous le rôle secondaire :  
  
-   Être configurés pour autoriser les connexions ou connexions en lecture seule (voir l'argument ALLOW_CONNECTIONS de l'option de SECONDARY_ROLE, ci-dessus).  
  
-   Disposer de leur URL de routage en lecture seule définie (voir l'argument READ_ONLY_ROUTING_URL de l'option SECONDARY_ROLE, ci-dessus).  
  
 Les valeurs de READ_ONLY_ROUTING_LIST sont les suivantes :  
  
 \<server_instance>  
 Spécifie l'adresse de l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui est l'hôte d'un réplica de disponibilité qui est un réplica secondaire lisible lors de l'exécution sous le rôle secondaire.  
  
 Utilisez une liste séparée par des virgules pour spécifier toutes les instances de serveur qui peuvent héberger un réplica secondaire lisible. Le routage en lecture seule suivra l'ordre dans lequel les instances de serveur sont spécifiées dans la liste. Si vous incluez l'instance de serveur hôte d'un réplica dans la liste de routage en lecture seule du réplica, il est généralement recommandé d'insérer cette instance de serveur à la fin de la liste, afin que les connexions de tentative de lecture soient dirigées vers un réplica secondaire (le cas échéant).  
  
 Depuis [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], vous pouvez équilibrer la charge des demandes d’intention de lecture entre les réplicas secondaires lisibles. Vous le spécifiez en plaçant les réplicas dans un jeu imbriqué de parenthèses dans la liste de routage en lecture seule. Pour plus d’informations et des exemples, consultez [Configurer l’équilibrage de charge entre des réplicas en lecture seule](../../database-engine/availability-groups/windows/configure-read-only-routing-for-an-availability-group-sql-server.md#loadbalancing).  
  
 Aucune  
 Spécifie que lorsque ce réplica de disponibilité est le réplica principal, le routage en lecture seule ne sera pas pris en charge. Il s'agit du comportement par défaut. Lorsqu'elle est utilisée avec MODIFY REPLICA ON, cette valeur désactive une liste existante (le cas échéant).  

 READ_WRITE_ROUTING_URL **=** { **('** \<server_instance> **')** }  
 S’applique à : SQL Server (à partir de SQL Server 2019 (15.x)) 

 Spécifie des instances de serveur qui hébergent les réplicas de disponibilité pour ce groupe de disponibilité qui répondent aux conditions suivantes lors de l’exécution sous le rôle principal :
-   La spécification du réplica PRIMARY_ROLE comprend READ_WRITE_ROUTING_URL.
-   La chaîne de connexion est ReadWrite en définissant ApplicationIntent comme ReadWrite ou en ne définissant pas ApplicationIntent et en laissant la valeur par défaut (ReadWrite) prendre effet.

Pour plus d’informations, consultez [Redirection de connexion en lecture/écriture depuis un réplica secondaire vers le réplica principal (groupes de disponibilité Always On)](../../database-engine/availability-groups/windows/secondary-replica-connection-redirection-always-on-availability-groups.md).

 SESSION_TIMEOUT **=** _seconds_  
 Spécifie le délai d'expiration de la session en secondes. Si vous ne précisez pas de valeur, le délai défini par défaut est 10 secondes. La valeur minimale est de 5 secondes.  
  
> [!IMPORTANT]  
>  Le temps d'attente recommandé est de 10 secondes minimum.  
  
 Pour plus d’informations sur le délai d’expiration de session, consultez [Vue d’ensemble des groupes de disponibilité Always On &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md).  
  
 MODIFY REPLICA ON  
 Modifie un réplica du groupe de disponibilité. La liste des réplicas à modifier contient l’adresse de l’instance de serveur et une clause WITH (...) pour chaque réplica.  
  
 Pris en charge uniquement sur le réplica principal.  
  
 REMOVE REPLICA ON  
 Supprime le réplica secondaire spécifié du groupe de disponibilité. Le réplica principal actuel ne peut pas être supprimé d'un groupe de disponibilité. Lorsqu'il est supprimé, le réplica cesse de recevoir des données. Ses bases de données secondaires sont supprimées du groupe de disponibilité et passent à l'état RESTORING.  
  
 Pris en charge uniquement sur le réplica principal.  
  
> [!NOTE]  
>  Si vous supprimez un réplica alors qu'il n'est pas disponible ou qu'il a échoué, lorsqu'il revient en ligne, il n'appartient plus au groupe de disponibilité.  
  
 JOIN  
 Provoque l'hébergement par l'instance de serveur locale d'un réplica secondaire dans le groupe de disponibilité spécifié.  
  
 Pris en charge uniquement sur un réplica secondaire qui n'a pas encore été joint au groupe de disponibilité.  
  
 Pour plus d’informations, consultez [Joindre un réplica secondaire à un groupe de disponibilité &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/join-a-secondary-replica-to-an-availability-group-sql-server.md).  
  
 FAILOVER  
Initialise un basculement manuel du groupe de disponibilité sur le réplica secondaire auquel vous êtes connecté, sans perte de données. Le réplica qui doit héberger le réplica principal est la *cible de basculement*.  La cible de basculement assure le rôle principal, récupère sa copie de chaque base de données et les met en ligne en tant que nouvelles bases de données principales. Le réplica principal précédent joue simultanément le rôle secondaire, et ses bases de données deviennent des bases de données secondaires et sont immédiatement suspendues. Éventuellement, ces rôles peuvent être basculés par une série d'échecs.  
  
 Pris en charge uniquement sur un réplica secondaire de validation synchrone qui est actuellement synchronisé avec le réplica principal. Notez que pour qu'un réplica secondaire soit synchronisé, le réplica principal doit également d'exécuter en mode de validation synchrone.  
  
> [!NOTE]  
>  Une commande de basculement est retournée dès que la cible de basculement a accepté la commande. Toutefois, la récupération de la base de données est asynchrone après que le basculement du groupe de disponibilité est terminé.  
  
 Pour plus d’informations sur les limitations, les prérequis et les recommandations relatives à l’exécution d’un basculement manuel planifié, consultez [Effectuer un basculement manuel planifié d’un groupe de disponibilité &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/perform-a-planned-manual-failover-of-an-availability-group-sql-server.md).  
  
 FORCE_FAILOVER_ALLOW_DATA_LOSS  
 > [!CAUTION]  
>  Forcer un basculement peut entraîner une perte de données et est à proprement parler une méthode de récupération après sinistre. Par conséquent, nous vous recommandons fortement de forcer le basculement uniquement si le réplica principal n'est plus exécuté, si vous êtes prêt à prendre le risque de perdre des données et si vous devez restaurer immédiatement le service dans le groupe de disponibilité.  
  
 Pris en chrage uniquement sur un réplica dont le rôle est dans l'état SECONDARY ou RESOLVING. --Le réplica sur lequel vous entrez une commande de basculement est appelé *cible de basculement*.  
  
 Force le basculement du groupe de disponibilité (avec possible perte de données) vers la cible de basculement. La cible de basculement assure le rôle principal, récupère sa copie de chaque base de données et les met en ligne en tant que nouvelles bases de données principales. Sur tous les réplicas secondaires restants, chaque base de données secondaire est suspendue jusqu'à ce qu'elle soit reprise manuellement. Lorsque le réplica principal précédent est disponible, il bascule sur le rôle secondaire, et ses bases de données deviennent des bases de données secondaires suspendues.  
  
> [!NOTE]  
>  Une commande de basculement est retournée dès que la cible de basculement a accepté la commande. Toutefois, la récupération de la base de données est asynchrone après que le basculement du groupe de disponibilité est terminé.  
  
 Pour plus d’informations sur les limitations, les prérequis et les recommandations pour forcer un basculement, ainsi que sur l’effet d’un basculement forcé sur les anciennes bases de données principales dans le groupe de disponibilité, consultez [Effectuer un basculement manuel forcé d’un groupe de disponibilité &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/perform-a-forced-manual-failover-of-an-availability-group-sql-server.md).  
  
 ADD LISTENER **'** _dns\_name_ **'(** \<add_listener_option> **)**  
 Définit un nouvel écouteur de groupe de disponibilité pour ce groupe de disponibilité. Pris en charge uniquement sur le réplica principal.  
  
> [!IMPORTANT]
>  Avant de créer votre premier écouteur, nous vous recommandons vivement de lire [Créer ou configurer un écouteur de groupe de disponibilité &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md).  
> 
>  Après avoir créé un écouteur pour un groupe de disponibilité spécifique, nous vous recommandons fortement de procéder comme suit :  
> 
>  -   Demandez à votre administrateur réseau de réserver l'adresse IP de l'écouteur pour son utilisation exclusive.  
> -   Fournissez le nom d'hôte DNS de l'écouteur aux développeurs d'applications pour qu'ils l'utilisent dans les chaînes de connexion lorsqu'ils demandent des connexions clientes vers ce groupe de disponibilité.  
  
 *dns_name*  
 Spécifie le nom d'hôte DNS de l'écouteur du groupe de disponibilité. Le nom DNS de l'écouteur doit être unique dans le domaine et dans NetBIOS.  
  
 *dns_name* est une valeur de chaîne. Ce nom ne peut contenir que des caractères alphanumériques, des tirets (-) et des caractères de soulignement (_), dans n'importe quel ordre. Les noms d'hôte DNS ne respectent pas la casse. La longueur maximale autorisée est de 63 caractères.  
  
 Nous vous recommandons de spécifier une chaîne explicite. Par exemple, pour un groupe de disponibilité nommé `AG1`, un nom d'hôte DNS explicite est `ag1-listener`.  
  
> [!IMPORTANT]  
>  NetBIOS identifie les 15 premiers caractères du nom_dns. Si vous avez deux clusters WSFC qui sont contrôlés par le même annuaire Active Directory et que vous tentez de créer des écouteurs de groupe de disponibilité dans les deux clusters à l'aide de noms contenant plus de 15 caractères et un préfixe identique de 15 caractères, vous obtenez une erreur signalant que la ressource de nom de réseau virtuel ne peut pas être mise en ligne. Pour plus d'informations sur les règles de préfixe des noms DNS, consultez [Attribution de noms de domaine](https://technet.microsoft.com/library/cc731265\(WS.10\).aspx).  
  
 JOIN AVAILABILITY GROUP ON  
 Joint à un *groupe de disponibilité distribué*. Quand vous créez un groupe de disponibilité distribué, le groupe de disponibilité du cluster sur lequel il est créé devient le groupe de disponibilité principal. Le groupe de disponibilité qui rejoint le groupe de disponibilité distribué devient le groupe de disponibilité secondaire.  
  
 \<ag_name>  
 Spécifie le nom du groupe de disponibilité qui constitue la moitié du groupe de disponibilité distribué.  
  
 LISTENER **='** TCP **://** _system-address_ **:** _port_ **'**  
 Spécifie le chemin de l’URL de l’écouteur associé au groupe de disponibilité.  
  
 La clause LISTENER est obligatoire.  
  
 **'** TCP **://** _system-address_ **:** _port_ **'**  
 Spécifie une URL pour l’écouteur associé au groupe de disponibilité. Les paramètres d'URL sont les suivants :  
  
 *system-address*  
 Chaîne, comme un nom de système, un nom de domaine complet ou une adresse IP, qui identifie sans ambiguïté l’écouteur.  
  
 *port*  
 Est un numéro de port associé au point de terminaison de mise en miroir du groupe de disponibilité. Notez que ce n’est pas le port de l’écouteur.  
  
 AVAILABILITY_MODE **=** { SYNCHRONOUS_COMMIT | ASYNCHRONOUS_COMMIT }  
 Spécifie si le réplica principal doit attendre le groupe de disponibilité secondaire pour accepter la sécurisation renforcée (écriture) des enregistrements de journal sur le disque avant que le réplica principal puisse valider la transaction sur une base de données principale donnée.  
  
 SYNCHRONOUS_COMMIT  
 Spécifie que le réplica principal attend que la sécurité soit renforcée sur les transactions de ce groupe de disponibilité secondaire avant de les valider. Vous pouvez spécifier SYNCHRONOUS_COMMIT pour deux groupes de disponibilité, y compris pour le groupe de disponibilité principal.  
  
 ASYNCHRONOUS_COMMIT  
 Spécifie que le réplica principal valide les transactions sans attendre que la sécurité du journal de ce groupe de disponibilité secondaire soit renforcée. Vous pouvez spécifier ASYNCHRONOUS_COMMIT pour deux groupes de disponibilité, y compris pour le groupe de disponibilité principal.  
  
 La clause AVAILABILITY_MODE est obligatoire.  
  
 FAILOVER_MODE **=** { MANUAL }  
 Spécifie le mode de basculement du groupe de disponibilité distribué.  
  
 MANUAL  
 Active le basculement manuel planifié ou le basculement manuel forcé (généralement appelé *basculement forcé*) par l’administrateur de base de données.  
  
 Le basculement automatique vers le groupe de disponibilité secondaire n’est pas pris en charge.  
  
 SEEDING_MODE **=** { AUTOMATIC | MANUAL }  
 Spécifie comment le groupe de disponibilité secondaire est initialement amorcé.  
  
 AUTOMATIC  
 Active l’amorçage automatique. Cette méthode amorce le groupe de disponibilité secondaire sur le réseau. Cette méthode ne vous demande pas de sauvegarder et de restaurer une copie de la base de données principal sur les réplicas du groupe de disponibilité secondaire.  
  
 MANUAL  
 Spécifie l’amorçage manuel. Cette méthode vous oblige à créer une sauvegarde de la base de données sur le réplica principal et de restaurer manuellement cette sauvegarde sur le ou les réplicas du groupe de disponibilité secondaire.  
  
 MODIFY AVAILABILITY GROUP ON  
 Modifie les paramètres de groupe de disponibilité d’un groupe de disponibilité distribué. La liste des groupes de disponibilité à modifier contient le nom du groupe de disponibilité et une clause WITH (...) pour chaque groupe de disponibilité.  
  
> [!IMPORTANT]  
>  Cette commande doit être répétée sur le groupe de disponibilité principal et sur les instances de groupes de disponibilité secondaires.  
  
 GRANT CREATE ANY DATABASE  
 Autorise le groupe de disponibilité à créer des bases de données pour le compte du réplica principal, qui prend en charge l’amorçage direct (SEEDING_MODE = AUTOMATIC). Ce paramètre doit être exécuté sur chaque réplica secondaire qui prend en charge l’amorçage direct, après que cette base de données secondaire a rejoint le groupe de disponibilité.  Nécessite l’autorisation CREATE ANY DATABASE.  
  
 DENY CREATE ANY DATABASE  
 Pour le groupe de disponibilité, supprime la possibilité de créer des bases de données pour le compte du réplica principal.  
  
 \<add_listener_option>  
 ADD LISTENER prend l'une des options suivantes :  
  
 WITH DHCP [ ON { **('** _four\_part\_ipv4\_address_ **','** _four\_part\_ipv4\_mask_ **')** } ]  
 Spécifie que l'écouteur du groupe de disponibilité utilise le protocole DHCP (Dynamic Host Configuration Protocol).  Utilisez éventuellement la clause ON pour identifier le réseau sur lequel cet écouteur sera créé. DHCP est limité à un seul sous-réseau utilisé pour chaque instance de serveur qui héberge un réplica de disponibilité dans le groupe de disponibilité.  
  
> [!IMPORTANT]  
>  Nous vous déconseillons d'utiliser DHCP dans un environnement de production. Lorsqu'un temps mort se produit et que le bail IP DHCP arrive à expiration, une période de temps supplémentaire est requise pour enregistrer la nouvelle adresse IP de réseau DHCP associée au nom DNS de l'écouteur et cela a un impact sur la connectivité client. Toutefois, DHCP peut tout à fait être utilisé pour configurer un environnement de développement et de test dans le but de vérifier les fonctions de base des groupes de disponibilité et l'intégration avec vos applications.  
  
 Par exemple :  
  
 `WITH DHCP ON ('10.120.19.0','255.255.254.0')`  
  
 WITH IP **(** { **('** _four\_part\_ipv4\_address_ **','** _four\_part\_ipv4\_mask_ **')**  |  **('** _ipv6\_address_ **')** } [ **,** ..._n_ ] **)** [ **,** PORT **=** _listener\_port_ ]  
 Spécifie que l'écouteur du groupe de disponibilité utilisera une ou plusieurs adresses IP statiques au lieu d'utiliser DHCP. Pour créer un groupe de service sur plusieurs sous-réseaux, chaque sous-réseau requiert une adresse IP statique dans la configuration de l'écouteur. Pour un sous-réseau donné, l'adresse IP statique peut être une adresse IPv4 ou une adresse IPv6. Contactez votre administrateur réseau pour obtenir une adresse IP statique pour chaque sous-réseau qui hébergera un réplica de disponibilité pour le nouveau groupe de disponibilité.  
  
 Par exemple :  
  
 `WITH IP ( ('10.120.19.155','255.255.254.0') )`  
  
 *ipv4_address*  
 Spécifie une adresse IPv4 en quatre parties pour un écouteur du groupe de disponibilité. Par exemple : `10.120.19.155`.  
  
 *ipv4_mask*  
 Spécifie un masque IPv4 en quatre parties pour un écouteur du groupe de disponibilité. Par exemple : `255.255.254.0`.  
  
 *ipv6_address*  
 Spécifie une adresse IPv6 pour un écouteur du groupe de disponibilité. Par exemple : `2001::4898:23:1002:20f:1fff:feff:b3a3`.  
  
 PORT **=** *listener_port*  
 Spécifie le numéro de port *listener_port* à utiliser par un écouteur du groupe de disponibilité spécifié par une clause WITH IP. Le PORT est facultatif.  
  
 Le numéro de port par défaut est 1433. Toutefois, si vous avez des problèmes de sécurité, nous vous recommandons d'utiliser un numéro de port différent.  
  
 Par exemple : `WITH IP ( ('2001::4898:23:1002:20f:1fff:feff:b3a3') ) , PORT = 7777`  
  
 MODIFY LISTENER **'** _dns\_name_ **'(** \<modify\_listener\_option\> **)**  
 Modifie un écouteur du groupe de disponibilité existant pour ce groupe de disponibilité. Pris en charge uniquement sur le réplica principal.  
  
 \<modify\_listener\_option\>  
 MODIFY LISTENER prend l'une des options suivantes :  
  
 ADD IP { **('** _four\_part\_ipv4\_address_ **','** _four\_part\_ipv4_mask_ **')** \| <b>('</b>dns\_name *ipv6\_address* __')__ }  
 Ajoute l’adresse IP spécifiée à l’écouteur de groupe de disponibilité spécifié par *dns\_name*.  
  
 PORT **=** *listener_port*  
 Une description de cet argument est fournie plus haut dans cette section.  
  
 RESTART LISTENER **'** _dns\_name_ **'**  
 Redémarre l'écouteur associé au nom DNS spécifié. Pris en charge uniquement sur le réplica principal.  
  
 REMOVE LISTENER **'** _dns\_name_ **'**  
 Supprime l'écouteur associé au nom DNS spécifié. Pris en charge uniquement sur le réplica principal.  
  
 OFFLINE  
 Place un groupe de disponibilité hors connexion. Il n'y a aucune perte de données des bases de données avec validation synchrone.  
  
 Après qu'un groupe de disponibilité a été mis hors connexion, ses bases de données deviennent indisponibles pour les clients et vous ne pouvez pas remettre le groupe de disponibilité en ligne. Par conséquent, utilisez l'option OFFLINE uniquement pendant une migration entre clusters de [!INCLUDE[ssHADR](../../includes/sshadr-md.md)], lorsque vous migrez des ressources du groupe de disponibilité vers un nouveau cluster WSFC.  
  
 Pour plus d’informations, consultez [Placer un groupe de disponibilité hors connexion &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/take-an-availability-group-offline-sql-server.md).  
  
## <a name="prerequisites-and-restrictions"></a>Conditions préalables requises et restrictions  
 Pour plus d’informations sur les prérequis et les restrictions des réplicas de disponibilité et de leurs ordinateurs et instances de serveur hôte, consultez [Prérequis, restrictions et recommandations pour les groupes de disponibilité AlwaysOn &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md).  
  
 Pour plus d’informations sur les restrictions relatives aux instructions Transact-SQL AVAILABILITY GROUP, consultez [Vue d’ensemble des instructions Transact-SQL pour les groupes de disponibilité AlwaysOn &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/transact-sql-statements-for-always-on-availability-groups.md).  
  
## <a name="security"></a>Sécurité  
  
### <a name="permissions"></a>Autorisations  
 Requiert l'autorisation ALTER AVAILABILITY GROUP sur le groupe de disponibilité, l'autorisation CONTROL AVAILABILITY GROUP, l'autorisation ALTER ANY AVAILABILITY GROUP ou l'autorisation CONTROL SERVER.  Nécessite également l’autorisation ALTER ANY DATABASE.   
  
## <a name="examples"></a>Exemples  
  
###  <a name="a-joining-a-secondary-replica-to-an-availability-group"></a><a name="Join_Secondary_Replica"></a> A. Jointure d'un réplica secondaire à un groupe de disponibilité  
 L’exemple suivant joint un réplica secondaire auquel vous êtes connecté au groupe de disponibilité `AccountsAG`.  
  
```SQL  
ALTER AVAILABILITY GROUP AccountsAG JOIN;  
GO  
```  
  
###  <a name="b-forcing-failover-of-an-availability-group"></a><a name="Force_Failover"></a> B. Forcer le basculement d'un groupe de disponibilité  
 L'exemple suivant force le basculement du groupe de disponibilité `AccountsAG` sur le réplica secondaire auquel vous êtes connecté.  
  
```SQL
ALTER AVAILABILITY GROUP AccountsAG FORCE_FAILOVER_ALLOW_DATA_LOSS;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [CREATE AVAILABILITY GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/create-availability-group-transact-sql.md)   
 [ALTER DATABASE SET HADR &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-hadr.md)   
 [DROP AVAILABILITY GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/drop-availability-group-transact-sql.md)   
 [sys.availability_replicas &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-availability-replicas-transact-sql.md)   
 [sys.availability_groups &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-availability-groups-transact-sql.md)   
 [Résoudre des problèmes de configuration des groupes de disponibilité Always On &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/troubleshoot-always-on-availability-groups-configuration-sql-server.md)   
 [Vue d’ensemble des groupes de disponibilité Always On &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Écouteurs de groupe de disponibilité, connectivité client et basculement d’application &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md)  
  
