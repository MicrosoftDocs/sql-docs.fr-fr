---
title: Conseils pour le déploiement
titleSuffix: SQL Server Big Data Clusters
description: Découvrez comment déployer des clusters Big Data SQL Server sur Kubernetes.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mihaelab
ms.date: 06/22/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: d97c5e9b8c508387298cf00a8fbbe96ded1a2eea
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100047149"
---
# <a name="how-to-deploy-big-data-clusters-2019-on-kubernetes"></a>Guide pratique pour déployer des [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] sur Kubernetes

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Un cluster Big Data SQL Server est déployé sous la forme de conteneurs Docker sur un cluster Kubernetes. Voici une vue d’ensemble des étapes d’installation et de configuration :

- Configurez un cluster Kubernetes sur une seule machine virtuelle, un cluster de machines virtuelles ou dans AKS (Azure Kubernetes Service), Red Hat OpenShift ou dans Azure Red Hat OpenShift (ARO).
- Installez l’outil de configuration de cluster [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] sur votre machine cliente.
- Déployez un cluster Big Data SQL Server dans un cluster Kubernetes.

## <a name="supported-platforms"></a>Plateformes prises en charge

Consultez [des plateformes prises en charge](release-notes-big-data-cluster.md#supported-platforms) pour obtenir la liste complète des différentes plateformes Kubernetes validées pour le déploiement de clusters Big Data SQL Server.

### <a name="sql-server-editions"></a>Éditions de SQL Server

|Édition|Notes|
|---------|---------|
|Entreprise<br/>standard<br/>Développeur| L’édition du cluster Big Data est déterminée par l’édition de l’instance principale SQL Server. Au moment du déploiement, l’édition Développeur est déployée par défaut. Vous pouvez changer l’édition après le déploiement. Consultez [Configurer l’instance principale SQL Server](./configure-sql-server-master-instance.md). |

## <a name="kubernetes"></a><a id="prereqs"></a> Kubernetes

### <a name="kubernetes-cluster-setup"></a><a id="kubernetes"></a> Configuration du cluster Kubernetes

Si vous avez déjà un cluster Kubernetes qui répond aux prérequis ci-dessus, vous pouvez passer directement à l’[étape de déploiement](#deploy). Cette section suppose que vous disposez de connaissances de base sur les concepts de Kubernetes.  Pour obtenir des informations plus détaillées, consultez la [documentation de Kubernetes](https://kubernetes.io/docs/home).

Vous pouvez choisir de déployer Kubernetes des manières suivantes :

| Déployer Kubernetes sur : | Description | Lien |
|---|---|---|
| **Azure Kubernetes Services (AKS)** | Service de conteneur Kubernetes managé dans Azure. | [Instructions](deploy-on-aks.md) |
| **Une ou plusieurs machines (`kubeadm`)** | Cluster Kubernetes déployé sur des machines physiques ou virtuelles à l’aide de `kubeadm` | [Instructions](deploy-with-kubeadm.md) |
|**Azure Red Hat OpenShift** | Une version managée d’OpenShift qui s’exécute dans Azure. | [Instructions](deploy-openshift.md)|
|**Red Hat OpenShift**|Une plate-forme d’application Kubernetes entreprise et Cloud hybride.| [Instructions](deploy-openshift.md)|

> [!TIP]
> Vous pouvez également créer un script pour déployer AKS et un cluster Big Data en une seule étape. Pour plus d’informations, consultez la procédure à suivre dans un [script Python](quickstart-big-data-cluster-deploy.md) ou dans un [notebook](notebooks-deploy.md) Azure Data Studio.

### <a name="verify-kubernetes-configuration"></a>Vérifier la configuration de Kubernetes

Exécutez la commande `kubectl` pour afficher la configuration du cluster. Vérifiez que kubectl pointe vers le bon contexte de cluster.

```bash
kubectl config view
```

> [!Important] 
> Si vous déployez sur un cluster Kubernetes à plusieurs nœuds que vous avez démarré à l’aide de `kubeadm`, avant de démarrer le déploiement du cluster Big Data, assurez-vous que les horloges sont synchronisées entre tous les nœuds Kubernetes ciblés par le déploiement. Le cluster Big Data intègre des propriétés d’intégrité temporaires pour différents services et des décalages d’horloge peuvent entraîner un état incorrect.

Après avoir configuré votre cluster Kubernetes, vous pouvez passer au déploiement d’un nouveau cluster Big Data SQL Server. Si vous effectuez une mise à niveau à partir d’une version précédente, consultez [Guide pratique pour mettre à niveau des [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]](deployment-upgrade.md).

## <a name="ensure-you-have-storage-configured"></a>Vérifier que le stockage est configuré

La plupart des déploiements de cluster Big Data doivent disposer d’un stockage persistant. À ce stade, vous devez vous assurer de disposer d’un plan pour la façon dont vous allez fournir un stockage persistant sur le cluster Kubernetes avant de déployer le cluster Big Data.

Si vous déployez dans AKS, aucune configuration de stockage n’est nécessaire. AKS fournit des classes de stockage intégrées au provisionnement dynamique. Vous pouvez personnaliser la classe de stockage (`default` ou `managed-premium`) dans le fichier de configuration de déploiement. Les profils intégrés utilisent une classe de stockage `default`. Si vous déployez sur un cluster Kubernetes que vous avez déployé à l’aide de `kubeadm`, vous devez vous veiller à disposer d’un espace de stockage suffisant pour qu’un cluster de l’échelle souhaitée soit disponible et son utilisation configurée. Si vous souhaitez personnaliser la façon dont votre stockage est utilisé, vous devez le faire avant de continuer. Consultez [Persistance des données avec un cluster Big Data SQL Server sur Kubernetes](concept-data-persistence.md).

## <a name="install-sql-server-2019-big-data-tools"></a>Installer les outils de Big Data SQL Server 2019

Avant de déployer un cluster Big Data SQL Server 2019, commencez par [installer les outils de Big Data](deploy-big-data-tools.md) :

- [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]
- `kubectl`
- Azure Data Studio
- [Extension de virtualisation de données](../azure-data-studio/extensions/data-virtualization-extension.md) pour Azure Data Studio


## <a name="deployment-overview"></a><a id="deploy"></a> Vue d’ensemble du déploiement

La plupart des paramètres de cluster Big Data sont définis dans un fichier de configuration de déploiement JSON. Vous pouvez utiliser un profil de déploiement par défaut pour AKS et les clusters Kubernetes créés avec `kubeadm` ou vous pouvez personnaliser un fichier de configuration de déploiement et l’utiliser pendant l’installation. Pour des raisons de sécurité, les paramètres d’authentification sont passés par le biais de variables d’environnement.

Les sections suivantes fournissent plus d’informations sur la façon de configurer vos déploiements de cluster Big Data, ainsi que des exemples de personnalisations courantes. Sachez que vous pouvez toujours modifier le fichier de configuration de déploiement personnalisé à l’aide d’un éditeur comme VS Code.

## <a name="default-configurations"></a><a id="configfile"></a> Configurations par défaut

Les options de déploiement d’un cluster Big Data sont définies dans les fichiers de configuration JSON. Vous pouvez démarrer votre personnalisation du déploiement de cluster à partir des profils de déploiement intégrés disponibles dans [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]. 

> [!NOTE]
> Les images conteneur nécessaires pour le déploiement de cluster Big Data sont hébergées dans le registre de conteneurs Microsoft (`mcr.microsoft.com`), dans le référentiel `mssql/bdc`. Par défaut, ces paramètres sont déjà inclus dans le fichier de configuration `control.json` dans chacun des profils de déploiement inclus avec [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]. De plus, l’étiquette d’image conteneur pour chaque version est également préremplie dans le même fichier de configuration. Si vous devez extraire (pull) les images conteneur dans votre propre registre de conteneurs privé et/ou modifier les paramètres du référentiel/registre de conteneurs, suivez les instructions fournies dans l’[article sur l’installation hors connexion](deploy-offline.md)

Exécutez cette commande pour rechercher les modèles disponibles :

```
azdata bdc config list -o table 
```

Les modèles suivants sont disponibles sur à compter de SQL Server 2019 CU5 : 

| Profil de déploiement | Environnement Kubernetes |
|---|---|
| `aks-dev-test` | Déployer un cluster Big Data SQL Server sur AKS (Azure Kubernetes Service)|
| `aks-dev-test-ha` | Déployez un cluster Big Data SQL Server sur AKS (Azure Kubernetes Service). Les services critiques comme le nœud de nom HDFS et le principal SQL Server sont configurés pour la haute disponibilité.|
| `aro-dev-test`|Déployez un cluster Big Data SQL Server sur Azure Red Hat OpenShift pour le développement et les tests. <br/><br/>Introduite dans SQL Server 2019 CU 5.|
| `aro-dev-test-ha`|Déployez un cluster Big Data SQL Server avec une haute disponibilité sur un cluster Red Hat OpenShift pour le développement et les tests. <br/><br/>Introduite dans SQL Server 2019 CU 5.|
| `kubeadm-dev-test` | Déployer le cluster Big Data SQL Server sur un cluster Kubernetes créé avec kubeadm à l’aide d’une seule ou de plusieurs machines physiques ou virtuelles.|
| `kubeadm-prod`| Déployer le cluster Big Data SQL Server sur un cluster Kubernetes créé avec kubeadm à l’aide d’une seule ou de plusieurs machines physiques ou virtuelles. Utilisez ce modèle pour permettre l’intégration des services du cluster Big Data avec Active Directory. Les services critiques tels que l’instance principale SQL Server et le nœud de nom HDFS sont déployés dans une configuration hautement disponible.  |
| `openshift-dev-test`|Déployez un cluster Big Data SQL Server sur un cluster Red Hat OpenShift pour le développement et les tests. <br/><br/>Introduite dans SQL Server 2019 CU 5.|
| `openshift-prod`|Déployez un cluster Big Data SQL Server avec une haute disponibilité sur un cluster Red Hat OpenShift. <br/><br/>Introduite dans SQL Server 2019 CU 5.|

Vous pouvez déployer un cluster Big Data en exécutant `azdata bdc create`. Vous êtes alors invité à choisir l’une des configurations par défaut, puis à suivre la procédure de déploiement.

La première fois que vous exécutez [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)], vous devez inclure `--accept-eula=yes` pour accepter le contrat de licence utilisateur final (CLUF).

```bash
azdata bdc create --accept-eula=yes
```

Dans ce scénario, vous êtes invité à fournir tous les paramètres qui ne font pas partie de la configuration par défaut, notamment les mots de passe. 

> [!IMPORTANT]
> Par défaut, le cluster Big Data est nommé `mssql-cluster`. Il est important de le connaître si vous souhaitez exécuter l’une des commandes `kubectl` spécifiant l’espace de noms Kubernetes avec le paramètre `-n`.

## <a name="custom-configurations"></a><a id="customconfig"></a> Configurations personnalisées

Vous pouvez également personnaliser votre déploiement pour prendre en charge les charges de travail que vous envisagez d’exécuter. Vous ne pouvez pas modifier la mise à l’échelle (nombre de réplicas) ni les paramètres de stockage des services de cluster Big Data après les déploiements. Vous devez donc planifier votre configuration de déploiement avec soin afin d’éviter les problèmes de capacité. Pour personnaliser votre déploiement, procédez comme suit :

1. Commencez par l’un des profils de déploiement standard correspondant à votre environnement Kubernetes. Vous pouvez utiliser la commande `azdata bdc config list` pour les lister :

   ```bash
   azdata bdc config list
   ```

1. Pour personnaliser votre déploiement, créez une copie du profil de déploiement avec la commande `azdata bdc config init`. Par exemple, la commande suivante crée une copie des fichiers de configuration de déploiement `aks-dev-test` dans un répertoire cible nommé `custom` :

   ```bash
   azdata bdc config init --source aks-dev-test --target custom
   ```

   >[!TIP]
   >`--target` spécifie un répertoire qui contient les fichiers de configuration, `bdc.json` et `control.json`, en fonction du paramètre `--source`.

1. Pour personnaliser les paramètres dans votre profil de configuration de déploiement, vous pouvez modifier le fichier de configuration de déploiement dans un outil adapté à la modification des fichiers JSON, par exemple VS Code. Pour une automatisation par script, vous pouvez également modifier le profil de déploiement personnalisé à l’aide de la commande `azdata bdc config`. Par exemple, la commande suivante modifie un profil de déploiement personnalisé en remplaçant le nom par défaut du cluster déployé (`mssql-cluster`) par `test-cluster` :  

   ```bash
   azdata bdc config replace --config-file custom/bdc.json --json-values "metadata.name=test-cluster"
   ```

   > [!TIP]
   > Vous pouvez également passer le nom du cluster au moment du déploiement à l’aide du paramètre *--name* pour la commande *azdata create bdc*. Les paramètres dans la commande ont priorité sur les valeurs dans les fichiers de configuration.
   >
   > [JSONPath Online Evaluator](https://jsonpath.com/) est un outil utile pour rechercher des chemins JSON.
   >
   En plus de passer des paires clé-valeur, vous pouvez également fournir des valeurs JSON inline ou passer des fichiers de correctif JSON. Pour plus d’informations, consultez [Configurer les paramètres de déploiement de clusters Big Data](deployment-custom-configuration.md).

1. Passez ensuite le fichier de configuration personnalisé à `azdata bdc create`. Notez que vous devez définir les [variables d’environnement](#env) requises ; sinon, le terminal vous invitera à fournir les valeurs :

   ```bash
   azdata bdc create --config-profile custom --accept-eula yes
   ```

> Pour plus de détails sur la structure d’un fichier de configuration de déploiement, consultez ces [informations de référence](reference-deployment-config.md). Pour obtenir d’autres exemples de configuration, consultez [Configurer les paramètres de déploiement de clusters Big Data](deployment-custom-configuration.md).

## <a name="environment-variables"></a><a id="env"></a> Variables d’environnement

Les variables d’environnement suivantes sont utilisées pour les paramètres de sécurité qui ne sont pas stockés dans un fichier de configuration de déploiement. Notez que les paramètres Docker, à l’exception des informations d’identification, peuvent être définis dans le fichier de configuration.

| Variable d’environnement | Condition requise |Description |
|---|---|---|
| `AZDATA_USERNAME` | Obligatoire |Nom d’utilisateur de l’administrateur du cluster Big Data SQL Server. Une connexion sysadmin portant le même nom est créée dans l’instance principale SQL Server. En guise de bonne pratique de sécurité, le compte `sa` est désactivé. <br/><br/>[!INCLUDE [big-data-cluster-root-user](../includes/big-data-cluster-root-user.md)]|
| `AZDATA_PASSWORD` | Obligatoire |Mot de passe des comptes d’utilisateur créés ci-dessus. Sur les clusters déployés avant SQL Server 2019 CU5, le même mot de passe est utilisé pour l’utilisateur `root`, afin de sécuriser la passerelle Knox et HDFS. |
| `ACCEPT_EULA`| Obligatoire pour la première utilisation de [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]| Définie sur « oui ». Si vous la définissez en tant que variable d’environnement, elle applique le CLUF à SQL Server et [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]. Si vous ne la définissez pas en tant que variable d’environnement, vous pouvez inclure `--accept-eula=yes` dans la première utilisation de la commande [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)].|
| `DOCKER_USERNAME` | Facultatif | Nom d’utilisateur utilisé pour accéder aux images de conteneur si elles sont stockées dans un dépôt privé. Pour plus d’informations sur l’utilisation d’un dépôt Docker privé pour le déploiement d’un cluster Big Data, consultez la rubrique [Déploiements hors connexion](deploy-offline.md).|
| `DOCKER_PASSWORD` | Facultatif |Mot de passe nécessaire pour accéder au dépôt privé ci-dessus. |

Ces variables d’environnement doivent être définies avant d’appeler `azdata bdc create`. Si une variable n’est pas définie, vous êtes invité à le faire.

L’exemple suivant montre comment définir les variables d’environnement pour Linux (bash) et Windows (PowerShell) :

```bash
export AZDATA_USERNAME=admin
export AZDATA_PASSWORD=<password>
export ACCEPT_EULA=yes
```

```PowerShell
SET AZDATA_USERNAME=admin
SET AZDATA_PASSWORD=<password>
```

> [!NOTE]
> Sur les clusters déployés avant SQL Server 2019 CU 5, vous devez utiliser l’utilisateur `root` pour la passerelle Knox avec le mot de passe ci-dessus. `root` est le seul utilisateur pris en charge dans cette authentification de base (nom d’utilisateur/mot de passe).
> [!INCLUDE [big-data-cluster-root-user](../includes/big-data-cluster-root-user.md)]
> Pour vous connecter à SQL Server avec l’authentification de base, utilisez les mêmes valeurs que les [variables d’environnement](#env) AZDATA_USERNAME et AZDATA_PASSWORD. 

Après avoir défini les variables d’environnement, vous devez exécuter `azdata bdc create` pour déclencher le déploiement. Cet exemple utilise le profil de configuration de cluster créé ci-dessus :

```bash
azdata bdc create --config-profile custom --accept-eula yes
```

Notez les consignes suivantes :

- Veillez à inclure le mot de passe entre guillemets doubles s’il contient des caractères spéciaux. Vous pouvez définir `AZDATA_PASSWORD` avec la valeur de votre choix, mais veillez à ce que le mot de passe soit suffisamment complexe et n’utilisez pas les caractères `!`, `&` et `'`. Notez que les délimiteurs de guillemets doubles fonctionnent uniquement dans les commandes bash.
- La connexion `AZDATA_USERNAME` est un administrateur système sur l’instance principale SQL Server dont la création a lieu lors de l’installation. Une fois le conteneur SQL Server créé, la variable d’environnement `AZDATA_PASSWORD` que vous avez spécifiée peut être découverte en exécutant `echo $AZDATA_PASSWORD` dans le conteneur. Pour des raisons de sécurité, modifiez le mot de passe en guise de bonne pratique.

## <a name="unattended-install"></a><a id="unattended"></a> Installation sans assistance

Pour un déploiement sans assistance, vous devez définir toutes les variables d’environnement nécessaires, utiliser un fichier de configuration et appeler la commande `azdata bdc create` avec le paramètre `--accept-eula yes`. Les exemples de la section précédente illustrent la syntaxe d’une installation sans assistance.

## <a name="monitor-the-deployment"></a><a id="monitor"></a> Superviser le déploiement

Durant l’amorçage du cluster, la fenêtre de commande du client retourne l’état du déploiement. Vous devez ensuite voir une série de messages indiquant que le processus de déploiement attend le pod du contrôleur :

```output
Waiting for cluster controller to start.
```

Après 15 à 30 minutes, vous devez être informé que le pod du contrôleur est en cours d’exécution :

```output
Cluster controller endpoint is available at 11.111.111.11:30080.
Cluster control plane is ready.
```

> [!IMPORTANT]
> L’ensemble du processus de déploiement peut durer longtemps en raison du temps nécessaire au téléchargement des images de conteneur pour les composants du cluster Big Data. Il ne devrait cependant pas prendre plusieurs heures. Si vous rencontrez des problèmes avec votre déploiement, consultez [Supervision et résolution des problèmes de [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]](cluster-troubleshooting-commands.md).

Une fois le déploiement terminé, vous êtes notifié dans la sortie :

```output
Cluster deployed successfully.
```

> [!TIP]
> Le nom par défaut du cluster Big Data déployé est `mssql-cluster`, sauf s’il est modifié par une configuration personnalisée.

## <a name="retrieve-endpoints"></a><a id="endpoints"></a> Récupérer des points de terminaison

Une fois le script de déploiement terminé, vous pouvez obtenir les adresses des points de terminaison externes du cluster Big Data en effectuant les étapes suivantes.

1. Après le déploiement, recherchez l’adresse IP du point de terminaison du contrôleur dans la sortie standard du déploiement ou la sortie EXTERNAL-IP de la commande `kubectl` suivante :

   ```bash
   kubectl get svc controller-svc-external -n <your-big-data-cluster-name>
   ```

   > [!TIP]
   > Si vous n’avez pas changé le nom par défaut durant le déploiement, utilisez `-n mssql-cluster` dans la commande précédente. `mssql-cluster` est le nom par défaut du cluster Big Data.

1. Connectez-vous au cluster Big Data avec [azdata login](../azdata/reference/reference-azdata.md). Affectez au paramètre `--endpoint` l’adresse IP externe du point de terminaison du contrôleur.

   ```bash
   azdata login --endpoint https://<ip-address-of-controller-svc-external>:30080 --username <user-name>
   ```

   Spécifiez le nom d’utilisateur et le mot de passe que vous avez configurés pour l’administrateur du cluster Big Data (AZDATA_USERNAME et AZDATA_PASSWORD) durant le déploiement.

   > [!TIP]
   > Si vous êtes l’administrateur du cluster Kubernetes et que vous avez accès au fichier de configuration du cluster (fichier de configuration Kube), vous pouvez configurer le contexte actuel pour qu’il pointe vers le cluster Kubernetes ciblé. Dans ce cas, vous pouvez vous connecter avec `azdata login -n <namespaceName>`, où `namespace` est le nom du cluster Big Data. Vous êtes invité à fournir des informations d’identification si elles ne sont pas spécifiées dans la commande de connexion.
   
1. Exécutez [azdata bdc endpoint list](../azdata/reference/reference-azdata-bdc-endpoint.md) pour obtenir une liste comprenant la description de chaque point de terminaison ainsi que les adresses IP et ports correspondants. 

   ```bash
   azdata bdc endpoint list -o table
   ```

   La liste suivante montre un exemple de sortie de cette commande :

   ```output
   Description                                             Endpoint                                                   Ip              Name               Port    Protocol
   ------------------------------------------------------  ---------------------------------------------------------  --------------  -----------------  ------  ----------
   Gateway to access HDFS files, Spark                     https://11.111.111.111:30443                               11.111.111.111  gateway            30443   https
   Spark Jobs Management and Monitoring Dashboard          https://11.111.111.111:30443/gateway/default/sparkhistory  11.111.111.111  spark-history      30443   https
   Spark Diagnostics and Monitoring Dashboard              https://11.111.111.111:30443/gateway/default/yarn          11.111.111.111  yarn-ui            30443   https
   Application Proxy                                       https://11.111.111.111:30778                               11.111.111.111  app-proxy          30778   https
   Management Proxy                                        https://11.111.111.111:30777                               11.111.111.111  mgmtproxy          30777   https
   Log Search Dashboard                                    https://11.111.111.111:30777/kibana                        11.111.111.111  logsui             30777   https
   Metrics Dashboard                                       https://11.111.111.111:30777/grafana                       11.111.111.111  metricsui          30777   https
   Cluster Management Service                              https://11.111.111.111:30080                               11.111.111.111  controller         30080   https
   SQL Server Master Instance Front-End                    11.111.111.111,31433                                       11.111.111.111  sql-server-master  31433   tcp
   HDFS File System Proxy                                  https://11.111.111.111:30443/gateway/default/webhdfs/v1    11.111.111.111  webhdfs            30443   https
   Proxy for running Spark statements, jobs, applications  https://11.111.111.111:30443/gateway/default/livy/v1       11.111.111.111  livy               30443   https
   ```

Vous pouvez également obtenir tous les points de terminaison de service déployés pour le cluster en exécutant la commande `kubectl` suivante :

```bash
kubectl get svc -n <your-big-data-cluster-name>
```

## <a name="verify-the-cluster-status"></a><a id="status"></a> Vérifier l’état du cluster

Après le déploiement, vous pouvez vérifier l’état du cluster à l’aide de la commande [azdata bdc status show](../azdata/reference/reference-azdata-bdc-status.md).

```bash
azdata bdc status show
```

> [!TIP]
> Pour exécuter les commandes d’état, vous devez d’abord vous connecter à l’aide de la commande `azdata login` (présentée dans la section précédente sur les points de terminaison).

Voici un exemple de sortie de cette commande :

```output
Bdc: ready                                                                                                                                                                                                          Health Status:  healthy
 ===========================================================================================================================================================================================================================================
 Services: ready                                                                                                                                                                                                     Health Status:  healthy
 -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
 Servicename    State    Healthstatus    Details

 sql            ready    healthy         -
 hdfs           ready    healthy         -
 spark          ready    healthy         -
 control        ready    healthy         -
 gateway        ready    healthy         -
 app            ready    healthy         -


 Sql Services: ready                                                                                                                                                                                                 Health Status:  healthy
 -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
 Resourcename    State    Healthstatus    Details

 master          ready    healthy         StatefulSet master is healthy
 compute-0       ready    healthy         StatefulSet compute-0 is healthy
 data-0          ready    healthy         StatefulSet data-0 is healthy
 storage-0       ready    healthy         StatefulSet storage-0 is healthy


 Hdfs Services: ready                                                                                                                                                                                                Health Status:  healthy
 -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
 Resourcename    State    Healthstatus    Details

 nmnode-0        ready    healthy         StatefulSet nmnode-0 is healthy
 storage-0       ready    healthy         StatefulSet storage-0 is healthy
 sparkhead       ready    healthy         StatefulSet sparkhead is healthy


 Spark Services: ready                                                                                                                                                                                               Health Status:  healthy
 -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
 Resourcename    State    Healthstatus    Details

 sparkhead       ready    healthy         StatefulSet sparkhead is healthy
 storage-0       ready    healthy         StatefulSet storage-0 is healthy


 Control Services: ready                                                                                                                                                                                             Health Status:  healthy
 -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
 Resourcename    State    Healthstatus    Details

 controldb       ready    healthy         -
 control         ready    healthy         -
 metricsdc       ready    healthy         DaemonSet metricsdc is healthy
 metricsui       ready    healthy         ReplicaSet metricsui is healthy
 metricsdb       ready    healthy         StatefulSet metricsdb is healthy
 logsui          ready    healthy         ReplicaSet logsui is healthy
 logsdb          ready    healthy         StatefulSet logsdb is healthy
 mgmtproxy       ready    healthy         ReplicaSet mgmtproxy is healthy


 Gateway Services: ready                                                                                                                                                                                             Health Status:  healthy
 -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
 Resourcename    State    Healthstatus    Details

 gateway         ready    healthy         StatefulSet gateway is healthy


 App Services: ready                                                                                                                                                                                                 Health Status:  healthy
 -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
 Resourcename    State    Healthstatus    Details

 appproxy        ready    healthy         ReplicaSet appproxy is healthy
```

Vous pouvez également obtenir un état plus détaillé avec les commandes suivantes :

- [azdata bdc control status show](../azdata/reference/reference-azdata-bdc-control-status.md) retourne l’état d’intégrité de tous les composants associés au service de gestion de contrôle
```
azdata bdc control status show
```
Exemple de sortie :
```output
Control: ready                                                                                                                                                                                                      Health Status:  healthy
 ===========================================================================================================================================================================================================================================
 Resources: ready                                                                                                                                                                                                    Health Status:  healthy
 -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
 Resourcename    State    Healthstatus    Details

 controldb       ready    healthy         -
 control         ready    healthy         -
 metricsdc       ready    healthy         DaemonSet metricsdc is healthy
 metricsui       ready    healthy         ReplicaSet metricsui is healthy
 metricsdb       ready    healthy         StatefulSet metricsdb is healthy
 logsui          ready    healthy         ReplicaSet logsui is healthy
 logsdb          ready    healthy         StatefulSet logsdb is healthy
 mgmtproxy       ready    healthy         ReplicaSet mgmtproxy is healthy
```

- `azdata bdc sql status show` retourne l’état d’intégrité de toutes les ressources qui ont un service SQL Server
```
azdata bdc sql status show
```
Exemple de sortie :
```output
Sql: ready                                                                                                                                                                                                          Health Status:  healthy
 ===========================================================================================================================================================================================================================================
 Resources: ready                                                                                                                                                                                                    Health Status:  healthy
 -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
 Resourcename    State    Healthstatus    Details

 master          ready    healthy         StatefulSet master is healthy
 compute-0       ready    healthy         StatefulSet compute-0 is healthy
 data-0          ready    healthy         StatefulSet data-0 is healthy
 storage-0       ready    healthy         StatefulSet storage-0 is healthy
```

> [!IMPORTANT]
> Lorsque vous utilisez le paramètre `--all`, la sortie de ces commandes contient des URL vers les tableaux de bord Kibana et Grafana pour une analyse plus détaillée.

Outre [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)], vous pouvez utiliser Azure Data Studio pour rechercher les points de terminaison et des informations d’état. Pour plus d’informations sur l’affichage de l’état du cluster avec [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] et Azure Data Studio, consultez [Guide pratique pour afficher l’état d’un cluster Big Data](view-cluster-status.md).

## <a name="connect-to-the-cluster"></a><a id="connect"></a> Se connecter au cluster

Pour plus d’informations sur la façon de se connecter au cluster Big Data, consultez [Se connecter à un cluster Big Data SQL Server avec Azure Data Studio](connect-to-big-data-cluster.md).

## <a name="next-steps"></a>Étapes suivantes

Pour en savoir plus sur le déploiement d’un cluster Big Data, consultez les ressources suivantes :

- [Configurer les paramètres de déploiement de clusters Big Data](deployment-custom-configuration.md).
- [Effectuer un déploiement hors connexion d’un cluster Big Data SQL Server](deploy-offline.md)
- [Atelier : Architecture des [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] Microsoft](https://github.com/Microsoft/sqlworkshops/tree/master/sqlserver2019bigdataclusters)