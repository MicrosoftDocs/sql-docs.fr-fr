---
description: Gérer la conservation des données d’historique dans les tables temporelles versionnées par le système
title: Gérer les données d’historique dans les tables temporelles gérées par le système
ms.custom: seo-lt-2019
ms.date: 05/18/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
ms.assetid: 7925ebef-cdb1-4cfe-b660-a8604b9d2153
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 2e37f8234a1b8ee2ab72c76a423ea72ac9d3f14a
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/01/2021
ms.locfileid: "99235830"
---
# <a name="manage-retention-of-historical-data-in-system-versioned-temporal-tables"></a>Gérer la conservation des données d’historique dans les tables temporelles versionnées par le système


[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]


Avec les tables temporelles avec version gérée par le système, la table d’historique peut faire croître la taille de la base de données plus que les tables normales, surtout dans les conditions suivantes :

- conservation des données d’historique sur une longue période ;
- existence d’un modèle de mise à jour ou de suppression des modifications des données lourd.

Une table d’historique volumineuse et qui ne cesse de croître peut devenir un problème en termes de coûts de stockage purs et parce qu’elle nuit aux performances d’interrogation temporelle. Par conséquent, le développement d’une stratégie de rétention des données pour gérer les données dans la table d’historique est un aspect important de la planification et de la gestion du cycle de vie de chaque table temporelle.

## <a name="data-retention-management-for-history-table"></a>Gestion de la rétention de données pour la table d’historique

La gestion de la rétention de données pour les tables temporelles consiste en premier lieu à déterminer la période de rétention nécessaire pour chaque table temporelle. Dans la plupart des cas, votre stratégie de rétention doit être considérée comme faisant partie de la logique métier de l’application utilisant les tables temporelles. Par exemple, les applications s’inscrivant dans des scénarios d’audit de données et de voyage dans le temps ont des exigences strictes qui définissent la durée pendant laquelle les données d’historique doivent être disponibles pour l’interrogation en ligne.

Une fois la période de rétention de données déterminée, l’étape suivante consiste à élaborer un plan de gestion des données d’historique, à définir l’emplacement et le mode de stockage de ces données et à spécifier le mode de suppression des données d’historique anciennes eu égard à vos conditions de rétention. Il existe quatre approches pour la gestion des données d’historique dans la table d’historique temporelle :

- [Stretch Database](#using-stretch-database-approach)
- [Partitionnement de tables](#using-table-partitioning-approach)
- [Script de nettoyage personnalisé](#using-custom-cleanup-script-approach)
- [Stratégie de rétention](#using-temporal-history-retention-policy-approach)

 Pour chacune de ces méthodes, la logique de migration ou de nettoyage des données d’historique est basée sur la colonne qui correspond à la fin de période dans la table active. La valeur de fin de période de chaque ligne détermine le moment où la version de la ligne devient « fermée », c’est-à-dire où elle arrive dans la table d’historique. Par exemple, la condition `SysEndTime < DATEADD (DAYS, -30, SYSUTCDATETIME ())` spécifie que les données d’historique de plus d’un mois doivent être supprimées ou déplacées de la table d’historique.

> [!NOTE]
> Les exemples de cette rubrique utilisent cet [exemple de table temporelle](creating-a-system-versioned-temporal-table.md).

## <a name="using-stretch-database-approach"></a>Utilisation de la méthode stretch database

> [!NOTE]
> La méthode Stretch Database vaut uniquement pour [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] et ne s’applique pas à [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].

[Stretch Database](../../sql-server/stretch-database/stretch-database.md) de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] migre vos données d’historique en toute transparence vers Azure. Pour renforcer la sécurité, vous pouvez chiffrer les données en mouvement à l’aide de la fonctionnalité [Always Encrypted](../security/encryption/always-encrypted-database-engine.md) de SQL Server. De plus, vous pouvez utiliser la [sécurité au niveau des lignes](../../relational-databases/security/row-level-security.md) et les autres fonctionnalités de sécurité avancée de SQL Server avec Temporal et Stretch Database pour protéger vos données.

La méthode Stretch Database vous permet d’étendre tout ou partie des tables d’historique temporelles vers Azure. SQL Server déplace alors discrètement les données d’historique vers Azure. Le fait d’activer Strech pour une table d’historique ne change rien à la façon dont vous interagissez avec la table temporelle en termes de modification des données et d’interrogation temporelle.

- **Étendre l’ensemble de la table d’historique :** configurez Stretch Database pour l’ensemble de la table d’historique si votre scénario principal est l’audit des données dans l’environnement qui implique des modifications de données fréquentes et des interrogations relativement rares sur les données historiques. En d’autres termes, employez cette méthode si les performances de l’interrogation temporelle ne sont pas un critère prépondérant. Dans ce cas, l’avantage offert par Azure sur le plan de la rentabilité peut s’avérer intéressant. Quand il s’agit d’étendre la table d’historique dans son ensemble, vous pouvez utiliser l’Assistant Stretch ou Transact-SQL. Vous trouverez à la suite des exemples des deux.

- **Étirer une partie de la table d’historique :** vous pouvez configurer Stretch Database pour une partie de votre table d’historique à des fins de performances si votre scénario principal consiste essentiellement à interroger les données d’historique récentes, mais que vous souhaitez toujours pouvoir interroger des données d’historique plus anciennes quand cela est nécessaire tout en stockant ces données à distance à moindre coût. Avec Transact-SQL, vous pouvez faire cela en spécifiant une fonction de prédicat pour sélectionner les seules lignes qui seront migrées à partir de la table d’historique, et non l’ensemble des lignes. Quand vous utilisez des tables temporelles, il est généralement judicieux de déplacer les données en fonction d’une condition temporelle (par exemple, selon l’ancienneté de la version de ligne dans la table d’historique).

  En utilisant une fonction de prédicat déterministe, vous pouvez conserver une partie de l’historique dans la même base de données avec les données actuelles, tandis que le reste est migré vers Azure. Pour les exemples et les limitations, consultez [Sélectionner les lignes à migrer à l’aide d’une fonction de filtre (Stretch Database)](../../sql-server/stretch-database/select-rows-to-migrate-by-using-a-filter-function-stretch-database.md). Les fonctions non déterministes n’étant pas valides, si vous souhaitez transférer des données d’historique à la manière d’une fenêtre glissante, vous devez modifier régulièrement la définition de la fonction de prédicat inline de sorte que la fenêtre de lignes que vous conservez localement soit constante en termes d’ancienneté. La fenêtre glissante vous permet de déplacer constamment les données d’historique de plus d’un mois vers Azure. Cette méthode est illustrée dans l’exemple ci-après.

> [!NOTE]
> Stretch Database migre les données vers Azure. Par conséquent, vous devez disposer d’un compte Azure et d’un abonnement pour la facturation. Pour obtenir un compte d’évaluation gratuite Azure, cliquez sur [Évaluation d’un mois gratuite](https://azure.microsoft.com/pricing/free-trial/).

Vous pouvez configurer une table d’historique temporelle pour Stretch à l’aide de l’Assistant Stretch ou de Transact-SQL, et vous pouvez activer Stretch pour une table d’historique temporelle tout en ayant la gestion système des versions définie sur **ACTIVÉ**. L’extension de la table active n’est pas autorisée, car une telle opération ne se justifie pas.

### <a name="using-the-stretch-wizard-to-stretch-the-entire-history-table"></a>Étendre l’ensemble de la table d’historique à l’aide de l’Assistant Stretch

Pour les débutants, la méthode la plus simple consiste à utiliser l’Assistant Stretch pour activer Stretch pour la base de données entière, puis à sélectionner la table d’historique temporelle dans l’Assistant Stretch (cet exemple part du principe que vous avez configuré la table Department en tant que table temporelle avec versions gérées par le système dans une base de données autrement vide). Dans [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], vous ne pouvez pas cliquer avec le bouton droit sur la table d’historique temporelle proprement dite et cliquer sur Stretch.

1. Cliquez avec le bouton droit sur votre base de données et pointez sur **Tâches**, sur **Stretch**, puis cliquez sur **Activer** pour lancer l’Assistant.
2. Dans la fenêtre **Sélectionner des tables** , cochez la case de la table d’historique temporelle et cliquez sur Suivant.

    ![Sélection de la table d’historique dans la page Sélectionner des tables](../../relational-databases/tables/media/stretch-wizard-2-for-temporal.png "Sélection de la table d’historique dans la page Sélectionner des tables")
3. Dans la fenêtre **Configurer Azure** , fournissez vos informations d’identification de connexion. Connectez-vous à Microsoft Azure ou inscrivez-vous pour ouvrir un compte. Sélectionnez l’abonnement à utiliser, ainsi que la région Azure. Créez ensuite un serveur ou sélectionnez-en un existant. Cliquez sur **Suivant**.

    ![Créer un serveur Azure - Assistant Stretch Database](../../relational-databases/tables/media/stretch-wizard-4.png "Créer un serveur Azure - Assistant Stretch Database")
4. Dans la fenêtre **Informations d’identification sécurisées** , indiquez un mot de passe pour la clé principale de la base de données afin de sécuriser vos informations d’identification de base de données SQL Server source et cliquez sur Suivant.

    ![Page d’informations d’identification sécurisées de l’Assistant Stretch Database](../../relational-databases/tables/media/stretch-wizard-6.png "Page d’informations d’identification sécurisées de l’Assistant Stretch Database")
5. Dans la fenêtre **Sélectionner une adresse IP**, indiquez la plage d’adresses IP pour que votre serveur SQL Server permette à votre serveur Azure de communiquer avec lui (si vous sélectionnez un serveur existant pour lequel une règle de pare-feu existe déjà, il suffit de cliquer sur Suivant ici pour utiliser cette règle de pare-feu existante). Cliquez sur **Suivant**, puis sur **Terminer** pour activer Stretch Database pour la table d’historique temporelle.

    ![Page de sélection de l’adresse IP de l’Assistant Stretch Database](../../relational-databases/tables/media/stretch-wizard-7.png "Page de sélection de l’adresse IP de l’Assistant Stretch Database")
6. Quand l’Assistant a terminé, vérifiez que Stretch est correctement activé pour votre base de données. Notez que les icônes de l’Explorateur d’objets indiquent que Stretch a été activé pour la base de données.

> [!NOTE]
> Si Activer la base de données pour Stretch échoue, consultez le journal des erreurs. Une erreur courante consiste à configurer incorrectement la règle de pare-feu.

Voir aussi :

- [Activer Stretch Database pour une base de données](../../sql-server/stretch-database/enable-stretch-database-for-a-database.md)
- [Commencez par exécuter l’Assistant Activer la base de données pour Stretch](../../sql-server/stretch-database/get-started-by-running-the-enable-database-for-stretch-wizard.md)
- [Activer Stretch Database pour une table](../../sql-server/stretch-database/enable-stretch-database-for-a-table.md)

### <a name="using-transact-sql-to-stretch-the-entire-history-table"></a>Utilisation de Transact-SQL pour activer Stretch pour toute la table d’historique

Vous pouvez également utiliser Transact-SQL pour activer Stretch sur le serveur local et [Activer Stretch Database pour une base de données](../../sql-server/stretch-database/enable-stretch-database-for-a-database.md). Vous pouvez ensuite utiliser [Transact-SQL pour activer Stretch Database sur une table](../../sql-server/stretch-database/enable-stretch-database-for-a-table.md). Avec une base de données préalablement activée pour Stretch Database, exécutez le script Transact-SQL suivant pour activer Stretch sur une table d’historique temporelle avec version gérée par le système existante :

```sql
ALTER TABLE <history table name>
SET (REMOTE_DATA_ARCHIVE = ON (MIGRATION_STATE = OUTBOUND));
```

### <a name="using-transact-sql-to-stretch-a-portion-of-the-history-table"></a>Utilisation de Transact-SQL pour activer Stretch sur une partie de la table d’historique

Pour activer Stretch uniquement sur une partie de la table d’historique, commencez par créer une [fonction de prédicat inline](../../sql-server/stretch-database/select-rows-to-migrate-by-using-a-filter-function-stretch-database.md). Pour cet exemple, supposons que vous avez configuré la fonction de prédicat incluse pour la première fois le 1er décembre 2015 et que voulez activer Stretch sur Azure pour toutes les dates d’historique antérieures au 1er novembre 2015. Pour ce faire, commencez par créer la fonction suivante :

```sql
CREATE FUNCTION dbo.fn_StretchBySystemEndTime20151101(@systemEndTime datetime2)
RETURNS TABLE
WITH SCHEMABINDING
AS
RETURN SELECT 1 AS is_eligible
  WHERE @systemEndTime < CONVERT(datetime2, '2015-11-01T00:00:00', 101) ;
```

Ensuite, utilisez le script suivant pour ajouter le prédicat de filtre à la table d’historique et définir l’état de migration sur OUTBOUND (sortant) pour permettre la migration des données en fonction du prédicat pour la table d’historique.

```sql
ALTER TABLE <history table name>
SET (
      REMOTE_DATA_ARCHIVE = ON
        (
          FILTER_PREDICATE = dbo.fn_StretchBySystemEndTime20151101 (SysEndTime)
            , MIGRATION_STATE = OUTBOUND
        )
    )
;
```

Pour conserver une fenêtre glissante, vous devez faire en sorte que la fonction de prédicat soit précise tous les jours (c’est-à-dire changer la condition de filtrage des lignes chaque jour d’un jour). Le script suivant est celui que vous devriez exécuter le 2 décembre 2015 :

```sql
BEGIN TRAN
/*(1) Create new predicate function definition */
  CREATE FUNCTION dbo.fn_StretchBySystemEndTime20151102(@systemEndTime datetime2)
   RETURNS TABLE
    WITH SCHEMABINDING
      AS
        RETURN SELECT 1 AS is_eligible
          WHERE @systemEndTime < CONVERT(datetime2,'2015-11-02T00:00:00', 101)
  GO
 
/*(2) Set the new function as filter predicate */
  ALTER TABLE <history table name>
    SET
      (
        REMOTE_DATA_ARCHIVE = ON
          (
            FILTER_PREDICATE = dbo.fn_StretchBySystemEndTime20151102(SysEndTime),
              MIGRATION_STATE = OUTBOUND
          )
      )
COMMIT ;
```

Utilisez SQL Server Agent ou tout autre mécanisme de planification pour vérifier en permanence la validité de la définition de la fonction de prédicat.

## <a name="using-table-partitioning-approach"></a>Utilisation de la méthode de partitionnement de table

[Le partitionnement de table](../partitions/create-partitioned-tables-and-indexes.md) peut favoriser la facilité de gestion et l’extensibilité des tables volumineuses. Avec cette méthode, vous pouvez utiliser des partitions de table d’historique pour implémenter un nettoyage de données personnalisé ou un archivage hors connexion basé sur une condition temporelle. Le partitionnement de table s’avère aussi bénéfique en termes de performances quand il s’agit d’interroger les tables temporelles d’un sous-ensemble d’historique de données grâce à l’élimination de partition.

Avec le partitionnement de table, vous pouvez implémenter une approche de fenêtre glissante pour déplacer une partie plus ancienne des données d’historique de la table d’historique et ainsi stabiliser la taille de la partie conservée en termes d’ancienneté (en conservant dans la table d’historique les données correspondant à la période de rétention requise). L’opération d’extraction des données de la table d’historique est prise en charge quand SYSTEM_VERSIONING a la valeur ON, ce qui signifie que vous pouvez nettoyer une partie des données d’historique sans introduire de fenêtres de maintenance ni bloquer vos charges de travail normales.

> [!NOTE]
> Pour procéder au basculement de partition, l’index cluster de la table d’historique doit être aligné sur le schéma de partitionnement (il doit contenir SysEndTime). La table d’historique par défaut créée par le système contient un index cluster qui inclut les colonnes SysEndTime et SysStartTime, ce qui est idéal pour le partitionnement, l’insertion de nouvelles données d’historique et l’interrogation temporelle classique. Pour plus d’informations, voir [Temporal Tables](../../relational-databases/tables/temporal-tables.md).

Dans une approche de fenêtre glissante, il y a deux ensembles de tâches à effectuer :

- une tâche de configuration du partitionnement ;
- des tâches de maintenance de partition périodiques.

Pour illustrer cela, supposons que vous voulez conserver les données d’historique pendant 6 mois et que vous voulez conserver les données de chaque mois dans une partition distincte. Par ailleurs, supposons que vous avez activé le contrôle de version du système d’exploitation en septembre 2015.

Une tâche de configuration du partitionnement permet de créer la configuration initiale du partitionnement de la table d’historique. Pour cet exemple, il faudrait créer le même nombre de partitions que la taille de fenêtre défilante, exprimée en mois, plus une partition vide préparée à l’avance (voir ci-après). Cette configuration est l’assurance que le système pourra stocker correctement les nouvelles données quand la tâche de maintenance périodique sera lancée pour la première fois. De même, elle garantit que les partitions ne seront jamais fractionnées avec des données pour éviter des déplacement des données coûteux. Vous devez effectuer cette tâche à l’aide de Transact-SQL en utilisant le script d’exemple ci-après.

Le schéma suivant illustre la configuration initiale du partitionnement visant à conserver six mois de données.

![Diagramme montrant la configuration initiale du partitionnement visant à conserver six mois de données.](../../relational-databases/tables/media/partitioning.png "Partitionnement")

> [!NOTE]
> Consultez la section Considérations relatives aux performances du partitionnement de table ci-après pour en savoir plus sur les conséquences d’une utilisation de RANGE LEFT plutôt que RANGE RIGHT sur les performances lors de la configuration du partitionnement.

La première et la dernière partition sont toutes deux « ouvertes » au niveau des limites inférieure et supérieure, respectivement. Chaque nouvelle ligne est donc assurée de trouver une partition de destination, quelle que soit la valeur de la colonne de partitionnement. Au fil du temps, les nouvelles lignes de la table d’historique atterriront dans les partitions supérieures. Quand la sixième partition sera remplie, la période de rétention ciblée aura été atteinte. C’est à ce moment-là que la tâche de maintenance périodique sera lancée pour la première fois (elle doit être planifiée pour s’exécuter périodiquement, une fois par mois dans cet exemple).

Le schéma suivant illustre les tâches de maintenance périodique de partition (voir la procédure détaillée ci-dessous).

![Diagramme montrant les tâches de maintenance de partition périodiques.](../../relational-databases/tables/media/partitioning2.png "Partitionnement2")

Voici la procédure à suivre pour effectuer les tâches de maintenance périodique de partition :

1. SWITCH OUT : créez une table de mise en lots et faites un échange de partition entre la table d’historique et la table de mise en lots en utilisant l’instruction [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md) avec l’argument SWITCH PARTITION (voir l’exemple C, Échange de partitions entre des tables).

    ```sql
    ALTER TABLE <history table> SWITCH PARTITION 1 TO <staging table>
    ```

    À l’issue de l’échange de partition, vous pouvez éventuellement archiver les données de la table de mise en lots et supprimer ou tronquer cette même table pour la préparer à la prochaine tâche de maintenance périodique de partition.
2. MERGE RANGE : fusionnez la partition vide 1 avec la partition 2 en utilisant [ALTER PARTITION FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-partition-function-transact-sql.md) avec MERGE RANGE (consultez l’exemple B). En supprimant la limite inférieure avec cette fonction, vous fusionnez en réalité la partition vide 1 avec l’ancienne partition 2 pour former une nouvelle partition 1. Les ordinaux des autres partitions changent également.
3. SPLIT RANGE : créez une partition vide 7 en utilisant [ALTER PARTITION FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-partition-function-transact-sql.md) avec SPLIT RANGE (consultez l’exemple A). En ajoutant une nouvelle limite supérieure avec cette fonction, vous créez en réalité une partition distincte pour le mois à venir.

### <a name="use-transact-sql-to-create-partitions-on-history-table"></a>Créer des partitions dans la table d’historique avec Transact-SQL

Utilisez le script Transact-SQL figurant dans la fenêtre de code ci-dessous pour créer la fonction de partition, le schéma de partition et recréer l’index cluster qui doit être aligné sur les partitions avec le schéma de partition, les partitions. Pour cet exemple, nous allons créer une approche de fenêtre glissante de six mois avec des partitions mensuelles débutant à septembre 2015.

```sql
BEGIN TRANSACTION
/*Create partition function*/
    CREATE PARTITION FUNCTION [fn_Partition_DepartmentHistory_By_SysEndTime] (datetime2(7))
        AS RANGE LEFT FOR VALUES
          (
            N'2015-09-30T23:59:59.999'
          , N'2015-10-31T23:59:59.999'
          , N'2015-11-30T23:59:59.999'
          , N'2015-12-31T23:59:59.999'
          , N'2016-01-31T23:59:59.999'
          , N'2016-02-29T23:59:59.999'
          )
/*Create partition scheme*/
    CREATE PARTITION SCHEME [sch_Partition_DepartmentHistory_By_SysEndTime]
        AS PARTITION [fn_Partition_DepartmentHistory_By_SysEndTime]
            TO ([PRIMARY], [PRIMARY], [PRIMARY], [PRIMARY], [PRIMARY], [PRIMARY], [PRIMARY])
/*Re-create index to be partition-aligned with the partitioning schema*/
    CREATE CLUSTERED INDEX [ix_DepartmentHistory] ON [dbo].[DepartmentHistory]
        (
            [SysEndTime] ASC
          , [SysStartTime] ASC
        )  
    WITH
        (
            PAD_INDEX = OFF
          , STATISTICS_NORECOMPUTE = OFF
          , SORT_IN_TEMPDB = OFF
          , DROP_EXISTING = ON
          , ONLINE = OFF
          , ALLOW_ROW_LOCKS = ON
          , ALLOW_PAGE_LOCKS = ON
          , DATA_COMPRESSION = PAGE
        )
    ON [sch_Partition_DepartmentHistory_By_SysEndTime] ([SysEndTime])

COMMIT TRANSACTION;
```

### <a name="using-transact-sql-to-maintain-partitions-in-sliding-window-scenario"></a>Gérer les partitions dans un scénario de fenêtre glissante à l’aide de Transact-SQL

Utilisez le script Transact-SQL figurant dans la fenêtre de code ci-dessous pour gérer les partitions dans le scénario de fenêtre glissante. Pour cet exemple, nous allons extraire la partition de septembre 2015 en utilisant MERGE RANGE et ajouter une nouvelle partition pour mars 2016 en utilisant SPLIT RANGE.

```sql
BEGIN TRANSACTION
/*(1) Create staging table */
    CREATE TABLE [dbo].[staging_DepartmentHistory_September_2015]
        (
            [DeptID] [int] NOT NULL
          , [DeptName] [varchar](50) COLLATE SQL_Latin1_General_CP1_CI_AS NOT NULL
          , [ManagerID] [int] NULL
          , [ParentDeptID] [int] NULL
          , [SysStartTime] [datetime2](7) NOT NULL
          , [SysEndTime] [datetime2](7) NOT NULL
        ) ON [PRIMARY]
    WITH
        (
            DATA_COMPRESSION = PAGE
        )
/*(2) Create index on the same filegroups as the partition that will be switched out*/
    CREATE CLUSTERED INDEX [ix_staging_DepartmentHistory_September_2015]
        ON [dbo].[staging_DepartmentHistory_September_2015]
        (
            [SysEndTime] ASC
          , [SysStartTime] ASC
        )
    WITH
        (
            PAD_INDEX = OFF
          , SORT_IN_TEMPDB = OFF
          , DROP_EXISTING = OFF
          , ONLINE = OFF
          , ALLOW_ROW_LOCKS = ON
          , ALLOW_PAGE_LOCKS = ON
        )
    ON [PRIMARY]
/*(3) Create constraints matching the partition that will be switched out*/
    ALTER TABLE [dbo].[staging_DepartmentHistory_September_2015] WITH CHECK
        ADD CONSTRAINT [chk_staging_DepartmentHistory_September_2015_partition_1]
            CHECK ([SysEndTime]<=N'2015-09-30T23:59:59.999')
    ALTER TABLE [dbo].[staging_DepartmentHistory_September_2015]
        CHECK CONSTRAINT [chk_staging_DepartmentHistory_September_2015_partition_1]
/*(4) Switch partition to staging table*/
    ALTER TABLE [dbo].[DepartmentHistory]
        SWITCH PARTITION 1 TO [dbo].[staging_DepartmentHistory_September_2015]
        WITH (WAIT_AT_LOW_PRIORITY (MAX_DURATION = 0 MINUTES, ABORT_AFTER_WAIT = NONE))
/*(5) [Commented out] Optionally archive the data and drop staging table
      INSERT INTO [ArchiveDB].[dbo].[DepartmentHistory]
      SELECT * FROM [dbo].[staging_DepartmentHistory_September_2015];
      DROP TABLE [dbo].[staging_DepartmentHIstory_September_2015];
*/
/*(6) merge range to move lower boundary one month ahead*/
    ALTER PARTITION FUNCTION [fn_Partition_DepartmentHistory_By_SysEndTime]()
        MERGE RANGE(N'2015-09-30T23:59:59.999')
/*(7) Create new empty partition for "April and after" by creating new boundary point and specifying NEXT USED file group*/
    ALTER PARTITION SCHEME [sch_Partition_DepartmentHistory_By_SysEndTime] NEXT USED [PRIMARY]
        ALTER PARTITION FUNCTION [fn_Partition_DepartmentHistory_By_SysEndTime]() SPLIT RANGE(N'2016-03-31T23:59:59.999')
COMMIT TRANSACTION
```

Vous pouvez rectifier légèrement le script ci-dessus et l’utiliser dans le processus normal de maintenance mensuelle :

1. À l’étape (1), il convient de créer une table de mise en lots intermédiaire pour le mois à supprimer (octobre serait le prochain mois dans notre exemple).
2. À l’étape (3), une contrainte de validation correspondant au mois de données à supprimer est créée : `[SysEndTime]<=N'2015-10-31T23:59:59.999'` pour la partition d’octobre.
3. À l’étape (4), la partition 1 est basculée (SWITCH) vers la table de mise en lots créée.
4. À l’étape (6), la fonction de partition est modifiée par la fusion de la limite inférieure : `MERGE RANGE(N'2015-10-31T23:59:59.999'` après le déplacement des données d’octobre.
5. À l’étape (7), la fonction de partition est fractionnée par la création d’une nouvelle limite supérieure : `SPLIT RANGE (N'2016-04-30T23:59:59.999'` après le déplacement des données d’octobre.

Cependant, la solution idéale serait d’exécuter régulièrement un script Transact-SQL générique qui soit capable effectuer l’action appropriée chaque mois sans modification du script. Il est possible de généraliser le script précédent de sorte qu’il agisse en fonction des paramètres fournis (limite inférieure devant être fusionnée et nouvelle limite créée avec fractionnement de partition). Pour éviter qu’une table de mise en lots soit créée tous les mois, vous pouvez en créer une à l’avance et la réutiliser en modifiant la contrainte de validation en fonction de la partition qui sera extraite. Consultez les pages suivantes pour savoir [comment automatiser entièrement une fenêtre glissante](/previous-versions/sql/sql-server-2005/administrator/aa964122(v=sql.90)) à l’aide d’un script Transact-SQL.

### <a name="performance-considerations-with-table-partitioning"></a>Considérations relatives aux performances du partitionnement de table

Il est important d’effectuer les opérations MERGE et SPLIT RANGE pour éviter tout déplacement de données, qui peut entraîner une baisse significative des performances. Pour plus d’informations, consultez [Modifier une fonction de partition](../../relational-databases/partitions/modify-a-partition-function.md). Pour ce faire, utilisez RANGE LEFT au lieu de RANGE RIGHT quand vous utilisez [CREATE PARTITION FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/create-partition-function-transact-sql.md).

Pour commencer, expliquons visuellement la signification des options RANGE LEFT et RANGE RIGHT :

![Diagramme montrant les options RANGE LEFT et RANGE RIGHT.](../../relational-databases/tables/media/partitioning3.png "Partitionnement3")

Quand vous définissez une fonction de partition avec RANGE LEFT, les valeurs spécifiées correspondent aux limites supérieures des partitions. Quand vous utilisez RANGE RIGHT, les valeurs spécifiées correspondent aux limites inférieures des partitions. Quand vous utilisez l’opération MERGE RANGE pour supprimer une limite de la définition de la fonction de partition, l’implémentation sous-jacente supprime aussi la partition qui contient la limite. Si cette partition n’est pas vide, les données sont déplacées dans la partition qui résulte de l’opération MERGE RANGE.

Dans un scénario de fenêtre glissante, la limite de partition inférieure est toujours supprimée.

- Cas RANGE LEFT : dans le cas de RANGE LEFT, la limite de partition inférieure appartient à la partition 1, qui est vide (après l’extraction de partition). Autrement dit, MERGE RANGE ne subit aucun déplacement de données.
- Cas RANGE RIGHT : dans le cas de RANGE RIGHT, la limite inférieure appartient à la partition 2, qui n’est pas vide, étant entendu que la partition 1 a été vidée par l’extraction. Dans ce cas, MERGE RANGE entraîne un déplacement de données (les données de la partition 2 sont déplacées vers la partition 1). Pour éviter cela, dans le scénario de fenêtre glissante, RANGE RIGHT doit avoir la partition 1, qui est toujours vide. Cela signifie que si nous utilisons RANGE RIGHT, nous devons créer et maintenir une partition supplémentaire par rapport au cas RANGE LEFT.

**Conclusion** : l’utilisation de RANGE LEFT dans une partition glissante facilite grandement la gestion des partitions et évite le déplacement des données. Cependant, définir les limites de partition avec RANGE RIGHT s’avère un peu plus simple, car vous n’êtes pas confronté aux problèmes de cycle datetime/time.

## <a name="using-custom-cleanup-script-approach"></a>Utilisation de la méthode de script de nettoyage personnalisé

Dans les cas où la méthode Stretch Database et le partitionnement de table ne sont pas des options viables, la troisième méthode consiste à supprimer les données de la table d’historique à l’aide d’un script de nettoyage personnalisé. La suppression des données de la table d’historique n’est possible que lorsque **SYSTEM_VERSIONING = OFF**. Pour éviter une incohérence de données, procédez à un nettoyage pendant la fenêtre de maintenance (quand les charges de travail qui modifient les données ne sont pas actives) ou lors d’une transaction (les autres charges de travail sont alors bloquées). Cette opération nécessite une autorisation **CONTROL** sur les tables actives et d’historique.

Pour éviter de trop bloquer les applications usuelles et les requêtes utilisateur, supprimez les données en blocs plus petits en prévoyant un laps de temps pendant l’exécution du script de nettoyage dans une transaction. Même s’il n’y a pas de taille optimale pour chaque bloc de données à supprimer pour tous les scénarios, le fait de supprimer plus de 10 000 lignes dans une même transaction peut avoir un impact important.

La logique de nettoyage étant identique pour toutes les tables temporelles, il est relativement facile de l’automatiser via une procédure stockée générique dont vous planifiez l’exécution périodique pour chaque table temporelle dont vous voulez limiter l’historique des données.

Le diagramme suivant illustre la façon dont votre logique de nettoyage doit être organisée pour une table unique afin de réduire l’impact sur les charges de travail en cours d’exécution.

![Diagramme montrant la façon dont votre logique de nettoyage doit être organisée pour une table unique afin de réduire l’impact sur les charges de travail en cours d’exécution.](../../relational-databases/tables/media/customcleanupscriptdiagram.png "CustomCleanUpScriptDiagram")

Voici quelques indications générales pour implémenter le processus. Planifiez une exécution quotidienne de la logique de nettoyage, ainsi que son itération sur toutes les tables temporelles nécessitant un nettoyage de données. À l’aide de SQL Server Agent ou d’un autre outil, planifiez ce processus :

- Supprimez les données d’historique dans chaque table temporelle en partant des lignes les plus anciennes aux plus récentes en plusieurs itérations et sur de petits blocs et évitez de supprimer toutes les lignes d’une même transaction unique, comme indiqué dans le schéma ci-dessus.
- Implémentez chaque itération comme un appel de procédure stockée générique qui supprime une partie des données de la table d’historique (voir l’exemple de code ci-dessous pour cette procédure).
- Calculez le nombre de lignes que vous devez supprimer pour une table temporelle unique chaque fois que vous appelez le processus. Sur la base de ce nombre et du nombre d’itérations souhaité, déterminez les points de fractionnement dynamique pour chaque appel de procédure.
- Prévoyez un laps de temps entre les itérations pour une table unique de façon à réduire l’impact sur les applications qui accèdent à la table temporelle.

Une procédure stockée qui supprime les données pour une table temporelle unique peut rappeler l’extrait de code suivant (examinez attentivement ce code et ajustez-le avant de l’appliquer dans votre environnement) :

```sql
DROP PROCEDURE IF EXISTS sp_CleanupHistoryData;
GO

CREATE PROCEDURE sp_CleanupHistoryData
        @temporalTableSchema sysname
      , @temporalTableName sysname
      , @cleanupOlderThanDate datetime2
AS
    DECLARE @disableVersioningScript nvarchar(max) = '';
    DECLARE @deleteHistoryDataScript nvarchar(max) = '';
    DECLARE @enableVersioningScript nvarchar(max) = '';

DECLARE @historyTableName sysname
DECLARE @historyTableSchema sysname
DECLARE @periodColumnName sysname

/*Generate script to discover history table name and end of period column for given temporal table name*/
EXECUTE sp_executesql
    N'SELECT @hst_tbl_nm = t2.name, @hst_sch_nm = s.name, @period_col_nm = c.name
        FROM sys.tables t1
            JOIN sys.tables t2 on t1.history_table_id = t2.object_id
        JOIN sys.schemas s on t2.schema_id = s.schema_id
            JOIN sys.periods p on p.object_id = t1.object_id
           JOIN sys.columns c on p.end_column_id = c.column_id and c.object_id = t1.object_id
                  WHERE
                 t1.name = @tblName and s.name = @schName'
                , N'@tblName sysname
                , @schName sysname
                , @hst_tbl_nm sysname OUTPUT
                , @hst_sch_nm sysname OUTPUT
                , @period_col_nm sysname OUTPUT'
                , @tblName = @temporalTableName
                , @schName = @temporalTableSchema
                , @hst_tbl_nm = @historyTableName OUTPUT
                , @hst_sch_nm = @historyTableSchema OUTPUT
                , @period_col_nm = @periodColumnName OUTPUT
  
IF @historyTableName IS NULL OR @historyTableSchema IS NULL OR @periodColumnName IS NULL
    THROW 50010, 'History table cannot be found. Either specified table is not system-versioned temporal or you have provided incorrect argument values.', 1

/*Generate 3 statements that will run inside a transaction:
  (1) SET SYSTEM_VERSIONING = OFF,
  (2) DELETE FROM history_table,
  (3) SET SYSTEM_VERSIONING = ON
  On SQL Server 2016, it is critical that (1) and (2) run in separate EXEC statements, or SQL Server will generate the following error:
  Msg 13560, Level 16, State 1, Line XXX
  Cannot delete rows from a temporal history table '<database_name>.<history_table_schema_name>.<history_table_name>'.
*/
SET @disableVersioningScript = @disableVersioningScript + 'ALTER TABLE [' + @temporalTableSchema + '].[' + @temporalTableName + '] SET (SYSTEM_VERSIONING = OFF)'
SET @deleteHistoryDataScript = @deleteHistoryDataScript + ' DELETE FROM [' + @historyTableSchema + '].[' + @historyTableName + ']
    WHERE ['+ @periodColumnName + '] < ' + '''' + convert(varchar(128), @cleanupOlderThanDate, 126) + ''''
SET @enableVersioningScript = @enableVersioningScript + ' ALTER TABLE [' + @temporalTableSchema + '].[' + @temporalTableName + ']
    SET (SYSTEM_VERSIONING = ON (HISTORY_TABLE = [' + @historyTableSchema + '].[' + @historyTableName + '], DATA_CONSISTENCY_CHECK = OFF )); '

BEGIN TRAN
    EXEC (@disableVersioningScript);
    EXEC (@deleteHistoryDataScript);
    EXEC (@enableVersioningScript);
COMMIT;
```

## <a name="using-temporal-history-retention-policy-approach"></a>Utilisation de l’approche de la stratégie de conservation d’historique temporelle

> [!NOTE]
> L’utilisation de l’approche de la stratégie de rétention d’historique temporelle s’applique à [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)] et à SQL Server 2017 (à partir de la version CTP 1.3).

La rétention d’historique temporelle peut être configurée au niveau de la table individuelle, ce qui permet aux utilisateurs de créer des stratégies de vieillissement flexibles. L’application de la rétention temporelle est simple : un seul paramètre doit être défini durant la création de la table ou le changement de schéma.

Une fois que vous avez défini la stratégie de conservation, Azure SQL Database commence par vérifier régulièrement s’il existe des lignes d’historique éligibles pour le nettoyage automatique des données. L’identification des lignes correspondantes et leur suppression de la table d’historique se produisent de façon transparente, dans la tâche en arrière-plan planifiée et exécutée par le système. La condition d’âge des lignes de la table d’historique est vérifiée en fonction de la colonne représentant la fin de la période SYSTEM_TIME. Par exemple, si la période de rétention est définie à six mois, les lignes de table éligibles pour un nettoyage répondent à la condition suivante :

```sql
ValidTo < DATEADD (MONTH, -6, SYSUTCDATETIME())
```
Dans l’exemple précédent, nous avons supposé que la colonne ValidTo correspond à la fin de la période SYSTEM_TIME.

### <a name="how-to-configure-retention-policy"></a>Configuration d’une stratégie de rétention

Avant de configurer la stratégie de rétention d’une table temporelle, vérifiez d’abord si la rétention d’historique temporelle est activée au niveau de la base de données :

```sql
SELECT is_temporal_history_retention_enabled, name
FROM sys.databases
```

L’indicateur de base de données **is_temporal_history_retention_enabled** a la valeur ON par défaut, mais les utilisateurs peuvent le changer à l’aide de l’instruction ALTER DATABASE. Il prend automatiquement la valeur OFF après une opération de limite de restauration dans le temps. Si vous souhaitez activer le nettoyage de la rétention d’historique temporelle pour votre base de données, exécutez l’instruction suivante :

```sql
ALTER DATABASE <myDB>
SET TEMPORAL_HISTORY_RETENTION ON
```

La stratégie de rétention est configurée durant la création de la table quand vous spécifiez la valeur du paramètre HISTORY_RETENTION_PERIOD :

```sql
CREATE TABLE dbo.WebsiteUserInfo
(
    [UserID] int NOT NULL PRIMARY KEY CLUSTERED
  , [UserName] nvarchar(100) NOT NULL
  , [PagesVisited] int NOT NULL
  , [ValidFrom] datetime2 (0) GENERATED ALWAYS AS ROW START
  , [ValidTo] datetime2 (0) GENERATED ALWAYS AS ROW END
  , PERIOD FOR SYSTEM_TIME (ValidFrom, ValidTo)
 )
 WITH
 (
    SYSTEM_VERSIONING = ON
    (
        HISTORY_TABLE = dbo.WebsiteUserInfoHistory,
        HISTORY_RETENTION_PERIOD = 6 MONTHS
    )
 );
```

Vous pouvez spécifier la période de rétention à l’aide de différentes unités de temps : DAYS, WEEKS, MONTHS et YEARS. Si HISTORY_RETENTION_PERIOD est omis, la rétention est censée être INFINITE. Vous pouvez également utiliser explicitement le mot clé INFINITE.
Dans certains scénarios, vous pouvez configurer la rétention après la création de la table, ou pour changer une valeur configurée. Dans ce cas, utilisez l’instruction ALTER TABLE :

```sql
ALTER TABLE dbo.WebsiteUserInfo
SET (SYSTEM_VERSIONING = ON (HISTORY_RETENTION_PERIOD = 9 MONTHS));
```

Pour passer en revue l’état actuel de la stratégie de rétention, utilisez la requête suivante qui permet de joindre l’indicateur d’activation de rétention temporelle au niveau de la base de données aux périodes de rétention de tables individuelles :

```sql
SELECT DB.is_temporal_history_retention_enabled,
SCHEMA_NAME(T1.schema_id) AS TemporalTableSchema,
T1.name as TemporalTableName, SCHEMA_NAME(T2.schema_id) AS HistoryTableSchema,
T2.name as HistoryTableName,T1.history_retention_period,
T1.history_retention_period_unit_desc
FROM sys.tables T1
OUTER APPLY (select is_temporal_history_retention_enabled from sys.databases
where name = DB_NAME()) AS DB
LEFT JOIN sys.tables T2
ON T1.history_table_id = T2.object_id WHERE T1.temporal_type = 2
```

### <a name="how-sql-database-deletes-aged-rows"></a>Comment SQL Database supprime les anciennes lignes

Le processus de nettoyage dépend de la disposition de l’index de la table d’historique. Il est important de noter que *seules les tables d’historique avec un index cluster (arbre B (B-tree) ou columnstore) peuvent avoir une stratégie de rétention limitée et configurée*. Une tâche en arrière-plan est créée afin d’effectuer le nettoyage des anciennes données pour toutes les tables temporelles dont la période de rétention est limitée. La logique de nettoyage de l’index cluster rowstore (arbre B (B-tree)) supprime les anciennes lignes par petits blocs (jusqu’à 10 Ko), ce qui réduit la pression exercée sur le journal de la base de données et le sous-système d’E/S. Bien que la logique de nettoyage utilise l’index d’arbre B (B-tree) nécessaire, l’ordre des suppressions des lignes antérieures à la période de rétention ne peut pas être garanti. Vous ne devez donc *pas accepter de dépendances relatives à l’ordre de nettoyage dans vos applications*.

La tâche de nettoyage de l’index columnstore cluster supprime des groupes de lignes entiers (qui contiennent généralement 1 million de lignes chacun), ce qui est très efficace, en particulier quand les données d’historique sont générées à un rythme élevé.

![Rétention de columnstore en cluster](../../relational-databases/tables/media/cciretention.png "Rétention de columnstore en cluster")

En raison de l’excellence de la compression des données et de l’efficacité du nettoyage de la rétention, l’index columnstore cluster est un choix idéal dans les scénarios où votre charge de travail génère rapidement une grande quantité de données d’historique. Ce modèle est généralement utilisé pour les charges de travail avec traitement transactionnel intensif, qui utilisent des tables temporelles pour l’audit et le suivi des changements, l’analyse des tendances ou l’ingestion des données IoT.

Pour plus d’informations, consultez [Gérer les données d’historique dans les tables temporelles avec une stratégie de rétention](/azure/sql-database/sql-database-temporal-tables-retention-policy).

## <a name="next-steps"></a>Étapes suivantes

- [Tables temporelles](../../relational-databases/tables/temporal-tables.md)
- [Prise en main des tables temporelles avec versions gérées par le système](../../relational-databases/tables/getting-started-with-system-versioned-temporal-tables.md)
- [Vérifications de cohérence système des tables temporelles](../../relational-databases/tables/temporal-table-system-consistency-checks.md)
- [Partitionnement des tables temporelles](../../relational-databases/tables/partitioning-with-temporal-tables.md)
- [Considérations et limitations liées aux tables temporelles](../../relational-databases/tables/temporal-table-considerations-and-limitations.md)
- [Sécurité de la table temporelle](../../relational-databases/tables/temporal-table-security.md)
- [Tables temporelles avec version gérée par le système avec tables à mémoire optimisée](../../relational-databases/tables/system-versioned-temporal-tables-with-memory-optimized-tables.md)
- [Vues et fonctions de métadonnées de table temporelle](../../relational-databases/tables/temporal-table-metadata-views-and-functions.md)