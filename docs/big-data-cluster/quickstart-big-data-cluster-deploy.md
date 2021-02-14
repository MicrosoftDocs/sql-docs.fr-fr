---
title: Déployer avec un script Python
titleSuffix: SQL Server Big Data Clusters
description: Découvrez comment utiliser un script de déploiement pour déployer des clusters Big Data SQL Server sur Azure Kubernetes Service (AKS).
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mihaelab
ms.date: 06/22/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 9c0ccb64a67d547b2027423dbc3571e44b3b21ef
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100046167"
---
# <a name="use-a-python-script-to-deploy-a-sql-server-big-data-cluster-on-azure-kubernetes-service-aks"></a>Utiliser un script Python pour déployer un cluster Big Data SQL Server sur Azure Kubernetes Service (AKS)

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Dans ce tutoriel, vous utilisez un exemple de script de déploiement Python pour déployer des [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)] sur AKS (Azure Kubernetes Service).

> [!TIP]
> AKS n’est qu’une option pour héberger Kubernetes pour votre cluster Big Data. Pour découvrir les autres options de déploiement et savoir comment les personnaliser, consultez [Guide pratique pour déployer [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] sur Kubernetes](deployment-guidance.md).

Le déploiement de cluster Big Data par défaut utilisé ici se compose d’une instance principale SQL, d’une instance de pool de calcul, de deux instances de pool de données et de deux instances de pool de stockage. Les données sont conservées avec des volumes persistants Kubernetes qui utilisent les classes de stockage par défaut d’AKS. La configuration par défaut utilisée dans ce tutoriel est adaptée aux environnements de développement et de test.

## <a name="prerequisites"></a>Prérequis

- Un abonnement Azure.
- [Outils Big Data](deploy-big-data-tools.md) :
   - **azdata**
   - **kubectl**
   - **Azure Data Studio**
   - **Extension SQL Server 2019**
   - **Azure CLI**

## <a name="log-in-to-your-azure-account"></a>Se connecter à votre compte Azure

Le script utilise Azure CLI pour automatiser la création d’un cluster AKS. Avant d’exécuter le script, vous devez vous connecter à votre compte Azure avec Azure CLI au moins une fois. Exécutez la commande suivante à partir d’une invite de commandes.

```
az login
```

## <a name="download-the-deployment-script"></a>Télécharger le script de déploiement

Ce tutoriel automatise la création du cluster Big Data sur AKS à l’aide d’un script Python **deploy-sql-big-data-aks.py**. Si vous avez déjà installé Python pour **azdata**, vous devez normalement pouvoir exécuter le script avec succès dans ce tutoriel. 

Dans une invite Windows PowerShell ou Linux bash, exécutez la commande suivante pour télécharger le script de déploiement à partir de GitHub.

```
curl -o deploy-sql-big-data-aks.py "https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/sql-big-data-cluster/deployment/aks/deploy-sql-big-data-aks.py"
```

## <a name="run-the-deployment-script"></a>Exécuter le script de déploiement

Procédez comme suit pour exécuter le script de déploiement dans une invite bash Windows PowerShell ou Linux. Ce script crée un service AKS dans Azure, puis déploie un cluster Big Data SQL Server 2019 sur AKS. Vous pouvez également modifier le script avec d’autres [variables d’environnement](deployment-guidance.md#configfile) pour créer un déploiement personnalisé.

1. Exécutez le script avec la commande suivante :

   ```
   python deploy-sql-big-data-aks.py
   ```

   > [!NOTE]
   > Si vous avez à la fois python3 et python2 sur votre machine cliente et dans le chemin, vous devez exécuter la commande avec python3 : `python3 deploy-sql-big-data-aks.py`.

1. Quand vous y êtes invité, entrez les informations suivantes :

   | Valeur | Description |
   |---|---|
   | **ID d’abonnement Azure** | ID d’abonnement Azure à utiliser pour AKS. Vous pouvez lister tous vos abonnements et leur ID en exécutant `az account list` à partir d’une autre ligne de commande. |
   | **Groupe de ressources Azure** | Nom du groupe de ressources Azure à créer pour le cluster AKS. |
   | **Région Azure** | Région Azure pour le nouveau cluster AKS (**westus** par défaut). |
   | **Taille de machine** | [Taille de machine](/azure/virtual-machines/windows/sizes) à utiliser pour les nœuds du cluster AKS (**Standard_D16s_v3** par défaut). |
   | **Nœuds worker** | Nombre de nœuds worker du cluster AKS (**1** par défaut). |
   | **Nom du cluster** | Nom du cluster AKS et du cluster Big Data. Le nom de votre cluster Big Data ne doit contenir que des caractères alphanumériques minuscules et aucun espace. (**sqlbigdata** par défaut). |
   | **Mot de passe** | Mot de passe pour le contrôleur, la passerelle HDFS/Spark et l’instance principale (**MySQLBigData2019** par défaut). |
   | **Nom d’utilisateur** | Nom d’utilisateur de l’utilisateur du contrôleur (**admin** par défaut). |

   > [!IMPORTANT]
   > La taille de machine **Standard_D16s_v3** par défaut peut ne pas être disponible dans toutes les régions Azure. Si vous sélectionnez une autre taille de machine, veillez à ce que le nombre total de disques pouvant être attachés sur les nœuds du cluster soit supérieur ou égal à 24. Chaque revendication de volume persistant dans le cluster nécessite un disque attaché. Actuellement, le cluster Big Data nécessite 24 revendications de volumes persistants.
   >
   >Exécutez la commande suivante pour identifier les types de machines virtuelles disponibles.
   >
   >```azurecli-interactive
   >az vm list-sizes --query "sort_by(@,&name)[?contains(name,'Standard_D16s')]" -l westus2 -o table
   >```

   > [!NOTE]
   > Le compte SQL Server `sa` est désactivé durant le déploiement de clusters Big Data. Une nouvelle connexion sysadmin est provisionnée dans l’instance maître de SQL Server avec le même nom que celui spécifié pour l’entrée du **Nom d’utilisateur** et le mot de passe correspondant à l’entrée du **Mot de passe**. Les mêmes valeurs correspondant à **Nom d’utilisateur** et **Mot de passe** sont utilisées pour provisionner un utilisateur administrateur de contrôleur. Le seul utilisateur pris en charge pour la passerelle (Knox) sur des clusters déployés avant SQL Server 2019 CU5 est **racine** et le mot de passe est le même que ci-dessus.
   >[!INCLUDE [big-data-cluster-root-user](../includes/big-data-cluster-root-user.md)]

1. Le script commence par créer un cluster AKS avec les paramètres que vous avez spécifiés. Cette étape prend plusieurs minutes.

## <a name="monitor-the-status"></a>Superviser l’état

Une fois que le script a créé le cluster AKS, il définit les variables d’environnement nécessaires avec les paramètres que vous avez spécifiés précédemment. Il appelle ensuite **azdata** pour déployer le cluster Big Data sur AKS.

La fenêtre de commande du client indique l’état du déploiement. Vous devez ensuite voir une série de messages indiquant que le processus de déploiement attend le pod du contrôleur :

```output
2018-11-15 15:42:02.0209 UTC | INFO | Waiting for controller pod to be up...
```

Après 10 à 20 minutes, vous devez être informé que le pod du contrôleur est en cours d’exécution :

```output
2018-11-15 15:50:50.0300 UTC | INFO | Controller pod is running.
2018-11-15 15:50:50.0585 UTC | INFO | Controller Endpoint: https://111.111.111.111:30080
```

> [!IMPORTANT]
> L’ensemble du processus de déploiement peut durer longtemps en raison du temps nécessaire au téléchargement des images de conteneur pour les composants du cluster Big Data. Il ne devrait cependant pas prendre plusieurs heures. Si vous rencontrez des problèmes avec votre déploiement, consultez [Supervision et résolution des problèmes de [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]](cluster-troubleshooting-commands.md).

## <a name="inspect-the-cluster"></a>Inspecter le cluster

À tout moment pendant le déploiement, vous pouvez utiliser **kubectl** ou **azdata** pour inspecter l’état et les détails sur le cluster Big Data en cours d’exécution.

### <a name="use-kubectl"></a>Utiliser kubectl

Ouvrez une nouvelle fenêtre de commande pour utiliser **kubectl** pendant le processus de déploiement.

1. Exécutez la commande suivante pour obtenir un récapitulatif de l’état de l’ensemble du cluster :

   ```
   kubectl get all -n <your-big-data-cluster-name>
   ```

   > [!TIP]
   > Si vous n’avez pas modifié le nom du cluster Big Data, le script utilise **sqlbigdata** par défaut.

1. Examinez les services kubernetes et leurs points de terminaison internes et externes avec la commande **kubectl** suivante :

   ```
   kubectl get svc -n <your-big-data-cluster-name>
   ```

1. Vous pouvez également inspecter l’état des pods kubernetes avec la commande suivante :

   ```
   kubectl get pods -n <your-big-data-cluster-name>
   ```

1. Trouvez plus d’informations sur un pod spécifique avec la commande suivante :

   ```
   kubectl describe pod <pod name> -n <your-big-data-cluster-name>
   ```

> [!TIP]
> Pour plus d’informations sur la supervision et la résolution des problèmes d’un déploiement, consultez [Supervision et résolution des problèmes de [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]](cluster-troubleshooting-commands.md).

## <a name="connect-to-the-cluster"></a>Se connecter au cluster

Une fois le script déploiement terminé, la sortie vous indique la réussite :

```output
2018-11-15 16:10:25.0583 UTC | INFO | Cluster state: Ready
2018-11-15 16:10:25.0583 UTC | INFO | Cluster deployed successfully.
```

Le cluster Big Data SQL Server est maintenant déployé sur AKS. Vous pouvez maintenant utiliser Azure Data Studio pour vous connecter au cluster. Pour plus d’informations, consultez [Se connecter à un cluster Big Data SQL Server avec Azure Data Studio](connect-to-big-data-cluster.md).

## <a name="clean-up"></a>Nettoyer

Si vous testez [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] dans Azure, vous devez supprimer le cluster AKS une fois que vous avez fini pour éviter les charges inattendues. Ne supprimez pas le cluster si vous envisagez de continuer à l’utiliser.

> [!WARNING]
> Les étapes suivantes détruisent le cluster AKS, ce qui supprime également le cluster Big Data SQL Server. Si vous voulez conserver des bases de données ou des données HDFS, sauvegardez-les avant de supprimer le cluster.

Exécutez la commande Azure CLI suivante pour supprimer le cluster Big Data et le service AKS dans Azure (remplacez `<resource group name>` par le **groupe de ressources Azure** que vous avez spécifié dans le script de déploiement) :

```azurecli
az group delete -n <resource group name>
```

## <a name="next-steps"></a>Étapes suivantes

Le script de déploiement a configuré Azure Kubernetes Service et a également déployé un cluster Big Data SQL Server 2019. Vous pouvez également choisir de personnaliser des déploiements futurs via des installations manuelles. Pour en savoir plus sur le déploiement des clusters Big Data et sur la personnalisation des déploiements, consultez [Guide pratique pour déployer [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] sur Kubernetes](deployment-guidance.md).

Maintenant que le cluster Big Data SQL Server est déployé, vous pouvez charger des exemples de données et explorer les tutoriels :

> [!div class="nextstepaction"]
> [Tutoriel : Charger un exemple de données dans un cluster Big Data SQL Server 2019](tutorial-load-sample-data.md)
