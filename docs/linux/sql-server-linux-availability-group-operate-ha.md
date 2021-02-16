---
title: Utiliser un groupe de disponibilité SQL Server sur Linux
description: Cet article décrit comment effectuer une mise à niveau propagée avec des instances SQL Server sur Linux qui utilisent des groupes de disponibilité. Avant de commencer la mise à niveau, prenez connaissance des bonnes pratiques à suivre.
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 03/01/2018
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: ''
ms.openlocfilehash: 00e576b6fd2fc92cca591a5ffa3d6e97556bc1ad
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352558"
---
# <a name="operate-always-on-availability-groups-on-linux"></a>Utiliser des groupes de disponibilité Always On sur Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

## <a name="upgrade-availability-group"></a>Mettre à niveau un groupe de disponibilité

Avant de mettre à niveau un groupe de disponibilité, passez en revue les modèles et les pratiques de la section [Mise à niveau d’instances de réplica d’un groupe de disponibilité](../database-engine/availability-groups/windows/upgrading-always-on-availability-group-replica-instances.md).

Les sections suivantes expliquent comment effectuer une mise à niveau continue avec des instances SQL Server sur Linux avec des groupes de disponibilité. 

### <a name="upgrade-steps-on-linux"></a>Étapes de la mise à niveau sur Linux

Lorsque des réplicas de groupes de disponibilité sont présents sur des instances SQL Server sur Linux, le type de cluster du groupe de disponibilité est `EXTERNAL` ou `NONE`. Un groupe de disponibilité géré par un gestionnaire de cluster en plus de Windows Server Failover Cluster (WSFC) est `EXTERNAL`. Pacemaker avec Corosync est un exemple de gestionnaire de cluster externe. Un groupe de disponibilité sans gestionnaire de cluster possède le type de cluster `NONE` Les étapes de mise à niveau décrites ici sont spécifiques aux groupes de disponibilité dont le type de cluster est `EXTERNAL` ou `NONE`.

L'ordre dans lequel vous mettez à niveau les instances varie selon que leur rôle est secondaire et si elles hébergent ou non des réplicas synchrones ou asynchrones. Mettez à niveau les instances SQL Server qui hébergent d'abord des réplicas secondaires asynchrones. Puis mettez à niveau les instances qui hébergent des réplicas secondaires synchrones. 

   >[!NOTE]
   >Si un groupe de disponibilité ne possède que des réplicas asynchrones, pour éviter toute perte de données, convertissez un réplica en réplica synchrone puis attendez sa synchronisation. Mettez à niveau ensuite ce réplica.
   
Avant de commencer, sauvegardez chaque base de données.

1. Arrêtez la ressource sur le nœud hébergeant le réplica secondaire à mettre à niveau.
   
   Avant d'exécuter la commande de mise à niveau, arrêtez la ressource pour que le cluster ne la surveille pas et la fasse échouer inutilement. L'exemple suivant ajoute une contrainte d'emplacement sur le nœud qui entraînera l’arrêt de la ressource. Mettez à jour `ag_cluster-master` avec le nom de la ressource et `nodeName1` avec le nœud hébergeant le réplica à mettre à niveau.

   ```bash
   pcs constraint location ag_cluster-master avoids nodeName1
   ```

1. Mettez à niveau SQL Server sur le réplica secondaire.

   L'exemple suivant met à jour les packages `mssql-server` et `mssql-server-ha`.

   ```bash
   sudo yum update mssql-server
   sudo yum update mssql-server-ha
   ```
1. Supprimez la contrainte d’emplacement.

   Avant d'exécuter la commande de mise à niveau, arrêtez la ressource pour que le cluster ne la surveille pas et la fasse échouer inutilement. L'exemple suivant ajoute une contrainte d'emplacement sur le nœud qui entraînera l’arrêt de la ressource. Mettez à jour `ag_cluster-master` avec le nom de la ressource et `nodeName1` avec le nœud hébergeant le réplica à mettre à niveau.

   ```bash
   pcs constraint remove location-ag_cluster-master-rhel1--INFINITY
   ```
   Comme bonne pratique, assurez-vous que la ressource est démarrée (en utilisant la commande `pcs status`) et que le réplica secondaire est connecté et synchronisé après la mise à niveau.

1. Une fois tous les réplicas secondaires mis à niveau, basculez manuellement vers l'un des réplicas secondaires synchrones.

   Pour les groupes de disponibilité avec le type de cluster `EXTERNAL`, effectuez le basculement avec les outils de gestion de cluster ; les groupes de disponibilité avec le type de cluster `NONE` doivent utiliser Transact-SQL pour le basculement. 
   L'exemple suivant permet de basculer vers un groupe de disponibilité avec les outils de gestion de cluster. Remplacez `<targetReplicaName>` par le nom du réplica secondaire qui deviendra le réplica principal :

   ```bash
   sudo pcs resource move ag_cluster-master <targetReplicaName> --master  
   ``` 
   
   >[!IMPORTANT]
   >Les étapes suivantes s'appliquent uniquement aux groupes de disponibilités qui n'ont pas de gestionnaire de cluster.

   Si le type de cluster du groupe de disponibilité est `NONE`, effectuez un basculement manuel. Exécutez les étapes suivantes dans l'ordre :

      a. La commande suivante convertit un réplica principal en réplica secondaire. Remplacez `AG1` par le nom de votre groupe de disponibilité. Exécutez la commande Transact-SQL sur l'instance SQL Server qui héberge le réplica principal.

      ```transact-sql
      ALTER AVAILABILITY GROUP [ag1] SET (ROLE = SECONDARY);
      ```

      b. La commande suivante convertit un réplica secondaire synchrone en réplica principal. Exécutez la commande Transact-SQL suivante sur l'instance SQL Server cible (l'instance qui héberge le réplica secondaire synchrone).

      ```transact-sql
      ALTER AVAILABILITY GROUP [ag1] FAILOVER;
      ```

1. Après le basculement, mettez à niveau SQL Server sur l'ancien réplica principal en répétant la procédure précédente.

   L'exemple suivant met à jour les packages `mssql-server` et `mssql-server-ha`.

   ```bash
   # add constraint for the resource to stop on the upgraded node
   # replace 'nodename2' with the name of the cluster node targeted for upgrade
   pcs constraint location ag_cluster-master avoids nodeName2
   sudo yum update mssql-server
   sudo yum update mssql-server-ha
   ```
   
   ```bash
   # upgrade mssql-server and mssql-server-ha packages
   sudo yum update mssql-server
   sudo yum update mssql-server-ha
   ```

   ```bash
   # remove the constraint; make sure the resource is started and replica is connected and synchronized
   pcs constraint remove location-ag_cluster-master-rhel1--INFINITY
   ```

1. Pour des groupes de disponibilités avec un gestionnaire de cluster externe, où le type de cluster est EXTERNE, supprimez la contrainte d’emplacement générée par le basculement manuel. 

   ```bash
   sudo pcs constraint remove cli-prefer-ag_cluster-master  
   ```

1. Reprenez le déplacement des données pour le réplica secondaire qui vient d’être mis à jour (l'ancien réplica principal). Cette étape est nécessaire lorsqu'une instance SQL Server de version supérieure transfère des blocs de journal vers une instance de version inférieure au sein d’un groupe de disponibilité. Exécutez la commande suivante sur le nouveau réplica secondaire (l'ancien réplica principal).

   ```transact-sql
   ALTER DATABASE database_name SET HADR RESUME;
   ```

Après avoir mis à niveau tous les serveurs, vous pouvez effectuer la restauration. Effectuez une restauration vers le réplica principal d’origine, si nécessaire. 

## <a name="drop-an-availability-group"></a>Supprimer un groupe de disponibilité

Pour supprimer un groupe de disponibilité, exécutez [DROP AVAILABILITY GROUP](../t-sql/statements/drop-availability-group-transact-sql.md). Si le type de cluster est `EXTERNAL` ou `NONE`, exécutez la commande sur chaque instance SQL Server qui héberge un réplica. Par exemple, pour supprimer un groupe de disponibilité nommé `group_name`, exécutez la commande suivante :

   ```transact-sql
   DROP AVAILABILITY GROUP group_name
   ```
 

## <a name="next-steps"></a>Étapes suivantes

[Configurer le cluster Red Hat Enterprise Linux pour les ressources de cluster du groupe de disponibilité SQL Server](sql-server-linux-availability-group-cluster-rhel.md)

[Configurer le cluster SUSE Linux Enterprise Server pour les ressources de cluster du groupe de disponibilité SQL Server](sql-server-linux-availability-group-cluster-sles.md)

[Configurer le cluster Ubuntu pour les ressources de cluster du groupe de disponibilité SQL Server](sql-server-linux-availability-group-cluster-ubuntu.md)
