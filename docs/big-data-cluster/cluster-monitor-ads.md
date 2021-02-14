---
title: Analyser le cluster avec Azure Data Studio
titleSuffix: SQL Server Big Data Clusters
description: Analyse du cluster avec Azure Data Studio sur le cluster Big Data SQL Server 2019.
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.metadata: seo-lt-2019
ms.date: 09/22/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: d03dc9c26f33fd512e035ab3cc9e167e1d9ef7cc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100047739"
---
# <a name="monitor-cluster-status-with-azure-data-studio"></a>Analyser l’état du cluster avec Azure Data Studio

Cet article explique comment afficher l’état d’un cluster Big Data à l’aide d’Azure Data Studio.

## <a name="use-azure-data-studio"></a><a id="datastudio"></a> Utiliser Azure Data Studio

Après avoir téléchargé la dernière **build Insiders** d’[Azure Data Studio](../azure-data-studio/download-azure-data-studio.md), vous pouvez accéder au tableau de bord Cluster Big Data SQL Server pour afficher les points de terminaison de service et l’état d’un cluster Big Data. Certaines des fonctionnalités présentées ci-dessous ne sont disponibles que dans la build Insiders d’Azure Data Studio.

1. Commencez par créer une connexion à votre cluster Big Data dans Azure Data Studio. Pour plus d’informations, consultez [Se connecter à un cluster Big Data SQL Server avec Azure Data Studio](connect-to-big-data-cluster.md).

1. Cliquez avec le bouton droit sur le point de terminaison du cluster Big Data, puis cliquez sur **Manage** (Gérer).

   ![Clic droit, puis Manage](media/view-cluster-status/right-click-manage.png)

1. Sélectionnez l’onglet **Cluster Big Data SQL Server** pour accéder au tableau de bord du cluster Big Data.

   ![Tableau de bord du cluster Big Data](media/view-cluster-status/bdc-dashboard.png)

### <a name="service-endpoints"></a>Points de terminaison de service

Il est important de pouvoir accéder facilement aux différents services au sein d’un cluster Big Data. Le tableau de bord du cluster Big Data fournit une table de points de terminaison de service qui vous permet de voir et de copier les points de terminaison de service.

![points de terminaison de service](media/view-cluster-status/service-endpoints.png)

Ces services listent les points de terminaison qui peuvent être copiés et collés quand vous avez besoin du point de terminaison pour vous connecter à ces services. Par exemple, vous pouvez cliquer sur l’icône de copie à droite du point de terminaison, puis la coller dans une fenêtre de texte demandant ce point de terminaison. Le point de terminaison Service de gestion de cluster est nécessaire pour exécuter le [notebook cluster status](#notebook).

### <a name="dashboards"></a>Tableaux de bord

La table des points de terminaison de service expose également plusieurs tableaux de bord de supervision :

- Metrics (Métriques) (Grafana)
- Logs (Journaux) (Kibana)
- Spark Job Monitoring (Supervision des travaux Spark)
- Spark Resource Management (Gestion des ressources Spark)

Vous pouvez cliquer directement sur ces liens. Vous serez invité à vous authentifier lors de l’accès à ces tableaux de bord. Pour les tableaux de bord des métriques et des journaux, fournissez les informations d’identification d’administrateur du contrôleur que vous avez définies au moment du déploiement à l’aide des variables d’environnement **AZDATA_USERNAME** et **AZDATA_PASSWORD**. Les tableaux de bord Spark utilisent des informations d’identification de passerelle (Knox) : l’identité Active Directory dans un cluster intégré à AD ou **AZDATA_USERNAME** et **AZDATA_PASSWORD** si l’authentification de base est utilisée dans votre cluster.

[!INCLUDE [big-data-cluster-root-user](../includes/big-data-cluster-root-user.md)]

### <a name="cluster-status-notebook"></a><a id="notebook"></a> Notebook Cluster Status

1. Vous pouvez également voir l’état du cluster Big Data en lançant le notebook Cluster Status (État du cluster). Pour lancer le notebook, cliquez sur la tâche **Cluster Status**.

    ![lancer](media/view-cluster-status/cluster-status-launch.png)

2. Avant de commencer, vous avez besoin des éléments suivants :

    - Nom du cluster Big Data
    - Nom d’utilisateur du contrôleur
    - Mot de passe du contrôleur
    - Points de terminaison du contrôleur

    Le nom du cluster Big Data par défaut est **mssql-cluster**, sauf si vous l’avez personnalisé durant votre déploiement. Vous trouverez le point de terminaison du contrôleur dans la table des points de terminaison de service du tableau de bord du cluster Big Data. Le point de terminaison est listé comme **Cluster Management Service**. Si vous ne connaissez pas les informations d’identification, demandez-les à l’administrateur ayant déployé votre cluster.

3. Cliquez sur **Exécuter les cellules** dans la barre d’outils supérieure.

4. À l’invite, entrez vos informations d’identification. Appuyez sur Entrée après chaque information d’identification : nom du cluster Big Data, nom d’utilisateur du contrôleur et mot de passe du contrôleur.

    > [!Note]
    > Si vous ne disposez pas d’un fichier de configuration avec votre cluster Big Data, vous êtes invité à indiquer le point de terminaison du contrôleur. Tapez-le ou collez-le, puis appuyez sur Entrée pour continuer.

5. Si vous avez réussi à vous connecter, le reste du notebook montre la sortie de chaque composant du cluster Big Data. Quand vous souhaitez réexécuter une certaine cellule de code, pointez sur celle-ci, puis cliquez sur l’icône **Exécuter**.


## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations sur les [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)], consultez [Que sont les [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)] ?](big-data-cluster-overview.md).