---
title: Instances de cluster de basculement Always On
description: Découvrez comment les instances de cluster de basculement AlwaysOn fournissent une haute disponibilité locale grâce à la redondance au niveau de l’instance de serveur dans SQL Server.
ms.custom: seo-lt-2019
ms.date: 01/18/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: failover-cluster-instance
ms.topic: conceptual
helpviewer_keywords:
- clustering [SQL Server]
- high availability [SQL Server], failover clustering
- virtual servers [SQL Server], about virtual servers
- clusters [SQL Server]
- servers [SQL Server], failover clustering
- resource groups [SQL Server]
- availability [SQL Server]
- failover clustering [SQL Server]
- AlwaysOn [SQL Server], see failover clustering [SQL Server]
ms.assetid: 86a15b33-4d03-4549-8ea2-b45e4f1baad7
author: cawrites
ms.author: chadam
ms.openlocfilehash: 722371558fe5d6f1a9a8a363322a28e7f89243f2
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250382"
---
# <a name="always-on-failover-cluster-instances-sql-server"></a>Instances de cluster de basculement Always On (SQL Server)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  Dans le cadre de l’offre [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] AlwaysOn, les instances de cluster de basculement AlwaysOn exploitent la fonctionnalité de clustering de basculement Windows Server (WSFC) pour fournir une haute disponibilité locale grâce à la redondance au niveau de l’instance de serveur, une *instance de cluster de basculement* (FCI). Une instance FCI est une instance unique de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] installée sur plusieurs nœuds WSFC (clustering de basculement Windows Server) et, éventuellement, sur plusieurs sous-réseaux. Sur le réseau, une instance de cluster de basculement FCI apparaît en tant qu'instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] s'exécutant sur un ordinateur unique, mais elle permet le basculement d'un nœud WSFC vers un autre en cas d'indisponibilité du nœud actuel.  
  
 Une instance FCI peut tirer parti des [groupes de disponibilité](../../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md) pour permettre la récupération d’urgence à distance au niveau de la base de données. Pour plus d’informations, consultez [Clustering de basculement et groupes de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/failover-clustering-and-always-on-availability-groups-sql-server.md).  
 
 > [!NOTE]  
 > Windows Server 2016 Datacenter introduit la prise en charge des espaces de stockage direct (S2D). Les instances de cluster de basculement SQL Server prennent en charge S2D pour les ressources de stockage en cluster. Pour plus d’informations, consultez [Espaces de stockage direct dans Windows Server 2016](/windows-server/storage/storage-spaces/storage-spaces-direct-overview).
 > 
 >Les instances de cluster de basculement prennent également en charge les volumes partagés de cluster. Pour plus d’informations, consultez [Présentation des volumes partagés de cluster dans un cluster de basculement](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd759255(v=ws.11)). 
   
 **Dans cette rubrique :**  
  
-   [Avantages](#Benefits)  
  
-   [Recommandations](#Recommendations)  
  
-   [Vue d'ensemble d'une instance de cluster de basculement](#Overview)  
  
-   [Éléments d'une instance de cluster de basculement](#FCIelements)  
  
-   [Concepts et tâches de basculement SQL Server](#ConceptsAndTasks)  
  
-   [Rubriques connexes](#RelatedTopics)  
  
##  <a name="benefits-of-a-failover-cluster-instance"></a><a name="Benefits"></a> Avantages d'une instance de cluster de basculement  
 En cas de défaillance matérielle ou logicielle d'un serveur, les applications ou les clients qui se connectent au serveur font face à un temps mort. Lorsqu'une instance [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] est configurée pour être une instance FCI (au lieu d'une instance autonome), la haute disponibilité de cette instance [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] est protégée par la présence de nœuds redondants dans l'instance FCI. Un seul des nœuds de l'instance FCI possède le groupe de ressources WSFC à la fois. En cas de défaillances (défaillances matérielles, défaillances du système d'exploitation, d'une application ou d'un service) ou lors d'une mise à niveau planifiée, la propriété du groupe de ressources est transférée vers un autre nœud WSFC. Ce processus est transparent pour un client ou une application se connectant à [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , ce qui permet de réduire les temps morts auxquels font face l'application ou les clients lors d'une défaillance. Les listes suivantes répertorient certains des avantages clés des instances de cluster de basculement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] :  
  
-   Protection au niveau de l'instance par redondance  
  
-   Basculement automatique en cas de défaillances (défaillances matérielles, défaillances du système d'exploitation, d'une application ou d'un service)  
  
    > [!IMPORTANT]  
    >  Dans un groupe de disponibilité, le basculement automatique depuis une instance FCI vers d’autres nœuds au sein du groupe de disponibilité n’est pas pris en charge. Cela signifie que les instances FCI et les nœuds autonomes ne doivent pas être associés dans un groupe de disponibilité si le basculement automatique est un composant important de votre solution haute disponibilité. Toutefois, cette association peut être effectuée pour votre solution de *récupération d'urgence* .  
  
-   Prise en charge d'une vaste gamme de solutions de stockage, y compris les disques de cluster WSFC (iSCSI, Fiber Channel, etc.) et les partages de fichiers de protocole SMB.  
  
-   Solution de récupération d’urgence à l’aide d’une instance FCI à plusieurs sous-réseaux ou exécutant une base de données hébergée par l’instance FCI au sein d’un groupe de disponibilité. Avec la nouvelle prise en charge de plusieurs sous-réseaux dans [!INCLUDE[msCoName](../../../includes/msconame-md.md)][!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)], une instance FCI à plusieurs sous-réseaux ne requiert plus un réseau local virtuel, ce qui augmente la facilité de gestion et la sécurité d’une telle instance.  
  
-   Absence de reconfiguration des applications et des clients pendant les basculements  
  
-   Stratégie flexible de basculement pour les événements déclencheurs granulaires dans le cas de basculements automatiques  
  
-   Basculements fiables par la détection périodique et détaillée de l'intégrité à l'aide de connexions dédiées et persistantes  
  
-   Possibilité de configuration et de prévision de la durée de basculement via des points de contrôle d'arrière-plan indirects  
  
-   Utilisation des ressources limitée au cours des basculements  
  
##  <a name="recommendations"></a><a name="Recommendations"></a> Recommandations  
 Dans un environnement de production, nous recommandons d'utiliser des adresses IP statiques en association avec l'adresse IP virtuelle d'une instance de cluster de basculement.  Nous déconseillons d'utiliser DHCP dans un environnement de production. En cas d'arrêt du système, si le bail IP DHCP expire, il faudra consacrer du temps supplémentaire pour réinscrire la nouvelle adresse IP DHCP associée au nom DNS.  
  
##  <a name="failover-cluster-instance-overview"></a><a name="Overview"></a> Vue d'ensemble d'une instance de cluster de basculement  
 Une instance FCI s'exécute dans un groupe de ressources WSFC avec un ou plusieurs nœuds WSFC. Au démarrage de l'instance FCI, l'un des nœuds suppose la propriété du groupe de ressources et met son instance [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] en ligne. Les ressources détenues par ce nœud sont les suivantes :  
  
-   Nom du réseau  
  
-   Adresse IP  
  
-   Disques partagés  
  
-   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Service Moteur de base de données  
  
-   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Service Agent  
  
-   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Analysis Services, s'il est installé  
  
-   Une ressource de partage de fichiers, si la fonctionnalité FILESTREAM est installée  
  
 À un moment donné, seul le propriétaire du groupe de ressources (et aucun autre nœud de l'instance FCI) exécute ses services [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] respectifs dans le groupe de ressources. En cas de basculement, qu'il s'agisse d'un basculement automatique ou d'un basculement planifié, la séquence d'événements suivante se produit :  
  
1.  Sauf en cas de défaillance du système ou du matériel, toutes les pages de modifications dans le cache de tampons sont écrites sur le disque.  
  
2.  Tous les services [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] respectifs du groupe de ressources sont arrêtés sur le nœud actif.  
  
3.  La propriété du groupe de ressources est transférée vers un autre nœud de l'instance FCI.  
  
4.  Le nouveau propriétaire du groupe de ressources démarre ses services [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
5.  Les demandes de connexion de l'application cliente sont automatiquement dirigées vers le nouveau nœud actif à l'aide du même nom de réseau virtuel (VNN).  
  
 L'instance FCI reste en ligne tant que son cluster WSFC sous-jacent présente une intégrité de quorum satisfaisante (la majorité des nœuds de quorum WSFC est disponible en tant que cibles de basculement automatique). Lorsque le cluster WSFC perd son quorum, soit en raison d'une défaillance du matériel, du logiciel ou du réseau, soit à cause d'une configuration de quorum inappropriée, l'intégralité du cluster WSFC, ainsi que l'instance FCI, sont mis hors connexion. Une intervention manuelle est alors requise dans ce scénario de basculement non planifié afin de rétablir le quorum dans les nœuds disponibles restants et de remettre le cluster WSFC et l'instance FCI en ligne. Pour plus d’informations, consultez [Modes de quorum WSFC et configuration de vote &#40;SQL Server&#41;](../../../sql-server/failover-clusters/windows/wsfc-quorum-modes-and-voting-configuration-sql-server.md).  
  
### <a name="predictable-failover-time"></a>Durée de basculement prévisible  
 Selon le moment auquel votre instance [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] a exécuté pour la dernière fois une opération de point de contrôle, le cache de tampons peut renfermer une quantité substantielle de pages de modifications. Par conséquent, les basculements durent aussi longtemps que nécessaire pour écrire les pages de modifications restantes sur le disque, opération qui peut provoquer une durée de basculement longue et imprévisible. À compter de [!INCLUDE[msCoName](../../../includes/msconame-md.md)][!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)], l’instance FCI peut utiliser des points de contrôle indirects pour limiter la quantité de pages de modifications conservées dans le cache des tampons. Même si cette opération consomme des ressources supplémentaires avec une charge de travail normale, elle rend la durée de basculement plus prévisible et plus facile à configurer. Cela s'avère très utile lorsque le contrat de niveau de service de votre organisation spécifie un objectif de durée maximale d'interruption admissible (RTO, Recovery Time Objective) pour votre solution haute disponibilité. Pour plus d'informations sur les points de contrôle indirects, consultez [Indirect Checkpoints](../../../relational-databases/logs/database-checkpoints-sql-server.md#IndirectChkpt).  
  
### <a name="reliable-health-monitoring-and-flexible-failover-policy"></a>Contrôle d'intégrité fiable et stratégie flexible de basculement  
 Après le démarrage réussi de l'instance FCI, le service WSFC surveille à la fois l'intégrité du cluster WSFC sous-jacent, ainsi que l'intégrité de l'instance [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . À compter de [!INCLUDE[msCoName](../../../includes/msconame-md.md)][!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)], le service WSFC utilise une connexion dédiée pour interroger l’instance [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] active en vue d’un diagnostic de composant détaillé via une procédure stockée système. Son implication est triple :  
  
-   La connexion dédiée à l'instance [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] permet d'interroger de manière fiable le diagnostic de composant à tout moment, même lorsque l'instance FCI est soumise à une charge importante. Cela permet de distinguer un système dont la charge est importante d'un système qui présente réellement des conditions d'échec, ce qui empêche la survenue de problèmes tels que les basculements inappropriés.  
  
-   Le diagnostic de composant détaillé permet de configurer une stratégie de basculement plus souple, dans laquelle vous pouvez choisir les conditions d'échec qui déclenchent des basculements.  
  
-   Le diagnostic de composant détaillé permet également rétroactivement un meilleur dépannage des basculements automatiques. Les informations de diagnostic sont stockées dans les fichiers journaux, qui sont colocalisés avec les journaux d'erreurs de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Vous pouvez les charger dans la Visionneuse du fichier journal afin d'examiner les états de composant qui mènent à la survenue d'un basculement et d'en déterminer la cause.  
  
 Pour plus d'informations, consultez [Failover Policy for Failover Cluster Instances](../../../sql-server/failover-clusters/windows/failover-policy-for-failover-cluster-instances.md)  
  
##  <a name="elements-of-a-failover-cluster-instance"></a><a name="FCIelements"></a> Éléments d'une instance de cluster de basculement  
 Une instance FCI se compose d'un ensemble de serveurs physiques (nœuds) qui présentent une configuration matérielle similaire, ainsi qu'une configuration logicielle identique qui inclut la version du système d'exploitation et le niveau de correctif, la version de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , le niveau de correctif, les composants et le nom de l'instance. Une configuration logicielle identique est nécessaire pour garantir le fonctionnement intégral de l'instance FCI au moment du basculement entre les nœuds.  
  
 Groupe de ressources WSFC  
 Une instance [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] FCI s'exécute dans un groupe de ressources WSFC. Chaque nœud du groupe de ressources conserve une copie synchronisée des paramètres de configuration, ainsi que les clés de Registre ayant fait l'objet d'un point de contrôle, pour garantir la fonctionnalité complète de l'instance FCI suite à un basculement. Seul un des nœuds du cluster possède le groupe de ressources à la fois (nœud actif). Le service WSFC gère le cluster de serveurs, la configuration de quorum, la stratégie de basculement et les opérations de basculement, ainsi que les adresses IP virtuelles et le nom VNN de l'instance FCI. En cas de défaillances (défaillances matérielles, défaillances du système d'exploitation, d'une application ou d'un service) ou lors d'une mise à niveau planifiée, la propriété du groupe de ressources est transférée vers un autre nœud de l'instance FCI. Le nombre de nœuds pris en charge dans un groupe de ressources WSFC dépend de l'édition de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] que vous utilisez. De plus, le même cluster WSFC peut exécuter plusieurs instances FCI (plusieurs groupes de ressources), selon la capacité du matériel, notamment le processeur, la mémoire et le nombre de disques.  
  
 Binaires de SQL Server  
 Les binaires de produit sont installés localement sur chaque nœud de l'instance FCI, un processus similaire aux installations autonomes de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Toutefois, lors du démarrage, les services ne sont pas démarrés automatiquement ; ils sont gérés par WSFC.  
  
 Stockage  
 Contrairement au groupe de disponibilité, une instance FCI doit utiliser le stockage partagé entre tous les nœuds de l’instance FCI pour le stockage des journaux et des bases de données. Le stockage partagé peut se présenter sous la forme de disques de cluster WSFC, de disques sur un réseau SAN, d’espaces de stockage direct (S2D) ou de partages de fichiers sur un serveur SMB. De cette façon, tous les nœuds de l'instance FCI ont la même vue des données d'instance lors d'un basculement. Cela signifie, toutefois, que le stockage partagé présente le risque d'être l'unique point de défaillance et que l'instance FCI dépend de la solution de stockage sous-jacente pour assurer la protection des données.  
  
 Nom de réseau  
 Le nom de réseau virtuel (VNN) de l'instance FCI fournit un point de connexion unifié pour l'instance FCI. Cela permet aux applications de se connecter au VNN sans avoir besoin de connaître le nœud actif. Lorsqu'un basculement se produit, le VNN est inscrit sur le nouveau nœud actif après son démarrage. Ce processus est transparent pour un client ou une application se connectant à [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , ce qui permet de réduire les temps morts auxquels font face l'application ou les clients lors d'une défaillance.  
  
 Adresses IP virtuelles  
 Dans le cas d'une instance FCI à plusieurs sous-réseaux, une adresse IP virtuelle est affectée à chaque sous-réseau au sein de l'instance FCI. Durant un basculement, le VNN sur le serveur DNS est mis à jour pour indiquer l'adresse IP virtuelle du sous-réseau respectif. Les applications et les clients peuvent ensuite se connecter à l'instance FCI à l'aide du même nom VNN après un basculement de plusieurs sous-réseaux.  
  
##  <a name="sql-server-failover-concepts-and-tasks"></a><a name="ConceptsAndTasks"></a> Concepts et tâches de basculement SQL Server  
  
|Concepts et tâches|Rubrique|  
|------------------------|-----------|  
|Décrit le mécanisme de détection de pannes et la stratégie flexible de basculement.|[Failover Policy for Failover Cluster Instances](../../../sql-server/failover-clusters/windows/failover-policy-for-failover-cluster-instances.md)|  
|Décrit les concepts dans l'administration et la maintenance de l'instance FCI.|[Administration et maintenance de l'instance de cluster de basculement](../../../sql-server/failover-clusters/windows/failover-cluster-instance-administration-and-maintenance.md)|  
|Décrit la configuration de sous-réseaux multiples et les concepts associés|[Clustering de sous-réseaux multiples SQL Server &#40;SQL Server&#41;](../../../sql-server/failover-clusters/windows/sql-server-multi-subnet-clustering-sql-server.md)|  
  
##  <a name="related-topics"></a><a name="RelatedTopics"></a> Rubriques connexes  
  
|**Descriptions des rubriques**|**Rubrique**|  
|----------------------------|---------------|  
|Décrit comment installer une nouvelle instance FCI [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .|[Créer un cluster de basculement SQL Server &#40;programme d’installation&#41;](../../../sql-server/failover-clusters/install/create-a-new-sql-server-failover-cluster-setup.md)|  
|Explique comment effectuer une mise à niveau vers un cluster de basculement [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] .|[Mettre à niveau une instance de cluster de basculement SQL Server](../../../sql-server/failover-clusters/windows/upgrade-a-sql-server-failover-cluster-instance.md)|  
|Décrit les concepts de clustering de basculement Windows et fournit des liens vers les tâches liées au clustering de basculement Windows.|[!INCLUDE[nextref_longhorn](../../../includes/nextref-longhorn-md.md)]: [Présentation des clusters de basculement](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc730692(v=ws.10))<br /><br /> [!INCLUDE[nextref_longhorn](../../../includes/nextref-longhorn-md.md)] R2 : [Présentation des clusters de basculement](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754482(v=ws.11))|  
|Décrit les différences de concepts entre les nœuds dans une instance FCI et les réplicas au sein d'un groupe de disponibilité, ainsi que les éléments à prendre en compte pour utiliser une instance FCI pour héberger un réplica pour un groupe de disponibilité.|[Clustering de basculement et groupes de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/failover-clustering-and-always-on-availability-groups-sql-server.md)|  
  
