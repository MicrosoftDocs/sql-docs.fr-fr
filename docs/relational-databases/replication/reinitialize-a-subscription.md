---
title: Réinitialiser un abonnement | Microsoft Docs
description: Découvrez comment réinitialiser un abonnement dans SQL Server à l’aide de SQL Server Management Studio, de Transact-SQL ou de Replication Management Objects.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- initializing subscriptions [SQL Server replication], reinitializing
- subscriptions [SQL Server replication], reinitializing
- reinitializing subscriptions
ms.assetid: ca3625c5-c62e-4ab7-9829-d511f838e385
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-current||>=sql-server-2016
ms.openlocfilehash: 03d97ad323c5e83e7cd0a5de0b09da6bad6b1a22
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250728"
---
# <a name="reinitialize-a-subscription"></a>Réinitialiser un abonnement
[!INCLUDE[sql-asdb](../../includes/applies-to-version/sql-asdb.md)]
  Cette rubrique explique comment réinitialiser un abonnement par extraction de données (pull) dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], de [!INCLUDE[tsql](../../includes/tsql-md.md)]ou d'objets RMO (Replication Management Objects). Des abonnements individuels peuvent être marqués pour réinitialisation afin qu'un nouvel instantané soit appliqué lors de la synchronisation suivante.  
  
[!INCLUDE[azure-sql-db-replication-supportability-note](../../includes/azure-sql-db-replication-supportability-note.md)]


  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
 La réinitialisation d'un abonnement est un processus en deux parties :  
  
1.  Un abonnement seul ou tous les abonnements à une publication sont *marqués* pour réinitialisation. Marquez des abonnements pour réinitialisation dans la boîte de dialogue **Réinitialiser les abonnements**, qui est disponible à partir du dossier **Publications locales** et du dossier **Abonnements locaux** dans [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Vous pouvez aussi marquer des abonnements à partir de l'onglet **Tous les abonnements** et du nœud des publications dans le moniteur de réplication. Pour plus d’informations sur le démarrage du Moniteur de réplication, consultez [Démarrer le Moniteur de réplication](../../relational-databases/replication/monitor/start-the-replication-monitor.md). Quand vous marquez un abonnement pour réinitialisation, vous disposez des options suivantes :  
  
     **Utiliser l'instantané actuel**  
     Permet d'appliquer l'instantané à l'Abonné lors de l'exécution suivante de l'agent de distribution ou de l'agent de fusion. Si aucun instantané valide n'est disponible, vous ne pouvez pas sélectionner cette option.  
  
     **Utiliser un nouvel instantané**  
     Permet de réinitialiser l'abonnement par un nouvel instantané. L'instantané ne peut être appliqué à l'Abonné qu'après sa génération par l'Agent d'instantané. Si l'Agent d'instantané est défini pour s'exécuter selon une planification, l'abonnement est réinitialisé seulement après l'exécution planifiée suivante de l'Agent d'instantané. Sélectionnez **Générer le nouvel instantané maintenant** pour lancer immédiatement l'Agent d'instantané.  
  
     **Télécharger les modifications non synchronisées avant la réinitialisation**  
     Réplication de fusion uniquement. Permet de télécharger vers le serveur toute modification en attente et apportée à la base de données d'abonnement avant que les données au niveau de l'Abonné ne soient écrasées par un instantané.  
  
     Si vous ajoutez, supprimez ou modifiez un filtre paramétré, les modifications en attente chez l'abonné ne peuvent pas être chargées sur le serveur de publication pendant la réinitialisation. Si vous voulez télécharger les modifications en attente, synchronisez tous les abonnements avant de modifier le filtre.  
  
2.  Un abonnement est réinitialisé la prochaine fois qu'il est synchronisé : l'Agent de distribution (pour la réplication transactionnelle) ou l'Agent de fusion (pour la réplication de fusion) applique l'instantané le plus récent à chaque Abonné qui a un abonnement marqué pour réinitialisation. Pour plus d'informations sur la synchronisation des abonnements, consultez [Synchronize a Push Subscription](../../relational-databases/replication/synchronize-a-push-subscription.md) et [Synchronize a Pull Subscription](../../relational-databases/replication/synchronize-a-pull-subscription.md).  
  
#### <a name="to-mark-a-single-push-or-pull-subscription-for-reinitialization-in-management-studio-at-the-publisher"></a>Pour marquer un abonnement par envoi de données (push) ou par extraction de données (pull) pour réinitialisation dans Management Studio (sur le serveur de publication)  
  
1.  Connectez-vous au serveur de publication dans [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)], puis développez le nœud du serveur.  
  
2.  Développez le dossier **Réplication** , puis développez le dossier **Publications locales** .  
  
3.  Développez la publication qui a l'abonnement que vous voulez réinitialiser.  
  
4.  Cliquez avec le bouton droit sur l'abonnement, puis sur **Réinitialiser**.  
  
5.  Dans la boîte de dialogue **Réinitialiser les abonnements** , sélectionnez les options, puis cliquez sur **Marquer pour réinitialisation**.  
  
#### <a name="to-mark-a-single-pull-subscription-for-reinitialization-in-management-studio-at-the-subscriber"></a>Pour marquer un seul abonnement par extraction de données (pull) pour réinitialisation dans Management Studio (sur l'Abonné)  
  
1.  Connectez-vous à l'Abonné dans [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)], puis développez le nœud du serveur.  
  
2.  Développez le dossier **Réplication** , puis développez le dossier **Abonnements locaux** .  
  
3.  Cliquez avec le bouton droit sur l'abonnement, puis sur **Réinitialiser**.  
  
4.  Dans la boîte de dialogue de confirmation qui s'affiche, cliquez sur **Oui**.  
  
#### <a name="to-mark-all-subscriptions-for-reinitialization-in-management-studio"></a>Pour marquer tous les abonnements pour réinitialisation dans Management Studio  
  
1.  Connectez-vous au serveur de publication dans [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)], puis développez le nœud du serveur.  
  
2.  Développez le dossier **Réplication** , puis développez le dossier **Publications locales** .  
  
3.  Cliquez avec le bouton droit sur la publication avec les abonnements que vous voulez réinitialiser, puis cliquez sur **Réinitialiser tous les abonnements**.  
  
4.  Dans la boîte de dialogue **Réinitialiser les abonnements** , sélectionnez les options, puis cliquez sur **Marquer pour réinitialisation**.  
  
#### <a name="to-mark-a-single-push-or-pull-subscription-for-reinitialization-in-replication-monitor"></a>Pour marquer un seul abonnement par envoi de données (push) ou par extraction de données (pull) pour réinitialisation dans le moniteur de réplication  
  
1.  Dans le moniteur de réplication, développez un groupe de serveurs de publication dans le volet gauche, développez un serveur de publication, puis cliquez sur une publication.  
  
2.  Cliquez sur l'onglet **Tous les abonnements** .  
  
3.  Cliquez avec le bouton droit sur l'abonnement que vous voulez réinitialiser, puis cliquez sur **Réinitialiser un abonnement**.  
  
4.  Dans la boîte de dialogue **Réinitialiser les abonnements** , sélectionnez les options, puis cliquez sur **Marquer pour réinitialisation**.  
  
#### <a name="to-mark-all-subscriptions-for-reinitialization-in-replication-monitor"></a>Pour marquer tous les abonnements pour réinitialisation dans le moniteur de réplication  
  
1.  Dans le moniteur de réplication, développez un groupe de serveurs de publication dans le volet gauche, puis développez un serveur de publication.  
  
2.  Cliquez avec le bouton droit sur la publication avec les abonnements que vous voulez réinitialiser, puis cliquez sur **Réinitialiser tous les abonnements**.  
  
3.  Dans la boîte de dialogue **Réinitialiser les abonnements** , sélectionnez les options, puis cliquez sur **Marquer pour réinitialisation**.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
 Les abonnements peuvent être réinitialisés par programme en utilisant des procédures stockées de réplication. La procédure stockée utilisée dépend du type d'abonnement (par extraction ou par émission de données) et du type de publication à laquelle l'abonnement appartient.  
  
#### <a name="to-reinitialize-a-pull-subscription-to-a-transactional-publication"></a>Pour réinitialiser un abonnement par extraction à une publication transactionnelle  
  
1.  Sur l’Abonné, dans la base de données d’abonnement, exécutez [sp_reinitpullsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-reinitpullsubscription-transact-sql.md). Spécifiez **\@publisher**, **\@publisher_db** et **\@publication**. L'abonnement est alors marqué pour réinitialisation lors de la prochaine exécution de l'Agent de distribution.  
  
2.  (Facultatif) Démarrez l'Agent de distribution sur l'Abonné pour synchroniser l'abonnement. Pour plus d’informations, consultez [Synchroniser un abonnement par extraction (pull)](../../relational-databases/replication/synchronize-a-pull-subscription.md).  
  
#### <a name="to-reinitialize-a-push-subscription-to-a-transactional-publication"></a>Pour réinitialiser un abonnement par émission de données à une publication transactionnelle  
  
1.  Sur le serveur de publication, exécutez [sp_reinitsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-reinitsubscription-transact-sql.md). Spécifiez **\@publication**, **\@subscriber** et **\@destination_db**. L'abonnement est alors marqué pour réinitialisation lors de la prochaine exécution de l'Agent de distribution.  
  
2.  (Facultatif) Démarrez l'Agent de distribution sur le serveur de distribution pour synchroniser l'abonnement. Pour plus d’informations, consultez [Synchroniser un abonnement par émission (push)](../../relational-databases/replication/synchronize-a-push-subscription.md).  
  
#### <a name="to-reinitialize-a-pull-subscription-to-a-merge-publication"></a>Pour réinitialiser un abonnement par extraction à une publication de fusion  
  
1.  Sur l’Abonné, dans la base de données d’abonnement, exécutez [sp_reinitmergepullsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-reinitmergepullsubscription-transact-sql.md). Spécifiez **\@publisher**, **\@publisher_db** et **\@publication**. Pour télécharger les modifications de l’Abonné avant la réinitialisation, affectez la valeur **true** à **\@upload_first**. L'abonnement est alors marqué pour réinitialisation lors de la prochaine exécution de l'Agent de fusion.  
  
    > [!IMPORTANT]  
    >  Si vous ajoutez, supprimez ou modifiez un filtre paramétré, les modifications en attente chez l'abonné ne peuvent pas être chargées sur le serveur de publication pendant la réinitialisation. Si vous voulez télécharger les modifications en attente, synchronisez tous les abonnements avant de modifier le filtre.  
  
2.  (Facultatif) Démarrez l'Agent de fusion sur l'Abonné pour synchroniser l'abonnement. Pour plus d’informations, consultez [Synchroniser un abonnement par extraction (pull)](../../relational-databases/replication/synchronize-a-pull-subscription.md).  
  
#### <a name="to-reinitialize-a-push-subscription-to-a-merge-publication"></a>Pour réinitialiser un abonnement par émission de données à une publication de fusion  
  
1.  Sur le serveur de publication, exécutez [sp_reinitmergesubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-reinitmergesubscription-transact-sql.md). Spécifiez **\@publication**, **\@subscriber** et **\@subscriber_db**. Pour télécharger les modifications de l’Abonné avant la réinitialisation, affectez la valeur **true** à **\@upload_first**. L'abonnement est alors marqué pour réinitialisation lors de la prochaine exécution de l'Agent de distribution.  
  
    > [!IMPORTANT]  
    >  Si vous ajoutez, supprimez ou modifiez un filtre paramétré, les modifications en attente chez l'abonné ne peuvent pas être chargées sur le serveur de publication pendant la réinitialisation. Si vous voulez télécharger les modifications en attente, synchronisez tous les abonnements avant de modifier le filtre.  
  
2.  (Facultatif) Démarrez l'Agent de fusion sur le serveur de distribution pour synchroniser l'abonnement. Pour plus d’informations, consultez [Synchroniser un abonnement par émission (push)](../../relational-databases/replication/synchronize-a-push-subscription.md).  
  
#### <a name="to-set-the-reinitialization-policy-when-creating-a-new-merge-publication"></a>Pour définir la stratégie de réinitialisation lors de la création d'une publication de fusion  
  
1.  Dans la base de données de publication sur le serveur de publication, exécutez [sp_addmergepublication](../../relational-databases/system-stored-procedures/sp-addmergepublication-transact-sql.md), en spécifiant l’une des valeurs suivantes pour **\@automatic_reinitialization_policy** :  
  
    -   **1** - les modifications sont téléchargées depuis l'Abonné avant réinitialisation automatique d'un abonnement suite à la modification de la publication.  
  
    -   **0** - les modifications au niveau de l'Abonné sont ignorées lorsqu'un abonnement est automatiquement réinitialisé suite à la modification de la publication.  
  
    > [!IMPORTANT]  
    >  Si vous ajoutez, supprimez ou modifiez un filtre paramétré, les modifications en attente chez l'abonné ne peuvent pas être chargées sur le serveur de publication pendant la réinitialisation. Si vous voulez télécharger les modifications en attente, synchronisez tous les abonnements avant de modifier le filtre.  
  
     Pour plus d’informations, voir [Create a Publication](../../relational-databases/replication/publish/create-a-publication.md).  
  
#### <a name="to-change-the-reinitialization-policy-for-an-existing-merge-publication"></a>Pour modifier la stratégie de réinitialisation pour une publication de fusion existante  
  
1.  Dans la base de données de publication sur le serveur de publication, exécutez [sp_changemergepublication](../../relational-databases/system-stored-procedures/sp-changemergepublication-transact-sql.md), en spécifiant **automatic_reinitialization_policy** pour **\@property** et l’une des valeurs suivantes pour **\@value** :  
  
    -   **1** - les modifications sont téléchargées depuis l'Abonné avant réinitialisation automatique d'un abonnement suite à la modification de la publication.  
  
    -   **0** - les modifications au niveau de l'Abonné sont ignorées lorsqu'un abonnement est automatiquement réinitialisé suite à la modification de la publication.  
  
    > [!IMPORTANT]  
    >  Si vous ajoutez, supprimez ou modifiez un filtre paramétré, les modifications en attente chez l'abonné ne peuvent pas être chargées sur le serveur de publication pendant la réinitialisation. Si vous voulez télécharger les modifications en attente, synchronisez tous les abonnements avant de modifier le filtre.  
  
     Pour plus d'informations, voir [View and Modify Publication Properties](../../relational-databases/replication/publish/view-and-modify-publication-properties.md).  
  
##  <a name="using-replication-management-objects-rmo"></a><a name="RMOProcedure"></a> Utilisation d'objets RMO (Replication Management Objects)  
 Des abonnements individuels peuvent être marqués pour réinitialisation afin qu'un nouvel instantané soit appliqué lors de la synchronisation suivante. Les abonnements peuvent être réinitialisés par programme à l'aide de Replication Management Objects. Les classes que vous utilisez dépendent du type de publication à laquelle l'abonnement appartient et du type d'abonnement (par extraction ou par émission de données).  
  
#### <a name="to-reinitialize-a-pull-subscription-to-a-transactional-publication"></a>Pour réinitialiser un abonnement par extraction à une publication transactionnelle  
  
1.  Créez une connexion à l'Abonné en utilisant la classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Créez une instance de la classe <xref:Microsoft.SqlServer.Replication.TransPullSubscription> , puis définissez <xref:Microsoft.SqlServer.Replication.PullSubscription.PublicationName%2A>, <xref:Microsoft.SqlServer.Replication.PullSubscription.DatabaseName%2A>, <xref:Microsoft.SqlServer.Replication.PullSubscription.PublisherName%2A>, <xref:Microsoft.SqlServer.Replication.PullSubscription.PublicationDBName%2A>et la connexion créée à l'étape 1 pour <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A>.  
  
3.  Appelez la méthode <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> pour obtenir les propriétés de l'objet.  
  
    > [!NOTE]  
    >  Si cette méthode retourne **false**, les propriétés de l'abonnement définies à l'étape 2 sont incorrectes ou l'abonnement par extraction n'existe pas.  
  
4.  Appelez la méthode <xref:Microsoft.SqlServer.Replication.TransPullSubscription.Reinitialize%2A> . Cette méthode marque l'abonnement pour réinitialisation.  
  
5.  Synchronisez l'abonnement par extraction. Pour plus d’informations, consultez [Synchroniser un abonnement par extraction (pull)](../../relational-databases/replication/synchronize-a-pull-subscription.md).  
  
#### <a name="to-reinitialize-a-push-subscription-to-a-transactional-publication"></a>Pour réinitialiser un abonnement par émission de données à une publication transactionnelle  
  
1.  Créez une connexion au serveur de publication en utilisant la classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Créez une instance de la classe <xref:Microsoft.SqlServer.Replication.TransSubscription> , puis définissez <xref:Microsoft.SqlServer.Replication.Subscription.PublicationName%2A>, <xref:Microsoft.SqlServer.Replication.Subscription.DatabaseName%2A>, <xref:Microsoft.SqlServer.Replication.Subscription.SubscriberName%2A>, <xref:Microsoft.SqlServer.Replication.Subscription.SubscriptionDBName%2A>et la connexion créée à l'étape 1 pour <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A>.  
  
3.  Appelez la méthode <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> pour obtenir les propriétés de l'objet.  
  
    > [!NOTE]  
    >  Si cette méthode retourne **false**, les propriétés de l'abonnement définies à l'étape 2 sont incorrectes ou l'abonnement par émission de données n'existe pas.  
  
4.  Appelez la méthode <xref:Microsoft.SqlServer.Replication.TransSubscription.Reinitialize%2A> . Cette méthode marque l'abonnement pour réinitialisation.  
  
5.  Synchronisez l'abonnement par émission de données. Pour plus d’informations, consultez [Synchroniser un abonnement par émission (push)](../../relational-databases/replication/synchronize-a-push-subscription.md).  
  
#### <a name="to-reinitialize-a-pull-subscription-to-a-merge-publication"></a>Pour réinitialiser un abonnement par extraction à une publication de fusion  
  
1.  Créez une connexion à l'Abonné en utilisant la classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Créez une instance de la classe <xref:Microsoft.SqlServer.Replication.MergePullSubscription> , puis définissez <xref:Microsoft.SqlServer.Replication.PullSubscription.PublicationName%2A>, <xref:Microsoft.SqlServer.Replication.PullSubscription.DatabaseName%2A>, <xref:Microsoft.SqlServer.Replication.PullSubscription.PublisherName%2A>, <xref:Microsoft.SqlServer.Replication.PullSubscription.PublicationDBName%2A>et la connexion créée à l'étape 1 pour <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A>.  
  
3.  Appelez la méthode <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> pour obtenir les propriétés de l'objet.  
  
    > [!NOTE]  
    >  Si cette méthode retourne **false**, les propriétés de l'abonnement définies à l'étape 2 sont incorrectes ou l'abonnement par extraction n'existe pas.  
  
4.  Appelez la méthode <xref:Microsoft.SqlServer.Replication.MergePullSubscription.Reinitialize%2A> . Passez la valeur **true** pour télécharger les modifications sur l'Abonné avant la réinitialisation ou la valeur **false** pour réinitialiser et perdre toute modification en attente sur l'Abonné. Cette méthode marque l'abonnement pour réinitialisation.  
  
    > [!NOTE]  
    >  Les modifications ne peuvent pas être téléchargées si l'abonnement a expiré. Pour plus d'informations, voir [Set the Expiration Period for Subscriptions](../../relational-databases/replication/publish/set-the-expiration-period-for-subscriptions.md).  
  
5.  Synchronisez l'abonnement par extraction. Pour plus d’informations, consultez [Synchroniser un abonnement par extraction (pull)](../../relational-databases/replication/synchronize-a-pull-subscription.md).  
  
#### <a name="to-reinitialize-a-push-subscription-to-a-merge-publication"></a>Pour réinitialiser un abonnement par émission de données à une publication de fusion  
  
1.  Créez une connexion au serveur de publication en utilisant la classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Créez une instance de la classe <xref:Microsoft.SqlServer.Replication.MergeSubscription> , puis définissez <xref:Microsoft.SqlServer.Replication.Subscription.PublicationName%2A>, <xref:Microsoft.SqlServer.Replication.Subscription.DatabaseName%2A>, <xref:Microsoft.SqlServer.Replication.Subscription.SubscriberName%2A>, <xref:Microsoft.SqlServer.Replication.Subscription.SubscriptionDBName%2A>et la connexion créée à l'étape 1 pour <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A>.  
  
3.  Appelez la méthode <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> pour obtenir les propriétés de l'objet.  
  
    > [!NOTE]  
    >  Si cette méthode retourne **false**, les propriétés de l'abonnement définies à l'étape 2 sont incorrectes ou l'abonnement par émission de données n'existe pas.  
  
4.  Appelez la méthode <xref:Microsoft.SqlServer.Replication.MergeSubscription.Reinitialize%2A> . Passez la valeur **true** pour télécharger les modifications sur l'Abonné avant la réinitialisation ou la valeur **false** pour réinitialiser et perdre toute modification en attente sur l'Abonné. Cette méthode marque l'abonnement pour réinitialisation.  
  
    > [!NOTE]  
    >  Les modifications ne peuvent pas être téléchargées si l'abonnement a expiré. Pour plus d'informations, voir [Set the Expiration Period for Subscriptions](../../relational-databases/replication/publish/set-the-expiration-period-for-subscriptions.md).  
  
5.  Synchronisez l'abonnement par émission de données. Pour plus d’informations, consultez [Synchroniser un abonnement par émission (push)](../../relational-databases/replication/synchronize-a-push-subscription.md).  
  
###  <a name="examples-rmo"></a><a name="PShellExample"></a> Exemples (RMO)  
 Cet exemple réinitialise un abonnement par extraction à une publication transactionnelle.  
  
 [!code-cs[HowTo#rmo_ReinitTranPullSub](../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_reinittranpullsub)]  
  
 [!code-vb[HowTo#rmo_vb_ReinitTranPullSub](../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_reinittranpullsub)]  
  
 Cet exemple réinitialise un abonnement par extraction à une publication de fusion après avoir téléchargé les modifications en attente sur l'Abonné.  
  
 [!code-cs[HowTo#rmo_ReinitMergePullSub_WithUpload](../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_reinitmergepullsub_withupload)]  
  
 [!code-vb[HowTo#rmo_vb_ReinitMergePullSub_WithUpload](../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_reinitmergepullsub_withupload)]  
  
## <a name="see-also"></a>Voir aussi  
 [Réinitialiser des abonnements](../../relational-databases/replication/reinitialize-subscriptions.md)   
 [Concepts liés à RMO (Replication Management Objects)](../../relational-databases/replication/concepts/replication-management-objects-concepts.md)   
 [Bonnes pratiques en matière de sécurité de la réplication](../../relational-databases/replication/security/replication-security-best-practices.md)  
  
  
