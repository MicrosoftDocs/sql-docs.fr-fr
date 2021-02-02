---
title: Créer un abonnement par émission de données | Microsoft Docs
description: Découvrez comment créer un abonnement par émission de données dans SQL Server à l’aide de SQL Server Management Studio, Transact-SQL ou Replication Management Objects.
ms.custom: ''
ms.date: 08/25/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- push subscriptions [SQL Server replication], creating
- merge replication subscribing [SQL Server replication], push subscriptions
- subscriptions [SQL Server replication], push
- snapshot replication [SQL Server], subscribing
- transactional replication, subscribing
ms.assetid: adfbbc61-58d1-4330-9ad6-b14ab1142e2b
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-current||>=sql-server-2016
ms.openlocfilehash: 9cb0106cc23c6c7bbae05528b82ad5d83617cefe
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076171"
---
# <a name="create-a-push-subscription"></a>Créer un abonnement par émission de données
[!INCLUDE[sql-asdb](../../includes/applies-to-version/sql-asdb.md)]
  Cette rubrique explique comment créer un abonnement par émission de données (push) dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], de [!INCLUDE[tsql](../../includes/tsql-md.md)]ou d'objets RMO (Replication Management Objects). Pour plus d’informations sur la création d’un abonnement par émission de données pour un abonné non-[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consultez [Créer un abonnement pour un abonné non-SQL Server](../../relational-databases/replication/create-a-subscription-for-a-non-sql-server-subscriber.md).  

[!INCLUDE[azure-sql-db-replication-supportability-note](../../includes/azure-sql-db-replication-supportability-note.md)]
  
 
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
Créez un abonnement par émission de données sur le serveur de publication ou l’abonné à l’aide de l’Assistant Nouvel abonnement. Exécutez les étapes de l'Assistant pour :  
  
- Spécifier le serveur de publication et la publication.  
  
- Sélectionner l'emplacement d'exécution des Agents de réplication. Pour un abonnement envoyé, sélectionnez **Exécuter tous les agents sur le serveur de distribution (abonnements par envoi de données (push))** dans la page **Emplacement de l'Agent de distribution** ou la page **Emplacement de l'Agent de fusion** selon le type de publication.  
  
- Spécifiez des Abonnés et des bases de données d'abonnement.  
  
- Spécifiez les noms de connexion et mots de passe utilisés pour les connexions établies par les Agents de réplication :  
  
  - Pour les abonnements aux publications d'instantané et transactionnelles, spécifiez les informations d'identification dans la page **Sécurité de l'Agent de distribution** .  
  
  - Pour les abonnements aux publications de fusion, spécifiez les informations d'identification dans la page **Sécurité de l'Agent de fusion** .  
  
    Pour plus d’informations sur les autorisations requises par chaque agent, consultez [Modèle de sécurité de l’agent de réplication](../../relational-databases/replication/security/replication-agent-security-model.md).  
  
- Spécifiez une planification de la synchronisation et le moment choisi pour initialiser l'Abonné.  
  
- Spécifiez d’autres options pour les publications de fusion : type d’abonnement et valeurs pour le filtrage paramétrable.  
  
- Spécifiez d’autres options pour les publications transactionnelles qui autorisent les abonnements de mise à jour. Une option consiste à décider si les abonnés doivent valider immédiatement les modifications sur le serveur de publication ou les écrire dans une file d’attente. Une autre option consiste à configurer les informations d’identification utilisées pour la connexion de l’abonné au serveur de publication.  
  
- Créez éventuellement un script pour l’abonnement.  
  
#### <a name="to-create-a-push-subscription-from-the-publisher"></a>Pour créer un abonnement envoyé à partir du serveur de publication  
  
1. Connectez-vous au serveur de publication dans [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], puis développez le nœud du serveur.  
  
2. Développez le dossier **Réplication** , puis développez le dossier **Publications locales** .  
  
3. Cliquez avec le bouton droit sur la publication pour laquelle vous souhaitez créer un ou plusieurs abonnements, puis sélectionnez **Nouveaux abonnements**.  
  
4. Exécutez les étapes de l'Assistant Nouvel abonnement.  
  
#### <a name="to-create-a-push-subscription-from-the-subscriber"></a>Pour créer un abonnement envoyé à partir de l'Abonné  
  
1. Connectez-vous à l'Abonné dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], puis développez le nœud du serveur.  
  
2. Développez le dossier **Réplication** .  
  
3. Cliquez avec le bouton droit sur le dossier **Abonnements locaux**, puis sélectionnez **Nouveaux abonnements**.  
  
4. Dans la page **Publication** de l’Assistant Nouvel abonnement, sélectionnez **\<Find SQL Server Publisher>** ou **\<Find Oracle Publisher>** dans la liste déroulante **Serveur de publication**.  
  
5. Connectez-vous au serveur de publication dans la boîte de dialogue **Se connecter au serveur** .  
  
6. Sélectionnez une publication dans la page **Publication** .  
  
7. Exécutez les étapes de l'Assistant Nouvel abonnement.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
Vous pouvez créer par programmation des abonnements par émission de données à l’aide de procédures stockées de réplication. Les procédures stockées utilisées dépendent du type de publication auquel l'abonnement appartient.  
  
> [!IMPORTANT]
> Lorsque c'est possible, demande aux utilisateurs de fournir les informations d'identification au moment de l'exécution. Si vous devez enregistrer les informations d'identification dans un fichier de script, vous devez sécuriser le fichier pour empêcher un accès non autorisé.  
  
#### <a name="to-create-a-push-subscription-to-a-snapshot-or-transactional-publication"></a>Pour créer un abonnement par envoi de données vers un instantané ou une publication transactionnelle  
  
1. Au niveau du serveur de publication sur la base de données de publication, vérifiez que la publication prend en charge les abonnements par émission de données en exécutant [sp_helppublication](../../relational-databases/system-stored-procedures/sp-helppublication-transact-sql.md).  
  
   - Si la valeur de **allow_push** est **1**, les abonnements par envoi de données sont pris en charge.  
  
   - Si la valeur de **allow_push** est **0**, exécutez [sp_changepublication](../../relational-databases/system-stored-procedures/sp-changepublication-transact-sql.md). Spécifiez **allow_push** pour **\@property** et **true** pour **\@value**.  
  
2. Au niveau du serveur de publication sur la base de données de publication, exécutez [sp_addsubscription](../system-stored-procedures/sp-addsubscription-transact-sql.md). Spécifiez **\@publication**, **\@subscriber** et **\@destination_db**. Spécifiez la valeur **push** pour **\@subscription_type**. Pour plus d’informations sur la mise à jour des abonnements, consultez [Créer un abonnement pouvant être mis à jour pour une publication transactionnelle](publish/create-an-updatable-subscription-to-a-transactional-publication.md).  
  
3. Au niveau du serveur de publication sur la base de données de publication, exécutez [sp_addpushsubscription_agent](../../relational-databases/system-stored-procedures/sp-addpushsubscription-agent-transact-sql.md). Spécifiez les éléments suivants :  
  
   - Les paramètres **\@subscriber**, **\@subscriber_db** et **\@publication**.  
  
   - Les informations d’identification [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows sous lesquelles l’Agent de distribution est exécuté sur le serveur de distribution pour **\@job_login** et **\@job_password**.  
  
     > [!NOTE]
     > Les connexions établies à l’aide de l’authentification Windows intégrée utilisent toujours les informations d’identification Windows spécifiées par **\@job_login** et **\@job_password**. L’Agent de distribution établit toujours la connexion locale au serveur de distribution à l’aide de l’authentification Windows intégrée. Par défaut, l’agent se connecte à l’abonné à l’aide de l’authentification Windows intégrée.  
  
   - (Facultatif) La valeur **0** pour **\@subscriber_security_mode** et les informations de connexion [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour **\@subscriber_login** et **\@subscriber_password**. Spécifiez ces paramètres si vous devez utiliser l'authentification SQL Server lors de la connexion à l'abonné.  
  
   - Planification du travail de l'Agent de distribution pour cet abonnement. Pour plus d’informations, consultez [Spécifier des planifications de synchronisation](../../relational-databases/replication/specify-synchronization-schedules.md).  
  
> [!IMPORTANT]
> Lors de la création d’un abonnement par émission de données sur un serveur de publication avec un serveur de distribution distant, les valeurs fournies pour tous les paramètres, notamment *job_login* et *job_password*, sont envoyées au serveur de distribution en texte brut. Vous devez chiffrer la connexion entre le serveur de publication et son serveur de distribution distant avant d’exécuter cette procédure stockée. Pour plus d’informations, consultez [Activer des connexions chiffrées dans le moteur de base de données &#40;Gestionnaire de configuration SQL Server&#41;](../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md).  
  
#### <a name="to-create-a-push-subscription-to-a-merge-publication"></a>Pour créer un abonnement par envoi de données vers une publication de fusion  
  
1. Au niveau du serveur de publication sur la base de données de publication, vérifiez que la publication prend en charge les abonnements par émission de données en exécutant [sp_helpmergepublication](../../relational-databases/system-stored-procedures/sp-helpmergepublication-transact-sql.md).  
  
   - Si la valeur de **allow_push** est **1**, la publication prend en charge les abonnements par envoi de données.  
  
   - Si la valeur de **allow_push** n’est pas **1**, exécutez [sp_changemergepublication](../../relational-databases/system-stored-procedures/sp-changemergepublication-transact-sql.md). Spécifiez **allow_push** pour **\@property** et **true** pour **\@value**.  
  
2. Au niveau du serveur de publication sur la base de données de publication, exécutez [sp_addmergesubscription](../../relational-databases/system-stored-procedures/sp-addmergesubscription-transact-sql.md). Spécifiez les paramètres suivants :  
  
   - **\@publication**. Il s'agit du nom de la publication.  
  
   - **\@subscriber_type**. Pour un abonnement client, spécifiez **local**. Pour un abonnement serveur, spécifiez **global**.  
  
   - **\@subscription_priority**. Pour un abonnement serveur, spécifiez la priorité de l'abonnement (de **0.00** à **99.99**).  
  
   Pour plus d’informations, consultez [Détection et résolution avancées des conflits de réplication de fusion](../../relational-databases/replication/merge/advanced-merge-replication-conflict-detection-and-resolution.md).  
  
3. Au niveau du serveur de publication sur la base de données de publication, exécutez [sp_addmergepushsubscription_agent](../../relational-databases/system-stored-procedures/sp-addmergepushsubscription-agent-transact-sql.md). Spécifiez les éléments suivants :  
  
   - Les paramètres **\@subscriber**, **\@subscriber_db** et **\@publication**.  
  
   - Les informations d’identification Windows sous lesquelles l’Agent de fusion est exécuté sur le serveur de distribution pour **\@job_login** et **\@job_password**.  
  
     > [!NOTE]
     > Les connexions établies à l’aide de l’authentification Windows intégrée utilisent toujours les informations d’identification Windows spécifiées par **\@job_login** et **\@job_password**. L’Agent de fusion établit toujours la connexion locale au serveur de distribution à l’aide de l’authentification Windows intégrée. Par défaut, l’agent se connecte à l’abonné à l’aide de l’authentification Windows intégrée.  
  
   - (Facultatif) La valeur **0** pour **\@subscriber_security_mode** et les informations de connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour **\@subscriber_login** et **\@subscriber_password**. Spécifiez ces paramètres si vous devez utiliser l'authentification SQL Server lors de la connexion à l'abonné.  
  
   - (Facultatif) La valeur **0** pour **\@publisher_security_mode** et les informations de connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour **\@publisher_login** et **\@publisher_password**. Spécifiez ces valeurs si vous devez utiliser l'authentification SQL Server lors de la connexion à l'abonné.  
  
   - Une planification du travail de l'Agent de fusion pour cet abonnement. Pour plus d’informations, consultez [Spécifier des planifications de synchronisation](../../relational-databases/replication/specify-synchronization-schedules.md).  
  
> [!IMPORTANT]
> Lors de la création d’un abonnement par émission de données sur un serveur de publication avec un serveur de distribution distant, les valeurs fournies pour tous les paramètres, notamment *job_login* et *job_password*, sont envoyées au serveur de distribution en texte brut. Vous devez chiffrer la connexion entre le serveur de publication et son serveur de distribution distant avant d’exécuter cette procédure stockée. Pour plus d’informations, consultez [Activer des connexions chiffrées dans le moteur de base de données &#40;Gestionnaire de configuration SQL Server&#41;](../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md).  
  
###  <a name="examples-transact-sql"></a><a name="TsqlExample"></a> Exemples (Transact-SQL)  
 L'exemple suivant crée un abonnement par envoi de données vers une publication transactionnelle. Les valeurs de connexion et de mot de passe sont fournies lors de l’exécution à l’aide des variables de script **sqlcmd**.  
  
 [!code-sql[HowTo#sp_addtranpushsubscription_agent](../../relational-databases/replication/codesnippet/tsql/create-a-push-subscription_1.sql)]  
  
 L'exemple suivant crée un abonnement par envoi de données vers une publication de fusion. Les valeurs de connexion et de mot de passe sont fournies lors de l’exécution à l’aide des variables de script **sqlcmd**.  
  
 [!code-sql[HowTo#sp_addmergepushsubscriptionagent](../../relational-databases/replication/codesnippet/tsql/create-a-push-subscription_2.sql)]  
  
##  <a name="using-replication-management-objects"></a><a name="RMOProcedure"></a> Utilisation d’objets RMO (Replication Management Object)  
 Vous pouvez créer par programme des abonnements par émission de données (push) à l'aide d'objets RMO (Replication Management Objects). Les classes RMO que vous utilisez pour créer un abonnement par émission de données dépendent du type de publication sur laquelle l’abonnement est créé.  
  
> [!IMPORTANT]
> Lorsque c'est possible, demande aux utilisateurs de fournir les informations d'identification au moment de l'exécution. Si vous devez stocker des informations d’identification, utilisez les [services de chiffrement](/previous-versions/aa719848(v=vs.71)) fournis par [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows .NET Framework.  
  
#### <a name="to-create-a-push-subscription-to-a-snapshot-or-transactional-publication"></a>Pour créer un abonnement par envoi de données vers un instantané ou une publication transactionnelle  
  
1. Créez une connexion au serveur de publication en utilisant la classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2. Créez une instance de la classe <xref:Microsoft.SqlServer.Replication.TransPublication> en utilisant la connexion au serveur de publication de l'étape 1. Spécifiez <xref:Microsoft.SqlServer.Replication.Publication.Name%2A>, <xref:Microsoft.SqlServer.Replication.Publication.DatabaseName%2A>et <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A>.  
  
3. Appelez la méthode <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> . Si cette méthode retourne **false**, les propriétés spécifiées à l'étape 2 sont incorrectes ou la publication n'existe pas sur le serveur.  
  
4. Effectuez une opération AND logique au niveau du bit ( **&** dans Visual C# et **And** dans Visual Basic) entre la propriété <xref:Microsoft.SqlServer.Replication.Publication.Attributes%2A> et <xref:Microsoft.SqlServer.Replication.PublicationAttributes.AllowPush>. Si le résultat est <xref:Microsoft.SqlServer.Replication.PublicationAttributes.None>, appliquez à <xref:Microsoft.SqlServer.Replication.Publication.Attributes%2A> le résutat d'une opération OR logique au niveau du bit ( **|** dans Visual C# et **Or** dans Visual Basic) entre <xref:Microsoft.SqlServer.Replication.Publication.Attributes%2A> et <xref:Microsoft.SqlServer.Replication.PublicationAttributes.AllowPush>. Appelez ensuite <xref:Microsoft.SqlServer.Replication.ReplicationObject.CommitPropertyChanges%2A> pour activer les abonnements par envoi de données.  
  
5. Si la base de données d'abonnements n'existe pas, créez-la en utilisant la classe <xref:Microsoft.SqlServer.Management.Smo.Database> . Pour plus d’informations, consultez [Création, modification et suppression de bases de données](../../relational-databases/server-management-objects-smo/tasks/creating-altering-and-removing-databases.md).  
  
6. Créez une instance de la classe <xref:Microsoft.SqlServer.Replication.TransSubscription>.  
  
7. Définissez les propriétés suivantes des abonnements :  
  
   - <xref:Microsoft.SqlServer.Management.Common.ServerConnection> sur l'Agent de publication créé à l'étape 1 pour <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A>.  
  
   - Nom de la base de données d'abonnements pour <xref:Microsoft.SqlServer.Replication.Subscription.SubscriptionDBName%2A>.  
  
   - Nom de l'abonné pour <xref:Microsoft.SqlServer.Replication.Subscription.SubscriberName%2A>. 
  
   - Nom de la base de données de publication pour <xref:Microsoft.SqlServer.Replication.Subscription.DatabaseName%2A>.  
  
   - Nom de la publication pour <xref:Microsoft.SqlServer.Replication.Subscription.PublicationName%2A>.
  
   - Les paramètres <xref:Microsoft.SqlServer.Replication.IProcessSecurityContext.Login%2A> et <xref:Microsoft.SqlServer.Replication.IProcessSecurityContext.Password%2A> de <xref:Microsoft.SqlServer.Replication.Subscription.SynchronizationAgentProcessSecurity%2A> pour fournir les informations d'identification pour le compte [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows sous lequel l'Agent de distribution fonctionne sur le serveur de distribution. Ce compte permet d’établir des connexions locales au serveur de distribution et des connexions distantes à l’aide de l’authentification Windows.  
  
     > [!NOTE]
     > La configuration de <xref:Microsoft.SqlServer.Replication.Subscription.SynchronizationAgentProcessSecurity%2A> quand l’abonnement est créé par un membre du rôle serveur fixe **sysadmin** n’est pas nécessaire, mais elle est néanmoins recommandée. Dans ce cas, l'Agent va emprunter l'identité du compte de l'Agent SQL Server. Pour plus d’informations, consultez [Modèle de sécurité de l’agent de réplication](../../relational-databases/replication/security/replication-agent-security-model.md).  
  
   - (Facultatif) La valeur **true** (par défaut) de <xref:Microsoft.SqlServer.Replication.Subscription.CreateSyncAgentByDefault%2A> pour créer un travail de l'Agent qui permet de synchroniser l'abonnement. Si vous spécifiez **false**, l'abonnement peut uniquement être synchronisé par programme.  
  
   - (Facultatif) Définissez les champs <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SqlStandardLogin%2A> et <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SqlStandardPassword%2A> ou <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SecureSqlStandardPassword%2A> de <xref:Microsoft.SqlServer.Replication.Subscription.SubscriberSecurity%2A> lorsque vous utilisez l'authentification SQL Server pour vous connecter à l'abonné.  
  
8. Appelez la méthode <xref:Microsoft.SqlServer.Replication.Subscription.Create%2A> .  
  
> [!IMPORTANT]
> Lors de la création d’un abonnement par émission de données sur un serveur de publication avec un serveur de distribution distant, les valeurs fournies pour toutes les propriétés, notamment <xref:Microsoft.SqlServer.Replication.Subscription.SynchronizationAgentProcessSecurity%2A>, sont envoyées au serveur de distribution en texte brut. Vous devez chiffrer la connexion entre le serveur de publication et son serveur de distribution distant avant d'appeler la méthode <xref:Microsoft.SqlServer.Replication.Subscription.Create%2A> . Pour plus d’informations, consultez [Activer des connexions chiffrées dans le moteur de base de données &#40;Gestionnaire de configuration SQL Server&#41;](../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md).  
  
#### <a name="to-create-a-push-subscription-to-a-merge-publication"></a>Pour créer un abonnement par envoi de données vers une publication de fusion  
  
1. Créez une connexion au serveur de publication en utilisant la classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2. Créez une instance de la classe <xref:Microsoft.SqlServer.Replication.MergePublication> en utilisant la connexion au serveur de publication de l'étape 1. Spécifiez <xref:Microsoft.SqlServer.Replication.Publication.Name%2A>, <xref:Microsoft.SqlServer.Replication.Publication.DatabaseName%2A>et <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A>.  
  
3. Appelez la méthode <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> . Si cette méthode retourne **false**, les propriétés spécifiées à l'étape 2 sont incorrectes ou la publication n'existe pas sur le serveur.  
  
4. Effectuez une opération AND logique au niveau du bit ( **&** dans Visual C# et **And** dans Visual Basic) entre la propriété <xref:Microsoft.SqlServer.Replication.Publication.Attributes%2A> et <xref:Microsoft.SqlServer.Replication.PublicationAttributes.AllowPush>. Si le résultat est <xref:Microsoft.SqlServer.Replication.PublicationAttributes.None>, appliquez à <xref:Microsoft.SqlServer.Replication.Publication.Attributes%2A> le résutat d'une opération OR logique au niveau du bit ( **|** dans Visual C# et **Or** dans Visual Basic) entre <xref:Microsoft.SqlServer.Replication.Publication.Attributes%2A> et <xref:Microsoft.SqlServer.Replication.PublicationAttributes.AllowPush>. Appelez ensuite <xref:Microsoft.SqlServer.Replication.ReplicationObject.CommitPropertyChanges%2A> pour activer les abonnements par envoi de données.  
  
5. Si la base de données d'abonnements n'existe pas, créez-la en utilisant la classe <xref:Microsoft.SqlServer.Management.Smo.Database> . Pour plus d’informations, consultez [Création, modification et suppression de bases de données](../../relational-databases/server-management-objects-smo/tasks/creating-altering-and-removing-databases.md).  
  
6. Créez une instance de la classe <xref:Microsoft.SqlServer.Replication.MergeSubscription>.  
  
7. Définissez les propriétés suivantes des abonnements :  
  
   - <xref:Microsoft.SqlServer.Management.Common.ServerConnection> sur l'Agent de publication créé à l'étape 1 pour <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A>.  
  
   - Nom de la base de données d'abonnements pour <xref:Microsoft.SqlServer.Replication.Subscription.SubscriptionDBName%2A>.  
  
   - Nom de l'abonné pour <xref:Microsoft.SqlServer.Replication.Subscription.SubscriberName%2A>.    
   - Nom de la base de données de publication pour <xref:Microsoft.SqlServer.Replication.Subscription.DatabaseName%2A>.  
  
   - Nom de la publication pour <xref:Microsoft.SqlServer.Replication.Subscription.PublicationName%2A>.    
   - Les paramètres <xref:Microsoft.SqlServer.Replication.IProcessSecurityContext.Login%2A> et <xref:Microsoft.SqlServer.Replication.IProcessSecurityContext.Password%2A> de <xref:Microsoft.SqlServer.Replication.Subscription.SynchronizationAgentProcessSecurity%2A> pour fournir les informations d'identification pour le compte [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows sous lequel l'Agent de fusion fonctionne sur le serveur de distribution. Ce compte permet d’établir des connexions locales au serveur de distribution et des connexions distantes à l’aide de l’authentification Windows.  
  
     > [!NOTE]
     > La configuration de <xref:Microsoft.SqlServer.Replication.Subscription.SynchronizationAgentProcessSecurity%2A> quand l’abonnement est créé par un membre du rôle serveur fixe **sysadmin** n’est pas nécessaire, mais elle est néanmoins recommandée. Dans ce cas, l'Agent va emprunter l'identité du compte de l'Agent SQL Server. Pour plus d’informations, consultez [Modèle de sécurité de l’agent de réplication](../../relational-databases/replication/security/replication-agent-security-model.md).  
  
   - (Facultatif) La valeur **true** (par défaut) de <xref:Microsoft.SqlServer.Replication.Subscription.CreateSyncAgentByDefault%2A> pour créer un travail de l'Agent qui permet de synchroniser l'abonnement. Si vous spécifiez **false**, l'abonnement peut uniquement être synchronisé par programme.  
  
   - (Facultatif) Définissez les champs <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SqlStandardLogin%2A> et <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SqlStandardPassword%2A> ou <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SecureSqlStandardPassword%2A> de <xref:Microsoft.SqlServer.Replication.Subscription.SubscriberSecurity%2A> lorsque vous utilisez l'authentification SQL Server pour vous connecter à l'abonné.  
  
   - (Facultatif) Définissez les champs <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SqlStandardLogin%2A> et <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SqlStandardPassword%2A> ou <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SecureSqlStandardPassword%2A> de <xref:Microsoft.SqlServer.Replication.PullSubscription.PublisherSecurity%2A> lorsque vous utilisez l'authentification SQL Server pour vous connecter au serveur de publication.  
  
8. Appelez la méthode <xref:Microsoft.SqlServer.Replication.Subscription.Create%2A> .  
  
> [!IMPORTANT]  
> Lors de la création d’un abonnement par émission de données sur un serveur de publication avec un serveur de distribution distant, les valeurs fournies pour toutes les propriétés, notamment <xref:Microsoft.SqlServer.Replication.Subscription.SynchronizationAgentProcessSecurity%2A>, sont envoyées au serveur de distribution en texte brut. Vous devez chiffrer la connexion entre le serveur de publication et son serveur de distribution distant avant d'appeler la méthode <xref:Microsoft.SqlServer.Replication.Subscription.Create%2A> . Pour plus d’informations, consultez [Activer des connexions chiffrées dans le moteur de base de données &#40;Gestionnaire de configuration SQL Server&#41;](../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md).  
  
###  <a name="examples-rmo"></a><a name="PShellExample"></a> Exemples (RMO)  
 Cet exemple crée un nouvel abonnement par envoi de données vers une publication transactionnelle. Les informations d’identification du compte Windows que vous utilisez pour exécuter le travail de l’Agent de distribution sont transmises lors de l’exécution.  
  
 [!code-cs[HowTo#rmo_CreateTranPushSub](../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_createtranpushsub)]  
  
 [!code-vb[HowTo#rmo_vb_CreateTranPushSub](../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_createtranpushsub)]  
  
 Cet exemple crée un nouvel abonnement par envoi de données vers une publication de fusion. Les informations d’identification du compte Windows que vous utilisez pour exécuter le travail de l’Agent de fusion sont transmises lors de l’exécution.  
  
 [!code-cs[HowTo#rmo_CreateMergePushSub](../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_createmergepushsub)]  
  
 [!code-vb[HowTo#rmo_vb_CreateMergePushSub](../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_createmergepushsub)]  
  
## <a name="see-also"></a>Voir aussi  
 [Afficher et modifier les propriétés d’un abonnement par émission de données](../../relational-databases/replication/view-and-modify-push-subscription-properties.md)   
 [Bonnes pratiques en matière de sécurité de la réplication](../../relational-databases/replication/security/replication-security-best-practices.md)   
 [Créer une publication](../../relational-databases/replication/publish/create-a-publication.md)   
 [Concepts liés aux objets RMO (Replication Management Object)](../../relational-databases/replication/concepts/replication-management-objects-concepts.md)   
 [Synchroniser un abonnement par émission de données](../../relational-databases/replication/synchronize-a-push-subscription.md)   
 [S’abonner à des publications](../../relational-databases/replication/subscribe-to-publications.md)   
 [Utiliser sqlcmd avec des variables de script](../../ssms/scripting/sqlcmd-use-with-scripting-variables.md)  
  
