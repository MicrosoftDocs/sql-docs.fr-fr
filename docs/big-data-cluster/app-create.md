---
title: Déployer des applications avec azdata
titleSuffix: SQL Server Big Data Clusters
description: Déployez un script Python ou R en tant qu’application sur le cluster Big Data SQL Server 2019.
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.metadata: seo-lt-2019
ms.date: 12/13/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 9d475edb720d329d5fb52f7bd75c055fc3a8494a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100047929"
---
# <a name="how-to-deploy-an-app-on-sql-server-big-data-clusters"></a>Comment déployer une application sur un cluster Big Data SQL Server

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Les applications déployées sur les clusters Big Data (BDC) SQL Server bénéficient non seulement de nombreux avantages, tels que la puissance de calcul du cluster, mais également de l’accès aux données volumineuses disponibles sur le cluster. Cela améliore considérablement les performances, car votre application se trouve dans le même cluster que celui où résident les données.

Cet article explique comment déployer et gérer des scripts R et Python en tant qu’applications au sein d’un cluster Big Data SQL Server.

## <a name="whats-new-and-improved"></a>Nouveautés et améliorations

- Un même utilitaire de ligne de commande pour la gestion du cluster et de l’application
- Simplification du déploiement des applications et contrôle granulaire à l’aide de fichiers de spécifications
- Prise en charge de l’hébergement de types d’application supplémentaires - SQL Server Integration Services (SSIS) et MLeap.
- [Extension Visual Studio Code](app-deployment-extension.md) pour gérer le déploiement des applications

Les applications sont déployées et managées à l’aide de [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]. Cet article fournit des exemples de déploiement d’applications à partir de la ligne de commande. Pour savoir comment l’utiliser dans Visual Studio Code, consultez la section [Extension Visual Studio Code](app-deployment-extension.md).

Les types d’applications suivants sont pris en charge :

- **Python** : un des langages de programmation généraux les plus populaires pour différents profils, comme les ingénieurs de données, les scientifiques des données ou les ingénieurs DevOps ; il est adapté à de nombreux scénarios, comme le data wrangling, l’automatisation et le prototypage, dans une certaine mesure. Il est également utilisé pour programmer des applications professionnelles conjointement avec des infrastructures de développement web telles que Flask et Django pour répondre à différents besoins professionnels.  
- **R** : un autre langage de programmation populaire pour l’ingénierie des données et les scientifiques des données. Par rapport à Python, R est un langage de programmation avec une focalisation plus spécifique sur le calcul statistique et les graphiques.  
- **SQL Server Integration Services (SSIS)** : des solutions d’intégration de données hautes performances pour la génération et le débogage de packages ETL. SSIS utilise le format de fichier de package de services de transformation de données (DTSX), un format de fichier XML qui stocke les instructions pour le traitement de la migration de données entre les bases de données et l’intégration de sources de données externes.   
- **MLeap** : un format de sérialisation courant qui fournit tout ce qui est nécessaire pour exécuter et sérialiser des pipelines SparkML et d’autres qui peuvent être chargés au moment de l’exécution pour traiter les tâches de scoring ML en temps quasi-réel et proches des données.  

## <a name="prerequisites"></a>Prérequis

- [Cluster Big Data SQL Server 2019](deployment-guidance.md)
- [[!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]](../azdata/install/deploy-install-azdata.md)

## <a name="capabilities"></a>Fonctionnalités

Dans SQL Server 2019, vous pouvez créer, supprimer, décrire, initialiser, exécuter et mettre à jour votre application. Le tableau suivant décrit les commandes de déploiement d’application que vous pouvez utiliser avec **azdata**.

|Commande |Description |
|:---|:---|
|`azdata login` | Pour se connecter à un cluster Big Data SQL Server |
|`azdata app create` | Créez une application. |
|`azdata app delete` | Supprimez une application. |
|`azdata app describe` | Décrivez une application. |
|`azdata app init` | Lancez un nouveau squelette d’applications. |
|`azdata app list` | Répertoriez une ou des applications. |
|`azdata app run` | Exécutez une application. |
|`azdata app update`| Mettez à jour une application. |

Vous pouvez utiliser le paramètre `--help` pour obtenir de l’aide, comme dans l’exemple suivant :

```bash
azdata app create --help
```

Les sections suivantes décrivent ces commandes de façon plus détaillée.

## <a name="sign-in"></a>Se connecter

Avant de déployer ou d’utiliser les applications, connectez-vous à votre cluster Big Data SQL Server à l’aide de la commande `azdata login`. Spécifiez l’adresse IP externe du service `controller-svc-external` (par exemple, `https://ip-address:30080`), ainsi que le nom d’utilisateur et le mot de passe du cluster.

```bash
azdata login --controller-endpoint https://<ip-address-of-controller-svc-external>:30080 --controller-username <user-name>
```

## <a name="azure-kubernetes-service-aks"></a>Azure Kubernetes Service (AKS)

Si vous utilisez AKS, vous devez exécuter la commande suivante pour obtenir l’adresse IP du service `controller-svc-external` en exécutant cette commande dans une fenêtre bash ou cmd :


```bash
kubectl get svc controller-svc-external -n <name of your big data cluster>
```

## <a name="kubernetes-clusters-created-with-kubeadm"></a>Clusters Kubernetes créés avec kubeadm

Exécutez la commande suivante pour obtenir l’adresse IP permettant de se connecter au cluster

```bash
kubectl get node --selector='node-role.kubernetes.io/master'
```

## <a name="create-an-app-skeleton"></a>Créer un squelette d’application

La commande **azdata app init** fournit une structure avec les artefacts nécessaires au déploiement d’une application. L’exemple ci-dessous crée add-app, en exécutant la commande suivante.

```bash
azdata app init --name add-app --version v1 --template python
```

Cela crée un dossier nommé « hello ».  Vous pouvez utiliser la commande `cd` dans le répertoire et inspecter les fichiers générés dans le dossier. spec.yaml définit l’application, par exemple son nom, sa version et son code source. Vous pouvez modifier les spécifications pour changer le nom, la version, l’entrée et les sorties.

Voici un exemple de sortie de la commande init que vous verrez dans le dossier

```
add-app.py
run-spec.yaml
spec.yaml
```

## <a name="create-an-app"></a>Créer une application

Pour créer une application, utilisez [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] avec la commande `app create`. Ces fichiers résident localement sur la machine à partir de laquelle vous créez l’application.

Pour créer une application sur un cluster Big Data, utilisez la syntaxe suivante :

```bash
azdata app create --spec <directory containing spec file>
```

La commande suivante montre ce à quoi cela peut ressembler :

```bash
azdata app create --spec ./addpy
```

Cela suppose que votre application est stockée dans le dossier `addpy`. Ce dossier doit également contenir un fichier de spécifications (nommé `spec.yaml`) pour l’application. Consultez la [page relative au déploiement d’applications](concept-application-deployment.md) pour plus d’informations sur le fichier `spec.yaml`.

Pour déployer cet exemple d’application, créez les fichiers suivants dans un répertoire appelé `addpy` :

- `add.py`. Copiez le code Python suivant dans ce fichier :
   ```py
   #add.py
  def add(x, y):
    result = x+y
    return result
  result=add(x,y)
   ```
- `spec.yaml`. Copiez le code suivant dans ce fichier :
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

Ensuite, exécutez la commande ci-dessous :

```bash
azdata app create --spec ./addpy
```

Vous pouvez vérifier si l’application est déployée à l’aide de la commande list :

```bash
azdata app list
```

Si le déploiement n’est pas terminé, `state` doit afficher `WaitingforCreate`, comme dans l’exemple suivant :

```json
[
  {
    "name": "add-app",
    "state": "WaitingforCreate",
    "version": "v1"
  }
]
```

Une fois le déploiement réussi, l’état de `state` doit passer à `Ready` :

```json
[
  {
    "name": "add-app",
    "state": "Ready",
    "version": "v1"
  }
]
```

## <a name="list-an-app"></a>Lister une application

Vous pouvez lister toutes les applications qui ont été créées à l’aide de la commande `app list`.

La commande suivante liste toutes les applications qui sont disponibles dans votre cluster Big Data :

```bash
azdata app list
```

Si vous spécifiez un nom et une version, la commande liste une application et son état (En cours de création ou Prêt) :

```bash
azdata app list --name <app_name> --version <app_version>
```

L’exemple ci-dessous montre cette commande :

```bash
azdata app list --name add-app --version v1
```

Vous devez obtenir une sortie similaire à la suivante :

```json
[
  {
    "name": "add-app",
    "state": "Ready",
    "version": "v1"
  }
]
```

## <a name="delete-an-app"></a>Supprimer une application

Pour supprimer une application de votre cluster Big Data, utilisez la syntaxe suivante :

```bash
azdata app delete --name add-app --version v1
```

## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations, découvrez comment intégrer des applications déployées sur [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] dans vos propres applications pour [Exécuter des applications sur des clusters Big Data](app-run.md) et [Utiliser des applications sur des clusters Big Data](app-consume.md). Vous pouvez également consulter d’autres [exemples de déploiement d’application](https://aka.ms/sql-app-deploy).

Pour plus d’informations sur les [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)], consultez [Que sont les [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)] ?](big-data-cluster-overview.md).