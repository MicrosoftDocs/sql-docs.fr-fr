---
description: Afficher et modifier les propriétés d'un abonnement par extraction (pull)
title: Afficher et modifier les propriétés d’un abonnement par extraction | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- modifying subscriptions
- viewing replication properties
- modifying replication properties, pull subscriptions
- pull subscriptions [SQL Server replication], modifying
- subscriptions [SQL Server replication], pull
- pull subscriptions [SQL Server replication], properties
- modifying subscriptions, SQL Server Management Studio
ms.assetid: 1601e54f-86f0-49e8-b023-87a5d1def033
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 91e312326e1d45ad4c5d747133baaa7c2c25e5d5
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250536"
---
# <a name="view-and-modify-pull-subscription-properties"></a>Afficher et modifier les propriétés d'un abonnement par extraction (pull)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
  Cette rubrique décrit comment afficher et modifier les propriétés de l'abonnement par extraction (pull) dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], de [!INCLUDE[tsql](../../includes/tsql-md.md)]ou d'objets RMO (Replication Management Objects).  
  
 **Dans cette rubrique**  
  
-   **Pour afficher et modifier les propriétés d'un abonnement par extraction à l'aide de :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
     [Objets RMO (Replication Management Objects)](#RMOProcedure)  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
 Affichez les propriétés de l'abonnement par extraction à partir du serveur de Publication ou de l'Abonné dans la boîte de dialogue **Propriétés de l'abonnement - \<Publisher> : \<PublicationDatabase>** , disponible dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Vous pouvez modifier des propriétés ou en afficher d'autres sur l'Abonné. Vous pouvez également afficher des propriétés à partir du serveur de publication sous l'onglet **Tous les abonnements** , disponible dans le moniteur de réplication. Pour plus d’informations sur le démarrage du Moniteur de réplication, consultez [Démarrer le Moniteur de réplication](../../relational-databases/replication/monitor/start-the-replication-monitor.md).  
  
#### <a name="to-view-pull-subscription-properties-from-the-publisher-in-management-studio"></a>Pour afficher des propriétés d'extraction d'abonnement à partir du serveur de publication dans Management Studio  
  
1.  Connectez-vous au serveur de publication dans [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)], puis développez le nœud du serveur.  
  
2.  Développez le dossier **Réplication** , puis développez le dossier **Publications locales** .  
  
3.  Développez la publication appropriée, cliquez avec le bouton droit sur un abonnement puis cliquez sur **Propriétés**.  
  
4.  Affichez les propriétés, puis cliquez sur **OK**.  

#### <a name="to-view-and-modify-pull-subscription-properties-from-the-subscriber-in-management-studio"></a>Pour afficher et modifier des propriétés d'extraction d'abonnement à partir de l'Abonné dans Management Studio  
  
1.  Connectez-vous à l'Abonné dans [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)], puis développez le nœud du serveur.  
  
2.  Développez le dossier **Réplication** , puis développez le dossier **Abonnements locaux** .  
  
3.  Cliquez avec le bouton droit sur un abonnement, puis cliquez sur **Propriétés**.  
  
4.  Modifiez les propriétés si nécessaire, puis cliquez sur **OK**.  
  
#### <a name="to-view-pull-subscription-properties-from-the-publisher-in-replication-monitor"></a>Pour afficher des propriétés d'extraction d'abonnement à partir du serveur de publication dans le moniteur de réplication  
  
1.  Développez un groupe Serveur de publication dans le volet gauche du moniteur de réplication, développez un serveur de publication puis cliquez sur une publication.  
  
2.  Cliquez sur l'onglet **Tous les abonnements** .  
  
3.  Cliquez avec le bouton droit sur un abonnement, puis cliquez sur **Propriétés**.  
  
4.  Affichez les propriétés, puis cliquez sur **OK**.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
 Il est possible de modifier des abonnements par extraction et d'accéder par programme à leurs propriétés en utilisant des procédures stockées de réplication. Les procédures stockées utilisées dépendent du type de publication auquel l'abonnement appartient.  
  
#### <a name="to-view-the-properties-of-a-pull-subscription-to-a-snapshot-or-transactional-publication"></a>Pour afficher les propriétés d'un abonnement par extraction à une publication transactionnelle ou d'instantané  
  
1.  Sur l'Abonné, exécutez [sp_helppullsubscription](../../relational-databases/system-stored-procedures/sp-helppullsubscription-transact-sql.md). Spécifiez `@publisher`, `@publisher_db`et `@publication`. Des informations relatives à l'abonnement qui est stocké dans les tables système de l'Abonné sont alors renvoyées.  
  
2.  Sur l'Abonné, exécutez [sp_helpsubscription_properties](../../relational-databases/system-stored-procedures/sp-helpsubscription-properties-transact-sql.md). Spécifiez `@publisher`, `@publisher_db`, `@publication`, et l’une des valeurs suivantes pour `@publication_type`:  
  
    -   **0** - l'abonnement appartient à une publication transactionnelle.  
  
    -   **1** - l'abonnement appartient à une publication d'instantané.  
  
3.  Sur le serveur de publication, exécutez [sp_helpsubscription](../../relational-databases/system-stored-procedures/sp-helpsubscription-transact-sql.md). Spécifiez `@publication` et `@subscriber`.  
  
4.  Sur le serveur de publication, exécutez [sp_helpsubscriberinfo](../../relational-databases/system-stored-procedures/sp-helpsubscriberinfo-transact-sql.md), en spécifiant `@subscriber`. Des informations relatives à l'Abonné sont alors affichées.  
  
#### <a name="to-change-the-properties-of-a-pull-subscription-to-a-snapshot-or-transactional-publication"></a>Pour modifier les propriétés d'un abonnement par extraction à une publication transactionnelle ou d'instantané  
  
1.  Sur l’Abonné, exécutez [sp_change_subscription_properties](../../relational-databases/system-stored-procedures/sp-change-subscription-properties-transact-sql.md), en spécifiant `@publisher`, `@publisher_db`, `@publication`, la valeur **0** (transactionnel) ou **1** (instantané) pour `@publication_type`, la nouvelle propriété d’abonnement `@property` et la nouvelle valeur `@value`.  
  
2.  (Facultatif) Dans la base de données d'abonnement de l'Abonné, exécutez [sp_changesubscriptiondtsinfo](../../relational-databases/system-stored-procedures/sp-changesubscriptiondtsinfo-transact-sql.md). Spécifiez l’ID du travail de l’Agent de distribution pour `@jobid`et les propriétés de package DTS (Data Transformation Services) suivantes :  
  
    -   `@dts_package_name`  
  
    -   `dts_package_password`  
  
    -   `@dts_package_location`  
  
     Cela modifie les propriétés de package DTS d'un abonnement.  
  
    > [!NOTE]  
    >  L'ID de travail peut être obtenu en exécutant [sp_helpsubscription](../../relational-databases/system-stored-procedures/sp-helpsubscription-transact-sql.md).  
  
#### <a name="to-view-the-properties-of-a-pull-subscription-to-a-merge-publication"></a>Pour afficher les propriétés d'un abonnement par extraction à une publication de fusion  
  
1.  Sur l'Abonné, exécutez [sp_helpmergepullsubscription](../../relational-databases/system-stored-procedures/sp-helpmergepullsubscription-transact-sql.md). Spécifiez `@publisher`, `@publisher_db`et `@publication`.  
  
2.  Sur l'Abonné, exécutez [sp_helpsubscription_properties](../../relational-databases/system-stored-procedures/sp-helpsubscription-properties-transact-sql.md). Spécifiez `@publisher`, `@publisher_db`, `@publication`, et la valeur 2 pour `@publication_type`.  
  
3.  Sur le serveur de publication, exécutez [sp_helpmergesubscription](../../relational-databases/system-stored-procedures/sp-helpmergesubscription-transact-sql.md) pour afficher les informations d'abonnement. Pour retourner des informations sur un abonnement spécifique, spécifiez `@publication`, `@subscriber`, et la valeur **pull** pour @subscription_type.  
  
4.  Sur le serveur de publication, exécutez [sp_helpsubscriberinfo](../../relational-databases/system-stored-procedures/sp-helpsubscriberinfo-transact-sql.md), en spécifiant `@subscriber`. Des informations relatives à l'Abonné sont alors affichées.  
  
#### <a name="to-change-the-properties-of-a-pull-subscription-to-a-merge-publication"></a>Pour modifier les propriétés d'un abonnement par extraction à une publication de fusion  
  
1.  Sur l'Abonné, exécutez [sp_changemergepullsubscription](../../relational-databases/system-stored-procedures/sp-changemergepullsubscription-transact-sql.md). Spécifiez `@publication`, `@publisher`, `@publisher_db`, la nouvelle propriété d’abonnement `@property` et la nouvelle valeur `@value`.  
  
##  <a name="using-replication-management-objects-rmo"></a><a name="RMOProcedure"></a> Utilisation d'objets RMO (Replication Management Objects)  
 Les classes RMO à utiliser pour afficher ou modifier les propriétés d'abonnements par extraction dépendent du type de publication auquel l'abonnement par extraction est souscrit.  
  
#### <a name="to-view-or-modify-properties-of-a-pull-subscription-to-a-snapshot-or-transactional-publication"></a>Pour afficher ou modifier les propriétés d'un abonnement par extraction à une publication transactionnelle ou d'instantané  
  
1.  Créez une connexion à l'Abonné en utilisant la classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Créez une instance de la classe <xref:Microsoft.SqlServer.Replication.TransPullSubscription>.  
  
3.  Définissez les propriétés <xref:Microsoft.SqlServer.Replication.PullSubscription.PublicationName%2A>, <xref:Microsoft.SqlServer.Replication.PullSubscription.DatabaseName%2A>, <xref:Microsoft.SqlServer.Replication.PullSubscription.PublisherName%2A>et <xref:Microsoft.SqlServer.Replication.PullSubscription.PublicationDBName%2A> .  
  
4.  Définissez la connexion créée à l'étape 1 pour la propriété <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> .  
  
5.  Appelez la méthode <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> pour obtenir les propriétés de l'objet. Si cette méthode retourne **false**, soit les propriétés de l'abonnement ont été définies de manière incorrecte à l'étape 3, soit l'abonnement n'existe pas sur le serveur.  
  
6.  (Facultatif) Pour modifier des propriétés, modifiez la valeur d'une des propriétés <xref:Microsoft.SqlServer.Replication.TransPullSubscription> qui peuvent être définies, puis appelez la méthode <xref:Microsoft.SqlServer.Replication.ReplicationObject.CommitPropertyChanges%2A> .  
  
7.  (Facultatif) Pour consulter les nouveaux paramètres, appelez la méthode <xref:Microsoft.SqlServer.Replication.ReplicationObject.Refresh%2A> pour recharger les propriétés pour l'article.  
  
8.  Fermez toutes les connexions.  
  
#### <a name="to-view-or-modify-properties-of-a-pull-subscription-to-a-merge-publication"></a>Pour afficher ou modifier les propriétés d'un abonnement par extraction à une publication de fusion  
  
1.  Créez une connexion à l'Abonné en utilisant la classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Créez une instance de la classe <xref:Microsoft.SqlServer.Replication.MergePullSubscription>.  
  
3.  Définissez les propriétés <xref:Microsoft.SqlServer.Replication.PullSubscription.PublicationName%2A>, <xref:Microsoft.SqlServer.Replication.PullSubscription.DatabaseName%2A>, <xref:Microsoft.SqlServer.Replication.PullSubscription.PublisherName%2A>et <xref:Microsoft.SqlServer.Replication.PullSubscription.PublicationDBName%2A> .  
  
4.  Définissez la connexion créée à l'étape 1 pour la propriété <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> .  
  
5.  Appelez la méthode <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> pour obtenir les propriétés de l'objet. Si cette méthode retourne **false**, soit les propriétés de l'abonnement ont été définies de manière incorrecte à l'étape 3, soit l'abonnement n'existe pas sur le serveur.  
  
6.  (Facultatif) Pour modifier des propriétés, modifiez la valeur d'une des propriétés <xref:Microsoft.SqlServer.Replication.MergePullSubscription> qui peuvent être définies, puis appelez la méthode <xref:Microsoft.SqlServer.Replication.ReplicationObject.CommitPropertyChanges%2A> .  
  
7.  (Facultatif) Pour consulter les nouveaux paramètres, appelez la méthode <xref:Microsoft.SqlServer.Replication.ReplicationObject.Refresh%2A> pour recharger les propriétés pour l'article.  
  
8.  Fermez toutes les connexions.  
  
## <a name="see-also"></a>Voir aussi  
 [Afficher des informations et effectuer des tâches à l’aide du moniteur de réplication](../../relational-databases/replication/monitor/view-information-and-perform-tasks-replication-monitor.md)   
 [Replication Security Best Practices](../../relational-databases/replication/security/replication-security-best-practices.md)   
 [S'abonner à des publications](../../relational-databases/replication/subscribe-to-publications.md)  
  
  
