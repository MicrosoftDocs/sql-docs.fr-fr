---
title: Amorçage automatique pour les réplicas secondaires
description: Découvrez comment l’amorçage automatique peut initialiser les réplicas secondaires dans un groupe de disponibilité Always On avec SQL 2016 (ou une version ultérieure).
ms.custom: seo-lt-2019
ms.date: 11/27/2018
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
helpviewer_keywords:
- Automatic seeding [SQL Server], secondary replica
ms.assetid: ''
author: cawrites
ms.author: chadam
ms.openlocfilehash: 75aa499fff65f9b7bf6ab10b6cdc501c172fbc03
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97644362"
---
# <a name="use-automatic-seeding-to-initialize-a-secondary-replica-for-an-always-on-availability-group"></a>Utiliser l’amorçage automatique pour initialiser un réplica secondaire dans un groupe de disponibilité Always On
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

Dans SQL Server 2012 et 2014, la seule façon d’initialiser un réplica secondaire dans un groupe de disponibilité SQL Server Always On est d’utiliser la sauvegarde, la copie et la restauration. SQL Server 2016 introduit une nouvelle fonctionnalité pour initialiser un réplica secondaire : l’*amorçage automatique*. L’amorçage automatique utilise le transport de flux de journaux pour transmettre en continu la sauvegarde à l’aide de VDI vers le réplica secondaire pour chaque base de données du groupe de disponibilité, en utilisant les points de terminaison configurés. Cette nouvelle fonctionnalité peut être utilisée lors de la création initiale d’un groupe de disponibilité ou quand une base de données lui est ajoutée. L’amorçage automatique est disponible dans toutes les éditions de SQL Server prenant en charge les groupes de disponibilité Always On et peut être utilisé avec les groupes de disponibilité traditionnels et les [groupes de disponibilité distribués](distributed-availability-groups.md).

## <a name="security"></a>Sécurité

Les autorisations de sécurité varient selon le type de réplica à initialiser :

* Pour un groupe de disponibilité traditionnel, les autorisations doivent être accordées au groupe de disponibilité sur le réplica secondaire quand il est joint au groupe de disponibilité. Dans Transact-SQL, utilisez la commande `ALTER AVAILABILITY GROUP [<AGName>] GRANT CREATE ANY DATABASE`.
* Pour un groupe de disponibilité distribué où les bases de données du réplica à créer sont sur le réplica principal du deuxième groupe de disponibilité, aucune autorisation supplémentaire n’est nécessaire, car il s’agit déjà d’un réplica principal.
* Pour un réplica secondaire sur le deuxième groupe de disponibilité d’un groupe de disponibilité distribué, vous devez utiliser la commande `ALTER AVAILABILITY GROUP [<2ndAGName>] GRANT CREATE ANY DATABASE`. Ce réplica secondaire est amorcé à partir du réplica principal du deuxième groupe de disponibilité.

## <a name="performance-and-transaction-log-impact-on-the-primary-replica"></a>Impact en termes de performances et de journal des transactions sur le réplica principal

L’amorçage automatique peut ou non être pratique pour initialiser un réplica secondaire, selon la taille de la base de données, la vitesse du réseau et la distance entre le réplica principal et les réplicas secondaires. Prenons l’exemple suivant :

* La taille de la base de données est de 5 To.
* La vitesse du réseau est de 1 Gbit/s.
* La distance entre les deux sites est de 1 600 kilomètres.

Si la bande passante complète est disponible, un réseau d’1 Gbit/s peut fournir un débit constant de 125 Mo par seconde. Dans cet exemple, l’amorçage automatique prendrait un petit peu plus de 11 heures. Dans la pratique, le processus d’amorçage automatique est plus lent, car le signal sur le réseau se dégrade sur les longues distances et la liaison est partagée avec d’autres ressources du réseau. Pendant l’amorçage, le journal des transactions sur la base de données du réplica principal continue de croître et il ne peut pas être tronqué tant que l’amorçage automatique de cette base de données n’est pas terminé.  Le journal des transactions peut ensuite être tronqué via une sauvegarde du journal des transactions.

L’amorçage automatique est un processus monothread qui peut gérer jusqu’à cinq bases de données. Le monothread peut affecter les performances, surtout si le groupe de disponibilité a plusieurs bases de données.

La compression peut être utilisée pour l’amorçage automatique, mais elle est désactivée par défaut. L’activation de la compression réduit la bande passante réseau et accélère éventuellement le processus, mais l’inconvénient est une charge supplémentaire pour le processeur. Pour utiliser la compression pendant l’amorçage automatique, activez l’indicateur de trace 9567. Consultez [Régler la compression pour un groupe de disponibilité](tune-compression-for-availability-group.md).

## <a name="disk-layout"></a><a name = "disklayout"></a> Disposition des disques

Dans SQL Server 2016 et antérieur, le dossier où la base de données est créée par l’amorçage automatique doit déjà exister et avoir le même chemin que sur le réplica principal. 

Dans SQL Server 2017, Microsoft recommande d’utiliser les mêmes chemins de fichiers journaux et de fichiers de données sur tous les réplicas qui participent à un groupe de disponibilité, mais vous pouvez utiliser des chemins différents si nécessaire. Par exemple, dans un groupe à haute disponibilité multiplateforme, une instance de SQL Server est sur Windows et une autre instance de SQL Server est sur Linux. Les différentes plateformes ont des chemins par défaut différents. SQL Server 2017 prend en charge les réplicas de groupe de disponibilité sur les instances de SQL Server avec des chemins par défaut différents.

Le tableau suivant présente des exemples de dispositions de disques de données prises en charge qui peuvent prendre en charge l’amorçage automatique :

|Instance principale</br>Chemin de données par défaut|Instance secondaire</br>Chemin de données par défaut|Instance principale</br>Emplacement du fichier source|Instance secondaire</br> Emplacement du fichier cible
|:------|:------|:------|:------
|c:\\data\\ |/var/opt/mssql/data/ |c:\\data\\ |/var/opt/mssql/data/|
|c:\\data\\ |/var/opt/mssql/data/ |c:\\data\\group1\\ |/var/opt/mssql/data/group1/|
|c:\\data\\ |d:\\data\\ |c:\\data\\ |d:\\data\\
|c:\\data\\ |d:\\data\\ |c:\\data\\group1\\ |d:\\data\\group1\

Les scénarios, où l’emplacement de la base de données du réplica principal et secondaire n’est pas le chemin par défaut de l’instance, ne sont pas impactés par cette modification. Les conditions qui exigent que les chemins de fichiers de réplica secondaire correspondent aux chemins de fichiers de réplica principal restent les mêmes.

|Instance principale</br>Chemin de données par défaut|Instance secondaire</br>Chemin de données par défaut|Instance principale</br>Emplacement du fichier|Instance secondaire</br> Emplacement du fichier
|:------|:------|:------|:------
|c:\\data\\ |c:\\data\\ |d:\\group1\\ |d:\\group1\\
|c:\\data\\ |c:\\data\\ |d:\\data\\ |d:\\data\\
|c:\\data\\ |c:\\data\\ |d:\\data\\group1\\ |d:\\data\\group1\\

Si vous mélangez des chemins par défaut et non par défaut sur les réplicas principaux et secondaires, SQL Server 2017 se comporte différemment des versions précédentes. Le tableau suivant montre le comportement de SQL Server 2017.

|Instance principale</br>Chemin de données par défaut |Instance secondaire</br>Chemin de données par défaut |Instance principale</br>Emplacement du fichier |SQL Server 2016 </br>Instance secondaire</br>Emplacement du fichier |SQL Server 2017 </br>Instance secondaire</br>Emplacement du fichier
|:------|:------|:------|:------|:------
|c:\\data\\ |d:\\data\\ |c:\\data\\ |c:\\data\\ |d:\\data\\ 
|c:\\data\\ |d:\\data\\ |c:\\data\\group1\\ |c:\\data\\group1\\ |d:\\data\\group1\\

Pour rétablir le comportement de SQL Server 2016 et antérieur, activez l’indicateur de trace 9571. Pour plus d’informations sur la façon d’activer des indicateurs de trace, consultez [DBCC TRACEON (Transact-SQL)](../../../t-sql/database-console-commands/dbcc-traceon-transact-sql.md).


## <a name="create-an-availability-group-with-automatic-seeding"></a>Créer un groupe de disponibilité par amorçage automatique

Vous créez un groupe de disponibilité par amorçage automatique avec Transact-SQL ou SQL Server Management Studio (SSMS, version 17 ou ultérieure). Pour utiliser l’Assistant Groupe de disponibilité dans SSMS, suivez [ces instructions](use-the-availability-group-wizard-sql-server-management-studio.md). Quand vous arrivez à l’étape 9, notez que l’amorçage automatique est la première option (et aussi l’option par défaut).

![Sélectionner la synchronisation de données initiale][1]

L’exemple suivant crée un groupe de disponibilité avec amorçage automatique à l’aide de Transact-SQL. Consultez aussi la rubrique [Créer un groupe de disponibilité (Transact-SQL)](create-an-availability-group-transact-sql.md). L’amorçage est activé sur un réplica secondaire en affectant la valeur `AUTOMATIC` à l’option `SEEDING_MODE`. Le comportement par défaut est `MANUAL`, qui est le comportement des versions antérieures à SQL Server 2016 nécessitant la réalisation d’une sauvegarde de la base de données sur le réplica principal, la copie du fichier de sauvegarde sur le réplica secondaire et la restauration de la sauvegarde `WITH NORECOVERY`.

```sql
CREATE AVAILABILITY GROUP [<AGName>]
  FOR DATABASE db1
  REPLICA ON N'Primary_Replica'
WITH (
  ENDPOINT_URL = N'TCP://Primary_Replica.Contoso.com:5022', 
  FAILOVER_MODE = AUTOMATIC, 
  AVAILABILITY_MODE = SYNCHRONOUS_COMMIT, 
),
  N'Secondary_Replica' WITH (
    ENDPOINT_URL = N'TCP://Secondary_Replica.Contoso.com:5022', 
    FAILOVER_MODE = AUTOMATIC, 
    SEEDING_MODE = AUTOMATIC);
 GO
```

La définition de `SEEDING_MODE` sur un réplica principal pendant une instruction `CREATE AVAILABILITY GROUP` n’a pas d’effet, car le réplica principal contient déjà la copie principale en lecture/écriture de la base de données. `SEEDING_MODE` peut s’appliquer seulement quand un autre réplica a été changé en réplica principal et qu’une base de données a été ajoutée. Le mode d’amorçage peut être changé ultérieurement. Consultez [Changer le mode d’amorçage d’un réplica](#change-the-seeding-mode-of-a-replica).

Sur une instance qui devient un réplica secondaire, une fois que l’instance est jointe, le message suivant est ajouté dans le journal SQL Server :

>Le réplica de disponibilité local pour le groupe de disponibilité « nom_groupe_de_disponibilité » n’a pas reçu l’autorisation de créer des bases de données, mais son paramètre `SEEDING_MODE` est défini sur `AUTOMATIC`. Utilisez `ALTER AVAILABILITY GROUP ... GRANT CREATE ANY DATABASE` pour autoriser la création de bases de données amorcée par le réplica de disponibilité principal.

### <a name="grant-create-database-permission-on-secondary-replica-to-availability-group"></a><a name = "grantCreate"></a> Octroyer l’autorisation de créer une base de données sur un réplica secondaire au groupe de disponibilité

Après la jointure, accordez au groupe de disponibilité l’autorisation de créer des bases de données sur l’instance de réplica secondaire de SQL Server. Pour que l’amorçage automatique fonctionne, le groupe de disponibilité a besoin de l’autorisation de créer une base de données. 

>[!TIP]
>Lorsque le groupe de disponibilité crée une base de données sur un réplica secondaire, il définit « sa » (c’est-à-dire, le compte avec le sid 0x01) en tant que propriétaire de la base de données. 
>
>Pour changer le propriétaire de la base de données après qu’un réplica secondaire a créé automatiquement une base de données, utilisez `ALTER AUTHORIZATION`. Consultez [ALTER AUTHORIZATION (Transact-SQL)](../../../t-sql/statements/alter-authorization-transact-sql.md).
 
L’exemple suivant accorde cette autorisation à un groupe de disponibilité nommé AGName.

```sql
ALTER AVAILABILITY GROUP [<AGName>] 
    GRANT CREATE ANY DATABASE
 GO
```

Si nécessaire, définissez le propriétaire de la base de données sur le réplica secondaire. 

### <a name="verify-automatic-seeding"></a>Vérifier l’amorçage automatique

En cas de réussite, la ou les bases de données sont créées automatiquement sur le réplica secondaire avec un état qui peut être :

* SYNCHRONIZED si le réplica secondaire est configuré pour être synchrone et que les données sont synchronisées.
* SYNCHRONIZING si le réplica secondaire est configuré avec le déplacement des données asynchrones, ou quand il est configuré pour être synchrone mais n’est pas encore synchronisé avec le réplica principal.

<a name="sql-server-log"></a> En plus des [vues de gestion dynamiques](#dynamic-management-views) décrites ci-dessous, le démarrage et l’achèvement de l’amorçage automatique figurent dans le journal SQL Server :

![Journal SQL Server][2]

## <a name="combine-backup-and-restore-with-automatic-seeding"></a>Combiner la sauvegarde et la restauration avec l’amorçage automatique

Il est possible de combiner la sauvegarde, la copie et la restauration traditionnelles avec l’amorçage automatique. Dans ce cas, restaurez d’abord la base de données sur un réplica secondaire, y compris tous les journaux de transactions disponibles. Ensuite, activez l’amorçage automatique lors de la création du groupe de disponibilité pour « rattraper » la base de données du réplica secondaire, comme si une sauvegarde de la fin du journal était restaurée (consultez [Sauvegardes de la fin du journal (SQL Server)](../../../relational-databases/backup-restore/tail-log-backups-sql-server.md)).

## <a name="add-a-database-to-an-availability-group-with-automatic-seeding"></a>Ajouter une base de données à un groupe de disponibilité par amorçage automatique

Vous pouvez ajouter une base de données à un groupe de disponibilité par amorçage automatique avec Transact-SQL ou SQL Server Management Studio (SSMS, version 17 ou ultérieure).
Si le réplica secondaire a utilisé l’amorçage automatique quand il a été ajouté au groupe de disponibilité, aucune opération supplémentaire ne doit être effectuée. Si le réplica secondaire a utilisé la sauvegarde, la copie et la restauration, changez d’abord le mode d’amorçage (voir la section suivante) puis, lors de l’ajout de la base de données, utilisez l’instruction `GRANT`. Consultez [Groupe de disponibilité - Ajouter une base de données](availability-group-add-a-database.md).

## <a name="change-the-seeding-mode-of-a-replica"></a>Changer le mode d’amorçage d’un réplica

Le mode d’amorçage d’un réplica peut être changé après la création du groupe de disponibilité : l’amorçage automatique peut donc être activé ou désactivé. L’activation de l’amorçage automatique après la création permet de créer une base de données à ajouter au groupe de disponibilité en utilisant l’amorçage automatique si elle a été créée avec la sauvegarde, la copie et la restauration. Par exemple :

```sql
ALTER AVAILABILITY GROUP [AGName]
  MODIFY REPLICA ON 'Replica_Name'
  WITH (SEEDING_MODE = AUTOMATIC)
```

Pour désactiver l’amorçage automatique, utilisez la valeur MANUAL.

## <a name="prevent-automatic-seeding-after-an-availability-group-is-created"></a>Empêcher l’amorçage automatique après la création d’un groupe de disponibilité

Si vous ne souhaitez pas désactiver complètement l’amorçage automatique pour un réplica secondaire, mais que vous voulez empêcher temporairement le réplica secondaire de pouvoir créer automatiquement des bases de données, refuser l’autorisation CREATE au groupe de disponibilité. C’est le cas quand une base de données est ajoutée au groupe de disponibilité, mais que le groupe de disponibilité ne doit pas être autorisé à créer la base de données sur un réplica secondaire.

```sql
ALTER AVAILABILITY GROUP [AGName] 
    DENY CREATE ANY DATABASE
GO
```

## <a name="monitor-automatic-seeding"></a>Surveiller l’amorçage automatique

Il existe quatre façons de surveiller et de résoudre les problèmes de l’amorçage automatique :

* [Journal SQL Server](#sql-server-log) comme décrit précédemment
* [Vues de gestion dynamique](#dynamic-management-views)
* [Tables d’historique des sauvegardes](#backup-history-tables)
* [Événements étendus](#extended-events)

### <a name="dynamic-management-views"></a>Vues de gestion dynamique

Il existe deux vues de gestion dynamique pour l’analyse de l’amorçage : `sys.dm_hadr_automatic_seeding` et `sys.dm_hadr_physical_seeding_stats`.

* `sys.dm_hadr_automatic_seeding` contient l’état général de l’amorçage automatique et conserve l’historique à chaque fois qu’il est exécuté (qu’il réussisse ou non). La colonne `current_state` a la valeur COMPLETED ou FAILED. Si la valeur est FAILED, utilisez la valeur de `failure_state_desc` pour diagnostiquer le problème. Il peut être nécessaire de combiner ces informations avec celles qui se trouvent dans le [journal SQL Server](#sql-server-log) pour voir ce qui s’est mal déroulé. Cette vue de gestion dynamique contient des données sur le réplica principal et sur tous les réplicas secondaires.

* `sys.dm_hadr_physical_seeding_stats` indique l’état de l’opération d’amorçage automatique au fil de son exécution. Comme avec `sys.dm_hadr_automatic_seeding`, elle retourne des valeurs pour les réplicas principaux et secondaires, mais cet historique n’est pas stocké. Les valeurs concernent seulement l’exécution en cours et ne sont pas conservées. Les colonnes dignes d’intérêt sont `start_time_utc`, `end_time_utc`, `estimate_time_complete_utc`, `total_disk_io_wait_time_ms`, `total_network_wait_time_ms` et, si l’opération d’amorçage échoue, failure_message.

### <a name="backup-history-tables"></a>Tables d’historique des sauvegardes

L’amorçage automatique place également des entrées dans les tables `msdb`, qui stockent l’historique pour les sauvegardes et les restaurations. Sur le réplica secondaire qui reçoit l’amorçage automatique, la colonne physical_device_name de la table `backupmediafamily` a comme valeur un GUID, et l’entrée correspondante dans `backupset` a le nom du réplica principal pour server_name and machine_name.

### <a name="extended-events"></a>Événements étendus

L’amorçage automatique ajoute de nouveaux événements étendus pour le suivi des modifications d’état, des échecs et des statistiques de performances lors de l’initialisation.
Par exemple, le script suivant crée une session d’événements étendus qui capture les événements liés à l’amorçage automatique.

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

Le tableau suivant répertorie les événements étendus relatifs à l’amorçage automatique.

|Name|Description|
|----|-----------|
|hadr_db_manager_seeding_request_msg|Message de demande d’amorçage.|
|hadr_physical_seeding_backup_state_change|Modification d’état côté sauvegarde d’amorçage physique.|
|hadr_physical_seeding_restore_state_change|Modification d’état côté restauration d’amorçage physique.|
|hadr_physical_seeding_forwarder_state_change|Modification d’état côté redirecteur d’amorçage physique.|
|hadr_physical_seeding_forwarder_target_state_change|Modification d’état côté cible du redirecteur d’amorçage physique.|
|hadr_physical_seeding_submit_callback|Événement de rappel de soumission d’amorçage physique.|
|hadr_physical_seeding_failure|Événement d’échec d’amorçage physique.|
|hadr_physical_seeding_progress|Événement de progression d’amorçage physique.|
|hadr_physical_seeding_schedule_long_task_failure|Événement d’échec de tâche longue de planification d’amorçage physique.|
|hadr_automatic_seeding_start|Se produit quand une opération d’amorçage automatique est soumise.|
|hadr_automatic_seeding_state_transition|Se produit quand une opération d’amorçage automatique change d’état.|
|hadr_automatic_seeding_success|Se produit quand une opération d’amorçage automatique aboutit.|
|hadr_automatic_seeding_failure|Se produit quand une opération d’amorçage automatique échoue.|
|hadr_automatic_seeding_timeout|Se produit quand une opération d’amorçage automatique expire.|

## <a name="see-also"></a>Voir aussi

[ALTER AVAILABILITY GROUP (Transact-SQL)](../../../t-sql/statements/alter-availability-group-transact-sql.md)

[CREATE AVAILABILITY GROUP (Transact-SQL)](../../../t-sql/statements/create-availability-group-transact-sql.md)

[Guide de surveillance et de résolution des problèmes des groupes de disponibilité Always On](/previous-versions/sql/sql-server-guides/dn135328(v=sql.110))

<!--Image references-->
[1]: ./media/auto-seed-new-availability-group.png
[2]: ./media/auto-seed-sql-server-log.png