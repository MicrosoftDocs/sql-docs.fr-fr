---
description: Définir et modifier un filtre de colonne
title: Définir et modifier un filtre de colonne | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- filters [SQL Server replication], column
- modifying filters, column
- modifying filters
- column filters [SQL Server replication]
ms.assetid: d7c3186a-9a8c-45d8-ab34-05beec4c26dd
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: e266e188719bd5ed3d84adcf81d49aa175f22872
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076757"
---
# <a name="define-and-modify-a-column-filter"></a>Définir et modifier un filtre de colonne
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]
  Cette rubrique décrit comment définir et modifier un filtre de colonne dans [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../../includes/tsql-md.md)].  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Limitations et restrictions](#Restrictions)  
  
-   **Pour définir et modifier un filtre de colonne à l'aide de :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitations et restrictions  
  
-   Certaines colonnes ne peuvent pas être filtrées. Pour plus d’informations, consultez [Filtrer des données publiées](../../../relational-databases/replication/publish/filter-published-data.md). Si vous modifiez un filtre de colonne après que des abonnements ont été initialisés, vous devez générer un nouvel instantané et réinitialiser tous les abonnements après avoir effectué la modification. Pour plus d’informations sur les exigences relatives aux changements de propriétés, consultez [Changer les propriétés des publications et des articles](../../../relational-databases/replication/publish/change-publication-and-article-properties.md).  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
 Définissez des filtres de colonnes sur la page **Articles** de l'Assistant Nouvelle Publication. Pour plus d’informations sur l’utilisation de l’Assistant Nouvelle publication, consultez [Créer une publication](../../../relational-databases/replication/publish/create-a-publication.md).  
  
 Définissez et modifiez des filtres de colonnes dans la page **Articles** de la boîte de dialogue **Propriétés de la publication - \<Publication>** . Pour plus d’informations sur les propriétés des publications et des articles, consultez [Afficher et modifier les propriétés d’une publication](../../../relational-databases/replication/publish/view-and-modify-publication-properties.md).  
  
#### <a name="to-define-a-column-filter"></a>Pour définir un filtre de colonne  
  
1.  Sur la page **Articles** de l'Assistant Nouvelle publication, développez la table à filtrer dans le volet **Objets à publier** .  
  
2.  Activez la case à cocher en regard des colonnes que vous voulez filtrer.  
  
#### <a name="to-modify-column-filtering"></a>Pour modifier le filtrage des colonnes  
  
1.  Dans la page **Articles** de la boîte de dialogue **Propriétés de la publication - \<Publication>** , développez la table à filtrer dans le volet **Objets à publier**.  
  
2.  Désactivez la case à cocher en regard de chaque colonne que vous voulez filtrer, et vérifiez que la case à cocher est activée pour chaque colonne qui doit être incluse dans l'article.  
  
3.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
 Lors de la création d'articles de table, vous pouvez définir les colonnes à inclure dans l'article et modifier ces colonnes une fois l'article défini. Vous pouvez créer et modifier par programme des colonnes filtrées en utilisant des procédures stockées de réplication.  
  
> [!NOTE]  
>  Les procédures suivantes supposent que la table sous-jacente n'a pas été modifiée. Pour plus d’informations sur la réplication des modifications du langage de définition de données (DDL) sur les tables publiées, consultez [Modifier le schéma dans les bases de données de publication](../../../relational-databases/replication/publish/make-schema-changes-on-publication-databases.md).  
  
#### <a name="to-define-a-column-filter-for-an-article-published-in-a-snapshot-or-transactional-publication"></a>Pour définir un filtre de colonne pour un article publié dans une publication transactionnelle ou d'instantané  
  
1.  Définissez l'article à filtrer. Pour plus d’informations, consultez [définir un Article](../../../relational-databases/replication/publish/define-an-article.md).  
  
2.  Dans la base de données de publication sur le serveur de publication, exécutez [sp_articlecolumn](../../../relational-databases/system-stored-procedures/sp-articlecolumn-transact-sql.md). Les colonnes à inclure ou à supprimer de l'article sont alors définies.  
  
    -   En cas de publication de quelques colonnes d'une table en contenant de nombreuses, exécutez [sp_articlecolumn](../../../relational-databases/system-stored-procedures/sp-articlecolumn-transact-sql.md) une fois pour chaque colonne ajoutée. Spécifiez le nom de la colonne pour **\@column** et affectez la valeur **add** à **\@operation**.  
  
    -   En cas de publication de la plupart des colonnes d’une table en contenant de nombreuses, exécutez [sp_articlecolumn](../../../relational-databases/system-stored-procedures/sp-articlecolumn-transact-sql.md), en affectant la valeur **null** à **\@column** et la valeur **add** à **\@operation** pour ajouter toutes les colonnes. Exécutez ensuite [sp_articlecolumn](../../../relational-databases/system-stored-procedures/sp-articlecolumn-transact-sql.md), une fois pour chaque colonne qui est exclue, en affectant la valeur **drop** à **\@operation** et le nom de la colonne exclue à **\@column**.  
  
3.  Dans la base de données de publication sur le serveur de publication, exécutez [sp_articleview](../../../relational-databases/system-stored-procedures/sp-articleview-transact-sql.md). Spécifiez le nom de la publication pour **\@publication** et le nom de l’article filtré pour **\@article**. Les objets de synchronisation pour l'article filtré sont alors créés.  
  
#### <a name="to-change-a-column-filter-to-include-additional-columns-for-an-article-published-in-a-snapshot-or-transactional-publication"></a>Pour modifier un filtre de colonne de manière à inclure des colonnes supplémentaires pour un article publié dans une publication transactionnelle ou d'instantané  
  
1.  Dans la base de données de publication sur le serveur de publication, exécutez [sp_articlecolumn](../../../relational-databases/system-stored-procedures/sp-articlecolumn-transact-sql.md) une fois pour chaque colonne ajoutée. Spécifiez le nom de la colonne pour **\@column** et affectez la valeur **add** à **\@operation**.  
  
2.  Dans la base de données de publication sur le serveur de publication, exécutez [sp_articleview](../../../relational-databases/system-stored-procedures/sp-articleview-transact-sql.md). Spécifiez le nom de la publication pour **\@publication** et le nom de l’article filtré pour **\@article**. Si la publication contient des abonnements existants, affectez la valeur **1** à **\@change_active**. Les objets de synchronisation pour l'article filtré sont alors recréés.  
  
3.  Exécutez de nouveau le travail de l'Agent d'instantané pour la publication afin de générer un instantané mis à jour.  
  
4.  Réinitialiser les abonnements. Pour plus d’informations, consultez [Réinitialiser des abonnements](../../../relational-databases/replication/reinitialize-subscriptions.md).  
  
#### <a name="to-change-a-column-filter-to-remove-columns-for-an-article-published-in-a-snapshot-or-transactional-publication"></a>Pour modifier un filtre de colonne de manière à supprimer des colonnes pour un article publié dans une publication transactionnelle ou d'instantané  
  
1.  Dans la base de données de publication sur le serveur de publication, exécutez [sp_articlecolumn](../../../relational-databases/system-stored-procedures/sp-articlecolumn-transact-sql.md) une fois pour chaque colonne supprimée. Spécifiez le nom de la colonne pour **\@column** et affectez la valeur **drop** à **\@operation**.  
  
2.  Dans la base de données de publication sur le serveur de publication, exécutez [sp_articleview](../../../relational-databases/system-stored-procedures/sp-articleview-transact-sql.md). Spécifiez le nom de la publication pour **\@publication** et le nom de l’article filtré pour **\@article**. Si la publication contient des abonnements existants, affectez la valeur **1** à **\@change_active**. Les objets de synchronisation pour l'article filtré sont alors recréés.  
  
3.  Exécutez de nouveau le travail de l'Agent d'instantané pour la publication afin de générer un instantané mis à jour.  
  
4.  Réinitialiser les abonnements. Pour plus d’informations, consultez [Réinitialiser des abonnements](../../../relational-databases/replication/reinitialize-subscriptions.md).  
  
#### <a name="to-define-a-column-filter-for-an-article-published-in-a-merge-publication"></a>Pour définir un filtre de colonne pour un article publié dans une publication de fusion  
  
1.  Définissez l'article à filtrer. Pour plus d’informations, consultez [définir un Article](../../../relational-databases/replication/publish/define-an-article.md).  
  
2.  Dans la base de données de publication sur le serveur de publication, exécutez [sp_mergearticlecolumn](../../../relational-databases/system-stored-procedures/sp-mergearticlecolumn-transact-sql.md). Les colonnes à inclure ou à supprimer de l'article sont alors définies.  
  
    -   En cas de publication de quelques colonnes d'une table en contenant de nombreuses, exécutez [sp_mergearticlecolumn](../../../relational-databases/system-stored-procedures/sp-mergearticlecolumn-transact-sql.md) une fois pour chaque colonne ajoutée. Spécifiez le nom de la colonne pour **\@column** et affectez la valeur **add** à **\@operation**.  
  
    -   En cas de publication de la plupart des colonnes d’une table en contenant de nombreuses, exécutez [sp_mergearticlecolumn](../../../relational-databases/system-stored-procedures/sp-mergearticlecolumn-transact-sql.md), en affectant la valeur **null** à **\@column** et la valeur **add** à **\@operation** pour ajouter toutes les colonnes. Exécutez ensuite [sp_mergearticlecolumn](../../../relational-databases/system-stored-procedures/sp-mergearticlecolumn-transact-sql.md), une fois pour chaque colonne qui est exclue, en affectant la valeur **drop** à **\@operation** et le nom de la colonne exclue à **\@column**.  
  
#### <a name="to-change-a-column-filter-to-include-additional-columns-for-an-article-published-in-a-merge-publication"></a>Pour modifier un filtre de colonne de manière à inclure des colonnes supplémentaires pour un article publié dans une publication de fusion  
  
1.  Dans la base de données de publication sur le serveur de publication, exécutez [sp_mergearticlecolumn](../../../relational-databases/system-stored-procedures/sp-mergearticlecolumn-transact-sql.md) une fois pour chaque colonne ajoutée. Spécifiez le nom de colonne pour **\@column**, affectez la valeur **add** à **\@operation** et la valeur **1** à **\@force_invalidate_snapshot** et à **\@force_reinit_subscription**.  
  
2.  Exécutez de nouveau le travail de l'Agent d'instantané pour la publication afin de générer un instantané mis à jour.  
  
3.  Réinitialiser les abonnements. Pour plus d’informations, consultez [Réinitialiser des abonnements](../../../relational-databases/replication/reinitialize-subscriptions.md).  
  
#### <a name="to-change-a-column-filter-to-remove-columns-for-an-article-published-in-a-merge-publication"></a>Pour modifier un filtre de colonne de manière à supprimer des colonnes pour un article publié dans une publication de fusion  
  
1.  Dans la base de données de publication sur le serveur de publication, exécutez [sp_mergearticlecolumn](../../../relational-databases/system-stored-procedures/sp-mergearticlecolumn-transact-sql.md) une fois pour chaque colonne supprimée. Spécifiez le nom de colonne pour **\@column**, affectez la valeur **drop** à **\@operation** et la valeur **1** à **\@force_invalidate_snapshot** et à **\@force_reinit_subscription**.  
  
2.  Exécutez de nouveau le travail de l'Agent d'instantané pour la publication afin de générer un instantané mis à jour.  
  
3.  Réinitialiser les abonnements. Pour plus d’informations, consultez [Réinitialiser des abonnements](../../../relational-databases/replication/reinitialize-subscriptions.md).  
  
###  <a name="example-transact-sql"></a><a name="TsqlExample"></a> Exemple (Transact-SQL)  
 Dans cet exemple de réplication transactionnelle, la colonne `DaysToManufacture` est supprimée d'un article reposant sur la table `Product` .  
  
 [!code-sql[HowTo#sp_AddTranArticle](../../../relational-databases/replication/codesnippet/tsql/define-and-modify-a-colu_1.sql)]  
  
 Dans cet exemple de réplication de fusion, la colonne `CreditCardApprovalCode` est supprimée d'un article reposant sur la table `SalesOrderHeader` .  
  
 [!code-sql[HowTo#sp_AddMergeArticle](../../../relational-databases/replication/codesnippet/tsql/define-and-modify-a-colu_2.sql)]  
  
## <a name="see-also"></a>Voir aussi  
 [Changer les propriétés des publications et des articles](../../../relational-databases/replication/publish/change-publication-and-article-properties.md)   
 [Filtrer des données publiées](../../../relational-databases/replication/publish/filter-published-data.md)   
 [Filtrer des données publiées en vue de la réplication de fusion](../../../relational-databases/replication/merge/filter-published-data-for-merge-replication.md)  
  
  
