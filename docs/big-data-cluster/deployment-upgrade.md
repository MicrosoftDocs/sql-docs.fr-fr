---
title: Effectuer une mise à niveau vers une nouvelle version
titleSuffix: SQL Server Big Data Clusters
description: Découvrez comment mettre à niveau des clusters Big Data SQL Server vers une nouvelle version.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mihaelab
ms.date: 09/02/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 48edd5e76a87a866a2090329f144c5a7dc0934a2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100046879"
---
# <a name="how-to-upgrade-big-data-clusters-2019"></a>Guide pratique pour effectuer la mise à niveau de [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Le chemin de mise à niveau dépend de la version actuelle de SQL Server Big Data Cluster (BDC). Pour effectuer une mise à niveau à partir d’une version prise en charge, comme une mise à jour à disponibilité générale (GDR), une mise à jour cumulative (CU) ou une mise à jour QFE (Quick Fix Engineering), vous pouvez mettre à niveau sur place. La mise à niveau sur place à partir d’une version CTP (Customer Technology Preview) ou d’une version Release Candidate de BDC n’est pas prise en charge. Vous devez supprimer et recréer le cluster. Les sections suivantes décrivent les étapes pour chaque scénario :

- [Mise à niveau à partir d’une version prise en charge](#upgrade-from-supported-release)
- [Mettre à jour un déploiement BDC à partir d’une version CTP ou Release Candidate](#update-a-bdc-deployment-from-ctp-or-release-candidate)

>[!NOTE]
>La première version prise en charge des clusters Big Data est SQL Server 2019 GDR1.

## <a name="upgrade-release-notes"></a>Notes de publication de mise à niveau

Avant de continuer, consultez les [notes de mise à niveau pour connaître les problèmes connus](release-notes-big-data-cluster.md#known-issues).

## <a name="upgrade-from-supported-release"></a>Mise à niveau à partir d’une version prise en charge

Cette section explique comment mettre à niveau une version de SQL Server BDC prise en charge (à compter de SQL Server 2019 GDR1) vers une version prise en charge plus récente.

1. Vérifiez l’absence de sessions Livy actives.

   Assurez-vous qu’aucune session Livy n’est active et qu’aucun travail de traitement par lots n’est en cours d’exécution dans Azure Data Studio. Pour ce faire, vous pouvez utiliser la commande `curl` ou un navigateur pour demander ces URL :

    - `<your-gateway-endpoint>/gateway/default/livy/v1/sessions`
    - `<your-gateway-endpoint>/gateway/default/livy/v1/batches`

1. Sauvegardez l’instance maître SQL Server.

1. Sauvegardez HDFS.

   ```
   azdata bdc hdfs cp --from-path <path> --to-path <path>
   ```
   
   Exemple : 

   ```
   azdata bdc hdfs cp --from-path hdfs://user/hive/warehouse/%%D --to-path ./%%D
   ```

1. Mettez à jour [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)].

   Suivez les instructions d’installation de [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]. 
   - [Windows installer](../azdata/install/deploy-install-azdata-installer.md)
   - [Linux avec apt](../azdata/install/deploy-install-azdata-linux-package.md)
   - [Linux avec yum](../azdata/install/deploy-install-azdata-yum.md)
   - [Linux avec zypper](../azdata/install/deploy-install-azdata-zypper.md)

   >[!NOTE]
   >Si [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] a été installé avec `pip` vous devez le supprimer manuellement avant de procéder à l’installation avec Windows Installer ou le gestionnaire de package Linux.

1. Mettez à jour le cluster Big Data.

   ```
   azdata bdc upgrade -n <clusterName> -t <imageTag> -r <containerRegistry>/<containerRepository>
   ```

   Par exemple, le script suivant utilise la balise d’image `2019-CU6-ubuntu-16.04` :

   ```
   azdata bdc upgrade -n bdc -t 2019-CU6-ubuntu-16.04 -r mcr.microsoft.com/mssql/bdc
   ```

>[!NOTE]
>Les balises d’image les plus récentes sont disponibles dans les [notes de publication des clusters Big Data SQL Server 2019](release-notes-big-data-cluster.md).

>[!IMPORTANT]
>Si vous utilisez un dépôt privé pour pré-extraire les images pour le déploiement ou la mise à niveau de BDC, assurez-vous que les images de build actuelles et les images de build >cibles se trouvent dans le dépôt privé. Cela permet une restauration réussie, si nécessaire. En outre, si vous avez modifié les >informations d’identification du dépôt privé depuis le déploiement d’origine, mettez à jour les variables d'environnement correspondantes DOCKER_PASSWORD et >DOCKER_USERNAME. La mise à niveau avec des dépôts privés différents pour les builds actuelles et cibles n’est pas prise en charge.

### <a name="increase-the-timeout-for-the-upgrade"></a>Augmentez le délai d’expiration de la mise à niveau

Un délai d’expiration peut se produire si certains composants ne sont pas mis à niveau dans le temps imparti. Le code suivant montre à quoi peut ressembler l’échec :

   ```
   >azdata.EXE bdc upgrade --name <mssql-cluster>
   Upgrading cluster to version 15.0.4003

   NOTE: Cluster upgrade can take a significant amount of time depending on
   configuration, network speed, and the number of nodes in the cluster.

   Upgrading Control Plane.
   Control plane upgrade failed. Failed to upgrade controller.
   ```

Pour augmenter les délais d’expiration d’une mise à niveau, utilisez les paramètres **--controller-timeout** et **--component-timeout** afin de spécifier des valeurs plus élevées quand vous effectuez la mise à niveau. Cette option est disponible à compter de SQL Server 2019 CU2 uniquement. Exemple :

   ```bash
   azdata bdc upgrade -t 2019-CU6-ubuntu-16.04 --controller-timeout=40 --component-timeout=40 --stability-threshold=3
   ```
**--controller-timeout** spécifie le nombre de minutes à attendre avant la fin de la mise à niveau du contrôleur ou de la base de données du contrôleur.
**--component-timeout** spécifie le délai d’exécution de chaque phase suivante de la mise à niveau.

Pour augmenter les délais d’expiration d’une mise à niveau antérieure à la version SQL Server 2019 CU2, modifiez le mappage de configuration de mise à niveau. Pour modifier le mappage de configuration de mise à niveau :

Exécutez la commande suivante :

   ```bash
   kubectl edit configmap controller-upgrade-configmap
   ```

Modifiez les champs suivants :

   **controllerUpgradeTimeoutInMinutes** Spécifie le nombre de minutes à attendre avant la fin de la mise à niveau du contrôleur ou de la base de données du contrôleur. La valeur par défaut est 5. Mettez à jour vers au moins 20.
   **totalUpgradeTimeoutInMinutes** : Spécifie le délai combiné du contrôleur et de la base de données du contrôleur pour terminer la mise à niveau (contrôleur + base de données contrôleur). La valeur par défaut est 10. Mettez à jour vers au moins 40.
   **componentUpgradeTimeoutInMinutes** : Désigne la durée d’exécution de chaque phase suivante de la mise à niveau. La valeur par défaut est 30. Mettez à jour vers 45.

Enregistrez et quittez.

## <a name="update-a-bdc-deployment-from-ctp-or-release-candidate"></a>Mettre à jour un déploiement BDC à partir d’une version CTP ou Release Candidate

La mise à niveau sur place à partir d’une version CTP ou Release Candidate des clusters Big Data SQL Server n’est pas prise en charge. La section suivante explique comment supprimer et recréer manuellement le cluster.

### <a name="backup-and-delete-the-old-cluster"></a>Sauvegarder et supprimer l’ancien cluster

Il n’existe aucune mise à niveau sur place pour les clusters Big Data déployés avant la version SQL Server 2019 GDR1. La seule façon de mettre à niveau vers une nouvelle version consiste à supprimer le cluster et à le recréer manuellement. Chaque version release comporte une version unique d’[!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)], qui n’est pas compatible avec la version précédente. En outre, si une nouvelle image de conteneur est téléchargée sur un cluster déployé avec une version antérieure différente, l’image la plus récente peut ne pas être compatible avec les anciennes images sur le cluster. L’image la plus récente est tirée (pull) si vous utilisez l’étiquette d’image `latest` dans le fichier config de déploiement pour les paramètres de conteneur. Par défaut, chaque version release a une étiquette d’image spécifique qui correspond à la version release de SQL Server. Pour effectuer une mise à niveau vers la dernière version, procédez comme suit :

1. Avant de supprimer l’ancien cluster, sauvegardez les données sur l’instance maître SQL Server et sur HDFS. Pour l’instance maître SQL Server, vous pouvez utiliser [Sauvegarde et restauration d’une base de données SQL Server](data-ingestion-restore-database.md). Pour HDFS, vous [pouvez copier les données avec `curl`](data-ingestion-curl.md).

1. Supprimez l’ancien cluster à l’aide de la commande `azdata delete cluster`.

   ```bash
    azdata bdc delete --name <old-cluster-name>
   ```

   > [!Important]
   > Utilisez la version d’[!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] qui correspond à votre cluster. Ne supprimez pas un ancien cluster avec la version la plus récente d’[!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)].

   > [!Note]
   > Si vous exécutez une commande `azdata bdc delete`, tous les objets créés dans l’espace de noms identifié par le nom du cluster Big Data sont supprimés, mais pas l’espace de noms lui-même. Vous pouvez réutiliser l’espace de noms pour d’autres déploiements, à condition qu’il soit vide et qu’aucune autre application n’y ait été créée.

1. Désinstallez l’ancienne version d’[!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)].

   ```powershell
   pip3 uninstall -r https://azdatacli.blob.core.windows.net/python/azdata/2019-rc1/requirements.txt
   ```

1. Installez la dernière version de [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]. Les commandes suivantes permettent d’installer [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] à partir de la dernière version release :

   **Windows :**

   ```powershell
   pip3 install -r https://aka.ms/azdata
   ```

   **Linux :**

   ```bash
   pip3 install -r https://aka.ms/azdata --user
   ```

   > [!IMPORTANT]
   > Pour chaque version release, le chemin de la version `n-1` d’[!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] change. Même si vous avez déjà installé [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)], vous devez le réinstaller à partir du chemin le plus récent avant de créer le cluster.

### <a name="verify-the-azdata-version"></a><a id="azdataversion"></a> Vérifier la version d’azdata

Avant de déployer un nouveau cluster Big Data, vérifiez que vous utilisez la dernière version de [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] avec le paramètre `--version` :

```bash
azdata --version
```

### <a name="install-the-new-release"></a>Installer la nouvelle version

Après avoir supprimé le cluster Big Data précédent et installé la dernière version d’[!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)], déployez le nouveau cluster Big Data à l’aide des instructions de déploiement actuelles. Pour plus d’informations, consultez [Guide pratique pour déployer [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] sur Kubernetes](deployment-guidance.md). Ensuite, restaurez les bases de données ou les fichiers requis.

## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations sur les clusters Big Data, consultez [Que sont les [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] ?](big-data-cluster-overview.md).