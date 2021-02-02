---
title: Définir et modifier un filtre de lignes paramétrable (Fusion)
description: Découvrez comment définir et modifier un filtre de lignes paramétrable pour un article de publication de fusion pour SQL Server.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- parameterized filters [SQL Server replication], defining
- parameterized filters [SQL Server replication], modifying
- merge replication [SQL Server replication], dynamic filters
- merge replication parameterized filters [SQL Server replication]
- filters [SQL Server replication], parameterized
- modifying filters, parameterized row
- dynamic filters [SQL Server replication]
ms.assetid: de0482a2-3cc8-4030-8a4a-14364549ac9f
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 40d9ecf88b81263d6e35b5ade9ee6d0f3adeef49
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076737"
---
# <a name="define-and-modify-a-parameterized-row-filter-for-a-merge-article"></a>Définir et modifier un filtre de lignes paramétrable pour un article de fusion
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Cette rubrique explique comment définir et modifier un filtre de lignes paramétrable dans [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../../includes/tsql-md.md)].  
  
 Lorsque vous créez des articles de table, vous pouvez utiliser des filtres de lignes paramétrables. Ces filtres utilisent une clause [WHERE](../../../t-sql/queries/where-transact-sql.md) pour sélectionner les données appropriées à publier. Au lieu de spécifier une valeur littérale dans la clause (comme c'est le cas avec un filtre de lignes statique), vous spécifiez l’une des deux fonctions système suivantes ou les deux : [SUSER_SNAME](../../../t-sql/functions/suser-sname-transact-sql.md) et [HOST_NAME](../../../t-sql/functions/host-name-transact-sql.md). Pour plus d'informations, voir [Parameterized Row Filters](../../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md).  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Limitations et restrictions](#Restrictions)  
  
-   **Pour définir et modifier un filtre de lignes paramétré à l'aide de :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitations et restrictions  
  
-   Si vous ajoutez, modifiez ou supprimez un filtre de ligne paramétré après que les abonnements à la publication aient été initialisés, vous devez générer un nouvel instantané et réinitialiser tous les abonnements une fois la modification effectuée. Pour plus d’informations sur les exigences relatives aux changements de propriétés, consultez [Changer les propriétés des publications et des articles](../../../relational-databases/replication/publish/change-publication-and-article-properties.md).  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> Recommandations  
  
-   Pour des raisons de performance, il est recommandé de ne pas appliquer de fonctions aux noms de colonne dans les clauses des filtres de lignes paramétrables, telles que `LEFT([MyColumn]) = SUSER_SNAME()`. Si vous utilisez HOST_NAME dans une clause de filtrage puis en remplacez la valeur, il peut s'avérer judicieux de convertir les types de données avec la fonction CONVERT. Pour plus d'informations sur la conduite à adopter dans cette situation, consultez la section « Substitution de la valeur de HOST_NAME » de la rubrique [Parameterized Row Filters](../../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md).  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
 Définissez, modifiez et supprimez des filtres de lignes paramétrables dans la page **Filtrer les lignes de la table** de l’Assistant Nouvelle publication ou dans la page **Filtrer les lignes** de la boîte de dialogue **Propriétés de la publication - \<Publication>** . Pour plus d’informations sur l’utilisation de l’Assistant et sur l’accès à la boîte de dialogue, consultez [Créer une publication](../../../relational-databases/replication/publish/create-a-publication.md) et [Afficher et modifier les propriétés d’une publication](../../../relational-databases/replication/publish/view-and-modify-publication-properties.md).  
  
#### <a name="to-define-a-parameterized-row-filter"></a>Pour définir un filtre de lignes paramétrable  
  
1.  Dans la page **Filtrer les lignes de la table** de l’Assistant Nouvelle publication, ou dans la page **Filtrer les lignes** de la boîte de dialogue **Propriétés de la publication - \<Publication>** , cliquez sur **Ajouter**, puis sur **Ajouter un filtre**.  
  
2.  Dans la boîte de dialogue **Ajouter un filtre** , sélectionnez une table à filtrer dans la zone de liste déroulante.  
  
3.  Créez une instruction de filtrage dans la zone de texte **Instruction de filtrage** . Vous pouvez taper directement dans la zone de texte, mais vous pouvez aussi faire glisser et déposer des colonnes depuis la zone de liste **Colonnes** .  
  
    -   La zone de texte **Instruction de filtrage** comprend un texte par défaut, qui est de la forme suivante :  
  
        ```  
        SELECT <published_columns> FROM [tableowner].[tablename] WHERE  
        ```  
  
    -   Le texte par défaut ne peut pas être modifié ; tapez la clause du filtre après le mot clé WHERE en utilisant la syntaxe SQL standard. Un filtrage paramétrable comprend un appel à la fonction système **HOST_NAME()** et/ou **SUSER_SNAME()** , ou bien une fonction définie par l'utilisateur qui référence une de ces fonctions ou les deux. Voici un exemple de clause de filtrage complète pour un filtre de lignes paramétrable :  
  
        ```  
        SELECT <published_columns> FROM [HumanResources].[Employee] WHERE LoginID = SUSER_SNAME()  
        ```  
  
         La clause WHERE doit utiliser un nommage en deux parties ; les nommages en trois et en quatre parties ne sont pas pris en charge.  
  
4.  Sélectionnez l'option qui correspond aux données qui seront partagées entre des Abonnés :  
  
    -   **Une ligne de cette table ira à plusieurs abonnements**  
  
    -   **Filtre paramétré créant des partitions qui ne se chevauchent pas, avec un seul abonnement par partition**  
  
     Si vous sélectionnez **Filtre paramétré créant des partitions qui ne se chevauchent pas, avec un seul abonnement par partition**, la réplication de fusion peut optimiser les performances en stockant et en traitant moins de métadonnées. Cependant, vous devez vérifier que les données sont partitionnées de telle façon qu'une ligne ne peut pas être répliquée sur plus d'un Abonné. Pour plus d'informations, consultez la section « Définition de « partition options » » dans la rubrique [Parameterized Row Filters](../../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md).  
  
5.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
6.  Si vous êtes dans la boîte de dialogue **Propriétés de la publication - \<Publication>** , cliquez sur **OK** pour enregistrer et fermer la boîte de dialogue.  
  
#### <a name="to-modify-a-parameterized-row-filter"></a>Pour modifier un filtre de lignes paramétrable  
  
1.  Dans la page **Filtrer les lignes de la table** de l’Assistant Nouvelle publication ou dans la page **Filtrer les lignes** de la boîte de dialogue **Propriétés de la publication - \<Publication>** , sélectionnez un filtre dans le volet **Tables filtrées**, puis cliquez sur **Modifier**.  
  
2.  Dans la boîte de dialogue **Modifier le filtre** , modifiez le filtre.  
  
3.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
#### <a name="to-delete-a-parameterized-row-filter"></a>Pour supprimer un filtre de lignes paramétrable  
  
1.  Dans la page **Filtrer les lignes de la table** de l’Assistant Nouvelle publication ou dans la page **Filtrer les lignes** de la boîte de dialogue **Propriétés de la publication - \<Publication>** , sélectionnez un filtre dans le volet **Tables filtrées**, puis cliquez sur **Supprimer**.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
 Les filtres de lignes paramétrables peuvent être créés et modifiés par programme en utilisant des procédures stockées de réplication.  
  
#### <a name="to-define-a-parameterized-row-filter-for-an-article-in-a-merge-publication"></a>Pour définir un filtre de lignes paramétrable pour un article dans une publication de fusion  
  
1.  Dans la base de données de publication sur le serveur de publication, exécutez [sp_addmergearticle &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md). Spécifiez **\@publication**, le nom de l’article pour **\@article**, la table qui est publiée pour **\@source_object**, la clause WHERE qui définit le filtre paramétrable pour **\@subset_filterclause** (sans `WHERE`) et affectez l’une des valeurs suivantes à **\@partition_options**, qui décrit le type de partitionnement qui résultera de l’application du filtre de lignes paramétrable :  
  
    -   **0** - le filtrage de l'article est statique ou ne génère pas un sous-ensemble unique de données pour chaque partition (partition avec chevauchement).  
  
    -   **1** - les partitions obtenues se chevauchent et les mises à jour apportées au niveau de l'Abonné ne peuvent pas modifier la partition à laquelle une ligne appartient.  
  
    -   **2** - le filtrage de l'article génère des partitions qui ne se chevauchent pas, mais plusieurs Abonnés peuvent recevoir la même partition.  
  
    -   **3** - le filtrage de l'article génère des partitions qui ne se chevauchent pas et qui sont uniques pour chaque abonnement.  
  
#### <a name="to-change-a-parameterized-row-filter-for-an-article-in-a-merge-publication"></a>Pour modifier un filtre de lignes paramétrable pour un article dans une publication de fusion  
  
1.  Dans la base de données de publication sur le serveur de publication, exécutez [sp_changemergearticle](../../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md). Spécifiez **\@publication**, **\@article**, affectez la valeur **subset_filterclause** à **\@property**, l’expression qui définit le filtre paramétrable à **\@value** (sans `WHERE`) et la valeur **1** à **\@force_invalidate_snapshot** et **\@force_reinit_subscription**.  
  
2.  Si cette modification conduit à un comportement de partitionnement différent, exécutez de nouveau [sp_changemergearticle](../../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md) . Spécifiez **\@publication**, **\@article**, affectez la valeur **partition_options** à **\@property** et l’option de partitionnement la plus appropriée à **\@value**, qui peut être l’une des suivantes :  
  
    -   **0** - le filtrage de l'article est statique ou ne génère pas un sous-ensemble unique de données pour chaque partition (partition avec chevauchement).  
  
    -   **1** - les partitions obtenues se chevauchent et les mises à jour apportées au niveau de l'Abonné ne peuvent pas modifier la partition à laquelle une ligne appartient.  
  
    -   **2** - le filtrage de l'article génère des partitions qui ne se chevauchent pas, mais plusieurs Abonnés peuvent recevoir la même partition.  
  
    -   **3** - le filtrage de l'article génère des partitions qui ne se chevauchent pas et qui sont uniques pour chaque abonnement.  
  
###  <a name="example-transact-sql"></a><a name="TsqlExample"></a> Exemple (Transact-SQL)  
 Cet exemple définit un groupe d'articles dans une publication de fusion où les articles sont filtrés à l'aide d'une série de filtres de jointure sur la table `Employee` , qui est elle-même filtrée à l'aide d'un filtre de lignes paramétrable sur la colonne **LoginID** . Pendant la synchronisation, la valeur retournée par la fonction [HOST_NAME](../../../t-sql/functions/host-name-transact-sql.md) est remplacée. Pour plus d'informations, consultez la section « Substitution de la valeur de HOST_NAME() » dans la rubrique [Parameterized Row Filters](../../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md).  
  
 [!code-sql[HowTo#sp_MergeDynamicPub1](../../../relational-databases/replication/codesnippet/tsql/define-and-modify-a-para_1.sql)]  
  
## <a name="see-also"></a>Voir aussi  
 [Définir et modifier un filtre de jointure entre des articles de fusion](../../../relational-databases/replication/publish/define-and-modify-a-join-filter-between-merge-articles.md)   
 [Changer les propriétés des publications et des articles](../../../relational-databases/replication/publish/change-publication-and-article-properties.md)   
 [Filtres de jointure](../../../relational-databases/replication/merge/join-filters.md)   
 [Filtres de lignes paramétrés](../../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md)  
  
  
