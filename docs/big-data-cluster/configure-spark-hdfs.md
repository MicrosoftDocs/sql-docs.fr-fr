---
title: Apache Spark et Apache Hadoop
titleSuffix: Configure Apache Spark and Apache Hadoop in Big Data Clusters
description: Les clusters Big Data SQL Server prennent en charge les solutions Spark et HDFS. Découvrez comment les configurer.
author: rajmera3
ms.author: raajmera
ms.reviewer: mikeray
ms.date: 08/04/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 2fa2b5a3e9b7f996866ae1fca16c32ff1e1ba39e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100039153"
---
# <a name="configure-apache-spark-and-apache-hadoop-in-big-data-clusters"></a>Configurer Apache Spark et Apache Hadoop dans les clusters Big Data

Pour configurer Apache Spark et Apache Hadoop dans des clusters Big Data, vous devez modifier le profil des clusters au moment du déploiement.

Un cluster Big Data comporte quatre catégories de configuration : 

- `sql` 
- `hdfs` 
- `spark` 
- `gateway` 

`sql`, `hdfs`, `spark` et `sql` sont des services. À chacun d’eux est associée une catégorie de configuration du même nom. Toutes les configurations de passerelle sont affectées à la catégorie `gateway`. 

Par exemple, toutes les configurations dans le service `hdfs` appartiennent à la catégorie `hdfs`. Notez que toutes les configurations Hadoop (core-site), HDFS et ZooKeeper appartiennent à la catégorie `hdfs`, et toutes les configurations Livy, Spark, YARN, Hive et Metastore à la catégorie `spark`. 

La page [Configurations prises en charge](reference-config-spark-hadoop.md#supported-configurations) répertorie les propriétés Apache Spark & Hadoop que vous pouvez configurer lorsque vous déployez un cluster Big Data SQL Server.

Les sections suivantes répertorient les propriétés que vous **ne pouvez pas** modifier dans un cluster :

- [Configurations `spark` non prises en charge](reference-config-spark-hadoop.md#unsupported-spark-configurations)
- [Configurations `hdfs` non prises en charge](reference-config-spark-hadoop.md#unsupported-hdfs-configurations)
- [Configurations `gateway` non prises en charge](reference-config-spark-hadoop.md#unsupported-gateway-configurations)


## <a name="configurations-via-cluster-profile"></a>Configurations par profil de cluster

Le profil du cluster comporte des ressources et des services. Au moment du déploiement, nous pouvons spécifier les configurations de deux manières : 

* Premièrement, au niveau de la ressource : 

   Les exemples suivants sont les fichiers de correctif pour le profil : 

   ```json
   { 
         "op": "add", 
         "path": "spec.resources.zookeeper.spec.settings", 
         "value": { 
           "hdfs": { 
             "zoo-cfg.syncLimit": "6" 
           } 
         } 
   }
   ```

   Ou : 

   ```json
   { 
         "op": "add", 
         "path": "spec.resources.gateway.spec.settings", 
         "value": { 
           "gateway": { 
             "gateway-site.gateway.httpclient.socketTimeout": "95s" 
           } 
         } 
   } 
   ```

* Deuxièmement, au niveau du service. Attribuez plusieurs ressources à un service et spécifiez les configurations pour le service.

Voici un exemple de fichier correctif pour le profil pour la définition de la taille de bloc HDFS : 

   ```json
   { 
         "op": "add", 
         "path": "spec.services.hdfs.settings", 
         "value": { 
           "hdfs-site.dfs.block.size": "268435456" 
        } 
   } 
   ```

Le service `hdfs` est défini comme suit :

```json
{ 
  "spec": { 
   "services": { 
     "hdfs": { 
        "resources": [ 
          "nmnode-0", 
          "zookeeper", 
          "storage-0", 
          "sparkhead" 
        ], 
        "settings":{ 
          "hdfs-site.dfs.block.size": "268435456" 
        } 
      } 
    } 
  } 
} 
```
 
> [!NOTE]
> Les configurations au niveau de la ressource remplacent les configurations au niveau du service. Une ressource peut être attribuée à plusieurs services.

## <a name="enable-spark-in-the-storage-pool"></a>Activer Spark dans le pool de stockage
Outre les configurations Apache prises en charge, vous pouvez également autoriser ou non l’exécution de travaux Spark dans le pool de stockage. Cette valeur booléenne, `includeSpark`, se trouve dans le fichier de configuration `bdc.json` à l’emplacement `spec.resources.storage-0.spec.settings.spark`.

Un exemple de définition de pool de stockage dans bdc.json peut se présenter comme suit :
```json
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
                            "includeSpark": "true"
                        }
                    }
                }
            }
```


## <a name="limitations"></a>Limites

Les configurations peuvent être spécifiées au niveau de la catégorie uniquement. Pour spécifier plusieurs configurations avec la même sous-catégorie, nous ne pouvons pas extraire le préfixe commun dans le profil du cluster. 

```json
{ 
      "op": "add", 
      "path": "spec.services.hdfs.settings.core-site.hadoop", 
      "value": { 
        “proxyuser.xyz.users”: “*”, 
        “proxyuser.abc.users”: “*” 
     } 
} 
```

## <a name="next-steps"></a>Étapes suivantes

- [Propriétés de configuration d’Apache Spark et Apache Hadoop (HDFS).](reference-config-spark-hadoop.md)
- [Référence [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]](../azdata/reference/reference-azdata.md)
- [Que sont les [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)] ?](big-data-cluster-overview.md)