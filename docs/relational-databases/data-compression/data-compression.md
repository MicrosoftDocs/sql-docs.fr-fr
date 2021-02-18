---
title: Compression de données | Microsoft Docs
description: Appliquez la compression des données de ligne et de page ou la compression columnstore et d’archivage columnstore, à l’aide de SQL Server et d’Azure SQL Database.
ms.custom: ''
ms.date: 02/11/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- page compression [Database Engine]
- indexes [SQL Server], compressed
- compressed indexes [SQL Server]
- storage compression [Database Engine]
- tables [SQL Server], compressed
- storage [SQL Server], compressed
- compression [SQL Server]
- row compression [Database Engine]
- compression [SQL Server], about compressed tables and indexes
- data compression [Database Engine]
- compressed tables [SQL Server]
ms.assetid: 5f33e686-e115-4687-bd39-a00c48646513
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: b8f28c1d2aaa3e1e1e431feaf265d7b1bc22f9d9
ms.sourcegitcommit: c83c17e44b5e1e3e2a3b5933c2a1c4afb98eb772
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/15/2021
ms.locfileid: "100525189"
---
# <a name="data-compression"></a>Compression des données

[!INCLUDE [sql-asdb-asdbmi](../../includes/applies-to-version/sql-asdb-asdbmi.md)]

[!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)], [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] et [!INCLUDE [sssdsmifull-md](../../includes/sssdsmifull-md.md)] prennent en charge la compression de page et de ligne pour les tables et les index rowstore, et prennent en charge la compression columnstore et d’archivage columnstore pour les tables et les index columnstore.  
  
 Pour les tables et les index rowstore, utilisez la fonctionnalité de compression de données afin de réduire la taille de la base de données. Outre les économies d'espace, la compression des données peut améliorer les performances des charges de travail nécessitant de nombreuses E/S, car les données sont stockées dans beaucoup moins de pages et les requêtes doivent lire moins pages sur le disque. Toutefois, des ressources processeur supplémentaires sont nécessaires sur le serveur de base de données pour compresser et décompresser les données, alors que les données sont échangées avec l'application. La compression des lignes et des pages peut être configurée pour les objets de base de données suivants :   
-   Une table entière stockée en tant que segment de mémoire.  
-   Une table entière stockée en tant qu'index cluster.  
-   Un index non cluster entier.  
-   Une vue indexée entière.  
-   Pour les tables et les index partitionnés, l'option de compression peut être configurée pour chaque partition et les différentes partitions d'un objet n'ont pas le même paramètre de compression.  
  
Pour les tables et les index columnstore, tous utilisent toujours la compression columnstore et celle-ci n'est pas configurable par l'utilisateur. Utilisez la compression d'archivage columnstore pour limiter davantage la taille des données dans les cas où vous pouvez supporter du temps et des ressources processeur supplémentaires pour stocker et récupérer les données.  La compression d'archivage columnstore peut être configurée pour les objets de base de données suivants :  
-   Une table columnstore entière ou un index cluster columnstore entier.  Étant donné qu'une table columnstore est stockée en tant qu'index cluster columnstore, les deux méthodes ont les mêmes résultats.  
-   Un index columnstore non cluster entier.  
-   Pour les tables et les index columnstore partitionnés, l'option de compression d'archivage peut être configurée pour chaque partition et les différentes partitions d'un objet n'ont pas le même paramètre de compression d'archivage.  
  
> [!NOTE]  
> Les données peuvent également être compressées à l’aide du format d’algorithme GZIP. Cette étape supplémentaire se révèle particulièrement adaptée pour la compression de portions de données pendant l’archivage d’anciennes données à des fins de stockage à long terme. Les données compressées avec la fonction `COMPRESS` ne peuvent pas être indexées. Pour plus d’informations, consultez [COMPRESS &#40;Transact-SQL&#41;](../../t-sql/functions/compress-transact-sql.md).  
  
## <a name="row-and-page-compression"></a>Compression de ligne et de page

En cas d'utilisation de la compression de page et de ligne, assurez-vous de prendre en compte les considérations suivantes :  
-   Les détails de la compression des données sont susceptibles de changer sans information préalable dans les Service Packs ou les versions ultérieures.
-   La compression est disponible dans [!INCLUDE[ssSDSfull_md](../../includes/sssdsfull-md.md)]  
-   La compression est disponible dans chaque édition de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour plus d’informations, consultez [Fonctionnalités prises en charge par les éditions de SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md).  
-   La compression n'est pas disponible pour les tables système.  
-   La compression permet de stocker davantage de lignes dans une page, mais elle ne modifie pas la taille maximale des lignes d'une table ou d'un index.  
-   Une table ne peut pas être activée pour la compression lorsque la taille de ligne maximale plus la charge mémoire de compression dépasse la taille de ligne maximale de 8 060 octets. Par exemple, une table qui comporte les colonnes `c1 CHAR(8000)` et `c2 CHAR(53)` ne peut pas être compressée en raison de la charge de compression supplémentaire. Lorsque le format de stockage VarDecimal est utilisé, le contrôle de taille de ligne est effectué lorsque le format est activé. Pour la compression de ligne et de page, le contrôle de taille de ligne est effectué lorsque l'objet est compressé initialement, puis vérifié à mesure que chaque ligne est insérée ou modifiée. La compression met en vigueur les deux règles suivantes :  
    -   Une mise à jour d'un type de longueur fixe doit toujours réussir.  
    -   La désactivation de la compression de données doit toujours réussir. Même si la ligne compressée est adaptée à la taille de la page, ce qui signifie qu'elle est inférieure à 8 060 octets, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] empêche les mises à jour qui ne seraient pas adaptées à la taille de la ligne lorsqu'elle est décompressée.  
-   Lorsqu'une liste de partitions est spécifiée, le type de compression peut être défini sur ROW, PAGE ou NONE sur chacune des partitions. Si la liste de partitions n'est pas spécifiée, toutes les partitions sont définies avec la propriété de compression de données spécifiée dans l'instruction. Lorsqu'une table ou un index est créé, la compression de données est définie sur NONE, sauf indication contraire. Lorsqu'une table est modifiée, la compression existante est conservée, sauf indication contraire.  
-   Si vous spécifiez une liste de partitions ou une partition hors limites, une erreur est générée.  
-   Les index non cluster n'héritent pas de la propriété de compression de la table. Pour compresser des index, vous devez définir explicitement la propriété de compression des index. Par défaut, le paramètre de compression des index est défini sur NONE quand l’index est créé.  
-   Lorsqu'un index cluster est créé sur un segment de mémoire, l'index cluster hérite de l'état de compression du segment, à moins qu'un autre état de compression soit spécifié.  
-   Lorsqu'un segment de mémoire est configuré pour la compression de niveau page, les pages reçoivent la compression de niveau page uniquement des manières suivantes :  
    -   Les données pour lesquelles l'optimisation en bloc est activée sont importées.  
    -   Les données sont insérées avec la syntaxe `INSERT INTO ... WITH (TABLOCK)` et la table n’a pas d’index non-cluster.  
    -   Une table est reconstruite en exécutant l’instruction `ALTER TABLE ... REBUILD` avec l’option de compression PAGE.  
-   Les changements de page alloués dans un segment de mémoire dans le cadre des opérations DML n’utilisent pas la compression PAGE tant que le segment de mémoire n’est pas reconstruit. Reconstruisez le segment de mémoire en supprimant puis en réappliquant la compression, ou en créant et en supprimant un index cluster.  
-   La modification du paramètre de compression d'un segment de mémoire nécessite la reconstruction de tous les index non cluster sur la table afin qu'ils aient des pointeurs vers les nouveaux emplacements de ligne dans le segment de mémoire.  
-   Vous pouvez activer ou désactiver la compression ROW ou PAGE en ligne ou hors connexion. L'activation de la compression sur un segment de mémoire est mono-thread pour une opération en ligne.  
-   L'espace disque nécessaire pour activer ou désactiver la compression de ligne ou de page est le même que pour créer ou reconstruire un index. Pour les données partitionnées, vous pouvez réduire l'espace requis en activant ou désactivant la compression pour une partition à la fois.  
-   Pour déterminer l'état de compression des partitions dans une table partitionnée, interrogez la colonne data_compression de l'affichage catalogue sys.partitions.  
-   Lorsque vous compressez des index, les pages de niveau feuille peuvent être compressées avec la compression de ligne et de page. Les pages de niveau non feuille ne reçoivent pas de compression de page.  
-   À cause de leur taille, les types de données de grande valeur sont quelquefois stockés séparément des données de ligne normales sur des pages à fonction spéciale. La compression des données n'est pas disponible pour les données stockées séparément.  
-   Les tables qui implémentaient le format de stockage VarDecimal dans [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] conservent ce paramètre en cas de mise à niveau. Vous pouvez appliquer la compression de ligne à une table qui a le format de stockage VarDecimal. Toutefois, la compression de ligne étant un sur-ensemble du format de stockage VarDecimal, il n'y a aucune raison de conserver le format de stockage VarDecimal. Les valeurs décimales ne bénéficient d'aucune compression supplémentaire lorsque vous combinez le format de stockage VarDecimal avec la compression de ligne. Vous pouvez appliquer la compression de page à une table qui a le format de stockage VarDecimal ; toutefois, les colonnes au format de stockage VarDecimal ne bénéficieront probablement pas d'une compression supplémentaire.  
  
    > [!NOTE]  
    > Toutes les versions prises en charge de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] prennent en charge le format de stockage vardecimal ; cependant, la compression des données ayant les mêmes objectifs, le format de stockage vardecimal est déprécié. [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]  
  
## <a name="columnstore-and-columnstore-archive-compression"></a>Compression columnstore et compression d’archivage columnstore  

Les tables et les index columnstore sont toujours enregistrés avec la compression columnstore. Limitez encore davantage la taille des données columnstore en configurant une compression supplémentaire appelée compression d'archivage.  Pour exécuter la compression d'archivage, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] exécute l'algorithme de compression Microsoft XPRESS sur les données. Ajoutez ou supprimez la compression d'archivage en utilisant les types de compression de données suivants :  
-   Utilisez la compression des données **COLUMNSTORE_ARCHIVE** pour compresser les données columnstore au moyen de la compression d’archivage.  
-   Utilisez la compression des données **COLUMNSTORE** pour décompresser la compression d'archivage. Les données résultantes continuent à être compressées au moyen de la compression columnstore.  
  
Pour ajouter la compression d’archivage, utilisez [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md) ou [ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md) avec l’option REBUILD et DATA COMPRESSION = COLUMNSTORE_ARCHIVE.  
  
Par exemple :

```sql  
ALTER TABLE ColumnstoreTable1   
REBUILD PARTITION = 1 WITH (DATA_COMPRESSION =  COLUMNSTORE_ARCHIVE) ;  
  
ALTER TABLE ColumnstoreTable1   
REBUILD PARTITION = ALL WITH (DATA_COMPRESSION =  COLUMNSTORE_ARCHIVE) ;  
  
ALTER TABLE ColumnstoreTable1   
REBUILD PARTITION = ALL WITH (DATA_COMPRESSION =  COLUMNSTORE_ARCHIVE ON PARTITIONS (2,4)) ;  
```  
  
Pour supprimer la compression d’archivage et restaurer les données sur la compression columnstoue, utilisez [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md) ou [ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md) avec l’option REBUILD et DATA COMPRESSION = COLUMNSTORE.  

Par exemple :

```sql  
ALTER TABLE ColumnstoreTable1   
REBUILD PARTITION = 1 WITH (DATA_COMPRESSION =  COLUMNSTORE) ;  
  
ALTER TABLE ColumnstoreTable1   
REBUILD PARTITION = ALL WITH (DATA_COMPRESSION =  COLUMNSTORE) ;  
  
ALTER TABLE ColumnstoreTable1   
REBUILD PARTITION = ALL WITH (DATA_COMPRESSION =  COLUMNSTORE ON PARTITIONS (2,4) ) ;  
```  
  
L'exemple suivant définit la compression des données à columnstore sur certaines partitions, et à archivage columnstore sur d'autres.  
  
```sql  
ALTER TABLE ColumnstoreTable1   
REBUILD PARTITION = ALL WITH (  
    DATA_COMPRESSION =  COLUMNSTORE ON PARTITIONS (4,5),  
    DATA COMPRESSION = COLUMNSTORE_ARCHIVE ON PARTITIONS (1,2,3)  
) ;  
```  
  
### <a name="performance"></a>Performances

 La compression des index columnstore avec compression d’archivage affecte les performances des index par rapport aux index columnstore sans compression d’archivage. Utilisez la compression d'archivage uniquement lorsque vous pouvez vous permettre d'utiliser du temps et les ressources supplémentaires pour compresser et récupérer les données.  
  
 La compression d’archivage offre l’avantage d’un stockage réduit, ce qui est utile pour les données auxquelles vous n’accédez pas fréquemment. Par exemple, si vous disposez d'une partition pour chaque mois de données, et la majeure partie de votre activité est réalisée au cours des mois les plus récents, vous pouvez archiver les mois antérieurs afin de réduire les besoins de stockage.  
  
### <a name="metadata"></a>Métadonnées

Les vues système suivantes contiennent des informations sur la compression de données pour les index cluster :  
-   [sys.indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md) - Les colonnes **type** et **type_desc** incluent CLUSTERED COLUMNSTORE et NONCLUSTERED COLUMNSTORE.  
-   [sys.partitions &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-partitions-transact-sql.md) - Les colonnes **data_compression** et **data_compression_desc** incluent COLUMNSTORE et COLUMNSTORE_ARCHIVE.  
  
La procédure [sp_estimate_data_compression_savings &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-estimate-data-compression-savings-transact-sql.md) peut aussi s’appliquer aux index columnstore.  
  
## <a name="impact-on-partitioned-tables-and-indexes"></a>Impact sur les tables et les index partitionnés

 Lorsque vous utilisez la compression des données avec des tables et des index partitionnés, assurez-vous de prendre en compte les considérations suivantes :  
-   Quand des partitions sont divisées avec l’instruction `ALTER PARTITION`, les deux partitions héritent de l’attribut de compression des données de la partition d’origine.  
-   Lorsque deux partitions sont fusionnées, la partition résultante hérite de l'attribut de compression des données de la partition de destination.  
-   Pour changer une partition, la propriété de compression des données de la partition doit correspondre à la propriété de compression de la table.  
-   Il existe deux variantes de syntaxe que vous pouvez utiliser pour modifier la compression d'une table ou d'un index partitionné :  
    -   La syntaxe suivante reconstruit uniquement la partition référencée :  
    
        ```sql  
        ALTER TABLE <table_name>   
        REBUILD PARTITION = 1 WITH (DATA_COMPRESSION =  <option>)  
        ```  
    
    -   La syntaxe suivante reconstruit la table entière en utilisant le paramètre de compression existant pour toute partition qui n'est pas référencée :  
    
        ```sql  
        ALTER TABLE <table_name>   
        REBUILD PARTITION = ALL   
        WITH (DATA_COMPRESSION = PAGE ON PARTITIONS(<range>),  
        ... )  
        ```  
  
     Les index partitionnés suivent le même principe avec `ALTER INDEX`.  
  
-   Lorsqu'un index cluster est supprimé, les partitions des segments de mémoire correspondants conservent leur paramètre de compression des données, à moins que le schéma de partitionnement soit modifié. Si le schéma de partitionnement est modifié, toutes les partitions sont reconstruites dans un état non compressé. Pour supprimer un index cluster et modifier le schéma de partitionnement, vous devez effectuer les opérations suivantes :  
     1. supprimer l'index cluster ;  
     2. modifier la table avec l’option `ALTER TABLE ... REBUILD` qui spécifie l’option de compression.  
  
     La suppression d'un index cluster HORS CONNEXION est une opération très rapide, car seuls les niveaux supérieurs des index clusters sont supprimés. Lorsqu'un index cluster est supprimé EN LIGNE, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doit reconstruire le segment de mémoire deux fois, une fois pour l'étape 1 et une fois pour l'étape 2.  
  
## <a name="how-compression-affects-replication"></a>Impact de la compression sur la réplication

Lorsque vous utilisez la compression des données avec la réplication, assurez-vous de prendre en compte les considérations suivantes :  
-   Quand l’Agent d’instantané génère le script de schéma initial, le nouveau schéma utilise les mêmes paramètres de compression pour la table et ses index. La compression ne peut être activée simplement sur la table et pas sur l'index.  
-   Pour la réplication transactionnelle, l'option de schéma d'article détermine les objets et propriétés dépendants qui doivent être écrits. Pour plus d’informations, consultez [sp_addarticle](../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md).  
     L'Agent de distribution ne vérifie pas les Abonnés de bas niveau lorsqu'il applique des scripts. Si la réplication de compression est sélectionnée, la création de la table sur des Abonnés de bas niveau échoue. Dans le cas d'une topologie mixte, n'activez pas la réplication de compression.  
-   Pour la réplication de fusion, le niveau de compatibilité de la publication remplace les options de schéma et détermine les objets de schéma qui sont écrits.  
     Dans le cas d'une topologie mixte, s'il n'est pas obligatoire de prendre en charge les nouvelles options de compression, le niveau de compatibilité de la publication doit être défini sur la version de l'Abonné de bas niveau. Si la prise en charge est requise, compressez les tables sur l'Abonné après qu'elles ont été créées.  
  
Le tableau suivant illustre les paramètres de réplication qui contrôlent la compression pendant la réplication.  
  
|Intention de l'utilisateur|Répliquer le schéma de partition pour une table ou un index|Répliquer les paramètres de compression|Comportement de script|  
|-----------------|-----------------------------------------------------|------------------------------------|------------------------|  
|Répliquer le schéma de partition et activer la compression sur l'Abonné sur la partition.|True|True|Inclut dans le script le schéma de partition et les paramètres de compression.|  
|Répliquer le schéma de partition mais ne pas compresser les données sur l'Abonné.|True|Faux|Exclut le schéma de partition du script, mais pas les paramètres de compression pour la partition.|  
|Ne pas répliquer le schéma de partition et ne pas compresser les données sur l'Abonné.|Faux|False|Exclut du script la partition et les paramètres de compression.|  
|Compresser la table sur l'Abonné si toutes les partitions sont compressées sur le serveur de publication, mais ne pas répliquer le schéma de partition.|False|True|Vérifie si toutes les partitions sont activées pour la compression.<br /><br /> Exclut la compression du script au niveau de la table.|  
  
## <a name="impact-on-other-sql-server-components"></a>Impact sur d’autres composants SQL Server

[!INCLUDE [sql-asdb-asdbmi](../../includes/applies-to-version/sql-asdb-asdbmi.md)]
   
 La compression se produit dans le moteur de stockage et les données sont présentées à la plupart des autres composants de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans un état non compressé. Cela limite les effets de la compression sur les autres composants de la manière suivante :  
-   Opérations d'importation et d'exportation en bloc  
     Lorsque des données sont exportées, même au format natif, les données sont sorties au format de ligne non compressé. La taille du fichier de données exporté peut par conséquent être beaucoup plus grande que les données sources.  
     Lorsque des données sont importées, si la table cible a été activée pour la compression, les données sont converties par le moteur de stockage au format de ligne compressé. Cela peut provoquer une augmentation de l'utilisation de l'UC, par rapport à une importation des données dans une table non compressée.  
     Quand des données sont importées en bloc dans un segment de mémoire avec la compression de page, l’opération d’importation en bloc tente de compresser les données avec la compression de page quand les données sont insérées.  
-   La compression n'affecte pas la sauvegarde et la restauration.  
-   La compression n'affecte pas la copie des journaux de transaction.  
-   La compression de données est incompatible avec les colonnes éparses. Par conséquent, les tables contenant des colonnes éparses ne peuvent pas être compressées et les colonnes éparses ne peuvent pas être ajoutées aux tables compressées.  
-   L'activation de la compression peut provoquer la modification des plans de requêtes, car les données sont stockées avec un nombre différent de pages et de lignes par page.  
  
## <a name="see-also"></a>Voir aussi  
 [Implémentation de la compression de ligne](../../relational-databases/data-compression/row-compression-implementation.md)   
 [Implémentation de la compression de page](../../relational-databases/data-compression/page-compression-implementation.md)   
 [Implémentation de la compression Unicode](../../relational-databases/data-compression/unicode-compression-implementation.md)   
 [CREATE PARTITION SCHEME &#40;Transact-SQL&#41;](../../t-sql/statements/create-partition-scheme-transact-sql.md)   
 [CREATE PARTITION FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/create-partition-function-transact-sql.md)   
 [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)   
 [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)   
 [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md)   
 [ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md)  
  
  

