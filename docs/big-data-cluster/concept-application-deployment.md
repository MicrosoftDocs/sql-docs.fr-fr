---
title: Présentation du déploiement d’application
titleSuffix: SQL Server Big Data Clusters
description: Découvrez comment le déploiement d’applications fournit des interfaces pour créer, gérer et exécuter des applications sur un cluster Big Data SQL Server 2019.
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.metadata: seo-lt-2019
ms.date: 06/22/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 8c1f6b4895e872289095411cfd8ecdaf1e2eb087
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100047649"
---
# <a name="what-is-application-deployment-on-a-sql-server-big-data-cluster"></a>Présentation du déploiement d’applications sur un cluster Big Data SQL Server

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Le déploiement d’applications sur un cluster Big Data SQL Server s’effectue au moyen d’interfaces conçues pour créer, gérer et exécuter des applications. Les applications déployées sur un cluster Big Data SQL Server bénéficient de la puissance de calcul du cluster et peuvent accéder aux données disponibles sur le cluster. Vous augmentez ainsi la scalabilité et les performances des applications tout en gérant les applications où résident les données. Les runtimes d’application pris en charge sur les [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] sont R, Python, SSIS et MLeap.

Les sections suivantes décrivent l’architecture et les fonctionnalités du déploiement d’application.

## <a name="application-deployment-architecture"></a>Architecture du déploiement d’application

Le déploiement d’application passe par un contrôleur et des gestionnaires d’exécution d’application. Quand vous créez une application, un fichier de spécification (`spec.yaml`) est fourni. Ce fichier `spec.yaml` contient tout ce que le contrôleur doit savoir pour déployer correctement l’application. Voici un exemple du contenu de `spec.yaml` :

```yaml
#spec.yaml
name: add-app #name of your python script
version: v1  #version of the app
runtime: Python #the language this app uses (R or Python)
src: ./add.py #full path to the location of the app
entrypoint: add #the function that will be called upon execution
replicas: 1  #number of replicas needed
poolsize: 1  #the pool size that you need your app to scale
inputs:  #input parameters that the app expects and the type
  x: int
  y: int
output: #output parameter the app expects and the type
  result: int
```

Le contrôleur inspecte le `runtime` spécifié dans le fichier `spec.yaml` et appelle le gestionnaire d’exécution correspondant. Le gestionnaire d’exécution crée l’application. Tout d’abord, un ReplicaSet Kubernetes est créé avec un ou plusieurs pods, chacun d’eux contenant l’application à déployer. Le nombre de pods est défini par le paramètre `replicas` défini dans le fichier `spec.yaml` de l’application. Chaque pod peut avoir un ou plusieurs pools. Le nombre de pools est défini par le paramètre `poolsize` défini dans le fichier `spec.yaml`.

Ces paramètres ont un impact sur la quantité de demandes que le déploiement peut traiter en parallèle. Le nombre maximal de demandes à un moment donné est égal à `replicas` fois `poolsize`. Si vous avez 5 réplicas et 2 pools par réplica, le déploiement peut donc gérer 10 demandes en parallèle. L’image ci-dessous est une représentation graphique de `replicas` et de `poolsize` :

![Taille de pool et réplicas](media/big-data-cluster-create-apps/poolsize-vs-replicas.png)

Une fois le ReplicaSet créé et les pods démarrés, un travail cron est créé si un `schedule` a été défini dans le fichier `spec.yaml`. Enfin, le service Kubernetes créé peut être utilisé pour gérer et exécuter l’application (voir ci-dessous).

Quand une application est exécutée, le service Kubernetes pour l’application transmet par proxy les demandes à un réplica et retourne les résultats.

## <a name="security-considerations-for-applications-deployments-on-openshift"></a><a id="app-deploy-security"></a> Considérations sur la sécurité pour le déploiement d'applications sur OpenShift

SQL Server 2019 CU5 prend en charge le déploiement de clusters Big Data sur Red Hat OpenShift, ainsi qu’un modèle de sécurité mis à jour pour BDC, si bien que les conteneurs privilégiés ne sont plus nécessaires. Outre les utilisateurs sans privilège, les conteneurs s’exécutent en tant qu’utilisateur non racine par défaut pour tous les nouveaux déploiements avec SQL Server 2019 CU5.

À l’heure où CU5 est publié, l’étape de configuration des applications déployées avec les interfaces de [déploiement d’application]() s’exécute toujours comme utilisateur *racine*. En effet, c’est à ce moment-là que sont installés les packages supplémentaires utilisés par l’application. Le reste du code utilisateur déployé dans le cadre de l’application s’exécute en tant qu’utilisateur à faibles privilèges. 

En outre, **CAP_AUDIT_WRITE** est une fonctionnalité facultative permettant la planification d’applications SSIS à l’aide de travaux cron. Lorsque le fichier de spécification yaml de l’application indique une planification, l’application est déclenchée par le biais d’un travail cron, qui nécessite une fonctionnalité supplémentaire.  L’application peut également être déclenchée à la demande avec *azdata app run* via un appel de service web, qui ne nécessite pas la fonctionnalité CAP_AUDIT_WRITE. 

> [!NOTE]
> La contrainte de contexte de sécurité (SCC) personnalisée de l’[article sur le déploiement OpenShift](deploy-openshift.md) n’inclut pas cette fonctionnalité, car elle n’est pas requise par le déploiement par défaut d’un cluster Big Data. Pour activer cette fonctionnalité, vous devez d’abord mettre à jour le fichier yaml du SCC personnalisé pour inclure CAP_AUDIT_WRITE 

```yml
...
allowedCapabilities:
- SETUID
- SETGID
- CHOWN
- SYS_PTRACE
- AUDIT_WRITE
...
```

## <a name="how-to-work-with-application-deployment"></a>Comment utiliser le déploiement d’application

Les deux interfaces principales pour le déploiement d’application sont les suivantes : 
- [Interface de ligne de commande [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]](app-create.md)
- [Visual Studio Code et extension Azure Data Studio](app-deployment-extension.md)

Vous pouvez également exécuter une application à l’aide d’un service web RESTful. Pour plus d’informations, consultez [Utiliser des applications sur des clusters Big Data](app-consume.md).

## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations sur la création et l’exécution d’applications sur des [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)], consultez les rubriques suivantes :

- [Déployer des applications avec azdata](app-create.md)
- [Déployer des applications à l’aide de l’extension de déploiement d’application](app-deployment-extension.md)
- [Utiliser des applications sur des clusters Big Data](app-consume.md)

Pour plus d’informations sur les [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)], consultez la vue d’ensemble suivante :

- [Que sont les [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)] ?](big-data-cluster-overview.md)