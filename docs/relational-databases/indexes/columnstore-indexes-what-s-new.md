---
description: Index columnstore - Nouveautés
title: Index columnStore - Nouveautés | Microsoft Docs
ms.custom: ''
ms.date: 05/11/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: a9fc87cef0244afc4459c789f94855c0c0530ddd
ms.sourcegitcommit: e5664d20ed507a6f1b5e8ae7429a172a427b066c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/19/2020
ms.locfileid: "97697120"
---
# <a name="columnstore-indexes---what39s-new"></a>Index columnstore - Nouveautés
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Récapitulatif des fonctionnalités columnstore disponibles pour chaque version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et pour les dernières versions de [!INCLUDE[ssSDS](../../includes/sssds-md.md)], [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)].  

 > [!NOTE]
 > Pour [!INCLUDE[ssSDS](../../includes/sssds-md.md)], les index columnstore sont disponibles dans les niveaux Premium, Standard (S3 et ultérieur) et dans tous les niveaux vCore de [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]. Pour [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] SP1 et ultérieur, les index columnstore sont disponibles dans toutes les éditions. Pour [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] (avant SP1), les index columnstore sont disponibles seulement dans l’édition Entreprise.
 
## <a name="feature-summary-for-product-releases"></a>Synthèse des fonctionnalités pour les versions du produit  
 Ce tableau récapitule les principales fonctionnalités des index columnstore et des produits dans lesquels ils sont disponibles.  

|Fonctionnalité d’index columnstore|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]|[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]|[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)]|[!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]|[!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)]|[!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)]|[!INCLUDE[ssSDW](../../includes/sssdw-md.md)]|  
|-------------------------------|---------------------------|---------------------------|---------------------------|---------------------------|--------------------------------------------|-------------------------|---|  
|Exécution en mode batch pour les requêtes multithread|Oui|Oui|Oui|Oui|Oui|Oui|Oui| 
|Exécution en mode batch pour les requêtes monothread|||Oui|Oui|Oui|Oui|Oui|  
|Option de compression de l’archivage||Oui|Oui|Oui|Oui|Oui|Oui|  
|Isolement de capture instantanée et isolement de capture instantanée Read Committed|||Oui|Oui|Oui|Oui|Oui| 
|Spécifier l’index columnstore lors de la création d’une table|||Oui|Oui|Oui|Oui|Oui|  
|Always On prend en charge les index columnstore|Oui|Oui|Oui|Oui|Oui|Oui|Oui| 
|Le secondaire accessible en lecture Always On prend en charge les index columnstore non-cluster en lecture seule|Oui|Oui|Oui|Oui|Oui|Oui|Oui|  
|Le secondaire accessible en lecture Always On prend en charge les index columnstore actualisables|||Oui||Oui|||  
|Index columnstore non-cluster en lecture seule sur segment de mémoire ou arbre B (B-tree)|Oui|Oui|oui <sup>1</sup>|oui <sup>1</sup>|oui <sup>1</sup>|oui <sup>1</sup>|oui <sup>1</sup>|  
|Index columnstore non-cluster actualisable sur segment de mémoire ou arbre B (B-tree)|||Oui|Oui|Oui|Oui|Oui|  
|Index B-tree supplémentaires autorisés sur un segment de mémoire ou arbre B (B-tree) ayant un index columnstore non-cluster|Oui|Oui|Oui|Oui|Oui|Oui|Oui|  
|Index columnstore cluster actualisable||Oui|Oui|Oui||Oui|Oui|  
|Index B-tree sur un index columnstore cluster|||Oui|Oui||Oui|Oui|  
|Index columnstore sur une table optimisée en mémoire|||Oui|Oui||Oui|Oui|  
|Définition d’index columnstore non-cluster prenant en charge l’utilisation d’une condition filtrée|||Oui|Oui|Oui|Oui|Oui|  
|Option de temporisation de la compression pour les index columnstore dans `CREATE TABLE` et `ALTER TABLE`|||Oui|Oui|Oui|Oui|Oui|
|Un index columnstore peut avoir une colonne calculée non persistante||||Oui|Oui|||   
|Prise en charge de la fusion en arrière-plan du moteur de tuple||||||Oui|Oui|Oui|
  
 <sup>1</sup> Pour créer un index columnstore non-cluster en lecture seule, stockez l’index sur un groupe de fichiers en lecture seule.  
 
> [!NOTE]
> Le degré de parallélisme (DOP) pour les opérations [en mode batch](../../relational-databases/query-processing-architecture-guide.md#batch-mode-execution) est limité à 2 pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Édition standard, et à 1 pour les éditions [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Web et Express. Ceci fait référence aux index columnstore créés sur des tables basées sur des disques et des tables à mémoire optimisée.

## [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] 
 [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] ajoute ces nouvelles fonctionnalités.

### <a name="functional"></a>Fonctionnelle
- À partir de [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)], le moteur de tuple est aidé par une tâche de fusion en arrière-plan qui compresse automatiquement les rowgroups delta OPEN plus petits qui existent depuis un certain temps, tel que déterminé par un seuil interne, ou qui fusionne les rowgroups COMPRESSED à partir desquels un grand nombre de lignes a été supprimé. Auparavant, une opération de réorganisation d’index était nécessaire pour fusionner les rowgroups avec des données partiellement supprimées. Cela améliore la qualité de l’index columnstore dans le temps. 

## [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] 
 [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] ajoute ces nouvelles fonctionnalités.

### <a name="functional"></a>Fonctionnelle
- [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] prend en charge les colonnes calculées non persistantes dans les index columnstore cluster. Les colonnes calculées non persistantes ne sont pas prises en charge dans les index columnstore cluster. Vous ne pouvez pas créer un index non-cluster sur un index columnstore qui comporte une colonne calculée. 

## [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)]  
 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] ajoute des améliorations clés pour optimiser les performances et la flexibilité des index columnstore. Ces améliorations touchent les scénarios d’entreposage de données et permettent l’analytique opérationnelle en temps réel.  
  
### <a name="functional"></a>Fonctionnelle  
  
-   Une table rowstore peut avoir un index columnstore non cluster actualisable. Auparavant, l’index columnstore non cluster était en lecture seule.  
  
-   La définition d’index columnstore non cluster prend en charge l’utilisation d’une condition filtrée. Pour minimiser l’impact sur les performances de l’ajout d’un index columnstore sur une table OLTP, utilisez une condition filtrée pour créer un index columnstore non cluster uniquement sur les données brutes de votre charge de travail opérationnelle. 
  
-   Une table en mémoire peut avoir un index columnstore. Vous pouvez le créer lors de la création de la table ou l’ajouter ultérieurement avec [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md). Auparavant, une table sur disque pouvait avoir un index columnstore.  
  
-   Un index columnstore cluster peut avoir un ou plusieurs index rowstore non cluster. Auparavant, l’index columnstore ne prenait pas en charge les index non cluster. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] gère automatiquement les index non-cluster pour les opérations DML.  
  
-   Prise en charge des clés primaires et étrangères à l’aide d’un index B-tree pour appliquer ces contraintes sur un index columnstore cluster.  
  
-   Les index columnstore ont une option de délai de compression qui réduit l’impact de la charge de travail transactionnelle sur l’analytique opérationnelle en temps réel.  Cette option permet de modifier fréquemment les lignes pour les stabiliser avant leur compression dans le columnstore. Pour plus d’informations, consultez [CREATE COLUMNSTORE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-columnstore-index-transact-sql.md) et [Bien démarrer avec columnstore pour l’analytique opérationnelle en temps réel](../../relational-databases/indexes/get-started-with-columnstore-for-real-time-operational-analytics.md).  
  
### <a name="performance-for-database-compatibility-level-120-or-130"></a>Performances pour les niveaux de compatibilité de base de données 120 ou 130  
  
-   Les index columnstore prennent en charge l’isolement de capture instantanée Read Committed (RCSI) et l’isolement de capture instantanée (SI). Cela permet d’effectuer des requêtes analytiques cohérentes d’un point de vue transactionnel sans verrou.  
  
-   L’index columnstore prend en charge la défragmentation de l’index en éliminant les lignes supprimées sans nécessité de reconstruire explicitement l’index. L’instruction `ALTER INDEX ... REORGANIZE` élimine les lignes supprimées de l’index columnstore selon une stratégie définie en interne, comme une opération en ligne  
  
-   Les index columnstore sont accessibles sur un réplica secondaire lisible Always On. Vous pouvez améliorer les performances de l’analytique opérationnelle en déchargeant les requêtes analytiques sur un réplica secondaire Always On.  
  
-   Le pushdown d’agrégation calcule les fonctions d’agrégation `MIN`, `MAX`, `SUM`, `COUNT` et `AVG` pendant les analyses de table quand le type de données n’utilise pas plus de 8 octets et n’a pas un type de données chaîne. Le pushdown d’agrégation est pris en charge avec ou sans la clause `GROUP BY` pour les index columnstore cluster et non-cluster. Sur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], cette amélioration est réservée à l’édition Enterprise.
  
-   Le pushdown de prédicats de chaîne accélère les requêtes qui comparent des chaînes de type VARCHAR/CHAR ou NVARCHAR/NCHAR. Cela s’applique aux opérateurs de comparaison courants, et inclut des opérateurs comme `LIKE` qui utilisent des filtres bitmap. Ceci fonctionne avec tous les classements pris en charge. Sur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], cette amélioration est réservée à l’édition Enterprise. 

-   Améliorations des opérations en mode batch en tirant parti des fonctionnalités matérielles basées sur des vecteurs. [!INCLUDE[ssde_md](../../includes/ssde_md.md)] détecte le niveau de prise en charge du processeur pour les extensions matérielles AVX 2 (Advanced Vector Extensions) et SSE 4 (Streaming SIMD Extensions 4), et les utilise si elles sont prises en charge. Sur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], cette amélioration est réservée à l’édition Enterprise.
  
### <a name="performance-for-database-compatibility-level-130"></a>Performances pour le niveau de compatibilité de base de données 130  
  
-   Nouvelle prise en charge de l’exécution en mode batch pour les requêtes utilisant l’une des opérations suivantes :  
    -   `SORT`  
    -   Agrégats avec plusieurs fonctions distinctes. Quelques exemples : `COUNT/COUNT`, `AVG/SUM`, `CHECKSUM_AGG`, `STDEV/STDEVP`  
    -   Fonctions d’agrégation de fenêtre : `COUNT`, `COUNT_BIG`, `SUM`, `AVG`, `MIN`, `MAX` et `CLR`  
    -   Agrégats de fenêtre définis par l’utilisateur : `CHECKSUM_AGG`, `STDEV`, `STDEVP`, `VAR`, `VARP` et `GROUPING`  
    -   Fonctions analytiques d’agrégation de fenêtre : `LAG`, `LEAD`, `FIRST_VALUE`, `LAST_VALUE`, `PERCENTILE_CONT`, `PERCENTILE_DISC`, `CUME_DIST` et `PERCENT_RANK`  

-   Les requêtes à thread unique s’exécutant sous `MAXDOP 1` ou avec un plan de requête série s’exécutent en mode batch. Auparavant, seules les requêtes multithread s’exécutaient en mode batch.  

-   Les requêtes de table à mémoire optimisée peuvent avoir des plans parallèles en mode SQL InterOp lors de l’accès aux données dans un index rowstore ou un index columnstore.
  
### <a name="supportability"></a>Prise en charge  
Ces vues système sont nouvelles pour columnstore :  
  
:::row:::
    :::column:::
        [sys.column_store_row_groups &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-column-store-row-groups-transact-sql.md)
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
        [sys.dm_db_column_store_row_group_physical_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-column-store-row-group-physical-stats-transact-sql.md)
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        [sys.dm_db_index_operational_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-operational-stats-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.dm_db_index_physical_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-physical-stats-transact-sql.md)
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        [sys.internal_partitions &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-internal-partitions-transact-sql.md)
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::
  
Ces DMV basées sur OLTP en mémoire contiennent des mises à jour pour columnstore :  

:::row:::
    :::column:::
        [sys.dm_db_xtp_hash_index_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-xtp-hash-index-stats-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.dm_db_xtp_index_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-xtp-index-stats-transact-sql.md)
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        [sys.dm_db_xtp_memory_consumers &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-xtp-memory-consumers-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.dm_db_xtp_nonclustered_index_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-xtp-nonclustered-index-stats-transact-sql.md)
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        [sys.dm_db_xtp_object_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-xtp-object-stats-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.dm_db_xtp_table_memory_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-xtp-table-memory-stats-transact-sql.md)
    :::column-end:::
:::row-end:::

### <a name="limitations"></a>Limites    
-   Pour les tables en mémoire, un index columnstore doit inclure toutes les colonnes. L’index columnstore ne peut pas avoir de condition filtrée.  
-   Pour les tables en mémoire, les requêtes sur les index columnstore s’exécutent uniquement en mode InterOP et non en mode natif en mémoire. L’exécution en parallèle est prise en charge.  
  
## [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]  
 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] a introduit l’index columnstore cluster en tant que format de stockage principal. Cela autorise des charges régulières ainsi que des opérations de mise à jour, de suppression et d’insertion.  
  
-   La table peut utiliser un index columnstore cluster en tant que stockage de table primaire. Aucun autre index n’est autorisé sur la table mais, l’index columnstore cluster étant actualisable, vous pouvez effectuer des chargements réguliers et apporter des modifications à des lignes individuelles.  
-   L’index columnstore non-cluster conserve les mêmes fonctionnalités que dans [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], à l’exception des opérateurs supplémentaires qui peuvent désormais être exécutés en mode batch. Il n’est toujours pas actualisable, sauf par reconstruction et par basculement de partition. L’index columnstore non cluster est pris en charge uniquement sur les tables sur disque, pas sur les tables en mémoire.  
-   Les index columnstore cluster et non cluster disposent d’une option de compression d’archivage qui compresse davantage les données. L’option d’archivage est utile pour réduire la taille des données en mémoire et sur disque, mais elle ralentit les performances des requêtes. Elle fonctionne bien pour les données rarement utilisées.  
-   Les index columnstore cluster et non cluster fonctionnent d’une manière très similaire. Ils utilisent le même format de stockage en colonnes, le même moteur de traitement des requêtes et le même jeu de vues de gestion dynamique. La différence a trait aux types d’index (primaire et secondaire), et au fait que l’index columnstore non cluster est en lecture seule.  
-   Les opérateurs suivants s’exécutent en mode batch pour les requêtes multithread : scan, filter, project, join, group by et union all.  
  
## [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]  
 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] a introduit l’index columnstore non-cluster comme autre type d’index sur les tables rowstore, et le traitement en mode batch pour les requêtes sur des données columnstore.  
  
-   Une table rowstore peut avoir un index columnstore non cluster.  
-   L’index columnstore est en lecture seule. Après avoir créé l’index columnstore, vous ne pouvez pas mettre à jour la table à l’aide d’opérations `INSERT`, `DELETE` et `UPDATE`. Pour effectuer ces opérations, vous devez supprimer l’index, mettre à jour la table, puis regénérer l’index columnstore. Vous pouvez charger des données supplémentaires dans la table à l’aide d’un basculement de partition. L’avantage du basculement de partition est que vous pouvez charger des données sans devoir supprimer et reconstruire l’index columnstore.  
-   L’index columnstore requiert toujours un stockage supplémentaire, généralement supérieur de 10 % à celui du rowstore, car il stocke une copie des données.  
-   Le traitement par lots offre des performances de requête au moins deux fois supérieures, mais il est disponible uniquement pour l’exécution de requêtes parallèles.  
  
## <a name="see-also"></a>Voir aussi  
 [Index columnstore - Guide de conception](../../relational-databases/indexes/columnstore-indexes-design-guidance.md)   
 [Index columnstore - Conseils en matière de chargement de données](../../relational-databases/indexes/columnstore-indexes-data-loading-guidance.md)   
 [Performances des requêtes d’index columnstore](../../relational-databases/indexes/columnstore-indexes-query-performance.md)   
 [Bien démarrer avec columnstore pour l’analytique opérationnelle en temps réel](../../relational-databases/indexes/get-started-with-columnstore-for-real-time-operational-analytics.md)   
 [Index columnstore pour l’entreposage des données](../../relational-databases/indexes/columnstore-indexes-data-warehouse.md)   
 [Réorganiser et reconstruire des index](../../relational-databases/indexes/reorganize-and-rebuild-indexes.md)
  
