---
title: Analyser le cluster avec le tableau de bord Grafana
titleSuffix: SQL Server Big Data Clusters
description: Analyse du cluster avec le tableau de bord Grafana sur le cluster Big Data SQL Server 2019.
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.metadata: seo-lt-2019
ms.date: 09/22/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 6b9e97e90e844391f62d920b64f7a09de5b6e7ea
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100047729"
---
# <a name="monitor-cluster-with-azdata-and-grafana-dashboard"></a>Analyser le cluster avec azdata et le tableau de bord Grafana

Cet article explique comment surveiller une application à l’intérieur d’un cluster Big Data SQL Server.

## <a name="prerequisites"></a>Prérequis

- [Cluster Big Data SQL Server 2019](deployment-guidance.md)
- [Utilitaire de ligne de commande azdata](../azdata/install/deploy-install-azdata.md)

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

Les valeurs `nodeMetricsUrl` et `sqlMetricsUrl` sont liées à un tableau de bord Grafana pour superviser les métriques de nœuds Kubernetes et les métriques de service de cluster Big Data :

![Tableau de bord Grafana](./media/view-cluster-status/grafana-dashboard.png)

![SQL](./media/view-cluster-status/grafana-sql-status.png)



## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations sur les [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)], consultez [Que sont les [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)] ?](big-data-cluster-overview.md).