---
title: Guide de conception et d’architecture d’index SQL Server | Microsoft Docs
description: Découvrez la conception d’index efficaces dans SQL Server pour que les bases de données et des applications fonctionnent bien. Lisez-en plus sur l’architecture d’index et les meilleures pratiques.
ms.custom: ''
ms.date: 01/19/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
helpviewer_keywords:
- index design guide
- index design guidance
- guide, index design
- guidance, index design
- index internals
- index architecture
- sql server index internals
- sql server index architecture
- sql server index design guide
- sql server index design guidance
ms.assetid: 11f8017e-5bc3-4bab-8060-c16282cfbac1
author: rothja
ms.author: jroth
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 57eb0bac3a794aaf3b7f84fc8cfb14d0207da1ae
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/01/2021
ms.locfileid: "99233253"
---
# <a name="sql-server-index-architecture-and-design-guide"></a>Guide de conception et d’architecture d’index SQL Server
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

L'engorgement des applications de base de données est souvent imputable à des index mal conçus ou en nombre insuffisant. La conception d'index efficaces est primordiale pour le bon fonctionnement des bases de données et des applications. Ce guide de conception d’index [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] contient des informations sur l’architecture des index, ainsi que des bonnes pratiques permettant de créer des index performants et adaptés aux besoins de votre application.  
    
Ce guide suppose que le lecteur connaît les types d'index disponibles dans [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Pour obtenir description générale des types d'index, consultez [Types d'index](../relational-databases/indexes/indexes.md).  

Ce guide couvre les types d’index suivants :

-   Cluster
-   Non-cluster
-   Unique
-   Filtré
-   columnstore
-   Hachage
-   Non-cluster à mémoire optimisée

Pour plus d’informations sur les index XML, consultez [Vue d’ensemble des index XML](../relational-databases/xml/xml-indexes-sql-server.md).

Pour plus d’informations sur les index spatiaux, consultez [Vue d’ensemble des index spatiaux](../relational-databases/spatial/spatial-indexes-overview.md).

Pour plus d’informations sur les index de recherche en texte intégral, consultez [Alimenter des index de recherche en texte intégral](../relational-databases/search/populate-full-text-indexes.md).
  
##  <a name="index-design-basics"></a><a name="Basics"></a> Notions de base de la conception d'index  
 Pensez à un livre ordinaire : à la fin du livre, il existe un index qui permet de rechercher rapidement les informations dans le livre. L’index est une liste triée de mots clés, chacun accompagné d’un ensemble de numéros de page pointant vers les pages où il se trouve. Un index SQL Server n’est pas différent : il s’agit d’une liste triée de valeurs et, pour chaque valeur, il existe des pointeurs vers les pages de [données](../relational-databases/pages-and-extents-architecture-guide.md) où ces valeurs se trouvent. L’index lui-même est stocké dans les pages, constituant ainsi les pages d’index dans SQL Server. Dans un livre ordinaire, si l’index s’étend sur plusieurs pages et que vous devez rechercher des pointeurs vers toutes les pages qui contiennent le mot « SQL », par exemple, vous devez feuilleter jusqu’à ce que vous trouviez la page d’index qui contient le mot clé « SQL ». À partir de là, vous suivez les pointeurs vers toutes les pages du livre.  Cela peut être optimisé si, au tout début de l’index, vous créez une page unique qui contient une liste alphabétique de chaque lettre. Par exemple : « A à D -  page 121 », « E à G - page 122 »,  et ainsi de suite. Cette page supplémentaire évite de feuilleter l’index pour rechercher le point de départ. Cette page n’existe pas dans les livres ordinaires, mais elle existe dans un index SQL Server. Cette page unique est appelée page racine de l’index. La page racine est la page de démarrage de l’arborescence utilisée par un index SQL Server. En suivant l’analogie de l’arborescence, les pages de fin qui contiennent des pointeurs vers les données réelles sont appelées « pages de feuille » de l’arborescence. 

 Un index SQL Server est une structure sur disque ou en mémoire associée à une table ou à une vue qui accélère l’extraction des lignes de la table ou de la vue. Il contient des clés créées à partir d'une ou plusieurs colonnes de la table ou de la vue. Pour les index sur disque, ces clés sont stockées dans une structure arborescente (B-tree) qui permet à SQL Server de trouver rapidement et efficacement la ou les lignes associées aux valeurs de clé.  

 Un index stocke des données organisées logiquement dans une table composée de lignes et de colonnes, et stockées physiquement dans un format de données en ligne appelé *rowstore* <sup>1</sup> ou dans un format de données en colonne appelé *[columnstore](#columnstore_index)* .  
    
 Le choix d'index adaptés à une base de données et à sa charge de travail est une opération complexe qui vise à trouver un compromis entre vitesse des requêtes et coûts de mise à jour. Les index étroits, c'est-à-dire les index ne comportant que quelques colonnes dans la clé d'index, requièrent moins d'espace disque et de besoins de maintenance. En revanche, les index larges couvrent plus de requêtes. Vous devrez éventuellement essayer plusieurs conceptions différentes avant de trouver l'index le plus performant. Il est possible d'ajouter, de modifier et de supprimer des index sans affecter le schéma de la base de données ou la conception des applications. Par conséquent, n'hésitez à faire des essais avec différents index.  
  
 Dans la majorité des cas, l'optimiseur de requête de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] choisit de manière fiable l'index le plus efficace. La stratégie globale de création d'index consiste à fournir à l'optimiseur de requête une sélection variée d'index et à se fier à lui pour faire le bon choix. Ce procédé permet de réduire le temps d'analyse et produit de bons résultats dans bon nombre de cas. Pour déterminer quels sont les index qu'utilise l'optimiseur de requête dans le cas d'une requête donnée, sélectionnez [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)]Inclure le plan d'exécution réel **dans le menu** Requête **de**.  
  
 L'utilisation d'index n'est pas forcément synonyme de bonnes performances, et inversement, de bonnes performances ne sauraient être nécessairement attribuables à l'utilisation d'index efficaces. Si l'utilisation d'un index contribuait toujours à produire les meilleurs résultats, le travail de l'optimiseur de requête en serait simplifié. En réalité, le choix d'un index inapproprié peut aboutir à des performances moins que satisfaisantes. La tâche de l'optimiseur de requête est donc de ne sélectionner un index, ou une combinaison d'index, que dans les cas où cette sélection est susceptible d'améliorer les performances et d'éviter la récupération par index si elle doit les détériorer.  

 <sup>1</sup> Rowstore est la méthode standard de stockage des données de table relationnelles. Dans [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], un rowstore fait référence à une table dans laquelle le format de stockage de données sous-jacent est un segment de mémoire, un arbre B-tree ([index cluster](#Clustered)) ou une table à mémoire optimisée.

### <a name="index-design-tasks"></a>Tâches de conception d'index  
 La stratégie de conception d’index que nous recommandons est constituée des tâches suivantes :  
  
1.  Comprendre les caractéristiques de la base de données elle-même. 
    * Par exemple, s’agit-il d’une base de données de traitement transactionnel en ligne (OLTP) dont les données sont souvent modifiées et devant garantir un débit élevé. À partir de [!INCLUDE[ssSQL14](../includes/sssql14-md.md)], les index et tables à mémoire optimisée sont particulièrement bien adaptés pour ce scénario, car ils offrent une structure sans verrous. Pour plus d’informations, consultez [Index pour les tables à mémoire optimisée](../relational-databases/in-memory-oltp/indexes-for-memory-optimized-tables.md), ou [Indications pour la conception d’index non-cluster pour les tables à mémoire optimisée](#inmem_nonclustered_index) et [Indications pour la conception d’index de hachage pour les tables à mémoire optimisée](#hash_index) dans ce guide.
    * Ou s’agit-il d’un exemple de base de données de support de décision (DSS) ou d’entreposage de données (OLAP) devant traiter rapidement des jeux de données très volumineux ? À partir de [!INCLUDE[ssSQL11](../includes/sssql11-md.md)], les index columnstore sont particulièrement bien adaptés pour les jeux de données d’entreposage de données standard. Les index columnstore peuvent transformer l'expérience utilisateur des entrepôts de données en améliorant considérablement les performances des requêtes communes liées aux entrepôts de données, par exemple en matière de filtrage, d'agrégation, de regroupement et de jointure en étoile. Pour plus d’informations, consultez [Index columnstore - Présentation](../relational-databases/indexes/columnstore-indexes-overview.md) et [Indications pour la conception d’index columnstore](#columnstore_index) dans ce guide.  

2.  Comprendre les caractéristiques des requêtes les plus fréquemment utilisées. Par exemple, si vous savez qu'une requête fréquemment utilisée crée une jointure entre deux tables ou plus, vous serez plus à même de choisir le type d'index le mieux adapté.  
  
3.  Comprendre les caractéristiques des colonnes utilisées dans les requêtes. Par exemple, un index s'avère idéal pour les colonnes associées à des données de type integer et qui sont également uniques ou n'acceptent pas les valeurs NULL. Pour les colonnes qui ont des sous-ensembles de données bien définis, utilisez un index filtré dans [!INCLUDE[ssKatmai](../includes/sskatmai-md.md)] et versions ultérieures. Pour plus d'informations, consultez [Instructions de conception d'index filtrés](#Filtered) dans ce guide.  
  
4.  Identifier les options d'index qui peuvent améliorer les performances au moment de la création ou de la maintenance de l'index. Par exemple, si vous créez un index cluster dans une table volumineuse existante, vous avez tout intérêt à utiliser l’option d’index `ONLINE`. Cette option permet en effet la poursuite des activités concurrentes sur les données sous-jacentes pendant la création ou la reconstruction de l'index. Pour plus d’informations, consultez [Définir les options d’index](../relational-databases/indexes/set-index-options.md).  
  
5.  Déterminer l'emplacement de stockage optimal pour l'index. Un index non-cluster peut être stocké dans le même groupe de fichiers que celui auquel appartient la table sous-jacente, ou dans un groupe de fichiers distinct. L'emplacement de stockage des index peut améliorer les performances des requêtes par l'amélioration des performances d'E/S des disques. Par exemple, en stockant un index non-cluster dans un groupe de fichiers résidant sur un disque différent de celui du groupe de fichiers de la table, vous pouvez améliorer les performances, car plusieurs disques peuvent être lus simultanément.  
     Une autre solution consiste à utiliser un schéma de partition sur plusieurs groupes de fichiers pour les index cluster et non-cluster. Le partitionnement permet une gestion plus simple des tables et index volumineux. Vous pouvez en effet accéder à des sous-ensembles de données ou les gérer de manière rapide et efficace, tout en préservant l'intégrité de la collection globale. Pour plus d’informations, consultez [Tables et index partitionnés](../relational-databases/partitions/partitioned-tables-and-indexes.md). Si vous envisagez de recourir au partitionnement, vous devez déterminer si l'index doit être aligné, c'est-à-dire, partitionné plus ou moins de la même façon que la table, ou s'il doit être partitionné de façon indépendante.   

##  <a name="general-index-design-guidelines"></a><a name="General_Design"></a> Consignes générales pour la création d'index  
 Les administrateurs de bases de données expérimentés peuvent concevoir de bons ensembles d'index, mais cette tâche est très complexe, sujette à erreurs et demande beaucoup de temps, même dans le cas de bases de données et de charges de travail peu complexes. La compréhension des caractéristiques de votre base de données, de vos requêtes et de vos colonnes de données peut vous aider à créer des index optimaux.  
  
### <a name="database-considerations"></a>Remarques sur la base de données  
 Lorsque vous créez un index, prenez en compte les directives suivantes relatives aux bases de données :  
  
-   La définition de nombreux index sur une table affecte les performances des instructions `INSERT`, `UPDATE`, `DELETE` et `MERGE`, car à mesure que les données de la table changent, tous les index doivent être mis à jour en conséquence. Par exemple, si une colonne est utilisée dans plusieurs index et que vous exécutez une instruction `UPDATE` qui modifie les données de cette colonne, chaque index contenant cette colonne doit être mis à jour, ainsi que la colonne de la table de base sous-jacente (segment de mémoire ou index cluster).  
  
    -   Évitez que les tables mises à jour ne soient trop abondamment indexées et faites en sorte que les index soient étroits, c'est-à-dire qu'ils comprennent le moins de colonnes possible.  
  
    -   Utilisez de nombreux index pour améliorer les performances des requêtes sur les tables possédant des besoins réduits en matière de mise à jour, mais de grands volumes de données. Un grand nombre d'index peut améliorer les performances des requêtes qui ne modifient pas les données (instructions SELECT), car l'optimiseur de requête dispose d'un choix d'index plus vaste pour déterminer la méthode d'accès la plus rapide.  
  
-   Il n'est peut-être pas idéal d'indexer des tables de taille réduite, car le temps nécessaire à l'optimiseur de requête pour parcourir l'index à la recherche de données peut être supérieur à la durée d'une simple analyse de la table. Par conséquent, les index de petites tables peuvent ne jamais être utilisés, mais doivent néanmoins être gérés, car les données de la table changent.  
  
-   Les index de vues peuvent vous permettre d'améliorer considérablement les performances lorsque la vue contient des agrégations, des jointures de tables ou une combinaison d'agrégations et de jointures. La vue ne doit pas être explicitement référencée dans la requête pour que l'optimiseur de requête puisse l'utiliser.  
  
-   Servez-vous de l'Assistant Paramétrage du moteur de base de données pour analyser votre base de données et obtenir des recommandations sur les index. Pour plus d'informations, consultez [Database Engine Tuning Advisor](../relational-databases/performance/database-engine-tuning-advisor.md).  
  
### <a name="query-considerations"></a>Remarques sur les requêtes  
 Lorsque vous créez un index, prenez en compte les directives suivantes relatives aux requêtes :  
  
-   Créez des index non cluster sur les colonnes fréquemment utilisées dans des prédicats et des conditions de jointure dans des requêtes. Il s’agit de vos colonnes SARGable<sup>1</sup>. Toutefois, évitez d'ajouter des colonnes superflues. L'ajout d'un trop grand nombre de colonnes d'index peut avoir une influence négative sur les performances de gestion des index et de l'espace disque.  
  
-   La couverture des index peut améliorer les performances des requêtes, car toutes les données nécessaires pour répondre aux exigences de la requête existent dans l'index proprement dit. Cela signifie que seules les pages d'index, et non les pages de données de la table ou de l'index cluster, sont nécessaires pour récupérer les données demandées, réduisant ainsi globalement le nombre d'E/S des disques. Par exemple, une requête de colonnes **a** et **b** sur une table possédant un index composite créé sur les colonnes **a**, **b** et **c** peut récupérer les données spécifiées à partir du seul index.  

    > [!IMPORTANT]
    > Un index couvrant désigne un [index non cluster](#nonclustered-index-architecture) qui résout directement un ou plusieurs résultats de requête similaires sans avoir accès à sa table de base ni entraîner de recherches.
    > Ces index ont toutes les colonnes [SARGable](#sargable) nécessaires au niveau feuille. Ainsi, les colonnes retournées par la clause SELECT et tous les arguments WHERE et JOIN sont couvertes par l’index.
    > Il y a potentiellement beaucoup moins E/S pour exécuter la requête si l’index est assez petit par rapport aux lignes et aux colonnes de la table proprement dite, ce qui signifie qu’il s’agit d’un véritable sous-ensemble de la totalité des colonnes. Les index couvrants peuvent être intéressants lorsque seule une petite partie d’une grande table est sélectionnée et qu’elle est définie par un prédicat fixe, comme les [colonnes éparses](../relational-databases/tables/use-sparse-columns.md) qui contiennent seulement quelques valeurs non NULL, par exemple.
    
-   Rédigez des requêtes insérant ou modifiant un maximum de lignes en une seule instruction, plutôt que de recourir à plusieurs requêtes pour mettre à jour les mêmes lignes. De cette façon, la maintenance d'index optimisée peut être exploitée.  
  
-   Évaluez le type de requête et la manière dont les colonnes sont utilisées dans la requête. Par exemple, une colonne utilisée dans un type de requête de correspondance exacte constitue un candidat valable à un index non-cluster ou cluster.

<a name="sargable"></a><sup>1</sup> Le terme SARGable dans les bases de données relationnelles fait référence à un prédicat « **S** earch **ARG** ument-**able** » capable d’exploiter un index pour accélérer l’exécution de la requête.
  
### <a name="column-considerations"></a>Remarques sur les colonnes  
 Lorsque vous créez un index, prenez en compte les directives suivantes relatives aux colonnes :  
  
-   Veillez à ce que la clé d'index des index cluster soit courte. En outre, les index cluster bénéficient du fait d'être créés sur des colonnes uniques ou non NULL.  
  
-   Les colonnes dont le type de données est **ntext**, **text**, **image**, **varchar(max)** , **nvarchar(max)** ou **varbinary(max)** ne peuvent pas être spécifiées en tant que colonnes de clés d’index. Cependant, les types de données **varchar(max)** , **nvarchar(max)** , **varbinary(max)** et **xml** peuvent participer à des index non-cluster en tant que colonnes d’index non-clés. Pour plus d'informations, consultez la section [Index avec colonnes incluses](#Included_Columns)dans ce guide.  
  
-   Un type de données **xml** ne peut être qu'une colonne clé dans un index XML. Pour plus d’informations, consultez [Index XML &#40;SQL Server&#41;](../relational-databases/xml/xml-indexes-sql-server.md). SQL Server 2012 SP1 introduit un nouveau type d'index XML appelé index XML sélectif. Ce nouvel index améliore les performances de requête sur les données stockées en XML dans SQL Server, permettant ainsi d'indexer plus rapidement les charges de travail comportant beaucoup de données XML et améliorant l'évolutivité en réduisant les coûts de stockage de l'index en lui-même. Pour plus d’informations, consultez [Index XML sélectifs &#40;SXI&#41;](../relational-databases/xml/selective-xml-indexes-sxi.md).  
  
-   Vérifiez l'unicité des colonnes. Un index unique plutôt que non unique sur la même combinaison de colonnes procure des informations supplémentaires à l'optimiseur de requête, ce qui améliore l'utilité de l'index. Pour plus d'informations, consultez [Instructions de conception d'index uniques](#Unique) dans ce guide.  
  
-   Examinez la distribution des données dans la colonne. Bien souvent, la longueur d'exécution d'une requête est due à l'indexation d'une colonne comportant peu de valeurs uniques ou à la réalisation d'une jointure sur ce type de colonne. Il s'agit d'un problème crucial pour les données et la requête, que l'on ne peut généralement pas résoudre sans identifier clairement la situation. Un répertoire téléphonique physique, par exemple, trié dans l'ordre alphabétique par nom de famille, ne permettra pas l'identification rapide d'une personne si tous les habitants de la ville se nomment Smith ou Jones. Pour plus d'informations sur la distribution de données, consultez [Statistics](../relational-databases/statistics/statistics.md).  
  
-   Envisagez d'utiliser des index filtrés sur les colonnes qui ont des sous-ensembles bien définis, par exemple les colonnes éparses, les colonnes contenant principalement des valeurs NULL, les colonnes contenant des catégories de valeurs et les colonnes contenant des plages de valeurs distinctes. Un index filtré bien conçu peut améliorer les performances des requêtes et réduire les coûts de maintenance et de stockage.  
  
-   Tenez compte de l'ordre des colonnes si l'index doit en contenir plusieurs. La colonne utilisée dans la clause WHERE au sein d'une condition de recherche de type égal à (=), supérieur à (>), inférieur à (<) ou BETWEEN, ou qui participe à une jointure, doit être insérée en premier. Les colonnes supplémentaires doivent être classées en fonction de leur niveau de différenciation, c'est-à-dire de la plus distincte à la moins distincte.  
  
     Par exemple, si l'index est défini en tant que `LastName`, la valeur `FirstName` de l'index sera utile si la condition de recherche est `WHERE LastName = 'Smith'` ou `WHERE LastName = Smith AND FirstName LIKE 'J%'`. Cependant, l'optimiseur de requête n'utilise pas l'index pour une requête portant uniquement sur `FirstName (WHERE FirstName = 'Jane')`.  
  
-   Pensez à indexer les colonnes calculées. Pour plus d'informations, consultez [Indexes on Computed Columns](../relational-databases/indexes/indexes-on-computed-columns.md).  
  
### <a name="index-characteristics"></a>Caractéristiques des index  
 Après avoir déterminé qu'un index est approprié pour une requête, vous pouvez sélectionner le type d'index qui convient le mieux à la situation. Un index doit posséder les caractéristiques suivantes :  
  
-   être cluster ou non-cluster ;  
-   être unique ou non unique ;  
-   être à une ou plusieurs colonnes ;  
-   être trié par ordre croissant ou décroissant d'après les colonnes qui le constituent ;  
-   table entière plutôt que filtré pour les index non cluster.  
-   être columnstore ou rowstore ;
-   être de type hachage ou non-cluster pour les tables à mémoire optimisée.
  
Vous pouvez également personnaliser les caractéristiques de stockage initiales de l'index afin d'optimiser ses performances ou sa maintenance en définissant une option telle que FILLFACTOR. Vous pouvez également déterminer l'emplacement de stockage de l'index en utilisant des groupes de fichiers ou des schémas de partition pour optimiser les performances.  
  
###  <a name="index-placement-on-filegroups-or-partitions-schemes"></a><a name="Index_placement"></a> Placement d'index sur les groupes de fichiers ou les schémas de partition  
 Lors du développement de votre stratégie de conception des index, vous devez tenir compte du placement de ces index sur les groupes de fichiers associés à la base de données. Une sélection rigoureuse du groupe de fichiers ou du schéma de partition peut améliorer les performances des requêtes.  
  
 Par défaut, les index sont stockés dans le même groupe de fichiers que la table de base sur laquelle est créé l'index. Un index cluster non partitionné et la table de base résident toujours dans le même groupe de fichiers. Toutefois, vous pouvez effectuer les opérations suivantes :  
  
-   créer des index non cluster dans un groupe de fichiers différent de celui de la table de base ou de l'index cluster ;  
-   partitionner des index cluster et non-cluster pour qu'ils concernent plusieurs groupes de fichiers ;  
-   déplacer une table d'un groupe de fichiers à un autre en supprimant l'index cluster et en spécifiant un nouveau groupe de fichiers ou un nouveau schéma de partition dans la clause MOVE TO de l'instruction DROP INDEX ou en utilisant l'instruction CREATE INDEX avec la clause DROP_EXISTING.  
  
Créer l'index non-cluster dans un autre groupe de fichiers permet de réaliser des gains de performances si les groupes de fichiers utilisent des lecteurs physiques différents avec leurs propres contrôleurs. Les informations d'index et les données peuvent alors être lues en parallèle par plusieurs têtes de disques. Par exemple, si la `Table_A` du groupe de fichiers `f1` et l' `Index_A` du groupe de fichiers `f2` sont utilisés par la même requête, des gains de performances sont possibles, car les deux groupes de fichiers sont utilisés totalement sans contention. Mais si la `Table_A` est analysée par la requête et si l' `Index_A` n'est pas référencé, seul le groupe de fichiers `f1` est utilisé, ce qui n'apporte aucun gain de performance.  
  
Comme vous ne pouvez pas prévoir le type d'accès qui se met en place ni le moment de cette mise en place, il peut s'avérer plus judicieux de répartir vos tables et vos index sur tous les groupes de fichiers. Ceci garantit l'accès à tous les disques, car toutes les données et tous les index sont répartis uniformément sur tous les disques, quel que soit le mode d'accès aux données. Cette approche est également plus simple pour les administrateurs système.  
  
#### <a name="partitions-across-multiple-filegroups"></a>Partitions sur plusieurs groupes de fichiers  
 Vous pouvez également envisager de partitionner des index cluster et non-cluster sur plusieurs groupes de fichiers. Les index partitionnés sont partitionnés horizontalement ou par ligne, selon la fonction de partition. La fonction de partition définit le mode de mappage de chaque ligne sur un ensemble de partitions basé sur les valeurs de certaines colonnes, nommées colonnes de partition. Un schéma de partition spécifie le mappage des partitions sur un ensemble de groupe de fichiers.  
  
 Le partitionnement d'un index peut présenter les avantages suivants :  
  
-   Systèmes évolutifs permettant de gérer plus facilement les grands index. Par exemple, les systèmes OLTP peuvent mettre en œuvre des applications sensibles aux partitions qui se chargent des grands index.  
  
-   Exécution plus rapide et plus efficace des requêtes. Lorsque des requêtes accèdent à plusieurs partitions d'un index, l'optimiseur de requête peut traiter plusieurs partitions individuelles en même temps et exclure les partitions qui ne sont pas concernées par la requête.  
  
Pour plus d’informations, consultez [Tables et index partitionnés](../relational-databases/partitions/partitioned-tables-and-indexes.md).  
  
###  <a name="index-sort-order-design-guidelines"></a><a name="Sort_Order"></a> Indications pour la conception de l'ordre de tri des index  
 Lorsque vous définissez des index, vous devez déterminer si les données de la colonne clé d'index doivent être stockées dans l'ordre croissant ou décroissant. L'ordre croissant est l'option par défaut et maintient la compatibilité avec les versions antérieures de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. La syntaxe des instructions CREATE INDEX, CREATE TABLE et ALTER TABLE permet l'application des mot clés ASC (croissant) et DESC (décroissant) à chaque colonne d'un index et d'une contrainte.  
  
 La spécification de l'ordre dans lequel les valeurs de clé sont stockées dans un index est utile lorsque les requêtes référençant la table possèdent des clauses ORDER BY qui définissent différents sens pour la ou les colonnes clés de cet index. Dans ces situations, l'index peut supprimer la nécessité d'un opérateur SORT dans le plan de requête, ce qui rend la requête plus efficace. Par exemple, les acheteurs du service achat de [!INCLUDE[ssSampleDBCoFull](../includes/sssampledbcofull-md.md)] doivent évaluer la qualité des produits qu'ils acquièrent auprès des fournisseurs. Les acheteurs souhaitent notamment rechercher, parmi les produits envoyés par ces fournisseurs, ceux qui affichent un degré de rejet élevé. Comme le montre la requête suivante, l’extraction des données en fonction de ce critère nécessite que la colonne `RejectedQty` de la table `Purchasing.PurchaseOrderDetail` soit triée dans l’ordre décroissant (de la valeur la plus élevée à la valeur la plus faible), et que la colonne `ProductID` soit triée dans l’ordre croissant (de la valeur la plus faible à la valeur la plus élevée).  
  
```sql  
SELECT RejectedQty, ((RejectedQty/OrderQty)*100) AS RejectionRate,  
    ProductID, DueDate  
FROM Purchasing.PurchaseOrderDetail  
ORDER BY RejectedQty DESC, ProductID ASC;  
```  
  
 Le plan d'exécution ci-dessous pour cette requête montre que l'optimiseur de requête a utilisé un opérateur SORT pour retourner l'ensemble de résultats dans l'ordre spécifié par la clause ORDER BY.  
  
 ![Diagramme d’un plan d’exécution pour cette requête montrant que l’optimiseur de requête a utilisé un opérateur SORT pour retourner l’ensemble de résultats dans l’ordre spécifié par la clause ORDER BY.](../relational-databases/media/indexsort1.gif)
  
 Si un index est créé avec les colonnes clés correspondant à celles de la clause ORDER BY de la requête, l'opérateur SORT peut être supprimé du plan de requête, ce qui rend celui-ci plus efficace.  
  
```sql  
CREATE NONCLUSTERED INDEX IX_PurchaseOrderDetail_RejectedQty  
ON Purchasing.PurchaseOrderDetail  
    (RejectedQty DESC, ProductID ASC, DueDate, OrderQty);  
```  
  
 Une fois la requête réexécutée, le plan d'exécution ci-dessous montre que l'opérateur SORT a été supprimé et que l'index non-cluster nouvellement créé est utilisé.  
  
 ![Diagramme d’un plan d’exécution montrant que l’opérateur SORT a été supprimé et que l’index non cluster créé est utilisé.](../relational-databases/media/insertsort2.gif)
  
 Le [!INCLUDE[ssDE](../includes/ssde-md.md)] peut parcourir les données aussi efficacement dans un sens que dans l'autre. Un index défini sous la forme `(RejectedQty DESC, ProductID ASC)` peut néanmoins être utilisé pour une requête dont la clause ORDER BY inverse le sens du tri des colonnes. Par exemple, une requête possédant la clause ORDER BY `ORDER BY RejectedQty ASC, ProductID DESC` peut utiliser l'index.  
  
 L’ordre de tri ne peut être spécifié que pour les colonnes clés dans l’index. L’affichage catalogue [sys.index_columns](../relational-databases/system-catalog-views/sys-index-columns-transact-sql.md) et la fonction INDEXKEY_PROPERTY indiquent si une colonne d’index est stockée dans l’ordre croissant ou décroissant.  

## <a name="metadata"></a>Métadonnées  
Utilisez ces vues de métadonnées pour voir les attributs des index. Des informations supplémentaires sur l’architecture sont incorporées dans certaines de ces vues.

> [!NOTE]
> Pour les index columnstore, toutes les colonnes sont stockées dans les métadonnées sous forme de colonnes incorporées. L'index columnstore n'a pas de colonnes clés.  

:::row:::
    :::column:::
        [sys.indexes &#40;Transact-SQL&#41;](../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.index_columns &#40;Transact-SQL&#41;](../relational-databases/system-catalog-views/sys-index-columns-transact-sql.md)
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        [sys.partitions &#40;Transact-SQL&#41;](../relational-databases/system-catalog-views/sys-partitions-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.internal_partitions &#40;Transact-SQL&#41;](../relational-databases/system-catalog-views/sys-internal-partitions-transact-sql.md)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [sys.dm_db_index_operational_stats &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-db-index-operational-stats-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.dm_db_index_physical_stats &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-db-index-physical-stats-transact-sql.md)
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        [sys.column_store_segments &#40;Transact-SQL&#41;](../relational-databases/system-catalog-views/sys-column-store-segments-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.column_store_dictionaries &#40;Transact-SQL&#41;](../relational-databases/system-catalog-views/sys-column-store-dictionaries-transact-sql.md)
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        [sys.column_store_row_groups &#40;Transact-SQL&#41;](../relational-databases/system-catalog-views/sys-column-store-row-groups-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.dm_db_column_store_row_group_operational_stats &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-db-column-store-row-group-operational-stats-transact-sql.md)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [sys.dm_db_column_store_row_group_physical_stats &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-db-column-store-row-group-physical-stats-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.dm_column_store_object_pool &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-column-store-object-pool-transact-sql.md)
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        [sys.dm_db_column_store_row_group_operational_stats &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-db-column-store-row-group-operational-stats-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.dm_db_xtp_hash_index_stats &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-db-xtp-hash-index-stats-transact-sql.md)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [sys.dm_db_xtp_index_stats &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-db-xtp-index-stats-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.dm_db_xtp_object_stats &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-db-xtp-object-stats-transact-sql.md)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [sys.dm_db_xtp_nonclustered_index_stats &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-db-xtp-nonclustered-index-stats-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.dm_db_xtp_table_memory_stats &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-db-xtp-table-memory-stats-transact-sql.md)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [sys.hash_indexes &#40;Transact-SQL&#41;](../relational-databases/system-catalog-views/sys-hash-indexes-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.memory_optimized_tables_internal_attributes &#40;Transact-SQL&#41;](../relational-databases/system-catalog-views/sys-memory-optimized-tables-internal-attributes-transact-sql.md)
    :::column-end:::
:::row-end:::

##  <a name="clustered-index-design-guidelines"></a><a name="Clustered"></a> Indications pour la conception d'index cluster  
 Les index cluster trient et stockent les lignes de données de la table en fonction de leurs valeurs de clé. Il n'y a qu'un index cluster par table car les lignes de données ne peuvent être triées que dans un seul ordre. À quelques exceptions près, toutes les tables doivent avoir un index cluster défini sur la ou les colonnes présentant les caractéristiques suivantes :  
  
-   utilisables pour les requêtes fréquemment utilisées ;  
  
-   assurant un niveau élevé d'unicité ;  
  
    > [!NOTE]  
    > Lorsque vous créez une contrainte PRIMARY KEY, un index unique sur la ou les colonnes est automatiquement créé. Par défaut, cet index est cluster ; toutefois, vous pouvez spécifier un index non-cluster lorsque vous créez la contrainte.  
  
-   utilisables dans les requêtes de plage.  
  
Si l’index cluster n’est pas créé avec la propriété `UNIQUE`, le [!INCLUDE[ssDE](../includes/ssde-md.md)] ajoute automatiquement une colonne d’indicateur d’unicité de quatre octets à la table. Si nécessaire, le [!INCLUDE[ssDE](../includes/ssde-md.md)] ajoute automatiquement une valeur d'indicateur d'unicité une ligne pour que chaque clé soit unique. Cette colonne et ses valeurs sont utilisées en interne et ne sont ni affichables, ni accessibles par les utilisateurs.  
  
### <a name="clustered-index-architecture"></a>Architecture des index cluster  
 Dans [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], les index sont organisés selon une arborescence binaire (B-tree). Chaque page d'une arborescence binaire d'index s'appelle un nœud d'index. Le nœud supérieur d'une arborescence binaire est le nœud racine. Les nœuds du niveau inférieur de l'index sont appelés les nœuds feuille. Tous les niveaux d'index situés entre la racine et les nœuds feuille s'appellent des niveaux intermédiaires. Dans un index cluster, les nœuds feuille contiennent les pages de données de la table sous-jacente. Les nœuds racine et de niveau intermédiaire contiennent les pages d'index dans lesquelles se trouvent les lignes d'index. Chaque ligne d'index contient une valeur de clé et un pointeur vers une page de niveau intermédiaire dans l'arborescence binaire ou vers une ligne de données dans le niveau feuille de l'index. Les pages de chaque niveau de l'index sont liées dans une liste à double liaison.  
  
 Un index cluster a une ligne dans [sys.partitions](../relational-databases/system-catalog-views/sys-partitions-transact-sql.md), où **index_id** = 1 pour chaque partition utilisée par celui-ci. Par défaut, un index cluster possède une seule partition. Lorsqu'un index cluster détient plusieurs partitions, chacune d'elles possède une structure d'arborescence binaire qui contient ses données. Par exemple, si un index cluster possède quatre partitions, il existe quatre structures d'arborescence binaire, à raison d'une dans chaque partition.  
  
 Suivant les types de données de l'index cluster, chaque structure d'index cluster possède une ou plusieurs unités d'allocation pour le stockage et la gestion des données d'une partition spécifique. Au minimum, chaque index cluster détient une unité d'allocation IN_ROW_DATA par partition. L’index cluster possède également une unité d’allocation *LOB_DATA* par partition s’il contient des colonnes LOB (Large Object). De plus, il a une unité d’allocation *ROW_OVERFLOW_DATA* par partition s’il contient des colonnes de longueur variable qui dépassent la limite de taille de ligne de 8 060 octets.  
  
 Les pages de la chaîne de données et les lignes qu'elles rassemblent sont organisées en fonction de la valeur de la clé d'index cluster. Toutes les insertions sont faites à l'endroit où la valeur de clé de la ligne insérée correspond parfaitement à la séquence de tri parmi les lignes existantes.  
  
 L'illustration suivante montre la structure d'un index cluster dans une partition unique.  
 
 ![Diagramme montrant la structure d’un index cluster dans une partition unique.](../relational-databases/media/bokind2.gif)  
  
### <a name="query-considerations"></a>Remarques sur les requêtes  
 Avant de créer des index cluster, il est important de comprendre le mode d'accès aux données. Envisagez l'emploi d'un index cluster pour les requêtes qui :  
  
-   retournent une plage de valeurs en utilisant des opérateurs tels que `BETWEEN`, >, >=, < et <=.  
  
     Dès que la ligne comportant la première valeur est trouvée à l'aide de l'index cluster, les lignes présentant les valeurs indexées suivantes sont garanties comme étant adjacentes physiquement. Par exemple, si une requête extrait des enregistrements compris dans une plage de numéros de commandes, un index cluster sur la colonne `SalesOrderNumber` permet de localiser rapidement la ligne qui contient le premier numéro de commande, puis d'extraire toutes les lignes successives de la table jusqu'à ce que le dernier numéro de commande soit atteint.  
  
-   retournent des jeux de résultats volumineux ;  
  
-   utilisent des clauses `JOIN` ; ce sont en général des colonnes clés étrangères ;  
  
-   Utilisent des clauses `ORDER BY` ou `GROUP BY`.  
  
     Si un index est présent sur les colonnes spécifiées dans la clause ORDER BY ou GROUP BY, le [!INCLUDE[ssDE](../includes/ssde-md.md)] n'a plus besoin de trier les données car les lignes le sont déjà. Les requêtes présentent dès lors des performances accrues.  
  
### <a name="column-considerations"></a>Remarques sur les colonnes  
 En général, vous devez définir la clé d'index cluster avec le moins de colonnes possible. Envisagez les colonnes présentant un ou plusieurs des attributs suivants :  
  
-   Colonnes uniques ou qui contiennent de nombreuses valeurs distinctes  
  
    Par exemple, l'ID d'un salarié l'identifie de manière unique. Un index cluster ou une contrainte [PRIMARY KEY](../relational-databases/tables/create-primary-keys.md) sur la colonne `EmployeeID` améliore les performances des requêtes qui recherchent des informations sur les salariés en fonction de leur ID. D'une autre manière, un index cluster peut être créé sur `LastName`, `FirstName`, `MiddleName` , car les enregistrements de salariés sont fréquemment groupés et interrogés de cette façon et l'association de ces colonnes présente toujours un niveau élevé de différenciation. 

    > [!TIP]
    > Sauf si cela est spécifié différemment, quand une contrainte [PRIMARY KEY](../relational-databases/tables/create-primary-keys.md) est créée, [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] crée un [index cluster](#Clustered) qui prend en charge cette contrainte.
    > Vous pouvez utiliser un *[uniqueidentifier](../t-sql/data-types/uniqueidentifier-transact-sql.md)* pour garantir l’unicité en tant que PRIMARY KEY, mais ce n’est pas une clé de clustering efficace.
    > Si vous utilisez un *uniqueidentifier* comme PRIMARY KEY, la recommandation est de le créer en tant qu’index non-cluster et d’utiliser une autre colonne (par exemple, un `IDENTITY`) pour créer l’index cluster.   
  
-   Accès séquentiel des colonnes  
  
    Par exemple, l'ID d'un produit l'identifie de manière unique dans la table `Production.Product` de la base de données [!INCLUDE[ssSampleDBobject](../includes/sssampledbobject-md.md)] . Les requêtes dans lesquelles une recherche séquentielle est spécifiée, telles que `WHERE ProductID BETWEEN 980 and 999`, tireront parti d'un index cluster sur `ProductID`. car les lignes sont stockées dans l'ordre sur cette colonne clé.  
  
-   Colonnes définies comme `IDENTITY`.  
  
-   Colonnes fréquemment utilisées pour trier les données extraites d'une table  
  
    Il peut être judicieux de mettre en cluster, c'est-à-dire de trier physiquement, la table sur cette colonne pour économiser le coup d'une opération de tri à chaque fois que la colonne est interrogée.  
  
 Les index cluster sont déconseillés pour les colonnes présentant les attributs suivants :  
  
-   Les colonnes sujettes à des modifications fréquentes.  
  
    La ligne tout entière est ainsi déplacée, car le [!INCLUDE[ssDE](../includes/ssde-md.md)] doit conserver les valeurs des données de la ligne dans l'ordre physique. Cette observation est importante dans les systèmes de traitement transactionnel à haut volume où les données sont en général éphémères.  
  
-   Les clés étendues.  
  
    Les clés étendues sont composées de plusieurs colonnes ou plusieurs colonnes de grande taille. Les valeurs de clé de l'index cluster sont utilisées par tous les index non-cluster comme clés de recherche. Tous les index non-cluster définis sur la même table sont considérablement plus grands car leurs entrées contiennent la clé de cluster et aussi les colonnes clés définies pour cet index non-cluster.  
  
##  <a name="nonclustered-index-design-guidelines"></a><a name="Nonclustered"></a> Indications pour la conception d'index non-cluster  
 Un index non-cluster contient les valeurs de clé d'index et les localisateurs de ligne qui pointent vers l'emplacement de stockage des données de table. Vous pouvez créer plusieurs index non cluster sur une table ou une vue indexée. Les index non-cluster doivent, en principe, améliorer les performances des requêtes fréquemment utilisées qui ne sont pas couvertes par l'index cluster.  
  
 De la même manière que vous utilisez un index dans un livre, l'optimiseur de requête recherche une valeur de données en examinant l'index non-cluster afin de trouver l'emplacement qu'occupe la valeur dans la table, puis récupère directement les données à partir de cet emplacement. C'est pour cette raison que les index non cluster constituent une solution idéale pour les requêtes à correspondance exacte ; l'index contient en effet des entrées décrivant l'emplacement exact qu'occupent dans la table les valeurs de données recherchées dans les requêtes. Par exemple, pour interroger la table `HumanResources. Employee` pour tous les employés qui réfèrent à un responsable spécifique, l'optimiseur de requête peut utiliser l'index non cluster `IX_Employee_ManagerID`; sa colonne clé est `ManagerID` . L'optimiseur de requête recherche rapidement toutes les entrées de l'index qui correspondent à la valeur `ManagerID`spécifiée. Chaque entrée d'index pointe vers la page et la ligne exactes de la table ou de l'index cluster contenant les données correspondantes. Après avoir trouvé toutes les entrées dans l'index, l'optimiseur de requête peut accéder directement à la page et à la ligne exactes pour récupérer les données.  
  
### <a name="nonclustered-index-architecture"></a>Architecture des index non cluster  
 Les index non-cluster possèdent la même structure arborescente binaire que les index cluster, à ces différences près :  
  
-   Les lignes de données de la table sous-jacente ne sont pas triées et stockées dans l'ordre des clés non cluster.  
  
-   Le niveau feuille d’un index non cluster n’est pas constitué de pages de données, mais de pages d’index.  
  
Dans les lignes des index non-cluster, le localisateur est soit un pointeur vers une ligne, soit une clé d'index cluster :  
  
-   Si la table est un segment de mémoire (dépourvue d'index cluster), le localisateur de ligne est un pointeur vers la ligne. Le pointeur est construit à partir de l'ID du fichier, du numéro de la page et du numéro de ligne dans la page. Le pointeur complet est appelé une ID de ligne (RID).  
  
-   Si la table a un index cluster, ou si l'index est sur une vue indexée, le localisateur de ligne est la clé d'index cluster pour la ligne.  
  
Les index non-cluster comprennent une ligne dans [sys.partitions](../relational-databases/system-catalog-views/sys-partitions-transact-sql.md) où **index_id** >1 pour chaque partition utilisée par l’index. Par défaut, un index non-cluster contient une seule partition. Lorsqu'un index non-cluster comprend plusieurs partitions, chaque partition a une structure arborescente binaire qui contient les lignes d'index correspondantes. Par exemple, si un index non-cluster a quatre partitions, il y a quatre arborescences binaires, une dans chaque partition.  
  
En fonction des types de données de l'index non-cluster, chaque structure d'index non-cluster aura une ou plusieurs unités d'allocation dans lesquelles stocker et gérer les données d'une partition spécifique. Chaque index non-cluster aura au minimum une unité d’allocation *IN_ROW_DATA* par partition pour stocker les pages de l’arborescence binaire (B-tree) de l’index. L’index non-cluster aura également une unité d’allocation *LOB_DATA* par partition s’il contient des colonnes LOB (Large Object). Il aura par ailleurs une unité d’allocation *ROW_OVERFLOW_DATA* par partition s’il contient des colonnes de longueur variable dont les lignes dépassent la taille limite de 8 060 octets.  
  
L'illustration suivante montre la structure d'un index non-cluster avec une seule partition.  

![Diagramme montrant la structure d’un index non cluster avec une seule partition.](../relational-databases/media/bokind1a.gif)  
  
### <a name="database-considerations"></a>Remarques sur la base de données  
 Les caractéristiques de la base de données sont importantes lors de la conception d'index non-cluster.  
  
-   Les bases de données ou les tables dont les mises à jour sont faibles, mais qui contiennent des volumes importants de données peuvent tirer parti de nombreux index non-cluster en vue d'améliorer les performances des requêtes. Envisagez de créer des index filtrés pour les sous-ensembles de données bien définis afin d'améliorer les performances des requêtes, réduire les coûts de stockage d'index et réduire les coûts de maintenance d'index par rapport aux index non cluster de table entière.  
  
     Les applications et bases de données d'aide à la décision contenant principalement des données en lecture seule peuvent tirer parti de nombreux index non-cluster. L'optimiseur de requête doit choisir parmi davantage d'index pour déterminer la méthode d'accès la plus rapide ; les caractéristiques de mise à jour faible de la base de données sont synonymes d'une maintenance d'index qui n'entravera pas les performances.  
  
-   Il est important d’éviter la surindexation des applications et bases de données OLTP (traitement transactionnel en ligne) qui contiennent des tables largement mises à jour. Les index doivent en outre être réduits, c'est-à-dire contenir le moins de colonnes possible.  
  
     La définition de nombreux index sur une table affecte les performances des instructions INSERT, UPDATE, DELETE et MERGE , car à mesure que les données de la table changent, tous les index doivent être mis à jour en conséquence.  
  
### <a name="query-considerations"></a>Remarques sur les requêtes  
 Avant de créer des index non-cluster, vous devez comprendre comment se déroulera l'accès aux données. Il est conseillé d'utiliser un index non-cluster pour les requêtes avec les attributs suivants :  
  
-   Utilisent des clauses `JOIN` ou `GROUP BY`.  
  
     Créez plusieurs index non-cluster sur des colonnes impliquées dans les opérations de jointure et de regroupement, ainsi qu'un index cluster sur les colonnes clés étrangère éventuelles.  
  
-   Requêtes qui ne retournent pas des ensembles de résultats volumineux.  
  
     Créez des index filtrés pour couvrir les requêtes qui retournent un sous-ensemble bien défini de lignes d'une grande table. 
     
     > [!TIP] 
     > En général, la clause WHERE de l’instruction CREATE INDEX correspond à la clause WHERE d’une requête couverte.  

-   Requêtes qui contiennent des colonnes souvent impliquées dans les conditions de recherche d'une requête (clause WHERE) qui retournent des correspondances exactes.  

    > [!TIP]
    > Examinez le rapport coût/avantages associé à l’ajout de nouveaux index. Il est parfois préférable de consolider les requêtes supplémentaires nécessaires dans un index existant. Par exemple, vous pouvez envisager d’ajouter une ou deux colonnes de niveau feuille supplémentaires à un index existant si cela permet de couvrir plusieurs requêtes critiques, plutôt que d’avoir exactement un index par requête critique.
    
### <a name="column-considerations"></a>Remarques sur les colonnes  
 Il est conseillé d'utiliser des colonnes qui possèdent un ou plusieurs de ces attributs :  
  
-   Couvrent la requête.  
  
     Performances accrues lorsque l'index contient toutes les colonnes de la requête. L'optimiseur de requête peut localiser toutes les valeurs de colonnes dans l'index ; les données de table ou d'index cluster ne sont pas accédées, avec pour conséquence une réduction des opérations d'E/S disque. Utilisez un index avec [colonnes incluses](#Included_Columns) pour ajouter des colonnes couvrantes au lieu de créer une clé d’index de grande taille.  
  
     Si la table a un index cluster, la ou les colonnes définies dans cet index sont automatiquement ajoutées à la fin de chaque index non-cluster de la table. Ceci peut produire une requête couverte sans spécifier les colonnes de l'index cluster dans la définition de l'index non-cluster. Par exemple, si une table a un index cluster sur la colonne `C`, un index non cluster sur les colonnes `B` et `A` aura comme valeurs de clé les colonnes `B`, `A`et `C`.  
      
-   Un nombre élevé de valeurs distinctes, comme une combinaison de nom et prénom, si un index cluster est utilisé pour d'autres colonnes.  
  
     Lorsqu'il existe très peu de valeurs distinctes (1 et 0 uniquement, par exemple), la plupart des requêtes utiliseront une analyse de table, généralement plus efficace, au lieu de l'index. Pour ce type de données, envisagez de créer un index filtré sur une valeur distincte qui se produit uniquement dans un petit nombre de lignes. Par exemple, si la plupart des valeurs sont 0, l'optimiseur de requête peut utiliser un index filtré pour les lignes de données qui contiennent 1.  
  
####  <a name="use-included-columns-to-extend-nonclustered-indexes"></a><a name="Included_Columns"></a> Utiliser des colonnes incluses pour étendre les index non cluster  
 Vous pouvez étendre la fonctionnalité des index non cluster en ajoutant des colonnes non-clés au niveau feuille de l'index non cluster. L'inclusion de colonnes non-clés permet de créer des index non-cluster qui couvrent davantage de requêtes. En effet, les colonnes non-clés présentent les avantages suivants :  
  
-   Elles peuvent contenir des types de données qui ne sont pas autorisés dans les colonnes de clés d'index.  
  
-   Elles ne sont pas prises en compte par le [!INCLUDE[ssDE](../includes/ssde-md.md)] lors du calcul du nombre de colonnes clés d'index ou de la taille de la clé d'index.  
  
 Un index contenant des colonnes non-clés incluses peut améliorer considérablement les performances des requêtes lorsque toutes les colonnes de la requête sont incluses dans l'index en tant que colonnes clés ou non-clés. Les gains de performances sont dus au fait que l'optimiseur de requête peut localiser toutes les valeurs des colonnes dans l'index ; l'accès aux données de table et d'index n'a pas lieu, produisant ainsi un nombre moindre d'opérations d'E/S sur le disque.  
  
> [!NOTE]  
> Lorsqu'un index contient toutes les colonnes auxquelles la requête fait référence, on dit qu'il couvre la requête.  
  
 Alors que les colonnes clés sont stockées à tous les niveaux de l'index, les colonnes non-clés sont stockées uniquement au niveau feuille.  
  
##### <a name="using-included-columns-to-avoid-size-limits"></a>Utilisation de colonnes incluses pour éviter les limites de taille  
 Vous pouvez inclure des colonnes non-clés dans un index non cluster pour éviter de dépasser les limitations actuelles de taille d'index, établies à 16 colonnes clés au maximum et une taille de clé d'index de 900 octets au maximum. Le [!INCLUDE[ssDE](../includes/ssde-md.md)] ne tient pas compte des colonnes non-clés lors du calcul du nombre de colonnes clés d'index ou de la taille de la clé d'index.   
 Par exemple, supposons que vous voulez indexer les colonnes suivantes de la table `Document` :  
 *  `Title nvarchar(50)`  
 *  `Revision nchar(5)`  
 *  `FileName nvarchar(400)`  
  
 Comme les types de données **nchar** et **nvarchar** nécessitent deux octets par caractère, un index qui contient ces trois colonnes dépasse de 10 octets (455 * 2) la limitation de taille de 900 octets. En utilisant la clause `INCLUDE` de l'instruction `CREATE INDEX` , la clé d'index peut être définie en tant que (`Title, Revision`) et `FileName` en tant que colonne non-clé. De cette manière, la taille de la clé d’index vaut 110 octets (55 \* 2) et l’index contient toujours toutes les colonnes requises. L'instruction ci-dessous crée cet index.  
  
```sql  
CREATE INDEX IX_Document_Title   
ON Production.Document (Title, Revision)   
INCLUDE (FileName);   
```  
  
##### <a name="index-with-included-columns-guidelines"></a>Directives sur les index contenant des colonnes incluses  
Lors de la conception d'index non-cluster contenant des colonnes incluses, tenez compte des directives suivantes :  
  
-   Les colonnes non-clés sont définies dans la clause INCLUDE de l'instruction CREATE INDEX.  
  
-   Les colonnes non-clés peuvent être définies uniquement sur les index non-cluster de tables ou de vues indexées.  
  
-   Tous les types de données sont autorisés, à l'exception de **text**, **ntext** et **image**.  
  
-   Les colonnes calculées déterministes et précises ou imprécises peuvent être des colonnes incluses. Pour plus d'informations, consultez [Indexes on Computed Columns](../relational-databases/indexes/indexes-on-computed-columns.md).  
  
-   Comme pour les colonnes clés, les colonnes calculées dérivées des types de données **image**, **ntext** et **text** peuvent être des colonnes non-clés (incluses) tant que le type de données de la colonne calculée est autorisé en tant que colonne d’index non-clé.  
  
-   Les noms des colonnes ne peuvent pas être spécifiés à la fois dans la liste INCLUDE et dans la liste des colonnes clés.  
  
-   Les noms des colonnes ne peuvent pas être répétés dans la liste INCLUDE.  

##### <a name="column-size-guidelines"></a>Directives sur la taille des colonnes  
  
-   Vous devez spécifier au moins une colonne clé. Le nombre maximal de colonnes non-clés est de 1023. Il équivaut au nombre maximal de colonnes de table moins 1.  
  
-   Les colonnes de clés d'index, colonnes non-clés exclues, doivent respecter les restrictions existantes de taille d'index, à savoir 16 colonnes clés au maximum et une taille totale de clé d'index de 900 octets.  
  
-   La taille totale de toutes les colonnes non-clés est limitée uniquement par la taille des colonnes spécifiées dans la clause INCLUDE ; par exemple, les colonnes **varchar(max)** sont limitées à 2 Go.  
  
##### <a name="column-modification-guidelines"></a>Directives sur la modification des colonnes  
 Lors de la définition d'une colonne de table définie en tant que colonne incluse, les restrictions suivantes s'appliquent :  
  
-   Les colonnes non-clés ne peuvent pas être supprimées de la table, sauf si l'index est d'abord supprimé.  
  
-   Les colonnes non-clés ne peuvent pas être modifiées, sauf pour effectuer les opérations suivantes :  
  
    -   modifier la possibilité de valeur NULL de la colonne de NOT NULL à NULL ;  
  
    -   augmenter la longueur des colonnes **varchar**, **nvarchar** ou **varbinary** .  
  
        > [!NOTE]  
        >  Ces restrictions sur la modification des colonnes s'appliquent également aux colonnes de clés d'index.  
  
##### <a name="design-recommendations"></a>Recommandations relatives à la conception  
 La conception d'index non-cluster doit être réalisée avec une clé d'index de grande taille, de sorte que seules les colonnes utilisées pour la recherche sont les colonnes clés. Toutes les autres colonnes qui couvrent la requête doivent être des colonnes non-clés incluses. De cette manière, vous disposez de toutes les colonnes nécessaires pour couvrir la requête, mais la clé d'index elle-même est petite et efficace.  
  
 Par exemple, supposons que vous voulez concevoir un index qui couvre la requête ci-dessous.  
  
```sql  
SELECT AddressLine1, AddressLine2, City, StateProvinceID, PostalCode  
FROM Person.Address  
WHERE PostalCode BETWEEN N'98000' and N'99999';  
```  
  
 Pour couvrir la requête, chaque colonne doit être définie dans l'index. Même si vous pouviez définir toutes les colonnes en tant que colonnes clés, la taille de clé serait 334 octets. Comme la seule colonne vraiment utilisée comme critère de recherche est la colonne `PostalCode` , dont la longueur vaut 30 octets, une meilleure conception d'index définirait `PostalCode` comme colonne clé et inclurait toutes les autres colonnes comme colonnes non-clés.  
  
 L'instruction suivante crée un index contenant des colonnes incluses pour couvrir la requête.  
  
```sql  
CREATE INDEX IX_Address_PostalCode  
ON Person.Address (PostalCode)  
INCLUDE (AddressLine1, AddressLine2, City, StateProvinceID);  
```  
  
##### <a name="performance-considerations"></a>Considérations relatives aux performances  
 Évitez d'ajouter des colonnes superflues. L'ajout de trop nombreuses colonnes d'index, clés et non-clés, peut avoir les conséquences suivantes sur les performances :  
  
-   Le nombre de lignes d'index contenues sur une page sera moindre. Ceci pourrait augmenter les E/S et réduire l'efficacité de la mémoire cache.  
  
-   L'espace disque requis pour stocker l'index sera supérieur. En particulier, l’ajout des types de données **varchar(max)** , **nvarchar(max)** , **varbinary(max)** et **xml** en tant que colonnes d’index non-clés peut accroître considérablement l’espace disque nécessaire. En effet, les valeurs des colonnes sont copiées dans le niveau feuille de l'index. Par conséquent, elles résident à la fois dans l'index et dans la table de base.  
  
-   La maintenance d'un index peut accroître la durée nécessaire pour effectuer des modifications, des insertions, des mises à jour ou des suppressions à la table sous-jacente ou à la vue indexée.  
  
 Vous devez déterminer si les gains de performances des requêtes compensent la dégradation des performances lors de la modification des données et la quantité d'espace disque supplémentaire nécessaire.  
  
##  <a name="unique-index-design-guidelines"></a><a name="Unique"></a> Instructions de conception d'index uniques  
 Un index unique garantit que la clé d'index ne contient aucune valeur dupliquée et que, par conséquent, chaque ligne de la table est unique d'une certaine manière. Spécifier un index unique n'a de sens que si l'unicité est une caractéristique des données elles-mêmes. Par exemple, si vous souhaitez que les valeurs de la colonne `NationalIDNumber` de la table `HumanResources.Employee` soient uniques, lorsque la clé primaire est `EmployeeID`, créez une contrainte UNIQUE sur la colonne `NationalIDNumber` . Si l'utilisateur essaie de saisir la même valeur dans cette colonne pour plusieurs employés, un message d'erreur apparaît et la valeur dupliquée n'est pas entrée.  
  
 Lorsque vous utilisez un index unique multicolonne, celui-ci garantit que chaque combinaison de valeurs dans la clé d'index est unique. Par exemple, si un index unique est créé sur une combinaison des colonnes `LastName`, `FirstName`et `MiddleName` , deux lignes de la table ne peuvent pas posséder la même combinaison de valeurs pour ces colonnes.  
  
 Tant les index cluster que les index non-cluster peuvent être uniques. À condition que les données contenues dans la colonne soient uniques, vous pouvez créer à la fois un index cluster unique et plusieurs index non-cluster uniques sur la même table.  
  
 Les index uniques présentent les avantages suivants :  
  
-   L'intégrité des données des colonnes définies est garantie.  
  
-   L'optimiseur de requête dispose d'informations utiles supplémentaires.  
  
 Lorsque vous créez une contrainte PRIMARY KEY ou UNIQUE, vous créez automatiquement un index unique sur les colonnes spécifiées. Il n'y a pas de différences significatives entre la création d'une contrainte UNIQUE et la création d'un index unique indépendant d'une contrainte. La validation des données se produit de la même manière et l'optimiseur de requête considère un index unique de la même façon, qu'il soit créé par une contrainte or manuellement. Toutefois, vous devez créer une contrainte UNIQUE ou PRIMARY KEY sur la colonne lorsque votre objectif est de préserver l'intégrité des données. Cette opération met en évidence la finalité de l'index.  
  
### <a name="considerations"></a>Considérations  
  
-   Un index unique, une contrainte UNIQUE ou une contrainte PRIMARY KEY ne peuvent pas être créés si les données comportent des valeurs de clé dupliquées.  
  
-   Si les données sont uniques et que l'unicité doit être assurée, la création d'un index unique au lieu d'un index non unique sur la même combinaison de colonnes fournit des informations supplémentaires à l'optimiseur de requête, qui peut générer des plans d'exécution plus efficaces. La création d'un index unique (si possible en créant une contrainte UNIQUE) est recommandée dans ce cas.  
  
-   Un index non cluster unique peut contenir des colonnes non-clés incluses. Pour plus d'informations, consultez [Index avec colonnes incluses](#Included_Columns).  
  
  
##  <a name="filtered-index-design-guidelines"></a><a name="Filtered"></a> Instructions de conception d'index filtrés  
 Un index filtré est un index non cluster optimisé, convenant tout particulièrement aux requêtes qui effectuent des sélections dans un sous-ensemble de données bien défini. Il utilise un prédicat de filtre pour indexer une partie des lignes de la table. Un index filtré bien conçu peut améliorer les performances des requêtes, réduire les coûts de maintenance des index et réduire les coûts de stockage des index par rapport aux index de table entière.  
  
**S'applique à**: [!INCLUDE[ssKatmai](../includes/sskatmai-md.md)] jusqu'à [!INCLUDE[ssCurrent](../includes/sscurrent-md.md)].  
  
 Les index filtrés peuvent présenter les avantages suivants par rapport aux index de table entière :  
  
-   **Meilleures performances des requêtes et qualité de plan améliorée**  
  
     Un index filtré bien conçu améliore les performances des requêtes et la qualité du plan d'exécution car il est plus petit qu'un index non cluster de table entière et contient des statistiques filtrées. Les statistiques filtrées sont plus précises que les statistiques de table entière car elles couvrent uniquement les lignes de l'index filtré.  
  
-   **Coûts réduits de maintenance des index**  
  
     La maintenance d'un index intervient uniquement lorsque les instructions de langage de manipulation de données (DML) affectent les données de l'index. Un index filtré réduit les coûts de maintenance des index par rapport à un index non cluster de table entière car il est plus petit et est demande une maintenance uniquement lorsque les données de l'index sont affectées. Il est possible d'avoir un grand nombre d'index filtrés, notamment s'ils contiennent des données qui sont rarement affectées. De la même façon, si un index filtré contient uniquement les données fréquemment affectées, la plus petite taille de l'index réduit le coût de la mise à jour des statistiques.  
  
-   **Coûts réduits de stockage des index**  
  
     La création d'un index filtré peut réduire le stockage sur disque des index non cluster lorsqu'un index de table entière n'est pas nécessaire. Vous pouvez remplacer un index non cluster de table entière par plusieurs index filtrés sans augmenter considérablement le stockage nécessaire.  
  
 Les index filtrés sont utiles lorsque les colonnes contiennent des sous-ensembles bien définis de données qui sont référencés par des requêtes dans des instructions SELECT. Voici quelques exemples :  
  
-   Colonnes éparses qui contiennent uniquement quelques valeurs non NULL.  
  
-   Colonnes hétérogènes qui contiennent des catégories de données.  
  
-   Colonnes qui contiennent des plages de valeurs, telles que des montants en devise, des heures et des dates.  
  
-   Partitions de table définies par une logique de comparaison simple pour les valeurs de colonne.  
  
 La réduction des coûts de maintenance pour les index filtrés est plus particulièrement notable lorsque le nombre de lignes de l'index est petit comparé à un index de table entière. Si l'index filtré inclut la plupart des lignes de la table, son coût de maintenance risque d'être plus élevé que celui d'un index de table entière. Dans ce cas, vous devez utiliser un index de table entière à la place d'un index filtré.  
  
 Les index filtrés sont définis sur une seule table et ne prennent en charge que les opérateurs de comparaison simples. Si vous avez besoin d'une expression de filtre qui référence plusieurs tables ou présente une logique complexe, vous devez créer une vue.  
  
### <a name="design-considerations"></a>Remarques sur la conception  
 Pour concevoir des index filtrés efficaces, il est important de comprendre les requêtes utilisées par votre application et leurs relations avec les sous-ensembles de données. Les colonnes contenant principalement des valeurs NULL, les colonnes contenant des catégories hétérogènes de valeurs et les colonnes contenant des plages de valeurs distinctes sont autant d'exemples de données avec des sous-ensembles bien définis. Les considérations suivantes relatives à la conception présentent divers scénarios dans lesquels un index filtré peut présenter des avantages par rapport à des index de table entière.  
 
> [!TIP] 
> Pour définir des [index columnstore](#columnstore_index) non-cluster, vous pouvez utiliser une condition filtrée. Pour minimiser l’impact sur les performances de l’ajout d’un index columnstore sur une table OLTP, utilisez une condition filtrée pour créer un index columnstore non cluster uniquement sur les données brutes de votre charge de travail opérationnelle. 
  
#### <a name="filtered-indexes-for-subsets-of-data"></a>Index filtrés pour des sous-ensembles de données  
Lorsqu'une colonne contient seulement un petit nombre de valeurs pertinentes pour les requêtes, vous pouvez créer un index filtré sur ce sous-ensemble de valeurs. Ainsi, lorsque les valeurs d'une colonne sont principalement NULL et que la requête effectue uniquement des sélections dans les valeurs non NULL, vous pouvez créer un index filtré pour les lignes de données non NULL. L'index ainsi créé sera plus petit et coûtera moins cher en maintenance qu'un index non cluster de table entière défini sur les mêmes colonnes clés.  
  
Par exemple, la base de données `AdventureWorks2012` a une table `Production.BillOfMaterials` avec 2679 lignes. Seules 199 lignes de la colonne `EndDate` contiennent une valeur non NULL ; les 2480 autres contiennent des valeurs NULL. L'index filtré suivant couvre les requêtes qui retournent les colonnes définies dans l'index et qui sélectionnent uniquement les lignes avec une valeur non NULL pour `EndDate`.  
  
```sql  
CREATE NONCLUSTERED INDEX FIBillOfMaterialsWithEndDate  
    ON Production.BillOfMaterials (ComponentID, StartDate)  
    WHERE EndDate IS NOT NULL ;  
GO  
```  
  
L'index filtré `FIBillOfMaterialsWithEndDate` est valide pour la requête suivante. Vous pouvez afficher le plan d'exécution de la requête pour déterminer si l'optimiseur de requête a utilisé l'index filtré.  
  
```sql  
SELECT ProductAssemblyID, ComponentID, StartDate   
FROM Production.BillOfMaterials  
WHERE EndDate IS NOT NULL   
    AND ComponentID = 5   
    AND StartDate > '20080101' ;  
```  
  
Pour plus d'informations sur la création d'index filtrés et la définition de l'expression de prédicat d'index filtré, consultez [Create Filtered Indexes](../relational-databases/indexes/create-filtered-indexes.md).  
  
#### <a name="filtered-indexes-for-heterogeneous-data"></a>Index filtrés pour des données hétérogènes  
 Lorsqu'une table contient des lignes des données hétérogènes, vous pouvez créer un index filtré pour une ou plusieurs catégories de données.  
  
 Par exemple, chacun des produits répertoriés dans la table `Production.Product` est affecté à un `ProductSubcategoryID`, qui est à son tour associé à une catégorie de produits (Bikes, Components, Clothing ou Accessories). Ces catégories sont hétérogènes car leurs valeurs de colonne dans la table `Production.Product` ne sont pas étroitement corrélées. Par exemple, les colonnes `Color`, `ReorderPoint`, `ListPrice`, `Weight`, `Class`et `Style` ont des caractéristiques uniques pour chaque catégorie de produit. Supposons que des requêtes portent fréquemment sur la catégorie Accessories qui comporte les sous-catégories 27-36. Améliorez les performances des requêtes portant sur Accessories en créant un index filtré sur les sous-catégories de la catégorie Accessories, tel que l'illustre l'exemple suivant.  
  
```sql  
CREATE NONCLUSTERED INDEX FIProductAccessories  
    ON Production.Product (ProductSubcategoryID, ListPrice)   
        Include (Name)  
WHERE ProductSubcategoryID >= 27 AND ProductSubcategoryID <= 36;  
```  
  
 L'index filtré `FIProductAccessories` couvre la requête suivante, car les résultats de la requête  
  
 sont contenus dans l'index et le plan de requête n'inclut pas de recherche de table de base. Par exemple, l'expression de prédicat de requête `ProductSubcategoryID = 33` est un sous-ensemble du prédicat d'index filtré `ProductSubcategoryID >= 27` et `ProductSubcategoryID <= 36`, les colonnes `ProductSubcategoryID` et `ListPrice` dans le prédicat de requête sont toutes deux des colonnes clés dans l'index et le nom est stocké au niveau feuille de l'index en tant que colonne incluse.  
  
```sql  
SELECT Name, ProductSubcategoryID, ListPrice  
FROM Production.Product  
WHERE ProductSubcategoryID = 33 AND ListPrice > 25.00 ;  
```  
  
#### <a name="key-columns"></a>Colonnes clés  
 Il est recommandé d'inclure un petit nombre de colonnes clés ou incluses dans une définition d'index filtré, et d'incorporer uniquement les colonnes qui sont nécessaires à l'optimiseur de requête pour choisir l'index filtré pour le plan d'exécution de la requête. L'optimiseur de requête peut choisir un index filtré pour la requête, qu'il couvre ou non la requête. Toutefois, l'optimiseur de requête choisira plus probablement un index filtré s'il couvre la requête.  
  
 Dans certains cas, un index filtré couvre la requête sans inclure les colonnes de l'expression d'index filtré en tant que colonnes clés ou incluses dans la définition de l'index filtré. Les règles suivantes expliquent dans quels cas une colonne de l'expression d'index filtré doit être une colonne clé ou incluse dans la définition de l'index filtré. Les exemples font référence à l'index filtré `FIBillOfMaterialsWithEndDate` qui a été créé précédemment.  
  
 Il n'est pas nécessaire qu'une colonne de l'expression d'index filtré soit une colonne clé ou incluse dans la définition de l'index filtré si l'expression d'index filtré est équivalente au prédicat de requête et si la requête ne retourne pas la colonne dans l'expression d'index filtré avec les résultats de la requête. Par exemple, l'index filtré `FIBillOfMaterialsWithEndDate` couvre la requête suivante car le prédicat de la requête est équivalent à l'expression de filtre et `EndDate` n'est pas retourné avec les résultats de la requête. `FIBillOfMaterialsWithEndDate` n'a pas besoin de la colonne `EndDate` comme colonne clé ou incluse dans la définition de l'index filtré.  
  
```sql  
SELECT ComponentID, StartDate FROM Production.BillOfMaterials  
WHERE EndDate IS NOT NULL;   
```  
  
 Une colonne de l'expression d'index filtré doit être une colonne clé ou incluse dans la définition de l'index filtré si le prédicat de la requête utilise cette colonne dans une comparaison qui n'est pas équivalente à l'expression d'index filtré. Par exemple, l'index filtré `FIBillOfMaterialsWithEndDate` est valide pour la requête suivante car il sélectionne un sous-ensemble de lignes dans l'index filtré. Toutefois, il ne couvre pas la requête suivante, car `EndDate` est utilisé dans la comparaison `EndDate > '20040101'`, qui n'est pas équivalente à l'expression d'index filtré. Le processeur de requête ne peut pas exécuter cette requête sans rechercher les valeurs de `EndDate`. Par conséquent, `EndDate` doit être une colonne clé ou incluse dans la définition de l'index filtré.  
  
```sql  
SELECT ComponentID, StartDate FROM Production.BillOfMaterials  
WHERE EndDate > '20040101';   
```  
  
 Une colonne de l'expression d'index filtré doit être une colonne clé ou incluse dans la définition de l'index filtré si la colonne se trouve dans le jeu de résultats de la requête. Par exemple, l'index filtré `FIBillOfMaterialsWithEndDate` ne couvre pas la requête suivante car il retourne la colonne `EndDate` dans les résultats de la requête. Par conséquent, `EndDate` doit être une colonne clé ou incluse dans la définition de l'index filtré.  
  
```sql  
SELECT ComponentID, StartDate, EndDate FROM Production.BillOfMaterials  
WHERE EndDate IS NOT NULL;  
```  
  
 Il n'est pas nécessaire que la clé de l'index cluster de la table soit une colonne clé ou incluse dans la définition de l'index filtré. La clé de l'index cluster est automatiquement incluse dans tous les index non cluster, y compris les index filtrés.  
  
#### <a name="data-conversion-operators-in-the-filter-predicate"></a>Opérateurs de conversion de données dans le prédicat du filtre  
 Si l'opérateur de comparaison spécifié dans l'expression d'index filtré de l'index filtré provoque une conversion de données implicite ou explicite, une erreur se produit si cette conversion se produit du côté gauche d'un opérateur de comparaison. Une solution consiste à écrire l'expression d'index filtré avec l'opérateur de conversion de données (CAST ou CONVERT) à droite de l'opérateur de comparaison.  
  
 L'exemple suivant crée une table avec différents types de données.  
  
```sql  
USE AdventureWorks2012;  
GO  
CREATE TABLE dbo.TestTable (a int, b varbinary(4));  
```  
  
 Dans la définition d'index filtré suivante, la colonne `b` est implicitement convertie en type de données integer afin de la comparer à la constante 1. Le message d'erreur 10611 est alors généré car la conversion se produit à gauche de l'opérateur dans le prédicat filtré.  
  
```sql  
CREATE NONCLUSTERED INDEX TestTabIndex ON dbo.TestTable(a,b)  
WHERE b = 1;  
```  
  
 La solution consiste à convertir la constante qui se trouve à droite de manière à ce que son type soit identique à celui de la colonne `b`, comme dans l'exemple suivant :  
  
```sql  
CREATE INDEX TestTabIndex ON dbo.TestTable(a,b)  
WHERE b = CONVERT(Varbinary(4), 1);  
```  
  
 Le fait de déplacer la conversion de données de la gauche vers la droite d'un opérateur de comparaison peut modifier la signification de la conversion. Dans l'exemple ci-dessus, lorsque l'opérateur CONVERT a été ajouté à droite, la comparaison de type integer est devenue une comparaison de type **varbinary** .  
  
## <a name="columnstore-index-design-guidelines"></a><a name="columnstore_index"></a> Indications pour la conception d’index columnstore

Un *columnstore index* est une technologie permettant de stocker, extraire et gérer les données à l'aide d'un format de données en colonnes, appelé columnstore. Pour plus d’informations, consultez [Index columnstore - Présentation](../relational-databases/indexes/columnstore-indexes-overview.md). 

Pour obtenir des informations de version, consultez [Index columnstore - Nouveautés](../relational-databases/indexes/columnstore-indexes-what-s-new.md).

### <a name="columnstore-index-architecture"></a>Architecture des index columnstore

La connaissances de ces informations de base permet de comprendre plus facilement d’autres articles relatifs aux columnstores qui expliquent comment les utiliser efficacement.

#### <a name="data-storage-uses-columnstore-and-rowstore-compression"></a>Le stockage de données utilise la compression de columnstore et de rowstore
Quand nous parlons des index columnstore, nous utilisons les termes *rowstore* et *columnstore* pour indiquer clairement le format du stockage de données. Les index columnstore utilisent les deux types de stockage.

 ![Index cluster columnstore](../relational-databases/indexes/media/sql-server-pdw-columnstore-physicalstorage.gif "Index columstore en cluster")

- Un **columnstore** représente des données qui sont organisées logiquement sous la forme d’une table avec des lignes et des colonnes, et stockées physiquement dans un format de données selon les colonnes.
  
  Un index columnstore stocke physiquement la plupart des données au format columnstore. Au format columnstore, les données sont compressées et décompressées sous la forme de colonnes. Il n’est pas nécessaire de décompresser les autres valeurs de chaque ligne qui ne sont pas demandées par la requête. Cela permet d’analyser rapidement une colonne entière d’une table volumineuse. 

- Un **rowstore** représente des données qui sont organisées logiquement sous la forme d’une table avec des lignes et des colonnes, puis stockées physiquement dans un format de données selon les lignes. Il s’agit de la méthode standard de stockage des données de table relationnelles, comme un segment de mémoire ou un index d’arbre B (B-tree) cluster.

  Un index columnstore stocke également physiquement des lignes dans un format rowstore appelé « deltastore ». Le deltastore, également appelé « rowgroups delta », est un espace de stockage pour les lignes qui sont en trop petit nombre pour bénéficier de la compression dans le columnstore. Chaque rowgroup delta est implémenté comme un index B-tree cluster. 

- Le **deltastore** est un espace de stockage pour les lignes qui sont en trop petit nombre pour être compressées dans le columnstore. Le deltastore stocke les lignes au format rowstore. 

Pour plus d’informations sur les termes et les concepts columnstore, consultez [Index Columnstore : Vue d’ensemble](../relational-databases/indexes/columnstore-indexes-overview.md).
  
#### <a name="operations-are-performed-on-rowgroups-and-column-segments"></a>Les opérations sont effectuées sur des rowgroups et des segments de colonne

L’index columnstore regroupe des lignes en unités gérables. Chacune de ces unités est appelée **rowgroup**. Pour des performances optimales, le nombre de lignes dans le rowgroup doit être suffisamment important pour améliorer le taux de compression et suffisamment petit pour tirer parti des opérations en mémoire.

Par exemple, l’index columnstore effectue les opérations suivantes sur des rowgroups :

* Compresse les rowgroups dans le columnstore. La compression est effectuée sur chaque segment de colonne d’un rowgroup.
* Fusionne les rowgroups lors d’une opération `ALTER INDEX ... REORGANIZE`, y compris la suppression des données supprimées.
* Crée des rowgroups lors d’une opération `ALTER INDEX ... REBUILD`.
* Génère des rapports sur l’intégrité et la fragmentation des rowgroups dans des vues de gestion dynamique (DMV).

Le deltastore se compose d’un ou plusieurs rowgroups appelés **rowgroups delta**. Chaque rowgroup delta est un index B-Tree en cluster qui stocke de petites charges en bloc et effectue une insertion jusqu’à ce que le rowgroup contienne 1 048 576 lignes. À ce moment-là, un processus nommé **moteur de tuple** compresse automatiquement le rowgroup fermé dans le columnstore. 

Pour plus d’informations sur les états de rowgroup, consultez [sys.dm_db_column_store_row_group_physical_stats (Transact-SQL)](../relational-databases/system-dynamic-management-views/sys-dm-db-column-store-row-group-physical-stats-transact-sql.md). 

> [!TIP]
> Avoir un trop grand nombre de rowgroups de petite taille réduit la qualité de l’index columnstore. Une opération de réorganisation fusionne des rowgroups plus petits, suite à une stratégie de seuil interne qui détermine comment supprimer les lignes supprimées et combiner les rowgroups compressés. Après une fusion, la qualité de l’index doit être améliorée. 

> [!NOTE]
> À partir de [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)], le moteur de tuple est aidé par une tâche de fusion en arrière-plan qui compresse automatiquement les rowgroups delta OPEN plus petits qui existent depuis un certain temps, tel que déterminé par un seuil interne, ou qui fusionne les rowgroups COMPRESSED à partir desquels un grand nombre de lignes a été supprimé.      

Chaque colonne a certaines de ses valeurs dans chaque rowgroup. Ces valeurs sont appelées **segments de colonne**. Chaque rowgroup contient un segment de colonne pour chaque colonne dans la table. Chaque colonne a un seul segment de colonne dans chaque rowgroup.

![Segment de colonne](../relational-databases/indexes/media/sql-server-pdw-columnstore-columnsegment.gif "segment de colonne") 
 
Quand l’index columnstore compresse un rowgroup, il compresse chaque segment de colonne séparément. Pour décompresser la totalité d’une colonne, l’index columnstore doit simplement décompresser un segment de colonne dans chaque rowgroup. 

Pour plus d’informations sur les termes et les concepts columnstore, consultez [Index Columnstore : Vue d’ensemble](../relational-databases/indexes/columnstore-indexes-overview.md). 

#### <a name="small-loads-and-inserts-go-to-the-deltastore"></a>Les petits chargements et les petites insertions sont placés dans le deltastore
Un index columnstore améliore la compression columnstore et les performances en compressant au moins 102 400 lignes à la fois dans l’index columnstore. Pour compresser des lignes en masse, l’index columnstore accumule les petits chargements et les petites insertions dans le deltastore. Les opérations deltastore sont effectuées en coulisse. Pour retourner des résultats de requête corrects, l'index columnstore cluster associe les résultats de columnstore et de deltastore. 

Les lignes sont placées dans le deltastore quand elles sont :
* Insérées avec l’instruction `INSERT INTO ... VALUES`.
* À la fin d’un chargement en masse et que leur nombre est inférieur à 102 400.
* Mises à jour. Chaque mise à jour correspond à une suppression et une insertion.

Le deltastore stocke également une liste des ID des lignes supprimées qui ont été marquées comme supprimées mais qui n’ont pas encore été supprimées physiquement de l’index columnstore. 

Pour plus d’informations sur les termes et les concepts columnstore, consultez [Index Columnstore : Vue d’ensemble](../relational-databases/indexes/columnstore-indexes-overview.md). 

#### <a name="when-delta-rowgroups-are-full-they-get-compressed-into-the-columnstore"></a>Quand les rowgroups delta sont pleins, ils sont compressés dans le columnstore

Les index columnstore cluster collectent jusqu’à 1 048 576 lignes dans chaque rowgroup delta avant de compresser le rowgroup dans le columnstore. Cela améliore la compression de l’index columnstore. Lorsqu’un rowgroup delta atteint le nombre maximal de lignes, il passe de l’état OPEN à l’état CLOSED. Un processus en arrière-plan nommé moteur de tuple vérifie les groupes de lignes fermés. Lorsqu'il trouve un rowgroup fermé, il le compresse et le stocke dans le columnstore.  

Quand un rowgroup delta a été compressé, le rowgroup delta existant passe à l’état TOMBSTONE pour être supprimé ultérieurement par le moteur de tuple lorsqu’il n’y a aucune référence à celui-ci. Le nouveau rowgroup compressé est marqué comme COMPRESSED. 

Pour plus d’informations sur les états de rowgroup, consultez [sys.dm_db_column_store_row_group_physical_stats (Transact-SQL)](../relational-databases/system-dynamic-management-views/sys-dm-db-column-store-row-group-physical-stats-transact-sql.md). 

Vous pouvez placer de force des rowgroups delta dans le columnstore à l’aide de l’instruction [ALTER INDEX](../t-sql/statements/alter-index-transact-sql.md) pour reconstruire ou réorganiser l’index. Notez que s’il existe une sollicitation de la mémoire lors de la compression, l’index columnstore peut réduire le nombre de lignes stockées dans le rowgroup compressé.   

Pour plus d’informations sur les termes et les concepts columnstore, consultez [Index Columnstore : Vue d’ensemble](../relational-databases/indexes/columnstore-indexes-overview.md). 

#### <a name="each-table-partition-has-its-own-rowgroups-and-delta-rowgroups"></a>Chaque partition de table a ses propres rowgroups et rowgroups delta

Le concept de partitionnement est identique dans un index cluster, un segment de mémoire et un index columnstore. Le partitionnement d’une table divise la table en groupes de lignes plus petits en fonction d’une plage de valeurs de colonne. Il est souvent utilisé pour la gestion des données. Par exemple, vous créez une partition pour chaque année de données, puis vous utilisez le basculement de partition pour archiver des données dans un stockage moins coûteux. Le basculement de partition fonctionne sur les index columnstore et facilite le déplacement d’une partition de données vers un autre emplacement.

Les rowgroups sont toujours définis dans une partition de table. Quand un index columnstore est partitionné, chaque partition possède ses propres rowgroups et rowgroups delta compressés.

> [!TIP]
> Envisagez d’utiliser le partitionnement de table si vous devez supprimer des données du columnstore. Le fait d’extraire et de tronquer les partitions qui ne sont plus nécessaires est une stratégie efficace pour supprimer les données sans générer la fragmentation introduite par des rowgroups plus petits.

##### <a name="each-partition-can-have-multiple-delta-rowgroups"></a>Chaque partition peut posséder plusieurs rowgroups delta
Chaque partition peut posséder plusieurs rowgroups delta. Quand l’index columnstore doit ajouter des données à un rowgroup delta et que ce dernier est verrouillé, l’index columnstore tente d’obtenir un verrou sur un autre rowgroup delta. Si aucun rowgroup delta n’est disponible, l’index columnstore en crée un. Par exemple, une table comportant 10 partitions peut facilement avoir au moins 20 rowgroups delta. 

#### <a name="you-can-combine-columnstore-and-rowstore-indexes-on-the-same-table"></a>Vous pouvez combiner des index columnstore et rowstore sur la même table
Un index non cluster contient une copie de tout ou partie des lignes et colonnes de la table sous-jacente. L’index est défini comme une ou plusieurs colonnes de la table et a une condition facultative qui filtre les lignes. 

À partir de [!INCLUDE[sssql15-md](../includes/sssql16-md.md)], vous pouvez créer un **index columnstore non-cluster pouvant être mis à jour sur une table rowstore**. L’index columnstore stocke une copie des données, afin que vous n’ayez pas besoin de stockage supplémentaire. Toutefois, les données de l’index columnstore sont compressées à une taille plus petite que ce que nécessite la table rowstore.  Ce faisant, vous pouvez exécuter l’analytique sur l’index columnstore et les transactions sur l’index rowstore en même temps. La banque des colonnes (columnstore) est mise à jour lors de la modification des données de la table rowstore. Les deux index utilisent donc les mêmes données.  
  
À partir de [!INCLUDE[sssql15-md](../includes/sssql16-md.md)], vous pouvez avoir **un ou plusieurs index rowstore non-cluster sur un index columnstore**. Ce faisant, vous pouvez effectuer des recherches de tables efficaces sur le columnstore sous-jacent. D’autres options sont également disponibles. Par exemple, vous pouvez appliquer une contrainte de clé primaire à l’aide d’une contrainte UNIQUE sur la table rowstore. Dans la mesure où une valeur non unique ne peut pas être insérée dans la table rowstore, SQL Server ne peut pas insérer la valeur dans le columnstore.  
 
### <a name="performance-considerations"></a>Considérations relatives aux performances 

-   La définition d’index columnstore non cluster prend en charge l’utilisation d’une condition filtrée. Pour minimiser l’impact sur les performances de l’ajout d’un index columnstore sur une table OLTP, utilisez une condition filtrée pour créer un index columnstore non cluster uniquement sur les données brutes de votre charge de travail opérationnelle. 
  
-   Une table en mémoire peut avoir un index columnstore. Vous pouvez le créer lors de la création de la table ou l’ajouter ultérieurement avec [ALTER TABLE &#40;Transact-SQL&#41;](../t-sql/statements/alter-table-transact-sql.md). Avant [!INCLUDE[sssql15-md](../includes/sssql16-md.md)], seule une table sur disque pouvait avoir un index columnstore. 

Pour plus d’informations, consultez [Index columnstore - Performances des requêtes](../relational-databases/indexes/columnstore-indexes-query-performance.md).

### <a name="design-guidance"></a>Conseils pour la conception 

-   Une table rowstore peut avoir un index columnstore non cluster actualisable. Avant [!INCLUDE[ssSQL14](../includes/sssql14-md.md)], les index columnstore non-cluster étaient en lecture seule.  
 
Pour plus d’informations, consultez [Index columnstore - Guide de conception](../relational-databases/indexes/columnstore-indexes-design-guidance.md).

##  <a name="hash-index-design-guidelines"></a><a name="hash_index"></a> Indications pour la conception d’index de hachage 

Chaque table à mémoire optimisée doit avoir au moins un index, car l’index permet de lier les lignes de la table entre elles. Sur une table optimisée en mémoire, chaque index est également optimisé en mémoire. Les index de hachage sont l’un des types d’index possibles dans une table optimisée en mémoire. Pour plus d’informations, consultez [Index pour les tables à mémoire optimisée](../relational-databases/in-memory-oltp/indexes-for-memory-optimized-tables.md).

**S'applique à**: [!INCLUDE[ssSQL14](../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../includes/sscurrent-md.md)].  

### <a name="hash-index-architecture"></a>Architecture des index de hachage
Un index de hachage se compose d’un tableau de pointeurs, chaque élément du tableau constituant un compartiment de hachage.
- Chaque compartiment comprend 8 octets, qui sont utilisés pour stocker l’adresse mémoire d’une liste de liens d’entrées d’index.  
- Chaque entrée correspond à une valeur pour une clé d’index, plus l’adresse de la ligne associée dans la table mémoire optimisée sous-jacente.  
- Chaque entrée pointe vers l’entrée suivante dans une liste de liens d’entrées, toutes chaînées au compartiment actuel.  

Le nombre de compartiments doit être spécifié au moment de la définition de l’index :
- Plus le rapport entre les compartiments et les lignes de table ou les valeurs distinctes est faible, plus la liste moyenne de liens de compartiments sera longue.  
- Les listes de liens courtes sont plus rapides que les listes de liens longues.
- Un index de hachage peut contenir 1 073 741 824 compartiments au maximum.

> [!TIP]
> Pour déterminer le droit `BUCKET_COUNT` pour vos données, consultez la page [Configuration du nombre de compartiments d’index de hachage](#configuring_bucket_count).

La fonction de hachage est appliquée aux colonnes de clés d’index. Son résultat détermine le compartiment auquel ces clés sont mappées. Chaque compartiment a un pointeur vers les lignes dont les valeurs de clés hachées sont mappées à ce compartiment.

La fonction de hachage utilisée pour les index de hachage présente les caractéristiques suivantes :
- [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] possède une fonction de hachage utilisée pour tous les index de hachage.
- La fonction de hachage est déterministe. La même valeur de clé d’entrée est toujours mappée au même compartiment dans l’index de hachage.
- Plusieurs clés d'index peuvent être mappées au même compartiment de hachage.
- La fonction de hachage est équilibrée, ce qui signifie que la distribution des valeurs de clé d’index sur les compartiments de hachage est généralement une distribution de type Poisson ou courbe en cloche, et pas une distribution linéaire plate.
- La distribution Poisson n'est pas une distribution uniforme. Les valeurs de clé d'index ne sont pas distribuées uniformément dans les compartiments de hachage.
- Si deux clés d’index sont mappées au même compartiment de hachage, il y a une *collision de hachage*. Un grand nombre de collisions de hachage peut avoir un impact sur les performances des opérations de lecture. Un objectif réaliste est que 30 % des compartiments contiennent deux valeurs de clés différentes.
  
L’interaction entre l’index de hachage et les compartiments est résumée dans l’image ci-dessous.  
  
![hekaton_tables_23d](../relational-databases/in-memory-oltp/media/hekaton-tables-23d.png "Clés d’index, entrée dans la fonction de hachage, la sortie est l’adresse d’un compartiment de hachage, qui pointe vers le début de la chaîne.")  

### <a name="configuring-the-hash-index-bucket-count"></a><a name="configuring_bucket_count"></a> Configuration du nombre de compartiments d’index de hachage
Le nombre de compartiments d’index de hachage est spécifié au moment de la création de l’index et peut être modifié à l’aide de la syntaxe `ALTER TABLE...ALTER INDEX REBUILD`.  
  
Dans la plupart des cas, le nombre de compartiments doit être compris entre 1 et 2 fois le nombre de valeurs distinctes dans la clé d’index.   
Vous ne pouvez pas toujours prédire le nombre exact de valeurs qu’une clé d’index donnée a ou aura. Les performances sont en général encore bonnes si la valeur **BUCKET_COUNT** est inférieure ou égale à 10 fois le nombre réel de valeurs de clés et il est généralement préférable de surestimer que de sous-estimer.  
  
Un trop **petit** nombre de compartiments présente les inconvénients suivants :  
  
- Davantage de collisions de hachage de valeurs de clés distinctes.  
- Chaque valeur distincte est forcée de partager le même compartiment avec une autre valeur distincte.  
- La longueur de chaîne moyenne par compartiment augmente.  
- Plus la chaîne de compartiment est longue, plus les recherches d’égalité sont lentes dans l’index.  
  
Un trop **grand** nombre de compartiments présente les inconvénients suivants :  
  
- Un nombre de compartiments trop élevé peut entraîner plus de compartiments vides.  
- Des compartiments vides affectent les performances des analyses d’index complètes. Si celles-ci sont effectuées régulièrement, envisagez de choisir un nombre de compartiments proche du nombre de valeurs de clés distinctes.  
- Des compartiments vides utilisent de la mémoire, même si chaque compartiment utilise seulement 8 octets.  
  
> [!NOTE]
> L’ajout de nouveaux compartiments ne fait rien pour réduire le chaînage groupé des entrées qui partagent une valeur en double. Le taux de duplication de valeurs est utilisé pour déterminer si un hachage constitue le type d’index approprié, et non pour calculer le nombre de compartiments.  

### <a name="performance-considerations"></a>Considérations relatives aux performances  
  
Les performances d’un index de hachage sont :  
  
- excellentes quand le prédicat dans la clause `WHERE` spécifie une valeur **exacte** pour chaque colonne dans la clé d’index de hachage ; Un index de hachage rétablit une analyse en fonction d'un prédicat d'inégalité. 
- médiocres quand le prédicat dans la clause `WHERE` recherche une **plage** de valeurs dans la clé d’index ;  
- médiocres quand le prédicat dans la clause `WHERE` stipule une valeur donnée pour la **première** colonne d’une clé d’index de hachage de deux colonnes, mais ne spécifie pas de valeur pour les **autres** colonnes de la clé.  

> [!TIP]
> Le prédicat doit inclure toutes les colonnes dans la clé d'index de hachage. L'index de hachage nécessite une clé (pour hacher) pour rechercher dans l'index. Si une clé d’index est constituée de deux colonnes et que la clause `WHERE` fournit uniquement la première colonne, [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] ne contient pas de clé complète pour le hachage. Cela génère un plan de requête d'analyse d'index.

Si l’index de hachage utilisé a un nombre de clés d’index uniques 100 fois (ou plus) supérieur au nombre de lignes, vous devez augmenter le nombre de compartiments pour éviter la création de chaînes de lignes trop longues ou bien utiliser un [index non-cluster](#inmem_nonclustered_index) à la place.

### <a name="declaration-considerations"></a><a name="h3-b2-declaration-limitations"></a> Considérations relatives à la déclaration  
Un index de hachage peut exister uniquement sur une table optimisée en mémoire. Il ne peut pas exister sur une table sur disque.  
  
Un index de hachage peut être déclaré comme :  
  
- UNIQUE, ou être défini comme non unique par défaut.  
- NONCLUSTERED, qui est la valeur par défaut.   
  
Le code suivant est un exemple de la syntaxe appropriée pour créer un index de hachage, en dehors de l’instruction CREATE TABLE :  
  
```sql
ALTER TABLE MyTable_memop  
ADD INDEX ix_hash_Column2 UNIQUE  
HASH (Column2) WITH (BUCKET_COUNT = 64);
``` 

### <a name="row-versions-and-garbage-collection"></a>Versions de lignes et garbage collection  
Dans une table à mémoire optimisée, quand une ligne est modifiée par une instruction `UPDATE`, la table crée une version mise à jour de la ligne. Lors de la transaction de mise à jour, d’autres sessions peuvent être en mesure de lire l’ancienne version de la ligne et d’éviter ainsi le ralentissement des performances associé à un verrou de ligne.  
  
L’index de hachage peut également avoir différentes versions de ses entrées pour prendre en charge la mise à jour.  
  
Par la suite, quand les anciennes versions ne sont plus nécessaires, un thread garbage collection (GC) traverse les compartiments et leurs listes de liens pour nettoyer les anciennes entrées. Le thread GC fonctionne mieux si les longueurs de chaîne des listes de liens sont courtes. Pour plus d’informations, consultez [Garbage collection de l’OLTP en mémoire](../relational-databases/in-memory-oltp/in-memory-oltp-garbage-collection.md). 

##  <a name="memory-optimized-nonclustered-index-design-guidelines"></a><a name="inmem_nonclustered_index"></a> Indications pour la conception d’index non-cluster à mémoire optimisée 

Les index non-cluster sont l’un des types d’index possibles dans une table à mémoire optimisée. Pour plus d’informations, consultez [Index pour les tables à mémoire optimisée](../relational-databases/in-memory-oltp/indexes-for-memory-optimized-tables.md).

**S'applique à**: [!INCLUDE[ssSQL14](../includes/sssql14-md.md)] jusqu'à [!INCLUDE[ssCurrent](../includes/sscurrent-md.md)].  

### <a name="in-memory-nonclustered-index-architecture"></a>Architecture des index non-cluster en mémoire

Les index non-cluster en mémoire sont implémentés à l’aide d’une structure de données appelée arbre Bw (Bw-tree). Cette structure a initialement été conçue et décrite par Microsoft Research en 2011. Un arbre Bw est une variante sans verrous d’un arbre B (B-tree). Pour plus d’informations, consultez [The Bw-Tree: A B-tree for New Hardware Platforms](https://www.microsoft.com/research/publication/the-bw-tree-a-b-tree-for-new-hardware/). 

Sur un plan très général, un arbre Bw peut être vu comme un mappage de pages organisées par ID de page (PidMap), un moyen d’allouer et de réutiliser des ID de page (PidAlloc) et un ensemble de pages liées entre elles dans le mappage de pages. Ces trois sous-composants principaux constituent la structure interne de base d’un arbre Bw.

Cet arbre est similaire à un arbre B (B-tree) standard, car chaque page a un ensemble de valeurs de clés ordonnées, l’index a plusieurs niveaux qui pointent chacun vers un niveau inférieur et les niveaux feuille pointent vers une ligne de données. Il y a toutefois quelques différences.

À l’instar des index de hachage, plusieurs lignes de données peuvent être liées entre elles (versions). Les pointeurs de page entre les niveaux sont des ID de page logiques qui indiquent une position dans une table de mappage de pages, laquelle contient l’adresse physique de chaque page.

Il n’y a aucune mise à jour sur place des pages d’index. De nouvelles pages delta sont introduites dans ce but.
-  Aucun verrouillage n’est nécessaire pour les mises à jour de pages.
-  Les pages d’index n’ont pas de taille fixe.

La valeur de clé dans chaque page de niveau non feuille décrite correspond à la plus grande valeur contenue dans l’enfant vers lequel elle pointe, et chaque ligne contient également l’ID de page logique de cette page. Dans les pages de niveau feuille, avec la valeur de clé, elle contient l’adresse physique de la ligne de données.

Les recherches de points sont semblables aux arbres B, mais étant donné que les pages sont liées dans une seule direction, le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] suit les pointeurs de pages droits, où chaque page non feuille a la plus grande valeur de son enfant, au lieu de la plus petite valeur comme dans un arbre B.

Si une page de niveau feuille doit être modifiée, le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] ne la modifie pas directement. Au lieu de cela, le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] crée un enregistrement delta qui décrit la modification et l’ajoute à la page précédente. Ensuite, il change également l’adresse de cette page précédente dans la table de mappage des pages par l’adresse de l’enregistrement delta qui devient alors l’adresse physique de cette page.

La gestion de la structure d’un arbre Bw peut nécessiter trois opérations différentes : la consolidation, le fractionnement et la fusion.

#### <a name="delta-consolidation"></a>Consolidation des enregistrements delta
La présence d’une longue chaîne d’enregistrements delta peut dégrader les performances de recherche, car cela peut signifier que de longues chaînes doivent être parcourues lors de la recherche à l’aide d’un index. Si un nouvel enregistrement delta est ajouté à une chaîne qui contient déjà 16 éléments, les modifications effectuées dans les enregistrements delta sont consolidées dans la page d’index référencée. La page est ensuite regénérée pour inclure les modifications indiquées par le nouvel enregistrement delta ayant déclenché la consolidation. La nouvelle page regénérée a le même ID de page, mais elle a une nouvelle adresse mémoire. 

![hekaton_tables_23e](../relational-databases/in-memory-oltp/media/HKNCI_Delta.gif "Centralisation des enregistrements delta")

#### <a name="split-page"></a>Fractionnement de page
Dans un arbre Bw, une page d’index croît selon les besoins pour stocker une seule ligne jusqu’à un maximum de 8 Ko. Quand la page d’index atteint une taille de 8 Ko, l’insertion d’une nouvelle ligne entraîne le fractionnement de la page d’index. Pour une page interne, cela se produit quand il n’y a plus assez d’espace pour ajouter une autre valeur et le pointeur associé. Pour une page feuille, cela se produit si la ligne est trop longue pour tenir dans la page après l’incorporation de tous les enregistrements delta. Les informations statistiques fournies dans l’en-tête d’une page feuille indiquent la quantité d’espace nécessaire pour consolider les enregistrements delta. Elles sont ajustées après chaque ajout d’un nouvel enregistrement delta. 

Un fractionnement s’effectue en deux étapes atomiques. Dans l’image ci-dessous, l’exemple suppose qu’une page de niveau feuille force un fractionnement après l’insertion d’une clé avec la valeur 5, et qu’il existe une page non feuille pointant vers la fin de la page feuille active (clé avec la valeur 4).

![hekaton_tables_23f](../relational-databases/in-memory-oltp/media/HKNCI_Split.gif "Fractionnement des pages")

**Étape 1 :** Allouez deux nouvelles pages P1 et P2, puis fractionnez les lignes de la page P1 précédente sur ces nouvelles pages, y compris la ligne venant d’être insérée. Dans la table de mappage des pages, un nouvel emplacement est utilisé pour stocker l’adresse physique de la page P2. Ces pages P1 et P2 ne sont pas encore accessibles pour d’autres opérations simultanées. En outre, le pointeur logique entre P1 et P2 est défini. Puis, en une seule étape atomique, mettez à jour la table de mappage des pages pour changer le pointeur de l’ancienne page P1 à la nouvelle page P1. 

**Étape 2 :** La page non feuille pointe vers P1, mais il n’y a pas de pointeur direct entre cette page non feuille et la page P2. P2 est uniquement accessible via P1. Pour créer un pointeur entre une page non feuille et la page P2, allouez une nouvelle page non feuille (page d’index interne), copiez toutes les lignes à partir de l’ancienne page non feuille et ajoutez une nouvelle ligne pour pointer vers P2. Après cela, en une seule étape atomique, mettez à jour la table de mappage des pages pour changer le pointeur de l’ancienne page non feuille à la nouvelle page non feuille.

#### <a name="merge-page"></a>Fusion de page
Quand une opération `DELETE` crée une page d’une taille inférieure à 10 % de la taille de page maximale (qui est de 8 Ko), ou une page contenant une seule ligne, cette page est fusionnée avec une page contiguë.

Quand une ligne est supprimée d’une page, un enregistrement delta correspondant à la suppression est ajouté. De plus, une vérification est effectuée pour déterminer si la page d’index (page non feuille) peut être fusionnée. Cette vérification détermine si l’espace restant après la suppression de la ligne sera inférieur à 10 % de la taille de page maximale. Si la fusion est possible, l’opération de fusion est effectuée en trois étapes atomiques.

Dans l’image ci-dessous, l’exemple suppose qu’une opération `DELETE` va supprimer la valeur de clé 10. 

![hekaton_tables_23g](../relational-databases/in-memory-oltp/media/HKNCI_Merge.gif "Fusion des pages")

**Étape 1 :** Une page delta représentant la valeur de clé 10 (triangle bleu) est créée, et son pointeur dans la page non feuille Pp1 est défini à la nouvelle page delta. De plus, une page delta spécifique pour la fusion (triangle vert) est créée, et est liée pour pointer vers la page delta. À ce stade, les deux pages (page delta et page delta de la fusion) ne sont pas visibles pour les autres transactions simultanées. En une seule étape atomique, le pointeur vers la page de niveau feuille P1 dans la table de mappage des pages est mis à jour pour pointer vers la page delta de la fusion. Après cette étape, l’entrée de la valeur de clé 10 dans la page Pp1 pointe maintenant vers la page delta de la fusion. 

**Étape 2 :** La ligne représentant la valeur de clé 7 dans la page non feuille Pp1 doit être supprimée, et l’entrée de la valeur de clé 10 doit être mise à jour pour pointer vers P1. Pour ce faire, une nouvelle page non feuille Pp2 est allouée, et toutes les lignes de la page Pp1 sont copiées à l’exception de la ligne représentant la valeur de clé 7. Ensuite, la ligne de la valeur de clé 10 est mise à jour pour pointer vers la page P1. Après cela, en une seule étape atomique, l’entrée de la table de mappage des pages pointant vers Pp1 est mise à jour pour pointer vers Pp2. Pp1 n’est plus accessible. 

**Étape 3 :** Les pages de niveau feuille P2 et P1 sont fusionnées, et les pages delta sont supprimées. Pour ce faire, une nouvelle page P3 est allouée, les lignes des pages P2 et P1 sont fusionnées, et les modifications des pages delta sont ajoutées à la nouvelle page P3. Ensuite, en une seule opération atomique, l’entrée de la table de mappage des pages pointant vers la page P1 est mise à jour pour pointer vers la page P3. 

### <a name="performance-considerations"></a>Considérations relatives aux performances

Les performances d’un index non-cluster sont meilleures que celles d’un index de hachage non-cluster lors de l’interrogation d’une table à mémoire optimisée avec des prédicats d’inégalité.

> [!NOTE]
> Une colonne dans une table optimisée en mémoire peut faire partie d'un index de hachage et d'un index non cluster.

> [!TIP]
> Quand un index non-cluster contient des colonnes clés avec un grand nombre de valeurs dupliquées, les performances peuvent baisser lors des mises à jour, des insertions et des suppressions. Une façon d'améliorer les performances dans cette situation consiste à ajouter une autre colonne à l'index non cluster.

##  <a name="additional-reading"></a><a name="Additional_Reading"></a> Lecture supplémentaire  
[CREATE INDEX &#40;Transact-SQL&#41;](../t-sql/statements/create-index-transact-sql.md)    
[ALTER INDEX &#40;Transact-SQL&#41;](../t-sql/statements/alter-index-transact-sql.md)   
[CREATE XML INDEX &#40;Transact-SQL&#41;](../t-sql/statements/create-xml-index-transact-sql.md)  
[CREATE SPATIAL INDEX &#40;Transact-SQL&#41;](../t-sql/statements/create-spatial-index-transact-sql.md)     
[Réorganiser et reconstruire des index](../relational-databases/indexes/reorganize-and-rebuild-indexes.md)         
[Amélioration des performances avec les vues indexées SQL Server 2008](/previous-versions/sql/sql-server-2008/dd171921(v=sql.100))  
[Tables et index partitionnés](../relational-databases/partitions/partitioned-tables-and-indexes.md)  
[Créer des clés primaires](../relational-databases/tables/create-primary-keys.md)    
[Index pour les tables optimisées en mémoire](../relational-databases/in-memory-oltp/indexes-for-memory-optimized-tables.md)  
[Index columnstore - Présentation](../relational-databases/indexes/columnstore-indexes-overview.md)  
[Résolution des problèmes des index de hachage pour les tables à mémoire optimisée](../relational-databases/in-memory-oltp/hash-indexes-for-memory-optimized-tables.md)    
[Vues de gestion dynamique des tables à mémoire optimisée &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/memory-optimized-table-dynamic-management-views-transact-sql.md)   
[Fonctions et vues de gestion dynamique relatives aux index &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/index-related-dynamic-management-views-and-functions-transact-sql.md)       
[Index sur les colonnes calculées](../relational-databases/indexes/indexes-on-computed-columns.md)   
[Index et ALTER TABLE](../t-sql/statements/alter-table-transact-sql.md#indexes-and-alter-table)      
[Adaptive Index Defrag](https://github.com/Microsoft/tigertoolbox/tree/master/AdaptiveIndexDefrag)
