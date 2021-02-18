---
title: Reprise d’activité par le quorum forcé
description: Une intervention manuelle est nécessaire pour la récupération d'une défaillance de quorum. Découvrez comment forcer le quorum en cas d’incident avec une instance de cluster de basculement SQL Server.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: failover-cluster-instance
ms.topic: how-to
helpviewer_keywords:
- Availability Groups [SQL Server], WSFC clusters
- quorum [SQL Server], AlwaysOn and WSFC quorum
- failover clustering [SQL Server], AlwaysOn Availability Groups
ms.assetid: 6cefdc18-899e-410c-9ae4-d6080f724046
author: cawrites
ms.author: chadam
ms.openlocfilehash: 2358f1f57a7d6b72f97daf4c27963436a40adfca
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100355234"
---
# <a name="wsfc-disaster-recovery-through-forced-quorum-sql-server"></a>Récupération d'urgence WSFC par le quorum forcé (SQL Server)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  L'échec du quorum est généralement dû à un problème systémique grave, à un échec de communication persistant ou à une mauvaise configuration impliquant plusieurs nœuds dans le cluster WSFC.  Une intervention manuelle est nécessaire pour la récupération d'une défaillance de quorum.  
  
-   **Avant de commencer :**  [Prérequis](#Prerequisites), [Sécurité](#Security)  
  
-   **Procédure de récupération d'urgence WSFC par le quorum forcé**[Procédure de récupération d'urgence WSFC par le quorum forcé](#Main)  
  
-   [Tâches associées](#RelatedTasks)  
  
-   [Contenu connexe](#RelatedContent)  
  
##  <a name="before-you-start"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="prerequisites"></a><a name="Prerequisites"></a> Conditions préalables  
 La procédure de quorum forcé suppose qu'un quorum sain existait avant l'échec de quorum.  
  
> [!WARNING]  
>  L'utilisateur doit bien connaître les concepts et les interactions du clustering de basculement Windows Server, des modèles de quorum WSFC, de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]et de la configuration de déploiement spécifique à l'environnement.  
>   
>  Pour plus d'informations, consultez les pages suivantes :  [Clustering de basculement Windows Server (WSFC) avec SQL Server](https://msdn.microsoft.com/library/hh270278\(v=SQL.110\).aspx), [Modes de quorum WSFC et configuration de vote (SQL Server)](https://msdn.microsoft.com/library/hh270280\(v=SQL.110\).aspx)  
  
###  <a name="security"></a><a name="Security"></a> Sécurité  
 L'utilisateur doit être un compte de domaine qui est membre du groupe Administrateurs local sur chaque nœud du cluster WSFC.  
  
##  <a name="wsfc-disaster-recovery-through-the-forced-quorum-procedure"></a><a name="Main"></a> Récupération d'urgence WSFC par le quorum forcé  
 N'oubliez pas qu'un échec de quorum met hors ligne tous les services cluster, toutes les instances SQL Server et [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]dans le cluster WSFC, car le cluster, tel que configuré, ne peut pas garantir la tolérance de panne au niveau du nœud.  Un échec de quorum signifie que les nœuds votants sains dans le cluster WSFC ne satisfont plus le modèle de quorum. Certains nœuds ont peut-être échoué complètement, et d'autres ont peut-être simplement arrêté le service WSFC et sont sains par ailleurs, sauf en ce qui concerne la perte de la capacité de communiquer avec un quorum.  
  
 Pour remettre le cluster WSFC en ligne, vous devez corriger la cause première de l'échec de quorum dans la configuration existante, récupérer les bases de données concernées si nécessaire et, éventuellement, reconfigurer les nœuds restants dans le cluster WSFC pour refléter la topologie de cluster survivante.  
  
 Vous pouvez utiliser la procédure de *quorum forcé* sur un nœud de cluster WSFC pour remplacer les contrôles de sécurité qui ont mis le cluster hors connexion.  Cela est efficace pour indiquer au cluster d'interrompre les contrôles de vote du quorum et vous permet de remettre en ligne les ressources du cluster WSFC et SQL Server sur tous les nœuds dans le cluster.  
  
 Ce type de processus de récupération d'urgence doit inclure les étapes suivantes :  
  
#### <a name="to-recover-from-quorum-failure"></a>Pour une récupération en cas d'échec de quorum :  
  
1.  **Déterminez l'étendue de l'échec.** Identifiez les groupes de disponibilité ou les instances de SQL Server non sensibles et les nœuds du cluster qui sont en ligne et disponibles à l'utilisation post-incident, puis examinez les journaux des événements Windows et les journaux système de SQL Server.  Si possible, vous devez conserver les données d'analyse et les journaux système pour les examiner ultérieurement.  
  
    > [!TIP]  
    >  Sur une instance de [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)]qui répond, vous pouvez obtenir des informations sur l’état d’intégrité des groupes de disponibilité qui possèdent un réplica de disponibilité sur l’instance de serveur local en interrogeant la vue de gestion dynamique (DMV) [sys.dm_hadr_availability_group_states](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-group-states-transact-sql.md) .  
  
2.  **Démarrez le cluster WSFC en utilisant le quorum forcé sur un nœud unique.** Identifiez un nœud avec un nombre minime de défaillances de composant, autre que celui qui a arrêté le service de cluster WSFC.  Vérifiez que ce nœud peut communiquer avec la majorité des autres nœuds.  
  
     Sur ce nœud, forcez manuellement la mise en ligne du cluster à l'aide de la procédure de quorum forcé.  Pour réduire au minimum la perte de données, sélectionnez le dernier nœud qui hébergeait un réplica principal de groupe de disponibilité.  
  
     Pour plus d'informations, consultez les pages suivantes :  [Forcer un cluster WSFC à démarrer sans quorum](https://msdn.microsoft.com/library/hh270275\(v=SQL.110\).aspx)  
  
    > [!NOTE]  
    >  L'application d'un quorum forcé bloque les contrôles de quorum sur l'ensemble du cluster jusqu'à ce que le cluster WSFC logique atteigne une majorité des votes et passe automatiquement à un mode d'opération de quorum standard.  
  
3.  **Démarrez le service WSFC normalement sur chaque nœud par ailleurs sain, en procédant avec un nœud à la fois.** Vous ne devez pas spécifier l'option de quorum forcé lorsque vous démarrez le service de cluster sur les autres nœuds.  
  
     Lorsque le service WSFC revient en ligne sur chaque nœud, il négocie avec les autres nœuds sains pour synchroniser le nouvel état de configuration du cluster.  N'oubliez pas d'effectuer ces opérations sur un nœud à la fois, afin d'éviter des conditions potentielles de concurrence lors de la résolution du dernier état connu du cluster.  
  
    > [!WARNING]  
    >  Vérifiez que chaque nœud que vous démarrez peut communiquer avec les autres nœuds récemment mis en ligne.  Éventuellement, désactivez le service WSFC sur les autres nœuds.  Sinon, vous courez le risque de créer plusieurs jeux de nœuds de quorum et de créer un scénario de fractionnement des partitions. Si vos résultats à l'étape 1 étaient précis, cela ne devrait pas se produire.  
  
4.  **Appliquez le nouveau mode de quorum et la nouvelle configuration de vote des nœuds.** Si l'application forcée d'un quorum a redémarré tous les nœuds du cluster et la cause première de l'échec de quorum a été corrigée, le mode de quorum d'origine et la configuration de vote des nœuds n'ont pas besoin d'être modifiés.  
  
     Sinon, vous devez évaluer la topologie de nœud de cluster et de réplica de disponibilité nouvellement créée et modifier le mode de quorum et les affectations de vote pour chaque nœud, comme il convient. Les nœuds non récupérés doivent être mis hors connexion, ou bien leurs votes de nœud doivent être définis sur zéro.  
  
    > [!TIP]  
    >  À ce stade, les nœuds et les instances de SQL Server dans le cluster peuvent apparaître comme restaurés et sembler fonctionner normalement.  Toutefois, il est possible qu'il n'y ait pas encore de quorum sain.  À l’aide du Gestionnaire du cluster de basculement, du Tableau de bord Always On dans SQL Server Management Studio ou des vues DMV appropriées, vérifiez qu’un quorum a été restauré.  
  
5.  **Récupérez les réplicas de base de données du groupe de disponibilité si nécessaire.** Les bases de données qui n'appartiennent pas au groupe de disponibilité doivent être restaurées et remises en ligne au cours du processus de démarrage normal de SQL Server, sans autre intervention.  
  
     Vous pouvez minimiser la perte potentielle de données et le temps de récupération pour les réplicas de groupe de disponibilité en les remettant en ligne dans cette séquence : réplica principal, réplicas secondaires synchrones, réplicas secondaires asynchrones.  
  
6.  **Réparez ou remplacez les composants en échec et re-validez le cluster.** Maintenant que vous avez remédié au sinistre et à l’échec de quorum, vous devez réparer ou remplacer les nœuds en échec et modifier les configurations WSFC et Always On associées en conséquence.  Cela peut inclure la suppression des réplicas de groupe de disponibilité, l'éviction des nœuds du cluster ou la mise à plat et la réinstallation des logiciels sur un nœud.  
  
     Vous devez réparer ou supprimer tous les réplicas de disponibilité.  [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ne tronquera pas le journal des transactions après le dernier point connu du réplica de disponibilité le plus lointain.   Si un réplica n'est pas réparé ou n'est pas supprimé du groupe de disponibilité, les journaux des transactions vont prendre du volume et vous allez courir le risque de manquer d'espace pour les autres réplicas.  
  
    > [!NOTE]  
    >  Si vous exécutez l'Assistant WSFC Valider une configuration alors qu'un écouteur du groupe de disponibilité existe sur le cluster WSFC, l'Assistant génère le message d'avertissement incorrect suivant :  
    >   
    >  « La propriété RegisterAllProviderIP du nom réseau 'Name:<nom_réseau>' est définie sur 1. Pour la configuration de cluster actuelle, cette valeur doit être définie sur 0. »  
    >   
    >  Veuillez ignorer ce message.  
  
7.  **Répétez l'étape 4 si nécessaire.** L'objectif est de rétablir le niveau de la tolérance de panne approprié et une haute disponibilité pour des opérations saines.  
  
8.  **Effectuez une analyse RPO/RTO.** Vous devez analyser les journaux système SQL Server, les horodateurs de base de données et les journaux des événements Windows pour déterminer la cause première de l'échec et pour documenter les expériences de point et de temps de récupération actuelles.  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Tâches associées  
  
-   [Forcer un cluster WSFC à démarrer sans quorum](../../../sql-server/failover-clusters/windows/force-a-wsfc-cluster-to-start-without-a-quorum.md)  
  
-   [Effectuer un basculement manuel forcé d’un groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/perform-a-forced-manual-failover-of-an-availability-group-sql-server.md)  
  
-   [Afficher les paramètres NodeWeight pour le quorum de cluster](../../../sql-server/failover-clusters/windows/view-cluster-quorum-nodeweight-settings.md)  
  
-   [Configurer les paramètres NodeWeight pour un quorum de cluster](../../../sql-server/failover-clusters/windows/configure-cluster-quorum-nodeweight-settings.md)  
  
-   [Utiliser le tableau de bord Always On &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-always-on-dashboard-sql-server-management-studio.md)
  
##  <a name="related-content"></a><a name="RelatedContent"></a> Contenu associé  
  
-   [Afficher les événements et journaux pour un cluster de basculement](https://technet.microsoft.com/library/cc772342\(WS.10\).aspx)  
  
-   [Applets de commande de cluster de basculement Get-ClusterLog](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee461045(v=technet.10))  
  
## <a name="see-also"></a>Voir aussi  
 [Clustering de basculement Windows Server &#40;WSFC&#41; avec SQL Server](../../../sql-server/failover-clusters/windows/windows-server-failover-clustering-wsfc-with-sql-server.md)  
  
