---
title: Gérer une base de données de serveur de publication répliquée et membre d’un groupe de disponibilité
description: Décrit comment gérer et tenir à jour une base de données qui est utilisée comme serveur de publication dans une réplication SQL et qui est également membre d’un groupe de disponibilité Always On.
ms.custom: seodec18
ms.date: 05/18/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
helpviewer_keywords:
- Availability Groups [SQL Server], interoperability
- replication [SQL Server], AlwaysOn Availability Groups
ms.assetid: 55b345fe-2eb9-4b04-a900-63d858eec360
author: cawrites
ms.author: chadam
ms.openlocfilehash: 3068eaae089c4a9e7cd6ab2599eb04695d56aa6f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348432"
---
# <a name="manage-a-replicated-publisher-database-as-part-of-an-always-on-availability-group"></a>Gérer une base de données de serveur de publication répliquée et membre d’un groupe de disponibilité Always On
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  Cette rubrique comporte des remarques spécifiques sur la gestion d’une base de données de publication lors de l’utilisation de groupes de disponibilité Always On.  
  
##  <a name="maintaining-a-published-database-in-an-availability-group"></a><a name="MaintainPublDb"></a> Gestion d'une base de données publiée dans un groupe de disponibilité  
 La gestion d’une base de données de publication Always On est quasiment identique à la gestion d’une base de données de publication standard. Il convient toutefois de prendre quelques précautions :  
  
-   L'administration doit être effectuée au niveau de l'hôte de réplica principal. Dans [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)], les publications apparaissent dans le dossier **Publications locales** pour l'hôte de réplica principal et également pour les réplicas secondaires lisibles. Après un basculement, il est possible que vous soyez contraint d'actualiser manuellement [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)] pour que la modification soit prise en compte si le serveur secondaire qui est devenu le principal n'était pas lisible.  
  
-   Le moniteur de réplication affiche toujours les informations de publication sous le serveur de publication d'origine. Toutefois, ces informations peuvent être consultées dans le moniteur de réplication à partir de tout réplica en ajoutant le serveur de publication d'origine comme serveur.  
  
-   Si vous faites appel aux procédures stockées ou aux Replication Management Objects pour gérer la réplication sur le principal actuel, vous devez spécifier comme serveur de publication le nom de l'instance où la base de données est activée pour la réplication (le serveur de publication d'origine). Pour déterminer le nom approprié, utilisez la fonction **PUBLISHINGSERVERNAME** . Lorsqu'une base de données de publication joint un groupe de disponibilité, les métadonnées de réplication stockées dans les réplicas de bases de données secondaires sont identiques à celles du principal. Par conséquent, en cas de bases de données de publication activées pour la réplication sur le principal, le nom d'instance du serveur de publication stocké dans les tables système du serveur secondaire est celui du serveur principal, et non du serveur secondaire. Cela affecte la configuration et l'administration de la réplication si la base de données de publication bascule sur un serveur secondaire. Par exemple, si vous configurez la réplication avec des procédures stockées sur un serveur secondaire après un basculement et souhaitez ajouter un abonnement par extraction à une base de données de publication qui a été activée sur un réplica différent, vous devez spécifier le nom du serveur de publication d’origine au lieu du serveur de publication actuel comme paramètre *\@publisher* de **sp_addpullsubscription** ou de **sp_addmergepullsubscription**. Toutefois, si vous activez une base de données de publication après un basculement, le nom de l'instance du serveur de publication stocké dans les tables système est le nom de l'hôte principal actuel. Dans ce cas, vous devez utiliser le nom d’hôte du réplica principal actuel pour le paramètre *\@publisher*.  
  
    > [!NOTE]  
    >  Dans certaines procédures, comme **sp_addpublication**, le paramètre *\@publisher* n’est pris en charge que pour les serveurs de publication qui ne sont pas des instances de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ; dans ce cas, il n’est pas utile pour [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Always On.  
  
-   Pour synchroniser un abonnement dans [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)] après un basculement, synchronisez les abonnements par extraction à partir de l'abonné, puis synchronisez les abonnements par émission de données à partir du serveur de publication actif.  
  
##  <a name="removing-a-published-database-from-an-availability-group"></a><a name="RemovePublDb"></a> Suppression d'une base de données publiée d'un groupe de disponibilité  
 Considérez les points suivants lorsqu'une base de données publiée est supprimée d'un groupe de disponibilité, ou lorsqu'un groupe de disponibilité avec une base de données membre publiée est supprimé.  
  
-   Si la base de données de publication du serveur de publication d’origine est supprimée d’un réplica principal de groupe de disponibilité, vous devez exécuter **sp_redirect_publisher** sans spécifier de valeur pour le paramètre *\@redirected_publisher* pour supprimer la redirection pour la paire « serveur de publication/base de données ».  
  
    ```  
    EXEC sys.sp_redirect_publisher   
        @original_publisher = 'MyPublisher',  
        @published_database = 'MyPublishedDB';  
    ```  
  
     La base de données restera à l'état de récupération dans le principal et devra être restaurée. Après cela, la réplication doit s'exécuter sans modification sur le serveur de publication d'origine.  
  
-   Si la base de données de publication bascule du serveur de publication d’origine vers un réplica et est supprimée du réplica principal de groupe de disponibilité, utilisez la procédure stockée **sp_redirect_publisher** pour rediriger explicitement le serveur de publication d’origine vers le nouveau serveur de publication. La base de données restera à l'état de récupération et devra être restaurée. Après cela, la réplication doit continuer à fonctionner de la même manière qu'avec le groupe de disponibilité.  
  
    ```  
    EXEC sys.sp_redirect_publisher   
        @original_publisher = 'MyPublisher',  
        @published_database = 'MyPublishedDB',  
        @redirected_publisher = 'MyNewPublisher';  
    ```  
  
     Ne supprimez pas le serveur distant du serveur de publication d'origine du serveur de distribution, même si le serveur n'est plus accessible. Les métadonnées du serveur du serveur de publication d'origine sont requises sur le serveur de distribution pour satisfaire les requêtes de métadonnées de publication.  
  
-   Si un groupe de disponibilité complet est supprimé, le comportement d’une base de données membre répliquée est le même que lorsqu’une base de données publiée est supprimée d’un groupe de disponibilité. La réplication peut être reprise à partir du dernier principal dès que la base de données a été restaurée et la redirection a été modifiée. Si la base de données est restaurée sur son serveur de publication d'origine, la redirection doit être supprimée. Si la base de données est restaurée sur un hôte différent, la redirection doit être explicitement dirigée vers le nouvel hôte.  
  
    > [!NOTE]  
    >  Lorsqu'un groupe de disponibilité comportant des bases de données membres publiées est supprimé, ou lorsqu'une base de données publiée est est supprimée d'un groupe de disponibilité, toutes les copies des bases de données publiées sont laissées à l'état de récupération. Si elles sont restaurées, chacune apparaîtra comme une base de données publiée. Une seule copie doit être conservée avec les métadonnées de publication. Pour désactiver la réplication d'une copie de base de données publiée, commencez par supprimer tous les abonnements et toutes les publications de la base de données.  
  
     Exécutez **sp_dropsubscription** pour supprimer les abonnements de publication. Veillez à affecter la valeur 1 au paramètre *\@ignore_distributor* pour conserver les métadonnées de la base de données de publication active sur le serveur de distribution.  
  
    ```  
    USE MyDBName;  
    GO  
  
    EXEC sys.sp_dropsubscription   
        @subscriber = 'MySubscriber',  
        @publication = 'MyPublication',  
        @article = 'all',  
        @ignore_distributor = 1;  
    ```  
  
     Exécutez **sp_droppublication** pour supprimer toutes les publications. De nouveau, affectez la valeur 1 au paramètre *\@ignore_distributor* pour conserver les métadonnées de la base de données de publication active sur le serveur de distribution.  
  
    ```  
    EXEC sys.sp_droppublication   
        @publication = 'MyPublication',  
        @ignore_distributor = 1;  
    ```  
  
     Exécutez **sp_replicationdboption** pour désactiver la réplication pour la base de données.  
  
    ```  
    EXEC sys.sp_replicationdboption  
        @dbname = 'MyDBName',  
        @optname = 'publish',  
        @value = 'false';  
    ```  
  
     À ce stade, la copie de la base de données publiée peut être conservée ou supprimée.  

## <a name="remove-original-publisher"></a>Supprimer le serveur de publication d’origine

Il peut y avoir des cas (remplacement d’ancien serveur, mise à niveau du système d’exploitation, etc.) dans lesquels vous souhaitez supprimer un serveur de publication d’origine d’un groupe de disponibilité Always On. Suivez les étapes de cette section pour supprimer le serveur de publication du groupe de disponibilité. 

Supposons que vous avez des serveurs N1, N2 et D1, où N1 et N2 sont le réplica principal et le réplica secondaire du groupe de disponibilité AG1, N1 est le serveur de publication d’origine d’une publication transactionnelle et D1 est le serveur de distribution. Vous souhaitez remplacer le serveur de publication original N1 par le nouveau serveur de publication N3. 

Pour supprimer le serveur de publication, procédez comme suit : 

1. Installez et configurez SQL Server sur le nœud N3. La version de SQL Server doit être identique à celle du serveur de publication d’origine. 
1. Sur le serveur de distribution D1, ajoutez N3 en tant que serveur de publication à l’aide de [sp_adddistpublisher](../../../relational-databases/system-stored-procedures/sp-adddistpublisher-transact-sql.md). 
1. Configurez N3 en tant que serveur de publication avec D1 comme serveur de distribution. 
1. Ajoutez N3 en tant que réplica au groupe de disponibilité AG1. 
1. Sur le réplica N3, vérifiez que les abonnés de type push de la publication apparaissent en tant que serveurs liés. Utilisez [sp_addlinkedserver](../../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md) ou SQL Server Management Studio. 
1. Une fois la synchronisation N3 terminée, faites basculer le groupe de disponibilité sur N3 comme serveur principal. 
1. Supprimez N1 du groupe de disponibilité AG1. 

Tenez compte des points suivants :
- Ne supprimez pas le serveur distant du serveur de publication d’origine (N1 dans le cas présent) ou les métadonnées qui lui sont associées du serveur de distribution, même si le serveur n’est plus accessible. Les métadonnées du serveur du serveur de publication d’origine sont requises sur le serveur de distribution pour satisfaire les requêtes de métadonnées de publication et sans elles la réplication échouera. 
- Pour SQL Server 2014, une fois le serveur de publication d’origine supprimé, vous ne pourrez pas utiliser le nom de l’éditeur d’origine pour administrer la réplication dans le moniteur de réplication. Si vous essayez d’enregistrer de nouveaux réplicas en tant que serveur de publication dans le moniteur de réplication, les informations ne s’affichent pas, car aucune métadonnée ne lui est associée. Pour administrer la réplication dans ce scénario, vous devez cliquer avec le bouton droit sur des publications et des abonnements individuels dans SQL Server Management Studio (SSMS).
- Pour SQL Server 2016 SP2-CU3, SQL Server 2017 CU6 et versions ultérieures, inscrivez l’écouteur du serveur de publication du groupe de disponibilité dans le moniteur de réplication pour administrer la réplication à l’aide de SQL Server Management Studio version 17.7 ou ultérieure. 
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Tâches associées  
  
-   [Configurer la réplication pour les groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-replication-for-always-on-availability-groups-sql-server.md)  
  
-   [Réplication, suivi des modifications, capture de données modifiées et groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/replicate-track-change-data-capture-always-on-availability.md)  
  
-   [FAQ sur l’administration de la réplication](../../../relational-databases/replication/administration/frequently-asked-questions-for-replication-administrators.md)  
  
-   [Abonnés de réplication et groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/replication-subscribers-and-always-on-availability-groups-sql-server.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Conditions préalables requises, restrictions et recommandations pour les groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)   
 [Vue d’ensemble des groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Groupes de disponibilité Always On : Interopérabilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/always-on-availability-groups-interoperability-sql-server.md)   
 [Réplication SQL Server](../../../relational-databases/replication/sql-server-replication.md)  
  
  
