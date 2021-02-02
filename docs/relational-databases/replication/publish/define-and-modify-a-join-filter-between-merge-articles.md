---
title: Définir et modifier un filtre de jointure entre des articles de fusion
description: Découvrez comment définir et modifier le filtre de jointure utilisé entre des articles de fusion à l’aide de SQL Server Management Studio (SSMS) ou de Transact-SQL (T-SQL).
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- filters [SQL Server replication], join
- merge replication join filters [SQL Server replication]
- modifying filters, join
- join filters
ms.assetid: f7f23415-43ff-40f5-b3e0-0be1d148ee5b
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 4ef84f22bf255b8dcb21ba371de5c3045da3ca6b
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076747"
---
# <a name="define-and-modify-a-join-filter-between-merge-articles"></a>Définir et modifier un filtre de jointure entre des articles de fusion
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Cette rubrique décrit comment définir et modifier un filtre de jointure entre des articles de fusion dans [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../../includes/tsql-md.md)]. La réplication de fusion prend en charge les filtres de jointure, qui sont en général utilisés conjointement aux filtres paramétrables pour étendre le partitionnement de table à d'autres articles de table connexes.  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Limitations et restrictions](#Restrictions)  
  
     [Recommandations](#Recommendations)  
  
-   **Pour définir et modifier un filtre de jointure entre des articles de fusion à l'aide de :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitations et restrictions  
  
-   Pour qu'il soit possible de créer un filtre de jointure, la publication doit contenir au minimum deux tables associées. Un filtre de jointure permet d'étendre un filtre de lignes : vous devez donc définir un filtre de lignes sur une table pour pouvoir étendre le filtre à une autre table avec une jointure. Après avoir défini un filtre de jointure, vous pouvez l'étendre avec un autre filtre de jointure si la publication contient des tables associées supplémentaires.  
  
-   Si vous ajoutez, modifiez ou supprimez un filtre de jointure après que les abonnements à la publication aient été initialisés, vous devez générer un nouvel instantané et réinitialiser tous les abonnements après avoir effectué la modification. Pour plus d’informations sur les exigences relatives aux changements de propriétés, consultez [Changer les propriétés des publications et des articles](../../../relational-databases/replication/publish/change-publication-and-article-properties.md).  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> Recommandations  
  
-   Les filtres de jointure peuvent être créés manuellement pour un ensemble de tables, ou bien la réplication peut générer les filtres automatiquement sur la base des relations entre les clés étrangères et les clés primaires définies sur les tables. Pour plus d’informations sur la génération automatique d’un ensemble de filtres de jointure, consultez [Générer automatiquement un ensemble de filtres de jointure entre des articles de fusion &#40;SQL Server Management Studio&#41;](../../../relational-databases/replication/publish/automatically-generate-join-filters-between-merge-articles.md).  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
 Définissez, modifiez et supprimez des filtres de jointure dans la page **Filtrer les lignes de la table** de l’Assistant Nouvelle publication ou dans la page **Filtrer les lignes** de la boîte de dialogue **Propriétés de la publication – \<Publication>** . Pour plus d’informations sur l’utilisation de l’Assistant et sur l’accès à la boîte de dialogue, consultez [Créer une publication](../../../relational-databases/replication/publish/create-a-publication.md) et [Afficher et modifier les propriétés d’une publication](../../../relational-databases/replication/publish/view-and-modify-publication-properties.md).  
  
#### <a name="to-define-a-join-filter"></a>Pour définir un filtre de jointure  
  
1.  Dans la page **Filtrer les lignes de la table** de l’Assistant Nouvelle publication ou dans la page **Filtrer les lignes** de la boîte de dialogue **Propriétés de la publication – \<Publication>** , sélectionnez un filtre de lignes existant ou un filtre de jointure dans le volet **Tables filtrées**.  
  
2.  Cliquez sur **Ajouter**, puis sur **Ajouter une jointure pour étendre le filtre sélectionné**.  
  
3.  Créez l'instruction de jointure : sélectionnez **Utiliser le générateur pour créer l'instruction** ou **Créer manuellement l'instruction de jointure**.  
  
    -   Si vous sélectionnez le générateur, utilisez les colonnes de la grille (**Conjonction**, **Colonnes de table filtrée**, **Opérateur** et **Colonnes de table jointe**) pour créer une instruction de jointure.  
  
         Chaque colonne de la grille contient une zone de liste déroulante fixe permettant de sélectionner deux colonnes et un opérateur ( **=** , **<>** , **<=** , **\<**, **>=** , **>** et **like**). Les résultats s'affichent dans la zone de texte **Aperçu** . Si la jointure concerne plus d'une paire de colonnes, sélectionnez une conjonction (AND ou OR) dans la colonne **Conjonction** , et entrez deux autres colonnes et un opérateur.  
  
    -   Si vous créez l'instruction manuellement, écrivez l'instruction de jointure dans la zone de texte **Instruction de jointure** . Utilisez la zone de liste **Colonnes de table filtrée** et la zone de liste **Colonnes de table jointe** pour faire glisser et déposer des colonnes dans la zone de texte **Instruction de jointure** .  
  
    -   L'instruction de jointure complète est par exemple celle-ci :  
  
        ```  
        SELECT <published_columns> FROM [Sales].[SalesOrderHeader] INNER JOIN [Sales].[SalesOrderDetail] ON [SalesOrderHeader].[SalesOrderID] = [SalesOrderDetail].[SalesOrderID]  
        ```  
  
         La clause JOIN doit utiliser un nommage en deux parties ; les nommages en trois et en quatre parties ne sont pas pris en charge.  
  
4.  Spécifiez les options de jointure :  
  
    -   Si la colonne sur laquelle vous effectuez la jointure dans la table filtrée (la table parente) est unique, sélectionnez **Clé unique**.  
  
        > [!CAUTION]  
        >  La sélection de cette option indique que la relation entre les tables enfant et parent dans un filtre de jointure correspond à une relation Un à un ou Un à plusieurs. Sélectionnez cette option seulement si vous avez une contrainte sur la colonne de jointure dans la table enfant qui garantit l'unicité. Si vous ne définissez pas correctement l'option, des erreurs de non-convergence de données peuvent se produire.  
  
    -   Par défaut, la réplication de fusion traite les modifications ligne par ligne lors de la synchronisation. Pour que les modifications dans les lignes de la table filtrée et de la table jointe soient traitées comme une unité, sélectionnez **Enregistrement logique** ([!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] et ses versions ultérieures uniquement). Cette option est disponible uniquement si les conditions d'article et de publication d'utilisation d'enregistrements logiques sont satisfaites. Pour plus d’informations, consultez la section « Considérations relatives à l’utilisation d’enregistrements logiques » dans [Regrouper les modifications apportées à des lignes connexes à l’aide d’enregistrements logiques](../../../relational-databases/replication/merge/group-changes-to-related-rows-with-logical-records.md).  
  
5.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
6.  Si vous êtes dans la boîte de dialogue **Propriétés de la publication – \<Publication>** , cliquez sur **OK** pour enregistrer et fermer la boîte de dialogue.  

#### <a name="to-modify-a-join-filter"></a>Pour modifier un filtre de jointure  
  
1.  Dans la page **Filtrer les lignes de la table** de l’Assistant Nouvelle publication ou dans la page **Filtrer les lignes** de la boîte de dialogue **Propriétés de la publication – \<Publication>** , sélectionnez un filtre dans le volet **Tables filtrées**, puis cliquez sur **Modifier**.  
  
2.  Dans la boîte de dialogue **Modifier une jointure** , modifiez le filtre.  
  
3.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
#### <a name="to-delete-a-join-filter"></a>Pour supprimer un filtre de jointure  
  
1.  Dans la page **Filtrer les lignes de la table** de l’Assistant Nouvelle publication ou dans la page **Filtrer les lignes** de la boîte de dialogue **Propriétés de la publication – \<Publication>** , sélectionnez un filtre dans le volet **Tables filtrées**, puis cliquez sur **Supprimer**. Si le filtre de jointure que vous supprimez est lui-même étendu par d'autres jointures, ces jointures seront aussi supprimées.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
 Ces procédures montrent un filtre paramétrable sur un article parent avec des filtres de jointure entre cet article et des articles enfants connexes. Les filtres de jointure peuvent être définis et modifiés par programme à l'aide des procédures stockées de réplication.  
  
#### <a name="to-define-a-join-filter-to-extend-an-article-filter-to-related-articles-in-a-merge-publication"></a>Pour définir un filtre de jointure pour étendre un filtre d'article aux articles connexes dans une publication de fusion  
  
1.  Définissez le filtrage pour l'article auquel s'effectue la jointure, qui est également connu comme l'article parent.  
  
    -   Pour un article filtré à l'aide d'un filtre de lignes paramétrable, consultez [Définir et modifier un filtre de lignes paramétrable pour un article de fusion](../../../relational-databases/replication/publish/define-and-modify-a-parameterized-row-filter-for-a-merge-article.md).  
  
    -   Pour un article filtré à l'aide d'un filtre de lignes statique, consultez [Définir et modifier un filtre de lignes statiques](../../../relational-databases/replication/publish/define-and-modify-a-static-row-filter.md)  
  
2.  Exécutez [sp_addmergearticle &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md) sur la base de données de publication du serveur de publication pour définir un ou plusieurs articles connexes, également appelés « articles enfants », pour la publication. Pour plus d’informations, consultez [définir un Article](../../../relational-databases/replication/publish/define-an-article.md).  
  
3.  Dans la base de données de publication sur le serveur de publication, exécutez [sp_addmergefilter &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addmergefilter-transact-sql.md). Spécifiez `@publication`, le nom unique de ce filtre pour `@filtername`, le nom de l’article enfant créé à l’étape 2 pour `@article`, le nom de l’article parent auquel s’effectue la jointure pour `@join_articlename`, et l’une des valeurs suivantes pour `@join_unique_key` :  
  
    -   **0** – indique une jointure plusieurs-à-un ou plusieurs-à-plusieurs entre les articles parents et enfants.  
  
    -   **1** – indique une jointure un-à-un ou un-à-plusieurs entre les articles parents et enfants.  
  
     Cela définit un filtre de jointure entre les deux articles.  
  
    > [!CAUTION]  
    >  Affectez uniquement à `@join_unique_key` la valeur **1** s’il existe une contrainte sur la colonne de jointure dans la table sous-jacente pour l’article parent qui garantit l’unicité. Si `@join_unique_key` se voit affecter la valeur **1** de manière inappropriée, une non-convergence des données peut se produire.  
  
###  <a name="examples-transact-sql"></a><a name="TsqlExample"></a> Exemples (Transact-SQL)  
 Cet exemple définit un article pour une publication de fusion, où l'article de la table `SalesOrderDetail` est filtré par rapport à la table `SalesOrderHeader` qui est elle-même filtrée à l'aide d'un filtre de ligne statique. Pour plus d'informations, voir [Définir et modifier un filtre de lignes statiques](../../../relational-databases/replication/publish/define-and-modify-a-static-row-filter.md).  
  
 [!code-sql[HowTo#sp_AddMergeArticle](../../../relational-databases/replication/codesnippet/tsql/define-and-modify-a-join_1.sql)]  
  
 Cet exemple définit un groupe d'articles dans une publication de fusion où les articles sont filtrés à l'aide d'une série de filtres de jointure par rapport à la table `Employee` , qui est elle-même filtrée à l'aide d'un filtre de lignes paramétrable sur la valeur de [HOST_NAME](../../../t-sql/functions/host-name-transact-sql.md) dans la colonne **LoginID** . Pour plus d'informations, voir [Définir et modifier un filtre de lignes paramétrable pour un article de fusion](../../../relational-databases/replication/publish/define-and-modify-a-parameterized-row-filter-for-a-merge-article.md).  
  
 [!code-sql[HowTo#sp_MergeDynamicPub1](../../../relational-databases/replication/codesnippet/tsql/define-and-modify-a-join_2.sql)]  
  
## <a name="see-also"></a>Voir aussi  
 [Filtres de jointure](../../../relational-databases/replication/merge/join-filters.md)   
 [Filtres de lignes paramétrés](../../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md)   
 [Changer les propriétés des publications et des articles](../../../relational-databases/replication/publish/change-publication-and-article-properties.md)   
 [Filtrer des données publiées en vue de la réplication de fusion](../../../relational-databases/replication/merge/filter-published-data-for-merge-replication.md)   
 [Procédure : définir et modifier un filtre de jointure entre des articles de fusion (SQL Server Management Studio)](../../../relational-databases/replication/publish/define-and-modify-a-join-filter-between-merge-articles.md)   
 [Concepts liés aux procédures stockées système de réplication](../../../relational-databases/replication/concepts/replication-system-stored-procedures-concepts.md)   
 [Définir une relation d’enregistrement logique entre des articles de table de fusion](../../../relational-databases/replication/publish/define-a-logical-record-relationship-between-merge-table-articles.md)   
 [Définir et modifier un filtre de lignes paramétrable pour un article de fusion](../../../relational-databases/replication/publish/define-and-modify-a-parameterized-row-filter-for-a-merge-article.md)  
  
  
