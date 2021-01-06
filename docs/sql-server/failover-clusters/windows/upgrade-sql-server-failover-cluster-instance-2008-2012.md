---
title: 'Mise à niveau d’instances SQL Server : Clusters Windows Server 2012 et versions antérieures'
description: Décrit comment mettre à niveau vos instances de cluster de basculement SQL Server s’exécutant sur Windows Server 2008, Windows Server 2008 R2 et Windows Server 2012.
ms.date: 06/15/2020
ms.custom: seo-lt-2019
ms.prod: sql
ms.technology: failover-cluster-instance
ms.topic: how-to
helpviewer_keywords:
- upgrading failover clusters
- clusters [SQL Server], upgrading
- failover clustering [SQL Server], upgrading
author: cawrites
ms.author: chadam
ms.openlocfilehash: 90ab761cdc9a84008803ebb4bc5493eb87778f20
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97642675"
---
# <a name="upgrade-sql-server-instances-running-on-windows-server-20082008-r22012-clusters"></a>Mettre à niveau les instances de SQL Server s’exécutant sur des clusters Windows Server 2008/2008 R2/2012

[!INCLUDE[nextref-longhorn-md](../../../includes/nextref-longhorn-md.md)], [!INCLUDE[winserver2008r2-md](../../../includes/winserver2008r2-md.md)] et [!INCLUDE[win8srv-md](../../../includes/win8srv-md.md)] empêchent les clusters de basculement Windows Server d’effectuer des mises à niveau du système d’exploitation sur place, ce qui limite la version autorisée de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] pour un cluster. Une fois que le cluster est mis à niveau avec au moins [!INCLUDE[winblue-server-2-md](../../../includes/winblue-server-2-md.md)], il peut rester à jour.

## <a name="prerequisites"></a>Prérequis

-   Avant d’effectuer l’une des stratégies de migration, un cluster de basculement Windows Server parallèle avec Windows Server 2016/2012 R2 doit être préparé. Tous les nœuds comprenant des instances de cluster de basculement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] doivent être joints au cluster Windows où les instances de cluster de basculement parallèles sont installées. Aucun ordinateur autonome **ne doit** être joint au cluster de basculement Windows Server avant la migration. Les bases de données utilisateur doivent être synchronisées dans le nouvel environnement avant la migration.
-   Toutes les instances de destination doivent exécuter la même version de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] que leur instance parallèle dans l’environnement d’origine, avec les mêmes noms et ID d’instance, et elles doivent être installées avec les mêmes fonctionnalités. Les chemins d’installation et la structure de répertoire doivent être identiques sur les ordinateurs de destination. Cela n’inclut pas les noms de réseaux virtuels des instances de cluster de basculement, qui doivent être différents avant la migration. Toutes les fonctionnalités activées par l’instance d’origine (Always On, FILESTREAM, etc.) doivent être activées sur l’instance de destination.

-   Aucun [!INCLUDE[sshadrc-md](../../../includes/sshadrc-md.md)] ne doit être installé sur le cluster parallèle avant la migration.

-   Le temps d’arrêt lors de la migration d’un cluster qui utilise strictement des groupes de disponibilité (avec ou sans instances de cluster de basculement SQL) peut être considérablement limité à l’aide de groupes de disponibilité distribués, mais cela nécessite que toutes les instances exécutent la version [!INCLUDE[sssql15-md](../../../includes/sssql15-md.md)] RTM (ou ultérieure).

-   Toutes les stratégies de migration requièrent le rôle sysadmin [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Tous les utilisateurs Windows employés par des services [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] (c’est-à-dire un compte Windows exécutant des agents de réplication) doivent disposer d’autorisations au niveau du système d’exploitation sur chaque ordinateur dans le nouvel environnement.

-   Tous les partages de fichiers réseau et lecteurs mappés réseau utilisés dans l’environnement de cluster [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] d’origine doivent toujours exister et rester accessibles au cluster cible avec les mêmes autorisations que les instances d’origine.

-   Tous les ports TCP/IP écoutés par l’instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] d’origine doivent être inutilisés et autoriser le trafic entrant sur l’ordinateur cible.
-   Tous les services liés à SQL doivent être installés et exécutés par le même utilisateur Windows.

-   Les instances de destination doivent être installées avec les mêmes paramètres régionaux que les instances d’origine.

## <a name="migration-scenarios"></a>Scénarios de migration

La stratégie de migration appropriée dépend de certains paramètres de la topologie de cluster [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] d’origine, à savoir l’utilisation de [!INCLUDE[sshadrc-md](../../../includes/sshadrc-md.md)] et des instances de cluster de basculement SQL. La stratégie choisie dépend également de la configuration requise de l’environnement de destination. Si le nouvel environnement requiert que chaque ordinateur ou instance de cluster de basculement SQL conserve le nom de réseau virtuel d’origine, ou si la topologie [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] dépend des nouvelles instances qui héritent de tous les objets système des instances d’origine, nous devons choisir une stratégie qui peut les migrer.

|                                   | Nécessite tous les objets serveur et noms de réseaux virtuels | Nécessite tous les objets serveur et noms de réseaux virtuels | Ne nécessite pas d’objets serveur/de noms de réseaux virtuels\* | Ne nécessite pas d’objets serveur/de noms de réseaux virtuels\* |
|-----------------------------------|--------------------------------------|--------------------------------------------------------------------|------------|------------|
| **_Groupes de disponibilité ? (O/N_)**                  | **_O_**                              | **_N_**                                                            | **_O_**    | **_N_**    |
| **Le cluster utilise l’instance de cluster de basculement SQL uniquement**         | [Scénario 3](#scenario-3-windows-cluster-has-both-sql-fcis-and-sql-server-availability-groups)                           | [Scénario 2](#scenario-2-windows-clusters-with-sql-server-failover-cluster-instances-fcis)                                                        | [Scénario 1](#scenario-1-windows-cluster-with-sql-server-availability-groups-and-no-failover-cluster-instances-fcis) | [Scénario 2](#scenario-2-windows-clusters-with-sql-server-failover-cluster-instances-fcis) |
| **Le cluster utilise des instances autonomes** | [Scénario 5](#scenario-5-windows-cluster-with-standalone-sql-server-instances-and-availability-groups)                           | [Scénario 4](#scenario-4-windows-cluster-with-standalone-sql-server-instances-and-no-availability-groups)                                                         | [Scénario 1](#scenario-1-windows-cluster-with-sql-server-availability-groups-and-no-failover-cluster-instances-fcis) | [Scénario 4](#scenario-4-windows-cluster-with-standalone-sql-server-instances-and-no-availability-groups) |

\* À l’exception des noms des écouteurs de groupe de disponibilité

## <a name="scenario-1-windows-cluster-with-sql-server-availability-groups-and-no-failover-cluster-instances-fcis"></a>Scénario 1 : Cluster Windows avec des groupes de disponibilité SQL Server et aucune instance de cluster de basculement
Si vous avez un programme d’installation [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] qui utilise des groupes de disponibilité mais pas d’instance de cluster de basculement, vous pouvez migrer vers un nouveau cluster en créant un déploiement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] parallèle sur un autre cluster Windows avec Windows Server 2016/2012 R2. Après cela, vous pouvez créer un groupe de disponibilité distribué où le cluster cible est le groupe secondaire pour le cluster de production actuel. Cela nécessite que l’utilisateur effectue la mise à niveau vers [!INCLUDE[sssql15-md](../../../includes/sssql15-md.md)] ou version ultérieure.

###  <a name="to-perform-the-upgrade"></a>Pour effectuer la mise à niveau

1.  Si nécessaire, mettez à niveau toutes les instances vers [!INCLUDE[sssql15-md](../../../includes/sssql15-md.md)] ou version ultérieure. Les instances parallèles doivent exécuter la même version de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].

2.  Créez un groupe de disponibilité pour le cluster cible. Si le nœud principal du cluster cible n’est pas une instance de cluster de basculement, créez un écouteur.

3.  Formez un groupe de disponibilité distribué où le cluster cible est le groupe de disponibilité secondaire.

    >[!NOTE]
    >Le paramètre LISTENER\_URL pour Créer un groupe de disponibilité distribué T-SQL se comporte différemment pour les groupes de disponibilité avec une instance de cluster de basculement SQL comme instance principale. Si c’est le cas pour le groupe de disponibilité principal ou secondaire, utilisez le nom de réseau virtuel de l’instance de cluster de basculement SQL principale comme URL d’écouteur à la place du nom de réseau de l’écouteur, avec le port du point de terminaison de mise en miroir de bases de données.

4.  Joignez le groupe de disponibilité secondaire au groupe de disponibilité distribué.

5.  Joignez les bases de données secondaires sur le groupe de disponibilité secondaire.

    >[!NOTE]
    >Cette opération est effectuée automatiquement si le groupe de disponibilité cible utilise l’amorçage automatique et elle n’est possible que si les chemins des journaux et données sont les mêmes sur tous les réplicas.

6.  Interrompez tout le trafic vers le groupe de disponibilité principal et autorisez le groupe de disponibilité secondaire à synchroniser.

7.  Remplacez la stratégie de validation sur les deux groupes de disponibilité par SYNCHRONOUS_COMMIT et basculez vers le cluster cible une fois que l’état est SYNCHRONIZED.

8.  Supprimez le groupe de disponibilité distribué.

9.  Supprimez ou renommez l’écouteur sur le groupe de disponibilité d’origine.

10. Renommez ou créez l’écouteur du nouveau groupe de disponibilité avec le nom de l’écouteur du groupe de disponibilité d’origine.

    >[!NOTE]
    >Alors que l’enregistrement DNS pour l’écouteur du groupe de disponibilité d’origine existe, les tentatives de création d’un écouteur avec ce nom échouent.

11. Reprenez le trafic vers l’écouteur.

## <a name="scenario-2-windows-clusters-with-sql-server-failover-cluster-instances-fcis"></a>Scénario 2 : Clusters Windows avec des instances de cluster de basculement SQL Server

Si vous avez un environnement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] avec uniquement des instances de cluster de basculement SQL, vous pouvez migrer vers un nouveau cluster en créant un environnement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] parallèle sur un autre cluster Windows avec Windows Server 2016/2012 R2. Vous allez migrer vers le cluster cible en « volant » les noms de réseaux virtuels des anciennes instances de cluster de basculement SQL et en les obtenant sur les nouveaux clusters. Cela va créer un temps d’arrêt supplémentaire qui varie selon les durées de propagation DNS.

###  <a name="to-perform-the-upgrade"></a>Pour effectuer la mise à niveau

1.  Effectuez une sauvegarde complète et arrêtez le trafic vers le cluster [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] d’origine.

2.  Effectuez une sauvegarde de la fin du journal des bases de données utilisateur et une restauration avec récupération dans le nouvel environnement.

3.  Sur le cluster cible dans le Gestionnaire du cluster de basculement, désactivez chaque rôle en cluster d’instance de cluster de basculement SQL.

4.  Toujours dans le Gestionnaire du cluster de basculement sur le cluster cible, réactivez les disques en cluster utilisés par chaque instance de cluster de basculement SQL.

5.  Sur le cluster d’origine dans le Gestionnaire du cluster de basculement, désactivez chaque rôle en cluster d’instance de cluster de basculement SQL.

6.  Toujours dans le Gestionnaire du cluster de basculement sur le cluster d’origine, réactivez les disques en cluster utilisés par chaque instance de cluster de basculement SQL.

7.  Copiez les bases de données système à partir des ordinateurs d’origine sur leur ordinateur cible parallèle.

8.  Dans l’environnement d’origine, dans le Gestionnaire du cluster de basculement, remplacez le nom de la ressource « Nom du serveur » de chaque rôle FCI [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].

9.  Maintenant, remettez juste la ressource de nom du serveur renommée en ligne pour chacun des rôles d’instance de cluster de basculement SQL.

10. Maintenant, sur le cluster cible dans le Gestionnaire du cluster de basculement, réaffectez à la ressource de nom du serveur de chacun des rôles FCI [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] le nom précédemment détenu par le cluster d’origine.

    >[!NOTE]
    >Les erreurs qui surviennent parce que le nom est déjà détenu par un autre ordinateur ne s’affichent plus une fois que les enregistrements DNS pour le nom sont supprimés.

11. Une fois que toutes les instances de cluster de basculement ont été renommées, redémarrez tous les ordinateurs dans le nouveau cluster.

12. Comme les ordinateurs reviennent en ligne après le redémarrage, démarrez chacun des rôles d’instance de cluster de basculement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] dans le Gestionnaire du cluster de basculement.

## <a name="scenario-3-windows-cluster-has-both-sql-fcis-and-sql-server-availability-groups"></a>Scénario 3 : Cluster Windows avec à la fois des instances de cluster de basculement SQL et des groupes de disponibilité SQL Server

Si vous avez un programme d’installation [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] qui n’utilise aucune instance autonome de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], uniquement des instances de cluster de basculement SQL contenues dans au moins un groupe de disponibilité, vous pouvez effectuer cette migration vers un nouveau cluster à l’aide de méthodes similaires à celles du scénario « aucun groupe de disponibilité, aucune instance autonome ». Avant de copier des tables système sur les disques partagés d’instance de cluster de basculement cible, vous devez supprimer tous les groupes de disponibilité dans l’environnement d’origine. Une fois que toutes les bases de données ont été migrées vers les ordinateurs cibles, vous allez recréer les groupes de disponibilité avec les mêmes noms de schéma et d’écouteur. Ce faisant, les ressources de cluster de basculement Windows Server sont correctement formées et gérées sur le cluster cible. **Always On doit être activé dans le Gestionnaire de configuration [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] sur chaque ordinateur dans l’environnement cible avant la migration.**

### <a name="to-perform-the-upgrade"></a>Pour effectuer la mise à niveau

1.  Arrêtez le trafic vers [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].

2.  Effectuez une sauvegarde de la fin du journal des bases de données utilisateur et une restauration avec récupération sur le réplica principal prévu du nouvel environnement, et avec NORECOVERY sur tous les réplicas secondaires prévus.

3.  Sur le cluster cible dans le Gestionnaire du cluster de basculement, désactivez chaque rôle en cluster d’instance de cluster de basculement SQL.

4.  Toujours dans le Gestionnaire du cluster de basculement sur le cluster cible, réactivez les disques en cluster utilisés par chaque instance de cluster de basculement SQL.

5.  Sur le cluster d’origine, supprimez le groupe de disponibilité.

6.  Sur le cluster d’origine dans le Gestionnaire du cluster de basculement, désactivez chaque rôle en cluster d’instance de cluster de basculement SQL.

7.  Toujours dans le Gestionnaire du cluster de basculement sur le cluster d’origine, réactivez les disques en cluster utilisés par chaque instance de cluster de basculement SQL.

8.  Copiez les bases de données système à partir des ordinateurs d’origine sur leur ordinateur cible parallèle.

9.  Dans l’environnement d’origine, dans le Gestionnaire du cluster de basculement, remplacez le nom de la ressource « Nom du serveur » de chaque rôle FCI [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].

10. Maintenant, remettez juste la ressource de nom du serveur renommée en ligne pour chacun des rôles d’instance de cluster de basculement SQL.

11. Maintenant, sur le cluster cible dans le Gestionnaire du cluster de basculement, réaffectez à la ressource de nom du serveur de chacun des rôles FCI [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] le nom précédemment détenu par le cluster d’origine.

12. Une fois que toutes les instances de cluster de basculement ont été renommées, redémarrez tous les ordinateurs dans le nouveau cluster.

13. Comme les ordinateurs reviennent en ligne après le redémarrage, démarrez chacun des rôles d’instance de cluster de basculement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] dans le Gestionnaire du cluster de basculement.

14. Une fois que toutes les instances sont en ligne, formez le groupe de disponibilité sur le réplica où les bases de données ont été restaurées avec récupération.

15. Joignez tous les réplicas secondaires et toutes les bases de données secondaires au groupe de disponibilité.

16. Créez un écouteur dans le nouveau groupe de disponibilité avec le nom de l’écouteur du groupe de disponibilité d’origine.

## <a name="scenario-4-windows-cluster-with-standalone-sql-server-instances-and-no-availability-groups"></a>Scénario 4 : Cluster Windows avec des instances SQL Server autonomes et sans aucun groupe de disponibilité

La migration d’un cluster avec des instances autonomes est similaire au processus de migration d’un cluster [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] avec uniquement des instances de cluster de basculement mais, au lieu de modifier le nom de réseau virtuel de la ressource de cluster de l’instance de cluster de basculement, vous changez le nom de l’ordinateur autonome d’origine et « volez » le nom de l’ancien ordinateur sur l’ordinateur cible. Cette opération génère un temps d’arrêt supplémentaire par rapport aux scénarios sans instance autonome, car vous ne pouvez pas joindre l’ordinateur autonome cible au cluster WSFC tant que vous n’avez pas obtenu le nom de réseau de l’ancien ordinateur.

###  <a name="to-perform-the-upgrade"></a>Pour effectuer la mise à niveau

1.  Arrêtez le trafic vers [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].

2.  Effectuez une sauvegarde de la fin du journal des bases de données utilisateur et une restauration avec récupération dans le nouvel environnement sur chaque ordinateur.

3.  Sur le cluster cible dans le Gestionnaire du cluster de basculement, désactivez chaque rôle en cluster d’instance de cluster de basculement SQL.

4.  Toujours sur le cluster cible dans le Gestionnaire du cluster de basculement, réactivez les disques en cluster utilisés par chaque instance de cluster de basculement SQL.

5.  Sur le cluster d’origine dans le Gestionnaire du cluster de basculement, désactivez chaque rôle en cluster d’instance de cluster de basculement SQL et arrêtez les instances autonomes de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] dans le Gestionnaire de configuration [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].

6.  Affectez à chaque ordinateur autonome dans le cluster d’origine un nouveau nom unique. Redémarrez chacun de ces ordinateurs comme indiqué.

7.  Sur le cluster d’origine dans le Gestionnaire du cluster de basculement, réactivez les disques en cluster utilisés par chaque instance de cluster de basculement SQL.

8.  Copiez les bases de données système sur les ordinateurs cibles.

9.  Dans l’environnement d’origine dans le Gestionnaire du cluster de basculement, affectez un nouveau nom unique à la ressource « Nom du serveur » de chaque rôle FCI [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].

10. Maintenant, remettez juste la ressource de nom du serveur renommée en ligne pour chacun des rôles d’instance de cluster de basculement SQL.

11. Maintenant, sur les instances autonomes parallèles, affectez aux ordinateurs les noms des ordinateurs autonomes d’origine. (Supprimez l’ancien nom de serveur, ajoutez le nouveau nom de serveur avec paramètre local.) Redémarrez les ordinateurs comme indiqué.

12. Après le redémarrage, joignez tous les ordinateurs autonomes au cluster de basculement Windows Server cible.

13. Maintenant, sur le cluster cible dans le Gestionnaire du cluster de basculement, réaffectez à la ressource de nom du serveur de chacun des rôles FCI [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] le nom précédemment détenu par le cluster d’origine.

14. Une fois que toutes les instances de cluster de basculement ont été renommées, redémarrez tous les ordinateurs dans le nouveau cluster.

15. Comme les ordinateurs reviennent en ligne après le redémarrage, démarrez chacun des rôles d’instance de cluster de basculement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] dans le Gestionnaire du cluster de basculement.

## <a name="scenario-5-windows-cluster-with-standalone-sql-server-instances-and-availability-groups"></a>Scénario 5 : Cluster Windows avec des instances SQL Server autonomes et des groupes de disponibilité

La migration d’un cluster qui utilise des groupes de disponibilité avec des réplicas autonomes est similaire au processus de migration d’un cluster avec des instances de cluster de basculement utilisant des groupes de disponibilité. Vous devez toujours supprimer les groupes de disponibilité d’origine et les reconstruire sur le cluster cible ; toutefois, un temps d’arrêt supplémentaire est généré en raison des frais additionnels de la migration des instances autonomes. **Always On doit être activé sur chaque instance de cluster de basculement dans l’environnement cible avant la migration.**

###  <a name="to-perform-the-upgrade"></a>Pour effectuer la mise à niveau

1.  Arrêtez le trafic vers [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].

2.  Effectuez une sauvegarde de la fin du journal des bases de données utilisateur et une restauration avec récupération dans le nouvel environnement sur le réplica principal prévu, et avec NORECOVERY sur chaque réplica secondaire prévu.

3.  Supprimez le groupe de disponibilité sur le cluster d’origine.

4.  Sur le cluster cible dans le Gestionnaire du cluster de basculement, désactivez chaque rôle en cluster d’instance de cluster de basculement SQL.

5.  Toujours sur le cluster cible dans le Gestionnaire du cluster de basculement, réactivez les disques en cluster utilisés par chaque instance de cluster de basculement SQL.

6.  Sur le cluster d’origine dans le Gestionnaire du cluster de basculement, désactivez chaque rôle en cluster d’instance de cluster de basculement SQL et arrêtez les instances autonomes de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] dans le Gestionnaire de configuration [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].

7.  Affectez à chaque ordinateur autonome dans le cluster d’origine un nouveau nom unique. Redémarrez chacun de ces ordinateurs comme indiqué.

8.  Sur le cluster d’origine dans le Gestionnaire du cluster de basculement, réactivez les disques en cluster utilisés par chaque instance de cluster de basculement SQL.

9.  Copiez les bases de données système sur les ordinateurs cibles.

10. Dans l’environnement d’origine dans le Gestionnaire du cluster de basculement, affectez un nouveau nom unique à la ressource « Nom du serveur » de chaque rôle FCI [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].

11. Maintenant, remettez juste la ressource de nom du serveur renommée en ligne pour chacun des rôles d’instance de cluster de basculement SQL.

12. Maintenant, sur les instances autonomes parallèles, affectez aux ordinateurs les noms des ordinateurs autonomes d’origine. (Supprimez et ajoutez le nom de serveur dans SQL.) Redémarrez les ordinateurs comme indiqué.

13. Après le redémarrage, joignez tous les ordinateurs autonomes au cluster de basculement Windows Server cible.

14. Maintenant, sur le cluster cible dans le Gestionnaire du cluster de basculement, réaffectez à la ressource de nom du serveur de chacun des rôles FCI [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] le nom précédemment détenu par le cluster d’origine.

15. Une fois que toutes les instances de cluster de basculement ont été renommées, redémarrez tous les ordinateurs dans le nouveau cluster.

16. Comme les ordinateurs reviennent en ligne après le redémarrage, démarrez chacun des rôles d’instance de cluster de basculement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] dans le Gestionnaire du cluster de basculement.

17. Une fois que toutes les instances sont en ligne, recréez le groupe de disponibilité sur le réplica principal prévu.

18. Joignez chaque réplica secondaire et base de données secondaire.

19. Recréez l’écouteur de groupe de disponibilité avec le même nom que celui d’origine.

## <a name="specific-concerns-for-individual-features"></a>Problèmes spécifiques pour des fonctionnalités individuelles

### [!INCLUDE[sshadrc-md](../../../includes/sshadrc-md.md)]

-   **Point de terminaison de mise en miroir de bases de données**

    D’un point de vue SQL, le point de terminaison de mise en miroir de bases de données est migré vers la nouvelle instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] avec les tables système. Avant la migration, vérifiez que les règles appropriées sont appliquées dans les pare-feu et qu’aucun autre processus n’est à l’écoute sur le même port.

-   **Groupes de disponibilité**

    Les groupes de disponibilité et leurs écouteurs ne peuvent pas migrer entre des instances. Les ressources de cluster de basculement Windows Server créées par le groupe de disponibilité ne peuvent pas être facilement recréées dans l’environnement cible. Plutôt que de tenter de migrer les groupes de disponibilité, nous cherchons à supprimer et recréer les groupes de disponibilité sur le cluster cible.

-   **Écouteurs de groupe de disponibilité**

    Comme pour les groupes de disponibilité eux-mêmes, nous supprimons et recréons les écouteurs au lieu de les migrer directement.

### <a name="replication"></a>Réplication

-   **Serveurs de distribution distants, serveurs de publication, abonnés**

    La relation entre un serveur de distribution et un serveur de publication s’appuie uniquement sur le nom de réseau virtuel des ordinateurs qui hébergent les deux, ce qui permet une résolution correcte en nouvel ordinateur. Les travaux de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Agent sont également correctement migrés avec les tables système pour que les divers agents de réplication soient en mesure de continuer l’exécution comme d’habitude. Il est nécessaire avant la migration que tous les comptes Windows qui exécutent [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Agent lui-même ou tout travail de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Agent aient les mêmes autorisations dans l’environnement cible. La communication avec le serveur de publication et les abonnés s’effectue comme d’habitude.

-   **Dossier d'instantanés**

    Il est nécessaire avant la migration que les partages réseau utilisés par les fonctionnalités [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] soient accessibles par les ordinateurs dans l’environnement cible avec les mêmes autorisations que dans l’environnement d’origine. Vous devez vérifier que cette condition est respectée avant la migration.

### <a name="service-broker"></a>Service Broker

-   **Point de terminaison Service Broker**

    D’un point de vue [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], il n’existe aucun problème avec le point de terminaison. Avant la migration, vous devez vérifier qu’aucun processus n’est déjà à l’écoute sur le même port et qu’aucune règle de pare-feu ne bloque ce port, ou qu’il existe une règle de pare-feu autorisant précisément le port.

-   **Certificates**

    Les certificats doivent également être sauvegardés et restaurés sur les ordinateurs cibles dans le cas où le certificat devrait être restauré sur un nouvel ordinateur.

-   **Itinéraires**

    Les itinéraires dépendent du nom de réseau virtuel de la cible ce qui permet d’obtenir les noms des ordinateurs et les noms de réseau d’instance de cluster de basculement SQL correspondant aux ordinateurs corrects dans le nouvel environnement. N’importe quel autre nom de réseau virtuel référencé doit également être redirigé vers le nouvel ordinateur.

-   **Liaisons de service distant**

    Les liaisons de service distant fonctionnent comme prévu après la migration, quand tout utilisateur employant une liaison de ce type est correctement migré.

### <a name="ssnoversion-agent"></a>[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Agent

-   **Tâches**

    Les travaux sont correctement migrés avec les bases de données système. Tout utilisateur qui exécute un travail de SQL Agent ou SQL Agent lui-même a les mêmes autorisations sur l’ordinateur cible, comme indiqué dans les conditions préalables.

-   **Alertes et opérateurs**

    Les alertes et opérateurs sont correctement migrés avec les bases de données système.

### <a name="filestream"></a>FILESTREAM

-   **Ports de partage de fichiers Windows**

    Les ports de partage de fichiers Windows 139 et 445 doivent tous deux avoir des règles autorisant le trafic entrant pour utiliser FILESTREAM

-   **Partage Windows**

    Le chemin du partage Windows dépend de la ressource de nom d’instance de cluster de basculement SQL, comme accessible par `\\ServerName\ShareName`. La migration de FILESTREAM nécessite que tous les nœuds dans une instance de cluster de basculement cible activent FILESTREAM et, si un partage Windows est utilisé, qu’ils soient configurés pour utiliser le même nom pour le partage Windows que l’ordinateur d’origine. Une fois que l’instance de cluster de basculement cible obtient le nom de serveur correct, elle héberge le partage Windows avec le chemin souhaité.

-   **Données FILESTREAM**

    Les données FILESTREAM sont incluses dans la sauvegarde.

### <a name="integration-services"></a>Integration Services

-   **Projets SSIS**

    Les projets SSIS sont migrés avec la base de données SSIS. Une fois que la base de données SSIS est déplacée, les packages sont immédiatement exécutables avant de déplacer des tables système.

-   **Sources de données basées sur des fichiers**

    Les fichiers plats, fichiers Excel, sources XML et autres doivent être accessibles au même emplacement spécifié par le package SSIS.

## <a name="next-steps"></a>Étapes suivantes
- [Mise à niveau du moteur de base de données](../../../database-engine/install-windows/complete-the-database-engine-upgrade.md)
- [Modifier le mode de compatibilité de base de données et utiliser le magasin des requêtes](../../../database-engine/install-windows/change-the-database-compatibility-mode-and-use-the-query-store.md)
- [Tirer parti des nouveautés de SQL Server 2016](../../what-s-new-in-sql-server-2017.md)
- [Mettre à niveau une instance de cluster de basculement SQL Server](upgrade-a-sql-server-failover-cluster-instance.md)
- [Afficher et lire les fichiers journaux d'installation de SQL Server](../../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md)
- [Ajouter des fonctionnalités à une instance de SQL Server 2016 (programme d’installation)](../../../database-engine/install-windows/add-features-to-an-instance-of-sql-server-setup.md)