---
title: Afficher et modifier les propriétés du serveur de distribution et du serveur de publication
description: Découvrez comment modifier les propriétés du serveur de distribution et du serveur de publication à l’aide de SQL Server Management Studio (SSMS), de Transact-SQL (T-SQL) ou de Replication Management Objects (RMO).
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- viewing replication properties
- Distributors [SQL Server replication], modifying
- modifying replication properties, Distributors
- Distributors [SQL Server replication], properties
ms.assetid: 5dae1d59-c377-4c6e-adc9-b68c5b328f79
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: d262e7e8d6599f149afd2ad2b378cfbf8556417a
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250583"
---
# <a name="view-and-modify-distributor-and-publisher-properties"></a>Afficher et modifier les propriétés d'un serveur de distribution ou d'un serveur de publication
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
  Cette rubrique décrit comment afficher et modifier les propriétés du serveur de distribution et du serveur de publication dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], de [!INCLUDE[tsql](../../includes/tsql-md.md)]ou d'objets RMO (Replication Management Objects).  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Recommandations](#Recommendations)  
  
     [Sécurité](#Security)  
  
-   **Pour afficher et modifier les propriétés d'un serveur de publication ou d'un serveur de distribution à l'aide de :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
     [Objets RMO (Replication Management Objects)](#RMOProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> Recommandations  
  
-   Pour les serveurs de publication exécutant des versions antérieures à [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)], un utilisateur avec le rôle serveur fixe **sysadmin** peut enregistrer des Abonnés dans la page **Abonnés**. À partir de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)], il n'est plus nécessaire d'inscrire de manière explicite les abonnés pour la réplication.  
  
###  <a name="security"></a><a name="Security"></a> Sécurité  
 Lorsque c'est possible, demande aux utilisateurs de fournir les informations d'identification au moment de l'exécution.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
  
#### <a name="to-view-and-modify-distributor-properties"></a>Pour afficher et modifier les propriétés du serveur de distribution  
  
1.  Connectez-vous au serveur de distribution dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], puis développez le nœud du serveur.  
  
2.  Cliquez avec le bouton droit sur le dossier **Réplication** , puis cliquez sur **Propriétés du serveur de distribution**.  
  
3.  Affichez et modifiez les propriétés dans la boîte de dialogue **Propriétés du serveur de distribution - \<Distributor>** .  
  
    -   Pour afficher et modifier les propriétés d’une base de données de distribution, cliquez sur le bouton des propriétés ( **...** ) pour la base de données dans la page **Général** de la boîte de dialogue.  
  
    -   Pour afficher et modifier les propriétés du serveur de publication associées au serveur de distribution, cliquez sur le bouton des propriétés ( **...** ) pour le serveur de publication sur la page **Serveurs de publication** de la boîte de dialogue.  
  
    -   Pour accéder aux profils des agents de réplication, cliquez sur le bouton **Profils par défaut** sur la page **Général** de la boîte de dialogue. Pour plus d'informations, voir [Replication Agent Profiles](../../relational-databases/replication/agents/replication-agent-profiles.md).  
  
    -   Pour modifier le mot de passe du compte utilisé lors de l'exécution des procédures stockées administratives sur le serveur de publication et de la mise à jour des informations sur le serveur de distribution, entrez un nouveau mot de passe dans les zones **Mot de passe** et **Confirmer le mot de passe** sur la page **Serveurs de publication** de la boîte de dialogue. Pour plus d’informations, consultez [Protéger le serveur de distribution](../../relational-databases/replication/security/secure-the-distributor.md).  
  
4.  Modifiez les propriétés si nécessaire, puis cliquez sur **OK**.  
  
#### <a name="to-view-and-modify-publisher-properties"></a>Pour afficher et modifier les propriétés du serveur de publication  
  
1.  Connectez-vous au serveur de publication dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], puis développez le nœud du serveur.  
  
2.  Cliquez avec le bouton droit sur le dossier **Réplication** , puis cliquez sur **Propriétés du serveur de publication**.  
  
3.  Affichez et modifiez les propriétés dans la boîte de dialogue **Propriétés du serveur de publication - <serveur_publication>** .  
  
    -   Un utilisateur du rôle serveur fixe **sysadmin** peut activer des bases de données pour la réplication sur la page **Bases de données de publication** . L'activation d'une base de données ne publie pas cette dernière, elle permet plutôt à n'importe quel utilisateur du rôle de base de données fixe **db_owner** de cette base de données de créer une ou plusieurs publications dans la base de données.  
  
4.  Modifiez les propriétés si nécessaire, puis cliquez sur **OK**.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
 Les propriétés du serveur de publication et du serveur de distribution peuvent être affichées par programme en utilisant des procédures stockées de réplication.  
  
#### <a name="to-view-distributor-and-distribution-database-properties"></a>Pour afficher les propriétés du serveur de distribution et de la base de données de distribution  
  
1.  Exécutez [sp_helpdistributor](../../relational-databases/system-stored-procedures/sp-helpdistributor-transact-sql.md) pour retourner des informations sur le serveur de distribution, la base de données de distribution et le répertoire de travail.  
  
2.  Exécutez [sp_helpdistributiondb](../../relational-databases/system-stored-procedures/sp-helpdistributiondb-transact-sql.md) pour retourner les propriétés d'une base de données de distribution spécifiée.  
  
#### <a name="to-change-distributor-and-distribution-database-properties"></a>Pour modifier les propriétés du serveur de distribution et de la base de données de distribution  
  
1.  Sur le serveur de distribution, exécutez [sp_changedistributor_property](../../relational-databases/system-stored-procedures/sp-changedistributor-property-transact-sql.md) pour modifier les propriétés du serveur de distribution.  
  
2.  Sur le serveur de distribution, exécutez [sp_changedistributiondb](../../relational-databases/system-stored-procedures/sp-changedistributiondb-transact-sql.md) pour modifier les propriétés de la base de données de distribution.  
  
3.  Sur le serveur de distribution, exécutez [sp_changedistributor_password](../../relational-databases/system-stored-procedures/sp-changedistributor-password-transact-sql.md) pour modifier le mot de passe du serveur de distribution.  
  
    > [!IMPORTANT]  
    >  Lorsque c'est possible, demande aux utilisateurs de fournir les informations d'identification au moment de l'exécution. Si vous devez stocker des informations d'identification dans un fichier de script, sécurisez le fichier pour empêcher tout accès non autorisé.  
  
4.  Sur le serveur de distribution, exécutez [sp_changedistpublisher](../../relational-databases/system-stored-procedures/sp-changedistpublisher-transact-sql.md) pour modifier les propriétés d'un serveur de publication à l'aide du serveur de distribution.  
  
###  <a name="examples-transact-sql"></a><a name="TsqlExample"></a> Exemples (Transact-SQL)  
 L'exemple de script [!INCLUDE[tsql](../../includes/tsql-md.md)] suivant retourne des informations sur le serveur de distribution et sur la base de données de distribution.  
  
 [!code-sql[HowTo#sp_helpdistributor](../../relational-databases/replication/codesnippet/tsql/view-and-modify-distribu_1.sql)]  
  
 [!code-sql[HowTo#sp_helpdistributiondb](../../relational-databases/replication/codesnippet/tsql/view-and-modify-distribu_2.sql)]  
  
 Cet exemple modifie les périodes de rétention du serveur de distribution, le mot de passe utilisé lors de la connexion au serveur de distribution et l'intervalle auquel le serveur de distribution vérifie l'état de différents Agents de réplication (également appelé intervalle d'interrogation).  
  
> [!IMPORTANT]  
>  Lorsque c'est possible, demande aux utilisateurs de fournir les informations d'identification au moment de l'exécution. Si vous devez stocker des informations d'identification dans un fichier de script, sécurisez le fichier pour empêcher tout accès non autorisé.  
  
 [!code-sql[HowTo#sp_changedistributor_property](../../relational-databases/replication/codesnippet/tsql/view-and-modify-distribu_3.sql)]  
  
 [!code-sql[HowTo#sp_changedistributiondb](../../relational-databases/replication/codesnippet/tsql/view-and-modify-distribu_4.sql)]  
  
 [!code-sql[HowTo#sp_changedistributor_password](../../relational-databases/replication/codesnippet/tsql/view-and-modify-distribu_5.sql)]  
  
##  <a name="using-replication-management-objects-rmo"></a><a name="RMOProcedure"></a> Utilisation d'objets RMO (Replication Management Objects)  
  
#### <a name="to-view-and-modify-distributor-properties"></a>Pour afficher et modifier les propriétés du serveur de distribution  
  
1.  Créez une connexion au serveur de distribution en utilisant la classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Créez une instance de la classe <xref:Microsoft.SqlServer.Replication.ReplicationServer>. Passez l'objet <xref:Microsoft.SqlServer.Management.Common.ServerConnection> créé à l'étape 1.  
  
3.  (Facultatif) Vérifiez la propriété <xref:Microsoft.SqlServer.Replication.ReplicationServer.IsDistributor%2A> pour vérifier que le serveur actuellement connecté est un serveur de distribution.  
  
4.  Appelez la méthode <xref:Microsoft.SqlServer.Replication.ReplicationObject.Load%2A> pour obtenir les propriétés du serveur.  
  
5.  (Facultatif) Pour modifier des propriétés, modifiez la valeur d'une ou de plusieurs des propriétés du serveur de distribution qui peuvent être définies sur l'objet <xref:Microsoft.SqlServer.Replication.ReplicationServer> .  
  
6.  (Facultatif) Si la propriété <xref:Microsoft.SqlServer.Replication.ReplicationObject.CachePropertyChanges%2A> sur l'objet <xref:Microsoft.SqlServer.Replication.ReplicationServer> a la valeur **true**, appelez la méthode <xref:Microsoft.SqlServer.Replication.ReplicationObject.CommitPropertyChanges%2A> pour valider les modifications sur le serveur.  
  
#### <a name="to-view-and-modify-distribution-database-properties"></a>Pour afficher et modifier les propriétés de base de données de distribution  
  
1.  Créez une connexion au serveur de distribution en utilisant la classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Créez une instance de la classe <xref:Microsoft.SqlServer.Replication.DistributionDatabase>. Spécifiez la propriété de nom et passez l'objet <xref:Microsoft.SqlServer.Management.Common.ServerConnection> créé à l'étape 1.  
  
3.  Appelez la méthode <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> pour obtenir les propriétés du serveur. Si cette méthode retourne **false**, la base de données avec le nom spécifié n'existe pas sur le serveur.  
  
4.  (Facultatif) Pour modifier des propriétés, modifiez la valeur d'une des propriétés <xref:Microsoft.SqlServer.Replication.DistributionDatabase> qui peuvent être définies.  
  
5.  (Facultatif) Si la propriété <xref:Microsoft.SqlServer.Replication.ReplicationObject.CachePropertyChanges%2A> sur l'objet <xref:Microsoft.SqlServer.Replication.DistributionDatabase> a la valeur **true**, appelez la méthode <xref:Microsoft.SqlServer.Replication.ReplicationObject.CommitPropertyChanges%2A> pour valider les modifications sur le serveur.  
  
#### <a name="to-view-and-modify-publisher-properties"></a>Pour afficher et modifier les propriétés du serveur de publication  
  
1.  Créez une connexion au serveur de publication en utilisant la classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Créez une instance de la classe <xref:Microsoft.SqlServer.Replication.DistributionPublisher>. Spécifiez la propriété <xref:Microsoft.SqlServer.Replication.DistributionPublisher.Name%2A> et passez l'objet <xref:Microsoft.SqlServer.Management.Common.ServerConnection> créé à l'étape 1.  
  
3.  (Facultatif) Pour modifier des propriétés, modifiez la valeur d'une des propriétés <xref:Microsoft.SqlServer.Replication.DistributionPublisher> qui peuvent être définies.  
  
4.  (Facultatif) Si la propriété <xref:Microsoft.SqlServer.Replication.ReplicationObject.CachePropertyChanges%2A> sur l'objet <xref:Microsoft.SqlServer.Replication.DistributionPublisher> a la valeur **true**, appelez la méthode <xref:Microsoft.SqlServer.Replication.ReplicationObject.CommitPropertyChanges%2A> pour valider les modifications sur le serveur.  
  
#### <a name="to-change-the-password-for-the-administrative-connection-from-the-publisher-to-the-distributor"></a>Pour modifier le mot de passe pour la connexion administrative du serveur de publication au serveur de distribution  
  
1.  Créez une connexion au serveur de distribution en utilisant la classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Créez une instance de la classe <xref:Microsoft.SqlServer.Replication.ReplicationServer>.  
  
3.  Définissez la propriété <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> en spécifiant la connexion créée à l'étape 1.  
  
4.  Appelez la méthode <xref:Microsoft.SqlServer.Replication.ReplicationObject.Load%2A> pour obtenir les propriétés de l'objet.  
  
5.  Appelez la méthode <xref:Microsoft.SqlServer.Replication.ReplicationServer.ChangeDistributorPassword%2A> . Passez la nouvelle valeur de mot de passe pour le paramètre *password* .  
  
    > [!IMPORTANT]  
    >  Lorsque c'est possible, demande aux utilisateurs de fournir les informations d'identification au moment de l'exécution. Si vous devez stocker des informations d'identification, utilisez les [Services de chiffrement](/previous-versions/aa719848(v=vs.71)) fournis par [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows .NET Framework.  
  
6.  (Facultatif) Effectuez la procédure suivante pour modifier le mot de passe sur chaque serveur de publication distant qui utilise ce serveur de distribution :  
  
    1.  Créez une connexion au serveur de publication en utilisant la classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
    2.  Créez une instance de la classe <xref:Microsoft.SqlServer.Replication.ReplicationServer>.  
  
    3.  Définissez la propriété <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> en spécifiant la connexion créée à l'étape 6a.  
  
    4.  Appelez la méthode <xref:Microsoft.SqlServer.Replication.ReplicationObject.Load%2A> pour obtenir les propriétés de l'objet.  
  
    5.  Appelez la méthode <xref:Microsoft.SqlServer.Replication.ReplicationServer.ChangeDistributorPassword%2A> . Passez la nouvelle valeur de mot de passe spécifiée à l'étape 5 pour le paramètre *password* .  
  
###  <a name="example-rmo"></a><a name="PShellExample"></a> Exemple (RMO)  
 Cet exemple montre comment modifier les propriétés du serveur de distribution et de la base de données de distribution.  
  
> [!IMPORTANT]  
>  Pour éviter de stocker les informations d'identification dans le code, le nouveau mot de passe du serveur de distribution est fourni au moment de l'exécution.  
  
 [!code-cs[HowTo#rmo_ChangeDistPub](../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_changedistpub)]  
  
 [!code-vb[HowTo#rmo_vb_ChangeDistPub](../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_changedistpub)]  
  
## <a name="see-also"></a>Voir aussi  
 [Concepts liés à RMO (Replication Management Objects)](../../relational-databases/replication/concepts/replication-management-objects-concepts.md)   
 [Désactiver la publication et la distribution](../../relational-databases/replication/disable-publishing-and-distribution.md)   
 [Configurer la distribution](../../relational-databases/replication/configure-distribution.md)   
 [Concepts liés à RMO (Replication Management Objects)](../../relational-databases/replication/concepts/replication-management-objects-concepts.md)   
 [Script d’information du serveur de distribution et du serveur de publication](../../relational-databases/replication/administration/distributor-and-publisher-information-script.md)   
 [Concepts liés aux procédures stockées système de réplication](../../relational-databases/replication/concepts/replication-system-stored-procedures-concepts.md)   
 [Afficher des informations et effectuer des tâches pour un serveur de publication &#40;moniteur de réplication&#41;](../../relational-databases/replication/monitor/view-information-and-perform-tasks-replication-monitor.md)  
  
