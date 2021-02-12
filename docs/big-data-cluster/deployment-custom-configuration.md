---
title: Configurer des déploiements
titleSuffix: SQL Server big data clusters
description: Découvrez comment personnaliser un déploiement de cluster Big Data à l’aide de fichiers de configuration intégrés à l’outil de gestion azdata.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mihaelab
ms.date: 06/22/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: d983b4d0d7cfb02a587675984fdc42c54bf9f0ec
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100047199"
---
# <a name="configure-deployment-settings-for-cluster-resources-and-services"></a>Configurer les paramètres de déploiement de services et ressources de cluster

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

En commençant avec un ensemble prédéfini de profils de configuration intégrés à l’outil de gestion [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)], vous pouvez facilement modifier les paramètres par défaut pour mieux répondre à vos exigences de charge de travail BDC. La structure des fichiers de configuration vous permet de mettre à jour précisément les paramètres de chaque service de la ressource.

Regardez cette vidéo de 13 minutes pour obtenir une vue d’ensemble de la configuration de cluster Big Data :

> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/Big-Data-Cluster-Configuration/player?WT.mc_id=dataexposed-c9-niner]

> [!TIP]
> Reportez-vous aux articles sur la façon de configurer la **haute disponibilité** pour des composants critiques tels que le [nœud maître SQL Server](deployment-high-availability.md) ou le [nœud de nom HDFS](deployment-high-availability-hdfs-spark.md), afin d’obtenir des détails sur la façon de déployer des services hautement disponibles.

Vous pouvez également définir des configurations au niveau des ressources ou mettre à jour les configurations de tous les services dans une ressource. Voici un résumé de la structure de `bdc.json` :

```json
{
    "apiVersion": "v1",
    "metadata": {
        "kind": "BigDataCluster",
        "name": "mssql-cluster"
    },
    "spec": {
        "resources": {
            "nmnode-0": {...
            },
            "sparkhead": {...
            },
            "zookeeper": {...
            },
            "gateway": {...
            },
            "appproxy": {...
            },
            "master": {...
            },
            "compute-0": {...
            },
            "data-0": {...
            },
            "storage-0": {...
        },
        "services": {
            "sql": {
                "resources": [
                    "master",
                    "compute-0",
                    "data-0",
                    "storage-0"
                ]
            },
            "hdfs": {
                "resources": [
                    "nmnode-0",
                    "zookeeper",
                    "storage-0",
                    "sparkhead"
                ],
                "settings": {...
            },
            "spark": {
                "resources": [
                    "sparkhead",
                    "storage-0"
                ],
                "settings": {...
            }
        }
    }
}
```

Pour mettre à jour les configurations au niveau des ressources comme les instances d’un pool, vous devez mettre à jour les spécifications de la ressource. Par exemple, pour mettre à jour le nombre d’instances dans le pool de calcul, vous allez modifier cette section dans le fichier de configuration `bdc.json` :

```json
"resources": {
    ...
    "compute-0": {
        "metadata": {
            "kind": "Pool",
            "name": "default"
        },
        "spec": {
            "type": "Compute",
            "replicas": 4
        }
    }
    ...
}
``` 

De même, pour modifier les paramètres d’un service individuel dans une ressource spécifique. Par exemple, si vous souhaitez modifier les paramètres de mémoire Spark uniquement pour le composant Spark dans le pool de stockage, vous devez mettre à jour la ressource `storage-0` avec une section `settings` pour le service `spark` dans le fichier de configuration `bdc.json`.

```json
"resources":{
    ...
     "storage-0": {
        "metadata": {
            "kind": "Pool",
            "name": "default"
        },
        "spec": {
            "type": "Storage",
            "replicas": 2,
            "settings": {
                "spark": {
                    "spark-defaults-conf.spark.driver.memory": "2g",
                    "spark-defaults-conf.spark.driver.cores": "1",
                    "spark-defaults-conf.spark.executor.instances": "3",
                    "spark-defaults-conf.spark.executor.memory": "1536m",
                    "spark-defaults-conf.spark.executor.cores": "1",
                    "yarn-site.yarn.nodemanager.resource.memory-mb": "18432",
                    "yarn-site.yarn.nodemanager.resource.cpu-vcores": "6",
                    "yarn-site.yarn.scheduler.maximum-allocation-mb": "18432",
                    "yarn-site.yarn.scheduler.maximum-allocation-vcores": "6",
                    "yarn-site.yarn.scheduler.capacity.maximum-am-resource-percent": "0.3"
                }
            }
        }
    }
    ...
}
```

Si vous souhaitez appliquer les mêmes configurations pour un service associé à plusieurs ressources, vous devez mettre à jour la section `settings` correspondante dans la section `services`. Par exemple, si vous souhaitez définir les mêmes paramètres pour Spark dans le pool de stockage et les pools Spark, vous devez mettre à jour la section `settings` dans la section de service `spark` dans le fichier de configuration `bdc.json`.

```json
"services": {
    ...
    "spark": {
        "resources": [
            "sparkhead",
            "storage-0"
        ],
        "settings": {
            "spark-defaults-conf.spark.driver.memory": "2g",
            "spark-defaults-conf.spark.driver.cores": "1",
            "spark-defaults-conf.spark.executor.instances": "3",
            "spark-defaults-conf.spark.executor.memory": "1536m",
            "spark-defaults-conf.spark.executor.cores": "1",
            "yarn-site.yarn.nodemanager.resource.memory-mb": "18432",
            "yarn-site.yarn.nodemanager.resource.cpu-vcores": "6",
            "yarn-site.yarn.scheduler.maximum-allocation-mb": "18432",
            "yarn-site.yarn.scheduler.maximum-allocation-vcores": "6",
            "yarn-site.yarn.scheduler.capacity.maximum-am-resource-percent": "0.3"
        }
    }
    ...
}
```

Pour personnaliser les fichiers de configuration de vos déploiement de clusters, vous pouvez utiliser n’importe quel éditeur pour le format JSON, comme VSCode. Pour générer un script avec ces modifications à des fins d’automatisation, utilisez la commande `azdata bdc config`. Cet article explique comment configurer des déploiements de clusters Big Data en modifiant les fichiers de configuration du déploiement. Il fournit des exemples de modification de la configuration pour différents scénarios. Pour plus d’informations sur la façon dont les fichiers de configuration sont utilisés dans les déploiements, consultez le [guide de déploiement](deployment-guidance.md#configfile).

## <a name="prerequisites"></a>Prérequis

- [Installer azdata](../azdata/install/deploy-install-azdata.md).

- Chacun des exemples de cette section suppose que vous avez créé une copie de l’une des configurations standard. Pour plus d’informations, consultez [Créer une configuration personnalisée](deployment-guidance.md#customconfig). Par exemple, la commande suivante crée un répertoire appelé `custom-bdc`, qui contient deux fichiers de configuration de déploiement JSON, `bdc.json` et `control.json`, basés sur la configuration d’`aks-dev-test` par défaut :

   ```bash
   azdata bdc config init --source aks-dev-test --target custom-bdc
   ```

## <a name="change-default-docker-registry-repository-and-images-tag"></a><a id="docker"></a> Modifier le registre, le référentiel et l’étiquette d’images Docker par défaut

Les fichiers de configuration intégrés, notamment control.json, incluent une section `docker` dans laquelle le registre de conteneurs, le référentiel et l’étiquette d’images sont préremplis. Par défaut, les images requises pour les clusters Big Data se trouvent dans le registre de conteneurs Microsoft (`mcr.microsoft.com`), dans le référentiel `mssql/bdc` :

```json
{
    "apiVersion": "v1",
    "metadata": {
        "kind": "Cluster",
        "name": "mssql-cluster"
    },
    "spec": {
        "docker": {
            "registry": "mcr.microsoft.com",
            "repository": "mssql/bdc",
            "imageTag": "2019-GDR1-ubuntu-16.04",
            "imagePullPolicy": "Always"
        },
        ...
    }
}
```

Avant le déploiement, vous pouvez personnaliser les paramètres `docker` en modifiant directement le fichier de configuration `control.json` ou en utilisant des commandes `azdata bdc config`. Par exemple, les commandes suivantes mettent à jour un fichier de configuration control.json `custom-bdc` avec d’autres `<registry>`, `<repository>` et `<image_tag>` :

```bash
azdata bdc config replace -c custom-bdc/control.json -j "$.spec.docker.registry=<registry>"
azdata bdc config replace -c custom-bdc/control.json -j "$.spec.docker.repository=<repository>"
azdata bdc config replace -c custom-bdc/control.json -j "$.spec.docker.imageTag=<image_tag>"
```

> [!TIP]
> En guise de bonne pratique, vous devez utiliser une étiquette d’image spécifique à la version et éviter d’utiliser l’étiquette d’image `latest`, car elle peut entraîner des incompatibilités de versions qui provoqueront des problèmes d’intégrité de cluster.

> [!TIP]
> Le déploiement de clusters Big Data doit avoir accès au registre de conteneurs et au référentiel à partir desquels tirer (pull) des images de conteneur. Si votre environnement n’a pas accès au registre de conteneurs Microsoft par défaut, vous pouvez effectuer une installation hors connexion où les images requises sont d’abord placées dans un référentiel Docker privé. Pour plus d’informations sur les installations hors connexion, consultez [Effectuer un déploiement hors connexion d’un cluster Big Data SQL Server](deploy-offline.md). Notez que vous devez définir les [variables d’environnement](deployment-guidance.md#env) `DOCKER_USERNAME` et `DOCKER_PASSWORD` avant d’émettre le déploiement pour vous assurer que le flux de travail de déploiement a accès à votre référentiel privé à partir duquel tirer (pull) les images.

## <a name="change-cluster-name"></a><a id="clustername"></a> Changer le nom du cluster

Le nom du cluster est à la fois le nom du cluster Big Data et celui de l’espace de noms Kubernetes qui sera créé lors du déploiement. Il est spécifié dans la partie suivante du fichier de configuration de déploiement `bdc.json` :

```json
"metadata": {
    "kind": "BigDataCluster",
    "name": "mssql-cluster"
},
```

La commande suivante envoie une paire clé-valeur au paramètre `--json-values` pour changer le nom du cluster Big Data en `test-cluster` :

```bash
azdata bdc config replace --config-file custom-bdc/bdc.json --json-values "metadata.name=test-cluster"
```

> [!IMPORTANT]
> Le nom de votre cluster Big Data ne doit contenir que des caractères alphanumériques minuscules et aucun espace. Tous les artefacts Kubernetes (conteneurs, pods, ensembles avec état, services) pour le cluster sont créés dans un espace de noms portant le même nom que le nom de cluster spécifié.

## <a name="update-endpoint-ports"></a><a id="ports"></a> Mettre à jour les ports d’un point de terminaison

Les points de terminaison sont définis pour le contrôleur dans `control.json`, et pour la passerelle et l’instance principale SQL Server, dans les sections correspondantes de `bdc.json`. La partie suivante du fichier de configuration `control.json` montre les définitions de point de terminaison pour le contrôleur :

```json
{
  "endpoints": [
    {
      "name": "Controller",
      "serviceType": "LoadBalancer",
      "port": 30080
    },
    {
      "name": "ServiceProxy",
      "serviceType": "LoadBalancer",
      "port": 30777
    }
  ]
}
```

L’exemple suivant utilise du code JSON inclus pour changer le port du point de terminaison `controller` :

```bash
azdata bdc config replace --config-file custom-bdc/control.json --json-values "$.spec.endpoints[?(@.name==""Controller"")].port=30000"
```

## <a name="configure-scale"></a><a id="replicas"></a> Configurer la mise à l’échelle

Les configurations de chaque ressource, telle que le pool de stockage, sont définies dans le fichier de configuration `bdc.json`. Par exemple, la partie suivante du fichier`bdc.json` illustre une définition de ressource `storage-0` :

```json
"storage-0": {
    "metadata": {
        "kind": "Pool",
        "name": "default"
    },
    "spec": {
        "type": "Storage",
        "replicas": 2,
        "settings": {
            "spark": {
                "spark-defaults-conf.spark.driver.memory": "2g",
                "spark-defaults-conf.spark.driver.cores": "1",
                "spark-defaults-conf.spark.executor.instances": "3",
                "spark-defaults-conf.spark.executor.memory": "1536m",
                "spark-defaults-conf.spark.executor.cores": "1",
                "yarn-site.yarn.nodemanager.resource.memory-mb": "18432",
                "yarn-site.yarn.nodemanager.resource.cpu-vcores": "6",
                "yarn-site.yarn.scheduler.maximum-allocation-mb": "18432",
                "yarn-site.yarn.scheduler.maximum-allocation-vcores": "6",
                "yarn-site.yarn.scheduler.capacity.maximum-am-resource-percent": "0.3"
            }
        }
    }
}
```

Vous pouvez configurer le nombre d’instances dans un pool de stockage, de calcul et/ou de données en modifiant la valeur de `replicas` pour chaque pool. L’exemple suivant utilise du code JSON inclus pour remplacer ces valeurs pour les pools de stockage, de calcul et de données respectivement par `10`, `4` et `4` :

```bash
azdata bdc config replace --config-file custom-bdc/bdc.json --json-values "$.spec.resources.storage-0.spec.replicas=10"
azdata bdc config replace --config-file custom-bdc/bdc.json --json-values "$.spec.resources.compute-0.spec.replicas=4"
azdata bdc config replace --config-file custom-bdc/bdc.json --json-values "$.spec.resources.data-0.spec.replicas=4"
```

> [!NOTE]
> Le nombre maximal d’instances validées pour les pools de calcul et de données est de `8` pour chacun. Il n’y a pas d’application de cette limite au moment du déploiement, mais nous vous déconseillons de configurer une mise à l’échelle plus élevée dans les déploiements de production.

## <a name="configure-storage"></a><a id="storage"></a> Configurer le stockage

Vous pouvez également changer la classe et les caractéristiques du stockage utilisées pour chaque pool. L’exemple suivant affecte une classe de stockage personnalisée aux pools de stockage et de données, et met à jour la taille de la revendication de volume persistant pour stocker jusqu’à 500 Go de données pour HDFS (pool de stockage) et jusqu’à 100 Go pour le maître et le pool de données. 

> [!TIP]
> Pour plus d’informations sur la configuration du stockage, consultez [Persistance des données avec un cluster Big Data SQL Server sur Kubernetes](concept-data-persistence.md).

Commencez par créer un fichier patch.json comme indiqué ci-dessous pour ajuster les paramètres de *stockage*

```json
{
        "patch": [
                {
                        "op": "add",
                        "path": "spec.resources.storage-0.spec.storage",
                        "value": {
                                "data": {
                                        "size": "500Gi",
                                        "className": "default",
                                        "accessMode": "ReadWriteOnce"
                                },
                                "logs": {
                                        "size": "30Gi",
                                        "className": "default",
                                        "accessMode": "ReadWriteOnce"
                                }
                        }
                },
        {
                        "op": "add",
                        "path": "spec.resources.master.spec.storage",
                        "value": {
                                "data": {
                                        "size": "100Gi",
                                        "className": "default",
                                        "accessMode": "ReadWriteOnce"
                                },
                                "logs": {
                                        "size": "30Gi",
                                        "className": "default",
                                        "accessMode": "ReadWriteOnce"
                                }
                        }
                },
                {
                        "op": "add",
                        "path": "spec.resources.data-0.spec.storage",
                        "value": {
                                "data": {
                                        "size": "100Gi",
                                        "className": "default",
                                        "accessMode": "ReadWriteOnce"
                                },
                                "logs": {
                                        "size": "30Gi",
                                        "className": "default",
                                        "accessMode": "ReadWriteOnce"
                                }
                        }
                }
        ]
}

```

Vous pouvez ensuite utiliser la commande `azdata bdc config patch` pour mettre à jour le fichier de configuration `bdc.json`.
```bash
azdata bdc config patch --config-file custom-bdc/bdc.json --patch ./patch.json
```

> [!NOTE]
> Un fichier de configuration basé sur `kubeadm-dev-test` n’a pas de définition de stockage pour chaque pool, mais vous pouvez utiliser le processus ci-dessus pour l’ajouter si nécessaire.

## <a name="configure-storage-pool-without-spark"></a><a id="sparkstorage"></a> Configurer un pool de stockage sans Spark

Vous pouvez également configurer les pools de stockage pour qu’ils s’exécutent sans Spark et créer un pool Spark distinct. Cette configuration vous permet de mettre à l’échelle la puissance de calcul Spark indépendamment du stockage. Pour savoir comment configurer le pool Spark, consultez la section [Créer un pool Spark](#sparkpool) dans cet article.

> [!NOTE]
> Le déploiement d’un cluster Big Data sans Spark n’est pas pris en charge. Par conséquent, vous devez avoir défini `includeSpark` sur `true` ou vous devez créer un [pool Spark](#sparkpool) distinct avec au moins une instance. Vous pouvez également exécuter Spark dans le pool de stockage (`includeSpark` a pour valeur `true`) et disposer d’un pool Spark distinct.

Par défaut, le paramètre `includeSpark` de la ressource de pool de stockage est défini sur true : vous devez donc modifier le champ `includeSpark` dans la configuration de stockage pour apporter des modifications. La commande suivante montre comment modifier cette valeur à l’aide de code JSON inclus.

```bash
azdata bdc config replace --config-file custom-bdc/bdc.json --json-values "$.spec.resources.storage-0.spec.settings.spark.includeSpark=false"
```

## <a name="create-a-spark-pool"></a><a id="sparkpool"></a> Créer un pool Spark

Vous pouvez créer un pool Spark en plus ou à la place des instances Spark en cours d’exécution dans le pool de stockage. L’exemple suivant montre comment créer un pool Spark avec deux instances en corrigeant le fichier de configuration `bdc.json`. 

Tout d’abord, créez un fichier `spark-pool-patch.json` comme indiqué ci-dessous :

```json
{
    "patch": [
        {
            "op": "add",
            "path": "spec.resources.spark-0",
            "value": {
                "metadata": {
                    "kind": "Pool",
                    "name": "default"
                },
                "spec": {
                    "type": "Spark",
                    "replicas": 2
                }
            }
        },
        {
            "op": "add",
            "path": "spec.services.spark.resources/-",
            "value": "spark-0"
        },
        {
            "op": "add",
            "path": "spec.services.hdfs.resources/-",
            "value": "spark-0"
        }
    ]
}
```

Ensuite, exécutez la commande `azdata bdc config patch` :

```bash
azdata bdc config patch -c custom-bdc/bdc.json -p spark-pool-patch.json
```

## <a name="configure-pod-placement-using-kubernetes-labels"></a><a id="podplacement"></a> Configurer le placement des pods avec des étiquettes Kubernetes

Vous pouvez contrôler le placement des pods sur les nœuds Kubernetes qui ont des ressources spécifiques pour prendre en charge différents types d’exigences de charge de travail. À l’aide des étiquettes Kubernetes, vous pouvez personnaliser les nœuds de votre cluster Kubernetes qui seront utilisés pour le déploiement des ressources de cluster Big Data, mais également limiter les nœuds utilisés pour des ressources spécifiques.
Par exemple, vous pouvez vérifier que les pods de ressource de pool de stockage sont placés sur les nœuds qui ont le plus de stockage, tandis que les instances principales SQL Server sont placées sur les nœuds qui ont des ressources processeur et mémoire plus importantes. Dans ce cas, vous allez d’abord créer un cluster Kubernetes hétérogène avec différents types de matériels, puis [affecter les étiquettes de nœud](https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/) en conséquence. Au moment du déploiement du cluster Big Data, vous pouvez spécifier les mêmes étiquettes au niveau du cluster pour indiquer quels nœuds sont utilisés pour le cluster Big Data à l’aide de l’attribut `clusterLabel` dans le fichier `control.json`. Des étiquettes différentes seront ensuite utilisées pour la sélection élective au niveau du pool. Ces étiquettes peuvent être spécifiées dans les fichiers de configuration du déploiement du cluster Big Data à l’aide de l’attribut `nodeLabel`. Kubernetes affecte les pods sur les nœuds qui correspondent aux étiquettes spécifiées. Les clés d’étiquette spécifiques qui doivent être ajoutées aux nœuds du cluster Kubernetes sont `mssql-cluster` (pour indiquer les nœuds utilisés pour le cluster Big Data) et `mssql-resource` (pour indiquer les nœuds spécifiques sur lesquels les pods sont placés pour différentes ressources). Les valeurs de ces étiquettes peuvent être n’importe quelle chaîne de votre choix.

> [!NOTE]
> En raison de la nature des pods qui effectuent la collecte des métriques au niveau des nœuds, les pods `metricsdc` sont déployés sur tous les nœuds dotés de l’étiquette `mssql-cluster`, et `mssql-resource` ne s’applique pas à ces pods.

L’exemple suivant montre comment modifier un fichier de configuration personnalisé pour inclure une étiquette de nœud `bdc` pour l’ensemble du cluster Big Data, une étiquette `bdc-master` pour placer des pods d’instance principale SQL Server sur un nœud spécifique, `bdc-storage-pool` pour les ressources de pool de stockage, `bdc-compute-pool` pour les pods de pools de calcul et de données, et `bdc-shared` pour le reste des ressources. 

Commencez par étiqueter les nœuds Kubernetes :

```bash
kubectl label node <kubernetesNodeName1> mssql-cluster=bdc mssql-resource=bdc-shared --overwrite=true
kubectl label node <kubernetesNodeName2> mssql-cluster=bdc mssql-resource=bdc-master --overwrite=true
kubectl label node <kubernetesNodeName3> mssql-cluster=bdc mssql-resource=bdc-compute-pool --overwrite=true
kubectl label node <kubernetesNodeName4> mssql-cluster=bdc mssql-resource=bdc-compute-pool --overwrite=true
kubectl label node <kubernetesNodeName5> mssql-cluster=bdc mssql-resource=bdc-storage-pool --overwrite=true
kubectl label node <kubernetesNodeName6> mssql-cluster=bdc mssql-resource=bdc-storage-pool --overwrite=true
kubectl label node <kubernetesNodeName7> mssql-cluster=bdc mssql-resource=bdc-storage-pool --overwrite=true
kubectl label node <kubernetesNodeName8> mssql-cluster=bdc mssql-resource=bdc-storage-pool --overwrite=true
```

Ensuite, mettez à jour les fichiers de configuration de déploiement de cluster pour inclure les valeurs d’étiquette. Cet exemple suppose que vous personnalisez les fichiers de configuration dans un profil `custom-bdc`. Par défaut, il n’existe aucune clé `nodeLabel` ni `clusterLabel` dans les configurations intégrées. Vous devez donc modifier manuellement un fichier de configuration personnalisé ou utiliser les commandes `azdata bdc config add` pour effectuer les modifications nécessaires.

```bash
azdata bdc config add -c custom-bdc/control.json -j "$.spec.clusterLabel=bdc"
azdata bdc config add -c custom-bdc/control.json -j "$.spec.nodeLabel=bdc-shared"

azdata bdc config add -c custom-bdc/bdc.json -j "$.spec.resources.master.spec.nodeLabel=bdc-master"
azdata bdc config add -c custom-bdc/bdc.json -j "$.spec.resources.compute-0.spec.nodeLabel=bdc-compute-pool"
azdata bdc config add -c custom-bdc/bdc.json -j "$.spec.resources.data-0.spec.nodeLabel=bdc-compute-pool"
azdata bdc config add -c custom-bdc/bdc.json -j "$.spec.resources.storage-0.spec.nodeLabel=bdc-storage-pool"

# below can be omitted in which case we will take the node label default from the control.json
azdata bdc config add -c custom-bdc/bdc.json -j "$.spec.resources.nmnode-0.spec.nodeLabel=bdc-shared"
azdata bdc config add -c custom-bdc/bdc.json -j "$.spec.resources.sparkhead.spec.nodeLabel=bdc-shared"
azdata bdc config add -c custom-bdc/bdc.json -j "$.spec.resources.zookeeper.spec.nodeLabel=bdc-shared"
azdata bdc config add -c custom-bdc/bdc.json -j "$.spec.resources.gateway.spec.nodeLabel=bdc-shared"
azdata bdc config add -c custom-bdc/bdc.json -j "$.spec.resources.appproxy.spec.nodeLabel=bdc-shared"
```
>[!NOTE]
> La bonne pratique consiste à accorder au maître Kubernetes l’un des rôles BDC ci-dessus. Si vous envisagez d’attribuer de toutes façons ces rôles au nœud maître Kubernetes, vous devez [supprimer sa teinte ``master:NoSchedule``.](https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/) Sachez que cela peut surcharger le nœud maître et l’empêcher d’effectuer ses tâches de gestion Kubernetes sur des clusters de plus grande taille. Il est normal de voir des pods planifiés sur le maître dans tout déploiement : ils tolèrent déjà la teinte ``master:NoSchedule`` et sont principalement utilisés pour aider à gérer le cluster. 

## <a name="other-customizations-using-json-patch-files"></a><a id="jsonpatch"></a> Autres personnalisations utilisant les fichiers de correctif JSON

Les fichiers de correctif JSON configurent plusieurs paramètres à la fois. Pour plus d’informations sur les correctifs JSON, consultez [Correctifs JSON dans Python](https://github.com/stefankoegl/python-json-patch) et l’[évaluateur en ligne JSONPath](https://jsonpath.com/).

Les fichiers `patch.json` suivants effectuent les modifications suivantes :

- Mise à jour du port d’un point de terminaison unique dans `control.json`.

```json
{
  "patch": [
    {
      "op": "replace",
      "path": "$.spec.endpoints[?(@.name=='Controller')].port",
      "value": 30000
    }
  ]
}
```

- Mise à jour de tous les points de terminaison (`port` et `serviceType`) dans `control.json`.

```json
{
  "patch": [
    {
      "op": "replace",
      "path": "spec.endpoints",
      "value": [
        {
          "serviceType": "LoadBalancer",
          "port": 30001,
          "name": "Controller"
        },
        {
          "serviceType": "LoadBalancer",
          "port": 30778,
          "name": "ServiceProxy"
        }
      ]
    }
  ]
}
```

- Mise à jour des paramètres de stockage du contrôleur dans `control.json`. Ces paramètres s’appliquent à tous les composants du cluster, sauf s’ils sont remplacés au niveau du pool.

```json
{
  "patch": [
    {
      "op": "replace",
      "path": "spec.storage",
      "value": {
        "data": {
          "className": "managed-premium",
          "accessMode": "ReadWriteOnce",
          "size": "100Gi"
        },
        "logs": {
          "className": "managed-premium",
          "accessMode": "ReadWriteOnce",
          "size": "32Gi"
        }
      }
    }
  ]
}
```

- Mise à jour du nom de la classe de stockage dans `control.json`.

```json
{
  "patch": [
    {
      "op": "replace",
      "path": "spec.storage.data.className",
      "value": "managed-premium"
    }
  ]
}
```

- Mise à jour des paramètres de stockage du pool pour le pool de stockage dans `bdc.json`.

```json
{
  "patch": [
    {
      "op": "replace",
      "path": "spec.resources.storage-0.spec",
      "value": {
        "type": "Storage",
        "replicas": 2,
        "storage": {
          "data": {
            "size": "100Gi",
            "className": "myStorageClass",
            "accessMode": "ReadWriteOnce"
          },
          "logs": {
            "size": "32Gi",
            "className": "myStorageClass",
            "accessMode": "ReadWriteOnce"
          }
        }
      }
    }
  ]
}
```

- Mise à jour des paramètres Spark pour le pool de stockage dans `bdc.json`.

```json
{
  "patch": [
    {
      "op": "replace",
      "path": "spec.services.spark.settings",
      "value": {
            "spark-defaults-conf.spark.driver.memory": "2g",
            "spark-defaults-conf.spark.driver.cores": "1",
            "spark-defaults-conf.spark.executor.instances": "3",
            "spark-defaults-conf.spark.executor.memory": "1536m",
            "spark-defaults-conf.spark.executor.cores": "1",
            "yarn-site.yarn.nodemanager.resource.memory-mb": "18432",
            "yarn-site.yarn.nodemanager.resource.cpu-vcores": "6",
            "yarn-site.yarn.scheduler.maximum-allocation-mb": "18432",
            "yarn-site.yarn.scheduler.maximum-allocation-vcores": "6",
            "yarn-site.yarn.scheduler.capacity.maximum-am-resource-percent": "0.3"
      }
    }
  ]
}
```

> [!TIP]
> Pour plus d’informations sur la structure et les options de modification d’un fichier de configuration de déploiement, consultez [Informations de référence sur les fichiers de configuration de déploiement des clusters Big Data](reference-deployment-config.md).

Utilisez les commandes `azdata bdc config` pour appliquer les modifications dans le fichier de correctif JSON. L’exemple suivant applique le fichier `patch.json` à un fichier de configuration de déploiement cible `custom-bdc/bdc.json`.

```bash
azdata bdc config patch --config-file custom-bdc/bdc.json --patch-file ./patch.json
```

## <a name="disable-elasticsearch-to-run-in-privileged-mode"></a>Désactiver l’exécution en mode privilégié d’ElasticSearch

Par défaut, le conteneur ElasticSearch s’exécute en mode privilégié dans le cluster Big Data. Ce paramètre garantit qu’au moment de l’initialisation du conteneur, le conteneur dispose des autorisations suffisantes pour mettre à jour un paramètre sur l’hôte requis quand ElasticSearch traite une plus grande quantité de journaux. Vous trouverez plus d’informations à ce sujet dans [cet article](https://www.elastic.co/guide/en/elasticsearch/reference/current/vm-max-map-count.html). 

Pour désactiver l’exécution en mode privilégié du conteneur qui exécute ElasticSearch, vous devez mettre à jour la section `settings` dans le fichier `control.json` et affecter à `vm.max_map_count` la valeur `-1`. Voici un exemple illustrant à quoi cette section peut ressembler :

```json
{
    "apiVersion": "v1",
    "metadata": {...},
    "spec": {
        "docker": {...},
        "storage": {...},
        "endpoints": [...],
        "settings": {
            "ElasticSearch": {
                "vm.max_map_count": "-1"
            }
        }
    }
}
```

Vous pouvez modifier manuellement le `control.json` et ajouter la section ci-dessus dans `spec` ou vous pouvez créer un fichier correctifs `elasticsearch-patch.json` comme ci-dessous et utiliser [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] pour corriger le fichier `control.json` :

```json
{
  "patch": [
    {
      "op": "add",
      "path": "spec.settings",
      "value": {
            "ElasticSearch": {
                "vm.max_map_count": "-1"
        }
      }
    }
  ]
}
```

Exécutez cette commande pour corriger le fichier de configuration :

```bash
azdata bdc config patch --config-file custom-bdc/control.json --patch-file elasticsearch-patch.json
```

> [!IMPORTANT]
> Nous vous recommandons de mettre à jour manuellement le paramètre `max_map_count` sur chaque hôte dans le cluster Kubernetes, conformément aux instructions de [cet article](https://www.elastic.co/guide/en/elasticsearch/reference/current/vm-max-map-count.html).

## <a name="turn-pods-and-nodes-metrics-collection-onoff"></a>Activer/désactiver la collecte de métriques sur les pods et les nœuds

SQL Server 2019 CU5 a activé deux commutateurs de fonctionnalités pour contrôler la collecte de métriques sur les pods et les nœuds. Si vous utilisez d’autres solutions pour superviser votre infrastructure Kubernetes, vous pouvez désactiver la collecte de métriques intégrée pour les pods et les nœuds hôtes en définissant *allowNodeMetricsCollection* et *allowPodMetricsCollection* sur *false* dans le fichier de configuration de déploiement *control.json*. Pour les environnements OpenShift, ces paramètres sont définis sur *false* par défaut dans les profils de déploiement intégrés, puisque la collecte des métriques sur les pods et les nœuds requiert des fonctionnalités privilégiées.
Exécutez cette commande pour mettre à jour les valeurs de ces paramètres dans votre fichier de configuration personnalisé à l’aide de l’interface CLI *azdata* :

```bash
 azdata bdc config replace -c custom-bdc/control.json -j "$.security.allowNodeMetricsCollection=false"
 azdata bdc config replace -c custom-bdc/control.json -j "$.security.allowPodMetricsCollection=false"
 ```

## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations sur l’utilisation de fichiers de configuration dans le déploiement de clusters Big Data, consultez [Guide pratique pour déployer des [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] sur Kubernetes](deployment-guidance.md#configfile).