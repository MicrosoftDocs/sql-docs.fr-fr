---
title: Configuration des clusters Big Data SQL Server avant CU9
titleSuffix: SQL Server big data clusters
description: Configuration des clusters Big Data avant CU9
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: rahul.ajmera
ms.date: 02/11/2021
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: f2adf48a0aef465f4c013b5adaf97a75be4abf77
ms.sourcegitcommit: 765262cdc6352a5325148afc22fa4f1499fe1aa3
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/09/2021
ms.locfileid: "102514866"
---
# <a name="configure-a-sql-server-big-data-cluster---pre-cu9-release"></a>Configurer un cluster Big Data SQL Server - Version antérieure à CU9

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

> [!NOTE]
> Avant la version CU9 et la prise en charge des clusters avec configuration activée, les clusters Big Data pouvaient être configurés uniquement au moment du déploiement, à l’exception de l’instance maître de SQL Server, qui pouvait être configurée après le déploiement uniquement à l’aide de mssql-conf. Vous trouverez les instructions permettant de configurer une version CU9 et ultérieure d’un cluster Big Data [ici](configure-bdc-overview.md).


Dans les versions CU8 et antérieures des clusters Big Data, vous pouvez configurer les paramètres au moment du déploiement par le biais du fichier `bdc.json` de déploiement. L’instance maître de SQL Server peut être configurée après le déploiement à l’aide de mssql-conf uniquement.

## <a name="configuration-scopes"></a>Étendues de configuration
La configuration des clusters Big Data de version antérieure à CU9 offre deux niveaux d’étendue : `service` et `resource`. La hiérarchie des paramètres suit également cet ordre, de la plus élevée à la plus faible. Les composants Clusters Big Data prennent la valeur du paramètre défini avec la portée la plus faible. Un paramètre dont la portée n’est pas définie hérite de la valeur de la portée parente supérieure.

Vous pouvez, par exemple, définir le nombre de cœurs par défaut utilisé par le pilote Spark dans le pool de stockage et les ressources `Sparkhead`. Il existe deux méthodes pour le faire :

* Spécifier une valeur de cœurs par défaut à la portée du service `Spark` 
* Spécifier une valeur de cœurs par défaut à la portée des ressources `storage-0` et `sparkhead`

Dans le premier scénario, toutes les ressources du service Spark dont l’étendue est inférieure (pool de stockage et `Sparkhead`) *héritent* du nombre de cœurs par défaut défini par la valeur par défaut du service Spark.

Dans le second scénario, chaque ressource emploie la valeur définie comme sa propre portée.

Si le nombre de cœurs par défaut est configuré à la fois à la portée du service et à celle de la ressource, la valeur de portée ressource écrase celle de portée service, car il s’agit de la plus petite portée **configurée par l’utilisateur** pour le paramètre.

Pour obtenir des informations spécifiques sur la configuration, consultez les articles appropriés :

## <a name="configure-the-sql-server-master-instance"></a>Configurer l’instance maître de SQL Server
Configurez l’instance maître des [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)].

Impossible de configurer les paramètres de configuration du serveur pour l’instance maître SQL Server au moment du déploiement. Cet article décrit une solution de contournement temporaire sur la manière de configurer des paramètres tels que l’édition SQL Server, d’activer ou de désactiver SQL Server Agent, d’activer des indicateurs de trace spécifiques ou d’activer/de désactiver les commentaires des clients.

Pour modifier l’un de ces paramètres, procédez comme suit :

1. Créez un fichier `mssql-custom.conf` personnalisé qui comprend les paramètres ciblés. L’exemple suivant active le service SQL Agent, la télémétrie, définit un PID pour Édition Entreprise et active l’indicateur de trace 1204. :

   ```
   [sqlagent]
   enabled=true
   
   [telemetry]
   customerfeedback=true
   userRequestedLocalAuditDirectory = /tmp/audit

   [DEFAULT]
   pid = Enterprise

   [traceflag]
   traceflag0 = 1204
   ```

1. Copiez le fichier `mssql-custom.conf` dans `/var/opt/mssql` dans le conteneur `mssql-server` du pod `master-0`. Remplacez `<namespaceName>` par le nom du cluster Big Data.

   ```bash
   kubectl cp mssql-custom.conf master-0:/var/opt/mssql/mssql-custom.conf -c mssql-server -n <namespaceName>
   ```

1. Redémarrez l’instance SQL Server.  Remplacez `<namespaceName>` par le nom du cluster Big Data.

   ```bash
   kubectl exec -it master-0  -c mssql-server -n <namespaceName> -- /bin/bash
   supervisorctl restart mssql-server
   exit
   ```

> [!IMPORTANT]
> Si l’instance maître SQL Server est dans une configuration de groupes de disponibilité, copiez le fichier `mssql-custom.conf` dans toutes les pods `master`. Notez que chaque redémarrage entraînant un basculement, vous devez veiller à planifier cette activité pendant les périodes d’inactivité.

### <a name="known-limitations"></a>Limitations connues

- Les étapes ci-dessus nécessitent des autorisations d’administrateur de cluster Kubernetes
- Vous ne pouvez pas modifier le classement du serveur pour l’instance maître SQL Server du cluster Big Data après le déploiement.

## <a name="configure-apache-spark-and-apache-hadoop"></a>Configurer Apache Spark et Apache Hadoop
Pour configurer Apache Spark et Apache Hadoop dans des clusters Big Data, vous devez modifier le profil des clusters au moment du déploiement.

Un cluster Big Data comporte quatre catégories de configuration : 

- `sql` 
- `hdfs` 
- `spark` 
- `gateway` 

`sql`, `hdfs`, `spark` et `sql` sont des services. À chacun d’eux est associée une catégorie de configuration du même nom. Toutes les configurations de passerelle sont affectées à la catégorie `gateway`. 

Par exemple, toutes les configurations dans le service `hdfs` appartiennent à la catégorie `hdfs`. Notez que toutes les configurations Hadoop (core-site), HDFS et ZooKeeper appartiennent à la catégorie `hdfs`, et toutes les configurations Livy, Spark, YARN, Hive et Metastore à la catégorie `spark`. 

La page [Configurations prises en charge](reference-config-spark-hadoop.md) répertorie les propriétés Apache Spark & Hadoop que vous pouvez configurer lorsque vous déployez un cluster Big Data SQL Server.

Les sections suivantes répertorient les propriétés que vous **ne pouvez pas** modifier dans un cluster :

- [Configurations `spark` non prises en charge](reference-config-spark-hadoop.md#unsupported-spark-configurations)
- [Configurations `hdfs` non prises en charge](reference-config-spark-hadoop.md#unsupported-hdfs-configurations)
- [Configurations `gateway` non prises en charge](reference-config-spark-hadoop.md#unsupported-gateway-configurations)

## <a name="next-steps"></a>Étapes suivantes

[Configurer un cluster Big Data SQL Server](configure-bdc-overview.md)