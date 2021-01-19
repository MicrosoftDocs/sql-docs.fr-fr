---
description: Index columnstore - Guide de conception
title: Index columnstore - Guide de conception | Microsoft Docs
ms.custom: ''
ms.date: 12/01/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
ms.assetid: fc3e22c2-3165-4ac9-87e3-bf27219c820f
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 72ab05dfce314119c30e08428fdcaa2b94ba25ed
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171771"
---
# <a name="columnstore-indexes---design-guidance"></a>Index columnstore - Guide de conception
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Recommandations générales pour la conception d’index columnstore. Un petit nombre de décisions conceptuelles judicieuses peuvent vous aider à obtenir les performances de requête élevées et l’excellente compression des données que les index columnstore sont censés fournir. 

## <a name="prerequisites"></a>Conditions préalables

Cet article part du principe que vous connaissez la terminologie et l’architecture de columnstore. Pour plus d’informations, consultez [Index columnstore - Présentation](../../relational-databases/indexes/columnstore-indexes-overview.md) et [Index columnstore - Architecture](../../relational-databases/sql-server-index-design-guide.md#columnstore_index).

### <a name="know-your-data-requirements"></a>Connaître vos exigences en matière de données
Avant de concevoir un index columnstore, essayez d’en savoir le plus possible sur vos exigences en matière de données. Par exemple, essayez de répondre à ces questions :

- Quelle est la taille ma table ?
- Mes requêtes effectuent-elles principalement une analytique sur de grandes plages de valeurs ?  Les index columnstore sont conçus pour bien fonctionner avec les analyses sur de grandes plages, plutôt que pour rechercher des valeurs spécifiques.
- Ma charge de travail effectue-t-elle un grand nombre de mises à jour et de suppressions ? Les index columnstore sont performants quand les données sont stables. Les requêtes doivent mettre à jour et supprimer moins de 10 % des lignes.
- Ai-je des tables de faits et de dimension pour un entrepôt de données ?
- Dois-je effectuer une analytique sur une charge de travail transactionnelle ? Si c’est le cas, consultez le Guide de conception de columnstore pour l’analytique opérationnelle en temps réel.

Vous n’avez peut-être pas besoin d’un index columnstore. Les index rowstore avec des segments de mémoire ou des index cluster fonctionnent de manière optimale sur les requêtes qui recherchent une valeur spécifique au sein des données ou sur les requêtes qui interrogent une petite plage de valeurs. Utilisez les index rowstore avec des charges de travail transactionnelles, car ils ont tendance à nécessiter principalement des recherches de table plutôt que des analyses de table sur des plages étendues.  

## <a name="choose-the-best-columnstore-index-for-your-needs"></a>Choisir l’index columnstore le plus adapté à vos besoins

Un index columnstore est un index cluster ou non cluster.  Un index columnstore cluster peut avoir un ou plusieurs index d’arbre B (B-tree) non-cluster. Les index columnstore sont faciles à essayer. Si vous créez une table en tant qu’index columnstore, vous pouvez facilement la reconvertir en table rowstore en supprimant l’index columnstore. 

Voici un récapitulatif des options et des recommandations. 

| Option de columnstore | Quand l’utiliser | Compression |
| :----------------- | :------------------- | :---------- |
| Index columnstore cluster | À utiliser pour :<br></br>1) Une charge de travail d’entrepôt des données traditionnelle avec un schéma en étoile ou en flocon<br></br>2) Des charges de travail Internet des objets (IOT) qui insèrent de grands volumes de données avec des mises à jour et des suppressions minimales. | 10x en moyenne |
| Index d’arbre B (B-tree) non-cluster sur un index cluster columnstore | À utiliser pour :<br></br>    1. Appliquer des contraintes de clé primaire et de clé étrangère sur un index cluster columnstore.<br></br>    2. Accélérer les requêtes qui recherchent des valeurs spécifiques ou de petites plages de valeurs.<br></br>    3. Accélérer les mises à jour et les suppressions de lignes spécifiques.| 10x en moyenne, plus du stockage supplémentaire pour les index non cluster.|
| Index columnstore non-cluster sur un segment de mémoire sur disque ou un index d’arbre B (B-tree) | À utiliser pour : <br></br>1) Une charge de travail OLTP ayant certaines requêtes analytiques. Vous pouvez supprimer les index d’arbre B (B-tree) créés pour l’analytique, et les remplacer par un index columnstore non-cluster.<br></br>2) De nombreuses charges de travail OLTP traditionnelles qui effectuent des opérations Extract, Transform et Load (ETL) pour déplacer des données vers un entrepôt de données distinct. Vous pouvez éliminer les opérations ETL et l’entrepôt de données distinct en créant un index non cluster columnstore sur certaines des tables OLTP. | NCCI est un index supplémentaire qui nécessite en moyenne 10 % de stockage en plus.|
| Index columnstore sur une table en mémoire | Mêmes recommandations que pour les index non cluster columnstore sur une table sur disque, mais la table de base est une table en mémoire. | L’index columnstore est un index supplémentaire.|

## <a name="use-a-clustered-columnstore-index-for-large-data-warehouse-tables"></a>Utiliser un index cluster columnstore pour les tables d’entrepôt de données de grande taille
L’index cluster columnstore est plus qu’un index, il s’agit du principal stockage de table. Il offre une compression élevée des données et améliore sensiblement les performances de requête sur les tables de faits et de dimension d’entreposage des données de grande taille. Les index cluster columnstore conviennent mieux aux requêtes analytiques qu’aux requêtes transactionnelles, car les requêtes analytiques ont tendance à effectuer des opérations sur de grandes plages de valeurs, plutôt que de rechercher des valeurs spécifiques. 

Utilisez un index cluster columnstore quand :

- Chaque partition a au moins un million de lignes. Les index columnstore ont des rowgroups dans chaque partition. Si la table est trop petite pour remplir un rowgroup dans chaque partition, vous ne tirerez pas parti des avantages de columnstore en matière de performances de requête et de compression.
- Les requêtes effectuent principalement une analytique sur des plages de valeurs. Par exemple, pour rechercher la valeur moyenne d’une colonne, la requête doit analyser toutes les valeurs de la colonne. Elle effectue ensuite une agrégation des valeurs en les additionnant, afin de déterminer la moyenne.
- La plupart des insertions concernent des volumes importants de données, avec des mises à jour et des suppressions minimales. De nombreuses charges de travail telles qu’Internet des objets (IOT) insèrent de grands volumes de données avec des mises à jour et des suppressions minimales. Ces charges de travail peuvent bénéficier des gains en matière de compression et de performances de requête liés à l’utilisation d’un index cluster columnstore.

N’utilisez pas d’index cluster columnstore quand :

* La table nécessite des types de données varchar(max), nvarchar(max) ou varbinary(max). Ou alors concevez l’index columnstore afin qu’il n’inclut pas ces colonnes.
* Les données de table ne sont pas permanentes. Utilisez plutôt un segment de mémoire ou une table temporaire quand vous avez besoin de stocker et de supprimer les données rapidement.
* La table a moins d’un million de lignes par partition. 
* Plus de 10 % des opérations sur la table sont des mises à jour et des suppressions. Un nombre élevé de mises à jour et de suppressions provoquent une fragmentation. La fragmentation affecte le taux de compression et les performances de requête jusqu’à ce que vous exécutiez une opération appelée « Réorganisation », qui force toutes les données dans le columnstore et supprime la fragmentation. Pour plus d’informations, consultez [Réduction de la fragmentation d’index dans les index columnstore](/archive/blogs/sqlserverstorageengine/columnstore-index-defragmentation-using-reorganize-command).

Pour plus d’informations, consultez [Index columnstore - Entreposage des données](../../relational-databases/indexes/columnstore-indexes-data-warehouse.md).

## <a name="add-b-tree-nonclustered-indexes-for-efficient-table-seeks"></a>Ajouter des index d’arbre B (B-tree) non-cluster pour améliorer l’efficacité des recherches dans les tables

À compter de [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)], vous pouvez créer des index d’arbre B (B-tree) non-cluster comme index secondaires sur un index columnstore cluster. L’index d’arbre B (B-tree) non-cluster est mis à jour à mesure que l’index columnstore est modifié. Il s’agit d’une fonctionnalité puissante que vous pouvez utiliser à votre avantage. 

L’index d’arbre B (B-tree) secondaire vous permet de rechercher efficacement des lignes spécifiques sans avoir à analyser toutes les lignes.  D’autres options sont également disponibles. Par exemple, vous pouvez appliquer une contrainte de clé primaire ou étrangère à l’aide d’une contrainte UNIQUE sur l’index d’arbre B (B-tree). Étant donné qu’une valeur non unique ne peut pas être insérée dans l’index d’arbre B (B-tree), [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne peut pas insérer la valeur dans le columnstore. 

Utilisez un index d’arbre B (B-tree) sur un index columnstore pour :
* Exécuter des requêtes qui recherchent des valeurs particulières ou de petites plages de valeurs.
* Appliquer une contrainte, telle qu’une contrainte de clé primaire ou de clé étrangère.
* Effectuer des opérations de mise à jour et de suppression de manière efficace. L’index d’arbre B (B-tree) peut localiser rapidement les lignes spécifiques pour les mises à jour et les suppressions sans avoir à analyser toute la table ou la partition d’une table.
* Vous disposez de stockage supplémentaire pour stocker l’index d’arbre B (B-tree).

## <a name="use-a-nonclustered-columnstore-index-for-real-time-analytics"></a>Utiliser un index non cluster columnstore pour l’analytique en temps réel

À compter de [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)], vous pouvez avoir un index non cluster columnstore sur une table sur disque rowstore ou dans une table OLTP en mémoire. Vous pouvez ainsi exécuter une analytique en temps réel sur une table transactionnelle. Pendant que les transactions ont lieu sur la table sous-jacente, vous pouvez exécuter l’analytique sur l’index columnstore. Étant donné qu’une même table gère les deux index, les modifications sont accessibles en temps réel aux index rowstore et columnstore.

Un index columnstore offrant une compression des données dix fois supérieure à celle d’un index rowstore, il n’a besoin que d’une petite quantité de stockage supplémentaire. Par exemple, si la table rowstore compressée prend 20 Go, l’index columnstore peut nécessiter 2 Go supplémentaires. L’espace supplémentaire requis dépend également du nombre de colonnes dans l’index non cluster columnstore. 

 Utilisez un index non cluster columnstore pour :

* Exécuter une analytique en temps réel sur une table rowstore transactionnelle. Vous pouvez remplacer les index d’arbre B (B-tree) existants qui sont conçus pour l’analytique par un index columnstore non-cluster. 
  
*   Éliminer la nécessité d’un entrepôt de données distinct. En règle générale, les entreprises exécutent des transactions sur une table rowstore, puis chargent les données dans un entrepôt de données distinct pour exécuter l’analytique. Pour de nombreuses charges de travail, vous pouvez éliminer le processus de chargement et l’entrepôt de données distinct en créant un index non cluster columnstore sur des tables transactionnelles.

  [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] propose plusieurs stratégies pour rendre ce scénario performant. Il est très facile de l’essayer, car vous pouvez activer un index non cluster columnstore sans modifier votre application OLTP. 

Pour ajouter des ressources de traitement supplémentaires, vous pouvez exécuter l’analytique sur un secondaire lisible. Le recours à un secondaire lisible sépare le traitement de la charge de travail transactionnelle de celui de la charge de travail analytique. 

Pour plus d’informations, consultez [Bien démarrer avec les index columnstore pour l’analytique opérationnelle en temps réel](../../relational-databases/indexes/get-started-with-columnstore-for-real-time-operational-analytics.md).

Pour plus d’informations sur le choix du meilleur index columnstore, consultez le blog de Sunil Agarwal intitulé [Quel est l’index columnstore le plus adapté à ma charge de travail ?](/archive/blogs/sql_server_team/columnstore-index-which-columnstore-index-is-right-for-my-workload).

## <a name="use-table-partitions-for-data-management-and-query-performance"></a>Utiliser des partitions de table pour les performances de requête et la gestion des données
Les index columnstore prennent en charge le partitionnement, qui est un bon moyen de gérer et d’archiver les données. Le partitionnement améliore également les performances de requête en limitant les opérations à une ou plusieurs partitions.

### <a name="use-partitions-to-make-the-data-easier-to-manage"></a>Utiliser des partitions pour simplifier la gestion des données
Pour les tables volumineuses, le seul moyen pratique de gérer les plages de données consiste à utiliser des partitions. Les avantages offerts par les partitions pour les tables rowstore s’appliquent également aux index columnstore. 

Par exemple, les tables rowstore et columnstore utilisent des partitions pour :

- Contrôler la taille des sauvegardes incrémentielles. Vous pouvez sauvegarder des partitions dans des groupes de fichiers distincts, puis les marquer comme étant en lecture seule. Ainsi, les sauvegardes ultérieures ignoreront les groupes de fichiers en lecture seule. 
- Réduisez les coûts de stockage en déplaçant une partition plus ancienne vers un stockage moins coûteux. Par exemple, vous pouvez utiliser le basculement de partition pour déplacer une partition vers un emplacement de stockage moins coûteux.
- Optimisez l’efficacité des opérations en les limitant à une partition. Par exemple, vous pouvez cibler uniquement les partitions fragmentées pour la maintenance d’index.

En outre, avec un index columnstore, vous utilisez le partitionnement pour :

* Économiser 30 % de plus sur les coûts de stockage. Vous pouvez compresser les anciennes partitions avec les options de compression COLUMNSTORE_ARCHIVE. Les données seront plus lentes pour les performances de requête, ce qui est acceptable si la partition est rarement interrogée.

### <a name="use-partitions-to-improve-query-performance"></a>Utiliser des partitions pour améliorer les performances de requête

Grâce aux partitions, vous pouvez limiter vos requêtes pour analyser uniquement des partitions spécifiques, ce qui limite le nombre de lignes à analyser. Par exemple, si l’index est partitionné par année et que la requête analyse des données de l’année précédente, elle n’a besoin d’analyser que les données d’une seule partition. 

### <a name="use-fewer-partitions-for-a-columnstore-index"></a>Utiliser moins de partitions pour un index columnstore

À moins d’avoir une taille de données suffisamment importante, un index columnstore offre de meilleures performances avec moins de partitions que pour un index rowstore. Si vous n’avez pas au moins un million de lignes par partition, la plupart de vos lignes risquent d’aller dans le deltastore où elles ne bénéficient pas de l’amélioration des performances de compression de columnstore. Par exemple, si vous chargez un million de lignes dans une table avec 10 partitions et que chaque partition reçoit 100 000 lignes, toutes les lignes iront dans des rowgroups delta. 

Exemple :
* Chargez 1 000 000 lignes dans une partition ou une table non partitionnée. Vous obtenez un rowgroup compressé avec 1 000 000 lignes. C’est parfait pour bénéficier d’une compression des données et de performances de requête élevées.
* Chargez 1 000 000 lignes uniformément dans 10 partitions. Chaque partition reçoit 100 000 lignes, ce qui est inférieur au seuil minimal pour la compression columnstore. Ainsi, l’index columnstore peut avoir 10 rowgroups delta avec 100 000 lignes dans chaque. Il existe des moyens de forcer les rowgroups delta dans le columnstore. Toutefois, s’il s’agit des seules lignes de l’index columnstore, les rowgroups compressés seront trop petits pour bénéficier d’un meilleur niveau de performance de compression et de requête.

Pour plus d’informations sur le partitionnement, voir le blog de Sunil Agarwal intitulé [Dois-je partitionner mon index columnstore ?](/archive/blogs/sqlserverstorageengine/columnstore-index-should-i-partition-my-columnstore-index).

## <a name="choose-the-appropriate-data-compression-method"></a>Choisir la méthode de compression des données appropriée
L’index columnstore propose deux options pour la compression des données : la compression columnstore et la compression d’archive. Vous pouvez choisir l’option de compression quand vous créez l’index, ou la changer ultérieurement à l’aide de [ALTER INDEX... REBUILD](../../t-sql/statements/alter-index-transact-sql.md).

### <a name="use-columnstore-compression-for-best-query-performance"></a>Utiliser la compression columnstore pour de meilleures performances de requête
La compression columnstore offre en général des taux de compression 10 fois supérieurs aux index rowstore. Il s’agit de la méthode de compression standard pour les index columnstore. Elle offre des performances de requête élevées. 

### <a name="use-archive-compression-for-best-data-compression"></a>Utiliser la compression d’archive pour une meilleure compression des données
La compression d’archive est conçue pour offrir une compression maximale quand les performances de requête ne sont pas aussi importantes. Elle permet d’obtenir des taux de compression des données supérieures à la compression columnstore, mais elle a un prix. La compression et la décompression des données étant plus longues, cette approche ne convient pas si la rapidité des requêtes est cruciale. 

## <a name="use-optimizations-when-you-convert-a-rowstore-table-to-a-columnstore-index"></a>Utiliser des optimisations quand vous convertissez une table rowstore en index columnstore

Si vos données sont déjà dans une table rowstore, vous pouvez utiliser [CREATE COLUMNSTORE INDEX](../../t-sql/statements/create-columnstore-index-transact-sql.md) pour convertir la table en index cluster columnstore. Les optimisations décrites ci-dessous permettent d’améliorer les performances de requête après la conversion de la table.

### <a name="use-maxdop-to-improve-rowgroup-quality"></a>Utiliser MAXDOP pour améliorer la qualité de rowgroup
Vous pouvez configurer le nombre maximal de processeurs pour la conversion d’un segment de mémoire ou d’un index d’arbre B (B-tree) en cluster en un index columnstore. Pour configurer les processeurs, utilisez l’option de degré maximal de parallélisme (MAXDOP). 

Si vous avez de grandes quantités de données, MAXDOP 1 sera probablement trop lent.  Augmenter MAXDOP à 4 donne de bons résultats. Si l’une des conséquences est que certains rowgroups n’ont pas le nombre de lignes optimal, vous pouvez exécuter [ALTER INDEX REORGANIZE](../../t-sql/statements/alter-index-transact-sql.md) pour les fusionner en arrière-plan.

### <a name="keep-the-sorted-order-of-a-b-tree-index"></a>Conserver l’ordre de tri d’un index d’arbre B (B-tree)
Étant donné que l’index d’arbre B (B-tree) stocke déjà les lignes dans un ordre trié, le fait de conserver cet ordre quand les lignes sont compressées dans l’index columnstore peut améliorer les performances.

L’index columnstore ne trie pas les données, mais il utilise des métadonnées pour effectuer le suivi des valeurs minimales et maximales de chaque segment de colonne dans chaque rowgroup.  Lors de l’analyse d’une plage de valeurs, il peut calculer rapidement quand il faut ignorer le rowgroup. Quand les données sont triées, davantage de rowgroups peuvent être ignorés. 

Pour conserver l’ordre de tri pendant la conversion :
* Utilisez [CREATE COLUMNSTORE INDEX](../../t-sql/statements/create-columnstore-index-transact-sql.md) avec la clause DROP_EXISTING. Cela conserve également le nom de l’index. Si vous avez des scripts qui utilisent déjà le nom de l’index rowstore, vous n’aurez pas besoin de les mettre à jour. 

    Cet exemple convertit un index rowstore cluster sur une table nommée `MyFactTable` en un index cluster columnstore. Le nom d’index, `ClusteredIndex_d473567f7ea04d7aafcac5364c241e09`, reste identique.

    ```sql
    CREATE CLUSTERED COLUMNSTORE INDEX ClusteredIndex_d473567f7ea04d7aafcac5364c241e09  
    ON MyFactTable  
    WITH (DROP_EXISTING = ON);  
    ```

## <a name="related-tasks"></a>Tâches associées  
Il s’agit de tâches pour créer et tenir à jour des index columnstore. 
  
|Tâche|Rubriques de référence|Notes|  
|----------|----------------------|-----------|  
|Créer une table sous forme de columnstore|[CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)|Depuis [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)], vous pouvez créer la table en tant qu’index cluster columnstore. Il est inutile de créer au préalable une table rowstore, puis de la convertir en columnstore.|  
|Créer une table mémoire avec un index columnstore.|[CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)|Depuis [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)], vous pouvez créer une table optimisée en mémoire avec un index columnstore. L’index columnstore peut également être ajouté après la création de la table, à l’aide de la syntaxe ALTER TABLE ADD INDEX.|  
|Convertir une table rowstore en table columnstore|[CREATE COLUMNSTORE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-columnstore-index-transact-sql.md)|Convertissez un segment de mémoire ou un arbre binaire existant en columnstore. Les exemples montrent comment gérer les index existants, ainsi que le nom de l’index lors de cette conversion.|  
|Convertir une table columnstore en rowstore|[CREATE CLUSTERED INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-columnstore-index-transact-sql.md#d-convert-a-columnstore-table-to-a-rowstore-table-with-a-clustered-index) ou [Reconvertir une table columnstore en segment rowstore](../../t-sql/statements/create-columnstore-index-transact-sql.md#e-convert-a-columnstore-table-back-to-a-rowstore-heap) |Cette conversion n’est généralement pas nécessaire, mais le cas peut se présenter. Les exemples montrent comment convertir un columnstore en segment de mémoire ou index cluster.|   
|Créer un index columnstore sur une table rowstore|[CREATE COLUMNSTORE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-columnstore-index-transact-sql.md)|Une table rowstore ne peut avoir qu’un seul index columnstore.  Depuis [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)], l’index columnstore peut avoir une condition de filtrage. Les exemples affichent la syntaxe de base.|  
|Créer des index performants pour l’analytique opérationnelle|[Prise en main de columnstore pour l’analytique opérationnelle en temps réel](../../relational-databases/indexes/get-started-with-columnstore-for-real-time-operational-analytics.md)|Décrit comment créer des index columnstore et B-tree complémentaires pour que les requêtes OLTP utilisent des index B-tree et que les requêtes analytiques utilisent des index columnstore.|  
|Créer des index columnstore performants pour l’entreposage des données|[Index columnstore - Entreposage des données](../../relational-databases/indexes/columnstore-indexes-data-warehouse.md)|Décrit comment utiliser des index B-tree sur les tables columnstore pour créer des requêtes performantes en matière d’entreposage des données.|  
|Utiliser un index B-tree pour appliquer une contrainte de clé primaire sur un index columnstore.|[Index columnstore - entreposage des données](../../relational-databases/indexes/columnstore-indexes-data-warehouse.md)|Montre comment combiner des index B-tree et columnstore pour appliquer des contraintes de clé primaire sur l’index columnstore.|  
|Abandonner un index columnstore|[DROP INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/drop-index-transact-sql.md)|L’abandon d’un index columnstore utilise la syntaxe DROP INDEX standard utilisée par les index B-tree. L’abandon d’un index cluster columnstore convertit la table columnstore en segment de mémoire.|  
|Supprimer une ligne d’un index columnstore|[DELETE &#40;Transact-SQL&#41;](../../t-sql/statements/delete-transact-sql.md)|Utilisez [DELETE &#40;Transact-SQL&#41;](../../t-sql/statements/delete-transact-sql.md) pour supprimer une ligne.<br /><br /> Ligne **columnstore** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] marque la ligne comme étant supprimée logiquement, mais ne récupère pas le stockage physique pour la ligne tant que l’index n’est pas reconstruit.<br /><br /> Ligne **deltastore** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] supprime la ligne logiquement et physiquement.|  
|Mettre à jour une ligne dans l’index columnstore|[UPDATE &#40;Transact-SQL&#41;](../../t-sql/queries/update-transact-sql.md)|Utilisez [UPDATE &#40;Transact-SQL&#41;](../../t-sql/queries/update-transact-sql.md) pour mettre à jour une ligne.<br /><br /> Ligne **columnstore** :  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] marque la ligne comme étant supprimée logiquement, puis insère la ligne mise à jour dans le deltastore.<br /><br /> Ligne **deltastore** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] met à jour la ligne dans le deltastore.|  
|Obliger toutes les lignes du deltastore à aller dans le columnstore.|[ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md) ... REBUILD<br /><br /> [Réorganiser et reconstruire des index](../../relational-databases/indexes/reorganize-and-rebuild-indexes.md)|ALTER INDEX avec l’option REBUILD oblige toutes les lignes à aller dans le columnstore.|  
|Défragmenter un index columnstore|[ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md)|ALTER INDEX ... REORGANIZE défragmente les index columnstore en ligne.|  
|Fusionner des tables avec les index columnstore|[MERGE &#40;Transact-SQL&#41;](../../t-sql/statements/merge-transact-sql.md)|

## <a name="next-steps"></a>Étapes suivantes
Pour créer un index columnstore vide pour :

* [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou [!INCLUDE[ssSDS](../../includes/sssds-md.md)], consultez [CREATE TABLE (Transact-SQL)](../../t-sql/statements/create-table-transact-sql.md).
* [!INCLUDE[ssSDW](../../includes/sssdw-md.md)], consultez [CREATE TABLE (Azure Synapse Analytics)](../../t-sql/statements/create-table-as-select-azure-sql-data-warehouse.md).

Pour plus d’informations sur la façon de convertir un segment de mémoire rowstore ou un index B-tree existant dans un index columnstore cluster, ou pour créer un index columnstore non-cluster, consultez [CREATE COLUMNSTORE INDEX (Transact-SQL)](../../t-sql/statements/create-columnstore-index-transact-sql.md).