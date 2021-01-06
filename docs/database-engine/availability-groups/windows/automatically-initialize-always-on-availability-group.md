---
title: Initialiser un groupe de disponibilité à l’aide de l’amorçage automatique
description: Utilisez l’amorçage automatique pour créer automatiquement des réplicas secondaires pour chaque base de données d’un groupe de disponibilité Always On sans avoir à effectuer de sauvegarde et restauration manuelles.
ms.custom: seo-lt-2019
ms.date: 03/26/2018
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
ms.assetid: 67c6a601-677a-402b-b3d1-8c65494e9e96
author: cawrites
ms.author: chadam
ms.openlocfilehash: a4b992ab522145959cb4e3fe4e4b978f15b11e9c
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97644154"
---
# <a name="use-automatic-seeding-to-initialize-an-always-on-availability-group"></a>Utiliser l’amorçage automatique pour initialiser un groupe de disponibilité Always On
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

SQL Server 2016 propose un amorçage automatique des groupes de disponibilité. Quand vous créez un groupe de disponibilité par amorçage automatique, SQL Server crée automatiquement les réplicas secondaires de chaque base de données du groupe. Vous n’avez plus besoin de sauvegarder ni de restaurer manuellement les réplicas secondaires. Pour activer l’amorçage automatique, créez le groupe de disponibilité avec T-SQL ou utilisez la dernière version de SQL Server Management Studio.

Pour plus d’informations, consultez [Amorçage automatique pour les réplicas secondaires](automatic-seeding-secondary-replicas.md).
 
## <a name="prerequisites"></a>Conditions préalables requises

Dans SQL Server 2016, l’amorçage automatique exige que le chemin des fichiers de données et des fichiers journaux soit le même sur chaque instance de SQL Server qui participe au groupe de disponibilité. Dans SQL Server 2017, vous pouvez utiliser différents chemins, mais Microsoft recommande d’utiliser les mêmes chemins quand tous les réplicas sont hébergés sur la même plateforme (par exemple Windows ou Linux). Les groupes de disponibilité multiplateformes ont des chemins différents pour les réplicas. Pour plus d’informations, consultez [Disposition des disques](automatic-seeding-secondary-replicas.md#disklayout).

L’amorçage du groupe de disponibilité communique par le biais du point de terminaison de mise en miroir de bases de données. Ouvrez des règles de pare-feu pour le trafic entrant dans le port du point de terminaison de mise en miroir sur chaque serveur.

Les bases de données incluses dans un groupe de disponibilité doivent être en mode de récupération complet. La base de données a besoin d’une sauvegarde complète actualisée et d’une sauvegarde du journal des transactions. Ces fichiers de sauvegarde ne sont pas utilisés pour l’amorçage automatique, mais ils sont exigés avant d’inclure la base de données dans un groupe de disponibilité. 
 
## <a name="create-availability-group-with-automatic-seeding"></a>Créer un groupe de disponibilité par amorçage automatique

Pour créer un groupe de disponibilité par amorçage automatique, définissez `SEEDING_MODE=AUTOMATIC`. 

L’exemple suivant crée un groupe de disponibilité sur un cluster de basculement Windows Server à deux nœuds. Avant d’exécuter les scripts, mettez à jour les valeurs conformément à votre environnement.

1. Créez les points de terminaison. Chaque serveur a besoin d’un point de terminaison. Le script suivant crée un point de terminaison qui utilise le port TCP 5022 pour l’écouteur. Définissez `<endpoint_name>` et `LISTENER_PORT` conformément à votre environnement et exécutez le script sur les deux serveurs :

    ```sql
    CREATE ENDPOINT [<endpoint_name>] 
        STATE=STARTED
        AS TCP (LISTENER_PORT = 5022, LISTENER_IP = ALL)
        FOR DATA_MIRRORING (
            ROLE = ALL, 
            AUTHENTICATION = WINDOWS NEGOTIATE, 
            ENCRYPTION = REQUIRED ALGORITHM AES
            )
    GO
    ```

1. Créez le groupe de disponibilité. Le script suivant crée le groupe de disponibilité. Mettez à jour les valeurs entre crochets `<>` du nom du groupe, des noms de serveurs et des noms de domaines, puis exécutez-le sur l’instance principale de SQL Server.  

    ```sql
    CREATE AVAILABILITY GROUP [<availability_group_name>]
        FOR DATABASE db1
        REPLICA ON'<*primary_server*>'
        WITH (ENDPOINT_URL = N'TCP://<primary_server>.<fully_qualified_domain_name>:5022', 
            FAILOVER_MODE = AUTOMATIC, 
            AVAILABILITY_MODE = SYNCHRONOUS_COMMIT, 
            BACKUP_PRIORITY = 50, 
            SECONDARY_ROLE(ALLOW_CONNECTIONS = NO), 
            SEEDING_MODE = AUTOMATIC),
        N'<secondary_server>' WITH (ENDPOINT_URL = N'TCP://<secondary_server>.<fully_qualified_domain_name>:5022', 
            FAILOVER_MODE = AUTOMATIC, 
            AVAILABILITY_MODE = SYNCHRONOUS_COMMIT, 
            BACKUP_PRIORITY = 50, 
            SECONDARY_ROLE(ALLOW_CONNECTIONS = NO), 
            SEEDING_MODE = AUTOMATIC);
    GO
    ``` 

1. Joignez l’instance de serveur secondaire au groupe de disponibilité et octroyez à ce dernier l’autorisation de créer des bases de données. Mettez à jour le script suivant, remplacez les valeurs figurant entre crochets `<>` en fonction de votre environnement, puis exécutez-le sur l’instance de réplica secondaire de SQL Server : 
 
    ```sql
    ALTER AVAILABILITY GROUP [<availability_group_name>] JOIN
    GO  
    ALTER AVAILABILITY GROUP [<availability_group_name>] GRANT CREATE ANY DATABASE
    GO
    ```

SQL Server crée automatiquement le réplica de base de données sur le serveur secondaire. Si la base de données est volumineuse, sa synchronisation peut prendre un certain temps. Si une base de données se trouve dans un groupe de disponibilité configuré pour l’amorçage automatique, vous pouvez interroger la vue système `sys.dm_hadr_automatic_seeding` pour surveiller la progression de l’amorçage. La requête suivante renvoie une seule ligne pour chaque base de données présente dans un groupe de disponibilité configuré pour l’amorçage automatique.

```sql 
SELECT start_time,
    ag.name,
    db.database_name,
    current_state,
    performed_seeding,
    failure_state,
    failure_state_desc
FROM sys.dm_hadr_automatic_seeding autos 
    JOIN sys.availability_databases_cluster db 
        ON autos.ag_db_id = db.group_database_id
    JOIN sys.availability_groups ag 
        ON autos.ag_id = ag.group_id
```

## <a name="prevent-automatic-seeding-after-an-availability-group"></a>Empêcher l’amorçage automatique sur un groupe de disponibilité

Pour empêcher temporairement le réplica principal d’amorcer davantage de bases de données sur le réplica secondaire, vous pouvez refuser au groupe de disponibilité l’autorisation de créer des bases de données. Exécutez la requête suivante sur l’instance qui héberge le réplica secondaire pour refuser au groupe de disponibilité l’autorisation de créer des bases de données.

```sql
ALTER AVAILABILITY GROUP [<availability_group_name>] 
    DENY CREATE ANY DATABASE
GO
```


## <a name="enable-automatic-seeding-on-an-existing-availability-group"></a>Activer l’amorçage automatique sur un groupe de disponibilité existant

Vous pouvez définir l’amorçage automatique sur une base de données existante. La commande suivante change un groupe de disponibilité pour qu’il utilise l’amorçage automatique. Exécutez la commande suivante sur le réplica principal.

```sql
ALTER AVAILABILITY GROUP [<availability_group_name>] 
    MODIFY REPLICA ON '<secondary_node>' 
    WITH (SEEDING_MODE = AUTOMATIC)
GO
```

La commande précédente force une base de données à redémarrer l’amorçage si nécessaire. Par exemple, si l’amorçage échoue en raison d’un espace disque insuffisant sur le réplica secondaire, exécutez `ALTER AVAILABILITY GROUP ... WITH (SEEDING_MODE=AUTOMATIC)` pour redémarrer l’amorçage après avoir ajouté de l’espace libre.

## <a name="stop-automatic-seeding"></a>Arrêter l’amorçage automatique

Pour arrêter l’amorçage automatique pour un groupe de disponibilité, exécutez le script suivant sur le réplica principal :

```sql
ALTER AVAILABILITY GROUP [<availability_group_name>] 
    MODIFY REPLICA ON '<secondary_node>'   
    WITH (SEEDING_MODE = MANUAL)
GO
```

Le script ci-dessus annule tous les réplicas en cours d’amorçage et empêche SQL Server d’initialiser automatiquement des réplicas dans ce groupe de disponibilité. Il n’interrompt pas la synchronisation des réplicas déjà initialisés. 


## <a name="monitor-automatic-seeding-availability-group"></a>Surveiller un groupe de disponibilité à amorçage automatique

### <a name="use-system-dynamic-management-views-to-monitor-seeding"></a>Utiliser les vues de gestion dynamique du système pour surveiller l’amorçage

Les vues système suivantes indiquent l’état de l’amorçage automatique SQL Server.

**sys.dm_hadr_automatic_seeding** 

Sur le réplica principal, interrogez `sys.dm_hadr_automatic_seeding` pour vérifier l’état du processus d’amorçage automatique. La vue retourne une seule ligne pour chaque processus d’amorçage. Par exemple :

```sql
SELECT start_time, 
    completion_time
    is_source,
    current_state,
    failure_state,
    failure_state_desc
FROM sys.dm_hadr_automatic_seeding
```
 
**sys.dm_hadr_physical_seeding_stats** 

Sur le réplica principal, interrogez la vue de gestion dynamique `sys.dm_hadr_physical_seeding_stats` pour voir les statistiques physiques de chaque processus d’amorçage en cours d’exécution. La requête suivante retourne des lignes quand l’amorçage est en cours d’exécution :

```sql
SELECT * FROM sys.dm_hadr_physical_seeding_stats;
```

Les deux colonnes *total_disk_io_wait_time_ms* et *total_network_wait_time_ms* peuvent être utilisées pour déterminer le goulot d’étranglement dans le processus d’amorçage automatique. Les deux colonnes sont également présentes dans l’événement étendu *hadr_physical_seeding_progress*.

**total_disk_io_wait_time_ms** représente le temps d’attente du thread de sauvegarde/restauration sur le disque. Cette valeur est cumulative depuis le début de l’opération d’amorçage. Si les disques ne sont pas prêts pour lire ou écrire le flux de sauvegarde, le thread de sauvegarde/restauration passe à l’état de veille et sort de veille chaque seconde pour vérifier si le disque est prêt.
        
**total_network_wait_time_ms** est interprété différemment sur le réplica principal et le réplica secondaire. Sur le réplica principal, ce compteur représente le temps de contrôle du flux réseau. Sur le réplica secondaire, ce compteur représente le temps que le thread de restauration attend un message permettant d’écrire sur le disque.

### <a name="diagnose-database-initialization-using-automatic-seeding-in-the-error-log"></a>Diagnostiquer l’initialisation de bases de données à l’aide de l’amorçage automatique dans le journal des erreurs

Quand vous ajoutez une base de données à un groupe de disponibilité configuré pour l’amorçage automatique, SQL Server effectue une sauvegarde VDI sur le point de terminaison du groupe de disponibilité. Consultez le journal des erreurs SQL Server pour déterminer quand la sauvegarde a été effectuée et le serveur secondaire synchronisé.

### <a name="diagnose-database-level-health-with-extended-events"></a>Diagnostiquer l’intégrité au niveau de la base de données avec des événements étendus

L’amorçage automatique comporte de nouveaux événements étendus pour le suivi des modifications d’état, des échecs et des statistiques de performance pendant l’initialisation. 

Par exemple, ce script crée une session d’événements étendus qui capture les événements liés à l’amorçage automatique : 

```sql
CREATE EVENT SESSION [AlwaysOn_autoseed] ON SERVER 
    ADD EVENT sqlserver.hadr_automatic_seeding_state_transition,
    ADD EVENT sqlserver.hadr_automatic_seeding_timeout,
    ADD EVENT sqlserver.hadr_db_manager_seeding_request_msg,
    ADD EVENT sqlserver.hadr_physical_seeding_backup_state_change,
    ADD EVENT sqlserver.hadr_physical_seeding_failure,
    ADD EVENT sqlserver.hadr_physical_seeding_forwarder_state_change,
    ADD EVENT sqlserver.hadr_physical_seeding_forwarder_target_state_change,
    ADD EVENT sqlserver.hadr_physical_seeding_progress,
    ADD EVENT sqlserver.hadr_physical_seeding_restore_state_change,
    ADD EVENT sqlserver.hadr_physical_seeding_submit_callback
    ADD TARGET package0.event_file(
        SET filename=N'autoseed.xel',
            max_file_size=(5),
            max_rollover_files=(4)
        )
WITH (
    MAX_MEMORY=4096 KB,
    EVENT_RETENTION_MODE=ALLOW_SINGLE_EVENT_LOSS,
    MAX_DISPATCH_LATENCY=30 SECONDS,
    MAX_EVENT_SIZE=0 KB,
    MEMORY_PARTITION_MODE=NONE,
    TRACK_CAUSALITY=OFF,
    STARTUP_STATE=ON
    )
GO 

ALTER EVENT SESSION AlwaysOn_autoseed ON SERVER STATE=START
GO 
```


Le tableau suivant répertorie les événements étendus liés à l’amorçage automatique : 

| Name | Description|
|------------ |---------------| 
|hadr_db_manager_seeding_request_msg |  Message de demande d’amorçage.
|hadr_physical_seeding_backup_state_change |    Modification d’état côté sauvegarde d’amorçage physique.
|hadr_physical_seeding_restore_state_change |Modification d’état côté restauration d’amorçage physique.
|hadr_physical_seeding_forwarder_state_change | Modification d’état côté redirecteur d’amorçage physique.
|hadr_physical_seeding_forwarder_target_state_change |  Modification d’état côté cible du redirecteur d’amorçage physique.
|hadr_physical_seeding_submit_callback  |Événement de rappel de soumission d’amorçage physique.
|hadr_physical_seeding_failure  |Événement d’échec d’amorçage physique.
|hadr_physical_seeding_progress |   Événement de progression d’amorçage physique.
|hadr_physical_seeding_schedule_long_task_failure   |Événement d’échec de longue tâche de planification d’amorçage physique.
|hadr_automatic_seeding_start   |Se produit quand une opération d’amorçage automatique est soumise.
|hadr_automatic_seeding_state_transition    |Se produit quand une opération d’amorçage automatique change d’état.
|hadr_automatic_seeding_success |Se produit quand une opération d’amorçage automatique aboutit.
|hadr_automatic_seeding_failure |Se produit quand une opération d’amorçage automatique échoue.
|hadr_automatic_seeding_timeout |Se produit quand une opération d’amorçage automatique expire.

### <a name="other-troubleshooting-considerations"></a>Autres considérations liées à la résolution des problèmes

**Surveillance lors de l’amorçage automatique**

Interrogez `sys.dm_hadr_physical_seeding_stats` pour connaître les processus d’amorçage automatique en cours d’exécution. La vue retourne une seule ligne pour chaque base de données. Par exemple :

```sql
SELECT local_database_name, 
    role_desc, 
    internal_state_desc, 
    transfer_rate_bytes_per_second, 
    transferred_size_bytes, 
    database_size_bytes, 
    start_time_utc, 
    end_time_utc, estimate_time_complete_utc, 
    total_disk_io_wait_time_ms, 
    total_network_wait_time_ms, 
    is_compression_enabled 
FROM sys.dm_hadr_physical_seeding_stats
```

**Résoudre les problèmes liés à la non-apparition d’une base de données dans un groupe de disponibilité configuré pour l’amorçage automatique**


Quand une base de données n’apparaît pas dans un groupe de disponibilité pour lequel l’amorçage automatique est activé, ce dernier a probablement échoué. La base de données n’est donc pas ajoutée au groupe de disponibilité sur les réplicas principal et secondaire. Interrogez `sys.dm_hadr_automatic_seeding` sur les réplicas principaux et secondaires. Par exemple, exécutez la requête suivante pour identifier l’état d’échec de l’amorçage automatique.

```sql
SELECT start_time, 
    completion_time, 
    is_source, 
    current_state, 
    failure_state, 
    failure_state_desc, 
    error_code 
FROM sys.dm_hadr_automatic_seeding
```

## <a name="automatic-seeding-and-performance-considerations"></a>Considérations liées à l’amorçage automatique et aux performances

SQL Server utilise un nombre fixe de threads pour l’amorçage automatique. Sur l’instance principale, SQL Server utilise un seul thread par numéro d’unité logique pour lire les modifications. Sur l’instance secondaire, SQL Server utilise un seul thread par numéro d’unité logique pour initialiser la base de données.

Définissez l’indicateur de trace 9567 sur le réplica principal pour activer la compression du flux de données pendant l’amorçage automatique. Le temps de transfert de l’amorçage automatique est alors considérablement réduit, mais l’utilisation du processeur est quant à elle accrue. Pour plus d’informations, consultez [Régler la compression pour le groupe de disponibilité](../../../database-engine/availability-groups/windows/tune-compression-for-availability-group.md). 


## <a name="when-not-to-use-automatic-seeding"></a>Quand ne pas utiliser l’amorçage automatique

Dans certains scénarios, l’amorçage automatique ne s’avère pas optimal pour initialiser un réplica secondaire. Au cours de l’amorçage automatique, SQL Server effectue une sauvegarde sur le réseau à des fins d’initialisation. Ce processus peut être lent si les bases de données sont très volumineuses ou si le réplica secondaire est distant. Le journal des transactions de ces bases de données ne peut pas être tronqué pendant le processus de sauvegarde. Par conséquent, tout processus d’initialisation qui se prolonge sur une base de données volumineuse peut entraîner une croissance importante du journal des transactions.
Avant d’ajouter une base de données à un groupe de disponibilité par amorçage automatique, évaluez la taille de la base de données, le chargement et la distance de site entre les réplicas.

## <a name="resources"></a>Ressources

[CREATE AVAILABILITY GROUP (Transact-SQL)](../../../t-sql/statements/create-availability-group-transact-sql.md)

[Guide de surveillance et de résolution des problèmes des groupes de disponibilité Always On](/previous-versions/sql/sql-server-guides/dn135328(v=sql.110))