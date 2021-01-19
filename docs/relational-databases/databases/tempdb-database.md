---
title: base de données tempdb | Microsoft Docs
description: Cette rubrique fournit des détails sur la configuration et l’utilisation de la base de données tempdb dans SQL Server et Azure SQL Database.
ms.custom: P360
ms.date: 09/16/2020
ms.prod: sql
ms.prod_service: database-engine
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- temporary tables [SQL Server], tempdb database
- tempdb database [SQL Server], about tempdb
- temporary stored procedures [SQL Server]
- tempdb database [SQL Server]
ms.assetid: ce4053fb-e37a-4851-b711-8e504059a780
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 910db0887a50e12163ddeda927159f5e6c97b74b
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/13/2021
ms.locfileid: "98169277"
---
# <a name="tempdb-database"></a>base de données tempdb

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

La base de données système `tempdb` est une ressource globale à la disposition de tous les utilisateurs qui sont connectés à l’instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou à Azure SQL Database. `tempdb` comprend :  
  
- les *objets utilisateurs* temporaires créés explicitement. Ils incluent les tables et index temporaires locaux ou globaux, les procédures stockées temporaires, les variables de table, les tables retournées dans des fonctions table et les curseurs.  
- Les *objets internes* créés par le moteur de base de données. Elles comprennent :
  - Les tables de travail afin de stocker les résultats intermédiaires pour les mises en spools, les curseurs, les tris et le stockage temporaire des des objets volumineux (LOB).
  - les fichiers de travail correspondant aux opérations de jointures ou d'agrégations hachées ;
  - les résultats de tris intermédiaires pour les opérations de création ou de reconstruction d'index (si `SORT_IN_TEMPDB` est spécifié) ou pour certaines requêtes `GROUP BY`, `ORDER BY` ou `UNION`.

  Chaque objet interne utilise un minimum de neuf pages : une page IAM et une étendue de huit pages. Pour plus d’informations sur les pages et les extensions, consultez [Pages et étendues](../../relational-databases/pages-and-extents-architecture-guide.md#pages-and-extents).

  > [!IMPORTANT]
  > Les pools élastiques et les bases de données uniques Azure SQL Database prennent en charge les tables temporaires globales et les procédures stockées temporaires globales qui sont stockées dans `tempdb` et dont l’étendue est limitée à la base de données. 
  >
  > Les tables temporaires globales et les procédures stockées temporaires globales sont partagées pour toutes les sessions utilisateur exécutées dans la même instance de base de données SQL. Les sessions utilisateur d’autres instances de bases de données SQL n’ont pas accès aux tables temporaires globales. Pour plus d’informations, consultez [Database scoped global temporary tables (Azure SQL Database)](../../t-sql/statements/create-table-transact-sql.md#database-scoped-global-temporary-tables-azure-sql-database). [Azure SQL Managed Instance](/azure/sql-database/sql-database-managed-instance) prend en charge les mêmes objets temporaires que SQL Server.
  >
  > Pour les pools élastiques et les bases de données uniques Azure SQL Database, seules les bases de données MASTER et `tempdb` s’appliquent. Pour plus d’informations, consultez [Qu’est-ce qu’un serveur Azure SQL Database ?](/azure/sql-database/sql-database-servers-databases#what-is-an-azure-sql-database-server). Pour une présentation de `tempdb` dans le contexte des bases de données uniques et des pools élastiques Azure SQL Database, consultez [Base de données tempdb dans les bases de données uniques et les pools élastiques Azure SQL Database](#tempdb-database-in-sql-database). 
  >
  > Pour Azure SQL Managed Instance, toutes les bases de données système s’appliquent.

- Des *banques de versions*, qui sont des collections de pages de données contenant les lignes de données qui prennent en charge des fonctionnalités de contrôle de version de ligne. Il y a deux types de banques : une banque de versions commune et une banque de versions de construction d'index en ligne. Les banques de versions contiennent les éléments suivants :
  - Les versions de ligne générées par les transactions de modification de données dans une base de données qui utilise `READ COMMITTED` via l'isolement basé sur le contrôle de version de ligne ou les transactions d'isolement d'instantané.  
  - Versions de ligne qui sont générées par les transactions de modification de données pour les fonctionnalités telles que : opérations d'index en ligne, MARS (Multiple Active Result Sets) et déclencheurs `AFTER`.  
  
Les opérations effectuées dans `tempdb` font l’objet d’un enregistrement minimal pour permettre la restauration des transactions. `tempdb` étant recréée chaque fois que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est démarré, le système démarre toujours avec une copie propre de la base de données. Les tables et les procédures stockées temporaires sont automatiquement supprimées à la déconnexion et aucune connexion n'est active lorsque le système est arrêté. 

`tempdb` n’a jamais rien à enregistrer d’une session de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à l’autre. La sauvegarde et la restauration ne sont pas autorisées pour la base de données `tempdb`.  

## <a name="physical-properties-of-tempdb-in-sql-server"></a>Propriétés physiques de tempdb dans SQL Server

Le tableau suivant répertorie les valeurs de configuration initiales des fichiers de données et des journaux de `tempdb` dans SQL Server. Les valeurs sont basées sur les valeurs par défaut de la base de données `model`. La taille de ces fichiers peut varier légèrement en fonction des éditions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
|Fichier|Nom logique|Nom physique|Taille initiale|Croissance du fichier|  
|----------|------------------|-------------------|------------------|-----------------|  
|Données primaires|tempdev|tempdb.mdf|8 mégaoctets|Croissance automatique de 64 Mo jusqu’à saturation du disque.|  
|Fichiers de données secondaires|temp#|tempdb_mssql_#.ndf|8 mégaoctets|Croissance automatique de 64 Mo jusqu’à saturation du disque.|  
|Journal|templog|templog.ldf|8 mégaoctets|Croissance automatique de 64 mégaoctets jusqu’à un maximum de 2 téraoctets.|  
  
Le nombre de fichiers de données secondaires dépend du nombre de processeurs (logiques) sur l’ordinateur. En règle générale, si le nombre de processeurs logiques est inférieur ou égal à huit, utilisez le même nombre de fichiers de données que de processeurs logiques. Si le nombre de processeurs logiques est supérieur à huit, utilisez huit fichiers de données. Si le conflit persiste, augmentez le nombre de fichiers de données par multiples de quatre jusqu’à ce que le conflit diminue à un niveau acceptable ou modifiez la charge de travail/le code.

> [!NOTE]
> La valeur par défaut du nombre de fichiers de données est basée sur les directives générales de l’article [KB 2154845](https://support.microsoft.com/kb/2154845/).  
  
### <a name="moving-the-tempdb-data-and-log-files-in-sql-server"></a>Déplacement des fichiers de données et journaux de tempdb dans SQL Server

Pour déplacer les données de `tempdb` et les fichiers journaux, consultez [Déplacer des bases de données système](../../relational-databases/databases/move-system-databases.md).  
  
### <a name="database-options-for-tempdb-in-sql-server"></a>Options de la base de données pour tempdb dans SQL Server

Le tableau ci-dessous indique la valeur par défaut de chaque option de la base de données `tempdb`, et précise si cette option est modifiable. Pour afficher les valeurs actuelles de ces options, utilisez l'affichage catalogue [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) .  
  
|Option de base de données|Valeur par défaut|Peut être modifiée|  
|---------------------|-------------------|---------------------|  
|ALLOW_SNAPSHOT_ISOLATION|OFF|Oui|  
|ANSI_NULL_DEFAULT|OFF|Oui|  
|ANSI_NULLS|OFF|Oui|  
|ANSI_PADDING|OFF|Oui|  
|ANSI_WARNINGS|OFF|Oui|  
|ARITHABORT|OFF|Oui|  
|AUTO_CLOSE|OFF|Non|  
|AUTO_CREATE_STATISTICS|ACTIVÉ|Oui|  
|AUTO_SHRINK|OFF|Non|  
|AUTO_UPDATE_STATISTICS|ACTIVÉ|Oui|  
|AUTO_UPDATE_STATISTICS_ASYNC|OFF|Oui|  
|CHANGE_TRACKING|OFF|Non|  
|CONCAT_NULL_YIELDS_NULL|OFF|Oui|  
|CURSOR_CLOSE_ON_COMMIT|OFF|Oui|  
|CURSOR_DEFAULT|GLOBAL|Oui|  
|Options de disponibilité de base de données|ONLINE<br /><br /> MULTI_USER<br /><br /> READ_WRITE|Non<br /><br /> Non<br /><br /> Non|  
|DATE_CORRELATION_OPTIMIZATION|OFF|Oui|  
|DB_CHAINING|ACTIVÉ|Non|  
|ENCRYPTION|OFF|Non|  
|MIXED_PAGE_ALLOCATION|OFF|Non|  
|NUMERIC_ROUNDABORT|OFF|Oui|  
|PAGE_VERIFY|CHECKSUM pour les nouvelles installations de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]<br /><br /> NONE pour les mises à niveau de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|Oui|  
|PARAMETERIZATION|SIMPLE|Oui|  
|QUOTED_IDENTIFIER|OFF|Oui|  
|READ_COMMITTED_SNAPSHOT|OFF|Non|  
|RECOVERY|SIMPLE|Non|  
|RECURSIVE_TRIGGERS|OFF|Oui|  
|Options de Service Broker|ENABLE_BROKER|Oui|  
|TRUSTWORTHY|OFF|Non|  
  
Pour obtenir une description de ces options de base de données, consultez [Options ALTER DATABASE SET (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-set-options.md).  
  
## <a name="tempdb-database-in-sql-database"></a>Base de données tempdb dans SQL Database

### <a name="tempdb-sizes-for-dtu-based-service-tiers"></a>Tailles de tempdb pour les niveaux de service basés sur DTU

<!-- tempdb being larger for Basic and 50 eDTU pools than for 100-400 eDTU pools reflects actual config (historical reasons) --> 

|Objectif de niveau de service|Taille maximale du fichier `tempdb` (Go)|Nombre de fichiers de données `tempdb`|Taille maximale des données `tempdb` (Go)|
|---|---:|---:|---:|
|De base|13.9|1|13.9|
|S0|13.9|1|13.9|
|S1|13.9|1|13.9|
|S2|13.9|1|13.9|
|S3|32|1|32
|S4|32|2|64|
|S6|32|3|96|
|S7|32|6|192|
|S9|32|12|384|
|S12|32|12|384|
|P1|13.9|12|166.7|
|P2|13.9|12|166.7|
|P4|13.9|12|166.7|
|P6|13.9|12|166.7|
|P11|13.9|12|166.7|
|P15|13.9|12|166.7|
|Pools élastiques De base (toutes les configurations de DTU)|13.9|12|166.7|
|Pools élastiques standard (50 eDTU)|13.9|12|166.7|
|Pools élastiques standard (100 eDTU)|32|1|32|
|Pools élastiques standard (200 eDTU)|32|2|64|
|Pools élastiques standard (300 eDTU)|32|3|96|
|Pools élastiques standard (400 eDTU)|32|3|96|
|Pools élastiques standard (800 eDTU)|32|6|192|
|Pools élastiques standard (1200 eDTU)|32|10|320|
|Pools élastiques standard (1600-3000 eDTU)|32|12|384|
|Pools élastiques Premium (toutes les configurations de DTU)|13.9|12|166.7|
||||

### <a name="tempdb-sizes-for-vcore-based-service-tiers"></a>Tailles de tempdb pour les niveaux de service basés sur vCore

Consultez les [limites des ressources basées sur vCore](/azure/sql-database/sql-database-vcore-resource-limits).

## <a name="restrictions"></a>Restrictions

Les opérations suivantes ne peuvent pas être effectuées sur la base de données `tempdb` :  
  
- Ajout de groupes de fichiers
- Sauvegarde ou restauration de la base de données
- Modification du classement. Le classement par défaut est le classement du serveur.
- Modification du propriétaire de la base de données. La base de données `tempdb` appartient à *sa*.
- Création d'un instantané de base de données
- Suppression de la base de données
- Suppression de l'utilisateur *Invité* de la base de données
- Activation de la capture des changements de données.
- Participation à la mise en miroir de bases de données
- Suppression du groupe de fichiers primaire, du fichier de données primaire ou du fichier journal
- Changement du nom de la base de données ou du groupe de fichiers primaire
- Exécution de `DBCC CHECKALLOC`.
- Exécution de `DBCC CHECKCATALOG`.
- Définition de la base de données sur `OFFLINE`.
- Définition de la base de données ou du groupe de fichiers primaire sur `READ_ONLY`.
  
## <a name="permissions"></a>Autorisations

Tous les utilisateurs peuvent créer des objets temporaires dans `tempdb`. Les utilisateurs n'ont accès qu'aux objets qu'ils possèdent, sauf s'ils ont reçu des autorisations supplémentaires. Il est possible de révoquer l’autorisation de connexion à `tempdb` pour empêcher un utilisateur d’utiliser `tempdb`. Cela n’est pas recommandé, car certaines opérations de routine nécessitent l’utilisation de `tempdb`.  

## <a name="optimizing-tempdb-performance-in-sql-server"></a>Optimisation des performances de tempdb dans SQL Server
La taille et l’emplacement physique de la base de données `tempdb` peuvent influer sur les performances d’un système. Par exemple, si la taille définie pour `tempdb` est trop petite, il se peut qu'à chaque redémarrage de l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], une partie de la charge de traitement du système soit absorbée par l'ajustement automatique de `tempdb` à la taille nécessaire à la gestion de la charge de travail.

Si possible, utilisez [l’initialisation instantanée de fichiers](../../relational-databases/databases/database-instant-file-initialization.md) pour améliorer les performances des opérations de croissance de fichiers de données.

Pré-allouez l’espace de tous les fichiers de `tempdb` en définissant leur taille avec une valeur suffisamment élevée pour assumer la charge de travail habituelle de l’environnement. La préallocation permet de limiter le rythme de croissance de `tempdb` pour ne pas impacter les performances. La base de données `tempdb` doit être définie de façon à autoriser la croissance automatique pour augmenter l’espace disque en cas d’exceptions non prévues.

Les fichiers de données doivent être de taille égale dans chaque [groupe de fichiers](../../relational-databases/databases/database-files-and-filegroups.md#filegroups), parce que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilise un algorithme de remplissage proportionnel qui privilégie les allocations dans les fichiers ayant davantage d’espace libre. Le fait de diviser `tempdb` en plusieurs fichiers de données de taille égale procure un niveau élevé d’efficacité parallèle dans les opérations qui utilisent `tempdb`.

Définissez l’incrément de croissance de la taille du fichier avec une taille suffisante afin d’empêcher que la valeur de croissance des fichiers de la base de données `tempdb` ne soit trop faible. Si, au vu de la quantité de données à écrire dans `tempdb`, l'incrément de croissance est trop faible, `tempdb` risque d'avoir à se développer en permanence. Les performances seront affectées.

Pour vérifier les paramètres actuels de croissance et de taille de `tempdb`, utilisez la requête suivante :

```sql
 SELECT name AS FileName,
    size*1.0/128 AS FileSizeInMB,
    CASE max_size
        WHEN 0 THEN 'Autogrowth is off.'
        WHEN -1 THEN 'Autogrowth is on.'
        ELSE 'Log file grows to a maximum size of 2 TB.'
    END,
    growth AS 'GrowthValue',
    'GrowthIncrement' =
        CASE
            WHEN growth = 0 THEN 'Size is fixed.'
            WHEN growth > 0 AND is_percent_growth = 0
                THEN 'Growth value is in 8-KB pages.'
            ELSE 'Growth value is a percentage.'
        END
FROM tempdb.sys.database_files;
GO
```

Placez la base de données `tempdb` sur un sous-système d’E/S rapide. Si plusieurs disques sont directement attachés, utilisez l'agrégation de disques. Il n’est pas obligatoire que les fichiers ou groupes de fichiers de données `tempdb` se trouvent sur des disques ou des broches différents, sauf si vous observez également des goulots d’étranglement d’E/S.

Placez la base de données `tempdb` sur des disques différents de ceux que les bases de données utilisateur emploient.

## <a name="performance-improvements-in-tempdb-for-sql-server"></a>Amélioration des performances dans tempdb pour SQL Server
À compter de [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)], les performances de `tempdb` sont optimisées de la façon suivante :  
  
- Les tables temporaires et les variables de table sont mises en cache. La mise en cache permet aux opérations de création et de suppression des objets temporaires de s'exécuter très rapidement. La mise en cache réduit également l’allocation de pages et les conflits de métadonnées.  
- Le protocole de verrouillage des pages d’allocation a été amélioré pour réduire le nombre de verrous `UP` (update) utilisés.  
- La surcharge d’enregistrement pour `tempdb` a été réduite pour consommer moins de bande passante d’E/S disque sur le fichier journal `tempdb`.  
- Le programme d’installation ajoute plusieurs fichiers de données `tempdb` lors de l’installation d’une nouvelle instance. Vous pouvez effectuer cette tâche à l’aide du nouveau contrôle d’entrée de l’interface utilisateur dans la section **Configuration du moteur de base de données** et du paramètre de ligne de commande `/SQLTEMPDBFILECOUNT`. Par défaut, le programme d’installation ajoute huit fichiers de données `tempdb` ou autant de fichiers de données que de processeurs logiques, la valeur la plus petite étant retenue.  
- S’il y a plusieurs fichiers de données `tempdb`, tous les fichiers continuent de croître automatiquement de la même manière et en même temps, sur la base des paramètres de croissance définis. [L’indicateur de trace 1117](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) n’est plus nécessaire.  
- Toutes les allocations dans `tempdb` utilisent des extensions uniformes. [L’indicateur de trace 1118](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) n’est plus nécessaire.  
- Pour le groupe de fichiers primaire, la propriété `AUTOGROW_ALL_FILES` est activée et la propriété ne peut pas être modifiée.

Pour plus d’informations sur les améliorations des performances dans `tempdb`, consultez l’article de blog [TEMPDB – Fichiers, indicateurs de traces et mises à jour, Oh My !](/archive/blogs/sql_server_team/tempdb-files-and-trace-flags-and-updates-oh-my).

## <a name="memory-optimized-tempdb-metadata"></a>Métadonnées tempdb à mémoire optimisée
La contention de métadonnées dans `tempdb` a toujours été un goulot d’étranglement pour la scalabilité de nombreuses charges de travail s’exécutant sur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] introduit une nouvelle fonctionnalité qui fait partie de la famille de fonctionnalités [base de données en mémoire](../in-memory-database.md) : métadonnées tempdb à mémoire optimisée. 

Cette fonctionnalité supprime efficacement ce goulot d’étranglement et déverrouille un nouveau niveau d’évolutivité pour les charges de travail lourdes dans tempdb. Dans [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)], les tables système impliquées dans la gestion des métadonnées de table temporaire peuvent être déplacées dans des tables à mémoire optimisée non durables, dépourvues de verrous.

Regardez cette vidéo de sept minutes pour obtenir une vue d’ensemble des scénarios et du mode d’utilisation des métadonnées tempdb à mémoire optimisée :

> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/How-and-When-To-Memory-Optimized-TempDB-Metadata/player?WT.mc_id=dataexposed-c9-niner]


### <a name="configuring-and-using-memory-optimized-tempdb-metadata"></a>Configuration et utilisation des métadonnées tempdb à mémoire optimisée

Pour bénéficier de cette nouvelle fonctionnalité, utilisez le script suivant :

```sql
ALTER SERVER CONFIGURATION SET MEMORY_OPTIMIZED TEMPDB_METADATA = ON;
```

Cette modification de la configuration nécessite un redémarrage du service.

Vous pouvez vérifier si `tempdb` est à mémoire optimisée à l’aide de la commande T-SQL suivante :

```sql
SELECT SERVERPROPERTY('IsTempdbMetadataMemoryOptimized');
```

Si le serveur ne parvient pas à démarrer pour une raison quelconque après que vous avez activé des métadonnées `tempdb` à mémoire optimisée, vous pouvez ignorer la fonctionnalité en démarrant l’instance SQL Server avec la [configuration minimale](../../database-engine/configure-windows/start-sql-server-with-minimal-configuration.md) via l’option de démarrage **-f**. Vous pouvez alors désactiver la fonctionnalité et redémarrer SQL Server en mode normal.

Pour protéger le serveur contre les conditions de mémoire insuffisante potentielles, vous pouvez lier `tempdb` à une [liste de ressources partagées](../in-memory-oltp/bind-a-database-with-memory-optimized-tables-to-a-resource-pool.md). Pour ce faire, utilisez la commande [`ALTER SERVER`](../../t-sql/statements/alter-server-configuration-transact-sql.md) plutôt que les étapes suivies habituellement pour lier un pool de ressources partagées à une base de données.

```sql
ALTER SERVER CONFIGURATION SET MEMORY_OPTIMIZED TEMPDB_METADATA = ON (RESOURCE_POOL = 'pool_name');
```

Cette modification nécessite également un redémarrage, même si les métadonnées tempdb à mémoire optimisée sont déjà activées.

### <a name="memory-optimized-tempdb-limitations"></a>LImitations tempdb à mémoire optimisée

- L’activation et la désactivation de la fonctionnalité ne sont pas dynamiques. En raison des modifications intrinsèques qui doivent être apportées à la structure de `tempdb`, un redémarrage est nécessaire pour activer ou désactiver la fonctionnalité.

- Une transaction n’est pas autorisée à accéder aux tables à mémoire optimisée dans plus d’une base de données. Toute transaction qui implique une table à mémoire optimisée dans une base de données utilisateur ne peut pas parallèlement accéder à des vues système `tempdb`. Si vous essayez d’accéder à des vues système `tempdb` dans la même transaction qu’une table à mémoire optimisée dans une base de données utilisateur, vous recevez l’erreur suivante :
    
  ```
  A user transaction that accesses memory optimized tables or natively compiled modules cannot access more than one user database or databases model and msdb, and it cannot write to master.
  ```
    
  Exemple :
    
  ```sql
  BEGIN TRAN;
  
  SELECT *
  FROM tempdb.sys.tables;  -----> Creates a user in-memory OLTP transaction in tempdb
  
  INSERT INTO <user database>.<schema>.<mem-optimized table>
  VALUES (1); ----> Tries to create a user in-memory OLTP transaction in the user database but will fail
  
  COMMIT TRAN;
  ```
    
- Les requêtes exécutées sur les tables à mémoire optimisée ne prennent pas en charge les indicateurs de verrouillage et d’isolation. Les requêtes sur les vues de catalogue `tempdb` à mémoire optimisée ne respectent donc pas les indicateurs de verrouillage et d’isolation. Comme avec les autres vues de catalogue système dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], toutes les transactions sur des vues système sont effectuées au niveau de l’isolation`READ COMMITTED` (ou `READ COMMITTED SNAPSHOT` dans ce cas).

- Les [index columnstore](../indexes/columnstore-indexes-overview.md) ne peuvent pas être créés sur les tables temporaires quand les métadonnées `tempdb` à mémoire optimisée sont activées.

- En raison de la limitation sur les index columnstore, l’utilisation de la procédure stockée système `sp_estimate_data_compression_savings` avec le paramètre de compression de données `COLUMNSTORE` ou `COLUMNSTORE_ARCHIVE` n’est pas prise en charge lorsque les métadonnées `tempdb` à mémoire optimisée sont activées.

> [!NOTE] 
> Ces limitations s’appliquent uniquement lorsque vous référencez des vues système `tempdb`. Si vous le souhaitez, vous pouvez créer une table temporaire dans la même transaction lorsque vous accédez à une table à mémoire optimisée dans une base de données utilisateur.

## <a name="capacity-planning-for-tempdb-in-sql-server"></a>Planification de la capacité de tempdb dans SQL Server
La détermination de la taille appropriée pour `tempdb` dans un environnement de production [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dépend de nombreux facteurs. Comme décrit précédemment, ces facteurs incluent la charge de travail existante et les fonctionnalités [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilisées. Nous vous recommandons d’analyser la charge de travail existante en effectuant les tâches suivantes dans un environnement de test SQL Server :

- Activez la croissance automatique de `tempdb`.
- Exécutez les requêtes individuelles ou les fichiers de trace de la charge de travail, et superviser l’utilisation de l’espace dans `tempdb`.
- Exécutez les opérations de maintenance des index, comme leur reconstruction, et supervisez l’espace de `tempdb`.
- Utilisez les valeurs d’utilisation de l’espace des étapes précédentes pour prédire votre utilisation totale en termes de charge de travail. Ajustez cette valeur pour une activité simultanée prévue, puis définissez la taille de `tempdb` en conséquence.

## <a name="monitoring-tempdb-use"></a>Surveillance de l’utilisation de tempdb
L’espace disque insuffisant dans `tempdb` peut entraîner des interruptions significatives dans l’environnement de production [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Il peut également empêcher des applications qui exécutent d’effectuer des opérations. Vous pouvez utiliser la vue de gestion dynamique [sys.dm_db_file_space_usage](../../relational-databases/system-dynamic-management-views/sys-dm-db-session-space-usage-transact-sql.md) pour surveiller l’espace disque utilisé dans les fichiers `tempdb` :

```sql
 -- Determining the amount of free space in tempdb
SELECT SUM(unallocated_extent_page_count) AS [free pages],
  (SUM(unallocated_extent_page_count)*1.0/128) AS [free space in MB]
FROM sys.dm_db_file_space_usage;

-- Determining the amount of space used by the version store
SELECT SUM(version_store_reserved_page_count) AS [version store pages used],
  (SUM(version_store_reserved_page_count)*1.0/128) AS [version store space in MB]
FROM sys.dm_db_file_space_usage;

-- Determining the amount of space used by internal objects
SELECT SUM(internal_object_reserved_page_count) AS [internal object pages used],
  (SUM(internal_object_reserved_page_count)*1.0/128) AS [internal object space in MB]
FROM sys.dm_db_file_space_usage;

-- Determining the amount of space used by user objects
SELECT SUM(user_object_reserved_page_count) AS [user object pages used],
  (SUM(user_object_reserved_page_count)*1.0/128) AS [user object space in MB]
FROM sys.dm_db_file_space_usage;
 ```

Pour superviser l’activité d’allocation et de désallocation de pages dans `tempdb` au niveau de la session ou de la tâche, vous pouvez utiliser les vues de gestion dynamique [sys.dm_db_session_space_usage](../../relational-databases/system-dynamic-management-views/sys-dm-db-session-space-usage-transact-sql.md) et [sys.dm_db_task_space_usage](../../relational-databases/system-dynamic-management-views/sys-dm-db-task-space-usage-transact-sql.md). Ces vues peuvent vous aider à identifier les requêtes, les tables temporaires et les variables de table qui utilisent un espace disque volumineux dans `tempdb`. Vous pouvez également utiliser plusieurs compteurs pour superviser l’espace libre disponible dans `tempdb`, ainsi que les ressources qui utilisent `tempdb`.

```sql
-- Obtaining the space consumed by internal objects in all currently running tasks in each session
SELECT session_id,
  SUM(internal_objects_alloc_page_count) AS task_internal_objects_alloc_page_count,
  SUM(internal_objects_dealloc_page_count) AS task_internal_objects_dealloc_page_count
FROM sys.dm_db_task_space_usage
GROUP BY session_id;

-- Obtaining the space consumed by internal objects in the current session for both running and completed tasks
SELECT R2.session_id,
  R1.internal_objects_alloc_page_count
  + SUM(R2.internal_objects_alloc_page_count) AS session_internal_objects_alloc_page_count,
  R1.internal_objects_dealloc_page_count
  + SUM(R2.internal_objects_dealloc_page_count) AS session_internal_objects_dealloc_page_count
FROM sys.dm_db_session_space_usage AS R1
INNER JOIN sys.dm_db_task_space_usage AS R2 ON R1.session_id = R2.session_id
GROUP BY R2.session_id, R1.internal_objects_alloc_page_count,
  R1.internal_objects_dealloc_page_count;;
```

## <a name="related-content"></a>Contenu connexe
[Option SORT_IN_TEMPDB pour les index](../../relational-databases/indexes/sort-in-TempDB-option-for-indexes.md)    
[Bases de données système](../../relational-databases/databases/system-databases.md)    
[sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)    
[sys.master_files](../../relational-databases/system-catalog-views/sys-master-files-transact-sql.md)    
[Déplacer des fichiers de bases de données](../../relational-databases/databases/move-database-files.md)    
