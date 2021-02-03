---
description: Afficher et modifier les propriétés d'un abonnement par émission (push)
title: Afficher et modifier les propriétés d’un abonnement par émission de données | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- viewing replication properties
- push subscriptions [SQL Server replication], properties
- subscriptions [SQL Server replication], push
- push subscriptions [SQL Server replication], modifying
- modifying replication properties, push subscriptions
- modifying subscriptions, SQL Server Management Studio
ms.assetid: 801d2995-7aa5-4626-906e-c8190758ec71
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-current||>=sql-server-2016
ms.openlocfilehash: 28abdcd83484be06b135252da8bc9af53b98448b
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250533"
---
# <a name="view-and-modify-push-subscription-properties"></a>Afficher et modifier les propriétés d'un abonnement par émission (push)
[!INCLUDE[sql-asdb](../../includes/applies-to-version/sql-asdb.md)]
  Cette rubrique décrit comment afficher et modifier les propriétés de l'abonnement par émission (push) dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], de [!INCLUDE[tsql](../../includes/tsql-md.md)]ou d'objets RMO (Replication Management Objects).  

[!INCLUDE[azure-sql-db-replication-supportability-note](../../includes/azure-sql-db-replication-supportability-note.md)]

  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
 Affichez et modifiez les propriétés d'abonnement par envoi de données (push) du serveur de publication dans :  
  
-   La boîte de dialogue **Propriétés de l'abonnement - \<Publisher>: \<PublicationDatabase>** disponible dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
-   L'onglet **Tous les abonnements** , disponible dans le Moniteur de réplication. Pour plus d’informations sur le démarrage du Moniteur de réplication, consultez [Démarrer le Moniteur de réplication](../../relational-databases/replication/monitor/start-the-replication-monitor.md).  
  
#### <a name="to-view-and-modify-push-subscription-properties-in-management-studio"></a>Pour afficher et modifier les propriétés d'abonnement par envoi de données (push) dans Management Studio  
  
1.  Connectez-vous au serveur de publication dans [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)], puis développez le nœud du serveur.  
  
2.  Développez le dossier **Réplication** , puis développez le dossier **Publications locales** .  
  
3.  Développez la publication appropriée, cliquez avec le bouton droit sur un abonnement puis cliquez sur **Propriétés**.  
  
4.  Modifiez les propriétés si nécessaire, puis cliquez sur **OK**.  
  
#### <a name="to-view-and-modify-push-subscription-properties-in-replication-monitor"></a>Pour afficher et modifier les propriétés d'abonnement par envoi de données (push) dans le Moniteur de réplication  
  
1.  Développez un groupe Serveur de publication dans le volet gauche du moniteur de réplication, développez un serveur de publication puis cliquez sur une publication.  
  
2.  Cliquez sur l'onglet **Tous les abonnements** .  
  
3.  Cliquez avec le bouton droit sur un abonnement, puis cliquez sur **Propriétés**.  
  
4.  Modifiez les propriétés si nécessaire, puis cliquez sur **OK**.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
 Il est possible de modifier des abonnements par émission de données et d'accéder à leurs propriétés, par programme, à l'aide des procédures stockées de réplication. Les procédures stockées utilisées dépendent du type de publication auquel l'abonnement appartient.  
  
#### <a name="to-view-the-properties-of-a-push-subscription-to-a-snapshot-or-transactional-publication"></a>Pour afficher les propriétés d'un abonnement par émission de données à une publication transactionnelle ou d'instantané  
  
1.  Exécutez [sp_helpsubscription](../../relational-databases/system-stored-procedures/sp-helpsubscription-transact-sql.md)sur la base de données de publication du serveur de publication. Spécifiez **\@publication**, **\@subscriber** et une valeur **all** pour **\@article**.  
  
2.  Exécutez [sp_helpsubscriberinfo](../../relational-databases/system-stored-procedures/sp-helpsubscriberinfo-transact-sql.md) en spécifiant **\@subscriber** sur la base de données de publication du serveur de publication.  
  
#### <a name="to-change-the-properties-of-a-push-subscription-to-a-snapshot-or-transactional-publication"></a>Pour modifier les propriétés d'un abonnement par émission de données à une publication transactionnelle ou d'instantané  
  
1.  Exécutez [sp_changesubscriber](../../relational-databases/system-stored-procedures/sp-changesubscriber-transact-sql.md)sur la base de données de publication du serveur de publication, en spécifiant **\@subscriber** et tous les paramètres des propriétés d’abonné en cours de modification.  
  
2.  Exécutez [sp_changesubscription](../../relational-databases/system-stored-procedures/sp-changesubscription-transact-sql.md)sur la base de données de publication du serveur de publication. Spécifiez **\@publication**, **\@subscriber**, **\@destination_db**, la valeur **all** pour **\@article**, la propriété d’abonnement modifiée pour **\@property** et la nouvelle valeur pour **\@value**. Cela modifie les paramètres de sécurité de l'abonnement par émission de données.  
  
3.  (Facultatif) Pour modifier les propriétés des packages DTS (Data Transformation Services) d'un abonnement, exécutez [sp_changesubscriptiondtsinfo](../../relational-databases/system-stored-procedures/sp-changesubscriptiondtsinfo-transact-sql.md) sur la base de données d'abonnement de l'Abonné. Spécifiez l’ID du travail de l’Agent de distribution pour **\@jobid** et les propriétés du package DTS suivantes :  
  
    -   **\@dts_package_name**  
  
    -   **\@dts_package_password**  
  
    -   **\@dts_package_location**  
  
     Cela modifie les propriétés de package DTS d'un abonnement.  
  
    > [!NOTE]  
    >  L'ID de travail peut être obtenu en exécutant [sp_helpsubscription](../../relational-databases/system-stored-procedures/sp-helpsubscription-transact-sql.md).  
  
#### <a name="to-view-the-properties-of-a-push-subscription-to-a-merge-publication"></a>Pour afficher les propriétés d'un abonnement par émission de données à une publication de fusion  
  
1.  Exécutez [sp_helpmergesubscription](../../relational-databases/system-stored-procedures/sp-helpmergesubscription-transact-sql.md)sur la base de données de publication du serveur de publication. Spécifiez **\@publication** et **\@subscriber**.  
  
2.  Sur le serveur de publication, exécutez [sp_helpsubscriberinfo](../../relational-databases/system-stored-procedures/sp-helpsubscriberinfo-transact-sql.md), en spécifiant **\@subscriber**.  
  
#### <a name="to-change-the-properties-of-a-push-subscription-to-a-merge-publication"></a>Pour modifier les propriétés d'un abonnement par émission de données à une publication de fusion  
  
1.  Exécutez [sp_changemergesubscription](../../relational-databases/system-stored-procedures/sp-changemergesubscription-transact-sql.md)sur la base de données de publication du serveur de publication. Spécifiez **\@publication**, **\@subscriber**, **\@subscriber_db**, la propriété de l’abonnement modifiée pour **\@property** et la nouvelle valeur pour **\@value**.  
  
###  <a name="example-transact-sql"></a><a name="TsqlExample"></a> Exemple (Transact-SQL)  
  
##  <a name="using-replication-management-objects-rmo"></a><a name="RMOProcedure"></a> Utilisation d'objets RMO (Replication Management Objects)  
 Les classes RMO à utiliser pour afficher ou modifier les propriétés d'un abonnement par émission de données dépendent du type de publication auquel l'abonnement par émission de données a été souscrit.  
  
#### <a name="to-view-or-modify-properties-of-a-push-subscription-to-a-snapshot-or-transactional-publication"></a>Pour afficher ou modifier les propriétés d'un abonnement par émission de données à une publication transactionnelle ou d'instantané  
  
1.  Créez une connexion au serveur de publication en utilisant la classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Créez une instance de la classe <xref:Microsoft.SqlServer.Replication.TransSubscription>.  
  
3.  Définissez les propriétés <xref:Microsoft.SqlServer.Replication.Subscription.PublicationName%2A>, <xref:Microsoft.SqlServer.Replication.Subscription.DatabaseName%2A>, <xref:Microsoft.SqlServer.Replication.Subscription.SubscriberName%2A>et <xref:Microsoft.SqlServer.Replication.Subscription.SubscriptionDBName%2A> .  
  
4.  Définissez la classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> créée à l'étape 1 pour le paramètre de propriété <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> .  
  
5.  Appelez la méthode <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> pour obtenir les propriétés de l'objet. Si cette méthode retourne **false**, soit les propriétés de l'abonnement ont été définies de manière incorrecte à l'étape 3, soit l'abonnement n'existe pas.  
  
6.  (Facultatif) Pour modifier des propriétés, modifiez la valeur d'une des propriétés <xref:Microsoft.SqlServer.Replication.TransSubscription> qui peuvent être définies, puis appelez la méthode <xref:Microsoft.SqlServer.Replication.ReplicationObject.CommitPropertyChanges%2A> .  
  
7.  (Facultatif) Pour afficher les nouveaux paramètres, appelez la méthode <xref:Microsoft.SqlServer.Replication.ReplicationObject.Refresh%2A> pour recharger les propriétés pour l'abonnement.  
  
#### <a name="to-view-or-modify-properties-of-a-push-subscription-to-a-merge-publication"></a>Pour afficher ou modifier les propriétés d'un abonnement par émission de données à une publication de fusion  
  
1.  Créez une connexion à l'Abonné en utilisant la classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Créez une instance de la classe <xref:Microsoft.SqlServer.Replication.MergeSubscription>.  
  
3.  Définissez les propriétés <xref:Microsoft.SqlServer.Replication.Subscription.PublicationName%2A>, <xref:Microsoft.SqlServer.Replication.Subscription.DatabaseName%2A>, <xref:Microsoft.SqlServer.Replication.Subscription.SubscriberName%2A>et <xref:Microsoft.SqlServer.Replication.Subscription.SubscriptionDBName%2A> .  
  
4.  Définissez la classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> créée à l'étape 1 pour le paramètre de propriété <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> .  
  
5.  Appelez la méthode <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> pour obtenir les propriétés de l'objet. Si cette méthode retourne **false**, soit les propriétés de l'abonnement ont été définies de manière incorrecte à l'étape 3, soit l'abonnement n'existe pas.  
  
6.  (Facultatif) Pour modifier des propriétés, modifiez la valeur d'une des propriétés <xref:Microsoft.SqlServer.Replication.MergeSubscription> qui peuvent être définies, puis appelez la méthode <xref:Microsoft.SqlServer.Replication.ReplicationObject.CommitPropertyChanges%2A> .  
  
7.  (Facultatif) Pour afficher les nouveaux paramètres, appelez la méthode <xref:Microsoft.SqlServer.Replication.ReplicationObject.Refresh%2A> pour recharger les propriétés pour l'abonnement.  
  
## <a name="see-also"></a>Voir aussi  
 [Afficher des informations et effectuer des tâches à l’aide du moniteur de réplication](../../relational-databases/replication/monitor/view-information-and-perform-tasks-replication-monitor.md)   
 [Replication Security Best Practices](../../relational-databases/replication/security/replication-security-best-practices.md)   
 [S'abonner à des publications](../../relational-databases/replication/subscribe-to-publications.md)  
  
  
