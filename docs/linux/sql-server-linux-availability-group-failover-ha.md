---
title: Gérer le basculement du groupe de disponibilité - SQL Server sur Linux
description: 'Cet article décrit trois types de basculement : le basculement automatique, le basculement manuel programmé et le basculement manuel forcé. Avec les types automatique et manuel programmé, vos données sont conservées.'
author: tejasaks
ms.author: tejasaks
ms.reviewer: vanto
ms.date: 03/01/2018
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: ''
ms.openlocfilehash: 52fbeee33dd992f4916f33a1545b59265a8b47f9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345654"
---
# <a name="always-on-availability-group-failover-on-linux"></a>Basculement du groupe de disponibilité Always On sur Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Dans le contexte d'un groupe de disponibilité, le rôle principal et le rôle secondaire des réplicas de disponibilité sont généralement interchangeables dans un processus appelé basculement. Trois formes de basculement existent : basculement automatique (sans perte de données), basculement manuel planifié (sans perte de données) et basculement manuel forcé (avec perte de données possible), ce dernier étant généralement appelé *basculement forcé*. Les basculements automatiques et manuels programmés préservent toutes vos données. Un groupe de disponibilité bascule au niveau d'un réplica de disponibilité. Autrement dit, un groupe de disponibilité bascule vers l’un de ses réplicas secondaires (cible de basculement actuelle). 

Pour obtenir des informations en arrière-plan sur le basculement, consultez [Basculement et modes de basculement](../database-engine/availability-groups/windows/failover-and-failover-modes-always-on-availability-groups.md).

## <a name="manual-failover"></a><a name="failover"></a>Basculement manuel

Utilisez les outils de gestion de cluster pour basculer un groupe de disponibilité managé par un gestionnaire de cluster externe. Par exemple, si une solution utilise Pacemaker pour gérer un cluster Linux, utilisez `pcs` pour effectuer des basculements manuels sur RHEL ou Ubuntu. Sur SLES, utilisez `crm`. 

> [!IMPORTANT]
> Dans des opérations normales, ne basculez pas avec les outils d’administration Transact-SQL ou SQL Server tels que SSMS ou PowerShell. Lorsque `CLUSTER_TYPE = EXTERNAL`, la seule valeur acceptable pour `FAILOVER_MODE` est `EXTERNAL`. Avec ces paramètres, toutes les actions de basculement manuelles ou automatiques sont exécutées par le gestionnaire de cluster externe. Pour obtenir des instructions et forcer le basculement avec perte de données potentielle, consultez [Forcer le basculement](#forceFailover).

### <a name="a-namemanualfailovermanual-failover-steps"></a><a name="manualFailover">Étape du basculement manuel

Pour effectuer un basculement, le réplica secondaire qui deviendra le réplica principal doit être synchrone. Si un réplica secondaire est asynchrone, [modifiez le mode de disponibilité](../database-engine/availability-groups/windows/change-the-availability-mode-of-an-availability-replica-sql-server.md).

Basculement manuel en deux étapes.

   Tout d'abord, [basculez manuellement en déplaçant la ressource du groupe de disponibilité](#manualMove) du nœud de cluster qui possède les ressources vers un nouveau nœud.

   Le cluster bascule la ressource du groupe de disponibilité et ajoute une contrainte d’emplacement. Cette contrainte configure la ressource pour qu’elle s’exécute sur le nouveau nœud. Supprimez cette contrainte pour pouvoir réussir le basculement à l’avenir.

   Ensuite, [supprimez la contrainte d’emplacement](#removeLocConstraint).

#### <a name="step-1-manually-fail-over-by-moving-availability-group-resource"></a><a name="manualMove"></a> Étape 1. Basculer manuellement en déplaçant la ressource du groupe de disponibilité

Pour basculer manuellement une ressource du groupe de disponibilité nommée *ag_cluster* vers le nœud de cluster nommé *nodeName2*, exécutez la commande appropriée pour votre distribution :

- **Exemple RHEL/Ubuntu**

   ```bash
   sudo pcs resource move ag_cluster-master nodeName2 --master
   ```

- **Exemple SLES**

   ```bash
   crm resource migrate ag_cluster nodeName2
   ```

>[!IMPORTANT]
>Quand vous faites basculer manuellement une ressource, vous devez supprimer une contrainte d’emplacement qui est automatiquement ajoutée.

#### <a name="step-2-remove-the-location-constraint"></a><a name="removeLocConstraint"> </a> Étape 2. Supprimer la contrainte d’emplacement

À l’occasion d’un déplacement manuel, la commande `pcs``move` ou la commande `crm``migrate` ajoute une contrainte d’emplacement pour que la ressource prenne place sur le nouveau nœud cible. Pour afficher la nouvelle contrainte, exécutez la commande suivante après avoir déplacé manuellement la ressource :

- **Exemple RHEL/Ubuntu**

   ```bash
   sudo pcs constraint list --full
   ```

- **Exemple SLES**

   ```bash
   crm config show
   ```

Exemple de contrainte créée en raison d’un basculement manuel. 
 `Enabled on: Node1 (score:INFINITY) (role: Master) (id:cli-prefer-ag_cluster-master)`

   > [!NOTE]
   > Le nom de la ressource du groupe de disponibilité dans les clusters Pacemaker sur Red Hat Enterprise Linux 8.x et Ubuntu 18.04 peut ressembler à *ag_cluster-clone*, car la nomenclature des ressources a été modifiée pour utiliser un *clone pouvant être promu*. 

- **Exemple RHEL/Ubuntu**

   Dans la commande `cli-prefer-ag_cluster-master` ci-dessous figure l’ID de la contrainte à supprimer. `sudo pcs constraint list --full` retourne cet ID. 
   
   ```bash
   sudo pcs resource clear ag_cluster-master  
   ```
   ou
   
   ```bash
   sudo pcs constraint remove cli-prefer-ag_cluster-master  
   ```
  
   Vous pouvez également déplacer et effacer des contraintes générées automatiquement sur une seule ligne, comme suit. L’exemple suivant utilise la terminologie de *clone*, conformément à Red Hat Enterprise Linux 8.x. 
  
   ```bash
   sudo pcs resource move ag_cluster-clone --master nodeName2 && sleep 30 && sudo pcs resource clear ag_cluster-clone

   ```
  
- **Exemple SLES**

   Dans la commande suivante `cli-prefer-ms-ag_cluster` figure l’ID de la contrainte. `crm config show` retourne cet ID. 
   
   ```bash
   crm configure
   delete cli-prefer-ms-ag_cluster 
   commit
   ```

>[!NOTE]
>Comme l’opération de basculement automatique n’ajoute pas de contrainte d’emplacement, aucun nettoyage n’est nécessaire. 

Pour plus d'informations :
- [Red Hat - Managing Cluster Resources](https://access.redhat.com/documentation/Red_Hat_Enterprise_Linux/6/html/Configuring_the_Red_Hat_High_Availability_Add-On_with_Pacemaker/ch-manageresource-HAAR.html) (Red Hat - Gestion des ressources de cluster)
- [Pacemaker - Déplacer des ressources manuellement](https://clusterlabs.org/pacemaker/doc/en-US/Pacemaker/1.1/html/Clusters_from_Scratch/_move_resources_manually.html)
 [Guide d’administration SLES - Ressources](https://www.suse.com/documentation/sle-ha-12/singlehtml/book_sleha/book_sleha.html#sec.ha.troubleshooting.resource) 
 
## <a name="force-failover"></a><a name="forceFailover"></a> Forcer le basculement 

Un basculement forcé est strictement destiné à la récupération d’urgence. Dans ce cas, vous ne pouvez pas effectuer le basculement avec les outils de gestion du cluster, car le centre de données principal est en panne. Si vous forcez le basculement vers un réplica secondaire qui n'est pas synchronisé, une perte de données est possible. Ne forcez le basculement uniquement si vous devez restaurer immédiatement le service sur le groupe de disponibilité et que vous êtes prêt à courir le risque de perdre des données.

Si vous ne pouvez pas utiliser les outils de gestion de clusters pour interagir avec le cluster, par exemple, si le cluster ne répond pas en raison d’un événement d’incident dans le centre de données principal, vous devrez peut-être forcer le basculement pour contourner le gestionnaire de cluster externe. Cette procédure n’est pas recommandée pour les opérations normales, car elle risque d’entraîner la perte de données. Utilisez-la lorsque les outils d’administration de clusters ne parviennent pas à exécuter l’action de basculement. Fonctionnellement, cette procédure est similaire à [l’exécution d’un basculement manuel forcé](../database-engine/availability-groups/windows/perform-a-forced-manual-failover-of-an-availability-group-sql-server.md) sur un groupe de disponibilité dans Windows.
 
Ce processus de forçage du basculement est spécifique à SQL Server sur Linux.

1. Vérifiez que la ressource du groupe de disponibilité n’est plus managée par le cluster. 

      - Définissez la ressource en mode non géré sur le nœud de cluster cible. Cette commande indique à l’agent de ressources d’arrêter la surveillance et la gestion des ressources. Par exemple : 
      
      ```bash
      sudo pcs resource unmanage <resourceName>
      ```

      - Si la tentative de définition du mode de ressource en mode non géré échoue, supprimez la ressource. Par exemple :

      ```bash
      sudo pcs resource delete <resourceName>
      ```

      >[!NOTE]
      >Lorsque vous supprimez une ressource, cela supprime également toutes les contraintes associées. 

1. Sur l’instance de SQL Server qui héberge le réplica secondaire, définissez la variable du contexte de la session `external_cluster`.

   ```Transact-SQL
   EXEC sp_set_session_context @key = N'external_cluster', @value = N'yes';
   ```

1. Basculez le groupe de disponibilité avec Transact-SQL. Dans l’exemple suivant, remplacez `<MyAg>` par le nom de votre groupe de disponibilité. Connectez-vous à l’instance de SQL Server qui héberge le réplica secondaire cible et exécutez la commande suivante :

   ```Transact-SQL
   ALTER AVAILABILITY GROUP <MyAg> FORCE_FAILOVER_ALLOW_DATA_LOSS;
   ```

1.  Après un basculement forcé, mettez le groupe de disponibilité à un état d’intégrité avant de redémarrer l’analyse et la gestion des ressources de cluster ou de recréer la ressource du groupe de disponibilité. Consultez les [Tâches essentielles après un basculement forcé](../database-engine/availability-groups/windows/perform-a-forced-manual-failover-of-an-availability-group-sql-server.md#FollowUp).

1.  Redémarrez l’analyse et la gestion des ressources de cluster :

   Pour redémarrer l’analyse et la gestion des ressources de cluster, exécutez la commande suivante :

   ```bash
   sudo pcs resource manage <resourceName>
   sudo pcs resource cleanup <resourceName>
   ```

   Si vous avez supprimé la ressource de cluster, recréez-la. Pour recréer la ressource de cluster, suivez les instructions de la rubrique [Créer une ressource de groupe de disponibilité](sql-server-linux-availability-group-cluster-rhel.md#create-availability-group-resource).

>[!Important]
>N’utilisez pas les étapes précédentes pour les explorations de récupération d’urgence, car elles risquent d’entraîner la perte de données. Au lieu de cela, remplacez le réplica asynchrone par synchrone et modifiez les instructions pour le [basculement manuel normal](#manualFailover).

## <a name="database-level-monitoring-and-failover-trigger"></a>Déclencheur de surveillance et de basculement au niveau de la base de données

Pour `CLUSTER_TYPE=EXTERNAL`, la sémantique du déclencheur de basculement est différente de celle de WSFC. Lorsque le groupe de disponibilité est sur une instance de SQL Server dans un WSFC, la transition hors de l’état `ONLINE` pour la base de données entraîne le signalement d’une erreur par l’intégrité du groupe de disponibilité. En réponse, le gestionnaire de cluster déclenche une action de basculement. Sur Linux, l’instance de SQL Server ne peut pas communiquer avec le cluster. La surveillance de l’intégrité de la base de données est effectuée *en interaction indirecte*. Si l’utilisateur a opté pour la surveillance et le basculement du basculement au niveau de la base de données (en définissant l’option `DB_FAILOVER=ON` lors de la création du groupe de disponibilité), le cluster vérifie si l’état de la base de données est `ONLINE` à chaque fois qu’il exécute une action de surveillance. Le cluster interroge l’état dans `sys.databases`. Pour tout état différent de `ONLINE`, il déclenche un basculement automatique (si les conditions de basculement automatique sont remplies). L’heure réelle du basculement dépend de la fréquence de l’action d’analyse, ainsi que de l’état de la base de données en cours de mise à jour dans sys.databases.

Le basculement automatique nécessite au moins un réplica synchrone.

## <a name="next-steps"></a>Étapes suivantes

[Configurer le cluster Red Hat Enterprise Linux pour les ressources de cluster du groupe de disponibilité SQL Server](sql-server-linux-availability-group-cluster-rhel.md)

[Configurer le cluster SUSE Linux Enterprise Server pour les ressources de cluster du groupe de disponibilité SQL Server](sql-server-linux-availability-group-cluster-sles.md)

[Configurer le cluster Ubuntu pour les ressources de cluster du groupe de disponibilité SQL Server](sql-server-linux-availability-group-cluster-ubuntu.md)
