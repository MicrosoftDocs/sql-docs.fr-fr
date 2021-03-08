---
title: Extraire les journaux de cluster avec le tableau de bord Kibana
titleSuffix: SQL Server Big Data Clusters
description: Analyse du cluster avec le tableau de bord Kibana sur le cluster Big Data SQL Server 2019.
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.metadata: seo-lt-2019
ms.date: 02/25/2021
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 7c26dc7e193d3dd5ad688af4d4dc79e2dd3eeea7
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/04/2021
ms.locfileid: "101835966"
---
# <a name="check-out-cluster-logs--with-kibana-dashboard"></a>Extraire les journaux de cluster avec le tableau de bord Kibana

Cet article explique comment analyser une application à l’intérieur de [!INCLUDE[ssbigdataclusters-ss-nover](../includes/ssbigdataclusters-ss-nover.md)].

## <a name="prerequisites"></a>Prérequis

- [[!INCLUDE[ssbigdataclusters-ss-nover](../includes/ssbigdataclusters-ss-nover.md)]](deployment-guidance.md)
- [Utilitaire de ligne de commande azdata](../azdata/install/deploy-install-azdata.md)

## <a name="capabilities"></a>Fonctionnalités

Dans [!INCLUDE[sssql19-md](../includes/sssql19-md.md)], vous pouvez créer, supprimer, décrire, initialiser, exécuter une liste et mettre à jour votre application. Le tableau suivant décrit les commandes de déploiement d’application que vous pouvez utiliser avec **azdata**.

|Commande |Description |
|:---|:---|
|`azdata bdc endpoint list` | Répertorie les points de terminaison pour le [!INCLUDE[ssbigdataclusters-ss-nover](../includes/ssbigdataclusters-ss-nover.md)]. |


Vous pouvez utiliser l’exemple suivant pour répertorier le point de terminaison du **tableau de bord Kibana** :

```bash
azdata bdc endpoint list --endpoint-name logsui 
```

La sortie vous donnera le point de terminaison, que vous pouvez utiliser pour vous connecter en utilisant le nom d’utilisateur et le mot de passe de votre cluster. 

![Tableau de bord Kibana](media/big-data-cluster-monitor-cluster/kibana-dashboard-endpoint.png)


Le lien vers un tableau de bord Kibana :

![Tableau de bord kibana](./media/view-cluster-status/kibana-dashboard.png)

> [!NOTE]
> L’ancien navigateur Microsoft Edge étant incompatible avec Kibana, vous devez utiliser le navigateur Chromium Edge pour que le tableau de bord s’affiche correctement. Une page vide s’affiche lors du chargement des tableaux de bord à l’aide d’un navigateur non pris en charge, consultez [navigateurs pris en charge pour Kibana](https://www.elastic.co/support/matrix#matrix_browsers).

## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations sur les [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)], consultez [Que sont les [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)] ?](big-data-cluster-overview.md).


