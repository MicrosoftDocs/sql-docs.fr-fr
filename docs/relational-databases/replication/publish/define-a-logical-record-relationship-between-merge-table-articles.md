---
title: Définir une relation d’enregistrement logique entre des articles de table de fusion
description: Découvrez comment définir la relation d’enregistrement logique entre les tables connexes utilisées pour les articles de réplication de fusion.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- merge replication logical records [SQL Server replication]
- articles [SQL Server replication], logical records
- logical records [SQL Server replication]
ms.assetid: ff847b3a-c6b0-4eaf-b225-2ffc899c5558
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 397ecaba4cf6b8e56f3dbcb6366dca8121ae345d
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076787"
---
# <a name="define-a-logical-record-relationship-between-merge-table-articles"></a>Définir une relation d'enregistrement logique entre des articles de table de fusion
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Cette rubrique explique comment définir une relation d'enregistrement logique entre des articles de table de fusion dans [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../../includes/tsql-md.md)]ou d'objets RMO (Replication Management Objects).  
  
 La réplication de fusion vous permet de définir une relation entre des lignes connexes dans des tables distinctes. Ces lignes peuvent alors être traitées comme une unité transactionnelle au cours de la synchronisation. Un enregistrement logique peut être défini entre deux articles qu'ils aient ou non une relation de filtre de jointure. Pour plus d’informations, consultez [Regrouper les modifications apportées à des lignes connexes à l’aide d’enregistrements logiques](../../../relational-databases/replication/merge/group-changes-to-related-rows-with-logical-records.md).  
  
> [!NOTE]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../../includes/ssnotedepfutureavoid-md.md)]  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Limitations et restrictions](#Restrictions)  
  
-   **Pour définir une relation d'enregistrement logique entre des articles de table de fusion à l'aide de :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
     [Objets RMO (Replication Management Objects)](#RMOProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitations et restrictions  
  
-   Si vous ajoutez, modifiez ou supprimez un enregistrement logique après que les abonnements à la publication aient été initialisés, vous devez générer un nouvel instantané et réinitialiser tous les abonnements après avoir effectué la modification. Pour plus d’informations sur les exigences relatives aux changements de propriétés, consultez [Changer les propriétés des publications et des articles](../../../relational-databases/replication/publish/change-publication-and-article-properties.md).  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
 Définissez des enregistrements logiques dans la boîte de dialogue **Ajouter une jointure**, disponible dans l’Assistant Nouvelle publication, ainsi que dans la boîte de dialogue **Propriétés de la publication - \<Publication>** . Pour plus d’informations sur l’utilisation de l’Assistant et sur l’accès à la boîte de dialogue, consultez [Créer une publication](../../../relational-databases/replication/publish/create-a-publication.md) et [Afficher et modifier les propriétés d’une publication](../../../relational-databases/replication/publish/view-and-modify-publication-properties.md).  
  
 Les enregistrements logiques peuvent être définis dans la boîte de dialogue **Ajouter une jointure** seulement s'ils sont appliqués à un filtre de jointure dans une publication de fusion, et si la publication satisfait aux conditions requises pour l'utilisation de partitions précalculées. Pour définir des enregistrements logiques qui ne sont pas appliqués à des filtres de jointure et pour définir la détection et la résolution des conflits au niveau des enregistrements logiques, vous devez utiliser des procédures stockées.  
  
#### <a name="to-define-a-logical-record-relationship"></a>Pour définir une relation d'enregistrement logique  
  
1.  Dans la page **Filtrer les lignes de la table** de l’Assistant Nouvelle publication ou la page **Filtrer les lignes** de la boîte de dialogue **Propriétés de la publication - \<Publication>** , sélectionnez un filtre de lignes dans le volet **Tables filtrées**.  
  
     Une relation d'enregistrement logique est associée à un filtre de jointure, qui étend un filtre de lignes. Vous devez donc définir un filtre de lignes avant de pouvoir étendre le filtre avec une jointure et appliquer une relation d'enregistrement logique. Après avoir défini un filtre de jointure, vous pouvez l'étendre avec un autre filtre de jointure. Pour plus d'informations sur la définition de filtres de jointure, consultez [Définir et modifier un filtre de jointure entre des articles de fusion](../../../relational-databases/replication/publish/define-and-modify-a-join-filter-between-merge-articles.md).  
  
2.  Cliquez sur **Ajouter**, puis sur **Ajouter une jointure pour étendre le filtre sélectionné**.  
  
3.  Définissez un filtre de jointure dans la boîte de dialogue **Ajouter une jointure** , puis activez la case à cocher **Enregistrement logique**.  
  
4.  Si vous êtes dans la boîte de dialogue **Propriétés de la publication - \<Publication>** , cliquez sur **OK** pour enregistrer et fermer la boîte de dialogue.  
  
#### <a name="to-delete-a-logical-record-relationship"></a>Pour supprimer une relation d'enregistrement logique  
  
-   Supprimer seulement la relation d'enregistrement logique ou supprimer la relation d'enregistrement logique et le filtre de jointure qui y est associé.  
  
     Pour supprimer seulement la relation d'enregistrement logique :  
  
    1.  Dans la page **Filtrer les lignes** de l’Assistant Nouvelle publication ou dans la page **Filtrer les lignes** de la boîte de dialogue **Propriétés de la publication - \<Publication>** , sélectionnez le filtre de jointure associé à la relation d’enregistrements logiques dans le volet **Tables filtrées**, puis cliquez sur **Modifier**.  
  
    2.  Dans la boîte de dialogue **Modifier une jointure** , désactivez la case à cocher **Enregistrement logique**.  
  
    3.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
     Pour supprimer la relation d'enregistrement logique et le filtre de jointure qui y est associé :  
  
    -   Dans la page **Filtrer les lignes** de l’Assistant Nouvelle publication ou dans la boîte de dialogue **Propriétés de la publication - \<Publication>** , sélectionnez un filtre dans le volet **Tables filtrées**, puis cliquez sur **Supprimer**. Si le filtre de jointure que vous supprimez est lui-même étendu par d'autres jointures, ces jointures seront aussi supprimées.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
 Vous pouvez spécifier par programme des relations d'enregistrements logiques entre des articles en utilisant des procédures stockées de réplication.  
  
#### <a name="to-define-a-logical-record-relationship-without-an-associated-join-filter"></a>Pour définir une relation d'enregistrement logique sans filtre de jointure associé  
  
1.  Si la publication contient des articles filtrés, exécutez [sp_helpmergepublication](../../../relational-databases/system-stored-procedures/sp-helpmergepublication-transact-sql.md)et notez la valeur de **use_partition_groups** dans le jeu de résultats.  
  
    -   Si la valeur est **1**, les partitions précalculées sont déjà utilisées.  
  
    -   Si la valeur est **0**, exécutez [sp_changemergepublication](../../../relational-databases/system-stored-procedures/sp-changemergepublication-transact-sql.md) au niveau du serveur de publication dans la base de données de publication. Affectez la valeur **use_partition_groups** à **\@property** et la valeur **true** à **\@value**.  
  
        > [!NOTE]  
        >  Si la publication ne prend pas en charge les partitions précalculées, les enregistrements logiques ne peuvent pas être utilisés. Pour plus d’informations, consultez « Conditions requises à l’utilisation des partitions précalculées » dans la rubrique [Optimiser les performances des filtres paramétrés avec des partitions précalculées](../../../relational-databases/replication/merge/parameterized-filters-optimize-for-precomputed-partitions.md).  
  
    -   Si la valeur est NULL, l'Agent d'instantané doit être exécuté pour générer l'instantané initial de la publication.  
  
2.  Si les articles qui constitueront l'enregistrement logique n'existent pas, exécutez [sp_addmergearticle](../../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md) au niveau du serveur de publication dans la base de données de publication. Spécifiez l'une des options de détection et de résolution des conflits suivantes pour l'enregistrement logique :  
  
    -   Pour détecter et résoudre les conflits qui se produisent dans des lignes connexes de l’enregistrement logique, affectez la valeur **true** à **\@logical_record_level_conflict_detection** et **\@logical_record_level_conflict_resolution**.  
  
    -   Pour utiliser la détection et la résolution standard des conflits au niveau des lignes ou des colonnes, affectez la valeur **false** à **\@logical_record_level_conflict_detection** et **\@logical_record_level_conflict_resolution**, qui est la valeur par défaut.  
  
3.  Répétez l'étape 2 pour chaque article qui constituera l'enregistrement logique. Vous devez utiliser la même option de détection et de résolution des conflits pour chaque article de l'enregistrement logique. Pour plus d'informations, voir [Détection et résolution des conflits dans les enregistrements logiques](../../../relational-databases/replication/merge/advanced-merge-replication-conflict-resolving-in-logical-record.md).  
  
4.  Dans la base de données de publication sur le serveur de publication, exécutez [sp_addmergefilter](../../../relational-databases/system-stored-procedures/sp-addmergefilter-transact-sql.md). Spécifiez **\@publication**, le nom d’un article de la relation pour **\@article**, le nom du deuxième article pour **\@join_articlename**, le nom de la relation pour **\@filtername**, une clause qui définit la relation entre les deux articles pour **\@join_filterclause**, le type de jointure pour **\@join_unique_key** et affectez l’une des valeurs suivantes à **\@filter_type** :  
  
    -   **2** - définit une relation logique.  
  
    -   **3** - définit une relation logique avec un filtre de jointure.  
  
    > [!NOTE]  
    >  Si aucun filtre de jointure n'est pas utilisé, la direction de la relation entre les deux articles n'est pas importante.  
  
5.  Répétez l'étape 2 pour chaque relation d'enregistrement logique restante dans la publication.  
  
#### <a name="to-change-conflict-detection-and-resolution-for-logical-records"></a>Pour modifier la détection et la résolution des conflits pour les enregistrements logiques  
  
1.  Pour détecter et résoudre les conflits qui se produisent dans des lignes connexes de l'enregistrement logique :  
  
    -   Dans la base de données de publication sur le serveur de publication, exécutez [sp_changemergearticle](../../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md). Affectez la valeur **logical_record_level_conflict_detection** à **\@property** et la valeur **true** à **\@value**. Affectez la valeur **1** à **\@force_invalidate_snapshot** et **\@force_reinit_subscription**.  
  
    -   Dans la base de données de publication sur le serveur de publication, exécutez [sp_changemergearticle](../../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md). Affectez la valeur **logical_record_level_conflict_resolution** à **\@property** et la valeur **true** à **\@value**. Affectez la valeur **1** à **\@force_invalidate_snapshot** et **\@force_reinit_subscription**.  
  
2.  Pour utiliser la détection et la résolution standard des conflits au niveau des lignes ou des colonnes :  
  
    -   Dans la base de données de publication sur le serveur de publication, exécutez [sp_changemergearticle](../../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md). Affectez la valeur **logical_record_level_conflict_detection** à **\@property** et la valeur **false** à **\@value**. Affectez la valeur **1** à **\@force_invalidate_snapshot** et **\@force_reinit_subscription**.  
  
    -   Dans la base de données de publication sur le serveur de publication, exécutez [sp_changemergearticle](../../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md). Affectez la valeur **logical_record_level_conflict_resolution** à **\@property** et la valeur **false** à **\@value**. Affectez la valeur **1** à **\@force_invalidate_snapshot** et **\@force_reinit_subscription**.  
  
#### <a name="to-remove-a-logical-record-relationship"></a>Pour supprimer une relation d'enregistrements logiques  
  
1.  Dans la base de données de publication sur le serveur de publication, exécutez la requête suivante afin que des informations sur toutes les relations d'enregistrements logiques définies pour la publication spécifiée soient retournées :  
  
     [!code-sql[HowTo#sp_ReturnMergeLogicalRecords](../../../relational-databases/replication/codesnippet/tsql/define-a-logical-record-_1.sql)]  
  
     Notez le nom de la relation d'enregistrements logiques en cours de suppression dans la colonne **filtername** du jeu de résultats.  
  
    > [!NOTE]  
    >  Cette requête retourne les mêmes informations que [sp_helpmergefilter](../../../relational-databases/system-stored-procedures/sp-helpmergefilter-transact-sql.md)Toutefois, cette procédure stockée système retourne seulement des informations sur les relations d'enregistrements logiques qui sont également des filtres de jointure.  
  
2.  Dans la base de données de publication sur le serveur de publication, exécutez [sp_dropmergefilter](../../../relational-databases/system-stored-procedures/sp-dropmergefilter-transact-sql.md). Spécifiez **\@publication**, le nom de l’un des articles de la relation pour **\@article** et le nom de la relation de l’étape 1 pour **\@filtername**.  
  
###  <a name="example-transact-sql"></a><a name="TsqlExample"></a> Exemple (Transact-SQL)  
 Cet exemple active les partitions précalculées sur une publication existante et crée un enregistrement logique qui comprend les deux nouveaux articles des tables `SalesOrderHeader` et `SalesOrderDetail` .  
  
 [!code-sql[HowTo#sp_AddMergeLogicalRecord](../../../relational-databases/replication/codesnippet/tsql/define-a-logical-record-_2.sql)]  
  
##  <a name="using-replication-management-objects-rmo"></a><a name="RMOProcedure"></a> Utilisation d'objets RMO (Replication Management Objects)  
  
> [!NOTE]  
>  La réplication de fusion vous permet de spécifier que les conflits soient suivis et résolus au niveau des enregistrements logiques, mais ces options ne peuvent pas être définies à l'aide des objets RMO.  
  
#### <a name="to-define-a-logical-record-relationship-without-an-associated-join-filter"></a>Pour définir une relation d'enregistrement logique sans filtre de jointure associé  
  
1.  Créez une connexion au serveur de publication en utilisant la classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Créez une instance de la classe <xref:Microsoft.SqlServer.Replication.MergePublication> , définissez les propriétés <xref:Microsoft.SqlServer.Replication.Publication.Name%2A> et <xref:Microsoft.SqlServer.Replication.Publication.DatabaseName%2A> pour la publication et définissez la propriété <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> sur la connexion créée à l'étape 1.  
  
3.  Appelez la méthode <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> pour obtenir les propriétés de l'objet. Si cette méthode retourne **false**, soit les propriétés de la publication ont été définies de manière incorrecte à l'étape 2, soit la publication n'existe pas.  
  
4.  Si la propriété <xref:Microsoft.SqlServer.Replication.MergePublication.PartitionGroupsOption%2A> a la valeur <xref:Microsoft.SqlServer.Replication.PartitionGroupsOption.False>, affectez-lui la valeur <xref:Microsoft.SqlServer.Replication.PartitionGroupsOption.True>.  
  
5.  Si les articles devant inclure l'enregistrement logique n'existent pas, créez une instance de la classe <xref:Microsoft.SqlServer.Replication.MergeArticle> et définissez les propriétés suivantes :  
  
    -   Le nom de l'article pour <xref:Microsoft.SqlServer.Replication.Article.Name%2A>.  
  
    -   Le nom de la publication pour <xref:Microsoft.SqlServer.Replication.Article.PublicationName%2A>.  
  
    -   (Facultatif) Si l'article est filtré horizontalement, spécifiez la clause de filtre de lignes pour la propriété <xref:Microsoft.SqlServer.Replication.MergeArticle.FilterClause%2A> . Utilisez cette propriété pour spécifier un filtre de lignes statique ou paramétrable. Pour plus d'informations, voir [Parameterized Row Filters](../../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md).  
  
     Pour plus d’informations, consultez [définir un Article](../../../relational-databases/replication/publish/define-an-article.md).  
  
6.  Appelez la méthode <xref:Microsoft.SqlServer.Replication.Article.Create%2A> .  
  
7.  Répétez les étapes 5 et 6 pour chaque article qui comprend l'enregistrement logique.  
  
8.  Créez une instance de la classe <xref:Microsoft.SqlServer.Replication.MergeJoinFilter> pour définir la relation d'enregistrement logique entre les articles. Définissez ensuite les propriétés suivantes :  
  
    -   Le nom de l'article enfant dans la relation d'enregistrement logique pour la propriété <xref:Microsoft.SqlServer.Replication.MergeJoinFilter.ArticleName%2A> .  
  
    -   Le nom de l'article parent existant dans la relation d'enregistrement logique pour la propriété <xref:Microsoft.SqlServer.Replication.MergeJoinFilter.JoinArticleName%2A> .  
  
    -   Un nom pour la relation d'enregistrement logique pour la propriété <xref:Microsoft.SqlServer.Replication.MergeJoinFilter.FilterName%2A> .  
  
    -   L'expression qui définit la relation pour la propriété <xref:Microsoft.SqlServer.Replication.MergeJoinFilter.JoinFilterClause%2A> .  
  
    -   Une valeur de <xref:Microsoft.SqlServer.Replication.FilterTypes.LogicalRecordLink> pour la propriété <xref:Microsoft.SqlServer.Replication.MergeJoinFilter.FilterTypes%2A> . Si la relation d'enregistrement logique correspond également à un filtre de jointure, spécifiez une valeur de <xref:Microsoft.SqlServer.Replication.FilterTypes.JoinFilterAndLogicalRecordLink> pour cette propriété. Pour plus d’informations, consultez [Regrouper les modifications apportées à des lignes connexes à l’aide d’enregistrements logiques](../../../relational-databases/replication/merge/group-changes-to-related-rows-with-logical-records.md).  
  
9. Appelez la méthode <xref:Microsoft.SqlServer.Replication.MergeArticle.AddMergeJoinFilter%2A> sur l'objet qui représente l'article enfant dans la relation. Passez l'objet <xref:Microsoft.SqlServer.Replication.MergeJoinFilter> créé à l'étape 8 pour définir la relation.  
  
10. Répétez les étapes 8 et 9 pour chaque relation d'enregistrement logique restante dans la publication.  
  
###  <a name="example-rmo"></a><a name="PShellExample"></a> Exemple (RMO)  
 Cet exemple crée un enregistrement logique qui comprend les deux nouveaux articles pour les tables `SalesOrderHeader` et `SalesOrderDetail` .  
  
 [!code-cs[HowTo#rmo_CreateLogicalRecord](../../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_createlogicalrecord)]  
  
 [!code-vb[HowTo#rmo_vb_CreateLogicalRecord](../../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_createlogicalrecord)]  
  
## <a name="see-also"></a>Voir aussi  
 [Définir et modifier un filtre de jointure entre des articles de fusion](../../../relational-databases/replication/publish/define-and-modify-a-join-filter-between-merge-articles.md)   
 [Définir et modifier un filtre de lignes paramétrable pour un article de fusion](../../../relational-databases/replication/publish/define-and-modify-a-parameterized-row-filter-for-a-merge-article.md)   
 [Définir et modifier un filtre de lignes statique](../../../relational-databases/replication/publish/define-and-modify-a-static-row-filter.md)   
 [Regrouper les modifications apportées à des lignes connexes à l’aide d’enregistrements logiques](../../../relational-databases/replication/merge/group-changes-to-related-rows-with-logical-records.md)   
 [Optimiser les performances des filtres paramétrés avec des partitions précalculées](../../../relational-databases/replication/merge/parameterized-filters-optimize-for-precomputed-partitions.md)   
 [Regrouper les modifications apportées à des lignes connexes à l’aide d’enregistrements logiques](../../../relational-databases/replication/merge/group-changes-to-related-rows-with-logical-records.md)  
  
  
