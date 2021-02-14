---
title: Exécuter des applications avec azdata
titleSuffix: SQL Server Big Data Clusters
description: Exécution d’applications avec azdata sur les clusters Big Data SQL Server 2019.
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.metadata: seo-lt-2019
ms.date: 08/16/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: b83c79398dbcc6c7d55eb0ddfe6cb1d8f0d398ed
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100039589"
---
# <a name="run-apps-with-azdata---sql-server-big-data-clusters"></a>Exécuter des applications avec azdata - Clusters Big Data SQL Server

Cet article explique comment exécuter une application à l’intérieur d’un cluster Big Data SQL Server.

## <a name="prerequisites"></a>Prérequis

- [Cluster Big Data SQL Server 2019](deployment-guidance.md)
- [[!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]](../azdata/install/deploy-install-azdata.md)

## <a name="capabilities"></a>Fonctionnalités

Dans SQL Server 2019, vous pouvez créer, supprimer, décrire, initialiser, exécuter et mettre à jour votre application. Le tableau suivant décrit les commandes de déploiement d’application que vous pouvez utiliser avec **azdata**.

|Commande |Description |
|:---|:---|
|`azdata app describe` | Décrivez une application. |
|`azdata app run` | Exécutez une application. |


Les sections suivantes décrivent ces commandes de façon plus détaillée.


## <a name="run-an-app"></a>Exécuter une application

Si l’état de l’application est `Ready`, vous pouvez l’utiliser en l’exécutant avec les paramètres d’entrée spécifiés. Utilisez la syntaxe suivante pour exécuter une application :

```bash
azdata app run --name <app_name> --version <app_version> --inputs <inputs_params>
```

L’exemple suivant montre la commande run :

```bash
azdata app run --name add-app --version v1 --inputs x=1,y=2
```

Si l’exécution a réussi, vous devez voir votre sortie telle que vous l’avez spécifiée lors de la création de l’application. Voici un exemple de sortie.

```json
{
  "changedFiles": [],
  "consoleOutput": "",
  "errorMessage": "",
  "outputFiles": {},
  "outputParameters": {
    "result": 3
  },
  "success": true
}
```


## <a name="describe-an-app"></a>Décrire une application

La commande describe fournit des informations détaillées sur l’application, notamment le point de terminaison de votre cluster. Ces informations sont généralement utilisées par les développeurs pour créer une application à l’aide du client Swagger et du service web pour interagir avec l’application de manière RESTful. Pour plus d’informations, consultez [Utiliser des applications sur des clusters Big Data](app-consume.md).

```json
{
  "input_param_defs": [
    {
      "name": "x",
      "type": "int"
    },
    {
      "name": "y",
      "type": "int"
    }
  ],
  "links": {
    "app": "https://10.1.1.3:30080/api/app/add-app/v1",
    "swagger": "https://10.1.1.3:30080/api/app/add-app/v1/swagger.json"
  },
  "name": "add-app",
  "output_param_defs": [
    {
      "name": "result",
      "type": "int"
    }
  ],
  "state": "Ready",
  "version": "v1"
}
```

Pour plus d’informations, découvrez comment intégrer des applications déployées sur des [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] dans vos propres applications en consultant [Consommer des applications sur des clusters Big Data](app-consume.md). Vous pouvez également consulter d’autres [exemples de déploiement d’application](https://aka.ms/sql-app-deploy).

Pour plus d’informations sur les [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)], consultez [Que sont les [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)] ?](big-data-cluster-overview.md).