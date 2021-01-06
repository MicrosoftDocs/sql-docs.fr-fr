---
title: Modes de quorum WSFC et configuration de vote
description: Découvrez les modes de quorum et les votes de quorum qui sont utilisés par le cluster de basculement Windows Server avec une instance de cluster de basculement SQL Server ou un groupe de disponibilité AlwaysOn.
ms.custom: seo-lt-2019
ms.date: 10/03/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: failover-cluster-instance
ms.topic: conceptual
helpviewer_keywords:
- Availability Groups [SQL Server], WSFC clusters
- quorum [SQL Server], AlwaysOn and WSFC quorum
- failover clustering [SQL Server], AlwaysOn Availability Groups
ms.assetid: ca0d59ef-25f0-4047-9130-e2282d058283
author: cawrites
ms.author: chadam
ms.openlocfilehash: 51e48eeead20cce35d4e4c58d767384bab1a8a65
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97642622"
---
# <a name="wsfc-quorum-modes-and-voting-configuration-sql-server"></a>Modes de quorum WSFC et configuration de vote (SQL Server)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Tant les [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)][!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] que les instances de cluster de basculement Always On tirent parti du clustering de basculement Windows Server (WSFC) en tant que technologie de plateforme.  WSFC utilise une approche basée sur le quorum pour la surveillance de l'intégrité globale du cluster et l'optimisation de la tolérance aux pannes au niveau du nœud. Il est très important de disposer d’une connaissance de base de la configuration du vote des nœuds et des modes de quorum WSFC pour concevoir, administrer et dépanner votre solution de récupération d’urgence haute disponibilité Always On.  
  
  
##  <a name="cluster-health-detection-by-quorum"></a><a name="ClusterHealthDetectionbyQuorum"></a> Détection de l'intégrité du cluster par quorum  
 Chaque nœud d'un cluster WSFC participe à la communication périodique de pulsation pour partager l'état d'intégrité du nœud avec les autres nœuds. Les nœuds qui ne répondent pas sont considérés comme étant en état d'échec.  
  
 Un ensemble de nœuds de *quorum* est une majorité des nœuds votants et des témoins dans le cluster WSFC. L'intégrité globale et le statut d'un cluster WSFC sont déterminés par un *vote de quorum* périodique.  La présence d'un quorum signifie que le cluster est intègre et en mesure d'assurer la tolérance aux pannes au niveau du nœud.  
  
 L'absence de quorum indique que le cluster n'est pas intègre.  L'intégrité globale du cluster WSFC doit être assurée pour garantir la disponibilité de nœuds secondaires intègres vers lesquels les nœuds principaux peuvent basculer.  Si le vote du quorum échoue, le cluster WSFC passe hors connexion par mesure de sécurité.  Cela entraîne également l'arrêt de toutes les instances de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] inscrites avec le cluster.  
  
> [!IMPORTANT]  
>  Si un cluster WSFC passe hors connexion suite à l'échec du quorum, une intervention manuelle est nécessaire pour le remettre en ligne.  
>   
>  Pour plus d'informations, consultez les pages suivantes : [Reprise d’activité après sinistre WSFC par le quorum forcé &#40;SQL Server&#41;](../../../sql-server/failover-clusters/windows/wsfc-disaster-recovery-through-forced-quorum-sql-server.md).  
  
##  <a name="quorum-modes"></a><a name="QuorumModes"></a> Modes de quorum  
 Un *mode de quorum* est configuré au niveau du cluster WSFC afin de dicter la méthodologie utilisée pour le vote du quorum.  L'utilitaire Gestionnaire du cluster de basculement recommande un mode de quorum en fonction du nombre de nœuds du cluster.  
  
 Les modes de quorum suivants peuvent être utilisés pour déterminer la constitution d'un quorum de votes :  
  
-   **Nœud majoritaire** Plus de la moitié des nœuds votants du cluster doit voter de manière affirmative pour que le cluster soit considéré comme intègre.  
  
-   **Nœud et partage de fichiers majoritaires.** Semblable au mode de quorum Nœud majoritaire, à ceci près qu'un partage de fichiers distant est également configuré en tant que témoin du vote, et que la connectivité de n'importe quel nœud à ce partage est également comptabilisée comme vote affirmatif.  Plus de la moitié des votes possibles doivent être affirmatifs pour que cluster soit considéré comme intègre.  
  
     À titre de recommandation, le partage de fichiers témoin ne doit pas résider sur un nœud du cluster, et il doit être visible pour tous les nœuds du cluster.  
  
-   **Nœud et disque majoritaires.** Semblable au mode de quorum Nœud majoritaire, à ceci près qu'une ressource de cluster de disque partagé est également désignée comme témoin du vote, et que la connectivité de n'importe quel nœud à ce disque partagé est également comptabilisée comme un vote affirmatif.  Plus de la moitié des votes possibles doivent être affirmatifs pour que cluster soit considéré comme intègre.  
  
-   **Disque uniquement.** Une ressource de cluster de disque partagé est désignée comme témoin, et la connectivité de n'importe quel nœud à ce disque partagé est comptabilisée comme un vote affirmatif.  
  
> [!TIP]  
>  Lorsque vous utilisez une configuration de stockage asymétrique pour [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)], il est généralement conseillé d'utiliser le mode de quorum Nœud majoritaire lorsque vous avez un nombre impair de nœuds votants, ou le mode de quorum Nœud et partage de fichiers majoritaires lorsque vous avez un nombre pair de nœuds votants.  
  
##  <a name="voting-and-non-voting-nodes"></a><a name="VotingandNonVotingNodes"></a> Nœuds votants et non-votants  
 Par défaut, chaque nœud du cluster WSFC est inclus en tant que membre du quorum du cluster ; chaque nœud dispose d'un seul vote pour déterminer l'intégrité globale de cluster, et chaque nœud tente en permanence d'établir un quorum.  À ce point précis, la discussion du quorum a soigneusement qualifié l'ensemble des nœuds de cluster WSFC qui votent pour déterminer l'intégrité du cluster en tant que *nœuds votants*.  
  
 Aucun nœud d'un cluster WSFC ne peut définitivement déterminer seul que le cluster est globalement intègre ou pas.  À tout moment, du point de vue de chaque nœud, certains autres nœuds peuvent sembler être hors connexion, ou en cours de basculement, ou ne pas répondre en raison d'une défaillance de communication réseau.  Une fonction principale du vote de quorum consiste à déterminer si l'état apparent de chaque nœud du cluster WSFC correspond à l'état réel du nœud.  
  
 Pour tous les modes de quorum à l’exception de « Disque uniquement », l’efficacité d’un vote de quorum dépend de la fiabilité des communications entre tous les nœuds votants du cluster. Les communications réseau entre les nœuds sur le même sous-réseau physique doivent être considérées comme fiables ; le vote du quorum doit être approuvé.  
  
 Toutefois, si un nœud d'un autre sous-réseau est considéré comme non-réactif dans un vote de quorum alors qu'il en ligne et intègre, ceci est probablement dû à une défaillance de communication réseau entre les sous-réseaux.  En fonction de la topologie de cluster, du mode de quorum et de la configuration de la stratégie de basculement, l'échec de communication réseau peut effectivement créer plusieurs ensembles (ou sous-ensembles) de nœuds votants.  
  
 Quand plusieurs sous-ensembles de nœuds votants sont en mesure d’établir un quorum par eux-mêmes, on parle de *scénario de fractionnement*.  Dans ce scénario, les nœuds appartenant à des quorums distincts peuvent se comporter différemment, et être en conflit les uns avec les autres.  
  
> [!NOTE]  
>  Le scénario de fractionnement est uniquement possible lorsqu'un administrateur système procède manuellement à une opération de quorum forcée, ou dans quelques cas très rares, à un basculement forcé, ce qui a pour effet de subdiviser l'ensemble de nœuds du quorum.  
  
 Pour simplifier la configuration de votre quorum et accroître la disponibilité, vous pouvez configurer le paramètre *NodeWeight* de chaque nœud de sorte que le vote du nœud ne soit pas comptabilisé dans le quorum.  
  
> [!IMPORTANT]  
>  Pour utiliser les paramètres NodeWeight, le correctif logiciel suivant doit être appliqué à tous les serveurs dans le cluster WSFC :  
>   
>  [KB2494036](https://support.microsoft.com/kb/2494036) : un correctif est disponible pour vous permettre de configurer un nœud de cluster qui n'a pas de votes de quorum dans [!INCLUDE[firstref_longhorn](../../../includes/firstref-longhorn-md.md)] et dans [!INCLUDE[winserver2008r2](../../../includes/winserver2008r2-md.md)]  
  
##  <a name="recommended-adjustments-to-quorum-voting"></a><a name="RecommendedAdjustmentstoQuorumVoting"></a> Réglages recommandés pour le vote du quorum  
 Lorsque vous activez ou désactivez le vote d’un nœud WSFC donné, respectez les instructions suivantes :  
  
-   **Aucun vote par défaut.** Imaginons que chaque nœud ne doit pas voter sans justification explicite.  
  
-   **Inclure tous les réplicas principaux.** Chaque nœud WSFC hébergeant un réplica principal de groupe de disponibilité ou qui est le propriétaire par défaut d'une instance de cluster de basculement doit disposer d'un vote.  
  
-   **Inclure les propriétaires possibles de basculement automatique.** Chaque nœud susceptible d'héberger un réplica de disponibilité suite à un basculement automatique du groupe de disponibilité ou d'une instance de cluster de basculement doit disposer d'un vote. S'il n'existe qu'un seul groupe de disponibilité dans le cluster WSFC et des réplicas de disponibilité sont hébergés uniquement par des instances autonomes, cette règle inclut uniquement le réplica secondaire qui est la cible du basculement automatique.  
  
-   **Exclure les nœuds de site secondaire.** En général, n'attribuez pas de vote aux nœuds WSFC résidant sur un site de récupération d'urgence secondaire.  Il n'est pas souhaitable que les nœuds du site secondaire puissent contribuer à une décision visant à mettre le cluster hors connexion lorsque le site principal fonctionne correctement.  
  
-   **Nombre impair de voix.** Si nécessaire, ajoutez un partage de fichiers témoin, un nœud témoin ou un disque témoin au cluster et modifiez le mode de quorum de manière à empêcher d'éventuels liens dans le vote du quorum.  
  
-   **Réévaluez les assignations de vote après le basculement** Il n'est pas souhaitable de basculer dans une configuration de cluster qui ne prend pas en charge un quorum intègre.  
  
> [!IMPORTANT]
>  Pendant la validation de la configuration de vote du quorum WSFC, l’Assistant Groupe de disponibilité Always On affiche un avertissement si l’une des conditions suivantes est satisfaite :  
> 
>  -   Le nœud de cluster qui héberge le réplica principal n'a pas de vote.  
> -   Un réplica secondaire est configuré pour le basculement automatique et le nœud de cluster n'a pas de vote.  
> -   [KB2494036](https://support.microsoft.com/kb/2494036) n'est pas installé sur tous les nœuds de cluster qui hébergent des réplicas de disponibilité. Ce correctif logiciel est requis pour ajouter ou supprimer des votes pour les nœuds de cluster dans les déploiements multisites. Toutefois, dans les déploiements sur un seul site, il n'est généralement pas nécessaire et vous pouvez ignorer l'avertissement en toute sécurité.  
> 
> [!TIP]
>  [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] expose plusieurs vues de gestion dynamique (DMV) du système qui peuvent vous aider à gérer la configuration des paramètres du cluster WSFC et le vote du quorum de nœud.  
> 
>  Pour plus d’informations, consultez :  [sys.dm_hadr_cluster](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-cluster-transact-sql.md), [sys.dm_hadr_cluster_members](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-cluster-members-transact-sql.md), [sys.dm_os_cluster_nodes](../../../relational-databases/system-dynamic-management-views/sys-dm-os-cluster-nodes-transact-sql.md), [sys.dm_hadr_cluster_networks](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-cluster-networks-transact-sql.md)  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Tâches associées  
  
-   [Afficher les paramètres NodeWeight pour le quorum de cluster](../../../sql-server/failover-clusters/windows/view-cluster-quorum-nodeweight-settings.md)  
  
-   [Configurer les paramètres NodeWeight pour un quorum de cluster](../../../sql-server/failover-clusters/windows/configure-cluster-quorum-nodeweight-settings.md)  
  
##  <a name="related-content"></a><a name="RelatedContent"></a> Contenu associé  
  
-   [Guide de solutions Microsoft SQL Server Always On pour la haute disponibilité et la récupération d’urgence](https://go.microsoft.com/fwlink/?LinkId=227600)  
  
-   [Quorum vote configuration check in Always On Availability Group Wizards (Vérification de la configuration du vote du quorum dans l’Assistant Groupe de disponibilité Always On)](https://blogs.msdn.microsoft.com/sqlalwayson/2012/03/13/quorum-vote-configuration-check-in-alwayson-availability-group-wizards-andy-jing/)  
  
-   [Technologies Windows Server :  Clusters de basculement](https://technet.microsoft.com/library/cc732488\(v=WS.10\).aspx)  
  
-   [Guide pas à pas des clusters de basculement : Configuration du quorum dans un cluster de basculement](https://technet.microsoft.com/library/cc770620\(WS.10\).aspx)  
  
## <a name="see-also"></a>Voir aussi  
 [Récupération d’urgence WSFC par le quorum forcé &#40;SQL Server&#41;](../../../sql-server/failover-clusters/windows/wsfc-disaster-recovery-through-forced-quorum-sql-server.md)   
 [Clustering de basculement Windows Server &#40;WSFC&#41; avec SQL Server](../../../sql-server/failover-clusters/windows/windows-server-failover-clustering-wsfc-with-sql-server.md)  
  
  
