---
title: Supprimer un abonnement par émission de données | Microsoft Docs
description: Découvrez comment supprimer un abonnement par émission de données dans SQL Server à l’aide de SQL Server Management Studio, Transact-SQL ou Replication Management Objects.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- removing subscriptions
- push subscriptions [SQL Server replication], deleting
- deleting subscriptions
- subscriptions [SQL Server replication], push
ms.assetid: 3c4847e2-aed9-4488-b45d-8164422bdb10
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-current||>=sql-server-2016
ms.openlocfilehash: 958919536049d4a2c023ee7b00a02f54daebed00
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076401"
---
# <a name="delete-a-push-subscription"></a>Supprimer un abonnement par émission (push)
[!INCLUDE[sql-asdb](../../includes/applies-to-version/sql-asdb.md)]
  Cette rubrique explique comment supprimer un abonnement par émission de données (push) dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], de [!INCLUDE[tsql](../../includes/tsql-md.md)]ou d'objets RMO (Replication Management Objects).  
  
 **Dans cette rubrique**  
  
-   **Pour supprimer un abonnement par émission de données (push) à l'aide de :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
     [Objets RMO (Replication Management Objects)](#RMOProcedure)  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
 Supprimez un abonnement par émission de données (push) sur le serveur de publication (à partir du dossier **Publications locales** dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]) ou sur l'Abonné (à partir du dossier **Publications locales** ). La suppression d'un abonnement ne supprime pas les objets ou les données de ce dernier : ils doivent être supprimés manuellement.  
  
#### <a name="to-delete-a-push-subscription-at-the-publisher"></a>Pour supprimer un abonnement par envoi de données au niveau du serveur de publication  
  
1.  Connectez-vous au serveur de publication dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], puis développez le nœud du serveur.  
  
2.  Développez le dossier **Réplication** , puis développez le dossier **Publications locales** .  
  
3.  Développez la publication associée à l'abonnement à supprimer.  
  
4.  Cliquez avec le bouton droit sur l'abonnement puis cliquez sur **Supprimer**.  
  
5.  Dans la boîte de dialogue de confirmation, indiquez si vous souhaitez vous connecter à l'Abonné pour supprimer les informations d'abonnement. Si vous désactivez la case à cocher **Se connecter à l'Abonné** , vous devrez vous connecter plus tard à l'Abonné pour supprimer les informations.  

#### <a name="to-delete-a-push-subscription-at-the-subscriber"></a>Pour supprimer un abonnement par envoi de données au niveau de l'Abonné  
  
1.  Connectez-vous à l'Abonné dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], puis développez le nœud du serveur.  
  
2.  Développez le dossier **Réplication** , puis développez le dossier **Abonnements locaux** .  
  
3.  Cliquez avec le bouton droit sur l'abonnement à supprimer, puis cliquez sur **Supprimer**.  
  
4.  Dans la boîte de dialogue de confirmation, indiquez si vous souhaitez vous connecter au serveur de publication pour supprimer les informations d'abonnement. Si vous désactivez la case à cocher **Se connecter au serveur de publication** , vous devez vous connecter ultérieurement au serveur de publication pour supprimer les informations.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
 Les abonnements par émission de données peuvent être supprimés par programme en utilisant des procédures stockées de réplication. Les procédures stockées utilisées dépendent du type de publication auquel l'abonnement appartient.  
  
#### <a name="to-delete-a-push-subscription-to-a-snapshot-or-transactional-publication"></a>Pour supprimer un abonnement par émission de données à une publication transactionnelle ou d'instantané  
  
1.  Dans la base de données de publication du serveur de publication, exécutez [sp_dropsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropsubscription-transact-sql.md). Spécifiez **\@publication** et **\@subscriber**. Affectez la valeur **all** à **\@article**. (Facultatif) Si le serveur de distribution n’est pas accessible, affectez la valeur **1** à **\@ignore_distributor** pour supprimer l’abonnement sans supprimer les objets connexes au niveau du serveur de distribution.  
  
2.  Dans la base de données d’abonnement de l’Abonné, exécutez [sp_subscription_cleanup &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-subscription-cleanup-transact-sql.md) pour supprimer les métadonnées de réplication dans la base de données d’abonnement.  
  
#### <a name="to-delete-a-push-subscription-to-a-merge-publication"></a>Pour supprimer un abonnement par émission de données à une publication de fusion  
  
1.  Sur le serveur de publication, exécutez [sp_dropmergesubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropmergesubscription-transact-sql.md), en spécifiant **\@publication**, **\@subscriber** et **\@subscriber_db**. (Facultatif) Si le serveur de distribution n’est pas accessible, affectez la valeur **1** à **\@ignore_distributor** pour supprimer l’abonnement sans supprimer les objets connexes au niveau du serveur de distribution.  
  
2.  Dans la base de données d’abonnement de l’Abonné, exécutez [sp_mergesubscription_cleanup &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-mergesubscription-cleanup-transact-sql.md). Spécifiez **\@publisher**, **\@publisher_db** et **\@publication**. Les métadonnées de fusion sont alors supprimées de la base de données d'abonnement.  
  
###  <a name="examples-transact-sql"></a><a name="TsqlExample"></a> Exemples (Transact-SQL)  
 Cet exemple supprime un abonnement par émission de données à une publication transactionnelle.  
  
 [!code-sql[HowTo#sp_droptransubscription](../../relational-databases/replication/codesnippet/tsql/delete-a-push-subscription_1.sql)]  
  
 Cet exemple supprime un abonnement par émission de données à une publication de fusion.  
  
 [!code-sql[HowTo#sp_dropmergesubscription](../../relational-databases/replication/codesnippet/tsql/delete-a-push-subscription_2.sql)]  
  
##  <a name="using-replication-management-objects-rmo"></a><a name="RMOProcedure"></a> Utilisation d'objets RMO (Replication Management Objects)  
 Les classes RMO à utiliser pour supprimer un abonnement par émission de données dépendent du type de publication auquel l'abonnement par émission de données est souscrit.  
  
#### <a name="to-delete-a-push-subscription-to-a-snapshot-or-transactional-publication"></a>Pour supprimer un abonnement par émission de données à une publication transactionnelle ou d'instantané  
  
1.  Créez une connexion à l'Abonné en utilisant la classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Créez une instance de la classe <xref:Microsoft.SqlServer.Replication.TransSubscription>.  
  
3.  Définissez les propriétés <xref:Microsoft.SqlServer.Replication.Subscription.PublicationName%2A>, <xref:Microsoft.SqlServer.Replication.Subscription.SubscriptionDBName%2A>, <xref:Microsoft.SqlServer.Replication.Subscription.SubscriberName%2A>et <xref:Microsoft.SqlServer.Replication.Subscription.DatabaseName%2A> .  
  
4.  Définissez la classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> créée à l'étape 1 pour la propriété <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> .  
  
5.  Vérifiez la propriété <xref:Microsoft.SqlServer.Replication.ReplicationObject.IsExistingObject%2A> pour vous assurer que l'abonnement existe. Si la valeur de cette propriété est **false**, les propriétés de l'abonnement ont été définies de manière incorrecte à l'étape 2, ou l'article n'existe pas.  
  
6.  Appelez la méthode <xref:Microsoft.SqlServer.Replication.Subscription.Remove%2A> .  
  
#### <a name="to-delete-a-push-subscription-to-a-merge-publication"></a>Pour supprimer un abonnement par émission de données à une publication de fusion  
  
1.  Créez une connexion à l'Abonné en utilisant la classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Créez une instance de la classe <xref:Microsoft.SqlServer.Replication.MergeSubscription>.  
  
3.  Définissez les propriétés <xref:Microsoft.SqlServer.Replication.Subscription.PublicationName%2A>, <xref:Microsoft.SqlServer.Replication.Subscription.SubscriptionDBName%2A>, <xref:Microsoft.SqlServer.Replication.Subscription.SubscriberName%2A>et <xref:Microsoft.SqlServer.Replication.Subscription.DatabaseName%2A> .  
  
4.  Définissez la classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> créée à l'étape 1 pour la propriété <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> .  
  
5.  Vérifiez la propriété <xref:Microsoft.SqlServer.Replication.ReplicationObject.IsExistingObject%2A> pour vous assurer que l'abonnement existe. Si la valeur de cette propriété est **false**, les propriétés de l'abonnement ont été définies de manière incorrecte à l'étape 2, ou l'article n'existe pas.  
  
6.  Appelez la méthode <xref:Microsoft.SqlServer.Replication.Subscription.Remove%2A> .  
  
###  <a name="examples-rmo"></a><a name="PShellExample"></a> Exemples (RMO)  
 Vous pouvez supprimer par programme des abonnements par émission de données à l'aide d'objets RMO (Replication Management Objects).  
  
 [!code-cs[HowTo#rmo_DropTranPushSub](../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_droptranpushsub)]  
  
 [!code-vb[HowTo#rmo_vb_DropTranPushSub](../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_droptranpushsub)]  
  
## <a name="see-also"></a>Voir aussi  
 [S’abonner aux Publications](../../relational-databases/replication/subscribe-to-publications.md)   
 [Bonnes pratiques en matière de sécurité de la réplication](../../relational-databases/replication/security/replication-security-best-practices.md)  
  
  
