---
description: BACKUP (Transact-SQL)
title: BACKUP (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/22/2020
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- BACKUP_TSQL
- BACKUP
- BACKUP_DATABASE_TSQL
- BACKUP_LOG_TSQL
- BACKUP LOG
- BACKUP DATABASE
dev_langs:
- TSQL
helpviewer_keywords:
- backup media [SQL Server], BACKUP statement
- backing up filegroups [SQL Server]
- backup file formats [SQL Server]
- backups [SQL Server]
- database backups [SQL Server], BACKUP statement
- multifamily media sets [SQL Server]
- mirrored media sets [SQL Server]
- backing up files [SQL Server]
- backup sets [SQL Server], BACKUP statement
- backup compression [SQL Server], BACKUP statement
- BACKUP statement
- backups [SQL Server], BACKUP statement
- backup history tables [SQL Server]
- transaction log backups [SQL Server], BACKUP LOG statement
- READ_WRITE_FILEGROUPS option
- BACKUP DATABASE statement
- concurrency [SQL Server], backups
- backing up databases [SQL Server]
- passwords [SQL Server], backups
- security [SQL Server], backups
- media families [SQL Server]
- BACKUP LOG statement
- backing up transaction logs [SQL Server]
- stripe sets [SQL Server]
- cross-platform backups
ms.assetid: 89a4658a-62f1-4289-8982-f072229720a1
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current||>=aps-pdw-2016'
ms.openlocfilehash: 6025ca2bec08327f668a437b5428519cf85790dc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100075770"
---
# <a name="backup-transact-sql"></a>BACKUP (Transact-SQL)

Sauvegarde une base de données SQL.

Cliquez sur l’un des onglets suivants pour connaître la syntaxe, les arguments, les remarques, les autorisations et des exemples propres à la version de SQL que vous utilisez.

Pour plus d’informations sur les conventions de la syntaxe, consultez [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).

[!INCLUDE[select-product](../../includes/select-product.md)]

::: moniker range=">=sql-server-2016||>=sql-server-linux-2017"

:::row:::
    :::column:::
        **_\* SQL Server \*_** &nbsp;
    :::column-end:::
    :::column:::
        [SQL Managed Instance](backup-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Analytics Platform<br />System (PDW)](backup-transact-sql.md?view=aps-pdw-2016&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="sql-server"></a>SQL Server

Sauvegarde une base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] complète pour créer une sauvegarde de la base de données, ou un ou plusieurs fichiers ou groupes de fichiers de la base de données pour créer une sauvegarde de fichiers (BACKUP DATABASE). De plus, en mode de restauration complète ou en mode de récupération utilisant les journaux de transactions, sauvegarde le journal des transactions de la base de données afin de créer une sauvegarde de journal (BACKUP LOG).

## <a name="syntax"></a>Syntaxe

```syntaxsql
--Backing Up a Whole Database
BACKUP DATABASE { database_name | @database_name_var }
  TO <backup_device> [ ,...n ]
  [ <MIRROR TO clause> ] [ next-mirror-to ]
  [ WITH { DIFFERENTIAL
           | <general_WITH_options> [ ,...n ] } ]
[;]

--Backing Up Specific Files or Filegroups
BACKUP DATABASE { database_name | @database_name_var }
 <file_or_filegroup> [ ,...n ]
  TO <backup_device> [ ,...n ]
  [ <MIRROR TO clause> ] [ next-mirror-to ]
  [ WITH { DIFFERENTIAL | <general_WITH_options> [ ,...n ] } ]
[;]

--Creating a Partial Backup
BACKUP DATABASE { database_name | @database_name_var }
 READ_WRITE_FILEGROUPS [ , <read_only_filegroup> [ ,...n ] ]
  TO <backup_device> [ ,...n ]
  [ <MIRROR TO clause> ] [ next-mirror-to ]
  [ WITH { DIFFERENTIAL | <general_WITH_options> [ ,...n ] } ]
[;]

--Backing Up the Transaction Log (full and bulk-logged recovery models)
BACKUP LOG
  { database_name | @database_name_var }
  TO <backup_device> [ ,...n ]
  [ <MIRROR TO clause> ] [ next-mirror-to ]
  [ WITH { <general_WITH_options> | \<log-specific_optionspec> } [ ,...n ] ]
[;]
  
<backup_device>::=
 {
  { logical_device_name | @logical_device_name_var }
 | {   DISK
     | TAPE
     | URL } =
     { 'physical_device_name' | @physical_device_name_var | 'NUL' }
 }

<MIRROR TO clause>::=
 MIRROR TO <backup_device> [ ,...n ]

<file_or_filegroup>::=
 {
   FILE = { logical_file_name | @logical_file_name_var }
 | FILEGROUP = { logical_filegroup_name | @logical_filegroup_name_var }
 }

<read_only_filegroup>::=
FILEGROUP = { logical_filegroup_name | @logical_filegroup_name_var }

<general_WITH_options> [ ,...n ]::=
--Backup Set Options
   COPY_ONLY
 | { COMPRESSION | NO_COMPRESSION }
 | DESCRIPTION = { 'text' | @text_variable }
 | NAME = { backup_set_name | @backup_set_name_var }
 | CREDENTIAL
 | ENCRYPTION
 | FILE_SNAPSHOT
 | { EXPIREDATE = { 'date' | @date_var }
        | RETAINDAYS = { days | @days_var } }

--Media Set Options
   { NOINIT | INIT }
 | { NOSKIP | SKIP }
 | { NOFORMAT | FORMAT }
 | MEDIADESCRIPTION = { 'text' | @text_variable }
 | MEDIANAME = { media_name | @media_name_variable }
 | BLOCKSIZE = { blocksize | @blocksize_variable }

--Data Transfer Options
   BUFFERCOUNT = { buffercount | @buffercount_variable }
 | MAXTRANSFERSIZE = { maxtransfersize | @maxtransfersize_variable }

--Error Management Options
   { NO_CHECKSUM | CHECKSUM }
 | { STOP_ON_ERROR | CONTINUE_AFTER_ERROR }

--Compatibility Options
   RESTART

--Monitoring Options
   STATS [ = percentage ]

--Tape Options
   { REWIND | NOREWIND }
 | { UNLOAD | NOUNLOAD }

--Log-specific Options
   { NORECOVERY | STANDBY = undo_file_name }
 | NO_TRUNCATE

--Encryption Options
 ENCRYPTION (ALGORITHM = { AES_128 | AES_192 | AES_256 | TRIPLE_DES_3KEY } , encryptor_options ) <encryptor_options> ::=
   `SERVER CERTIFICATE` = Encryptor_Name | SERVER ASYMMETRIC KEY = Encryptor_Name
```

## <a name="arguments"></a>Arguments

DATABASE Spécifie une sauvegarde complète de la base de données. Si une liste de fichiers et de groupes de fichiers est spécifiée, seuls ceux-ci sont sauvegardés. Au cours d'une sauvegarde de base de données complète ou différentielle, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sauvegarde une portion suffisante du journal des transactions afin d'assurer la cohérence de la base de données lors de la restauration de la sauvegarde.

Lorsque vous restaurez une sauvegarde créée par BACKUP DATABASE (une *sauvegarde de données*), l’ensemble de la sauvegarde est restauré. Seule une sauvegarde du fichier journal peut être restaurée à un moment ou une transaction spécifique au sein de la sauvegarde.

> [!NOTE]
> Seule une sauvegarde complète peut être effectuée sur la base de données **master**.

LOG

Indique que la sauvegarde ne doit porter que sur le journal des transactions. Le journal est sauvegardé à partir de la dernière sauvegarde réussie du fichier journal et jusqu'à sa fin actuelle. Avant de pouvoir créer la première sauvegarde du fichier journal, vous devez créer une sauvegarde complète.

Vous pouvez restaurer une sauvegarde du fichier journal jusqu’à une date et heure précises ou une transaction spécifique en spécifiant `WITH STOPAT`,`STOPATMARK` ou `STOPBEFOREMARK` dans votre instruction [RESTORE LOG](../../t-sql/statements/restore-statements-transact-sql.md).

> [!NOTE]
> Après une sauvegarde de fichier journal standard, certains enregistrements du journal des transactions deviennent inactifs, sauf si vous spécifiez `WITH NO_TRUNCATE` ou `COPY_ONLY`. Le journal est tronqué une fois que tous les enregistrements d'un ou de plusieurs fichiers journaux virtuels sont devenus inactifs. Si le journal n'est pas tronqué après des sauvegardes normales du journal, il se peut que quelque chose retarde la troncation du journal. Pour plus d’informations, consultez [Facteurs pouvant retarder la troncation du journal](../../relational-databases/logs/the-transaction-log-sql-server.md#FactorsThatDelayTruncation).

{ _database\_name_ |  **@** _database\_name\_var_ } Spécifie la base de données à partir de laquelle le journal des transactions, la base de données partielle ou la base de données complète sont sauvegardés. S’il est fourni comme variable ( **@** _database\_name\_var_), ce nom peut être spécifié comme constante de chaîne ( **@** _database\_name\_var_ **=** _database name_) ou comme variable de type de données chaîne de caractères, sauf pour les types de données **ntext** ou **text**.

> [!NOTE]
> La base de données miroir d'un partenariat de mise en miroir de bases de données ne peut pas être sauvegardée.

\<file_or_filegroup> [ **,** ...*n* ] Utilisé uniquement avec BACKUP DATABASE, cet argument spécifie un fichier ou groupe de fichiers de base de données à inclure dans une sauvegarde de fichiers ou spécifie un fichier ou groupe de fichiers en lecture seule à inclure dans une sauvegarde partielle.

FILE **=** { *logical_file_name* | **@** _logical\_file\_name\_var_ } Spécifie le nom logique d’un fichier ou une variable dont la valeur correspond au nom logique d’un fichier à inclure dans la sauvegarde.

FILEGROUP **=** { _logical\_filegroup\_name_ |  **@** _logical\_filegroup\_name\_var_ } Spécifie le nom logique d’un groupe de fichiers ou une variable dont la valeur correspond au nom logique d’un groupe de fichiers à inclure dans la sauvegarde. En mode de récupération simple, la sauvegarde d'un groupe de fichiers n'est autorisée que pour un groupe de fichiers en lecture seule.

> [!NOTE]
> Pensez à utiliser des sauvegardes de fichiers lorsque la taille de la base de données et les exigences en matière de performances rendent la sauvegarde de la base de données totalement inadaptée. L’appareil NULL peut être utilisé pour tester les performances des sauvegardes, mais il ne doit pas être utilisé dans les environnements de production.

*n* Correspond à un espace réservé indiquant qu’il est possible de spécifier plusieurs fichiers et groupes de fichiers dans une liste séparée par des virgules. Le nombre est illimité.

Pour plus d’informations, consultez les articles [Sauvegardes de fichiers complètes](../../relational-databases/backup-restore/full-file-backups-sql-server.md) et [Sauvegarder des fichiers et des groupes de fichiers](../../relational-databases/backup-restore/back-up-files-and-filegroups-sql-server.md).

READ_WRITE_FILEGROUPS [ **,** FILEGROUP = { _logical\_filegroup\_name_ |  **@** _logical\_filegroup\_name\_var_ } [ **,** ..._n_ ] ] Spécifie une sauvegarde partielle. Une sauvegarde partielle inclut tous les fichiers en lecture/écriture dans une base de données : le groupe de fichiers primaire, tous les groupes de fichiers secondaires en lecture/écriture, ainsi que les fichiers ou groupes de fichiers en lecture seule qui ont été spécifiés.

READ_WRITE_FILEGROUPS Spécifie que tous les groupes de fichiers en lecture/écriture doivent être sauvegardés dans la sauvegarde partielle. Si la base de données est en lecture seule, READ_WRITE_FILEGROUPS inclut uniquement le groupe de fichiers primaire.

> [!IMPORTANT]
> Si, au lieu d'utiliser READ_WRITE_FILEGROUPS, vous listez de manière explicite les groupes de fichiers en lecture/écriture en utilisant FILEGROUP, vous allez créer une sauvegarde de fichiers.

FILEGROUP = { *logical_filegroup_name* |  **@** _logical\_filegroup\_name\_var_ } Spécifie le nom logique d’un groupe de fichiers en lecture seule ou une variable dont la valeur correspond au nom logique d’un groupe de fichiers en lecture seule à inclure dans la sauvegarde partielle. Pour plus d'informations, consultez « \<file_or_filegroup> » plus haut dans cette rubrique.

*n* Correspond à un espace réservé indiquant qu’il est possible de spécifier plusieurs groupes de fichiers en lecture seule dans une liste séparée par des virgules.

Pour plus d’informations sur les sauvegardes partielles, consultez l’article [Sauvegardes partielles](../../relational-databases/backup-restore/partial-backups-sql-server.md).

TO \<backup_device> [ **,** ...*n* ] Indique que le jeu d'[unités de sauvegarde](../../relational-databases/backup-restore/backup-devices-sql-server.md) associé est soit un support de sauvegarde non miroir, soit le premier miroir d'un support de sauvegarde miroir (pour lequel une ou plusieurs clauses MIRROR TO sont déclarées).

\<backup_device>

Spécifie l'unité de sauvegarde logique ou physique à utiliser pour l'opération de sauvegarde.

{ *logical_device_name* \| **@** _logical\_device\_name\_var_ } **S’applique à :** SQL Server Spécifie le nom logique de l’unité de sauvegarde dans laquelle la base de données est sauvegardée. Le nom logique doit se conformer aux règles en vigueur pour les identificateurs. Fourni comme variable (@*logical_device_name_var*), le nom de l’unité de sauvegarde peut être spécifié sous la forme d’une constante de chaîne (@_logical\_device\_name\_var_ **=** logical backup device name) ou d’une variable de type chaîne de caractères, sauf pour les types de données **ntext** ou **text**.

{ DISK \| TAPE \| URL} **=** { **'** _physical\_device\_name_ **'** \| **@** _physical\_device\_name\_var_ \| 'NUL' } **S’applique à :** DISK, TAPE et URL s’appliquent à SQL Server.
Spécifie un fichier sur disque ou support à bandes, ou un service de stockage Blob Microsoft Azure. Le format d’URL est utilisé pour créer des sauvegardes dans le service de stockage Microsoft Azure. Pour plus d’informations et d’exemples, consultez [Sauvegarde et restauration SQL Server avec le service de stockage Microsoft Azure](../../relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service.md). Pour suivre un tutoriel, consultez [Tutoriel : Sauvegarde et restauration SQL Server dans le service Stockage Blob Azure](~/relational-databases/tutorial-sql-server-backup-and-restore-to-azure-blob-storage-service.md).

> [!NOTE]
> L’unité de disque NUL supprime toutes les informations qui lui sont envoyées et ne devrait être utilisée qu’à des fins de test. À ne pas utiliser en production.
> [!IMPORTANT]
> De [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SP1 CU2 jusqu’à [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)], vous ne pouvez sauvegarder que sur une seule unité de disque lorsque vous effectuez une sauvegarde vers une URL. Pour effectuer une sauvegarde sur plusieurs unités lorsque vous sauvegardez vers une URL, vous devez utiliser [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] et versions ultérieures, ainsi que des jetons de signature d’accès partagé. Pour obtenir des exemples de signatures d’accès partagé, consultez [Sauvegarde SQL Server vers une URL](../../relational-databases/backup-restore/sql-server-backup-to-url.md) et [Simplifying creation of SQL Credentials with Shared Access Signature (SAS) tokens on Azure Storage with PowerShell](/archive/blogs/sqlcat/simplifying-creation-of-sql-credentials-with-shared-access-signature-sas-tokens-on-azure-storage-with-powershell).

**URL s’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SP1 CU2 et versions ultérieures).

Une unité de disque n'est pas tenue d'exister pour pouvoir être spécifiée dans une instruction BACKUP. Si l'unité physique existe et si l'option INIT n'est pas spécifiée dans l'instruction BACKUP, la sauvegarde est ajoutée à l'unité.

> [!NOTE]
> L’unité NUL va supprimer toutes les entrées envoyées à ce fichier. Toutefois, la sauvegarde continue de marquer toutes les pages comme étant sauvegardées.

Pour plus d’informations, consultez l’article [Unités de sauvegarde](../../relational-databases/backup-restore/backup-devices-sql-server.md).

> [!NOTE]
> L'option TAPE sera supprimée dans une future version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité.

*n* Correspond à un espace réservé indiquant qu’il est possible de spécifier jusqu’à 64 unités de sauvegarde dans une liste séparée par des virgules.

MIRROR TO \<backup_device> [ **,** ...*n* ] Spécifie un jeu constitué au maximum de trois unités de sauvegarde, dont chacune est le miroir des unités de sauvegarde spécifiées dans la clause TO. La clause MIRROR TO doit spécifier le même type et le même nombre d’unités de sauvegarde que la clause TO. Vous pouvez spécifier jusqu'à trois clauses MIRROR TO.

Cette option est disponible uniquement dans l'édition Enterprise de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

> [!NOTE]
> Pour MIRROR TO = DISK, BACKUP détermine automatiquement la taille de bloc appropriée des unités de disques, en fonction de la taille du secteur. Si le disque MIRROR TO est formaté avec une taille de secteur différente de celle du disque spécifié comme unité de sauvegarde principale, la commande de sauvegarde échoue. Pour mettre en miroir des sauvegardes sur des appareils qui ont des tailles de secteur différentes, le paramètre BLOCKSIZE doit être spécifié et doit être défini sur la taille de secteur la plus élevée parmi tous les appareils cibles. Pour plus d’informations sur la taille de bloc, consultez « BLOCKSIZE » plus loin dans cette rubrique.

\<backup_device> Voir « \<backup_device> » plus haut dans cette section.

*n* Correspond à un espace réservé indiquant qu’il est possible de spécifier jusqu’à 64 unités de sauvegarde dans une liste séparée par des virgules. Le nombre d'unités spécifiées dans la clause MIRROR TO doit être égal au nombre d'unités spécifiées dans la clause TO.

Pour plus d’informations, consultez « Familles de supports de sauvegarde miroirs » dans la section [Remarques](#general-remarks), plus loin dans cette rubrique.

[ *next-mirror-to* ] Correspond à un espace réservé indiquant qu’une instruction BACKUP peut contenir jusqu’à trois clauses MIRROR TO, en plus de la clause TO.

### <a name="with-options"></a>Options WITH

Spécifie les options à utiliser avec une opération de sauvegarde.

CREDENTIAL **S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SP1 CU2 et versions ultérieures).
S’utilise uniquement lors de la création d’une sauvegarde dans le service de stockage Blob Microsoft Azure.

FILE_SNAPSHOT **S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ( [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] et versions ultérieures).

Utilisé pour créer un instantané Azure des fichiers de base de données lorsque tous les fichiers de base de données SQL Server sont stockés à l’aide du service Azure Blob Storage. Pour plus d’informations, consultez [Fichiers de données SQL Server dans Microsoft Azure](../../relational-databases/databases/sql-server-data-files-in-microsoft-azure.md). La sauvegarde d’instantanés [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] crée des instantanés Azure des fichiers de base de données (données et fichiers journaux) dont l’état est cohérent. Un ensemble cohérent d’instantanés Azure constitue une sauvegarde, qui est enregistrée dans le fichier de sauvegarde. La seule différence entre `BACKUP DATABASE TO URL WITH FILE_SNAPSHOT` et `BACKUP LOG TO URL WITH FILE_SNAPSHOT` est que ce dernier tronque le journal des transactions. Avec la sauvegarde d’instantanés [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], une fois effectuée la sauvegarde complète initiale dont a besoin [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour établir la chaîne de sauvegarde, seule une sauvegarde de fichier journal est nécessaire pour restaurer une base de données vers le point dans le temps correspondant à la sauvegarde de fichier journal. En outre, seules deux sauvegardes de fichier journal sont nécessaires pour restaurer une base de données vers un point dans le temps situé entre les deux sauvegardes de fichier journal.

DIFFERENTIAL

Utilisé uniquement avec BACKUP DATABASE, ce paramètre spécifie que la sauvegarde de base de données ou de fichier ne doit porter que sur les parties de la base de données ou du fichier qui ont été modifiées depuis la dernière sauvegarde complète. Une sauvegarde différentielle occupe en général moins d'espace qu'une sauvegarde complète. Utilisez cette option de façon à ne pas avoir à appliquer toutes les sauvegardes successives du journal effectuées depuis la dernière sauvegarde complète.

> [!NOTE]
> Par défaut, `BACKUP DATABASE` crée une sauvegarde complète.

Pour plus d’informations, consultez l’article [Sauvegardes différentielles](../../relational-databases/backup-restore/differential-backups-sql-server.md).

ENCRYPTION Utilisé pour spécifier le chiffrement d’une sauvegarde. Vous pouvez spécifier un algorithme de chiffrement pour chiffrer la sauvegarde ou spécifier `NO_ENCRYPTION` pour ne pas chiffrer la sauvegarde. Il est recommandé d'utiliser le chiffrement pour sécuriser les fichiers de sauvegarde. Liste des algorithmes possibles :

- `AES_128`
- `AES_192`
- `AES_256`
- `TRIPLE_DES_3KEY`
- `NO_ENCRYPTION`

Si vous choisissez de chiffrer, vous devez également spécifier le chiffreur à l'aide des options de chiffreur :

- `SERVER CERTIFICATE` = Encryptor_Name
- `SERVER ASYMMETRIC KEY` = Encryptor_Name

`SERVER CERTIFICATE` et `SERVER ASYMMETRIC KEY` correspondent à un certificat et à une clé asymétrique créés dans une base de données `master`. Pour plus d’informations, consultez [`CREATE CERTIFICATE`](../../t-sql/statements/create-certificate-transact-sql.md) et [`CREATE ASYMMETRIC KEY`](../../t-sql/statements/create-asymmetric-key-transact-sql.md).

> [!WARNING]
> Lorsque le chiffrement est utilisé conjointement à l’argument `FILE_SNAPSHOT`, le fichier de métadonnées est chiffré à l’aide de l’algorithme de chiffrement spécifié, et le système vérifie qu’un [chiffrement TDE (Transparent Data Encryption)](../../relational-databases/security/encryption/transparent-data-encryption.md) a été effectué pour la base de données. Aucun autre chiffrement n’est effectué pour les données. La sauvegarde échoue si la base de données n’a pas été chiffrée ou si le chiffrement n’a pas été exécuté avant l’émission de l’instruction BACKUP.

**Options du jeu de sauvegarde**

Ces options s'appliquent au jeu de sauvegarde qui est créé par cette opération de sauvegarde.

> [!NOTE]
> Pour spécifier un jeu de sauvegarde pour une opération de restauration, utilisez l’option `FILE = <backup_set_file_number>`. Pour plus d’informations sur la façon de spécifier un jeu de sauvegarde, consultez la section « Spécification d’un jeu de sauvegarde » dans l’article [Instructions RESTORE – Arguments](../../t-sql/statements/restore-statements-arguments-transact-sql.md).

COPY_ONLY spécifie que la sauvegarde est une *sauvegarde en copie seule* qui n’a aucun impact sur la séquence normale des sauvegardes. Une sauvegarde en copie seule est créée indépendamment de vos sauvegardes régulières standard. Ce type de sauvegarde n'a aucun effet sur les procédures globales de sauvegarde et de restauration de la base de données.

Les sauvegardes en copie seule doivent être utilisées dans les cas où une sauvegarde est effectuée dans un but particulier, par exemple pour sauvegarder le journal avant une restauration de fichiers en ligne. En règle générale, une sauvegarde de fichier journal en copie seule est utilisée une fois, puis supprimée.

- Lorsqu’elle est utilisée avec `BACKUP DATABASE`, l’option `COPY_ONLY` crée une sauvegarde complète qui ne peut pas servir de base différentielle. La bitmap différentielle n'est pas mise à jour et les sauvegardes différentielles se comportent comme si la sauvegarde en copie seule n'existait pas. Les sauvegardes différentielles ultérieures utilisent la dernière sauvegarde complète standard en tant que base.

    > [!IMPORTANT]
    > Si `DIFFERENTIAL` et `COPY_ONLY` sont utilisés ensemble, `COPY_ONLY` est ignoré, et une sauvegarde différentielle est créée.

- Lorsqu’elle est utilisée avec `BACKUP LOG`, l’option `COPY_ONLY` crée une *sauvegarde de fichier journal en copie seule* qui ne tronque pas le journal des transactions. La sauvegarde de journal en copie seule n'a aucun effet sur la séquence de journaux de transactions consécutifs et les autres sauvegardes de journal se comportent comme si la sauvegarde en copie seule n'existait pas.

Pour plus d’informations, consultez l’article [Sauvegardes de type copie seule](../../relational-databases/backup-restore/copy-only-backups-sql-server.md).

{ COMPRESSION | NO_COMPRESSION } Dans [!INCLUDE[ssEnterpriseEd10](../../includes/ssenterpriseed10-md.md)] et les versions ultérieures uniquement, spécifie si la [compression des sauvegardes](../../relational-databases/backup-restore/backup-compression-sql-server.md) est effectuée sur cette sauvegarde, remplaçant la valeur par défaut au niveau du serveur.

Lors de l'installation, le comportement par défaut exclut toute compression des sauvegardes. Toutefois, ce paramétrage par défaut peut être modifié en définissant l’option de configuration du serveur [Compression par défaut des sauvegardes](../../database-engine/configure-windows/view-or-configure-the-backup-compression-default-server-configuration-option.md). Pour plus d’informations sur l’affichage de la valeur actuelle de cette option, consultez l’article [Afficher ou modifier des propriétés de serveur](../../database-engine/configure-windows/view-or-change-server-properties-sql-server.md).

Pour plus d’informations sur l’utilisation de la compression de sauvegarde avec des bases de données où [Transparent Data Encryption (TDE)](../../relational-databases/security/encryption/transparent-data-encryption.md) est activé, consultez la section [Remarques](#general-remarks).

COMPRESSION Active explicitement la compression des sauvegardes.

NO_COMPRESSION Désactive explicitement la compression des sauvegardes.

DESCRIPTION **=** { **'** _text_ **'**  |  **@** _text\_variable_ } Spécifie le texte en forme libre servant à décrire le jeu de sauvegarde. La chaîne peut compter jusqu'à 255 caractères.

NAME **=** { *backup_set_name* |  **@** _backup\_set\_var_ } Spécifie le nom du jeu de sauvegarde. Les noms peuvent contenir jusqu'à 128 caractères. Si l'option NAME n'est pas spécifiée, le nom reste vide.

{ EXPIREDATE **='** _date_ **'** | RETAINDAYS **=** _days_ } Spécifie la date à laquelle le jeu de sauvegarde de cette sauvegarde peut être remplacé. Si ces options sont toutes les deux utilisées, RETAINDAYS l'emporte sur EXPIREDATE.

Si aucune de ces options n’est spécifiée, la date d’expiration est déterminée par le paramètre de configuration **mediaretention**. Pour plus d’informations, consultez l’article [Options de configuration du serveur](../../database-engine/configure-windows/server-configuration-options-sql-server.md).

> [!IMPORTANT]
> Ces options empêchent seulement [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] d'écraser un fichier. Le contenu des bandes peut être écrasé par d'autres méthodes, et les fichiers sur disque peuvent être supprimés à partir du système d'exploitation. Pour plus d'informations sur le contrôle du délai d'expiration, consultez SKIP et FORMAT dans cette rubrique.

EXPIREDATE **=** { **'** _date_ **'**  |  **@** _date\_var_ } Spécifie la date à laquelle le jeu de sauvegarde expire et peut donc être remplacé. Si elle est fournie en tant que variable (@_date\_var_), cette date doit suivre le format **datetime** configuré par le système et prendre l’une des formes suivantes :

- Une constante de chaîne (@_date\_var_ **=** date)
- Une variable de type chaîne de caractères (à l’exception des types de données **ntext** ou **text**)
- Un **smalldatetime**
- Une variable **datetime**

Par exemple :

- `'Dec 31, 2020 11:59 PM'`
- `'1/1/2021'`

Pour plus d’informations sur la spécification des valeurs **datetime**, consultez [Types Date et Time](../../t-sql/data-types/date-and-time-types.md).

> [!NOTE]
> Pour ignorer la date d’expiration, utilisez l’option `SKIP`.

RETAINDAYS **=** { *days* |  **@** _days\_var_ } Spécifie le nombre de jours qui doivent s’écouler avant que ce support de sauvegarde puisse être remplacé. S’il est fourni en tant que variable ( **@** _days\_var_), sa valeur doit être un entier.

**Options du support de sauvegarde**

Ces options s'appliquent à l'ensemble du support de sauvegarde.

{ **NOINIT** | INIT } Détermine si l’opération de sauvegarde ajoute les nouvelles sauvegardes ou si elle remplace les jeux de sauvegardes déjà présents sur le support de sauvegarde. La valeur par défaut (NOINIT) consiste à ajouter les nouvelles sauvegardes après le jeu de sauvegarde le plus récent sur le support.

> [!NOTE]
> Pour plus d’informations sur les interactions entre les options { **NOINIT** | INIT } et { **NOSKIP** | SKIP }, reportez-vous à la section [Remarques](#general-remarks), plus loin dans cette rubrique.

NOINIT Indique que le jeu de sauvegarde est ajouté au support de sauvegarde spécifié, préservant ainsi les jeux de sauvegarde existants. Si un mot de passe de support est défini pour le support de sauvegarde, il doit être fourni. NOINIT est la valeur par défaut.

Pour plus d’informations, consultez l’article [Jeux de supports, familles de supports et jeux de sauvegarde](../../relational-databases/backup-restore/media-sets-media-families-and-backup-sets-sql-server.md).

INIT Indique que tous les jeux de sauvegarde doivent être remplacés, mais préserve l’en-tête de support. Si INIT est spécifié, tous les jeux de sauvegardes qui se trouvent sur l'unité concernée sont écrasés si les conditions l'autorisent. Par défaut, BACKUP vérifie les conditions ci-après et n'écrase pas le support de sauvegarde si l'une des conditions est vraie :

- Un jeu de sauvegarde n'a pas encore expiré. Pour plus d’informations, consultez les options `EXPIREDATE` et `RETAINDAYS`.
- Le nom du jeu de sauvegarde donné dans l'instruction BACKUP, s'il est fourni, ne correspond pas à celui du support de sauvegarde. Pour plus d'informations, consultez l'option NAME, plus haut dans cette section.

Pour ignorer ces vérifications, utilisez l’option `SKIP`.

Pour plus d’informations, consultez l’article [Jeux de supports, familles de supports et jeux de sauvegarde](../../relational-databases/backup-restore/media-sets-media-families-and-backup-sets-sql-server.md).

{ **NOSKIP** | SKIP } Détermine si une opération de sauvegarde vérifie la date et l’heure d’expiration des jeux de sauvegarde figurant sur le support avant de remplacer ces derniers.

> [!NOTE]
> Pour plus d’informations sur les interactions entre les options { **NOINIT** | INIT } et { **NOSKIP** | SKIP }, reportez-vous à la section « Remarques », plus loin dans cette rubrique.

NOSKIP Ordonne à l’instruction BACKUP de vérifier la date d’expiration de tous les jeux de sauvegarde qui se trouvent sur le support avant d’autoriser leur remplacement. Il s'agit du comportement par défaut.

SKIP Désactive la vérification de la date d’expiration et du nom de jeu de sauvegarde qui est habituellement effectuée par l’instruction BACKUP pour empêcher le remplacement des jeux de sauvegarde. Pour plus d'informations sur les interactions entre les options { INIT | NOINIT } et { NOSKIP | SKIP }, reportez-vous à « Remarques », plus loin dans cette rubrique.
Pour afficher les dates d’expiration des jeux de sauvegardes, interrogez la colonne **expiration_date** de la table d’historique [backupset](../../relational-databases/system-tables/backupset-transact-sql.md).

{ **NOFORMAT** | FORMAT } Spécifie si l’en-tête de support doit être écrit sur les volumes utilisés pour cette opération de sauvegarde, en remplaçant l’en-tête de support et les jeux de sauvegarde existants.

NOFORMAT Spécifie que l’opération de sauvegarde conserve l’en-tête de support et les jeux de sauvegarde existants sur les volumes de support utilisés pour cette opération de sauvegarde. Il s'agit du comportement par défaut.

FORMAT Indique qu’un nouveau support de sauvegarde est créé. Si FORMAT est utilisé, l'opération de sauvegarde écrit un nouvel en-tête de support sur tous les volumes utilisés pour cette opération de sauvegarde. Le contenu précédent du volume devient non valide étant donné que l'en-tête de support et les jeux de sauvegardes existants sont écrasés.

> [!IMPORTANT]
> Soyez prudent lorsque vous utilisez `FORMAT`. Si vous formatez l'un des volumes d'un support de sauvegarde, la totalité du support de sauvegarde devient inutilisable. Par exemple, si une bande appartenant à un support de sauvegarde distribuée existant est initialisée, tout le support de sauvegarde devient inutilisable.

La spécification de FORMAT implique `SKIP`. `SKIP` n’a pas besoin d’être explicitement spécifié.

MEDIADESCRIPTION **=** { *text* | **@** _text\_variable_ } Indique le texte descriptif en forme libre (255 caractères maximum) du support de sauvegarde.

MEDIANAME **=** { *media_name* |  **@** _media\_name\_variable_ } Spécifie le nom du support de sauvegarde complet. Le nom du support ne doit pas dépasser 128 caractères. Si `MEDIANAME` est spécifié, il doit correspondre au nom spécifié précédemment existant sur les volumes de sauvegarde. Si elle n'est pas spécifiée, ou si l'option SKIP l'est, aucune vérification du nom de support n'est effectuée.

BLOCKSIZE **=** { *blocksize* |  **@** _blocksize\_variable_ } Indique, en octets, la taille de bloc physique. Les tailles prises en charge sont 512, 1024, 2048, 4096, 8192, 16384, 32768 et 65536 (64 Ko) octets. La valeur par défaut est 65536 pour les périphériques à bandes, 512 sinon. En règle générale, cette option est superflue car BACKUP sélectionne automatiquement une taille de bloc appropriée pour le périphérique. Si vous spécifiez explicitement une taille de bloc, la sélection automatique est annulée et remplacée.

Si vous effectuez une sauvegarde que vous envisagez de copier sur un CD-ROM pour la restaurer à partir de celui-ci, spécifiez BLOCKSIZE=2048.

> [!NOTE]
> En règle générale, cette option n'affecte les performances que si les données sont écrites sur des périphériques à bandes.

**Options de transfert de données**

BUFFERCOUNT **=** { *buffercount* |  **@** _buffercount\_variable_ } Spécifie le nombre total de tampons d’E/S à utiliser pour l’opération de sauvegarde. Vous pouvez spécifier n'importe quel entier positif ; toutefois, un nombre élevé de tampons peut provoquer des erreurs liées à une insuffisance de mémoire. En effet, l'espace d'adressage virtuel peut s'avérer inapproprié dans la tâche Sqlservr.exe.

L’espace total utilisé par les mémoires tampons est déterminé par : `BUFFERCOUNT * MAXTRANSFERSIZE`.

> [!NOTE]
> Pour des informations importantes sur l’utilisation de l’option `BUFFERCOUNT`, consultez le blog [Incorrect BufferCount data transfer option can lead to OOM condition](/archive/blogs/sqlserverfaq/incorrect-buffercount-data-transfer-option-can-lead-to-oom-condition).

MAXTRANSFERSIZE **=** { *maxtransfersize* | _**@** maxtransfersize\_variable_ } Spécifie la plus grande unité de transfert, en octets, à utiliser entre [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et le support de sauvegarde. Les valeurs possibles sont les multiples de 65536 octets (64 Ko), dans la limite de 4194304 octets (4 Mo).

> [!NOTE]
> Lors de la création de sauvegardes à l’aide du service SQL Writer, si la base de données est configurée avec [FILESTREAM](../../relational-databases/blob/filestream-sql-server.md), ou si elle comprend des [groupes de fichiers à mémoire optimisée](../../relational-databases/in-memory-oltp/the-memory-optimized-filegroup.md), la valeur de `MAXTRANSFERSIZE` au moment de la restauration doit être supérieure ou égale à celle du `MAXTRANSFERSIZE` qui a été utilisée lors de la création de la sauvegarde.

> [!NOTE]
> Pour les bases de données comprenant un seul fichier de données et où [Transparent Data Encryption (TDE)](../../relational-databases/security/encryption/transparent-data-encryption.md) est activé, la valeur `MAXTRANSFERSIZE` par défaut est 65 536 (64 Ko). Pour les bases de données non chiffrées à l’aide de TDE, la valeur `MAXTRANSFERSIZE` par défaut est 1 048 576 (1 Mo) lors d’une sauvegarde vers DISK, et 65 536 (64 Ko) lors d’une sauvegarde vers VDI ou TAPE.
> Pour plus d’informations sur l’utilisation de la compression de sauvegarde avec des bases de données chiffrées avec TDE, consultez la section [Remarques](#general-remarks).

**Options de gestion des erreurs**

Ces options vous permettent de déterminer si les sommes de contrôle de sauvegarde sont activées pour l’opération de sauvegarde, et si l’opération doit s’arrêter en présence d’une erreur.

{ **NO_CHECKSUM** | CHECKSUM } Détermine si les sommes de contrôle de sauvegarde sont activées.

NO_CHECKSUM Désactive explicitement la génération de sommes de contrôle de sauvegarde (et la validation des sommes de contrôle de page). Il s'agit du comportement par défaut.

CHECKSUM Indique que l’opération de sauvegarde vérifie dans chaque page les informations de somme de contrôle et de page endommagée, si elles sont activées et disponibles, et génère une somme de contrôle pour l’ensemble de la sauvegarde.

L'utilisation des sommes de contrôle de sauvegarde peut affecter la charge de travail et le débit de sauvegarde.

Pour plus d’informations, consultez l’article [Erreurs de support possibles pendant les opérations de sauvegarde et de restauration](../../relational-databases/backup-restore/possible-media-errors-during-backup-and-restore-sql-server.md).

{ **STOP_ON_ERROR** | CONTINUE_AFTER_ERROR } Détermine si une opération de sauvegarde s’arrête ou continue après avoir rencontré une erreur de somme de contrôle de page.

STOP_ON_ERROR Ordonne à BACKUP de s’arrêter si une somme de contrôle de page n’est pas validée. Il s'agit du comportement par défaut.

CONTINUE_AFTER_ERROR Ordonne à BACKUP de continuer en dépit des erreurs rencontrées, telles que des sommes de contrôle de page non valides ou des pages endommagées.

Si vous ne parvenez pas à effectuer une sauvegarde de la fin du journal à l’aide de l’option NO_TRUNCATE lorsque la base de données est endommagée, vous pouvez essayer d’effectuer une [sauvegarde de la fin du journal](../../relational-databases/backup-restore/tail-log-backups-sql-server.md) en spécifiant CONTINUE_AFTER_ERROR au lieu de NO_TRUNCATE.

Pour plus d’informations, consultez l’article [Erreurs de support possibles pendant les opérations de sauvegarde et de restauration](../../relational-databases/backup-restore/possible-media-errors-during-backup-and-restore-sql-server.md).

**Options de compatibilité**

RESTART N’a aucun effet depuis [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]. Elle est acceptée par cette version à des fins de compatibilité avec les versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

**Options de surveillance**

STATS [ **=** _percentage_ ] Affiche un message chaque fois qu’un autre élément *percentage* se termine, et sert à évaluer l’état d’avancement de l’opération. Si *percentage* est omis, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] affiche un message à chaque incrément de 10 pour cent.

L'option STATS signale le pourcentage terminé comme seuil de rapport de l'intervalle suivant. C'est-à-dire approximativement le pourcentage spécifié ; par exemple, si STATS=10, et si le pourcentage terminé est 40 pour cent, l'option peut afficher 43 pour cent. Pour les jeux de sauvegardes volumineux, cela n'est pas un problème car le pourcentage terminé varie très lentement entre les appels d'E/S terminés.

**Options des périphériques à bandes**

Ces options sont utilisées uniquement pour les périphériques À BANDES. S'il ne s'agit pas d'un périphérique à bandes, ces options sont ignorées.

{ **REWIND** | NOREWIND } REWIND

Indique que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] libère et rembobine la bande. REWIND est le paramètre par défaut.

NOREWIND

Indique que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] maintient la bande ouverte après l'opération de sauvegarde. Cette option vous permet d'améliorer les performances lorsque vous effectuez plusieurs opérations de sauvegarde sur une bande.

NOREWIND implique NOUNLOAD, et ces options sont incompatibles dans une instruction BACKUP unique.

> [!NOTE]
> Si vous utilisez `NOREWIND`, l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] conserve la propriété du lecteur de bande jusqu’à ce qu’une instruction BACKUP ou RESTORE s’exécutant dans le même processus utilise l’option `REWIND` ou `UNLOAD`, ou jusqu’à l’arrêt de l’instance du serveur. Le fait de maintenir la bande ouverte empêche les autres processus d'y accéder. Pour savoir comment afficher la liste des bandes ouvertes et comment fermer ces bandes, consultez l’article [Unités de sauvegarde](../../relational-databases/backup-restore/backup-devices-sql-server.md).

{ **UNLOAD** | NOUNLOAD }

> [!NOTE]
> `UNLOAD` et `NOUNLOAD` sont des paramètres de session qui sont conservés jusqu’à la fin de la session ou tant qu’ils ne sont pas réinitialisés par la spécification d’une autre option.

UNLOAD

Indique que la bande est automatiquement rembobinée et démontée lorsque la sauvegarde est terminée. UNLOAD est l'option par défaut au démarrage d'une session.

NOUNLOAD

Indique qu’au terme de l’opération BACKUP, la bande reste chargée dans le lecteur de bande.

> [!NOTE]
> Dans le cas d’une sauvegarde sur une unité de sauvegarde sur bande, l’option `BLOCKSIZE` affecte les performances de l’opération de sauvegarde. En règle générale, cette option n'affecte les performances que si les données sont écrites sur des périphériques à bandes.

**Options spécifiques au journal**

Ces options ne sont utilisées qu’avec `BACKUP LOG`.

> [!NOTE]
> Si vous ne voulez pas effectuer de sauvegarde du journal, utilisez le mode de récupération simple. Pour plus d’informations, voir [Modes de récupération](../../relational-databases/backup-restore/recovery-models-sql-server.md).

{ NORECOVERY | STANDBY **=** _undo_file_name_ }

NORECOVERY

Effectue une sauvegarde de la fin du journal et laisse la base de données en état de restauration (RESTORING). NORECOVERY s'avère utile lors du basculement vers une base de données secondaire ou de l'exécution d'une sauvegarde de la fin du journal avant une opération RESTORE.

Pour effectuer au mieux une sauvegarde du journal qui évite la troncation du journal et place la base de données en état RESTORING de manière atomique, utilisez conjointement les options `NO_TRUNCATE` et `NORECOVERY`.

STANDBY **=** _standby_file_name_

Effectue une sauvegarde de la fin du journal et laisse la base de données en lecture seule et en état STANDBY. La clause STANDBY écrit les données en attente (annulation avec option de restauration ultérieure). L'option STANDBY est semblable à BACKUP LOG WITH NORECOVERY suivie par RESTORE WITH STANDBY.

Le mode d’attente nécessite un fichier d’annulation, spécifié par *standby_file_name*, dont l’emplacement figure dans le journal de la base de données. Si le fichier spécifié existe déjà, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] l'écrase ; sinon, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] le crée. Le fichier d'annulation devient partie intégrante de la base de données.

Ce fichier contient les modifications annulées, qui doivent être restaurées si des opérations RESTORE LOG sont effectuées ultérieurement. Vous devez disposer d'un espace disque suffisant pour que le fichier d'annulation puisse contenir toutes les pages distinctes de la base de données qui ont été modifiées par suite du rejet des transactions non validées.

NO_TRUNCATE

Indique que le journal n’est pas tronqué et que [!INCLUDE[ssDE](../../includes/ssde-md.md)] tente la sauvegarde, quel que soit l’état de la base de données. Par conséquent, les métadonnées d’une sauvegarde effectuée avec `NO_TRUNCATE` peuvent être incomplètes. Cette option permet de sauvegarder le journal lorsque la base de données est endommagée.

L'option NO_TRUNCATE de BACKUP LOG revient à spécifier COPY_ONLY et CONTINUE_AFTER_ERROR.

Sans l’option `NO_TRUNCATE`, l’état de la base de données doit avoir la valeur ONLINE. Si l’état de la base de données a la valeur SUSPENDED, vous pouvez créer une sauvegarde en spécifiant `NO_TRUNCATE`. Toutefois, si l’état de la base de données a la valeur OFFLINE ou EMERGENCY, l’option BACKUP n’est pas autorisée même avec l’option `NO_TRUNCATE`. Pour plus d’informations sur les états de base de données, consultez [États d’une base de données](../../relational-databases/databases/database-states.md).

## <a name="about-working-with-sql-server-backups"></a>À propos de l’utilisation des sauvegardes SQL Server

Cette section présente les concepts de sauvegarde essentiels suivants :

[Types de sauvegarde](#Backup_Types)
[Troncation du journal des transactions](#Tlog_Truncation)
[Formatage du support de sauvegarde](#Formatting_Media)
[Utilisation des unités de sauvegarde et des supports de sauvegarde](#Backup_Devices_and_Media_Sets)
[Restauration de sauvegardes SQL Server](#Restoring_Backups)

> [!NOTE]
> Pour découvrir une présentation de la sauvegarde dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consultez l’article [Vue d’ensemble de la sauvegarde](../../relational-databases/backup-restore/backup-overview-sql-server.md).

### <a name="backup-types"></a><a name="Backup_Types"></a> Types de sauvegarde

Les types de sauvegarde pris en charge dépendent du mode de récupération de la base de données conformément à ce qui suit :

- Tous les modes de récupération prennent en charge les sauvegardes de données complètes et différentielles.

    |Étendue de la sauvegarde|Types de sauvegarde|
    |---------------------|------------------|
    |Base de données entière|Les [sauvegardes de base de données](../../relational-databases/backup-restore/full-database-backups-sql-server.md) couvrent l’ensemble de la base de données.<br /><br /> Chaque sauvegarde de base de données peut éventuellement servir de base pour une série d’une ou de plusieurs [sauvegardes de bases de données différentielles](../../relational-databases/backup-restore/differential-backups-sql-server.md).|
    |Base de données partielle|Les [sauvegardes partielles](../../relational-databases/backup-restore/partial-backups-sql-server.md)couvrent les groupes de fichiers en lecture/écriture et, éventuellement, un ou plusieurs fichiers ou groupes de fichiers en lecture seule.<br /><br /> Chaque sauvegarde partielle peut éventuellement servir de base pour une série d’une ou de plusieurs [sauvegardes partielles différentielles](../../relational-databases/backup-restore/differential-backups-sql-server.md).|
    |Fichier ou groupe de fichiers|Les [sauvegardes de fichiers](../../relational-databases/backup-restore/full-file-backups-sql-server.md) couvrent un ou plusieurs fichiers ou groupes de fichiers et ne conviennent qu’aux bases de données contenant plusieurs groupes de fichiers. En mode de récupération simple, les sauvegardes de fichiers se limitent essentiellement aux groupes de fichiers secondaires en lecture seule.<br /> Chaque sauvegarde de fichiers peut éventuellement servir de base pour une série d’une ou de plusieurs [sauvegardes de fichiers différentielles](../../relational-databases/backup-restore/differential-backups-sql-server.md).|

- En mode de récupération complète ou en mode de récupération utilisant les journaux de transactions, les sauvegardes standard incluent également les *sauvegardes des journaux de transactions* (ou *sauvegardes de fichier journal*) séquentielles qui sont nécessaires. Chaque sauvegarde de fichier journal couvre la partie du journal des transactions qui est active au moment de la création de la sauvegarde et inclut tous les enregistrements de journal qui n'ont pas été sauvegardés lors d'une précédente sauvegarde de journal.

    Pour réduire au maximum les risques de perte de travail, mais avec un coût en termes de charge d'administration, vous devez planifier des sauvegardes de fichier journal fréquentes. La planification de sauvegardes différentielles entre des sauvegardes complètes peut réduire le temps de restauration en diminuant le nombre de sauvegardes de fichier journal à restaurer après la restauration des données.

     Nous vous recommandons de placer les sauvegardes de fichier journal sur un autre volume que les sauvegardes de base de données.

    > [!NOTE]
    > Avant de pouvoir créer la première sauvegarde du fichier journal, vous devez créer une sauvegarde complète.

- Une *sauvegarde en copie seule* est une sauvegarde complète ou une sauvegarde de fichier journal qui est réalisée dans un but précis, et qui est indépendante de la séquence normale des sauvegardes standard. Pour créer une sauvegarde en copie seule, spécifiez l'option COPY_ONLY dans votre instruction BACKUP. Pour plus d’informations, consultez l’article [Sauvegardes de type copie seule](../../relational-databases/backup-restore/copy-only-backups-sql-server.md).

### <a name="transaction-log-truncation"></a><a name="Tlog_Truncation"></a> Troncation du journal des transactions

Pour éviter de remplir le journal des transactions d'une base de données, les sauvegardes régulières sont essentielles. En mode de récupération simple, la troncation du journal se produit automatiquement après la sauvegarde de la base de données ; en mode de récupération complète, elle se produit automatiquement après la sauvegarde du journal des transactions. Toutefois, le processus de troncation peut parfois être différé. Pour plus d’informations sur les facteurs susceptibles de retarder la troncation du journal, consultez l’article [Journal des transactions](../../relational-databases/logs/the-transaction-log-sql-server.md).

> [!NOTE]
> Les options `BACKUP LOG WITH NO_LOG` et `WITH TRUNCATE_ONLY` ne sont plus disponibles. Si vous utilisez le mode de récupération complète ou le mode de récupération utilisant les journaux de transactions et si vous devez supprimer la chaîne de sauvegarde de fichier journal d'une base de données, passez au mode de récupération simple. Pour plus d’informations, consultez l’article [Afficher ou modifier le mode de récupération d’une base de données](../../relational-databases/backup-restore/view-or-change-the-recovery-model-of-a-database-sql-server.md).

### <a name="formatting-backup-media"></a><a name="Formatting_Media"></a> Formatage du support de sauvegarde

Le support de sauvegarde est formaté par une instruction BACKUP si, et uniquement si, l'une des conditions suivantes est vraie :

- L’option `FORMAT` est spécifiée.
- Le support est vide.
- L'opération écrit sur une bande magnétique de sauvegarde consécutive.

### <a name="working-with-backup-devices-and-media-sets"></a><a name="Backup_Devices_and_Media_Sets"></a> Utilisation des unités de sauvegarde et des supports de sauvegarde

#### <a name="backup-devices-in-a-striped-media-set-a-stripe-set"></a>Unités de sauvegarde d’un jeu de sauvegarde distribuée (jeu de bandes)
Un *jeu de bandes* est un ensemble de fichiers disque sur lesquels les données sont divisées en blocs et distribuées dans un ordre fixe. Le nombre d’unités de sauvegarde utilisées dans un jeu de bandes doit rester le même (sauf si le support est réinitialisé avec`FORMAT`).

L'exemple ci-dessous écrit une sauvegarde de la base de données [!INCLUDE[ssSampleDBUserInputNonLocal](../../includes/sssampledbuserinputnonlocal-md.md)] sur un nouveau jeu de sauvegarde distribuée qui utilise trois fichiers disque.

```sql
BACKUP DATABASE AdventureWorks2012
TO DISK='X:\SQLServerBackups\AdventureWorks1.bak',
DISK='Y:\SQLServerBackups\AdventureWorks2.bak',
DISK='Z:\SQLServerBackups\AdventureWorks3.bak'
WITH FORMAT,
  MEDIANAME = 'AdventureWorksStripedSet0',
  MEDIADESCRIPTION = 'Striped media set for AdventureWorks2012 database';
GO
```

Lorsqu'une unité de sauvegarde a été définie comme faisant partie d'un jeu de bandes, elle ne peut plus être utilisée pour une sauvegarde d'unité unique, sauf si l'option FORMAT est spécifiée. De la même façon, une unité qui contient des sauvegardes non agrégées ne peut pas être utilisée dans un jeu d'agrégats, sauf si l'option FORMAT est spécifiée. Pour diviser un jeu de sauvegarde par bandes, utilisez l'option FORMAT.

Si ni MEDIANAME ni MEDIADESCRIPTION n’est spécifié lorsqu’un en-tête de support est écrit, le champ d’en-tête de support correspondant à l’élément non spécifié est vide.

#### <a name="working-with-a-mirrored-media-set"></a>Utilisation d’un support de sauvegarde miroir

En règle générale, les sauvegardes ne sont pas mises en miroir et les instructions BACKUP contiennent simplement une clause TO. Toutefois, il est possible de créer jusqu'à quatre miroirs par support de sauvegarde. Dans le cas d'un support de sauvegarde miroir, l'opération de sauvegarde écrit dans plusieurs groupes d'unités de sauvegarde. Chaque groupe d'unités de sauvegarde constitue un miroir au sein du support de sauvegarde miroir. Chaque miroir doit utiliser le même nombre et le même type d'unités de sauvegarde physiques, lesquelles doivent toutes avoir les mêmes propriétés.

Pour effectuer une sauvegarde dans un support de sauvegarde miroir, tous les miroirs doivent être présents. Pour effectuer une sauvegarde sur un support de sauvegarde miroir, spécifiez la clause `TO` pour le premier miroir et spécifiez une clause `MIRROR TO` pour chacun des autres miroirs.

Pour un support de sauvegarde miroir, chaque clause `MIRROR TO` doit contenir le même nombre et le même type d’unités que la clause TO. L'exemple suivant écrit dans un support de sauvegarde miroir contenant deux miroirs et utilisant trois unités par miroir :

```sql
BACKUP DATABASE AdventureWorks2012
TO DISK='X:\SQLServerBackups\AdventureWorks1a.bak',
  DISK='Y:\SQLServerBackups\AdventureWorks2a.bak',
  DISK='Z:\SQLServerBackups\AdventureWorks3a.bak'
MIRROR TO DISK='X:\SQLServerBackups\AdventureWorks1b.bak',
  DISK='Y:\SQLServerBackups\AdventureWorks2b.bak',
  DISK='Z:\SQLServerBackups\AdventureWorks3b.bak';
GO
```

> [!IMPORTANT]
> Cet exemple vous permet d'effectuer un test sur votre système local. En pratique, effectuer une sauvegarde dans plusieurs unités se trouvant sur le même lecteur nuit aux performances et élimine la redondance pour laquelle les supports de sauvegarde miroirs sont conçus.

##### <a name="media-families-in-mirrored-media-sets"></a>Familles de supports de sauvegarde miroirs

Chaque unité de sauvegarde spécifiée dans la clause `TO` d’une instruction BACKUP correspond à une famille de supports. Par exemple, si la clause `TO` répertorie trois unités, BACKUP écrit des données dans trois familles de supports. Dans un support de sauvegarde miroir, chaque miroir doit contenir une copie de chacune des familles de supports. C'est pour cette raison que le nombre d'unités doit être identique pour tous les miroirs.

Si plusieurs unités sont spécifiées pour chaque miroir, leur ordre détermine la famille de supports qui est écrite sur chacune d'elles. Par exemple, dans chaque liste d'unités, la deuxième unité correspond à la deuxième famille de supports. Le tableau suivant établit la correspondance entre unités et familles de supports pour les unités de l'exemple ci-dessus.

|Miroir|Famille de supports 1|Famille de supports 2|Famille de supports 3|
|---------|---------|---------|---------|
|0|`Z:\AdventureWorks1a.bak`|`Z:\AdventureWorks2a.bak`|`Z:\AdventureWorks3a.bak`|
|1|`Z:\AdventureWorks1b.bak`|`Z:\AdventureWorks2b.bak`|`Z:\AdventureWorks3b.bak`|

 Une famille de supports doit toujours être sauvegardée sur la même unité à l'intérieur d'un miroir donné. Par conséquent, à chaque fois que vous utilisez un support de sauvegarde existant, répertoriez les unités de chaque miroir dans l'ordre dans lequel elles ont été spécifiées au moment de la création du support de sauvegarde.

Pour plus d’informations sur les supports de sauvegarde miroir, consultez l’article [Jeux de supports de sauvegarde en miroir](../../relational-databases/backup-restore/mirrored-backup-media-sets-sql-server.md). Pour plus d’informations sur les supports de sauvegarde et les familles de supports en général, consultez l’article [Jeux de supports, familles de supports et jeux de sauvegarde](../../relational-databases/backup-restore/media-sets-media-families-and-backup-sets-sql-server.md).

### <a name="restoring-sql-server-backups"></a><a name="Restoring_Backups"></a> Restauration de sauvegardes SQL Server

Pour restaurer une base de données et, éventuellement, la récupérer pour la mettre en ligne, ou pour restaurer un fichier ou un groupe de fichiers, utilisez l’instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md) ou les tâches [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] de **restauration**. Pour plus d’informations, consultez l’article [Vue d’ensemble de la restauration et de la récupération](../../relational-databases/backup-restore/restore-and-recovery-overview-sql-server.md).

## <a name="additional-considerations-about-backup-options"></a><a name="Additional_Considerations"></a> Considérations supplémentaires à propos des options BACKUP

### <a name="interaction-of-skip-noskip-init-and-noinit"></a><a name="Interactions_SKIP_etc"></a> Interactions de SKIP, NOSKIP, INIT et NOINIT

Ce tableau décrit les interactions entre les options { **NOINIT** | INIT } et { **NOSKIP** | SKIP }.

> [!NOTE]
> Si le support de bande est vide ou que le fichier de sauvegarde sur disque n'existe pas, toutes ces interactions écrivent un en-tête de support, puis se poursuivent. Si le support n'est pas vide et qu'il ne contient pas d'en-tête de support valide, ces opérations signalent qu'il ne s'agit pas d'un support MTF valide et mettent fin à l'opération de sauvegarde.

|Option Skip|NOINIT|INIT|
|------|------------|----------|
|NOSKIP|Si le volume contient un en-tête de support valide, vérifie que le nom du support correspond à la valeur de `MEDIANAME`, si elle est spécifiée. Si les deux noms correspondent, ajoute le jeu de sauvegarde en gardant ceux qui existent déjà.<br /> Si le volume ne contient pas d'en-tête de support valide, une erreur se produit.|Si le volume contient un en-tête de support valide, effectue les vérifications suivantes :<br /><ul><li>Si `MEDIANAME` a été spécifié, vérifie que le nom indiqué correspond à celui qui est mentionné dans l’en-tête du support.<sup>1</sup></li><li>Vérifie qu'il n'y a pas déjà sur le support des jeux de sauvegardes qui ne seraient pas encore arrivés à expiration. S'il y en a, met fin à la sauvegarde.</li></ul><br />Si toutes ces vérifications sont validées, écrase tous les jeux de sauvegardes présents sur le support en ne conservant que l'en-tête de support.<br /> Si le volume ne contient pas d’en-tête de support valide, en génère un en utilisant les valeurs de `MEDIANAME` et `MEDIADESCRIPTION`, si elles sont spécifiées.|
|SKIP|Si le volume contient un en-tête de support valide, ajoute le jeu de sauvegarde, en gardant ceux qui existent déjà.|Si le volume contient un en-tête de support valide<sup>2</sup>, écrase tous les jeux de sauvegardes présents sur le support en ne gardant que l’en-tête.<br /> Si le support est vide, génère un en-tête de support en utilisant les valeurs de `MEDIANAME` et `MEDIADESCRIPTION`, si elles sont spécifiées.|

<sup>1</sup> L’utilisateur doit avoir le rôle serveur ou le rôle de base de données fixe approprié pour effectuer une opération de sauvegarde.

<sup>2</sup> Pour être valide, il doit faire état du numéro de version MTF et d’autres informations d’en-tête. Si la version indiquée n'est pas prise en charge ou pas reconnue, une erreur se produit.

## <a name="compatibility"></a>Compatibilité

> [!CAUTION]
> Les sauvegardes créées avec une version plus récente de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne peuvent pas être restaurées dans les versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

BACKUP prend en charge l’option `RESTART` pour assurer la compatibilité descendante avec les versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Mais RESTART est inopérant.

## <a name="general-remarks"></a>Remarques générales

Les sauvegardes de base de données ou de fichier journal peuvent être ajoutées à n'importe quel périphérique de disque ou à bandes, ce qui permet de conserver au même emplacement physique la base de données et ses journaux de transactions.

L'instruction BACKUP n'est pas autorisée dans une transaction explicite ou implicite.

Les opérations de sauvegarde inter-plateformes, impliquant éventuellement des types de processeurs différents, peuvent être réalisées tant que le classement de la base de données est pris en charge par le système d'exploitation.

Avec [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] et versions ultérieures, définir `MAXTRANSFERSIZE` **sur une valeur supérieure à 65536 (64 Ko)** permet d’utiliser un algorithme de compression optimisé pour les bases de données chiffrées avec [Transparent Data Encryption (TDE)](../../relational-databases/security/encryption/transparent-data-encryption.md), qui chiffre d’abord une page, la compresse, puis la chiffre de nouveau. Si `MAXTRANSFERSIZE` n’est pas spécifiée, ou si `MAXTRANSFERSIZE = 65536` (64 Ko) est utilisé, la compression de sauvegarde pour les bases de données chiffrées avec TDE compresse directement les pages chiffrées et peut ne pas fournir de bons taux de compression. Pour plus d’informations, consultez [Backup Compression for TDE-enabled Databases](/archive/blogs/sqlcat/sqlsweet16-episode-1-backup-compression-for-tde-enabled-databases).

À partir de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] CU5, la définition de `MAXTRANSFERSIZE` n’est plus nécessaire pour activer cet algorithme de compression optimisé avec TDE. Si la commande de sauvegarde est spécifiée `WITH COMPRESSION` ou que la configuration serveur de *compression par défaut des sauvegardes* est définie sur 1, `MAXTRANSFERSIZE` sera automatiquement augmentée à 128 K pour activer l’algorithme optimisé. Si `MAXTRANSFERSIZE` est spécifiée dans la commande Backup avec une valeur > 64 Ko, la valeur fournie est respectée. En d’autres termes, SQL Server ne diminue jamais automatiquement la valeur, elle l’augmente uniquement. Si vous avez besoin de sauvegarder une base de données chiffrée TDE avec `MAXTRANSFERSIZE = 65536`, vous devez spécifier `WITH NO_COMPRESSION` ou vous assurer que la configuration serveur de *compression par défaut des sauvegardes* est définie sur 0.

> [!NOTE]
> Dans certains cas, la valeur par défaut `MAXTRANSFERSIZE` est supérieure à 64 Ko :
>
> - Quand la base de données a plusieurs fichiers de données créés, elle utilise `MAXTRANSFERSIZE` > 64 Ko
> - Quand vous effectuez une sauvegarde vers une URL, la valeur par défaut `MAXTRANSFERSIZE = 1048576` (1 Mo)
>
> Même si une de ces conditions s’applique, vous devez définir explicitement `MAXTRANSFERSIZE` sur une valeur supérieure à 64 Ko dans votre commande de sauvegarde afin d’obtenir l’algorithme de compression de sauvegarde optimisé, sauf si vous êtes sur [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] CU5 ou version ultérieure.

Par défaut, chaque opération de sauvegarde réussie ajoute une entrée au journal des erreurs [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et au journal des événements système. Si vous sauvegardez très fréquemment le journal, ces messages de réussite peuvent rapidement s'accumuler, créer des journaux d'erreurs très volumineux et compliquer la recherche d'autres messages. Dans de tels cas, vous pouvez supprimer ces entrées de journal en utilisant l'indicateur de trace 3226 si aucun de vos scripts ne dépend de ces entrées. Pour plus d’informations, consultez l’article [Indicateurs de trace](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md).

## <a name="interoperability"></a>Interopérabilité

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilise un processus de sauvegarde en ligne pour permettre qu’une base de données soit sauvegardée alors qu’elle est encore utilisée. Lors d'une sauvegarde, la plupart des opérations sont possibles ; par exemple, les instructions INSERT, UPDATE et DELETE sont autorisées.

Parmi les opérations qui ne peuvent pas être effectuées lors d'une sauvegarde de base de données ou du journal des transactions, citons :

- Les opérations de gestion de fichiers telles que l’instruction `ALTER DATABASE` avec les options `ADD FILE` ou `REMOVE FILE`.

- Les opérations de compactage de base de données ou de fichier. Cela comprend également les opérations de compactage automatique.

Si une opération de sauvegarde chevauche une opération de réduction ou de gestion des fichiers, un conflit se produit. Quelle que soit l'opération effectuée la première, la seconde opération attend que le verrou défini par la première opération expire (le délai d'expiration est contrôlé par un paramètre d'expiration de la session). Si le verrou est libéré au cours du délai d'expiration, la seconde opération se poursuit. Si le verrou expire, la seconde opération échoue.

## <a name="metadata"></a>Métadonnées

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] intègre les tables d'historique de sauvegarde suivantes pour assurer le suivi des activités de sauvegarde :

- [backupfile](../../relational-databases/system-tables/backupfile-transact-sql.md)
- [backupfilegroup](../../relational-databases/system-tables/backupfilegroup-transact-sql.md)
- [backupmediafamily](../../relational-databases/system-tables/backupmediafamily-transact-sql.md)
- [backupmediaset](../../relational-databases/system-tables/backupmediaset-transact-sql.md)
- [backupset](../../relational-databases/system-tables/backupset-transact-sql.md)

Si une restauration est effectuée et si le jeu de sauvegarde n’est pas encore enregistré dans la base de données **msdb**, les tables d’historique de sauvegarde peuvent être modifiées.

## <a name="security"></a>Sécurité

À compter de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], les options `PASSWORD` et `MEDIAPASSWORD` ne sont plus disponibles pour la création de sauvegardes. Il est encore possible de restaurer des sauvegardes créées avec des mots de passe.

### <a name="permissions"></a>Autorisations

Les autorisations BACKUP DATABASE et BACKUP LOG reviennent par défaut aux membres du rôle serveur fixe **sysadmin** et des rôles de base de données fixes **db_owner** et **db_backupoperator** .

Des problèmes de propriété et d'autorisations sur le fichier physique de l'unité de sauvegarde sont susceptibles de perturber une opération de sauvegarde. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doit être en mesure de lire et d'écrire sur l'unité ; le compte sous lequel le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] s'exécute doit avoir des autorisations d'écriture. Toutefois, [sp_addumpdevice](../../relational-databases/system-stored-procedures/sp-addumpdevice-transact-sql.md), qui ajoute une entrée pour une unité de sauvegarde dans les tables système, ne vérifie pas les autorisations d’accès au fichier. De tels problèmes pour le fichier physique de l'unité de sauvegarde peuvent n'apparaître que lorsque la ressource physique est sollicitée au moment de la sauvegarde ou de la restauration.

## <a name="examples"></a><a name="examples"></a> Exemples

Cette section contient les exemples suivants :

- R. [Sauvegarde d'une base de données complète](#backing_up_db)
- B. [Sauvegarde de la base de données et du journal](#backing_up_db_and_log)
- C. [Création d’une sauvegarde de fichiers complète à partir de groupes de fichiers secondaires](#full_file_backup)
- D. [Création d’une sauvegarde de fichiers différentielle à partir de groupes de fichiers secondaires](#differential_file_backup)
- E. [Création et sauvegarde sur un support de sauvegarde miroir d’une seule famille](#create_single_family_mirrored_media_set)
- F. [Création et sauvegarde sur un support de sauvegarde miroir de plusieurs familles](#create_multifamily_mirrored_media_set)
- G. [Sauvegarde dans un support de sauvegarde miroir existant](#existing_mirrored_media_set)
- H. [Création d’une sauvegarde compressée sur un nouveau support de sauvegarde](#creating_compressed_backup_new_media_set)
- I. [Sauvegarde dans le service Stockage Blob Microsoft Azure](#url)
- J. [Suivi de la progression de l’instruction de sauvegarde](#backup_progress)

> [!NOTE]
> Les rubriques de procédures de sauvegarde contiennent des exemples supplémentaires. Pour plus d’informations, consultez l’article [Vue d’ensemble de la sauvegarde](../../relational-databases/backup-restore/backup-overview-sql-server.md).

### <a name="a-backing-up-a-complete-database"></a><a name="backing_up_db"></a> A. Sauvegarde d'une base de données complète

L’exemple ci-dessous sauvegarde la base de données [!INCLUDE[ssSampleDBUserInputNonLocal](../../includes/sssampledbuserinputnonlocal-md.md)] dans un fichier disque.

```sql
BACKUP DATABASE AdventureWorks2012
 TO DISK = 'Z:\SQLServerBackups\AdvWorksData.bak'
    WITH FORMAT;
GO
```

### <a name="b-backing-up-the-database-and-log"></a><a name="backing_up_db_and_log"></a> B. Sauvegarde de la base de données et du journal

L'exemple suivant sauvegarde l'exemple de base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] qui utilise par défaut le mode de récupération simple. Pour prendre en charge les sauvegardes de fichier journal, la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] est modifiée pour utiliser le mode de récupération complète.

Ensuite, l’exemple utilise [sp_addumpdevice](../../relational-databases/system-stored-procedures/sp-addumpdevice-transact-sql.md) pour créer une [unité de sauvegarde](../../relational-databases/backup-restore/backup-devices-sql-server.md) logique où sauvegarder les données (`AdvWorksData`), puis crée une autre unité de sauvegarde logique où sauvegarder le journal (`AdvWorksLog`).

Enfin, l'exemple crée une sauvegarde complète de la base de données dans `AdvWorksData` et, après la mise à jour, sauvegarde le journal dans `AdvWorksLog`.

```sql
-- To permit log backups, before the full database backup, modify the database
-- to use the full recovery model.
USE master;
GO
ALTER DATABASE AdventureWorks2012
    SET RECOVERY FULL;
GO
-- Create AdvWorksData and AdvWorksLog logical backup devices.
USE master
GO
EXEC sp_addumpdevice 'disk', 'AdvWorksData',
'Z:\SQLServerBackups\AdvWorksData.bak';
GO
EXEC sp_addumpdevice 'disk', 'AdvWorksLog',
'X:\SQLServerBackups\AdvWorksLog.bak';
GO

-- Back up the full AdventureWorks2012 database.
BACKUP DATABASE AdventureWorks2012 TO AdvWorksData;
GO
-- Back up the AdventureWorks2012 log.
BACKUP LOG AdventureWorks2012
    TO AdvWorksLog;
GO
```

> [!NOTE]
> Pour une base de données de production, sauvegardez régulièrement le journal. Les sauvegardes du journal doivent être suffisamment fréquentes pour assurer une protection contre la perte des données.

### <a name="c-creating-a-full-file-backup-of-the-secondary-filegroups"></a><a name="full_file_backup"></a> C. Création d'une sauvegarde de fichiers complète des groupes de fichiers secondaires

L'exemple suivant crée une sauvegarde complète de tous les fichiers se trouvant dans les deux groupes de fichiers secondaires.

```sql
--Back up the files in SalesGroup1:
BACKUP DATABASE Sales
    FILEGROUP = 'SalesGroup1',
    FILEGROUP = 'SalesGroup2'
    TO DISK = 'Z:\SQLServerBackups\SalesFiles.bck';
GO
```

### <a name="d-creating-a-differential-file-backup-of-the-secondary-filegroups"></a><a name="differential_file_backup"></a> D. Création d'une sauvegarde de fichiers différentielle des groupes de fichiers secondaires

L'exemple suivant crée une sauvegarde différentielle de tous les fichiers se trouvant dans les deux groupes de fichiers secondaires.

```sql
--Back up the files in SalesGroup1:
BACKUP DATABASE Sales
    FILEGROUP = 'SalesGroup1',
    FILEGROUP = 'SalesGroup2'
    TO DISK = 'Z:\SQLServerBackups\SalesFiles.bck'
    WITH
      DIFFERENTIAL;
GO
```

### <a name="e-creating-and-backing-up-to-a-single-family-mirrored-media-set"></a><a name="create_single_family_mirrored_media_set"></a> E. Création et sauvegarde dans un support de sauvegarde miroir d'une seule famille

L'exemple ci-dessous illustre la création d'un support de sauvegarde miroir contenant une seule famille de supports et quatre miroirs, ainsi que la sauvegarde de la base de données [!INCLUDE[ssSampleDBUserInputNonLocal](../../includes/sssampledbuserinputnonlocal-md.md)] dans ces derniers.

```sql
BACKUP DATABASE AdventureWorks2012
TO TAPE = '\\.\tape0'
MIRROR TO TAPE = '\\.\tape1'
MIRROR TO TAPE = '\\.\tape2'
MIRROR TO TAPE = '\\.\tape3'
WITH
    FORMAT,
    MEDIANAME = 'AdventureWorksSet0';
```

### <a name="f-creating-and-backing-up-to-a-multifamily-mirrored-media-set"></a><a name="create_multifamily_mirrored_media_set"></a> F. Création et sauvegarde dans un support de sauvegarde miroir de plusieurs familles

L'exemple suivant illustre la création d'un support de sauvegarde miroir dans lequel chaque miroir contient deux familles de supports. La base de données [!INCLUDE[ssSampleDBUserInputNonLocal](../../includes/sssampledbuserinputnonlocal-md.md)] est sauvegardée sur les deux miroirs.

```sql
BACKUP DATABASE AdventureWorks2012
TO TAPE = '\\.\tape0', TAPE = '\\.\tape1'
MIRROR TO TAPE = '\\.\tape2', TAPE = '\\.\tape3'
WITH
    FORMAT,
    MEDIANAME = 'AdventureWorksSet1';
```

### <a name="g-backing-up-to-an-existing-mirrored-media-set"></a><a name="existing_mirrored_media_set"></a> G. Sauvegarde dans un support de sauvegarde miroir existant

L'exemple suivant illustre l'ajout d'un jeu de sauvegarde au support de sauvegarde créé dans l'exemple précédent.

```sql
BACKUP LOG AdventureWorks2012
TO TAPE = '\\.\tape0', TAPE = '\\.\tape1'
MIRROR TO TAPE = '\\.\tape2', TAPE = '\\.\tape3'
WITH
    NOINIT,
    MEDIANAME = 'AdventureWorksSet1';
```

> [!NOTE]
> NOINIT, par défaut, est indiqué ici pour plus de clarté.

### <a name="h-creating-a-compressed-backup-in-a-new-media-set"></a><a name="creating_compressed_backup_new_media_set"></a> H. Création d'une sauvegarde compressée dans un nouveau support de sauvegarde

L'exemple suivant illustre le formatage du support avec la création d'un nouveau jeu de médias et une sauvegarde complète compressée de la base de données [!INCLUDE[ssSampleDBUserInputNonLocal](../../includes/sssampledbuserinputnonlocal-md.md)].

```sql
BACKUP DATABASE AdventureWorks2012 TO DISK='Z:\SQLServerBackups\AdvWorksData.bak'
WITH
    FORMAT,
    COMPRESSION;
```

### <a name="i-backing-up-to-the-microsoft-azure-blob-storage-service"></a><a name="url"></a> I. Sauvegarde du service de stockage d’objets blob Microsoft Azure

L’exemple effectue une sauvegarde complète de la base de données `Sales` vers le service Microsoft Azure Storage Blob. Le nom du compte de stockage est `mystorageaccount`. Le conteneur se nomme `myfirstcontainer`. Une stratégie d’accès stockée a été créée avec des droits de lecture, écriture, suppression et liste. Les informations d’identification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (`https://mystorageaccount.blob.core.windows.net/myfirstcontainer`) ont été créées à l’aide d’une signature d’accès partagé associée à la stratégie d’accès stockée. Pour plus d’informations sur la sauvegarde [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans le service de stockage Blob Microsoft Azure, consultez [Sauvegarde et restauration SQL Server avec le service de stockage Blob Microsoft Azure](../../relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service.md) et [Sauvegarde SQL Server vers une URL](../../relational-databases/backup-restore/sql-server-backup-to-url.md).

```sql
BACKUP DATABASE Sales
TO URL = 'https://mystorageaccount.blob.core.windows.net/myfirstcontainer/Sales_20160726.bak'
WITH STATS = 5;
```

### <a name="j-track-the-progress-of-backup-statement"></a><a name="backup_progress"></a> J. Suivi de la progression de l’instruction de sauvegarde

La requête suivante retourne des informations sur les instructions de sauvegarde en cours d’exécution :
```sql
SELECT query = a.text, start_time, percent_complete,
    eta = dateadd(second,estimated_completion_time/1000, getdate())
FROM sys.dm_exec_requests r
    CROSS APPLY sys.dm_exec_sql_text(r.sql_handle) a
WHERE r.command LIKE 'BACKUP%'
```

## <a name="see-also"></a>Voir aussi

- [Unités de sauvegarde](../../relational-databases/backup-restore/backup-devices-sql-server.md)
- [Jeux de supports, familles de supports et jeux de sauvegarde](../../relational-databases/backup-restore/media-sets-media-families-and-backup-sets-sql-server.md)
- [Sauvegardes de la fin du journal](../../relational-databases/backup-restore/tail-log-backups-sql-server.md)
- [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql.md)
- [DBCC SQLPERF](../../t-sql/database-console-commands/dbcc-sqlperf-transact-sql.md)
- [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md)
- [RESTORE FILELISTONLY](../../t-sql/statements/restore-statements-filelistonly-transact-sql.md)
- [RESTORE HEADERONLY](../../t-sql/statements/restore-statements-headeronly-transact-sql.md)
- [RESTORE LABELONLY](../../t-sql/statements/restore-statements-labelonly-transact-sql.md)
- [RESTORE VERIFYONLY](../../t-sql/statements/restore-statements-verifyonly-transact-sql.md)
- [sp_addumpdevice](../../relational-databases/system-stored-procedures/sp-addumpdevice-transact-sql.md)
- [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)
- [sp_helpfile](../../relational-databases/system-stored-procedures/sp-helpfile-transact-sql.md)
- [sp_helpfilegroup](../../relational-databases/system-stored-procedures/sp-helpfilegroup-transact-sql.md)
- [Options de configuration de serveur](../../database-engine/configure-windows/server-configuration-options-sql-server.md)
- [Restauration fragmentaire de bases de données avec des tables à mémoire optimisée](../../relational-databases/in-memory-oltp/piecemeal-restore-of-databases-with-memory-optimized-tables.md)

::: moniker-end
::: moniker range="=azuresqldb-mi-current"

:::row:::
    :::column:::
        [SQL Server](backup-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* SQL Managed Instance \*_** &nbsp;
    :::column-end:::
    :::column:::
        [Analytics Platform<br />System (PDW)](backup-transact-sql.md?view=aps-pdw-2016&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="azure-sql-managed-instance"></a>Azure SQL Managed Instance

Sauvegarde une base de données SQL placée/hébergée dans Azure SQL Managed Instance. SQL [Managed Instance](/azure/sql-database/sql-database-managed-instance) dispose de sauvegardes automatiques et permet aux utilisateurs de créer des sauvegardes complètes `COPY_ONLY` des bases de données. Les sauvegardes différentielles, de journaux et d’instantanés de fichiers ne sont pas prises en charge.

## <a name="syntax"></a>Syntaxe

```syntaxsql
BACKUP DATABASE { database_name | @database_name_var }
  TO URL = { 'physical_device_name' | @physical_device_name_var }[ ,...n ]
  WITH COPY_ONLY [, { <general_WITH_options> } ]
[;]

<general_WITH_options> [ ,...n ]::=

--Media Set Options
   MEDIADESCRIPTION = { 'text' | @text_variable }
 | MEDIANAME = { media_name | @media_name_variable }
 | BLOCKSIZE = { blocksize | @blocksize_variable }

--Data Transfer Options
   BUFFERCOUNT = { buffercount | @buffercount_variable }
 | MAXTRANSFERSIZE = { maxtransfersize | @maxtransfersize_variable }

--Error Management Options
   { NO_CHECKSUM | CHECKSUM }
 | { STOP_ON_ERROR | CONTINUE_AFTER_ERROR }

--Compatibility Options
   RESTART

--Monitoring Options
   STATS [ = percentage ]

--Encryption Options
 ENCRYPTION (ALGORITHM = { AES_128 | AES_192 | AES_256 | TRIPLE_DES_3KEY } , encryptor_options ) <encryptor_options> ::=
   SERVER CERTIFICATE = Encryptor_Name | SERVER ASYMMETRIC KEY = Encryptor_Name
```

## <a name="arguments"></a>Arguments

DATABASE Spécifie une sauvegarde complète de la base de données. Au cours d’une sauvegarde de base de données, Azure SQL Managed Instance sauvegarde une portion suffisante du journal des transactions afin d’assurer la cohérence de la base de données lors de la restauration de la sauvegarde.

> [!IMPORTANT]
> Une sauvegarde de base de données créée sur une instance managée ne peut être restaurée que sur une autre instance Azure SQL Managed Instance. Elle ne peut pas être restaurée sur une instance locale de SQL Server (de même qu’une sauvegarde d’une base de données SQL Server 2016 ne peut pas être restaurée sur une instance de SQL Server 2012).

Lorsque vous restaurez une sauvegarde créée par BACKUP DATABASE (une *sauvegarde de données*), l’ensemble de la sauvegarde est restauré. Pour effectuer une restauration à partir de sauvegardes automatiques SQL Managed Instance, consultez [Restaurer une base de données sur une instance managée](/azure/sql-database/sql-database-managed-instance-get-started-restore).

{ *database_name* |  **@** _database\_name\_var_ } Spécifie la base de données à partir de laquelle la base de données complète est sauvegardée. S’il est fourni comme variable ( **@** _database\_name\_var_), ce nom peut être spécifié comme constante de chaîne ( **@** _database\_name\_var_ **=** _database name_) ou comme variable de type de données chaîne de caractères, sauf pour les types de données **ntext** ou **text**.

Pour plus d’informations, consultez les articles [Sauvegardes de fichiers complètes](../../relational-databases/backup-restore/full-file-backups-sql-server.md) et [Sauvegarder des fichiers et des groupes de fichiers](../../relational-databases/backup-restore/back-up-files-and-filegroups-sql-server.md).

TO URL

Spécifie l’URL à utiliser pour l’opération de sauvegarde. Le format d’URL est utilisé pour créer des sauvegardes dans le service de stockage Microsoft Azure.

> [!IMPORTANT]
> Pour sauvegarder vers plusieurs unités quand l’opération s’effectue vers une URL, vous devez utiliser des jetons de signature d’accès partagé (SAP). Pour obtenir des exemples de signatures d’accès partagé, consultez [Sauvegarde SQL Server vers une URL](../../relational-databases/backup-restore/sql-server-backup-to-url.md) et [Simplifying creation of SQL Credentials with Shared Access Signature (SAS) tokens on Azure Storage with PowerShell](/archive/blogs/sqlcat/simplifying-creation-of-sql-credentials-with-shared-access-signature-sas-tokens-on-azure-storage-with-powershell).

*n* Correspond à un espace réservé indiquant qu’il est possible de spécifier jusqu’à 64 unités de sauvegarde dans une liste séparée par des virgules.

### <a name="with-optionsspecifies-options-to-be-used-with-a-backup-operation"></a>Options WITH Spécifie les options à utiliser avec une opération de sauvegarde.

CREDENTIAL S’utilise uniquement lors de la création d’une sauvegarde dans le service Stockage Blob Microsoft Azure.

ENCRYPTION Utilisé pour spécifier le chiffrement d’une sauvegarde. Vous pouvez spécifier un algorithme de chiffrement pour chiffrer la sauvegarde ou spécifier `NO_ENCRYPTION` pour ne pas chiffrer la sauvegarde. Il est recommandé d'utiliser le chiffrement pour sécuriser les fichiers de sauvegarde. Liste des algorithmes possibles :

- `AES_128`
- `AES_192`
- `AES_256`
- `TRIPLE_DES_3KEY`
- `NO_ENCRYPTION`

Si vous choisissez de chiffrer, vous devez également spécifier le chiffreur à l'aide des options de chiffreur :

- `SERVER CERTIFICATE = <Encryptor_Name>`
- `SERVER ASYMMETRIC KEY = <Encryptor_Name>`

**Options du jeu de sauvegarde**

COPY_ONLY spécifie que la sauvegarde est une *sauvegarde en copie seule* qui n’a aucun impact sur la séquence normale des sauvegardes. Une sauvegarde en copie seule est créée indépendamment des sauvegardes automatiques Azure SQL Database. Pour plus d’informations, consultez l’article [Sauvegardes de type copie seule](../../relational-databases/backup-restore/copy-only-backups-sql-server.md).

{ COMPRESSION | NO_COMPRESSION } Spécifie si la [compression des sauvegardes](../../relational-databases/backup-restore/backup-compression-sql-server.md) est effectuée sur cette sauvegarde, remplaçant la valeur par défaut au niveau du serveur.

Le comportement par défaut exclut toute compression des sauvegardes. Toutefois, ce paramétrage par défaut peut être modifié en définissant l’option de configuration du serveur [Compression par défaut des sauvegardes](../../database-engine/configure-windows/view-or-configure-the-backup-compression-default-server-configuration-option.md). Pour plus d’informations sur l’affichage de la valeur actuelle de cette option, consultez l’article [Afficher ou modifier des propriétés de serveur](../../database-engine/configure-windows/view-or-change-server-properties-sql-server.md).

COMPRESSION Active explicitement la compression des sauvegardes.

NO_COMPRESSION Désactive explicitement la compression des sauvegardes.

DESCRIPTION **=** { **'** _text_ **'**  |  **@** _text\_variable_ } Spécifie le texte en forme libre servant à décrire le jeu de sauvegarde. La chaîne peut compter jusqu'à 255 caractères.

NAME **=** { *backup_set_name* |  **@** _backup\_set\_var_ } Spécifie le nom du jeu de sauvegarde. Les noms peuvent contenir jusqu'à 128 caractères. Si l'option NAME n'est pas spécifiée, le nom reste vide.

MEDIADESCRIPTION **=** { *text* | **@** _text\_variable_ } Indique le texte descriptif en forme libre (255 caractères maximum) du support de sauvegarde.

MEDIANAME **=** { *media_name* |  **@** _media\_name\_variable_ } Spécifie le nom du support de sauvegarde complet. Le nom du support ne doit pas dépasser 128 caractères. Si `MEDIANAME` est spécifié, il doit correspondre au nom spécifié précédemment existant sur les volumes de sauvegarde. Si elle n'est pas spécifiée, ou si l'option SKIP l'est, aucune vérification du nom de support n'est effectuée.

BLOCKSIZE **=** { *blocksize* |  **@** _blocksize\_variable_ } Indique, en octets, la taille de bloc physique. Les tailles prises en charge sont 512, 1024, 2048, 4096, 8192, 16384, 32768 et 65536 (64 Ko) octets. La valeur par défaut est 65536 pour les périphériques à bandes, 512 sinon. En règle générale, cette option est superflue car BACKUP sélectionne automatiquement une taille de bloc appropriée pour le périphérique. Si vous spécifiez explicitement une taille de bloc, la sélection automatique est annulée et remplacée.

**Options de transfert de données**

BUFFERCOUNT **=** { *buffercount* |  **@** _buffercount\_variable_ } Spécifie le nombre total de tampons d’E/S à utiliser pour l’opération de sauvegarde. Vous pouvez spécifier n'importe quel entier positif ; toutefois, un nombre élevé de tampons peut provoquer des erreurs liées à une insuffisance de mémoire. En effet, l'espace d'adressage virtuel peut s'avérer inapproprié dans la tâche Sqlservr.exe.

L’espace total utilisé par les mémoires tampons est déterminé par : `BUFFERCOUNT * MAXTRANSFERSIZE`.

> [!NOTE]
> Pour des informations importantes sur l’utilisation de l’option `BUFFERCOUNT`, consultez le blog [Incorrect BufferCount data transfer option can lead to OOM condition](/archive/blogs/sqlserverfaq/incorrect-buffercount-data-transfer-option-can-lead-to-oom-condition).

MAXTRANSFERSIZE **=** { *maxtransfersize* | _**@** maxtransfersize\_variable_ } Spécifie la plus grande unité de transfert, en octets, à utiliser entre [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et le support de sauvegarde. Les valeurs possibles sont les multiples de 65536 octets (64 Ko), dans la limite de 4194304 octets (4 Mo).

> [!NOTE]
> Pour les bases de données comprenant un seul fichier de données et où [Transparent Data Encryption (TDE)](../../relational-databases/security/encryption/transparent-data-encryption.md) est activé, la valeur `MAXTRANSFERSIZE` par défaut est 65 536 (64 Ko). Pour les bases de données non chiffrées à l’aide de TDE, la valeur `MAXTRANSFERSIZE` par défaut est 1 048 576 (1 Mo) lors d’une sauvegarde vers DISK, et 65 536 (64 Ko) lors d’une sauvegarde vers VDI ou TAPE.

**Options de gestion des erreurs**

Ces options vous permettent de déterminer si les sommes de contrôle de sauvegarde sont activées pour l’opération de sauvegarde, et si l’opération doit s’arrêter en présence d’une erreur.

{ **NO_CHECKSUM** | CHECKSUM } Détermine si les sommes de contrôle de sauvegarde sont activées.

NO_CHECKSUM Désactive explicitement la génération de sommes de contrôle de sauvegarde (et la validation des sommes de contrôle de page). Il s'agit du comportement par défaut.

CHECKSUM Indique que l’opération de sauvegarde vérifie dans chaque page les informations de somme de contrôle et de page endommagée, si elles sont activées et disponibles, et génère une somme de contrôle pour l’ensemble de la sauvegarde.

L'utilisation des sommes de contrôle de sauvegarde peut affecter la charge de travail et le débit de sauvegarde.

Pour plus d’informations, consultez l’article [Erreurs de support possibles pendant les opérations de sauvegarde et de restauration](../../relational-databases/backup-restore/possible-media-errors-during-backup-and-restore-sql-server.md).

{ **STOP_ON_ERROR** | CONTINUE_AFTER_ERROR } Détermine si une opération de sauvegarde s’arrête ou continue après avoir rencontré une erreur de somme de contrôle de page.

STOP_ON_ERROR Ordonne à BACKUP de s’arrêter si une somme de contrôle de page n’est pas validée. Il s'agit du comportement par défaut.

CONTINUE_AFTER_ERROR Ordonne à BACKUP de continuer en dépit des erreurs rencontrées, telles que des sommes de contrôle de page non valides ou des pages endommagées.

Si vous ne parvenez pas à effectuer une sauvegarde de la fin du journal à l’aide de l’option NO_TRUNCATE lorsque la base de données est endommagée, vous pouvez essayer d’effectuer une [sauvegarde de la fin du journal](../../relational-databases/backup-restore/tail-log-backups-sql-server.md) en spécifiant CONTINUE_AFTER_ERROR au lieu de NO_TRUNCATE.

Pour plus d’informations, consultez l’article [Erreurs de support possibles pendant les opérations de sauvegarde et de restauration](../../relational-databases/backup-restore/possible-media-errors-during-backup-and-restore-sql-server.md).

**Options de compatibilité**

RESTART Cette option n’a aucun effet. Elle est acceptée par cette version à des fins de compatibilité avec les versions antérieures de SQL Server.

**Options de surveillance**

STATS [ **=** _percentage_ ] Affiche un message chaque fois qu’un autre élément *percentage* se termine, et sert à évaluer l’état d’avancement de l’opération. Si *percentage* est omis, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] affiche un message à chaque incrément de 10 pour cent.

L'option STATS signale le pourcentage terminé comme seuil de rapport de l'intervalle suivant. C'est-à-dire approximativement le pourcentage spécifié ; par exemple, si STATS=10, et si le pourcentage terminé est 40 pour cent, l'option peut afficher 43 pour cent. Pour les jeux de sauvegardes volumineux, cela n'est pas un problème car le pourcentage terminé varie très lentement entre les appels d'E/S terminés.

## <a name="limitations-for-sql-managed-instance"></a>Limitations pour SQL Managed Instance

La taille maximale d’une bande de sauvegarde est de 195 Go (taille maximale de l’objet blob). Augmentez le nombre de bandes défini dans la commande de sauvegarde pour réduire la taille de chaque bande et ainsi ne pas dépasser cette limite.

## <a name="security"></a>Sécurité

### <a name="permissions"></a>Autorisations

Les autorisations BACKUP DATABASE reviennent par défaut aux membres du rôle serveur fixe **sysadmin** et des rôles de base de données fixes **db_owner** et **db_backupoperator**.

Des problèmes de propriété et d’autorisations sur l’URL sont susceptibles de perturber une opération de sauvegarde. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doit être en mesure de lire et d'écrire sur l'unité ; le compte sous lequel le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] s'exécute doit avoir des autorisations d'écriture.

## <a name="examples"></a><a name="examples"></a> Exemples

L’exemple effectue une sauvegarde COPY_ONLY de `Sales` vers le service de stockage Blob Microsoft Azure. Le nom du compte de stockage est `mystorageaccount`. Le conteneur se nomme `myfirstcontainer`. Une stratégie d’accès stockée a été créée avec des droits de lecture, écriture, suppression et liste. Les informations d’identification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (`https://mystorageaccount.blob.core.windows.net/myfirstcontainer`) ont été créées à l’aide d’une signature d’accès partagé associée à la stratégie d’accès stockée. Pour plus d’informations sur la sauvegarde [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans le service de stockage Blob Microsoft Azure, consultez [Sauvegarde et restauration SQL Server avec le service de stockage Blob Microsoft Azure](../../relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service.md) et [Sauvegarde SQL Server vers une URL](../../relational-databases/backup-restore/sql-server-backup-to-url.md).

```sql
BACKUP DATABASE Sales
TO URL = 'https://mystorageaccount.blob.core.windows.net/myfirstcontainer/Sales_20160726.bak'
WITH STATS = 5, COPY_ONLY;
```

## <a name="see-also"></a>Voir aussi

[Restaurer la base de données](restore-statements-transact-sql.md)

::: moniker-end
::: moniker range=">=aps-pdw-2016"

:::row:::
    :::column:::
        [SQL Server](backup-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        [SQL Managed Instance](backup-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* Analytics<br />Platform System (PDW) \*_** &nbsp;
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="analytics-platform-system"></a>Système de la plateforme d'analyse

Crée une sauvegarde d’une base de données [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] et stocke la sauvegarde de l’appliance dans un emplacement réseau spécifié par l’utilisateur. Utilisez cette instruction avec [RESTORE DATABASE - Analytics Platform System](../../t-sql/statements/restore-statements-transact-sql.md) pour la récupération d’urgence, ou pour copier une base de données d’une appliance vers une autre.

**Avant de commencer**, lisez la section relative à l’acquisition et à la configuration d’un serveur de sauvegarde dans la [!INCLUDE[pdw-product-documentation](../../includes/pdw-product-documentation-md.md)].

Deux types de sauvegardes sont possibles dans [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]. Une *sauvegarde complète de base de données* correspond à la sauvegarde de l’intégralité d’une base de données [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]. Une *sauvegarde différentielle de base de données* contient uniquement les modifications effectuées depuis la dernière sauvegarde complète. Une sauvegarde de base de données utilisateur comprend les utilisateurs de la base de données, ainsi que ses rôles. La sauvegarde de la base de données MASTER comprend les connexions.

Pour plus d’informations sur les sauvegardes de bases de données [!INCLUDE[ssPDW](../../includes/sspdw-md.md)], consultez la section relative à la sauvegarde et à la restauration dans la [!INCLUDE[pdw-product-documentation](../../includes/pdw-product-documentation-md.md)].

## <a name="syntax"></a>Syntaxe

```syntaxsql
--Create a full backup of a user database or the master database.
BACKUP DATABASE database_name
    TO DISK = '\\UNC_path\backup_directory'
    [ WITH [ ( ]<with_options> [ ,...n ][ ) ] ]
[;]

--Create a differential backup of a user database.
BACKUP DATABASE database_name
    TO DISK = '\\UNC_path\backup_directory'
    WITH [ ( ] DIFFERENTIAL
    [ , <with_options> [ ,...n ] [ ) ]
[;]

<with_options> ::=
    DESCRIPTION = 'text'
    | NAME = 'backup_name'
```

## <a name="arguments"></a>Arguments

*database_name* Nom de la base de données pour laquelle créer une sauvegarde. Il peut s’agir de la base de données MASTER ou d’une base de données utilisateur.

TO DISK = '\\\\*UNC_path*\\*backup_directory*' Chemin réseau et répertoire dans lesquels [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] doit écrire les fichiers de sauvegarde. Par exemple, « \\\xxx.xxx.xxx.xxx\backups\2012\Monthly\08.2012.Mybackup ».

- Le chemin où se trouve le nom du répertoire de sauvegarde doit déjà exister et être spécifié comme un chemin UNC complet.
- Le répertoire de sauvegarde (*backup_directory*) ne doit pas exister avant l’exécution de la commande de sauvegarde. [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] crée le répertoire de sauvegarde.
- Le chemin du répertoire de sauvegarde ne peut pas être un chemin local ni un emplacement sur un nœud d’appliance [!INCLUDE[ssPDW](../../includes/sspdw-md.md)].
- La longueur maximale du chemin UNC et du nom du répertoire de sauvegarde est de 200 caractères.
- Le serveur ou l’hôte doivent être spécifiés comme une adresse IP. Vous ne pouvez pas le spécifier comme le nom de l’hôte ou du serveur.

DESCRIPTION = **'** _text_ **'** Spécifie une description textuelle de la sauvegarde. La longueur maximale du texte est de 255 caractères.

La description est stockée dans les métadonnées et s’affiche lorsque l’en-tête de sauvegarde est restauré avec RESTORE HEADERONLY.

NAME = **'** _backup \_name_ **'** Spécifie le nom de la sauvegarde. Le nom de la sauvegarde peut être différent de celui de la base de données.

- Les noms peuvent contenir jusqu'à 128 caractères.
- Ne peut pas inclure un chemin.
- Doit commencer par une lettre, un numéro ou un trait de soulignement (_). Les caractères spéciaux autorisés sont le trait de soulignement (\_), le trait d’union (-) ou l’espace ( ). Les noms de sauvegarde ne peuvent pas se terminer par un espace.
- L’instruction échoue si *backup_name* existe déjà à l’emplacement spécifié.

Le nom est stocké dans les métadonnées et s’affiche lorsque l’en-tête de sauvegarde est restauré avec RESTORE HEADERONLY.

DIFFERENTIAL Spécifie l’exécution d’une sauvegarde différentielle pour une base de données utilisateur. Si vous ne spécifiez pas de valeur, une sauvegarde complète est exécutée par défaut. Le nom de la sauvegarde différentielle ne doit pas nécessairement être le même que celui de la sauvegarde complète. Pour effectuer le suivi de la sauvegarde différentielle et de la sauvegarde complète correspondante, il est conseillé d’utiliser le même nom suivi de « complète » et de « diff ».

Par exemple :

`BACKUP DATABASE Customer TO DISK = '\\xxx.xxx.xxx.xxx\backups\CustomerFull';`

`BACKUP DATABASE Customer TO DISK = '\\xxx.xxx.xxx.xxx\backups\CustomerDiff' WITH DIFFERENTIAL;`

## <a name="permissions"></a>Autorisations

Nécessite l’autorisation **BACKUP DATABASE** ou l’appartenance au rôle de base de données fixe **db_backupoperator**. La base de données MASTER peut uniquement être sauvegardée par un utilisateur standard ayant reçu le rôle de base de données fixe **db_backupoperator**. La base de données MASTER peut uniquement être sauvegardée par un **administrateur système**, un administrateur d’infrastructure ou un membre du rôle serveur fixe **sysadmin**.

Nécessite un compte Windows doté d’un droit d’accès, de création et d’écriture sur le répertoire de sauvegarde. Vous devez aussi stocker le nom de compte et le mot de passe Windows dans [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]. Pour ajouter ces informations d’identification réseau à [!INCLUDE[ssPDW](../../includes/sspdw-md.md)], utilisez la procédure stockée [sp_pdw_add_network_credentials [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]](../../relational-databases/system-stored-procedures/sp-pdw-add-network-credentials-sql-data-warehouse.md).

Pour plus d’informations sur la gestion des informations d’identification dans [!INCLUDE[ssPDW](../../includes/sspdw-md.md)], consultez la section [Sécurité](#Security).

## <a name="error-handling"></a>Gestion des erreurs

Des erreurs BACKUP DATABASE se produisent dans les conditions suivantes :

- Les autorisations de l’utilisateur ne sont pas suffisantes pour effectuer une sauvegarde.
- [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] ne dispose pas des autorisations nécessaires pour accéder à l’emplacement réseau où est stockée la sauvegarde.
- La base de données n’existe pas.
- Le répertoire cible existe déjà sur le partage réseau.
- Le partage réseau cible n’est pas disponible.
- Le partage réseau cible n’a pas suffisamment d’espace pour la sauvegarde. La commande BACKUP DATABASE ne vérifie pas la présence d’un espace disque suffisant avant de lancer la sauvegarde, ce qui peut entraîner une erreur d’espace disque insuffisant lors de l’exécution de BACKUP DATABASE. Quand l’espace disque est insuffisant, [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] annule la commande BACKUP DATABASE. Pour réduire la taille de votre base de données, exécutez [DBCC SHRINKLOG ([!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)])](../../t-sql/database-console-commands/dbcc-shrinklog-azure-sql-data-warehouse.md)
- Un lancement de sauvegarde a été tenté dans une transaction.

::: moniker-end
::: moniker range=">=aps-pdw-2016||>=sql-server-2016||>=sql-server-linux-2017"
## <a name="general-remarks"></a>Remarques d'ordre général

Avant d’effectuer une sauvegarde de base de données, utilisez [DBCC SHRINKLOG ([!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)])](../../t-sql/database-console-commands/dbcc-shrinklog-azure-sql-data-warehouse.md) pour réduire la taille de votre base de données.

Une sauvegarde [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] est stockée sous la forme d’un ensemble de fichiers regroupés dans un même répertoire.

Généralement, une sauvegarde différentielle prend moins de temps qu’une sauvegarde complète et peut être effectuée plus souvent. Quand plusieurs sauvegardes différentielles sont basées sur une même sauvegarde complète, chaque sauvegarde différentielle inclut l’ensemble des modifications de la dernière sauvegarde différentielle.

Si vous annulez une commande BACKUP, [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] supprime le répertoire cible et tous les fichiers créés pour la sauvegarde. Si [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] perd la connexion au partage réseau, l’annulation ne peut pas être effectuée.

Les sauvegardes complètes et les sauvegardes différentielles sont stockées dans des répertoires différents. Les conventions de nommage ne sont pas appliquées lorsque vous associez une sauvegarde complète et une sauvegarde différentielle. Vous pouvez effectuer ce suivi à l’aide de vos propres conventions de nommage. Vous pouvez également effectuer ce suivi à l’aide de l’option WITH DESCRIPTION pour ajouter une description, puis à l’aide de l’instruction RESTORE HEADERONLY pour récupérer la description.

::: moniker-end
::: moniker range=">=aps-pdw-2016"
## <a name="limitations-and-restrictions"></a>Limitations et restrictions

Vous ne pouvez pas effectuer de sauvegarde différentielle pour la base de données MASTER. Seules les sauvegardes complètes sont possibles avec les bases de données MASTER.

Les fichiers de sauvegarde sont stockés dans un format uniquement adapté à la restauration de la sauvegarde vers une appliance [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] à l’aide de l’instruction [RESTORE DATABASE - Analytics Platform System](../../t-sql/statements/restore-statements-transact-sql.md).

La sauvegarde réalisée à l’aide de l’instruction BACKUP DATABASE ne peut pas être utilisée pour transférer des données ou des informations utilisateur vers des bases de données SMP [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour cela, vous pouvez utiliser la fonctionnalité de copie de la table distante. Pour plus d’informations, consultez la section relative à la copie de données à partir d’une table distante dans la [!INCLUDE[pdw-product-documentation](../../includes/pdw-product-documentation-md.md)].

[!INCLUDE[ssPDW](../../includes/sspdw-md.md)] utilise la technologie de sauvegarde [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour sauvegarder et restaurer des bases de données. Les options de sauvegarde [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sont préconfigurées pour utiliser la compression de sauvegarde. Vous ne pouvez pas définir des options de sauvegarde comme la compression, la somme de contrôle, la taille des blocs ou le nombre de tampons.

Vous ne pouvez exécuter qu’une seule sauvegarde ou restauration de base de données à la fois sur une même appliance. [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] met en file d’attente les commandes BACKUP et RESTORE tant que la commande BACKUP ou RESTORE en cours n’est pas terminée.

L’appliance cible pour la restauration de la sauvegarde doit comprendre au moins autant de nœuds de calcul que l’appliance source. L’appliance cible peut avoir plus de nœuds de calcul que l’appliance source, mais pas moins.

[!INCLUDE[ssPDW](../../includes/sspdw-md.md)] n’effectue pas le suivi de l’emplacement et des noms des sauvegardes, puisque celles-ci ne sont pas stockées sur l’appliance.

[!INCLUDE[ssPDW](../../includes/sspdw-md.md)] effectue le suivi des sauvegardes de base de données ayant réussi et ayant échoué.

Une sauvegarde différentielle est uniquement autorisée si la dernière sauvegarde complète a réussi. Par exemple, supposons que lundi vous ayez créé une sauvegarde complète de la base de données Ventes et que la sauvegarde ait réussi. Supposons ensuite que mardi, vous ayez créé une sauvegarde complète de la base de données Ventes et que celle-ci ait échoué. Après cet échec, vous ne pouvez pas créer une sauvegarde différentielle basée sur la sauvegarde complète de lundi. Pour créer une sauvegarde différentielle, une sauvegarde complète réussie est nécessaire.

## <a name="metadata"></a>Métadonnées

Ces vues de gestion dynamique contiennent des informations sur toutes les opérations de sauvegarde, de restauration et de chargement. Ces informations sont conservées après le redémarrage du système.

- [sys.pdw_loader_backup_runs](../../relational-databases/system-catalog-views/sys-pdw-loader-backup-runs-transact-sql.md)
- [sys.pdw_loader_backup_run_details](../../relational-databases/system-catalog-views/sys-pdw-loader-backup-run-details-transact-sql.md)
- [sys.pdw_loader_run_stages](../../relational-databases/system-catalog-views/sys-pdw-loader-run-stages-transact-sql.md)

## <a name="performance"></a>Performances

Pour effectuer une sauvegarde, [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] sauvegarde d’abord les métadonnées, puis effectue une sauvegarde parallèle des données de base de données qui sont stockées dans les nœuds de calcul. Les données sont copiées de chaque nœud de calcul vers le répertoire de sauvegarde. Pour obtenir de meilleures performances quand vous déplacez des données des nœuds de calcul vers le répertoire de sauvegarde, [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] permet de contrôler le nombre de nœuds de calcul qui copient des données simultanément.

## <a name="locking"></a>Verrouillage

Applique un verrou ExclusiveUpdate sur l’objet DATABASE.

## <a name="security"></a><a name="Security"></a> Sécurité

Les sauvegardes [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] ne sont pas stockées sur l’appliance. Il revient donc à votre équipe informatique de gérer tous les aspects liés à la sécurité de la sauvegarde. Cela comprend, par exemple, la gestion de la sécurité des données de sauvegarde, de la sécurité du serveur utilisé pour stocker les sauvegardes et de la sécurité de l’infrastructure réseau qui connecte le serveur de sauvegarde à l’appliance [!INCLUDE[ssPDW](../../includes/sspdw-md.md)].

**Gérer les informations d’identification réseau**

L’accès réseau au répertoire de sauvegarde est basé sur la sécurité standard des partages de fichiers du système d’exploitation. Avant d’effectuer une sauvegarde, vous devez créer ou désigner un compte Windows qui sera utilisé pour l’authentification de [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] auprès du répertoire de sauvegarde. Ce compte Windows doit être doté d’un droit d’accès, de création et d’écriture sur le répertoire de sauvegarde.

> [!IMPORTANT]
> Pour réduire les risques de sécurité liés à vos données, il vous est conseillé de désigner un compte Windows qui servira uniquement aux opérations de sauvegarde et de restauration. Autorisez ce compte à accéder à l’emplacement de sauvegarde et à aucun autre emplacement.

Vous devez stocker le nom d’utilisateur et le mot de passe dans [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] en exécutant la procédure stockée [ sp_pdw_add_network_credentials[!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]](../../relational-databases/system-stored-procedures/sp-pdw-add-network-credentials-sql-data-warehouse.md). [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] utilise le Gestionnaire d’informations d’identification Windows pour stocker et chiffrer les noms d’utilisateur et les mots de passe sur le nœud de contrôle et les nœuds de calcul. Les informations d’identification ne sont pas sauvegardées avec la commande BACKUP DATABASE.

Pour supprimer les informations d’identification réseau de [!INCLUDE[ssPDW](../../includes/sspdw-md.md)], consultez [sp_pdw_remove_network_credentials[!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]](../../relational-databases/system-stored-procedures/sp-pdw-remove-network-credentials-sql-data-warehouse.md).

Pour répertorier toutes les informations d’identification réseau stockées dans [!INCLUDE[ssPDW](../../includes/sspdw-md.md)], utilisez la vue de gestion dynamique [sys.dm_pdw_network_credentials](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-network-credentials-transact-sql.md).

## <a name="examples"></a>Exemples

### <a name="a-add-network-credentials-for-the-backup-location"></a>R. Ajouter des informations d’identification réseau pour l’emplacement de sauvegarde

Pour créer une sauvegarde, [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] doit disposer d’autorisations de lecture/écriture pour le répertoire de sauvegarde. L’exemple suivant montre comment ajouter les informations d’identification d’un utilisateur. [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] va stocker ces informations d’identification et les utiliser pour les opérations de sauvegarde et de restauration.

> [!IMPORTANT]
> Pour des raisons de sécurité, nous vous recommandons de créer un compte de domaine qui servira uniquement pour les sauvegardes.

```sql
EXEC sp_pdw_add_network_credentials 'xxx.xxx.xxx.xxx', 'domain1\backupuser', '*****';
```

### <a name="b-remove-network-credentials-for-the-backup-location"></a>B. Supprimer les informations d’identification réseau pour l’emplacement de sauvegarde

L’exemple suivant montre comment supprimer de [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] les informations d’identification d’un utilisateur de domaine.

```sql
EXEC sp_pdw_remove_network_credentials 'xxx.xxx.xxx.xxx';
```

### <a name="c-create-a-full-backup-of-a-user-database"></a>C. Créer une sauvegarde complète d’une base de données utilisateur

L’exemple suivant crée une sauvegarde complète de la base de données utilisateur Invoices. [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] crée le répertoire Invoices2013 et enregistre les fichiers de sauvegarde dans le répertoire \\\10.192.63.147\backups\yearly\Invoices2013Full.

```sql
BACKUP DATABASE Invoices TO DISK = '\\xxx.xxx.xxx.xxx\backups\yearly\Invoices2013Full';
```

### <a name="d-create-a-differential-backup-of-a-user-database"></a>D. Créer une sauvegarde différentielle d’une base de données utilisateur

L’exemple suivant crée une sauvegarde différentielle, qui inclut toutes les modifications apportées depuis la dernière sauvegarde complète de la base de données Invoices. [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] crée le répertoire \\\xxx.xxx.xxx.xxx\backups\yearly\Invoices2013Diff dans lequel il stocke les fichiers. La description « Invoices 2013 differential backup » est stockée avec les informations d’en-tête de la sauvegarde.

La sauvegarde différentielle ne peut être effectuée que si la dernière sauvegarde complète d’Invoices a réussi.

```sql
BACKUP DATABASE Invoices TO DISK = '\\xxx.xxx.xxx.xxx\backups\yearly\Invoices2013Diff'
    WITH DIFFERENTIAL,
    DESCRIPTION = 'Invoices 2013 differential backup';
```

### <a name="e-create-a-full-backup-of-the-master-database"></a>E. Créer une sauvegarde complète de la base de données MASTER

L’exemple suivant crée une sauvegarde complète de la base de données MASTER et la stocke dans le répertoire « \\\10.192.63.147\backups\2013\daily\20130722\master ».

```sql
BACKUP DATABASE master TO DISK = '\\xxx.xxx.xxx.xxx\backups\2013\daily\20130722\master';
```

### <a name="f-create-a-backup-of-appliance-login-information"></a>F. Créer une sauvegarde des informations de connexion de l’appliance

La base de données MASTER stocke les informations de connexion de l’appliance. Pour sauvegarder les informations de connexion de l’appliance, vous devez effectuer une sauvegarde de la base de données MASTER.

L’exemple suivant crée une sauvegarde complète de la base de données MASTER.

```sql
BACKUP DATABASE master TO DISK = '\\xxx.xxx.xxx.xxx\backups\2013\daily\20130722\master'
WITH (
    DESCRIPTION = 'Master Backup 20130722',
    NAME = 'login-backup'
)
;
```

## <a name="see-also"></a>Voir aussi

[RESTORE DATABASE - Parallel Data Warehouse](../../t-sql/statements/restore-statements-transact-sql.md)

::: moniker-end
