---
description: Répliquer des tables et des index partitionnés
title: Répliquer des tables et des index partitionnés | Microsoft Docs
ms.custom: ''
ms.date: 09/10/2015
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- partitioned indexes [SQL Server], replicating
- partitioned tables [SQL Server], replicating
- replication [SQL Server], partitioned tables
- publishing [SQL Server replication], partitioned tables
- transactional replication, partitioned tables
ms.assetid: c9fa81b1-6c81-4c11-927b-fab16301a8f5
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 75c0f38e67fd4d02791c5d2b953f4354a5945dc2
ms.sourcegitcommit: e5664d20ed507a6f1b5e8ae7429a172a427b066c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/19/2020
ms.locfileid: "97697141"
---
# <a name="replicate-partitioned-tables-and-indexes"></a>Répliquer des tables et des index partitionnés
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]
  Le partitionnement facilite la gestion des tables et des index de grande taille, car il permet de gérer et d'accéder rapidement et efficacement à des sous-ensembles de données, tout en conservant l'intégrité d'une collecte de données. Pour plus d’informations, consultez [Tables et index partitionnés](../../../relational-databases/partitions/partitioned-tables-and-indexes.md). La réplication prend en charge le partitionnement en fournissant un ensemble de propriétés qui indiquent comment les tables et les index partitionnés doivent être traités.  
  
## <a name="article-properties-for-transactional-and-merge-replication"></a>Propriétés d'article pour la réplication transactionnelle et de fusion  
 Le tableau suivant répertorie les objets utilisés pour partitionner des données.  
  
|Object|Créé en utilisant|  
|------------|----------------------|  
|Table ou index partitionné|CREATE TABLE ou CREATE INDEX|  
|Fonction de partition|CREATE PARTITION FUNCTION|  
|Schéma de partition|CREATE PARTITION SCHEME|  
  
 Les propriétés de partitionnement sont les options de schéma d’article qui déterminent si le partitionnement d’objets doit être copié vers l’Abonné. Ces options de schéma peuvent être définies de plusieurs façons :  
  
-   Sur la page **Propriétés de l'article** de l'Assistant Nouvelle publication ou la boîte de dialogue Propriétés de la publication. Pour copier les objets répertoriés dans le tableau précédent, spécifiez la valeur **true** pour les propriétés **Copier les schémas de partition de table** et **Copier les schémas de partition d'index**. Pour plus d’informations sur la façon d’accéder à la page **Propriétés de l’article**, consultez [Afficher et modifier des propriétés de publication](../../../relational-databases/replication/publish/view-and-modify-publication-properties.md).  
  
-   En utilisant le paramètre *schema_option* de l'une des procédures stockées suivantes :  
  
    -   [sp_addarticle](../../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md) ou [sp_changearticle](../../../relational-databases/system-stored-procedures/sp-changearticle-transact-sql.md) pour la réplication transactionnelle  
  
    -   [sp_addmergearticle](../../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md) ou [sp_changemergearticle](../../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md) pour la réplication de fusion  
  
     Pour copier les objets répertoriés dans le tableau précédent, spécifiez les valeurs d'options de schéma appropriées. Pour plus d'informations sur la spécification d'options de schéma, consultez [Specify Schema Options](../../../relational-databases/replication/publish/specify-schema-options.md).  
  
 La réplication copie les objets vers l'Abonné pendant la synchronisation initiale. Si le schéma de partition utilise des groupes de fichiers autres que le groupe de fichiers PRIMARY, ces groupes de fichiers doivent exister sur l'Abonné avant la synchronisation initiale.  
  
 Après avoir initialisé l'Abonné, les modifications de données sont propagées à l'Abonné et appliquées aux partitions appropriées. Toutefois, les modifications apportées au schéma de partition ne sont pas prises en charge. La réplication transactionnelle et de fusion ne prend pas en charge la réplication des commandes suivantes : ALTER PARTITION FUNCTION, ALTER PARTITION SCHEME ou l’instruction REBUILD WITH PARTITION d’ALTER INDEX. Les modifications associées ne sont pas automatiquement répliquées sur l’Abonné. Il incombe à l'utilisateur d'apporter des modifications similaires manuellement sur l'Abonné.  
  
## <a name="replication-support-for-partition-switching"></a>Prise en charge de la réplication pour le basculement de partition  
 La possibilité de déplacer rapidement et efficacement des sous-ensembles de données entre des partitions constitue l'un des principaux avantages du partitionnement de table. Les données sont déplacées à l'aide de la commande SWITCH PARTITION. Par défaut, lorsqu'une table est activée pour la réplication, les opérations SWITCH PARTITION sont bloquées pour les raisons suivantes :  
  
-   Si les données sont déplacées dans ou hors d'une table qui existe sur le serveur de publication mais pas sur l'Abonné, le serveur de publication et l'Abonné risquent de devenir incohérents entre eux. Ce problème se produit généralement lorsque les données sont déplacées dans ou hors d'une table intermédiaire.  
  
-   Si l'Abonné et le serveur de publication ont des définitions différentes pour la table partitionnée, l'Agent de distribution échouera lorsqu'il tentera d'appliquer des modifications à l'Abonné.  
  
Malgré ces problèmes potentiels, le basculement de partition peut être activé pour la réplication transactionnelle. Avant d'activer le basculement de partition, assurez-vous que toutes les tables impliquées dans cette opération existent sur le serveur de publication et sur l'Abonné, et vérifiez que la définition des tables et de la partition sont identiques.  
  
Lorsque des partitions ont exactement le même schéma de partition sur les serveurs de publication et les abonnés, vous pouvez activer *allow_partition_switch* avec *replication_partition_switch* pour répliquer uniquement l’instruction de basculement de partition vers l’abonné. Vous pouvez également activer *allow_partition_switch* sans répliquer le DDL. Cela s'avère utile lorsque vous souhaitez supprimer des mois antérieurs de la partition tout en conservant la réplication de la partition pendant une autre année à des fins de sauvegarde sur l'abonné.  
  
Si vous activez le basculement de partition (sur SQL Server 2008 R2 jusqu’à la version actuelle), vous devrez peut-être également fractionner et fusionner des opérations dans un futur proche. Avant d’exécuter une opération de fractionnement ou de fusion sur une table répliquée ou compatible CDC, vérifiez que la partition en question n’a pas de commandes répliquées en attente. Vous devez également vous assurer qu’aucune opération DML n’est exécutée sur la partition pendant les opérations de fractionnement et de fusion. Si des transactions n’ont pas été traitées par le lecteur de journal ou travail de capture CDC, ou que des opérations DML sont exécutées sur la partition d’une table répliquée ou compatible CDC en même temps qu’une opération de fractionnement ou de fusion (sur la même partition), il est possible que l’Agent de lecture du journal ou le travail de capture CDC fasse l’objet d’une erreur de traitement (erreur 608 : Aucune entrée de catalogue trouvée pour l’ID de partition). Pour corriger l’erreur, une réinitialisation de l’abonnement ou la désactivation de CDC sur cette table ou cette base de données peut s’avérer nécessaire. 

### <a name="unsupported-scenarios"></a>Scénarios non pris en charge

Les scénarios suivants ne sont pas pris en charge lors de l’utilisation de la réplication avec basculement de partition : 

**Réplication d’égal à égal**   
La réplication d’égal à égal n’est pas prise en charge avec le basculement de partition. 

**Utilisation de variables avec le basculement de partition**   

L’utilisation de variables avec le basculement de partition sur les tables publiées avec la réplication transactionnelle ou la capture des changements de données (CDC) n’est pas prise en charge pour l’instruction `ALTER TABLE ... SWITCH TO ... PARTITION ...`.

Par exemple, le code de basculement de partition suivant ne fonctionne pas avec la capture des changements de données activée sur la base de données, ou avec la TableA qui participe à une publication transactionnelle : 

```sql
DECLARE @SomeVariable INT = $PARTITION.pf_test(10);
ALTER TABLE dbo.TableA
SWITCH TO dbo.TableB 
PARTITION @SomeVariable;
```

Au lieu de cela, basculez votre partition directement à l’aide de la fonction de partition, comme dans l’exemple suivant : 

```sql
ALTER TABLE NonPartitionedTable 
SWITCH TO PartitionedTable PARTITION $PARTITION.pf_test(10);
```


### <a name="enabling-partition-switching"></a>Activation du basculement de partition  
 Les propriétés suivantes pour les publications transactionnelles permettent aux utilisateurs de contrôler le comportement de l'insertion de partition dans un environnement répliqué :  
  
-   `@allow_partition_switch`, quand la valeur spécifiée est `true`, SWITCH PARTITION peut être exécuté sur la base de données de publication.  
  
-   `@replicate_partition_switch` détermine si l’instruction SWITCH PARTITION DDL doit être répliquée sur les Abonnés. Cette option est valide uniquement quand `@allow_partition_switch` a la valeur `true`.  
  
 Vous pouvez définir ces propriétés en utilisant [sp_addpublication](../../../relational-databases/system-stored-procedures/sp-addpublication-transact-sql.md) lors de la création de la publication, ou en utilisant [sp_changepublication](../../../relational-databases/system-stored-procedures/sp-changepublication-transact-sql.md) après la création de la publication. Comme indiqué précédemment, la réplication de fusion ne prend pas en charge le basculement de partition. Pour exécuter SWITCH PARTITION sur une table qui est activée pour la réplication de fusion, supprimez la table de la publication.  
  
## <a name="see-also"></a>Voir aussi  
 [Publier des données et des objets de base de données](../../../relational-databases/replication/publish/publish-data-and-database-objects.md)  
  
  
