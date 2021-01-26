---
title: Utilisation du tableau de bord de groupe de disponibilité dans SSMS
description: Explique comment utiliser le tableau de bord de groupe de disponibilité Always On pour superviser l’intégrité de votre groupe de disponibilité dans SSMS (SQL Server Management Studio).
ms.custom: seo-lt-2019
ms.date: 08/09/2018
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
f1_keywords:
- sql13.swb.agdashboard.f1
helpviewer_keywords:
- Availability Groups [SQL Server], policies
- Availability Groups [SQL Server], dashboard
ms.assetid: c9ba2589-139e-42bc-99e1-94546717c64d
author: cawrites
ms.author: chadam
ms.openlocfilehash: ea85d4be41b5f7dd5c4619cd4e2f6dd315cc3a99
ms.sourcegitcommit: 2f3f5920e0b7a84135c6553db6388faf8e0abe67
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/26/2021
ms.locfileid: "98783066"
---
# <a name="use-the-always-on-availability-group-dashboard-sql-server-management-studio"></a>Utiliser le tableau de bord du groupe de disponibilité Always On (SQL Server Management Studio)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  Les administrateurs de base de données utilisent le tableau de bord du groupe de disponibilité Always On pour obtenir un aperçu rapide de l’intégrité d’un groupe de disponibilité, de ses réplicas de disponibilité et de ses bases de données dans [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)]. Voici quelques-unes des utilisations courantes du tableau de bord du groupe de disponibilité :  
  
-   Choix d'un réplica pour un basculement manuel.    
-   Estimation de la perte de données en cas de basculement forcé.  
-   Évaluation des performances de synchronisation des données.   
-   Évaluation de l'impact sur les performances d'un réplica secondaire avec validation synchrone 
  -  Le tableau de bord fournit les états principaux et les indicateurs de performance du groupe de disponibilité, et vous permet ainsi de prendre facilement des décisions opérationnelles en matière de haute disponibilité sur la base des types d’informations suivants.  
-   État de restauration de réplica    
-   Mode et état de synchronisation   
-   Estimer la perte de données    
-   Durée estimée de récupération (rattrapage de la phase de restauration par progression)    
-   Détails du réplica de base de données    
-   Mode et état de synchronisation    
-   Durée de restauration du journal  
  
  
## <a name="prerequisites"></a>Prérequis  
 Vous devez être connecté à l'instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] (instance de serveur) qui héberge soit le réplica principal, soit un réplica secondaire d'un groupe de disponibilité.  
  
 
### <a name="permissions"></a>Autorisations  
 Requiert les autorisations CONNECT, VIEW SERVER STATE et VIEW ANY DEFINITION.  
  
##  <a name="to-start-the-always-on-dashboard"></a>Pour démarrer le tableau de bord Always On  
  
1.  Dans l’Explorateur d’objets, connectez-vous à l’instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] sur laquelle vous souhaitez exécuter le tableau de bord Always On.  
  
2.  Développez le nœud **Haute disponibilité Always On** , cliquez avec le bouton droit sur le nœud **Groupes de disponibilité** , puis cliquez sur **Afficher le tableau de bord**.  
  
##  <a name="change-always-on-dashboard-options"></a>Changer les options du tableau de bord Always On  
 Vous pouvez utiliser la boîte de dialogue **Options de [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)]** pour configurer le comportement du tableau de bord [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Always On en ce qui concerne l’actualisation automatique et l’activation d’une stratégie Always On définie automatiquement.  
  
1.  Dans le menu **Outils** , cliquez sur **Options**.  
  
2.  Pour actualiser automatiquement le tableau de bord, dans la boîte de dialogue **Options** , sélectionnez **Activer l'actualisation automatique**, entrez l'intervalle d'actualisation en secondes, puis entrez le nombre de fois où vous souhaitez réessayer la connexion.  
  
3.  Pour activer une stratégie définie par l’utilisateur, sélectionnez **Activer la stratégie Always On définie par l’utilisateur**.  
  
##  <a name="availability-group-summary"></a>Résumé des groupes de disponibilité  
 L'écran de groupe de disponibilité affiche une ligne de résumé pour chaque groupe de disponibilité pour lequel l'instance de serveur connectée héberge un réplica. Ce volet inclut les colonnes suivantes.  
  
 **Nom du groupe de disponibilité**  
 Nom d'un groupe de disponibilité pour lequel l'instance de serveur connectée héberge un réplica.  
  
 **Instance principale**  
 Nom de l'instance de serveur qui héberge le réplica principal du groupe de disponibilité.  
  
 **Mode de basculement**  
 Affiche le mode de basculement pour lequel le réplica est configuré. Les valeurs possibles pour le mode de basculement sont les suivantes :  
  
-   **Automatique**. Indique qu'un ou plusieurs réplicas se trouvent en mode de basculement automatique.  
  
-   **Manuel**. Indique qu'aucun réplica n'est en mode de basculement automatique.  
  
 **Problèmes**  
 Cliquez sur le lien **Problèmes** pour ouvrir la documentation de dépannage relative à un problème donné. Pour obtenir la liste de tous les problèmes de stratégie Always On, consultez [Stratégies Always On pour les problèmes opérationnels avec des groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/always-on-policies-for-operational-issues-always-on-availability.md).  
  
> [!TIP]  
>  Cliquez sur les en-têtes de colonne pour trier les informations de groupe de disponibilité selon le nom du groupe de disponibilité, l'instance principale, le mode de basculement ou le problème.  
  
##  <a name="availability-group-details"></a><a name="AvGroupDetails"></a> Détails du groupe de disponibilité  
 Les informations détaillées suivantes sont affichées pour le groupe de disponibilité que vous sélectionnez dans l'écran récapitulatif :  
  
 **État du groupe de disponibilité**  
 Affiche l'état d'intégrité du groupe de disponibilité.  
  
 **Primary instance**  
 Nom de l'instance de serveur qui héberge le réplica principal du groupe de disponibilité.  
  
 **Failover mode**  
 Affiche le mode de basculement pour lequel le réplica est configuré. Les valeurs possibles pour le mode de basculement sont les suivantes :  
  
-   **Automatique**. Indique qu'un ou plusieurs réplicas se trouvent en mode de basculement automatique.  
  
-   **Manuel**. Indique qu'aucun réplica n'est en mode de basculement automatique.  
  
 **État du cluster**  
 Nom et état du cluster dans lequel l'instance du serveur connecté et du groupe de disponibilité est un nœud membre.  
  
##  <a name="availability-replica-details"></a><a name="AvReplicaDetails"></a> Détails du réplica de disponibilité  

Quand vous êtes connecté au réplica principal, **Détails du réplica de disponibilité** affiche des informations issues de tous les réplicas du groupe de disponibilité. Quand vous êtes connecté à un réplica secondaire, seules apparaissent les informations issues du réplica connecté.  

Le volet **Réplica de disponibilité** affiche les colonnes suivantes :  
  
 **Nom**  
 Nom de l'instance du serveur qui héberge le réplica de disponibilité. Cette colonne est affichée par défaut.  
  
 **Rôle**  
 Indique le rôle actuel du réplica de disponibilité, à savoir **Principal** ou **Secondaire**. Pour plus d’informations sur les rôles des [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)], consultez [Vue d’ensemble des groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md). Cette colonne est affichée par défaut.  
  
 **Mode de basculement**  
 Affiche le mode de basculement pour lequel le réplica est configuré. Les valeurs possibles pour le mode de basculement sont les suivantes :  
  
-   **Automatique**. Indique qu'un ou plusieurs réplicas se trouvent en mode de basculement automatique.  
  
-   **Manuel**. Indique qu'aucun réplica n'est en mode de basculement automatique.  
  
 **État de synchronisation**  
 Indique si un réplica secondaire est actuellement synchronisé avec le réplica principal. Cette colonne est affichée par défaut. Les valeurs possibles sont les suivantes :  
  
-   **Non synchronisé**. Une ou plusieurs bases de données du réplica ne sont pas synchronisées ou n'ont pas encore été jointes au groupe de disponibilité.  
  
-   **Synchronisation**. Une ou plusieurs bases de données du réplica sont synchronisées.  
  
-   **Synchronisé**. Toutes les bases de données du réplica secondaire sont synchronisées avec les base de données primaires correspondantes sur le réplica principal actuel, le cas échéant, ou sur le dernier réplica principal.  
  
    > [!NOTE]  
    >  En mode de performances, la base de données n'est jamais dans l'état synchronisé.  
  
-   **NULL**. État inconnu. Cette valeur se produit lorsque l'instance de serveur locale ne peut pas communiquer avec le cluster de basculement WSFC (le nœud local ne fait pas partie du quorum WSFC).  
  
 **Problèmes**  
 Énonce le nom du problème. Cette valeur est affichée par défaut. Pour obtenir la liste de tous les problèmes de stratégie Always On, consultez [Stratégies Always On pour les problèmes opérationnels avec des groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/always-on-policies-for-operational-issues-always-on-availability.md).  
  
 **Mode de disponibilité**  
 Indique la propriété de réplica que vous définissez séparément pour chaque réplica de disponibilité. Cette valeur est masquée par défaut. Les valeurs possibles sont les suivantes :  
  
-   **Asynchrone**. Le réplica secondaire n'est jamais synchronisé avec le réplica principal.  
  
-   **Synchronous**. Lors du rattrapage du retard par rapport à la base de données primaire, une base de données secondaire entre dans cet état, mais reste en retard tant que la synchronisation des données se poursuit pour la base de données.  
  
 **Mode de connexion principal**  
 Indique le mode utilisé pour se connecter au réplica principal.  Cette valeur est masquée par défaut.  
  
 **Mode de connexion secondaire**  
 Indique le mode utilisé pour se connecter au réplica secondaire.  Cette valeur est masquée par défaut.  
  
 **État de la connexion**  
 Indique si un réplica secondaire est actuellement connecté au réplica principal. Cette colonne est masquée par défaut. Les valeurs possibles sont les suivantes :  
  
-   **Déconnecté**. Pour un réplica de disponibilité distant, indique qu'il est déconnecté du réplica de disponibilité local. La réponse du réplica local à l'état déconnecté dépend de son rôle, comme suit :  
  
    -   Sur le réplica principal, si un réplica secondaire est déconnecté, les bases de données secondaires sont marquées comme **Non synchronisé** sur le réplica principal, et le réplica principal attend que le réplica secondaire se reconnecte.  
  
    -   Une fois la déconnexion détectée, le réplica secondaire tente de se reconnecter au réplica principal.  
  
-   **Connected**. Réplica de disponibilité distant qui est actuellement connecté au réplica local.  
  
 **État opérationnel**  
 Indique l'état opérationnel actuel du réplica secondaire. Cette valeur est masquée par défaut. Les valeurs possibles sont les suivantes :  
  
 **0**. Basculement en attente    
 **1**. Pending    
 **2**. En ligne    
 **3**. Hors connexion   
 **4**. Échec    
 **5**. Échec, pas de quorum  
  
 **NULL**. Le réplica n'est pas local  
  
 **Numéro de la dernière erreur de connexion**  
 Numéro de la dernière erreur de connexion.  Cette valeur est masquée par défaut.  
  
 **Description de la dernière erreur de connexion**  
 Description de la dernière erreur de connexion.  Cette valeur est masquée par défaut.  
  
 **Horodateur de la dernière erreur de connexion**  
 Horodateur de la dernière erreur de connexion. Cette valeur est masquée par défaut.  
  
> [!NOTE]  
>  Pour plus d’informations sur les compteurs de performances pour les réplicas de disponibilité, consultez [SQL Server, réplica de disponibilité](../../../relational-databases/performance-monitor/sql-server-availability-replica.md).  
  
##  <a name="group-by-availability-group-information"></a>Regrouper les informations du groupe de disponibilité  
 Pour regrouper les informations, cliquez sur **Regrouper par**, puis sélectionnez l'une des commandes suivantes :  
  
-   **Réplicas de disponibilité**    
-   **Bases de données de disponibilité** 
-   **Synchronization state**  
-   **Disponibilité de basculement**   
-   **Problèmes**  
  
 Le volet qui affiche les informations regroupées comporte les colonnes suivantes :  
  
 **Nom**  
 Nom de la base de données de disponibilité. Cette valeur est affichée par défaut.  
  
 **Réplica**  
 Nom de l'instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] qui héberge le réplica de disponibilité. Cette valeur est affichée par défaut.  
  
 **État de synchronisation**  
 Indique si la base de données de disponibilité est actuellement synchronisée avec le réplica principal. Cette valeur est affichée par défaut. Les états de synchronisation possibles sont les suivants :  
  
-   **Sans synchronisation** :  
-   
    -   Pour le rôle principal, indique que la base de données n'est pas prête à synchroniser son journal des transactions avec les bases de données secondaires correspondantes.   
    -   Pour une base de données secondaire, indique que la base de données n'a pas commencé la synchronisation du journal en raison d'un problème de connexion, est suspendue, ou passe par des états de transition pendant le démarrage ou lors d'un changement de rôle.  
  
-   **Avec synchronisation** :
- Sur un réplica principal :   
    - Sur une base de données primaire, indique que cette base de données est prête à recevoir une demande d’analyse d’une base de données secondaire.  
    - Sur un réplica secondaire, indique qu'il existe un déplacement des données actif pour cette base de données secondaire. 
  
  
-   **Synchronisé** : 
  
    - Pour une base de données primaire, indique qu'au moins une base de données secondaire est synchronisée.
    - Pour une base de données secondaire, indique que la base de données est synchronisée avec la base de données primaire correspondante.  
  
-   **Rétablissement**.  
  
     Indique la phase dans le processus de restauration lorsqu'une base de données secondaire obtient activement les pages de la base de données primaire.  
  
    > [!CAUTION]  
    >  Lorsqu'une base de données est dans l'état REVERTING, forcer le basculement vers le réplica secondaire peut laisser cette base de données dans un état dans lequel elle ne peut pas être démarrée.  
  
-   **Initialisation en cours**.  
  
     Indique la phase de restauration lorsque le journal des transactions requis pour qu'une base de données secondaire rattrape le retard du numéro séquentiel de restauration dans le journal est livré et renforcé sur un réplica secondaire.  
  
    > [!CAUTION]  
    >  Lorsqu'une base de données est dans l'état INITIALIZING, forcer le basculement vers le réplica secondaire laisse toujours cette base de données dans un état dans lequel elle ne peut pas être démarrée.  
  
 **Disponibilité du basculement**  
 Indique le réplica de disponibilité qui peut basculer avec ou sans perte possible de données. Cette colonne est affichée par défaut. Les valeurs possibles sont les suivantes :  
  
-   **Perte de données**   
-   **Aucune perte de données**  
  
 **Problèmes**  
 Énonce le nom du problème. Cette colonne est affichée par défaut. Les valeurs possibles sont les suivantes :  
  
-   **Avertissements**. Cliquez pour afficher les problèmes d'avertissements et de seuils.   
-   **Critique**. Cliquez pour afficher les problèmes critiques.  
  
 Pour obtenir la liste de tous les problèmes de stratégie Always On, consultez [Stratégies Always On pour les problèmes opérationnels avec des groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/always-on-policies-for-operational-issues-always-on-availability.md).  
  
 **Suspendu**  
 Indique si la base de données est à l’état **Suspendu** ou si elle a été **reprise**. Cette valeur est masquée par défaut.  
  
 **Motif de suspension**  
 Indique le motif de l'état suspendu. Cette valeur est masquée par défaut.  
  
 **Estimer la perte de données (secondes)**  
 Indique la différence de temps du dernier enregistrement des journaux de transactions dans le réplica principal et le réplica secondaire. Si le réplica principal échoue, tous les enregistrements du journal des transactions se trouvant dans cette fenêtre temporelle seront perdus. Cette valeur est masquée par défaut.  
  
 **Durée estimée de récupération (secondes)**  
 Indique la durée (en secondes) nécessaire pour rétablir le temps de rattrapage. Le *temps de rattrapage* correspond au temps nécessaire pour que le réplica secondaire rattrape le réplica principal. Cette valeur est masquée par défaut.  
  
 **Performances de synchronisation (secondes)**  
 Indique le temps (en secondes) nécessaire pour effectuer la synchronisation entre le réplica principal et le réplica secondaire. Cette valeur est masquée par défaut.  
  
 **Taille de la file d'attente d'envoi du journal (Ko)**  
 Indique le nombre d’enregistrements de journal dans les fichiers journaux de la base de données primaire qui n’ont pas été envoyés au réplica secondaire. Cette valeur est masquée par défaut.  
  
 **Débit d'envoi du journal (Ko/s)**  
 Indique le débit, en Ko par seconde, auquel les enregistrements de journal sont envoyés au réplica secondaire. Cette valeur est masquée par défaut.  
  
 **Taille de la file d'attente de restauration par progression (Ko)**  
 Indique le nombre d’enregistrements du journal dans les fichiers journaux du réplica secondaire qui n’ont pas encore été restaurés. Cette valeur est masquée par défaut.  
  
 **Débit de la restauration par progression (Ko/s)**  
 Indique le débit, en Ko par seconde, auquel les enregistrements de journal sont restaurés. Cette valeur est masquée par défaut.  
  
 **Vitesse d'envoi de flux de fichiers (Ko/s)**  
 Indique le débit de FileStream, en Ko par seconde, auquel les transactions sont envoyées au réplica. Cette valeur est masquée par défaut.  
  
 **LSN de fin du journal**  
 Indique le numéro séquentiel dans le journal (LSN) réel qui correspond au dernier enregistrement du journal dans le cache du journal sur les réplicas principal et secondaire. Cette valeur est masquée par défaut.  
  
 **LSN de récupération**  
 Indique la fin du journal des transactions avant que le réplica n'écrive de nouveaux enregistrements de journal après la récupération ou le basculement sur le réplica principal. Cette valeur est masquée par défaut.  
  
 **LSN de troncation**  
 Indique la valeur minimale de troncation du journal du réplica principal. Cette valeur est masquée par défaut.  
  
 **LSN de dernière validation**  
 Indique le numéro séquentiel dans le journal réel correspondant au dernier enregistrement de validation dans le journal des transactions. Cette valeur est masquée par défaut.  
  
 **Heure de dernière validation**  
 Indique l'heure correspondant au dernier enregistrement de validation. Cette valeur est masquée par défaut.  
  
 **LSN de dernier envoi**  
 Indique le point jusqu'auquel tous les blocs de journal ont été envoyés par le réplica principal. Cette valeur est masquée par défaut.  
  
 **Heure de dernier envoi**  
 Indique l'heure à laquelle le dernier bloc du journal a été envoyé. Cette valeur est masquée par défaut.  
  
 **LSN de dernière réception**  
 Indique le point jusqu'auquel tous les blocs de journal ont été reçus par le réplica secondaire qui héberge la base de données secondaire. Cette valeur est masquée par défaut.  
  
 **Heure de dernière réception**  
 Indique l'heure à laquelle l'identificateur de bloc de journal du dernier message reçu a été lu sur le réplica secondaire. Cette valeur est masquée par défaut.  
  
 **LSN de dernière sécurisation renforcée**  
 Indique le point jusqu'auquel tous les enregistrements de journal ont été vidés sur le disque du réplica secondaire. Cette valeur est masquée par défaut.  
  
 **Heure de dernière sécurisation renforcée**  
 Indique l'heure à laquelle l'identificateur de bloc de journal a été reçu pour le LSN de dernière sécurisation renforcée sur le réplica secondaire. Cette valeur est masquée par défaut.  
  
 **LSN de dernière restauration par progression**  
 Indique le numéro séquentiel réel dans l'enregistrement du journal qui a été reconstruit pour la dernière fois sur le réplica secondaire. Cette valeur est masquée par défaut.  
  
 **Heure de dernière restauration par progression**  
 Indique l'heure à laquelle le dernier enregistrement du journal a été restauré sur la base de données secondaire. Cette valeur est masquée par défaut.  
 

   > [!NOTE]  
   >  La plupart des données repose sur sys.dm_hadr_database_replica_states, donc certaines restrictions peuvent s’appliquer. Pour plus d’informations, consultez [sys.dm_hadr_database_replica_states (Transact-SQL)](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-states-transact-sql.md).


## <a name="always-on-availability-group-latency-reports"></a>Rapports de latence du groupe de disponibilité Always On
Le rapport de latence du groupe de disponibilité est un outil de rapport basé dans le tableau de bord du groupe de disponibilité et disponible dans la version [SQL Server Management Studio 17.4](../../../ssms/download-sql-server-management-studio-ssms.md). Cette fonctionnalité fournit un rapport facile à comprendre qui détaille le temps passé dans les différentes phases du processus de transport de journal. Elle constitue un moyen de limiter la cause potentielle de la latence pendant le processus de synchronisation. 

SQL Agent exécute la collecte de données et doit être activé sur le réplica principal et au moins sur l’un des réplicas secondaires. Affichez le rapport en cliquant avec le bouton droit sur le groupe de disponibilité > Rapports > Rapports standard dans l’**Explorateur d’objets** de SQL Server Management Studio.  

Pour plus d’informations, consultez [Rapports de latence du groupe de disponibilité AlwaysOn (SQL Server)](/archive/blogs/sql_server_team/new-in-ssms-always-on-availability-group-latency-reports).

## <a name="related-tasks"></a>Tâches associées  
  
-   [Utiliser les stratégies Always On pour afficher l’intégrité d’un groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/use-always-on-policies-to-view-the-health-of-an-availability-group-sql-server.md)  
  
## <a name="see-also"></a>Voir aussi  
 [sys.dm_os_performance_counters &#40;Transact-SQL&#41;](../../../relational-databases/system-dynamic-management-views/sys-dm-os-performance-counters-transact-sql.md)   
 [Outils pour superviser les groupes de disponibilité Always On](../../../database-engine/availability-groups/windows/monitoring-of-availability-groups-sql-server.md)  
  
