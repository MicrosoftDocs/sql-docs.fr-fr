---
title: Créer un abonnement par extraction | Microsoft Docs
description: Découvrez comment créer une abonnement par extraction dans SQL Server à l’aide de SQL Server Management Studio, Transact-SQL ou Replication Management Objects.
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- pull subscriptions [SQL Server replication], creating
- merge replication subscribing [SQL Server replication], pull subscriptions
- subscriptions [SQL Server replication], pull
- snapshot replication [SQL Server], subscribing
- transactional replication, subscribing
ms.assetid: 41d1886d-59c9-41fc-9bd6-a59b40e0af6e
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 069c63b27714d9d0428b84846a19f2a31687c82e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347683"
---
# <a name="create-a-pull-subscription"></a>Créer un abonnement par extraction de données (pull)

<!--
2019-10-24 , GeneMi:
This article .md file exists in the so-called "2016+" section of repo 'sql-docs-pr'.
No article in 2016+ should ever have the moniker 'sql-server-2014' on its metadata line 'monikerRange:', I think.
Presently 'sql-server-2014' moniker is on this 'monikerRange'. This situation deserves further investigation.
-->

[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
  Cette rubrique explique comment créer un abonnement par extraction de données (pull) dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], de [!INCLUDE[tsql](../../includes/tsql-md.md)]ou d'objets RMO (Replication Management Objects).  
  
 La définition d'un abonnement par extraction de données (pull) pour la réplication P2P est possible au moyen d'un script, mais n'est pas disponible via l'assistant.  
 
  ##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
 Créer un abonnement par extraction de données sur le serveur de publication ou sur l'Abonné à l'aide de l'Assistant Nouvel abonnement. Exécutez les étapes de l'Assistant pour :  
  
-   Spécifier le serveur de publication et la publication.  
  
-   Sélectionner l'emplacement d'exécution des Agents de réplication. Pour un abonnement par extraction de données, sélectionnez **Exécuter chaque Agent sur son Abonné (abonnements par extraction de données (pull))** dans la page **Emplacement de l'Agent de distribution** ou dans la page **Emplacement de l'Agent de fusion** , selon le type de publication.  
  
-   Spécifiez des Abonnés et des bases de données d'abonnement.  
  
-   Spécifiez les noms de connexion et mots de passe utilisés pour les connexions établies par les Agents de réplication :  
  
    -   Pour les abonnements aux publications d'instantané et transactionnelles, spécifiez les informations d'identification dans la page **Sécurité de l'Agent de distribution** .  
  
    -   Pour les abonnements aux publications de fusion, spécifiez les informations d'identification dans la page **Sécurité de l'Agent de fusion** .  
  
     Pour plus d’informations sur les autorisations requises par chaque agent, consultez [Modèle de sécurité de l’Agent de réplication](../../relational-databases/replication/security/replication-agent-security-model.md).  
  
-   Spécifiez une planification de la synchronisation et le moment choisi pour initialiser l'Abonné.  
  
-   Spécifiez les autres options pour les publications de fusion : type d'abonnement, valeurs pour le filtrage paramétré, informations pour la synchronisation via HTTPS si la publication est activée pour la synchronisation Web.  
  
-   Spécifiez les autres options pour les publications transactionnelles qui autorisent la mise à jour des abonnements : si les Abonnés doivent valider immédiatement les modifications sur le serveur de publication ou les écrire dans une file d'attente ou encore les informations d'identification utilisées pour la connexion de l'Abonné au serveur de publication.  
  
-   Créez éventuellement un script pour l'abonnement.  
  
#### <a name="to-create-a-pull-subscription-from-the-publisher"></a>Pour créer un abonnement par extraction de données à partir du serveur de publication  
  
1.  Connectez-vous au serveur de publication dans [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], puis développez le nœud du serveur.  
  
2.  Développez le dossier **Réplication** , puis développez le dossier **Publications locales** .  
  
3.  Cliquez avec le bouton droit sur la publication pour laquelle vous souhaitez créer un ou plusieurs abonnements puis cliquez sur **Nouveaux abonnements**.  
  
4.  Exécutez les étapes de l'Assistant Nouvel abonnement.  

#### <a name="to-create-a-pull-subscription-from-the-subscriber"></a>Pour créer un abonnement par extraction de données à partir de l'Abonné  
  
1.  Connectez-vous à l'Abonné dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], puis développez le nœud du serveur.  
  
2.  Développez le dossier **Réplication** .  
  
3.  Cliquez avec le bouton droit sur le dossier **Abonnements locaux** , puis cliquez sur **Nouveaux abonnements**.  
  
4.  Dans la page **Publication** de l’Assistant Nouvel abonnement, sélectionnez **\<Find SQL Server Publisher>** ou **\<Find Oracle Publisher>** dans la liste déroulante **Serveur de publication**.  
  
5.  Connectez-vous au serveur de publication dans la boîte de dialogue **Se connecter au serveur** .  
  
6.  Sélectionnez une publication dans la page **Publication** .  
  
7.  Exécutez les étapes de l'Assistant Nouvel abonnement.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
 Les abonnements par extraction de données peuvent être créés par programme en utilisant des procédures stockées de réplication. Les procédures stockées utilisées dépendent du type de publication auquel l'abonnement appartient.  
  
#### <a name="to-create-a-pull-subscription-to-a-snapshot-or-transactional-publication"></a>Pour créer un abonnement par extraction vers une publication d'instantané ou transactionnelle  
  
1.  Sur le serveur de publication, vérifiez que la publication prend en charge les abonnements par extraction en exécutant [sp_helppublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helppublication-transact-sql.md).  
  
    -   Si la valeur de **allow_pull** dans le jeu de résultats est **1**, la publication prend en charge les abonnements par extraction de données.  
  
    -   Si la valeur de **allow_pull** est **0**, exécutez [sp_changepublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changepublication-transact-sql.md) en spécifiant **allow_pull** pour **\@property** et **true** pour **\@value**.  
  
2.  Sur l’Abonné, exécutez [sp_addpullsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addpullsubscription-transact-sql.md). Spécifiez **\@publisher** et **\@publication**. Pour plus d'informations sur la mise à jour des abonnements, consultez [Créer un abonnement pouvant être mis à jour pour une publication transactionnelle](publish/create-an-updatable-subscription-to-a-transactional-publication.md).   
  
3.  Sur l’Abonné, exécutez [sp_addpullsubscription_agent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addpullsubscription-agent-transact-sql.md). Spécifiez les éléments suivants :  
  
    -   Les paramètres **\@publisher**, **\@publisher_db** et **\@publication**.  
  
    -   Les informations d’identification [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows sous lesquelles l’Agent de distribution est exécuté sur l’Abonné pour **\@job_login** et **\@job_password**.  
  
        > [!NOTE]  
        >  Les connexions effectuées à l’aide de l’authentification intégrée Windows utilisent toujours les informations d’identification Windows spécifiées par **\@job_login** et **\@job_password**. L'Agent de distribution établit toujours la connexion locale à l'Abonné à l'aide de l'authentification intégrée Windows. Par défaut, l'Agent se connecte au serveur de distribution à l'aide de ces informations.  
  
    -   (Facultatif) La valeur **0** pour **\@distributor_security_mode** et les informations de connexion SQL Server pour **\@distributor_login** et **\@distributor_password**, si vous devez utiliser l’authentification SQL Server lors de la connexion au serveur de distribution.  
  
    -   Planification du travail de l'Agent de distribution pour cet abonnement. Pour plus d’informations, consultez [Spécifier des planifications de synchronisation](../../relational-databases/replication/specify-synchronization-schedules.md).  
  
4.  Sur le serveur de publication, exécutez [sp_addsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md) pour inscrire l’abonnement par extraction. Spécifiez **\@publication**, **\@subscriber** et **\@destination_db**. Affectez la valeur **pull** à **\@subscription_type**.  
  
#### <a name="to-create-a-pull-subscription-to-a-merge-publication"></a>Pour créer un abonnement par extraction de données (pull) vers une publication de fusion  
  
1.  Sur le serveur de publication, vérifiez que la publication prend en charge les abonnements par extraction en exécutant [sp_helpmergepublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpmergepublication-transact-sql.md).  
  
    -   Si la valeur de **allow_pull** dans le jeu de résultats est **1**, la publication prend en charge les abonnements par extraction de données.  
  
    -   Si la valeur de **allow_pull** est **0**, exécutez [sp_changemergepublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changemergepublication-transact-sql.md) en spécifiant **allow_pull** pour **\@property** et **true** pour **\@value**.  
  
2.  Sur l’Abonné, exécutez [sp_addmergepullsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergepullsubscription-transact-sql.md). Spécifiez **\@publisher**, **\@publisher_db**, **\@publication** et les paramètres suivants :  
  
    -   **\@subscriber_type** : spécifiez **local** pour un abonnement client et **global** pour un abonnement serveur.  
  
    -   **\@subscription_priority** : spécifiez la priorité de l’abonnement (de **0,00** à **99,99**). Ceci est requis uniquement pour un abonnement serveur.  
  
         Pour plus d’informations, consultez [Détection et résolution avancées des conflits de réplication de fusion](../../relational-databases/replication/merge/advanced-merge-replication-conflict-detection-and-resolution.md).  
  
3.  Sur l’Abonné, exécutez [sp_addmergepullsubscription_agent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergepullsubscription-agent-transact-sql.md). Spécifiez les paramètres suivants :  
  
    -   **\@publisher**, **\@publisher_db** et **\@publication**.  
  
    -   Informations d’identification Windows sous lesquelles l’Agent de fusion est exécuté sur l’Abonné pour **\@job_login** et **\@job_password**.  
  
        > [!NOTE]  
        >  Les connexions effectuées à l’aide de l’authentification intégrée Windows utilisent toujours les informations d’identification Windows spécifiées par **\@job_login** et **\@job_password**. L'Agent de fusion crée toujours la connexion locale à l'abonné à l'aide de l'authentification intégrée Windows. Par défaut, l'Agent se connecte au serveur de distribution et au serveur de publication à l'aide de ces informations.  
  
    -   (Facultatif) La valeur **0** pour **\@distributor_security_mode** et les informations de connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour **\@distributor_login** et **\@distributor_password**, si vous devez utiliser l’authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] lors de la connexion au serveur de distribution.  
  
    -   (Facultatif) La valeur **0** pour **\@publisher_security_mode** et les informations de connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour **\@publisher_login** et **\@publisher_password** si vous devez utiliser l’authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] lors de la connexion au serveur de publication.  
  
    -   Une planification du travail de l'Agent de fusion pour cet abonnement. Pour plus d'informations, voir [Créer un abonnement pouvant être mis à jour pour une publication transactionnelle](publish/create-an-updatable-subscription-to-a-transactional-publication.md).  
  
4.  Sur le serveur de publication, exécutez [sp_addmergesubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergesubscription-transact-sql.md). Spécifiez **\@publication**, **\@subscriber**, **\@subscriber_db** et la valeur **pull** pour **\@subscription_type**. L'abonnement par extraction est inscrit.  
  
###  <a name="examples-transact-sql"></a><a name="TsqlExample"></a> Exemples (Transact-SQL)  
 L'exemple suivant crée un abonnement par extraction de données vers une publication transactionnelle. Le premier traitement est exécuté sur l'abonné, et le second sur le serveur de publication. Les valeurs de connexion et le mot de passe sont fournis lors de l'exécution à l'aide des variables de script sqlcmd.  
  
```  
-- This script uses sqlcmd scripting variables. They are in the form  
-- $(MyVariable). For information about how to use scripting variables    
-- on the command line and in SQL Server Management Studio, see the   
-- "Executing Replication Scripts" section in the topic  
-- "Programming Replication Using System Stored Procedures".  
  
-- Execute this batch at the Subscriber.  
DECLARE @publication AS sysname;  
DECLARE @publisher AS sysname;  
DECLARE @publicationDB AS sysname;  
SET @publication = N'AdvWorksProductTran';  
SET @publisher = $(PubServer);  
SET @publicationDB = N'AdventureWorks';  
  
-- At the subscription database, create a pull subscription   
-- to a transactional publication.  
USE [AdventureWorksReplica]  
EXEC sp_addpullsubscription   
  @publisher = @publisher,   
  @publication = @publication,   
  @publisher_db = @publicationDB;  
  
-- Add an agent job to synchronize the pull subscription.  
EXEC sp_addpullsubscription_agent   
  @publisher = @publisher,   
  @publisher_db = @publicationDB,   
  @publication = @publication,   
  @distributor = @publisher,   
  @job_login = $(Login),   
  @job_password = $(Password);  
GO  
```  
  
```  
-- This script uses sqlcmd scripting variables. They are in the form  
-- $(MyVariable). For information about how to use scripting variables    
-- on the command line and in SQL Server Management Studio, see the   
-- "Executing Replication Scripts" section in the topic  
-- "Programming Replication Using System Stored Procedures".  
  
-- Execute this batch at the Publisher.  
DECLARE @publication AS sysname;  
DECLARE @subscriber AS sysname;  
DECLARE @subscriptionDB AS sysname;  
SET @publication = N'AdvWorksProductTran';  
SET @subscriber = $(SubServer);  
SET @subscriptionDB = N'AdventureWorksReplica';  
  
-- At the Publisher, register the subscription, using the defaults.  
EXEC sp_addsubscription   
  @publication = @publication,   
  @subscriber = @subscriber,   
  @destination_db = @subscriptionDB,   
  @subscription_type = N'pull',  
  @status = N'subscribed';  
GO  
  
```  
  
 L'exemple suivant crée un abonnement par extraction de données vers une publication de fusion. Le premier traitement est exécuté sur l'abonné, et le second sur le serveur de publication. Les valeurs de connexion et le mot de passe sont fournis lors de l'exécution à l'aide des variables de script **sqlcmd** .  
  
```  
-- This script uses sqlcmd scripting variables. They are in the form  
-- $(MyVariable). For information about how to use scripting variables    
-- on the command line and in SQL Server Management Studio, see the   
-- "Executing Replication Scripts" section in the topic  
-- "Programming Replication Using System Stored Procedures".  
  
-- Execute this batch at the Subscriber.  
DECLARE @publication AS sysname;  
DECLARE @publisher AS sysname;  
DECLARE @publicationDB AS sysname;  
DECLARE @hostname AS sysname;  
SET @publication = N'AdvWorksSalesOrdersMerge';  
SET @publisher = $(PubServer);  
SET @publicationDB = N'AdventureWorks';  
SET @hostname = N'adventure-works\david8';  
  
-- At the subscription database, create a pull subscription   
-- to a merge publication.  
USE [AdventureWorksReplica]  
EXEC sp_addmergepullsubscription   
  @publisher = @publisher,   
  @publication = @publication,   
  @publisher_db = @publicationDB;  
  
-- Add an agent job to synchronize the pull subscription.   
EXEC sp_addmergepullsubscription_agent   
  @publisher = @publisher,   
  @publisher_db = @publicationDB,   
  @publication = @publication,   
  @distributor = @publisher,   
  @job_login = $(Login),   
  @job_password = $(Password),  
  @hostname = @hostname;  
GO  
```  
  
```  
-- Execute this batch at the Publisher.  
DECLARE @myMergePub  AS sysname;  
DECLARE @mySub       AS sysname;  
DECLARE @mySubDB     AS sysname;  
  
SET @myMergePub = N'AdvWorksSalesOrdersMerge';  
SET @mySub = N'MYSUBSERVER';  
SET @mySubDB = N'AdventureWorksReplica';  
  
-- At the Publisher, register the subscription, using the defaults.  
USE [AdventureWorks]  
EXEC sp_addmergesubscription @publication = @myMergePub,   
@subscriber = @mySub, @subscriber_db = @mySubDB,   
@subscription_type = N'pull';  
GO  
```  
  
##  <a name="using-replication-management-objects-rmo"></a><a name="RMOProcedure"></a> Utilisation d'objets RMO (Replication Management Objects)  
 Les classes RMO utilisées pour créer un abonnement par extraction de données (pull) sont fonction du type de publication à laquelle l'abonnement appartient.  
  
#### <a name="to-create-a-pull-subscription-to-a-snapshot-or-transactional-publication"></a>Pour créer un abonnement par extraction vers une publication d'instantané ou transactionnelle  
  
1.  Créez une connexion à l'Abonné et au serveur de publication à l'aide de la classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Créez une instance de la classe <xref:Microsoft.SqlServer.Replication.TransPublication> en utilisant la connexion au serveur de publication de l'étape 1. Spécifiez <xref:Microsoft.SqlServer.Replication.Publication.Name%2A>, <xref:Microsoft.SqlServer.Replication.Publication.DatabaseName%2A> et <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A>.  
  
3.  Appelez la méthode <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> . Si cette méthode retourne **false**, les propriétés spécifiées à l'étape 2 sont incorrectes ou la publication n'existe pas sur le serveur.  
  
4.  Effectuez une opération AND logique au niveau du bit ( **&** dans Visual C# et **And** dans Visual Basic) entre la propriété <xref:Microsoft.SqlServer.Replication.Publication.Attributes%2A> et <xref:Microsoft.SqlServer.Replication.PublicationAttributes.AllowPull>. Si le résultat est <xref:Microsoft.SqlServer.Replication.PublicationAttributes.None>, appliquez à <xref:Microsoft.SqlServer.Replication.Publication.Attributes%2A> le résutat d'une opération OR logique au niveau du bit ( **|** dans Visual C# et **Or** dans Visual Basic) entre <xref:Microsoft.SqlServer.Replication.Publication.Attributes%2A> et <xref:Microsoft.SqlServer.Replication.PublicationAttributes.AllowPull>. Ensuite, appelez <xref:Microsoft.SqlServer.Replication.ReplicationObject.CommitPropertyChanges%2A> pour activer les abonnements par extraction de données (pull).  
  
5.  Si la base de données d'abonnements n'existe pas, créez-la en utilisant la classe <xref:Microsoft.SqlServer.Management.Smo.Database> . Pour plus d’informations, consultez [Création, modification et suppression de bases de données](../../relational-databases/server-management-objects-smo/tasks/creating-altering-and-removing-databases.md).  
  
6.  Créez une instance de la classe <xref:Microsoft.SqlServer.Replication.TransPullSubscription>.  
  
7.  Définissez les propriétés suivantes des abonnements :  
  
    -   La <xref:Microsoft.SqlServer.Management.Common.ServerConnection> à l'Abonné créée à l'étape 1 pour <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A>.  
  
    -   Nom de la base de données d'abonnements pour <xref:Microsoft.SqlServer.Replication.PullSubscription.DatabaseName%2A>.  
  
    -   Nom du serveur de publication pour <xref:Microsoft.SqlServer.Replication.PullSubscription.PublisherName%2A>.  
  
    -   Nom de la base de données de publication pour <xref:Microsoft.SqlServer.Replication.PullSubscription.PublicationDBName%2A>.  
  
    -   Nom de la publication pour <xref:Microsoft.SqlServer.Replication.PullSubscription.PublicationName%2A>.  
  
    -   Les paramètres <xref:Microsoft.SqlServer.Replication.IProcessSecurityContext.Login%2A> et <xref:Microsoft.SqlServer.Replication.IProcessSecurityContext.Password%2A> de <xref:Microsoft.SqlServer.Replication.PullSubscription.SynchronizationAgentProcessSecurity%2A> pour fournir les informations d'identification pour le compte [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows sous lequel l'Agent de distribution s'exécute sur l'Abonné. Ce compte est utilisé pour effectuer des connexions locales à l'Abonné et pour effectuer des connexions distantes au moyen de l'authentification Windows.  
  
        > [!NOTE]  
        >  La configuration de <xref:Microsoft.SqlServer.Replication.PullSubscription.SynchronizationAgentProcessSecurity%2A> lorsqu'un abonnement est créé par un membre du rôle serveur fixe **sysadmin** n'est pas nécessaire, mais elle est néanmoins recommandée. Dans ce cas, l'Agent va emprunter l'identité du compte de l'Agent SQL Server. Pour plus d’informations, voir [Replication Agent Security Model](../../relational-databases/replication/security/replication-agent-security-model.md).  
  
    -   (Facultatif) La valeur **true** pour <xref:Microsoft.SqlServer.Replication.PullSubscription.CreateSyncAgentByDefault%2A> afin de créer un travail d'agent qui sera utilisé pour synchroniser l'abonnement. Si vous spécifiez **false** (la valeur par défaut), l'abonnement peut uniquement être synchronisé par le biais de la programmation et vous devez spécifier des propriétés supplémentaires de <xref:Microsoft.SqlServer.Replication.TransSynchronizationAgent> lorsque vous accédez à cet objet à partir de la propriété <xref:Microsoft.SqlServer.Replication.TransPullSubscription.SynchronizationAgent%2A> . Pour plus d’informations, consultez [Synchroniser un abonnement par extraction (pull)](../../relational-databases/replication/synchronize-a-pull-subscription.md).  
  
        > [!NOTE]  
        >  SQL Server Agent n’est pas disponible dans toutes les éditions de [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour obtenir la liste des fonctionnalités prises en charge par les éditions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consultez [Fonctionnalités prise en charge par les éditions de SQL Server 2016](../../sql-server/editions-and-components-of-sql-server-2016.md). Lorsque vous spécifiez la valeur **true** pour les Abonnés Express, le travail d'agent n'est pas créé. Toutefois, certaines métadonnées importantes liées à l'abonnement sont stockées sur l'Abonné.  
  
    -   (Facultatif) Définissez les champs <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SqlStandardLogin%2A> et <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SqlStandardPassword%2A> ou <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SecureSqlStandardPassword%2A> de <xref:Microsoft.SqlServer.Replication.PullSubscription.DistributorSecurity%2A> lorsque vous utilisez l'authentification SQL Server pour vous connecter au serveur de distribution.  
  
8.  Appelez la méthode <xref:Microsoft.SqlServer.Replication.PullSubscription.Create%2A> .  
  
9. Au moyen de l'instance de la classe <xref:Microsoft.SqlServer.Replication.TransPublication> de l'étape 2, appelez la méthode <xref:Microsoft.SqlServer.Replication.TransPublication.MakePullSubscriptionWellKnown%2A> pour inscrire l'abonnement par extraction de données (pull) sur le serveur de publication. Si l'abonnement est déjà inscrit dans le Registre, une erreur se produit.  
  
#### <a name="to-create-a-pull-subscription-to-a-merge-publication"></a>Pour créer un abonnement par extraction de données (pull) vers une publication de fusion  
  
1.  Créez des connexions à la fois à l'Abonné et au serveur de publication au moyen de la classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Créez une instance de la classe <xref:Microsoft.SqlServer.Replication.MergePublication> en utilisant la connexion au serveur de publication de l'étape 1. Spécifiez <xref:Microsoft.SqlServer.Replication.Publication.Name%2A>, <xref:Microsoft.SqlServer.Replication.Publication.DatabaseName%2A>et <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A>.  
  
3.  Appelez la méthode <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> . Si cette méthode retourne **false**, les propriétés spécifiées à l'étape 2 sont incorrectes ou la publication n'existe pas sur le serveur.  
  
4.  Effectuez une opération AND logique au niveau du bit ( **&** dans Visual C# et **And** dans Visual Basic) entre la propriété <xref:Microsoft.SqlServer.Replication.Publication.Attributes%2A> et <xref:Microsoft.SqlServer.Replication.PublicationAttributes.AllowPull>. Si le résultat est <xref:Microsoft.SqlServer.Replication.PublicationAttributes.None>, appliquez à <xref:Microsoft.SqlServer.Replication.Publication.Attributes%2A> le résutat d'une opération OR logique au niveau du bit ( **|** dans Visual C# et **Or** dans Visual Basic) entre <xref:Microsoft.SqlServer.Replication.Publication.Attributes%2A> et <xref:Microsoft.SqlServer.Replication.PublicationAttributes.AllowPull>. Ensuite, appelez <xref:Microsoft.SqlServer.Replication.ReplicationObject.CommitPropertyChanges%2A> pour activer les abonnements par extraction de données (pull).  
  
5.  Si la base de données d'abonnements n'existe pas, créez-la en utilisant la classe <xref:Microsoft.SqlServer.Management.Smo.Database> . Pour plus d’informations, consultez [Création, modification et suppression de bases de données](../../relational-databases/server-management-objects-smo/tasks/creating-altering-and-removing-databases.md).  
  
6.  Créez une instance de la classe <xref:Microsoft.SqlServer.Replication.MergePullSubscription>.  
  
7.  Définissez les propriétés suivantes des abonnements :  
  
    -   La <xref:Microsoft.SqlServer.Management.Common.ServerConnection> à l'Abonné créée à l'étape 1 pour <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A>.  
  
    -   Nom de la base de données d'abonnements pour <xref:Microsoft.SqlServer.Replication.PullSubscription.DatabaseName%2A>.  
  
    -   Nom du serveur de publication pour <xref:Microsoft.SqlServer.Replication.PullSubscription.PublisherName%2A>.  
  
    -   Nom de la base de données de publication pour <xref:Microsoft.SqlServer.Replication.PullSubscription.PublicationDBName%2A>.  
  
    -   Nom de la publication pour <xref:Microsoft.SqlServer.Replication.PullSubscription.PublicationName%2A>.  
  
    -   Les paramètres <xref:Microsoft.SqlServer.Replication.IProcessSecurityContext.Login%2A> et <xref:Microsoft.SqlServer.Replication.IProcessSecurityContext.Password%2A> de <xref:Microsoft.SqlServer.Replication.PullSubscription.SynchronizationAgentProcessSecurity%2A> pour fournir les informations d'identification pour le compte [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows sous lequel l'Agent de fusion s'exécute sur l'Abonné. Ce compte est utilisé pour effectuer des connexions locales à l'Abonné et pour effectuer des connexions distantes au moyen de l'authentification Windows.  
  
        > [!NOTE]  
        >  La configuration de <xref:Microsoft.SqlServer.Replication.PullSubscription.SynchronizationAgentProcessSecurity%2A> lorsqu'un abonnement est créé par un membre du rôle serveur fixe **sysadmin** n'est pas nécessaire, mais elle est néanmoins recommandée. Dans ce cas, l'Agent va emprunter l'identité du compte de l'Agent SQL Server. Pour plus d’informations, voir [Replication Agent Security Model](../../relational-databases/replication/security/replication-agent-security-model.md).  
  
    -   (Facultatif) La valeur **true** pour <xref:Microsoft.SqlServer.Replication.PullSubscription.CreateSyncAgentByDefault%2A> afin de créer un travail d'agent qui sera utilisé pour synchroniser l'abonnement. Si vous spécifiez **false** (la valeur par défaut), l'abonnement peut uniquement être synchronisé par le biais de la programmation et vous devez spécifier des propriétés supplémentaires de <xref:Microsoft.SqlServer.Replication.MergeSynchronizationAgent> lorsque vous accédez à cet objet à partir de la propriété <xref:Microsoft.SqlServer.Replication.MergePullSubscription.SynchronizationAgent%2A> . Pour plus d’informations, consultez [Synchroniser un abonnement par extraction (pull)](../../relational-databases/replication/synchronize-a-pull-subscription.md).  
  
    -   (Facultatif) Définissez les champs <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SqlStandardLogin%2A> et <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SqlStandardPassword%2A> ou <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SecureSqlStandardPassword%2A> de <xref:Microsoft.SqlServer.Replication.PullSubscription.DistributorSecurity%2A> lorsque vous utilisez l'authentification SQL Server pour vous connecter au serveur de distribution.  
  
    -   (Facultatif) Définissez les champs <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SqlStandardLogin%2A> et <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SqlStandardPassword%2A> ou <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SecureSqlStandardPassword%2A> de <xref:Microsoft.SqlServer.Replication.PullSubscription.PublisherSecurity%2A> lorsque vous utilisez l'authentification SQL Server pour vous connecter au serveur de publication.  
  
8.  Appelez la méthode <xref:Microsoft.SqlServer.Replication.PullSubscription.Create%2A> .  
  
9. Au moyen de l'instance de la classe <xref:Microsoft.SqlServer.Replication.MergePublication> de l'étape 2, appelez la méthode <xref:Microsoft.SqlServer.Replication.MergePublication.MakePullSubscriptionWellKnown%2A> pour inscrire l'abonnement par extraction de données (pull) sur le serveur de publication. Si l'abonnement est déjà inscrit dans le Registre, une erreur se produit.  
  
###  <a name="example-rmo"></a><a name="PShellExample"></a> Exemple (RMO)  
 Cet exemple crée un abonnement par extraction de données (pull) vers une publication transactionnelle. Les informations d'identification de compte [!INCLUDE[msCoName](../../includes/msconame-md.md)] utilisées pour créer le travail d'agent de distribution sont transmises au moment de l'exécution.  
  
```csharp  
// Define the Publisher, publication, and databases.  
string publicationName = "AdvWorksProductTran";  
string publisherName = publisherInstance;  
string subscriberName = subscriberInstance;  
string subscriptionDbName = "AdventureWorksReplica";  
string publicationDbName = "AdventureWorks";  
  
//Create connections to the Publisher and Subscriber.  
ServerConnection subscriberConn = new ServerConnection(subscriberName);  
ServerConnection publisherConn = new ServerConnection(publisherName);  
  
// Create the objects that we need.  
TransPublication publication;  
TransPullSubscription subscription;  
  
try  
{  
    // Connect to the Publisher and Subscriber.  
    subscriberConn.Connect();  
    publisherConn.Connect();  
  
    // Ensure that the publication exists and that   
    // it supports pull subscriptions.  
    publication = new TransPublication();  
    publication.Name = publicationName;  
    publication.DatabaseName = publicationDbName;  
    publication.ConnectionContext = publisherConn;  
  
    if (publication.IsExistingObject)  
    {  
        if ((publication.Attributes & PublicationAttributes.AllowPull) == 0)  
        {  
            publication.Attributes |= PublicationAttributes.AllowPull;  
        }  
  
        // Define the pull subscription.  
        subscription = new TransPullSubscription();  
        subscription.ConnectionContext = subscriberConn;  
        subscription.PublisherName = publisherName;  
        subscription.PublicationName = publicationName;  
        subscription.PublicationDBName = publicationDbName;  
        subscription.DatabaseName = subscriptionDbName;  
  
        // Specify the Windows login credentials for the Distribution Agent job.  
        subscription.SynchronizationAgentProcessSecurity.Login = winLogin;  
        subscription.SynchronizationAgentProcessSecurity.Password = winPassword;  
  
        // Make sure that the agent job for the subscription is created.  
        subscription.CreateSyncAgentByDefault = true;  
  
        // By default, subscriptions to transactional publications are synchronized   
        // continuously, but in this case we only want to synchronize on demand.  
        subscription.AgentSchedule.FrequencyType = ScheduleFrequencyType.OnDemand;  
  
        // Create the pull subscription at the Subscriber.  
        subscription.Create();  
  
        Boolean registered = false;  
  
        // Verify that the subscription is not already registered.  
        foreach (TransSubscription existing  
            in publication.EnumSubscriptions())  
        {  
            if (existing.SubscriberName == subscriberName  
                && existing.SubscriptionDBName == subscriptionDbName)  
            {  
                registered = true;  
            }  
        }  
        if (!registered)  
        {  
            // Register the subscription with the Publisher.  
            publication.MakePullSubscriptionWellKnown(  
                subscriberName, subscriptionDbName,  
                SubscriptionSyncType.Automatic,  
                TransSubscriberType.ReadOnly);  
        }  
    }  
    else  
    {  
        // Do something here if the publication does not exist.  
        throw new ApplicationException(String.Format(  
            "The publication '{0}' does not exist on {1}.",  
            publicationName, publisherName));  
    }  
}  
catch (Exception ex)  
{  
    // Implement the appropriate error handling here.  
    throw new ApplicationException(String.Format(  
        "The subscription to {0} could not be created.", publicationName), ex);  
}  
finally  
{  
    subscriberConn.Disconnect();  
    publisherConn.Disconnect();  
}  
  
```  
  
```vb  
' Define the Publisher, publication, and databases.  
Dim publicationName As String = "AdvWorksProductTran"  
Dim publisherName As String = publisherInstance  
Dim subscriberName As String = subscriberInstance  
Dim subscriptionDbName As String = "AdventureWorksReplica"  
Dim publicationDbName As String = "AdventureWorks"  
  
'Create connections to the Publisher and Subscriber.  
Dim subscriberConn As ServerConnection = New ServerConnection(subscriberName)  
Dim publisherConn As ServerConnection = New ServerConnection(publisherName)  
  
' Create the objects that we need.  
Dim publication As TransPublication  
Dim subscription As TransPullSubscription  
  
Try  
    ' Connect to the Publisher and Subscriber.  
    subscriberConn.Connect()  
    publisherConn.Connect()  
  
    ' Ensure that the publication exists and that   
    ' it supports pull subscriptions.  
    publication = New TransPublication()  
    publication.Name = publicationName  
    publication.DatabaseName = publicationDbName  
    publication.ConnectionContext = publisherConn  
  
    If publication.IsExistingObject Then  
        If (publication.Attributes And PublicationAttributes.AllowPull) = 0 Then  
            publication.Attributes = publication.Attributes _  
            Or PublicationAttributes.AllowPull  
        End If  
  
        ' Define the pull subscription.  
        subscription = New TransPullSubscription()  
        subscription.ConnectionContext = subscriberConn  
        subscription.PublisherName = publisherName  
        subscription.PublicationName = publicationName  
        subscription.PublicationDBName = publicationDbName  
        subscription.DatabaseName = subscriptionDbName  
        subscription.Description = "Pull subscription to " + publicationDbName _  
        + " on " + subscriberName + "."  
  
        ' Specify the Windows login credentials for the Distribution Agent job.  
        subscription.SynchronizationAgentProcessSecurity.Login = winLogin  
        subscription.SynchronizationAgentProcessSecurity.Password = winPassword  
  
        ' Make sure that the agent job for the subscription is created.  
        subscription.CreateSyncAgentByDefault = True  
  
        ' By default, subscriptions to transactional publications are synchronized   
        ' continuously, but in this case we only want to synchronize on demand.  
        subscription.AgentSchedule.FrequencyType = ScheduleFrequencyType.OnDemand  
  
        ' Create the pull subscription at the Subscriber.  
        subscription.Create()  
  
        Dim registered As Boolean = False  
  
        ' Verify that the subscription is not already registered.  
        For Each existing As TransSubscription In publication.EnumSubscriptions()  
            If existing.SubscriberName = subscriberName And _  
                existing.SubscriptionDBName = subscriptionDbName Then  
                registered = True  
            End If  
        Next existing  
        If Not registered Then  
            ' Register the subscription with the Publisher.  
            publication.MakePullSubscriptionWellKnown( _  
             subscriberName, subscriptionDbName, _  
             SubscriptionSyncType.Automatic, _  
             TransSubscriberType.ReadOnly)  
        End If  
    Else  
        ' Do something here if the publication does not exist.  
        Throw New ApplicationException(String.Format( _  
         "The publication '{0}' does not exist on {1}.", _  
         publicationName, publisherName))  
    End If  
Catch ex As Exception  
    ' Implement the appropriate error handling here.  
    Throw New ApplicationException(String.Format( _  
        "The subscription to {0} could not be created.", publicationName), ex)  
Finally  
    subscriberConn.Disconnect()  
    publisherConn.Disconnect()  
End Try  
  
```  
  
 Cet exemple crée un abonnement par extraction de données (pull) vers une publication de fusion. Les informations d'identification de compte Windows utilisées pour créer le travail d'agent de fusion sont transmises au moment de l'exécution.  
  
```cpp#  
// Define the Publisher, publication, and databases.  
string publicationName = "AdvWorksSalesOrdersMerge";  
string publisherName = publisherInstance;  
string subscriberName = subscriberInstance;  
string subscriptionDbName = "AdventureWorksReplica";  
string publicationDbName = "AdventureWorks";  
string hostname = @"adventure-works\garrett1";  
  
//Create connections to the Publisher and Subscriber.  
ServerConnection subscriberConn = new ServerConnection(subscriberName);  
ServerConnection publisherConn = new ServerConnection(publisherName);  
  
// Create the objects that we need.  
MergePublication publication;  
MergePullSubscription subscription;  
  
try  
{  
    // Connect to the Subscriber.  
    subscriberConn.Connect();  
  
    // Ensure that the publication exists and that   
    // it supports pull subscriptions.  
    publication = new MergePublication();  
    publication.Name = publicationName;  
    publication.DatabaseName = publicationDbName;  
    publication.ConnectionContext = publisherConn;  
  
    if (publication.LoadProperties())  
    {  
        if ((publication.Attributes & PublicationAttributes.AllowPull) == 0)  
        {  
            publication.Attributes |= PublicationAttributes.AllowPull;  
        }  
  
        // Define the pull subscription.  
        subscription = new MergePullSubscription();  
        subscription.ConnectionContext = subscriberConn;  
        subscription.PublisherName = publisherName;  
        subscription.PublicationName = publicationName;  
        subscription.PublicationDBName = publicationDbName;  
        subscription.DatabaseName = subscriptionDbName;  
        subscription.HostName = hostname;  
  
        // Specify the Windows login credentials for the Merge Agent job.  
        subscription.SynchronizationAgentProcessSecurity.Login = winLogin;  
        subscription.SynchronizationAgentProcessSecurity.Password = winPassword;  
  
        // Make sure that the agent job for the subscription is created.  
        subscription.CreateSyncAgentByDefault = true;  
  
        // Create the pull subscription at the Subscriber.  
        subscription.Create();  
  
        Boolean registered = false;  
  
        // Verify that the subscription is not already registered.  
        foreach (MergeSubscription existing  
            in publication.EnumSubscriptions())  
        {  
            if (existing.SubscriberName == subscriberName  
                && existing.SubscriptionDBName == subscriptionDbName  
                && existing.SubscriptionType == SubscriptionOption.Pull)  
            {  
                registered = true;  
            }  
        }  
        if (!registered)  
        {  
            // Register the local subscription with the Publisher.  
            publication.MakePullSubscriptionWellKnown(  
                subscriberName, subscriptionDbName,  
                SubscriptionSyncType.Automatic,  
                MergeSubscriberType.Local, 0);  
        }  
    }  
    else  
    {  
        // Do something here if the publication does not exist.  
        throw new ApplicationException(String.Format(  
            "The publication '{0}' does not exist on {1}.",  
            publicationName, publisherName));  
    }  
}  
catch (Exception ex)  
{  
    // Implement the appropriate error handling here.  
    throw new ApplicationException(String.Format(  
        "The subscription to {0} could not be created.", publicationName), ex);  
}  
finally  
{  
    subscriberConn.Disconnect();  
    publisherConn.Disconnect();  
}  
  
```  
  
```vb  
' Define the Publisher, publication, and databases.  
Dim publicationName As String = "AdvWorksSalesOrdersMerge"  
Dim publisherName As String = publisherInstance  
Dim subscriberName As String = subscriberInstance  
Dim subscriptionDbName As String = "AdventureWorksReplica"  
Dim publicationDbName As String = "AdventureWorks"  
Dim hostname As String = "adventure-works\garrett1"  
  
'Create connections to the Publisher and Subscriber.  
Dim subscriberConn As ServerConnection = New ServerConnection(subscriberName)  
Dim publisherConn As ServerConnection = New ServerConnection(publisherName)  
  
' Create the objects that we need.  
Dim publication As MergePublication  
Dim subscription As MergePullSubscription  
  
Try  
    ' Connect to the Subscriber.  
    subscriberConn.Connect()  
  
    ' Ensure that the publication exists and that   
    ' it supports pull subscriptions.  
    publication = New MergePublication()  
    publication.Name = publicationName  
    publication.DatabaseName = publicationDbName  
    publication.ConnectionContext = publisherConn  
  
    If publication.LoadProperties() Then  
        If (publication.Attributes And PublicationAttributes.AllowPull) = 0 Then  
            publication.Attributes = publication.Attributes _  
            Or PublicationAttributes.AllowPull  
        End If  
  
        ' Define the pull subscription.  
        subscription = New MergePullSubscription()  
        subscription.ConnectionContext = subscriberConn  
        subscription.PublisherName = publisherName  
        subscription.PublicationName = publicationName  
        subscription.PublicationDBName = publicationDbName  
        subscription.DatabaseName = subscriptionDbName  
        subscription.HostName = hostname  
  
        ' Specify the Windows login credentials for the Merge Agent job.  
        subscription.SynchronizationAgentProcessSecurity.Login = winLogin  
        subscription.SynchronizationAgentProcessSecurity.Password = winPassword  
  
        ' Make sure that the agent job for the subscription is created.  
        subscription.CreateSyncAgentByDefault = True  
  
        ' Create the pull subscription at the Subscriber.  
        subscription.Create()  
  
        Dim registered As Boolean = False  
  
        ' Verify that the subscription is not already registered.  
        For Each existing As MergeSubscription In _  
        publication.EnumSubscriptions()  
            If existing.SubscriberName = subscriberName Then  
                registered = True  
            End If  
        Next  
        If Not registered Then  
            ' Register the local subscription with the Publisher.  
            publication.MakePullSubscriptionWellKnown( _  
             subscriberName, subscriptionDbName, _  
             SubscriptionSyncType.Automatic, _  
             MergeSubscriberType.Local, 0)  
        End If  
    Else  
        ' Do something here if the publication does not exist.  
        Throw New ApplicationException(String.Format( _  
         "The publication '{0}' does not exist on {1}.", _  
         publicationName, publisherName))  
    End If  
Catch ex As Exception  
    ' Implement the appropriate error handling here.  
    Throw New ApplicationException(String.Format( _  
        "The subscription to {0} could not be created.", publicationName), ex)  
Finally  
    subscriberConn.Disconnect()  
    publisherConn.Disconnect()  
End Try  
```  
  
 Cet exemple crée un abonnement par extraction de données (pull) vers une publication de fusion sans créer de travail d'agent et de métadonnées d'abonnement associés dans [MSsubscription_properties](../../relational-databases/system-tables/mssubscription-properties-transact-sql.md). Les informations d'identification de compte Windows utilisées pour créer le travail d'agent de fusion sont transmises au moment de l'exécution.  
  
```csharp  
// Define the Publisher, publication, and databases.  
string publicationName = "AdvWorksSalesOrdersMerge";  
string publisherName = publisherInstance;  
string subscriberName = subscriberInstance;  
string subscriptionDbName = "AdventureWorksReplica";  
string publicationDbName = "AdventureWorks";  
  
//Create connections to the Publisher and Subscriber.  
ServerConnection subscriberConn = new ServerConnection(subscriberName);  
ServerConnection publisherConn = new ServerConnection(publisherName);  
  
// Create the objects that we need.  
MergePublication publication;  
MergePullSubscription subscription;  
  
try  
{  
    // Connect to the Subscriber.  
    subscriberConn.Connect();  
  
    // Ensure that the publication exists and that   
    // it supports pull subscriptions.  
    publication = new MergePublication();  
    publication.Name = publicationName;  
    publication.DatabaseName = publicationDbName;  
    publication.ConnectionContext = publisherConn;  
  
    if (publication.LoadProperties())  
    {  
        if ((publication.Attributes & PublicationAttributes.AllowPull) == 0)  
        {  
            publication.Attributes |= PublicationAttributes.AllowPull;  
        }  
  
        // Define the pull subscription.  
        subscription = new MergePullSubscription();  
        subscription.ConnectionContext = subscriberConn;  
        subscription.PublisherName = publisherName;  
        subscription.PublicationName = publicationName;  
        subscription.PublicationDBName = publicationDbName;  
        subscription.DatabaseName = subscriptionDbName;  
  
        // Specify that an agent job not be created for this subscription. The  
        // subscription can only be synchronized by running the Merge Agent directly.  
        // Subscripition metadata stored in MSsubscription_properties will not  
        // be available and must be specified at run time.  
        subscription.CreateSyncAgentByDefault = false;  
  
        // Create the pull subscription at the Subscriber.  
        subscription.Create();  
  
        Boolean registered = false;  
  
        // Verify that the subscription is not already registered.  
        foreach (MergeSubscription existing  
            in publication.EnumSubscriptions())  
        {  
            if (existing.SubscriberName == subscriberName  
                && existing.SubscriptionDBName == subscriptionDbName  
                && existing.SubscriptionType == SubscriptionOption.Pull)  
            {  
                registered = true;  
            }  
        }  
        if (!registered)  
        {  
            // Register the local subscription with the Publisher.  
            publication.MakePullSubscriptionWellKnown(  
                subscriberName, subscriptionDbName,  
                SubscriptionSyncType.Automatic,  
                MergeSubscriberType.Local, 0);  
        }  
    }  
    else  
    {  
        // Do something here if the publication does not exist.  
        throw new ApplicationException(String.Format(  
            "The publication '{0}' does not exist on {1}.",  
            publicationName, publisherName));  
    }  
}  
catch (Exception ex)  
{  
    // Implement the appropriate error handling here.  
    throw new ApplicationException(String.Format(  
        "The subscription to {0} could not be created.", publicationName), ex);  
}  
finally  
{  
    subscriberConn.Disconnect();  
    publisherConn.Disconnect();  
}  
```  
  
```vb  
' Define the Publisher, publication, and databases.  
Dim publicationName As String = "AdvWorksSalesOrdersMerge"  
Dim publisherName As String = publisherInstance  
Dim subscriberName As String = subscriberInstance  
Dim subscriptionDbName As String = "AdventureWorksReplica"  
Dim publicationDbName As String = "AdventureWorks"  
  
'Create connections to the Publisher and Subscriber.  
Dim subscriberConn As ServerConnection = New ServerConnection(subscriberName)  
Dim publisherConn As ServerConnection = New ServerConnection(publisherName)  
  
' Create the objects that we need.  
Dim publication As MergePublication  
Dim subscription As MergePullSubscription  
  
Try  
    ' Connect to the Subscriber.  
    subscriberConn.Connect()  
  
    ' Ensure that the publication exists and that   
    ' it supports pull subscriptions.  
    publication = New MergePublication()  
    publication.Name = publicationName  
    publication.DatabaseName = publicationDbName  
    publication.ConnectionContext = publisherConn  
  
    If publication.LoadProperties() Then  
        If (publication.Attributes And PublicationAttributes.AllowPull) = 0 Then  
            publication.Attributes = publication.Attributes _  
            Or PublicationAttributes.AllowPull  
        End If  
  
        ' Define the pull subscription.  
        subscription = New MergePullSubscription()  
        subscription.ConnectionContext = subscriberConn  
        subscription.PublisherName = publisherName  
        subscription.PublicationName = publicationName  
        subscription.PublicationDBName = publicationDbName  
        subscription.DatabaseName = subscriptionDbName  
  
        ' Specify that an agent job not be created for this subscription. The  
        ' subscription can only be synchronized by running the Merge Agent directly.  
        ' Subscripition metadata stored in MSsubscription_properties will not  
        ' be available and must be specified at run time.  
        subscription.CreateSyncAgentByDefault = False  
  
        ' Create the pull subscription at the Subscriber.  
        subscription.Create()  
  
        Dim registered As Boolean = False  
  
        ' Verify that the subscription is not already registered.  
        For Each existing As MergeSubscription In _  
        publication.EnumSubscriptions()  
            If existing.SubscriberName = subscriberName Then  
                registered = True  
            End If  
        Next  
        If Not registered Then  
            ' Register the local subscription with the Publisher.  
            publication.MakePullSubscriptionWellKnown( _  
             subscriberName, subscriptionDbName, _  
             SubscriptionSyncType.Automatic, _  
             MergeSubscriberType.Local, 0)  
        End If  
    Else  
        ' Do something here if the publication does not exist.  
        Throw New ApplicationException(String.Format( _  
         "The publication '{0}' does not exist on {1}.", _  
         publicationName, publisherName))  
    End If  
Catch ex As Exception  
    ' Implement the appropriate error handling here.  
    Throw New ApplicationException(String.Format( _  
     "The subscription to {0} could not be created.", publicationName), ex)  
Finally  
    subscriberConn.Disconnect()  
    publisherConn.Disconnect()  
End Try  
```  
  
 Cet exemple crée un abonnement par envoi de données (pull) vers une publication de fusion qui peut être synchronisée via Internet au moyen de la synchronisation Web. Les informations d'identification de compte Windows utilisées pour créer le travail d'agent de fusion sont transmises au moment de l'exécution. Pour plus d’informations, consultez [Configurer la synchronisation Web](../../relational-databases/replication/configure-web-synchronization.md).  
  
```csharp  
// Define the Publisher, publication, and databases.  
string publicationName = "AdvWorksSalesOrdersMerge";  
string publisherName = publisherInstance;  
string subscriberName = subscriberInstance;  
string subscriptionDbName = "AdventureWorksReplica";  
string publicationDbName = "AdventureWorks";  
string hostname = @"adventure-works\garrett1";  
string webSyncUrl = "https://" + publisherInstance + "/WebSync/replisapi.dll";  
  
//Create connections to the Publisher and Subscriber.  
ServerConnection subscriberConn = new ServerConnection(subscriberName);  
ServerConnection publisherConn = new ServerConnection(publisherName);  
  
// Create the objects that we need.  
MergePublication publication;  
MergePullSubscription subscription;  
  
try  
{  
    // Connect to the Subscriber.  
    subscriberConn.Connect();  
  
    // Ensure that the publication exists and that   
    // it supports pull subscriptions and Web synchronization.  
    publication = new MergePublication();  
    publication.Name = publicationName;  
    publication.DatabaseName = publicationDbName;  
    publication.ConnectionContext = publisherConn;  
  
    if (publication.LoadProperties())  
    {  
        if ((publication.Attributes & PublicationAttributes.AllowPull) == 0)  
        {  
            publication.Attributes |= PublicationAttributes.AllowPull;  
        }  
        if ((publication.Attributes & PublicationAttributes.AllowWebSynchronization) == 0)  
        {  
            publication.Attributes |= PublicationAttributes.AllowWebSynchronization;  
        }  
  
        // Define the pull subscription.  
        subscription = new MergePullSubscription();  
        subscription.ConnectionContext = subscriberConn;  
        subscription.PublisherName = publisherName;  
        subscription.PublicationName = publicationName;  
        subscription.PublicationDBName = publicationDbName;  
        subscription.DatabaseName = subscriptionDbName;  
        subscription.HostName = hostname;  
  
        // Specify the Windows login credentials for the Merge Agent job.  
        subscription.SynchronizationAgentProcessSecurity.Login = winLogin;  
        subscription.SynchronizationAgentProcessSecurity.Password = winPassword;  
  
        // Enable Web synchronization.  
        subscription.UseWebSynchronization = true;  
        subscription.InternetUrl = webSyncUrl;  
  
        // Specify the same Windows credentials to use when connecting to the  
        // Web server using HTTPS Basic Authentication.  
        subscription.InternetSecurityMode = AuthenticationMethod.BasicAuthentication;  
        subscription.InternetLogin = winLogin;  
        subscription.InternetPassword = winPassword;  
  
        // Ensure that we create a job for this subscription.  
        subscription.CreateSyncAgentByDefault = true;  
  
        // Create the pull subscription at the Subscriber.  
        subscription.Create();  
  
        Boolean registered = false;  
  
        // Verify that the subscription is not already registered.  
        foreach (MergeSubscription existing  
            in publication.EnumSubscriptions())  
        {  
            if (existing.SubscriberName == subscriberName  
                && existing.SubscriptionDBName == subscriptionDbName  
                && existing.SubscriptionType == SubscriptionOption.Pull)  
            {  
                registered = true;  
            }  
        }  
        if (!registered)  
        {  
            // Register the local subscription with the Publisher.  
            publication.MakePullSubscriptionWellKnown(  
                subscriberName, subscriptionDbName,  
                SubscriptionSyncType.Automatic,  
                MergeSubscriberType.Local, 0);  
        }  
    }  
    else  
    {  
        // Do something here if the publication does not exist.  
        throw new ApplicationException(String.Format(  
            "The publication '{0}' does not exist on {1}.",  
            publicationName, publisherName));  
    }  
}  
catch (Exception ex)  
{  
    // Implement the appropriate error handling here.  
    throw new ApplicationException(String.Format(  
        "The subscription to {0} could not be created.", publicationName), ex);  
}  
finally  
{  
    subscriberConn.Disconnect();  
    publisherConn.Disconnect();  
}  
```  
  
```vb  
' Define the Publisher, publication, and databases.  
Dim publicationName As String = "AdvWorksSalesOrdersMerge"  
Dim publisherName As String = publisherInstance  
Dim subscriberName As String = subscriberInstance  
Dim subscriptionDbName As String = "AdventureWorksReplica"  
Dim publicationDbName As String = "AdventureWorks"  
Dim hostname As String = "adventure-works\garrett1"  
Dim webSyncUrl As String = "https://" + publisherInstance + "/WebSync/replisapi.dll"  
  
'Create connections to the Publisher and Subscriber.  
Dim subscriberConn As ServerConnection = New ServerConnection(subscriberName)  
Dim publisherConn As ServerConnection = New ServerConnection(publisherName)  
  
' Create the objects that we need.  
Dim publication As MergePublication  
Dim subscription As MergePullSubscription  
  
Try  
    ' Connect to the Subscriber.  
    subscriberConn.Connect()  
  
    ' Ensure that the publication exists and that   
    ' it supports pull subscriptions and Web synchronization.  
    publication = New MergePublication()  
    publication.Name = publicationName  
    publication.DatabaseName = publicationDbName  
    publication.ConnectionContext = publisherConn  
  
    If publication.LoadProperties() Then  
        If (publication.Attributes And PublicationAttributes.AllowPull) = 0 Then  
            publication.Attributes = publication.Attributes _  
            Or PublicationAttributes.AllowPull  
        End If  
        If (publication.Attributes And PublicationAttributes.AllowWebSynchronization) = 0 Then  
            publication.Attributes = publication.Attributes _  
            Or PublicationAttributes.AllowWebSynchronization  
        End If  
  
        ' Define the pull subscription.  
        subscription = New MergePullSubscription()  
        subscription.ConnectionContext = subscriberConn  
        subscription.PublisherName = publisherName  
        subscription.PublicationName = publicationName  
        subscription.PublicationDBName = publicationDbName  
        subscription.DatabaseName = subscriptionDbName  
        subscription.HostName = hostname  
        subscription.CreateSyncAgentByDefault = True  
  
        ' Specify the Windows login credentials for the Merge Agent job.  
        subscription.SynchronizationAgentProcessSecurity.Login = winLogin  
        subscription.SynchronizationAgentProcessSecurity.Password = winPassword  
  
        ' Enable Web synchronization.  
        subscription.UseWebSynchronization = True  
        subscription.InternetUrl = webSyncUrl  
  
        ' Specify the same Windows credentials to use when connecting to the  
        ' Web server using HTTPS Basic Authentication.  
        subscription.InternetSecurityMode = AuthenticationMethod.BasicAuthentication  
        subscription.InternetLogin = winLogin  
        subscription.InternetPassword = winPassword  
  
        ' Create the pull subscription at the Subscriber.  
        subscription.Create()  
  
        Dim registered As Boolean = False  
  
        ' Verify that the subscription is not already registered.  
        For Each existing As MergeSubscription In _  
        publication.EnumSubscriptions()  
            If existing.SubscriberName = subscriberName Then  
                registered = True  
            End If  
        Next  
        If Not registered Then  
            ' Register the local subscription with the Publisher.  
            publication.MakePullSubscriptionWellKnown( _  
             subscriberName, subscriptionDbName, _  
             SubscriptionSyncType.Automatic, _  
             MergeSubscriberType.Local, 0)  
        End If  
    Else  
        ' Do something here if the publication does not exist.  
        Throw New ApplicationException(String.Format( _  
         "The publication '{0}' does not exist on {1}.", _  
         publicationName, publisherName))  
    End If  
Catch ex As Exception  
    ' Implement the appropriate error handling here.  
    Throw New ApplicationException(String.Format( _  
     "The subscription to {0} could not be created.", publicationName), ex)  
Finally  
    subscriberConn.Disconnect()  
    publisherConn.Disconnect()  
End Try  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Concepts liés à RMO (Replication Management Objects)](../../relational-databases/replication/concepts/replication-management-objects-concepts.md)   
 [Afficher et modifier les propriétés d’un abonnement par extraction](../../relational-databases/replication/view-and-modify-pull-subscription-properties.md)   
 [Configurer la synchronisation web](../../relational-databases/replication/configure-web-synchronization.md)   
 [S’abonner aux Publications](../../relational-databases/replication/subscribe-to-publications.md)   
 [Bonnes pratiques en matière de sécurité de la réplication](../../relational-databases/replication/security/replication-security-best-practices.md)  
  
