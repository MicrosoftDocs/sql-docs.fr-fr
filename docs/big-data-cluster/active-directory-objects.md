---
title: Objets Active Directory
titleSuffix: SQL Server Big Data Cluster
description: Décrit les objets Active Directory créés pour Clusters Big Data SQL Server.
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.date: 09/30/2020
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 347129c6627bce865ffd4962a9cf2cb3b0beb862
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100039780"
---
# <a name="auto-generated-active-directory-objects"></a>Objets Active Directory générés automatiquement

Cet article décrit les groupes et les comptes Active Directory (AD) créés par SQL Server lors d’un déploiement de cluster Big Data.

## <a name="accounts--groups"></a>Comptes et groupes

Les comptes et les groupes d’utilisateurs sont générés dans [l’unité d’organisation (UO)](/windows-server/identity/ad-ds/plan/reviewing-ou-design-concepts) fournie lors du déploiement du cluster.

Chacun des comptes représente un service dans Clusters Big Data. Ils possèdent les noms de principal du service (SPN) requis par chaque service. La commande [setspn](https://social.technet.microsoft.com/wiki/contents/articles/717.service-principal-names-spn-setspn-syntax.aspx) permet de lister les noms SPN appartenant aux différents comptes.

Le déploiement génère automatiquement des noms de comptes et de groupes. À partir de SQL Server 2019 CU5, le préfixe du nom de compte ou de groupe correspond au nom de l’espace de noms de déploiement (nom du cluster Big Data). Si le nom du cluster est `bdc` pour les éléments de cet article, remplacez `<prefix>` par `bdc` pour identifier vos comptes.

Le suffixe de pod (-x) désigne un ID de pod de variable ci-dessous. Les noms ci-dessous ne comportent pas de préfixe de variable fourni par l’utilisateur lors du déploiement.

Le nom de compte classique s’applique aux déploiements utilisant des versions antérieures à SQL Server 2019 CU5, ainsi qu’aux déploiements effectués avec l’option « useSubdomain » définie sur false dans la configuration de la sécurité.

| Nom du compte ou du groupe|Informations complémentaires|
|----|---|
|`<prefix>-ctrl`|[Compte de service du contrôleur](#controller-service-account)|
|`<prefix>-ngxm`|[Compte de service proxy du service de supervision](#monitoring-service-proxy-service-account)|
|`<prefix>-ldap`|[Utilisateur de recherche LDAP](#ldap-lookup-user)|
|`<prefix>-sqc0-x/<prefix>-sqc0`|[Utilisateur SQL Server du pool de calcul](#compute-pool-sql-server-user)|
|`<prefix>-dmc0-x`|[Utilisateur Data Warehouse DMS du pool de calcul](#compute-pool-data-warehouse-dms-user)|
|`<prefix>-dec0-x`|[Utilisateur Data Warehouse Engine du pool de calcul](#compute-pool-data-warehouse-engine-user)|
|`<prefix>-sqd0`|[Utilisateur SQL Server du pool de données](#data-pool-sql-server-user)|
|`<prefix>-sqs0`|[Utilisateur SQL Server du pool de stockage](#storage-pool-sql-server-user)|
|`<prefix>-ynt0-x`|[Utilisateur du service de gestionnaire de nœuds YARN du pool de stockage](#storage-pool-yarn-node-manager-service-user)|
|`<prefix>-htt0`|[Utilisateur du service HTTP du pool de stockage](#storage-pool-http-service-user)|
|`<prefix>-hdt0`|[Utilisateur du service de nœuds de données HDFS du pool de stockage](#storage-pool-hdfs-datanode-service-user)|
|`<prefix>-hdnn`|[Utilisateur du service de nœuds de nom HDFS](#hdfs-name-node-service-user)|
|`<prefix>-htnn`|[Utilisateur du service HTTP de nœuds de nom HDFS](#hdfs-name-node-http-service-user)|
|`<prefix>-kmnn-x`|[Utilisateur du service KMS de nœuds de nom](#name-node-kms-service-user)|
|`<prefix>-jnzk-x`|[Utilisateurs du service ZooKeeper JournalNode](#zookeeper-journalnode-service-users)|
|`<prefix>-htzk`|[Utilisateur du service ZooKeeper HTTP](#zookeeper-http-service-user)|
|`<prefix>-yrsh-x`|[Utilisateur du service de gestionnaire des ressources Sparkhead YARN](#sparkhead-yarn-resource-manager-service-user)|
|`<prefix>-htsh`|[Utilisateur Sparkhead HTTP](#sparkhead-http-user)|
|`<prefix>-shsh-x`|[Utilisateur du service d’historique Sparkhead Spark](#sparkhead-spark-history-service-user)|
|`<prefix>-lvsh-x`|[Utilisateur du service Sparkhead Livy](#sparkhead-livy-service-user)|
|`<prefix>-hvsh-x`|[Utilisateur du service Sparkhead Hive](#sparkhead-hive-service-user)|
|`<prefix>-yns0-x`|[Utilisateur du service de gestionnaire de nœuds YARN du pool Spark](#spark-pool-yarn-node-manager-service-user)|
|`<prefix>-hts0`|[Utilisateur HTTP du gestionnaire de nœuds YARN du pool Spark](#spark-pool-yarn-node-manager-http-user)|
|`<prefix>-knox-x`|[Utilisateur de la passerelle Knox](#knox-gateway-user)|
|`<prefix>-htgw`|[Utilisateur HTTP de la passerelle Knox](#knox-gateway-http-user)|
|`<prefix>-apst`|[Utilisateur du programme d’installation d’applications](#app-setup-user)|
|`<prefix>-dmsvc`|[Groupe de services Data Warehouse DMS](#data-warehouse-dms-service-group)|
|`<prefix>-desvc`|[Groupe de services Data Warehouse Engine](#data-warehouse-engine-service-group)|

La section suivante donne des informations complémentaires sur les différents comptes. Pour plus d’informations sur les groupes, passez à [Groupes](#groups).

## <a name="controller-management--ldap-related-accounts"></a>Comptes liés au contrôleur, à la gestion et au protocole LDAP

### <a name="controller-service-account"></a>Compte de service du contrôleur

|Object|Nom du compte|
|---|---|
|Nom du groupe identique|`control`|
|Nom du pod|`control-x`|
|Nom du conteneur|`controller`|
|Nom du service|`controller`|
|Nom du compte (sans préfixe)| `ctrl`|
|Compte (avec préfixe d’espace de noms)|`<prefix>-ctrl`|
|Nom du compte classique|`ctrl-controller`|

### <a name="monitoring-service-proxy-service-account"></a>Compte de service proxy du service de supervision

|Object|Nom du compte|
|---|---|
|Nom du groupe identique|`mgmtproxy`|
|Nom du pod|`mgmtproxy-x`|
|Nom du conteneur|`service-proxy`|
|Nom du service|`nginx`|
|Compte (sans préfixe)| `ngxm`|
|Compte (avec préfixe d’espace de noms)|`<prefix>-ngxm`|
|Nom du compte classique|`nginx-mgmtproxy`|

### <a name="ldap-lookup-user"></a>Utilisateur de recherche LDAP

Utilisé par les services Grafana et Hadoop pour rechercher des utilisateurs par le biais du protocole LDAP.

|Object|Nom du compte|
|---|---|
|Nom du groupe identique|`metricsui`|
|Nom du pod|`metricsui-x`|
|Nom du conteneur|`grafana`|
|Nom du service|`grafana`|
|Nom du compte (sans préfixe)| `ldap`|
|Nom du compte (avec préfixe d’espace de noms)|`<prefix>-ldap`|
|Nom du compte classique|`ldap-user`|

## <a name="master-pool-accounts"></a>Comptes du pool principal

### <a name="master-pool-sql-server-user"></a>Utilisateur SQL Server du pool principal

|Object|Nom du compte|
|---|---|
|Nom du groupe identique|`master`|
|Nom du pod|`master-x`|
|Nom du conteneur|`mssql-server`|
|Nom du service|`mssql`|
|Nom du compte (sans préfixe)| `sqmp-x/sqmp`|
|Nom du compte (avec préfixe d’espace de noms)|`<prefix>-sqmp-x/<prefix>-sqmp`|
|Nom du compte classique|`mssql-master-x`|

### <a name="master-pool-data-warehouse-dms-user"></a>Utilisateur Data Warehouse DMS du pool principal

|Object|Nom du compte|
|---|---|
|Nom du groupe identique|`master`|
|Nom du pod|`master-x`|
|Nom du conteneur|`mssql-server`|
|Nom du service|`dwdms`|
|Compte (sans préfixe)| `dmmp-x`|
|Compte (avec préfixe d’espace de noms)|`<prefix>-dmmp-x`|
|Nom du compte classique|`dwdms-master-x`|

### <a name="master-pool-data-warehouse-engine-user"></a>Utilisateur Data Warehouse Engine du pool principal

|Object|Nom du compte|
|---|---|
|Nom du groupe identique|`master`|
|Nom du pod|`master-x`|
|Nom du conteneur|`mssql-server`|
|Nom du service|`dweng`|
|Compte (sans préfixe)| `demp`|
|Compte (avec préfixe d’espace de noms)|`<prefix>-demp-x`|
|Nom du compte classique|`dweng-master-x`|

## <a name="compute-pool-accounts"></a>Comptes du pool de calcul

### <a name="compute-pool-sql-server-user"></a>Utilisateur SQL Server du pool de calcul

|Object|Nom du compte|
|---|---|
|Nom du groupe identique|`compute-0`|
|Nom du pod|`compute-0-x`|
|Nom du conteneur|`mssql-server`|
|Nom du service|`mssql`|
|Compte (sans préfixe)| `sqc0-x/sqlc0`|
|Compte (avec préfixe d’espace de noms)|`<prefix>-sqc0-x/<prefix>-sqc0`|
|Nom du compte classique|`mssql-compute-0-x`|

### <a name="compute-pool-data-warehouse-dms-user"></a>Utilisateur Data Warehouse DMS du pool de calcul

|Object|Nom du compte|
|---|---|
|Nom du groupe identique|`compute-0`|
|Nom du pod|`compute-0-x`|
|Nom du conteneur|`mssql-server`|
|Nom du service|`dwdms`|
|Compte (sans préfixe)| `dmc0-x`|
|Compte (avec préfixe d’espace de noms)|`<prefix>-dmc0-x`|
|Nom du compte classique|`dwdms-compute-0-x`|

### <a name="compute-pool-data-warehouse-engine-user"></a>Utilisateur Data Warehouse Engine du pool de calcul

|Object|Nom du compte|
|---|---|
|Nom du groupe identique|`compute-0`|
|Nom du pod|`compute-0-x`|
|Nom du conteneur|`mssql-server`|
|Nom du service|`dweng`|
|Compte (sans préfixe)| `dec0-x`|
|Compte (avec préfixe d’espace de noms)|`<prefix>-dec0-x`|
|Nom du compte classique|`dweng-compute-0-x`|

## <a name="data-pool-accounts"></a>Comptes du pool de données

### <a name="data-pool-sql-server-user"></a>Utilisateur SQL Server du pool de données

|Object|Nom du compte|
|---|---|
|Nom du groupe identique|`data-0`|
|Nom du pod|`data-0-x`|
|Nom du conteneur|`mssql-server`|
|Nom du service|`mssql`|
|Compte (sans préfixe)| `sqd0`|
|Compte (avec préfixe d’espace de noms)|`<prefix>-sqd0`|
|Nom du compte classique|`mssql-data-0`|

## <a name="storage-pool-accounts"></a>Comptes du pool de stockage

### <a name="storage-pool-sql-server-user"></a>Utilisateur SQL Server du pool de stockage

|Object|Nom du compte|
|---|---|
|Nom du groupe identique|`storage-0`|
|Nom du pod|`storage-0-x`|
|Nom du conteneur|`mssql-server`|
|Nom du service|`mssql`|
|Compte (sans préfixe)| `sqs0`|
|Compte (avec préfixe d’espace de noms)|`<prefix>-sqs0`|
|Nom du compte classique|`mssql-storage-0`|

### <a name="storage-pool-yarn-node-manager-service-user"></a>Utilisateur du service de gestionnaire de nœuds YARN du pool de stockage

|Object|Nom du compte|
|---|---|
|Nom du groupe identique|`storage-0`|
|Nom du pod|`storage-0-x`|
|Nom du conteneur|`hadoop`|
|Nom du service|`Yarn Node Manager`|
|Compte (sans préfixe)| `ynt0-x`|
|Compte (avec préfixe d’espace de noms)|`<prefix>-ynt0-x`|
|Nom du compte classique|`yarnnm-storage-0-x`|

### <a name="storage-pool-http-service-user"></a>Utilisateur du service HTTP du pool de stockage

|Object|Nom du compte|
|---|---|
|Nom du groupe identique|`storage-0`|
|Nom du pod|`storage-0-x`|
|Nom du conteneur|`hadoop`|
|Nom du service|`HDFS Datanode`|
|Compte (sans préfixe)| `hdt0`|
|Compte (avec préfixe d’espace de noms)|`<prefix>-hdt0`|
|Nom du compte classique|`http-storage-0`|

### <a name="storage-pool-hdfs-datanode-service-user"></a>Utilisateur du service de nœuds de données HDFS du pool de stockage

|Object|Nom du compte|
|---|---|
|Nom du groupe identique|`storage-0`|
|Nom du pod|`storage-0-x`|
|Nom du conteneur|`hadoop`|
|Nom du service|`HDFS Datanode`|
|Compte (sans préfixe)| `hdt0`|
|Compte (avec préfixe d’espace de noms)|`<prefix>-hdt0`|
|Nom du compte classique|`hdfsdn-storage-0`|

## <a name="hdfs-accounts"></a>Comptes HDFS

### <a name="hdfs-name-node-service-user"></a>Utilisateur du service de nœuds de nom HDFS

|Object|Nom du compte|
|---|---|
|Nom du groupe identique|`nmnode-0`|
|Nom du pod|`nmnode-0-x`|
|Nom du conteneur|`hadoop`|
|Nom du service|`HDFS Namenode`|
|Compte (sans préfixe)| `hdnn`|
|Compte (avec préfixe d’espace de noms)|`<prefix>-hdnn`|
|Nom du compte classique|`hdfsnn-nmnode`|

### <a name="hdfs-name-node-http-service-user"></a>Utilisateur du service HTTP de nœuds de nom HDFS

|Object|Nom du compte|
|---|---|
|Nom du groupe identique|`nmnode-0`|
|Nom du pod|`nmnode-0-x`|
|Nom du conteneur|`hadoop`|
|Nom du service|`HDFS Namenode`|
|Compte (sans préfixe)| `htnn`|
|Compte (avec préfixe d’espace de noms)|`<prefix>-htnn`|
|Nom du compte classique|`http-nmnode`|

## <a name="kms-accounts"></a>Comptes KMS

### <a name="name-node-kms-service-user"></a>Utilisateur du service KMS de nœuds de nom

|Object|Nom du compte|
|---|---|
|Nom du groupe identique|`nmnode-0`|
|Nom du pod|`nmnode-0-x`|
|Nom du conteneur|`hadoop`|
|Nom du service|`KMS`|
|Compte (sans préfixe)| `kmnn-x`|
|Compte (avec préfixe d’espace de noms)|`<prefix>-kmnn-x`|
|Nom du compte classique|`kms-nmnode-x`|

## <a name="zookeeper-accounts"></a>Comptes ZooKeeper

### <a name="zookeeper-journalnode-service-users"></a>Utilisateurs du service ZooKeeper JournalNode

|Object|Nom du compte|
|---|---|
|Nom du groupe identique|`zookeeper`|
|Nom du pod|`zookeeper-x`|
|Nom du conteneur|`zookeeper`|
|Nom du service|`Journal node`|
|Compte (sans préfixe)| `jnzk-x`|
|Compte (avec préfixe d’espace de noms)|`<prefix>-jnzk-x`|
|Nom du compte classique|`jn-zookeeper-x`|

### <a name="zookeeper-http-service-user"></a>Utilisateur du service ZooKeeper HTTP

|Object|Nom du compte|
|---|---|
|Nom du groupe identique|`zookeeper`|
|Nom du pod|`zookeeper-x`|
|Nom du conteneur|`zookeeper`|
|Nom du service|`Zookeeper`|
|Compte (sans préfixe)| `htzk`|
|Compte (avec préfixe d’espace de noms)|`<prefix>-htzk`|
|Nom du compte classique|`http-zookeeper`|

## <a name="spark-related-accounts"></a>Comptes liés à Spark

### <a name="sparkhead-yarn-resource-manager-service-user"></a>Utilisateur du service de gestionnaire des ressources Sparkhead YARN

|Object|Nom du compte|
|---|---|
|Nom du groupe identique|`sparkhead`|
|Nom du pod|`sparkhead-x`|
|Nom du conteneur|`hadoop-yarn-jobhistory`|
|Nom du service|`Yarn Resource Manager`|
|Compte (sans préfixe)| `yrsh-x`|
|Compte (avec préfixe d’espace de noms)|`<prefix>-yrsh-x`|
|Nom du compte classique|`yarnrm-sparkhead-x`|

### <a name="sparkhead-http-user"></a>Utilisateur Sparkhead HTTP

|Object|Nom du compte|
|---|---|
|Nom du groupe identique|`sparkhead`|
|Nom du pod|`sparkhead-x`|
|Nom du conteneur|`*`|
|Nom du service|`*`|
|Compte (sans préfixe)| `htsh`|
|Compte (avec préfixe d’espace de noms)|`<prefix>-htsh`|
|Nom du compte classique|`http-sparkhead`|

### <a name="sparkhead-spark-history-service-user"></a>Utilisateur du service d’historique Sparkhead Spark

|Object|Nom du compte|
|---|---|
|Nom du groupe identique|`sparkhead`|
|Nom du pod|`sparkhead-x`|
|Nom du conteneur|`hadoop-livy-sparkhistory`|
|Nom du service|`Spark History Server`|
|Compte (sans préfixe)| `shsh-x`|
|Compte (avec préfixe d’espace de noms)|`<prefix>-shsh-x`|
|Nom du compte classique|`sph-sparkhead-x`|

### <a name="sparkhead-livy-service-user"></a>Utilisateur du service Sparkhead Livy

|Object|Nom du compte|
|---|---|
|Nom du groupe identique|`sparkhead`|
|Nom du pod|`sparkhead-x`|
|Nom du conteneur|`hadoop-livy-sparkhistory`|
|Nom du service|`Livy`|
|Compte (sans préfixe)| `lvsh-x`|
|Compte (avec préfixe d’espace de noms)|`<prefix>-lvsh-x`|
|Nom du compte classique|`livy-sparkhead-x`|

### <a name="sparkhead-hive-service-user"></a>Utilisateur du service Sparkhead Hive

|Object|Nom du compte|
|---|---|
|Nom du groupe identique|`sparkhead`|
|Nom du pod|`sparkhead-x`|
|Nom du conteneur|`hadoop-hivemetastore`|
|Nom du service|`Hive Metastore`|
|Compte (sans préfixe)| `hvsh-x`|
|Compte (avec préfixe d’espace de noms)|`<prefix>-hvsh-x`|
|Nom du compte classique|`hive-sparkhead-x`|

### <a name="spark-pool-yarn-node-manager-service-user"></a>Utilisateur du service de gestionnaire de nœuds YARN du pool Spark

|Object|Nom du compte|
|---|---|
|Nom du groupe identique|`spark-0`|
|Nom du pod|`spark-0-x`|
|Nom du conteneur|`hadoop`|
|Nom du service|`Yarn Node Manager`|
|Compte (sans préfixe)| `yns0-x`|
|Compte (avec préfixe d’espace de noms)|`<prefix>-yns0-x`|
|Nom du compte classique|`yarnnm-spark-0-x`|

### <a name="spark-pool-yarn-node-manager-http-user"></a>Utilisateur HTTP du gestionnaire de nœuds YARN du pool Spark

|Object|Nom du compte|
|---|---|
|Nom du groupe identique|`spark-0`|
|Nom du pod|`spark-0-x`|
|Nom du conteneur|`hadoop`|
|Nom du service|`Yarn Node Manager`|
|Compte (sans préfixe)| `hts0`|
|Compte (avec préfixe d’espace de noms)|`<prefix>-hts0`|
|Nom du compte classique|`http-spark-0`|

## <a name="knox-accounts"></a>Comptes Knox

### <a name="knox-gateway-user"></a>Utilisateur de la passerelle Knox

|Object|Nom du compte|
|---|---|
|Nom du groupe identique|`gateway`|
|Nom du pod|`gateway-x`|
|Nom du conteneur|`knox`|
|Nom du service|`Knox`|
|Compte (sans préfixe)| `knox-x`|
|Compte (avec préfixe d’espace de noms)|`<prefix>-knox-x`|
|Nom du compte classique|`knox-gateway-x`|

### <a name="knox-gateway-http-user"></a>Utilisateur HTTP de la passerelle Knox

|Object|Nom du compte|
|---|---|
|Nom du groupe identique|`gateway`|
|Nom du pod|`gateway-x`|
|Nom du conteneur|`knox`|
|Nom du service|`Knox`|
|Compte (sans préfixe)| `htgw`|
|Compte (avec préfixe d’espace de noms)|`<prefix>-htgw`|
|Nom du compte classique|`http-gateway`|

## <a name="app-accounts"></a>Comptes d’applications

### <a name="app-setup-user"></a>Utilisateur du programme d’installation d’applications

|Object|Nom du compte|
|---|---|
|Nom du groupe identique|`appproxy`|
|Nom du pod|`appproxy-x`|
|Nom du conteneur|`App Service Proxy`|
|Nom du service|`nginx`|
|Compte (sans préfixe)| `apst`|
|Compte (avec préfixe d’espace de noms)|`<prefix>-apst`|
|Nom du compte classique|`app-setup`|

## <a name="groups"></a>Groupes

Les groupes suivants sont créés dans l’UO fournie par l’utilisateur. Les membres des groupes sont les utilisateurs créés ci-dessus pour les services correspondants.

### <a name="data-warehouse-dms-service-group"></a>Groupe de services Data Warehouse DMS

|Object|Nom du groupe|
|---|---|
|Nom du groupe identique|`master/compute-0`|
|Nom du pod|`master-x/compute-0-x`|
|Nom du conteneur|`mssql-server`|
|Nom du service|`dwdms`|
|Groupe (sans préfixe)| `dmsvc`|
|Compte (avec préfixe d’espace de noms)|`<prefix>-dmsvc`|
|Nom du compte classique|`dwdms-service`|

### <a name="data-warehouse-engine-service-group"></a>Groupe de services Data Warehouse Engine

|Object|Nom du groupe|
|---|---|
|Nom du groupe identique|`master/compute-0`|
|Nom du pod|`master-x/compute-0-x`|
|Nom du conteneur|`mssql-server`|
|Nom du service|`dweng`|
|Groupe (sans préfixe)| `desvc`|
|Compte (avec préfixe d’espace de noms)|`<prefix>-desvc`|
|Nom du compte classique|`desvc`|

## <a name="next-steps"></a>Étapes suivantes

[Déploiement de [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] en mode Active Directory](active-directory-deploy.md)

[Déploiement de plusieurs [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] dans le même domaine Active Directory](active-directory-deployment-background.md)
