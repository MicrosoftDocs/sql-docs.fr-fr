---
title: 'Groupes de disponibilité : solution de haute disponibilité et de reprise d’activité'
description: Les groupes de disponibilité Always On sont une solution de haute disponibilité et de reprise d’activité SQL Server qui offre une alternative au niveau de l’entreprise à la mise en miroir de bases de données, avec davantage de fonctionnalités. Découvrez les principes de base et les fonctions de cette fonctionnalité.
ms.custom: seodec18
ms.date: 04/23/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
helpviewer_keywords:
- Availability Groups [SQL Server], about
- secondary replicas, see Availability Groups [SQL Server]
- availability [SQL Server]
- AlwaysOn [SQL Server], see Availability Groups [SQL Server]
- Availability Groups [SQL Server]
ms.assetid: aa427606-8422-4656-b205-c9e665ddc8c1
author: cawrites
ms.author: chadam
ms.openlocfilehash: 7f9b06bd6805e75c053037b18fc6870ef1469500
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97642191"
---
# <a name="always-on-availability-groups-a-high-availability-and-disaster-recovery-solution"></a>Groupes de disponibilité Always On : solution de haute disponibilité et de reprise d’activité
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  La fonctionnalité [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] est une solution de haute disponibilité et de récupération d'urgence qui fournit une alternative au niveau de l'entreprise à la mise en miroir de bases de données. Introduite dans [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)], [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] optimise la disponibilité d'un ensemble de bases de données utilisateur pour l'entreprise. Un *groupe de disponibilité* prend en charge un environnement de basculement pour un ensemble discret de bases de données utilisateur, appelées *bases de données de disponibilité*, qui basculent de concert. Un groupe de disponibilité prend en charge un ensemble de bases de données primaires en lecture-écriture et un à huit ensembles de bases de données secondaires correspondantes. Éventuellement, les bases de données secondaires peuvent être rendues disponibles pour l'accès en lecture seule et/ou certaines opérations de sauvegarde.  
  
 Un groupe de disponibilité bascule au niveau d'un réplica de disponibilité. Les basculements ne sont pas dus à des problèmes de base de données, tels qu'une base de données devenant suspecte en raison de la perte d'un fichier de données, de la suppression d'une base de données ou de l'altération d'un journal des transactions.  
 
 >[!NOTE]
 >« Groupe de disponibilité Always On » est le nom complet et formel de cette fonctionnalité de disponibilité. L’abréviation est AG, et non AOAG ni AAG. 
  
##  <a name="benefits"></a><a name="Benefits"></a> Avantages  
 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] fournissent un riche ensemble d'options qui améliorent la disponibilité des bases de données et l'utilisation des ressources. Les composants clés sont les suivants :  
  
-   Prend en charge jusqu'à neuf réplicas de disponibilité. Un *réplica de disponibilité* est une instanciation d'un groupe de disponibilité hébergé par une instance spécifique de SQL Server qui conserve une copie locale de chaque base de données de disponibilité appartenant au groupe de disponibilité. Chaque groupe de disponibilité prend en charge un réplica principal et jusqu'à huit réplicas secondaires. Pour plus d’informations, consultez [Vue d’ensemble des groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md).  
  
    > [!IMPORTANT]  
    >  Chaque réplica de disponibilité doit résider sur un nœud différent d'un cluster de clustering de basculement Windows Server (WSFC). Pour plus d’informations sur les composants requis, les restrictions et les recommandations pour les groupes de disponibilité, consultez [Conditions préalables, restrictions et recommandations pour les groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md).  
  
-   Prend en charge d'autres modes de disponibilité, comme suit :  
  
    -   *Mode de validation asynchrone*. Ce mode avec validation asynchrone est une solution de récupération d'urgence qui fonctionne bien lorsque les réplicas de disponibilité sont séparés par des distances considérables.  
  
    -   *Mode de validation synchrone*. Ce mode de disponibilité privilégie la haute disponibilité et la protection des données plutôt que les performances, au prix d'une latence accrue des transactions. Un groupe de disponibilité donné peut prendre en charge jusqu'à trois réplicas de disponibilité avec validation synchrone, y compris le réplica principal actuel.  
  
     Pour plus d’informations, consultez [Modes de disponibilité &#40;groupes de disponibilité Always On&#41;](../../../database-engine/availability-groups/windows/availability-modes-always-on-availability-groups.md). 

     [!INCLUDE[sql-server-2019](../../../includes/sssqlv15-md.md)] augmente le nombre maximal de réplicas synchrones à 5, contre 3 dans [!INCLUDE[ssSQL17](../../../includes/sssql17-md.md)]. Vous pouvez configurer ce groupe de cinq réplicas de manière à instaurer le basculement automatique en son sein. Il existe un seul réplica principal, plus quatre réplicas secondaires synchrones.
  
-   Prend en charge plusieurs formes de basculement de groupe de disponibilité : le basculement automatique, le basculement manuel planifié (généralement appelé simplement « basculement manuel ») et le basculement manuel forcé (généralement appelé simplement « basculement forcé »). Pour plus d’informations, consultez [Basculement et modes de basculement &#40;groupes de disponibilité Always On&#41;](../../../database-engine/availability-groups/windows/failover-and-failover-modes-always-on-availability-groups.md).  
  
-   Vous permet de configurer un réplica de disponibilité donné pour prendre en charge l'une des deux fonctions secondaires actives suivantes :  
  
    -   Accès à la connexion en lecture seule, permettant aux connexions en lecture seule au réplica d'accéder à et lire ses bases de données lorsqu'il s'exécute comme réplica secondaire. Pour plus d’informations, consultez [Secondaires actifs : réplicas secondaires lisibles &#40;groupes de disponibilité Always On&#41;](../../../database-engine/availability-groups/windows/active-secondaries-readable-secondary-replicas-always-on-availability-groups.md).  
  
    -   Exécution d'opérations de sauvegarde sur ses bases de données lorsqu'il s'exécute comme réplica secondaire. Pour plus d’informations, consultez [Secondaires actifs : sauvegarde sur les réplicas secondaires &#40;groupes de disponibilité Always On&#41;](../../../database-engine/availability-groups/windows/active-secondaries-backup-on-secondary-replicas-always-on-availability-groups.md).  
  
     L'utilisation de fonctions secondaires actives améliore l'efficacité informatique et réduit les coûts grâce à une meilleure utilisation des ressources du matériel secondaire. En outre, le déchargement des applications de tentative de lecture et des travaux de sauvegarde vers des réplicas secondaires permet d'améliorer les performances au niveau du réplica principal.  
  
-   Prend en charge un écouteur de groupe de disponibilité pour chaque groupe de disponibilité. Un *écouteur de groupe de disponibilité* est un nom de serveur auquel les clients peuvent se connecter afin d’accéder à une base de données dans un réplica principal ou secondaire d’un groupe de disponibilité Always On. Les écouteurs de groupe de disponibilité dirigent les connexions entrantes vers un réplica principal ou un réplica secondaire en lecture seule. L'écouteur fournit un basculement d'application rapide après le basculement d'un groupe de disponibilité. Pour plus d’informations, consultez [Écouteurs de groupe de disponibilité, connectivité client et basculement d’application &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md).  
  
-   Prend en charge une stratégie de basculement flexible pour un contrôle optimisé du basculement de cluster de disponibilité. Pour plus d’informations, consultez [Basculement et modes de basculement &#40;groupes de disponibilité Always On&#41;](../../../database-engine/availability-groups/windows/failover-and-failover-modes-always-on-availability-groups.md).  
  
-   Prend en charge la réparation de page automatique pour éviter les pages endommagées. Pour plus d'informations, consultez [Automatic Page Repair &#40;Availability Groups: Database Mirroring&#41;](../../../sql-server/failover-clusters/automatic-page-repair-availability-groups-database-mirroring.md).  
  
-   Prend en charge le chiffrement et la compression, qui fournissent un transport sécurisé et efficace.  
  
-   Fournit un jeu intégré d'outils pour simplifier le déploiement et la gestion de groupes de disponibilité, notamment :  
  
    -   [!INCLUDE[tsql](../../../includes/tsql-md.md)] pour créer et gérer les groupes de disponibilité. Pour plus d’informations, consultez [Vue d’ensemble des instructions Transact-SQL pour les groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/transact-sql-statements-for-always-on-availability-groups.md).  
  
    -   [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] , comme suit :  
  
        -   L' [!INCLUDE[ssAoNewAgWiz](../../../includes/ssaonewagwiz-md.md)] crée et configure un groupe de disponibilité. Dans certains environnements, cet Assistant peut également préparer automatiquement les bases de données secondaires et démarrer la synchronisation de données pour chacune d'elles. Pour plus d’informations, consultez [Utiliser la boîte de dialogue Nouveau groupe de disponibilité &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-new-availability-group-dialog-box-sql-server-management-studio.md).  
  
        -   L'[!INCLUDE[ssAoAddDbWiz](../../../includes/ssaoadddbwiz-md.md)] ajoute une ou plusieurs bases de données primaires à un groupe de disponibilité existant. Dans certains environnements, cet Assistant peut également préparer automatiquement les bases de données secondaires et démarrer la synchronisation de données pour chacune d'elles. Pour plus d’informations, consultez [Utiliser l’Assistant Ajouter une base de données au groupe de disponibilité (SQL Server)](../../../database-engine/availability-groups/windows/availability-group-add-database-to-group-wizard.md).  
  
        -   L' [!INCLUDE[ssAoAddRepWiz](../../../includes/ssaoaddrepwiz-md.md)] ajoute un ou plusieurs réplicas secondaires à un groupe de disponibilité existant. Dans certains environnements, cet Assistant peut également préparer automatiquement les bases de données secondaires et démarrer la synchronisation de données pour chacune d'elles. Pour plus d’informations, consultez [Utiliser l’Assistant Ajouter un réplica au groupe de disponibilité &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-add-replica-to-availability-group-wizard-sql-server-management-studio.md).  
  
        -   L'[!INCLUDE[ssAoFoAgWiz](../../../includes/ssaofoagwiz-md.md)] démarre un basculement manuel sur un groupe de disponibilité. En fonction de la configuration et de l'état du réplica secondaire que vous spécifiez comme cible de basculement, l'Assistant peut effectuer un basculement manuel planifié ou forcé. Pour plus d’informations, consultez [Utiliser l’Assistant Basculer le groupe de disponibilité &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-fail-over-availability-group-wizard-sql-server-management-studio.md).  
  
    -   Le [!INCLUDE[ssAoDash](../../../includes/ssaodash-md.md)] surveille les groupes de disponibilité, les réplicas de disponibilité et les bases de données de disponibilité Always On, et évalue les résultats des stratégies Always On. Pour plus d’informations, consultez [Utiliser le tableau de bord Always On &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-always-on-dashboard-sql-server-management-studio.md).  
  
    -   Le volet Détails de l'Explorateur d'objets affiche des informations de base à propos des groupes de disponibilité existants. Pour plus d’informations, consultez [Utiliser les détails de l’Explorateur d’objets pour surveiller les groupes de disponibilité &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-object-explorer-details-to-monitor-availability-groups.md).  
  
    -   Applets de commande PowerShell. Pour plus d’informations, consultez [Vue d’ensemble des applets de commande PowerShell pour les groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-powershell-cmdlets-for-always-on-availability-groups-sql-server.md).  
  
##  <a name="terms-and-definitions"></a><a name="TermsAndDefinitions"></a> Termes et définitions  
 **groupe de disponibilité**  
 Conteneur d’un ensemble de bases de données ( *bases de données de disponibilité*) qui basculent ensemble.  
  
 **base de données de disponibilité**  
 Base de données qui appartient à un groupe de disponibilité. Pour chaque base de données de disponibilité, le groupe de disponibilité conserve une seule copie en lecture-écriture (la *base de données primaire*) et une à huit copies en lecture seule (les *bases de données secondaires*).  
  
 **base de données primaire**  
 Copie en lecture-écriture d'une base de données de disponibilité.  
  
 **base de données secondaire**  
 Copie en lecture seule d'une base de données de disponibilité.  
  
 **réplica de disponibilité**  
 Instanciation d'un groupe de disponibilité hébergé par une instance spécifique de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] et qui conserve une copie locale de chaque base de données de disponibilité appartenant au groupe de disponibilité. Il existe deux types de réplicas de disponibilité : un seul *réplica principal* et un à huit *réplicas secondaires*.  
  
 **réplica principal**  
 Réplica de disponibilité qui rend les bases de données primaires disponibles pour les connexions en lecture-écriture à partir des clients et envoie également des enregistrements du journal des transactions pour chaque base de données primaire à chaque réplica secondaire.  
  
 **réplica secondaire**  
 Réplica de disponibilité qui conserve une copie secondaire de chaque base de données de disponibilité, et sert de cible potentielle d'un basculement du groupe de disponibilité. Éventuellement, un réplica secondaire peut prendre en charge l'accès en lecture seule et la création de sauvegardes sur des bases de données secondaires.  
  
 **écouteur de groupe de disponibilité**  
 Nom du serveur auquel les clients peuvent se connecter afin d’accéder à une base de données dans un réplica principal ou secondaire d’un groupe de disponibilité Always On. Les écouteurs de groupe de disponibilité dirigent les connexions entrantes vers un réplica principal ou un réplica secondaire en lecture seule.  
  
> [!NOTE]  
>  Pour plus d’informations, consultez [Vue d’ensemble des groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md).  
  
##  <a name="interoperability-and-coexistence-with-other-database-engine-features"></a><a name="Interoperability"></a> Interopérabilité et coexistence avec d'autres fonctionnalités de moteur de base de données  
 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] peuvent être utilisés avec les fonctionnalités ou les composants [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]suivants :  
  
-   [À propos de la capture de données modifiées &#40;SQL Server&#41;](../../../relational-databases/track-changes/about-change-data-capture-sql-server.md)  
  
-   [À propos du suivi des modifications &#40;SQL Server&#41;](../../../relational-databases/track-changes/about-change-tracking-sql-server.md)  
  
-   [Bases de données autonomes](../../../relational-databases/databases/contained-databases.md)  
  
-   [Chiffrement de base de données](../../../relational-databases/security/encryption/transparent-data-encryption.md)  
  
-   [Instantanés de base de données](../../../database-engine/availability-groups/windows/database-snapshots-with-always-on-availability-groups-sql-server.md)  
  
-   [FILESTREAM](../../../relational-databases/blob/filestream-sql-server.md)  
  
-   [FileTable](../../../relational-databases/blob/filetables-sql-server.md)  
  
-   [Copie des journaux de transaction](../../../database-engine/log-shipping/about-log-shipping-sql-server.md)  
  
-   [Magasin d'objets blob distants (RBS)](../../../relational-databases/blob/remote-blob-store-rbs-sql-server.md)  
  
-   [Réplication](../../../relational-databases/replication/sql-server-replication.md)  
  
-   [Service Broker](../../../database-engine/configure-windows/sql-server-service-broker.md)  
  
-   [SQL Server Agent](../../../ssms/agent/sql-server-agent.md)  
  
-   [Reporting Services](../../../database-engine/availability-groups/windows/reporting-services-with-always-on-availability-groups-sql-server.md)  
  
> [!WARNING]  
>  Pour plus d’informations sur les restrictions et les limitations d’utilisation d’autres fonctionnalités avec les [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)], consultez [Groupes de disponibilité Always On : interopérabilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/always-on-availability-groups-interoperability-sql-server.md).  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Tâches associées  
  
-   [Commencer à utiliser les groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/getting-started-with-always-on-availability-groups-sql-server.md)  
  
##  <a name="related-content"></a><a name="RelatedContent"></a> Contenu associé  
  
-   **Blogs :**  
  
     [Blogs de l’équipe de SQL Server Always On : Blog officiel de l’équipe de SQL Server Always On](/archive/blogs/sqlalwayson/)  
  
     [Blogs des ingénieurs du Service clientèle et du Support technique de SQL Server](/archive/blogs/psssql/)  
  
-   **Vidéos :**  
  
     [Microsoft SQL Server Code-Named "Denali" Always On Series,Part 1: Introducing the Next Generation High Availability Solution (Présentation de la solution haute disponibilité de nouvelle génération)](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2011/DBI302)  
  
     [Microsoft SQL Server Code-Named "Denali" Always On Series,Part 2: Building a Mission-Critical High Availability Solution Using Always On (Génération d’une solution haute disponibilité critique à l’aide d’Always On)](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2011/DBI404)  
  
-   **Livres blancs :**  
  
     [Guide de solutions Microsoft SQL Server Always On pour la haute disponibilité et la récupération d’urgence](/previous-versions/sql/sql-server-2012/hh781257(v=msdn.10))  
  
     [Livres blancs de Microsoft pour SQL Server 2012](https://social.technet.microsoft.com/wiki/contents/articles/13146.white-paper-gallery-for-sql-server.aspx#[Category]SQLServer2012)  
  
     [Livres blancs de l'équipe de consultants clients de SQL Server](https://techcommunity.microsoft.com/t5/DataCAT/bg-p/DataCAT/)  
  
## <a name="see-also"></a>Voir aussi  
 [Vue d’ensemble des groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Conditions préalables requises, restrictions et recommandations pour les groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)   
 [Configuration d’une instance de serveur pour les groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configuration-of-a-server-instance-for-always-on-availability-groups-sql-server.md)   
 [Création et configuration des groupes de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/creation-and-configuration-of-availability-groups-sql-server.md)   
 [Administration d’un groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/administration-of-an-availability-group-sql-server.md)   
 [Surveillance des groupes de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/monitoring-of-availability-groups-sql-server.md)   
 [Vue d’ensemble des instructions Transact-SQL pour les groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/transact-sql-statements-for-always-on-availability-groups.md)   
 [Vue d’ensemble des applets de commande PowerShell pour les groupes de disponibilité Always On](../../../database-engine/availability-groups/windows/overview-of-powershell-cmdlets-for-always-on-availability-groups-sql-server.md)  
  
