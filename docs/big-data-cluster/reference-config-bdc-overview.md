---
title: Propriétés de configuration des clusters Big Data SQL Server
titleSuffix: SQL Server big data clusters
description: Article de référence sur les propriétés de configuration des clusters Big Data
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: rahul.ajmera
ms.date: 02/11/2021
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: ecaba704d9c08619f42c5cdf8d726917ccc61b9c
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/13/2021
ms.locfileid: "100343541"
---
# <a name="sql-server-big-data-clusters-configuration-properties"></a>Propriétés de configuration des clusters Big Data SQL Server

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Les paramètres de configuration des clusters Big Data peuvent être définis au niveau des étendues suivantes : `cluster`, `service` et `resource`. La hiérarchie des paramètres suit également cet ordre, de la plus élevée à la plus faible. Les composants Clusters Big Data prennent la valeur du paramètre défini avec la portée la plus faible. Un paramètre dont la portée n’est pas définie hérite de la valeur de la portée parente supérieure. Voici la liste des paramètres disponibles pour chaque composant de cluster Big Data dans les différentes étendues. Vous pouvez également afficher les paramètres configurables pour votre cluster Big Data avec azdata.

## <a name="bdc-cluster-scope-settings"></a>Paramètres d’étendue du cluster Big Data
Vous pouvez configurer les paramètres suivants au niveau de l’étendue du cluster.

|Propriété|Options|
| --- | --- |
|`mssql.telemetry`|`customerfeedback = { true | false }` |
|`mssql.traceflag`|`traceflag<#> = <####>` |

## <a name="sql-service-scope-settings"></a>Paramètres d’étendue du service SQL
Vous pouvez configurer les paramètres suivants au niveau de l’étendue du service SQL.

|Propriété|Options|
| --- | --- |
|`mssql.language`|`lcid = <language_identifier>` |

## <a name="spark-service-scope-settings"></a>Paramètres d’étendue du service Spark
Consultez l’[article sur la configuration Apache Spark et Apache Hadoop](reference-config-spark-hadoop.md) pour voir tous les paramètres pris en charge et non pris en charge.

## <a name="hdfs-service-scope-settings"></a>Paramètres d’étendue du service HDFS
Consultez l’[article sur la configuration Apache Spark et Apache Hadoop](reference-config-spark-hadoop.md) pour voir tous les paramètres pris en charge et non pris en charge.

## <a name="gateway-service-scope-settings"></a>Paramètres d’étendue du service de passerelle
Aucun paramètre d’étendue du service de passerelle n’est configurable. Configurez les paramètres au niveau de l’étendue des ressources de la passerelle.

## <a name="app-service-scope-settings"></a>Paramètres d’étendue du service d’application
Aucun paramètre disponible.

## <a name="master-pool-resource-scope-settings"></a>Paramètres d’étendue des ressources du pool principal
|Propriété|Options|
| --- | --- |
|`mssql.sqlagent`|`enabled = { true | false }` |
|`mssql.licensing`|`pid = { Enterprise | Developer }` |
<!-- |`mssql.collation`|`x = <language_identifier>` | -->

> [!NOTE]
> La modification du classement par défaut d’une instance de SQL Server est une opération complexe. En plus de modifier le paramètre `mssql.collation`, vous devrez peut-être recréer vos bases de données utilisateur et tous les objets qu’elles contiennent. Pour obtenir des instructions sur la façon de procéder, consultez [Définir ou modifier le classement du serveur](../relational-databases/collations/set-or-change-the-server-collation.md#changing-the-server-collation-in-sql-server).

## <a name="storage-pool-resource-scope-settings"></a>Paramètres d’étendue des ressources du pool de stockage
Le pool de stockage est constitué de composants SQL, Spark et HDFS.

### <a name="available-sql-configurations"></a>Configurations SQL disponibles
|Propriété|Options|
| --- | --- |
|`mssql.degreeOfParallelism`| |
|`mssql.minServerMemory`| |
|`mssql.maxServerMemory`| |
|`mssql.network.tlscert`| |
|`mssql.network.tlskey`| |
|`mssql.numberOfCpus`| |
|`mssql.storagePoolCacheSize`| |
|`mssql.storagePoolMaxCacheSize`| |
|`mssql.storagePoolCacheAutogrowth`| |
|`mssql.tempdb.autogrowthPerDataFile`| |
|`mssql.tempdb.autogrowthPerLogFile`| |
|`mssql.tempdb.dataFileSize`| |
|`mssql.tempdb.dataFileMaxSize`| |
|`mssql.tempdb.logFileMaxSize`| |
|`mssql.tempdb.numberOfDataFiles`| |
|`mssql.traceflag`|`traceflag<#> = <####>` |


### <a name="available-apache-spark-and-hadoop-configurations"></a>Configurations Apache Spark et Hadoop disponibles
Consultez l’[article sur la configuration Apache Spark et Apache Hadoop](reference-config-spark-hadoop.md) pour voir tous les paramètres pris en charge et non pris en charge.

## <a name="data-pool-resource-scope-settings"></a>Paramètres d’étendue des ressources du pool de données
|Propriété|Options|
| --- | --- |
|`mssql.degreeOfParallelism`| |
|`mssql.minServerMemory`| |
|`mssql.maxServerMemory`| |
|`mssql.network.tlscert`| |
|`mssql.network.tlskey`| |
|`mssql.numberOfCpus`| |
|`mssql.tempdb.autogrowthPerDataFile`| |
|`mssql.tempdb.autogrowthPerLogFile`| |
|`mssql.tempdb.dataFileSize`| |
|`mssql.tempdb.dataFileMaxSize`| |
|`mssql.tempdb.logFileMaxSize`| |
|`mssql.tempdb.numberOfDataFiles`| |
|`mssql.traceflag`|`traceflag<#> = <####>` |

## <a name="compute-pool-resource-scope-settings"></a>Paramètres d’étendue des ressources du pool de calcul
|Propriété|Options|
| --- | --- |
|`mssql.degreeOfParallelism`| |
|`mssql.minServerMemory`| |
|`mssql.maxServerMemory`| |
|`mssql.network.tlscert`| |
|`mssql.network.tlskey`| |
|`mssql.numberOfCpus`| |
|`mssql.tempdb.autogrowthPerDataFile`| |
|`mssql.tempdb.autogrowthPerLogFile`| |
|`mssql.tempdb.dataFileSize`| |
|`mssql.tempdb.dataFileMaxSize`| |
|`mssql.tempdb.logFileMaxSize`| |
|`mssql.tempdb.numberOfDataFiles`| |
|`mssql.traceflag`|`traceflag<#> = <####>` |

## <a name="spark-pool-resource-scope-settings"></a>Paramètres d’étendue des ressources du pool Spark
Consultez l’[article sur la configuration Apache Spark et Apache Hadoop](reference-config-spark-hadoop.md) pour voir tous les paramètres pris en charge et non pris en charge.

## <a name="gateway-resource-scope-settings"></a>Paramètres d’étendue des ressources de passerelle
Consultez l’[article sur la configuration Apache Spark et Apache Hadoop](reference-config-spark-hadoop.md) pour voir tous les paramètres pris en charge et non pris en charge.

## <a name="sparkhead-resource-scope-settings"></a>Paramètres d’étendue des ressources `Sparkhead`
Consultez l’[article sur la configuration Apache Spark et Apache Hadoop](reference-config-spark-hadoop.md) pour voir tous les paramètres pris en charge et non pris en charge.

## <a name="zookeeper-resource-scope-settings"></a>Paramètres d’étendue des ressources Zookeeper
Consultez l’[article sur la configuration Apache Spark et Apache Hadoop](reference-config-spark-hadoop.md) pour voir tous les paramètres pris en charge et non pris en charge.

## <a name="namenode-resource-scope-settings"></a>Paramètres d’étendue des ressources Namenode
Consultez l’[article sur la configuration Apache Spark et Apache Hadoop](reference-config-spark-hadoop.md) pour voir tous les paramètres pris en charge et non pris en charge.

## <a name="app-proxy-resource-scope-settings"></a>Paramètres d’étendue des ressources de proxy d’application
Aucun paramètre disponible.

## <a name="next-steps"></a>Étapes suivantes

[Configurer des clusters Big Data SQL Server](configure-bdc-overview.md)