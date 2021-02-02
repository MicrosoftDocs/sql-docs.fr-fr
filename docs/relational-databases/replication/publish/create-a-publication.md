---
title: Créer une publication | Microsoft Docs
description: Découvrez comment créer une publication dans SQL Server à l’aide de SQL Server Management Studio, Transact-SQL ou Replication Management Objects.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- publications [SQL Server replication], creating
- articles [SQL Server replication], defining
- adding articles
- articles [SQL Server replication], adding
ms.assetid: 52ee6de9-1d58-4cb9-8711-372bddbe7154
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: b3c1655be2909c46368c42ea7781617674dc3025
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076817"
---
# <a name="create-a-publication"></a>Créer une publication
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]
  Cette rubrique explique comment créer une publication dans [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)], de [!INCLUDE[tsql](../../../includes/tsql-md.md)]ou d'objets RMO (Replication Management Objects).  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Limitations et restrictions](#Restrictions)  
  
     [Sécurité](#Security)  
  
-   **Pour créer une publication et définir des articles à l'aide de :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
     [Objets RMO (Replication Management Objects)](#RMOProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitations et restrictions  
  
-   Les noms de publications et d’articles ne peuvent pas contenir les caractères suivants : % , \* , [ , ] , | , : , " , ? , ' , \ , / , < , >. Si les objets de la base de données contiennent un de ces caractères et que vous voulez les répliquer, vous devez spécifier un nom d’article différent du nom de l’objet dans la boîte de dialogue **Propriétés de l’article - \<Article>** , qui est disponible dans la page **Articles** de l’Assistant.  
  
###  <a name="security"></a><a name="Security"></a> Sécurité  
 Lorsque c'est possible, demande aux utilisateurs de fournir les informations d'identification au moment de l'exécution. Si vous devez stocker des informations d'identification, utilisez les [Services de chiffrement](/previous-versions/aa719848(v=vs.71)) fournis par [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Windows .NET Framework.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
 Créez des publication et définissez des articles avec l'Assistant Nouvelle publication. Après avoir créé une publication, affichez et modifiez les propriétés de publication dans la boîte de dialogue **Propriétés de la publication – \<Publication>** . Pour plus d’informations sur la création d’une publication à partir d’une base de données Oracle, consultez [Créer une publication à partir d’une base de données Oracle](../../../relational-databases/replication/publish/create-a-publication-from-an-oracle-database.md).  
  
#### <a name="to-create-a-publication-and-define-articles"></a>Pour créer une publication et définir des articles  
  
1.  Connectez-vous au serveur de publication dans [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)], puis développez le nœud du serveur.  
  
2.  Développez le dossier **Réplication** , puis cliquez avec le bouton droit sur le dossier **Publications locales** .  
  
3.  Cliquez sur **Nouvelle publication**.  
  
4.  Exécutez les pages de l'Assistant Nouvelle publication pour.  
  
    -   Spécifier un serveur de distribution si la distribution n'a pas été configurée sur le serveur. Pour plus d’informations sur la configuration de la distribution, consultez [Configurer la publication et la distribution](../../../relational-databases/replication/configure-publishing-and-distribution.md).  
  
         Si vous spécifiez dans la page **Serveur de distribution** que le serveur de publication se comportera comme son propre serveur de distribution (serveur de distribution local), et si ce serveur n'est pas configuré comme serveur de distribution, l'Assistant Nouvelle publication configurera ce serveur. Vous spécifierez un dossier d'instantanés par défaut pour le serveur de distribution dans la page **Dossier d'instantanés** . Le dossier d'instantanés correspond à un simple répertoire que vous définissez sous la forme d'un partage ; les agents qui lisent et écrivent dans le dossier doivent disposer des autorisations suffisantes pour pouvoir y accéder. Pour plus d’informations sur une sécurisation appropriée du dossier, consultez [Sécuriser le dossier d’instantanés](../../../relational-databases/replication/security/secure-the-snapshot-folder.md).  
  
         Si vous spécifiez qu'un autre serveur doit jouer le rôle de serveur de distribution, vous devez entrer un mot de passe dans la page **Mot de passe d'administration** pour les connexions effectuées du serveur de publication sur le serveur de distribution. Ce mot de passe doit correspondre à celui qui a été spécifié lorsque le serveur de publication a été activé sur le serveur de distribution distant.  
  
         Pour plus d'informations, voir [Configure Distribution](../../../relational-databases/replication/configure-distribution.md).  
  
    -   Choisissez une base de données de publication.  
  
    -   Sélectionnez un type de publication. Pour plus d’informations, consultez [Types de réplication](../../../relational-databases/replication/types-of-replication.md).  
  
    -   Spécifiez les données et les objets de base de données à publier ; en option, filtrez les colonnes des articles des tables, et définissez des propriétés d'articles.  
  
    -   En option, filtrez les lignes des articles des tables. Pour plus d’informations, consultez [Filtrer des données publiées](../../../relational-databases/replication/publish/filter-published-data.md).  
  
    -   Définissez la planification de l'Agent d'instantané.  
  
    -   Spécifiez les informations de connexion sous lesquelles les Agents de réplication suivants s'exécuteront et se connecteront :  
  
         \- Agent d’instantané pour toutes les publications.  
  
         \- Agent de lecture du journal pour toutes les publications transactionnelles.  
  
         \- Agent de lecture de la file d’attente pour les publications transactionnelles acceptant les abonnements avec mise à jour.  
  
         Pour plus d'informations, consultez [Replication Agent Security Model](../../../relational-databases/replication/security/replication-agent-security-model.md) et [Replication Security Best Practices](../../../relational-databases/replication/security/replication-security-best-practices.md).  
  
    -   En option, scriptez la publication. Pour plus d'informations, voir [Scripting Replication](../../../relational-databases/replication/scripting-replication.md).  
  
    -   Spécifiez le nom de la publication.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
 Il est possible de créer des publications par programme en utilisant les procédures stockées de réplication. Les procédures stockées à utiliser dépendent du type de publication à créer.  
  
#### <a name="to-create-a-snapshot-or-transactional-publication"></a>Pour créer une publication d'instantané ou une publication transactionnelle  
  
1.  Dans la base de données de publication sur le serveur de publication, exécutez [sp_replicationdboption &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-replicationdboption-transact-sql.md) pour activer la publication de la base de données actuelle à l’aide de la réplication transactionnelle ou d’instantané.  
  
2.  Pour une publication transactionnelle, déterminez si un travail de l'Agent de lecture du journal existe pour la base de données de publication. (Cette étape n'est pas requise pour les publications d'instantané.)  
  
    -   Si un travail de l'Agent de lecture du journal existe pour la base de données de publication, passez à l'étape 3.  
  
    -   Si vous ne savez pas si un travail de l’Agent de lecture du journal existe pour une base de données publiée, exécutez [sp_helplogreader_agent &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-helplogreader-agent-transact-sql.md) au niveau du serveur de publication dans la base de données de publication.  
  
    -   Si le jeu de résultats est vide, créez un travail de l'Agent de lecture du journal. Sur le serveur de publication, exécutez [sp_addlogreader_agent &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addlogreader-agent-transact-sql.md). Indiquez les informations d’identification Windows [!INCLUDE[msCoName](../../../includes/msconame-md.md)] sous lesquelles l’agent s’exécute pour **\@job_login** et **\@job_password**. Si l’agent doit utiliser l’authentification SQL Server lors de la connexion au serveur de publication, vous devez également affecter la valeur **0** à **\@publisher_security_mode** et spécifier les informations de connexion [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] pour **\@publisher_login** et **\@publisher_password**. Passez à l'étape 3.  
  
3.  Sur le serveur de publication, exécutez [sp_addpublication &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addpublication-transact-sql.md). Spécifiez un nom de publication pour **\@publication** et, pour le paramètre **\@repl_freq**, spécifiez la valeur **snapshot** pour une publication d’instantané ou la valeur **continuous** pour une publication transactionnelle. Spécifiez d'autres options de publication éventuelles. Cela définit la publication.  
  
    > [!NOTE]  
    >  Un nom de publication ne doit pas contenir les caractères suivants :  
    >   
    >  % * [ ] | : " ? \ / < >  
  
4.  Sur le serveur de publication, exécutez [sp_addpublication_snapshot &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addpublication-snapshot-transact-sql.md). Spécifiez le nom de publication utilisé à l’étape 3 pour **\@publication** et les informations d’identification Windows sous lesquelles l’Agent d’instantané s’exécute pour **\@snapshot_job_name** et **\@password**. Si l’agent doit utiliser l’authentification SQL Server lors de la connexion au serveur de publication, vous devez également affecter la valeur **0** à **\@publisher_security_mode** et spécifier les informations de connexion [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] pour **\@publisher_login** et **\@publisher_password**. Il s'ensuit la création d'un travail de l'Agent d'instantané pour la publication.  
  
    > [!IMPORTANT]  
    >  Lors de la configuration d'un serveur de publication avec un serveur de distribution distant, les valeurs fournies pour tous les paramètres, y compris *job_login* et *job_password*, sont envoyées en texte brut au serveur de distribution. Vous devez chiffrer la connexion entre le serveur de publication et son serveur de distribution distant avant d'exécuter cette procédure stockée. Pour plus d’informations, consultez [Activer des connexions chiffrées dans le moteur de base de données &#40;Gestionnaire de configuration SQL Server&#41;](../../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md).  
  
5.  Ajoutez des articles à la publication. Pour plus d’informations, consultez [définir un Article](../../../relational-databases/replication/publish/define-an-article.md).  
  
6.  Démarrez le travail de l'Agent d'instantané pour générer l'instantané initial pour cette publication. Pour plus d’informations, voir [Create and Apply the Initial Snapshot](../../../relational-databases/replication/create-and-apply-the-initial-snapshot.md).  
  
#### <a name="to-create-a-merge-publication"></a>Pour créer une publication de fusion  
  
1.  Sur le serveur de publication, exécutez [sp_replicationdboption &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-replicationdboption-transact-sql.md) pour activer la publication de la base de données actuelle à l’aide de la réplication de fusion.  
  
2.  Dans la base de données de publication sur le serveur de publication, exécutez [sp_addmergepublication &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addmergepublication-transact-sql.md). Spécifiez le nom de la publication pour **\@publication** et d’autres options de publication éventuelles. Cela définit la publication.  
  
    > [!NOTE]  
    >  Un nom de publication ne doit pas contenir les caractères suivants :  
    >   
    >  % * [ ] | : " ? \ / < >  
  
3.  Sur le serveur de publication, exécutez [sp_addpublication_snapshot &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addpublication-snapshot-transact-sql.md). Spécifiez le nom de publication utilisé à l’étape 2 pour **\@publication** et les informations d’identification Windows sous lesquelles l’Agent d’instantané s’exécute pour **\@snapshot_job_name** et **\@password**. Si l’agent doit utiliser l’authentification SQL Server lors de la connexion au serveur de publication, vous devez également affecter la valeur **0** à **\@publisher_security_mode** et spécifier les informations de connexion [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] pour **\@publisher_login** et **\@publisher_password**. Il s'ensuit la création d'un travail de l'Agent d'instantané pour la publication.  
  
    > [!IMPORTANT]  
    >  Lors de la configuration d'un serveur de publication avec un serveur de distribution distant, les valeurs fournies pour tous les paramètres, y compris *job_login* et *job_password*, sont envoyées en texte brut au serveur de distribution. Vous devez chiffrer la connexion entre le serveur de publication et son serveur de distribution distant avant d'exécuter cette procédure stockée. Pour plus d’informations, consultez [Activer des connexions chiffrées dans le moteur de base de données &#40;Gestionnaire de configuration SQL Server&#41;](../../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md).  
  
4.  Ajoutez des articles à la publication. Pour plus d’informations, consultez [définir un Article](../../../relational-databases/replication/publish/define-an-article.md).  
  
5.  Démarrez le travail de l'Agent d'instantané pour générer l'instantané initial pour cette publication. Pour plus d’informations, voir [Create and Apply the Initial Snapshot](../../../relational-databases/replication/create-and-apply-the-initial-snapshot.md).  
  
###  <a name="example-transact-sql"></a><a name="TsqlExample"></a> Exemple (Transact-SQL)  
 Cet exemple crée une publication transactionnelle. Des variables de script sont utilisées pour transmettre les informations d'identification Windows nécessaires pour créer les travaux pour l'Agent d'instantané et l'Agent de lecture du journal.  
  
 [!code-sql[HowTo#sp_AddTranPub](../../../relational-databases/replication/codesnippet/tsql/create-a-publication_1.sql)]  
  
 Cet exemple crée une publication de fusion. Des variables de script sont utilisées pour transmettre les informations d'identification Windows nécessaires pour créer le travail pour l'Agent d'instantané.  
  
 [!code-sql[HowTo#sp_AddMergePub](../../../relational-databases/replication/codesnippet/tsql/create-a-publication_2.sql)]  
  
##  <a name="using-replication-management-objects-rmo"></a><a name="RMOProcedure"></a> Utilisation d'objets RMO (Replication Management Objects)  
 Vous pouvez créer des publications par programme à l'aide d'objets RMO (Replication Management Objects). Les classes RMO que vous utilisez pour créer une publication dépendent du type de publication créé.  
  
#### <a name="to-create-a-snapshot-or-transactional-publication"></a>Pour créer une publication d'instantané ou une publication transactionnelle  
  
1.  Créez une connexion au serveur de publication en utilisant la classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Créez une instance de la classe <xref:Microsoft.SqlServer.Replication.ReplicationDatabase> pour la base de données de publication, affectez à la propriété <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> l'instance de <xref:Microsoft.SqlServer.Management.Common.ServerConnection> créée à l'étape 1, puis appelez la méthode <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> . Si <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> retourne **false**, vérifiez que la base de données existe.  
  
3.  Si la propriété <xref:Microsoft.SqlServer.Replication.ReplicationDatabase.EnabledTransPublishing%2A> a la valeur **false**, affectez-lui la valeur **true**.  
  
4.  Pour une publication transactionnelle, vérifiez la valeur de la propriété <xref:Microsoft.SqlServer.Replication.ReplicationDatabase.LogReaderAgentExists%2A> . Si elle a la valeur **true**, un travail de l'Agent de lecture du journal existe déjà pour cette base de données. Si elle a la valeur **false**, procédez comme suit :  
  
    -   Définissez les champs <xref:Microsoft.SqlServer.Replication.IProcessSecurityContext.Login%2A> et <xref:Microsoft.SqlServer.Replication.IProcessSecurityContext.Password%2A> de <xref:Microsoft.SqlServer.Replication.ReplicationDatabase.LogReaderAgentProcessSecurity%2A> afin de fournir les informations d'identification pour le compte [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Windows sous lequel l'Agent de lecture du journal s'exécute.  
  
        > [!NOTE]  
        >  Il n'est pas nécessaire de définir <xref:Microsoft.SqlServer.Replication.ReplicationDatabase.LogReaderAgentProcessSecurity%2A> lorsque la publication est créée par un membre du rôle serveur fixe **sysadmin** . Dans ce cas, l'Agent va emprunter l'identité du compte de l'Agent SQL Server. Pour plus d’informations, voir [Replication Agent Security Model](../../../relational-databases/replication/security/replication-agent-security-model.md).  
  
    -   (Facultatif) Définissez les champs <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SqlStandardLogin%2A> et <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SqlStandardPassword%2A> ou <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SecureSqlStandardPassword%2A> de <xref:Microsoft.SqlServer.Replication.ReplicationDatabase.LogReaderAgentPublisherSecurity%2A> lorsque vous utilisez l'authentification SQL Server pour vous connecter au serveur de publication.  
  
    -   Appelez la méthode <xref:Microsoft.SqlServer.Replication.ReplicationDatabase.CreateLogReaderAgent%2A> pour créer le travail de l'Agent de lecture du journal pour la base de données.  
  
5.  Créez une instance de la classe <xref:Microsoft.SqlServer.Replication.TransPublication> et définissez les propriétés suivantes pour cet objet :  
  
    -   La classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> créée à l'étape 1 pour la propriété <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A>.  
  
    -   Le nom de la base de données publiée pour <xref:Microsoft.SqlServer.Replication.Publication.DatabaseName%2A>.  
  
    -   Le nom de la publication pour <xref:Microsoft.SqlServer.Replication.Publication.Name%2A>.  
  
    -   <xref:Microsoft.SqlServer.Replication.PublicationType> ayant pour valeur <xref:Microsoft.SqlServer.Replication.PublicationType.Transactional> ou <xref:Microsoft.SqlServer.Replication.PublicationType.Snapshot>.  
  
    -   Les champs <xref:Microsoft.SqlServer.Replication.IProcessSecurityContext.Login%2A> et <xref:Microsoft.SqlServer.Replication.IProcessSecurityContext.Password%2A> de <xref:Microsoft.SqlServer.Replication.Publication.SnapshotGenerationAgentProcessSecurity%2A> pour fournir les informations d'identification pour le compte Windows sous lequel le travail de l'Agent d'instantané s'exécute. Ce compte est également utilisé lorsque l'Agent d'instantané établit des connexions au serveur de distribution local et pour toute connexion distante lors de l'utilisation de l'authentification Windows.  
  
        > [!NOTE]  
        >  Il n'est pas nécessaire de définir <xref:Microsoft.SqlServer.Replication.Publication.SnapshotGenerationAgentProcessSecurity%2A> lorsque la publication est créée par un membre du rôle serveur fixe **sysadmin** . Dans ce cas, l'Agent va emprunter l'identité du compte de l'Agent SQL Server. Pour plus d’informations, voir [Replication Agent Security Model](../../../relational-databases/replication/security/replication-agent-security-model.md).  
  
    -   (Facultatif) Les champs <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SqlStandardLogin%2A> et <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SqlStandardPassword%2A> ou <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SecureSqlStandardPassword%2A> de <xref:Microsoft.SqlServer.Replication.Publication.SnapshotGenerationAgentPublisherSecurity%2A> lorsque vous utilisez l'authentification SQL Server pour vous connecter au serveur de publication.  
  
    -   (Facultatif) Utilisez l'opérateur OR logique inclusif ( **|** en Visual C# et **Or** en Visual Basic) et l'opérateur OR logique exclusif ( **^** en Visual C# et **Xor** en Visual Basic) pour définir les valeurs <xref:Microsoft.SqlServer.Replication.PublicationAttributes> de la propriété <xref:Microsoft.SqlServer.Replication.Publication.Attributes%2A> .  
  
    -   (Facultatif) Nom du serveur de publication pour <xref:Microsoft.SqlServer.Replication.TransPublication.PublisherName%2A> lorsqu'il s'agit d'un serveur non-SQL Server.  
  
6.  Appelez la méthode <xref:Microsoft.SqlServer.Replication.Publication.Create%2A> pour créer la publication.  
  
    > [!IMPORTANT]  
    >  Lors de la configuration d'un serveur de publication avec un serveur de distribution distant, les valeurs fournies pour toutes les propriétés, y compris <xref:Microsoft.SqlServer.Replication.Publication.SnapshotGenerationAgentProcessSecurity%2A>, sont envoyées sous forme de texte brut au serveur de distribution. Vous devez chiffrer la connexion entre le serveur de publication et son serveur de distribution distant avant d'appeler la méthode <xref:Microsoft.SqlServer.Replication.Publication.Create%2A> . Pour plus d’informations, consultez [Activer des connexions chiffrées dans le moteur de base de données &#40;Gestionnaire de configuration SQL Server&#41;](../../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md).  
  
7.  Appelez la méthode <xref:Microsoft.SqlServer.Replication.Publication.CreateSnapshotAgent%2A> pour créer le travail de l'Agent d'instantané pour le serveur de publication.  
  
#### <a name="to-create-a-merge-publication"></a>Pour créer une publication de fusion  
  
1.  Créez une connexion au serveur de publication en utilisant la classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Créez une instance de la classe <xref:Microsoft.SqlServer.Replication.ReplicationDatabase> pour la base de données de publication, affectez à la propriété <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> l'instance de <xref:Microsoft.SqlServer.Management.Common.ServerConnection> créée à l'étape 1, puis appelez la méthode <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> . Si <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> retourne **false**, vérifiez que la base de données existe.  
  
3.  Si <xref:Microsoft.SqlServer.Replication.ReplicationDatabase.EnabledMergePublishing%2A> Property is **false**, affectez-lui la valeur **true** et appelez <xref:Microsoft.SqlServer.Replication.ReplicationObject.CommitPropertyChanges%2A>.  
  
4.  Créez une instance de la classe <xref:Microsoft.SqlServer.Replication.MergePublication> et définissez les propriétés suivantes pour cet objet :  
  
    -   La classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> créée à l'étape 1 pour la propriété <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A>.  
  
    -   Le nom de la base de données publiée pour <xref:Microsoft.SqlServer.Replication.Publication.DatabaseName%2A>.  
  
    -   Le nom de la publication pour <xref:Microsoft.SqlServer.Replication.Publication.Name%2A>.  
  
    -   Les champs <xref:Microsoft.SqlServer.Replication.IProcessSecurityContext.Login%2A> et <xref:Microsoft.SqlServer.Replication.IProcessSecurityContext.Password%2A> de <xref:Microsoft.SqlServer.Replication.Publication.SnapshotGenerationAgentProcessSecurity%2A> pour fournir les informations d'identification pour le compte Windows sous lequel le travail de l'Agent d'instantané s'exécute. Ce compte est également utilisé lorsque l'Agent d'instantané établit des connexions au serveur de distribution local et pour toute connexion distante lors de l'utilisation de l'authentification Windows.  
  
        > [!NOTE]  
        >  Il n'est pas nécessaire de définir <xref:Microsoft.SqlServer.Replication.Publication.SnapshotGenerationAgentProcessSecurity%2A> lorsque la publication est créée par un membre du rôle serveur fixe **sysadmin** . Pour plus d’informations, voir [Replication Agent Security Model](../../../relational-databases/replication/security/replication-agent-security-model.md).  
  
    -   (Facultatif) Utilisez l'opérateur OR logique inclusif ( **|** en Visual C# et **Or** en Visual Basic) et l'opérateur OR logique exclusif ( **^** en Visual C# et **Xor** en Visual Basic) pour définir les valeurs <xref:Microsoft.SqlServer.Replication.PublicationAttributes> de la propriété <xref:Microsoft.SqlServer.Replication.Publication.Attributes%2A> .  
  
5.  Appelez la méthode <xref:Microsoft.SqlServer.Replication.Publication.Create%2A> pour créer la publication.  
  
    > [!IMPORTANT]  
    >  Lors de la configuration d'un serveur de publication avec un serveur de distribution distant, les valeurs fournies pour toutes les propriétés, y compris <xref:Microsoft.SqlServer.Replication.Publication.SnapshotGenerationAgentProcessSecurity%2A>, sont envoyées sous forme de texte brut au serveur de distribution. Vous devez chiffrer la connexion entre le serveur de publication et son serveur de distribution distant avant d'appeler la méthode <xref:Microsoft.SqlServer.Replication.Publication.Create%2A> . Pour plus d’informations, consultez [Activer des connexions chiffrées dans le moteur de base de données &#40;Gestionnaire de configuration SQL Server&#41;](../../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md).  
  
6.  Appelez la méthode <xref:Microsoft.SqlServer.Replication.Publication.CreateSnapshotAgent%2A> pour créer le travail de l'Agent d'instantané pour le serveur de publication.  
  
###  <a name="examples-rmo"></a><a name="PShellExample"></a> Exemples (RMO)  
 Cet exemple active la base de données AdventureWorks pour une publication transactionnelle, définit un travail de l'Agent de lecture du journal et crée la publication AdvWorksProductTran. Un article doit être défini pour cette publication. Les informations d'identification du compte Windows nécessaires pour créer le travail de l'Agent de lecture du journal et de l'Agent d'instantané sont passées au moment de l'exécution. Pour savoir comment utiliser des objets RMO pour définir des articles d'instantané et des articles transactionnels, consultez [Define an Article](../../../relational-databases/replication/publish/define-an-article.md).  
  
 [!code-cs[HowTo#rmo_CreateTranPub](../../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_createtranpub)]  
  
 [!code-vb[HowTo#rmo_vb_CreateTranPub](../../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_createtranpub)]  
  
 Cet exemple active la base de données AdventureWorks pour la publication de fusion et crée la publication AdvWorksSalesOrdersMerge. Des articles doivent encore être définis pour cette publication. Les informations d'identification du compte Windows nécessaires pour créer le travail de l'Agent d'instantané sont passées au moment de l'exécution. Pour savoir comment utiliser des objets RMO pour définir des articles de fusion, consultez [Define an Article](../../../relational-databases/replication/publish/define-an-article.md).  
  
 [!code-cs[HowTo#rmo_CreateMergePub](../../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_createmergepub)]  
  
 [!code-vb[HowTo#rmo_vb_CreateMergePub](../../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_createmergepub)]  
  
## <a name="see-also"></a>Voir aussi  
 [Utiliser sqlcmd avec des variables de script](../../../ssms/scripting/sqlcmd-use-with-scripting-variables.md)   
 [Publier des données et des objets de base de données](../../../relational-databases/replication/publish/publish-data-and-database-objects.md)   
 [Concepts liés à RMO (Replication Management Objects)](../../../relational-databases/replication/concepts/replication-management-objects-concepts.md)   
 [Define an Article](../../../relational-databases/replication/publish/define-an-article.md)   
 [Afficher et modifier les propriétés d’une publication](../../../relational-databases/replication/publish/view-and-modify-publication-properties.md)   
 [Configurer la distribution](../../../relational-databases/replication/configure-distribution.md)   
 [Protéger le serveur de distribution](../../../relational-databases/replication/security/secure-the-distributor.md)   
 [Sécuriser le serveur de publication](../../../relational-databases/replication/security/secure-the-publisher.md)  
  
