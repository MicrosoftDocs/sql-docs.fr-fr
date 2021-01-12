---
description: Niveau de compatibilité ALTER DATABASE (Transact-SQL)
title: Niveau de compatibilité ALTER DATABASE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/22/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- COMPATIBILITY_LEVEL_TSQL
- COMPATIBILITY_LEVEL
dev_langs:
- TSQL
helpviewer_keywords:
- 80 compatibility level
- ALTER DATABASE statement, compatibility levels
- 90 compatibility level
- compatibility levels [SQL Server]
- 100 compatibility level
- db compatibility level
- db compat level
ms.assetid: ca5fd220-d5ea-4182-8950-55d4101a86f6
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 168f902453d2897e50186cc513977b2eb10e8627
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094735"
---
# <a name="alter-database-transact-sql-compatibility-level"></a>Niveau de compatibilité ALTER DATABASE (Transact-SQL)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Définit [!INCLUDE[tsql](../../includes/tsql-md.md)] et des comportements de traitement des requêtes pour qu’ils soient compatibles avec la version spécifiée du moteur SQL. Pour connaître les autres options d’ALTER DATABASE, voir [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql.md).  

Pour plus d’informations sur les conventions de la syntaxe, consultez [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).

## <a name="syntax"></a>Syntaxe

```syntaxsql
ALTER DATABASE database_name
SET COMPATIBILITY_LEVEL = { 150 | 140 | 130 | 120 | 110 | 100 | 90 }
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments

*database_name* Spécifie le nom de la base de données à modifier.

COMPATIBILITY_LEVEL { 150 \| 140 \| 130 \| 120 \| 110 \| 100 \| 90 \| 80 } correspond à la version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] avec laquelle la base de données doit être compatible. Les valeurs de niveau de compatibilité suivantes peuvent être configurées (toutes les versions ne prennent pas en charge l’ensemble des niveaux de compatibilité listés ci-dessus) :

<a name="supported-dbcompats"></a>

|Produit|Version du moteur de base de données|Désignation du niveau de compatibilité par défaut|Valeurs de niveau de compatibilité prises en charge|
|-------------|-----------------------------|-------------------------------------|------------------------------------------|
|[!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)]|15|150|150, 140, 130, 120, 110, 100|
|[!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]|14|140|140, 130, 120, 110, 100|
|[!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]|12|150|150, 140, 130, 120, 110, 100|
|[!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] Managed Instance|12|150|150, 140, 130, 120, 110, 100|
|[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)]|13|130|130, 120, 110, 100|
|[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]|12|120|120, 110, 100|
|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]|11|110|110, 100, 90|
|[!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]|10.5|100|100, 90, 80|
|[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]|10|100|100, 90, 80|
|[!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]|9|90|90, 80|
|[!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)]|8|80|80|

> [!IMPORTANT]
> Les numéros de version du moteur de base de données pour SQL Server et Azure SQL Database ne sont pas comparables les uns aux autres. Il s’agit plutôt de numéros de build internes pour ces produits distincts. Le moteur de base de données pour Azure SQL Database repose sur la même base de code que le moteur de base de données SQL Server. Plus important encore, le moteur de base de données dans Azure SQL Database a toujours les bits les plus récents du moteur de base de données SQL. La version 12 d’Azure SQL Database est plus récente que la version 15 de SQL Server.

## <a name="remarks"></a>Notes
Pour toutes les installations de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], le niveau de compatibilité par défaut est associé à la version du [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Ce niveau est attribué aux nouvelles bases de données, sauf si la base de données **model** a un niveau de compatibilité inférieur. Pour les bases de données attachées ou restaurées à partir d’une version antérieure de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], la base de données conserve son niveau de compatibilité existant si celui-ci correspond au moins à la valeur minimale autorisée pour cette instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Le déplacement d’une base de données ayant un niveau de compatibilité inférieur à celui autorisé par le [!INCLUDE[ssde_md](../../includes/ssde_md.md)] a pour effet de lui attribuer automatiquement le niveau de compatibilité autorisé le plus bas. Cela s'applique aussi bien aux bases de données système qu'aux bases de données utilisateur.

Voici les comportements auxquels vous pouvez vous attendre avec [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] quand une base de données est attachée ou restaurée et après une mise à niveau sur place :

- Si le niveau de compatibilité d'une base de données utilisateur est à 100 ou supérieur avant la mise à niveau, il reste le même après la mise à niveau.
- Si le niveau de compatibilité d’une base de données utilisateur était à 90 avant la mise à niveau, dans la base de données mise à niveau, le niveau de compatibilité est défini à 100, ce qui correspond au niveau de compatibilité le plus bas pris en charge dans [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)].
- Les niveaux de compatibilité des bases de données tempdb, model, msdb et Resource sont définis sur le niveau de compatibilité par défaut d’une version [!INCLUDE[ssDE](../../includes/ssde-md.md)] donnée. 
- La base de données système master conserve le niveau de compatibilité qu’elle avait avant la mise à niveau. Cela n’a pas d’impact sur le comportement de la base de données utilisateur. 

Concernant les bases de données déjà exécutées à des niveaux de compatibilité moins élevés, tant que l’application n’a pas besoin de tirer parti des améliorations qui ne sont disponibles que dans un niveau de compatibilité de base de données plus élevé, il est préférable de conserver le niveau de compatibilité de base de données précédent. Pour un nouveau travail de développement ou quand une application existante exige l’utilisation de nouvelles fonctionnalités comme le [traitement de requêtes intelligent](../../relational-databases/performance/intelligent-query-processing.md) ou un nouveau [!INCLUDE[tsql](../../includes/tsql-md.md)], envisagez de mettre à niveau le niveau de compatibilité de base de données vers la dernière version disponible. Pour plus d’informations, consultez [Niveaux de compatibilité et mises à niveau du moteur de base de données](../../database-engine/install-windows/compatibility-certification.md#compatibility-levels-and-database-engine-upgrades).     

> [!NOTE]
> S’il n’y a pas ni objets utilisateur ni dépendances, il est généralement sûr de mettre à niveau vers le niveau de compatibilité par défaut. Pour plus d’informations, consultez [Recommandations – base de données MASTER](../../relational-databases/databases/master-database.md#recommendations).

Utilisez `ALTER DATABASE` pour modifier le niveau de compatibilité de la base de données. Le nouveau paramètre de compatibilité d’une base de données prend effet à partir du moment où une commande `USE <database>` est émise ou qu’un nouveau compte de connexion est traité avec cette base de données définie comme contexte de base de données par défaut.
Pour voir le niveau de compatibilité actuel d’une base de données, interrogez la colonne `compatibility_level` dans la vue de catalogue [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md).

> [!NOTE]
> Une [base de données de distribution](../../relational-databases/replication/distribution-database.md) créée dans une version antérieure de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et mise à niveau vers [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] RTM ou Service Pack 1 a un niveau de compatibilité de 90, qui n’est pas pris en charge pour les autres bases de données. Cela n’a aucun impact sur la fonctionnalité de réplication. Une mise à niveau vers des Service Packs et des versions ultérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se traduit par une élévation du niveau de compatibilité de la base de données de distribution pour atteindre celui de la base de données **MASTER**.

> [!NOTE]
> À partir de **novembre 2019**, dans [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], le niveau de compatibilité par défaut est 150 pour les bases de données nouvellement créées. [!INCLUDE[msCoName](../../includes/msconame-md.md)] ne met pas à jour le niveau de compatibilité pour les bases de données existantes. Il incombe aux clients de le faire à leur convenance.        
> [!INCLUDE[msCoName](../../includes/msconame-md.md)] recommande vivement aux clients d’envisager de passer au dernier niveau de compatibilité afin de tirer parti des dernières améliorations apportées à l’optimisation des requêtes.        

Si vous souhaitez utiliser un niveau de compatibilité de base de données de 120 (ou supérieur) pour l’intégralité d’une base de données, et adhérer au modèle d’[**estimation de la cardinalité**](../../relational-databases/performance/cardinality-estimation-sql-server.md) de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], qui correspond au niveau de compatibilité de base de données 110, consultez [ALTER DATABASE SCOPED CONFIGURATION](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md), et en particulier son mot clé `LEGACY_CARDINALITY_ESTIMATION = ON`.

Pour plus d’informations sur la façon d’évaluer les différences de performances de vos requêtes les plus importantes entre deux niveaux de compatibilité différents sur [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], consultez [Meilleures performances des requêtes avec le niveau de compatibilité 130 dans Azure SQL Database](/archive/blogs/sqlserverstorageengine/improved-query-performance-with-compatibility-level-130-in-azure-sql-database). Notez que cet article fait référence au niveau de compatibilité 130 et à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], mais la même méthodologie s’applique pour passer au niveau 140 ou à des niveaux supérieurs dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Exécutez la requête suivante pour déterminer la version du [!INCLUDE[ssDE](../../includes/ssde-md.md)] auquel vous êtes connecté.

```sql
SELECT SERVERPROPERTY('ProductVersion');
```

> [!NOTE]
> Notez que [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] ne prend pas en charge l’intégralité des fonctionnalités disponibles avec les différents niveaux de compatibilité.

Pour déterminer le niveau de compatibilité actuel, interrogez la colonne **compatibility_level** de [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md).

```sql
SELECT name, compatibility_level FROM sys.databases;
```

## <a name="compatibility-levels-and-database-engine-upgrades"></a>Niveaux de compatibilité et mises à niveau du moteur de base de données
Le niveau de compatibilité de base de données est un outil précieux quand il s’agit de moderniser une base de données. Il permet en effet de mettre à niveau le [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] tout en continuant de connecter l’état opérationnel des applications en conservant le niveau de compatibilité de base de données antérieur à la mise à niveau. Cela signifie qu’il est possible de procéder à une mise à niveau à partir d’une version antérieure de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (telle que [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)]) vers [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] ou [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] (avec instance managée) sans aucune modification de l’application (à l’exception de la connectivité de base de données). Pour plus d’informations, consultez [Certification de compatibilité](../../database-engine/install-windows/compatibility-certification.md).

Tant que l’application n’a pas besoin d’utiliser des améliorations disponibles uniquement dans un niveau de compatibilité de base de données plus élevé, il est préférable de mettre à niveau le [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] et de conserver le niveau de compatibilité de base de données précédent. Pour plus d’informations sur l’utilisation du niveau de compatibilité pour la compatibilité descendante, consultez [Certification de compatibilité](../../database-engine/install-windows/compatibility-certification.md).

## <a name="best-practices-for-upgrading-database-compatibility-level"></a>Bonnes pratiques pour la mise à niveau du niveau de compatibilité de base de données
Pour connaître le workflow recommandé pour la mise à niveau du niveau de compatibilité, consultez [Changer le mode de compatibilité de la base de données et utiliser le magasin des requêtes](../../database-engine/install-windows/change-the-database-compatibility-mode-and-use-the-query-store.md). En outre, pour obtenir de l’aide concernant la mise à niveau du niveau de compatibilité de base de données, consultez [Mise à niveau de bases de données à l’aide de l’Assistant Paramétrage de requêtes](../../relational-databases/performance/upgrade-dbcompat-using-qta.md).

## <a name="compatibility-levels-and-stored-procedures"></a>Niveaux de compatibilité et procédures stockées
Lors de l'exécution d'une procédure stockée, elle utilise le niveau de compatibilité en cours de la base de données dans laquelle elle est définie. Lors de la modification du paramètre de compatibilité d'une base de données, l'ensemble de ses procédures stockées sont automatiquement recompilées en conséquence.

## <a name="using-compatibility-level-for-backward-compatibility"></a><a name="backwardCompat"></a> Utilisation du niveau de compatibilité pour la compatibilité descendante
Le paramètre [Niveau de compatibilité de la base de données](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md) fournit une compatibilité descendante avec les versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en ce qui concerne [!INCLUDE[tsql](../../includes/tsql-md.md)] et les comportements d’optimisation des requêtes, uniquement pour la base de données spécifiée, et non pour l’ensemble du serveur.  

À partir du mode de compatibilité 130, les nouveaux plans de requête affectant les correctifs et les fonctionnalités ne sont ajoutés intentionnellement qu’au nouveau niveau de compatibilité. Lors des mises à niveau, cela permet de réduire les risques liés à la dégradation des performances en raison des modifications du plan de requête potentiellement introduites par de nouveaux comportements d’optimisation des requêtes.      

Du point de vue de l’application, utilisez le niveau de compatibilité le plus bas comme chemin de migration plus sûr pour contourner les problèmes liés aux différences de versions dans les comportements qui sont contrôlés par le paramètre de niveau de compatibilité approprié. L’objectif doit toujours être de procéder à une mise à niveau vers le niveau de compatibilité le plus récent à un moment donné, de façon à hériter de certaines nouvelles fonctionnalités comme le [traitement de requêtes intelligent](../../relational-databases/performance/intelligent-query-processing.md), mais cette opération doit être effectuée de façon contrôlée. 

Pour plus d’informations, notamment sur le workflow recommandé pour la mise à niveau du niveau de compatibilité de base de données, consultez [Bonnes pratiques pour la mise à niveau du niveau de compatibilité de base de données](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md#best-practices-for-upgrading-database-compatibility-level).

> [!IMPORTANT]
> Les fonctionnalités **obsolètes** obtenues précédemment via une version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne sont **pas** protégées par le niveau de compatibilité. Il s’agit des fonctionnalités qui ont été supprimées du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)].
> Par exemple, l’indicateur `FASTFIRSTROW` a été abandonné dans [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], et remplacé par l’indicateur `OPTION (FAST n )`. Le fait de définir le niveau de compatibilité de la base de données sur 110 ne permet pas de restaurer l’indicateur obsolète.  
>  
> Pour plus d’informations sur les fonctionnalités qui ne sont plus disponibles, consultez [Fonctionnalités du moteur de base de données qui ne sont plus disponibles dans SQL Server](../../database-engine/discontinued-database-engine-functionality-in-sql-server.md) et [Fonctionnalités du moteur de base de données qui ne sont plus disponibles dans SQL Server 2014](https://docs.microsoft.com/previous-versions/sql/2014/database-engine/discontinued-database-engine-functionality-in-sql-server-2016?view=sql-server-2014&preserve-view=true).

> [!IMPORTANT]
> Les **changements importants** introduits par une version donnée de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peuvent ne **pas** être protégés par le niveau de compatibilité. Il s’agit des changements de comportement entre les versions du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]. Le comportement [!INCLUDE[tsql](../../includes/tsql-md.md)] est généralement protégé par le niveau de compatibilité. Toutefois, les objets système modifiés ou supprimés **ne sont pas** protégés par le niveau de compatibilité.
>
> Parmi les changements importants **protégés** par le niveau de compatibilité figure la conversion implicite du type de données datetime en type de données datetime2. Le niveau de compatibilité de base de données de 130 permet une plus grande précision en prenant en compte les fractions de milliseconde, ce qui génère différentes valeurs converties. Pour restaurer l’ancien comportement de conversion, définissez le niveau de compatibilité de la base de données sur 120 ou sur une valeur inférieure.
>
> Parmi les changements importants **non protégés** par le niveau de compatibilité figurent :
>
> - Les noms de colonne modifiés dans les objets système. Dans [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], la colonne *single_pages_kb* de sys.dm_os_sys_info a été renommée *pages_kb*. Quel que soit le niveau de compatibilité, la requête `SELECT single_pages_kb FROM sys.dm_os_sys_info` génère l’erreur 207 (nom de colonne non valide).
> - Les objets système supprimés. Dans [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], `sp_dboption` a été supprimé. Quel que soit le niveau de compatibilité, l’instruction `EXEC sp_dboption 'AdventureWorks2016', 'autoshrink', 'FALSE';` génère l’erreur 2812 (procédure stockée ’sp_dboption’ introuvable).
>
> Pour plus d’informations sur les changements cassants, consultez [Changements cassants dans les fonctionnalités du moteur de base de données de SQL Server 2019](../../database-engine/breaking-changes-to-database-engine-features-in-sql-server-version-15.md), [Changements cassants dans les fonctionnalités du moteur de base de données de SQL Server 2017](../../database-engine/breaking-changes-to-database-engine-features-in-sql-server-2017.md), [Changements cassants dans les fonctionnalités du moteur de base de données de SQL Server 2016](../../database-engine/breaking-changes-to-database-engine-features-in-sql-server-2016.md) et [Changements cassants dans les fonctionnalités du moteur de base de données de SQL Server 2014](https://docs.microsoft.com/previous-versions/sql/2014/database-engine/breaking-changes-to-database-engine-features-in-sql-server-2016?view=sql-server-2014&preserve-view=true).

## <a name="differences-between-compatibility-levels"></a>Comparaison des différents niveaux de compatibilité
Pour toutes les installations de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], le niveau de compatibilité par défaut est associé à la version du [!INCLUDE[ssDE](../../includes/ssde-md.md)], comme vous pouvez le voir dans [ce tableau](#supported-dbcompats). Pour chaque nouvelle tâche de développement, prévoyez toujours de certifier les applications avec le niveau de compatibilité de base de données le plus récent.

La nouvelle syntaxe [!INCLUDE[tsql](../../includes/tsql-md.md)] n’est pas contrôlée par le niveau de compatibilité de la base de données, sauf si elle risque de créer un conflit avec le code utilisateur [!INCLUDE[tsql](../../includes/tsql-md.md)] et ainsi d’empêcher les applications existantes de fonctionner. Ces exceptions sont documentées dans les sections suivantes de cet article qui décrivent les différences qui existent entre chaque niveau de compatibilité.

Le niveau de compatibilité de base de données offre également une compatibilité descendante avec les versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], car les bases de données attachées ou restaurées à partir de n’importe quelle version antérieure de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] conservent leur niveau de compatibilité existant (si celui-ci est identique ou supérieur au niveau de compatibilité minimal autorisé). Ceci a été abordé dans la section [Utilisation du niveau de compatibilité pour la compatibilité descendante](#backwardCompat) de cet article.

À partir du niveau de compatibilité de base de données 130, les nouveaux correctifs et nouvelles fonctionnalités qui affectent les plans de requête sont ajoutés uniquement au niveau de compatibilité le plus récent, également appelé « niveau de compatibilité par défaut ». Lors des mises à niveau, cela permet de réduire les risques liés à la dégradation des performances en raison des modifications du plan de requête potentiellement apportées par de nouveaux comportements d’optimisation des requêtes. 

Les principales modifications qui affectent le plan et qui sont ajoutées uniquement au niveau de compatibilité par défaut d’une nouvelle version du [!INCLUDE[ssDE](../../includes/ssde-md.md)] sont les suivantes :

1.  **Les correctifs de l’optimiseur de requête publiés pour les versions précédentes de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sous l’indicateur de trace 4199 sont automatiquement activés avec le niveau de compatibilité par défaut d’une version plus récente de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]** . **S’applique à :** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)]) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

    Par exemple, lorsque [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] est sorti, tous les correctifs de l’optimiseur de requête publiés pour les versions précédentes de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (et les niveaux de compatibilité de 100 à 120) étaient activés automatiquement pour les bases de données qui utilisaient le niveau de compatibilité par défaut (130) de [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)]. Seuls les correctifs post-RTM de l’optimiseur de requête doivent être activés explicitement.
    
    > [!NOTE]
    > Pour activer les correctifs de l’optimiseur de requête, vous pouvez utiliser les méthodes suivantes :    
    >
    > - Au niveau du serveur, avec l’[indicateur de trace 4199](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md#4199).
    > - Au niveau de la base de données, avec l’option `QUERY_OPTIMIZER_HOTFIXES` dans [ALTER DATABASE SCOPED CONFIGURATION (Transact-SQL)](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md).
    > - Au niveau de la requête, avec l’[indicateur de requête](../../t-sql/queries/hints-transact-sql-query.md#use_hint) `USE HINT 'ENABLE_QUERY_OPTIMIZER_HOTFIXES'`.
    
    Plus tard, lorsque [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] est sorti, tous les correctifs de l’optimiseur de requête publiés après la version RTM de [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] étaient automatiquement activés pour les bases de données utilisant le niveau de compatibilité par défaut (140) de [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]. Il s’agit d’un comportement cumulatif qui inclut tous les correctifs des versions précédentes. Pour rappel, seuls les correctifs post-RTM de l’optimiseur de requête doivent être activés explicitement.  
    
    Le tableau suivant récapitule ce comportement :
    
    |Version du moteur de base de données|Niveau de compatibilité de la base de données|TF 4199|Modifications de l’optimiseur de requête de tous les précédents niveaux de compatibilité de base de données|Modifications de l’optimiseur de requête pour la version post-RTM du moteur de base de données|
    |----------|----------|---|------------|--------|
    |13 ([!INCLUDE[ssSQL15](../../includes/sssql15-md.md)])|100 à 120<br /><br /><br />130|Off<br />Il en va<br /><br />Off<br />Il en va|**Désactivé**<br />activé<br /><br />**Activé**<br />activé|Désactivé<br />activé<br /><br />Désactivé<br />activé|
    |14 ([!INCLUDE[ssSQL17](../../includes/sssql17-md.md)])|100 à 120<br /><br /><br />130<br /><br /><br />140|Off<br />Il en va<br /><br />Off<br />Il en va<br /><br />Off<br />Il en va|**Désactivé**<br />activé<br /><br />**Activé**<br />activé<br /><br />**Activé**<br />activé|Désactivé<br />activé<br /><br />Désactivé<br />activé<br /><br />Désactivé<br />activé|
    |15 ([!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)]) et 12 ([!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)])|100 à 120<br /><br /><br />130 à 140<br /><br /><br />150|Off<br />Il en va<br /><br />Off<br />Il en va<br /><br />Off<br />Il en va|**Désactivé**<br />activé<br /><br />**Activé**<br />activé<br /><br />**Activé**<br />activé|Désactivé<br />activé<br /><br />Désactivé<br />activé<br /><br />Désactivé<br />activé|
    
    > [!IMPORTANT]
    > Les correctifs de l’optimiseur de requête qui résolvent les erreurs relatives à des résultats erronés ou à des violations d’accès ne sont pas protégés par l’indicateur de trace 4199. Ces correctifs ne sont pas considérés comme facultatifs.
 
2.  **Les modifications apportées à l’[estimateur de cardinalité](../../relational-databases/performance/cardinality-estimation-sql-server.md) sur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] sont activées uniquement pour le niveau de compatibilité par défaut d’une nouvelle version du [!INCLUDE[ssDE](../../includes/ssde-md.md)]** , mais pas pour les niveaux de compatibilité précédents. 

    Par exemple, lorsque [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] est sorti, les modifications apportées au processus d’estimation de la cardinalité étaient uniquement disponibles pour les bases de données utilisant le niveau de compatibilité par défaut (130) de [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)]. Les niveaux de compatibilité précédents ont conservé le comportement d’estimation de la cardinalité qui était disponible avant [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)]. 
    
    Plus tard, lorsque [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] est sorti, les nouvelles modifications apportées au processus d’estimation de la cardinalité étaient uniquement disponibles pour les bases de données utilisant le niveau de compatibilité par défaut (140) de [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]. Le niveau de compatibilité de base de données 130 a conservé le comportement d’estimation de la cardinalité de [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)].
    
    Le tableau suivant récapitule ce comportement :
    
    |Version du moteur de base de données|Niveau de compatibilité de la base de données|Nouvelles modifications apportées à la version CE|
    |----------|--------|-------------|
    |13 ([!INCLUDE[ssSQL15](../../includes/sssql15-md.md)])|< 130<br />130|Désactivé<br />activé|
    |14 ([!INCLUDE[ssSQL17](../../includes/sssql17-md.md)])<sup>1</sup>|< 140<br />140|Désactivé<br />activé|
    |15 ([!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)])<sup>1</sup>|< 150<br />150|Désactivé<br />activé|
    
    <sup>1</sup> Également applicable à [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].
    
> [!IMPORTANT]
> Les sections suivantes de cet article abordent d’autres différences qui existent entre certains niveaux de compatibilité.

## <a name="differences-between-compatibility-level-140-and-level-150"></a>Différences entre le niveau de compatibilité 140 et le niveau 150
Cette section décrit les nouveaux comportements introduits avec le niveau de compatibilité 150.

|Paramètre de niveau de compatibilité inférieur ou égal à 140|Paramètre de niveau de compatibilité égal à 150|
|--------------------------------------------------|-----------------------------------------|
|L’entrepôt de données relationnelles et les charges de travail analytiques peuvent ne pas être en mesure de tirer parti des index columnstore en raison d’une charge mémoire OLTP, de l’absence de prise en charge du fournisseur ou d’autres limitations.  Sans les index columnstore, ces charges de travail ne peuvent pas bénéficier du mode d’exécution par lot.|Le mode d’exécution par lot est désormais disponible pour les charges de travail analytiques sans avoir besoin d’index columnstore. Pour plus d’informations, consultez [mode batch sur rowstore](../../relational-databases/performance/intelligent-query-processing.md#batch-mode-on-rowstore).|
|Les requêtes en mode ligne qui demandent des tailles insuffisantes d’allocation de mémoire et entraînent des dépassements sur le disque, peuvent continuer à avoir des problèmes lors des exécutions suivantes.|Les requêtes en mode ligne qui demandent des tailles insuffisantes d’allocation de mémoire et entraînent des dépassements sur le disque, peuvent avoir de meilleures performances lors des exécutions suivantes. Pour plus d’informations, consultez [rétroaction d’allocation de mémoire en mode ligne](../../relational-databases/performance/intelligent-query-processing.md#row-mode-memory-grant-feedback).|
|Les requêtes en mode ligne qui demandent des tailles excessives d’allocation de mémoire et entraînent des problèmes de concurrence, peuvent continuer à avoir des problèmes lors des exécutions suivantes.|Les requêtes en mode ligne qui demandent des tailles excessives d’allocation de mémoire et entraînent des problèmes de concurrence, peuvent avoir une concurrence améliorée lors des exécutions suivantes. Pour plus d’informations, consultez [rétroaction d’allocation de mémoire en mode ligne](../../relational-databases/performance/intelligent-query-processing.md#row-mode-memory-grant-feedback).|
|Les requêtes faisant référence à des fonctions scalaires définies par l’utilisateur T-SQL utilisent l’invocation itérative, ne disposent pas de l’évaluation des coûts et forcent l’exécution en série. |Les fonctions scalaires définies par l’utilisateur T-SQL sont transformées en expressions relationnelles équivalentes qui sont « placées inline » dans la requête appelante, ce qui entraîne souvent des gains de performances significatifs. Pour plus d’informations, consultez [Incorporation des fonctions UDF scalaires T-SQL](../../relational-databases/performance/intelligent-query-processing.md#scalar-udf-inlining).|
|Les variables de table utilisent une estimation fixe pour l’estimation de la cardinalité.  Si le nombre réel de lignes est nettement supérieur à la valeur devinée, les performances des opérations en aval peuvent être dégradées. |Les nouveaux plans utilisent la cardinalité réelle de la variable de table rencontrée à la première compilation, au lieu d’une estimation fixe. Pour plus d'informations, consultez [compilation différée de variable de table](../../relational-databases/performance/intelligent-query-processing.md#table-variable-deferred-compilation).|

Pour plus d’informations sur les fonctionnalités de traitement des requêtes activées dans le niveau de compatibilité de base de données 150, consultez [Nouveautés de SQL Server 2019](../../sql-server/what-s-new-in-sql-server-ver15.md) et [Traitement de requêtes intelligent dans les bases de données SQL](../../relational-databases/performance/intelligent-query-processing.md).

## <a name="differences-between-compatibility-level-130-and-level-140"></a>Différences entre le niveau de compatibilité 130 et le niveau 140

Cette section décrit les nouveaux comportements introduits avec le niveau de compatibilité 140.

|Paramètre de niveau de compatibilité inférieur ou égal à 130|Paramètre de niveau de compatibilité égal à 140|
|--------------------------------------------------|-----------------------------------------|
|Les estimations de cardinalité pour les instructions qui référencent des fonctions table à instructions multiples utilisent une estimation de ligne fixe.|Les estimations de cardinalité pour les instructions éligibles référençant des fonctions table à instructions multiples utilisent la cardinalité de la sortie de la fonction. Ceci peut être activé via **l’exécution entrelacée** pour les fonctions table à instructions multiples.|
|Les requêtes en mode batch qui demandent des tailles insuffisantes d’allocation de mémoire et entraînent des dépassements sur le disque, peuvent continuer à avoir des problèmes lors des exécutions suivantes.|Les requêtes en mode de traitement par lot qui demandent des tailles insuffisantes d’allocation de mémoire et entraînent des dépassements sur le disque, peuvent avoir de meilleures performances lors des exécutions suivantes. Ceci est possible avec la **rétroaction d’allocation de mémoire en mode batch**, qui met à jour la taille de l’allocation de mémoire d’un plan mis en cache, si des dépassements se produisent pour les opérateurs en mode batch. |
|Les requêtes en mode batch qui demandent des tailles excessives d’allocation de mémoire et entraînent des problèmes de concurrence, peuvent continuer à avoir des problèmes lors des exécutions suivantes.|Les requêtes en mode batch qui demandent des tailles excessives d’allocation de mémoire et entraînent des problèmes de concurrence, peuvent avoir une concurrence améliorée lors des exécutions suivantes. Ceci est possible avec la **rétroaction d’allocation de mémoire en mode batch**, qui met à jour la taille de l’allocation de mémoire d’un plan mis en cache, si une quantité excessive de mémoire a été demandée.|
|Les requêtes en mode batch qui contiennent des opérateurs de jointure sont éligibles pour trois algorithmes de jointures physiques, que sont les boucles imbriquées, les jointures hachées et les jointures de fusion. Si les estimations de cardinalité sont incorrectes pour les entrées de jointure, un algorithme de jointure non adapté peut être sélectionné. Dans ce cas, les performances en pâtissent et l’algorithme de jointure non adapté continue d’être utilisé jusqu’à la recompilation du plan mis en cache.|Il existe un opérateur de jointure supplémentaire appelé **jointure adaptive**. Si les estimations de cardinalité sont incorrectes pour les entrées de jointure de build extérieures, un algorithme de jointure non adapté peut être sélectionné. Si cela se produit et si l’instruction est éligible pour une jointure adaptive, une boucle imbriquée est utilisée pour les entrées de jointure peu volumineuses, et une jointure hachée est utilisée pour les entrées de jointure volumineuses, tout cela, dynamiquement, sans nécessiter de recompilation. |
|Les plans simples qui référencent des index columnstore ne sont pas éligibles pour l’exécution en mode batch. |Un plan simple qui référence des index columnstore sera supprimé en faveur d’un plan éligible pour l’exécution en mode batch.|
|L’opérateur UDX `sp_execute_external_script` peut uniquement être exécuté en mode ligne.|L’opérateur UDX `sp_execute_external_script` est éligible pour une exécution en mode batch.|
|Les fonctions table à instructions multiples ne peuvent pas utiliser l’exécution entrelacée |Pour les fonctions table à instructions multiples, l’exécution entrelacée améliore la qualité du plan.|

Les correctifs qui se trouvaient sous l’indicateur de trace 4199 dans les versions de SQL Server antérieures à SQL Server 2017 sont maintenant activés par défaut avec le niveau de compatibilité 140. L’indicateur de trace 4199 est toujours applicable aux correctifs de l’optimiseur de requête qui ont été publiés après la publication de SQL Server 2017. Pour plus d’informations sur l’indicateur de trace 4199, consultez [Indicateur de trace 4199](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md#4199).

## <a name="differences-between-compatibility-level-120-and-level-130"></a>Différences entre le niveau de compatibilité 120 et le niveau 130

Cette section décrit les nouveaux comportements introduits avec le niveau de compatibilité 130.

|Paramètre de niveau de compatibilité inférieur ou égal à 120|Paramètre de niveau de compatibilité égal à 130|
|--------------------------------------------------|-----------------------------------------|
|Dans une instruction INSERT-SELECT, INSERT est monothread.|Dans une instruction INSERT-SELECT, INSERT est multithread ou peut présenter un plan parallèle.|
|Les requêtes d’une table à mémoire optimisée sont exécutées en monothread.|Les requêtes d’une table à mémoire optimisée peuvent désormais avoir des plans parallèles.|
|Comprend l’estimateur de cardinalité SQL 2014 **CardinalityEstimationModelVersion="120"**|Améliorations supplémentaires de l’estimation de cardinalité avec le niveau de compatibilité 130, qui est visible à partir d’un plan de requête. **CardinalityEstimationModelVersion="130"**|
|Changements au niveau du mode batch et du mode ligne avec les index columnstore :<br /><ul><li>Le tri du contenu d’une table avec un index columnstore s’effectue en mode ligne <li>Les agrégats de fonction de fenêtrage fonctionnent en mode ligne (par exemple, `LAG` ou `LEAD`) <li>Les requêtes exécutées sur des tables Columnstore avec plusieurs clauses distinctes sont exécutées en mode ligne <li>Les requêtes s’exécutant sous MAXDOP 1 ou avec un plan en série sont exécutées en mode ligne</li></ul>| Changements au niveau du mode batch et du mode ligne avec les index columnstore :<br /><ul><li>Le tri du contenu d’une table avec un index columnstore s’effectue désormais en mode batch <li>Les agrégats de fenêtrage fonctionnent désormais en mode batch (par exemple, `LAG` ou `LEAD`) <li>Les requêtes exécutées sur des tables Columnstore avec plusieurs clauses distinctes sont exécutées en mode batch <li>Les requêtes exécutées sous MAXDOP 1 ou avec un plan en série sont exécutées en mode batch</li></ul>|
|Les statistiques peuvent être automatiquement mises à jour. | La logique qui met à jour automatiquement les statistiques est plus agressive sur les tables volumineuses. Dans la pratique, cela doit réduire les problèmes de performances des requêtes, lorsque des lignes nouvellement insérées sont interrogées fréquemment, mais que les statistiques n’ont pas été mises à jour pour inclure ces valeurs. |
|La trace 2371 est désactivée par défaut dans [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]. | La [trace 2371](/archive/blogs/psssql/default-auto-statistics-update-threshold-change-for-sql-server-2016) est activée par défaut dans [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)]. L’indicateur de trace 2371 demande au programme de mise à jour automatique des statistiques d’échantillonner un sous-ensemble de lignes plus petit mais plus raisonnable, dans une table qui comporte un grand nombre de lignes. <br/> <br/> L’une des améliorations est qu’il est désormais possible d’inclure dans l’échantillon plus de lignes que ce qui a été inséré récemment. <br/> <br/> Une autre amélioration est que vous pouvez laisser les requêtes s’exécuter pendant que le processus de mise à jour des statistiques s’exécute, plutôt que de bloquer les requêtes. |
|Pour le niveau 120, les statistiques sont échantillonnées par un processus monothread.|Pour le niveau 130, les statistiques sont échantillonnées par un processus multithread (parallèle). |
|Le nombre de clés étrangères entrantes est limité à 253.| Une table peut être référencée par un nombre maximal de 10 000 clés étrangères entrantes (ou types de références similaires). Pour connaître les restrictions associées, consultez [Create Foreign Key Relationships](../../relational-databases/tables/create-foreign-key-relationships.md). |
|Les algorithmes de hachage dépréciés MD2, MD4, MD5, SHA et SHA1 sont autorisés.|Seuls les algorithmes de hachage SHA2_256 et SHA2_512 sont autorisés.|
||[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] comprend des améliorations au niveau de certaines conversions de types de données et de certaines opérations (dont la plupart sont peu courantes). Pour plus d’informations, consultez [Améliorations de SQL Server 2016 dans le traitement de certains types de données et des opérations peu courantes](https://support.microsoft.com/help/4010261/sql-server-2016-improvements-in-handling-some-data-types-and-uncommon).|
|La fonction `STRING_SPLIT` n’est pas disponible.|La fonction `STRING_SPLIT` est disponible avec le niveau de compatibilité 130 ou supérieur. Si votre niveau de compatibilité de base de données est inférieur à 130, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne peut pas trouver ni exécuter la fonction `STRING_SPLIT`.|

Les correctifs qui se trouvaient sous l’indicateur de trace 4199 dans les versions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] antérieures à [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] sont maintenant activés par défaut avec le niveau de compatibilité 130. L’indicateur de trace 4199 est toujours applicable aux correctifs de l’optimiseur de requête qui ont été publiés après la publication de [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)]. Pour utiliser l’ancien optimiseur de requête de [!INCLUDE[ssSDS](../../includes/sssds-md.md)], vous devez sélectionner le niveau de compatibilité 110. Pour plus d’informations sur l’indicateur de trace 4199, consultez [Indicateur de trace 4199](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md#4199).

## <a name="differences-between-lower-compatibility-levels-and-level-120"></a>Différences entre les niveaux de compatibilité inférieurs et le niveau 120

Cette section décrit les nouveaux comportements introduits avec le niveau de compatibilité 120.

|Paramètre de niveau de compatibilité inférieur ou égal à 110|Paramètre de niveau de compatibilité égal à 120|
|--------------------------------------------------|-----------------------------------------|
|L'ancien optimiseur de requête est utilisé.|[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] comprend d’importantes améliorations au niveau du composant qui crée et optimise les plans de requête. Cette nouvelle fonctionnalité de l’optimiseur de requête dépend de l’utilisation du niveau de compatibilité 120 de la base de données. Pour bénéficier de ces améliorations, vous devez développer des applications de base de données à l’aide d’un niveau de compatibilité de base de données 120. Les applications qui sont migrées des versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doivent être soigneusement testées pour vérifier que de bonnes performances sont conservées ou améliorées. Si les performances se dégradent, définissez le niveau de compatibilité 110 ou inférieur de base de données pour utiliser la méthodologie de l’ancien optimiseur de requête.<br /><br /> Le niveau de compatibilité 120 de la base de données utilise un nouvel estimateur de cardinalité qui est réglé pour le stockage des données et les charges de travail OLTP modernes. Avant de définir le niveau de compatibilité de la base de données sur 110 en raison de problèmes de performances, consultez les recommandations de la section *Plans de requête* dans la rubrique [Nouveautés du moteur de base de données](../../sql-server/what-s-new-in-sql-server-2016.md) de [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)].|
|Dans les niveaux de compatibilité inférieurs à 120, le paramètre de langue est ignoré lors de la conversion d’une valeur de **date** en une valeur de chaîne. Notez que ce comportement ne s’applique qu’au type **date**. Consultez l’exemple B dans la section Exemples ci-dessous.|Le paramètre de langue est pris en compte lors de la conversion d’une valeur de **date** en une valeur de chaîne.|
|Les références récursives à droite d’une clause `EXCEPT` créent une boucle infinie. L’exemple C de la section Exemples ci-dessous illustre ce comportement.|Les références récursives d’une clause `EXCEPT` génèrent une erreur, conformément à la norme SQL ANSI.|
|L’expression de table commune récursive permet les noms de colonnes en double.|Les expressions CTE récursives n'autorisent pas les noms de colonnes en double.|
|Les déclencheurs désactivés sont activés en cas de modifications.|La modification d'un déclencheur ne modifie pas son état (activé ou désactivé).|
|La clause de table OUTPUT INTO ignore `IDENTITY_INSERT SETTING = OFF` et permet l’insertion de valeurs explicites.|Vous ne pouvez pas insérer de valeurs explicites dans une colonne d’identité de table quand `IDENTITY_INSERT` a la valeur OFF.|
|Lorsque la relation contenant-contenu de la base de données a la valeur partielle, la validation du champ `$action` dans la clause `OUTPUT` d’une instruction `MERGE` peut retourner une erreur de classement.|Le classement des valeurs retournées par la clause `$action` d’une instruction `MERGE` est le classement de la base de données et non celui du serveur, et aucune erreur de conflit de classement n’est retournée.|
|Une instruction `SELECT INTO` crée toujours une opération d'insertion monothread.|Une instruction `SELECT INTO` peut créer une opération d'insertion parallèle. Lors de l'insertion d'un grand nombre de lignes, l'opération parallèle peut améliorer les performances.|

## <a name="differences-between-lower-compatibility-levels-and-levels-100-and-110"></a>Différences entre les niveaux de compatibilité inférieurs et les niveaux 100 et 110

Cette section décrit les nouveaux comportements introduits avec le niveau de compatibilité 110. Cette section s’applique également aux niveaux de compatibilité au-dessus de 110.

|Paramètre de niveau de compatibilité inférieur ou égal à 100|Paramètre de niveau de compatibilité d’au moins 110|
|--------------------------------------------------|--------------------------------------------------|
|Les objets de base de données CLR (Common Language Runtime) sont exécutés avec la version 4 du CLR. Toutefois, quelques changements de comportement introduits dans la version 4 du CLR sont évités. Pour plus d’informations, consultez [Intégration du CLR - Nouveautés](../../relational-databases/clr-integration/clr-integration-what-s-new.md).|Les objets de base de données CLR sont exécutés avec la version 4 du CLR.|
|Les fonctions XQuery **string-length** et **substring** comptent chaque caractère de substitution comme deux caractères.|Les fonctions XQuery **string-length** et **substring** comptent chaque caractère de substitution comme un seul caractère.|
|`PIVOT` est autorisé dans une requête d’expression de table commune récursive. Cependant, la requête retourne des résultats incorrects lorsqu'il existe plusieurs lignes par regroupement.|`PIVOT` n’est pas autorisé dans une requête d’expression de table commune récursive. Une erreur est retournée.|
|L'algorithme RC4 est uniquement pris en charge pour des raisons de compatibilité descendante. Le nouveau matériel ne peut être chiffré à l'aide de RC4 ou de RC4_128 que lorsque la base de données se trouve dans le niveau de compatibilité 90 ou 100. (Non recommandé.) Dans [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], le matériel chiffré à l’aide de RC4 ou de RC4_128 peut être déchiffré avec n’importe quel niveau de compatibilité.|Le nouveau matériel ne peut pas être chiffré à l'aide de RC4 ou RC4_128. Utilisez à la place un algorithme plus récent, tel qu'un des algorithmes AES. Dans [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], le matériel chiffré à l’aide de RC4 ou de RC4_128 peut être déchiffré avec n’importe quel niveau de compatibilité.|
|Le style par défaut des opérations `CAST` et `CONVERT` effectuées sur les types de données **time** et **datetime2** est 121, sauf lorsque l’un des types est utilisé dans une expression de colonne calculée. Pour les colonnes calculées, le style par défaut est 0. Ce comportement influe sur les colonnes calculées lorsqu'elles sont créées, utilisées dans des requêtes impliquant le paramétrage automatique, ou utilisées dans des définitions de contraintes.<br /><br /> L’exemple D de la section Exemples ci-dessous montre les différences entre les styles 0 et 121. Il ne présente pas le comportement décrit ci-dessus. Pour plus d’informations sur les styles de date et d’heure, consultez [CAST et CONVERT](../../t-sql/functions/cast-and-convert-transact-sql.md).|Lorsque le niveau de compatibilité est 110, le style par défaut pour les opérations `CAST` et `CONVERT` effectuées sur les types de données **time** et **datetime2** est toujours 121. Si votre requête repose sur l'ancien comportement, utilisez un niveau de compatibilité inférieur à 110, ou spécifiez explicitement le style 0 dans la requête affectée.<br /><br /> La mise à niveau de la base de données vers le niveau de compatibilité 110 ne modifie pas les données utilisateur stockées sur le disque. Vous devez corriger manuellement ces données comme il convient. Par exemple, si vous avez utilisé `SELECT INTO` pour créer une table à partir d’une source qui contenait une expression de colonne calculée décrite ci-dessus, les données (utilisant le style 0) sont stockées à la place de la définition de colonne calculée. Vous devez mettre à jour manuellement ces données pour qu'elles correspondent au style 121.|
|Toutes les colonnes des tables distantes du type **smalldatetime** qui sont référencées dans une vue partitionnée sont mappées en tant que **datetime**. Les colonnes correspondantes dans les tables locales (dans la même position ordinale de la liste de sélection) doivent être de type **datetime**.|Toutes les colonnes des tables distantes du type **smalldatetime** qui sont référencées dans une vue partitionnée sont mappées en tant que **smalldatetime**. Les colonnes correspondantes dans les tables locales (dans la même position ordinale de la liste de sélection) doivent être de type **smalldatetime**.<br /><br /> Après la mise à niveau en 110, la vue partitionnée distribuée échoue en raison d'une incompatibilité de type de données. Vous pouvez résoudre ce problème en remplaçant le type de données dans la table distante par **datetime** ou en définissant le niveau de compatibilité de la base de données locale sur 100 (ou valeur inférieure).|
|La fonction `SOUNDEX` implémente les règles suivantes :<br /><br /> 1) Les lettres H et W majuscules sont ignorées lors de la séparation de deux consonnes qui portent le même numéro dans le code `SOUNDEX`.<br /><br /> 2) Si les 2 premiers caractères de *character_expression* portent le même numéro dans le code `SOUNDEX`, ils sont tous les deux inclus. Sinon, si plusieurs consonnes côte à côte portent le même numéro dans le code `SOUNDEX`, toutes sont exclues à l’exception de la première.|La fonction `SOUNDEX` implémente les règles suivantes :<br /><br /> 1) Si un H ou un W majuscule sépare deux consonnes qui portent le même numéro dans le code `SOUNDEX`, la consonne située à droite est ignorée.<br /><br /> 2) Si plusieurs consonnes côte à côte portent le même numéro dans le code `SOUNDEX`, toutes sont exclues à l’exception de la première.<br /><br /> <br /><br /> Des règles supplémentaires peuvent entraîner des disparités entre les valeurs calculées par la fonction `SOUNDEX` et les valeurs calculées avec les anciens niveaux de compatibilité. Après la mise à niveau vers le niveau de compatibilité 110, vous pouvez être amené à regénérer les index, les segments de mémoire ou les contraintes CHECK qui utilisent la fonction `SOUNDEX`. Pour plus d’informations, consultez [SOUNDEX](../../t-sql/functions/soundex-transact-sql.md).|

## <a name="differences-between-compatibility-level-90-and-level-100"></a>Différences entre le niveau de compatibilité 90 et le niveau 100

Cette section décrit les nouveaux comportements introduits avec le niveau de compatibilité 100.

|Paramètre de niveau de compatibilité égal à 90|Paramètre de niveau de compatibilité égal à 100|Possibilité d'impact|
|----------------------------------------|-----------------------------------------|---------------------------|
|Le paramètre QUOTED_IDENTIFER est toujours défini sur ON pour les fonctions de table à instructions multiples lorsqu'elles sont créées indépendamment du paramètre de niveau de session.|Le paramètre de session QUOTED IDENTIFIER est respecté lorsque les fonctions de table à instructions multiples sont créées.|Moyenne|
|Lorsque vous créez ou altérez une fonction de partition, les littéraux **datetime** et **smalldatetime** de la fonction sont évalués en supposant que US_English (Anglais États-Unis) est le paramètre de langue.|Le paramètre de langue actuel est utilisé pour évaluer les littéraux **datetime** et **smalldatetime** dans la fonction de partition.|Moyenne|
|La clause `FOR BROWSE` est autorisée (et ignorée) dans les instructions `INSERT` et `SELECT INTO`.|La clause `FOR BROWSE` n’est pas autorisée dans les instructions `INSERT` et `SELECT INTO`.|Moyenne|
|Les prédicats de texte intégral sont autorisés dans la clause `OUTPUT`.|Les prédicats de texte intégral ne sont pas autorisés dans la clause `OUTPUT`.|Faible|
|`CREATE FULLTEXT STOPLIST`, `ALTER FULLTEXT STOPLIST`, et `DROP FULLTEXT STOPLIST` ne sont pas pris en charge. La liste de mots vides système est associée automatiquement aux nouveaux index de recherche en texte intégral.|`CREATE FULLTEXT STOPLIST`, `ALTER FULLTEXT STOPLIST` et `DROP FULLTEXT STOPLIST` sont pris en charge.|Faible|
|`MERGE` n’est pas appliqué comme un mot clé réservé.|MERGE est un mot clé entièrement réservé. L’instruction `MERGE` est prise en charge avec les niveaux de compatibilité 100 et 90.|Faible|
|L’utilisation de l’argument \<dml_table_source> de l’instruction INSERT entraîne une erreur de syntaxe.|Vous pouvez capturer les résultats d'une clause OUTPUT dans une instruction imbriquée INSERT, UPDATE, DELETE ou MERGE, puis les insérer dans une table ou une vue cible. Cette opération s’effectue en utilisant l’argument \<dml_table_source> de l’instruction INSERT.|Faible|
|Sauf si `NOINDEX` est spécifié, `DBCC CHECKDB` ou `DBCC CHECKTABLE` effectue les vérifications de la cohérence physique et logique sur une table ou une vue indexée, ainsi que sur tous ses index non-cluster et XML. Les index spatiaux ne sont pas pris en charge.|Sauf si `NOINDEX` est spécifié, `DBCC CHECKDB` ou `DBCC CHECKTABLE` effectue les vérifications de la cohérence physique et logique sur une table, ainsi que sur tous ses index non-cluster. Toutefois, seules des vérifications de cohérence physique sont effectuées par défaut sur les index XML, les index spatiaux et les vues indexées.<br /><br /> Si `WITH EXTENDED_LOGICAL_CHECKS` est spécifié, des vérifications logiques sont effectuées sur des vues indexées, des index XML et des index spatiaux, là où ils sont présents. Par défaut, les vérifications de cohérence physique sont effectuées avant les vérifications de cohérence logique. Si `NOINDEX` est également spécifié, seules les vérifications logiques sont effectuées.|Faible|
|Lorsqu'une clause OUTPUT est utilisée avec une instruction des langages de manipulation de données (DML) et une erreur d'exécution se produit pendant l'exécution d'instruction, la transaction complète est terminée et restaurée.|Lorsqu’une clause `OUTPUT` est utilisée avec une instruction DML (Data Manipulation Language) et qu’une erreur d’exécution se produit pendant l’exécution de l’instruction, le comportement est déterminé par le paramètre `SET XACT_ABORT`. Si `SET XACT_ABORT` a la valeur OFF, une erreur d’abandon d’instruction générée par l’instruction DML à l’aide de la clause `OUTPUT` met fin à l’instruction, mais l’exécution du lot continue et la transaction n’est pas restaurée. Si `SET XACT_ABORT` a la valeur ON, toutes les erreurs d’exécution générées par l’instruction DML à l’aide de la clause OUTPUT mettent fin au lot, et la transaction est restaurée.|Faible|
|CUBE et ROLLUP ne sont pas appliqués comme mots clé réservés.|`CUBE` et `ROLLUP` sont des mots clés réservés dans la clause GROUP BY.|Faible|
|La validation stricte est appliquée aux éléments du type XML **anyType**.|La validation souple (lax) est appliquée aux éléments du type **anyType**. Pour plus d’informations, consultez [Composants génériques et validation de contenu](../../relational-databases/xml/wildcard-components-and-content-validation.md).|Faible|
|Les attributs spéciaux **xsi:nil** et **xsi:type** ne peuvent pas être interrogés ou modifiés par les instructions de langage de manipulation de données.<br /><br /> Cela signifie que `/e/@xsi:nil` échoue alors que `/e/@*` ignore les attributs **xsi:nil** et **xsi:type**. Toutefois, `/e` retourne les attributs **xsi:nil** et **xsi:type** pour des raisons de cohérence avec `SELECT xmlCol`, même si `xsi:nil = "false"`.|Les attributs spéciaux **xsi:nil** et **xsi:type** sont stockés comme attributs réguliers et peuvent être interrogés et modifiés.<br /><br /> Par exemple, l’exécution de la requête `SELECT x.query('a/b/@*')` retourne tous les attributs, y compris **xsi:nil** et **xsi:type**. Pour exclure ces types dans la requête, remplacez `@*` par `@*[namespace-uri(.) != "`*insert xsi namespace uri*`"` et pas `(local-name(.) = "type"` ou `local-name(.) ="nil".`|Faible|
|Une fonction définie par l'utilisateur qui convertit une valeur de chaîne constante XML en un type datetime [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est marquée comme déterministe.|Une fonction définie par l'utilisateur qui convertit une valeur de chaîne constante XML en un type datetime [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est marquée comme non déterministe.|Faible|
|Les types de liste et d'union XML ne sont pas pris en charge complètement.|Les types de liste et d'union sont complètement pris en charge ainsi que les fonctionnalités suivantes :<br /><br /> Union de liste<br /><br /> Union d'union<br /><br /> Liste de types atomiques<br /><br /> Liste d'union|Faible|
|Les options SET requises pour une méthode xQuery ne sont pas validées lorsque la méthode est contenue dans une vue ou une fonction table incluse.|Les options SET requises pour une méthode xQuery sont validées lorsque la méthode est contenue dans une vue ou une fonction table incluse. Une erreur survient si les options SET de la méthode sont définies incorrectement.|Faible|
|Les valeurs d'attribut XML qui contiennent des caractères de fin de ligne (retour chariot et saut de ligne) ne sont pas normalisées selon la norme XML. Autrement dit, les deux caractères sont retournés à la place d'un caractère de saut de ligne unique.|Les valeurs d'attribut XML qui contiennent des caractères de fin de ligne (retour chariot et saut de ligne) sont normalisées selon la norme XML. Autrement dit, tous les sauts de ligne dans les entités analysées externes (y compris l'entité de document) sont normalisés à l'entrée en un caractère unique #xA par la traduction de la séquence de deux caractères #xD #xA et #xD qui n'est pas suivi de #xA.<br /><br /> Les applications qui utilisent des attributs pour transporter des valeurs de chaîne qui contiennent des caractères de fin de ligne ne recevront pas ces caractères en retour lorsqu'ils sont soumis. Pour éviter le processus de normalisation, utilisez les entités de caractère numérique XML pour encoder tous les caractères de fin de ligne.|Faible|
|Les propriétés de colonne `ROWGUIDCOL` et `IDENTITY` peuvent être nommées de manière incorrecte en tant que contrainte. Par exemple, l'instruction `CREATE TABLE T (C1 int CONSTRAINT MyConstraint IDENTITY)` s'exécute, mais le nom de contrainte n'est pas conservé et n'est pas accessible à l'utilisateur.|Les propriétés de colonne `ROWGUIDCOL` et `IDENTITY` ne peuvent pas être nommées en tant que contrainte. L'erreur 156 est retournée.|Faible|
|La mise à jour des colonnes, à l’aide d’une affectation bidirectionnelle telle que `UPDATE T1 SET @v = column_name = <expression>`, peut produire des résultats inattendus car la valeur dynamique de la variable peut être utilisée dans d’autres clauses, telles que les clauses `WHERE` et `ON`, pendant l’exécution de l’instruction au lieu de la valeur de départ de l’instruction. Cette opération peut modifier les significations des prédicats de façon imprévisible et ligne par ligne.<br /><br /> Ce comportement est applicable uniquement lorsque le niveau de compatibilité est défini à 90.|La mise à jour de colonnes en utilisant une affectation bidirectionnelle produit des résultats attendus car seule la valeur de départ d'instruction de la colonne fait l'objet d'un accès pendant l'exécution de l'instruction.|Faible|
|Consultez l’exemple E dans la section Exemples ci-dessous.|Consultez l’exemple F dans la section Exemples ci-dessous.|Faible|
|La fonction ODBC {fn CONVERT()} utilise le format de date par défaut de la langue. Pour certaines langues, le format par défaut est YDM, ce qui peut provoquer des erreurs de conversion lorsque CONVERT() est associé à d’autres fonctions, telles que `{fn CURDATE()}`, qui attendent un format YMD.|La fonction ODBC `{fn CONVERT()}` utilise le style 121 (format YMD indépendant de la langue) lors de la conversion aux types de données ODBC SQL_TIMESTAMP, SQL_DATE, SQL_TIME, SQLDATE, SQL_TYPE_TIME et SQL_TYPE_TIMESTAMP.|Faible|
|Les intrinsèques datetime tels que DATEPART ne requièrent pas que les valeurs d'entrée de chaîne soient des littéraux datetime valides. Par exemple, `SELECT DATEPART (year, '2007/05-30')` est compilé correctement.|Les intrinsèques datetime tels que `DATEPART` nécessitent que les valeurs d’entrée de chaîne soient des littéraux datetime valides. L'erreur 241 est retournée lorsqu'un littéral datetime non valide est utilisé.|Faible|
|Les espaces de fin spécifiés dans le premier paramètre d’entrée de la fonction REPLACE sont supprimés lorsque le paramètre est de type char. Par exemple, dans l’instruction SELECT '<' + REPLACE(CONVERT(char(6), 'ABC '), ' ', 'L') + '>', la valeur 'ABC ' est évaluée de manière incorrecte comme 'ABC'.|Les espaces de fin sont toujours conservés. Pour les applications basées sur le comportement antérieur de la fonction, utilisez la fonction RTRIM lors de la spécification du premier paramètre d’entrée de la fonction. Par exemple, la syntaxe suivante reproduira le comportement SQL Server 2005 SELECT '<' + REPLACE(RTRIM(CONVERT(char(6), 'ABC ')), ' ', 'L') + '>'.|Faible|

## <a name="reserved-keywords"></a>Mots clés réservés

Le paramètre de compatibilité détermine aussi les mots clés réservés par le [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Le tableau suivant illustre les mots clés réservés introduits par chacun des niveaux de compatibilité.

|Paramètre de niveau de compatibilité|Mots clés réservés|
|----------------------------------|-----------------------|
|130|À déterminer.|
|120|Aucun.|
|110|`WITHIN GROUP`, `TRY_CONVERT`, `SEMANTICKEYPHRASETABLE`, `SEMANTICSIMILARITYDETAILSTABLE`, `SEMANTICSIMILARITYTABLE`|
|100|`CUBE`, `MERGE`, `ROLLUP`|
|90|`EXTERNAL`, `PIVOT`, `UNPIVOT`, `REVERT`, `TABLESAMPLE`|

À un niveau de compatibilité spécifique, les mots clés réservés incluent l'ensemble des mots clés introduits à partir de ce niveau ou sous celui-ci. Ainsi, pour les applications au niveau 110, par exemple, l'ensemble des mots clés répertoriés dans le tableau précédent sont réservés. À des niveaux de compatibilité inférieurs, les mots clés de niveau 100 demeurent des noms d'objet valides, mais les fonctions de langage de niveau 110 correspondant à ces mots clés sont indisponibles.

Une fois introduit, un mot clé demeure réservé. Le mot clé réservé PIVOT, par exemple, introduit au niveau de compatibilité 90, est également réservé aux niveaux 100 et 110 et 120.

Si une application utilise un identificateur réservé en tant que mot clé pour son niveau de compatibilité, l'application échoue. Pour contourner ce problème, placez l’identificateur entre crochets ( **[]** ) ou entre guillemets ( **""** ). Par exemple, pour effectuer la mise à niveau d’une application qui utilise l’identificateur `EXTERNAL` vers le niveau de compatibilité 90, vous pouvez remplacer l’identificateur par `[EXTERNAL]` ou `"EXTERNAL"`.

Pour plus d’informations, consultez [Mots clés réservés](../../t-sql/language-elements/reserved-keywords-transact-sql.md).

## <a name="permissions"></a>Autorisations

Requiert l'autorisation `ALTER` sur la base de données.

## <a name="examples"></a>Exemples

### <a name="a-changing-the-compatibility-level"></a>R. Modification du niveau de compatibilité

L’exemple suivant remplace le niveau de compatibilité de la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] par 110, c’est-à-dire, le niveau par défaut pour [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)].

```sql
ALTER DATABASE AdventureWorks2012
SET COMPATIBILITY_LEVEL = 110;
GO
```

L’exemple suivant retourne le niveau de compatibilité de la base de données actuelle.

```sql
SELECT name, compatibility_level
FROM sys.databases
WHERE name = db_name();
```

### <a name="b-ignoring-the-set-language-statement-except-under-compatibility-level-120"></a>B. Instruction SET LANGUAGE non prise en compte, sauf avec le niveau de compatibilité 120

La requête suivante ignore l’instruction `SET LANGUAGE`, sauf avec le niveau de compatibilité 120.

```sql
SET DATEFORMAT dmy;
DECLARE @t2 date = '12/5/2011' ;
SET LANGUAGE dutch;
SELECT CONVERT(varchar(11), @t2, 106);

-- Results when the compatibility level is less than 120.
12 May 2011

-- Results when the compatibility level is set to 120).
12 mei 2011
```

### <a name="c-for-compatibility-level-setting-of-110-or-lower-recursive-references-on-the-right-hand-side-of-an-except-clause-create-an-infinite-loop"></a>C. Pour un paramètre de compatibilité de 110 ou inférieur, les références récursives dans la partie droite d’une clause EXCEPT créent une boucle infinie

```sql
WITH
cte AS (SELECT * FROM (VALUES (1),(2),(3)) v (a)),
r
AS (SELECT a FROM Table1
UNION ALL
(SELECT a FROM Table1 EXCEPT SELECT a FROM r) )
SELECT a
FROM r;

```

### <a name="d-the-difference-between-styles-0-and-121"></a>D. Différence entre les styles 0 et 121

Pour plus d’informations sur les styles de date et d’heure, consultez [CAST et CONVERT](../../t-sql/functions/cast-and-convert-transact-sql.md).

```sql
CREATE TABLE t1 (c1 time(7), c2 datetime2);

INSERT t1 (c1,c2) VALUES (GETDATE(), GETDATE());

SELECT CONVERT(nvarchar(16),c1,0) AS TimeStyle0
       ,CONVERT(nvarchar(16),c1,121)AS TimeStyle121
       ,CONVERT(nvarchar(32),c2,0) AS Datetime2Style0
       ,CONVERT(nvarchar(32),c2,121)AS Datetime2Style121
FROM t1;

-- Returns values such as the following.
TimeStyle0       TimeStyle121
Datetime2Style0      Datetime2Style121
---------------- ----------------
-------------------- --------------------------
3:15PM           15:15:35.8100000
Jun  7 2011  3:15PM  2011-06-07 15:15:35.8130000
```

### <a name="e-variable-assignment---top-level-union-operator"></a>E. Attribution de variable - opérateur UNION de niveau supérieur
L'attribution de variable est autorisée dans une instruction contenant un opérateur UNION de niveau supérieur, celle-ci produit néanmoins des résultats inattendus. Par exemple, dans les instructions suivantes, la variable locale `@v` reçoit la valeur de la colonne `BusinessEntityID` issue de l'union de deux tables. Par définition, lorsque l'instruction SELECT retourne plusieurs valeurs, la dernière valeur retournée est affectée à la variable. Dans ce cas, la dernière valeur est attribuée correctement à la variable, toutefois, le jeu de résultats de l'instruction SELECT UNION est également retourné.

```sql
ALTER DATABASE AdventureWorks2012
SET compatibility_level = 110;
GO
USE AdventureWorks2012;
GO
DECLARE @v int;
SELECT @v = BusinessEntityID FROM HumanResources.Employee
UNION ALL
SELECT @v = BusinessEntityID FROM HumanResources.EmployeeAddress;
SELECT @v;
```

L'attribution de variable n'est pas autorisée dans une instruction contenant un opérateur UNION de niveau supérieur. L'erreur 10734 est retournée. Pour résoudre l'erreur, réécrivez la requête, comme dans l'exemple suivant.

```sql
DECLARE @v int;
SELECT @v = BusinessEntityID FROM
    (SELECT BusinessEntityID FROM HumanResources.Employee
     UNION ALL
     SELECT BusinessEntityID FROM HumanResources.EmployeeAddress) AS Test;
SELECT @v;
```

## <a name="see-also"></a>Voir aussi 
[Certification de compatibilité](../../database-engine/install-windows/compatibility-certification.md)       
[ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql.md)       
[Mots clés réservés](../../t-sql/language-elements/reserved-keywords-transact-sql.md)       
[CREATE DATABASE](../../t-sql/statements/create-database-transact-sql.md)       
[DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md)       
[sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)       
[sys.database_files](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md)       
[Afficher ou modifier le niveau de compatibilité d’une base de données](../../relational-databases/databases/view-or-change-the-compatibility-level-of-a-database.md)       
[Changer le mode de compatibilité de la base de données et utiliser le magasin des requêtes](../../database-engine/install-windows/change-the-database-compatibility-mode-and-use-the-query-store.md)       
[Mise à niveau des bases de données à l’aide de l’Assistant Paramétrage de requête](../../relational-databases/performance/upgrade-dbcompat-using-qta.md)
