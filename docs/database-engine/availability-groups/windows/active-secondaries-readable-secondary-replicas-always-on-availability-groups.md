---
title: Déplacer une charge de travail vers un réplica de groupe de disponibilité secondaire
description: Découvrez le déchargement de rapports et requêtes en lecture seule vers un réplica secondaire d’un groupe de disponibilité Always On sur SQL Server.
ms.custom: seo-lt-2019
ms.date: 06/06/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
helpviewer_keywords:
- connection access to availability replicas
- Availability Groups [SQL Server], availability replicas
- Availability Groups [SQL Server], readable secondary replicas
- active secondary replicas [SQL Server], read-only access to
- readable secondary replicas
- Availability Groups [SQL Server], active secondary replicas
ms.assetid: 78f3f81a-066a-4fff-b023-7725ff874fdf
author: cawrites
ms.author: chadam
ms.openlocfilehash: 1b7be0ce3e3be27633c32e031f7c674804a43d8b
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/13/2021
ms.locfileid: "98170811"
---
# <a name="offload-read-only-workload-to-secondary-replica-of-an-always-on-availability-group"></a>Décharger une charge de travail en lecture seule vers un réplica secondaire d’un groupe de disponibilité Always On
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  Les fonctions secondaires actives de [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] incluent la prise en charge de l’accès en lecture seule à un ou plusieurs réplicas secondaires (*réplicas secondaires accessibles en lecture*). Un réplica secondaire accessible en lecture peut être en mode de disponibilité en validation synchrone ou en mode de disponibilité en validation asynchrone. Un réplica secondaire accessible en lecture autorise l'accès en lecture seule à toutes ses bases de données secondaires. Toutefois, les bases de données secondaires accessibles en lecture ne sont pas définies en lecture seule. Elles sont dynamiques. Chaque base de données secondaire change à mesure que les modifications apportées à la base de données primaire sont répercutées sur elle. Pour un réplica secondaire classique, les données des bases de données secondaires, y compris les tables optimisées en mémoire durables, sont quasiment synchronisées en temps réel. De plus, les index de recherche en texte intégral sont synchronisés avec les bases de données secondaires. En général, la latence des données entre une base de données primaire et la base de données secondaire correspondante n'est que de quelques secondes.  
  
 Les paramètres de sécurité définis pour les bases de données primaires sont également appliqués dans les bases de données secondaires. Cela inclut les utilisateurs, les rôles de base de données et les rôles d'applications avec leurs autorisations respectives, ainsi que le chiffrement transparent des données s'il est activé sur la base de données primaire.  
  
> [!NOTE]  
>  Bien que vous ne puissiez pas écrire de données dans les bases de données secondaires, vous pouvez écrire dans d’autres bases de données en lecture-écriture sur l’instance de serveur qui héberge le réplica secondaire, notamment les bases de données utilisateur et les bases de données système telles que **tempdb**.  
  
 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] prend également en charge le reroutage des demandes de connexion de tentative de lecture sur un réplica secondaire accessible en lecture (*routage en lecture seule*). Pour plus d’informations sur le routage en lecture seule, consultez [Utilisation d’un écouteur pour se connecter à un réplica secondaire en lecture seule (routage en lecture seule)](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md#ConnectToSecondary).  
  
##  <a name="benefits"></a><a name="bkmk_Benefits"></a> Avantages  
 La direction des connexions en lecture seule vers les réplicas secondaires accessibles en lecture offre les avantages suivants :  
  
-   Décharge vos charges de travail en lecture seule secondaires de votre réplica principal, qui conserve ses ressources pour vos charges de travail critiques. Si vous avez une charge de travail en lecture critique ou si la charge de travail ne tolère aucune latence, vous pouvez l'exécuter sur le réplica principal.  
  
-   Améliore votre retour sur investissement pour les systèmes qui hébergent des réplicas secondaires accessibles en lecture.  
  
 En outre, les réplicas secondaires accessibles en lecture assurent une prise en charge fiable des opérations en lecture seule, comme suit :  
  
-   Les statistiques temporaires automatiques sur la base de données secondaire accessible en lecture optimisent les requêtes en lecture seule sur les tables sur disque. Pour les tables optimisées en mémoire, les statistiques manquantes sont créées automatiquement. Cependant, aucune mise à jour automatique des statistiques obsolètes n'a lieu. Vous devrez mettre manuellement à jour les statistiques sur le réplica principal. Pour plus d’informations, consultez [Statistiques des bases de données d’accès en lecture seule](#Read-OnlyStats), plus loin dans cette rubrique.  
  
-   Les charges de travail en lecture seule pour les tables sur disque utilisent le contrôle de version de ligne pour supprimer la contention de blocage sur les bases de données secondaires. Toutes les requêtes exécutées sur les bases de données secondaires sont automatiquement mappées au niveau des transactions d'isolation d'instantané, même lorsque d'autres niveaux d'isolation des transactions sont définis de manière explicite. De plus, tous les indicateurs de verrouillage sont ignorés. Cela élimine la contention de lecture/écriture.  
  
-   Les charges de travail en lecture seule pour les tables durables optimisées en mémoire accèdent aux données de la même manière que sur la base de données primaire, à l’aide des procédures stockées natives ou de l’interopérabilité SQL présentant les mêmes limitations de niveau d’isolation des transactions (consultez [Niveaux d’isolation du moteur de base de données](/previous-versions/sql/sql-server-2008-r2/ms189122(v=sql.105))). La charge de travail de création de rapports ou les requêtes en lecture seule en cours d'exécution sur le réplica principal peuvent être exécutées sur le réplica secondaire sans procéder à la moindre modification. De la même manière, une charge de travail de création de rapports ou des requêtes en lecture seule en cours d'exécution sur un réplica secondaire peuvent être exécutées sur le réplica principal sans procéder à la moindre modification.  Similaires aux tables sur disque, toutes les requêtes exécutées sur les bases de données secondaires sont automatiquement mappées au niveau des transactions d'isolation d'instantané, même lorsque d'autres niveaux d'isolation des transactions sont définis de manière explicite.  
  
-   Les opérations DML sont autorisées sur les variables de table pour les types de tables sur disque et optimisées en mémoire sur le réplica secondaire.  
  
##  <a name="prerequisites-for-the-availability-group"></a><a name="bkmk_Prerequisites"></a> Conditions préalables requises pour le groupe de disponibilité  
  
-   **Accès en lecture aux réplicas secondaires (requis)**  
  
     L'administrateur de base de données doit configurer un ou plusieurs réplicas de sorte qu'en cas d'exécution sous le rôle secondaire, ces derniers autorisent toutes les connexions (uniquement pour l'accès en lecture seule) ou uniquement les connexions de tentative de lecture.  
  
    > [!NOTE]  
    >  Éventuellement, l'administrateur de base de données peut configurer un réplica de disponibilité pour exclure les connexions en lecture seule en cas d'exécution sous le rôle principal.
  
     Pour plus d’informations, consultez [À propos de l’accès de la connexion client aux réplicas de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/about-client-connection-access-to-availability-replicas-sql-server.md).  
  
    >[!WARNING]
    >  Seuls les réplicas qui se trouvent sur la même version majeure de SQL Server seront lisibles. Pour plus d’informations, consultez [Bases de la mise à niveau propagée](upgrading-always-on-availability-group-replica-instances.md#rolling-upgrade-basics-for-always-on-ags).
  
  
-   **Écouteur de groupe de disponibilité**  
  
     Pour prendre en charge le routage en lecture seule, un groupe de disponibilité doit posséder un [écouteur de groupe de disponibilité](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md). Le client en lecture seule doit diriger ses demandes de connexion à cet écouteur, et la chaîne de connexion du client doit spécifier la tentative d'application « en lecture seule ». Autrement dit, il doit s’agir de *demandes de connexion d’intention de lecture*.  
  
-   **Routage en lecture seule**  
  
     Le *routage en lecture seule* fait référence à la capacité de SQL Server d’acheminer les demandes de connexions de tentative de lecture entrantes, qui sont dirigées vers un écouteur de groupe de disponibilité, vers un réplica secondaire accessible en lecture disponible. Les conditions préalables requises pour le routage en lecture seule sont les suivantes :  
  
    -   Pour prendre en charge le routage en lecture seule, un réplica secondaire accessible en lecture requiert une URL de routage en lecture seule. Cette URL est effective uniquement lorsque le réplica local s'exécute sous le rôle secondaire. L'URL de routage en lecture seule doit être spécifiée par réplica, si nécessaire. Chaque URL de routage en lecture seule est utilisée pour acheminer les demandes de connexion d'intention de lecture vers un réplica secondaire lisible spécifique. En général, à chaque réplica secondaire lisible est affecté une URL de routage en lecture seule.  
  
    -   Chaque réplica de disponibilité qui doit prendre en charge le routage en lecture seule lorsqu'il est le réplica principal requiert une liste de routage en lecture seule. Une liste de routage en lecture seule donnée est effective uniquement lorsque le réplica local s'exécute sous le rôle principal. Cette liste doit être spécifiée par réplica, si nécessaire. En général, chaque liste de routage en lecture seule contient toutes les URL de routage en lecture seule, avec l'URL du réplica local à la fin de la liste.  
  
        > [!NOTE]  
        >  Les demandes de connexion d’intention de lecture peuvent faire l’objet d’un équilibrage de charge entre les réplicas. Pour plus d’informations, consultez [Configurer l’équilibrage de charge entre des réplicas en lecture seule](../../../database-engine/availability-groups/windows/configure-read-only-routing-for-an-availability-group-sql-server.md#loadbalancing).  
  
     Pour plus d’informations, consultez [Configurer le routage en lecture seule pour un groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-read-only-routing-for-an-availability-group-sql-server.md).  
  
> [!NOTE]  
>  Pour plus d’informations sur les écouteurs de groupe de disponibilité et sur le routage en lecture seule, consultez [Écouteurs de groupe de disponibilité, connectivité client et basculement d’application &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md).  
  
##  <a name="limitations-and-restrictions"></a><a name="bkmk_LimitationsRestrictions"></a> Limitations et restrictions  
 Certaines opérations ne sont pas entièrement prises en charge, comme expliqué ci-après :  
  
-   Dès qu'un réplica accessible en lecture est activé en lecture, il peut commencer à accepter des connexions à ses bases de données secondaires. Toutefois, si des transactions sont actives sur une base de données primaire, les versions de ligne ne sont pas entièrement disponibles sur la base de données secondaire correspondante. Toutes les transactions actives qui existaient sur le réplica principal lorsque le réplica secondaire a été configuré doivent être validées ou restaurées. Tant que ce processus n'est pas terminé, le mappage du niveau d'isolement de la transaction sur la base de données secondaire est incomplet et les requêtes sont temporairement bloquées.  
  
    > [!WARNING]  
    >  Les transactions de longue durée ont un impact sur le nombre de lignes avec version conservées, à la fois pour les tables sur disque et les tables optimisées en mémoire.  
  
-   Dans une base de données secondaire comportant des tables optimisées en mémoire, bien que les versions de ligne soient toujours générées pour les tables optimisées en mémoire, les requêtes sont bloquées jusqu’à la fin de toutes les transactions actives qui existaient dans le réplica principal au moment où le réplica secondaire a été activé pour un accès en lecture. Cela garantit que les tables sur disque et optimisées en mémoire sont disponibles en même temps pour la charge de travail de création de rapports et les requêtes en lecture seule.  
  
-   Le suivi des modifications et la capture de données modifiées ne sont pas pris en charge sur les bases de données secondaires qui appartiennent à un réplica secondaire accessible en lecture :  
  
    -   Le suivi des modifications est explicitement désactivé sur les bases de données secondaires.  
  
    -   La capture des données modifiées ne peut pas être activée uniquement sur une base de données du réplica secondaire. La capture des données modifiées peut être activée sur la base de données du réplica principal et les modifications peuvent être lues à partir des tables de capture des données modifiées en utilisant les fonctions de la base de données du réplica secondaire.  
  
-   Étant donné que les opérations de lecture sont mappées au niveau des transactions d'isolation d'instantané, le nettoyage des enregistrements fantômes sur le réplica principal peut être bloqué par des transactions sur un ou plusieurs réplicas secondaires. La tâche de nettoyage des enregistrements fantômes nettoiera automatiquement les enregistrements fantômes pour les tables sur disque sur le réplica principal lorsqu'aucun réplica secondaire n'en aura plus besoin.  Cela s'apparente aux opérations réalisées lorsque vous exécutez des transactions sur le réplica principal. Dans les cas extrêmes sur la base de données secondaire, vous devrez éventuellement abandonner une longue requête de lecture qui bloque le nettoyage des enregistrements fantômes. Notez que le nettoyage des enregistrements fantômes peut être bloqué si le réplica secondaire est déconnecté ou si le déplacement des données est interrompu sur la base de données secondaire. Cet état empêche également la troncation du journal ; par conséquent, si cet état persiste, nous vous recommandons de supprimer cette base de données secondaire du groupe de disponibilité. Il n'existe aucun problème de nettoyage d'enregistrements fantômes avec les tables optimisées en mémoire, car les versions de ligne sont conservées en mémoire et sont indépendantes des versions de ligne sur le réplica principal.  
  
-   L'opération DBCC SHRINKFILE sur les fichiers contenant des tables sur disque peut échouer sur le réplica principal si le fichier contient des enregistrements fantômes qui sont toujours requis par un réplica secondaire.  
  
-   Depuis [!INCLUDE[ssSQL14](../../../includes/sssql14-md.md)], les réplicas secondaires accessibles en lecture peuvent rester en ligne même quand le réplica principal est hors connexion en raison d’une action utilisateur ou d’un échec, par exemple, quand la synchronisation a été suspendue en raison d’une commande utilisateur ou d’un échec, ou quand un réplica résout l’état parce que le cluster de basculement Windows Server est hors connexion. Toutefois, le routage en lecture seule ne fonctionne pas dans ce cas, car l'écouteur du groupe de disponibilité est également hors connexion. Les clients doivent se connecter directement aux réplicas secondaires en lecture seule pour les charges de travail en lecture seule.  
  
> [!NOTE]  
>  Si vous interrogez la vue de gestion dynamique [sys.dm_db_index_physical_stats](../../../relational-databases/system-dynamic-management-views/sys-dm-db-index-physical-stats-transact-sql.md) sur une instance de serveur qui héberge un réplica secondaire accessible en lecture, vous pouvez rencontrer un problème de blocage REDO. Ce problème est dû au fait que la vue de gestion dynamique acquiert un verrou IS sur la table ou la vue utilisateur spécifiée qui peut bloquer les demandes d'un thread de phase de restauration par progression concernant un verrou X sur la table ou vue utilisateur en question.  
  
##  <a name="performance-considerations"></a><a name="bkmk_Performance"></a> Considérations relatives aux performances  
 Cette section présente plusieurs considérations relatives aux performances des bases de données secondaires accessibles en lecture  
  
 **Dans cette section :**  
  
-   [Latence des données](#DataLatency)  
  
-   [Impact d'une charge de travail en lecture seule](#ReadOnlyWorkloadImpact)  
  
-   [Indexation](#bkmk_Indexing)  
  
-   [Statistiques des bases de données d’accès en lecture seule](#Read-OnlyStats)  
  
###  <a name="data-latency"></a><a name="DataLatency"></a> Latence des données  
 L'implémentation de l'accès en lecture seule aux réplicas secondaires est utile si vos charges de travail en lecture seule peuvent tolérer une certaine latence des données. Dans les situations où la latence des données est inacceptable, envisagez d'exécuter les charges de travail en lecture seule sur le réplica principal.  
  
 Le réplica principal envoie les enregistrements du journal des modifications sur la base de données primaire aux réplicas secondaires. Sur chaque base de données secondaire, un thread de phase de restauration par progression (REDO) dédié applique les enregistrements de journal. Sur une base de données secondaire accessible en lecture, une modification de données n'apparaît pas dans les résultats de la requête tant que l'enregistrement du journal qui contient la modification n'a pas été appliqué à la base de données secondaire et que la transaction n'a pas été validée sur la base de données primaire.  
  
 Cela signifie qu'il y a une certaine latence, en général de quelques secondes, entre les réplicas principal et secondaire. Dans des cas exceptionnels, toutefois, par exemple si des problèmes réseau réduisent le débit, la latence peut devenir importante. La latence augmente en cas de survenue de goulots d'étranglement d'E/S et lorsque le déplacement des données est suspendu. Pour surveiller le déplacement des données suspendu, utilisez le [Tableau de bord Always On](../../../database-engine/availability-groups/windows/use-the-always-on-dashboard-sql-server-management-studio.md) ou la vue de gestion dynamique [sys.dm_hadr_database_replica_states](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-states-transact-sql.md) .  
  
####  <a name="data-latency-on-databases-with-memory-optimized-tables"></a><a name="bkmk_LatencyWithInMemOLTP"></a> Latence des données sur des bases de données avec des tables optimisées en mémoire  
 Dans [!INCLUDE[ssSQL14](../../../includes/sssql14-md.md)], la latence des données sur les secondaires actifs faisait l’objet de considérations spéciales. Consultez [[!INCLUDE[ssSQL14](../../../includes/sssql14-md.md)]Secondaires actifs : réplicas secondaires accessibles en lecture](https://technet.microsoft.com/library/ff878253(v=sql.120).aspx). À partir de [!INCLUDE[ssSQL15](../../../includes/sssql16-md.md)] , la latence des données ne fait plus l’objet de considérations spéciales pour les tables optimisées en mémoire. La latence des données attendue pour les tables optimisées en mémoire est comparable à celle des tables sur disque.  
  
###  <a name="read-only-workload-impact"></a><a name="ReadOnlyWorkloadImpact"></a> Impact d'une charge de travail en lecture seule  
 Lorsque vous configurez un réplica secondaire pour l'accès en lecture seule, vos charges de travail en lecture seule sur les bases de données secondaires consomment des ressources système, telles que le processeur et les E/S (pour les tables sur disque) des threads de phase de restauration par progression, surtout si les charges de travail en lecture seule sur les tables sur disque nécessitent de nombreuses E/S. Aucun impact n'est à constater sur les E/S lors de l'accès aux tables optimisées en mémoire, car toutes les lignes résident en mémoire.  
  
 En outre, les charges de travail en lecture seule sur les réplicas secondaires peuvent bloquer les modifications du langage de définition de données (DDL) qui sont appliquées par les enregistrements du journal.  
  
-   Même si les opérations de lecture ne prennent pas de verrous partagés en raison du contrôle de version de ligne, ces opérations acceptent les verrous de stabilité de schéma (Sch-S), qui peuvent bloquer les opérations de restauration qui appliquent des modifications DDL. Les opérations DDL incluent les vues et tables ALTER/DROP, mais pas DROP ou ALTER des procédures stockées. Par exemple, si vous supprimez une table sur disque ou optimisée en mémoire sur le réplica principal. Lorsque le thread REDO traite l'enregistrement de journal pour supprimer la table, il doit acquérir un verrou SCH_M sur la table et peut être bloqué par une requête en cours d'exécution qui accède à la table.  Ce comportement est le même sur le réplica principal, sauf que la suppression de la table s'effectue dans le cadre d'une session utilisateur et non d'un thread REDO.  
  
-   D'autres formes de blocage peuvent avoir lieu avec les tables optimisées en mémoire. La suppression d'une procédure stockée native peut provoquer le blocage du thread REDO s'il existe une exécution simultanée de la procédure stockée native sur le réplica secondaire. Ce comportement est le même sur le réplica principal, sauf que la suppression de la procédure stockée s'effectue dans le cadre d'une session utilisateur et non d'un thread REDO.  
  
 Tenez compte des meilleures pratiques pour créer des requêtes, et utilisez ces meilleures pratiques dans les bases de données secondaires. Par exemple, planifiez les requêtes de longue durée, telles que les agrégations de données, pendant les périodes de faible activité.  
  
> [!NOTE]  
>  Si un thread de restauration par progression est bloqué par des requêtes sur un réplica secondaire, le XEvent **sqlserver.lock_redo_blocked** est déclenché.  
  
###  <a name="indexing"></a><a name="bkmk_Indexing"></a> Indexation  
 Pour optimiser les charges de travail en lecture seule sur les réplicas secondaires accessibles en lecture, vous pouvez créer des index sur les tables des bases de données secondaires. Étant donné que vous ne pouvez pas modifier le schéma ou les données des bases de données secondaires, créez des index dans les bases de données primaires et autorisez le transfert des modifications sur la base de données secondaire par le biais du processus de restauration par progression.  
  
 Pour surveiller l’utilisation des index sur un réplica secondaire, interrogez les colonnes **user_seeks**, **user_scans** et **user_lookups** de la vue de gestion dynamique [sys.dm_db_index_usage_stats](../../../relational-databases/system-dynamic-management-views/sys-dm-db-index-usage-stats-transact-sql.md) .  
  
###  <a name="statistics-for-read-only-access-databases"></a><a name="Read-OnlyStats"></a> Statistiques des bases de données d’accès en lecture seule  
 Les statistiques sur les colonnes des tables et des vues indexées permettent d'optimiser les plans de requête. Pour les groupes de disponibilité, des statistiques créées et conservées sur les bases de données primaires sont automatiquement rendues persistantes sur les bases de données secondaires dans le cadre de l'application des enregistrements du journal des transactions. Toutefois, la charge de travail en lecture seule sur les bases de données secondaires peut avoir besoin de statistiques différentes de celles créées sur les bases de données primaires. En outre, étant donné que les bases de données secondaires sont limitées à l'accès en lecture seule, les statistiques ne peuvent pas être créées sur les bases de données secondaires.  
  
 Pour résoudre ce problème, le réplica secondaire crée et conserve les statistiques temporaires pour les bases de données secondaires dans **tempdb**. Le suffixe _readonly_database_statistic est ajouté au nom des statistiques temporaires pour les différencier des statistiques permanentes qui sont conservées à partir de la base de données primaire.  
  
 Seul [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] peut créer et mettre à jour les statistiques temporaires. Toutefois, vous pouvez supprimer des statistiques temporaires et analyser leurs propriétés en utilisant les mêmes outils que ceux que vous utilisez pour les statistiques permanentes :  
  
-   Supprimez les statistiques temporaires en utilisant l’instruction [DROP STATISTICS](../../../t-sql/statements/drop-statistics-transact-sql.md) [!INCLUDE[tsql](../../../includes/tsql-md.md)].  
  
-   Analysez les statistiques à l’aide des affichages catalogue **sys.stats** et **sys.stats_columns** . L’affichage **sys_stats** inclut la colonne **is_temporary** pour distinguer les statistiques permanentes des statistiques temporaires.  
  
 Il n'existe aucune prise en charge de la mise à jour automatique des statistiques pour les tables optimisées en mémoire sur le réplica principal ou secondaire. Vous devez surveiller les performances des requêtes et les plans sur le réplica secondaire et mettre manuellement à jour les statistiques sur le réplica principal quand cela est nécessaire. Toutefois, les statistiques manquantes sont créées automatiquement sur le réplica principal et le réplica secondaire.  
  
 Pour plus d’informations sur les statistiques SQL Server, consultez [Statistiques](../../../relational-databases/statistics/statistics.md).  
  
 **Dans cette section :**  
  
-   [Statistiques permanentes obsolètes sur les bases de données secondaires](#StalePermStats)  
  
-   [Limitations et restrictions](#StatsLimitationsRestrictions)  
  
####  <a name="stale-permanent-statistics-on-secondary-databases"></a><a name="StalePermStats"></a> Statistiques permanentes obsolètes sur les bases de données secondaires  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] détecte si les statistiques permanentes sur une base de données secondaire sont obsolètes. Or, il n'est pas possible d'apporter des modifications aux statistiques permanentes, sauf en modifiant la base de données primaire. Pour l'optimisation des requêtes, [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] crée des statistiques temporaires pour les tables sur disque sur la base de données secondaire et les utilise en remplacement des statistiques permanentes obsolètes.  
  
 Lorsque les statistiques permanentes sont mises à jour sur la base de données primaire, elles sont automatiquement conservées dans la base de données secondaire. Ensuite, [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] utilise les statistiques permanentes mises à jour, qui sont plus récentes que les statistiques temporaires.  
  
 Si le groupe de disponibilité bascule, les statistiques temporaires sont supprimées sur tous les réplicas secondaires.  
  
####  <a name="limitations-and-restrictions"></a><a name="StatsLimitationsRestrictions"></a> Limitations et restrictions  
  
-   Étant donné que les statistiques temporaires sont stockées dans **tempdb**, un redémarrage du service [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] provoque la disparition de toutes les statistiques temporaires.  
  
-   Le suffixe _readonly_database_statistic est réservé aux statistiques générées par [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Vous ne pouvez pas utiliser ce suffixe lorsque vous créez des statistiques sur une base de données primaire. Pour plus d’informations, consultez [Statistiques](../../../relational-databases/statistics/statistics.md).  
  
##  <a name="accessing-memory-optimized-tables-on-a-secondary-replica"></a><a name="bkmk_AccessInMemTables"></a> Accès aux tables optimisées en mémoire sur un réplica secondaire  
 Les niveaux d’isolation de la transaction qui peuvent être utilisés avec des tables optimisées en mémoire sur un réplica secondaire sont les mêmes que sur le réplica principal. Il est recommandé de définir le niveau d’isolation au niveau de la session sur READ COMMITTED et de définir l’option au niveau de la base de données MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT sur ON. Par exemple :  
  
```sql  
ALTER DATABASE CURRENT SET MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT=ON  
GO  
SET TRANSACTION ISOLATION LEVEL READ COMMITTED  
GO  
SELECT SUM(UnitPrice*OrderQty)   
FROM Sales.SalesOrderDetail_inmem  
GO  
  
```  
  
##  <a name="capacity-planning-considerations"></a><a name="bkmk_CapacityPlanning"></a> Considérations relatives à la planification des capacités  
  
-   Dans le cas de tables sur disque, les réplicas secondaires accessibles en lecture peuvent nécessiter de l’espace dans **tempdb** pour deux raisons :  
  
    -   Le niveau d'isolation d'instantané copie les versions de ligne dans **tempdb**.  
  
    -   Les statistiques temporaires des bases de données secondaires sont créées et conservées dans **tempdb**. Les statistiques temporaires peuvent provoquer une légère augmentation de la taille de **tempdb**. Pour plus d’informations, consultez [Statistiques des bases de données d’accès en lecture seule](#Read-OnlyStats), plus loin dans cette section.  
  
-   Lorsque vous configurez l'accès en lecture pour un ou plusieurs réplicas secondaires, les bases de données primaires ajoutent 14 octets de surcharge sur les lignes de données supprimées, modifiées ou insérées pour stocker les pointeurs vers les versions de ligne sur les bases de données secondaires pour les tables sur disque. Cette surcharge de 14 octets est reportée aux bases de données secondaires. La surcharge de 14 octets étant ajoutée aux lignes de données, des fractionnements de page peuvent se produire.  
  
     Les données de version de ligne ne sont pas générées par les bases de données primaires. Au lieu de cela, les bases de données secondaires génèrent les versions de ligne. Toutefois, le contrôle de version de ligne augmente le stockage des données dans les bases de données primaires et secondaires.  
  
     L'ajout de données de version de ligne dépend du niveau d'isolation d'instantané ou d'isolation d'instantané Read Committed défini sur la base de données primaire. Le tableau ci-dessous décrit le comportement du contrôle de version sur une base de données secondaire accessible en lecture avec différents paramètres pour les tables sur disque.  
  
    |Accès en lecture au réplica secondaire ?|Isolation d'instantané ou isolation d'instantané Read Committed ?|Base de données primaire|Base de données secondaire|  
    |---------------------------------|-----------------------------------------------|----------------------|------------------------|  
    |Non|Non|Aucune version de ligne ni surcharge de 14 octets|Aucune version de ligne ni surcharge de 14 octets|  
    |Non|Oui|Versions de ligne et surcharge de 14 octets|Aucune version de ligne, mais surcharge de 14 octets|  
    |Oui|Non|Aucune version de ligne, mais surcharge de 14 octets|Versions de ligne et surcharge de 14 octets|  
    |Oui|Oui|Versions de ligne et surcharge de 14 octets|Versions de ligne et surcharge de 14 octets|  
  
##  <a name="related-tasks"></a><a name="bkmk_RelatedTasks"></a> Tâches associées  
  
-   [Configurer l’accès en lecture seule sur un réplica de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-read-only-access-on-an-availability-replica-sql-server.md)  
  
-   [Configurer le routage en lecture seule pour un groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-read-only-routing-for-an-availability-group-sql-server.md)  
  
-   [Créer ou configurer un écouteur de groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md)  
  
-   [Surveiller des groupes de disponibilité &#40;Transact-SQL&#41;](../../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)  
  
-   [Afficher les propriétés d’un réplica de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/view-availability-replica-properties-sql-server.md)  
  
-   [Utiliser la boîte de dialogue Nouveau groupe de disponibilité &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-new-availability-group-dialog-box-sql-server-management-studio.md)  
  
##  <a name="related-content"></a><a name="RelatedContent"></a> Contenu associé  
  
-   [Blog de l’équipe SQL Server Always On : Blog officiel de l’équipe SQL Server Always On](/archive/blogs/sqlalwayson/)  
  
## <a name="see-also"></a>Voir aussi  
 [Vue d’ensemble des groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [À propos de l’accès de la connexion client aux réplicas de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/about-client-connection-access-to-availability-replicas-sql-server.md)   
 [Écouteurs de groupe de disponibilité, connectivité client et basculement d’application &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md)   
 [Statistiques](../../../relational-databases/statistics/statistics.md)  
  
