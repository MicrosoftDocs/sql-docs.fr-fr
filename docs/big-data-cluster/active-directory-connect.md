---
title: Se connecter en mode Active Directory
titleSuffix: SQL Server Big Data Cluster
description: Découvrez comment vous connecter à Clusters Big Data SQL Server dans un domaine Active Directory.
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.date: 09/30/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 809a91c59973bee1602f335ed37fbef5931bbfeb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100039909"
---
# <a name="connect-big-data-clusters-2019-active-directory-mode"></a>Connecter [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] : Mode Active Directory

Cet article explique comment se connecter à des points de terminaison de cluster Big Data SQL Server déployés en mode Active Directory. Les tâches décrites dans cet article nécessitent que vous disposiez d’un cluster Big Data SQL Server déployé en mode Active Directory. Si vous n’avez pas de cluster, consultez [Déployer [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] en mode Active Directory](active-directory-deploy.md).

## <a name="overview"></a>Vue d’ensemble

Connectez-vous à l’instance principale SQL Server avec l’authentification AD.

Pour vérifier les connexions AD à l’instance SQL Server, connectez-vous à l’instance principale SQL avec `sqlcmd`. Les connexions sont automatiquement créées pour les groupes fournis lors du déploiement (`clusterUsers` et `clusterAdmins`).

Si vous utilisez Linux, commencez par exécuter `kinit` en tant qu’utilisateur Active Directory, puis exécutez `sqlcmd`. Si vous utilisez Windows, connectez-vous simplement en tant qu’utilisateur souhaité à partir d’un **ordinateur client joint au domaine**.

## <a name="connect-to-master-instance-from-linuxmac"></a>Se connecter à l’instance principale à partir de Linux/Mac

```bash
kinit <username>@<domain name>
sqlcmd -S <DNS name for master instance>,31433 -E
```

## <a name="connect-to-master-instance-from-windows"></a>Se connecter à l’instance principale à partir de Windows

```cmd
sqlcmd -S <DNS name for master instance>,31433 -E
```

## <a name="log-in-to-sql-server-master-instance-using-azure-data-studio-or-ssms"></a>Se connecter à l’instance principale SQL Server à l’aide d’Azure Data Studio ou SSMS

À partir d’un client joint au domaine, vous pouvez ouvrir SSMS ou Azure Data Studio et vous connecter à l’instance principale. Il s’agit de la même expérience que la connexion à n’importe quelle instance SQL Server à l’aide de l’authentification Active Directory.

À partir de SSMS :

![Boîte de dialogue Se connecter à SQL Server dans SSMS](./media/deploy-active-directory/image23.png)

À partir d’Azure Data Studio :

![Boîte de dialogue Se connecter à SQL Server dans Azure Data Studio](./media/deploy-active-directory/image24.png)}

## <a name="log-in-to-controller-with-ad-authentication"></a>Se connecter au contrôleur avec l’authentification Active Directory

### <a name="connect-to-controller-with-ad-authentication-from-linuxmac"></a>Se connecter au contrôleur avec l’authentification Active Directory à partir de Linux/Mac

Il existe deux solutions pour se connecter au point de terminaison du contrôleur à l’aide de [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] et de l’authentification AD. Vous pouvez utiliser le paramètre *--endpoint/-e* :

```bash
kinit <username>@<domain name>
azdata login -e https://<controller DNS name>:30080 --auth ad
```

Vous pouvez également vous connecter à l’aide du paramètre *--namespace/-n*, à savoir le nom du cluster Big Data :

```bash
kinit <username>@<domain name>
azdata login -n <clusterName> --auth ad
```

### <a name="connect-to-controller-with-ad-authentication-from-windows"></a>Se connecter au contrôleur avec l’authentification Active Directory à partir de Windows

```cmd
azdata login -e https://<controller DNS name>:30080 --auth ad
```

## <a name="use-ad-authentication-to-knox-gateway-webhdfs"></a>Utiliser l’authentification AD pour la passerelle Knox (webHDFS)

Vous pouvez également émettre des commandes HDFS à l’aide de curl via le point de terminaison de la passerelle Knox. Cela nécessite l’authentification Active Directory pour Knox. La commande curl ci-dessous émet un appel REST webHDFS via la passerelle Knox pour créer un répertoire appelé `products`

```bash
curl -k -v --negotiate -u : https://<Gateway DNS name>:30443/gateway/default/webhdfs/v1/products?op=MKDIRS -X PUT
```

## <a name="next-steps"></a>Étapes suivantes

[Résolution des problèmes d’intégration Active Directory Clusters Big Data SQL Server](troubleshoot-active-directory.md)

[Concept : Déploiement de [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] en mode Active Directory](active-directory-deployment-background.md)
