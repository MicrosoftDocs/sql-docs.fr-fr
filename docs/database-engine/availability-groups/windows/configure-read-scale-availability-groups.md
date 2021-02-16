---
title: Configurer une échelle lecture pour un groupe de disponibilité
description: Découvrez comment configurer votre groupe de disponibilité SQL Server Always On pour les charges de travail avec échelle lecture sur Windows.
ms.custom: seodec18
author: cawrites
ms.author: chadam
ms.reviewer: ''
ms.date: 05/24/2018
ms.topic: how-to
ms.prod: sql
ms.technology: availability-groups
ms.openlocfilehash: 995d8d2238ef9ded767544423093c8352e02bb1c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343369"
---
# <a name="configure-read-scale-for-an-always-on-availability-group"></a>Configurer une échelle lecture pour un groupe de disponibilité Always On

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

Vous pouvez configurer un groupe de disponibilité SQL Server AlwaysOn pour les charges de travail d’échelle lecture sur Windows. Il existe deux types d’architecture pour les groupes de disponibilité :
* Une architecture visant la haute disponibilité qui utilise un gestionnaire de cluster pour améliorer la continuité d’activité et qui peut inclure des réplicas secondaires accessibles en lecture. Pour créer cette architecture haute disponibilité, consultez [Créer et configurer des groupes de disponibilité sur Windows](creation-and-configuration-of-availability-groups-sql-server.md). 
* Une architecture qui prend en charge seulement des charges de travail avec échelle lecture. 

Cet article explique comment créer un groupe de disponibilité sans gestionnaire de cluster pour les charges de travail avec échelle lecture. Cette architecture fournit uniquement une échelle lecture. Elle n’assure pas la haute disponibilité.

>[!NOTE]
>Un groupe de disponibilité avec `CLUSTER_TYPE = NONE` peut inclure des réplicas hébergés sur différentes plateformes de système d’exploitation. Il ne peut pas prendre en charge la haute disponibilité. Pour le système d’exploitation Linux, consultez [Configurer un groupe de disponibilité SQL Server pour l’échelle lecture sur Linux](../../../linux/sql-server-linux-availability-group-configure-rs.md).

[!INCLUDE [Create prerequisites](../../../includes/ss-availability-group-rs-prereq.md)]

## <a name="create-an-availability-group"></a>Créer un groupe de disponibilité

Créez un groupe de disponibilité. Définissez `CLUSTER_TYPE = NONE`. Pour chaque réplica, définissez également `FAILOVER_MODE = NONE`. Les applications clientes qui exécutent des charges de travail d’analytique et de rapports peuvent se connecter directement aux bases de données secondaires. Vous pouvez également créer une liste de routage en lecture seule. Les connexions au réplica principal transfèrent les demandes de connexion à chacun des réplicas secondaires de la liste de routage en mode tourniquet (round-robin).

Le script Transact-SQL suivant crée un groupe de disponibilité nommé `ag1`. Le script configure les réplicas du groupe de disponibilité avec `SEEDING_MODE = AUTOMATIC`. Ce paramètre fait que SQL Server crée automatiquement la base de données sur chaque serveur secondaire après son ajout au groupe de disponibilité. 

Mettez à jour le script suivant en fonction de votre environnement. Remplacez les valeurs `<node1>` et `<node2>` par les noms des instances de SQL Server qui hébergent les réplicas. Remplacez la valeur `<5022>` par le port que vous définissez pour le point de terminaison. Exécutez le script Transact-SQL suivant sur le réplica SQL Server principal :

```sql
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

### <a name="join-secondary-sql-server-instances-to-the-availability-group"></a>Joindre des instances SQL Server secondaires au groupe de disponibilité

Le script Transact-SQL suivant joint un serveur à un groupe de disponibilité nommé `ag1`. Mettez à jour le script en fonction de votre environnement. Pour joindre le groupe de disponibilité, exécutez le script Transact-SQL suivant sur chaque réplica SQL Server secondaire :

```sql
ALTER AVAILABILITY GROUP [ag1] JOIN WITH (CLUSTER_TYPE = NONE);

ALTER AVAILABILITY GROUP [ag1] GRANT CREATE ANY DATABASE;
```

[!INCLUDE [Create post](../../../includes/ss-availability-group-rs-postactivity.md)]

Ce groupe de disponibilité n’est pas une configuration à haute disponibilité. Si vous avez besoin de la haute disponibilité, suivez les instructions dans [Configurer un groupe de disponibilité AlwaysOn pour SQL Server sur Linux](../../../linux/sql-server-linux-availability-group-configure-ha.md) ou dans [Création et configuration de groupes de disponibilité sur Windows](creation-and-configuration-of-availability-groups-sql-server.md).

## <a name="connect-to-read-only-secondary-replicas"></a>Se connecter à des réplicas secondaires en lecture seule

Vous pouvez vous connecter à des réplicas secondaires en lecture seule de deux façons :
* Les applications peuvent se connecter directement à l’instance de SQL Server qui héberge le réplica secondaire et interroger les bases de données. Pour plus d’informations, consultez [Accès en lecture aux réplicas secondaires](active-secondaries-readable-secondary-replicas-always-on-availability-groups.md).
* Les applications peuvent aussi utiliser le routage en lecture seule, qui nécessite un écouteur. Si vous déployez un scénario d’échelle lecture sans gestionnaire de cluster, vous pouvez toujours créer un écouteur qui pointe vers l’adresse IP du réplica principal actuel et un port différent de celui sur lequel écoute le serveur SQL Server. Vous devrez recréer l’écouteur pour qu’il pointe vers la nouvelle adresse IP principale après un basculement. Pour plus d’informations, consultez [Routage en lecture seule](listeners-client-connectivity-application-failover.md#ConnectToSecondary).

## <a name="fail-over-the-primary-replica-on-a-read-scale-availability-group"></a>Basculer le réplica principal sur un groupe de disponibilité avec échelle lecture

[!INCLUDE[Force failover](../../../includes/ss-force-failover-read-scale-out.md)]

## <a name="next-steps"></a>Étapes suivantes

* [Configurer un groupe de disponibilité distribué](./distributed-availability-groups.md)
* [En savoir plus sur les groupes de disponibilité](overview-of-always-on-availability-groups-sql-server.md)
* [Effectuer un basculement manuel forcé](perform-a-forced-manual-failover-of-an-availability-group-sql-server.md)