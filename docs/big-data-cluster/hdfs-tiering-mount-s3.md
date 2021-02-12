---
title: Monter S3 pour la hiérarchisation HDFS
titleSuffix: SQL Server big data clusters
description: Cet article explique comment configurer la hiérarchisation HDFS pour monter un système de fichiers S3 externe dans HDFS sur un cluster Big Data SQL Server 2019.
author: nelgson
ms.author: negust
ms.reviewer: mikeray
ms.date: 08/21/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 6fce2d87f618fec23f204d14cd813beea4ac74bd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100046509"
---
# <a name="how-to-mount-s3-for-hdfs-tiering-in-a-big-data-cluster"></a>Guide pratique pour monter S3 pour la hiérarchisation HDFS dans un cluster Big Data

Les sections suivantes fournissent un exemple de configuration de la hiérarchisation HDFS avec une source de données de stockage S3.

## <a name="prerequisites"></a>Prérequis

- [Cluster Big Data déployé](deployment-guidance.md)
- [Outils Big Data](deploy-big-data-tools.md)
  - **azdata**
  - **kubectl**
- Créer et charger des données dans un compartiment S3 
  - Chargez des fichiers CSV ou Parquet dans votre compartiment S3. Il s’agit de données HDFS externes qui vont être montées sur HDFS dans le cluster Big Data.

## <a name="access-keys"></a>Clés d'accès

### <a name="set-environment-variable-for-access-key-credentials"></a>Définir la variable d’environnement pour les informations d’identification de la clé d’accès

Ouvrez une invite de commandes sur une machine client pouvant accéder à votre cluster Big Data. Définissez une variable d’environnement au format suivant. Notez que les informations d’identification doivent figurer dans une liste de valeurs séparées par des virgules. La commande « set » est utilisée sur Windows. Si vous êtes sur Linux, utilisez « export » à la place.

   ```text
    set MOUNT_CREDENTIALS=fs.s3a.access.key=<Access Key ID of the key>,
    fs.s3a.secret.key=<Secret Access Key of the key>
   ```

   > [!TIP]
   > Pour plus d’informations sur la création de clés d’accès S3, consultez [clés d’accès S3](https://docs.aws.amazon.com/general/latest/gr/aws-sec-cred-types.html#access-keys-and-secret-access-keys).

## <a name="mount-the-remote-hdfs-storage"></a><a id="mount"></a> Monter le stockage HDFS distant

Une fois que vous avez préparé un fichier d’informations d’identification avec les clés d’accès, vous pouvez commencer le montage. Les étapes suivantes permettent de monter le stockage HDFS distant dans S3 vers le stockage HDFS local de votre cluster Big Data.

1. Utilisez **kubectl** pour rechercher l’adresse IP du service **controller-svc-external** du point de terminaison dans votre cluster Big Data. Recherchez **External-IP**.

   ```bash
   kubectl get svc controller-svc-external -n <your-big-data-cluster-name>
   ```

1. Connectez-vous à **azdata** en utilisant l’adresse IP externe du point de terminaison du contrôleur ainsi que votre nom d’utilisateur et votre mot de passe de cluster :

   ```bash
   azdata login -e https://<IP-of-controller-svc-external>:30080/
   ```
   
1. Définissez la variable d’environnement MOUNT_CREDENTIALS en suivant les instructions ci-dessus

1. Montez le stockage HDFS distant dans Azure en utilisant **azdata bdc hdfs mount create**. Remplacez les valeurs d’espace réservé avant d’exécuter la commande suivante :

   ```bash
   azdata bdc hdfs mount create --remote-uri s3a://<S3 bucket name> --mount-path /mounts/<mount-name>
   ```

   > [!NOTE]
   > La commande créer un montage est asynchrone. À ce stade, aucun message n’indique si le montage a réussi. Consultez la section [état](#status) pour vérifier l’état de vos montages.

Si le montage a été correctement effectué, vous devez pouvoir interroger les données HDFS et exécuter des tâches Spark sur ces dernières. Il apparaît dans le stockage HDFS de votre cluster Big Data à l’emplacement spécifié par `--mount-path`.

## <a name="get-the-status-of-mounts"></a><a id="status"></a> Obtenir l’état des montages

Pour répertorier l’état de tous les montages de votre cluster Big Data, utilisez la commande suivante :

```bash
azdata bdc hdfs mount status
```

Pour répertorier l’état d’un montage situé sur un chemin spécifique dans HDFS, utilisez la commande suivante :

```bash
azdata bdc hdfs mount status --mount-path <mount-path-in-hdfs>
```

## <a name="refresh-a-mount"></a>Actualiser un montage

L’exemple suivant actualise le montage.

```bash
azdata bdc hdfs mount refresh --mount-path <mount-path-in-hdfs>
```

## <a name="delete-the-mount"></a><a id="delete"></a> Supprimer le montage

Pour supprimer le montage, utilisez la commande **azdata bdc hdfs mount delete** et spécifiez le chemin de montage dans HDFS :

```bash
azdata bdc hdfs mount delete --mount-path <mount-path-in-hdfs>
```

## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations sur les [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)], consultez [Que sont les [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)] ?](big-data-cluster-overview.md).
