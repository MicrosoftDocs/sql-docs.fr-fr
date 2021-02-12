---
title: Conteneurs Clusters Big Data non racine
titleSuffix: SQL Server big data clusters
description: Cet article explique comment déployer des conteneurs non racine dans Clusters Big Data SQL Server
author: mihaelablendea
ms.author: mihaelab
ms.reviewer: mikeray
ms.date: 06/22/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 52a24c1dbe2a239450b440c07de1a7852f9c627a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100046039"
---
# <a name="non-root-big-data-clusters-containers"></a>Conteneurs Clusters Big Data non racine

SQL Server 2019 CU5 introduit la prise en charge des conteneurs non racine. L’implémentation sur plateforme est plus sûre, car toutes les applications de conteneur s’exécutant dans Clusters Big Data sont démarrées par défaut en tant qu’utilisateurs non racine sur toutes les plateformes prises en charge. Ces fonctionnalités sont disponibles pour l’ensemble les nouveaux déploiements à l’aide de la balise d’image SQL Server 2019 CU5 correspondante. Les déploiements Clusters Big Data existants antérieurs à CU5 ne seront pas affectés par cette modification, et les applications de ces clusters continueront à s’exécuter en tant qu’utilisateur racine. 

## <a name="technical-background"></a>Contexte technique

Consultez [ce livre blanc technique](https://aka.ms/sql-bdc-openshift-security), qui donne les détails de la conception de la sécurité pour la prise en charge des déploiements à l’aide d’utilisateurs non racine, en mettant en évidence les raisons pour lesquelles Clusters Big Data élève temporairement les autorisations dans certains cas. Le contenu du livre blanc, développé en collaboration avec des experts en sécurité de SQL Server et de Red Hat, se concentre sur les contextes de sécurité et les fonctions d’OpenShift, mais les concepts et la conception de la sécurité de Clusters Big Data s’appliquent à toutes les plateformes prises en charge.

> [!NOTE]
> À l’heure où CU5 est publié, l’étape de configuration des applications déployées avec les interfaces de [déploiement d’application](concept-application-deployment.md) s’exécute toujours comme utilisateur *racine*. En effet, c’est à ce moment-là que sont installés les packages supplémentaires utilisés par l’application. Le reste du code utilisateur déployé dans le cadre de l’application s’exécute en tant qu’utilisateur à faibles privilèges. 

> [!NOTE]
> Nous recommandons d’exécuter le cluster avec le paramètre non racine par défaut. Si vous souhaitez rétablir le comportement antérieur à CU5 afin que les conteneurs Clusters Big Data s’exécutent en tant qu’utilisateur `root`, vous pouvez utiliser le nouveau commutateur de fonctionnalité `allowRunAsRoot` pour désactiver le comportement par défaut. Ce paramètre peut également être défini au moment du déploiement. Pour cela, spécifiez-le sous la section `security` dans le fichier de configuration de déploiement `control.json` :

```json
 "security": {
  …
    "allowRunAsRoot": true,
  …
}
```

> [!IMPORTANT]
> Il n’est pas possible de modifier ce paramètre dans les environnements OpenShift.

Dans la mesure où les services Clusters Big Data s’exécutent en tant qu’utilisateurs non racine, les informations d’identification servant à s’y connecter via le point de terminaison de la passerelle n’ont pas recours au nom d’utilisateur `root`. Si l’application permettant de se connecter au point de terminaison de la passerelle utilise des informations d’identification incorrectes, une erreur d’authentification se produit. Le nouveau nom d’utilisateur du point de terminaison de la passerelle dépend de la valeur transmise par la variable d’environnement `AZDATA_USERNAME`. Il s’agit du même nom d’utilisateur que pour le contrôleur et les points de terminaison SQL Server. Seuls les nouveaux déploiements sont affectés ; les clusters Big Data existants déployés avec les versions précédentes continuent d’utiliser `root`. Il n’y a aucun impact sur les informations d’identification si le cluster est déployé de façon à appliquer l’authentification Active Directory. 

## <a name="use-the-latest-azure-data-studio"></a>Utilisation de la dernière version d’Azure Data Studio

Azure Data Studio gère la modification des informations d’identification de manière transparente pour la connexion établie via la passerelle afin d’activer l’expérience de navigation HDFS dans l’Explorateur d’objets ou la soumission de travaux Spark par le biais des notebooks. Installez la dernière version de la [build Insider Azure Data Studio](../azure-data-studio/download-azure-data-studio.md#download-insiders-build-of-azure-data-studio). Elle comprend les modifications nécessaires pour ce cas d’usage.

En ce qui concerne les autres scénarios dans lesquels vous devez fournir des informations d’identification pour accéder au service via la passerelle (par exemple, connexion avec [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] ou accès aux tableaux de bord web pour Spark), vérifiez que les informations d’identification utilisées sont correctes. Si vous ciblez un cluster existant déployé avant CU5, continuez d’utiliser le nom d’utilisateur `root` pour vous connecter à la passerelle, même après la mise à niveau du cluster vers CU5. Si vous déployez un nouveau cluster à l’aide de la build CU5, connectez-vous en fournissant le nom d’utilisateur correspondant à la variable d’environnement `AZDATA_USERNAME`.

## <a name="configuration-file-switches"></a>Commutateurs du fichier de configuration

À partir de CU5, deux nouveaux commutateurs de fonctionnalités ont été ajoutés pour contrôler la collecte de métriques sur les pods et les nœuds. Si vous utilisez d’autres solutions pour superviser votre infrastructure Kubernetes, vous pouvez désactiver la collecte de métriques intégrée pour les pods et les nœuds hôtes en définissant `allowNodeMetricsCollection` et `allowPodMetricsCollection`* sur `false` dans le fichier de configuration de déploiement `control.json`. 

Par exemple 

```json
"security": {
  ...
  "allowPodMetricsCollection": true,
  "allowNodeMetricsCollection": true,
  ...
}
```

> [!NOTE]
> Pour les environnements OpenShift, ces paramètres sont par défaut définis sur false dans les profils de déploiement intégrés. La collecte des métriques sur les pods et les nœuds exige des fonctionnalités privilégiées ; le contexte de sécurité recommandé pour OpenShift est basé sur les contraintes *restreintes*.

En plus des conteneurs non privilégiés, les conteneurs s’exécutent en tant qu’utilisateur non racine par défaut sur toutes les plateformes prises en charge pour l’ensemble des nouveaux déploiements Clusters Big Data à partir de CU5. Ces fonctionnalités sont disponibles pour l’ensemble les nouveaux déploiements à l’aide de la balise d’image SQL Server 2019 CU5 correspondante. Les déploiements Clusters Big Data existants antérieurs à CU5 ne seront pas affectés, et les applications de ces clusters continueront à s’exécuter en tant qu’utilisateur racine.

## <a name="next-steps"></a>Étapes suivantes
[Guide pratique pour déployer des [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] sur Kubernetes](deployment-guidance.md)

[Déploiement de [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] sur OpenShift](deploy-openshift.md)

[Livre blanc sur la sécurité](https://aka.ms/sql-bdc-openshift-security)
