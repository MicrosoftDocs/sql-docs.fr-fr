---
title: Ressources déployées
titleSuffix: SQL Server Big Data Clusters
description: Description des pods généralement déployés dans un cluster Big Data SQL Server.
author: mihaelablendea
ms.author: mihaelab
ms.reviewer: mikeray
ms.date: 03/30/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 1df5bfe16ca1d64e3d748923796d521c95aacc78
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100039559"
---
# <a name="resources-deployed-with-big-data-cluster"></a>Ressources déployées avec un cluster Big Data

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Cet article décrit les ressources déployées par un cluster Big Data SQL Server.

Un cluster Big Data déploie des pods en fonction du profil de déploiement. Pour plus d’informations, consultez [Configurations par défaut](deployment-guidance.md#configfile). 

Cet article décrit les pods déployés avec un profil `aks-dev-test-ha` et comprend un pool Spark. Interrogez Kubernetes pour voir les pods déployés dans votre cluster. L’exemple suivant retourne une liste de pods sous un espace de noms spécifique.

```bash
kubectl get pods -n <namespace>
```

Remplacez `<namespace>` par le nom de votre cluster Big Data. 

Pour plus d’informations, consultez [Guide pratique pour déployer [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] sur Kubernetes](deployment-guidance.md#configfile).

Le diagramme suivant montre les composants déployés dans un cluster Big Data :

:::image type="content" source="media/big-data-cluster-overview/architecture-diagram-overview.png" alt-text="big-data-cluster-diagram":::

Pour plus d’informations sur l’architecture, consultez [Que sont les [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] ?](big-data-cluster-overview.md).

## <a name="deployed-pods"></a>Pods déployés

Le tableau suivant liste les pods déployés dans un cluster Big Data.

|Nom  |Domaine|
|---------|---------|
|`control-<nnnn>`        |[Contrôle](#control)|
|`controldb-<#>`         |[Contrôle](#control)|
|`controlwd-<nnnn>`      |[Contrôle](#control)|
|`logsdb-<#>`            |[Contrôle](#control)|
|`logsui-<nnnn>`         |[Contrôle](#control)|
|`metricsdb-<#>`         |[Contrôle](#control)|
|`metricsdc-<nnnn>`      |[Contrôle](#control)|
|`metricsui-<nnnn>`      |[Contrôle](#control)|
|`mgmtproxy-<nnnn>`      |[Contrôle](#control)|
|`zookeeper-<#>`         |[Contrôle](#control)|
|`dns-<nnnn>`            |[Contrôle](#control)|
|`master-<#n>`           |[Instance maître](#master-instance)|
|`operator-<nnnn>`       |[Instance maître](#master-instance)
|`compute-<#n>-<#m>`     |[Pool de calcul](#compute-pool)|
|`data-<#>-<#>`          |[Pool de données](#data-pool) |
|`storage-<#>-<#>`       |[Pool de stockage](#storage-pool)|
|`nmnode-<#>-<#>`        |[Pool de stockage](#storage-pool)|
|`sparkhead-<#>`         |[Pool de stockage](#storage-pool)|
|`appproxy-<#m>`         |[Pool d’applications](#application-pool)|
|`gateway-<#>`           |[Service de passerelle](#gateway-service)|

Tous les pods ne sont pas inclus dans chaque cluster BDC. Les déploiements avec une haute disponibilité ou une intégration à Active Directory incluent des pods spécifiques. 

### <a name="high-availability-specific-pods"></a>Pods propres à la haute disponibilité :

- `operator-<nnnn>`
- `zookeeper-<#>`

### <a name="active-directory-specific-pods"></a>Pod propres à Active Directory :

- `dns-<nnnn>`

Les sections suivantes décrivent les pods et listent les conteneurs dans chaque pod.

## <a name="control"></a>Control

Les pods de contrôle fournissent le service de contrôle.

|Nom du pod |Count| Type de contrôleur Kubernetes | Containers |
|--------|----|------|--------|-------|
|`control-#`|1| ReplicaSet |- `controller`<br><br>- `security-support`<br><br>- `fluentbit`
|`controldb`|1| StatefulSet |- `mssql-server`<br><br>- `fluentbit`
|`controlwd`|1|  ReplicaSet |- `controlwatchdog`
|`logsdb-#` |1| StatefulSet |- `elasticsearch`
|`logsui`   |1| ReplicaSet |- `kibana`
|`metricsdb-#`|1| StatefulSet |- `influxdb`
|`metricsdc`|1 par nœud Kubernetes. | DaemonSet |- `telegraf` |
|`metricsui-nnnn`|1| ReplicaSet |- `grafana` |
|`mgmtproxy-nnnn`|1| ReplicaSet |- `service-proxy`<br><br>- `fluentbit`|
|`dns-nnnn`|0 ou 1 pour l’intégration à Active Directory| ReplicaSet |- `dns`<br><br>- `fluentbit`|

## <a name="master-instance"></a>Instance principale

`master-<#n>` est l’instance maître SQL Server.

- Gère le pool de données via DDL
- Manipule les données dans le pool de données via DML
- Déplace l’exécution des requêtes analytiques vers le pool de données

|Nom du pod |Count| Type de contrôleur Kubernetes | Containers |
|--------|----|------|--------|
|`master-<#n>`|1 ou plus pour la haute disponibilité.| StatefulSet|- `mssql-server`<br><br>- `fluentbit`<br><br>- `collectd`<br><br>- `mssql-ha-supervisor` <sup>*</sup>|
|`operator`<sup>*</sup>| 0 ou 1 pour la haute disponibilité | ReplicaSet |- `mssql-ha-operator`

<sup>*</sup> Uniquement les déploiements à haute disponibilité. L’opérateur implémente et inscrit la définition de ressource personnalisée pour SQL Server et les ressources de groupe de disponibilité. Quand l’opérateur est déployé, il s’inscrit lui-même en tant qu’écouteur pour les notifications relatives aux ressources SQL Server déployées dans le cluster Kubernetes. `mssql-ha-supervisor` prend en charge le groupe de disponibilité.

Chaque pod `master` contient une instance de SQL Server. Un déploiement à haute disponibilité comprend trois pods. Chaque pod comprend une instance de SQL Server avec des bases de données dans un groupe de disponibilité Always On SQL Server.

Ajoutez des pods supplémentaires au moment du déploiement, en fonction de votre charge de travail. 

## <a name="compute-pool"></a>Pool de calcul

Le pool de calcul fournit une instance de SQL Server pour le calcul.

|Nom du pod |Count| Type de contrôleur Kubernetes | Containers |
|--------|----|------|--------|
|`compute-<#n>-<#m>`|1 ou plus.| StatefulSet |- `mssql-server`<br><br>- `fluentbit`<br><br>- `collectd`

- `#n` identifie le pool de calcul.
- `#m` identifie l’ID d’instance dans le pool.

Les instances du pool de calcul SQL Server sont sans état. Elles nécessitent uniquement le stockage pour `tempdb`.

Ajoutez des pods supplémentaires au moment du déploiement, en fonction de votre charge de travail. 

## <a name="data-pool"></a>Pool de données

Le pool de données fournit des instances de SQL Server pour le stockage et le calcul.

|Nom du pod |Count| Type de contrôleur Kubernetes | Containers |
|--------|----|------|--------|-------|
|`data-<#n>-<#m>` | 0 ou plus | StatefulSet | -` mssql-server` <br><br>- `fluentbit`<br><br>- `collectd`|

- `#n` identifie le pool de données.
- `#m` identifie l’ID d’instance dans le pool.

Ajoutez des pods supplémentaires au moment du déploiement, en fonction de la charge de travail.

## <a name="storage-pool"></a>Pool de stockage

Le pool de stockage fournit l’ingestion des données par le biais de Spark, le stockage dans HDFS, l’accès aux données par le biais de HDFS et des points de terminaison SQL Server.

|Nom du pod |Count| Type de contrôleur Kubernetes | Containers |
|--------|----|------|--------|
|`storage-0-#`|1 ou plus. Ajoutez des pods supplémentaires au moment du déploiement, en fonction de la charge de travail. | StatefulSet |- `hadoop`<br><br>- `mssql-server`<br><br>- `fluentbit`<br><br>
|`nmnode-0-#`|1 ou plus pour la haute disponibilité| StatefulSet |- `hadoop`<br><br>- `fluentbit`
|`sparkehead-#`|1 ou plus pour la haute disponibilité| StatefulSet |- `hadoop-yarn-jobhistory`<br><br>- `hadoop-livy-sparkhistory`<br><br>- `hadoop-hivemetastore`<br><br>-- `fluentbit`
|`zookeeper`|0 ou 3 pour la haute disponibilité. | StatefulSet |- `zookeeper`<br><br>- `fluentbit`

## <a name="application-pool"></a>Pool d'applications

Le pool d’applications est inclus dans certains profils de configuration de test. Le pool d’applications héberge des proxys de service d’application que vous définissez quand vous déployez vos applications pour des clusters Big Data. 

`appproxy` est une API web qui se trouve devant les applications du pool d’applications. Elle authentifie les utilisateurs, puis route les requêtes vers les applications.

|Nom du pod | Type de contrôleur Kubernetes | Containers  |
|--------|----|------|
|`appproxy`| ReplicaSet |- `app-service-proxy`<br><br>- `fluentbit`

Pour plus d’informations, consultez [Présentation du déploiement d’application sur un cluster Big Data](concept-application-deployment.md).

Ajoutez des pods supplémentaires au moment du déploiement, en fonction de la charge de travail. 

## <a name="gateway-service"></a>Service de passerelle

Les services de passerelle fournissent la passerelle Knox à Spark, HDFS, [Yarn](https://hadoop.apache.org/docs/current/hadoop-yarn/hadoop-yarn-site/YARN.html), l’interface utilisateur Yarn et l’interface utilisateur Spark.

|Nom du pod | Type de contrôleur Kubernetes | Containers |
|--------|-----|-----|
|`gateway-<#>`| StatefulSet |- `knox`<br><br>- `fluentbit`

Une seule passerelle est prise en charge.

## <a name="open-source-container-references"></a>Références de conteneur open source

Certains conteneurs sont développés par des projets open source. Pour plus d’informations sur les conteneurs open source utilisés, consultez :

- [Elasticsearch](https://www.elastic.co/)
- [Kibana](https://www.elastic.co/kibana)
- [InfluxDB](https://www.influxdata.com)
- [Grafana](https://grafana.com/)
- [Fluent Bit](https://docs.fluentbit.io/manual/about/what-is-fluent-bit)
- [HDFS DataNode](concept-storage-pool.md)
- [HDFS NameNode](https://cwiki.apache.org/confluence/display/HADOOP2/NameNode) 
- [Spark](configure-spark-hdfs.md)
- [ZooKeeper](https://kubernetes.io/docs/tutorials/stateful-application/zookeeper/) 


## <a name="next-steps"></a>Étapes suivantes

Pour en savoir plus sur les [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)], consultez les ressources suivantes :

- [Que sont les [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)] ?](big-data-cluster-overview.md)
- [Atelier : Architecture des [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] Microsoft](https://github.com/Microsoft/sqlworkshops/tree/master/sqlserver2019bigdataclusters)
- [Guide pratique pour déployer des [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] sur Kubernetes](deployment-guidance.md#configfile)
