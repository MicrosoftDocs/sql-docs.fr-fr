---
title: Persistance des données sur Kubernetes
titleSuffix: SQL Server big data clusters
description: Découvrez comment les volumes persistants fournissent un modèle de plug-in pour le stockage dans Kubernetes. Découvrez aussi plus d’informations sur le fonctionnement de la persistance des données dans un cluster Big Data SQL Server 2019.
author: mihaelablendea
ms.author: mihaelab
ms.reviewer: mikeray
ms.date: 11/04/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: a2836a6966727c0c2d27b363048f2e1dcd6afa49
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052085"
---
# <a name="data-persistence-with-sql-server-big-data-cluster-in-kubernetes"></a>Persistance des données avec un cluster Big Data SQL Server sur Kubernetes

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Les [volumes persistants](https://kubernetes.io/docs/concepts/storage/persistent-volumes/) fournissent un modèle de plug-in pour le stockage dans Kubernetes. Dans ce modèle, la façon dont le stockage est fourni dépend de la façon dont il est consommé. Par conséquent, vous pouvez utiliser votre propre stockage à haute disponibilité et le connecter au cluster Big Data SQL Server. Ceci vous donne un contrôle total sur le type de stockage, sur la disponibilité et sur les performances dont vous avez besoin. Kubernetes prend en charge [différents types de solutions de stockage](https://kubernetes.io/docs/concepts/storage/storage-classes/#provisioner), notamment les disques/fichiers Azure, NFS (Network File System) et le stockage local.

## <a name="configure-persistent-volumes"></a>Configurer des volumes persistants

Un cluster Big Data SQL Server consomme ces volumes persistants en utilisant des [classes de stockage](https://kubernetes.io/docs/concepts/storage/storage-classes/). Vous pouvez créer différentes classes de stockage pour différents types de stockage et les spécifier au moment du déploiement du cluster Big Data. Vous pouvez configurer la classe de stockage et la taille de revendication des volumes persistants à utiliser pour un objectif particulier au niveau du pool. Un cluster Big Data SQL Server crée des [revendications de volumes persistants](https://kubernetes.io/docs/concepts/storage/persistent-volumes/#persistentvolumeclaims) en utilisant le nom de la classe de stockage spécifié pour chaque composant nécessitant des volumes persistants. Il monte ensuite le ou les volumes persistants correspondants dans le pod. 

Voici quelques aspects importants à prendre en compte lorsque vous planifiez la configuration du stockage pour votre cluster Big Data :

- Pour réussir le déploiement du cluster Big Data, assurez-vous que le nombre requis de volumes persistants est disponible. Si vous effectuez le déploiement sur un cluster Azure Kubernetes Service (AKS) et que vous utilisez une classe de stockage intégrée (`default` ou `managed-premium`), celle-ci prend en charge le provisionnement dynamique pour les volumes persistants. Ainsi, vous n’avez pas besoin de créer au préalable les volumes persistants, mais vous devez vous assurer que les nœuds worker disponibles dans le cluster AKS peuvent attacher un nombre de disques égal au nombre de volumes persistants nécessaires pour le déploiement. Selon la [taille de machine virtuelle](/azure/virtual-machines/linux/sizes) spécifiée pour les nœuds worker, chaque nœud peut attacher un certain nombre de disques. Pour un cluster ayant la taille par défaut (sans haute disponibilité), un minimum de 24 disques est requis. Si vous activez la haute disponibilité ou si vous effectuez un scale-up d’un pool quelconque, veillez à avoir au moins deux volumes persistants pour chaque réplica supplémentaire, quelle que soit la ressource dont vous effectuez l’augmentation.

- Si le provisionneur de stockage pour la classe de stockage que vous fournissez dans la configuration ne prend pas en charge le provisionnement dynamique, vous devez créer au préalable les volumes persistants. Par exemple, le provisionneur `local-storage` n’active pas le provisionnement dynamique. Consultez cet [exemple de script](https://github.com/microsoft/sql-server-samples/tree/master/samples/features/sql-big-data-cluster/deployment/kubeadm/ubuntu) pour des indications sur la façon de procéder dans un cluster Kubernetes déployé avec `kubeadm`.

- Lorsque vous déployez un cluster Big Data, vous pouvez configurer la même classe de stockage à utiliser par tous les composants du cluster. Mais, en guide de bonne pratique, pour un déploiement en production, différents composants requièrent différentes configurations de stockage pour prendre en charge les différentes charges de travail en termes de taille ou de débit. Vous pouvez remplacer la configuration de stockage par défaut spécifiée dans le contrôleur pour chaque instance principale SQL Server, jeu de données et pool de données. Cet article fournit des exemples sur la procédure à suivre.

- Lors du calcul des exigences de dimensionnement du pool de stockage, vous devez prendre en compte le facteur de réplication avec lequel HDFS est configuré.  Le facteur de réplication peut être configuré au moment du déploiement dans le fichier de configuration du déploiement de cluster. La valeur par défaut des profils dev-test (c’est-à-dire, `aks-dev-test` ou `kubeadm-dev-test`) est de 2. Pour les profils que nous recommandons d’utiliser avec les déploiements de production (c’est-à-dire `kubeadm-prod`), la valeur par défaut est de 3. Selon nos bonnes pratiques, il est recommandé de configurer le déploiement de production de votre cluster Big Data avec un facteur de réplication d’au moins 3 pour HDFS. La valeur du facteur de réplication aura un impact sur le nombre d’instances présentes dans le pool de stockage. Le nombre d’instances de pool de stockage à déployer doit être au moins égal à la valeur du facteur de réplication. De plus, vous devez dimensionner le stockage en conséquence, car les données seront répliquées dans HDFS autant de fois que l’indique la valeur du facteur de réplication. Pour plus d’informations sur la réplication des données dans HDFS, [cliquez ici](https://hadoop.apache.org/docs/r3.2.1/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html#Data_Replication). 

- À partir de la version SQL Server 2019 CU1, BDC ne permet plus de mettre à jour le paramètre de configuration du stockage après le déploiement. Cette contrainte vous empêche d’utiliser des opérations BDC pour modifier la taille de la revendication de volume persistant pour chaque instance, et pour mettre à l’échelle des pools après le déploiement. Par conséquent, il est très important de planifier la disposition du stockage avant de déployer un cluster Big Data. Toutefois, vous pouvez étendre la taille du volume persistant en utilisant directement des API Kubernetes. Dans ce cas, les métadonnées BDC ne seront pas mises à jour et vous verrez des incohérences au niveau de la taille des volumes dans les métadonnées du cluster de configuration.

- Par déploiement sur Kubernetes en tant qu’applications conteneurisées et par l’utilisation de fonctionnalités telles que les ensembles avec état et le stockage persistant, Kubernetes garantit le redémarrage des pods en cas de problèmes d’intégrité et leur attachement au même stockage persistant. Toutefois, en cas de défaillance d’un nœud exigeant le redémarrage du pod sur un autre nœud, le risque d’indisponibilité augmente si le stockage est local sur le nœud défaillant. Pour réduire ce risque, vous devez configurer une redondance supplémentaire et activer des [fonctionnalités de haute disponibilité](deployment-high-availability.md) ou utiliser un stockage redondant à distance. Voici une vue d’ensemble des options de stockage pour les différents composants des clusters Big Data :

| Ressources | Type de stockage pour les données | Type de stockage pour le journal |  Notes |
|---|---|---|--|
| Instance principale SQL Server | Stockage local (réplicas >= 3) ou redondant à distance (réplica = 1) | Stockage local | Une implémentation basée sur un ensemble avec état, où les pods sont associés aux nœuds, garantit que les redémarrages et les défaillances temporaires n’entraînent pas de perte de données. |
| Pool de calcul | Stockage local | Stockage local | Aucune donnée utilisateur stockée. |
| Pool de données | Stockage redondant local/distant | Stockage local | Utilisez un stockage redondant à distance si le pool de données ne peut pas être reconstruit à partir d’autres sources de données.  |
| Pool de stockage (HDFS) | Stockage redondant local/distant | Stockage local | La réplication HDFS est activée. |
| Pool Spark | Stockage local | Stockage local | Aucune donnée utilisateur stockée. |
| Contrôle (controldb, metricsdb, logsdb)| Stockage redondant à distance | Stockage local | Essentiel pour utiliser la redondance de niveau de stockage pour les métadonnées des clusters Big Data. |

> [!IMPORTANT]
> Assurez-vous que les composants associés au contrôle figurent dans un appareil de stockage redondant à distance. Un pod `controldb-0` héberge une instance SQL Server avec des bases de données qui stockent toutes les métadonnées sur les états de service de cluster, les différents paramètres et les autres informations pertinentes. Si cette instance devient indisponible, cela entraîne des problèmes d’intégrité avec d’autres services dépendants dans le cluster.

## <a name="configure-big-data-cluster-storage-settings"></a>Configurer les paramètres de stockage du cluster Big Data

Comme pour les autres personnalisations, vous pouvez spécifier des paramètres de stockage dans les fichiers de configuration de cluster au moment du déploiement pour chaque pool dans le fichier de configuration `bdc.json` et pour les services de contrôle dans le fichier `control.json`. S’il n’y a pas de paramètres de configuration de stockage dans les spécifications du pool, les paramètres de stockage de contrôle sont utilisés pour tous les autres composants, y compris l’instance principale SQL Server (ressource `master`), HDFS (ressource `storage-0`) et les composants de pool de données. Voici un exemple de code représentant la section de configuration du stockage que vous pouvez inclure dans la spécification :

```json
    "storage": 
    {
      "data": {
        "className": "default",
        "accessMode": "ReadWriteOnce",
        "size": "15Gi"
      },
      "logs": {
        "className": "default",
        "accessMode": "ReadWriteOnce",
        "size": "10Gi"
    }
```

Le déploiement du cluster Big Data utilise un stockage persistant pour stocker les données, les métadonnées et les journaux pour différents composants. Vous pouvez personnaliser la taille des revendications de volumes persistants créées dans le cadre du déploiement. À titre de bonne pratique, nous vous recommandons d’utiliser des classes de stockage avec une [stratégie de récupération](https://kubernetes.io/docs/concepts/storage/storage-classes/#reclaim-policy)*Retain* (Conserver).

> [!WARNING]
> L’exécution sans stockage persistant peut fonctionner dans un environnement de test, mais elle peut aboutir à un cluster non fonctionnel. Au redémarrage d’un pod, les métadonnées du cluster et/ou les données utilisateur sont définitivement perdues. Nous vous déconseillons d’exécuter cette configuration.

La section [Configuration du stockage](#config-samples) fournit d’autres exemples de configuration des paramètres de stockage pour le déploiement de votre cluster Big Data SQL Server.

## <a name="aks-storage-classes"></a>Classes de stockage d’AKS

AKS est fourni avec [deux classes de stockage intégrées](/azure/aks/azure-disks-dynamic-pv/), `default` et `managed-premium`, et avec un provisionneur dynamique pour celles-ci. Vous pouvez spécifier l’une ou l’autre de ces classes, ou créer votre propre classe de stockage, pour déployer un cluster Big Data avec le stockage persistant activé. Par défaut, le fichier de configuration de cluster intégré pour AKS, `aks-dev-test`, est fourni avec des configurations de stockage persistantes qui utilisent la classe de stockage `default`.

> [!WARNING]
> Les volumes persistants créés avec les classes de stockage intégrées `default` et `managed-premium` ont une stratégie de récupération *Delete* (Supprimer). Lorsque vous supprimez le cluster Big Data SQL Server, les revendications de volumes persistants sont supprimées, puis les volumes persistants le sont. Vous pouvez créer des classes de stockage personnalisées en utilisant le provisionneur `azure-disk` avec une stratégie de récupération `Retain` (Conserver), comme décrit dans [Stockage de concepts](/azure/aks/concepts-storage/#storage-classes).

## <a name="storage-classes-for-kubeadm-clusters"></a>Classes de stockage pour les clusters `kubeadm` 

Les clusters Kubernetes déployés avec `kubeadm` n’ont pas de classe de stockage intégrée. Vous devez créer vos propres classes de stockage et vos propres volumes persistants en utilisant un stockage local ou votre [provisionneur de stockage préféré](https://kubernetes.io/docs/concepts/storage/storage-classes/#provisioner). Dans cette situation, vous définissez `className` sur la classe de stockage que vous avez configurée.

> [!IMPORTANT]
>  Dans les fichiers de configuration de déploiement intégré pour kubeadm (`kubeadm-dev-test` et `kubeadm-prod`), aucun nom de classe de stockage n’est spécifié pour le stockage des données et des journaux. Avant le déploiement, vous devez personnaliser le fichier de configuration et définir la valeur de `className`. Dans le cas contraire, les validations préalables au déploiement échouent. Le déploiement comporte également une étape de validation qui vérifie l’existence de la classe de stockage, mais pas pour les volumes persistants nécessaires. Vérifiez que vous créez suffisamment de volumes en fonction de l’échelle de votre cluster. Pour la taille de cluster minimale par défaut (échelle par défaut, pas de haute disponibilité), vous devez créer au moins 24 volumes persistants. Pour un exemple de la façon dont vous pouvez créer des volumes persistants à l’aide du provisionneur local, consultez [Créer un cluster Kubernetes avec Kubeadm](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/sql-big-data-cluster/deployment/kubeadm/ubuntu).

## <a name="customize-storage-configurations-for-each-pool"></a>Personnaliser les configurations de stockage pour chaque pool

Pour toutes les personnalisations, vous devez d’abord créer une copie du fichier de configuration intégré que vous voulez utiliser. Par exemple, la commande suivante crée une copie des fichiers de configuration de déploiement `aks-dev-test` dans un sous-répertoire nommé `custom` :

```bash
azdata bdc config init --source aks-dev-test --target custom
```

Ce processus crée deux fichiers, `bdc.json` et `control.json`, que vous pouvez personnaliser soit en les modifiant manuellement, soit à l’aide de la commande `azdata bdc config`. Vous pouvez utiliser une combinaison de bibliothèques jsonpath et jsonpatch pour fournir des moyens de modifier vos fichiers de configuration.


### <a name="configure-storage-class-name-andor-claims-size"></a><a id="config-samples"></a> Configurer le nom de la classe de stockage et/ou la taille des revendications

Par défaut, la taille des revendications de volumes persistants provisionnés pour chaque pod provisionné dans le cluster est de 10 gigaoctets (Go). Vous pouvez mettre à jour cette valeur pour prendre en charge les charges de travail que vous exécutez dans un fichier de configuration personnalisé avant le déploiement du cluster.

Dans l’exemple suivant, la taille des revendications de volumes persistants est mise à jour à 32 Gio dans `control.json`. Si cette valeur n’est pas remplacée au niveau du pool, ce paramètre est appliqué à tous les pools.

```bash
azdata bdc config replace --config-file custom/control.json --json-values "$.spec.storage.data.size=100Gi"
```

L’exemple suivant montre comment modifier la classe de stockage pour le fichier `control.json` :

```bash
azdata bdc config replace --config-file custom/control.json --json-values "$.spec.storage.data.className=<yourStorageClassName>"
```

Une autre option consiste à modifier manuellement le fichier de configuration personnalisé ou à utiliser le correctif json qui modifie la classe de stockage pour le pool de stockage, comme dans l’exemple suivant :

```json
{
  "patch": [
    {
      "op": "replace",
      "path": "$.spec.resources.storage-0.spec",
      "value": {
        "type":"Storage",
        "replicas":2,
        "storage":{
            "data":{
                    "size": "100Gi",
                    "className": "myStorageClass",
                    "accessMode":"ReadWriteOnce"
                    },
            "logs":{
                    "size":"32Gi",
                    "className":"myStorageClass",
                    "accessMode":"ReadWriteOnce"
                    }
                }
            }
        }
    ]
}
```

Appliquez le fichier de correctif. Utilisez la commande `azdata bdc config patch` pour appliquer les modifications dans le fichier de correctif .json. L’exemple suivant applique le fichier `patch.json` à un fichier de configuration de déploiement cible `custom.json` :

```bash
azdata bdc config patch --config-file custom/bdc.json --patch-file ./patch.json
```

## <a name="next-steps"></a>Étapes suivantes

- Pour obtenir une documentation complète sur les volumes dans Kubernetes, consultez la [documentation de Kubernetes sur les volumes](https://kubernetes.io/docs/concepts/storage/volumes/).

- Pour plus d’informations sur le déploiement d’un cluster Big Data SQL Server, consultez [Guide pratique pour déployer des clusters Big Data SQL Server sur Kubernetes](deployment-guidance.md).
