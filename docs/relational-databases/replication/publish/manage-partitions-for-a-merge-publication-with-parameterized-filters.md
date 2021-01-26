---
title: Gérer les partitions avec des filtres paramétrables (Fusion)
description: Gérez les partitions avec des filtres paramétrables utilisés pour la réplication de fusion SQL Server.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- partitions [SQL Server replication]
- merge replication partitions [SQL Server replication], SQL Server Management Studio
- parameterized filters [SQL Server replication], partition management
ms.assetid: fb5566fe-58c5-48f7-8464-814ea78e6221
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 0b24e2e404dbea75d77cdfc8acdfa50189db2286
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/25/2021
ms.locfileid: "98765728"
---
# <a name="manage-partitions-for-a-merge-publication-with-parameterized-filters"></a>Gérer les partitions d'une publication de fusion avec des filtres paramétrables
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Cette rubrique explique comment gérer des partitions pour une publication de fusion avec des filtres paramétrables dans [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)], de [!INCLUDE[tsql](../../../includes/tsql-md.md)]ou d'objets RMO (Replication Management Objects). Les filtres de lignes paramétrables peuvent être utilisés pour générer des partitions qui ne se chevauchent pas. Ces partitions peuvent être restreintes afin qu'un seul abonnement puisse recevoir une partition donnée. Dans ces cas, un grand nombre d'abonnés se traduit par un nombre élevé de partitions, ce qui requiert ensuite un nombre égal d'instantanés partitionnés. Pour plus d'informations, voir [Parameterized Row Filters](../../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md).  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Recommandations](#Recommendations)  
  
-   **Pour gérer les partitions d'une publication de fusion avec les filtres paramétrables à l'aide de :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
     [Objets RMO (Replication Management Objects)](#RMOProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> Recommandations  
  
-   Si vous créez un script de la topologie de réplication (ce qui est recommandé), les scripts de publication contiennent les appels de procédures stockées pour créer des partitions de données. Le script fournit une référence pour les partitions créées et un moyen permettant de recréer si nécessaire une ou plusieurs partitions. Pour plus d'informations, voir [Scripting Replication](../../../relational-databases/replication/scripting-replication.md).  
  
-   Lorsqu'une publication a paramétré des filtres qui génèrent des abonnements avec des partitions ne se chevauchant pas, et si un abonnement particulier est perdu et doit être recréé, vous devez effectuer les opérations suivantes : supprimez la partition à laquelle s'abonner, recréez l'abonnement, puis recréez la partition. Pour plus d'informations, voir [Parameterized Row Filters](../../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md). La réplication génère des scripts de création pour les partitions d'Abonné existantes lors de la génération d'un script de création de publication. Pour plus d'informations, voir [Scripting Replication](../../../relational-databases/replication/scripting-replication.md).  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
 Gérez des partitions dans la page **Partitions de données** de la boîte de dialogue **Propriétés de la publication - \<Publication>** . Pour plus d'informations sur l'accès à cette boîte de dialogue, consultez [Afficher et modifier les propriétés d’un serveur de publication](../../../relational-databases/replication/publish/view-and-modify-publication-properties.md). Sur cette page, vous pouvez : créer et supprimer des partitions, autoriser des Abonnés à initier la génération et la remise d'instantanés, générer des instantanés pour une ou plusieurs partitions et nettoyer des instantanés.  
  
#### <a name="to-create-a-partition"></a>Pour créer une partition  
  
1.  Dans la page **Partitions de données** de la boîte de dialogue **Propriétés de la publication - \<Publication>** , cliquez sur **Ajouter**.  
  
2.  Dans la boîte de dialogue **Ajouter une partition de données** , entrez une valeur pour la valeur **HOST_NAME()** et/ou **SUSER_SNAME()** associée à la valeur que vous voulez créer.  
  
3.  En option, spécifiez une planification pour l'actualisation des instantanés :  
  
    1.  Sélectionnez **Planifier l'Agent d'instantané pour l'exécution de cette partition à l'heure ou aux heures suivantes**.  
  
    2.  Acceptez la planification par défaut pour l'actualisation des instantanés, ou cliquez sur **Modifier** pour spécifier une autre planification.  
  
4.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  

#### <a name="to-delete-a-partition"></a>Pour supprimer une partition  
  
1.  Sur la page **Partitions de données** , sélectionnez une partition dans la grille.  
  
2.  Cliquez sur **Supprimer**.  
  
#### <a name="to-allow-subscribers-to-initiate-snapshot-generation-and-delivery"></a>Pour permettre aux Abonnés d'initier la génération et la remise d'instantanés  
  
1.  Sur la page **Partitions de données** , sélectionnez **Définir automatiquement une partition et générer un instantané si nécessaire lorsqu'un nouvel abonné essaie de se synchroniser**.  
  
2.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
#### <a name="to-generate-a-snapshot-for-a-partition"></a>Pour générer un instantané pour une partition  
  
1.  Sur la page **Partitions de données** , sélectionnez une partition dans la grille.  
  
2.  Cliquez sur **Générer les instantanés sélectionnés maintenant**.  
  
#### <a name="to-clean-up-a-snapshot-for-a-partition"></a>Pour nettoyer un instantané pour une partition  
  
1.  Sur la page **Partitions de données** , sélectionnez une partition dans la grille.  
  
2.  Cliquez sur **Nettoyer les instantanés existants**.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
 Pour mieux gérer une publication avec des filtres paramétrables, vous pouvez énumérer par programmation les partitions existantes à l'aide de procédures stockées de réplication. Vous pouvez également créer et supprimer des partitions existantes. Vous pouvez obtenir les informations suivantes sur les partitions existantes :  
  
-   Comment une partition est filtrée (à l’aide de [SUSER_SNAME &#40;Transact-SQL&#41;](../../../t-sql/functions/suser-sname-transact-sql.md) ou de [HOST_NAME &#40;Transact-SQL&#41;](../../../t-sql/functions/host-name-transact-sql.md)).  
  
-   Le nom du travail qui génère un instantané partitionné.  
  
-   La dernière fois qu'un travail d'instantané partitionné a été exécuté.  
  
 Tandis que la deuxième partie de l'instantané bipartite peut être généré à la demande lors de l'initialisation d'un nouvel abonnement, les procédures suivantes permettent de contrôler comment cet instantané est généré et de le prégénérer dans les cas les plus pratiques. Pour plus d'informations, voir [Snapshots for Merge Publications with Parameterized Filters](../../../relational-databases/replication/create-a-snapshot-for-a-merge-publication-with-parameterized-filters.md).  
  
#### <a name="to-view-information-on-existing-partitions"></a>Pour consulter les informations sur les partitions existantes  
  
1.  Dans la base de données de publication sur le serveur de publication, exécutez [sp_helpmergepartition &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-helpmergepartition-transact-sql.md). Spécifiez le nom de la publication pour `@publication`. (Facultatif) Spécifiez `@suser_sname` ou `@host_name` pour retourner uniquement les informations basées sur un critère de filtre unique.  
  
#### <a name="to-define-a-new-partition-and-generate-a-new-partitioned-snapshot"></a>Pour définir une nouvelle partition et générer un nouvel instantané partitionné  
  
1.  Dans la base de données de publication sur le serveur de publication, exécutez [sp_addmergepartition &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addmergepartition-transact-sql.md). Spécifiez le nom de la publication pour `@publication` et la valeur paramétrable qui définit la partition pour l’un des éléments suivants :  
  
    -   `@suser_sname` - quand le filtre paramétrable est défini par la valeur retournée par [SUSER_SNAME &#40;Transact-SQL&#41;](../../../t-sql/functions/suser-sname-transact-sql.md).  
  
    -   `@host_name` - quand le filtre paramétrable est défini par la valeur retournée par [HOST_NAME &#40;Transact-SQL&#41;](../../../t-sql/functions/host-name-transact-sql.md).  
  
2.  Créez et initialisez l'instantané paramétrable de cette nouvelle partition. Pour plus d'informations, voir [Créer un instantané d’une publication de fusion avec des filtres paramétrés](../../../relational-databases/replication/create-a-snapshot-for-a-merge-publication-with-parameterized-filters.md).  
  
#### <a name="to-delete-a-partition"></a>Pour supprimer une partition  
  
1.  Dans la base de données de publication sur le serveur de publication, exécutez [sp_dropmergepartition &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-dropmergepartition-transact-sql.md). Spécifiez le nom de la publication pour `@publication` et la valeur paramétrable qui définit la partition pour l’un des éléments suivants :  
  
    -   `@suser_sname` - quand le filtre paramétrable est défini par la valeur retournée par [SUSER_SNAME &#40;Transact-SQL&#41;](../../../t-sql/functions/suser-sname-transact-sql.md).  
  
    -   `@host_name` - quand le filtre paramétrable est défini par la valeur retournée par [HOST_NAME &#40;Transact-SQL&#41;](../../../t-sql/functions/host-name-transact-sql.md).  
  
     Le travail d'instantané et les fichiers d'instantané de la partition sont également supprimés.  
  
##  <a name="using-replication-management-objects-rmo"></a><a name="RMOProcedure"></a> Utilisation d'objets RMO (Replication Management Objects)  
 Pour mieux gérer une publication avec les filtres paramétrables, vous pouvez par programmation créer, énumérer ou supprimer les partitions d'Abonné, en utilisant les objets RMO (Replication Management Objects). Pour plus d'informations sur la création de partitions d'Abonné, consultez [Créer un instantané d’une publication de fusion avec des filtres paramétrés](../../../relational-databases/replication/create-a-snapshot-for-a-merge-publication-with-parameterized-filters.md). Vous pouvez obtenir les informations suivantes sur les partitions existantes :  
  
-   la valeur et la fonction de filtrage sur lesquelles la partition est basée ;  
  
-   le nom du travail qui génère un instantané paramétrable pour l'Abonné ;  
  
-   la dernière fois qu'un travail d'instantané paramétrable a été exécuté ;  
  
#### <a name="to-view-information-on-existing-partitions"></a>Pour consulter les informations sur les partitions existantes  
  
1.  Créez une connexion au serveur de publication en utilisant la classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Créez une instance de la classe <xref:Microsoft.SqlServer.Replication.MergePublication>. Définissez les propriétés <xref:Microsoft.SqlServer.Replication.Publication.Name%2A> et <xref:Microsoft.SqlServer.Replication.Publication.DatabaseName%2A> de la publication et définissez la propriété <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> avec la valeur <xref:Microsoft.SqlServer.Management.Common.ServerConnection> créée au cours de l'étape 1.  
  
3.  Appelez la méthode <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> pour obtenir les propriétés de l'objet. Si cette méthode retourne **false**, soit les propriétés de la publication ont été définies de manière incorrecte à l'étape 2, soit la publication n'existe pas.  
  
4.  Appelez la méthode <xref:Microsoft.SqlServer.Replication.MergePublication.EnumMergePartitions%2A> et passez le résultat à un tableau d'objets <xref:Microsoft.SqlServer.Replication.MergePartition> .  
  
5.  Pour chaque objet <xref:Microsoft.SqlServer.Replication.MergePartition> du tableau, récupérez les propriétés dignes d'intérêt.  
  
#### <a name="to-delete-existing-partitions"></a>Pour supprimer les partitions existantes  
  
1.  Créez une connexion au serveur de publication en utilisant la classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Créez une instance de la classe <xref:Microsoft.SqlServer.Replication.MergePublication>. Définissez les propriétés <xref:Microsoft.SqlServer.Replication.Publication.Name%2A> et <xref:Microsoft.SqlServer.Replication.Publication.DatabaseName%2A> de la publication et définissez la propriété <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> avec la valeur <xref:Microsoft.SqlServer.Management.Common.ServerConnection> créée au cours de l'étape 1.  
  
3.  Appelez la méthode <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> pour obtenir les propriétés de l'objet. Si cette méthode retourne **false**, soit les propriétés de la publication ont été définies de manière incorrecte à l'étape 2, soit la publication n'existe pas.  
  
4.  Appelez la méthode <xref:Microsoft.SqlServer.Replication.MergePublication.EnumMergePartitions%2A> et passez le résultat à un tableau d'objets <xref:Microsoft.SqlServer.Replication.MergePartition> .  
  
5.  Pour chaque objet <xref:Microsoft.SqlServer.Replication.MergePartition> du tableau, déterminez si la partition doit être supprimée. Cette décision repose habituellement sur la valeur de la propriété <xref:Microsoft.SqlServer.Replication.MergePartition.DynamicFilterLogin%2A> ou de la propriété <xref:Microsoft.SqlServer.Replication.MergePartition.DynamicFilterHostName%2A> .  
  
6.  Appelez la méthode <xref:Microsoft.SqlServer.Replication.MergePublication.RemoveMergePartition%2A> sur l'objet <xref:Microsoft.SqlServer.Replication.MergePublication> à partir de l'étape 2. Passez l'objet <xref:Microsoft.SqlServer.Replication.MergePartition> créé à l'étape 5.  
  
7.  Répétez l'étape 6 pour chaque partition supprimée.  
  
## <a name="see-also"></a>Voir aussi  
 [Filtres de lignes paramétrés](../../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md)   
 [Instantanés des publications de fusion avec des filtres paramétrés](../../../relational-databases/replication/create-a-snapshot-for-a-merge-publication-with-parameterized-filters.md)  
  
  
  
