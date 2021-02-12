---
title: Installer des outils de Big Data
titleSuffix: SQL Server big data clusters
description: Découvrez comment installer les outils utilisés avec les clusters Big Data SQL Server 2019.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mihaelab
ms.date: 06/22/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 98226c45b9bc677386870e432ee835803bde32a2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100047509"
---
# <a name="install-sql-server-2019-big-data-tools"></a>Installer les outils de Big Data SQL Server 2019

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Cet article décrit les outils clients qui doivent être installés pour la création, la gestion et l’utilisation des [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]. La section suivante fournit une liste d’outils et de liens vers les instructions d’installation. Avant de déployer un cluster Big Data, configurez les outils marqués comme requis sur Windows ou Linux.

## <a name="big-data-cluster-tools"></a>Outils de cluster Big Data

Le tableau suivant liste les outils de cluster Big Data courants et explique comment les installer :

| Outil | Obligatoire | Description | Installation |
|---|---|---|---|
| `python` | Oui | Python est un langage de programmation de haut niveau, orienté objet, interprété et doté d’une sémantique dynamique. De nombreuses parties des clusters Big Data pour SQL Server utilisent Python. | [Installer python](#python)|
| [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] | Oui | Outil en ligne de commande pour l’installation et la gestion d’un cluster Big Data. | [Installer](../azdata/install/deploy-install-azdata.md) |
| `kubectl`<sup>1</sup> | Oui | Outil en ligne de commande permettant de superviser le cluster Kubernetes sous-jacent ([plus d’informations](https://kubernetes.io/docs/tasks/tools/install-kubectl/)). | [Windows](https://kubernetes.io/docs/tasks/tools/install-kubectl/#install-with-powershell-from-psgallery) \| [Linux](https://kubernetes.io/docs/tasks/tools/install-kubectl/#install-using-native-package-management) |
| **Azure Data Studio** | Oui | Outil graphique multiplateforme permettant d’interroger SQL Server. | [Installer](../azure-data-studio/download-azure-data-studio.md) |
| **Extension de virtualisation de données** | Oui | Extension pour Azure Data Studio qui fournit un Assistant Virtualisation de données. | [Installer](../azure-data-studio/extensions/data-virtualization-extension.md) |
| **Azure CLI**<sup>2</sup> | Pour AKS | Interface de ligne de commande moderne pour la gestion des services Azure. Utilisée avec les déploiements de cluster Big Data AKS ([plus d’informations](/cli/azure/)). | [Installer](/cli/azure/install-azure-cli) |
| **mssql-cli** | Facultatif | Interface de ligne de commande moderne permettant d’interroger SQL Server ([plus d’informations](../tools/mssql-cli.md)). | [Windows](https://github.com/dbcli/mssql-cli/blob/master/doc/installation/windows.md) \| [Linux](https://github.com/dbcli/mssql-cli/blob/master/doc/installation/linux.md) |
| **sqlcmd** | Pour certains scripts | Outil en ligne de commande hérité permettant d’interroger SQL Server ([plus d’informations](../tools/sqlcmd-utility.md)). Vous devrez peut-être installer le pilote Microsoft ODBC 11 pour SQL Server avant d’installer le package SQLCMD. | [Windows](https://www.microsoft.com/download/details.aspx?id=36433) \| [Linux](../linux/sql-server-linux-setup-tools.md) |
| `curl` <sup>3</sup> | Pour certains scripts | Outil en ligne de commande pour transférer des données avec des URL. | [Windows](https://curl.haxx.se/windows/) \| Linux : installer le package d’installation |
| `oc` | Requis pour les déploiements Red Hat OpenShift et Azure RedHat OpenShift. |`oc` est l’interface de ligne de commande (CLI) Shift ouvert. | [Installation de l’interface CLI](https://docs.openshift.com/container-platform/4.4/cli_reference/openshift_cli/getting-started-cli.html#installing-the-cli)

<sup>1</sup> Vous devez utiliser `kubectl` version 1.13 ou ultérieure. En outre, la version de `kubectl` doit être une version mineure plus une ou moins une de votre cluster Kubernetes. Si vous souhaitez installer une version spécifique sur le client `kubectl`, consultez [Installer le binaire `kubectl` via curl](https://kubernetes.io/docs/tasks/tools/install-kubectl/#install-kubectl-binary-using-curl) (sur Windows 10, utilisez cmd.exe et non Windows PowerShell pour exécuter curl).

> [!TIP]
> Pour utiliser `kubectl` avec un cluster déployé sur AKS (Azure Kubernetes Service), vous devez définir le contexte de cluster à l’aide de la commande Azure CLI suivante :
>
>    ```azurecli
>    az aks get-credentials --name <aks_cluster_name> --resource-group <azure_resource_group_name>
>    ```

<sup>2</sup> Vous devez utiliser Azure CLI version 2.0.4 ou ultérieure. Exécutez `az --version` pour trouver la version, si nécessaire.

<sup>3</sup> Sur Windows 10, `curl` est déjà dans le chemin (PATH) quand vous effectuez l’exécution à partir d’une invite cmd. Pour les autres versions de Windows, téléchargez `curl` en utilisant le lien et placez-le dans votre chemin (PATH).

## <a name="which-tools-are-required"></a>Quels sont les outils requis ?

Le tableau précédent indique tous les outils courants qui sont utilisés avec les clusters Big Data. Les outils requis varient en fonction de votre scénario. Mais, en général, les outils suivants sont les plus importants pour gérer le cluster, s’y connecter et l’interroger :

- [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]
- `kubectl`
- **Azure Data Studio**
- **Extension de virtualisation de données**

Les autres outils sont requis uniquement dans certains scénarios. **Azure CLI** peut être utilisé pour gérer les services Azure associés aux déploiements AKS. **mssql-cli** est un outil facultatif mais utile qui vous permet de vous connecter à l’instance maître SQL Server dans le cluster et d’exécuter des requêtes à partir de la ligne de commande. Pour leur part, **sqlcmd** et `curl` sont nécessaires si vous envisagez d’installer des exemples de données avec le script GitHub.

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

## <a name="download-and-install-azure-data-studio"></a>Télécharger et installer Azure Data Studio

Azure Data Studio fournit des capacités et des fonctionnalités spécifiques pour les clusters Big Data SQL Server.

[Procurez-vous la dernière version d’Azure Data Studio](../azure-data-studio/download-azure-data-studio.md).

Pour plus d’informations sur la dernière version, consultez les [notes de publication](./release-notes-big-data-cluster.md).

## <a name="next-steps"></a>Étapes suivantes

Après avoir configuré les outils, déployez un cluster Big Data SQL Server 2019 sur Kubernetes dans le cloud ou localement. Pour plus d’informations, consultez les articles suivants consacrés au déploiement :

- [Démarrage rapide : déployer un cluster Big Data SQL Server sur AKS (Azure Kubernetes Service)](quickstart-big-data-cluster-deploy.md)
- [Guide pratique pour déployer des [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] sur Kubernetes](deployment-guidance.md)

Pour plus d’informations sur les clusters Big Data, consultez [Présentation des [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]](big-data-cluster-overview.md).