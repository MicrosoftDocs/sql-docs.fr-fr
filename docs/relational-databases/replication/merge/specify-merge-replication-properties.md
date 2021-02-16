---
description: Spécifier les propriétés de la réplication de fusion
title: Spécifier les propriétés de la réplication de fusion | Microsoft Docs
ms.custom: ''
ms.date: 11/20/2018
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- merge replication [SQL Server replication], download-only articles
- articles [SQL Server replication], download-only
- download-only articles
ms.assetid: 14839cec-6dbf-49c2-aa27-56847b09b4db
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: f53a43fa102505121cd176eca6c94d4697a25372
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100336186"
---
# <a name="specify-merge-replication-properties"></a>Spécifier les propriétés de la réplication de fusion
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
Cette rubrique explique comment spécifier différentes propriétés pour votre réplication de fusion. 

## <a name="merge-article-is-download-only"></a>Article sur la fusion en téléchargement uniquement
Les articles en téléchargement uniquement sont conçus pour les applications dont les données ne sont pas mises à jour au niveau des Abonnés. Pour plus d’informations, consultez [Optimiser les performances de la réplication de fusion avec les articles en téléchargement seul](../../../relational-databases/replication/merge/optimize-merge-replication-performance-with-download-only-articles.md).  
   
###  <a name="considerations"></a>Considérations   
-   Si vous spécifiez qu'un article est en téléchargement seul après l'initialisation des abonnements, tous les abonnements client qui ont reçu l'aticle doivent être réinitialisés. Les abonnements serveur n'ont pas besoin d'être réinitialisés. Pour plus d’informations sur les effets des modifications de propriétés, consultez [Changer des propriétés de publication et d’article](../../../relational-databases/replication/publish/change-publication-and-article-properties.md).  
  
### <a name="use-sql-server-management-studio"></a>Utiliser SQL Server Management Studio  
  
#### <a name="on-the-articles-page"></a>Dans la page Articles
Dans la page **Articles** de l'Assistant Nouvelle publication, sélectionnez une table, puis activez la case à cocher **La table sélectionnée est en téléchargement seul**.  
  
#### <a name="on-the-properties-tab-of-the-article-properties"></a>Sous l’onglet Propriétés de Propriétés de l’article  
  
1.  Dans la page **Articles** de l’Assistant Nouvelle publication ou la boîte de dialogue **Propriétés de la publication - \<Publication>** , sélectionnez une table, puis cliquez sur **Propriétés de l’article**.    
2.  Cliquez sur **Définir les propriétés de l'article de la table en surbrillance** ou **Définir les propriétés de tous les articles de la table**.  
  
3.  Dans la section **Objet de destination** de l’onglet **Propriétés** de la boîte de dialogue **Propriétés de l’article - \<Article>** , spécifiez l’une des valeurs suivantes pour **Direction de la synchronisation** :    
    -   **Téléchargement seul pour l'Abonné, interdire les modifications de l'Abonné**    
    -   **Téléchargement seul pour l'Abonné, autoriser les modifications de l'Abonné**    
4.  Si vous êtes dans la boîte de dialogue **Propriétés de la publication - \<Publication>** , cliquez sur **OK** pour enregistrer et fermer la boîte de dialogue.  

###  <a name="use-transact-sql"></a>Utiliser Transact-SQL  
  
#### <a name="new-article"></a>Nouvel article  
  
1.  Exécutez [sp_addmergearticle](../../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md), en affectant la valeur **1** ou de **2** au paramètre `@subscriber_upload_options`. Ces chiffres correspondent aux comportements suivants :  
  
    -   **0** - aucune restriction (valeur par défaut). Les modifications sur l'abonné sont téléchargées par le serveur de publication.    
    -   **1** -  modifications autorisées au niveau de l'Abonné, mais elles ne sont pas téléchargées sur le serveur de publication.    
    -   **2** - modifications interdites au niveau de l'Abonné.  
  
       > [!NOTE]  
       > Si la table source d’un article est déjà publiée dans une autre publication, la valeur de `@subscriber_upload_options` doit être la même pour les deux articles.  
  
#### <a name="existing-article"></a>Article existant
  
1.  Pour déterminer si un article est en téléchargement uniquement, exécutez [sp_helpmergearticle](../../../relational-databases/system-stored-procedures/sp-helpmergearticle-transact-sql.md) et vérifiez la valeur de **upload_options** pour l’article dans le jeu de résultats. 
  
2.  Si la valeur retournée à l’étape 1 est **0**, exécutez [sp_changemergearticle](../../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md), en affectant la valeur **subscriber_upload_options** à `@property`, la valeur **1** à `@force_invalidate_snapshot` et `@force_reinit_subscription`, et la valeur **1** ou **2** à `@value`, qui correspondent aux comportements suivants :  
  
    -   **1** -  modifications autorisées au niveau de l'Abonné, mais elles ne sont pas téléchargées sur le serveur de publication.    
    -   **2** - modifications interdites au niveau de l'Abonné.  
  
        > [!NOTE]  
        >  Si la table source d'un article est déjà publiée dans une autre publication, les deux articles doivent être en téléchargement uniquement ou ne pas l'être.  
  
## <a name="interactive-conflict-resolution"></a>Interactive Conflict Resolution 
  
 La réplication Microsoft SQL Server propose un outil de résolution interactif qui vous permet de résoudre manuellement des conflits au cours d’une synchronisation à la demande dans le Gestionnaire de synchronisation Microsoft Windows. Quand la résolution de conflits est activée, résolvez les conflits de façon interactive au cours de la synchronisation, à l'aide du résolveur interactif. L’outil de résolution interactif est disponible par le biais du Gestionnaire de synchronisation Microsoft Windows. Pour plus d’informations, consultez [Synchroniser un abonnement à l’aide du Gestionnaire de synchronisation Windows &#40;Gestionnaire de synchronisation Windows&#41;](../../../relational-databases/replication/synchronize-a-subscription-using-windows-synchronization-manager.md).  
  
  
### <a name="recommendations"></a><a name="Recommendations"></a> Recommandations  
  
-   Si une synchronisation est effectuée en dehors du Gestionnaire de synchronisation Windows (comme une synchronisation planifiée ou à la demande dans SQL Server Management Studio ou le moniteur de réplication), les conflits sont résolus automatiquement sans intervention de l'utilisateur, en utilisant l'outil de résolution des conflits par défaut spécifié pour l'article. Pour plus d’informations, consultez [Interactive Conflict Resolution](../../../relational-databases/replication/merge/advanced-merge-replication-conflict-interactive-resolution.md).  
  
### <a name="use-sql-server-management-studio"></a>Utiliser SQL Server Management Studio  
  
#### <a name="enable-interactive-conflict-resolution-for-an-article"></a>Activer la résolution de conflits interactive pour un article  
  
1.  Dans la page **Articles** de l’Assistant Nouvelle publication ou la boîte de dialogue **Propriétés de la publication - \<Publication>** , sélectionnez une table. Pour plus d’informations sur l’utilisation de l’Assistant et sur l’accès à la boîte de dialogue, consultez [Créer une publication](../../../relational-databases/replication/publish/create-a-publication.md) et [Afficher et modifier les propriétés d’une publication](../../../relational-databases/replication/publish/view-and-modify-publication-properties.md).   
2.  Cliquez sur **Propriétés de l'article**, puis cliquez sur **Définir les propriétés de l'article de Table en surbrillance** ou sur **Définir les propriétés de tous les articles de table**.    
3.  Dans la page **Propriétés de l’article - \<Article>** ou **Propriétés de l’article - \<ArticleType>** , cliquez sur l’onglet **Résolveur**.    
4.  Sélectionnez **Autoriser l'abonné à résoudre les conflits de manière interactive au cours de la synchronisation à la demande**.    
5.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]    
6.  Si vous êtes dans la boîte de dialogue **Propriétés de la publication - \<Publication>** , cliquez sur **OK** pour enregistrer et fermer la boîte de dialogue.  
  
#### <a name="specify-that-a-subscription-should-use-interactive-conflict-resolution"></a>Spécifier qu’un abonnement doit utiliser la résolution de conflits interactive  
  
1.  Dans la boîte de dialogue **Propriétés de l’abonnement - \<Subscriber> : \<SubscriptionDatabase>** , spécifiez la valeur **True** pour l’option **Résoudre les conflits interactivement**. Pour plus d'informations sur l'accès à cette boîte de dialogue, consultez [View and Modify Push Subscription Properties](../../../relational-databases/replication/view-and-modify-push-subscription-properties.md) et [View and Modify Pull Subscription Properties](../../../relational-databases/replication/view-and-modify-pull-subscription-properties.md).   
2.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
###  <a name="use-transact-sql"></a>Utiliser Transact-SQL  
 Vous pouvez spécifier par programme qu'un Abonné utilisera cette interface graphique pour résoudre les conflits dans les articles lorsqu'un abonnement par extraction à une publication de fusion est créé. Seuls les conflits dans les articles qui prennent en charge cette option seront affichés dans le programme de résolution interactif.  
  
#### <a name="create-a-merge-pull-subscription-that-uses-the-interactive-resolver"></a>Créer un abonnement de fusion par extraction qui utilise l’outil de résolution interactif  
  
1.  Dans la base de données de publication sur le serveur de publication, exécutez [sp_helpmergearticle](../../../relational-databases/system-stored-procedures/sp-helpmergepublication-transact-sql.md), en spécifiant `@publication`. Notez la valeur de **allow_interactive_resolver** pour chaque article du jeu de résultats pour lequel le programme de résolution interactif sera utilisé.   
    -   Si cette valeur est **1**, le programme de résolution interactif sera utilisé.    
    -   Si cette valeur est **0**, vous devez tout d'abord activer le programme de résolution interactif pour chaque article. Pour cela, exécutez [sp_changemergearticle](../../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md), en spécifiant `@publication`, `@article`, en affectant la valeur **allow_interactive_resolver** à `@property` et la valeur **true** à `@value`.    
2.  Dans la base de données d'abonnement de l'Abonné, exécutez [sp_addmergepullsubscription](../../../relational-databases/system-stored-procedures/sp-addmergepullsubscription-transact-sql.md). Pour plus d’informations, consultez [Créer un abonnement par extraction de données (pull)](../../../relational-databases/replication/create-a-pull-subscription.md).    
3.  Dans la base de données d'abonnement de l'Abonné, exécutez [sp_addmergepullsubscription_agent](../../../relational-databases/system-stored-procedures/sp-addmergepullsubscription-agent-transact-sql.md), en spécifiant les paramètres suivants :    
    -   `@publisher`, `@publisher_db` (la base de données publiée) et `@publication`.    
    -   La valeur **true** pour `@enabled_for_syncmgr`.    
    -   La valeur **true** pour `@use_interactive_resolver`.    
    -   Les informations de compte de sécurité requises par l'Agent de fusion. Pour plus d’informations, consultez [Créer un abonnement par extraction de données (pull)](../../../relational-databases/replication/create-a-pull-subscription.md).    
4.  Dans la base de données de publication sur le serveur de publication, exécutez [sp_addmergesubscription](../../../relational-databases/system-stored-procedures/sp-addmergesubscription-transact-sql.md).  
  
#### <a name="define-an-article-that-supports-the-interactive-resolver"></a>Définir un article qui prend en charge l’outil de résolution interactif  
  
1.  Dans la base de données de publication sur le serveur de publication, exécutez [sp_addmergearticle](../../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md). Spécifiez le nom de la publication à laquelle l’article appartient pour `@publication`, le nom de l’article pour `@article`, l’objet de base de données qui est publié pour `@source_object` et la valeur **true** pour `@allow_interactive_resolver`. Pour plus d’informations, consultez [définir un Article](../../../relational-databases/replication/publish/define-an-article.md).  
 
## <a name="conflict-tracking-and-resolution-level-for-merge-articles"></a>Niveau de suivi et de résolution des conflits pour les articles de fusion
Cette rubrique explique comment spécifier le niveau de suivi et de résolution des conflits pour les articles de fusion dans [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../../includes/tsql-md.md)].  
  
 Lorsqu'un abonnement à une publication de fusion est synchronisé, la réplication vérifie la présence de conflits faisant suite à de la modification des mêmes données au niveau du serveur de publication et de l'Abonné. Vous pouvez spécifier si les conflits sont détectés au niveau de la ligne, auquel cas toute modification apportée à la ligne est considérée comme un conflit, ou au niveau de la colonne, auquel cas seules les modifications apportées aux mêmes ligne et colonne sont considérées comme un conflit. La résolution des conflits pour les articles est réalisée au niveau de la ligne. Pour plus d'informations sur la détection et la résolution des conflits avec des enregistrements logiques, consultez [Detecting and Resolving Conflicts in Logical Records](../../../relational-databases/replication/merge/advanced-merge-replication-conflict-resolving-in-logical-record.md).  
 
### <a name="limitations-and-restrictions"></a>Limitations et restrictions  
  
-   Si vous modifiez le niveau de suivi une fois les abonnements initialisés, ces derniers doivent être réinitialisés. Pour plus d’informations sur les effets des modifications de propriétés, consultez [Changer des propriétés de publication et d’article](../../../relational-databases/replication/publish/change-publication-and-article-properties.md).   
-   Avec le suivi au niveau des lignes et des colonnes, la résolution des conflits est toujours effectuée au niveau des lignes : la ligne gagnante remplace la ligne perdante. La réplication de fusion vous permet également de spécifier que les conflits sont suivis et résolus au niveau des enregistrements logiques, mais ces options ne sont pas disponibles à partir de [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)]. Pour plus d'informations sur la définition de ces options à partir des procédures stockées de réplication, consultez [Définir une relation d’enregistrement logique entre des articles de table de fusion](../../../relational-databases/replication/publish/define-a-logical-record-relationship-between-merge-table-articles.md).  
  
### <a name="use-sql-server-management-studio"></a>Utiliser SQL Server Management Studio  
 Spécifiez le suivi au niveau des lignes ou des colonnes pour les articles de fusion sous l’onglet **Propriétés** de la boîte de dialogue **Propriétés de l’article**, disponible dans l’Assistant Nouvelle publication et la boîte de dialogue **Propriétés de la publication - \<Publication>** . Pour plus d’informations sur l’utilisation de l’Assistant et sur l’accès à la boîte de dialogue, consultez [Créer une publication](../../../relational-databases/replication/publish/create-a-publication.md) et [Afficher et modifier les propriétés d’une publication](../../../relational-databases/replication/publish/view-and-modify-publication-properties.md).  
  
#### <a name="specify-row--or-column-level-tracking"></a>Spécifier le suivi au niveau des lignes ou des colonnes  
  
1.  Dans la page **Articles** de l’Assistant Nouvelle publication ou la boîte de dialogue **Propriétés de la publication - \<Publication>** , sélectionnez une table.  
2.  Cliquez sur **Propriétés de l'article**, puis cliquez sur **Définir les propriétés de l'article de Table en surbrillance** ou sur **Définir les propriétés de tous les articles de table**.   
3.  Sous l’onglet **Propriétés** de la boîte de dialogue **Propriétés de l’article \<Article>** , sélectionnez l’une des valeurs suivantes pour la propriété **Niveau de suivi** : **Suivi au niveau des lignes** ou **Suivi au niveau des colonnes**.   
4.  Si vous êtes dans la boîte de dialogue **Propriétés de la publication - \<Publication>** , cliquez sur **OK** pour enregistrer et fermer la boîte de dialogue.  
  
### <a name="use-transact-sql"></a>Utiliser Transact-SQL  
  
#### <a name="to-specify-conflict-tracking-options-for-a-new-merge-article"></a>Pour spécifier les options de suivi des conflits pour un nouvel article de fusion  
  
1.  Dans la base de données de publication sur le serveur de publication, exécutez [sp_addmergearticle](../../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md) et spécifiez l’une des valeurs suivantes pour `@column_tracking` :  
  
    -   **true** - utiliser le suivi au niveau des colonnes pour l'article.    
    -   **false** - utiliser le suivi au niveau des lignes, qui est la valeur par défaut.  
  
#### <a name="change-conflict-tracking-options-for-a-merge-article"></a>Changer les options de suivi des conflits pour un article de fusion  
  
1.  Pour déterminer les options de suivi des conflits pour un article de fusion, exécutez [sp_helpmergearticle](../../../relational-databases/system-stored-procedures/sp-helpmergearticle-transact-sql.md). Notez la valeur de l'option **column_tracking** dans le jeu de résultats de l'article. La valeur **1** indique que le suivi au niveau des colonnes est utilisé, tandis que la valeur **0** indique que le suivi au niveau des lignes est utilisé.    
2.  Dans la base de données de publication sur le serveur de publication, exécutez [sp_changemergearticle](../../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md). Affectez la valeur **column_tracking** à `@property` et l’une des valeurs suivantes à `@value` :  
  
    -   **true** - utiliser le suivi au niveau des colonnes pour l'article.    
    -   **false** - utiliser le suivi au niveau des lignes, qui est la valeur par défaut.  
  
     Affectez la valeur **1** à `@force_invalidate_snapshot` et `@force_reinit_subscription`. 

## <a name="manage-tracking--deletes"></a>Gérer les suppressions de suivi
    
> [!NOTE]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../../includes/ssnotedepfutureavoid-md.md)]  
  
 Par défaut, la réplication de fusion synchronise les commandes DELETE entre le serveur de publication et l'Abonné. La réplication de fusion vous permet de conserver des lignes qui ont été supprimées de la publication dans la base de données d'abonnement, et vice versa. Vous pouvez spécifier par programme que les commandes DELETE doivent être ignorées lors de la création d'un article ou vous pouvez activer cette fonctionnalité ultérieurement à l'aide de procédures stockées de réplication.  
  
> [!IMPORTANT]  
>  L'activation de cette fonctionnalité conduit à une non-convergence ; autrement dit, les données présentes au niveau de l'Abonné ne reflèteront pas correctement les données présentes au niveau du serveur de publication. Vous devez implémenter votre propre mécanisme de manière à supprimer manuellement les lignes supprimées.  
  
### <a name="specify-that-deletes-be-ignored-for-a-new-merge-article"></a>Spécifier que les suppressions doivent être ignorées pour un nouvel article de fusion  
  
Dans la base de données de publication sur le serveur de publication, exécutez [sp_addmergearticle &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md). Affectez la valeur **false** à `@delete_tracking`. Pour plus d’informations, consultez [définir un Article](../../../relational-databases/replication/publish/define-an-article.md).
  
> [!NOTE]  
>  Si la table source d'un article est déjà publiée dans une autre publication, la valeur de **delete_tracking** doit être la même pour les deux articles.  
  
### <a name="specify-that-deletes-be-ignored-for-an-existing-merge-article"></a>Spécifier que les suppressions doivent être ignorées pour un article de fusion existant  
  
1.  Pour déterminer si la compensation d’erreur est activée pour un article, exécutez [sp_helpmergearticle &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-helpmergearticle-transact-sql.md) et notez la valeur de **delete_tracking** dans le jeu de résultats. Si cette valeur est **0**, les suppressions sont déjà ignorées.  
  
2.  Si la valeur à l’étape 1 est **1**, exécutez [sp_changemergearticle &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md) sur la base de données de publication au niveau du serveur de publication. Affectez la valeur `delete_tracking` à `@property` et la valeur **false** à `@value`.  
  
    > [!NOTE]  
    >  Si la table source d'un article est déjà publiée dans une autre publication, la valeur de **delete_tracking** doit être la même pour les deux articles.  
  

## <a name="processing-order"></a>Ordre de traitement
  La réplication de fusion vous permet de spécifier l'ordre dans lequel les articles sont traités par l'Agent de fusion pendant le processus de synchronisation. Vous pouvez attribuer par programme un ordre à chaque article lors de la création de l'article à l'aide des procédures stockées de réplication. Les articles sont traités à partir de la valeur la plus faible vers la valeur la plus élevée. Si deux articles ont la même valeur, ils sont traités simultanément. 

### <a name="how-processing-order-is-determined"></a>Détermination de l'ordre de traitement  
 Lors d'une synchronisation de fusion, les articles sont par défaut traités dans l'ordre requis par les dépendances entre les objets, et notamment les contraintes d'intégrité référentielle déclarative (DRI, Declarative Referential Integrity) définies sur les tables de base de données. Le traitement implique l'énumération des modifications apportées à une table, puis l'application de ces modifications. Si aucune intégrité référentielle déclarative n'est présente mais que des filtres de jointure ou des enregistrements logiques existent entre les articles des tables, les articles sont traités dans l'ordre requis par les filtres et les enregistrements logiques. Les articles non liés à d’autres articles via une intégrité référentielle déclarative, des filtres de jointure, des enregistrements logiques ou d’autres dépendances sont traités en fonction du surnom de l’article dans la table système [sysmergearticles &#40;Transact-SQL&#41;](../../../relational-databases/system-tables/sysmergearticles-transact-sql.md).  
  
 Supposons une publication qui contient les tables **SalesOrderHeader** et **SalesOrderDetail** avec une colonne clé primaire **SalesOrderID** pour la table **SalesOrderHeader** et une colonne clé étrangère **SalesOrderID** pour la table **SalesOrderDetail** . Lors de la synchronisation, la réplication de fusion empêche les violations de clé étrangère en insérant toutes les nouvelles lignes dans **SalesOrderHeader** avant d'insérer les lignes associées dans **SalesOrderDetail**. De la même façon, les lignes sont supprimées dans **SalesOrderDetail** avant que la ligne associée soit supprimée dans **SalesOrderHeader**.  
  
 Cependant, dans certaines applications, l'intégrité référentielle est appliquée via des déclencheurs de base de données ou bien au niveau de l'application, et non pas via une intégrité référentielle déclarative. Étant donnée la publication décrite plus haut, à la place de l'intégrité référentielle déclarative, la table **SalesOrderDetail** pourrait avoir un déclencheur d'insertion qui vérifie que la ligne associée dans la table **SalesOrderHeader** existe avant d'autoriser une insertion. **SalesOrderHeader** pourrait avoir un déclencheur de suppression qui vérifie qu'il n' y a pas de ligne associée dans la table **SalesOrderDetail** avant d'autoriser une suppression. La réplication de fusion ne prend pas en compte les déclencheurs lors de la détermination de l'ordre de traitement des articles car elle ne peut pas déterminer ce que sera le résultat du déclencheur avant qu'il soit exécuté. De la même façon, la réplication ne peut pas prendre en compte les contraintes définies au niveau de l'application.  
  
 Quand l'intégrité référentielle est gérée via des déclencheurs ou au niveau de l'application, vous devez spécifier l'ordre dans lequel les articles doivent être traités. Dans l'exemple avec les déclencheurs, vous pouvez spécifier que la table **SalesOrderHeader** doit être traitée avant **SalesOrderDetail**, car l'ordre des articles est basé sur l'ordre d'insertion. La réplication de fusion inverse automatiquement l'ordre pour les suppressions. La réplication de fusion n'échoue pas s'il n'y a pas d'ordre pour les articles, car l'Agent de fusion continue à traiter les articles si une violation de contrainte se produit ; il essaye ensuite à nouveau toutes les opérations qui ont échoué, après le traitement des autres articles. La spécification de l'ordre des articles évite simplement les nouveaux essais et le temps de traitement supplémentaire qui y est associé. Si vous spécifiez un ordre incorrect (par exemple un ordre tel que les enregistrements de détail soient traités avant les enregistrements d'en-tête), la réplication de fusion essaye à nouveau le traitement jusqu'à ce qu'il réussisse.   
  
### <a name="new-article"></a>Nouvel article
  
1.  Dans la base de données de publication sur le serveur de publication, exécutez [sp_addmergearticle &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md). Spécifiez une valeur entière qui représente l’ordre de traitement de l’article pour `@processing_order`. Pour plus d’informations, consultez [définir un Article](../../../relational-databases/replication/publish/define-an-article.md).  
  
    > [!NOTE]  
    >  Lorsque vous créez des articles ordonnés, vous devez laisser des intervalles entre les valeurs d'ordre des articles. Cela permet de définir facilement de nouvelles valeurs dans le futur. Par exemple, si vous avez trois articles pour lesquels vous devez spécifier un ordre de traitement fixe, affectez à `@processing_order` les valeurs 10, 20 et 30 plutôt que 1, 2 et 3, respectivement.  
  
### <a name="existing-article"></a>Article existant
  
1.  Pour déterminer l’ordre de traitement d’un article, exécutez [sp_helpmergearticle &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-helpmergearticle-transact-sql.md) et notez la valeur de **processing_order** dans le jeu de résultats.   
2.  Dans la base de données de publication sur le serveur de publication, exécutez [sp_changemergearticle &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md). Spécifiez la valeur **processing_order** pour `@property` et une valeur entière qui représente l’ordre de traitement pour `@value`.  


## <a name="see-also"></a>Voir aussi  
 [Optimiser les performances de la réplication de fusion avec les articles en téléchargement seul](../../../relational-databases/replication/merge/optimize-merge-replication-performance-with-download-only-articles.md)   
 [Define an Article](../../../relational-databases/replication/publish/define-an-article.md)   
 [Afficher et modifier les propriétés d’un article](../../../relational-databases/replication/publish/view-and-modify-article-properties.md)  
  
  
