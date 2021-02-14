---
title: Configurer la hiérarchisation HDFS
titleSuffix: SQL Server big data clusters
description: Cet article explique comment configurer la hiérarchisation HDFS pour monter un système de fichiers Azure Data Lake Storage externe dans HDFS sur un cluster Big Data SQL Server 2019.
author: nelgson
ms.author: negust
ms.reviewer: mikeray
ms.date: 08/21/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 3c95bb10dbd063c56029a01b5a828f80eb5ba4d3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100046685"
---
# <a name="configure-hdfs-tiering-on-big-data-clusters-2019"></a>Configurer la hiérarchisation HDFS sur [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

La hiérarchisation HDFS offre la possibilité de monter un système de fichiers externe et compatible HDFS dans HDFS. Cet article explique comment configurer la hiérarchisation HDFS pour les clusters Big Data SQL Server. À ce stade, nous prenons en charge la connexion à Azure Data Lake Storage Gen2 et à Amazon S3. 

## <a name="hdfs-tiering-overview"></a>Vue d’ensemble de la hiérarchisation HDFS

Avec la hiérarchisation, les applications peuvent accéder de manière fluide aux données d’un grand nombre de magasins externes comme si ces données se trouvaient dans le HDFS local. Le montage est une opération de métadonnées, consistant à copier sur votre HDFS local les métadonnées qui décrivent l’espace de noms sur le système de fichiers externe. Ces métadonnées incluent des informations sur les répertoires et les fichiers externes, ainsi que les autorisations et listes de contrôle d’accès associées. Les données correspondantes sont copiées uniquement à la demande, lors de l’accès aux données par l’intermédiaire d’une requête, par exemple. Vous pouvez désormais accéder aux données du système de fichiers externe à partir du cluster Big Data SQL Server. Vous pouvez exécuter des travaux Spark et des requêtes SQL sur ces données de la même façon que vous les exécutez sur toutes données locales stockées dans HDFS sur le cluster.

Cette vidéo de 7 minutes fournit une vue d’ensemble de la hiérarchisation HDFS :

> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/Unify-your-data-lakes-with-HDFS-tiering/player?WT.mc_id=dataexposed-c9-niner]


### <a name="caching"></a>Mise en cache
Aujourd’hui, par défaut, 1 % du stockage HDFS total est réservé à la mise en cache des données montées. La mise en cache est un paramètre global sur les montages.

> [!NOTE]
> La hiérarchisation HDFS est une fonctionnalité développée par Microsoft, dont une version antérieure a été publiée dans le cadre de la distribution d’Apache Hadoop 3.1. Pour plus d’informations, consultez [https://issues.apache.org/jira/browse/HDFS-9806](https://issues.apache.org/jira/browse/HDFS-9806).

Les sections suivantes fournissent un exemple de configuration de la hiérarchisation HDFS avec une source de données Azure Data Lake Storage Gen2.

## <a name="refresh"></a>Actualiser

La hiérarchisation HDFS prend en charge l’actualisation. Actualisez un montage existant pour la dernière capture instantanée des données distantes.

## <a name="prerequisites"></a>Prérequis

- [Cluster Big Data déployé](deployment-guidance.md)
- [Outils Big Data](deploy-big-data-tools.md)
  - **azdata**
  - **kubectl**

## <a name="mounting-instructions"></a>Instructions de montage

Nous prenons en charge la connexion à Azure Data Lake Storage Gen2 et à Amazon S3. Vous trouverez des instructions sur la façon de procéder à un montage sur ces types de stockage dans les articles suivants :

- [Guide pratique pour monter ADLS Gen2 pour la hiérarchisation HDFS dans un cluster Big Data](hdfs-tiering-mount-adlsgen2.md)
- [Guide pratique pour monter S3 pour la hiérarchisation HDFS dans un cluster Big Data](hdfs-tiering-mount-s3.md)

## <a name="known-issues-and-limitations"></a><a id="issues"></a> Problèmes connus et limitations

La liste suivante contient les problèmes connus et les limitations actuelles lors de l’utilisation de la hiérarchisation HDFS dans [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] :

- Si le montage est bloqué dans un état `CREATING` pendant une longue période, il a probablement échoué. Dans ce cas, annulez la commande et supprimez le montage, si nécessaire. Vérifiez que vos paramètres et informations d’identification sont corrects avant de réessayer.

- Les montages ne peuvent pas être créés sur des répertoires existants.

- Les montages ne peuvent pas être créés dans des montages existants.

- Si un ou plusieurs ancêtres du point de montage n’existent pas, ils sont créés avec des autorisations définies par défaut sur r-xr-xr-x (555).

- La création du montage peut prendre un certain temps en fonction du nombre et de la taille des fichiers montés. Pendant ce processus, les fichiers sous le montage ne sont pas visibles par les utilisateurs. Pendant la création du montage, tous les fichiers sont ajoutés à un chemin temporaire, qui est par défaut `/_temporary/_mounts/<mount-location>`.

- La commande de création de montage est asynchrone. Une fois la commande exécutée, vous pouvez vérifier le statut du montage pour comprendre son état.

- Lors de la création du montage, l’argument utilisé pour **--mount-path** est essentiellement un identificateur unique du montage. La même chaîne (y compris le caractère « / » à la fin, le cas échéant) doit être utilisée dans les commandes suivantes.

- Les montages sont en lecture seule. Vous ne pouvez pas créer de répertoires ou de fichiers sous un montage.

- Nous vous déconseillons de monter des répertoires et des fichiers qui peuvent changer. Une fois le montage créé, les modifications ou mises à jour apportées à l’emplacement distant ne sont pas reflétées dans le montage dans HDFS. Si des modifications se produisent à l’emplacement distant, vous pouvez choisir de supprimer et de recréer le montage pour refléter l’état mis à jour.

## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations sur les [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)], consultez [Que sont les [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)] ?](big-data-cluster-overview.md).
