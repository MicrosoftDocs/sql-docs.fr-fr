---
title: 'Deploy : Notebook Azure Data Studio'
titleSuffix: SQL Server Big Data Clusters
description: Découvrez comment utiliser le code et la documentation d’un notebook à partir d’Azure Data Studio pour déployer un cluster Big Data SQL Server.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mihaelab
ms.metadata: seo-lt-2019
ms.date: 12/13/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: ac8d7bb761b95edea14d18008fbd96a968665725
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/01/2021
ms.locfileid: "99235717"
---
# <a name="deploy-sql-server-big-data-cluster-with-azure-data-studio-notebook"></a>Déployer un cluster Big Data SQL Server avec un notebook Azure Data Studio

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

[!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] fournit une extension pour Azure Data Studio qui inclut des notebooks de déploiement. Un notebook de déploiement comprend la documentation et le code que vous pouvez utiliser dans Azure Data Studio pour créer un cluster Big Data SQL Server.

Implémentés à l’origine sous forme de projet open source, les [notebooks](../azure-data-studio/notebooks/notebooks-guidance.md) ont été implémentés dans [Azure Data Studio](../azure-data-studio/download-azure-data-studio.md). Vous pouvez utiliser un marquage Markdown pour le texte des cellules de texte et un des noyaux disponibles pour écrire du code dans les cellules de code.

Vous pouvez utiliser des notebooks pour déployer des clusters Big Data pour [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)].

## <a name="prerequisites"></a>Prérequis

Les prérequis suivants sont obligatoires pour lancer également le notebook :

* Dernière version de la [build Azure Data Studio Insiders](https://github.com/microsoft/azuredatastudio#try-out-the-latest-insiders-build-from-master) installée

En plus de ce qui précède, le déploiement d’un cluster Big Data SQL Server 2019 nécessite également :

* [azdata](../azdata/install/deploy-install-azdata.md)
* [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/#install-kubectl-binary-using-native-package-management)
* [Azure CLI (en cas de déploiement sur Azure)](/cli/azure/install-azure-cli)

## <a name="launch-the-notebook"></a>Lancer le notebook

1. Lancez Azure Data Studio.

2. Sous l’onglet **Connexions**, sélectionnez le bouton de sélection ( **...** ), puis sélectionnez **Déployer SQL Server**.

   ![Déployer SQL Server](media/notebooks-deploy/deploy-notebooks.png)

3. Dans les options de déploiement, sélectionnez **SQL Server Big Data Cluster (Cluster Big Data SQL Server)** .

4. À partir **Deployment Target** (Cible de déploiement), sous **Options**, sélectionnez **New Azure Kubernetes Cluster** (Nouveau cluster Azure Kubernetes) ou **Existing Azure Kubernetes Service cluster** (Cluster Azure Kubernetes Service existant).

5. Acceptez la déclaration de confidentialité et les termes du contrat de licence

6. Cette boîte de dialogue permet également de vérifier si les outils nécessaires au type de déploiement SQL choisi existent sur l’hôte. Le bouton **Sélectionner** s’active uniquement en cas de réussite de la vérification des outils.

7. Sélectionnez le bouton **Sélectionner**. Cette action lance l’expérience de déploiement.

## <a name="set-deployment-configuration-template"></a>Définir le modèle de configuration de déploiement

Vous pouvez personnaliser les paramètres du profil de déploiement en suivant les instructions ci-dessous.

### <a name="target-configuration-template"></a>Modèle de configuration cible

Sélectionnez le modèle configuration cible parmi les modèles disponibles. Les profils disponibles sont filtrés en fonction du type de cible de déploiement choisi dans la boîte de dialogue précédente.

   ![Modèle de configuration de déploiement - Étape 1](media/notebooks-deploy/deployment-configuration-template.png)

### <a name="azure-settings"></a>Paramètres Azure

Si la cible de déploiement est un nouvel AKS, des informations supplémentaires telles que l’ID d’abonnement Azure, le groupe de ressources, le nom du cluster AKS, le nombre de machines virtuelles, la taille et d’autres informations sont nécessaires pour créer le cluster AKS.

   ![Paramètres Azure](media/notebooks-deploy/azure-settings.png)

Si la cible de déploiement est un cluster Kubernetes existant, l’Assistant vous invite à entrer le chemin du fichier config *kube* pour importer les paramètres de cluster Kubernetes. Vérifiez que le contexte de cluster approprié est sélectionné et que vous pouvez déployer le cluster Big Data SQL Server 2019.

   ![Contexte de cluster cible](media/notebooks-deploy/target-cluster-context.png)

### <a name="cluster-docker-and-ad-settings"></a>Paramètres de cluster, Docker et AD

1. Entrez le nom du cluster Big Data SQL Server 2019, le nom d’utilisateur administrateur et le mot de passe correspondant.
Remarque : Le même compte est utilisé pour le contrôleur et SQL Server.

   ![Paramètres de cluster](media/notebooks-deploy/cluster-settings.png)

2. Entrez les paramètres Docker appropriés

   ![Paramètres Docker](media/notebooks-deploy/docker-settings.png)

3. Si l’authentification AD est disponible, entrez les paramètres AD

   ![Paramètres Active Directory](media/notebooks-deploy/active-directory-settings.png)

### <a name="service-settings"></a>Paramètres de service

Cet écran contient des entrées pour divers paramètres, notamment la **mise à l’échelle**, les **points de terminaison**, le **stockage** et les autres **paramètres de stockage avancés**. Entrez les valeurs appropriées, puis sélectionnez **Suivant**.

#### <a name="scale-settings"></a>Paramètres de mise à l’échelle

Entrez le nombre d’instances de chacun des composants du cluster Big Data.

Une instance de Spark peut être incluse avec HDFS. Elle est incluse dans le pool de stockage ou est seule dans le pool Spark.

   ![Paramètres de service](media/notebooks-deploy/service-settings.png)

Pour plus d’informations sur chacun de ces composants, vous pouvez consulter les détails relatifs à l’[instance maître](concept-master-instance.md), au [pool de données](concept-data-pool.md), au [pool de stockage](concept-storage-pool.md) ou au [pool de calcul](concept-compute-pool.md).

#### <a name="endpoint-settings"></a>Paramètres de point de terminaison

Les points de terminaison par défaut ont été préremplis. Toutefois, vous pouvez les changer, si nécessaire.

   ![Paramètres de point de terminaison](media/notebooks-deploy/endpoint-settings.png)

#### <a name="storage-settings"></a>Paramètres de stockage

Les paramètres de stockage incluent la classe de stockage et la taille des revendications pour les données et les journaux. Vous pouvez appliquer les paramètres au pool de stockage, au pool de données et au pool SQL Server Master.

   ![Paramètres de stockage](media/notebooks-deploy/storage-settings.png)

#### <a name="advanced-storage-settings"></a>Paramètres de stockage avancés

Vous pouvez ajouter des paramètres de stockage supplémentaires sous **Paramètres de stockage avancés**

* Pool de stockage (HDFS)
* Pool de données
* SQL Server Master

   ![Paramètres de stockage avancés](media/notebooks-deploy/advanced-storage-settings.png)

### <a name="summary"></a>Résumé

Cet écran récapitule toutes les entrées fournies pour déployer le cluster Big Data SQL Server 2019. Vous pouvez télécharger les fichiers config via le bouton **Save config files (Enregistrer les fichiers config)** . Sélectionnez **Script to Notebook (Script dans un notebook)** pour créer un script de l’ensemble de la configuration de déploiement dans un notebook. Une fois le notebook ouvert, sélectionnez **Run Cells (Exécuter les cellules)** pour commencer à déployer le cluster Big Data SQL Server 2019 sur la cible sélectionnée.

   ![Résumé](media/notebooks-deploy/deploy-sql-server-big-data-cluster-on-a-new-AKS-cluster.png)

## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations sur le déploiement, consultez [Guide de déploiement pour les clusters Big Data SQL Server](deployment-guidance.md).