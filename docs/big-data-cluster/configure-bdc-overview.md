---
title: Vue d’ensemble de la configuration des clusters Big Data SQL Server
titleSuffix: SQL Server big data clusters
description: Vue d’ensemble de la configuration des clusters Big Data
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: rahul.ajmera
ms.date: 02/11/2021
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 8cda6e61e8f5f13f5fd414879f888c7ed72a1bd0
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/13/2021
ms.locfileid: "100343981"
---
# <a name="configure-a-sql-server-big-data-cluster"></a>Configurer un cluster Big Data SQL Server

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]
La gestion de la configuration permet aux administrateurs de vérifier que leur cluster Big Data est toujours préparé à leurs besoins de charge de travail. Grâce à cette fonctionnalité, les administrateurs de clusters peuvent modifier ou ajuster diverses parties du cluster Big Data au moment du déploiement ou après le déploiement afin d’obtenir des insights plus approfondis sur les configurations en cours d’exécution dans leur cluster Big Data. 

La gestion de la configuration permet à un administrateur d’activer l’agent SQL, de définir les ressources de base de référence pour les travaux Spark de leur organisation, voire même de voir quels paramètres sont configurables au niveau de chaque étendue. Au moment du déploiement, les configurations peuvent être définies par le biais du fichier `bdc.json` du déploiement, et après le déploiement, par le biais de l’interface CLI azdata.

## <a name="configuration-scopes"></a>Étendues de configuration
La configuration des clusters Big Data porte sur trois niveaux d’étendue : `cluster`, `service` et `resource`. La hiérarchie des paramètres suit également cet ordre, de la plus élevée à la plus faible. Les composants Clusters Big Data prennent la valeur du paramètre défini avec la portée la plus faible. Un paramètre dont la portée n’est pas définie hérite de la valeur de la portée parente supérieure.

Vous pouvez, par exemple, définir le nombre de cœurs par défaut utilisé par le pilote Spark dans le pool de stockage et les ressources `Sparkhead`. Pour définir le nombre de cœurs par défaut, vous pouvez effectuer l’une des actions suivantes :

- Spécifier une valeur de cœurs par défaut à la portée du service `Spark`

- Spécifier une valeur de cœurs par défaut à la portée des ressources `storage-0` et `sparkhead`

Dans le premier scénario, toutes les ressources du service Spark dont l’étendue est inférieure (pool de stockage et `Sparkhead`) *héritent* du nombre de cœurs par défaut défini par la valeur par défaut du service Spark.

Dans le second scénario, chaque ressource emploie la valeur définie comme sa propre portée.

Si le nombre de cœurs par défaut est configuré à la fois à la portée du service et à celle de la ressource, la valeur de portée ressource écrase celle de portée service, car il s’agit de la plus petite portée **configurée par l’utilisateur** pour le paramètre.

## <a name="next-steps"></a>Étapes suivantes

Pour obtenir des informations spécifiques sur la configuration, consultez les articles appropriés :

- [Personnaliser le déploiement de clusters Big Data](deployment-custom-configuration.md)
- [Configurer des clusters Big Data après le déploiement](configure-bdc-postdeployment.md)
- [Configurer des clusters Big Data - Version CU8 et antérieure](configure-bdc-pre-configuration.md)

Référence : 
- [Propriétés de configuration des clusters Big Data SQL Server](reference-config-bdc-overview.md)
- [Propriétés de configuration d’Apache Spark et Apache Hadoop (HDFS)](reference-config-spark-hadoop.md)
- [Propriétés de configuration de l’instance maître SQL Server - Version antérieure à CU9](reference-config-master-instance.md)
- [azdata CLI](../azdata/reference/reference-azdata.md)