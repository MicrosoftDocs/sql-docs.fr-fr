---
title: Supprimer une publication | Microsoft Docs
description: Découvrez comment supprimer une publication dans SQL Server à l’aide de SQL Server Management Studio, Transact-SQL ou Replication Management Objects.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- removing publications
- publications [SQL Server replication], deleting
- articles [SQL Server replication], deleting
- deleting publications
ms.assetid: 408a1360-12ee-4896-ac94-482ae839593b
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: b1a87a8f9c2344b21cd67a24c35ab33615965300
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076727"
---
# <a name="delete-a-publication"></a>Supprimer une publication
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]
  Cette rubrique explique comment supprimer une publication dans [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)], de [!INCLUDE[tsql](../../../includes/tsql-md.md)]ou d'objets RMO (Replication Management Objects).  
  
 **Dans cette rubrique**  
  
-   **Pour supprimer une publication à l'aide de :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
     [Objets RMO (Replication Management Objects)](#RMOProcedure)  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
 Supprimer les publications dans le dossier **Publications locales** de [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)].  
  
#### <a name="to-delete-a-publication"></a>Pour supprimer une publication  
  
1.  Connectez-vous au serveur de publication dans [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)], puis développez le nœud du serveur.  
  
2.  Développez le dossier **Réplication** , puis développez le dossier **Publications locales** .  
  
3.  Cliquez avec le bouton droit sur la publication à supprimer, puis cliquez sur **Supprimer**.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
 Il est possible de supprimer des publications par programme en utilisant les procédures stockées de réplication. Les procédures stockées à utiliser dépendent du type de publication à supprimer.  
  
> [!NOTE]  
>  La suppression d'une publication ne supprime pas les objets publiés de la base de données de publication ni les objets correspondants de la base de données d'abonnement. Utilisez la commande `DROP <object>` pour supprimer manuellement ces objets, si nécessaire.  
  
#### <a name="to-delete-a-snapshot-or-transactional-publication"></a>Pour supprimer une publication d'instantané ou une publication transactionnelle  
  
1.  Effectuez l’une des actions suivantes :  
  
    -   Pour supprimer une publication unique, exécutez [sp_droppublication](../../../relational-databases/system-stored-procedures/sp-droppublication-transact-sql.md) sur la base de données de publication du serveur de publication.  
  
    -   Pour supprimer toutes les publications et tous les objets de réplication d'une base de données publiée, exécutez [sp_removedbreplication](../../../relational-databases/system-stored-procedures/sp-removedbreplication-transact-sql.md) sur le serveur de publication. Spécifiez la valeur **tran** pour **\@type**. (Facultatif) Si le serveur de distribution est inaccessible ou si l’état de la base de données est suspect ou hors connexion, spécifiez la valeur **1** pour **\@force**. (Facultatif) Spécifiez le nom de la base de données pour **\@dbname** si [sp_removedbreplication](../../../relational-databases/system-stored-procedures/sp-removedbreplication-transact-sql.md) n’est pas exécuté sur la base de données de publication.  
  
        > [!NOTE]  
        >  Spécifier la valeur **1** pour **\@force** peut laisser des objets de publication liés à la réplication dans la base de données.  
  
2.  (Facultatif) Si cette base de données n’a pas d’autres publications, exécutez [sp_replicationdboption &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-replicationdboption-transact-sql.md) pour désactiver la publication de la base de données actuelle à l’aide de la réplication transactionnelle ou d’instantané.  
  
3.  (Facultatif) Exécutez [sp_subscription_cleanup](../../../relational-databases/system-stored-procedures/sp-subscription-cleanup-transact-sql.md) sur la base de données d'abonnement de l'Abonné pour supprimer toutes les métadonnées de réplication restantes dans la base de données d'abonnement.  
  
#### <a name="to-delete-a-merge-publication"></a>Pour supprimer une publication de fusion  
  
1.  Effectuez l’une des actions suivantes :  
  
    -   Pour supprimer une publication unique, exécutez [sp_dropmergepublication &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-dropmergepublication-transact-sql.md) sur la base de données de publication du serveur de publication.  
  
    -   Pour supprimer toutes les publications et tous les objets de réplication d'une base de données publiée, exécutez [sp_removedbreplication](../../../relational-databases/system-stored-procedures/sp-removedbreplication-transact-sql.md) sur le serveur de publication. Spécifiez la valeur **merge** pour **\@type**. (Facultatif) Si le serveur de distribution est inaccessible ou si l’état de la base de données est suspect ou hors connexion, spécifiez la valeur **1** pour **\@force**. (Facultatif) Spécifiez le nom de la base de données pour **\@dbname** si [sp_removedbreplication](../../../relational-databases/system-stored-procedures/sp-removedbreplication-transact-sql.md) n’est pas exécuté sur la base de données de publication.  
  
        > [!NOTE]  
        >  Spécifier la valeur **1** pour **\@force** peut laisser des objets de publication liés à la réplication dans la base de données.  
  
2.  (Facultatif) Si cette base de données n’a pas d’autres publications, exécutez [sp_replicationdboption &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-replicationdboption-transact-sql.md) pour désactiver la publication de la base de données actuelle à l’aide de la réplication de fusion.  
  
3.  (Facultatif) Exécutez [sp_mergesubscription_cleanup &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-mergesubscription-cleanup-transact-sql.md) sur la base de données d’abonnement de l’Abonné pour supprimer toutes les métadonnées de réplication restantes dans la base de données d’abonnement.  
  
###  <a name="examples-transact-sql"></a><a name="TsqlExample"></a> Exemples (Transact-SQL)  
 Cet exemple montre comment supprimer une publication transactionnelle et désactiver la publication transactionnelle d'une base de données. Cet exemple suppose que tous les abonnements ont été supprimés précédemment. Pour plus d'informations, consultez [Delete a Pull Subscription](../../../relational-databases/replication/delete-a-pull-subscription.md) ou [Delete a Push Subscription](../../../relational-databases/replication/delete-a-push-subscription.md).  
  
 [!code-sql[HowTo#sp_droppublication](../../../relational-databases/replication/codesnippet/tsql/delete-a-publication_1.sql)]  
  
 Cet exemple montre comment supprimer une publication de fusion et désactiver la publication de fusion d'une base de données. Cet exemple suppose que tous les abonnements ont été supprimés précédemment. Pour plus d'informations, consultez [Delete a Pull Subscription](../../../relational-databases/replication/delete-a-pull-subscription.md) ou [Delete a Push Subscription](../../../relational-databases/replication/delete-a-push-subscription.md).  
  
 [!code-sql[HowTo#sp_dropmergepublication](../../../relational-databases/replication/codesnippet/tsql/delete-a-publication_2.sql)]  
  
##  <a name="using-replication-management-objects-rmo"></a><a name="RMOProcedure"></a> Utilisation d'objets RMO (Replication Management Objects)  
 Vous pouvez supprimer des publications par programme à l'aide d'objets RMO (Replication Management Objects). Les classes RMO que vous utilisez pour supprimer une publication dépendent du type de publication que vous supprimez.  
  
#### <a name="to-remove-a-snapshot-or-transactional-publication"></a>Pour supprimer une publication transactionnelle ou d'instantané  
  
1.  Créez une connexion au serveur de publication en utilisant la classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Créez une instance de la classe <xref:Microsoft.SqlServer.Replication.TransPublication>.  
  
3.  Définissez les propriétés <xref:Microsoft.SqlServer.Replication.Publication.Name%2A> et <xref:Microsoft.SqlServer.Replication.Publication.DatabaseName%2A> de la publication, et définissez la propriété <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> avec la connexion créée à l'étape 1.  
  
4.  Vérifiez la propriété <xref:Microsoft.SqlServer.Replication.ReplicationObject.IsExistingObject%2A> pour vous assurer que la publication existe. Si la valeur de cette propriété est **false**, les propriétés de la publication définies à l'étape 3 sont incorrectes ou la publication n'existe pas.  
  
5.  Appelez la méthode <xref:Microsoft.SqlServer.Replication.Publication.Remove%2A> .  
  
6.  (Facultatif) Si aucune autre publication transactionnelle n'existe pour cette base de données, vous pouvez désactiver la base de données pour la publication transactionnelle en procédant comme suit :  
  
    1.  Créez une instance de la classe <xref:Microsoft.SqlServer.Replication.ReplicationDatabase>. Affectez à la propriété <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> l'instance de <xref:Microsoft.SqlServer.Management.Common.ServerConnection> créée à l'étape 1.  
  
    2.  Appelez la méthode <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> . Si cette méthode retourne **false**, confirmez que la base de données existe.  
  
    3.  Définissez les propriétés <xref:Microsoft.SqlServer.Replication.ReplicationDatabase.EnabledTransPublishing%2A> la valeur **false**.  
  
    4.  Appelez la méthode <xref:Microsoft.SqlServer.Replication.ReplicationObject.CommitPropertyChanges%2A> .  
  
7.  Fermez les connexions.  
  
#### <a name="to-remove-a-merge-publication"></a>Pour supprimer une publication de fusion  
  
1.  Créez une connexion au serveur de publication en utilisant la classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Créez une instance de la classe <xref:Microsoft.SqlServer.Replication.MergePublication>.  
  
3.  Définissez les propriétés <xref:Microsoft.SqlServer.Replication.Publication.Name%2A> et <xref:Microsoft.SqlServer.Replication.Publication.DatabaseName%2A> de la publication, et définissez la propriété <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> avec la connexion créée à l'étape 1.  
  
4.  Vérifiez la propriété <xref:Microsoft.SqlServer.Replication.ReplicationObject.IsExistingObject%2A> pour vous assurer que la publication existe. Si la valeur de cette propriété est **false**, les propriétés de la publication définies à l'étape 3 sont incorrectes ou la publication n'existe pas.  
  
5.  Appelez la méthode <xref:Microsoft.SqlServer.Replication.Publication.Remove%2A> .  
  
6.  (Facultatif) Si aucune autre publication de fusion n'existe pour cette base de données, vous pouvez désactiver la base de données pour la publication de fusion en procédant comme suit :  
  
    1.  Créez une instance de la classe <xref:Microsoft.SqlServer.Replication.ReplicationDatabase>. Affectez à la propriété <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> l'instance de <xref:Microsoft.SqlServer.Management.Common.ServerConnection> créée à l'étape 1.  
  
    2.  Appelez la méthode <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> . Si cette méthode retourne **false**, vérifiez que la base de données existe.  
  
    3.  Définissez les propriétés <xref:Microsoft.SqlServer.Replication.ReplicationDatabase.EnabledMergePublishing%2A> la valeur **false**.  
  
    4.  Appelez la méthode <xref:Microsoft.SqlServer.Replication.ReplicationObject.CommitPropertyChanges%2A> .  
  
7.  Fermez les connexions.  
  
###  <a name="examples-rmo"></a><a name="PShellExample"></a> Exemples (RMO)  
 L'exemple suivant supprime une publication transactionnelle. Si aucune autre publication transactionnelle n'existe pour cette base de données, la publication transactionnelle est également désactivée.  
  
 [!code-cs[HowTo#rmo_DropTranPub](../../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_droptranpub)]  
  
 [!code-vb[HowTo#rmo_vb_DropTranPub](../../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_droptranpub)]  
  
 L'exemple suivant supprime une publication de fusion. Si aucune autre publication de fusion n'existe pour cette base de données, la publication de fusion est également désactivée.  
  
 [!code-cs[HowTo#rmo_DropMergePub](../../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_dropmergepub)]  
  
 [!code-vb[HowTo#rmo_vb_DropMergePub](../../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_dropmergepub)]  
  
## <a name="see-also"></a>Voir aussi  
 [Concepts liés aux procédures stockées système de réplication](../../../relational-databases/replication/concepts/replication-system-stored-procedures-concepts.md)   
 [Publier des données et des objets de base de données](../../../relational-databases/replication/publish/publish-data-and-database-objects.md)  
  
  
