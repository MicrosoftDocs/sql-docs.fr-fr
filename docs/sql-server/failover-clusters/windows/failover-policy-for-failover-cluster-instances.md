---
title: Stratégie de basculement pour les instances de cluster de basculement
description: Description des différentes stratégies de basculement disponibles pour une instance de cluster de basculement SQL Server.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: failover-cluster-instance
ms.topic: conceptual
helpviewer_keywords:
- flexible failover policy
ms.assetid: 39ceaac5-42fa-4b5d-bfb6-54403d7f0dc9
author: cawrites
ms.author: chadam
ms.openlocfilehash: 9101f2cf26e797cc63a909a85c3eefa3fb083b4f
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/02/2021
ms.locfileid: "99251191"
---
# <a name="failover-policy-for-failover-cluster-instances"></a>Stratégie de basculement pour les instances de cluster de basculement
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Dans une instance de cluster de basculement (FCI) [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , un seul nœud peut posséder le groupe de ressources de cluster de basculement Windows Server (WSFC) à un moment donné. Les demandes des clients sont servies par ce nœud dans la FCI. En cas d'échec et d'un redémarrage infructueux, la propriété du groupe est déplacée vers un autre nœud WSFC dans la FCI. Ce processus s'appelle le basculement. [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] augmente la fiabilité de la détection de pannes et fournit une stratégie de basculement flexible.  
  
 Une FCI [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] dépend du service WSFC sous-jacent pour la détection de basculement. Par conséquent, deux mécanismes déterminent le comportement du basculement pour la FCI : l'ancien mécanisme est une fonctionnalité WSFC native et le dernier est une fonctionnalité ajoutée par la configuration [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
-   Le cluster WSFC conserve la configuration du quorum, ce qui garantit une seule cible de basculement dans un basculement automatique. Le service WSFC détermine si le cluster est dans une intégrité de quorum optimale à tout moment et met le groupe de ressources en ligne et hors connexion en conséquence.  
  
-   L'instance active de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] communique périodiquement un ensemble de diagnostics de composant au groupe de ressources WSFC sur une connexion dédiée. Le groupe de ressources WSFC conserve la stratégie de basculement, qui définit les conditions d'échec qui déclenchent les redémarrages et les basculements.  
  
 Cette rubrique décrit le deuxième mécanisme précédemment cité. Pour plus d’informations sur le comportement de WSFC en ce qui concerne la configuration du quorum et la détection d’intégrité, consultez [Modes de quorum WSFC et configuration de vote &#40;SQL Server&#41;](../../../sql-server/failover-clusters/windows/wsfc-quorum-modes-and-voting-configuration-sql-server.md).  
  
> [!IMPORTANT]  
>  Les basculements automatiques vers et depuis une instance de cluster de basculement ne sont pas autorisés dans un groupe de disponibilité Always On. Toutefois, les basculements manuels vers et depuis une instance de cluster de basculement dans un groupe de disponibilité Always On sont possibles.  
  
##  <a name="failover-policy-overview"></a><a name="Concepts"></a> Présentation de la stratégie de basculement  
 Le processus de basculement comporte les étapes suivantes :  
  
1.  [Surveillance de l'état d'intégrité](../../../sql-server/failover-clusters/windows/failover-policy-for-failover-cluster-instances.md#monitor)  
  
2.  [Détermination des échecs](../../../sql-server/failover-clusters/windows/failover-policy-for-failover-cluster-instances.md#determine)  
  
3.  [Réponse aux échecs](../../../sql-server/failover-clusters/windows/failover-policy-for-failover-cluster-instances.md#respond)  
  
###  <a name="monitor-the-health-status"></a><a name="monitor"></a> Surveillance de l'état d'intégrité  
 Il existe trois types d'états qui sont surveillés pour la FCI :  
  
-   [État de l'instance du service SQL Server](../../../sql-server/failover-clusters/windows/failover-policy-for-failover-cluster-instances.md#service)  
  
-   [Réactivité de l'instance SQL Server](../../../sql-server/failover-clusters/windows/failover-policy-for-failover-cluster-instances.md#instance)  
  
-   [Diagnostics de composant SQL Server](../../../sql-server/failover-clusters/windows/failover-policy-for-failover-cluster-instances.md#component)  
  
####  <a name="state-of-the-sql-server-service"></a><a name="service"></a> État de l'instance du service SQL Server  
 Le service WSFC surveille l'état de démarrage du service SQL Server sur le nœud FCI actif pour détecter l'arrêt du service SQL Server.  
  
####  <a name="responsiveness-of-the-sql-server-instance"></a><a name="instance"></a> Réactivité de l'instance SQL Server  
 Pendant le démarrage de SQL Server, le service WSFC utilise la DLL de ressource du moteur de base de données SQL Server pour créer une nouvelle connexion sur un thread séparé utilisé exclusivement pour surveiller l'état d'intégrité. Cela garantit que l'instance SQL dispose des ressources requises pour signaler son état d'intégrité lorsqu'elle est sous charge. En utilisant cette connexion dédiée, SQL Server exécute la procédure stockée système [sp_server_diagnostics &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-server-diagnostics-transact-sql.md) en mode de répétition pour signaler périodiquement l’état d’intégrité des composants SQL Server à la DLL de ressource.  
  
 La DLL de ressource détermine la réactivité de l'instance SQL en utilisant un délai d'attente de contrôle d'intégrité. La propriété HealthCheckTimeout définit la durée pendant laquelle la DLL de ressource doit attendre la procédure stockée sp_server_diagnostics avant de signaler que l'instance SQL ne répond pas au service WSFC. Cette propriété est configurable à l'aide de T-SQL ainsi que dans le composant logiciel enfichable Gestionnaire du cluster de basculement. Pour plus d’informations, consultez [Configurer les paramètres de propriété HealthCheckTimeout](../../../sql-server/failover-clusters/windows/configure-healthchecktimeout-property-settings.md). Les éléments suivants décrivent comment cette propriété affecte le délai d'attente et les paramètres d'intervalle de répétition :  
  
-   La DLL de ressource appelle la procédure stockée sp_server_diagnostics et définit l'intervalle de répétition à un tiers du paramètre HealthCheckTimeout.  
  
-   Si la procédure stockée sp_server_diagnostics est lente ou si elle ne retourne pas d'informations, la DLL de ressource attendra l'intervalle spécifié par HealthCheckTimeout avant de signaler au service WSFC que l'instance SQL est sans réponse.  
  
-   Si la connexion dédiée est perdue, la DLL de ressource retentera la connexion à l'instance SQL à l'intervalle spécifié par HealthCheckTimeout avant de signaler au service WSFC que l'instance SQL ne répond pas.  
  
####  <a name="sql-server-component-diagnostics"></a><a name="component"></a> Diagnostics de composant SQL Server  
 La procédure stockée système sp_server_diagnostics collecte périodiquement les diagnostics de composant sur l'instance SQL. Les informations de diagnostic collectées sont exposées en surface sous forme de ligne pour chacun des composants suivants et passées au thread appelant.  
  
1.  système  
  
2.  resource  
  
3.  query process  
  
4.  io_subsystem  
  
5.  événements  
  
 Les composants system, resource et query process sont utilisés pour la détection de défaillance. Les composants io_subsytem et events sont utilisés uniquement à des fins de diagnostic.  
  
 Chaque ensemble de lignes d'informations est également écrit dans le journal de diagnostic du cluster SQL Server. Pour plus d’informations, consultez [Afficher et lire le journal de diagnostic de l’instance de cluster de basculement](../../../sql-server/failover-clusters/windows/view-and-read-failover-cluster-instance-diagnostics-log.md).  
  
> [!TIP]  
>  Alors que la procédure stockée sp_server_diagnostic est utilisée par la technologie SQL Server Always On, elle peut être utilisée dans n’importe quelle instance de SQL Server pour aider à détecter et résoudre les problèmes.  
  
####  <a name="determining-failures"></a><a name="determine"></a> Détermination des échecs  
 La DLL de ressource du moteur de base de données SQL Server détermine si l'état d'intégrité détecté est une condition d'échec à l'aide de la propriété FailureConditionLevel. La propriété FailureConditionLevel définit les états d'intégrité détectés qui causent les redémarrages ou les basculements. Plusieurs niveaux d'options sont disponibles : il est possible de définir qu'aucun redémarrage ou du basculement automatique ne sera effectué ou bien de configurer les conditions d'échec possibles aboutissant à un redémarrage ou un basculement automatique. Pour plus d’informations sur la configuration de cette propriété, consultez [Configurer les paramètres de propriété FailureConditionLevel](../../../sql-server/failover-clusters/windows/configure-failureconditionlevel-property-settings.md).  
  
 Les conditions d'échec sont définies sur une échelle croissante. Pour les niveaux 1-5, chaque niveau inclut toutes les conditions des niveaux précédents en plus de ses propres conditions. Cela signifie qu'à chaque niveau, la probabilité de basculement ou de redémarrage est plus importante. Les niveaux de la condition d'échec sont décrits dans le tableau suivant.  
  
 Examinez la procédure stockée système [sp_server_diagnostics &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-server-diagnostics-transact-sql.md), car elle joue un rôle important pour les niveaux de condition d’échec.  
  
|Level|Condition|Description|  
|-----------|---------------|-----------------|  
|0|Aucun basculement ou redémarrage automatique|Indique qu'aucun basculement ou redémarrage ne sera déclenché automatiquement sur n'importe quelle condition d'échec. Ce niveau existe uniquement à des fins de maintenance système.|  
|1|Basculement ou redémarrage sur arrêt du serveur|Indique qu'un redémarrage ou basculement de serveur sera déclenché en fonction de la condition suivante :<br /><br /> Le service SQL Server est fermé.|  
|2|Basculement ou redémarrage sur non-réponse du serveur|Indique qu'un redémarrage ou basculement de serveur sera déclenché si l'une des conditions suivantes est rencontrée :<br /><br /> Le service SQL Server est fermé.<br /><br /> L'instance SQL Server ne répond pas (la DLL de ressource ne reçoit pas des données de sp_server_diagnostics dans les paramètres HealthCheckTimeout).|  
|3*|Basculement ou redémarrage sur des erreurs de serveur critiques|Indique qu'un redémarrage ou basculement de serveur sera déclenché si l'une des conditions suivantes est rencontrée :<br /><br /> Le service SQL Server est fermé.<br /><br /> L'instance SQL Server ne répond pas (la DLL de ressource ne reçoit pas des données de sp_server_diagnostics dans les paramètres HealthCheckTimeout).<br /><br /> La procédure stockée système sp_server_diagnostics retourne « erreur système ».|  
|4|Basculement ou redémarrage sur des erreurs de serveur modérées|Indique qu'un redémarrage ou basculement de serveur sera déclenché si l'une des conditions suivantes est rencontrée :<br /><br /> Le service SQL Server est fermé.<br /><br /> L'instance SQL Server ne répond pas (la DLL de ressource ne reçoit pas des données de sp_server_diagnostics dans les paramètres HealthCheckTimeout).<br /><br /> La procédure stockée système sp_server_diagnostics retourne « erreur système ».<br /><br /> La procédure stockée système sp_server_diagnostics retourne « erreur de ressource ».|  
|5|Basculement ou redémarrage sur toutes les conditions d'échec qualifiées|Indique qu'un redémarrage ou basculement de serveur sera déclenché si l'une des conditions suivantes est rencontrée :<br /><br /> Le service SQL Server est fermé.<br /><br /> L'instance SQL Server ne répond pas (la DLL de ressource ne reçoit pas des données de sp_server_diagnostics dans les paramètres HealthCheckTimeout).<br /><br /> La procédure stockée système sp_server_diagnostics retourne « erreur système ».<br /><br /> La procédure stockée système sp_server_diagnostics retourne « erreur de ressource ».<br /><br /> La procédure stockée système sp_server_diagnostics retourne une « erreur query_processing ».|  
  
 *Valeur par défaut  
  
####  <a name="responding-to-failures"></a><a name="respond"></a> Réponse aux échecs  
 Lorsqu'une ou plusieurs conditions d'échec sont détectées, la manière dont le service WSFC y répond dépend de l'état du quorum WSFC et des paramètres de redémarrage et de basculement du groupe de ressources de la FCI. Si la FCI a perdu le quorum WSFC, alors elle est entièrement mise hors connexion et perd sa haute disponibilité. Si la FCI conserve son quorum WSFC, le service WSFC peut répondre en tentant d'abord de redémarrer le nœud en échec, puis tente un basculement si les tentatives de redémarrage ont échoué. Les paramètres de redémarrage et de basculement sont configurés dans le composant logiciel enfichable Gestionnaire du cluster de basculement. Pour plus d’informations sur ces paramètres, consultez [\<Resource>Propriétés : onglet Stratégies](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc725685(v=ws.11)).  
  
 Pour plus d’informations sur la conservation de l’intégrité du quorum, consultez [Modes de quorum WSFC et configuration de vote &#40;SQL Server&#41;](../../../sql-server/failover-clusters/windows/wsfc-quorum-modes-and-voting-configuration-sql-server.md).  
  
## <a name="see-also"></a>Voir aussi  
 [ALTER SERVER CONFIGURATION &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-server-configuration-transact-sql.md)  
  
