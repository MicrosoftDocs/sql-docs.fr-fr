---
description: Optimiser les filtres de lignes paramétrables
title: Optimiser les filtres de lignes paramétrables | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- precomputed partitions [SQL Server replication]
- filters [SQL Server replication], parameterized
- merge replication precomputed partitions [SQL Server replication], SQL Server Management Studio
- parameterized filters [SQL Server replication], optimizing
ms.assetid: 49349605-ebd0-4757-95be-c0447f30ba13
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: c8b58c7cf7ffe2b56feb8f59df9b0adef857ab02
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076657"
---
# <a name="optimize-parameterized-row-filters"></a>Optimiser les filtres de lignes paramétrables
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Cette rubrique explique comment optimiser les filtres de lignes paramétrables dans [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../../includes/tsql-md.md)].  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Recommandations](#Recommendations)  
  
-   **Pour optimiser les filtres de lignes paramétrables à l'aide de :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> Recommandations  
  
-   Lorsque vous utilisez des filtres paramétrables, vous pouvez contrôler le traitement de ces filtres par la réplication de fusion en spécifiant l'option **use partition groups** ou l'option **keep partition changes** au moment de la création d'une publication. Ces options améliorent les performances de la synchronisation des publications avec articles filtrés en stockant des métadonnées supplémentaires dans la base de données de publication. Vous pouvez contrôler le partage des données entre les Abonnés en définissant **partition options** au moment de la création d'un article. Pour plus d'informations sur ces conditions requises, consultez [Parameterized Row Filters](../../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md).  
  
     Avec les abonnés SQL Server Compact de [!INCLUDE[ssEW](../../../includes/ssew-md.md)], keep_partition_changes doit avoir la valeur true afin que les suppressions soient correctement propagées. Lorsque la valeur est false, l'abonné peut avoir plus de lignes que prévu.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
 Les paramètres suivants permettent d'optimiser les filtres de lignes paramétrés :  
  
 **Partition Options**  
 Définissez cette option dans la page **Propriétés** de la boîte de dialogue **Propriétés de l’article - \<Article>** , ou dans la boîte de dialogue **Ajouter un filtre**. Ces deux boîtes de dialogue sont disponibles dans l’Assistant Nouvelle publication et dans la boîte de dialogue **Propriétés de la publication - \<Publication>** . La boîte de dialogue **Propriétés de l’article - \<Article>** vous permet de spécifier pour cette option d’autres valeurs qui ne sont pas disponibles dans la boîte de dialogue **Ajouter un filtre**.  
  
 **Précalculer les partitions**  
 Par défaut, cette option est définie à **True** si les articles de votre publication satisfont à un ensemble de conditions. Pour plus d’informations sur ces exigences, consultez [Optimiser les performances des filtres paramétrés avec des partitions précalculées](../../../relational-databases/replication/merge/parameterized-filters-optimize-for-precomputed-partitions.md). Modifiez cette option dans la page **Options d’abonnement** de la boîte de dialogue **Propriétés de la publication - \<Publication>** .  
  
 **Optimiser la synchronisation**  
 Cette option ne doit être définie à **True** que si **Précalculer les partitions** est défini à **False**. Définissez cette option dans la page **Options d’abonnement** de la boîte de dialogue **Propriétés de la publication - \<Publication>** .  
  
 Pour plus d’informations sur l’utilisation de l’Assistant Nouvelle publication et sur l’accès à la boîte de dialogue **Propriétés de la publication- \<Publication>** , consultez [Créer une publication](../../../relational-databases/replication/publish/create-a-publication.md) et [Afficher et modifier les propriétés d’une publication](../../../relational-databases/replication/publish/view-and-modify-publication-properties.md).  
  
#### <a name="to-set-partition-options-in-the-add-filter-or-edit-filter-dialog-box"></a>Pour définir les options de la partition dans la boîte de dialogue Ajouter un filtre ou Modifier le filtre  
  
1.  Dans la page **Filtrer les lignes de la table** de l’Assistant Nouvelle publication, ou la page **Filtrer les lignes** de la boîte de dialogue **Propriétés de la Publication - \<Publication>** , cliquez sur **Ajouter**, puis sur **Ajouter un filtre**.  
  
2.  Créer un filtre paramétré. Pour plus d'informations, voir [Définir et modifier un filtre de lignes paramétrable pour un article de fusion](../../../relational-databases/replication/publish/define-and-modify-a-parameterized-row-filter-for-a-merge-article.md).  
  
3.  Sélectionnez l'option qui correspond aux données qui seront partagées entre des Abonnés :  
  
    -   **Une ligne de cette table ira à plusieurs abonnements**  
  
    -   **Filtre paramétré créant des partitions qui ne se chevauchent pas, avec un seul abonnement par partition**  
  
     Si vous sélectionnez **Filtre paramétré créant des partitions qui ne se chevauchent pas, avec un seul abonnement par partition**, la réplication de fusion peut optimiser les performances en stockant et en traitant moins de métadonnées. Cependant, vous devez vérifier que les données sont partitionnées de telle façon qu'une ligne ne peut pas être répliquée sur plus d'un Abonné. Pour plus d'informations, consultez la section « Définition de « partition options » » dans la rubrique [Parameterized Row Filters](../../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md).  
  
4.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
5.  Si vous êtes dans la boîte de dialogue **Propriétés de la publication - \<Publication>** , cliquez sur **OK** pour enregistrer et fermer la boîte de dialogue.  
  
#### <a name="to-set-partition-options-in-the-article-properties---article-dialog-box"></a>Pour définir les options de la partition dans la boîte de dialogue Propriétés de l’article - \<Article>  
  
1.  Dans la page **Articles** de l’Assistant Nouvelle publication ou la boîte de dialogue **Propriétés de la publication - \<Publication>** , sélectionnez une table, puis cliquez sur **Propriétés de l’article**.  
  
2.  Cliquez sur **Définir les propriétés de l'article de la table en surbrillance** ou **Définir les propriétés de tous les articles de la table**.  
  
3.  Dans la section **Objet de destination** de l’onglet **Propriétés** de la boîte de dialogue **Propriétés de l’article - \<Article>** , spécifiez l’une des valeurs suivantes pour **Options de la partition** :  
  
    -   **Chevauchement**  
  
    -   **Chevauchement ; refus des modifications de données hors partition**  
  
    -   **Non-chevauchement ; abonnement unique**  
  
    -   **Non-chevauchement ; partage entre les abonnements**  
  
     Pour plus d'informations sur ces options et sur leurs liens avec les options disponibles dans les boîtes de dialogue **Ajouter un filtre** et **Modifier le filtre** , consultez la section relative à la définition de la propriété « partition options » dans la rubrique [Parameterized Row Filters](../../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md).  
  
4.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
5.  Si vous êtes dans la boîte de dialogue **Propriétés de la publication - \<Publication>** , cliquez sur **OK** pour enregistrer et fermer la boîte de dialogue.  
  
#### <a name="to-set-precompute-partitions"></a>Pour définir Précalculer les partitions  
  
1.  Dans la page **Options d’abonnement** de la boîte de dialogue **Propriétés de la publication - \<Publication>** , sélectionnez une valeur pour l’option **Précalculer les partitions**. Cette propriété est en lecture seule si :  
  
    -   La publication ne satisfait pas aux conditions requises pour les partitions précalculées.  
  
    -   Aucun instantané n'a encore été généré pour la publication. Dans ce cas, l'option affiche la valeur **Définir automatiquement lorsqu'un instantané est créé**.  
  
2.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
#### <a name="to-set-optimize-synchronization"></a>Pour définir Optimiser la synchronisation  
  
1.  Dans la page **Options d’abonnement** de la boîte de dialogue **Propriétés de la publication - \<Publication>** , sélectionnez la valeur `True` pour l’option **Optimiser la synchronisation**.  
  
2.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
 Pour connaître la définition des options de filtrage `@keep_partition_changes` et `@use_partition_groups`, consultez [sp_addmergepublication](../../../relational-databases/system-stored-procedures/sp-addmergepublication-transact-sql.md).  
  
#### <a name="to-specify-merge-filter-optimizations-when-creating-a-new-publication"></a>Pour spécifier des optimisations du filtre de fusion au moment de la création d'une publication  
  
1.  Dans la base de données de publication sur le serveur de publication, exécutez [sp_addmergepublication](../../../relational-databases/system-stored-procedures/sp-addmergepublication-transact-sql.md). Spécifiez `@publication` et affectez la valeur `true` à l’un des paramètres suivants :  
  
    -   `@use_partition_groups` : optimisation maximale des performances, à condition que les articles soient conformes aux spécifications pour les partitions précalculées. Pour plus d’informations, consultez [Optimiser les performances des filtres paramétrés avec des partitions précalculées](../../../relational-databases/replication/merge/parameterized-filters-optimize-for-precomputed-partitions.md).  
  
    -   `@keep_partition_changes` : utilisez cette optimisation si les partitions précalculées ne peuvent pas être utilisées.  
  
2.  Ajoutez un travail d'instantané pour la publication. Pour plus d’informations, consultez [Créer une publication](../../../relational-databases/replication/publish/create-a-publication.md).  
  
3.  Dans la base de données de publication sur le serveur de publication, exécutez [sp_addmergearticle](../../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md), en spécifiant les paramètres suivants :  
  
    -   `@publication` : nom de la publication de l’étape 1.  
  
    -   `@article` : nom de l’article.  
  
    -   `@source_object` : objet de base de données qui est publié.  
  
    -   `@subset_filterclause` : clause de filtre paramétrable facultative utilisée pour filtrer horizontalement l’article.  
  
    -   `@partition_options` : options de partition pour l’article filtré.  
  
4.  Répétez l'étape 3 pour chaque article de la publication.  
  
5.  (Facultatif) Dans la base de données de publication sur le serveur de publication, exécutez [sp_addmergefilter](../../../relational-databases/system-stored-procedures/sp-addmergefilter-transact-sql.md) pour définir un filtre de jointure entre deux articles. Pour plus d'informations, voir [Définir et modifier un filtre de jointure entre des articles de fusion](../../../relational-databases/replication/publish/define-and-modify-a-join-filter-between-merge-articles.md).  
  
#### <a name="to-view-and-modify-merge-filter-behaviors-for-an-existing-publication"></a>Pour afficher et modifier les comportements de filtre de fusion d'une publication existante  
  
1.  (Facultatif) Dans la base de données de publication sur le serveur de publication, exécutez [sp_helpmergepublication](../../../relational-databases/system-stored-procedures/sp-helpmergepublication-transact-sql.md), en spécifiant `@publication`. Notez la valeur de `keep_partition_changes` et `use_partition_groups` dans le jeu de résultats.  
  
2.  (Facultatif) Dans la base de données de publication sur le serveur de publication, exécutez [sp_changemergepublication](../../../relational-databases/system-stored-procedures/sp-changemergepublication-transact-sql.md). Spécifiez la valeur `use_partition_groups` pour `@property` et `true` ou `false` pour `@value`.  
  
3.  (Facultatif) Dans la base de données de publication sur le serveur de publication, exécutez [sp_changemergepublication](../../../relational-databases/system-stored-procedures/sp-changemergepublication-transact-sql.md). Spécifiez la valeur `keep_partition_changes` pour `@property` et `true` ou `false` pour `@value`.  
  
    > [!NOTE]  
    >  Quand vous activez `keep_partition_changes`, vous devez d’abord désactiver `use_partition_groups` et spécifier la valeur `1` pour `@force_reinit_subscription`.  
  
4.  (Facultatif) Dans la base de données de publication sur le serveur de publication, exécutez [sp_changemergearticle](../../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md). Spécifiez la valeur `partition_options` pour `@property` et la valeur appropriée pour `@value`. Pour connaître la définition de ces options de filtrage, consultez [sp_addmergearticle](../../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md) .  
  
5.  (Facultatif) Lancez l'Agent d'instantané afin de régénérer l'instantané si nécessaire. Pour plus d’informations sur les modifications qui requièrent un nouvel instantané, consultez [Modifier les propriétés des publications et des articles](../../../relational-databases/replication/publish/change-publication-and-article-properties.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Générer automatiquement un ensemble de filtres de jointure entre des articles de fusion &#40;SQL Server Management Studio&#41;](../../../relational-databases/replication/publish/automatically-generate-join-filters-between-merge-articles.md)   
 [Définir et modifier un filtre de lignes paramétrable pour un article de fusion](../../../relational-databases/replication/publish/define-and-modify-a-parameterized-row-filter-for-a-merge-article.md)   
 [Filtres de lignes paramétrés](../../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md)  
  
  
