---
title: Consommer des applications
titleSuffix: SQL Server Big Data Clusters
description: Consommez une application déployée sur un cluster Big Data SQL Server avec un service web RESTful.
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.date: 06/22/2020
ms.metadata: seo-lt-2019
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 78d353130bfa4240c1d3eacb09c8e50d08a4c350
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100039789"
---
# <a name="consume-an-app-deployed-on-big-data-clusters-2019-using-a-restful-web-service"></a>Consommer une application déployée sur [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] avec un service web RESTful

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Cet article décrit comment consommer une application déployée sur un cluster Big Data SQL Server avec un service web RESTful.

## <a name="prerequisites"></a>Conditions préalables requises

- [Cluster Big Data SQL Server](deployment-guidance.md)
- [[!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]](../azdata/install/deploy-install-azdata.md)
- Application déployée avec [azdata](app-create.md) ou l’[extension de déploiement d’application](app-deployment-extension.md)

> [!NOTE]
> Lorsque le fichier de spécification yaml de l’application indique une planification, l’application est déclenchée par le biais d’un travail cron. Si votre cluster Big Data est déployé sur OpenShift, le lancement du travail cron nécessite des fonctionnalités supplémentaires. Pour obtenir des instructions spécifiques, consultez les détails relatifs aux [considérations de sécurité sur OpenShift](concept-application-deployment.md#app-deploy-security).

## <a name="capabilities"></a>Fonctionnalités

Après avoir déployé une application sur votre [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)], vous pouvez accéder à cette application et la consommer en utilisant un service web RESTful. Vous pouvez ainsi intégrer cette application à partir d’autres applications ou services (par exemple, une application mobile ou un site web). Le tableau suivant décrit les commandes de déploiement d’application que vous pouvez utiliser avec **azdata** pour obtenir des informations sur le service web RESTful pour votre application.

|Commande |Description |
|:---|:---|
|`azdata app describe` | Décrivez une application. |

Vous pouvez utiliser le paramètre `--help` pour obtenir de l’aide, comme dans l’exemple suivant :

```bash
azdata app describe --help
```

Les sections suivantes décrivent comment récupérer un point de terminaison pour une application et comment utiliser le service web RESTful pour l’intégration d’applications.

## <a name="retrieve-the-endpoint"></a>Récupérer le point de terminaison

Les clusters Big Data (BDC) fournissent des points de terminaison auxquels vous pouvez accéder et qui consomment cette application à l’aide d’un service web RESTful ; l’objectif principal est de faciliter l’interaction avec d’autres applications web ou mobiles et d’être plus proactif pour l’architecture de ces microservices. La commande **azdata app describe** fournit des informations détaillées sur l’application, notamment le point de terminaison de votre cluster. Ces informations sont généralement utilisées par les développeurs pour créer une application à l’aide du client Swagger et du service web pour interagir avec l’application de manière RESTful.

Décrivez votre application en exécutant une commande similaire à l’exemple suivant :

```bash
azdata app describe --name add-app --version v1
```

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
    "app": "https://10.1.1.3:30080/app/addpy/v1",
    "swagger": "https://10.1.1.3:30080/app/addpy/v1/swagger.json"
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

Notez l’adresse IP (`10.1.1.3` dans cet exemple) et le numéro de port (`30080`) dans la sortie.

Une des autres méthodes permettant d’obtenir ces informations est de cliquer avec le bouton droit sur Gérer sur le serveur dans Azure Data Studio où se trouvent les points de terminaison des services listés.

![Point de terminaison Azure Data Studio](media/big-data-cluster-consume-apps/ads_end_point.png)

## <a name="generate-a-jwt-access-token"></a>Générer un jeton d’accès JWT

Pour accéder au service web RESTful pour l’application que vous avez déployée, vous devez d’abord générer un jeton d’accès JWT. L’URL du jeton d’accès dépend de la version du cluster Big Data. 

|Version |URL|
|------------|------|
|GDR1|  `https://[IP]:[PORT]/docs/swagger.json`|
|CU1 et versions ultérieures| `https://[IP]:[PORT]/api/v1/swagger.json`|

 À partir du résultat de l’exemple précédent, avec la version CU4 et l’adresse IP du contrôleur (10.1.1.3 dans l’exemple) et le numéro de port (30080), l’URL ressemblera à ce qui suit : 
 
 ```bash
    https://10.1.1.3 :30080/api/v1/swagger.json
```
 
> Pour plus d’informations sur la version, consultez [l’historique des versions](release-notes-big-data-cluster.md#release-history).

Ouvrez l’URL appropriée dans votre navigateur à l’aide de l’adresse IP et du port que vous avez récupérés en exécutant la commande [`describe`](#retrieve-the-endpoint) ci-dessus. Connectez-vous avec les mêmes informations d’identification que celles utilisées pour `azdata login`.

Collez le contenu de `swagger.json` dans [Swagger Editor](https://editor.swagger.io) pour comprendre les méthodes disponibles :

![API Swagger](media/big-data-cluster-consume-apps/api_swagger.png)

Notez que `app` est une méthode GET et que `token` utilisera une méthode POST. Comme l’authentification des applications utilise des jetons JWT, vous devez obtenir un jeton en utilisant votre outil favori pour effectuer un appel POST à la méthode`token`. Dans le même exemple, l’URL permettant d’obtenir le jeton JWT ressemblera à ce qui suit :

 ```bash
    https://10.1.1.3 :30080/api/v1/token
```


Voici un exemple de la procédure à suivre dans [Postman](https://www.getpostman.com/) :

![Jeton Postman](media/big-data-cluster-consume-apps/postman_token.png)


Le résultat de cette demande vous donne un `access_token` JWT avec lequel devez appeler l’URL pour exécuter l’application.

## <a name="execute-the-app-using-the-restful-web-service"></a>Exécuter l’application à l’aide du service web RESTful

Il existe plusieurs façons d’utiliser une application sur un BDC. Vous pouvez choisir d’utiliser la [commande d’exécution de l’application azdata](app-create.md). Cette section montre comment utiliser des outils de développement courants tels que Postman pour exécuter l’application. 

Vous pouvez ouvrir l’URL de `swagger` qui a été retournée quand vous avez exécuté `azdata app describe --name [appname] --version [version]` dans votre navigateur, ce qui doit être similaire à `https://[IP]:[PORT]/app/[appname]/[version]/swagger.json`. 

> [!NOTE]
> Connectez-vous avec les mêmes informations d’identification que celles utilisées pour `azdata login`. Dans le même exemple, la commande ressemblera à ce qui suit :

 ```bash
    azdata app describe --name add-app --version v1
```

Collez le contenu du fichier `swagger.json` dans [Swagger Editor](https://editor.swagger.io). Vous constaterez que le service web expose la méthode `run` et qu’il a transité par un proxy d’application, c’est-à-dire une API web qui authentifie les utilisateurs puis achemine les requêtes vers les applications. Notez l’URL de base affichée en haut. Vous pouvez utiliser l’outil de votre choix pour appeler la méthode `run` (`https://[IP]:30778/api/app/[appname]/[version]/run`), en passant les paramètres dans le corps de votre demande POST au format JSON. 


Dans cet exemple, nous utilisons [Postman](https://www.getpostman.com/). Avant d’effectuer l’appel, vous devez définir `Authorization` avec la valeur `Bearer Token` et coller le jeton récupéré précédemment. Un en-tête est alors défini dans votre demande. Consultez la capture d’écran ci-dessous.

![En-têtes d’exécution Postman](media/big-data-cluster-consume-apps/postman_run_1.png)

Ensuite, dans le corps de la demande, passez les paramètres à l’application que vous appelez et définissez `content-type` avec la valeur `application/json` :

![Corps d’exécution Postman](media/big-data-cluster-consume-apps/postman_run_2.png)

Quand vous envoyez la demande, vous obtenez la même sortie que celle générée lors de l’exécution de l’application avec `azdata app run` :

![Résultat d’exécution Postman](media/big-data-cluster-consume-apps/postman_result.png)

L’application a été appelée par le biais du service web. Vous pouvez suivre des étapes similaires pour intégrer ce service web dans votre application.


## <a name="next-steps"></a>Étapes suivantes

Découvrez comment [Surveiller des applications sur des clusters Big Data](app-monitor.md) pour plus d’informations. Vous pouvez également consulter d’autres [exemples de déploiement d’application](https://aka.ms/sql-app-deploy).

Pour plus d’informations sur les [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)], consultez [Que sont les [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)] ?](big-data-cluster-overview.md).