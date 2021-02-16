---
description: 'Index columnstore : Vue d’ensemble'
title: 'Index columnstore : vue d’ensemble | Microsoft Docs'
ms.custom: ''
ms.date: 05/08/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- indexes creation, columnstore
- indexes [SQL Server], columnstore
- columnstore index
- batch mode execution
- columnstore index, described
- xVelocity, columnstore indexes
ms.assetid: f98af4a5-4523-43b1-be8d-1b03c3217839
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: f4ac369f42fccd251cffb5b06f3603f8349f857a
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/01/2021
ms.locfileid: "100351222"
---
# <a name="columnstore-indexes-overview"></a>Index columnstore : Vue d’ensemble
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Les index columnstore sont la norme pour le stockage et l’interrogation des tables de faits d’entreposage de données de grande taille. Ils utilisent un stockage de données en colonnes et un traitement des requêtes allant jusqu’à multiplier par 10 les performances des requêtes dans l’entrepôt de données par rapport au stockage orienté lignes classique. Vous pouvez également obtenir jusqu’à 10 fois la compression de données par rapport à la taille des données décompressées. À partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP1, les index columnstore sont compatibles avec l’analytique opérationnelle, qui permet d’exécuter des analyses en temps réel performantes sur une charge de travail transactionnelle.  
  
Voici un scénario connexe :  
  
-   [Index columnstore pour l’entreposage des données](../../relational-databases/indexes/columnstore-indexes-data-warehouse.md)  
-   [Bien démarrer avec columnstore pour l’analytique opérationnelle en temps réel](../../relational-databases/indexes/get-started-with-columnstore-for-real-time-operational-analytics.md)  
  
## <a name="what-is-a-columnstore-index"></a>Qu’est-ce qu’un index columnstore ?  
Un index columnstore est une technologie permettant de stocker, de récupérer et de gérer les données suivant un format de données en colonnes, appelé *columnstore*.  
  
### <a name="key-terms-and-concepts"></a>Termes et concepts clés  
Les termes et concepts clés suivants sont associés aux index columnstore.  
  
#### <a name="columnstore"></a>columnstore
Un columnstore représente des données organisées logiquement sous la forme d’une table comportant des lignes et des colonnes, et stockées physiquement dans un format de données en colonnes.  
  
#### <a name="rowstore"></a>Rowstore
Un rowstore représente des données organisées logiquement sous la forme d’une table comportant des lignes et des colonnes, et stockées physiquement dans un format de données en colonnes. Il s'agit de la méthode classique de stockage des données de tables relationnelles. Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], un rowstore correspond à une table dont le format de stockage de données sous-jacent est un segment de mémoire, un index cluster ou une table à mémoire optimisée.  
  
> [!NOTE]  
> Dans les discussions au sujet des index columnstore, les termes rowstore et columnstore sont utilisés pour mettre en évidence le format du stockage de données.  
  
#### <a name="rowgroup"></a>Rowgroup
Un rowgroup est un groupe de lignes compressées simultanément au format columnstore. Il contient généralement le nombre maximal de lignes par rowgroup, soit 1 048 576 lignes.  
  
Dans un souci de haute performance et de taux de compression élevés, l’index columnstore découpe la table en rowgroups, puis compresse chaque rowgroup en colonnes. Le nombre de lignes dans le groupe de lignes doit être assez grand pour améliorer le taux de compression et assez petit pour tirer parti des opérations en mémoire.    

Un rowgroup à partir duquel toutes les données ont été supprimées passe de l’état COMPRESSED à l’état TOMBSTONE, et est ensuite supprimé par un processus en arrière-plan nommé moteur de tuple. Pour plus d’informations sur les états de rowgroup, consultez [sys.dm_db_column_store_row_group_physical_stats (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-db-column-store-row-group-physical-stats-transact-sql.md).

> [!TIP]
> Avoir un trop grand nombre de rowgroups de petite taille réduit la qualité de l’index columnstore. Jusqu’à [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)], une opération de réorganisation est nécessaire pour fusionner des rowgroups COMPRESSED plus petits, suite à une stratégie de seuil interne qui détermine comment supprimer les lignes supprimées et combiner les rowgroups compressés.    
> À partir de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)], une tâche de fusion en arrière-plan fonctionne également pour fusionner les rowgroups COMPRESSED à partir desquels un grand nombre de lignes a été supprimé.     
> Après la fusion de plusieurs rowgroups plus petits, la qualité de l’index doit être améliorée. 

> [!NOTE]
> À partir de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)], le moteur de tuple est aidé par une tâche de fusion en arrière-plan qui compresse automatiquement les rowgroups delta OPEN plus petits qui existent depuis un certain temps, tel que déterminé par un seuil interne, ou qui fusionne les rowgroups COMPRESSED à partir desquels un grand nombre de lignes a été supprimé. Cela améliore la qualité de l’index columnstore dans le temps.         

#### <a name="column-segment"></a>segment de colonne
Un segment de colonne est une colonne de données issue du rowgroup.  
  
-   Chaque rowgroup contient un segment de colonne pour chaque colonne dans la table.  
-   Chaque segment de colonne est compressé et stocké sur un support physique.  
  
![Segment de colonne](../../relational-databases/indexes/media/sql-server-pdw-columnstore-columnsegment.gif "segment de colonne")  
  
#### <a name="clustered-columnstore-index"></a>Index columnstore cluster
Un index columnstore cluster représente le stockage physique de la totalité de la table.    
  
![Index columnstore cluster](../../relational-databases/indexes/media/sql-server-pdw-columnstore-physicalstorage.gif "Index Columnstore cluster")  
  
Pour réduire la fragmentation des segments de colonne et améliorer les performances, l’index columnstore est susceptible de stocker temporairement des données dans un index cluster appelé *deltastore*, ainsi que la liste btree des ID des lignes supprimées. Les opérations deltastore sont effectuées en coulisse. Pour retourner des résultats de requête corrects, l'index columnstore cluster associe les résultats de columnstore et de deltastore.  
  
#### <a name="delta-rowgroup"></a>Rowgroup delta
Un rowgroup delta est un index B-tree en cluster utilisé uniquement avec des index columnstore. Il améliore la compression columnstore et les performances en stockant des lignes jusqu’à ce que leur nombre atteigne un certain seuil (1 048 576 lignes) et qu’elles soient alors déplacées dans le columnstore.  

Lorsqu’un rowgroup delta atteint le nombre maximal de lignes, il passe de l’état OPEN à l’état CLOSED. Un processus en arrière-plan nommé moteur de tuple vérifie les groupes de lignes fermés. Lorsqu’il trouve un rowgroup fermé, il compresse le rowgroup delta et le stocke dans le columnstore en tant que rowgroup COMPRESSED. 

Quand un rowgroup delta a été compressé, le rowgroup delta existant passe à l’état TOMBSTONE pour être supprimé ultérieurement par le moteur de tuple lorsqu’il n’y a aucune référence à celui-ci. 

Pour plus d’informations sur les états de rowgroup, consultez [sys.dm_db_column_store_row_group_physical_stats (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-db-column-store-row-group-physical-stats-transact-sql.md). 

> [!NOTE]
> À partir de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)], le moteur de tuple est aidé par une tâche de fusion en arrière-plan qui compresse automatiquement les rowgroups delta OPEN plus petits qui existent depuis un certain temps, tel que déterminé par un seuil interne, ou qui fusionne les rowgroups COMPRESSED à partir desquels un grand nombre de lignes a été supprimé. Cela améliore la qualité de l’index columnstore dans le temps.         
  
#### <a name="deltastore"></a>Deltastore
Un index columnstore peut avoir plusieurs rowgroups delta, qui sont appelés collectivement le deltastore.   

Lors d'un chargement en masse important, la plupart des lignes sont directement placées dans le columnstore sans passer par le deltastore. Il peut arriver que, à la fin du chargement en masse, le nombre de lignes soit inférieur à la taille minimale d'un rowgroup, qui est de 102 400 lignes. Dans ce cas, les lignes finales sont placées dans le deltastore plutôt que dans le columnstore. Pour les petits chargements en masse de taille inférieure à 102 400 lignes, toutes les lignes vont directement au deltastore.  
  
#### <a name="nonclustered-columnstore-index"></a>index columnstore non cluster
Un index columnstore non cluster et un index columnstore cluster fonctionnent de la même manière. La différence réside dans le fait qu’un index non cluster est un index secondaire créé sur une table rowstore, tandis qu’un index cluster columnstore correspond au stockage principal de la table entière.  
  
L’index non cluster contient une copie de tout ou partie des lignes et des colonnes de la table sous-jacente. L’index est défini comme une ou plusieurs colonnes de la table ; il a une condition facultative qui filtre les lignes.  
  
Un index columnstore non cluster est compatible avec l’analytique opérationnelle en temps réel dans laquelle la charge de travail OLTP utilise l’index cluster sous-jacent, tandis que l’analytique s’exécute simultanément sur l’index columnstore. Pour plus d’informations, voir [Bien démarrer avec columnstore pour l’analytique opérationnelle en temps réel](../../relational-databases/indexes/get-started-with-columnstore-for-real-time-operational-analytics.md).  
  
#### <a name="batch-mode-execution"></a>Exécution en mode batch
L’exécution en mode batch est une méthode de traitement des requêtes utilisée pour traiter plusieurs lignes ensemble. L’exécution en mode batch est étroitement intégrée au format de stockage columnstore et optimisée pour celui-ci. L’exécution en mode batch est parfois appelée *exécution vectorielle* ou *vectorisée*. Les requêtes sur les index columnstore utilisent l’exécution en mode batch, qui permet généralement de multiplier les performances des requêtes par deux ou quatre. Pour plus d’informations, voir [Guide d’architecture de traitement des requêtes](../query-processing-architecture-guide.md#execution-modes). 
  
##  <a name="why-should-i-use-a-columnstore-index"></a><a name="benefits"></a> Pourquoi utiliser un index columnstore ?  
Un index columnstore peut offrir un niveau très élevé de compression de données (généralement multiplié par 10) afin de réduire de manière significative le coût de stockage de l’entrepôt de données. Pour l’analytique, il présente des performances réellement meilleures que celles d’un index btree. Il s’agit du format de stockage de données de prédilection pour les charges de travail d’entreposage des données et d’analytique. Depuis [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], vous pouvez utiliser des index columnstore pour l’analyse en temps réel sur votre charge de travail opérationnelle.  
  
Voici les raisons pour lesquelles les index columnstore sont si rapides :  
  
-   Les valeurs columnstore d’un même domaine sont généralement similaires, ce qui se traduit par des taux de compression élevés. Les goulots d’étranglement d’E/S du système sont atténués ou éliminés, et l’encombrement en mémoire est considérablement réduit.  
  
-   Les taux de compression élevés améliorent les performances des requêtes en utilisant un plus faible encombrement en mémoire. En effet, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut exécuter davantage d'opérations de requêtes et de données en mémoire.  
  
-   L’exécution par lot multiplie généralement les performances des requêtes par deux ou quatre grâce au traitement simultané de plusieurs lignes.  
  
-   Les requêtes sélectionnent souvent seulement quelques colonnes d'une table, ce qui réduit les E/S totales à partir du support physique.  
  
## <a name="when-should-i-use-a-columnstore-index"></a>Quand utiliser un index columnstore ?  
Voici les cas d’utilisation recommandés :  
  
-   Utilisez un index cluster columnstore pour stocker les tables de faits et de grandes tables de dimension pour les charges de travail d’entreposage de données. Cette méthode peut permettre de multiplier par 10 les performances des requêtes et la compression des données. Pour plus d’informations, voir [Index columnstore pour l’entreposage des données](~/relational-databases/indexes/columnstore-indexes-data-warehouse.md).  
  
-   Utilisez un index columnstore non cluster pour effectuer une analyse en temps réel sur une charge de travail OLTP. Pour plus d’informations, voir [Bien démarrer avec columnstore pour l’analytique opérationnelle en temps réel](../../relational-databases/indexes/get-started-with-columnstore-for-real-time-operational-analytics.md).  
  
### <a name="how-do-i-choose-between-a-rowstore-index-and-a-columnstore-index"></a>Comment choisir entre un index rowstore et un index columnstore ?  
Les index rowstore fonctionnent de manière optimale sur les requêtes qui recherchent une valeur spécifique au sein des données et sur celles qui interrogent une petite plage de valeurs. Utilisez les index rowstore avec des charges de travail transactionnelles, car ils passent principalement par des recherches de tables plutôt que par des analyses de tables.  
  
Les index columnstore offrent des gains de performances élevés pour les requêtes analytiques qui analysent de grandes quantités de données, en particulier sur des tables volumineuses. Utilisez les index columnstore sur les charges de travail d’analytique et d’entreposage des données, en particulier sur les tables de faits, car ils passent généralement par des analyses de tables complètes plutôt que par des recherches de tables.  
  
### <a name="can-i-combine-rowstore-and-columnstore-on-the-same-table"></a>Puis-je combiner les formats rowstore et columnstore dans la même table ?  
Oui. À partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], il est possible de créer un index columnstore non cluster pouvant être mis à jour sur une table rowstore. L’index columnstore stocke une copie des colonnes sélectionnées. Il faut donc de l’espace supplémentaire pour ces données, même si elles sont compressées en moyenne 10 fois. Il est possible d’exécuter simultanément l’analytique sur l’index columnstore et les transactions sur l’index rowstore. Le columnstore est mis à jour en cas de modification des données de la table rowstore. Les deux index utilisent donc les mêmes données.  
  
À partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], il est possible d’avoir un ou plusieurs index rowstore non cluster sur un index columnstore et d’effectuer des recherches de tables efficaces sur le columnstore sous-jacent. D’autres options sont également disponibles. Par exemple, vous pouvez appliquer une contrainte de clé primaire à l’aide d’une contrainte UNIQUE sur la table rowstore. Dans la mesure où une valeur non unique ne s’insère pas dans la table rowstore, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne peut pas l’ajouter au columnstore.  
  
## <a name="metadata"></a>Métadonnées  
Toutes les colonnes dans un index columnstore sont stockées dans les métadonnées en tant que colonnes incluses. L'index columnstore n'a pas de colonnes clés.  

:::row:::
    :::column:::
        [sys.indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.index_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-index-columns-transact-sql.md)
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        [sys.partitions &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-partitions-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.internal_partitions &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-internal-partitions-transact-sql.md)
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        [sys.column_store_segments &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-column-store-segments-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.column_store_dictionaries &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-column-store-dictionaries-transact-sql.md)
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        [sys.column_store_row_groups &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-column-store-row-groups-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.dm_db_column_store_row_group_operational_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-column-store-row-group-operational-stats-transact-sql.md)
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        [sys.dm_db_column_store_row_group_physical_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-column-store-row-group-physical-stats-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.dm_column_store_object_pool &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-column-store-object-pool-transact-sql.md)
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        [sys.dm_db_column_store_row_group_operational_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-column-store-row-group-operational-stats-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.dm_db_index_operational_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-operational-stats-transact-sql.md)
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        [sys.dm_db_index_physical_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-physical-stats-transact-sql.md)
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::
 

## <a name="related-tasks"></a>Tâches associées  
Toutes les tables relationnelles, sauf si vous les spécifiez en tant qu’index cluster columnstore, utilisent rowstore comme format de données sous-jacent. `CREATE TABLE` crée une table rowstore, sauf si vous spécifiez l’option `WITH CLUSTERED COLUMNSTORE INDEX`.  
  
Quand vous créez une table avec l’instruction `CREATE TABLE`, vous pouvez en faire un columnstore en spécifiant l’option `WITH CLUSTERED COLUMNSTORE INDEX`. Si vous avez déjà une table rowstore et que vous souhaitez la convertir au format columnstore, utilisez l’instruction `CREATE COLUMNSTORE INDEX`.  
  
|Tâche|Rubriques de référence|Notes|  
|----------|----------------------|-----------|  
|Créer une table sous forme de columnstore|[CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)|Depuis [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], vous pouvez créer la table en tant qu’index cluster columnstore. Il est inutile de créer au préalable une table rowstore pour la convertir ensuite en columnstore.|  
|Créer une table mémoire avec un index columnstore.|[CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)|Depuis [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], vous pouvez créer une table optimisée en mémoire avec un index columnstore. L’index columnstore peut également être ajouté après la création de la table, suivant la syntaxe `ALTER TABLE ADD INDEX`.|  
|Convertir une table rowstore en table columnstore|[CREATE COLUMNSTORE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-columnstore-index-transact-sql.md)|Convertissez un segment de mémoire ou un arbre binaire existant en columnstore. Les exemples montrent comment gérer les index existants, ainsi que le nom de l’index lors de cette conversion.|  
|Convertir une table columnstore en rowstore|[CREATE CLUSTERED INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-columnstore-index-transact-sql.md#d-convert-a-columnstore-table-to-a-rowstore-table-with-a-clustered-index) ou [Reconvertir une table columnstore en segment rowstore](../../t-sql/statements/create-columnstore-index-transact-sql.md#e-convert-a-columnstore-table-back-to-a-rowstore-heap) |Cette conversion n’est généralement pas nécessaire, mais le cas peut se présenter. Les exemples montrent comment convertir un columnstore en segment de mémoire ou index cluster.|  
|Créer un index columnstore sur une table rowstore|[CREATE COLUMNSTORE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-columnstore-index-transact-sql.md)|Une table rowstore ne peut avoir qu’un seul index columnstore. Depuis [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], l’index columnstore peut avoir une condition de filtrage. Les exemples affichent la syntaxe de base.|  
|Créer des index performants pour l’analytique opérationnelle|[Bien démarrer avec columnstore pour l’analytique opérationnelle en temps réel](../../relational-databases/indexes/get-started-with-columnstore-for-real-time-operational-analytics.md)|Explique comment créer des index columnstore et btree complémentaires afin que les requêtes OLTP utilisent des index btree et les requêtes analytiques des index columnstore.|  
|Créer des index columnstore performants pour l’entreposage des données|[Index columnstore pour l’entreposage des données](~/relational-databases/indexes/columnstore-indexes-data-warehouse.md)|Décrit l’utilisation des index btree sur les tables columnstore pour créer des requêtes performantes en matière d’entreposage des données.|  
|Utiliser un index btree pour appliquer une contrainte de clé primaire sur un index columnstore|[Index columnstore pour l’entreposage des données](~/relational-databases/indexes/columnstore-indexes-data-warehouse.md)|Montre comment combiner des index btree et columnstore pour appliquer des contraintes de clé primaire sur l’index columnstore.|  
|Supprimer un index columnstore|[DROP INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/drop-index-transact-sql.md)|La suppression d’un index columnstore utilise la syntaxe `DROP INDEX` standard des index btree. La suppression d’un index columnstore cluster convertit la table columnstore en segment de mémoire.|  
|Supprimer une ligne d’un index columnstore|[DELETE &#40;Transact-SQL&#41;](../../t-sql/statements/delete-transact-sql.md)|Utilisez [DELETE &#40;Transact-SQL&#41;](../../t-sql/statements/delete-transact-sql.md) pour supprimer une ligne.<br /><br /> **Ligne columnstore** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] marque la ligne comme étant supprimée logiquement, mais ne récupère pas le stockage physique associé tant que l’index n’est pas régénéré.<br /><br /> **Ligne deltastore** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] supprime la ligne logiquement et physiquement.|  
|Mettre à jour une ligne dans l’index columnstore|[UPDATE &#40;Transact-SQL&#41;](../../t-sql/queries/update-transact-sql.md)|Utilisez [UPDATE &#40;Transact-SQL&#41;](../../t-sql/queries/update-transact-sql.md) pour mettre à jour une ligne.<br /><br /> **Ligne columnstore** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] marque la ligne comme étant supprimée logiquement, puis insère la ligne mise à jour dans le deltastore.<br /><br /> **Ligne deltastore** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] met à jour la ligne dans le deltastore.|  
|Charger des données dans un index columnstore|[Chargement de données d’index columnstore](~/relational-databases/indexes/columnstore-indexes-data-loading-guidance.md)||  
|Obliger toutes les lignes du deltastore à aller dans le columnstore.|[ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md)... `REBUILD`<br /><br /> [Réorganiser et reconstruire des index](../../relational-databases/indexes/reorganize-and-rebuild-indexes.md)|`ALTER INDEX` avec l’option `REBUILD` oblige toutes les lignes à aller dans le columnstore.|  
|Défragmenter un index columnstore|[ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md)|`ALTER INDEX ... REORGANIZE` défragmente les index columnstore en ligne.|  
|Fusionner des tables avec les index columnstore|[MERGE &#40;Transact-SQL&#41;](../../t-sql/statements/merge-transact-sql.md)||  
  
## <a name="see-also"></a>Voir aussi  
 [Chargement de données d’index columnstore](~/relational-databases/indexes/columnstore-indexes-data-loading-guidance.md)   
 [Synthèse des fonctionnalités des index columnstore en fonction des versions](~/relational-databases/indexes/columnstore-indexes-what-s-new.md)   
 [Performances des requêtes des index columnstore](~/relational-databases/indexes/columnstore-indexes-query-performance.md)   
 [Bien démarrer avec columnstore pour l’analytique opérationnelle en temps réel](../../relational-databases/indexes/get-started-with-columnstore-for-real-time-operational-analytics.md)   
 [Index columnstore pour l’entreposage des données](~/relational-databases/indexes/columnstore-indexes-data-warehouse.md)   
 [Défragmentation d’index columnstore](~/relational-databases/indexes/columnstore-indexes-defragmentation.md)   
 [Guide de conception d’index SQL Server](../../relational-databases/sql-server-index-design-guide.md)   
 [Architecture des index columnstore](../../relational-databases/sql-server-index-design-guide.md#columnstore_index)   
  
  
