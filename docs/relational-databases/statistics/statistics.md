---
title: Statistiques
description: L'optimiseur de requête utilise des statistiques dans l'optique de créer des plans de requête qui améliorent les performances des requêtes. Découvrez les concepts et les recommandations sur l’utilisation de l’optimisation des requêtes.
ms.custom: ''
ms.date: 1/7/2021
ms.prod: sql
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
dev_langs:
- TSQL
helpviewer_keywords:
- statistical information [SQL Server], query optimization
- query performance [SQL Server], statistics
- query optimization statistics [SQL Server]
- statistical information [SQL Server], database options
- query optimization statistics [SQL Server], about query optimization statistics
- statistical information [SQL Server], guidelines
- statistical information [SQL Server]
- using statistics [SQL Server]
- statistical information [SQL Server], indexes
- index statistics [SQL Server]
- query optimizer [SQL Server], statistics
- statistics [SQL Server]
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 7800e75ee2b35b491053bbcbc2ef3c05e86e1939
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/13/2021
ms.locfileid: "98172781"
---
# <a name="statistics"></a>Statistiques

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  L'optimiseur de requête utilise des statistiques dans l'optique de créer des plans de requête qui améliorent les performances des requêtes. Pour la plupart des requêtes, l'optimiseur de requête génère déjà les statistiques utiles à un plan de requête de haute qualité ; dans certains cas, vous devez créer des statistiques supplémentaires ou modifier la conception des requêtes pour obtenir des résultats optimaux. Cet article traite des concepts de statistiques et fournit des instructions pour vous permettre d'utiliser efficacement les statistiques d'optimisation de requête.  
  
##  <a name="components-and-concepts"></a><a name="DefinitionQOStatistics"></a> Composants et concepts  
### <a name="statistics"></a>Statistiques  
 Les statistiques utilisées dans le cadre de l’optimisation des requêtes sont des objets BLOB (Binary Large Object) qui contiennent des informations statistiques sur la distribution des valeurs dans une ou plusieurs colonnes d’une table ou d’une vue indexée. L'optimiseur de requête utilise ces statistiques pour estimer le nombre de lignes, également appelé *cardinalité*, dans le résultat de la requête. Ces *estimations de cardinalité* permettent à l’optimiseur de requête de créer un plan de requête de haute qualité. Par exemple, selon vos prédicats, l'optimiseur de requête peut utiliser des estimations de cardinalité pour choisir l'opérateur de recherche d'index plutôt que l'opérateur d'analyse d'index, plus vorace en ressources, contribuant ainsi à améliorer les performances des requêtes.  
  
 Chaque objet de statistiques est créé dans une liste constituée d’une ou de plusieurs colonnes de table, et comprend un *histogramme* présentant la distribution des valeurs dans la première colonne. Les objets de statistiques sur plusieurs colonnes stockent également des informations statistiques sur la corrélation des valeurs entre les colonnes. Ces statistiques de corrélation, également appelées *densités*, sont dérivées du nombre de lignes distinctes de valeurs de colonne. 

#### <a name="histogram"></a><a name="histogram"></a> Histogramme  
Un **histogramme** mesure la fréquence des occurrences de chaque valeur distincte dans un jeu de données. L'optimiseur de requête calcule un histogramme sur les valeurs de colonnes de la première colonne clé de l'objet de statistiques, en sélectionnant les valeurs de colonnes au moyen d'un échantillonnage statistique des lignes ou d'une analyse complète de toutes les lignes dans la table ou la vue. Si l'histogramme est créé à partir d'un jeu de lignes échantillonnées, les totaux stockés pour le nombre de lignes et le nombre de valeurs distinctes sont des estimations et ne doivent pas nécessairement être des nombres entiers.

> [!NOTE]
> <a name="frequency"></a> Les histogrammes dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sont créés pour une seule colonne, en l’occurrence, la première colonne du jeu de colonnes clés de l’objet de statistiques.
  
Pour créer l’histogramme, l’optimiseur de requête trie les valeurs de colonnes, calcule le nombre de valeurs qui correspondent à chaque valeur de colonne distincte, puis regroupe les valeurs de colonnes dans 200 étapes d’histogramme contiguës au maximum. Chaque étape de l’histogramme inclut une plage de valeurs de colonnes, suivie d’une valeur de colonne de limite supérieure. La plage comprend toutes les valeurs de colonnes possibles entre des valeurs limites, à l'exception des valeurs limites elles-mêmes. La plus basse des valeurs de colonnes triées est la valeur de limite supérieure pour la première étape d'histogramme.

Plus précisément, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] crée **l’histogramme** à partir du jeu de valeurs de colonnes trié, en trois étapes :

- **Initialisation de l’histogramme** : Dans la première étape, une suite de valeurs situées au début du jeu trié est traitée, et jusqu’à 200 valeurs de *range_high_key*, *equal_rows*, *range_rows* et *distinct_range_rows* sont collectées (*range_rows* et *distinct_range_rows* ont toujours une valeur égale à zéro lors de cette étape). La première étape se termine quand toutes les entrées ont été traitées ou quand 200 valeurs ont été trouvées. 
- **Analyse avec fusion des compartiments** : Chaque valeur supplémentaire issue de la première colonne de la clé de statistiques est traitée selon l’ordre de tri à la deuxième étape. Chaque valeur suivante est ajoutée à la dernière plage ou une nouvelle plage est créée à la fin (ceci est possible, car les valeurs d’entrée sont triées). Si une plage est créée, une paire de plages voisines existantes est réduite en une plage unique. Cette paire de plages est sélectionnée pour minimiser la perte d’informations. Cette méthode utilise un algorithme de *différence maximale* pour réduire le nombre d’étapes dans l’histogramme, tout en augmentant la différence entre les valeurs limites. Le nombre d’étapes après réduction des plages reste à 200 pendant toute la durée de cette étape.
- **Consolidation de l’histogramme** : Dans la troisième étape, d’autres plages peuvent être réduites s’il n’y a pas de perte importante d’informations. Le nombre d'étapes d'histogramme peut être inférieur au nombre de valeurs distinctes, même pour les colonnes comportant moins de 200 points de limite. Par conséquent, même si la colonne contient plus de 200 valeurs uniques, l’histogramme peut comporter moins de 200 étapes. Pour une colonne comprenant uniquement des valeurs uniques, l’histogramme consolidé comporte un minimum de trois étapes.

> [!NOTE]
> Si l’histogramme a été créé à l’aide de l’option SAMPLE plutôt qu’avec l’option FULLSCAN, les valeurs de *equal_rows*, *range_rows*, *distinct_range_rows* et  *average_range_rows* sont estimées, et n’ont donc pas besoin d’être des entiers.

Le diagramme suivant illustre un histogramme avec six étapes : La zone située à gauche de la première valeur limite supérieure représente la première étape.
  
![Histogramme](../../relational-databases/system-dynamic-management-views/media/histogram-2.svg "Histogramme") 
  
Pour chaque étape de l’histogramme ci-dessus :
-   La ligne en gras représente la valeur limite supérieure (*range_high_key*) et le nombre d’occurrences (*equal_rows*) correspondant.  
  
-   La zone pleine située à gauche de *range_high_key* représente la plage de valeurs de colonnes et le nombre moyen d’occurrences de chacune des valeurs de colonnes (*average_range_rows*). Pour la première étape de l’histogramme, la valeur de *average_range_rows* est toujours égale à 0.  
  
-   Les lignes pointillées représentent les valeurs échantillonnées utilisées pour estimer le nombre total de valeurs distinctes dans la plage (*distinct_range_rows*) et le nombre total de valeurs dans la plage (*range_rows*). L’optimiseur de requête utilise *range_rows* et *distinct_range_rows* pour calculer *average_range_rows*, et ne stocke pas les valeurs échantillonnées.   
  
#### <a name="density-vector"></a><a name="density"></a> Vecteur de densité  
La **densité** correspond aux informations sur le nombre de doublons d’une colonne donnée ou d’une combinaison de colonnes. Elle est calculée ainsi : 1/(nombre de valeurs distinctes). L'optimiseur de requête utilise des densités afin d'améliorer les estimations de cardinalité pour les requêtes qui retournent plusieurs colonnes à partir de la même table ou vue indexée. Lorsque la densité diminue, la sélectivité d’une valeur augmente. Par exemple, dans une table représentant des voitures, plusieurs voitures proviennent du même constructeur mais chacune a un numéro d'identification unique. Un index sur le numéro d'identification du véhicule est plus sélectif qu'un index sur le constructeur, car le numéro d'identification du véhicule a une plus faible densité que le constructeur. 

> [!NOTE]
> La fréquence correspond aux informations sur l’occurrence de chaque valeur distincte dans la première colonne de clé de l’objet de statistiques. Elle est calculée ainsi : nombre de lignes x densité. Les colonnes qui comportent des valeurs uniques ont une fréquence maximale de 1.

Le vecteur de densité contient une densité pour chaque préfixe des colonnes dans l'objet de statistiques. Par exemple, si un objet de statistiques contient les colonnes clés `CustomerId`, `ItemId` et `Price`, la densité est calculée à partir des préfixes de colonnes suivants :
  
|Préfixe de colonne|Densité calculée sur|  
|---|---|
|(CustomerId)|Lignes avec des valeurs correspondantes pour CustomerID|  
|(CustomerId, ItemId)|Lignes avec des valeurs correspondantes pour CustomerId et ItemId|  
|(CustomerId, ItemId, Price)|Lignes avec des valeurs correspondantes pour CustomerId, ItemId et Price| 

### <a name="filtered-statistics"></a>Statistiques filtrées  
 Les statistiques filtrées peuvent améliorer les performances des requêtes qui effectuent des sélections dans des sous-ensembles bien définis de données. Les statistiques filtrées utilisent un prédicat de filtre pour sélectionner le sous-ensemble de données qui est inclus dans les statistiques. Les statistiques filtrées correctement conçues peuvent améliorer le plan d'exécution de requête par rapport aux statistiques de table complète. Pour plus d’informations sur le prédicat de filtre, consultez [CREATE STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/create-statistics-transact-sql.md). Pour plus d’informations sur l’opportunité de créer des statistiques filtrées, consultez la section [Quand créer des statistiques](#CreateStatistics) dans cet article.  
 
### <a name="statistics-options"></a>Options de statisques  
 Il existe trois options qui affectent quand et comment les statistiques sont créées et mises à jour. Ces options sont configurables au niveau de la base de données uniquement.  
  
#### <a name="auto_create_statistics-option"></a><a name="AutoUpdateStats"></a>Option AUTO_CREATE_STATISTICS  
 Quand l’option de création automatique de statistiques [AUTO_CREATE_STATISTICS](../../t-sql/statements/alter-database-transact-sql-set-options.md#auto_create_statistics) est activée, l’optimiseur de requête crée les statistiques nécessaires sur les colonnes individuelles du prédicat de requête pour améliorer les estimations de cardinalité pour le plan de requête. Ces statistiques propres à une colonne sont créées sur les colonnes où ne figure pas déjà un [histogramme](#histogram) dans un objet de statistiques existant. L'option AUTO_CREATE_STATISTICS ne détermine pas si les statistiques sont créées pour des index. De même, cette option ne génère pas de statistiques filtrées. Elle s'applique exclusivement aux statistiques de colonne unique pour la table entière.  
  
 Lorsque l'optimiseur de requête crée des statistiques suite à l'utilisation de l'option AUTO_CREATE_STATISTICS, le nom des statistiques commence par `_WA`. Vous pouvez utiliser la requête suivante pour déterminer si l'optimiseur de requête a créé des statistiques pour une colonne de prédicat de requête.  
  
```sql  
SELECT OBJECT_NAME(s.object_id) AS object_name,  
    COL_NAME(sc.object_id, sc.column_id) AS column_name,  
    s.name AS statistics_name  
FROM sys.stats AS s 
INNER JOIN sys.stats_columns AS sc  
    ON s.stats_id = sc.stats_id AND s.object_id = sc.object_id  
WHERE s.name like '_WA%'  
ORDER BY s.name;  
```  
  
#### <a name="auto_update_statistics-option"></a>Option AUTO_UPDATE_STATISTICS  
 Quand l’option de mise à jour automatique des statistiques [AUTO_UPDATE_STATISTICS](../../t-sql/statements/alter-database-transact-sql-set-options.md#auto_update_statistics) est activée, l’optimiseur de requête détermine si les statistiques sont obsolètes, puis les met à jour le cas échéant quand elles sont utilisées par une requête. Les statistiques deviennent obsolètes après que des opérations d'insertion, de mise à jour, de suppression ou de fusion ont modifié la distribution des données dans la table ou la vue indexée. L'optimiseur de requête détermine si les statistiques sont obsolètes en comptant le nombre de modifications de données depuis la dernière mise à jour des statistiques et en comparant le nombre de modifications à un seuil. Ce seuil est basé sur le nombre de lignes contenues dans la table ou la vue indexée.  
  
* Jusqu’à [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)], [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilise un seuil reposant sur le pourcentage de lignes modifiées. Cette valeur est indépendante du nombre de lignes figurant dans la table. Le seuil est :
    * Si la cardinalité de la table affichait une valeur de 500 ou moins au moment de l’évaluation des statistiques, une mise à jour est effectuée toutes les 500 modifications.
    * Si la cardinalité de la table affichait une valeur supérieure à 500 au moment de l’évaluation des statistiques, une mise à jour est effectuée toutes les 500 modifications + 20 %.

* À compter de [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] et avec un [niveau de compatibilité de base de données](../../relational-databases/databases/view-or-change-the-compatibility-level-of-a-database.md) de 130, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilise un seuil dynamique décroissant de mise à jour des statistiques qui s’ajuste en fonction du nombre de lignes contenues de la table. Il est obtenu en calculant la racine carrée du produit de 1 000 et de la cardinalité de la table actuelle. Par exemple, si votre table contient 2 millions de lignes, le calcul est le suivant : sqrt(1000 * 2000000) = 44721.359. Du fait de cette modification, les statistiques sur des tables volumineuses sont mises à jour plus fréquemment. Toutefois, si une base de données affiche un niveau de compatibilité inférieur à 130, le seuil [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] s’applique. 

> [!IMPORTANT]
> Dans [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] à [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)], ou dans [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] et versions ultérieures sous le [niveau de compatibilité de la base de données](../../relational-databases/databases/view-or-change-the-compatibility-level-of-a-database.md) 120 et inférieur, activez [trace flag 2371](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) pour que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilise un seuil de mise à jour des statistiques décroissant et dynamique.

Vous pouvez utiliser les conseils suivants pour activer l’indicateur de trace 2371 dans votre environnement pré-[!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] :

 - Si vous n’avez pas constaté de problèmes de performances dus à des statistiques obsolètes, il n’est pas nécessaire d’activer cet indicateur de trace.
 - Si vous utilisez des systèmes SAP, activez cet indicateur de trace.  Pour plus d’informations, reportez-vous à ce [blog](/archive/blogs/saponsqlserver/changes-to-automatic-update-statistics-in-sql-server-traceflag-2371).
 - Si vous exécutez des tâches nocturnes pour mettre à jour les statistiques car la mise à jour automatique actuelle n’est pas déclenchée assez fréquemment, vous pouvez activer l’indicateur de trace 2371 pour réduire le seuil.
  
L'optimiseur de requête vérifie s'il existe des statistiques obsolètes avant de compiler une requête et avant d'exécuter un plan de requête mis en cache. Avant de compiler une requête, l'optimiseur de requête utilise les colonnes, les tables et les vues indexées du prédicat de requête pour identifier les statistiques susceptibles d'être obsolètes. Avant d'exécuter un plan de requête mis en cache, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] vérifie que le plan de requête fait référence à des statistiques à jour.  
  
L’option AUTO_UPDATE_STATISTICS s’applique aux objets de statistiques créés pour les index, aux colonnes uniques contenues dans les prédicats de requête et aux statistiques créées à l’aide de l’instruction [CREATE STATISTICS](../../t-sql/statements/create-statistics-transact-sql.md) . Cette option s'applique également aux statistiques filtrées.  
 
Vous pouvez utiliser le paramètre [sys.dm_db_stats_properties](../../relational-databases/system-dynamic-management-views/sys-dm-db-stats-properties-transact-sql.md) pour suivre avec précision le nombre de lignes modifiées dans une table et déterminer si vous souhaitez mettre à jour les statistiques manuellement.



  
#### <a name="auto_update_statistics_async"></a>AUTO_UPDATE_STATISTICS_ASYNC  
L’option de mise à jour asynchrone des statistiques [AUTO_UPDATE_STATISTICS_ASYNC](../../t-sql/statements/alter-database-transact-sql-set-options.md#auto_update_statistics_async) détermine si l’optimiseur de requête utilise des mises à jour des statistiques synchrones ou asynchrones. Par défaut, l’option de mise à jour asynchrone des statistiques est désactivée, et l’optimiseur de requête met à jour les statistiques de façon synchrone. L’option AUTO_UPDATE_STATISTICS_ASYNC s’applique aux objets de statistiques créés pour les index, aux colonnes uniques contenues dans les prédicats de requête et aux statistiques créées à l’aide de l’instruction [CREATE STATISTICS](../../t-sql/statements/create-statistics-transact-sql.md) .  
 
> [!NOTE]
> Pour définir une option de mise à jour des statistiques de manière asynchrone dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], sur la page *Options* de la fenêtre *Propriétés de la base de données*, les deux options *Mise à jour automatique des statistiques* et *Mise à jour automatique des statistiques de manière asynchrone* doivent être définies sur **True**.
  
La mise à jour des statistiques peut être synchrone (par défaut) ou asynchrone. 

* Avec les mises à jour synchrones des statistiques, les requêtes sont toujours compilées et exécutées avec des statistiques à jour. Lorsque les statistiques sont obsolètes, l’optimiseur de requête attend les statistiques mises à jour avant de compiler et d’exécuter la requête. 

* Avec les mises à jour asynchrones des statistiques, les requêtes sont compilées avec les statistiques existantes, même si celles-ci sont obsolètes. L’optimiseur de requête peut choisir un plan de requête non optimal si les statistiques sont obsolètes lors de la compilation de la requête. Les statistiques sont généralement mises à jour peu de temps après. Les requêtes compilées à l’issue des mises à jour des statistiques ont l’avantage d’utiliser les statistiques mises à jour, comme d’habitude.   

Envisagez d'utiliser des statistiques synchrones lorsque vous effectuez des opérations qui modifient la distribution des données, telles que la troncation d'une table ou une mise à jour en bloc d'un fort pourcentage de lignes. Si vous ne mettez pas à jour manuellement les statistiques à l’issue de l’opération, l’utilisation de statistiques synchrones vous garantit que les statistiques sont à jour avant d’exécuter des requêtes sur les données modifiées.  
  
Envisagez d'utiliser des statistiques asynchrones pour obtenir des temps de réponse des requêtes plus prévisibles pour les scénarios suivants :  
  
* Votre application exécute régulièrement la même requête, des requêtes similaires ou des plans de requête mis en cache similaires. Il se peut que les temps de réponse de vos requêtes soient plus prévisibles avec des mises à jour de statistiques asynchrones qu'avec des mises à jour de statistiques synchrones, car l'optimiseur de requête peut exécuter les requêtes entrantes sans attendre la mise à jour des statistiques. Cela évite de retarder certaines requêtes et pas les autres.  
  
* Votre application a connu des expirations de délai de demandes clientes causées par une ou plusieurs requêtes en attente de statistiques mises à jour. Dans certains cas, l'attente de statistiques synchrones peut entraîner l'échec des applications dont les délais d'expiration sont agressifs.  

> [!NOTE]
> Les statistiques sur les tables temporaires locales sont toujours mises à jour de façon synchrone, quelle que soit l’option AUTO_UPDATE_STATISTICS_ASYNC. Les statistiques sur les tables temporaires globales sont mises à jour de façon synchrone ou asynchrone en fonction de l’option AUTO_UPDATE_STATISTICS_ASYNC définie pour la base de données utilisateur.

La mise à jour asynchrone des statistiques est effectuée par une requête en arrière-plan. Lorsque la requête est prête à écrire des statistiques mises à jour dans la base de données, elle tente d’acquérir un verrou de modification de schéma sur l’objet de métadonnées des statistiques. Si une autre session détient déjà un verrou sur le même objet, la mise à jour asynchrone des statistiques est bloquée jusqu’à ce que le verrou de modification de schéma puisse être acquis. De même, les sessions qui doivent acquérir un verrou de stabilité de schéma sur l’objet de métadonnées des statistiques pour compiler une requête peuvent être bloquées par la session d’arrière-plan de mise à jour asynchrone des statistiques, qui détient déjà ou attend l’acquisition du verrou de modification de schéma. Par conséquent, pour les charges de travail avec des compilations de requêtes très fréquentes et des mises à jour fréquentes de statistiques, l’utilisation de statistiques asynchrones peut augmenter la probabilité de problèmes d’accès concurrentiel dus à un blocage des verrous.

Dans Azure SQL Database et Azure SQL Managed Instance, vous pouvez éviter les éventuels problèmes d’accès concurrentiel à l’aide de la mise à jour asynchrone des statistiques si vous activez la [configuration à l’échelle de la base de données](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md) ASYNC_STATS_UPDATE_WAIT_AT_LOW_PRIORITY. Lorsque cette configuration est activée, la requête en arrière-plan attend d’acquérir le verrou de modification de schéma (Sch-M) sur une file d’attente basse priorité distincte, ce qui permet à d’autres requêtes de continuer à compiler des requêtes avec des statistiques existantes. Une fois qu’aucune autre session ne détient un verrou sur l’objet de métadonnées des statistiques, la requête en arrière-plan acquiert le verrou de modification de schéma et met à jour les statistiques. Dans le cas improbable où la requête en arrière-plan ne peut pas acquérir le verrou dans un délai de plusieurs minutes, la mise à jour des statistiques asynchrones est abandonnée et les statistiques ne sont pas mises à jour tant qu’une autre mise à jour automatique des statistiques n’est pas déclenchée, ou jusqu’à ce que les statistiques soient [mises à jour manuellement](update-statistics.md).

> [!Note]
> L’option de configuration délimitée aux bases de données ASYNC_STATS_UPDATE_WAIT_AT_LOW_PRIORITY est maintenant disponible dans Azure SQL Database et Azure SQL Managed Instance. Il est prévu qu’elle le soit dans SQL Server vNext. 

#### <a name="incremental"></a>INCREMENTAL  
 Quand l’option INCREMENTAL de CREATE STATISTICS est définie sur ON, les statistiques sont créées pour chaque partition. Si la valeur est OFF, l’arborescence des statistiques est supprimée et [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] recalcule les statistiques. La valeur par défaut est OFF. Ce paramètre remplace la propriété INCREMENTAL de niveau base de données. Pour plus d’informations sur la création de statistiques incrémentielles, consultez [CREATE STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/create-statistics-transact-sql.md). Pour plus d’informations sur la création automatique de statistiques par partition, consultez [Propriétés de la base de données &#40;page Options&#41;](../../relational-databases/databases/database-properties-options-page.md#automatic) et [Options ALTER DATABASE SET &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md). 
  
 Lorsque de nouvelles partitions sont ajoutées à une table volumineuse, les statistiques doivent être mises à jour afin d'inclure les nouvelles partitions. Cependant, la durée nécessaire pour analyser la table entière (option FULLSCAN ou SAMPLE) peut être assez longue. En outre, l'analyse la table entière n'est pas nécessaire car seules les statistiques sur les nouvelles partitions peuvent être requises. L'option incrémentielle crée et stocke des statistiques par partition, et une fois la mise à jour terminée, seules sont actualisées les statistiques sur les partitions qui ont besoin de nouvelles statistiques  
  
 Si les statistiques par partition ne sont pas prises en charge, l'option est ignorée et un avertissement est généré. Les statistiques incrémentielles ne sont pas prises en charge pour les types de statistiques suivants :  
  
* statistiques créées avec des index qui ne sont pas alignés sur les partitions avec la table de base ;  
* statistiques créées sur les bases de données secondaires lisibles Always On ;  
* statistiques créées sur les bases de données en lecture seule ;  
* statistiques créées sur les index filtrés ;  
* statistiques créées sur les vues ;  
* statistiques créées sur les tables internes ;  
* Statistiques créées avec les index spatiaux ou les index XML.  
  
**S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions ultérieures. 
  
## <a name="when-to-create-statistics"></a><a name="CreateStatistics"></a> Quand créer des statistiques  
 L'optimiseur de requête crée déjà des statistiques selon les méthodes suivantes :  
  
1.  L'optimiseur de requête crée des statistiques pour les index de tables ou de vues lors de la création des index. Ces statistiques sont créées sur les colonnes de clés de l'index. Si l'index est un index filtré, l'optimiseur de requête crée des statistiques filtrées sur le même sous-ensemble de lignes spécifié pour l'index filtré. Pour plus d’informations sur les index filtrés, consultez [Créer des index filtrés](../../relational-databases/indexes/create-filtered-indexes.md) et [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md).  

    > [!NOTE]
    > À partir de [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)], les statistiques ne sont pas créées en analysant toutes les lignes de la table quand un index partitionné est créé ou reconstruit. Au lieu de cela, l’optimiseur de requête utilise l’algorithme d’échantillonnage par défaut pour générer des statistiques. Après la mise à niveau d'une base de données avec des index partitionnés, vous pouvez remarquer une différence dans les données d'histogramme pour ces index. Cette modification du comportement peut ne pas affecter les performances des requêtes. Pour obtenir des statistiques sur les index partitionnés en analysant toutes les lignes de la table, utilisez `CREATE STATISTICS` ou `UPDATE STATISTICS` avec la clause `FULLSCAN`. 
  
2.  L’optimiseur de requête crée des statistiques pour les colonnes individuelles des prédicats de requête quand l’option [AUTO_CREATE_STATISTICS](../../t-sql/statements/alter-database-transact-sql-set-options.md#auto_create_statistics) est activée.  

Pour la plupart des requêtes, ces deux méthodes de création de statistiques sont l’assurance de disposer d’un plan de requête de haute qualité. Dans certains cas, vous pouvez améliorer les plans de requête en créant des statistiques supplémentaires à l’aide de l’instruction [CREATE STATISTICS](../../t-sql/statements/create-statistics-transact-sql.md) . Ces statistiques supplémentaires peuvent capturer des corrélations statistiques dont l'optimiseur de requête ne tient pas compte lorsqu'il crée des statistiques pour des index ou des colonnes uniques. Il se peut que votre application présente des corrélations statistiques supplémentaires dans les données de table qui, si elles sont calculées dans un objet de statistiques, peuvent permettre à l'optimiseur de requête d'améliorer les plans de requête. Par exemple, les statistiques filtrées sur un sous-ensemble de lignes de données ou les statistiques multicolonnes sur des colonnes de prédicat de requête sont susceptibles d'améliorer le plan de requête.  
  
Dans le cadre de la création de statistiques à l'aide de l'instruction CREATE STATISTICS, nous vous recommandons de maintenir l'option AUTO_CREATE_STATISTICS activée de sorte que l'optimiseur de requête continue de créer de manière régulière des statistiques de colonne unique pour les colonnes de prédicat de requête. Pour plus d’informations sur les prédicats de requête, consultez [Condition de recherche &#40;Transact-SQL&#41;](../../t-sql/queries/search-condition-transact-sql.md).  
  
Envisagez de créer des statistiques avec l'instruction CREATE STATISTICS dans l'un des cas suivants :  

* l'Assistant Paramétrage du [!INCLUDE[ssDE](../../includes/ssde-md.md)] suggère de créer des statistiques ; 
* le prédicat de requête contient plusieurs colonnes corrélées qui ne figurent pas déjà dans le même index ;  
* la requête effectue une sélection dans un sous-ensemble de données ;  
* il manque des statistiques pour la requête.  
  
### <a name="query-predicate-contains-multiple-correlated-columns"></a>Le prédicat de requête contient plusieurs colonnes corrélées  
Lorsqu'un prédicat de requête contient plusieurs colonnes ayant entre elles des relations et des dépendances, des statistiques sur les différentes colonnes peuvent améliorer le plan de requête. Les statistiques sur plusieurs colonnes contiennent des statistiques de corrélation entre les colonnes appelées *densités*, qui ne sont pas disponibles dans les statistiques de colonne unique. Les densités peuvent améliorer les estimations de cardinalité lorsque les résultats de requête dépendent des relations de données entre plusieurs colonnes.  
  
Si les colonnes figurent déjà dans le même index, l'objet de statistiques multicolonnes existe déjà et il n'est pas nécessaire de le créer manuellement. Si les colonnes ne figurent pas déjà dans le même index, vous pouvez créer des statistiques multicolonnes en créant un index sur les colonnes ou en utilisant l’instruction [CREATE STATISTICS](../../t-sql/statements/create-statistics-transact-sql.md). Un index exige davantage de ressources système qu'un objet de statistiques. Ainsi, si l'application n'a pas besoin de l'index multicolonne, vous pouvez économiser des ressources système en créant l'objet de statistiques sans créer l'index.  
  
Lors de la création de statistiques multicolonnes, l'ordre des colonnes dans la définition de l'objet de statistiques a une incidence sur l'efficacité des densités lorsqu'il s'agit de réaliser des estimations de cardinalité. L'objet de statistiques stocke des densités pour chaque préfixe de colonnes de clés contenu dans la définition de l'objet de statistiques. Pour plus d’informations sur les densités, consultez la section [Densité](#density) dans cette page.  
  
Pour créer des densités utiles aux estimations de cardinalité, les colonnes du prédicat de requête doivent correspondre à l'un des préfixes de colonnes contenus dans la définition de l'objet de statistiques. L’exemple suivant crée un objet de statistiques multicolonnes sur les colonnes `LastName`, `MiddleName` et `FirstName`.  
  
```sql  
USE AdventureWorks2012;  
GO  
IF EXISTS (SELECT name FROM sys.stats  
    WHERE name = 'LastFirst'  
    AND object_ID = OBJECT_ID ('Person.Person'))  
DROP STATISTICS Person.Person.LastFirst;  
GO  
CREATE STATISTICS LastFirst ON Person.Person (LastName, MiddleName, FirstName);  
GO  
```  
  
Dans cet exemple, l'objet de statistiques `LastFirst` comprend des densités pour les préfixes de colonne suivants: `(LastName)`, `(LastName, MiddleName)` et `(LastName, MiddleName, FirstName)`. La densité n'est pas disponible pour `(LastName, FirstName)`. Si la requête utilise `LastName` et `FirstName` sans utiliser `MiddleName`, la densité n'est pas disponible pour les estimations de cardinalité.  
  
### <a name="query-selects-from-a-subset-of-data"></a>La requête effectue une sélection dans un sous-ensemble de données  
Lorsque l'optimiseur de requête crée des statistiques pour des colonnes et des index uniques, il crée les statistiques pour les valeurs contenues dans toutes les lignes. Lorsque les requêtes effectuent une sélection dans un sous-ensemble de lignes et que ce sous-ensemble de lignes présente une distribution de données unique, des statistiques filtrées peuvent améliorer les plans de requête. Vous pouvez créer des statistiques filtrées en utilisant l’instruction [CREATE STATISTICS](../../t-sql/statements/create-statistics-transact-sql.md) avec la clause [WHERE](../../t-sql/queries/where-transact-sql.md) pour définir l’expression de prédicat du filtre.  
  
Par exemple, avec [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)], chaque produit de la table `Production.Product` appartient à une des quatre catégories de la table `Production.ProductCategory` : Bikes, Components, Clothing et Accessories. Chacune de ces catégories présente une distribution de données différente pour le poids (Weight) : le poids des bicyclettes (Bikes) varie de 13,77 à 30,0, le poids des composants (Components) varie de 2,12 à 1 050,00 avec quelques valeurs NULL, le poids des vêtements (Clothing) est toujours NULL et le poids des accessoires (Accessories) est également toujours NULL.  
  
Si l'on prend la catégorie Bikes pour exemple, les statistiques filtrées sur tous les poids de bicyclettes fournissent des statistiques plus précises à l'optimisateur de requête et peuvent améliorer la qualité du plan de requête par rapport aux statistiques de table entière ou aux statistiques inexistantes de la colonne Weight (Poids). Si la colonne où figure le poids des bicyclettes constitue un candidat valable pour les statistiques filtrées, tel n'est pas forcément le cas pour un index filtré si le nombre de recherches de poids est relativement faible. Les gains en performances offerts par un index filtré lors des recherches risquent de ne pas compenser les coûts de maintenance et de stockage supplémentaires liés à l'ajout d'un index filtré à la base de données.  
  
L'instruction suivante crée les statistiques filtrées `BikeWeights` sur toutes les sous-catégories de bicyclettes (Bikes). L'expression de prédicat filtré définit les bicyclettes en énumérant toutes les sous-catégories de bicyclettes à l'aide de la comparaison `Production.ProductSubcategoryID IN (1,2,3)`. Le prédicat ne peut pas utiliser le nom de catégorie Bikes car il est stocké dans la table Production.ProductCategory, et toutes les colonnes spécifiées dans l'expression de filtre doivent se trouver dans la même table.  
  
[!code-sql[StatisticsDDL#FilteredStats2](../../relational-databases/statistics/codesnippet/tsql/statistics_1.sql)]  
  
L'optimiseur de requête peut utiliser les statistiques filtrées `BikeWeights` pour améliorer le plan de requête de la requête suivante qui sélectionne toutes les bicyclettes dont le poids est supérieur à `25`.  
  
```sql  
SELECT P.Weight AS Weight, S.Name AS BikeName  
FROM Production.Product AS P  
    JOIN Production.ProductSubcategory AS S   
    ON P.ProductSubcategoryID = S.ProductSubcategoryID  
WHERE P.ProductSubcategoryID IN (1,2,3) AND P.Weight > 25  
ORDER BY P.Weight;  
GO  
```  
  
### <a name="query-identifies-missing-statistics"></a>La requête identifie les statistiques manquantes  
Si une erreur ou tout autre événement empêche l'optimiseur de requête de créer des statistiques, il crée le plan de requête sans utiliser de statistiques. L'optimiseur de requête indique que les statistiques sont manquantes et tente de les régénérer à la prochaine exécution de la requête.  
  
Les statistiques manquantes sont indiquées comme des avertissements (nom de la table en rouge) lorsque le plan d'exécution d'une requête est affiché sous forme graphique à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. D'autre part, le **permet de surveiller la classe d'événements** Missing Column Statistics [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] et d'être tenu informé lorsque des statistiques manquent. Pour plus d’informations, consultez [Catégorie d’événement Erreurs et avertissements &#40;moteur de base de données&#41;](../../relational-databases/event-classes/errors-and-warnings-event-category-database-engine.md).  
  
 S'il manque des statistiques, procédez comme suit :  
  
* Vérifiez que les options [AUTO_CREATE_STATISTICS](../../t-sql/statements/alter-database-transact-sql-set-options.md#auto_create_statistics) et [AUTO_UPDATE_STATISTICS](../../t-sql/statements/alter-database-transact-sql-set-options.md#auto_update_statistics) sont activées.  
* vérifiez que la base de données n'est pas en lecture seule. Si la base de données est en lecture seule, un nouvel objet de statistiques ne peut pas être enregistré.  
* Créez les statistiques manquantes en utilisant l’instruction [CREATE STATISTICS](../../t-sql/statements/create-statistics-transact-sql.md).  
  
Lorsque les statistiques sur une base de données en lecture seule ou un instantané en lecture seule sont absentes ou obsolètes, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] crée et gère les statistiques temporaires dans **tempdb**. Lorsque le [!INCLUDE[ssDE](../../includes/ssde-md.md)] crée des statistiques temporaires, le nom des statistiques est ajouté avec le suffixe *_readonly_database_statistic* pour différencier les statistiques temporaires des statistiques permanentes. Le suffixe *_readonly_database_statistic* est réservé aux statistiques générées par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Des scripts pour les statistiques temporaires peuvent être créés et reproduits sur une base de données en lecture-écriture. Le cas échéant, [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] remplace le suffixe du nom des statistiques *_readonly_database_statistic* par *_readonly_database_statistic_scripted*.  
  
Seul [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut créer et mettre à jour les statistiques temporaires. Toutefois, vous pouvez supprimer des statistiques temporaires et analyser les propriétés des statistiques en utilisant les mêmes outils que ceux que vous utilisez pour les statistiques permanentes :  
  
* Supprimez les statistiques temporaires en utilisant l’instruction [DROP STATISTICS](../../t-sql/statements/drop-statistics-transact-sql.md).  
* Surveillez les statistiques en utilisant les vues du catalogue **[sys.stats](../../relational-databases/system-catalog-views/sys-stats-transact-sql.md)** et **[sys.stats_columns](../../relational-databases/system-catalog-views/sys-stats-columns-transact-sql.md)** . **sys_stats** inclut la colonne **is_temporary** pour indiquer les statistiques permanentes et temporaires.  
  
 Étant donné que les statistiques temporaires sont stockées dans **tempdb**, un redémarrage du service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] provoque la disparition de toutes les statistiques temporaires.  
    
## <a name="when-to-update-statistics"></a><a name="UpdateStatistics"></a> Quand mettre à jour des statistiques  
 L'optimiseur de requête détermine si les statistiques sont obsolètes et les met éventuellement à jour lorsqu'elles sont requises par un plan de requête. Dans certains cas, vous pouvez améliorer le plan de requête et donc les performances des requêtes en mettant à jour les statistiques plus fréquemment que quand l’option [AUTO_UPDATE_STATISTICS](../../t-sql/statements/alter-database-transact-sql-set-options.md#auto_update_statistics) est activée. Vous pouvez mettre à jour les statistiques à l'aide de l'instruction UPDATE STATISTICS ou de la procédure stockée sp_updatestats.  
  
 La mise à jour des statistiques est l'assurance que les requêtes sont compilées avec des statistiques à jour. Toutefois, la mise à jour des statistiques entraîne une recompilation des requêtes. À ce titre, il est déconseillé de mettre à jour les statistiques de façon trop régulière eu égard aux performances. Un compromis doit être trouvé entre le souhait d'améliorer les plans de requête et le temps nécessaire à la recompilation des requêtes. Ce compromis peut varier en fonction de votre application.  
  
 Dans le cadre d'une mise à jour des statistiques avec UPDATE STATISTICS ou sp_updatestats, nous vous recommandons de maintenir l'option AUTO_UPDATE_STATISTICS activée (valeur ON) de sorte que l'optimiseur de requête continue de mettre à jour les statistiques de façon régulière. Pour plus d’informations sur la façon de mettre à jour les statistiques sur une colonne, un index, une table ou une vue indexée, consultez [UPDATE STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/update-statistics-transact-sql.md). Pour plus d’informations sur la mise à jour des statistiques pour toutes les tables définies par l’utilisateur et les tables internes de la base de données, consultez la procédure stockée [sp_updatestats &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-updatestats-transact-sql.md).  
  
 Pour déterminer la date de la dernière mise à jour des statistiques, utilisez la fonction [sys.dm_db_stats_properties](../../relational-databases/system-dynamic-management-views/sys-dm-db-stats-properties-transact-sql.md) ou [STATS_DATE](../../t-sql/functions/stats-date-transact-sql.md).  
  
 Envisagez de mettre à jour les statistiques dans les cas suivants :  
  
* lenteur d'exécution des requêtes ;  
* opérations d'insertion appliquées à des colonnes de clés triées par ordre croissant ou décroissant ;  
* opérations de maintenance fraîchement effectuées.  

### <a name="query-execution-times-are-slow"></a>Lenteur d'exécution des requêtes  
 Si les temps de réponse des requêtes sont longs ou imprévisibles, assurez-vous que les requêtes disposent de statistiques à jour avant d'exécuter d'autres procédures de dépannage.  
  
### <a name="insert-operations-occur-on-ascending-or-descending-key-columns"></a>Opérations d'insertion appliquées à des colonnes de clés triées par ordre croissant ou décroissant  
 Les statistiques sur les colonnes de clés triées par ordre croissant ou décroissant, telles que les colonnes IDENTITY ou les colonnes de type timestamp en temps réel, peuvent nécessiter des mises à jour plus régulières que celles effectuées par l'optimiseur de requête. Les opérations d'insertion ajoutent de nouvelles valeurs aux colonnes triées par ordre croissant ou décroissant. Le nombre de lignes ajoutées peut s'avérer trop faible pour déclencher une mise à jour des statistiques. Si les statistiques ne sont pas à jour et que les requêtes sélectionnent des lignes récemment ajoutées, les statistiques actuelles ne comporteront pas d'estimations de cardinalité pour ces nouvelles valeurs. Cela peut se traduire par des estimations de cardinalité imprécises et des performances de requêtes en retrait.  
  
 Par exemple, une requête qui sélectionne des dates de commande parmi les plus récentes présentera des estimations de cardinalité imprécises si les statistiques ne sont pas mises à jour pour inclure les estimations de cardinalité des dates de commande les plus récentes.  
  
### <a name="after-maintenance-operations"></a>Opérations de maintenance fraîchement effectuées  
 Envisagez de mettre à jour les statistiques après avoir exécuté des procédures de maintenance qui modifient la distribution des données, telles que la troncation d'une table ou l'exécution d'une insertion en bloc d'un fort pourcentage de lignes. Vous éviterez ainsi des retards dans le traitement ultérieur des requêtes du fait de l'attente des mises à jour automatiques des statistiques.  
  
 Les opérations de reconstruction, de défragmentation ou de réorganisation d'index ne modifient pas la distribution des données. Par conséquent, vous n’avez pas besoin de mettre à jour les statistiques après avoir effectué des opérations [ALTER INDEX REBUILD](../../t-sql/statements/alter-index-transact-sql.md#rebuilding-indexes), [DBCC DBREINDEX](../../t-sql/database-console-commands/dbcc-dbreindex-transact-sql.md), [DBCC INDEXDEFRAG](../../t-sql/database-console-commands/dbcc-indexdefrag-transact-sql.md) ou [ALTER INDEX REORGANIZE](../../t-sql/statements/alter-index-transact-sql.md#reorganizing-indexes). Si l'optimiseur de requête met effectivement à jour les statistiques lors de la reconstruction d'un index sur une table ou une vue via ALTER INDEX REBUILD ou DBCC DBREINDEX, cette mise à jour des statistiques est une conséquence de la recréation de l'index. L'optimiseur de requête ne met pas à jour les statistiques suite à des opérations DBCC INDEXDEFRAG ou ALTER INDEX REORGANIZE. 
 
> [!TIP]
> À partir de [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] SP1 CU4, utilisez l’option PERSIST_SAMPLE_PERCENT de [CREATE STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/create-statistics-transact-sql.md) ou [UPDATE STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/update-statistics-transact-sql.md) pour définir et conserver un pourcentage d’échantillonnage spécifique pour les mises à jour des statistiques suivantes qui ne définissent pas explicitement un pourcentage d’échantillonnage.

### <a name="automatic-index-and-statistics-management"></a>Gestion automatique des index et des statistiques

Tirez parti de solutions comme [Adaptive Index Defrag](https://github.com/Microsoft/tigertoolbox/tree/master/AdaptiveIndexDefrag) pour gérer automatiquement la défragmentation des index et les mises à jour des statistiques pour une ou plusieurs bases de données. Cette procédure choisit automatiquement s’il faut reconstruire ou réorganiser un index en fonction de son niveau de fragmentation, entre autres, et mettre à jour les statistiques avec un seuil linéaire.
  
##  <a name="queries-that-use-statistics-effectively"></a><a name="DesignStatistics"></a> Requêtes pour une utilisation efficace des statistiques  
 Certaines implémentations de requête, telles que les variables locales et les expressions complexes contenues dans le prédicat de requête, peuvent produire des plans de requête non optimaux. Cela peut s'éviter en suivant les recommandations en matière de conception de requêtes pour une utilisation efficace des statistiques. Pour plus d’informations sur les prédicats de requête, consultez [Condition de recherche &#40;Transact-SQL&#41;](../../t-sql/queries/search-condition-transact-sql.md).  
  
 Vous pouvez améliorer les plans de requête en appliquant les recommandations en matière de conception de requêtes pour une utilisation efficace des statistiques. Les *estimations de cardinalité* relatives aux expressions, aux variables et aux fonctions utilisées dans les prédicats de requête s'en trouveront améliorées. Lorsque l'optimiseur de requête ne connaît pas la valeur d'une expression, d'une variable ou d'une fonction, il ne sait pas quelle valeur rechercher dans l'histogramme et ne peut donc pas extraire la meilleure estimation de cardinalité de l'histogramme. Au lieu de cela, l'optimiseur de requête base l'estimation de cardinalité sur le nombre moyen de lignes par valeur distincte pour toutes les lignes échantillonnées dans l'histogramme. Il en résulte des estimations de cardinalité non optimales et une altération potentielle des performances des requêtes. Pour plus d’informations sur les histogrammes, consultez la section [histogramme](#histogram) de cette page ou [sys.dm_db_stats_histogram](../../relational-databases/system-dynamic-management-views/sys-dm-db-stats-histogram-transact-sql.md).
  
 Les recommandations suivantes indiquent comment écrire les requêtes pour améliorer les plans de requête à travers l'amélioration des estimations de cardinalité.  
  
### <a name="improving-cardinality-estimates-for-expressions"></a>Amélioration des estimations de cardinalité pour les expressions  
Pour améliorer les estimations de cardinalité pour les expressions, respectez les principes suivants :  
  
* Dans la mesure du possible, simplifiez les expressions en y intégrant des constantes. L'optimiseur de requête n'évalue pas toutes les fonctions et les expressions contenant des constantes avant de déterminer les estimations de cardinalité. Par exemple, simplifiez l'expression `ABS(-100)` en `100`.  
  
* Si l'expression utilise plusieurs variables, songez à créer une colonne calculée pour l'expression, puis créez des statistiques ou un index sur la colonne calculée. Par exemple, le prédicat de requête `WHERE PRICE + Tax > 100` bénéficiera peut-être d'une meilleure estimation de cardinalité si vous créez une colonne calculée pour l'expression `Price + Tax`.  
  
### <a name="improving-cardinality-estimates-for-variables-and-functions"></a>Amélioration des estimations de cardinalité pour les variables et les fonctions  
Pour améliorer les estimations de cardinalité pour les variables et les fonctions, respectez les principes suivants :  
  
* Si le prédicat de requête utilise une variable locale, envisagez de réécrire la requête de sorte qu'elle utilise un paramètre au lieu d'une variable locale. La valeur d'une variable locale n'est pas connue au moment où l'optimiseur de requête crée le plan d'exécution de requête. Lorsqu'une requête utilise un paramètre, l'optimiseur de requête utilise l'estimation de cardinalité pour la première valeur effective du paramètre qui est transmise à la procédure stockée.  
  
* Envisagez d'utiliser une table standard ou une table temporaire pour consigner les résultats de fonctions table à instructions multiples (mstvf). L'optimiseur de requête ne crée pas de statistiques pour les fonctions table à instructions multiples. Du fait de cette approche, l'optimiseur de requête peut créer des statistiques sur les colonnes de table et s'en servir pour créer un meilleur plan de requête.  
  
* Envisagez d'utiliser une table standard ou une table temporaire en remplacement de variables de table. L'optimiseur de requête ne crée pas de statistiques pour les variables de table. Du fait de cette approche, l'optimiseur de requête peut créer des statistiques sur les colonnes de table et s'en servir pour créer un meilleur plan de requête. Il convient de peser le pour et le contre au moment d'opter pour une table temporaire ou une variable de table : les variables de table utilisées dans les procédures stockées aboutissent à moins de recompilations de ces dernières que les tables temporaires. Selon l'application, l'utilisation d'une table temporaire à la place d'une variable de table n'améliorera pas forcément les performances.  
  
* Si une procédure stockée contient une requête qui utilise un paramètre transmis, évitez de modifier la valeur du paramètre dans la procédure stockée avant de l'utiliser dans la requête. Les estimations de cardinalité de la requête sont basées sur la valeur du paramètre transmis et non sur la valeur mise à jour. Pour éviter de modifier la valeur du paramètre, vous pouvez réécrire la requête de sorte qu'elle utilise deux procédures stockées.  
  
     Par exemple, la procédure stockée suivante `Sales.GetRecentSales` modifie la valeur du paramètre `@date` lorsque `@date` a la valeur NULL.  
  
    ```sql  
    USE AdventureWorks2012;  
    GO  
    IF OBJECT_ID ( 'Sales.GetRecentSales', 'P') IS NOT NULL  
        DROP PROCEDURE Sales.GetRecentSales;  
    GO  
    CREATE PROCEDURE Sales.GetRecentSales (@date datetime)  
    AS BEGIN  
        IF @date IS NULL  
            SET @date = DATEADD(MONTH, -3, (SELECT MAX(ORDERDATE) FROM Sales.SalesOrderHeader))  
        SELECT * FROM Sales.SalesOrderHeader h, Sales.SalesOrderDetail d  
        WHERE h.SalesOrderID = d.SalesOrderID  
        AND h.OrderDate > @date  
    END  
    GO  
    ```  
  
     Si le premier appel à la procédure stockée `Sales.GetRecentSales` transmet une valeur NULL pour le paramètre `@date` , l'optimiseur de requête compilera la procédure stockée avec l'estimation de cardinalité de `@date = NULL` même si le prédicat de requête n'est pas appelé avec `@date = NULL`. Il se peut que cette estimation de cardinalité soit sensiblement différente du nombre de lignes présenté dans le résultat réel de la requête. En conséquence, l'optimiseur de requête risque de choisir un plan de requête non optimisé. Pour éviter cela, vous pouvez réécrire la procédure stockée dans deux procédures comme dans l'exemple suivant :  
  
    ```sql  
    USE AdventureWorks2012;  
    GO  
    IF OBJECT_ID ( 'Sales.GetNullRecentSales', 'P') IS NOT NULL  
        DROP PROCEDURE Sales.GetNullRecentSales;  
    GO  
    CREATE PROCEDURE Sales.GetNullRecentSales (@date datetime)  
    AS BEGIN  
        IF @date is NULL  
            SET @date = DATEADD(MONTH, -3, (SELECT MAX(ORDERDATE) FROM Sales.SalesOrderHeader))  
        EXEC Sales.GetNonNullRecentSales @date;  
    END  
    GO  
    IF OBJECT_ID ( 'Sales.GetNonNullRecentSales', 'P') IS NOT NULL  
        DROP PROCEDURE Sales.GetNonNullRecentSales;  
    GO  
    CREATE PROCEDURE Sales.GetNonNullRecentSales (@date datetime)  
    AS BEGIN  
        SELECT * FROM Sales.SalesOrderHeader h, Sales.SalesOrderDetail d  
        WHERE h.SalesOrderID = d.SalesOrderID  
        AND h.OrderDate > @date  
    END  
    GO  
    ```  
  
### <a name="improving-cardinality-estimates-with-query-hints"></a>Amélioration des estimations de cardinalité au moyen d'indicateurs de requête  
 Pour améliorer les estimations de cardinalité des variables locales, vous pouvez utiliser les indicateurs de requête `OPTIMIZE FOR <value>` ou `OPTIMIZE FOR UNKNOWN` avec RECOMPILE. Pour plus d’informations, consultez [Indicateurs de requête &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-query.md).  
  
 Pour certaines applications, la recompilation de la requête à chacune de ses exécutions peu prendre trop de temps. L'indicateur de requête `OPTIMIZE FOR` peut s'avérer utile même si vous n'utilisez pas l'option `RECOMPILE`. Par exemple, vous pouvez ajouter une option `OPTIMIZE FOR` à la procédure stockée Sales.GetRecentSales pour spécifier une date précise. L'exemple suivant ajoute l'option `OPTIMIZE FOR` à la procédure Sales.GetRecentSales.  
  
```sql  
USE AdventureWorks2012;  
GO  
IF OBJECT_ID ( 'Sales.GetRecentSales', 'P') IS NOT NULL  
    DROP PROCEDURE Sales.GetRecentSales;  
GO  
CREATE PROCEDURE Sales.GetRecentSales (@date datetime)  
AS BEGIN  
    IF @date is NULL  
        SET @date = DATEADD(MONTH, -3, (SELECT MAX(ORDERDATE) FROM Sales.SalesOrderHeader))  
    SELECT * FROM Sales.SalesOrderHeader h, Sales.SalesOrderDetail d  
    WHERE h.SalesOrderID = d.SalesOrderID  
    AND h.OrderDate > @date  
    OPTION ( OPTIMIZE FOR ( @date = '2004-05-01 00:00:00.000'))  
END;  
GO  
```  
  
### <a name="improving-cardinality-estimates-with-plan-guides"></a>Amélioration des estimations de cardinalité au moyen de repères de plan  
 Pour certaines applications, les recommandations en conception de requêtes peuvent ne pas s'appliquer, soit parce que vous ne pouvez pas modifier la requête, soit parce que l’indicateur de requête RECOMPILE risque d’entraîner un nombre trop important de recompilations. Vous pouvez utiliser des repères de plan pour spécifier d'autres indicateurs, tels que USE PLAN, dans le but de contrôler le comportement de la requête, en attendant de trouver une solution avec l'éditeur de l'application. Pour plus d'informations sur les repères de plan, consultez [Plan Guides](../../relational-databases/performance/plan-guides.md).  
  
  
## <a name="see-also"></a>Voir aussi  
 [CREATE STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/create-statistics-transact-sql.md)   
 [UPDATE STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/update-statistics-transact-sql.md)   
 [sp_updatestats &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-updatestats-transact-sql.md)   
 [DBCC SHOW_STATISTICS &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-show-statistics-transact-sql.md)   
 [Options ALTER DATABASE SET &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md)   
 [DROP STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/drop-statistics-transact-sql.md)   
 [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md)   
 [ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md)   
 [Créer des index filtrés](../../relational-databases/indexes/create-filtered-indexes.md)   
 [Contrôle du comportement des statistiques automatiques (AUTO_UPDATE_STATISTICS) dans SQL Server](https://support.microsoft.com/help/2754171)   
 [STATS_DATE &#40;Transact-SQL&#41;](../../t-sql/functions/stats-date-transact-sql.md)   
 [sys.dm_db_stats_properties &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-stats-properties-transact-sql.md)   
 [sys.dm_db_stats_histogram &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-stats-histogram-transact-sql.md)  
 [sys.stats](../../relational-databases/system-catalog-views/sys-stats-transact-sql.md)  
 [sys.stats_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-stats-columns-transact-sql.md)    
 [Adaptive Index Defrag](https://github.com/Microsoft/tigertoolbox/tree/master/AdaptiveIndexDefrag)