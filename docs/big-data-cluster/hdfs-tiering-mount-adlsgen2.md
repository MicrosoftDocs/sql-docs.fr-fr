---
title: Monter ADLS Gen2 pour la hiérarchisation HDFS
titleSuffix: How to mount ADLS Gen2
description: Cet article donne un exemple de configuration de la hiérarchisation HDFS avec une source de données Azure Data Lake Storage Gen2.
author: nelgson
ms.author: negust
ms.reviewer: mikeray
ms.date: 06/29/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: b41ad6a56da30fb30677d781630e30ad5e07d333
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100046549"
---
# <a name="how-to-mount-adls-gen2-for-hdfs-tiering-in-a-big-data-cluster"></a>Comment monter ADLS Gen2 pour la hiérarchisation HDFS dans un cluster Big Data

Les sections suivantes fournissent un exemple de configuration de la hiérarchisation HDFS avec une source de données Azure Data Lake Storage Gen2.

## <a name="prerequisites"></a>Prérequis

- [Cluster Big Data déployé](deployment-guidance.md)
- [Outils Big Data](deploy-big-data-tools.md)
  - **azdata**
  - **kubectl**

## <a name="load-data-into-azure-data-lake-storage"></a><a id="load"></a> Charger des données dans Azure Data Lake Storage

La section suivante décrit comment configurer Azure Data Lake Storage Gen2 pour le test de la hiérarchisation HDFS. Si vous avez déjà des données stockées dans Azure Data Lake Storage, vous pouvez ignorer cette section pour utiliser vos propres données.

1. [Créer un compte de stockage avec des fonctionnalités de Data Lake Storage Gen2](/azure/storage/blobs/data-lake-storage-quickstart-create-account).

1. [Créez un système de fichiers](/azure/storage/blobs/data-lake-storage-explorer) dans ce compte de stockage pour vos données.

1. Chargez un fichier CSV ou Parquet dans le conteneur. Il s’agit de données HDFS externes qui vont être montées sur HDFS dans le cluster Big Data.

## <a name="credentials-for-mounting"></a>Informations d'identification pour le montage

### <a name="use-oauth-credentials-to-mount"></a>Utiliser les informations d’identification OAuth pour le montage

Pour pouvoir utiliser les informations d’identification OAuth pour le montage, vous devez suivre les étapes ci-dessous :

1. Accédez au [Portail Azure](https://portal.azure.com).
1. Accédez à « Azure Active Directory ». Ce service doit figurer dans la barre de navigation de gauche.
1. Dans la barre de navigation de droite, sélectionnez « Inscriptions d’applications » et créez une nouvelle inscription.
1. Créez une application web et suivez l’Assistant. **N’oubliez pas le nom de l’application que vous créez ici**. Vous devrez ajouter ce nom à votre compte ADLS en tant qu’utilisateur autorisé. Notez également l’ID du client d’application dans la vue d’ensemble lorsque vous sélectionnez l’application.
1. Une fois l’application web créée, accédez à « Certificats & secrets », créez un **nouveau secret client**, puis sélectionnez une durée pour la clé. **Ajoutez** le secret.
1. Revenez à la page Inscriptions des applications et cliquez sur « Points de terminaison » en haut de la page. Notez l’URL du **Point de terminaison de jeton OAuth (v2)** .
1. Les éléments suivants doivent maintenant être signalés pour OAuth :

    - « ID du client d’application » de l’application web
    - Secret client
    - Point de terminaison de jeton

### <a name="adding-the-service-principal-to-your-adls-account"></a>Ajout du principal du service à votre compte ADLS

1. Accédez de nouveau au portail et au système de fichiers de votre compte de stockage ADLS, puis sélectionnez Contrôle d’accès (IAM) dans le menu de gauche.
1. Sélectionnez « Ajouter une attribution de rôle ». 
1. Sélectionnez le rôle « Contributeur aux données Blob du stockage ».
1. Recherchez le nom que vous avez créé ci-dessus (notez qu’il n’apparaît pas dans la liste, mais qu’il sera trouvé si vous recherchez le nom complet).
1. Enregistrez le rôle.

Attendre 5 à 10 minutes avant d’utiliser les informations d’identification pour le montage

### <a name="set-environment-variable-for-oauth-credentials"></a>Définir la variable d’environnement pour les informations d’identification OAuth

Ouvrez une invite de commandes sur une machine client pouvant accéder à votre cluster Big Data. Définissez une variable d’environnement en utilisant le format suivant. Les informations d’identification doivent figurer dans une liste séparée par des virgules. La commande « set » est utilisée sur Windows. Si vous êtes sur Linux, utilisez « export » à la place.

**Notez** que vous devez supprimer les sauts de ligne et les espaces entre les virgules « , » lorsque vous fournissez les informations d’identification. La mise en forme ci-dessous a pour simple but de faciliter la lecture.

```console
   set MOUNT_CREDENTIALS=fs.azure.account.auth.type=OAuth,
   fs.azure.account.oauth.provider.type=org.apache.hadoop.fs.azurebfs.oauth2.ClientCredsTokenProvider,
   fs.azure.account.oauth2.client.endpoint=[token endpoint],
   fs.azure.account.oauth2.client.id=[Application client ID],
   fs.azure.account.oauth2.client.secret=[client secret]
```

## <a name="use-access-keys-to-mount"></a>Utiliser des clés d’accès pour le montage

Vous pouvez également monter à l’aide des clés d’accès que vous pouvez obtenir pour votre compte ADLS sur le Portail Azure.

 > [!TIP]
   > Pour plus d’informations sur la recherche de la touche d'accès (`<storage-account-access-key>`) pour votre compte de stockage, consultez [Afficher les clés de compte et la chaîne de connexion](/azure/storage/common/storage-account-keys-manage#view-access-keys-and-connection-string).

### <a name="set-environment-variable-for-access-key-credentials"></a>Définir la variable d’environnement pour les informations d’identification de la clé d’accès

1. Ouvrez une invite de commandes sur une machine client pouvant accéder à votre cluster Big Data.

1. Ouvrez une invite de commandes sur une machine client pouvant accéder à votre cluster Big Data. Définissez une variable d’environnement au format suivant. Les informations d’identification doivent figurer dans une liste de valeurs séparées par des virgules. La commande « set » est utilisée sur Windows. Si vous êtes sur Linux, utilisez « export » à la place.

**Notez** que vous devez supprimer les sauts de ligne et les espaces entre les virgules « , » lorsque vous fournissez les informations d’identification. La mise en forme ci-dessous a pour simple but de faciliter la lecture.

```console
set MOUNT_CREDENTIALS=fs.azure.abfs.account.name=<your-storage-account-name>.dfs.core.windows.net,
fs.azure.account.key.<your-storage-account-name>.dfs.core.windows.net=<storage-account-access-key>
```

## <a name="mount-the-remote-hdfs-storage"></a><a id="mount"></a> Monter le stockage HDFS distant

Maintenant que vous avez défini la variable d’environnement MOUNT_CREDENTIALS pour les clés d’accès ou à l’aide d’OAuth, vous pouvez commencer le montage. Les étapes suivantes permettent de monter le stockage HDFS distant dans Azure Data Lake vers le stockage HDFS local de votre cluster Big Data.

1. Utilisez **kubectl** pour rechercher l’adresse IP du service **controller-svc-external** du point de terminaison dans votre cluster Big Data. Recherchez **External-IP**.

   ```bash
   kubectl get svc controller-svc-external -n <your-big-data-cluster-name>
   ```

1. Connectez-vous à **azdata** en utilisant l’adresse IP externe du point de terminaison du contrôleur ainsi que votre nom d’utilisateur et votre mot de passe de cluster :

   ```bash
   azdata login -e https://<IP-of-controller-svc-external>:30080
   ```
1. Définissez la variable d’environnement MOUNT_CREDENTIALS (faites défiler les instructions)

1. Montez le stockage HDFS distant dans Azure en utilisant **azdata bdc hdfs mount create**. Remplacez les valeurs d’espace réservé avant d’exécuter la commande suivante :

   ```bash
   azdata bdc hdfs mount create --remote-uri abfs://<blob-container-name>@<storage-account-name>.dfs.core.windows.net/ --mount-path /mounts/<mount-name>
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

L’exemple suivant actualise le montage. Cette actualisation efface également le cache de montage.

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
