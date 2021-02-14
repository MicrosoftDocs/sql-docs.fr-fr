---
title: Effectuer un déploiement hors connexion
titleSuffix: SQL Server big data clusters
description: Découvrez comment effectuer un déploiement hors connexion d’un cluster Big Data SQL Server 2019 et comment charger des images de conteneur dans un dépôt privé.
author: mihaelablendea
ms.author: mihaelab
ms.reviewer: mikeray
ms.date: 11/04/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 9cbe53718818116ae9a0c435a1cf985c132f759b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100047275"
---
# <a name="perform-an-offline-deployment-of-a-sql-server-big-data-cluster"></a>Effectuer un déploiement hors connexion d’un cluster Big Data SQL Server

Cet article explique comment déployer hors connexion un [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]. Les clusters Big Data doivent avoir accès à un référentiel Docker à partir duquel tirer (pull) des images conteneur. Lors d’une installation hors connexion, les images nécessaires sont placées dans un référentiel Docker privé. Ce référentiel privé est ensuite utilisé comme source des images d’un nouveau déploiement.

## <a name="prerequisites"></a>Conditions préalables requises

- Docker Engine 1.8+ sur n’importe quelle distribution Linux prise en charge ou Docker pour Mac/Windows. Pour plus d’informations, consultez [Installer Docker](https://docs.docker.com/engine/installation/).

## <a name="load-images-into-a-private-repository"></a>Charger des images dans un référentiel privé

Les étapes suivantes expliquent comment tirer (pull) les images conteneur du cluster Big Data à partir du référentiel Microsoft, puis comment les pousser (push) vers votre référentiel privé.

> [!TIP]
> Ce processus est expliqué ci-dessous. Toutefois, pour simplifier cette tâche, vous pouvez utiliser le [script automatisé](#automated) au lieu d’exécuter manuellement ces commandes.

1. Tirez (pull) les images conteneur du cluster Big Data en répétant la commande suivante. Remplacez `<SOURCE_IMAGE_NAME>` par chaque [nom d’image](#images). Remplacez `<SOURCE_DOCKER_TAG>` par l’étiquette de la version du cluster Big Data, par exemple **2019-GDR1-ubuntu-16.04**.  

   ```PowerShell
   docker pull mcr.microsoft.com/mssql/bdc/<SOURCE_IMAGE_NAME>:<SOURCE_DOCKER_TAG>
   ```

1. Connectez-vous au registre Docker privé cible.

   ```PowerShell
   docker login <TARGET_DOCKER_REGISTRY> -u <TARGET_DOCKER_USERNAME> -p <TARGET_DOCKER_PASSWORD>
   ```

1. Étiquetez chacune des images locales à l’aide de la commande suivante :

   ```PowerShell
   docker tag mcr.microsoft.com/mssql/bdc/<SOURCE_IMAGE_NAME>:<SOURCE_DOCKER_TAG> <TARGET_DOCKER_REGISTRY>/<TARGET_DOCKER_REPOSITORY>/<SOURCE_IMAGE_NAME>:<TARGET_DOCKER_TAG>
   ```

1. Poussez (push) les images locales vers le référentiel Docker privé :

   ```PowerShell
   docker push <TARGET_DOCKER_REGISTRY>/<TARGET_DOCKER_REPOSITORY>/<SOURCE_IMAGE_NAME>:<TARGET_DOCKER_TAG>
   ```
 
> [!WARNING]
> Ne modifiez pas les images du cluster Big Data une fois qu’elles sont transmises à votre dépôt privé. L’exécution d’un déploiement avec des images modifiées entraîne une configuration de cluster Big Data non prise en charge.


### <a name="big-data-cluster-container-images"></a><a id="images"></a> Images conteneur de cluster Big Data

Les images conteneur de cluster Big Data suivantes sont nécessaires pour une installation hors connexion :
- **mssql-app-service-proxy**
- **mssql-control-watchdog**
- **mssql-controller**
- **mssql-dns**
- **mssql-hadoop**
- **mssql-mleap-serving-runtime**
- **mssql-mlserver-py-runtime**
- **mssql-mlserver-r-runtime**
- **mssql-monitor-collectd**
- **mssql-monitor-elasticsearch**
- **mssql-monitor-fluentbit**
- **mssql-monitor-grafana**
- **mssql-monitor-influxdb**
- **mssql-monitor-kibana**
- **mssql-monitor-telegraf**
- **mssql-security-knox**
- **mssql-security-support**
- **mssql-server-controller**
- **mssql-server-data**
- **mssql-ha-operator**
- **mssql-ha-supervisor**
- **mssql-service-proxy**
- **mssql-ssis-app-runtime**


## <a name="automated-script"></a><a id="automated"></a> Script automatisé

Vous pouvez utiliser un script Python automatisé qui tire (pull) automatiquement toutes les images conteneur nécessaires, puis les pousse (push) vers un référentiel privé.

> [!NOTE]
> Python est un prérequis à l’utilisation du script. Pour plus d’informations sur l’installation de Python, consultez la [documentation Python](https://wiki.python.org/moin/BeginnersGuide/Download).

1. À partir de Bash ou de PowerShell, téléchargez le script avec **curl** :

   ```PowerShell
   curl -o push-bdc-images-to-custom-private-repo.py "https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/sql-big-data-cluster/deployment/offline/push-bdc-images-to-custom-private-repo.py"
   ```

1. Ensuite, exécutez le script avec l’une des commandes suivantes :

   **Windows :**

   ```PowerShell
   python push-bdc-images-to-custom-private-repo.py
   ```

   **Linux :**

   ```bash
   sudo python push-bdc-images-to-custom-private-repo.py
   ```

1. Suivez les invites pour entrer le référentiel Microsoft, ainsi que les informations de votre référentiel privé. Une fois l’exécution du script terminée, toutes les images nécessaires doivent se trouver dans le référentiel privé.

1. Suivez les instructions fournies [ici](deployment-custom-configuration.md#docker) pour découvrir comment personnaliser le fichier de configuration de déploiement `control.json` afin d’utiliser votre référentiel et votre registre de conteneurs. Notez que vous devez définir les variables d’environnement `DOCKER_USERNAME` et `DOCKER_PASSWORD` avant le déploiement pour permettre l’accès à votre référentiel privé.

## <a name="install-tools-offline"></a>Installer des outils hors connexion

Les déploiements de clusters Big Data nécessitent plusieurs outils, tels que **Python**, [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] et **kubectl**. Suivez les étapes suivantes pour installer ces outils sur un serveur hors connexion.

### <a name="install-python-offline"></a><a id="python"></a> Installer Python hors connexion

1. Sur une machine disposant d’un accès à Internet, téléchargez l’un des fichiers compressés suivants contenant Python :

   | Système d’exploitation | Téléchargement |
   |---|---|
   | Windows | [https://go.microsoft.com/fwlink/?linkid=2074021](https://go.microsoft.com/fwlink/?linkid=2074021) |
   | Linux   | [https://go.microsoft.com/fwlink/?linkid=2065975](https://go.microsoft.com/fwlink/?linkid=2065975) |
   | OSX     | [https://go.microsoft.com/fwlink/?linkid=2065976](https://go.microsoft.com/fwlink/?linkid=2065976) |

1. Copiez le fichier compressé sur la machine cible et extrayez-le dans un dossier de votre choix.

1. Pour Windows uniquement, exécutez `installLocalPythonPackages.bat` à partir de ce dossier en indiquant comme paramètre le chemin complet au dossier.

   ```PowerShell
   installLocalPythonPackages.bat "C:\python-3.6.6-win-x64-0.0.1-offline\0.0.1"
   ```

### <a name="install-azdata-offline"></a><a id="azdata"></a> Installer azdata hors connexion

1. Sur une machine disposant d’un accès à Internet et de [Python](https://wiki.python.org/moin/BeginnersGuide/Download), exécutez la commande suivante pour télécharger tous les packages [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] dans le dossier actif.

   ```PowerShell
   pip download -r https://aka.ms/azdata
   ```

1. Copiez les packages téléchargés et le fichier `requirements.txt` sur la machine cible.

1. Exécutez la commande suivante sur la machine cible, en spécifiant le dossier dans lequel vous avez copié les fichiers précédents.

   ```PowerShell
   pip install --no-index --find-links <path-to-packages> -r <path-to-requirements.txt>
   ```

### <a name="install-kubectl-offline"></a><a id="kubectl"></a> Installer kubectl hors connexion

Pour installer **kubectl** sur une machine hors connexion, utilisez les étapes suivantes.

1. Utilisez **curl** pour télécharger **kubectl** dans le dossier de votre choix. Pour plus d’informations, consultez [Installer le fichier binaire kubectl avec curl](https://kubernetes.io/docs/tasks/tools/install-kubectl/#install-kubectl-binary-using-curl).

1. Copiez le dossier sur la machine cible.

## <a name="deploy-from-private-repository"></a>Effectuer le déploiement à partir d’un référentiel privé

Pour effectuer un déploiement à partir du référentiel privé, utilisez les étapes décrites dans [le guide de déploiement](deployment-guidance.md), mais utilisez un fichier de configuration de déploiement personnalisé spécifiant les informations de votre référentiel Docker privé. Les commandes [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] suivantes montrent comment modifier les paramètres Docker dans un fichier de configuration de déploiement personnalisé nommé `control.json` :

```bash
azdata bdc config replace --config-file custom/control.json --json-values "$.spec.docker.repository=<your-docker-repository>"
azdata bdc config replace --config-file custom/control.json --json-values "$.spec.docker.registry=<your-docker-registry>"
azdata bdc config replace --config-file custom/control.json --json-values "$.spec.docker.imageTag=<your-docker-image-tag>"
```

Le déploiement vous invite à entrer le nom d’utilisateur et le mot de passe Docker. Vous pouvez également les spécifier dans les variables d’environnement `DOCKER_USERNAME` et `DOCKER_PASSWORD`.

## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations sur le déploiement des clusters Big Data, consultez [Guide pratique pour déployer des [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] sur Kubernetes](deployment-guidance.md).
