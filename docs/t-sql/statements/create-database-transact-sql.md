---
title: CREATE DATABASE (Transact-SQL) | Microsoft Docs
description: Syntaxe de création de base de données pour SQL Server, Azure SQL Database, Azure Synapse Analytics et Système de plateforme d’analyse
ms.custom: references_regions
ms.date: 12/11/2020
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DATABASE_TSQL
- DATABASE
- CONTAINMENT_TSQL
- CREATE DATABASE
- CREATE_DATABASE_TSQL
- CONTAINS_FILESTREAM_TSQL
- CONTAINS FILESTREAM
- CONTAINMENT
dev_langs:
- TSQL
helpviewer_keywords:
- snapshots [SQL Server database snapshots], creating
- databases [SQL Server], creating
- model database [SQL Server], database creation
- mounted drives [SQL Server]
- CREATE DATABASE
- CREATE DATABASE statement
- file creation [SQL Server]
- creating databases
- containment
- filegroups [SQL Server], database creation
- database creation [SQL Server], CREATE DATABASE statement
- moving databases
- attaching databases [SQL Server], CREATE DATABASE...FOR ATTACH
ms.assetid: 29ddac46-7a0f-4151-bd94-75c1908c89f8
author: markingmyname
ms.author: maghan
monikerRange: '>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-current||=azuresqldb-mi-current||=azure-sqldw-latest||>=aps-pdw-2016'
ms.openlocfilehash: 08ea5c807e6a8b6bb2d53286bf7a338ee9ba70d2
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97637285"
---
# <a name="create-database"></a>CREATE DATABASE

Crée une base de données.

Cliquez sur l’un des onglets suivants pour connaître la syntaxe, les arguments, les remarques, les autorisations et des exemples propres à la version de SQL que vous utilisez.

Pour plus d’informations sur les conventions de la syntaxe, consultez [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).

[!INCLUDE[select-product](../../includes/select-product.md)]

::: moniker range=">=sql-server-2016||>=sql-server-linux-2017"

:::row:::
    :::column:::
        **_\* SQL Server \*_** &nbsp;
    :::column-end:::
    :::column:::
        [Base de données SQL](create-database-transact-sql.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [SQL Managed Instance](create-database-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](create-database-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
    :::column:::
        [Analytics Platform<br />System (PDW)](create-database-transact-sql.md?view=aps-pdw-2016&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="sql-server"></a>SQL Server

## <a name="overview"></a>Vue d’ensemble

Dans SQL Server, cette instruction crée une nouvelle base de données et les fichiers utilisés avec leurs groupes de fichiers. Elle permet également de créer une capture instantanée de base de données et de joindre des fichiers de base de données pour créer une base de données à partir des fichiers détachés d’une autre base de données.

## <a name="syntax"></a>Syntaxe

Création d'une base de données

```syntaxsql
CREATE DATABASE database_name
[ CONTAINMENT = { NONE | PARTIAL } ]
[ ON
      [ PRIMARY ] <filespec> [ ,...n ]
      [ , <filegroup> [ ,...n ] ]
      [ LOG ON <filespec> [ ,...n ] ]
]
[ COLLATE collation_name ]
[ WITH <option> [,...n ] ]
[;]

<option> ::=
{
      FILESTREAM ( <filestream_option> [,...n ] )
    | DEFAULT_FULLTEXT_LANGUAGE = { lcid | language_name | language_alias }
    | DEFAULT_LANGUAGE = { lcid | language_name | language_alias }
    | NESTED_TRIGGERS = { OFF | ON }
    | TRANSFORM_NOISE_WORDS = { OFF | ON}
    | TWO_DIGIT_YEAR_CUTOFF = <two_digit_year_cutoff>
    | DB_CHAINING { OFF | ON }
    | TRUSTWORTHY { OFF | ON }
    | PERSISTENT_LOG_BUFFER=ON ( DIRECTORY_NAME='<Filepath to folder on DAX formatted volume>' )
}

<filestream_option> ::=
{
      NON_TRANSACTED_ACCESS = { OFF | READ_ONLY | FULL }
    | DIRECTORY_NAME = 'directory_name'
}

<filespec> ::=
{
(
    NAME = logical_file_name ,
    FILENAME = { 'os_file_name' | 'filestream_path' }
    [ , SIZE = size [ KB | MB | GB | TB ] ]
    [ , MAXSIZE = { max_size [ KB | MB | GB | TB ] | UNLIMITED } ]
    [ , FILEGROWTH = growth_increment [ KB | MB | GB | TB | % ] ]
)
}

<filegroup> ::=
{
FILEGROUP filegroup name [ [ CONTAINS FILESTREAM ] [ DEFAULT ] | CONTAINS MEMORY_OPTIMIZED_DATA ]
    <filespec> [ ,...n ]
}
```

Attacher une base de données

```syntaxsql
CREATE DATABASE database_name
    ON <filespec> [ ,...n ]
    FOR { { ATTACH [ WITH <attach_database_option> [ , ...n ] ] }
        | ATTACH_REBUILD_LOG }
[;]

<attach_database_option> ::=
{
      <service_broker_option>
    | RESTRICTED_USER
    | FILESTREAM ( DIRECTORY_NAME = { 'directory_name' | NULL } )
}

<service_broker_option> ::=
{
    ENABLE_BROKER
  | NEW_BROKER
  | ERROR_BROKER_CONVERSATIONS
}
```

Créer un instantané de base de données

```syntaxsql
CREATE DATABASE database_snapshot_name
    ON
    (
        NAME = logical_file_name,
        FILENAME = 'os_file_name'
    ) [ ,...n ]
    AS SNAPSHOT OF
[;]
```

## <a name="arguments"></a>Arguments

*database_name* correspond au nom de la nouvelle base de données. Les noms de base de données doivent être uniques au sein d’une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et respecter les règles applicables aux [identificateurs](../../relational-databases/databases/database-identifiers.md).

*database_name* peut comporter 128 caractères maximum, à moins qu’aucun nom logique ne soit spécifié pour le fichier journal. Si aucun nom logique de fichier journal n’est spécifié, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] génère *logical_file_name* et the *os_file_name* pour le journal en ajoutant un suffixe à *database_name*. Cela limite la taille de *database_name* à 123 caractères de sorte que le nom logique du fichier généré ne comporte pas plus de 128 caractères.

Si le nom de fichier de données n’est pas spécifié, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilise *database_name* à la fois comme *logical_file_name* et comme *os_file_name*. Le chemin d'accès par défaut est obtenu à partir du Registre. Le chemin par défaut peut être modifié à l’aide de **Propriétés du serveur (page Paramètres de base de données)** dans [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]. La modification du chemin d'accès par défaut requiert le redémarrage de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

CONTAINMENT = { NONE | PARTIAL }

**S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et ultérieur

Spécifie l'état de la relation contenant-contenu de la base de données. NONE = Base de données non autonome. PARTIAL = Base de données partiellement autonome.

ON spécifie que les fichiers disque servant à stocker les parties données de la base de données (fichiers des données) sont définis de manière explicite. ON est nécessaire s’il est suivi d’une liste d’éléments \<filespec> séparés par des virgules, définissant les fichiers de données du groupe de fichiers primaire. La liste des fichiers du groupe de fichiers primaire peut être suivie d’une liste facultative d’éléments \<filegroup> séparés par des virgules, définissant les groupes de fichiers utilisateur et leurs fichiers.

PRIMARY spécifie que la liste \<filespec> associée définit le fichier primaire. Le premier fichier spécifié dans l’entrée \<filespec> du groupe de fichiers primaire devient le fichier primaire. Une base de données ne peut avoir qu’un seul fichier primaire. Pour plus d'informations, consultez [Database Files and Filegroups](../../relational-databases/databases/database-files-and-filegroups.md).

Si vous ne précisez pas PRIMARY, le premier fichier spécifié dans l'instruction CREATE DATABASE devient le fichier primaire.

LOG ON spécifie que les fichiers disque servant à stocker le journal de la base de données (fichiers journaux) sont définis de manière explicite. LOG ON est suivi d’une liste d’éléments \<filespec> séparés par des virgules, définissant les fichiers journaux. Si vous ne spécifiez pas LOG ON, un fichier journal est automatiquement créé, dont la taille correspond à 512 Ko, ou, si ce volume est plus élevé, à 25 pour cent de la somme des tailles de tous les fichiers de données de la base de données. Ce fichier est placé à l'emplacement de fichier journal par défaut. Pour plus d’informations sur cet emplacement, consultez [Afficher ou modifier les emplacements par défaut des fichiers de données et des fichiers journaux - SSMS](../../database-engine/configure-windows/view-or-change-the-default-locations-for-data-and-log-files.md).

LOG ON ne peut pas être spécifié sur un instantané de base de données.

COLLATE *collation_name* spécifie le classement par défaut de la base de données. Le nom du classement peut être un nom de classement Windows ou SQL. S'il n'est pas spécifié, la base de données est affectée au classement par défaut de l'instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Un nom de classement ne peut pas être spécifié sur un instantané de base de données.

Un nom de classement ne peut pas être spécifié pour les clauses FOR ATTACH ou FOR ATTACH_REBUILD_LOG. Pour plus d’informations sur la façon de changer le classement d’une base de données attachée, visitez ce [site web de Microsoft](https://go.microsoft.com/fwlink/?linkid=16419&kbid=325335).

Pour plus d’informations sur les noms de classements Windows et SQL, voir [COLLATE](~/t-sql/statements/collations.md).

> [!NOTE]
> Les bases de données autonomes sont classées différemment des bases de données non autonomes. Pour plus d’informations, consultez [Classements de base de données autonome](../../relational-databases/databases/contained-database-collations.md).

WITH \<option>
 **\<filestream_option>**

NON_TRANSACTED_ACCESS = { **OFF** | READ_ONLY | FULL } **S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et versions ultérieures.

Spécifie le niveau d'accès FILESTREAM non transactionnel à la base de données.

|Valeur|Description|
|-----------|-----------------|
|OFF|L'accès non transactionnel est désactivé.|
|READONLY|Les données FILESTREAM de cette base de données peuvent être lues par des processus non transactionnels.|
|FULL|L'accès non transactionnel complet aux FileTables FILESTREAM est activé.|

DIRECTORY_NAME = \<directory_name>
**S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et versions ultérieures

Nom de répertoire compatible avec Windows. Ce nom doit être unique parmi tous les noms Database_Directory dans l'instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. La comparaison d'unicité n'est pas sensible à la casse, indépendamment des paramètres de classement [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Cette option doit être définie avant de créer un FileTable dans cette base de données.

Les options suivantes sont autorisées uniquement lorsque CONTAINMENT a été défini sur PARTIAL. Si CONTAINMENT est défini sur NONE, des erreurs se produiront.

- **DEFAULT_FULLTEXT_LANGUAGE = \<lcid> | \<language name> | \<language alias>**

  **S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et ultérieur

  Pour obtenir une description complète de cette option, consultez [Configurer l’option de configuration de serveur default full-text language](../../database-engine/configure-windows/configure-the-default-full-text-language-server-configuration-option.md).

- **DEFAULT_LANGUAGE = \<lcid> | \<language name> | \<language alias>**

  **S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et ultérieur

  Pour obtenir une description complète de cette option, consultez [Configurer l’option de configuration de serveur default language](../../database-engine/configure-windows/configure-the-default-language-server-configuration-option.md).

- **NESTED_TRIGGERS = { OFF | ON}**

  **S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et ultérieur

  Pour obtenir une description complète de cette option, consultez [Configurer l’option de configuration de serveur nested triggers](../../database-engine/configure-windows/configure-the-nested-triggers-server-configuration-option.md).

- **TRANSFORM_NOISE_WORDS = { OFF | ON}**

  **S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et ultérieur

  Pour obtenir une description complète de cette option, consultez [Configurer l’option de configuration de serveur transform noise words](../../database-engine/configure-windows/transform-noise-words-server-configuration-option.md).

- **TWO_DIGIT_YEAR_CUTOFF = { 2049 | \<any year between 1753 and 9999> }**

  Quatre chiffres qui représente une année. 2049 est la valeur par défaut. Pour obtenir une description complète de cette option, consultez [Configurer l’option de configuration du serveur two digit year cutoff](../../database-engine/configure-windows/configure-the-two-digit-year-cutoff-server-configuration-option.md).

- **DB_CHAINING { OFF | ON }**

  Si ON est spécifié, la base de données peut être la source ou la cible d'un chaînage des propriétés des bases de données croisées.

  Lorsque OFF est spécifié, la base de données ne peut pas participer au chaînage des propriétés des bases de données croisées. La valeur par défaut est OFF.

  > [!IMPORTANT]
  > L'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] reconnaît ce paramètre lorsque l'option de serveur cross db ownership chaining a la valeur 0 (OFF). Lorsque cross db ownership chaining a la valeur 1 (ON), toutes les bases de données utilisateur peuvent participer aux chaînages des propriétés des bases de données croisées, quelle que soit la valeur de cette option. Cette option est configurée à l’aide de [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md).

  Pour définir cette option, l'appartenance au rôle serveur fixe sysadmin est nécessaire. L’option DB_CHAINING ne peut pas être définie sur les bases de données système suivantes : master, model et tempdb.

- **TRUSTWORTHY { OFF | ON }**

  Si ON est spécifié, les modules de base de données (par exemple les vues, les fonctions définies par l'utilisateur ou les procédures stockées) utilisant le contexte d'emprunt d'identité peuvent accéder à des ressources en dehors de la base de données.

  Lorsque OFF est spécifié, les modules de base de données dans le contexte d'emprunt d'identité ne peuvent pas accéder à des ressources en dehors de la base de données. La valeur par défaut est OFF.

  TRUSTWORTHY prend la valeur OFF chaque fois que la base de données est attachée.

  Par défaut, pour toutes les bases de données système, sauf pour la base msdb, l'option TRUSTWORTHY est définie à OFF (désactivé). La valeur ne peut pas être modifiée pour les bases de données model et tempdb. Nous vous recommandons de ne jamais définir l'option TRUSTWORTHY à ON (activé) pour la base de données master.

- **PERSISTENT_LOG_BUFFER=ON ( DIRECTORY_NAME=’’ )**

  Lorsque cette option est spécifiée, le tampon du journal des transactions est créé sur un volume situé sur un disque avec mémoire de classe de stockage (stockage non volatil NVDIMM-N), également appelé tampon de journal persistant. Pour plus d’informations, voir [Accélération de la latence des validations de transactions avec la mémoire de classe de stockage](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2016/12/02/transaction-commit-latency-acceleration-using-storage-class-memory-in-windows-server-2016sql-server-2016-sp1/). **S’applique à** : [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] et versions ultérieures.

FOR ATTACH [ WITH \< attach_database_option > ] spécifie que la base de données est créée en [joignant](../../relational-databases/databases/database-detach-and-attach-sql-server.md) un ensemble existant de fichiers du système d’exploitation. Il doit exister une entrée \<filespec> spécifiant le premier fichier primaire. Les seules autres entrées \<filespec> nécessaires sont celles relatives aux fichiers dont le chemin est différent de celui existant lors de la première création de la base de données ou de son dernier attachement. Vous devez spécifier une entrée \<filespec> pour ces fichiers.

FOR ATTACH exige les conditions suivantes :

- Tous les fichiers de données (MDF et NDF) doivent être disponibles.
- Si plusieurs fichiers journaux existent, tous doivent être disponibles.

Si une base de données en lecture-écriture possède un seul fichier journal qui n'est pas disponible actuellement, et si la base de données a été fermée sans aucun utilisateur ou transaction ouverte avant l'opération d'attachement, FOR ATTACH reconstruit automatiquement le fichier journal et met à jour le fichier primaire. Par opposition, pour une base de données en lecture seule, le fichier journal ne peut pas être reconstruit car le fichier primaire ne peut pas être mis à jour. Par conséquent, lorsque vous attachez une base de données en lecture seule dont le fichier journal n'est pas disponible, vous devez fournir les fichiers journaux ou les fichiers pour la cause FOR ATTACH.

> [!NOTE]
> Une base de données créée dans une version plus récente de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne peut pas être attachée à des versions antérieures.

Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], tous les fichiers de texte intégral appartenant à la base de données qui est attachée seront attachés avec la base de données. Pour spécifier un nouveau chemin d'accès pour le catalogue de texte intégral, spécifiez le nouvel emplacement sans le nom de fichier du système d'exploitation en texte intégral. Pour plus d’informations, consultez la section Exemples.

Le fait d’attacher une base de données qui contient une option FILESTREAM de « nom de répertoire », dans une instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] invitera [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à vérifier que le nom Database_Directory est unique. Si ce n’est pas le cas, l’opération d’attachement échoue avec l’erreur « FILESTREAM Database_Directory name \<name> n’est pas unique dans cette instance SQL Server ». Pour éviter cette erreur, le paramètre facultatif, *directory_name*, doit être passé à cette opération.

FOR ATTACH ne peut pas être spécifié sur un instantané de base de données.

FOR ATTACH peut spécifier l'option RESTRICTED_USER. RESTRICTED_USER permet uniquement aux membres du rôle de base de données fixe db_owner et aux rôles serveurs fixes dbcreator et sysadmin de se connecter à la base de données, mais il n'en limite pas le nombre. Les tentatives par des utilisateurs non qualifiés sont refusées.

Si la base de données utilise [!INCLUDE[ssSB](../../includes/sssb-md.md)], utilisez WITH \<service_broker_option> dans votre clause FOR ATTACH :

\<service_broker_option> contrôle la remise des messages [!INCLUDE[ssSB](../../includes/sssb-md.md)] et l’identificateur [!INCLUDE[ssSB](../../includes/sssb-md.md)] pour la base de données. Les options [!INCLUDE[ssSB](../../includes/sssb-md.md)] peuvent être spécifiées uniquement quand la clause FOR ATTACH est utilisée.

ENABLE_BROKER spécifie que [!INCLUDE[ssSB](../../includes/sssb-md.md)] est activé pour la base de données spécifiée. Autrement dit, la remise des messages est démarrée et is_broker_enabled a la valeur True dans la vue de catalogue sys.databases. La base de données conserve l'identificateur [!INCLUDE[ssSB](../../includes/sssb-md.md)] existant.

NEW_BROKER crée une nouvelle valeur service_broker_guid dans sys.databases et les bases restaurées, et termine tous les points de terminaison de conversation avec un nettoyage. Service Broker est activé, mais aucun message n'est envoyé aux points de terminaison de conversation distants. Tout itinéraire qui fait référence à l'ancien identificateur [!INCLUDE[ssSB](../../includes/sssb-md.md)] doit être recréé avec le nouvel identificateur.

ERROR_BROKER_CONVERSATIONS termine toutes les conversations avec une erreur indiquant que la base de données est attachée ou restaurée. Service Broker est désactivé jusqu'à la fin de l'opération, puis il est activé. La base de données conserve l'identificateur [!INCLUDE[ssSB](../../includes/sssb-md.md)] existant.

Lorsque vous attachez une base de données répliquée qui a été copiée au lieu d'être détachée, tenez compte des conditions suivantes :

- Si vous attachez la base de données à la même version et à la même instance de serveur que celles de la base de données d'origine, aucune opération supplémentaire n'est nécessaire.
- Si vous attachez la base de données à la même instance de serveur alors que sa version a été mise à niveau, vous devez exécuter [sp_vupgrade_replication](../../relational-databases/system-stored-procedures/sp-vupgrade-replication-transact-sql.md) pour mettre à jour la réplication à la fin de l’opération de rattachement.
- Si vous attachez la base de données à une instance de serveur différente, sans tenir compte de la version, vous devez exécuter [sp_removedbreplication](../../relational-databases/system-stored-procedures/sp-removedbreplication-transact-sql.md) pour supprimer la réplication, une fois l’opération de rattachement effectuée.

> [!NOTE]
> L’attachement fonctionne avec le format de stockage **vardecimal**, mais le [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] doit être mis à niveau au minimum vers [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] SP2. Vous ne pouvez pas attacher une base de données à l'aide du format de stockage vardecimal à une version antérieure de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour plus d’informations sur le format de stockage **vardecimal**, consultez [Compression des données](../../relational-databases/data-compression/data-compression.md).

Lorsqu'une base de données est attachée ou restaurée pour la première fois à une nouvelle instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], une copie de la clé principale de la base de données (chiffrée par la clé principale du service) n'est pas encore stockée sur le serveur. Vous devez utiliser l’instruction **OPEN MASTER KEY** pour déchiffrer la clé principale de la base de données. Une fois la clé principale de la base de données déchiffrée, vous avez la possibilité d’activer le déchiffrement automatique dans le futur en exécutant l’instruction **ALTER MASTER KEY REGENERATE** pour fournir au serveur une copie de la clé principale de la base de données chiffrée avec la clé principale du service. Lorsqu'une base de données a été mise à niveau à partir d'une version antérieure, la clé DMK doit être régénérée de façon à utiliser le nouvel algorithme AES. Pour plus d’informations sur la régénération de la clé DMK, consultez l’article [ALTER MASTER KEY](../../t-sql/statements/alter-master-key-transact-sql.md). La durée nécessaire pour régénérer la clé DMK à mettre à niveau vers AES dépend du nombre d'objets protégés par la clé DMK. La régénération de la clé DMK à mettre à niveau vers AES est nécessaire une seule fois et n'a aucune incidence sur les régénérations ultérieures effectuées dans le cadre d'une stratégie de rotation de clés. Pour plus d’informations sur la façon de mettre à niveau une base de données à l’aide de l’attachement, consultez [Mettre à niveau une base de données avec Detach et Attach](../../relational-databases/databases/upgrade-a-database-using-detach-and-attach-transact-sql.md).

> [!IMPORTANT]
> Nous vous recommandons de ne pas attacher des bases de données issues de sources inconnues ou non approuvées. Ces bases de données peuvent contenir du code malveillant susceptible d'exécuter du code [!INCLUDE[tsql](../../includes/tsql-md.md)] indésirable ou de provoquer des erreurs en modifiant le schéma ou la structure physique des bases de données. Avant d’utiliser une base de données issue d’une source inconnue ou non approuvée, exécutez [DBCC CHECKDB](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md) sur la base de données sur un serveur autre qu’un serveur de production et examinez également le code, notamment les procédures stockées ou le code défini par l’utilisateur, de la base de données.
> [!NOTE]
> Les options **TRUSTWORTHY** et **DB_CHAINING** n’ont aucun effet lors de l’attachement d’une base de données.

FOR ATTACH_REBUILD_LOG spécifie que la base de données est créée en joignant un ensemble existant de fichiers du système d’exploitation. Cette option est limitée aux bases de données en lecture/écriture. Il doit exister une entrée *\<filespec>* spécifiant le fichier primaire. S'il manque un ou plusieurs fichiers du journal des transactions, le fichier journal est reconstruit. ATTACH_REBUILD_LOG crée automatiquement un nouveau fichier journal de 1 Mo. Ce fichier est placé à l'emplacement de fichier journal par défaut. Pour plus d’informations sur cet emplacement, consultez [Afficher ou modifier les emplacements par défaut des fichiers de données et des fichiers journaux - SSMS](../../database-engine/configure-windows/view-or-change-the-default-locations-for-data-and-log-files.md).

> [!NOTE]
> Si les fichiers journaux sont disponibles, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] utilise ces fichiers au lieu de reconstruire les fichiers journaux.

FOR ATTACH_REBUILD_LOG requiert ce qui suit :

- Un arrêt propre de la base de données.
- Tous les fichiers de données (MDF et NDF) doivent être disponibles.

> [!IMPORTANT]
> Cette opération brise la chaîne de sauvegarde du journal. Nous recommandons d'effectuer une sauvegarde complète avant de réaliser cette opération. Pour plus d’informations, consultez [BACKUP](../../t-sql/statements/backup-transact-sql.md).

Généralement, FOR ATTACH_REBUILD_LOG est utilisé lorsque vous copiez une base de données en lecture/écriture avec un grand journal vers un autre serveur où la copie sera principalement, ou uniquement, utilisée pour des opérations de lecture, et nécessite donc moins d'espace de journal que la base de données d'origine.

FOR ATTACH_REBUILD_LOG ne peut pas être spécifié sur un instantané de base de données.

Pour plus d’informations sur l’attachement et le détachement de base de données, consultez [Attacher et détacher une base de données](../../relational-databases/databases/database-detach-and-attach-sql-server.md).

\<filespec> contrôle les propriétés des fichiers.

NAME *logical_file_name* spécifie le nom logique du fichier. NAME est requis lorsque FILENAME est spécifié, sauf lors de la spécification d'une des clauses FOR ATTACH. Un groupe de fichiers FILESTREAM ne peut pas être nommé PRIMARY.

*logical_file_name* Spécifie le nom logique utilisé pour référencer le fichier dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. *Logical_file_name* doit être unique dans la base de données et doit respecter les règles relatives aux [identificateurs](../../relational-databases/databases/database-identifiers.md). Le nom peut être une constante de type caractère ou Unicode, un identificateur régulier ou un identificateur délimité.

FILENAME { **'** _os\_file\_name_ **'** | **'** _filestream\_path_ **'** } : spécifie un nom de fichier du système d’exploitation (physique).

**'** *os_file_name* **'**  : correspond au chemin d’accès et nom de fichier utilisé par le système d’exploitation lorsque vous créez le fichier. Le fichier doit résider sur l'un des périphériques suivants : le serveur local sur lequel [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est installé, un réseau de zone de stockage [réseau SAN] ou un réseau basé sur iSCSI. Le chemin d'accès spécifié doit exister avant l'exécution de l'instruction CREATE DATABASE. Pour plus d'informations, consultez le paragraphe « Groupes de fichiers et fichiers de base de données » dans la section Notes.

Les paramètres SIZE, MAXSIZE et FILEGROWTH peuvent être définis lorsqu'un chemin d'accès UNC est spécifié pour le fichier.

Si le fichier se trouve sur une partition brute, *os_file_name* doit spécifier uniquement la lettre d’une unité correspondant à une partition brute existante. Un seul fichier de données peut être créé sur chaque partition brute.

Les fichiers de données ne doivent pas être placés sur des systèmes de fichiers compressés, sauf si les fichiers sont des fichiers secondaires en lecture seule ou si la base de données est en en lecture seule. Les fichiers journaux ne doivent jamais être placés sur des systèmes de fichiers compressés.

**'** *filestream_path* **'** Pour un groupe de fichiers FILESTREAM, FILENAME fait référence à un chemin où les données FILESTREAM seront stockées. Le chemin d'accès jusqu'au dernier dossier doit exister, et le dernier dossier ne doit pas exister. Par exemple, si vous spécifiez le chemin d'accès C:\MyFiles\MyFilestreamData, C:\MyFiles doit exister avant l'exécution d'ALTER DATABASE, mais le dossier MyFilestreamData ne doit pas exister.

Le groupe de fichiers et le fichier (`<filespec>`) doivent être créés dans la même instruction.

Les propriétés SIZE et FILEGROWTH ne s'appliquent pas à un groupe de fichiers FILESTREAM.

SIZE *size* spécifie la taille du fichier.

SIZE ne peut pas être spécifié quand *os_file_name* est spécifié en tant que chemin UNC. SIZE ne s'applique pas à un groupe de fichiers FILESTREAM.

*size* correspond à la taille initiale du fichier.

Quand *size* n’est pas spécifié pour le fichier primaire, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] utilise la taille du fichier primaire dans la base de données model. La taille par défaut de la base de données model est de 8 Mo (à partir de [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)]) ou de 1 Mo (pour les versions antérieures). Quand vous spécifiez un fichier de données secondaire ou un fichier journal, mais que *size* n’est pas spécifié pour le fichier, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] lui donne une taille de 8 Mo (à partir de [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)]) ou de 1 Mo (pour les versions antérieures). La taille spécifiée pour le fichier primaire doit être au moins égale à la taille du fichier primaire de la base de données model.

Les suffixes kilo-octet (Ko), mégaoctet (Mo), gigaoctet (Go) ou téraoctet (To) peuvent être utilisés. La valeur par défaut est Mo. Indiquez un nombre entier sans aucune décimale. *Size* est une valeur entière. Pour les valeurs supérieures à 2147483647, utilisez des unités plus grandes.

MAXSIZE *max_size* spécifie la taille maximale que peut atteindre le fichier. MAXSIZE ne peut pas être spécifié quand *os_file_name* est spécifié en tant que chemin UNC.

*max_size* Spécifie la taille de fichier maximale. Les indications Ko, Mo, Go et To peuvent être utilisées. La valeur par défaut est Mo. Indiquez un nombre entier sans aucune décimale. Si vous ne spécifiez pas *max_size*, le fichier peut s’accroître jusqu’à occuper tout l’espace disque disponible. *Max_size* est une valeur entière. Pour les valeurs supérieures à 2147483647, utilisez des unités plus grandes.

UNLIMITED Spécifie que la taille du fichier peut croître jusqu’à ce que le disque soit plein. Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], un fichier journal spécifié avec une croissance illimitée a une taille maximale de 2 To et un fichier de données une taille maximale de 16 To.

> [!NOTE]
> Aucune taille maximale n'est définie lorsque cette option est spécifiée pour un conteneur FILESTREAM. Il continue à grandir jusqu'à ce que le disque soit saturé.

FILEGROWTH *growth_increment* Spécifie l’incrément de croissance automatique du fichier. Le paramètre FILEGROWTH d'un fichier ne peut dépasser le paramètre MAXSIZE. FILEGROWTH ne peut pas être spécifié quand *os_file_name* est spécifié en tant que chemin UNC. FILEGROWTH ne s'applique pas à un groupe de fichiers FILESTREAM.

*growth_increment* Spécifie la quantité d’espace ajoutée au fichier quand de l’espace supplémentaire est nécessaire.

La valeur peut être exprimée en Mo, Ko, Go, To ou pourcentage (%). Si un nombre est mentionné sans spécifier Mo, Ko ou %, la valeur par défaut est Mo. Lorsque % est spécifié, la taille de l'incrément de croissance est le pourcentage choisi de la taille du fichier au moment où l'incrémentation a lieu. La taille spécifiée est arrondie à la valeur multiple de 64 Ko la plus proche, et la valeur minimale est de 64 Ko.

La valeur 0 indique que la croissance automatique est désactivée et qu'aucun espace supplémentaire n'est autorisé.

Si FILEGROWTH n’est pas spécifié, les valeurs par défaut sont les suivantes :

|Version|Valeurs par défaut|
|-------------|--------------------|
|À partir de [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)]|64 Mo de données. 64 Mo de fichiers journaux.|
|À partir de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]|1 Mo de données. 10 % de fichiers journaux.|
|Avant [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]|10 % de données. 10 % de fichiers journaux.|

\<filegroup> contrôle les propriétés des groupes de fichiers. Filegroup ne peut pas être spécifié sur un instantané de base de données.

FILEGROUP *filegroup_name* correspond au nom logique du groupe de fichiers.

*filegroup_name*
*filegroup_name* doit être unique dans la base de données et ne peut pas être les noms PRIMARY et PRIMARY_LOG fournis par le système. Le nom peut être une constante de type caractère ou Unicode, un identificateur régulier ou un identificateur délimité. Le nom doit respecter les règles applicables aux [identificateurs](../../relational-databases/databases/database-identifiers.md).

CONTAINS FILESTREAM Spécifie que le groupe de fichiers stocke des objets BLOB (Binary Large Objects) FILESTREAM dans le système de fichiers.

CONTAINS MEMORY_OPTIMIZED_DATA

**S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et ultérieur

Spécifie que le groupe de fichiers stocke des données optimisées en mémoire dans le système de fichiers. Pour plus d’informations, consultez l’article [OLTP en mémoire (optimisation en mémoire)](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md). Un seul groupe de fichiers MEMORY_OPTIMIZED_DATA est autorisé par base de données. Pour obtenir des exemples de code qui créent un groupe de fichiers pour stocker des données à mémoire optimisée, consultez [Création d’une table optimisée en mémoire et d’une procédure stockée compilée en mode natif](../../relational-databases/in-memory-oltp/creating-a-memory-optimized-table-and-a-natively-compiled-stored-procedure.md).

DEFAULT spécifie le groupe de fichiers nommé qui est le groupe de fichiers par défaut de la base de données.

*database_snapshot_name* correspond au nom de l’instantané de la nouvelle base de données. Les noms des instantanés de bases de données doivent être uniques au sein d'une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et respecter les règles applicables aux identificateurs. *database_snapshot_name* peut avoir un maximum de 128 caractères.

ON **(** NAME **=** _logical\_file\_name_ **,** FILENAME **='** _os\_file\_name_ **')** [ **,** ... *n* ] pour créer un instantané de base de données, spécifie une liste de fichiers dans la base de données source. Pour que l'instantané fonctionne, tous les fichiers de données doivent être spécifiés individuellement. Cependant, les fichiers journaux ne sont pas autorisés pour les instantanés de base de données. Les groupes de fichiers FILESTREAM ne sont pas pris en charge par les instantanés de base de données. Si un fichier de données FILESTREAM est inclus dans une clause CREATE DATABASE ON, l'instruction échoue et une erreur est levée.

Pour obtenir des descriptions de NAME et FILENAME et leurs valeurs, consultez les descriptions des valeurs \<filespec> équivalentes.

> [!NOTE]
> Quand vous créez un instantané de base de données, les autres options \<filespec> et le mot clé PRIMARY ne sont pas autorisés.

AS SNAPSHOT OF *source_database_name* spécifie que la base de données créée est un instantané de la base de données source spécifiée par *source_database_name*. Les bases de données d'instantané et source doivent se trouver sur la même instance.

Pour plus d'informations, consultez [Instantanés de base de données](#database-snapshots) dans la section Notes.

## <a name="remarks"></a>Notes

La [base de données master](../../relational-databases/databases/master-database.md) doit être sauvegardée chaque fois qu’une base de données utilisateur est créée, modifiée ou supprimée.

L'instruction `CREATE DATABASE` doit être exécutée en mode de validation automatique (mode de gestion des transactions par défaut) et n’est pas autorisée dans une transaction explicite ou implicite.

Vous pouvez utiliser une instruction `CREATE DATABASE` pour créer une base de données et les fichiers qui stockent la base de données. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] implémente l'instruction CREATE DATABASE à l'aide des étapes suivantes :

1. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilise une copie de la [base de données model](../../relational-databases/databases/model-database.md) pour initialiser la base de données et ses métadonnées.
2. Un GUID Service Broker est affecté à la base de données.
3. Le [!INCLUDE[ssDE](../../includes/ssde-md.md)] complète ensuite le reste de la base de données avec des pages vides, à l'exception des pages qui contiennent des données internes enregistrant la manière dont est utilisé l'espace dans la base de données.

Vous pouvez spécifier un maximum de 32 767 bases de données sur une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

Chaque base de données appartient à un propriétaire qui peut réaliser des activités spéciales dans la base de données. Le propriétaire est l'utilisateur qui crée la base de données. Le propriétaire de base de données peut être changé avec [ALTER AUTHORIZATION](../../t-sql/statements/alter-authorization-transact-sql.md).

Certaines fonctionnalités de base de données dépendent de fonctionnalités ou de capacités présentes dans le système de fichiers. Voici quelques exemples des fonctionnalités qui dépendent de jeux de fonctionnalités du système de fichiers :

- DBCC CHECKDB
- FileStream
- Sauvegardes en ligne à l’aide de VSS et d’instantanés de fichiers
- Création d’instantané de base de données
- Groupe de fichiers de données à mémoire optimisée

## <a name="database-files-and-filegroups"></a>Groupes de fichiers et fichiers de base de données

Chaque base de données comprend au moins deux fichiers, un *fichier primaire* et un *fichier journal des transactions*, et au moins un groupe de fichiers. Un maximum de 32 767 fichiers et 32 767 groupes de fichiers peut être spécifié pour chaque base de données.

Lorsque vous créez une base de données, attribuez aux fichiers une taille aussi grande que possible, en tenant compte du volume maximal de données qu'est censée contenir la base de données.

Nous vous recommandons d'utiliser un réseau de zone de stockage (SAN, Storage Area Network), un réseau basé sur iSCSI ou un disque attaché localement pour le stockage de vos fichiers de base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], car cette configuration optimise les performances et la fiabilité de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

## <a name="database-snapshots"></a>Instantanés de base de données

Vous pouvez utiliser l’instruction `CREATE DATABASE` pour créer une vue en lecture seule statique, un *instantané de base de données* de la *base de données source*. Chaque instantané de base de données est transactionnellement cohérent avec la base de données source existante au moment de la création de l'instantané. Une base de données source peut posséder plusieurs instantanés.

> [!NOTE]
> Quand vous créez un instantané de base de données, l'instruction `CREATE DATABASE` ne peut pas faire référence à des fichiers journaux, hors connexion, de restauration ou anciens.

Si la création d'un instantané de base de données échoue, l'instantané devient suspect et doit être supprimé. Pour plus d’informations, consultez [DROP DATABASE](../../t-sql/statements/drop-database-transact-sql.md).

Chaque instantané est conservé jusqu’à ce qu’il soit supprimé par `DROP DATABASE`.

Pour plus d’informations, consultez [Instantanés de base de données](../../relational-databases/databases/database-snapshots-sql-server.md).

## <a name="database-options"></a>Options de base de données

Plusieurs options de base de données sont définies automatiquement chaque fois que vous créez une base de données. Pour obtenir la liste de ces options, consultez [Options ALTER DATABASE SET](../../t-sql/statements/alter-database-transact-sql-set-options.md).

## <a name="the-model-database-and-creating-new-databases"></a>Base de données model et création de nouvelles bases de données

Les objets définis par l’utilisateur dans la [base de données model](../../relational-databases/databases/model-database.md) sont copiés dans toutes les nouvelles bases de données. Vous pouvez ajouter dans la base de données model tous les objets, tels que tables, vues, procédures stockées ou types de données, à inclure dans toutes les bases de données nouvellement créées.

Quand une instruction `CREATE DATABASE <database_name>` est spécifiée sans paramètre de taille supplémentaire, le fichier de données primaire a la même taille que celui de la base de données model.

Sauf si `FOR ATTACH` est spécifié, chaque nouvelle base de données hérite des paramètres d’option de base de données de la base de données model. Par exemple, l’option de base de données auto shrink a la valeur **true** dans model et dans toutes les nouvelles bases de données que vous créez. Si vous modifiez les options de la base de données model, ces nouveaux paramètres d'options sont valables pour toutes les nouvelles bases de données que vous créez. Les opérations de modification dans la base de données model n'affectent pas les bases de données existantes. Si vous avez précisé FOR ATTACH dans l'instruction CREATE DATABASE, la nouvelle base de données héritera des paramètres d'option de la base de données originale.

## <a name="viewing-database-information"></a>Affichage des informations de bases de données

Vous pouvez utiliser les affichages catalogue, les fonctions système et les procédures stockées du système pour retourner des informations sur les bases de données, les fichiers et les groupes de fichiers. Pour plus d’informations, consultez [Vues système](https://msdn.microsoft.com/library/35a6161d-7f43-4e00-bcd3-3091f2015e90).

## <a name="permissions"></a>Autorisations

Requiert l’autorisation `CREATE DATABASE`, `CREATE ANY DATABASE` ou `ALTER ANY DATABASE`.

Pour garder le contrôle de l'utilisation du disque sur une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], l'autorisation de création de bases de données est généralement limitée à quelques comptes de connexion.

L'exemple suivant fournit l'autorisation de créer une base de données à l'utilisateur de base de données Fay.

```sql
USE master;
GO
GRANT CREATE DATABASE TO [Fay];
GO
```

### <a name="permissions-on-data-and-log-files"></a>Autorisations sur les données et les journaux

Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], certaines autorisations sont définies sur les données et les journaux de chaque base de données. Les autorisations suivantes sont définies chaque fois que les opérations suivantes sont appliquées à une base de données :

- Attachée
- Sauvegardée
- Date de création
- Détachée
- Modifiée pour ajouter un nouveau fichier
- Restaurée

Les autorisations empêchent les fichiers d'être accidentellement falsifiés s'ils résident dans un répertoire doté d'autorisations d'ouverture.

> [!NOTE]
> [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssExpressEd2005](../../includes/ssexpressed2005-md.md)] ne définit pas d'autorisations sur les fichiers de données et les journaux.

## <a name="examples"></a>Exemples

### <a name="a-creating-a-database-without-specifying-files"></a>R. Création d'une base de données sans spécifier de fichiers

Cet exemple crée une base de données appelée `mytest` et crée un fichier primaire et un fichier de journal des transactions correspondants. L’instruction ne disposant pas d’éléments \<filespec>, le fichier primaire de la base de données a la taille du fichier primaire de la base de données model. Le journal des transactions est défini selon la plus grande de ces valeurs : 512 Ko ou 25 % de la taille du fichier de données primaire. Puisque MAXSIZE n'est pas spécifié, la taille des fichiers peut s'accroître jusqu'à occuper tout l'espace disque disponible. Cet exemple montre également comment supprimer la base de données nommée `mytest`, si elle existe, avant de créer la base de données `mytest`.

```sql
USE master;
GO
IF DB_ID (N'mytest') IS NOT NULL
DROP DATABASE mytest;
GO
CREATE DATABASE mytest;
GO
-- Verify the database files and sizes
SELECT name, size, size*1.0/128 AS [Size in MBs]
FROM sys.master_files
WHERE name = N'mytest';
GO
```

### <a name="b-creating-a-database-that-specifies-the-data-and-transaction-log-files"></a>B. Création d'une base de données qui spécifie les fichiers de données et les fichiers journaux de transactions

L'exemple suivant crée la base de données `Sales`. Le mot clé PRIMARY n’étant pas utilisé, le premier fichier (`Sales_dat`) devient le fichier principal. Le paramètre SIZE n'étant spécifié ni en Mo ni en Ko pour le fichier `Sales_dat` , la valeur par défaut est Mo et il est alloué en mégaoctets. La base de données `Sales_log` est alloué en mégaoctets car le suffixe `MB` est défini explicitement dans le paramètre `SIZE` .

```sql
USE master;
GO
CREATE DATABASE Sales
ON
( NAME = Sales_dat,
    FILENAME = 'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\DATA\saledat.mdf',
    SIZE = 10,
    MAXSIZE = 50,
    FILEGROWTH = 5 )
LOG ON
( NAME = Sales_log,
    FILENAME = 'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\DATA\salelog.ldf',
    SIZE = 5MB,
    MAXSIZE = 25MB,
    FILEGROWTH = 5MB ) ;
GO
```

### <a name="c-creating-a-database-by-specifying-multiple-data-and-transaction-log-files"></a>C. Création d'une base de données en spécifiant plusieurs fichiers de données et plusieurs fichiers journaux de transactions

Cet exemple crée une base de données appelée `Archive` qui comprend trois fichiers de données de `100-MB` et deux fichiers du journal des transactions de `100-MB`. Le fichier primaire est le premier fichier dans la liste et il est spécifié de manière explicite à l'aide du mot clé `PRIMARY`. Les fichiers du journal des transactions sont spécifiés à la suite des mots clés `LOG ON`. Notez les extensions utilisées pour les fichiers dans l'option `FILENAME` : `.mdf` pour les fichiers de données primaires, `.ndf` pour les fichiers de données secondaires et `.ldf` pour les fichiers journaux de transactions. Cet exemple place la base de données sur le lecteur `D:` plutôt qu'avec la base de données `master`.

```sql
USE master;
GO
CREATE DATABASE Archive
ON
PRIMARY
    (NAME = Arch1,
    FILENAME = 'D:\SalesData\archdat1.mdf',
    SIZE = 100MB,
    MAXSIZE = 200,
    FILEGROWTH = 20),
    ( NAME = Arch2,
    FILENAME = 'D:\SalesData\archdat2.ndf',
    SIZE = 100MB,
    MAXSIZE = 200,
    FILEGROWTH = 20),
    ( NAME = Arch3,
    FILENAME = 'D:\SalesData\archdat3.ndf',
    SIZE = 100MB,
    MAXSIZE = 200,
    FILEGROWTH = 20)
LOG ON
  (NAME = Archlog1,
    FILENAME = 'D:\SalesData\archlog1.ldf',
    SIZE = 100MB,
    MAXSIZE = 200,
    FILEGROWTH = 20),
  (NAME = Archlog2,
    FILENAME = 'D:\SalesData\archlog2.ldf',
    SIZE = 100MB,
    MAXSIZE = 200,
    FILEGROWTH = 20) ;
GO
```

### <a name="d-creating-a-database-that-has-filegroups"></a>D. Création d'une base de données possédant des groupes de fichiers

L'exemple suivant crée la base de données `Sales` qui possède les groupes de fichiers suivants :

- Le groupe de fichiers primaire avec les fichiers `Spri1_dat` et `Spri2_dat`. Les incréments FILEGROWTH de ces fichiers sont spécifiés à `15%`.
- Un groupe de fichiers nommé `SalesGroup1` avec les fichiers `SGrp1Fi1` et `SGrp1Fi2`.
- Un groupe de fichiers nommé `SalesGroup2` avec les fichiers `SGrp2Fi1` et `SGrp2Fi2`.

Cet exemple place les fichiers de données et les fichiers journaux sur des disques différents afin d'améliorer les performances.

```sql
USE master;
GO
CREATE DATABASE Sales
ON PRIMARY
( NAME = SPri1_dat,
    FILENAME = 'D:\SalesData\SPri1dat.mdf',
    SIZE = 10,
    MAXSIZE = 50,
    FILEGROWTH = 15% ),
( NAME = SPri2_dat,
    FILENAME = 'D:\SalesData\SPri2dt.ndf',
    SIZE = 10,
    MAXSIZE = 50,
    FILEGROWTH = 15% ),
FILEGROUP SalesGroup1
( NAME = SGrp1Fi1_dat,
    FILENAME = 'D:\SalesData\SG1Fi1dt.ndf',
    SIZE = 10,
    MAXSIZE = 50,
    FILEGROWTH = 5 ),
( NAME = SGrp1Fi2_dat,
    FILENAME = 'D:\SalesData\SG1Fi2dt.ndf',
    SIZE = 10,
    MAXSIZE = 50,
    FILEGROWTH = 5 ),
FILEGROUP SalesGroup2
( NAME = SGrp2Fi1_dat,
    FILENAME = 'D:\SalesData\SG2Fi1dt.ndf',
    SIZE = 10,
    MAXSIZE = 50,
    FILEGROWTH = 5 ),
( NAME = SGrp2Fi2_dat,
    FILENAME = 'D:\SalesData\SG2Fi2dt.ndf',
    SIZE = 10,
    MAXSIZE = 50,
    FILEGROWTH = 5 )
LOG ON
( NAME = Sales_log,
    FILENAME = 'E:\SalesLog\salelog.ldf',
    SIZE = 5MB,
    MAXSIZE = 25MB,
    FILEGROWTH = 5MB ) ;
GO
```

### <a name="e-attaching-a-database"></a>E. Attachement d'une base de données

L'exemple ci-dessous détache la base de données `Archive` créée dans l'exemple D, puis l'attache à l'aide de la clause `FOR ATTACH`. `Archive` a été défini de manière à posséder plusieurs fichiers de données et fichiers journaux. Cependant, l'emplacement des fichiers n'ayant pas été modifié depuis leur création, seuls les fichiers primaires doivent être spécifiés dans la clause `FOR ATTACH`. À compter de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)], tout fichier de texte intégral appartenant à la base de données qui est attachée est attaché avec la base de données.

```sql
USE master;
GO
sp_detach_db Archive;
GO
CREATE DATABASE Archive
  ON (FILENAME = 'D:\SalesData\archdat1.mdf')
  FOR ATTACH ;
GO
```

### <a name="f-creating-a-database-snapshot"></a>F. Création d'un instantané de base de données

L’exemple suivant crée l’instantané de base de données `sales_snapshot0600`. Un instantané de base de données étant en lecture seule, un fichier journal ne peut pas être spécifié. Conformément à la syntaxe, chaque fichier de la base de données source est spécifié et les groupes de fichiers ne sont pas spécifiés.

La base de données source dans cet exemple est la base de données `Sales` créée dans l'exemple D.

```sql
USE master;
GO
CREATE DATABASE sales_snapshot0600 ON
    ( NAME = SPri1_dat, FILENAME = 'D:\SalesData\SPri1dat_0600.ss'),
    ( NAME = SPri2_dat, FILENAME = 'D:\SalesData\SPri2dt_0600.ss'),
    ( NAME = SGrp1Fi1_dat, FILENAME = 'D:\SalesData\SG1Fi1dt_0600.ss'),
    ( NAME = SGrp1Fi2_dat, FILENAME = 'D:\SalesData\SG1Fi2dt_0600.ss'),
    ( NAME = SGrp2Fi1_dat, FILENAME = 'D:\SalesData\SG2Fi1dt_0600.ss'),
    ( NAME = SGrp2Fi2_dat, FILENAME = 'D:\SalesData\SG2Fi2dt_0600.ss')
AS SNAPSHOT OF Sales ;
GO
```

### <a name="g-creating-a-database-and-specifying-a-collation-name-and-options"></a>G. Création d'une base de données et spécification d'un nom de classement et d'options

L'exemple suivant crée la base de données `MyOptionsTest`. Un nom de classement est spécifié et les options `TRUSTYWORTHY` et `DB_CHAINING` ont la valeur `ON`.

```sql
USE master;
GO
IF DB_ID (N'MyOptionsTest') IS NOT NULL
DROP DATABASE MyOptionsTest;
GO
CREATE DATABASE MyOptionsTest
COLLATE French_CI_AI
WITH TRUSTWORTHY ON, DB_CHAINING ON;
GO
--Verifying collation and option settings.
SELECT name, collation_name, is_trustworthy_on, is_db_chaining_on
FROM sys.databases
WHERE name = N'MyOptionsTest';
GO
```

### <a name="h-attaching-a-full-text-catalog-that-has-been-moved"></a>H. Attachement d'un catalogue de texte intégral qui a été déplacé

L'exemple suivant attache le catalogue de texte intégral `AdvWksFtCat` ainsi que les fichiers de données et fichiers journaux de `AdventureWorks2012`. Dans cet exemple, le catalogue de texte intégral est déplacé de son emplacement par défaut vers un nouvel emplacement `c:\myFTCatalogs`. Les fichiers de données et les fichiers journaux restent dans leurs emplacements par défaut.

```sql
USE master;
GO
--Detach the AdventureWorks2012 database
sp_detach_db AdventureWorks2012;
GO
-- Physically move the full text catalog to the new location.
--Attach the AdventureWorks2012 database and specify the new location of the full-text catalog.
CREATE DATABASE AdventureWorks2012 ON
    (FILENAME = 'c:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Data\AdventureWorks2012_data.mdf'),
    (FILENAME = 'c:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Data\AdventureWorks2012_log.ldf'),
    (FILENAME = 'c:\myFTCatalogs\AdvWksFtCat')
FOR ATTACH;
GO
```

### <a name="i-creating-a-database-that-specifies-a-row-filegroup-and-two-filestream-filegroups"></a>I. Création d'une base de données qui spécifie un groupe de fichiers de ligne et deux groupes de fichiers FILESTREAM

L'exemple ci-dessous crée la base de données `FileStreamDB`. La base de données est créée avec un groupe de fichiers de ligne et deux groupes de fichiers FILESTREAM. Chaque groupe de fichiers contient un fichier :

- `FileStreamDB_data` contient des données de ligne. Il contient un seul fichier, `FileStreamDB_data.mdf` avec le chemin d'accès par défaut.
- `FileStreamPhotos` contient des données FILESTREAM. Il contient deux conteneurs de données FILESTREAM : `FSPhotos` (dans `C:\MyFSfolder\Photos`) et `FSPhotos2` (dans `D:\MyFSfolder\Photos`). Il est marqué comme groupe de fichiers FILESTREAM par défaut.
- `FileStreamResumes` contient des données FILESTREAM. Il contient un seul conteneur de données FILESTREAM, `FSResumes`, qui se trouve dans `C:\MyFSfolder\Resumes`.

```sql
USE master;
GO
-- Get the SQL Server data path.
DECLARE @data_path nvarchar(256);
SET @data_path = (SELECT SUBSTRING(physical_name, 1, CHARINDEX(N'master.mdf', LOWER(physical_name)) - 1)
      FROM master.sys.master_files
      WHERE database_id = 1 AND file_id = 1);

 -- Execute the CREATE DATABASE statement.
EXECUTE ('CREATE DATABASE FileStreamDB
ON PRIMARY
    (
    NAME = FileStreamDB_data
    ,FILENAME = ''' + @data_path + 'FileStreamDB_data.mdf''
    ,SIZE = 10MB
    ,MAXSIZE = 50MB
    ,FILEGROWTH = 15%
    ),
FILEGROUP FileStreamPhotos CONTAINS FILESTREAM DEFAULT
    (
    NAME = FSPhotos
    ,FILENAME = ''C:\MyFSfolder\Photos''
-- SIZE and FILEGROWTH should not be specified here.
-- If they are specified an error will be raised.
, MAXSIZE = 5000 MB
    ),
    (
      NAME = FSPhotos2
      , FILENAME = ''D:\MyFSfolder\Photos''
      , MAXSIZE = 10000 MB
     ),
FILEGROUP FileStreamResumes CONTAINS FILESTREAM
    (
    NAME = FileStreamResumes
    ,FILENAME = ''C:\MyFSfolder\Resumes''
    )
LOG ON
    (
    NAME = FileStream_log
    ,FILENAME = ''' + @data_path + 'FileStreamDB_log.ldf''
    ,SIZE = 5MB
    ,MAXSIZE = 25MB
    ,FILEGROWTH = 5MB
    )'
);
GO
```

### <a name="j-creating-a-database-that-has-a-filestream-filegroup-with-multiple-files"></a>J. Création d'une base de données disposant d'un groupe de fichiers FILESTREAM avec de nombreux fichiers

L'exemple ci-dessous crée la base de données `BlobStore1`. La base de données est créée avec un groupe de fichiers de ligne et un groupe de fichiers FILESTREAM, `FS`. Le groupe de fichiers FILESTREAM contient deux fichiers, `FS1` et `FS2`. Puis la base de données est modifiée par l'ajout d'un troisième fichier, `FS3`, au groupe de fichiers FILESTREAM.

```sql
USE master;
GO

CREATE DATABASE [BlobStore1]
CONTAINMENT = NONE
ON PRIMARY
(
    NAME = N'BlobStore1',
    FILENAME = N'C:\BlobStore\BlobStore1.mdf',
    SIZE = 100MB,
    MAXSIZE = UNLIMITED,
    FILEGROWTH = 1MB
),
FILEGROUP [FS] CONTAINS FILESTREAM DEFAULT
(  
    NAME = N'FS1',
    FILENAME = N'C:\BlobStore\FS1',
    MAXSIZE = UNLIMITED
),
(
    NAME = N'FS2',
    FILENAME = N'C:\BlobStore\FS2',
    MAXSIZE = 100MB
)
LOG ON
(
    NAME = N'BlobStore1_log',
    FILENAME = N'C:\BlobStore\BlobStore1_log.ldf',
    SIZE = 100MB,
    MAXSIZE = 1GB,
    FILEGROWTH = 1MB
);
GO

ALTER DATABASE [BlobStore1]
ADD FILE
(
    NAME = N'FS3',
    FILENAME = N'C:\BlobStore\FS3',
    MAXSIZE = 100MB
)
TO FILEGROUP [FS];
GO
```

## <a name="see-also"></a>Voir aussi

- [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql.md)
- [Attacher et détacher une base de données](../../relational-databases/databases/database-detach-and-attach-sql-server.md)
- [DROP DATABASE](../../t-sql/statements/drop-database-transact-sql.md)
- [EVENTDATA](../../t-sql/functions/eventdata-transact-sql.md)
- [ALTER AUTHORIZATION](../../t-sql/statements/alter-authorization-transact-sql.md)
- [sp_detach_db](../../relational-databases/system-stored-procedures/sp-detach-db-transact-sql.md)
- [sp_removedbreplication](../../relational-databases/system-stored-procedures/sp-removedbreplication-transact-sql.md)
- [Instantanés de base de données](../../relational-databases/databases/database-snapshots-sql-server.md)
- [Déplacer des fichiers de bases de données](../../relational-databases/databases/move-database-files.md)
- [Bases de données](../../relational-databases/databases/databases.md)
- [Données Blob (Binary Large Object)](../../relational-databases/blob/binary-large-object-blob-data-sql-server.md)

::: moniker-end
::: moniker range="=azuresqldb-current"

:::row:::
    :::column:::
        [SQL Server](create-database-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* SQL Database \*_**
    :::column-end:::
    :::column:::
        [SQL Managed Instance](create-database-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](create-database-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
    :::column:::
        [Analytics Platform<br />System (PDW)](create-database-transact-sql.md?view=aps-pdw-2016&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="sql-database"></a>SQL Database

## <a name="overview"></a>Vue d’ensemble

Dans [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], cette instruction est utilisable avec un serveur SQL Azure pour créer une base de données unique ou une base de données dans un pool élastique. Elle implique de spécifier le nom, le classement, la taille maximale, l’édition, l’objectif de service et, le cas échéant, le pool élastique de la nouvelle base de données. Elle permet également créer la base de données dans un pool élastique et de créer une copie de la base de données sur un autre serveur SQL Database.

## <a name="syntax"></a>Syntaxe

### <a name="create-a-database"></a>Création d'une base de données

```syntaxsql
CREATE DATABASE database_name [ COLLATE collation_name ]
{
  (<edition_options> [, ...n])
}
[ WITH <with_options> [,..n]]
[;]

<with_options> ::=
{
  CATALOG_COLLATION = { DATABASE_DEFAULT | SQL_Latin1_General_CP1_CI_AS }
  | BACKUP_STORAGE_REDUNDANCY = { 'LOCAL' | 'ZONE' | 'GEO' }
}

<edition_options> ::=
{

  MAXSIZE = { 100 MB | 500 MB | 1 ... 1024 ... 4096 GB }
  | ( EDITION = { 'Basic' | 'Standard' | 'Premium' | 'GeneralPurpose' | 'BusinessCritical' | 'Hyperscale' }
  | SERVICE_OBJECTIVE =
    { 'Basic' | 'S0' | 'S1' | 'S2' | 'S3' | 'S4'| 'S6'| 'S7'| 'S9'| 'S12'
      | 'P1' | 'P2' | 'P4'| 'P6' | 'P11' | 'P15'
      | 'GP_Gen4_1' | 'GP_Gen4_2' | 'GP_Gen4_3' | 'GP_Gen4_4' | 'GP_Gen4_5' | 'GP_Gen4_6'
      | 'GP_Gen4_7' | 'GP_Gen4_8' | 'GP_Gen4_9' | 'GP_Gen4_10' | 'GP_Gen4_16' | 'GP_Gen4_24'
      | 'GP_Gen5_2' | 'GP_Gen5_4' | 'GP_Gen5_6' | 'GP_Gen5_8' | 'GP_Gen5_10' | 'GP_Gen5_12' | 'GP_Gen5_14'
      | 'GP_Gen5_16' | 'GP_Gen5_18' | 'GP_Gen5_20' | 'GP_Gen5_24' | 'GP_Gen5_32' | 'GP_Gen5_40' | 'GP_Gen5_80'
      | 'GP_Fsv2_8' | 'GP_Fsv2_10' | 'GP_Fsv2_12' | 'GP_Fsv2_14' | 'GP_Fsv2_16' | 'GP_Fsv2_18'
      | 'GP_Fsv2_20' | 'GP_Fsv2_24' | 'GP_Fsv2_32' | 'GP_Fsv2_36' | 'GP_Fsv2_72'
      | 'GP_S_Gen5_1' | 'GP_S_Gen5_2' | 'GP_S_Gen5_4' | 'GP_S_Gen5_6' | 'GP_S_Gen5_8'
      | 'GP_S_Gen5_10' | 'GP_S_Gen5_12' | 'GP_S_Gen5_14' | 'GP_S_Gen5_16'
      | 'GP_S_Gen5_18' | 'GP_S_Gen5_20' | 'GP_S_Gen5_24' | 'GP_S_Gen5_32' | 'GP_S_Gen5_40'
      | 'BC_Gen4_1' | 'BC_Gen4_2' | 'BC_Gen4_3' | 'BC_Gen4_4' | 'BC_Gen4_5' | 'BC_Gen4_6'
      | 'BC_Gen4_7' | 'BC_Gen4_8' | 'BC_Gen4_9' | 'BC_Gen4_10' | 'BC_Gen4_16' | 'BC_Gen4_24'
      | 'BC_Gen5_2' | 'BC_Gen5_4' | 'BC_Gen5_6' | 'BC_Gen5_8' | 'BC_Gen5_10' | 'BC_Gen5_12' | 'BC_Gen5_14'
      | 'BC_Gen5_16' | 'BC_Gen5_18' | 'BC_Gen5_20' | 'BC_Gen5_24' | 'BC_Gen5_32' | 'BC_Gen5_40' | 'BC_Gen5_80'
      | 'BC_M_8' | 'BC_M_10' | 'BC_M_12' | 'BC_M_14' | 'BC_M_16' | 'BC_M_18'
      | 'BC_M_20' | 'BC_M_24' | 'BC_M_32' | 'BC_M_64' | 'BC_M_128'
      | 'HS_GEN4_1' | 'HS_GEN4_2' | 'HS_GEN4_4' | 'HS_GEN4_8' | 'HS_GEN4_16' | 'HS_GEN4_24'
      | 'HS_GEN5_2' | 'HS_GEN5_4' | 'HS_GEN5_8' | 'HS_GEN5_16' | 'HS_GEN5_24' | 'HS_GEN5_32' | 'HS_GEN5_48' | 'HS_GEN5_80'
      | { ELASTIC_POOL(name = <elastic_pool_name>) } })
}
```

### <a name="copy-a-database"></a>Copier une base de données

```syntaxsql
CREATE DATABASE database_name
    AS COPY OF [source_server_name.] source_database_name
    [ ( SERVICE_OBJECTIVE =
      { 'Basic' |'S0' | 'S1' | 'S2' | 'S3'| 'S4'| 'S6'| 'S7'| 'S9'| 'S12'
      | 'P1' | 'P2' | 'P4'| 'P6' | 'P11' | 'P15'
      | 'GP_Gen4_1' | 'GP_Gen4_2' | 'GP_Gen4_3' | 'GP_Gen4_4' | 'GP_Gen4_5' | 'GP_Gen4_6'
      | 'GP_Gen4_7' | 'GP_Gen4_8' | 'GP_Gen4_9' | 'GP_Gen4_10' | 'GP_Gen4_16' | 'GP_Gen4_24'
      | 'GP_Gen5_2' | 'GP_Gen5_4' | 'GP_Gen5_6' | 'GP_Gen5_8' | 'GP_Gen5_10' | 'GP_Gen5_12' | 'GP_Gen5_14'
      | 'GP_Gen5_16' | 'GP_Gen5_18' | 'GP_Gen5_20' | 'GP_Gen5_24' | 'GP_Gen5_32' | 'GP_Gen5_40' | 'GP_Gen5_80'
      | 'GP_Fsv2_8' | 'GP_Fsv2_10' | 'GP_Fsv2_12' | 'GP_Fsv2_14' | 'GP_Fsv2_16' | 'GP_Fsv2_18'
      | 'GP_Fsv2_20' | 'GP_Fsv2_24' | 'GP_Fsv2_32' | 'GP_Fsv2_36' | 'GP_Fsv2_72'
      | 'GP_S_Gen5_1' | 'GP_S_Gen5_2' | 'GP_S_Gen5_4' | 'GP_S_Gen5_6' | 'GP_S_Gen5_8'
      | 'GP_S_Gen5_10' | 'GP_S_Gen5_12' | 'GP_S_Gen5_14' | 'GP_S_Gen5_16'
      | 'GP_S_Gen5_18' | 'GP_S_Gen5_20' | 'GP_S_Gen5_24' | 'GP_S_Gen5_32' | 'GP_S_Gen5_40'
      | 'BC_Gen4_1' | 'BC_Gen4_2' | 'BC_Gen4_3' | 'BC_Gen4_4' | 'BC_Gen4_5' | 'BC_Gen4_6'
      | 'BC_Gen4_7' | 'BC_Gen4_8' | 'BC_Gen4_9' | 'BC_Gen4_10' | 'BC_Gen4_16' | 'BC_Gen4_24'
      | 'BC_Gen5_2' | 'BC_Gen5_4' | 'BC_Gen5_6' | 'BC_Gen5_8' | 'BC_Gen5_10' | 'BC_Gen5_12' | 'BC_Gen5_14'
      | 'BC_Gen5_16' | 'BC_Gen5_18' | 'BC_Gen5_20' | 'BC_Gen5_24' | 'BC_Gen5_32' | 'BC_Gen5_40' | 'BC_Gen5_80'
      | 'BC_M_8' | 'BC_M_10' | 'BC_M_12' | 'BC_M_14' | 'BC_M_16' | 'BC_M_18'
      | 'BC_M_20' | 'BC_M_24' | 'BC_M_32' | 'BC_M_64' | 'BC_M_128'
      | 'HS_GEN4_1' | 'HS_GEN4_2' | 'HS_GEN4_4' | 'HS_GEN4_8' | 'HS_GEN4_16' | 'HS_GEN4_24'
      | 'HS_GEN5_2' | 'HS_GEN5_4' | 'HS_GEN5_8' | 'HS_GEN5_16' | 'HS_GEN5_24' | 'HS_GEN5_32' | 'HS_GEN5_48' | 'HS_GEN5_80'
      | { ELASTIC_POOL(name = <elastic_pool_name>) } })
   ]
[;]
```

## <a name="arguments"></a>Arguments

*database_name* est le nom de la nouvelle base de données. Ce nom doit être unique sur le serveur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et doit respecter les règles [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] relatives aux identificateurs. Pour plus d’informations, consultez [Identificateurs](https://go.microsoft.com/fwlink/p/?LinkId=180386).

*Collation_name* : spécifie le classement par défaut de la base de données. Le nom du classement peut être un nom de classement Windows ou SQL. S’il est omis, le classement par défaut, SQL_Latin1_General_CP1_CI_AS, est affecté à la base de données.

Pour plus d’informations sur les noms de classements Windows et SQL, consultez [COLLATE (Transact-SQL)](../../t-sql/statements/collations.md).

CATALOG_COLLATION : spécifie le classement par défaut du catalogue de métadonnées. *DATABASE_DEFAULT* Spécifie que le catalogue de métadonnées utilisé pour les vues et tables système doit être assemblé de façon à correspondre au classement par défaut pour la base de données. Il s’agit du comportement adopté dans SQL Server.

*SQL_Latin1_General_CP1_CI_AS* Précise que le catalogue de métadonnées utilisé pour les vues et tables système doit être assemblé par rapport à un classement SQL_Latin1_General_CP1_CI_AS fixe. Il s’agit du paramètre par défaut dans Azure SQL Database si non spécifié.

BACKUP_STORAGE_REDUNDANCY spécifie comment la restauration à un point dans le temps et les sauvegardes à conservation longue d’une base de données sont répliquées. La géorestauration, ou la possibilité de récupérer après une panne d’une région, est disponible seulement quand la base de données est créée avec la redondance de stockage de sauvegarde « GEO ». Sauf spécification explicite, les bases de données créées avec T-SQL utilisent un stockage de sauvegarde géoredondant. 

> [!IMPORTANT]
> L’option BACKUP_STORAGE_REDUNDANCY pour Azure SQL Database est uniquement disponible en préversion publique dans la région Brésil Sud et mise à la disposition générale dans la région Azure Asie Sud-Est.  

EDITION : spécifie la couche de service de la base de données.

Bases de données uniques et mises en pool. Les valeurs disponibles sont : 'Basic', 'Standard', 'Premium', 'GeneralPurpose', 'BusinessCritical' et 'Hyperscale'.

MAXSIZE : spécifie la taille maximale de la base de données. MAXSIZE doit être valide pour l'EDITION (niveau de service) spécifiée. Voici les valeurs de MAXSIZE prises en charge et les valeurs par défaut (D) des niveaux de service.

> [!NOTE]
> L’argument **MAXSIZE** ne s’applique pas aux bases de données uniques dans le niveau de service Hyperscale. Les bases de données de niveau Hyperscale croissent en fonction des besoins, jusqu'à 100 To. Le service SQL Database ajoute automatiquement du stockage : vous n’avez pas besoin de définir une taille maximale.

**Modèle DTU des bases de données uniques et mises en pool sur un serveur SQL Database**

|**MAXSIZE**|**De base**|**S0-S2**|**S3-S12**|**P1-P6**| **P11-P15** |
|:---|:---|:---|:---|:---|:---|
|100 Mo|√|√|√|√|√|
|500 Mo|√|√|√|√|√|
|1 Go|√|√|√|√|√|
|2 Go|√ (D)|√|√|√|√|
|5 Go|N/A|√|√|√|√|
|10 Go|N/A|√|√|√|√|
|20 Go|N/A|√|√|√|√|
|30 Go|N/A|√|√|√|√|
|40 Go|N/A|√|√|√|√|
|50 Go|N/A|√|√|√|√|
|100 Go|N/A|√|√|√|√|
|150 Go|N/A|√|√|√|√|
|200 Go|N/A|√|√|√|√|
|250 Go|N/A|√ (D)|√ (D)|√|√|
|300 Go|N/A|N/A|√|√|√|
|400 Go|N/A|N/A|√|√|√|
|500 Go|N/A|N/A|√|√ (D)|√|
|750 Go|N/A|N/A|√|√|√|
|1 024 Go|N/A|N/A|√|√|√ (D)|
|À partir de 1 024 Go jusqu’à 4 096 Go par incréments de 256 Go* |N/A|N/A|N/A|NON APPLICABLE|√|√|

\* P11 et P15 autorisent MAXSIZE jusqu’à 4 To, 1 024 Go étant la taille par défaut. P11 et P15 peuvent utiliser jusqu’à 4 To de stockage inclus sans frais supplémentaires. Au niveau Premium, une valeur MAXSIZE supérieure à 1 To est actuellement disponible dans les régions suivantes : USA Est 2, USA Ouest, US Gov Virginie, Europe Ouest, Allemagne Centre, Asie Sud-Est, Japon Est, Australie Est, Canada Centre et Canada Est. Pour plus d’informations sur les limitations des ressources du modèle DTU, consultez [Limites des ressources DTU](https://docs.microsoft.com/azure/sql-database/sql-database-dtu-resource-limits).

La valeur MAXSIZE pour le modèle DTU, si elle est spécifiée, doit être une valeur valide indiquée dans le tableau ci-dessus pour le niveau de service spécifié.

**Modèle vCore**

**Usage général - calcul provisionné - Gen4 (partie 1)**

|MAXSIZE|GP_Gen4_1|GP_Gen4_2|GP_Gen4_3|GP_Gen4_4|GP_Gen4_5|GP_Gen4_6|
|:----- | ------: |-------: |-------: |-------: |-------: |--------:|
|Taille maximale des données (Go)|1 024|1 024|1 024|1536|1536|1536|

**Usage général - calcul provisionné - Gen4 (partie 2)**

|MAXSIZE|GP_Gen4_7|GP_Gen4_8|GP_Gen4_9|GP_Gen4_10|GP_Gen4_16|GP_Gen4_24
|:----- | ------: |-------: |-------: |-------: |-------: |--------:|
|Taille maximale des données (Go)|1536|3 072|3 072|3 072|4096|4096|

**Usage général - calcul provisionné - Gen5 (partie 1)**

|MAXSIZE|GP_Gen5_2|GP_Gen5_4|GP_Gen5_6|GP_Gen5_8|GP_Gen5_10|GP_Gen5_12|GP_Gen5_14|
|:----- | ------: |-------: |-------: |-------: |--------: |---------:|--------: |
|Taille maximale des données (Go)|1 024|1 024|1 024|1536|1536|1536|1536|

**Usage général - calcul provisionné - Gen5 (partie 2)**

|MAXSIZE|GP_Gen5_16|GP_Gen5_18|GP_Gen5_20|GP_Gen5_24|GP_Gen5_32|GP_Gen5_40|GP_Gen5_80|
|:----- | ------: |-------: |-------: |-------: |--------: |---------:|--------: |
|Taille maximale des données (Go)|3 072|3 072|3 072|4096|4096|4096|4096|

**Usage général - calcul provisionné - série Fsv2 (partie 1)**

|MAXSIZE|GP_Fsv2_8|GP_Fsv2_10|GP_Fsv2_12|GP_Fsv2_14|GP_Fsv2_16|GP_Fsv2_18|
|:----- | ------: | ------: | ------: | ------: | ------: | ------: |
|Taille maximale des données (Go)|1 024|1 024|1 024|1 024|1536|1536|

**Usage général - calcul provisionné - série Fsv2 (partie 2)**

|MAXSIZE|GP_Fsv2_20|GP_Fsv2_24|GP_Fsv2_32|GP_Fsv2_36|GP_Fsv2_72|
|:----- | ------: | ------: | ------: | ------: | ------: |
|Taille maximale des données (Go)|1536|1536|3 072|3 072|4096|

**Usage général - calcul serverless - Gen5 (partie 1)**

|MAXSIZE|GP_S_Gen5_1|GP_S_Gen5_2|GP_S_Gen5_4|GP_S_Gen5_6|GP_S_Gen5_8|
|:----- | ------: |-------: |-------: |-------: |--------: |
|vCores max.|1|2|4|6|8|

**Usage général - calcul serverless - Gen5 (partie 2)**

|MAXSIZE|GP_S_Gen5_10|GP_S_Gen5_12|GP_S_Gen5_14|GP_S_Gen5_16|
|:----- | ------: |-------: |-------: |-------: |
|vCores max.|10|12|14|16|

**Usage général - calcul serverless - Gen5 (partie 3)**

|MAXSIZE|GP_S_Gen5_18|GP_S_Gen5_20|GP_S_Gen5_24|GP_S_Gen5_32|GP_S_Gen5_40|
|:----- | ------: |-------: |-------: |-------: |--------: |
|vCores max.|18|20|24|32|40|

**Critique pour l’entreprise - calcul provisionné - Gen4 (partie 1)**

|Taille de calcul (objectif de service)|BC_Gen4_1|BC_Gen4_2|BC_Gen4_3|BC_Gen4_4|BC_Gen4_5|BC_Gen4_6|
|:--------------- | ------: |-------: |-------: |-------: |-------: |-------: |
|Taille maximale des données (Go)|1 024|1 024|1 024|1 024|1 024|1 024|

**Critique pour l’entreprise - calcul provisionné - Gen4 (partie 2)**

|Taille de calcul (objectif de service)|BC_Gen4_7|BC_Gen4_8|BC_Gen4_9|BC_Gen4_10|BC_Gen4_16|BC_Gen4_24|
|:--------------- | ------: |-------: |-------: |--------: |--------: |--------: |
|Taille maximale des données (Go)|1 024|1 024|1 024|1 024|1 024|1 024|

**Critique pour l’entreprise - calcul provisionné - Gen5 (partie 1)**

|MAXSIZE|BC_Gen5_2|BC_Gen5_4|BC_Gen5_6|BC_Gen5_8|BC_Gen5_10|BC_Gen5_12|BC_Gen5_14|
|:----- | ------: |-------: |-------: |-------: |---------: |--------:|--------: |
|Taille maximale des données (Go)|1 024|1 024|1 024|1536|1536|1536|1536|

**Critique pour l’entreprise - calcul provisionné - Gen5 (partie 2)**

|MAXSIZE|BC_Gen5_16|BC_Gen5_18|BC_Gen5_20|BC_Gen5_24|BC_Gen5_32|BC_Gen5_40|BC_Gen5_80|
|:----- | -------: |--------: |--------: |--------: |--------: |---------:|--------: |
|Taille maximale des données (Go)|3 072|3 072|3 072|4096|4096|4096|4096|

**Critique pour l’entreprise - calcul provisionné - série M (partie 1)**

|MAXSIZE|BC_M_8|BC_M_10|BC_M_12|BC_M_14|BC_M_16|BC_M_18|
|:----- | -------: | -------: | -------: | -------: | -------: | -------: |
|Taille maximale des données (Go)|512|640|768|896|1 024|1152|

**Critique pour l’entreprise - calcul provisionné - série M (partie 2)**

|MAXSIZE|BC_M_20|BC_M_24|BC_M_32|BC_M_64|BC_M_128|
|:----- | -------: | -------: | -------: | -------: | -------: |
|Taille maximale des données (Go)|1 280|1536|2 048|4096|4096|

Si aucune valeur `MAXSIZE` n’est définie lors de l’utilisation du modèle vCore, la valeur par défaut est de 32 Go. Pour plus d’informations sur les limitations des ressources du modèle vCore, consultez [Limites des ressources vCore](https://docs.microsoft.com/azure/sql-database/sql-database-dtu-resource-limits).

Les règles suivantes s'appliquent aux arguments MAXSIZE et EDITION.

- Si EDITION est spécifié, mais MAXSIZE n'est pas spécifié, la valeur par défaut de l'édition est utilisée. Par exemple, si la valeur de EDITION est définie sur Standard, et que MAXSIZE n’est pas spécifié, la valeur de MAXSIZE est automatiquement définie sur 250 Mo.
- Si MAXSIZE et EDITION ne sont pas spécifiés, EDITION est défini sur `GeneralPurpose` et MAXSIZE est défini sur 32 Go.

SERVICE_OBJECTIVE

- **Pour les bases de données uniques et mises en pool**

  - Spécifie la taille de calcul (objectif de service). Les valeurs disponibles pour l’objectif du service sont : `S0`, `S1`, `S2`, `S3`, `S4`, `S6`, `S7`, `S9`, `S12`, `P1`, `P2`, `P4`, `P6`, `P11`, `P15`, `GP_GEN4_1`, `GP_GEN4_2`, `GP_GEN4_3`, `GP_GEN4_4`, `GP_GEN4_5`, `GP_GEN4_6`, `GP_GEN4_7`, `GP_GEN4_8`, `GP_GEN4_7`, `GP_GEN4_8`, `GP_GEN4_9`, `GP_GEN4_10`, `GP_GEN4_16`, `GP_GEN4_24`, `BC_GEN4_1`, `BC_GEN4_2`, `BC_GEN4_3`, `BC_GEN4_4`, `BC_GEN4_5`, `BC_GEN4_6`, `BC_GEN4_7`, `BC_GEN4_8`, `BC_GEN4_9`, `BC_GEN4_10`, `BC_GEN4_16`, `BC_GEN4_24`, `GP_Gen5_2`, `GP_Gen5_4`, `GP_Gen5_6`, `GP_Gen5_8`, `GP_Gen5_10`, `GP_Gen5_12`, `GP_Gen5_14`, `GP_Gen5_16`, `GP_Gen5_18`, `GP_Gen5_20`, `GP_Gen5_24`, `GP_Gen5_32`, `GP_Gen5_40`, `GP_Gen5_80`, `GP_Fsv2_8`, `GP_Fsv2_10`, `GP_Fsv2_12`, `GP_Fsv2_14`, `GP_Fsv2_16`, `GP_Fsv2_18`, `GP_Fsv2_20`, `GP_Fsv2_24`, `GP_Fsv2_32`, `GP_Fsv2_36`, `GP_Fsv2_72`, `BC_Gen5_2`, `BC_Gen5_4`, `BC_Gen5_6`, `BC_Gen5_8`, `BC_Gen5_10`, `BC_Gen5_12`, `BC_Gen5_14`, `BC_Gen5_16`, `BC_Gen5_18`, `BC_Gen5_20`, `BC_Gen5_24`, `BC_Gen5_32`,`BC_Gen5_40`, `BC_Gen5_80`, `BC_M_8`, `BC_M_10`, `BC_M_12`, `BC_M_14`, `BC_M_16`, `BC_M_18`, `BC_M_20`, `BC_M_24`, `BC_M_32`, `BC_M_64`, `BC_M_128`.

- **Pour les bases de données serverless**
- 
  - Spécifie la taille de calcul (objectif de service). Les valeurs disponibles pour l’objectif de service sont : `GP_S_Gen5_1`, `GP_S_Gen5_2`, `GP_S_Gen5_4`, `GP_S_Gen5_6`, `GP_S_Gen5_8`, `GP_S_Gen5_10`, `GP_S_Gen5_12`, `GP_S_Gen5_14`, `GP_S_Gen5_16`, `GP_S_Gen5_18`, `GP_S_Gen5_20`, `GP_S_Gen5_24`, `GP_S_Gen5_32`, `GP_S_Gen5_40`.

- **Pour les bases de données uniques du niveau de service Hyperscale**

  - Spécifie la taille de calcul (objectif de service). Les valeurs disponibles pour l’objectif du service sont : `HS_GEN4_1` `HS_GEN4_2` `HS_GEN4_4` `HS_GEN4_8` `HS_GEN4_16`, `HS_GEN4_24`, `HS_Gen5_2`, `HS_Gen5_4`, `HS_Gen5_8`, `HS_Gen5_16`, `HS_Gen5_24`, `HS_Gen5_32`, `HS_Gen5_48`, `HS_Gen5_80`.

Pour plus d’informations sur les objectifs de service, ainsi que sur la taille, les éditions et les combinaisons d’objectifs de service, consultez [Niveaux de service d’Azure SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-service-tiers). Si le SERVICE_OBJECTIVE spécifié n’est pas pris en charge par l’EDITION, un message d’erreur s’affiche. Si vous voulez modifier la valeur de SERVICE_OBJECTIVE pour passer d'un niveau de service à un autre (par exemple de S1 à P1), vous devrez également modifier la valeur d'EDITION. Pour plus d’informations sur les objectifs de service, ainsi que sur la taille, les éditions et les combinaisons d’objectifs de service, consultez [Niveaux de service et de performance d’Azure SQL Database](https://azure.microsoft.com/documentation/articles/sql-database-service-tiers/), [Limites des ressources DTU](https://docs.microsoft.com/azure/sql-database/sql-database-dtu-resource-limits) et [Limites des ressources vCore](https://docs.microsoft.com/azure/sql-database/sql-database-dtu-resource-limits). La prise en charge des objectifs de service PRS a été supprimée. Pour poser des questions, utilisez cet alias de messagerie : premium-rs@microsoft.com.

ELASTIC_POOL (name = \<elastic_pool_name>) **S’applique à :** Bases de données uniques et mises en pool uniquement. Ne s’applique pas aux bases de données dans le niveau de service Hyperscale.
Pour créer une base de données dans un pool de bases de données élastique, définissez SERVICE_OBJECTIVE de la base de données sur ELASTIC_POOL et fournissez le nom du pool. Pour plus d’informations, consultez [Créer et gérer un pool élastique SQL Database](https://azure.microsoft.com/documentation/articles/sql-database-elastic-pool-portal/).

AS COPY OF [source_server_name.]source_database_name **S’applique à :** Bases de données uniques et mises en pool uniquement.
Pour la copie d'une base de données sur le même serveur ou sur un serveur [!INCLUDE[ssSDS](../../includes/sssds-md.md)] différent.

*source_server_name* : nom du serveur [!INCLUDE[ssSDS](../../includes/sssds-md.md)] où se trouve la base de données source. Ce paramètre est facultatif lorsque la base de données source et la base de données de destination se trouveront sur le même serveur [!INCLUDE[ssSDS](../../includes/sssds-md.md)].

> [!NOTE]
> l'argument `AS COPY OF` ne prend pas en charge les noms de domaine complets uniques. En d'autres termes, si le nom de domaine complet de votre serveur est `serverName.database.windows.net`, utilisez uniquement `serverName` pendant la copie de base de données.

*source_database_name*

Nom de la base de données copiée.

## <a name="remarks"></a>Notes

Les bases de données dans [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] ont plusieurs paramètres par défaut définis lors de la création de la base de données. Pour plus d’informations sur ces paramètres par défaut, consultez la liste de valeurs dans [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

`MAXSIZE` permet de limiter la taille de la base de données. Si la taille de la base de données atteint sa valeur `MAXSIZE`, vous recevez un code d’erreur 40544. Lorsque cela se produit, vous ne pouvez pas insérer ou mettre à jour des données, ni créer des objets (tels que des tables, des procédures stockées, des vues et des fonctions). Toutefois, vous pouvez encore lire et supprimer des données, tronquer des tables, supprimer des tables et des index et reconstruire des index. Vous pouvez ensuite mettre à jour `MAXSIZE` avec une valeur supérieure à votre taille de base de données actuelle ou supprimer certaines données afin de libérer de l’espace de stockage. Vous devrez peut-être patienter jusqu'à quinze minutes avant de pouvoir insérer de nouvelles données.

Pour modifier ultérieurement les valeurs de taille, d’édition ou d’objectif de service, utilisez [ALTER DATABASE - Azure SQL Database](../../t-sql/statements/alter-database-transact-sql.md?view=azuresqldb-current&preserve-view=true).

L’argument `CATALOG_COLLATION` est uniquement disponible lors de la création de base de données.

## <a name="database-copies"></a>Copies de bases de données

**S’applique à :** Bases de données uniques et mises en pool uniquement.

La copie d’une base de données à l’aide de l’instruction `CREATE DATABASE` est une opération asynchrone. Par conséquent, une connexion au serveur [!INCLUDE[ssSDS](../../includes/sssds-md.md)] n'est pas nécessaire pendant toute la durée du processus de copie. L’instruction `CREATE DATABASE` redonne le contrôle à l’utilisateur après la création de l’entrée dans sys.databases, mais avant que l’opération de copie de base de données soit terminée. Autrement dit, l'instruction `CREATE DATABASE` est renvoyée avec succès lorsque la copie de base de données est encore en cours.

- Surveillance du processus de copie sur un serveur [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)] : Interroger les colonnes `percentage_complete` ou `replication_state_desc` des [dm_database_copies](../../relational-databases/system-dynamic-management-views/sys-dm-database-copies-azure-sql-database.md) ou de la colonne `state` dans la vue **sys.databases**. La vue [sys.dm_operation_status](../../relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database.md) peut aussi être utilisée, car elle retourne l’état des opérations de base de données, y compris la copie de base de données.

Lorsque le processus de copie est terminé avec succès, la base de données de destination est cohérente avec la base de données source au niveau des transactions.

La syntaxe et les règles sémantiques suivante s'appliquent à votre utilisation de l'argument `AS COPY OF` :

- Le nom du serveur source et le nom du serveur pour la cible de copie peuvent être identiques ou différents. Lorsqu’ils sont identiques, ce paramètre est facultatif et le contexte de serveur de la session active est utilisé par défaut.
- Les noms des bases de données source et de destination doivent être spécifiées, uniques et conformes aux règles applicables aux identificateurs [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour plus d’informations, consultez [Identificateurs](https://go.microsoft.com/fwlink/p/?LinkId=180386).
- L'instruction `CREATE DATABASE` doit être exécutée dans le contexte de la base de données master du serveur [!INCLUDE[ssSDS](../../includes/sssds-md.md)] où la nouvelle base de données sera créée.
- Une fois la copie terminée, la base de données de destination doit être gérée comme une base de données indépendante. Vous pouvez exécuter les instructions `ALTER DATABASE` et `DROP DATABASE` contre la nouvelle base de données indépendamment de la base de données source. Vous pouvez également copier la nouvelle base de données vers une autre nouvelle base de données.
- La base de données source est toujours accessible pendant que la copie de base de données est en cours.

Pour plus d’informations, consultez [Créer une copie de base de données Azure SQL à l’aide de Transact-SQL](https://azure.microsoft.com/documentation/articles/sql-database-copy-transact-sql/).

> [!IMPORTANT]
> Par défaut, la copie de la base de données est créée avec la même redondance du stockage de sauvegarde que celle de la base de données source. La modification de la redondance du stockage de sauvegarde lors de la création d’une copie de base de données n’est pas prise en charge via T-SQL. 


## <a name="permissions"></a>Autorisations

Pour créer une base de données, une connexion doit correspondre à l’un des éléments suivants :

- La connexion du principal au niveau du serveur
- L’administrateur Azure AD pour Azure SQL Server local
- Une connexion qui est membre du rôle de base de données `dbmanager`

**Exigences supplémentaires relatives à l’utilisation de la syntaxe `CREATE DATABASE ... AS COPY OF` :** La connexion exécutant l’instruction sur le serveur local doit également être au moins `db_owner` sur le serveur source. Si la connexion est basée sur l’authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], la connexion exécutant l’instruction sur le serveur local doit avoir une connexion correspondante sur le serveur [!INCLUDE[ssSDS](../../includes/sssds-md.md)] source, avec les mêmes nom et mot de passe.

## <a name="examples"></a>Exemples

### <a name="simple-example"></a>Exemple simple

Voici un exemple simple de création d’une base de données.

```sql
CREATE DATABASE TestDB1;
```

### <a name="simple-example-with-edition"></a>Exemple simple avec une édition

Voici un exemple simple de création d’une base de données d’usage général.

```sql
CREATE DATABASE TestDB2
( EDITION = 'GeneralPurpose' );
```

### <a name="example-with-additional-options"></a>Exemple avec des options supplémentaires

Voici un exemple utilisant plusieurs options.

```sql
CREATE DATABASE hito
COLLATE Japanese_Bushu_Kakusu_100_CS_AS_KS_WS
( MAXSIZE = 500 MB, EDITION = 'GeneralPurpose', SERVICE_OBJECTIVE = 'GP_GEN4_8' ) ;
```

### <a name="creating-a-copy"></a>Création d’une copie

Voici un exemple de création d’une copie de base de données.

**S’applique à :** Bases de données uniques et mises en pool uniquement.

```sql
CREATE DATABASE escuela
AS COPY OF school;
```

### <a name="creating-a-database-in-an-elastic-pool"></a>Création d’une base de données dans un pool élastique

Crée une base de données dans le pool nommé S3M100 :

**S’applique à :** Bases de données uniques et mises en pool uniquement.

```sql
CREATE DATABASE db1 ( SERVICE_OBJECTIVE = ELASTIC_POOL ( name = S3M100 ) ) ;
```

### <a name="creating-a-copy-of-a-database-on-another-server"></a>Création d’une copie de base de données sur un autre serveur

L’exemple suivant crée une copie de la base de données db_original, nommée db_copy dans la taille de calcul P2 (objectif de service) pour une base de données. Cette opération est possible, que db_original se trouve dans un pool élastique ou une taille de calcul (objectif de service) pour une base de données.

**S’applique à :** Bases de données uniques et mises en pool uniquement.

```sql
CREATE DATABASE db_copy
  AS COPY OF ozabzw7545.db_original ( SERVICE_OBJECTIVE = 'P2' );
```

L’exemple suivant crée une copie de la base de données db_original, nommée db_copy, dans le pool élastique nommé ep1. Cette opération est possible, que db_original se trouve dans un pool élastique ou une taille de calcul (objectif de service) pour une base de données. Si db_original se trouve dans un pool élastique avec un nom différent, la création de db_copy est malgré tout effectuée dans ep1.

**S’applique à :** Bases de données uniques et mises en pool uniquement.

```sql
CREATE DATABASE db_copy
  AS COPY OF ozabzw7545.db_original
  (SERVICE_OBJECTIVE = ELASTIC_POOL( name = ep1 ) ) ;
```

### <a name="create-database-with-specified-catalog-collation-value"></a>Créer la base de données avec la valeur de classement de catalogue spécifiée

L’exemple suivant définit le classement de catalogue sur DATABASE_DEFAULT lors de création de bases de données, rendant le classement de catalogue identique au classement de base de données.

```sql
CREATE DATABASE TestDB3 COLLATE Japanese_XJIS_140 (MAXSIZE = 100 MB, EDITION = 'Basic')
  WITH CATALOG_COLLATION = DATABASE_DEFAULT
```

### <a name="create-database-using-zone-redundancy-for-backups"></a>Créer une base de données en utilisant la redondance interzone pour les sauvegardes

L’exemple suivant définit la redondance interzone pour les sauvegardes de base de données. Les sauvegardes de restauration à un point dans le temps et les sauvegardes de conservation à long terme (si elles sont configurées) utilisent la même redondance du stockage de sauvegarde.

```sql
CREATE DATABASE test_zone_redundancy 
  WITH BACKUP_STORAGE_REDUNDANCY = 'ZONE';
```

## <a name="see-also"></a>Voir aussi

- [sys.dm_database_copies - Azure SQL Database](../../relational-databases/system-dynamic-management-views/sys-dm-database-copies-azure-sql-database.md)
- [ALTER DATABASE - Azure SQL Database](alter-database-transact-sql.md?view=azuresqldb-current&preserve-view=true)

::: moniker-end
::: moniker range="=azuresqldb-mi-current"

:::row:::
    :::column:::
        [SQL Server](create-database-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        [Base de données SQL](create-database-transact-sql.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* SQL Managed Instance \*_**
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](create-database-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
    :::column:::
        [Analytics Platform<br />System (PDW)](create-database-transact-sql.md?view=aps-pdw-2016&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="azure-sql-managed-instance"></a>Azure SQL Managed Instance

## <a name="overview"></a>Vue d’ensemble

Dans Azure SQL Managed Instance, cette instruction permet de créer une base de données. La création d’une base de données sur une instance managée implique de spécifier son nom et son classement.

## <a name="syntax"></a>Syntaxe

```syntaxsql
CREATE DATABASE database_name [ COLLATE collation_name ]
[;]
```

> [!IMPORTANT]
> Pour ajouter des fichiers ou définir l’autonomie d’une base de données dans une instance managée, utilisez l’instruction [ALTER DATABASE](alter-database-transact-sql.md?tabs=sqldbmi).

## <a name="arguments"></a>Arguments

*database_name* est le nom de la nouvelle base de données. Ce nom doit être unique sur le serveur SQL et respecter les règles [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] relatives aux identificateurs. Pour plus d’informations, consultez [Identificateurs](https://go.microsoft.com/fwlink/p/?LinkId=180386).

*Collation_name* : spécifie le classement par défaut de la base de données. Le nom du classement peut être un nom de classement Windows ou SQL. S’il est omis, le classement par défaut, SQL_Latin1_General_CP1_CI_AS, est affecté à la base de données.

Pour plus d’informations sur les noms de classements Windows et SQL, consultez [COLLATE (Transact-SQL)](../../t-sql/statements/collations.md).

## <a name="remarks"></a>Notes

Les bases de données dans [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] ont plusieurs paramètres par défaut définis lors de la création de la base de données. Pour plus d’informations sur ces paramètres par défaut, consultez la liste de valeurs dans [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

> [!IMPORTANT]
> L'instruction `CREATE DATABASE` doit être la seule instruction dans un lot [!INCLUDE[tsql](../../includes/tsql-md.md)].

`CREATE DATABASE` présente les limitations suivantes :

- Il n’est pas possible de définir des fichiers ou des groupes de fichiers.
- Les options `WITH` ne sont pas prises en charge.

  > [!TIP]
  > Pour contourner le problème, utilisez [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true) après `CREATE DATABASE` afin de définir les options de base de données et d’ajouter des fichiers.

## <a name="permissions"></a>Autorisations

Pour créer une base de données, une connexion doit correspondre à l’un des éléments suivants :

- La connexion du principal au niveau du serveur
- L’administrateur Azure AD pour Azure SQL Server local
- Une connexion qui est membre du rôle de base de données `dbcreator`

## <a name="examples"></a>Exemples

### <a name="simple-example"></a>Exemple simple

Voici un exemple simple de création d’une base de données.

```sql
CREATE DATABASE TestDB1;
```

## <a name="see-also"></a>Voir aussi

Voir [ALTER DATABASE](alter-database-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true).

::: moniker-end
::: moniker range="=azure-sqldw-latest"

:::row:::
    :::column:::
        [SQL Server](create-database-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        [Base de données SQL](create-database-transact-sql.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [SQL Managed Instance](create-database-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* Azure Synapse<br />Analytics \*_**
    :::column-end:::
    :::column:::
        [Analytics Platform<br />System (PDW)](create-database-transact-sql.md?view=aps-pdw-2016&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="azure-synapse-analytics"></a>Azure Synapse Analytics

## <a name="overview"></a>Vue d’ensemble

Dans Azure Synapse, cette instruction peut être utilisée avec un serveur Azure SQL Database pour créer une base de données SQL Analytics. Cette instruction implique de spécifier le nom, le classement, la taille maximale, l’édition et l’objectif de service de la base de données.

## <a name="syntax"></a>Syntaxe

### <a name="dedicated-sql-pool"></a>[Pool SQL dédié](#tab/sqlpool)
```syntaxsql
CREATE DATABASE database_name [ COLLATE collation_name ]
(
    [ MAXSIZE = {
          250 | 500 | 750 | 1024 | 5120 | 10240 | 20480 | 30720
        | 40960 | 51200 | 61440 | 71680 | 81920 | 92160 | 102400
        | 153600 | 204800 | 245760
      } GB ,
    ]
    EDITION = 'datawarehouse',
    SERVICE_OBJECTIVE = {
         'DW100' | 'DW200' | 'DW300' | 'DW400' | 'DW500' | 'DW600'
        | 'DW1000' | 'DW1200' | 'DW1500' | 'DW2000' | 'DW3000' | 'DW6000'
        |'DW100c' | 'DW200c' | 'DW300c' | 'DW400c' | 'DW500c'
        | 'DW1000c' | 'DW1500c' | 'DW2000c' | 'DW2500c' | 'DW3000c' | 'DW5000c'
        | 'DW6000c' | 'DW7500c' | 'DW10000c' | 'DW15000c' | 'DW30000c'
    }
)
[;]
```
### <a name="serverless-sql-pool"></a>[Pool SQL serverless](#tab/sqlod)
```syntaxsql
CREATE DATABASE database_name [ COLLATE collation_name ]
[;] 
```
---

## <a name="arguments"></a>Arguments

*database_name* est le nom de la nouvelle base de données. Ce nom doit respecter les règles [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] applicables aux identificateurs et être unique sur le serveur SQL qui peut héberger à la fois des bases de données [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] et des bases de données [!INCLUDE[ssSDW](../../includes/sssdw-md.md)]. Pour plus d’informations, consultez [Identificateurs](https://go.microsoft.com/fwlink/p/?LinkId=180386).

*collation_name* spécifie le classement par défaut de la base de données. Le nom du classement peut être un nom de classement Windows ou SQL. S’il est omis, le classement par défaut, SQL_Latin1_General_CP1_CI_AS, est affecté à la base de données.

Pour plus d’informations sur les noms de classements Windows et SQL, consultez [COLLATE (Transact-SQL)](https://msdn.microsoft.com/library/ms184391.aspx).

*EDITION* spécifie la couche de service de la base de données. Pour [!INCLUDE[ssSDW](../../includes/sssdw-md.md)], utilisez « datawarehouse ».

*MAXSIZE* La valeur par défaut est 245 760 Go (240 To).

**S’applique à :** Optimisé pour le calcul Gen1

Taille maximale autorisée pour la base de données. La base de données ne peut pas croître au-delà de MAXSIZE.

**S’applique à :** Optimisé pour le calcul Gen2

Taille maximale autorisée pour les données rowstore dans la base de données. Les données stockées dans les tables rowstore, dans un deltastore d’index columnstore ou un index non cluster sur un index columnstore cluster, ne peuvent pas croître au-delà de MAXSIZE. Les données compressées au format columnstore n’ont pas de taille limite et ne sont pas restreintes par MAXSIZE.

SERVICE_OBJECTIVE spécifie la taille de calcul (objectif de service). Pour plus d’informations sur les objectifs de service d’Azure Synapse, voir [Data Warehouse Units (DWU)](https://docs.microsoft.com/azure/sql-data-warehouse/what-is-a-data-warehouse-unit-dwu-cdwu).

## <a name="general-remarks"></a>Remarques d'ordre général

Utilisez [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md) pour afficher les propriétés de la base de données.

Utilisez [ALTER DATABASE - Azure Synapse Analytics](../../t-sql/statements/alter-database-transact-sql.md?view=aps-pdw-2016-au7&preserve-view=true) pour changer la taille maximale ou les valeurs des objectifs de service par la suite.

Azure Synapse est défini sur COMPATIBILITY_LEVEL 130 et ne peut pas être modifié. Pour plus d’informations, consultez [Meilleures performances des requêtes avec le niveau de compatibilité 130 dans Azure SQL Database](https://azure.microsoft.com/documentation/articles/sql-database-compatibility-level-query-performance-130/).

## <a name="permissions"></a>Autorisations

Autorisations nécessaires :

- Connexion au principal de niveau serveur, créé par le processus de déploiement ou
- Membre du rôle de base de données `dbmanager`.

## <a name="error-handling"></a>Gestion des erreurs

Si la taille de la base de données atteint la valeur MAXSIZE, vous recevez un code d’erreur 40544. Lorsque cela se produit, vous ne pouvez pas insérer ou mettre à jour des données, ni créer des objets (tels que des tables, des procédures stockées, des vues et des fonctions). Vous pouvez toujours lire et supprimer des données, tronquer des tables, supprimer des tables et des index, et reconstruire des index. Vous pouvez ensuite mettre à jour MAXSIZE avec une valeur supérieure à votre taille de base de données actuelle ou supprimer certaines données afin de libérer de l'espace de stockage. Vous devrez peut-être patienter jusqu'à quinze minutes avant de pouvoir insérer de nouvelles données.

## <a name="limitations-and-restrictions"></a>Limitations et restrictions

Vous devez être connecté à la base de données master pour créer une base de données.

L'instruction `CREATE DATABASE` doit être la seule instruction dans un lot [!INCLUDE[tsql](../../includes/tsql-md.md)].

Vous ne pouvez pas modifier le classement de la base de données après la création de celle-ci.

## <a name="examples-sssdwfull"></a>Exemples : [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)]

### <a name="a-simple-example"></a>R. Exemple simple
Voici un exemple simple de création d’une base de données de l’entrepôt de données. La base de données est créée avec la plus petite taille maximale (10 240 Go), le classement par défaut (SQL_Latin1_General_CP1_CI_AS) et la plus petite puissance de calcul (DW100).

```sql
CREATE DATABASE TestDW
(EDITION = 'datawarehouse', SERVICE_OBJECTIVE='DW100');
```

### <a name="b-create-a-data-warehouse-database-with-all-the-options"></a>B. Créer une base de données de l’entrepôt de données avec toutes les options
Voici un exemple de création d’un entrepôt de données de 10 téraoctets en utilisant toutes les options.

```sql
CREATE DATABASE TestDW COLLATE Latin1_General_100_CI_AS_KS_WS
(MAXSIZE = 10240 GB, EDITION = 'datawarehouse', SERVICE_OBJECTIVE = 'DW1000');
```

## <a name="see-also"></a>Voir aussi

- [ALTER DATABASE- Azure Synapse Analytics](../../t-sql/statements/alter-database-transact-sql.md?view=aps-pdw-2016-au7&preserve-view=true)
- [CREATE TABLE- Azure Synapse Analytics](../../t-sql/statements/create-table-azure-sql-data-warehouse.md)
- [DROP DATABASE - Transact-SQL](../../t-sql/statements/drop-database-transact-sql.md)

::: moniker-end
::: moniker range=">=aps-pdw-2016"

:::row:::
    :::column:::
        [SQL Server](create-database-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        [Base de données SQL](create-database-transact-sql.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [SQL Managed Instance](create-database-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](create-database-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* Système de plateforme<br />d’analyse (PDW) \*_**
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="analytics-platform-system"></a>Système de la plateforme d'analyse

## <a name="overview"></a>Vue d’ensemble

Dans le système de plateforme d’analyse, cette instruction permet de créer une nouvelle base de données sur une appliance Système de plateforme d’analyse. Utilisez cette instruction pour créer tous les fichiers associés à une base de données d’appliance, et pour définir les options de croissance automatique et de taille maximale pour les tables de base de données et le journal des transactions.

## <a name="syntax"></a>Syntaxe

```syntaxsql
CREATE DATABASE database_name
WITH (
    [ AUTOGROW = ON | OFF , ]
    REPLICATED_SIZE = replicated_size [ GB ] ,
    DISTRIBUTED_SIZE = distributed_size [ GB ] ,
    LOG_SIZE = log_size [ GB ] )
[;]
```

## <a name="arguments"></a>Arguments

*database_name* est le nom de la nouvelle base de données. Pour plus d’informations sur les noms de bases de données autorisés, consultez « Règles de nommage d’objet » et « Noms de bases de données réservés » dans la [!INCLUDE[pdw-product-documentation](../../includes/pdw-product-documentation-md.md)].

AUTOGROW = ON | **OFF** spécifie si les paramètres *replicated_size*, *distributed_size* et *log_size* pour cette base de données croissent automatiquement en fonction des besoins au-delà de leur taille spécifiée. La valeur par défaut est **OFF**.

Si AUTOGROW est ON, *replicated_size*, *distributed_size*, et *log_size* croissent en fonction des besoins (et non par blocs de la taille spécifiée initiale) lors de chaque insertion de données, mise à jour ou autre action nécessitant davantage de stockage que ce qui a déjà été alloué.

Si AUTOGROW est OFF, les tailles n’augmentent pas automatiquement. [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] retourne une erreur en cas de tentative d’exécution d’une action qui exige que *replicated_size*, *distributed_size* ou *log_size* croisse au-delà de sa valeur spécifiée.

AUTOGROW est ON pour toutes les tailles ou OFF pour toutes les tailles. Par exemple, vous ne pouvez pas définir AUTOGROW ON pour *log_size* sans le définir également pour *replicated_size*.

*replicated_size* [Go] est un nombre positif. Définit la taille (en gigaoctets entier ou décimal) pour l’espace total alloué aux tables répliquées et aux données correspondantes *sur chaque nœud de calcul*. Pour plus d’informations sur les exigences liées aux valeurs *replicated_size* minimales et maximales, consultez « Valeurs minimales et maximales » dans la [!INCLUDE[pdw-product-documentation](../../includes/pdw-product-documentation-md.md)].

Si AUTOGROW est ON, les tables répliquées sont autorisées à croître au-delà de cette limite.

Si AUTOGROW est OFF, une erreur est retournée si un utilisateur tente de créer une table répliquée, d’insérer des données dans une table répliquée existante, ou de mettre à jour une table répliquée existante d’une manière qui augmenterait la taille au-delà de *replicated_size*.

*distributed_size* [Go] est un nombre positif. Taille (en gigaoctets entier ou décimal) pour l’espace total alloué aux tables distribuées et aux données correspondantes *à l’échelle de l’appliance*. Pour plus d’informations sur les exigences liées aux valeurs *distributed_size* minimales et maximales, consultez « Valeurs minimales et maximales » dans la [!INCLUDE[pdw-product-documentation](../../includes/pdw-product-documentation-md.md)].

Si AUTOGROW est ON, les tables distribuées sont autorisées à croître au-delà de cette limite.

Si AUTOGROW est OFF, une erreur est retournée si un utilisateur tente de créer une table distribuée, d’insérer des données dans une table distribuée existante, ou de mettre à jour une table distribuée existante d’une manière qui augmenterait la taille au-delà de *replicated_size*.

*log_size* [Go] est un nombre positif. Taille (en gigaoctets entier ou décimal) du journal des transactions *à l’échelle de l’appliance*.

Pour plus d’informations sur les exigences liées aux valeurs *log_size* minimales et maximales, consultez « Valeurs minimales et maximales » dans la [!INCLUDE[pdw-product-documentation](../../includes/pdw-product-documentation-md.md)].

Si AUTOGROW est ON, le fichier journal est autorisé à croître au-delà de cette limite. Utilisez l’instruction [DBCC SHRINKLOG (Azure Synapse Analytics)](../../t-sql/database-console-commands/dbcc-shrinklog-azure-sql-data-warehouse.md) afin de réduire la taille des fichiers journaux à leur taille d’origine.

Si AUTOGROW est OFF, une erreur est retournée pour toute action qui augmenterait la taille du journal sur un nœud de calcul au-delà de *log_size*.

## <a name="permissions"></a>Autorisations

Nécessite l’autorisation `CREATE ANY DATABASE` dans la base de données master ou l’appartenance au rôle serveur fixe **sysadmin**.

L'exemple suivant fournit l'autorisation de créer une base de données à l'utilisateur de base de données Fay.

```sql
USE master;
GO
GRANT CREATE ANY DATABASE TO [Fay];
GO
```

## <a name="general-remarks"></a>Remarques d'ordre général

Les bases de données sont créées avec le niveau de compatibilité de base de données 120, qui est le niveau de compatibilité pour [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]. Cela garantit que la base de données pourra utiliser toutes les fonctionnalités de [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] utilisées par PDW.

## <a name="limitations-and-restrictions"></a>Limitations et restrictions

L’instruction CREATE DATABASE n’est pas autorisée dans une transaction explicite. Pour plus d’informations, consultez [Instructions](../../t-sql/statements/statements.md).

Pour plus d’informations sur les contraintes minimales et maximales sur les bases de données, consultez « Valeurs minimales et maximales » dans la [!INCLUDE[pdw-product-documentation](../../includes/pdw-product-documentation-md.md)].

Lors de la création d’une base de données, il doit y avoir suffisamment d’espace libre disponible *sur chaque nœud de calcul* pour allouer le total combiné des tailles suivantes :

- Base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] avec des tables de la taille de *replicated_table_size*.
- Base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] avec des tables de la taille de (*distributed_table_size* / nombre de nœuds de calcul).
- Journaux [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de la taille de (*log_size* / nombre de nœuds de calcul).

## <a name="locking"></a>Verrouillage

Prend un verrou partagé sur l’objet DATABASE.

## <a name="metadata"></a>Métadonnées

Une fois cette opération réussie, une entrée pour cette base de données apparaît dans les vues de métadonnées [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) et [sys.objects](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md).

## <a name="examples-sspdw"></a>Exemples : [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]

### <a name="a-basic-database-creation-examples"></a>R. Exemples simples de création de base de données

L’exemple suivant crée la base de données `mytest` avec une allocation de stockage de 100 Go par nœud de calcul pour les tables répliquées, 500 Go par appliance pour les tables distribuées et 100 Go par appliance pour le journal des transactions. Dans cet exemple, AUTOGROW est OFF par défaut.

```sql
CREATE DATABASE mytest
  WITH
    (REPLICATED_SIZE = 100 GB,
    DISTRIBUTED_SIZE = 500 GB,
    LOG_SIZE = 100 GB );
```

L’exemple suivant crée la base de données `mytest` avec les mêmes paramètres que ci-dessus, sauf que AUTOGROW est ON. Cela permet à la base de données de croître au-delà des paramètres de taille spécifiés.

```sql
CREATE DATABASE mytest
  WITH
    (AUTOGROW = ON,
    REPLICATED_SIZE = 100 GB,
    DISTRIBUTED_SIZE = 500 GB,
    LOG_SIZE = 100 GB);
```

### <a name="b-creating-a-database-with-partial-gigabyte-sizes"></a>B. Création d’une base de données avec des valeurs de taille en gigaoctets partielles

L’exemple suivant crée la base de données `mytest` avec AUTOGROW OFFn une allocation de stockage de 1,5 Go par nœud de calcul pour les tables répliquées, 5,25 Go par appliance pour les tables distribuées et 10 Go par appliance pour le journal des transactions.

```sql
CREATE DATABASE mytest
  WITH
    (REPLICATED_SIZE = 1.5 GB,
    DISTRIBUTED_SIZE = 5.25 GB,
    LOG_SIZE = 10 GB);
```

## <a name="see-also"></a>Voir aussi

- [ALTER DATABASE - Système de plateforme d’analyse](../../t-sql/statements/alter-database-transact-sql.md?view=aps-pdw-2016-au7&preserve-view=true)
- [DROP DATABASE](../../t-sql/statements/drop-database-transact-sql.md)

::: moniker-end
