---
title: Modèles de déploiement des groupes de disponibilité - SQL Server sur Linux
description: Découvrez les configurations de déploiement prises en charge pour les groupes de disponibilité Always On SQL Server sur serveurs Linux.
ms.custom: seo-lt-2019
ms.date: 04/17/2019
ms.prod: sql
ms.technology: linux
ms.topic: conceptual
ms.assetid: edd75f68-dc62-4479-a596-57ce8ad632e5
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.openlocfilehash: edeb7ab3de19b50f78f2b4b5bab9c0f8afe80780
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347783"
---
# <a name="high-availability-and-data-protection-for-availability-group-configurations"></a>Haute disponibilité et protection des données pour les configurations des groupes de disponibilité

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Cet article présente les configurations de déploiement prises en charge pour les groupes de disponibilité Always On SQL Server sur des serveurs Linux. Un groupe de disponibilité prend en charge la haute disponibilité et la protection des données. La détection automatique des défaillances, le basculement automatique et la reconnexion transparente après le basculement offrent une haute disponibilité. Les réplicas synchronisés assurent la protection des données. 

Sur un cluster de basculement Windows Server (WSFC), une configuration courante pour la haute disponibilité utilise deux réplicas synchrones et un troisième serveur ou partage de fichiers pour fournir le quorum. Le témoin de partage de fichiers valide la configuration du groupe de disponibilité, l’état de la synchronisation et le rôle du réplica, par exemple. Cette configuration garantit que le réplica secondaire choisi comme cible de basculement présente les dernières modifications de configuration des groupes de disponibilité et de données. 

Le WSFC synchronise les métadonnées de configuration pour l’arbitrage de basculement entre les réplicas du groupe de disponibilité et le témoin de partage de fichiers. Quand un groupe de disponibilité n’est pas sur un WSFC, les instances de SQL Server stockent les métadonnées de configuration dans la base de données MASTER.

Par exemple, un groupe de disponibilité sur un cluster Linux possède `CLUSTER_TYPE = EXTERNAL`. Il n’existe aucun WSFC pour arbitrer le basculement. Dans ce cas, les métadonnées de configuration sont managées et maintenues par les instances de SQL Server. Étant donné qu’il n’existe aucun serveur témoin dans ce cluster, une troisième instance de SQL Server est requise pour stocker les métadonnées de l’état de configuration. Les trois instances de SQL Server fournissent ensemble un stockage de métadonnées distribué pour le cluster. 

Le gestionnaire de clusters peut interroger les instances de SQL Server dans le groupe de disponibilité et orchestrer le basculement pour maintenir une haute disponibilité. Dans un cluster Linux, le Pacemaker est le gestionnaire de clusters. 

SQL Server 2017 CU1 offre une haute disponibilité pour un groupe de disponibilité avec `CLUSTER_TYPE = EXTERNAL` pour deux réplicas synchrones et un réplica de configuration uniquement. Le réplica de configuration uniquement peut être hébergé sur n’importe quelle édition de SQL Server 2017 CU1 ou version ultérieure, y compris l’édition SQL Server Express. Le réplica de configuration uniquement conserve les informations de configuration sur le groupe de disponibilité dans la base de données MASTER, mais ne contient pas les bases de données utilisateur dans le groupe de disponibilité. 

## <a name="how-the-configuration-affects-default-resource-settings"></a>Impact de la configuration sur les paramètres de ressource par défaut

SQL Server 2017 introduit le paramètre de ressource de cluster `REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT`. Ce paramètre garantit que le nombre spécifié de réplicas secondaires écrivent les données de transaction dans le journal avant que le réplica principal ne valide chaque transaction. Lorsque vous utilisez un gestionnaire de cluster externe, ce paramètre affecte la haute disponibilité et la protection des données. La valeur par défaut du paramètre dépend de l’architecture au moment de la création de la ressource de cluster. Quand vous installez l’agent de ressource SQL Server `mssql-server-ha` et que vous créez une ressource de cluster pour le groupe de disponibilité, le gestionnaire de clusters détecte la configuration du groupe de disponibilité et définit `REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT` en conséquence. 

S’il est pris en charge par la configuration, le paramètre de l’agent de ressource `REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT` est défini sur la valeur qui fournit la haute disponibilité et la protection des données. Pour plus d’informations, consultez [Comprendre l’agent de ressource SQL Server pour Pacemaker](#pacemakerNotify).

Les sections suivantes expliquent le comportement par défaut de la ressource de cluster. 

Choisissez une conception de groupe de disponibilité pour répondre aux exigences spécifiques de l’entreprise en matière de haute disponibilité, de protection des données et de lecture à l’échelle.

Les configurations suivantes décrivent les modèles de conception de groupes de disponibilité et les fonctionnalités de chaque modèle. Ces modèles de conception s’appliquent aux groupes de disponibilité avec `CLUSTER_TYPE = EXTERNAL` pour les solutions à haute disponibilité. 

- **Trois réplicas synchrones**
- **Deux réplicas synchrones**
- **Deux réplicas synchrones et un réplica de configuration uniquement**

<a name="threeSynch"></a>

## <a name="three-synchronous-replicas"></a>Trois réplicas synchrones

Cette configuration se compose de trois réplicas synchrones. Par défaut, il offre une haute disponibilité et une protection des données. Il peut également fournir une mise à l’échelle en lecture.

![Trois réplicas][3]

Un groupe de disponibilité avec trois réplicas synchrones peut fournir une mise à l’échelle en lecture, une haute disponibilité et une protection des données. Le tableau suivant décrit le comportement de disponibilité. 

|Comportement de disponibilité |échelle lecture|Haute disponibilité et </br> protection de données | Protection de données|
|:---|---|---|---|
|`REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT=`|0 |1<sup>\*</sup>|2|
|Indisponibilité du réplica principal |Basculement automatique. Le nouveau principal est en lecture/écriture. |Basculement automatique. Le nouveau principal est en lecture/écriture. |Basculement automatique. Le nouveau principal n’est pas disponible pour les transactions utilisateur jusqu’à la récupération du principal précédent puis se joint au groupe de disponibilité comme réplica secondaire. |
|Une indisponibilité du réplica secondaire  | Le principal est en lecture/écriture. Aucun basculement automatique si le principal échoue. |Le principal est en lecture/écriture. Aucun basculement automatique en cas d’échec du principal également. | Le principal n’est pas disponible pour les transactions utilisateur. |

<sup>\*</sup> Par défaut

<a name="twoSynch"></a>

## <a name="two-synchronous-replicas"></a>Deux réplicas synchrones

Cette configuration permet la protection des données. À l’instar des autres configurations des groupes de disponibilité, il peut activer l’échelle de lecture. La configuration des deux réplicas synchrones ne fournit pas une haute disponibilité automatique. Une configuration à deux réplicas est uniquement applicable à la version RTM de SQL Server 2017 et n’est plus prise en charge avec les versions supérieures (CU1 et ultérieures) de SQL Server 2017.

![Deux réplicas synchrones][1]

Un groupe de disponibilité avec deux réplicas synchrones offre une mise à l’échelle en lecture et une protection des données. Le tableau suivant décrit le comportement de disponibilité. 

|Comportement de disponibilité |échelle lecture |Protection de données|
|:---|---|---|
|`REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT=`|0 <sup>\*</sup>|1|
|Indisponibilité du réplica principal | Basculement manuel. Perte de données possible. Le nouveau principal est en lecture/écriture.| Basculement automatique. Le nouveau principal n’est pas disponible pour les transactions utilisateur jusqu’à la récupération du principal précédent puis se joint au groupe de disponibilité comme réplica secondaire.|
|Une indisponibilité du réplica secondaire  |Le principal est en lecture/écriture, il est exposé à des pertes de données. |Le principal n’est pas disponible pour les transactions utilisateur jusqu’à la récupération du secondaire.|

<sup>\*</sup> Par défaut

<a name = "configOnly"></a>

## <a name="two-synchronous-replicas-and-a-configuration-only-replica"></a>Deux réplicas synchrones et un réplica de configuration uniquement

Un groupe de disponibilité avec deux réplicas synchrones (ou plus) et un réplica de configuration uniquement assurent la protection des données et peuvent également fournir une haute disponibilité. Le diagramme suivant illustre cette architecture :

![Groupe de disponibilité de configuration uniquement][2]

1. Réplication synchrone des données utilisateur vers le réplica secondaire. Il comprend également des métadonnées de configuration des groupes de disponibilité.
2. Réplication synchrone des métadonnées de configuration des groupes de disponibilité. Elle n’inclut pas les données utilisateur.

Dans le diagramme des groupes de disponibilité, un réplica principal envoie (push) des données de configuration au réplica secondaire et au réplica de configuration uniquement. Le réplica secondaire reçoit également les données utilisateur. Le réplica de configuration uniquement ne reçoit pas de données utilisateur. Le réplica secondaire est en mode de disponibilité synchrone. Le réplica de configuration uniquement ne contient pas les bases de données dans le groupe de disponibilité, uniquement les métadonnées relatives au groupe de disponibilité. Les données de configuration sur le réplica de configuration uniquement sont validées de façon synchrone.

> [!NOTE]
> Un groupe de disponibilité avec un réplica de configuration uniquement est nouveau pour SQL Server 2017 CU1. Toutes les instances de SQL Server dans le groupe de disponibilité doivent avoir la version SQL Server 2017 CU1 ou ultérieure. 

La valeur par défaut pour `REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT` est 0. Le tableau suivant décrit le comportement de disponibilité. 

|Comportement de disponibilité |Haute disponibilité et </br> protection de données | Protection de données|
|:---|---|---|
|`REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT=`|0 <sup>\*</sup>|1|
|Indisponibilité du réplica principal | Basculement automatique. Le nouveau principal est en lecture/écriture. | Basculement automatique. Le nouveau principal n’est pas disponible pour les transactions utilisateur. |
|Interruption du réplica secondaire | Le principal est en lecture/écriture, en cours d’exécution exposée à la perte de données (si le principal échoue et ne peut pas être récupéré). Aucun basculement automatique en cas d’échec du principal également. | Le principal n’est pas disponible pour les transactions utilisateur. Aucun réplica à basculer si le principal échoue également. |
|Interruption du réplica de configuration uniquement | Le principal est en lecture/écriture. Aucun basculement automatique en cas d’échec du principal également. | Le principal est en lecture/écriture. Aucun basculement automatique en cas d’échec du principal également. |
|Interruption du réplica secondaire synchrone + configuration uniquement| Le principal n’est pas disponible pour les transactions utilisateur. Aucun basculement automatique. | Le principal n’est pas disponible pour les transactions utilisateur. Aucun réplica à basculer si le principal échoue également. |

<sup>\*</sup> Par défaut

> [!NOTE]
> L’instance de SQL Server qui héberge le réplica de configuration uniquement peut également héberger d’autres bases de données. Elle peut également faire partie d’une base de données de configuration uniquement pour plusieurs groupes de disponibilité. 

## <a name="requirements"></a>Spécifications

- Tous les réplicas d’un groupe de disponibilité avec un réplica de configuration uniquement doivent être SQL Server 2017 CU 1 ou une version ultérieure.
- Toute édition de SQL Server peut héberger un réplica de configuration uniquement, y compris des SQL Server Express. 
- Le groupe de disponibilité a besoin d’au moins un réplica secondaire en plus du réplica principal.
- Les réplicas de configuration uniquement ne sont pas comptabilisés dans le nombre maximal de réplicas par instance de SQL Server. L’édition standard SQL Server autorise jusqu’à trois réplicas, SQL Server Entreprise Edition en autorise jusqu’à 9.

## <a name="considerations"></a>Considérations

- Il n’y a pas plus d’un réplica de configuration uniquement par groupe de disponibilité. 
- Un réplica de configuration uniquement ne peut pas être un réplica principal.
- Vous ne pouvez pas modifier le mode de disponibilité d’un réplica de configuration uniquement. Pour passer d’un réplica de configuration uniquement à un réplica secondaire synchrone ou asynchrone, supprimez le réplica de configuration uniquement et ajoutez un réplica secondaire avec le mode de disponibilité requis. 
- Un réplica de configuration uniquement est synchrone avec les métadonnées du groupe de disponibilité. Il n’y a aucune donnée utilisateur. 
- Un groupe de disponibilité avec un réplica principal et un réplica de configuration uniquement, mais aucun réplica secondaire n’est pas valide. 
- Vous ne pouvez pas créer un groupe de disponibilité sur une instance de l’édition SQL Server Express. 

<a name="pacemakerNotify"></a>

## <a name="understand-sql-server-resource-agent-for-pacemaker"></a>Comprendre l’agent de ressources SQL Server pour Pacemaker

SQL Server CTP 2017 1.4 a ajouté `sequence_number` à `sys.availability_groups` pour permettre au Pacemaker d’identifier la manière dont les réplicas secondaires à jour sont associés au réplica principal. `sequence_number` est une valeur de type BIGINT à croissance monotone qui représente l’état de mise à jour du réplica des groupes de disponibilité local. Pacemaker met à jour le `sequence_number` avec chaque modification de configuration du groupe de disponibilité. Les exemples de modifications de configuration incluent le basculement, l’ajout ou la suppression de réplicas. Le numéro est mis à jour sur le principal avant d’être répliqué aux réplicas secondaires. Ainsi, un réplica secondaire avec une configuration à jour a le même numéro de séquence que le principal. 

Quand Pacemaker décide de promouvoir un réplica en principal, il envoie dans un premier temps une notification de *prépromotion* à tous les réplicas. Les réplicas retournent le nombre de séquences. Ensuite, quand Pacemaker tente effectivement de promouvoir un réplica en principal, le réplica n’assure sa promotion que si son numéro de séquence est supérieur à tous les numéros de séquence. Si son numéro de séquence ne correspond pas au numéro de séquence le plus élevé, le réplica rejette l’opération de promotion. De cette façon, seul le réplica ayant le numéro de séquence le plus élevé peut être promu principal, ce qui évite toute perte de données. 

Ce processus exige au moins un réplica disponible pour la promotion avec le même numéro de séquence que le principal précédent. L’agent de ressource Pacemaker définit `REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT` de sorte qu’au moins un réplica secondaire synchrone soit à jour et disponible pour être la cible d’un basculement automatique par défaut. Avec chaque action de surveillance, la valeur de `REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT` est calculée (et mise à jour si nécessaire). La valeur `REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT` est le « nombre de réplicas synchrones » divisé par 2. Au moment du basculement, l’agent de ressource requiert (`total number of replicas` - `REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT` réplicas) pour répondre à la notification de prépromotion. Le réplica avec le `sequence_number` le plus élevé sera promu en principal. 

Par exemple, un groupe de disponibilité avec trois réplicas synchrones : un réplica principal et deux réplicas secondaires avec validation synchrone.

- `REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT` est 1 ; (3 / 2 -> 1).

- Le nombre de réplicas nécessaires pour répondre à l’action de prépromotion est 2 ; (3 - 1 = 2). 

Dans ce scénario, deux réplicas doivent répondre pour que le basculement soit déclenché. Pour un basculement automatique réussi après une interruption du réplica principal, les deux réplicas secondaires doivent être à jour et répondre à la notification de prépromotion. S’ils sont en ligne et synchrones, ils ont le même numéro de séquence. Le groupe de disponibilité promeut l’un d’eux. Si un seul des réplicas secondaires répond à l’action de prépromotion, l’agent de ressource ne peut pas garantir que le secondaire qui a répondu au sequence_number le plus élevé et le basculement n’est pas déclenché.

> [!IMPORTANT]
> Quand la valeur de `REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT` est 0, il existe un risque de perte de données. En cas d’interruption d’un réplica principal, l’agent de ressource ne déclenche pas automatiquement un basculement. Vous pouvez attendre la récupération du principal ou effectuer un basculement manuel à l'aide de `FORCE_FAILOVER_ALLOW_DATA_LOSS`.

Vous pouvez choisir de remplacer le comportement par défaut et d’éviter que la ressource du groupe de disponibilité ne définisse `REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT` automatiquement.

Le script suivant définit `REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT` sur 0 sur un groupe de disponibilité nommé `<**ag1**>`. Avant de l’exécuter, remplacez `<**ag1**>` par le nom de votre groupe de disponibilité.

```bash
sudo pcs resource update <**ag1**> required_synchronized_secondaries_to_commit=0
```

Pour rétablir la valeur par défaut, en fonction de l’exécution de la configuration du groupe de disponibilité :

```bash
sudo pcs resource update <**ag1**> required_synchronized_secondaries_to_commit=
```

> [!NOTE]
> Lorsque vous exécutez les commandes précédentes, le principal est temporairement rétrogradé au secondaire, puis de nouveau promu. La mise à jour des ressources provoque l’arrêt et le redémarrage de tous les réplicas. La nouvelle valeur de `REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT` est définie uniquement une fois que les réplicas sont redémarrés, pas instantanément.

## <a name="see-also"></a>Voir aussi

[Groupes de disponibilité sur Linux](sql-server-linux-availability-group-overview.md)

<!--Image references-->
[1]: ./media/sql-server-linux-availability-group-ha/1-read-scale-out.png
[2]: ./media/sql-server-linux-availability-group-ha/2-configuration-only.png
[3]: ./media/sql-server-linux-availability-group-ha/3-three-replica.png
[4]: ./media/sql-server-linux-availability-group-ha/configuration-only-example.png
