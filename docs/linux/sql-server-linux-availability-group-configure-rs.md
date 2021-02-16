---
title: Configurer un groupe de disponibilité d’échelle lecture (SQL Server sur Linux)
description: Découvrez comment configurer un groupe de disponibilité Always On SQL Server pour les charges de travail avec échelle lecture sur Linux.
ms.custom: seo-lt-2019
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 01/09/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.openlocfilehash: 5cad1c63817bf55231c065123b2456590dbfebb2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352743"
---
# <a name="configure-a-sql-server-availability-group-for-read-scale-on-linux"></a>Configurer un groupe de disponibilité SQL Server pour l’échelle lecture sur Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Vous pouvez configurer un groupe de disponibilité Always On SQL Server pour les charges de travail avec échelle lecture sur Linux. L’architecture des groupes de disponibilité se présente sous deux formes. Une architecture visant la haute disponibilité utilise un gestionnaire de clusters pour améliorer la continuité d’activité. Cette architecture peut également inclure des réplicas de mise à l’échelle en lecture. Pour créer l’architecture à haute disponibilité, consultez [Configurer le groupe de disponibilité Always On SQL Server pour la haute disponibilité sur Linux](sql-server-linux-availability-group-configure-ha.md). L’autre architecture prend uniquement en charge les charges de travail avec échelle lecture. Cet article explique comment créer un groupe de disponibilité sans gestionnaire de cluster pour les charges de travail avec échelle lecture. Cette architecture fournit uniquement une échelle lecture. Elle n’assure pas la haute disponibilité.

> [!NOTE]
> Un groupe de disponibilité avec `CLUSTER_TYPE = NONE` peut inclure des réplicas hébergés sur des plateformes de système d’exploitation différentes. Il ne peut pas prendre en charge la haute disponibilité. 

[!INCLUDE [Create prerequisites](../includes/ss-linux-cluster-availability-group-create-prereq.md)]

## <a name="create-the-ag"></a>Créer le groupe de disponibilité

Créez le groupe de disponibilité. Définissez `CLUSTER_TYPE = NONE`. Pour chaque réplica, définissez également `FAILOVER_MODE = MANUAL`. Les applications clientes qui exécutent des charges de travail analytiques ou de création de rapports peuvent se connecter directement aux bases de données secondaires. Vous pouvez également créer une liste de routage en lecture seule. Les connexions au réplica principal transfèrent les demandes de connexion à chacun des réplicas secondaires de la liste de routage en mode tourniquet (round-robin).

Le script Transact-SQL suivant crée un groupe de disponibilité nommé `ag1`. Le script configure les réplicas de groupe de disponibilité avec `SEEDING_MODE = AUTOMATIC`. Ce paramètre permet à SQL Server de créer automatiquement la base de données sur chaque serveur secondaire après son ajout au groupe de disponibilité. Mettez à jour le script suivant en fonction de votre environnement. Remplacez les valeurs `<node1>` et `<node2>` par les noms des instances de SQL Server qui hébergent les réplicas. Remplacez la valeur `<5022>` par le port que vous définissez pour le point de terminaison. Exécutez le script Transact-SQL suivant sur le réplica SQL Server principal :

```SQL
CREATE AVAILABILITY GROUP [ag1]
    WITH (CLUSTER_TYPE = NONE)
    FOR REPLICA ON
        N'<node1>' WITH (
            ENDPOINT_URL = N'tcp://<node1>:<5022>',
            AVAILABILITY_MODE = ASYNCHRONOUS_COMMIT,
            FAILOVER_MODE = MANUAL,
            SEEDING_MODE = AUTOMATIC,
                    SECONDARY_ROLE (ALLOW_CONNECTIONS = ALL)
            ),
        N'<node2>' WITH ( 
            ENDPOINT_URL = N'tcp://<node2>:<5022>', 
            AVAILABILITY_MODE = ASYNCHRONOUS_COMMIT,
            FAILOVER_MODE = MANUAL,
            SEEDING_MODE = AUTOMATIC,
            SECONDARY_ROLE (ALLOW_CONNECTIONS = ALL)
            );
        
ALTER AVAILABILITY GROUP [ag1] GRANT CREATE ANY DATABASE;
```

### <a name="join-secondary-sql-servers-to-the-ag"></a>Joindre des serveurs SQL Server secondaires au groupe de disponibilité

Le script Transact-SQL suivant joint un serveur à un groupe de disponibilité nommé `ag1`. Mettez à jour le script en fonction de votre environnement. Sur chaque réplica SQL Server secondaire, exécutez le script Transact-SQL suivant pour joindre le groupe de disponibilité :

```SQL
ALTER AVAILABILITY GROUP [ag1] JOIN WITH (CLUSTER_TYPE = NONE);
         
ALTER AVAILABILITY GROUP [ag1] GRANT CREATE ANY DATABASE;
```

[!INCLUDE [Create post](../includes/ss-linux-cluster-availability-group-create-post.md)]

Ce groupe de disponibilité n’est pas une configuration à haute disponibilité. Si vous avez besoin de la haute disponibilité, suivez les instructions dans [Configurer un groupe de disponibilité Always On pour SQL Server sur Linux](sql-server-linux-availability-group-configure-ha.md). En particulier, créez le groupe de disponibilité avec `CLUSTER_TYPE=WSFC` (dans Windows) ou `CLUSTER_TYPE=EXTERNAL` (dans Linux). Puis, intégrez-le à un gestionnaire de clusters en utilisant un clustering de basculement Windows Server sur Windows ou Pacemaker sur Linux.

## <a name="connect-to-read-only-secondary-replicas"></a>Se connecter à des réplicas secondaires en lecture seule

Il existe deux façons de se connecter à des réplicas secondaires en lecture seule. Les applications peuvent se connecter directement à l’instance de SQL Server qui héberge le réplica secondaire et interroger les bases de données. Elles peuvent aussi utiliser le routage en lecture seule, ce qui nécessite un écouteur.

* [Réplicas secondaires accessibles en lecture](../database-engine/availability-groups/windows/active-secondaries-readable-secondary-replicas-always-on-availability-groups.md)
* [Routage en lecture seule](../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md#ConnectToSecondary)

## <a name="fail-over-the-primary-replica-on-a-read-scale-availability-group"></a>Basculer le réplica principal sur un groupe de disponibilité avec échelle lecture

[!INCLUDE[Force failover](../includes/ss-force-failover-read-scale-out.md)]

## <a name="next-steps"></a>Étapes suivantes

* [Configurer un groupe de disponibilité distribué](../database-engine/availability-groups/windows/distributed-availability-groups.md)
* [En savoir plus sur les groupes de disponibilité](../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)
* [Effectuer un basculement manuel forcé](../database-engine/availability-groups/windows/perform-a-forced-manual-failover-of-an-availability-group-sql-server.md)