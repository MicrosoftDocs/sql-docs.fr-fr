---
title: Instance de cluster de basculement avec des groupes de disponibilité
description: Améliorez la haute disponibilité et la récupération d’urgence en combinant les fonctionnalités d’une instance de cluster de basculement SQL Server et un groupe de disponibilité Always On.
ms.custom: seo-lt-2019
ms.date: 07/02/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
helpviewer_keywords:
- clustering [SQL Server]
- Availability Groups [SQL Server], WSFC clusters
- Failover Cluster Instances [SQL Server], see failover clustering [SQL Server]
- quorum [SQL Server]
- failover clustering [SQL Server], AlwaysOn Availability Groups
- Availability Groups [SQL Server], Failover Cluster Instances
ms.assetid: 613bfbf1-9958-477b-a6be-c6d4f18785c3
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 1cb8d5776ffa69832b001b7f3594f743cf4c3d39
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97644130"
---
# <a name="failover-clustering-and-always-on-availability-groups-sql-server"></a>Clustering de basculement et groupes de disponibilité Always On (SQL Server)

[!INCLUDE[sql windows only](../../../includes/applies-to-version/sql-windows-only.md)]

   [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)], la solution haute disponibilité et de récupération d'urgence introduite dans [!INCLUDE[sssql11](../../../includes/sssql11-md.md)], requiert le Clustering de basculement Windows Server (WSFC). En outre, bien que [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] ne dépende pas du Clustering de basculement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , vous pouvez utiliser une instance de clustering de basculement pour héberger un réplica de disponibilité pour un groupe de disponibilité. Il est important de connaître le rôle de chaque technologie de clustering, ainsi que les observations nécessaires lorsque vous concevez votre environnement [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] .  
  
> [!NOTE]  
>  Pour plus d’informations sur les concepts [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] , consultez [Vue d’ensemble des groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md).  
  
  
##  <a name="windows-server-failover-clustering-and-availability-groups"></a><a name="WSFC"></a> Clustering de basculement de serveur Windows et groupes de disponibilité  
 Le déploiement de [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] exige un cluster de basculement Windows Server (cluster WSFC). Pour que [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] soit activé, une instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] doit résider sur un nœud WSFC, et le cluster et le nœud WSFC doivent être en ligne. De plus, chaque réplica de disponibilité d’un groupe de disponibilité donné doit résider sur un nœud différent du même cluster WSFC. La seule exception survient lors de la migration vers un autre cluster WSFC : un groupe de disponibilité peut temporairement chevaucher deux clusters.  
  
 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] s’appuie sur le cluster WSFC pour superviser et gérer les rôles actuels des réplicas de disponibilité qui appartiennent à un groupe de disponibilité donné et pour déterminer de quelle manière un événement de basculement affecte les réplicas de disponibilité. Un groupe de ressources WSFC est créé pour chaque groupe de disponibilité que vous créez. Le cluster WSFC supervise ce groupe de ressources pour évaluer l’intégrité du réplica principal.  
  
 Le quorum pour [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] est basé sur tous les nœuds du cluster WSFC, indépendamment du fait qu’un nœud de cluster donné héberge des réplicas de disponibilité. Contrairement à la mise en miroir de bases de données, il n'existe aucun rôle de témoin dans [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)].  
  
 L’intégrité globale d’un cluster WSFC est déterminée par les votes d’un quorum de nœuds du cluster. Si le cluster WSFC est mis hors connexion en raison d’un problème grave non planifié, ou en raison d’un problème de matériel ou de communication persistant, une intervention administrative manuelle est nécessaire. Un administrateur Windows Server ou de cluster WSFC doit forcer un quorum et remettre les nœuds de cluster survivants en ligne dans une configuration sans tolérance de panne.  
  
> [!IMPORTANT]  
>  Les clés de Registre [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] sont des sous-clés du cluster WSFC. Si vous supprimez et recréez un cluster WSFC, vous devez désactiver puis réactiver la fonctionnalité [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] sur chaque instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] qui hébergeait un réplica de disponibilité sur le cluster WSFC d’origine.  
  
 Pour plus d’informations sur l’exécution de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] sur des nœuds WSFC et sur le quorum WSFC, consultez [Clustering de basculement Windows Server &#40;WSFC&#41; avec SQL Server](../../../sql-server/failover-clusters/windows/windows-server-failover-clustering-wsfc-with-sql-server.md).  
  
##  <a name="ssnoversion-failover-cluster-instances-fcis-and-availability-groups"></a><a name="SQLServerFC"></a> [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Instances de cluster de basculement et groupes de disponibilité  
 Vous pouvez configurer une deuxième couche de basculement au niveau de l’instance serveur en implémentant une instance de cluster de basculement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] avec le cluster WSFC. Un réplica de disponibilité peut être hébergé par une instance autonome de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ou par une instance de cluster de basculement. Un seul partenaire FCI peut héberger un réplica pour un groupe de disponibilité donné. Lorsqu'un réplica de disponibilité s'exécute sur une FCI, la liste des propriétaires possibles pour le groupe de disponibilité contiendra uniquement le nœud FCI actif.  
  
 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] ne dépend d'aucune forme de stockage partagé. Toutefois, si vous utilisez une instance de cluster de basculement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] pour héberger un ou plusieurs réplicas de disponibilité, chacune de ces instances de cluster de basculement requiert un stockage partagé conformément à l'installation standard de l'instance de cluster de basculement SQL Server.  
  
 Pour plus d’informations sur les conditions préalables requises, consultez la section « Conditions préalables requises et restrictions pour l’utilisation d’une instance de cluster de basculement SQL Server afin d’héberger un réplica de disponibilité » de [Conditions préalables requises, restrictions et recommandations pour les groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md).  
  
### <a name="comparison-of-failover-cluster-instances-and-availability-groups"></a>Comparaison des instances de cluster de basculement et des groupes de disponibilité  
 Quel que soit le nombre de nœuds de l'instance de cluster de basculement, celle-ci ne peut héberger qu'un seul réplica dans un groupe de disponibilité. Le tableau suivant décrit les distinctions de concepts entre les nœuds d'une instance de cluster de basculement et les réplicas dans un groupe de disponibilité.  
  
||Nœuds dans une instance de cluster de basculement|Réplicas dans un groupe de disponibilité|  
|-|-------------------------|-------------------------------------------|  
|**Utilise WSFC**|Oui|Oui|  
|**Niveau de protection**|Instance|Base de données|  
|**Type de stockage**|Partagé|Non partagé<br /><br /> Même si les réplicas dans un groupe de disponibilité ne partagent pas le stockage, un réplica hébergé par une instance de cluster de basculement utilise une solution de stockage partagé conforme aux exigences de cette instance de cluster de basculement. La solution de stockage est partagée uniquement par les nœuds de l'instance de cluster de basculement et pas entre les réplicas du groupe de disponibilité.|  
|**Solutions de stockage**|attachement direct, SAN, points de montage, SMB|Dépend du type de nœud|  
|**Secondaires accessibles en lecture**|Non*|Oui|  
|**Paramètres de stratégie de basculement applicables**|Quorum WSFC<br /><br /> Spécifique à l'instance de cluster de basculement<br /><br /> Paramètres du groupe de disponibilité**|Quorum WSFC<br /><br /> Paramètres de groupe de disponibilité|  
|**Ressources basculées**|Serveur, instance et base de données|Base de données uniquement|  
  
 *Alors que les réplicas secondaires synchrones dans un groupe de disponibilité s’exécutent toujours sur leurs instances [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] respectives, les nœuds secondaires dans une instance de cluster de basculement n’ont pas démarré leurs instances [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] respectives et sont donc inaccessibles en lecture. Dans une instance de cluster de basculement, un nœud secondaire démarre son instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] uniquement lorsque la propriété du groupe de ressources lui est transférée lors d'un basculement de l'instance de cluster de basculement. Toutefois, sur le nœud actif de l'instance de cluster de basculement, lorsqu'une base de données hébergée par l'instance de cluster de basculement appartient à un groupe de disponibilité, si le réplica de disponibilité local s'exécute comme réplica secondaire accessible en lecture, la base de données est accessible en lecture.  
  
 **Les paramètres de stratégie de basculement du groupe de disponibilité s’appliquent à tous les réplicas, qu’il soit hébergé dans une instance autonome ou une instance de cluster de basculement.  
  
> [!NOTE]  
>  Pour plus d’informations sur le **nombre de nœuds** dans les instances de cluster de basculement et les **groupes de disponibilité Always On** pour les différentes éditions de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], consultez [Fonctionnalités prises en charge par les éditions de SQL Server 2012](/previous-versions/sql/sql-server-2012/cc645993(v=sql.110)) (https://go.microsoft.com/fwlink/?linkid=232473).  
  
### <a name="considerations-for-hosting-an-availability-replica-on-an-fci"></a>Considérations relatives à l'hébergement d'un réplica de disponibilité sur une instance de cluster de disponibilité  
  
> [!IMPORTANT]  
>  Si vous envisagez d’héberger un réplica de disponibilité sur une instance de cluster de basculement SQL Server, assurez-vous que les nœuds hôtes Windows Server 2008 respectent les conditions préalables requises et les restrictions Always On applicables aux instances de cluster de basculement. Pour plus d’informations, consultez [Conditions préalables requises, restrictions et recommandations pour les groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md).  
  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Les instances de cluster de basculement ne prennent pas en charge le basculement automatique par les groupes de disponibilité. Par conséquent, tout réplica de disponibilité hébergé par une instance de cluster de basculement ne peut être configuré que pour un basculement manuel.  
  
 Vous devrez peut-être configurer un cluster WSFC de sorte à inclure les disques partagés qui ne sont pas disponibles sur tous les nœuds. Prenons par exemple un cluster WSFC réparti entre deux centres de données comportant trois nœuds. Deux des nœuds hébergent une instance de cluster de basculement SQL Server dans le centre de données principal et ont accès aux mêmes disques partagés. Le troisième nœud héberge une instance autonome de SQL Server dans un centre de données différent et n'a pas accès aux disques partagés du centre de données principal. Cette configuration de cluster WSFC prend en charge le déploiement d’un groupe de disponibilité si l’instance de cluster de basculement héberge le réplica principal et l’instance autonome héberge le réplica secondaire.  
  
 Lorsque vous choisissez une instance de cluster de basculement pour l'hébergement d'un réplica de disponibilité pour un groupe de disponibilité donné, vérifiez qu'un basculement de l'instance de cluster de basculement ne peut pas provoquer de tentative d'hébergement, par un seul nœud WSFC, de deux réplicas de disponibilité pour le même groupe de disponibilité.  
  
 L'exemple de scénario suivant montre en quoi cette configuration risque de provoquer des problèmes :  
  
 Marcel configure un cluster WSFC avec deux nœuds, `NODE01` et `NODE02`. Il installe une instance de cluster de basculement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , `fciInstance1`, sur `NODE01` et `NODE02` , où `NODE01` est le propriétaire actuel de `fciInstance1`.  
 Sur `NODE02`, Marcel installe une autre instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], `Instance3`, qui est une instance autonome.  
 Sur `NODE01`, Marcel active fciInstance1 pour [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]. Sur `NODE02`, il active `Instance3` pour [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]. Il configure ensuite un groupe de disponibilité pour lequel `fciInstance1` héberge le réplica principal et `Instance3` héberge le réplica secondaire.  
 À un certain stade, `fciInstance1` devient indisponible sur `NODE01` et le cluster WSFC provoque un basculement de `fciInstance1` vers `NODE02`. Après le basculement, `fciInstance1` est une instance activée pour [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]qui s'exécute sous le rôle principal sur `NODE02`. Toutefois, `Instance3` réside maintenant sur le même nœud WSFC que `fciInstance1`. Cela viole la contrainte [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] .  
 Pour résoudre le problème présenté par ce scénario, l’instance autonome, `Instance3`, doit résider sur un autre nœud dans le même cluster WSFC que `NODE01` et `NODE02`.  
  
 Pour plus d’informations sur les instances de cluster de basculement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], consultez [Instances de cluster de basculement Always On &#40;SQL Server&#41;](../../../sql-server/failover-clusters/windows/always-on-failover-cluster-instances-sql-server.md).  
  
##  <a name="restrictions-on-using-the-wsfc-failover-cluster-manager-with-availability-groups"></a><a name="FCMrestrictions"></a> Restrictions d'utilisation du Gestionnaire du cluster de basculement WSFC avec des groupes de disponibilité  
 N'utilisez pas le Gestionnaire du cluster de basculement pour manipuler des groupes de disponibilité, par exemple :  
  
-   N'ajoutez pas ou ne supprimez pas de ressources dans le service cluster (groupe de ressources) du groupe de disponibilité.  
  
-   Ne modifiez pas les propriétés du groupe de disponibilité, telles que les propriétaires possibles et par défaut. Ces propriétés sont définies automatiquement par le groupe de disponibilité.  
  
-   **N’utilisez pas le Gestionnaire du cluster de basculement pour déplacer des groupes de disponibilité vers des nœuds différents ou faire basculer des groupes de disponibilité**. Le Gestionnaire du cluster de basculement n'a pas connaissance de l'état de synchronisation des réplicas de disponibilité, et cela peut provoquer temps morts étendus. Vous devez utiliser [!INCLUDE[tsql](../../../includes/tsql-md.md)] ou [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)].  

  >[!WARNING]
  > Le fait d’utiliser le Gestionnaire du cluster de basculement pour déplacer une *instance de cluster de basculement* hébergeant un groupe de disponibilité vers un nœud qui héberge *déjà* un réplica du même groupe de disponibilité, peut entraîner la perte du réplica du groupe de disponibilité, l’empêchant ainsi d’être en ligne sur le nœud cible. Un même nœud d’un cluster de basculement ne peut pas héberger plusieurs réplicas du même groupe de disponibilité. Pour plus d’informations à ce sujet et sur la récupération, consultez le blog [Replica unexpectedly dropped in availability group](/archive/blogs/alwaysonpro/issue-replica-unexpectedly-dropped-in-availability-group). 
  
##  <a name="related-content"></a><a name="RelatedContent"></a> Contenu associé  
  
-   **Blogs :**  
  
     [Configurer le clustering de basculement Windows pour SQL Server (groupe de disponibilité ou instance de cluster de basculement) avec une sécurité limitée](/archive/blogs/sqlalwayson/configure-windows-failover-clustering-for-sql-server-availability-group-or-fci-with-limited-security)  
  
     [Blogs de l’équipe de SQL Server Always On : Blog officiel de l’équipe de SQL Server Always On](/archive/blogs/sqlalwayson/)  
  
     [Blogs des ingénieurs du Service clientèle et du Support technique de SQL Server](/archive/blogs/psssql/)  
  
-   **Livres blancs :**  
  
     [Guide de l’architecture Always On : Création d’une solution haute disponibilité et de récupération d’urgence en utilisant des instances de cluster de basculement et des groupes de disponibilité](/previous-versions/sql/sql-server-2012/jj215886(v=msdn.10))  
  
     [Guide de solutions Microsoft SQL Server Always On pour la haute disponibilité et la récupération d’urgence](/previous-versions/sql/sql-server-2012/hh781257(v=msdn.10))  
  
     [Livres blancs de Microsoft pour SQL Server 2012](https://msdn.microsoft.com/library/hh403491.aspx)  
  
     [Livres blancs de l'équipe de consultants clients de SQL Server](https://techcommunity.microsoft.com/t5/DataCAT/bg-p/DataCAT/)  
  
## <a name="see-also"></a>Voir aussi  
 [Vue d’ensemble des groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Activer et désactiver les groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/enable-and-disable-always-on-availability-groups-sql-server.md)   
 [Surveiller des groupes de disponibilité &#40;Transact-SQL&#41;](../../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)   
 [Instances de cluster de basculement Always On &#40;SQL Server&#41;](../../../sql-server/failover-clusters/windows/always-on-failover-cluster-instances-sql-server.md)  
  
