---
title: Surveiller les applications avec azdata et le tableau de bord Grafana
titleSuffix: SQL Server Big Data Clusters
description: Surveillance des applications avec azdata et Grafana sur un cluster Big Data SQL Server 2019.
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.metadata: seo-lt-2019
ms.date: 08/16/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 380a6c8ab8812ee6a8aae132cbc12125777e2960
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100047789"
---
# <a name="monitor-applications-with-azdata-and-grafana-dashboard"></a>Surveiller les applications avec azdata et le tableau de bord Grafana

Grafana est l’un des meilleurs outils de virtualisation cloud natifs ; vous pouvez l’utiliser pour fournir diverses métriques de surveillance de votre application exécutée dans Kubernetes.  

Cet article explique comment surveiller une application à l’intérieur d’un cluster Big Data SQL Server.

## <a name="prerequisites"></a>Prérequis

- [Cluster Big Data SQL Server 2019](deployment-guidance.md)
- [[!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]](../azdata/install/deploy-install-azdata.md)

## <a name="capabilities"></a>Fonctionnalités

Dans SQL Server 2019, vous pouvez créer, supprimer, décrire, initialiser, exécuter et mettre à jour votre application. Le tableau suivant décrit les commandes de déploiement d’application que vous pouvez utiliser avec **azdata**.

|Commande |Description |
|:---|:---|
|`azdata bdc endpoint list` | Lister les points de terminaison du cluster Big Data. |


Vous pouvez utiliser l’exemple suivant pour répertorier le point de terminaison du **tableau de bord Grafana** :

```bash
azdata bdc endpoint list --endpoint-name metricsui 
```

La sortie vous donnera le point de terminaison, que vous pouvez utiliser pour vous connecter en utilisant le nom d’utilisateur et le mot de passe de votre cluster. 

![Tableau de bord Grafana](media/big-data-cluster-monitor-apps/grafana-dashboard-endpoint.png)


Lorsque vous ouvrez le tableau de bord, accédez à aux  **Métriques des applications hôtes**, où vous obtiendrez plus d’informations à jour sur votre application.  

![Métriques des applications hôtes](media/big-data-cluster-monitor-apps/host-apps-metrics.png)


Pour obtenir plus d’informations sur un pod particulier de l’application (dans certains cas, vous avez plusieurs copies de votre application), accédez à  **Métriques des pods hôtes**  et choisissez un pod pour obtenir une vue des métriques comme suit :  

![Métriques des pods hôtes](media/big-data-cluster-monitor-apps/host-pods-metrics.png) 


## <a name="next-steps"></a>Étapes suivantes

Vous pouvez également consulter d’autres [Exemples de déploiement d’application](https://aka.ms/sql-app-deploy).

Pour plus d’informations sur les [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)], consultez [Que sont les [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)] ?](big-data-cluster-overview.md).