---
description: Instructions RESTORE (Transact-SQL)
title: RESTORE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/04/2019
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- RESTORE DATABASE
- RESTORE_TSQL
- RESTORE_DATABASE_TSQL
- RESTORE
- RESTORE_LOG_TSQL
- RESTORE LOG
dev_langs:
- TSQL
helpviewer_keywords:
- RESTORE DATABASE, see RESTORE statement
- full backups [SQL Server]
- RECOVERY option
- database snapshots [SQL Server], reverting to
- STOPAT syntax
- differential backups, RESTORE statement
- point in time recovery [SQL Server]
- restoring [SQL Server]
- NORECOVERY option
- online restores [SQL Server], RESTORE statement
- moving databases
- RESTORE statement, syntax
- cross-platform restores
- restoring databases [SQL Server], options
- RESTORE statement
- snapshots [SQL Server database snapshots], reverting to
- reverting database snapshots
- transaction log backups [SQL Server], RESTORE statement
- RESTORE LOG, see RESTORE statement
ms.assetid: 877ecd57-3f2e-4237-890a-08f16e944ef1
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current||>=aps-pdw-2016'
ms.openlocfilehash: fe799de197de0fc18bb8750b4a6f8db2e288be32
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100017465"
---
# <a name="restore-statements-transact-sql"></a>Instructions RESTORE (Transact-SQL)

Restaure les sauvegardes des bases de données SQL réalisées à l’aide de la commande BACKUP.

Cliquez sur l’un des onglets suivants pour connaître la syntaxe, les arguments, les remarques, les autorisations et des exemples propres à la version de SQL que vous utilisez.

Pour plus d’informations sur les conventions de la syntaxe, consultez [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).

[!INCLUDE[select-product](../../includes/select-product.md)]

::: moniker range=">=sql-server-2016||>=sql-server-linux-2017"

:::row:::
    :::column:::
        **_\* SQL Server \*_** &nbsp;
    :::column-end:::
    :::column:::
        [SQL Managed Instance](restore-statements-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Analytics Platform<br />System (PDW)](restore-statements-transact-sql.md?view=aps-pdw-2016&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="sql-server"></a>SQL Server

Cette commande vous permet d'effectuer les scénarios de restauration suivants :

- Restaurer une base de données complète à partir d'une sauvegarde de base de données complète (restauration complète)
- Restaurer une partie d'une base de données (restauration partielle)
- Restaurer des fichiers ou des groupes de fichiers spécifiques dans une base de données (restauration de fichiers)
- Restaurer des pages spécifiques dans une base de données (restauration de pages)
- Restaurer un journal des transactions dans une base de données (restauration de journal des transactions)
- Rétablir une base de données au point d'instantané de base de données.

Pour plus d’informations sur les scénarios de restauration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consultez l’article [Vue d’ensemble de la restauration et de la récupération](../../relational-databases/backup-restore/restore-and-recovery-overview-sql-server.md). Pour plus d’informations sur les descriptions des arguments, consultez l’article [Instructions RESTORE – Arguments ](../../t-sql/statements/restore-statements-arguments-transact-sql.md). Lors de la restauration d’une base de données d’une autre instance, tenez compte des informations de la page [Gérer les métadonnées lors de la mise à disposition d’une base de données sur une autre instance de serveur (SQL Server)](../../relational-databases/databases/manage-metadata-when-making-a-database-available-on-another-server.md).

> [!NOTE]
> Pour plus d’informations sur la restauration à partir du service Stockage Blob Microsoft Azure, consultez [Sauvegarde et restauration SQL Server avec le service Stockage Blob Microsoft Azure](../../relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service.md).

## <a name="syntax"></a>Syntaxe

```syntaxsql
--To Restore an Entire Database from a Full database backup (a Complete Restore):
RESTORE DATABASE { database_name | @database_name_var }
 [ FROM <backup_device> [ ,...n ] ]
 [ WITH
   {
    [ RECOVERY | NORECOVERY | STANDBY =
        {standby_file_name | @standby_file_name_var }
       ]
   | ,  <general_WITH_options> [ ,...n ]
   | , <replication_WITH_option>
   | , <change_data_capture_WITH_option>
   | , <FILESTREAM_WITH_option>
   | , <service_broker_WITH options>
   | , \<point_in_time_WITH_options-RESTORE_DATABASE>
   } [ ,...n ]
 ]
[;]

--To perform the first step of the initial restore sequence of a piecemeal restore:
RESTORE DATABASE { database_name | @database_name_var }
   <files_or_filegroups> [ ,...n ]
 [ FROM <backup_device> [ ,...n ] ]
   WITH
      PARTIAL, NORECOVERY
      [  , <general_WITH_options> [ ,...n ]
       | , \<point_in_time_WITH_options-RESTORE_DATABASE>
      ] [ ,...n ]
[;]  
  
--To Restore Specific Files or Filegroups:
RESTORE DATABASE { database_name | @database_name_var }
   <file_or_filegroup> [ ,...n ]
 [ FROM <backup_device> [ ,...n ] ]
   WITH
   {
      [ RECOVERY | NORECOVERY ]
      [ , <general_WITH_options> [ ,...n ] ]
   } [ ,...n ]
[;]  
  
--To Restore Specific Pages:
RESTORE DATABASE { database_name | @database_name_var }
   PAGE = 'file:page [ ,...n ]'
 [ , <file_or_filegroups> ] [ ,...n ]
 [ FROM <backup_device> [ ,...n ] ]
   WITH
       NORECOVERY
      [ , <general_WITH_options> [ ,...n ] ]
[;]

--To Restore a Transaction Log:
RESTORE LOG { database_name | @database_name_var }
 [ <file_or_filegroup_or_pages> [ ,...n ] ]
 [ FROM <backup_device> [ ,...n ] ]
 [ WITH
   {
     [ RECOVERY | NORECOVERY | STANDBY =
        {standby_file_name | @standby_file_name_var }
       ]
    | , <general_WITH_options> [ ,...n ]
    | , <replication_WITH_option>
    | , \<point_in_time_WITH_options-RESTORE_LOG>
   } [ ,...n ]
 ]
[;]

--To Revert a Database to a Database Snapshot:
RESTORE DATABASE { database_name | @database_name_var }
FROM DATABASE_SNAPSHOT = database_snapshot_name

<backup_device>::=
{
   { logical_backup_device_name |
      @logical_backup_device_name_var }
 | { DISK
     | TAPE
     | URL
   } = { 'physical_backup_device_name' |
      @physical_backup_device_name_var }
}
Note: URL is the format used to specify the location and the file name for the Microsoft Azure Blob. Although Microsoft Azure storage is a service, the implementation is similar to disk and tape to allow for a consistent and seamless restore experience for all the three devices.
<files_or_filegroups>::=
{
   FILE = { logical_file_name_in_backup | @logical_file_name_in_backup_var }
 | FILEGROUP = { logical_filegroup_name | @logical_filegroup_name_var }
 | READ_WRITE_FILEGROUPS
}

<general_WITH_options> [ ,...n ]::=
--Restore Operation Options
   MOVE 'logical_file_name_in_backup' TO 'operating_system_file_name'
          [ ,...n ]
 | REPLACE
 | RESTART
 | RESTRICTED_USER | CREDENTIAL

--Backup Set Options
 | FILE = { backup_set_file_number | @backup_set_file_number }
 | PASSWORD = { password | @password_variable }

--Media Set Options
 | MEDIANAME = { media_name | @media_name_variable }
 | MEDIAPASSWORD = { mediapassword | @mediapassword_variable }
 | BLOCKSIZE = { blocksize | @blocksize_variable }

--Data Transfer Options
 | BUFFERCOUNT = { buffercount | @buffercount_variable }
 | MAXTRANSFERSIZE = { maxtransfersize | @maxtransfersize_variable }

--Error Management Options
 | { CHECKSUM | NO_CHECKSUM }
 | { STOP_ON_ERROR | CONTINUE_AFTER_ERROR }

--Monitoring Options
 | STATS [ = percentage ]

--Tape Options.
 | { REWIND | NOREWIND }
 | { UNLOAD | NOUNLOAD }

<replication_WITH_option>::=
 | KEEP_REPLICATION

<change_data_capture_WITH_option>::=
 | KEEP_CDC

<FILESTREAM_WITH_option>::=
 | FILESTREAM ( DIRECTORY_NAME = directory_name )

<service_broker_WITH_options>::=
 | ENABLE_BROKER
 | ERROR_BROKER_CONVERSATIONS
 | NEW_BROKER

\<point_in_time_WITH_options-RESTORE_DATABASE>::=
 | {
   STOPAT = { 'datetime'| @datetime_var }
 | STOPATMARK = 'lsn:lsn_number'
                 [ AFTER 'datetime']
 | STOPBEFOREMARK = 'lsn:lsn_number'
                 [ AFTER 'datetime']
   }

\<point_in_time_WITH_options-RESTORE_LOG>::=
 | {
   STOPAT = { 'datetime'| @datetime_var }
 | STOPATMARK = { 'mark_name' | 'lsn:lsn_number' }
                 [ AFTER 'datetime']
 | STOPBEFOREMARK = { 'mark_name' | 'lsn:lsn_number' }
                 [ AFTER 'datetime']
   }
```

## <a name="arguments"></a>Arguments

Pour une description des arguments, consultez l’article [Instructions RESTORE – Arguments ](../../t-sql/statements/restore-statements-arguments-transact-sql.md).

## <a name="about-restore-scenarios"></a>À propos des scénarios de restauration

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] prend en charge un large éventail de scénarios de restauration :

- restauration de base de données complète

  Restaure l'ensemble de la base de données ; commence par une sauvegarde de base de données complète, qui peut être suivie par la restauration d'une sauvegarde de base de données différentielle (et des sauvegardes du journal). Pour plus d’informations, consultez l’article [Restaurations complètes de bases de données (mode de récupération simple)](../../relational-databases/backup-restore/complete-database-restores-simple-recovery-model.md) ou [Restaurations complètes de bases de données (mode de récupération complète)](../../relational-databases/backup-restore/complete-database-restores-full-recovery-model.md).

- Restauration de fichiers

  Restaure un fichier ou un groupe de fichiers dans une base de données de plusieurs groupes de fichiers. Notez qu'en mode de récupération simple, le fichier doit appartenir à un groupe de fichiers en lecture seule. Une fois que la restauration complète des fichiers a été effectuée, une sauvegarde différentielle des fichiers peut être restaurée. Pour plus d’informations, consultez les articles [Restaurations de fichiers (mode de récupération complète)](../../relational-databases/backup-restore/file-restores-full-recovery-model.md) et [Restauration de fichiers (mode de récupération simple)](../../relational-databases/backup-restore/file-restores-simple-recovery-model.md).

- Restauration de pages

  Restaure des pages individuelles. La restauration de pages n'est disponible qu'en mode de récupération complète et en mode de récupération utilisant les journaux de transactions. Pour plus d’informations, consultez l’article [Restaurer des pages (SQL Server)](../../relational-databases/backup-restore/restore-pages-sql-server.md).

- Restauration fragmentaire

  Restaure la base de données par étape ; commence avec le groupe de fichiers primaire et se poursuit avec un ou plusieurs des groupes de fichiers secondaires. Une restauration fragmentaire commence par une instruction RESTORE DATABASE utilisant l'option PARTIAL et spécifiant un ou plusieurs groupes de fichiers à restaurer. Pour plus d’informations, consultez l’article [Restaurations fragmentaires (SQL Server)](../../relational-databases/backup-restore/piecemeal-restores-sql-server.md).

- Récupération uniquement

  Récupère les données déjà cohérentes avec la base de données et qui doivent être disponibles. Pour plus d’informations, consultez l’article [Récupérer une base de données sans restaurer les données](../../relational-databases/backup-restore/recover-a-database-without-restoring-data-transact-sql.md).

- Restauration du journal des transactions

  En mode de récupération complète ou en mode de récupération utilisant les journaux de transactions, la restauration des sauvegardes du journal est nécessaire pour atteindre le point de récupération souhaité. Pour plus d’informations sur la restauration des sauvegardes de fichiers journaux, consultez l’article [Appliquer les sauvegardes du journal de transactions (SQL Server)](../../relational-databases/backup-restore/apply-transaction-log-backups-sql-server.md).

- Préparer une base de données de disponibilité pour un groupe de disponibilité AlwaysOn

  Pour plus d’informations, consultez l’article [Préparer manuellement une base de données secondaire pour un groupe de disponibilité (SQL Server)](../../database-engine/availability-groups/windows/manually-prepare-a-secondary-database-for-an-availability-group-sql-server.md).

- Préparer une base de données miroir pour la mise en miroir de bases de données

  Pour plus d’informations, consultez l’article [Préparer une base de données miroir pour la mise en miroir (SQL Server)](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md).

- Restauration en ligne

  > [!NOTE]
  > La restauration en ligne est autorisée uniquement dans l'édition Enterprise de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et versions ultérieures.

Si la restauration en ligne est prise en charge et si la base de données est en ligne, les restaurations de fichiers et de pages s'effectuent automatiquement en ligne, ainsi que les restaurations du groupe de fichiers secondaire une fois la phase initiale de restauration fragmentaire effectuée.

  > [!NOTE]
  > Les restaurations en ligne peuvent impliquer des [transactions différées](../../relational-databases/backup-restore/deferred-transactions-sql-server.md).

Pour plus d’informations, consultez l’article [Restauration en ligne](../../relational-databases/backup-restore/online-restore-sql-server.md).

## <a name="additional-considerations-about-restore-options"></a>Considérations supplémentaires à propos des options RESTORE

### <a name="discontinued-restore-keywords"></a>Mots clés RESTORE supprimés

Les mots clés suivants ont été supprimés dans [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] :

|Mot clé supprimé|Remplacé par...|Exemple de mot clé de remplacement|
|--------------------------|------------------|------------------------------------|
|LOAD|RESTORE|`RESTORE DATABASE`|
|TRANSACTION|LOG|`RESTORE LOG`|
|DBO_ONLY|RESTRICTED_USER|`RESTORE DATABASE ... WITH RESTRICTED_USER`|

### <a name="restore-log"></a>RESTORE LOG

RESTORE LOG peut contenir une liste de fichiers pour permettre la création des fichiers lors de la restauration par progression. Elle est utilisée lorsque la sauvegarde du journal contient des enregistrements qui ont été écrits lorsqu'un fichier a été ajouté à la base de données.

> [!NOTE]
> Pour une base de données en mode de récupération complète ou en mode de récupération utilisant les journaux de transactions, vous devez dans la plupart des cas effectuer une sauvegarde de la fin du journal, avant la restauration de la base de données. Restaurer une base de données sans effectuer une sauvegarde de la fin du journal au préalable entraîne une erreur, sauf si l'instruction RESTORE DATABASE contient la clause WITH REPLACE ou WITH STOPAT, qui doit spécifier une heure ou une transaction qui s'est produite après la fin de la sauvegarde des données. Pour plus d’informations sur les sauvegardes de la fin du journal, consultez l’article [Sauvegardes de la fin du journal](../../relational-databases/backup-restore/tail-log-backups-sql-server.md).

### <a name="comparison-of-recovery-and-norecovery"></a>Comparaison entre RECOVERY et NORECOVERY
La restauration est contrôlée par l'instruction RESTORE et les options [ RECOVERY | NORECOVERY ] :

- NORECOVERY indique que la restauration ne s'effectue pas. Cela permet la poursuite de la restauration par progression avec l'instruction suivante de la séquence.

  Dans ce cas, la séquence de restauration restaure d'autres sauvegardes, puis les restaure par progression.

- RECOVERY (valeur par défaut) indique que la restauration doit être effectuée après la restauration par progression de la sauvegarde actuelle.

  Pour récupérer la base de données, le jeu de données faisant l’objet de la restauration (*jeu de restauration par progression*) doit être entièrement cohérent par rapport à la base de données. Si la restauration par progression du jeu de restauration par progression n'est pas allée assez loin pour être cohérente par rapport à la base de données et si l'option RECOVERY est spécifiée, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] génère une erreur. Pour plus d’informations sur le processus de récupération, consultez [Vue d’ensemble de la restauration et de la récupération (SQL Server)](../../relational-databases/backup-restore/restore-and-recovery-overview-sql-server.md#TlogAndRecovery).

## <a name="compatibility-support"></a>Prise en charge de la compatibilité
Les sauvegardes des bases de données **master**, **model** et **msdb** créées avec une version antérieure de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne peuvent pas être restaurées par [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)].

> [!NOTE]
> Aucune sauvegarde [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne peut être restaurée dans une version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] antérieure à celle sur laquelle la sauvegarde a été créée.

Chaque version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilise un chemin d'accès par défaut différent de celui des versions antérieures. Par conséquent, pour restaurer une base de données créée à l'emplacement par défaut des sauvegardes de version antérieure, vous devez utiliser l'option MOVE. Pour plus d’informations sur le nouveau chemin par défaut, consultez [Emplacements des fichiers pour les instances par défaut et les instances nommées de SQL Server](../../sql-server/install/file-locations-for-default-and-named-instances-of-sql-server.md).

Après avoir restauré une base de données de version antérieure dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)], la base de données est automatiquement mise à niveau. En général, la base de données est immédiatement disponible. Toutefois, si une base de données [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] comprend des index de recherche en texte intégral, le processus de mise à niveau les importe, les réinitialise ou les reconstruit, selon la valeur de la propriété de serveur **upgrade_option**. Si l’option de mise à niveau a la valeur Importer (**upgrade_option** = 2) ou Reconstruire (**upgrade_option** = 0), les index de recherche en texte intégral ne seront pas disponibles pendant la mise à niveau. Selon le volume de données indexé, l'importation peut prendre plusieurs heures et la reconstruction jusqu'à dix fois plus longtemps. Notez également que lorsque l'option de mise à niveau est Importer, les index de recherche en texte intégral associés sont reconstruits si aucun catalogue de texte intégral n'est disponible. Pour modifier le paramètre de la propriété de serveur **upgrade_option** , utilisez [sp_fulltext_service](../../relational-databases/system-stored-procedures/sp-fulltext-service-transact-sql.md).

Lorsqu'une base de données est attachée ou restaurée pour la première fois à une nouvelle instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], une copie de la clé principale de la base de données (chiffrée par la clé principale du service) n'est pas encore stockée sur le serveur. Vous devez utiliser l’instruction **OPEN MASTER KEY** pour déchiffrer la clé principale de la base de données. Une fois la clé principale de la base de données déchiffrée, vous avez la possibilité d’activer le déchiffrement automatique dans le futur en exécutant l’instruction **ALTER MASTER KEY REGENERATE** pour fournir au serveur une copie de la clé principale de la base de données chiffrée avec la clé principale du service. Lorsqu'une base de données a été mise à niveau à partir d'une version antérieure, la clé DMK doit être régénérée de façon à utiliser le nouvel algorithme AES. Pour plus d’informations sur la régénération de la clé DMK, consultez l’article [ALTER MASTER KEY](../../t-sql/statements/alter-master-key-transact-sql.md). La durée nécessaire pour régénérer la clé DMK à mettre à niveau vers AES dépend du nombre d'objets protégés par la clé DMK. La régénération de la clé DMK à mettre à niveau vers AES est nécessaire une seule fois et n'a aucune incidence sur les régénérations ultérieures effectuées dans le cadre d'une stratégie de rotation de clés.

## <a name="general-remarks"></a>Remarques d'ordre général
Lors d'une restauration hors ligne, si la base de données spécifiée est en cours d'utilisation, RESTORE force la déconnexion des utilisateurs après un court délai. Pour la restauration en ligne d'un groupe de fichiers non primaire, la base de données peut rester active, sauf si le groupe de fichiers restaurés est hors ligne. Toutes les données que la base contient sont remplacées par les données restaurées.

Les opérations de restauration inter-plateformes, impliquant éventuellement des types de processeurs différents, peuvent être réalisées tant que le classement de la base de données est pris en charge par le système d'exploitation.

La commande RESTORE peut être redémarrée après une erreur. En outre, vous pouvez contraindre la commande RESTORE à poursuivre son traitement en dépit des erreurs. Elle restaure ainsi le plus de données possible (voir l'option `CONTINUE_AFTER_ERROR`).

La commande RESTORE n'est pas autorisée dans une transaction explicite ou implicite.

La restauration d’une base de données **master** endommagée s’effectue selon une procédure spéciale. Pour plus d’informations, consultez l’article [Sauvegarder et restaurer des bases de données système](../../relational-databases/backup-restore/back-up-and-restore-of-system-databases-sql-server.md).

La restauration d’une base de données efface le cache de plan pour la base de données en cours de restauration. Cette opération entraîne la recompilation de tous les plans d'exécution ultérieurs et peut entraîner une baisse temporaire et brutale des performances des requêtes. 

Pour restaurer une base de données de disponibilité, restaurez d'abord la base de données en tant qu'instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], puis ajoutez la base de données au groupe de disponibilité.

## <a name="interoperability"></a>Interopérabilité

### <a name="database-settings-and-restoring"></a>Paramètres de bases de données et restauration

Lors d'une restauration, les valeurs de la plupart des options de base de données pouvant être définies avec ALTER DATABASE et en vigueur à la fin de la sauvegarde sont rétablies.

Cependant, l'utilisation de WITH RESTRICTED_USER annule ce comportement pour le paramètre de l'option d'accès utilisateur. Ce paramètre est toujours défini suivant une instruction RESTORE qui inclut l'option WITH RESTRICTED_USER.

### <a name="restoring-an-encrypted-database"></a>Restauration d'une base de données chiffrée

Pour restaurer une base de données chiffrée, vous devez avoir accès au certificat ou à la clé asymétrique qui a servi à chiffrer la base de données. Sans le certificat et la clé asymétrique, la base de données ne peut pas être restaurée. En conséquence, le certificat utilisé pour chiffrer la clé de chiffrement de base de données doit être conservé tant que la sauvegarde est utile. Pour plus d'informations, consultez [SQL Server Certificates and Asymmetric Keys](../../relational-databases/security/sql-server-certificates-and-asymmetric-keys.md).

### <a name="restoring-a-database-enabled-for-vardecimal-storage"></a>Restauration d'une base de données activée pour le stockage vardecimal

Les sauvegardes et les restaurations fonctionnent correctement avec le format de stockage **vardecimal**. Pour plus d’informations sur le format de stockage **vardecimal**, consultez l’article [sp_db_vardecimal_storage_format](../../relational-databases/system-stored-procedures/sp-db-vardecimal-storage-format-transact-sql.md).

### <a name="restore-full-text-data"></a>Restauration de données de texte intégral

Les données de texte intégral sont restaurées avec d'autres données de base de données lors d'une restauration complète. La syntaxe régulière `RESTORE DATABASE database_name FROM backup_device` permet de restaurer les fichiers de texte intégral comme des éléments faisant partie intégrante de la restauration de fichiers de base de données.

L'instruction RESTORE permet également d'effectuer des restaurations dans d'autres emplacements, des restaurations différentielles, des restaurations de fichiers et de groupes de fichiers, ainsi que des restaurations de fichiers et groupes de fichiers différentielles de données de texte intégral. En outre, cette instruction permet de restaurer des fichiers de texte intégral uniquement, ainsi que des données de base de données.

> [!NOTE]
> Les catalogues de texte intégral importés à partir de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] sont toujours traités en tant que fichiers de base de données. Pour ceux-ci, la procédure [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] de sauvegarde des catalogues de texte intégral reste applicable, mais la suspension et la reprise en cours de sauvegarde ne sont plus nécessaires. Pour plus d’informations, consultez [Sauvegarde et restauration de catalogues de texte intégral](../../relational-databases/search/back-up-and-restore-full-text-catalogs-and-indexes.md).

### [!INCLUDE [ssbigdataclusters-ss-nover](../../includes/ssbigdataclusters-ss-nover.md)]

[!INCLUDE [big-data-clusters-master-instance-ha-endpoint-requirement](../../includes/big-data-clusters-master-instance-ha-endpoint-requirement.md)]

## <a name="metadata"></a>Métadonnées

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] intègre des tables d'historique de sauvegarde et de restauration qui assurent le suivi des activités de sauvegarde et de restauration pour chaque instance de serveur. Lorsqu'une restauration est effectuée, les tables d'historique de sauvegarde sont également modifiées. Pour plus d’informations sur ces tables, consultez l’article [Historique de sauvegarde et informations d’en-tête](../../relational-databases/backup-restore/backup-history-and-header-information-sql-server.md).

## <a name="replace-option-impact"></a><a name="REPLACEoption"></a> Impact de l’option REPLACE
L'option REPLACE ne doit être utilisée que rarement et après un examen attentif. Généralement, la restauration empêche le remplacement accidentel d'une base de données par une autre. Si la base de données nommée dans l'instruction RESTORE existe déjà sur le serveur actif et que le GUID de famille de la base de données spécifié ne correspond pas à celui qui est enregistré dans le jeu de sauvegarde, la base de données n'est pas restaurée. Cette mesure de sécurité est très importante,

L'option REPLACE annule d'importants contrôles de sécurité normalement effectués lors d'une restauration. Ces contrôles ignorés sont les suivants :

- Restauration par remplacement d'une base de données existante par une sauvegarde d'une autre base de données.

  Avec l'option REPLACE, la restauration vous permet de remplacer une base de données existante par la base de données figurant dans le jeu de sauvegarde, même si le nom de base de données spécifié diffère de celui enregistré dans le jeu en question. Cela peut entraîner un remplacement accidentel d'une base de données par une autre.
  
- Restauration par remplacement d'une base de données à l'aide du mode de restauration complète ou du mode de récupération utilisant les journaux de transactions quand une sauvegarde de la fin du journal n'a pas été réalisée et quand l'option STOPAT n'est pas employée.

  Avec l'option REPLACE, vous pouvez perdre des transactions déjà validées du fait que le dernier journal écrit n'a pas été sauvegardé.

- Remplacement des fichiers existants

  Par exemple, une erreur peut entraîner le remplacement des fichiers existants d'un type incorrect (par exemple, fichiers .xls) ou de fichiers actuellement utilisés par une autre base de données qui n'est pas en ligne. Une perte de données arbitraire peut avoir lieu si des fichiers existants sont remplacés, même si la base de données restaurée est complète.

## <a name="redoing-a-restore"></a>Rétablir une restauration
Il n'est pas possible d'annuler les effets d'une restauration. Cependant, vous pouvez annuler les effets de la copie et de la restauration par progression des données en recommençant fichier par fichier. Pour recommencer, restaurez le fichier approprié et réeffectuez la restauration par progression. Par exemple, si vous avez accidentellement restauré trop de sauvegardes du journal et dépassé le point d'arrêt voulu, redémarrez la séquence.

Une séquence de restauration peut être annulée et redémarrée en restaurant l'ensemble du contenu des fichiers concernés.

## <a name="reverting-a-database-to-a-database-snapshot"></a>Restauration d'une base de données vers un instantané de base de données
Une *opération de rétablissement de base de données* (spécifiée avec l’option DATABASE_SNAPSHOT) ramène une base de données source complète à un moment donné dans le temps, en rétablissant l’instantané de base de données à cet instant-là, c’est-à-dire en remplaçant la base de données source par les données correspondant à une date et heure précises dans l’instantané de base de données spécifié. Seul l'instantané vers lequel vous effectuez le rétablissement peut exister. Cette opération reconstruit ensuite le journal (par conséquent, vous ne pouvez pas effectuer une restauration par progression d'une base de données rétablie au point d'erreur de l'utilisateur).

La perte de données est limitée aux mises à jour de la base de données depuis la création de l'instantané. Les métadonnées d'une base de données rétablie sont identiques aux métadonnées au moment de la création de l'instantané. Toutefois, le retour à un instantané supprime tous les catalogues de texte intégral.

Le rétablissement à partir d'un instantané de base de données n'est pas destiné à la récupération des supports. Contrairement à un jeu de sauvegarde classique, l'instantané de base de données est une copie incomplète des fichiers de la base de données. Si la base de données ou l'instantané de base de données est endommagé, le rétablissement à partir d'un instantané sera probablement impossible. En outre, même lorsque le rétablissement est possible, il est peu probable qu'il permette de corriger le problème en cas de dommages.

### <a name="restrictions-on-reverting"></a>Restrictions liées au rétablissement
Le rétablissement n'est pas pris en charge dans les conditions suivantes :

- La base de données source contient des groupes de fichiers en lecture seule ou compressés.
- Des fichiers sont hors connexion alors qu'ils étaient en ligne lors de la création de l'instantané.
- Il existe plus d'un instantané de la base de données.

Pour plus d’informations, consultez [Rétablir une base de données dans l’état d’un instantané de base de données](../../relational-databases/databases/revert-a-database-to-a-database-snapshot.md).

## <a name="security"></a>Sécurité
Une opération de sauvegarde peut éventuellement spécifier des mots de passe pour un support de sauvegarde, un jeu de sauvegarde ou les deux. Lorsqu'un mot de passe a été défini sur un support de sauvegarde ou un jeu de sauvegarde, vous devez entrer le ou les mots de passe corrects dans l'instruction RESTORE. Ces mots de passe empêchent les opérations non autorisées de restauration et d'ajouts de jeux de sauvegarde au support à l'aide d'outils [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Cependant, les supports protégés par un mot de passe peuvent être remplacés par l'option FORMAT de l'instruction BACKUP.

> [!IMPORTANT]
> Le niveau de protection de ce mot de passe est faible. Son but est d'éviter que des utilisateurs autorisés ou non autorisés effectuent une restauration incorrecte à l'aide des outils [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. En aucun cas, elle n'empêche la lecture des données de la sauvegarde par d'autres moyens ou le remplacement du mot de passe. [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]La bonne pratique en matière de protection des sauvegardes consiste à stocker les bandes de sauvegarde dans un emplacement sûr ou à sauvegarder les fichiers disque protégés par une liste de contrôle d’accès (ACL). La liste de contrôle d'accès doit être définie à la racine du répertoire dans lequel les sauvegardes sont effectuées.

> [!NOTE]
> Pour obtenir des informations spécifiques sur la sauvegarde et la restauration SQL Server avec le service de stockage Microsoft Blob Azure, consultez [Sauvegarde et restauration SQL Server avec le service de stockage Microsoft Blob Azure](../../relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service.md).

### <a name="permissions"></a>Autorisations
Si la base de données restaurée n'existe pas, l'utilisateur doit posséder les autorisations `CREATE DATABASE` afin de pouvoir exécuter RESTORE. Si la base de données existe, les autorisations RESTORE reviennent par défaut aux membres des rôles serveur fixe `sysadmin` et `dbcreator` et au propriétaire (`dbo`) de la base de données (pour l’option `FROM DATABASE_SNAPSHOT`, la base de données existe toujours).

Les autorisations RESTORE sont attribuées aux rôles dont les informations d'appartenance sont toujours immédiatement accessibles à partir du serveur. Étant donné que l’appartenance au rôle de base de données fixe ne peut être contrôlée que quand la base de données est accessible et non endommagée, ce qui n’est pas toujours le cas lorsque RESTORE est exécuté, les membres du rôle de base de données fixe `db_owner` ne détiennent pas d’autorisations RESTORE.

## <a name="examples"></a><a name="examples"></a> Exemples

Tous les exemples partent du principe qu'une sauvegarde complète de la base de données a été effectuée.

Parmi les exemples d'instruction RESTORE, citons :

- R. [Restauration d’une base de données complète](#restoring_full_db)
- B. [Restauration de sauvegardes complètes et différentielles d’une base de données](#restoring_full_n_differential_db_backups)
- C. [Restauration d’une base de données en utilisant la syntaxe RESTART](#restoring_db_using_RESTART)
- D. [Restauration d’une base de données et déplacement des fichiers](#restoring_db_n_move_files)
- E. [Copie d’une base de données en utilisant BACKUP et RESTORE](#copying_db_using_bnr)
- F. [Restauration jusqu'à une date et heure en utilisant STOPAT](#restoring_to_pit_using_STOPAT)
- G. [Restauration du journal des transactions jusqu’à une marque](#restoring_transaction_log_to_mark)
- H. [Restauration en utilisant la syntaxe TAPE](#restoring_using_TAPE)
- I. [Restauration en utilisant la syntaxe FILE et FILEGROUP](#restoring_using_FILE_n_FG)
- J. [Rétablissement à partir d’un instantané de base de données](#reverting_from_db_snapshot)
- K. [Restauration à partir du service de stockage Microsoft Blob Azure](#Azure_Blob)

> [!NOTE]
> Pour obtenir d’autres exemples, consultez les rubriques de procédures de restauration répertoriées dans l’article [Vue d’ensemble de la restauration et de la récupération](../../relational-databases/backup-restore/restore-and-recovery-overview-sql-server.md).

### <a name="a-restoring-a-full-database"></a><a name="restoring_full_db"></a> A. Restauration d’une base de données complète

L'exemple suivant restaure une sauvegarde de base de données complète à partir de l'unité de sauvegarde logique `AdventureWorksBackups`. Pour obtenir un exemple de création de cette unité, consultez [Unités de sauvegarde](../../relational-databases/backup-restore/backup-devices-sql-server.md).

```sql
RESTORE DATABASE AdventureWorks2012
  FROM AdventureWorks2012Backups;
```

> [!NOTE]
> Pour une base de données en mode de récupération complète ou utilisant les journaux de transactions, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nécessite dans la plupart des cas une sauvegarde de la fin du journal, avant la restauration de la base de données. Pour plus d’informations, consultez l’article [Sauvegardes de la fin du journal](../../relational-databases/backup-restore/tail-log-backups-sql-server.md).

[&#91;Début des exemples&#93;](#examples)

### <a name="b-restoring-full-and-differential-database-backups"></a><a name="restoring_full_n_differential_db_backups"></a> B. Restauration de sauvegardes complètes et différentielles d'une base de données

L'exemple suivant restaure une sauvegarde de base de données complète, puis une sauvegarde différentielle, à partir de l'unité de sauvegarde `Z:\SQLServerBackups\AdventureWorks2012.bak` qui contient les deux sauvegardes. La sauvegarde de base de données complète à restaurer correspond au sixième jeu de sauvegarde se trouvant sur l'unité (`FILE = 6`), tandis que la sauvegarde de base de données différentielle correspond au neuvième jeu de sauvegarde se trouvant sur l'unité (`FILE = 9`). Une fois la sauvegarde différentielle récupérée, la récupération de la base de données est terminée.

```sql
RESTORE DATABASE AdventureWorks2012
    FROM DISK = 'Z:\SQLServerBackups\AdventureWorks2012.bak'
    WITH FILE = 6
      NORECOVERY;
RESTORE DATABASE AdventureWorks2012
    FROM DISK = 'Z:\SQLServerBackups\AdventureWorks2012.bak'
    WITH FILE = 9
      RECOVERY;
```

[&#91;Début des exemples&#93;](#examples)

### <a name="c-restoring-a-database-using-restart-syntax"></a><a name="restoring_db_using_RESTART"></a> C. Restauration d'une base de données en utilisant la syntaxe RESTART

Dans l'exemple suivant, l'option `RESTART` est utilisée pour redémarrer une opération `RESTORE` interrompue par une coupure de courant sur le serveur.

```sql
-- This database RESTORE halted prematurely due to power failure.
RESTORE DATABASE AdventureWorks2012
    FROM AdventureWorksBackups;
-- Here is the RESTORE RESTART operation.
RESTORE DATABASE AdventureWorks2012
    FROM AdventureWorksBackups WITH RESTART;
```

[&#91;Début des exemples&#93;](#examples)

### <a name="d-restoring-a-database-and-move-files"></a><a name="restoring_db_n_move_files"></a> D. Restauration d'une base de données et déplacement des fichiers

L'exemple suivant restaure l'intégralité d'une base de données et de son journal des transactions, puis déplace la base de données restaurée vers le répertoire `C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Data`.

```sql
RESTORE DATABASE AdventureWorks2012
    FROM AdventureWorksBackups
    WITH NORECOVERY,
      MOVE 'AdventureWorks2012_Data' TO
'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Data\NewAdvWorks.mdf',
      MOVE 'AdventureWorks2012_Log'
TO 'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Data\NewAdvWorks.ldf';
RESTORE LOG AdventureWorks2012
    FROM AdventureWorksBackups
    WITH RECOVERY;
```

[&#91;Début des exemples&#93;](#examples)

### <a name="e-copying-a-database-using-backup-and-restore"></a><a name="copying_db_using_bnr"></a> E. Copie d'une base de données en utilisant BACKUP et RESTORE

L'exemple suivant utilise à la fois les instructions `BACKUP` et `RESTORE` pour effectuer une copie de la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]. L'instruction `MOVE` entraîne la restauration des données et du fichier journal aux emplacements spécifiés. L'instruction `RESTORE FILELISTONLY` permet de déterminer le nombre et le nom des fichiers de la base de données en cours de restauration. La nouvelle copie de la base de données se nomme `TestDB`. Pour plus d’informations, consultez l’article [RESTORE FILELISTONLY](../../t-sql/statements/restore-statements-filelistonly-transact-sql.md).

```sql
BACKUP DATABASE AdventureWorks2012
    TO AdventureWorksBackups ;

RESTORE FILELISTONLY
    FROM AdventureWorksBackups ;

RESTORE DATABASE TestDB
    FROM AdventureWorksBackups
    WITH MOVE 'AdventureWorks2012_Data' TO 'C:\MySQLServer\testdb.mdf',
    MOVE 'AdventureWorks2012_Log' TO 'C:\MySQLServer\testdb.ldf';
GO
```

[&#91;Début des exemples&#93;](#examples)

### <a name="f-restoring-to-a-point-in-time-using-stopat"></a><a name="restoring_to_pit_using_STOPAT"></a> F. Restauration jusqu'à une date et heure en utilisant STOPAT

L'exemple suivant restaure une base de données dans l'état où elle se trouvait le `12:00 AM` à `April 15, 2020` et décrit une opération de restauration impliquant plusieurs sauvegardes de fichiers journaux. Sur l'unité de sauvegarde `AdventureWorksBackups`, la sauvegarde de base de données complète à restaurer correspond au troisième jeu de sauvegarde (`FILE = 3`), la première sauvegarde de fichier journal correspond au quatrième jeu de sauvegarde (`FILE = 4`) et la seconde sauvegarde de fichier journal correspond au cinquième jeu de sauvegarde (`FILE = 5`).

```sql
RESTORE DATABASE AdventureWorks2012
    FROM AdventureWorksBackups
    WITH FILE=3, NORECOVERY;
  
RESTORE LOG AdventureWorks2012
    FROM AdventureWorksBackups
    WITH FILE=4, NORECOVERY, STOPAT = 'Apr 15, 2020 12:00 AM';
  
RESTORE LOG AdventureWorks2012
    FROM AdventureWorksBackups
    WITH FILE=5, NORECOVERY, STOPAT = 'Apr 15, 2020 12:00 AM';
RESTORE DATABASE AdventureWorks2012 WITH RECOVERY;

```

[&#91;Début des exemples&#93;](#examples)

### <a name="g-restoring-the-transaction-log-to-a-mark"></a><a name="restoring_transaction_log_to_mark"></a> G. Restauration du journal des transactions jusqu'à une marque

L'exemple suivant restaure le journal des transactions jusqu'à la marque dans la transaction marquée nommée `ListPriceUpdate`.

```sql
USE AdventureWorks2012
GO
BEGIN TRANSACTION ListPriceUpdate
    WITH MARK 'UPDATE Product list prices';
GO

UPDATE Production.Product
    SET ListPrice = ListPrice * 1.10
    WHERE ProductNumber LIKE 'BK-%';
GO

COMMIT TRANSACTION ListPriceUpdate;
GO

-- Time passes. Regular database
-- and log backups are taken.
-- An error occurs in the database.
USE master;
GO

RESTORE DATABASE AdventureWorks2012
FROM AdventureWorksBackups
WITH FILE = 3, NORECOVERY;
GO

RESTORE LOG AdventureWorks2012
  FROM AdventureWorksBackups
    WITH FILE = 4,
    RECOVERY,
    STOPATMARK = ListPriceUpdate;
```

[&#91;Début des exemples&#93;](#examples)

### <a name="h-restoring-using-tape-syntax"></a><a name="restoring_using_TAPE"></a> H. Restauration en utilisant la syntaxe TAPE

L'exemple suivant restaure une sauvegarde de base de données complète à partir d'une unité de sauvegarde `TAPE`.

```sql
RESTORE DATABASE AdventureWorks2012
    FROM TAPE = '\\.\tape0';
```

[&#91;Début des exemples&#93;](#examples)

### <a name="i-restoring-using-file-and-filegroup-syntax"></a><a name="restoring_using_FILE_n_FG"></a> I. Restauration en utilisant la syntaxe FILE et FILEGROUP

L'exemple suivant restaure une base de données nommée `MyDatabase` qui est composée de deux fichiers, d'un groupe de fichiers secondaire et d'un journal des transactions. La base de données utilise le mode de récupération complète.

La sauvegarde de la base de données est le neuvième jeu de sauvegarde dans le support de sauvegarde sur une unité de sauvegarde logique nommée `MyDatabaseBackups`. Trois sauvegardes de fichier journal, qui se trouvent dans les trois jeux de sauvegarde suivants (`10`, `11` et `12`) sur l'unité `MyDatabaseBackups` sont ensuite restaurés à l'aide de `WITH NORECOVERY`. Une fois restaurée la dernière sauvegarde de fichier journal, la base de données est récupérée.

> [!NOTE]
> La récupération est réalisée séparément afin d'éviter qu'elle n'ait lieu trop tôt, c'est-à-dire avant que la totalité des sauvegardes de fichier journal n'ait été restaurée. Pour plus d’informations sur le processus de récupération, consultez [Vue d’ensemble de la restauration et de la récupération (SQL Server)](../../relational-databases/backup-restore/restore-and-recovery-overview-sql-server.md#TlogAndRecovery).

Dans l'instruction `RESTORE DATABASE`, il existe deux types d'options `FILE`. Les options `FILE` qui précèdent le nom de l'unité de sauvegarde spécifient les noms de fichiers logiques des fichiers de base de données à restaurer à partir du jeu de sauvegarde, par exemple `FILE = 'MyDatabase_data_1'`. Ce jeu de sauvegarde n'est pas la première sauvegarde de base de données dans le support de sauvegarde. Par conséquent, sa position dans le support de sauvegarde est indiquée via l'option `FILE` dans la clause `WITH`, en l'occurrence `FILE=9`.

```sql
RESTORE DATABASE MyDatabase
    FILE = 'MyDatabase_data_1',
    FILE = 'MyDatabase_data_2',
    FILEGROUP = 'new_customers'
    FROM MyDatabaseBackups
    WITH
      FILE = 9,
      NORECOVERY;
GO  
-- Restore the log backups
RESTORE LOG MyDatabase
    FROM MyDatabaseBackups
    WITH FILE = 10,
      NORECOVERY;
GO
RESTORE LOG MyDatabase
    FROM MyDatabaseBackups
    WITH FILE = 11,
      NORECOVERY;
GO
RESTORE LOG MyDatabase
    FROM MyDatabaseBackups
    WITH FILE = 12,
      NORECOVERY;
GO
--Recover the database
RESTORE DATABASE MyDatabase WITH RECOVERY;
GO
```

[&#91;Début des exemples&#93;](#examples)

### <a name="j-reverting-from-a-database-snapshot"></a><a name="reverting_from_db_snapshot"></a> J. Rétablissement à partir d'un instantané de base de données

L'exemple suivant rétablit un instantané de base de données. Il part du principe qu'un seul instantané existe actuellement dans la base de données. Pour obtenir un exemple de la façon de créer cet instantané de base de données, consultez l’article [Créer un instantané de base de données](../../relational-databases/databases/create-a-database-snapshot-transact-sql.md).

> [!NOTE]
> Le rétablissement d'un instantané supprime tous les catalogues de texte intégral.

```sql
USE master;
RESTORE DATABASE AdventureWorks2012 FROM DATABASE_SNAPSHOT = 'AdventureWorks_dbss1800';
GO
```

Pour plus d’informations, consultez [Rétablir une base de données dans l’état d’un instantané de base de données](../../relational-databases/databases/revert-a-database-to-a-database-snapshot.md).

[&#91;Début des exemples&#93;](#examples)

### <a name="k-restoring-from-the-microsoft-azure-blob-storage-service"></a><a name="Azure_Blob"></a> K. Restauration à partir du service de stockage Microsoft Blob Azure

Les trois exemples ci-dessous impliquent l’utilisation du service de stockage Microsoft Azure. Le nom du compte de stockage est `mystorageaccount`. Le conteneur des fichiers de données est appelé `myfirstcontainer`. Le conteneur des fichiers de sauvegarde est appelé `mysecondcontainer`. Une stratégie d’accès stockée a été créée avec des droits de lecture, écriture, suppression et liste pour chaque conteneur. Des informations d’identification SQL Server ont été créées en utilisant des signatures d’accès partagé associées aux stratégies d’accès stockées. Pour obtenir des informations spécifiques sur la sauvegarde et la restauration SQL Server avec le service de stockage Microsoft Blob Azure, consultez [Sauvegarde et restauration SQL Server avec le service de stockage Microsoft Blob Azure](../../relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service.md).

**K1. Restaurer une sauvegarde complète de base de données à partir du service de stockage Microsoft Azure**    
Une sauvegarde complète de base de données située dans le conteneur `mysecondcontainer` de `Sales` sera restaurée dans `myfirstcontainer`. `Sales` n’existe actuellement pas sur le serveur.

```sql
RESTORE DATABASE Sales
  FROM URL = 'https://mystorageaccount.blob.core.windows.net/mysecondcontainer/Sales.bak'
  WITH MOVE 'Sales_Data1' to 'https://mystorageaccount.blob.core.windows.net/myfirstcontainer/Sales_Data1.mdf',
  MOVE 'Sales_log' to 'https://mystorageaccount.blob.core.windows.net/myfirstcontainer/Sales_log.ldf',
  STATS = 10;
```

**K2. Restaurer une sauvegarde complète de base de données à partir du service Stockage Microsoft Azure vers un stockage local** Une sauvegarde complète de base de données, située dans le conteneur `mysecondcontainer`, de `Sales` sera restaurée dans le stockage local. `Sales` n’existe actuellement pas sur le serveur.

```sql
RESTORE DATABASE Sales
  FROM URL = 'https://mystorageaccount.blob.core.windows.net/mysecondcontainer/Sales.bak'
  WITH MOVE 'Sales_Data1' to 'H:\DATA\Sales_Data1.mdf',
  MOVE 'Sales_log' to 'O:\LOG\Sales_log.ldf',
  STATS = 10;
```

**K3. Restaurer une sauvegarde complète de base de données du stockage local vers le service de stockage Microsoft Azure**

```sql
RESTORE DATABASE Sales
  FROM DISK = 'E:\BAK\Sales.bak'
  WITH MOVE 'Sales_Data1' to 'https://mystorageaccount.blob.core.windows.net/myfirstcontainer/Sales_Data1.mdf',
  MOVE 'Sales_log' to 'https://mystorageaccount.blob.core.windows.net/myfirstcontainer/Sales_log.ldf',
  STATS = 10;
```

[&#91;Début des exemples&#93;](#examples)

## <a name="more-information"></a>Informations complémentaires

[Vue d'ensemble de la restauration et de la récupération (SQL Server)](../../relational-databases/backup-restore/restore-and-recovery-overview-sql-server.md#TlogAndRecovery)     
[Sauvegarde et restauration des bases de données SQL Server](../../relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases.md)    
[Sauvegarder et restaurer des bases de données système (SQL Server)](../../relational-databases/backup-restore/back-up-and-restore-of-system-databases-sql-server.md)      
[Restaurer une sauvegarde de base de données à l’aide de SSMS](../../relational-databases/backup-restore/restore-a-database-backup-using-ssms.md)     
[Sauvegarder et restaurer des catalogues et des index de recherche en texte intégral](../../relational-databases/search/back-up-and-restore-full-text-catalogs-and-indexes.md)      
[Sauvegarder et restaurer des bases de données répliquées](../../relational-databases/replication/administration/back-up-and-restore-replicated-databases.md)      
[BACKUP](../../t-sql/statements/restore-statements-transact-sql.md)      
[Jeux de supports, familles de supports et jeux de sauvegarde](../../relational-databases/backup-restore/media-sets-media-families-and-backup-sets-sql-server.md)      
[RESTORE REWINDONLY](../../t-sql/statements/restore-statements-rewindonly-transact-sql.md)     
[RESTORE VERIFYONLY](../../t-sql/statements/restore-statements-verifyonly-transact-sql.md)     
[RESTORE FILELISTONLY (Transact-SQL)](../../t-sql/statements/restore-statements-filelistonly-transact-sql.md)     
[RESTORE HEADERONLY (Transact-SQL)](../../t-sql/statements/restore-statements-headeronly-transact-sql.md)     
[Historique de sauvegarde et informations d’en-tête](../../relational-databases/backup-restore/backup-history-and-header-information-sql-server.md)       

::: moniker-end
::: moniker range="=azuresqldb-mi-current"

:::row:::
    :::column:::
        [SQL Server](restore-statements-transact-sql.md?view=sql-server-2017&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* SQL Managed Instance \*_**
    :::column-end:::
    :::column:::
        [Analytics Platform<br />System (PDW)](restore-statements-transact-sql.md?view=aps-pdw-2016&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="azure-sql-managed-instance"></a>Azure SQL Managed Instance

Cette commande vous permet de restaurer une base de données complète à partir d’une sauvegarde de base de données complète (restauration complète) depuis un compte Stockage Blob Azure.

Pour afficher d’autres commandes RESTORE prises en charge, consultez :

- [RESTORE FILELISTONLY (Transact-SQL)](../../t-sql/statements/restore-statements-filelistonly-transact-sql.md)
- [RESTORE HEADERONLY (Transact-SQL)](../../t-sql/statements/restore-statements-headeronly-transact-sql.md)
- [RESTORE LABELONLY ONLY (Transact-SQL)](../../t-sql/statements/restore-statements-labelonly-transact-sql.md)
- [RESTORE VERIFYONLY (Transact-SQL)](../../t-sql/statements/restore-statements-verifyonly-transact-sql.md)

> [!IMPORTANT]
> Pour effectuer une restauration à partir de sauvegardes automatiques SQL Managed Instance, consultez [Restauration de base de données SQL](/azure/sql-database/sql-database-recovery-using-backups).

## <a name="syntax"></a>Syntaxe

```syntaxsql
--To Restore an Entire Database from a Full database backup (a Complete Restore):
RESTORE DATABASE { database_name | @database_name_var }
 FROM URL = { 'physical_device_name' | @physical_device_name_var } [ ,...n ]
[;]

```

## <a name="arguments"></a>Arguments

DATABASE

Spécifie la base de données cible.

FROM URL

Spécifie une ou plusieurs unités de sauvegarde placées sur les URL qui seront utilisées pour l’opération de restauration. Le format d’URL est utilisé pour la restauration des sauvegardes à partir du service de stockage Microsoft Azure.

> [!IMPORTANT]
> Pour restaurer à partir de plusieurs unités quand l’opération s’effectue depuis une URL, vous devez utiliser des jetons de signature d’accès partagé (SAP). Pour obtenir des exemples de signatures d’accès partagé, consultez [Sauvegarde SQL Server vers une URL](../../relational-databases/backup-restore/sql-server-backup-to-url.md) et [Simplifying creation of SQL Credentials with Shared Access Signature (SAS) tokens on Azure Storage with PowerShell](/archive/blogs/sqlcat/simplifying-creation-of-sql-credentials-with-shared-access-signature-sas-tokens-on-azure-storage-with-powershell).

*n* Correspond à un espace réservé indiquant qu’il est possible de spécifier jusqu’à 64 unités de sauvegarde dans une liste séparée par des virgules.

## <a name="general-remarks"></a>Remarques d'ordre général

En tant que prérequis, vous devez créer une information d’identification avec un nom qui correspond à l’URL du compte de Stockage Blob et une Signature d’accès partagé placée en tant que secret. La commande RESTORE recherche les informations d’identification à l’aide de l’URL du compte de Stockage Blob afin de trouver les informations nécessaires pour lire l’appareil de sauvegarde.

L’opération RESTORE est asynchrone, la restauration continue même si la connexion cliente est rompue. Si votre connexion est supprimée, vous pouvez afficher la vue [sys.dm_operation_status](../../relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database.md) pour vérifier l’état d’une opération de restauration (et d’une opération CREATE ou DROP sur une base de données).

Les options de base de données suivantes sont définies/remplacées et ne peuvent pas être modifiées ultérieurement :

- NEW_BROKER (si le service Broker n’est pas activé dans le fichier .bak)
- ENABLE_BROKER (si le service Broker n’est pas activé dans le fichier .bak)
- AUTO_CLOSE=OFF (si une base de données dans le fichier .bak est définie avec AUTO_CLOSE=ON)
- RECOVERY FULL (si une base de données dans le fichier .bak est définie avec le mode de récupération SIMPLE ou BULK_LOGGED)
- Un groupe de fichiers à mémoire optimisée, appelé XTP, est ajouté au fichier source .bak s’il n’y est pas déjà. Chaque groupe de fichiers à mémoire optimisée existant est renommé XTP
- Les options SINGLE_USER et RESTRICTED_USER sont remplacées par l’option MULTI_USER

## <a name="limitations---sql-managed-instance"></a>Limitations - SQL Managed Instance

Les limitations suivantes s’appliquent :

- Les fichiers .bak contenant plusieurs jeux de sauvegarde ne peuvent pas être restaurés.
- Les fichiers .bak contenant plusieurs fichiers journaux ne peuvent pas être restaurés.
- La restauration échoue si le fichier .bak contient des données FILESTREAM.
- Les sauvegardes contenant des bases de données avec des objets en mémoire actifs ne peuvent pas être restaurées au niveau de performance Usage général.
- Les sauvegardes contenant des bases de données en mode lecture seule ne peuvent pas être restaurées. Cette limitation sera supprimée prochainement.

Pour plus d’informations, consultez [Azure SQL Managed Instance](/azure/sql-database/sql-database-managed-instance)

## <a name="restoring-an-encrypted-database"></a>Restauration d'une base de données chiffrée
Pour restaurer une base de données chiffrée, vous devez avoir accès au certificat ou à la clé asymétrique qui a servi à chiffrer la base de données. Sans le certificat et la clé asymétrique, la base de données ne peut pas être restaurée. En conséquence, le certificat utilisé pour chiffrer la clé de chiffrement de base de données doit être conservé tant que la sauvegarde est utile. Pour plus d'informations, consultez [SQL Server Certificates and Asymmetric Keys](../../relational-databases/security/sql-server-certificates-and-asymmetric-keys.md).

## <a name="permissions"></a>Autorisations
L’utilisateur doit posséder les autorisations `CREATE DATABASE` afin de pouvoir exécuter RESTORE.

```sql
CREATE LOGIN mylogin WITH PASSWORD = 'Very Strong Pwd123!';
GRANT CREATE ANY DATABASE TO [mylogin];
```

Les autorisations RESTORE sont attribuées aux rôles dont les informations d'appartenance sont toujours immédiatement accessibles à partir du serveur. Étant donné que l’appartenance au rôle de base de données fixe ne peut être contrôlée que quand la base de données est accessible et non endommagée, ce qui n’est pas toujours le cas lorsque RESTORE est exécuté, les membres du rôle de base de données fixe `db_owner` ne détiennent pas d’autorisations RESTORE.

## <a name="examples"></a><a name="examples"></a> Exemples

Les exemples suivants restaurent une sauvegarde de base de données en copie seule à partir de l’URL, avec la création d’informations d’identification.

### <a name="a-restore-database-from-four-backup-devices"></a><a name="restore-mi-database"></a> A. Restaurer la base de données à partir de quatre unités de sauvegarde

```sql

-- Create credential
CREATE CREDENTIAL [https://mybackups.blob.core.windows.net/wide-world-importers]
WITH IDENTITY = 'SHARED ACCESS SIGNATURE',
      SECRET = 'sv=2017-11-09&ss=bq&srt=sco&sp=rl&se=2022-06-19T22:41:07Z&st=2018-06-01T14:41:07Z&spr=https&sig=s7wddcf0w%3D';
GO
-- Restore database
RESTORE DATABASE WideWorldImportersStandard
FROM URL = N'https://mybackups.blob.core.windows.net/wide-world-importers/00-WideWorldImporters-Standard.bak',
URL = N'https://mybackups.blob.core.windows.net/wide-world-importers/01-WideWorldImporters-Standard.bak',
URL = N'https://mybackups.blob.core.windows.net/wide-world-importers/02-WideWorldImporters-Standard.bak',
URL = N'https://mybackups.blob.core.windows.net/wide-world-importers/03-WideWorldImporters-Standard.bak'
```

L’erreur suivante s’affiche si la base de données existe déjà :

```
Msg 1801, Level 16, State 1, Line 9
Database 'WideWorldImportersStandard' already exists. Choose a different database name.
```

### <a name="b-restore-database-specified-via-variable"></a><a name="restore-mi-database-variables"></a> B. Restaurer la base de données spécifiée par le biais de la variable

```sql
DECLARE @db_name sysname = 'WideWorldImportersStandard';
DECLARE @url nvarchar(400) = N'https://mybackups.blob.core.windows.net/wide-world-importers/WideWorldImporters-Standard.bak';

RESTORE DATABASE @db_name
FROM URL = @url
```

### <a name="c-track-progress-of-restore-statement"></a><a name="restore-mi-database-progress"></a> C. Suivre la progression de l’instruction restore

```sql
SELECT query = a.text, start_time, percent_complete,
    eta = dateadd(second,estimated_completion_time/1000, getdate())
FROM sys.dm_exec_requests r
    CROSS APPLY sys.dm_exec_sql_text(r.sql_handle) a
WHERE r.command = 'RESTORE DATABASE'
```

> [!Note]
> Cette vue affichera probablement deux requêtes restore. L’une est l’instruction RESTORE d’origine envoyée par le client, et l’autre est une instruction RESTORE en arrière-plan qui s’exécute même si la connexion du client échoue.

::: moniker-end
::: moniker range=">=aps-pdw-2016"

:::row:::
    :::column:::
        [SQL Server](restore-statements-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        [SQL Managed Instance](restore-statements-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* Analytics<br />Platform System (PDW) \*_**
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="analytics-platform-system"></a>Système de la plateforme d'analyse

Restaure une base de données utilisateur [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] à partir d’une sauvegarde de base de données dans une appliance [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]. La base de données est restaurée à partir d’une sauvegarde créée précédemment par la commande [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] [BACKUP DATABASE - Analytics Platform System](../../t-sql/statements/backup-transact-sql.md). Les opérations de sauvegarde et de restauration vous permettent d’établir un plan de récupération d’urgence ou de déplacer des bases de données d’une appliance vers une autre.

> [!NOTE]
> La restauration de la base de données MASTER inclut la restauration des informations de connexion d’appliance. Pour restaurer la base de données MASTER, accédez à la page [Restaurer la base de données MASTER](../../relational-databases/backup-restore/restore-the-master-database-transact-sql.md) de l’outil **Configuration Manager**. Un administrateur ayant accès au nœud de contrôle peut effectuer cette opération. Pour plus d’informations sur les sauvegardes de bases de données [!INCLUDE[ssPDW](../../includes/sspdw-md.md)], consultez la section relative à la sauvegarde et à la restauration dans la [!INCLUDE[pdw-product-documentation](../../includes/pdw-product-documentation-md.md)].

## <a name="syntax"></a>Syntaxe

```syntaxsql

-- Restore the master database
-- Use the Configuration Manager tool.

Restore a full user database backup.
RESTORE DATABASE database_name
    FROM DISK = '\\UNC_path\full_backup_directory'
[;]

--Restore a full user database backup and then a differential backup.
RESTORE DATABASE database_name
    FROM DISK = '\\UNC_path\differential_backup_directory'
    WITH [ ( ] BASE = '\\UNC_path\full_backup_directory' [ ) ]
[;]

--Restore header information for a full or differential user database backup.
RESTORE HEADERONLY
    FROM DISK = '\\UNC_path\backup_directory'
[;]
```

## <a name="arguments"></a>Arguments

RESTORE DATABASE *database_name* Spécifie la restauration d’une base de données utilisateur dans une base de données appelée *database_name*. La base de données restaurée peut avoir un nom différent du nom de la base de données source qui a été sauvegardée. *database_name* ne peut pas déjà exister comme base de données dans l’appliance de destination. Pour plus d’informations sur les noms de base de données autorisés, consultez les règles de nommage d’objets dans la [!INCLUDE[pdw-product-documentation](../../includes/pdw-product-documentation-md.md)].

La restauration d’une base de données utilisateur a pour effet de restaurer une sauvegarde complète de base de données et éventuellement de restaurer une sauvegarde différentielle dans l’appliance. La restauration d’une base de données utilisateur inclut la restauration des utilisateurs de la base de données, ainsi que les rôles de base de données.

FROM DISK = '\\\\*UNC_path*\\*backup_directory*' Spécifie le chemin réseau et le répertoire dans lesquels [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] restaurera les fichiers de sauvegarde. Par exemple, FROM DISK = « \\\xxx.xxx.xxx.xxx\backups\2012\Monthly\08.2012.Mybackup» .

*backup_directory* Spécifie le nom d’un répertoire qui contient la sauvegarde complète ou différentielle. Par exemple, vous pouvez effectuer une opération RESTORE HEADERONLY sur une sauvegarde complète ou différentielle.

*full_backup_directory* Spécifie le nom d’un répertoire qui contient la sauvegarde complète.

*differential_backup_directory* Spécifie le nom du répertoire qui contient la sauvegarde différentielle.

- Le chemin et le répertoire de sauvegarde doivent déjà exister et être spécifiés comme chemin UNC (Universal Naming Convention).
- Le chemin du répertoire de sauvegarde ne peut pas être un chemin local ni un emplacement sur un nœud d’appliance [!INCLUDE[ssPDW](../../includes/sspdw-md.md)].
- La longueur maximale du chemin UNC et du nom du répertoire de sauvegarde est de 200 caractères.
- Le serveur ou l’hôte doivent être spécifiés comme une adresse IP.

RESTORE HEADERONLY Spécifie le renvoi des seules informations d’en-tête d’une sauvegarde de base de données utilisateur unique. L’en-tête comprend différents champs, notamment le texte descriptif de la sauvegarde et le nom de la sauvegarde. Le nom de la sauvegarde ne doit pas nécessairement être le même que celui du répertoire où sont stockés les fichiers de sauvegarde.

Les résultats de RESTORE HEADERONLY sont modélisés d’après les résultats de RESTORE HEADERONLY de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Les résultats comportent plus de 50 colonnes, qui ne sont pas toutes utilisées par [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]. Pour une description des colonnes présentes dans les résultats de RESTORE HEADERONLY de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consultez l’article [RESTORE HEADERONLY](../../t-sql/statements/restore-statements-headeronly-transact-sql.md).

## <a name="permissions"></a>Autorisations
Nécessite l’autorisation `CREATE ANY DATABASE`.

Nécessite un compte Windows doté d’un droit d’accès et de lecture à partir du répertoire de sauvegarde. Vous devez aussi stocker le nom de compte et le mot de passe Windows dans [!INCLUDE[ssPDW](../../includes/sspdw-md.md)].

- Pour vérifier si les informations d’identification s’y trouvent déjà, utilisez [sys.dm_pdw_network_credentials](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-network-credentials-transact-sql.md).
- Pour ajouter ou mettre à jour les informations d’identification, utilisez [sp_pdw_add_network_credentials[!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]](../../relational-databases/system-stored-procedures/sp-pdw-add-network-credentials-sql-data-warehouse.md).
- Pour supprimer les informations d’identification de [!INCLUDE[ssPDW](../../includes/sspdw-md.md)], utilisez [sp_pdw_remove_network_credentials - SQL [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]](../../relational-databases/system-stored-procedures/sp-pdw-remove-network-credentials-sql-data-warehouse.md).

## <a name="error-handling"></a>Gestion des erreurs

La commande RESTORE DATABASE génère des erreurs dans les cas suivants :

- Le nom de la base de données à restaurer existe déjà dans l’appliance cible. Pour éviter cela, choisissez un nom de base de données unique ou supprimez la base de données existante avant d’exécuter la restauration.
- Le répertoire de sauvegarde contient un ensemble de fichiers de sauvegarde non valide.
- Les autorisations de connexion ne sont pas suffisantes pour restaurer une base de données.
- [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] ne dispose pas des autorisations appropriées pour accéder à l’emplacement réseau où se trouvent les fichiers de sauvegarde.
- L’emplacement réseau du répertoire de sauvegarde n’existe pas ou n’est pas disponible.
- Les nœuds de calcul ou le nœud de contrôle n’ont pas suffisamment d’espace disque. [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] ne confirme pas que l’appliance dispose d’une quantité d’espace disque suffisante avant le lancement de la restauration. De ce fait, il est possible qu’une erreur d’espace disque insuffisant soit générée pendant l’exécution de l’instruction RESTORE DATABASE. Quand l’espace disque est insuffisant, [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] annule la restauration.
- L’appliance cible dans laquelle la base de données est restaurée compte moins de nœuds de calcul que l’appliance source à partir de laquelle la base de données a été sauvegardée.
- La tentative de restauration de la base de données a pour cadre une transaction.

## <a name="general-remarks"></a>Remarques d'ordre général

[!INCLUDE[ssPDW](../../includes/sspdw-md.md)] suit la bonne réalisation des restaurations de base de données. Avant de restaurer une sauvegarde différentielle de base de données, [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] vérifie que la restauration complète de base de données a abouti correctement.

Après une restauration, la base de données utilisateur présente le niveau de compatibilité de base de données 120. Cela vaut pour toutes les bases de données, quel que soit leur niveau de compatibilité d’origine.

## <a name="restoring-to-an-appliance-with-a-larger-number-of-compute-nodes"></a>Restauration dans une appliance dotée d’un plus grand nombre de nœuds de calcul

Exécutez [DBCC SHRINKLOG ([!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)])](../../t-sql/database-console-commands/dbcc-shrinklog-azure-sql-data-warehouse.md) après la restauration d’une base de données à partir d’une appliance plus petite ou plus grande, car la redistribution augmentera le journal des transactions.

Le fait de restaurer une sauvegarde dans une appliance dotée d’un plus grand nombre de nœuds de calcul a pour effet d’accroître la taille de la base de données allouée de façon proportionnelle au nombre de nœuds de calcul.

Par exemple, si vous restaurez une base de données de 60 Go d’une appliance à 2 nœuds (30 Go par nœud) vers une appliance à 6 nœuds, [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] crée une base de données de 180 Go (6 nœuds à raison de 30 Go par nœud) dans l’appliance à 6 nœuds. [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] commence par restaurer la base de données sur 2 nœuds pour être en adéquation avec la configuration source, puis redistribue les données aux 6 nœuds.

À l’issue de la redistribution, chaque nœud de calcul contient moins de données réelles et plus d’espace libre que chaque nœud de calcul de l’appliance source de plus petite taille. Profitez de l’espace supplémentaire pour ajouter davantage de données à la base de données. Si la base de données restaurée est trop volumineuse, vous pouvez utiliser [ALTER DATABASE - PDW](../../t-sql/statements/alter-database-transact-sql.md?view=aps-pdw-2016-au7&preserve-view=true) pour réduire la taille des fichiers de base de données.

## <a name="limitations-and-restrictions"></a>Limitations et restrictions

Dans le cadre de ces limitations et restrictions, l’appliance source est celle à partir de laquelle la sauvegarde de base de données a été créée, tandis que l’appliance cible est celle sur laquelle la base de données est restaurée.

- À l’occasion d’une restauration de base de données, les statistiques ne sont pas régénérées automatiquement.
- Seule une instruction RESTORE DATABASE ou BACKUP DATABASE peut s’exécuter dans l’appliance à un moment donné. Si plusieurs instructions de sauvegarde et de restauration sont envoyées simultanément, l’appliance les met en file d’attente et les traite l’une après l’autre.
- Vous ne pouvez restaurer une sauvegarde de base de données que dans une appliance cible [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] qui compte au moins le même nombre de nœuds de calcul que l’appliance source. L’appliance cible ne peut pas contenir moins de nœuds de calcul que l’appliance source.
- Une sauvegarde qui a été créée dans une appliance basée sur du matériel SQL Server 2012 PDW ne pas être restaurée dans une appliance basée sur du matériel SQL Server 2008 R2. Cela est vrai même si l’appliance a été acquise à l’origine avec du matériel SQL Server 2008 R2 PDW et exécute maintenant un logiciel SQL Server 2012 PDW.

## <a name="locking"></a>Verrouillage
Applique un verrou exclusif sur l’objet DATABASE.

## <a name="examples"></a>Exemples

### <a name="a-simple-restore-examples"></a>R. Exemples simples RESTORE

L’exemple suivant restaure une sauvegarde complète dans la base de données `SalesInvoices2013`. Les fichiers de sauvegarde sont stockés dans le répertoire `\\\xxx.xxx.xxx.xxx\backups\yearly\Invoices2013Full`. La base de données SalesInvoices2013 ne doit pas déjà se trouver dans l’appliance cible, car cette commande échouerait avec une erreur.

```sql
RESTORE DATABASE SalesInvoices2013
FROM DISK = '\\xxx.xxx.xxx.xxx\backups\yearly\Invoices2013Full';
```

### <a name="b-restore-a-full-and-differential-backup"></a>B. Restaurer une sauvegarde complète et différentielle

L’exemple suivant restaure une sauvegarde complète, puis une sauvegarde différentielle dans la base de données SalesInvoices2013.

La sauvegarde complète de la base de données est restaurée à partir de la sauvegarde complète, qui est stockée dans le répertoire `\\\xxx.xxx.xxx.xxx\backups\yearly\Invoices2013Full`. Si la restauration aboutit, la sauvegarde différentielle est restaurée dans la base de données SalesInvoices2013. La sauvegarde différentielle est stockée dans le répertoire `\\\xxx.xxx.xxx.xxx\backups\yearly\Invoices2013Diff`.

```syntaxsql
RESTORE DATABASE SalesInvoices2013
    FROM DISK = '\\xxx.xxx.xxx.xxx\backups\yearly\Invoices2013Diff'
    WITH BASE = '\\xxx.xxx.xxx.xxx\backups\yearly\Invoices2013Full'
[;]

```

### <a name="c-restoring-the-backup-header"></a>C. Restauration de l’en-tête de sauvegarde

Cet exemple restaure les informations d’en-tête pour la sauvegarde de base de données `\\\xxx.xxx.xxx.xxx\backups\yearly\Invoices2013Full`. La commande génère une ligne d’informations pour la sauvegarde Invoices2013Full.

```syntaxsql
RESTORE HEADERONLY
    FROM DISK = '\\xxx.xxx.xxx.xxx\backups\yearly\Invoices2013Full'
[;]
```

Vous pouvez vous servir des informations d’en-tête pour vérifier le contenu d’une sauvegarde ou pour vous assurer que l’appliance de restauration cible est compatible avec l’appliance de sauvegarde source avant d’entreprendre la restauration de la sauvegarde.

## <a name="see-also"></a>Voir aussi
[BACKUP DATABASE - Analytics Platform System](../../t-sql/statements/backup-transact-sql.md?view=aps-pdw-2016-au7&preserve-view=true)     

::: moniker-end