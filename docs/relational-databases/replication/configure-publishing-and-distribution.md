---
title: Configurer la publication et la distribution | Microsoft Docs
description: Découvrez comment configurer la publication et la distribution dans SQL Server à l’aide de SQL Server Management Studio, Transact-SQL ou Replication Management Objects.
ms.custom: ''
ms.date: 09/23/2018
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- replication [SQL Server], distribution
- distribution configuration [SQL Server replication]
- publishing [SQL Server replication], configuring
ms.assetid: 3cfc8966-833e-42fa-80cb-09175d1feed7
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 18f45f1cd625f3cfe724371cb787fb80942e4be0
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076371"
---
# <a name="configure-publishing-and-distribution"></a>Configurer la publication et la distribution
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
 Cette rubrique explique comment configurer la publication et la distribution dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../includes/tsql-md.md)]ou d'objets RMO (Replication Management Objects).

##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer 

###  <a name="security"></a><a name="Security"></a> Sécurité 
Pour plus d’informations, consultez [Afficher et modifier les paramètres de sécurité de la réplication](../../relational-databases/replication/security/view-and-modify-replication-security-settings.md).

##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio 
Configurez la distribution à l'aide de l'Assistant Nouvelle publication ou de l'Assistant Configuration de la distribution. Après avoir configuré le serveur de distribution, affichez et modifiez des propriétés dans la boîte de dialogue **Propriétés du serveur de distribution - \<Distributor>** . Utilisez l'Assistant Configuration de la distribution si vous voulez configurer un serveur de distribution de telle sorte que les membres des rôles de base de données fixes `db_owner` puissent créer des publications, ou si vous souhaitez configurer un serveur de distribution distant qui ne soit pas un serveur de publication.

#### <a name="to-configure-distribution"></a>Pour configurer la distribution 

1. Dans [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], connectez-vous au serveur qui servira de serveur de distribution (souvent, le serveur de publication et le serveur de distribution sont les mêmes), puis développez le nœud du serveur.

2. Cliquez avec le bouton droit sur le dossier **Réplication** , puis cliquez sur **Configurer la distribution**.

3. Suivez les instructions de l'Assistant Configuration de la distribution pour : 

  - Sélectionner un serveur de distribution. Pour utiliser un serveur de distribution local, sélectionnez **nom_serveur agit comme son propre serveur de distribution ; SQL Server crée une base de données de distribution et un journal**. Pour utiliser un serveur de distribution distant, sélectionnez l'option **Utiliser le serveur suivant comme serveur de distribution**, puis sélectionnez un serveur. Ce dernier doit déjà être configuré comme un serveur de distribution et le serveur de publication configuré pour utiliser ce serveur de distribution. Pour plus d’informations, consultez [Activer un serveur de publication distant sur un serveur de distribution &#40;SQL Server Management Studio&#41;](../../relational-databases/replication/enable-a-remote-publisher-at-a-distributor-sql-server-management-studio.md).

     Si vous sélectionnez un serveur de distribution distant, vous devez entrer un mot de passe dans la page **Mot de passe d'administration** pour les connexions effectuées à partir du serveur de publication sur le serveur de distribution. Ce mot de passe doit correspondre à celui qui a été spécifié lorsque le serveur de publication a été activé sur le serveur de distribution distant.

  - Spécifier un dossier d'instantanés racine (pour un serveur de distribution local). Le dossier d'instantanés correspond à un simple répertoire que vous définissez sous la forme d'un partage ; les agents qui lisent et écrivent dans le dossier doivent disposer des autorisations suffisantes pour pouvoir y accéder. Chaque serveur de publication qui utilise ce serveur de distribution crée un dossier sous le dossier racine, et chaque publication crée des dossiers sous le dossier Serveur de publication, pour y stocker les fichiers d'instantanés. Pour plus d’informations sur une sécurisation appropriée du dossier, consultez [Sécuriser le dossier d’instantané](../../relational-databases/replication/security/secure-the-snapshot-folder.md).

  - Spécifier la base de données de distribution (pour un serveur de distribution local). La base de données de distribution stocke les métadonnées et les données d'historique pour tous les types de réplications, et les transactions pour la réplication transactionnelle.

  - Permettre en option aux autres serveurs de publication d'utiliser le serveur de distribution. Si d'autres serveurs de publication sont activés pour utiliser le serveur de distribution, vous devez entrer un mot de passe dans la page **Mot de passe du serveur de distribution** pour les connexions effectuées à partir de ces serveurs de publication sur le serveur de distribution.

  - Scripter en option les paramètres de configuration. Pour plus d'informations, voir [Scripting Replication](../../relational-databases/replication/scripting-replication.md).

##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL 
La publication et la distribution de réplication peuvent être configurées par programme à l'aide de procédures stockées de réplication.
### <a name="to-configure-publishing-using-a-local-distributor"></a>Pour configurer la publication à l'aide d'un serveur de distribution local
1. Exécutez [sp_get_distributor &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-get-distributor-transact-sql.md) pour déterminer si le serveur est déjà configuré comme serveur de distribution.

  - Dans le jeu de résultats, si `installed` a la valeur `0`, exécutez [sp_adddistributor &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-adddistributor-transact-sql.md) sur le serveur de distribution, sur la base de données MASTER.

  - Dans le jeu de résultats, si `distribution db installed` a la valeur `0`, exécutez [sp_adddistributiondb &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-adddistributiondb-transact-sql.md) sur le serveur de distribution, sur la base de données MASTER. Spécifiez le nom de la base de données de distribution pour `@database`. Vous pouvez si vous le souhaitez spécifier la période maximale de rétention de transaction de `@max_distretention` et la période de rétention de l'historique de `@history_retention`. Si une nouvelle base de données est créée, spécifiez les paramètres de propriété de base de données de votre choix.

2. Sur le serveur de distribution, qui est également le serveur de publication, exécutez [sp_adddistpublisher &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-adddistpublisher-transact-sql.md) en spécifiant le partage UNC qui sera utilisé comme dossier d’instantanés par défaut pour `@working_directory`.

   Pour un serveur de distribution sur SQL Managed Instance, utilisez un compte de stockage Azure pour `@working_directory` et la clé d’accès de stockage pour `@storage_connection_string`. 

3. Sur le serveur de publication, exécutez [sp_replicationdboption &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-replicationdboption-transact-sql.md). Spécifiez la base de données publiée pour `@dbname`, le type de réplication pour `@optname` et la valeur `true` pour `@value`.

#### <a name="to-configure-publishing-using-a-remote-distributor"></a>Pour configurer la publication à l'aide d'un serveur de distribution distant 

1. Exécutez [sp_get_distributor &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-get-distributor-transact-sql.md) pour déterminer si le serveur est déjà configuré comme serveur de distribution.

   - Dans le jeu de résultats, si `installed` a la valeur `0`, exécutez [sp_adddistributor &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-adddistributor-transact-sql.md) sur le serveur de distribution, sur la base de données MASTER. Spécifiez un mot de passe fort pour `@password`. Ce mot de passe du compte `distributor_admin` sera utilisé par le serveur de publication lors de la connexion au serveur de distribution.

   - Dans le jeu de résultats, si `distribution db installed` a la valeur `0`, exécutez [sp_adddistributiondb &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-adddistributiondb-transact-sql.md) sur le serveur de distribution, sur la base de données MASTER. Spécifiez le nom de la base de données de distribution pour `@database`. Vous pouvez si vous le souhaitez spécifier la période maximale de rétention de transaction de `@max_distretention` et la période de rétention de l'historique de `@history_retention`. Si une nouvelle base de données est créée, spécifiez les paramètres de propriété de base de données de votre choix.

2. Sur le serveur de distribution, exécutez [sp_adddistpublisher &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-adddistpublisher-transact-sql.md) en spécifiant le partage UNC qui sera utilisé comme dossier d’instantanés par défaut pour `@working_directory`. Si le serveur de distribution doit utiliser l’authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] lors de la connexion au serveur de publication, spécifiez également la valeur `0` pour `@security_mode` et les informations de connexion [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour `@login` et `@password`.

   Pour un serveur de distribution sur SQL Managed Instance, utilisez un compte de stockage Azure pour `@working_directory` et la clé d’accès de stockage pour `@storage_connection_string`. 

3. Sur le serveur de publication, sur la base de données MASTER, exécutez [sp_adddistributor &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-adddistributor-transact-sql.md). Spécifiez le mot de passe fort utilisé à l'étape 1 pour `@password`. Ce mot de passe sera utilisé par le serveur de publication lors de la connexion au serveur de distribution.

4. Sur le serveur de publication, exécutez [sp_replicationdboption &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-replicationdboption-transact-sql.md). Spécifiez la base de données publiée pour `@dbname`, le type de réplication pour `@optname` et la valeur true pour `@value`.

###  <a name="example-transact-sql"></a><a name="TsqlExample"></a> Exemple (Transact-SQL) 
L'exemple ci-dessous montre comment configurer par programme la publication et la distribution. Dans cet exemple, le nom du serveur configuré comme serveur de publication et serveur de distribution local est fourni au moyen de variables de script. La publication et la distribution de réplication peuvent être configurées par programme à l'aide de procédures stockées de réplication.

[!code-sql[HowTo#AddDistPub](../../relational-databases/replication/codesnippet/tsql/configure-publishing-and_1.sql)] 

##  <a name="using-replication-management-objects-rmo"></a><a name="RMOProcedure"></a> Utilisation d'objets RMO (Replication Management Objects) 

#### <a name="to-configure-publishing-and-distribution-on-a-single-server"></a>Pour configurer la publication et la distribution sur un serveur unique 

1. Créez une connexion au serveur en utilisant la classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .

2. Créez une instance de la classe <xref:Microsoft.SqlServer.Replication.ReplicationServer>. Passez l'objet <xref:Microsoft.SqlServer.Management.Common.ServerConnection> créé à l'étape 1.

3. Créez une instance de la classe <xref:Microsoft.SqlServer.Replication.DistributionDatabase>.

4. Affectez le nom de la base de données à la propriété <xref:Microsoft.SqlServer.Replication.DistributionDatabase.Name%2A> et le <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> créé à l'étape 1 à la propriété <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .

5. Installez le serveur de distribution en appelant la méthode <xref:Microsoft.SqlServer.Replication.ReplicationServer.InstallDistributor%2A> . Passez l'objet <xref:Microsoft.SqlServer.Replication.DistributionDatabase> créé à l'étape 3.

6. Créez une instance de la classe <xref:Microsoft.SqlServer.Replication.DistributionPublisher>.

7. Définissez les propriétés suivantes de <xref:Microsoft.SqlServer.Replication.DistributionPublisher>: 

  - <xref:Microsoft.SqlServer.Replication.DistributionPublisher.Name%2A> - nom du serveur de publication.

  - <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> - <xref:Microsoft.SqlServer.Management.Common.ServerConnection> créé à l'étape 1.

  - <xref:Microsoft.SqlServer.Replication.DistributionPublisher.DistributionDatabase%2A> - nom de la base de données créée à l'étape 5.

  - <xref:Microsoft.SqlServer.Replication.DistributionPublisher.WorkingDirectory%2A> - partage utilisé pour accéder aux fichiers d'instantanés.

  - <xref:Microsoft.SqlServer.Replication.DistributionPublisher.PublisherSecurity%2A> - Mode de sécurité utilisé lors de la connexion au serveur de publication. <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.WindowsAuthentication%2A> est recommandé.

8. Appelez la méthode <xref:Microsoft.SqlServer.Replication.DistributionPublisher.Create%2A> .

#### <a name="to-configure-publishing-and-distribution-using-a-remote-distributor"></a>Pour configurer la publication et la distribution à l'aide d'un serveur de distribution distant 

1. Créez une connexion au serveur de distribution distant en utilisant la classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .

2. Créez une instance de la classe <xref:Microsoft.SqlServer.Replication.ReplicationServer>. Passez l'objet <xref:Microsoft.SqlServer.Management.Common.ServerConnection> créé à l'étape 1.

3. Créez une instance de la classe <xref:Microsoft.SqlServer.Replication.DistributionDatabase>.

4. Affectez le nom de la base de données à la propriété <xref:Microsoft.SqlServer.Replication.DistributionDatabase.Name%2A> et le <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> créé à l'étape 1 à la propriété <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .

5. Installez le serveur de distribution en appelant la méthode <xref:Microsoft.SqlServer.Replication.ReplicationServer.InstallDistributor%2A> . Spécifiez un mot de passe sécurisé (utilisé par le serveur de publication lors de la connexion au serveur de distribution distant) et l'objet <xref:Microsoft.SqlServer.Replication.DistributionDatabase> créé à l'étape 3. Pour plus d’informations, consultez [Protéger le serveur de distribution](../../relational-databases/replication/security/secure-the-distributor.md).

   > `IMPORTANT!!` Si possible, invitez les utilisateurs à entrer leurs informations d'identification au moment de l'exécution. Si vous devez stocker des informations d'identification, utilisez les [Services de chiffrement](/previous-versions/aa719848(v=vs.71)) fournis par [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows .NET Framework.

6. Créez une instance de la classe <xref:Microsoft.SqlServer.Replication.DistributionPublisher>.

7. Définissez les propriétés suivantes de <xref:Microsoft.SqlServer.Replication.DistributionPublisher>: 

  - <xref:Microsoft.SqlServer.Replication.DistributionPublisher.Name%2A> - nom du serveur de publication local.

  - <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> - <xref:Microsoft.SqlServer.Management.Common.ServerConnection> créé à l'étape 1.

  - <xref:Microsoft.SqlServer.Replication.DistributionPublisher.DistributionDatabase%2A> - nom de la base de données créée à l'étape 5.

  - <xref:Microsoft.SqlServer.Replication.DistributionPublisher.WorkingDirectory%2A> - partage utilisé pour accéder aux fichiers d'instantanés.

  - <xref:Microsoft.SqlServer.Replication.DistributionPublisher.PublisherSecurity%2A> - Mode de sécurité utilisé lors de la connexion au serveur de publication. <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.WindowsAuthentication%2A> est recommandé.

8. Appelez la méthode <xref:Microsoft.SqlServer.Replication.DistributionPublisher.Create%2A> .

9. Créez une connexion au serveur de publication local en utilisant la classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .

10. Créez une instance de la classe <xref:Microsoft.SqlServer.Replication.ReplicationServer>. Passez l’objet <xref:Microsoft.SqlServer.Management.Common.ServerConnection> créé à l’étape 9.

11. Appelez la méthode <xref:Microsoft.SqlServer.Replication.ReplicationServer.InstallDistributor%2A> . Passez le nom du serveur de distribution distant et son mot de passe spécifié à l'étape 5.

> [!IMPORTANT]
> Lorsque c'est possible, demande aux utilisateurs de fournir les informations d'identification au moment de l'exécution. Si vous devez stocker des informations d'identification, utilisez les [Services de chiffrement](/previous-versions/aa719848(v=vs.71)) fournis par Windows .NET Framework.

###  <a name="example-rmo"></a><a name="PShellExample"></a> Exemple (RMO) 
Vous pouvez configurer par programme la publication et la distribution de la réplication à l'aide d'objets RMO (Replication Management Objects).

[!code-cs[HowTo#rmo_AddDistPub](../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_adddistpub)] 

[!code-vb[HowTo#rmo_vb_AddDistPub](../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_adddistpub)] 

## <a name="see-also"></a>Voir aussi 
[Afficher et modifier les propriétés d’un serveur de distribution ou d’un serveur de publication](../../relational-databases/replication/view-and-modify-distributor-and-publisher-properties.md)  
[Replication System Stored Procedures Concepts](../../relational-databases/replication/concepts/replication-system-stored-procedures-concepts.md)  
[Configurer la distribution](../../relational-databases/replication/configure-distribution.md)  
[Concepts liés à Replication Management Objects](../../relational-databases/replication/concepts/replication-management-objects-concepts.md)  
[Configurer la réplication pour les groupes de disponibilité Always On &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/configure-replication-for-always-on-availability-groups-sql-server.md)