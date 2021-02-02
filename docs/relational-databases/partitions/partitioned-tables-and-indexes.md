---
description: Partitioned Tables and Indexes
title: Tables et index partitionnés | Microsoft Docs
ms.custom: ''
ms.date: 1/5/2021
ms.prod: sql
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- partitioned tables [SQL Server], about partitioned tables
- partitioned indexes [SQL Server], architecture
- partitioned tables [SQL Server], architecture
- partitioned indexes [SQL Server], about partitioned indexes
- index partitions
ms.assetid: cc5bf181-18a0-44d5-8bd7-8060d227c927
author: julieMSFT
ms.author: jrasnick
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 8a8f1575cab349c6c501f6aa98509e8595efeb77
ms.sourcegitcommit: 04d101fa6a85618b8bc56c68b9c006b12147dbb5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99049123"
---
# <a name="partitioned-tables-and-indexes"></a>Partitioned Tables and Indexes
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] prend en charge le partitionnement des tables et des index. Les données des tables et des index partitionnés sont divisées en unités potentiellement réparties sur plusieurs groupes de fichiers d'une base de données. Les données sont partitionnées horizontalement, de sorte que les groupes de lignes sont mappés à des partitions individuelles. Toutes les partitions d'un index ou d'une table unique doivent résider dans la même base de données. La table ou l'index est traité en tant qu'entité logique unique lorsque des requêtes ou des mises à jour sont effectuées sur les données. Dans les versions antérieures à [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)]SP1, les tables et les index partitionnés n’étaient pas disponibles dans toutes les éditions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour obtenir la liste des fonctionnalités prises en charge par les éditions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consultez [Fonctionnalités prises en charge par les éditions de SQL Server 2016](../../sql-server/editions-and-components-of-sql-server-2016.md).  
  
> [!IMPORTANT]  
> [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] prend en charge jusqu'à 15 000 partitions par défaut. Dans les versions antérieures à [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], le nombre de partitions était limité à 1 000 par défaut.  
  
## <a name="benefits-of-partitioning"></a>Avantages du partitionnement  
 Le partitionnement des tables ou des index peut offrir les avantages suivants en matière de gestion et de performances.  
  
-   Vous pouvez créer des sous-ensembles de données et y accéder facilement et efficacement, tout en conservant l'intégrité d'une collection de données. Par exemple, une opération telle que le chargement des données d'un système OLTP vers un système OLAP ne prend que quelques secondes au lieu des minutes et des heures qu'elle exige lorsque les données ne sont pas partitionnées.  
  
-   Vous pouvez effectuer des opérations de maintenance sur une ou plusieurs partitions plus rapidement. Les opérations sont plus efficaces car elles ne ciblent que ces sous-ensembles de données, au lieu de la totalité de la table. Par exemple, vous pouvez choisir de compresser les données dans une ou plusieurs partitions ou de reconstruire une ou plusieurs partitions d'un index.  
  
-   Suivant les types de requêtes fréquemment exécutées et la configuration matérielle, vous pouvez améliorer les performances des requêtes. Par exemple, l'optimiseur de requête peut traiter des requêtes d'équi-jointure entre plusieurs tables partitionnées plus rapidement lorsque les colonnes de partitionnement sont identiques que les colonnes sur lesquelles les tables sont jointes. Consultez [Requêtes](#queries) ci-dessous pour plus d’informations.
  
Lorsque [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] trie des données pour des opérations d'entrée/sortie, il trie d'abord les données par partition. Pour améliorer les performances de tri des données, distribuez les fichiers de données des partitions sur plusieurs disques en définissant un volume RAID. Ainsi, bien que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] trie toujours les données par partition, il peut accéder à tous les lecteurs de chaque partition au même moment.  
  
De plus, il est possible d'améliorer les performances en activant l'escalade de verrous au niveau de la partition plutôt qu'au niveau de la table entière. Cela peut réduire les conflits de verrouillage de la table. Pour réduire les contentions de verrou en autorisant l’escalade de verrous sur la partition, définissez l’option `LOCK_ESCALATION` de l’instruction `ALTER TABLE` avec la valeur AUTO. 

> [!TIP]
> Les partitions d’une table ou d’un index peuvent être placées sur un seul groupe de fichiers (par exemple, le groupe de fichiers `PRIMARY`) ou sur plusieurs. Si vous utilisez le stockage hiérarchisé, la présence de plusieurs groupes de fichiers offre la possibilité d’attribuer des partitions spécifiques selon le niveau de stockage. Tous les autres avantages liés au partitionnement s’appliquent indépendamment du nombre de groupes de fichiers utilisés ou du placement des partitions sur des groupes de fichiers spécifiques.
  
## <a name="components-and-concepts"></a>Composants et concepts  
Les termes suivants s'appliquent aux partitionnement de table et d'index.  
  
### <a name="partition-function"></a>Fonction de partition  
Objet de base de données qui définit comment les lignes d’une table ou d’un index sont mappées à un ensemble de partitions en fonction des valeurs d’une certaine colonne, appelée « colonne de partitionnement ». Chaque valeur de la colonne de partitionnement est une entrée de la fonction de partitionnement, qui retourne une valeur de partition. La fonction de partition définit le nombre de partitions et les limites de partition qu’utilise la table. Prenons l’exemple d’une table qui contient des données de commande client ; vous pouvez partitionner la table en douze partitions (mensuellement) en fonction d’une colonne **datetime** , telle qu’une date de vente.  
  
### <a name="partition-scheme"></a>Schéma de partition 
Objet de base de données qui mappe les partitions d'une fonction de partition à un ensemble de groupes de fichiers. Le principal motif de placement des partitions sur des groupes de fichiers distincts est la possibilité de réaliser des opérations de sauvegarde indépendantes sur les partitions. En effet, vous pouvez réaliser des sauvegardes sur des groupes de fichiers spécifiques.  

> [!NOTE]
> Dans Azure [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)], seuls les groupes de fichiers primaires sont pris en charge.  
  
### <a name="partitioning-column"></a>Colonne de partitionnement  
Colonne d'une table ou d'un index utilisée par une fonction de partition pour partitionner la table ou l'index. Les colonnes calculées qui font partie d'une fonction de partition doivent présenter l'attribut PERSISTED. Tous les types de données autorisés dans les colonnes d'index peuvent être utilisés dans la colonne de partitionnement, sauf **timestamp**. Les types de données **ntext**, **text**, **image**, **xml**, **varchar(max)** , **nvarchar(max)** ou **varbinary(max)** ne peuvent pas être spécifiés. Le type défini par l’utilisateur CLR (Common Langage Runtime) Microsoft .NET Framework et les colonnes de type de données alias ne peuvent pas être non plus spécifiés.  
  
### <a name="aligned-index"></a>Index aligné  
Index créé sur le même schéma de partition que la table qui lui correspond. Lorsqu'une table et ses index sont alignés, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut commuter rapidement et efficacement les partitions tout en préservant leur structure aussi bien dans la table que dans les index. Un index n'a pas besoin de participer à la même fonction de partition nommée pour être aligné avec sa table de base. Toutefois, la fonction de partition de l’index et la table de base doivent être essentiellement les mêmes en ceci :
 1. Les arguments des fonctions de partition ont le même type de données.
 2. Ils définissent le même nombre de partitions.
 3. Ils définissent les mêmes valeurs limites pour les partitions.  

#### <a name="partitioning-clustered-indexes"></a>Partitionnement des index cluster
Lors du partitionnement d'un index cluster, la clé de clustering doit contenir la colonne de partitionnement. Lors du partitionnement d'un index cluster non unique, alors que la colonne de partitionnement n'est pas spécifiée explicitement dans la clé de clustering, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ajoute cette colonne par défaut à la liste des clés d'index cluster. Si l'index cluster est unique, vous devez spécifier explicitement que la clé d'index cluster contient la colonne de partitionnement. Pour plus d’informations sur les index cluster et l’architecture des index, consultez [Instructions de conception d’index cluster](../../relational-databases/sql-server-index-design-guide.md#Clustered).       

#### <a name="partitioning-nonclustered-indexes"></a>Partitionnement d'index non-cluster
Lors du partitionnement d'un index non-cluster unique, la clé d'index doit contenir la colonne de partitionnement. Lors du partitionnement d’un index non-cluster non unique, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ajoute la colonne de partitionnement par défaut comme colonne non-clé (incluse) de l’index pour garantir que l’index est aligné avec la table de base. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] n’ajoute pas la colonne de partitionnement à l’index si elle y figure déjà. Pour plus d’informations sur les index non cluster et l’architecture des index, consultez [Instructions de conception d’index non cluster](../../relational-databases/sql-server-index-design-guide.md#Nonclustered).

### <a name="non-aligned-index"></a>Index non aligné  
Index partitionné indépendamment de la table correspondante. Autrement dit, l'index a un schéma de partition différent ou il est placé dans un groupe de fichiers différent de la table de base. La conception d’un index partitionné non aligné peut être utile dans les cas suivants :  
-   la table de base n'a pas été partitionnée ;  
-   la clé d'index est unique et elle ne doit pas contenir la colonne de partitionnement de la table ;  
-   vous souhaitez que la table de base soit impliquée dans des jointures communes à plusieurs tables en utilisant différentes colonnes de jointure.  

### <a name="partition-elimination"></a>Élimination de partition
Processus par lequel l'optimiseur de requête accède uniquement aux partitions pertinentes pour satisfaire les critères de la requête.  

## <a name="performance-guidelines"></a>Recommandations relatives aux performances  
 La nouvelle limite plus élevée de 15 000 partitions affecte la mémoire, les opérations d'index partitionnés, les commandes DBCC et les requêtes. Cette section décrit les implications en matière de performances de l'augmentation du nombre de partitions au-delà de 1000 et fournit des solutions de contournement si nécessaire. La quantité maximale de partitions étant passée à 15 000, vous pouvez stocker des données pendant plus longtemps. Toutefois, vous devez conserver les données uniquement pendant la durée nécessaire et obtenir un compromis entre les performances et le nombre de partitions.  
  
### <a name="processor-cores-and-number-of-partitions-guidelines"></a>Recommandations concernant les cœurs de processeur et le nombre de partitions  
 Pour optimiser les performances liées aux opérations parallèles, nous vous recommandons d’utiliser le même nombre de partitions que de cœurs de processeur, à hauteur de 64 (cette valeur correspondant au nombre maximal de processeurs parallèles que peut utiliser [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]).  
  
### <a name="memory-usage-and-guidelines"></a>Utilisation de la mémoire et recommandations  
 Nous vous recommandons d'utiliser au moins 16 Go de RAM si un grand nombre de partitions sont en cours d'utilisation. Si le système n'a pas assez de mémoire, les instructions DML (Data Manipulation Language), les instructions DDL (Data Definition Language) et d'autres opérations peuvent échouer en raison d'une insuffisance de mémoire. Les systèmes avec 16 Go de RAM qui exécutent un grand nombre de processus nécessitant beaucoup de mémoire risque de ne pas disposer de suffisamment de mémoire lors des opérations qui s'exécutent sur un grand nombre de partitions. Par conséquent, plus vous disposez de mémoire au-delà de 16 Go, moins vous risquez de rencontrer des problèmes de performances et de mémoire.  
  
 Les limitations de mémoire peuvent affecter les performances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou sa capacité à créer un index partitionné. C'est le cas notamment lorsque l'index n'est pas aligné avec sa table de base ou son index cluster, si un index cluster a été appliqué à la table. Dans ce cas, il peut être utile d’augmenter l’option de configuration Création d’index en mémoire. Pour plus d’informations, consultez [Configurer l’option de configuration Création d’index en mémoire](../../database-engine/configure-windows/configure-the-index-create-memory-server-configuration-option.md). 
  
### <a name="partitioned-index-operations"></a>Opérations d'index partitionné  
La création et la reconstruction des index non alignés sur une table contenant plus de 1 000 partitions sont possibles, mais ne sont pas prises en charge. Ces opérations peuvent entraîner une dégradation des performances ou une consommation de mémoire excessive.  
  
La création et la reconstruction d'index alignés peuvent exiger davantage de temps à mesure que le nombre de partitions augmente. Nous vous recommandons de ne pas exécuter simultanément plusieurs commandes de création et de reconstruction d'index, car vous risquez de rencontrer des problèmes de performances et de mémoire.  
  
 Lorsque [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] effectue un tri pour créer des index partitionnés, il commence par créer une table de tri pour chaque partition. Ensuite, il produit les tables de tri soit dans le groupe de fichiers de chaque partition, soit dans **tempdb**, si l’option d’index SORT_IN_TEMPDB est spécifiée. La création de chaque table de tri nécessite une quantité minimale de mémoire. Lorsque vous créez un index partitionné qui est aligné avec sa table de base, les tables de tri sont créées une par une, ce qui utilise moins de mémoire. Toutefois, lorsque vous créez un index partitionné non aligné, les tables de tri sont produites en même temps. De ce fait, il doit y avoir assez de mémoire pour gérer ces tri simultanés. Plus il y a de partitions, plus il faut de mémoire. La taille minimale pour chaque table de tri, pour chaque partition, est de 40 pages, à raison de 8 kilo-octets par page. Par exemple, un index partitionné non aligné avec 100 partitions nécessite une quantité de mémoire suffisante pour trier en série 4 000 (40 * 100) pages à la fois. Si cette mémoire est disponible, l'opération de création réussit, mais les performances risquent d'en pâtir. Sinon, la création échoue. À l'inverse, un index partitionné aligné avec 100 partitions n'a besoin que de la mémoire suffisante pour trier 40 pages, parce que les tris ne sont pas effectués en même temps.  
  
 Pour les deux types d’index, alignés et non alignés, la mémoire requise peut être beaucoup plus importante si [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] applique divers degrés de parallélisme à l’opération de création sur un ordinateur multiprocesseur. En effet, plus il y a de degrés de parallélisme, plus il faut de mémoire. Par exemple, si [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] définit les degrés de parallélisme avec la valeur 4, un index partitionné non aligné avec 100 partitions a besoin d'une quantité de mémoire suffisante pour quatre processeurs pour trier 4 000 pages à la fois, ou 16 000 pages. Si l'index partitionné est aligné, la mémoire requise est moins importante puisqu'il en faut pour quatre processeurs triant 40 pages ou 160 (4 * 40) pages. Vous pouvez utiliser l'option d'index MAXDOP pour réduire manuellement les degrés de parallélisme.  

### <a name="dbcc-commands"></a>Commandes DBCC  
Avec un plus grand nombre de partitions, l'exécution des commandes DBCC peut exiger davantage de temps à mesure que le nombre de partitions augmente.  
  
### <a name="queries"></a>Requêtes  
Les requêtes qui utilisent l'élimination de partition peuvent présenter des performances comparables ou meilleures avec un plus grand nombre de partitions. Les requêtes qui n'utilisent pas l'élimination de partition peuvent être plus longues à mesure que le nombre de partitions augmente.  
  
Par exemple, supposons qu'une table a 100 millions de lignes et de colonnes `A`, `B`et `C`. 
-  Dans le scénario 1, la table est divisée en 1 000 partitions sur la colonne `A`. 
-  Dans le scénario 2, la table est divisée en 10 000 partitions sur la colonne `A`. Une requête sur la table qui contient une clause `WHERE` filtrant sur la colonne `A` effectue une élimination de partition et analyse une partition. Il se peut que cette même requête s'exécute plus rapidement dans le scénario 2 car il y a moins de lignes à analyser dans une partition. Une requête qui contient une clause `WHERE` filtrant sur la colonne B analyse toutes les partitions. Il se peut que cette requête s'exécute plus rapidement dans le scénario 1 que dans le scénario 2 car il y a moins de partitions à analyser.  

Les requêtes qui utilisent des opérateurs tels que TOP ou MAX/MIN sur des colonnes autres que la colonne de partitionnement peuvent enregistrer une baisse des performances lors du partitionnement, du fait que toutes les partitions doivent être évaluées.  

Si vous exécutez fréquemment des requêtes qui impliquent une équijointure entre au moins deux tables partitionnées, leurs colonnes de partitionnement doivent être les mêmes que celles par lesquelles les tables sont jointes. En outre, les tables, ou leurs index, doivent subir une colocation. Cela signifie qu'ils utilisent la même fonction de partition nommée ou des fonctions de partition nommée différentes mais fondamentalement similaires en ce sens qu'elles :
-  possèdent le même nombre de paramètres utilisés pour le partitionnement et que les types de données des paramètres correspondants sont les mêmes ;
-  définissent le même nombre de partitions ;
-  définissent les mêmes valeurs limites pour les partitions.
Ainsi, l’optimiseur de requête peut traiter la jointure plus rapidement car les partitions elles-mêmes peuvent être jointes. Si une requête joint deux tables qui n'ont pas subi de colocation ou qui ne sont pas partitionnées sur le champ de jointure, la présence de partitions peut réellement ralentir le traitement des requêtes au lieu de l'accélérer.

Pour plus d’informations sur la gestion des partitions dans le traitement des requêtes, consultez [Améliorations apportées au traitement des requêtes sur les tables et les index partitionnés](../../relational-databases/query-processing-architecture-guide.md#query-processing-enhancements-on-partitioned-tables-and-indexes).

## <a name="behavior-changes-in-statistics-computation-during-partitioned-index-operations"></a>Changements de comportement dans le calcul des statistiques pour les opérations d’index partitionnés  
 À partir de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], les statistiques ne sont pas créées en analysant toutes les lignes de la table quand un index partitionné est créé ou reconstruit. Au lieu de cela, l'optimiseur de requête utilise l'algorithme d'échantillonnage par défaut pour générer des statistiques. Après la mise à niveau d'une base de données avec des index partitionnés, vous pouvez remarquer une différence dans les données d'histogramme pour ces index. Cette modification du comportement peut ne pas affecter les performances des requêtes. Pour obtenir des statistiques sur les index partitionnés en analysant toutes les lignes de la table, utilisez `CREATE STATISTICS` ou `UPDATE STATISTICS` avec la clause `FULLSCAN`.  
  
## <a name="related-tasks"></a>Tâches associées  
  
|Tâches|Rubrique|  
|-|-|   
|Décrit comment créer des fonctions de partition et des schémas de partition et les appliquer ensuite à une table ou à un index.|[Créer des tables et des index partitionnés](../../relational-databases/partitions/create-partitioned-tables-and-indexes.md)|  
|||  
  
## <a name="related-content"></a>Contenu associé  
 Les livres blancs suivants relatifs aux stratégies et implémentations de tables et index partitionnés pourront se révéler utiles.  
-   [Stratégies de tables et d’index partitionnés avec SQL Server 2008](https://msdn.microsoft.com/library/dd578580\(SQL.100\).aspx)    
-   [Comment implémenter une fenêtre glissante automatique](https://msdn.microsoft.com/library/aa964122\(SQL.90\).aspx)    
-   [Chargement en masse dans une table partitionnée](/previous-versions/sql/sql-server-2005/administrator/cc966380(v=technet.10))    
-   [Projet REAL : Cycle de vie de données -- Partitionnement](/previous-versions/sql/sql-server-2005/administrator/cc966424(v=technet.10))    
-   [Améliorations du traitement des requêtes sur les tables et les index partitionnés](/previous-versions/sql/sql-server-2008-r2/ms345599(v=sql.105))    
-   [Top 10 Best Practices for Building a Large Scale Relational Data Warehouse](https://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/SQLCAT's%20Guide%20to%20Relational%20Engine.pdf) dans _SQLCAT's Guide to: Relational Engineering_
  
