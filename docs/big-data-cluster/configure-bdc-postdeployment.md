---
title: Vue d’ensemble de la configuration post-déploiement de clusters Big Data SQL Server
titleSuffix: SQL Server big data clusters
description: Vue d’ensemble de la configuration post-déploiement de clusters Big Data
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: rahul.ajmera
ms.date: 02/11/2021
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 478ecc9888bbd3c8f51ee96c6c796856472f93d5
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/13/2021
ms.locfileid: "100343912"
---
# <a name="how-to-configure-bdc-settings-post-deployment"></a>Guide pratique pour configurer les paramètres des clusters Big Data après le déploiement

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

> [!NOTE]
> La configuration des paramètres après le déploiement est uniquement disponible dans les déploiements de clusters Big Data CU9 et versions ultérieures. La configuration des paramètres n’inclut pas celle de l’échelle, du stockage ou du point de terminaison. Vous trouverez les options et instructions de configuration d’un cluster Big Data avant l’application de CU9 [ici](configure-bdc-pre-configuration.md).

Les paramètres du cluster, du service et de l’étendue des ressources pour les clusters Big Data peuvent être configurés après le déploiement par le biais de l’interface CLI azdata. Cette fonctionnalité permet aux administrateurs de clusters Big Data d’ajuster les configurations pour toujours satisfaire aux exigences de charge de travail. Cet article passe en revue des exemples d’étapes à suivre pour configurer un cluster Big Data selon les exigences de charge de travail Spark. La fonctionnalité de configuration après le déploiement suit un flux Set, Diff, Apply.

## <a name="step-by-step-configure-bdc-to-meet-your-spark-workload-requirements"></a>Étape par étape : configurer un cluster Big Data pour satisfaire aux exigences de charge de travail Spark

### <a name="view-the-current-configurations-of-the-big-data-cluster-spark-service"></a>Afficher les configurations actuelles du service Spark du cluster Big Data
L’exemple suivant montre comment afficher les paramètres configurés par l’utilisateur du service Spark. Vous pouvez afficher tous les paramètres configurables, les paramètres gérés par le système et tous les paramètres configurables, ainsi que les paramètres en attente par le biais de paramètres facultatifs. Pour plus d’informations, consultez [Instruction `azdata bdc spark`](../azdata/reference/reference-azdata-bdc-spark-statement.md).

```bash
azdata bdc spark settings show
```
#### <a name="sample-output"></a>Exemple de sortie
Service Spark 

|Paramètre|Valeur d'exécution|
| --- | --- |
|`spark-defaults-conf.spark.driver.cores`|`1` |
|`spark-defaults-conf.spark.driver.memory`|`1664m` |

### <a name="change-the-default-number-of-cores-and-memory-for-the-spark-driver-across-all-resources-with-spark-ie-for-the-spark-service"></a>Modifiez le nombre de cœurs et la mémoire par défaut pour le pilote Spark sur toutes les ressources avec Spark (c’est-à-dire, pour le service Spark).
Mettez à jour le nombre de cœurs par défaut en le définissant sur 2 et la mémoire par défaut en la définissant sur 7424m pour le service Spark.

```bash
azdata bdc spark settings set spark-defaults-conf.spark.driver.cores=2, spark-defaults-conf.spark.driver.memory=7424m
```

### <a name="change-the-default-number-of-cores-and-memory-for-the-spark-executors-in-the-storage-pool"></a>Modifier le nombre de cœurs et la mémoire par défaut pour les exécuteurs Spark dans le pool de stockage
Mettez à jour le nombre de cœurs Exécuteur par défaut en le définissant sur 4 pour le pool de stockage.

```bash
azdata bdc spark settings set spark-defaults-conf.spark.executor.cores=4 --resource=storage-0
```

### <a name="view-the-pending-settings-changes-staged-in-the-bdc"></a>Afficher les modifications de paramètres en attente indexées dans le cluster Big Data
Affichez les modifications de paramètres en attente pour le service Spark uniquement et dans l’ensemble du cluster Big Data.

#### <a name="pending-spark-service-settings"></a>Paramètres du service Spark en attente
```bash
azdata bdc spark settings show --filter-option=pending --include-details
```

### <a name="spark-service"></a>Service Spark

|Paramètre|Valeur d'exécution|Valeur configurée|Configurable|Configuré |Heure de la dernière mise à jour|
| --- | --- | --- | --- | --- | --- |
|`spark-defaults-conf.spark.driver.cores`|`1`| `2` | `true` | `true` |
|`spark-defaults-conf.spark.driver.memory`|`1664m`| `7424m` | `true` | `true` |

#### <a name="all-pending-settings"></a>Tous les paramètres en attente
```bash
azdata bdc settings show --filter-option=pending --include-details --recursive
```

Paramètres du service Spark - En attente

|Paramètre|Valeur d'exécution|Valeur configurée|Configurable|Configuré|Heure de la dernière mise à jour|
| --- | --- | --- | --- | --- | --- |
|`spark-defaults-conf.spark.driver.cores`|`1`| `2` | `true` | `true` |
|`spark-defaults-conf.spark.driver.memory`|`1664m`| `7424m` | `true` | `true` |

Paramètres Spark des ressources Stockage-0 - En attente

|Paramètre|Valeur d'exécution|Valeur configurée|Configurable|Configuré|Heure de la dernière mise à jour|
| --- | --- | --- | --- | --- | --- |
|`spark-defaults-conf.spark.executor.cores`|`1`| `4` | `true` | `true` |

### <a name="apply-the-pending-settings-to-the-bdc"></a>Appliquer les paramètres en attente au cluster Big Data

```bash
azdata bdc settings apply
```

### <a name="monitor-the-status-of-the-bdc-configuration-update"></a>Superviser l’état de la mise à jour de la configuration du cluster Big Data

```bash
azdata bdc status show -all
```

## <a name="optional-steps"></a>Étapes facultatives

### <a name="revert-pending-configuration-settings"></a>Rétablir les paramètres de configuration en attente

Si vous déterminez que vous n’avez plus besoin de modifier les paramètres de configuration en attente, vous pouvez annuler l’indexation de ces paramètres. Les paramètres en attente sont alors rétablis dans toutes les étendues.

```bash
azdata bdc settings revert
```

### <a name="abort-the-configuration-upgrade"></a>Abandonner la mise à niveau de la configuration

Si la mise à niveau de la configuration échoue pour l’un des composants, vous pouvez annuler le processus de mise à niveau et rétablir les configurations précédentes du cluster Big Data. Les paramètres indexés pour modification pendant la mise à niveau seront de nouveau listés en tant que paramètres en attente.

```bash
azdata bdc settings cancel-apply
```

## <a name="next-steps"></a>Étapes suivantes

[Configurer un cluster Big Data SQL Server](configure-bdc-overview.md)