---
title: Utilisez curl pour charger des données dans HDFS | Microsoft Docs
titleSuffix: SQL Server big data clusters
description: Utilisez curl pour charger des données dans HDFS sur un cluster Big Data SQL Server 2019.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mihaelab
ms.date: 06/22/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 2310ffa3849a9fc4b72cbb21e5f2f881fb447019
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100038922"
---
# <a name="use-curl-to-load-data-into-hdfs-on-big-data-clusters-2019"></a>Utilisez curl pour charger des données dans HDFS sur [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Cet article explique comment utiliser **curl** pour charger des données dans HDFS sur [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)].

## <a name="prerequisites"></a><a id="prereqs"></a> Conditions préalables

- [Charger des exemples de données dans votre cluster Big Data](tutorial-load-sample-data.md)

## <a name="obtain-the-service-external-ip"></a>Obtenir l’adresse IP externe du service

WebHDFS démarre lorsque le déploiement est terminé et que son accès est passé par Knox. Le point de terminaison Knox est exposé via un service Kubernetes nommé **gateway-svc-external**.  Pour créer l’URL WebHDFS nécessaire au téléchargement et au chargement des fichiers, vous avez besoin de l’adresse IP externe du service **gateway-svc-external** et du nom du cluster Big Data. Vous pouvez récupérer l’adresse IP externe du service **gateway-svc-external** en exécutant la commande suivante :

```terminal
kubectl get service gateway-svc-external -n <big data cluster name> -o json | jq -r .status.loadBalancer.ingress[0].ip
```

> [!NOTE]
> `<big data cluster name>` correspond au nom du cluster que vous avez spécifié dans le fichier de configuration du déploiement. Le nom par défaut est `mssql-cluster`.

## <a name="construct-the-url-to-access-webhdfs"></a>Construire l’URL pour accéder à WebHDFS

Vous pouvez maintenant construire l’URL pour accéder à WebHDFS de la façon suivante :

`https://<gateway-svc-external service external IP address>:30443/gateway/default/webhdfs/v1/`

Par exemple :

`https://13.66.190.205:30443/gateway/default/webhdfs/v1/`

## <a name="authentication-with-active-directory"></a>Authentification auprès d’Active Directory

Pour les déploiements avec Active Directory, utilisez le paramètre d’authentification avec `curl` avec l’authentification par négociation. 

Pour utiliser `curl` avec l’authentification Active Directory, exécutez la commande suivante :

```
kinit <username>
```

La commande génère un jeton Kerberos utilisable par `curl`. Les commandes présentées dans les sections suivantes spécifient le paramètre `--anyauth` pour `curl`. Pour les URL qui nécessitent l’authentification par négociation, `curl` détecte et utilise automatiquement le jeton Kerberos généré au lieu du nom d’utilisateur et du mot de passe pour s’authentifier auprès des URL.

## <a name="list-a-file"></a>Lister un fichier

Pour lister le fichier sous **hdfs:///product_review_d**, utilisez la commande curl suivante :

```terminal
curl -i -k --anyauth -u root:<AZDATA_PASSWORD> -X GET 'https://<gateway-svc-external IP external address>:30443/gateway/default/webhdfs/v1/product_review_data/?op=liststatus'
```

[!INCLUDE [big-data-cluster-root-user](../includes/big-data-cluster-root-user.md)]

Pour les points de terminaison qui n’utilisent pas la racine, utilisez la commande cURL suivante :

```terminal
curl -i -k --anyauth -u <AZDATA_USERNAME>:<AZDATA_PASSWORD> -X GET 'https://<gateway-svc-external IP external address>:30443/gateway/default/webhdfs/v1/product_review_data/?op=liststatus'
```

## <a name="put-a-local-file-into-hdfs"></a>Placer un fichier local dans HDFS

Pour déplacer un nouveau fichier **test.csv** du répertoire local vers le répertoire product_review_data, utilisez la commande curl suivante (le paramètre **Content-Type** est obligatoire) :

```terminal
curl -i -L -k --anyauth -u root:<AZDATA_PASSWORD> -X PUT 'https://<gateway-svc-external IP external address>:30443/gateway/default/webhdfs/v1/product_review_data/test.csv?op=create' -H 'Content-Type: application/octet-stream' -T 'test.csv'
```

[!INCLUDE [big-data-cluster-root-user](../includes/big-data-cluster-root-user.md)]

Pour les points de terminaison qui n’utilisent pas la racine, utilisez la commande cURL suivante :

```terminal
curl -i -L -k --anyauth -u <AZDATA_USERNAME>:<AZDATA_PASSWORD> -X PUT 'https://<gateway-svc-external IP external address>:30443/gateway/default/webhdfs/v1/product_review_data/test.csv?op=create' -H 'Content-Type: application/octet-stream' -T 'test.csv'
```

## <a name="create-a-directory"></a>Créer un répertoire

Pour créer un répertoire **test** sous `hdfs:///`, utilisez la commande suivante :

```terminal
curl -i -L -k --anyauth -u root:<AZDATA_PASSWORD> -X PUT 'https://<gateway-svc-external IP external address>:30443/gateway/default/webhdfs/v1/test?op=MKDIRS'
```

[!INCLUDE [big-data-cluster-root-user](../includes/big-data-cluster-root-user.md)]
Pour les points de terminaison qui n’utilisent pas la racine, utilisez la commande cURL suivante :

```terminal
curl -i -L -k --anyauth -u <AZDATA_USERNAME>:<AZDATA_PASSWORD> -X PUT 'https://<gateway-svc-external IP external address>:30443/gateway/default/webhdfs/v1/test?op=MKDIRS'
```

## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations sur les clusters Big Data SQL Server, consultez [Qu’est-ce qu’un cluster Big Data SQL Server ?](big-data-cluster-overview.md).
