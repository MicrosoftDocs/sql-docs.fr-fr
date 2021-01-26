---
title: Afficher et modifier les propriétés d’une publication | Microsoft Docs
description: Découvrez comment afficher et modifier les propriétés d’une publication dans SQL Server à l’aide de SQL Server Management Studio, Transact-SQL ou Replication Management Objects.
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- viewing replication properties
- modifying replication properties, articles
- articles [SQL Server replication], modifying
- publications [SQL Server replication], properties
- articles [SQL Server replication], properties
- modifying replication properties, publications
- publications [SQL Server replication], modifying
ms.assetid: 27d72ea4-bcb6-48f2-b4aa-eb1410da7efc
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: dfb58f47996df608218e867fda4225f8cb36a7e6
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/25/2021
ms.locfileid: "98766468"
---
# <a name="view-and-modify-publication-properties"></a>Afficher et modifier les propriétés d'une publication
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]
  Cette rubrique décrit comment afficher et modifier les propriétés de la publication dans [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)], de [!INCLUDE[tsql](../../../includes/tsql-md.md)]ou d'objets RMO (Replication Management Objects).  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Limitations et restrictions](#Restrictions)  
  
     [Recommandations](#Recommendations)  
  
-   **Pour afficher et modifier les propriétés d'une publication à l'aide de :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
     [Objets RMO (Replication Management Objects)](#RMOProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitations et restrictions  
  
-   Certaines propriétés ne sont pas modifiables après la création d'une publication et d'autres s'il existe des abonnements à la publication. Les propriétés non modifiables sont affichées en lecture seule.  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> Recommandations  
  
-   Après qu'une publication a été créée, certaines modifications de propriétés nécessitent un nouvel instantané. Si une publication dispose d'abonnements, ces abonnements pourraient devoir alors être réinitialisés selon les modifications que vous avez apportées. Pour plus d’informations, consultez [Modifier les propriétés des publications et des articles](../../../relational-databases/replication/publish/change-publication-and-article-properties.md) et [Ajouter et supprimer des articles de publications existantes](../../../relational-databases/replication/publish/add-articles-to-and-drop-articles-from-existing-publications.md).  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
 Affichez et modifiez les propriétés de la publication dans la boîte de dialogue **Propriétés de la publication - \<Publication>** , qui est disponible dans [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] et dans le Moniteur de réplication. Pour plus d’informations sur le démarrage du Moniteur de réplication, consultez [Démarrer le Moniteur de réplication](../../../relational-databases/replication/monitor/start-the-replication-monitor.md).  
  
 La boîte de dialogue **Propriétés de la publication - \<Publication>** comprend les pages suivantes :  
  
-   La page **Général** contient le nom et la description de la publication, le nom de la base de données, le type de publication et les paramètres d'expiration des abonnements.  
  
-   La page **Articles** correspond à la page **Articles** de l'Assistant Nouvelle publication. Utilisez-la pour ajouter et supprimer des articles, ainsi que pour modifier des propriétés et le filtrage des colonnes pour des articles.  
  
-   La page **Filtrer les lignes** correspond à la page **Filtrer les lignes de la table** de l'Assistant Nouvelle publication. Utilisez-la pour ajouter, modifier et supprimer des filtres de lignes statiques pour tous les types de publications, ainsi que pour ajouter, modifier et supprimer des filtres de lignes paramétrés et des filtres de jointure pour des publications de fusion.  
  
-   La page **Instantané** permet de spécifier le format et l'emplacement de l'instantané, la compression éventuelle de l'instantané, ainsi que des scripts à exécuter avant et après l'application de l'instantané.  
  
-   La page **Instantané FTP** (pour les publications d'instantané et transactionnelles, et les publications de fusion pour les serveurs de publication exécutant des versions antérieures à SQL Server 2005) permet de spécifier si des Abonnés peuvent télécharger des fichiers d'instantanés via le protocole FTP.  
  
-   La page **Instantané FTP et Internet** (pour les publications de fusion provenant des serveurs de publication exécutant SQL Server 2005 ou version ultérieure) permet de spécifier si des Abonnés peuvent télécharger des fichiers d'instantanés via FTP et si des Abonnés peuvent synchroniser des abonnements via HTTPS.  
  
-   La page **Options d'abonnement** permet de définir diverses options qui s'appliquent à tous les abonnements. Les options diffèrent en fonction du type de publication.  
  
-   La page **Liste d'accès à la publication** permet de spécifier les connexions et les groupes qui peuvent accéder à une publication.  
  
-   La page **Sécurité de l'agent** permet d'accéder aux paramètres des comptes sous lesquels les agents ci-après s'exécutent et se connectent aux ordinateurs d'une topologie de réplication : l'Agent d'instantané pour toutes les publications, l'Agent de lecture du journal pour toutes les publications transactionnelles et l'Agent de lecture de la file d'attente pour les publications transactionnelles qui autorisent les abonnements mis à jour en attente.  
  
-   La page **Partitions de données** (pour les publications de fusion provenant des serveurs de publication exécutant SQL Server 2005 ou version ultérieure) permet de spécifier si des Abonnés à des publications avec des filtres paramétrés peuvent demander un instantané si aucun n'est disponible. Elle permet de générer des instantanés pour une ou plusieurs partitions, soit au coup par coup, soit à intervalles récurrents.  
  
#### <a name="to-view-and-modify-publication-properties-in-management-studio"></a>Pour afficher et modifier les propriétés d'une publication dans Management Studio  
  
1.  Connectez-vous au serveur de publication dans [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)], puis développez le nœud du serveur.  
  
2.  Développez le dossier **Réplication** , puis développez le dossier **Publications locales** .  
  
3.  Cliquez avec le bouton droit sur une publication, puis cliquez sur **Propriétés**.  
  
4.  Modifiez les propriétés si nécessaire, puis cliquez sur **OK**.  

#### <a name="to-view-and-modify-publication-properties-in-replication-monitor"></a>Pour afficher et modifier les propriétés d'une publication dans le moniteur de réplication  
  
1.  Développez un groupe du serveur de publication dans le volet gauche du moniteur de réplication, puis développez un serveur de publication.  
  
2.  Cliquez avec le bouton droit sur une publication, puis cliquez sur **Propriétés**.  
  
3.  Modifiez les propriétés si nécessaire, puis cliquez sur **OK**.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
 Les publications peuvent être modifiées et leurs propriétés retournées par programme à l'aide de procédures stockées de réplication. Les procédures stockées que vous utilisez dépendent du type de publication.  
  
#### <a name="to-view-the-properties-of-a-snapshot-or-transactional-publication"></a>Pour afficher les propriétés d'une publication transactionnelle ou d'instantané  
  
1.  Exécutez [sp_helppublication](../../../relational-databases/system-stored-procedures/sp-helppublication-transact-sql.md), en spécifiant le nom de la publication pour le paramètre **\@publication**. Si vous ne spécifiez pas ce paramètre, les informations sur toutes les publications sur le serveur de publication sont retournées.  
  
#### <a name="to-change-the-properties-of-a-snapshot-or-transactional-publication"></a>Pour modifier les propriétés d'une publication transactionnelle ou d'instantané  
  
1.  Exécutez [sp_changepublication](../../../relational-databases/system-stored-procedures/sp-changepublication-transact-sql.md), en spécifiant la propriété de publication à modifier dans le paramètre **\@property** et la nouvelle valeur de cette propriété dans le paramètre **\@value**.  
  
    > [!NOTE]  
    >  Si la modification nécessite la génération d’un nouvel instantané, vous devez également spécifier la valeur **1** pour **\@force_invalidate_snapshot** et si la modification nécessite la réinitialisation des Abonnés, vous devez spécifier la valeur **1** pour **\@force_reinit_subscription**. Pour plus d’informations sur les propriétés qui, une fois modifiées, nécessitent un nouvel instantané ou une réinitialisation, consultez [Modifier les propriétés des publications et des articles](../../../relational-databases/replication/publish/change-publication-and-article-properties.md).  
  
#### <a name="to-view-the-properties-of-a-merge-publication"></a>Pour afficher les propriétés d'une publication de fusion  
  
1.  Exécutez [sp_helpmergepublication](../../../relational-databases/system-stored-procedures/sp-helpmergepublication-transact-sql.md), en spécifiant le nom de la publication pour le paramètre **\@publication**. Si vous ne spécifiez pas ce paramètre, les informations sur toutes les publications sur le serveur de publication sont retournées.  
  
#### <a name="to-change-the-properties-of-a-merge-publication"></a>Pour modifier les propriétés d'une publication de fusion  
  
1.  Exécutez [sp_changemergepublication](../../../relational-databases/system-stored-procedures/sp-changemergepublication-transact-sql.md), en spécifiant la propriété de publication à modifier dans le paramètre **\@property** et la nouvelle valeur de cette propriété dans le paramètre **\@value**.  
  
    > [!NOTE]  
    >  Si la modification nécessite la génération d’un nouvel instantané, vous devez également spécifier la valeur **1** pour **\@force_invalidate_snapshot** et si la modification nécessite la réinitialisation des Abonnés, vous devez spécifier la valeur **1** pour **\@force_reinit_subscription**. Pour plus d’informations sur les propriétés qui, une fois modifiées, nécessitent un nouvel instantané ou une réinitialisation, consultez [Modifier les propriétés des publications et des articles](../../../relational-databases/replication/publish/change-publication-and-article-properties.md).  
  
#### <a name="to-view-the-properties-of-a-snapshot"></a>Pour afficher les propriétés d'un instantané  
  
1.  Exécutez [sp_helppublication_snapshot](../../../relational-databases/system-stored-procedures/sp-helppublication-snapshot-transact-sql.md), en spécifiant le nom de la publication pour le paramètre **\@publication**.  
  
#### <a name="to-change-the-properties-of-a-snapshot"></a>Pour modifier les propriétés d'un instantané  
  
1.  Exécutez [sp_changepublication_snapshot](../../../relational-databases/system-stored-procedures/sp-changepublication-snapshot-transact-sql.md), en spécifiant une ou plusieurs des nouvelles propriétés d'instantané pour les paramètres d'instantané appropriés.  
  
###  <a name="examples-transact-sql"></a><a name="TsqlExample"></a> Exemples (Transact-SQL)  
 Cet exemple de réplication transactionnelle retourne les propriétés de la publication.  
  
 [!code-sql[HowTo#sp_helppublication](../../../relational-databases/replication/codesnippet/tsql/view-and-modify-publicat_1.sql)]  
  
 Cet exemple de réplication transactionnelle désactive la réplication du schéma pour la publication.  
  
 [!code-sql[HowTo#sp_changepublication](../../../relational-databases/replication/codesnippet/tsql/view-and-modify-publicat_2.sql)]  
  
 Cet exemple de réplication de fusion retourne les propriétés de la publication.  
  
 [!code-sql[HowTo#sp_helpmergepublication](../../../relational-databases/replication/codesnippet/tsql/view-and-modify-publicat_3.sql)]  
  
 Cet exemple de réplication de fusion désactive la réplication du schéma pour la publication.  
  
 [!code-sql[HowTo#sp_changemergepublication](../../../relational-databases/replication/codesnippet/tsql/view-and-modify-publicat_4.sql)]  
  
##  <a name="using-replication-management-objects-rmo"></a><a name="RMOProcedure"></a> Utilisation d'objets RMO (Replication Management Objects)  
 Vous pouvez modifier des publications et accéder par programme à leurs propriétés à l'aide d'objets RMO (Replication Management Objects). Les classes RMO utilisées pour afficher ou modifier les propriétés d'une publication dépendent du type de publication.  
  
#### <a name="to-view-or-modify-properties-of-a-snapshot-or-transactional-publication"></a>Pour afficher ou modifier les propriétés d'une publication transactionnelle ou d'instantané  
  
1.  Créez une connexion au serveur de publication en utilisant la classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Créez une instance de la classe <xref:Microsoft.SqlServer.Replication.TransPublication> , définissez les propriétés <xref:Microsoft.SqlServer.Replication.Publication.Name%2A> et <xref:Microsoft.SqlServer.Replication.Publication.DatabaseName%2A> pour la publication et définissez la propriété <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> sur la connexion créée à l'étape 1.  
  
3.  Appelez la méthode <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> pour obtenir les propriétés de l'objet. Si cette méthode retourne **false**, soit les propriétés de la publication ont été définies de manière incorrecte à l'étape 2, soit la publication n'existe pas.  
  
4.  (Facultatif) Pour modifier les propriétés, définissez une nouvelle valeur pour l'une des propriétés paramétrables. Utilisez l'opérateur AND logique ( **&** en Microsoft Visual C# et **And** en Microsoft Visual Basic) pour déterminer si une valeur <xref:Microsoft.SqlServer.Replication.PublicationAttributes> donnée est définie pour la propriété <xref:Microsoft.SqlServer.Replication.Publication.Attributes%2A> . Utilisez l'opérateur OR logique inclusif ( **|** en Visual C# et **Or** en Visual Basic) et l'opérateur OR logique exclusif ( **^** en Visual C# et **Xor** en Visual Basic) pour modifier les valeurs <xref:Microsoft.SqlServer.Replication.PublicationAttributes> de la propriété <xref:Microsoft.SqlServer.Replication.Publication.Attributes%2A> .  
  
5.  (Facultatif) Si vous avez spécifié la valeur **true** pour <xref:Microsoft.SqlServer.Replication.ReplicationObject.CachePropertyChanges%2A>, appelez la méthode <xref:Microsoft.SqlServer.Replication.ReplicationObject.CommitPropertyChanges%2A> pour valider les modifications sur le serveur. Si vous avez spécifié la valeur **false** pour <xref:Microsoft.SqlServer.Replication.ReplicationObject.CachePropertyChanges%2A> (valeur par défaut), les modifications sont envoyées immédiatement au serveur.  
  
#### <a name="to-view-or-modify-properties-of-a-merge-publication"></a>Pour afficher ou modifier les propriétés d'une publication de fusion  
  
1.  Créez une connexion au serveur de publication en utilisant la classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Créez une instance de la classe <xref:Microsoft.SqlServer.Replication.MergePublication> , définissez les propriétés <xref:Microsoft.SqlServer.Replication.Publication.Name%2A> et <xref:Microsoft.SqlServer.Replication.Publication.DatabaseName%2A> pour la publication et définissez la propriété <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> sur la connexion créée à l'étape 1.  
  
3.  Appelez la méthode <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> pour obtenir les propriétés de l'objet. Si cette méthode retourne **false**, soit les propriétés de la publication ont été définies de manière incorrecte à l'étape 2, soit la publication n'existe pas.  
  
4.  (Facultatif) Pour modifier les propriétés, définissez une nouvelle valeur pour l'une des propriétés paramétrables. Utilisez l'opérateur AND logique ( **&** en Visual C# et **And** en Visual Basic) pour déterminer si une valeur <xref:Microsoft.SqlServer.Replication.PublicationAttributes> donnée est définie pour la propriété <xref:Microsoft.SqlServer.Replication.Publication.Attributes%2A> . Utilisez l'opérateur OR logique inclusif ( **|** en Visual C# et **Or** en Visual Basic) et l'opérateur OR logique exclusif ( **^** en Visual C# et **Xor** en Visual Basic) pour modifier les valeurs <xref:Microsoft.SqlServer.Replication.PublicationAttributes> de la propriété <xref:Microsoft.SqlServer.Replication.Publication.Attributes%2A> .  
  
5.  (Facultatif) Si vous avez spécifié la valeur **true** pour <xref:Microsoft.SqlServer.Replication.ReplicationObject.CachePropertyChanges%2A>, appelez la méthode <xref:Microsoft.SqlServer.Replication.ReplicationObject.CommitPropertyChanges%2A> pour valider les modifications sur le serveur. Si vous avez spécifié la valeur **false** pour <xref:Microsoft.SqlServer.Replication.ReplicationObject.CachePropertyChanges%2A> (valeur par défaut), les modifications sont envoyées immédiatement au serveur.  
  
###  <a name="examples-rmo"></a><a name="PShellExample"></a> Exemples (RMO)  
 Cet exemple définit des attributs de publication pour une publication transactionnelle. Les modifications sont mises en cache jusqu'à leur envoi explicite au serveur.  
  
 [!code-cs[HowTo#rmo_ChangeTranPub_cached](../../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_changetranpub_cached)]  
  
 [!code-vb[HowTo#rmo_vb_ChangeTranPub_cached](../../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_changetranpub_cached)]  
  
 Cet exemple désactive la réplication DDL pour une publication de fusion.  
  
 [!code-cs[HowTo#rmo_ChangeMergePub_ddl](../../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_changemergepub_ddl)]  
  
 [!code-vb[HowTo#rmo_vb_ChangeMergePub_ddl](../../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_changemergepub_ddl)]  
  
## <a name="see-also"></a>Voir aussi  
 [Publier des données et des objets de base de données](../../../relational-databases/replication/publish/publish-data-and-database-objects.md)   
 [Changer les propriétés des publications et des articles](../../../relational-databases/replication/publish/change-publication-and-article-properties.md)   
 [Modifier le schéma dans les bases de données de publication](../../../relational-databases/replication/publish/make-schema-changes-on-publication-databases.md)   
 [Concepts liés aux procédures stockées système de réplication](../../../relational-databases/replication/concepts/replication-system-stored-procedures-concepts.md)   
 [Ajouter et supprimer des articles dans une publication &#40;SQL Server Management Studio&#41;](../../../relational-databases/replication/publish/add-articles-to-and-drop-articles-from-a-publication.md)   
 [Afficher des informations et effectuer des tâches à l’aide du moniteur de réplication](../../../relational-databases/replication/monitor/view-information-and-perform-tasks-replication-monitor.md)   
 [Afficher et modifier les propriétés d’un article](../../../relational-databases/replication/publish/view-and-modify-article-properties.md)  
  
  
