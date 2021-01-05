---
title: Configurer la réplication avec les groupes de disponibilité
description: Découvrez le processus détaillé requis pour configurer la réplication SQL Server avec votre groupe de disponibilité Always On.
ms.custom: seodec18
ms.date: 01/25/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
helpviewer_keywords:
- Availability Groups [SQL Server], interoperability
- replication [SQL Server], AlwaysOn Availability Groups
ms.assetid: 4e001426-5ae0-4876-85ef-088d6e3fb61c
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 7a8fc6787491c50b490ca0b9c25ab0d45acd52f2
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97640048"
---
# <a name="configure-replication-with-always-on-availability-groups"></a>Configurer la réplication avec les groupes de disponibilité Always On

[!INCLUDE[sql windows only](../../../includes/applies-to-version/sql-windows-only.md)]

  La configuration de la réplication [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] et des groupes de disponibilité Always On implique sept étapes. Chaque étape est décrite plus en détail dans les sections qui suivent.  
  
##  <a name="1-configure-the-database-publications-and-subscriptions"></a><a name="step1"></a> 1. Configurez les publications et les abonnements de la base de données.  
 **Configurer le serveur de distribution**  
  
 La base de données de distribution ne peut pas être placée dans un groupe de disponibilité avec SQL Server 2012 et SQL Server 2014. Il est possible de placer la base de données de distribution dans un groupe de disponibilité avec SQL 2016 et les versions ultérieures, sauf si elle est utilisée dans une topologie de réplication de fusion, de réplication bidirectionnelle ou de réplication d’égal à égal. Pour plus d’informations, consultez [Configurer une base de données de distribution dans un groupe de disponibilité](../../../relational-databases/replication/configure-distribution-availability-group.md).
  
1.  Configurez la distribution sur le serveur de distribution. Si des procédures stockées sont utilisées pour la configuration, exécutez **sp_adddistributor**. Utilisez le paramètre *\@password* pour identifier le mot de passe qui est utilisé lorsqu'un serveur de publication distant se connecte au serveur de distribution. Le mot de passe est également nécessaire sur chaque serveur de publication distant lorsque le serveur de distribution distant est configuré.  
  
    ```  
    USE master;  
    GO  
    EXEC sys.sp_adddistributor  
        @distributor = 'MyDistributor',  
        @password = '**Strong password for distributor**';  
    ```  
  
2.  Créez la base de données de distribution sur le serveur de distribution. Si des procédures stockées sont utilisées pour la configuration, exécutez **sp_adddistributiondb**.  
  
    ```  
    USE master;  
    GO  
    EXEC sys.sp_adddistributiondb  
        @database = 'distribution',  
        @security_mode = 1;  
    ```  
  
3.  Configurez le serveur de publication distant. Si des procédures stockées sont utilisées pour configurer le serveur de distribution, exécutez **sp_adddistpublisher**. Le paramètre *\@security_mode* sert à déterminer comment la procédure stockée de validation du serveur de publication exécutée à partir des agents de réplication se connecte au principal actuel. Si la valeur est 1, l'authentification Windows est utilisée pour la connexion au principal actuel. Si la valeur est 0, l’authentification [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] est utilisée avec les valeurs *\@login* et *\@password* spécifiées. La connexion et le mot de passe spécifiés doivent être valides sur chaque réplica secondaire pour que la procédure stockée de validation se connecte avec succès à ce réplica.  
  
    > [!NOTE]  
    >  Si des agents de réplication modifiés s'exécutent sur un ordinateur autre que le serveur de distribution, l'utilisation de l'authentification Windows pour la connexion au principal requiert la configuration de l'authentification Kerberos pour la communication entre les ordinateurs hôtes de réplica. L'utilisation d'une connexion [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] pour la connexion au principal actuel ne requiert pas l'authentification Kerberos.  
  
    ```  
    USE master;  
    GO  
    EXEC sys.sp_adddistpublisher  
        @publisher = 'AGPrimaryReplicaHost',  
        @distribution_db = 'distribution',  
        @working_directory = '\\MyReplShare\WorkingDir',  
        @login = 'MyPubLogin',  
        @password = '**Strong password for publisher**';  
    ```  
  
 Pour plus d’informations, consultez [sp_adddistpublisher &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-adddistpublisher-transact-sql.md).  
  
 **Configurer le serveur de publication sur le serveur de publication d'origine**  
  
1.  Configurez la distribution à distance. Si des procédures stockées sont utilisées pour configurer le serveur de publication, exécutez **sp_adddistributor**. Spécifiez pour *\@password* la même valeur que celle utilisée quand **sp_adddistrbutor** a été exécuté sur le serveur de distribution pour configurer la distribution.  
  
    ```  
    exec sys.sp_adddistributor  
        @distributor = 'MyDistributor',  
        @password = 'MyDistPass'  
    ```  
  
2.  Activez la base de données pour la réplication. Si des procédures stockées sont utilisées pour configurer le serveur de publication, exécutez **sp_replicationdboption**. Si la réplication transactionnelle et la réplication de fusion doivent être configurées pour la base de données, chacune doit être activée.  
  
    ```  
    USE master;  
    GO  
    EXEC sys.sp_replicationdboption  
        @dbname = 'MyDBName',  
        @optname = 'publish',  
        @value = 'true';  
  
    EXEC sys.sp_replicationdboption  
        @dbname = 'MyDBName',  
        @optname = 'merge publish',  
        @value = 'true';  
    ```  
  
3.  Créez la publication, les articles et les abonnements de réplication. Pour plus d'informations sur la configuration de la réplication, consultez « Publication de données et d'objets de base de données ».  
  
##  <a name="2-configure-the-always-on-availability-group"></a><a name="step2"></a> 2. Configurez le groupe de disponibilité Always On.  
 Dans le principal visé, créez le groupe de disponibilité avec la base de données publiée (ou à publier) en tant que base de données membre. Si vous utilisez l'Assistant Groupe de disponibilité, vous pouvez autoriser l'Assistant à synchroniser pour la première fois les bases de données de réplica secondaire ou vous pouvez effectuer l'initialisation manuellement à l'aide des fonctionnalités de sauvegarde et de restauration.  
  
 Créez un écouteur DNS pour le groupe de disponibilité qui sera utilisé par les agents de réplication pour la connexion au principal actuel. Le nom de l'écouteur spécifié sera utilisé comme cible de redirection pour la paire « serveur de publication d'origine/base de données publiée ». Par exemple, si vous utilisez DDL pour configurer le groupe de disponibilité, l'exemple de code suivant peut être utilisé pour spécifier un écouteur d'un groupe de disponibilité nommé `MyAG` :  
  
```  
ALTER AVAILABILITY GROUP 'MyAG'   
    ADD LISTENER 'MyAGListenerName' (WITH IP (('10.120.19.155', '255.255.254.0')));  
```  
  
 Pour plus d’informations, consultez [Création et configuration des groupes de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/creation-and-configuration-of-availability-groups-sql-server.md).  

  
##  <a name="3-ensure-that-all-of-the-secondary-replica-hosts-are-configured-for-replication"></a><a name="step3"></a> 3. Vérifiez que tous les hôtes de réplica secondaire sont configurés pour la réplication  
 Pour chaque hôte de réplica secondaire, vérifiez que [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] a été configuré pour prendre en charge la réplication. La requête suivante peut être exécutée sur chaque hôte de réplica secondaire pour déterminer si la réplication est installée :  
  
```  
USE master;  
GO  
DECLARE @installed int;  
EXEC @installed = sys.sp_MS_replication_installed;  
SELECT @installed;  
```  
  
 Si *\@installed* est 0, la réplication doit être ajoutée à l’installation de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
##  <a name="4-configure-the-secondary-replica-hosts-as-replication-publishers"></a><a name="step4"></a> 4. Configurer les hôtes de réplica secondaire comme des serveurs de publication de réplication  
 Un réplica secondaire ne peut pas servir de serveur de publication ou de de republication de réplication, mais la réplication doit être configurée de sorte que le serveur secondaire puisse prendre la suite après un basculement. Sur le serveur de distribution, configurez la distribution pour chaque hôte de réplica secondaire. Indiquez la même base de données de distribution et le même répertoire de travail spécifiés lorsque le serveur de publication d'origine a été ajouté au serveur de distribution. Si vous utilisez des procédures stockées pour configurer la distribution, utilisez **sp_adddistpublisher** pour associer les serveurs de publication distants au serveur de distribution. Si *\@login* et *\@password* ont été utilisés pour le serveur de publication d'origine, spécifiez les mêmes valeurs lorsque vous ajoutez les hôtes de réplica secondaire comme serveurs de publication.  
  
```  
EXEC sys.sp_adddistpublisher  
    @publisher = 'AGSecondaryReplicaHost',  
    @distribution_db = 'distribution',  
    @working_directory = '\\MyReplShare\WorkingDir',  
    @login = 'MyPubLogin',  
    @password = '**Strong password for publisher**';  
```  
  
 Pour chaque hôte de réplica secondaire, configurez la distribution. Identifiez le serveur de distribution du serveur de publication d'origine comme serveur de distribution distant. Utilisez le même mot de passe que celui utilisé quand **sp_adddistributor** a été exécuté initialement sur le serveur de distribution. Si des procédures stockées sont utilisées pour configurer la distribution, le paramètre *\@password* de **sp_adddistributor** est utilisé pour spécifier le mot de passe.  
  
```  
EXEC sp_adddistributor   
    @distributor = 'MyDistributor',  
    @password = '**Strong password for distributor**';  
```  
  
 Pour chaque hôte de réplica secondaire, assurez-vous que les abonnés de transmission de type push des publications dans la base de données apparaissent en tant que serveurs liés. Si des procédures stockées sont utilisées pour configurer les serveurs de publication distants, utilisez **sp_addlinkedserver** pour ajouter les abonnés (s’ils sont absents) en tant que serveurs liés aux serveurs de publication.  
  
```  
EXEC sys.sp_addlinkedserver   
    @server = 'MySubscriber';  
```  
  
##  <a name="5-redirect-the-original-publisher-to-the-ag-listener-name"></a><a name="step5"></a> 5. Rediriger le serveur de publication d'origine sur le nom de l'écouteur du groupe de disponibilité (AG)  
 Sur le serveur de distribution, dans la base de données de distribution, exécutez la procédure stockée **sp_redirect_publisher** pour associer le serveur de publication d’origine et la base de données publiée au nom de l’écouteur du groupe de disponibilité.  
  
```  
USE distribution;  
GO  
EXEC sys.sp_redirect_publisher   
@original_publisher = 'MyPublisher',  
    @publisher_db = 'MyPublishedDB',  
    @redirected_publisher = 'MyAGListenerName';  
```  
  
##  <a name="6-run-the-replication-validation-stored-procedure-to-verify-the-configuration"></a><a name="step6"></a> 6. Exécuter la procédure stockée de validation de réplication pour vérifier la configuration  
 Sur le serveur de distribution, dans la base de données de distribution, exécutez la procédure stockée **sp_validate_replica_hosts_as_publishers** pour vérifier que tous les hôtes de réplica sont désormais configurés pour servir de serveurs de publication pour la base de données publiée.  
  
```  
USE distribution;  
GO  
DECLARE @redirected_publisher sysname;  
EXEC sys.sp_validate_replica_hosts_as_publishers  
    @original_publisher = 'MyPublisher',  
    @publisher_db = 'MyPublishedDB',  
    @redirected_publisher = @redirected_publisher output;  
```  
  
 La procédure stockée **sp_validate_replica_hosts_as_publishers** doit être exécutée à partir d’une connexion disposant d’autorisations suffisantes sur chaque hôte de réplica de groupe de disponibilité pour demander les informations sur le groupe de disponibilité. Contrairement à la procédure **sp_validate_redirected_publisher**, elle utilise les informations d’identification de l’appelant et non la connexion conservée dans msdb.dbo.MSdistpublishers pour se connecter aux réplicas de groupe de disponibilité.  
  
> [!NOTE]  
>  **sp_validate_replica_hosts_as_publishers** échoue avec l’erreur suivante lors de la validation des hôtes de réplica secondaire qui n’autorisent pas l’accès en lecture, ou nécessitent la spécification de l’intention de lecture.  
>   
>  Msg 21899, Niveau 11, État 1, Procédure **sp_hadr_verify_subscribers_at_publisher**, Ligne 109  
>   
>  La requête au serveur de publication redirigé 'MyReplicaHostName' pour déterminer s’il y a des entrées sysserver pour les abonnés du serveur de publication d’origine 'MyOriginalPublisher' a échoué avec l’erreur '976', message d’erreur 'Erreur 976, Niveau 14, État 1, Message : La base de données cible, 'MyPublishedDB', est membre d’un groupe de disponibilité et n’est actuellement pas accessible pour les requêtes. Le déplacement des données est alors suspendu ou le réplica de disponibilité n'est pas activé pour l'accès en lecture. Pour autoriser l'accès en lecture seule à cette base de données et à d'autres dans le groupe de disponibilité, activez l'accès en lecture sur un ou plusieurs réplicas de disponibilité secondaires dans le groupe.  Pour plus d’informations, consultez l’instruction **ALTER AVAILABILITY GROUP** dans la documentation en ligne de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
>   
>  Une ou plusieurs erreurs de validation de serveur de publication ont été rencontrées pour l'hôte de réplica 'MyReplicaHostName'.  
  
 Ce comportement est normal. Vous devez vérifier la présence des entrées de serveur d’abonné sur ces hôtes de réplica secondaire en interrogeant les entrées sysserver directement sur l’hôte.  
  
##  <a name="7-add-the-original-publisher-to-replication-monitor"></a><a name="step7"></a> 7. Ajouter un serveur de publication d'origine au moniteur de réplication  
 Pour chaque réplica de groupe de disponibilité, ajoutez le serveur de publication d'origine au moniteur de réplication.  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Tâches associées  
 **Réplication**  
  
-   [Gestion d’une base de données de publication Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/maintaining-an-always-on-publication-database-sql-server.md)  
  
-   [Réplication, suivi des modifications, capture de données modifiées et groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/replicate-track-change-data-capture-always-on-availability.md)  
  
-   [FAQ sur l’administration de la réplication](../../../relational-databases/replication/administration/frequently-asked-questions-for-replication-administrators.md)  
  
 **Pour créer et configurer un groupe de disponibilité**  
  
-   [Utiliser l’Assistant Groupe de disponibilité &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-availability-group-wizard-sql-server-management-studio.md)  
  
-   [Utiliser la boîte de dialogue Nouveau groupe de disponibilité &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-new-availability-group-dialog-box-sql-server-management-studio.md)  
  
-   [Créer un groupe de disponibilité &#40;Transact-SQL&#41;](../../../database-engine/availability-groups/windows/create-an-availability-group-transact-sql.md)  
  
-   [Créer un groupe de disponibilité &#40;SQL Server PowerShell&#41;](../../../database-engine/availability-groups/windows/create-an-availability-group-sql-server-powershell.md)  
  
-   [Spécifier l’URL de point de terminaison lors de l’ajout ou lors de la modification d’un réplica de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/specify-endpoint-url-adding-or-modifying-availability-replica.md)  
  
-   [Créer un point de terminaison de mise en miroir de bases de données pour les groupes de disponibilité Always On &#40;SQL Server PowerShell&#41;](../../../database-engine/availability-groups/windows/database-mirroring-always-on-availability-groups-powershell.md)  
  
-   [Joindre un réplica secondaire à un groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/join-a-secondary-replica-to-an-availability-group-sql-server.md)  
  
-   [Préparer manuellement une base de données secondaire pour un groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/manually-prepare-a-secondary-database-for-an-availability-group-sql-server.md)  
  
-   [Joindre une base de données secondaire à un groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/join-a-secondary-database-to-an-availability-group-sql-server.md)  
  
-   [Créer ou configurer un écouteur de groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md)  
  
## <a name="see-also"></a>Voir aussi  
- [Conditions préalables requises, restrictions et recommandations pour les groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)   
- [Vue d’ensemble des groupes de disponibilité Always On (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md) 
- [Groupes de disponibilité Always On : Interopérabilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/always-on-availability-groups-interoperability-sql-server.md)   
- [Réplication SQL Server](../../../relational-databases/replication/sql-server-replication.md)  
- Si vous utilisez des ports autres que ceux par défaut, consultez [Walkthrough Publisher, Distributor, Subscriber in AlwaysOn Availability Groups](https://repltalk.com/2019/03/09/walkthrough-publisher-distributor-subscriber-in-alwayson-availability-groups).
