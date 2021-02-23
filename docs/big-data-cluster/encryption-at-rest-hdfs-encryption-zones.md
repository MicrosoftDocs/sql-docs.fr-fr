---
title: Guide d’utilisation des zones de chiffrement HDFS des Clusters Big Data SQL Server
titleSuffix: SQL Server big data clusters
description: Cet article explique comment utiliser la fonction des zones de chiffrement du BDC HDFS SQL Server
author: DaniBunny
ms.author: dacoelho
ms.reviewer: mihaelab
ms.date: 10/19/2020
ms.topic: tutorial
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 4c56d065de396a282c97c0723f118e5911617544
ms.sourcegitcommit: e8c0c04eb7009a50cbd3e649c9e1b4365e8994eb
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/14/2021
ms.locfileid: "100489293"
---
# <a name="sql-server-big-data-clusters-hdfs-encryption-zones-usage-guide"></a>Guide d’utilisation des zones de chiffrement HDFS des Clusters Big Data SQL Server

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Ce guide décrit comment utiliser les fonctionnalités de chiffrement au repos des Clusters Big Data SQL Server pour chiffrer des dossiers HDFS à l’aide des zones de chiffrement. Il aborde également les tâches de gestion des clés HDFS.

Notez qu’il existe une zone de chiffrement par défaut montée sur __```/securelake```__ prête à être utilisée. Elle a été créée avec une clé 256 bits générée par le système et nommée __securelakekey__. Cette clé peut être utilisée pour créer des zones de chiffrement supplémentaires.

## <a name="prerequisites"></a><a id="prereqs"></a> Conditions préalables

- [Cluster Big Data SQL Server CU8+](release-notes-big-data-cluster.md) avec intégration à [Active Directory](active-directory-prerequisites.md).
- Utilisateur avec des privilèges d'administrateur.
- [!INCLUDE[azdata](../includes/azure-data-cli-azdata.md)] configuré et connecté au cluster en mode AD.

## <a name="create-an-encryption-zone-using-the-provided-system-managed-key"></a>Créer une zone de chiffrement à l’aide de la clé managée par le système fournie

1. Créer un dossier HDFS

   ```console
   azdata bdc hdfs mkdir -p /user/zone/folder
   ```

1. Émettez la commande de création de zone de chiffrement pour chiffrer le dossier à l’aide de la clé __securelakekey__.

   ```console
   azdata bdc hdfs encryption-zone create --path /user/zone/folder --keyname securelakekey
   ```

## <a name="create-a-custom-new-key-and-encryption-zone"></a>Créer une nouvelle clé et une zone de chiffrement personnalisées

1. Utilisez le modèle suivant pour créer une clé de 256 bits.

   ```console
   azdata bdc hdfs key create -n mydatalakekey
   ```

1. Créez et chiffrez un nouveau chemin d’accès HDFS à l’aide de la clé utilisateur.

   ```console
   azdata bdc hdfs encryption-zone create --path /user/mydatalake --keyname mydatalakekey
   ```

## <a name="hdfs-key-rotation-and-encryption-zone-re-encryption"></a>Rotation des clés HDFS et rechiffrement des zones de chiffrement

1. Une nouvelle version de __securelakekey__ est alors créée avec du nouveau matériel clé.

   ```console
   azdata hdfs bdc key roll -n securelakekey
   ```

1. Rechiffrer la zone de chiffrement associée à la clé ci-dessus

   ```console
   azdata bdc hdfs encryption-zone reencrypt --path /securelake --action start
   ```

## <a name="hdfs-key-and-encryption-zone-monitoring"></a>Supervision de la clé et de la zone de chiffrement HDFS

1. Pour superviser l’état du rechiffrement d’une zone de chiffrement 

   ```console
   azdata bdc hdfs encryption-zone status
   ```

1. Pour obtenir les informations de chiffrement relatives à un fichier dans une zone de chiffrement

   ```console
   azdata bdc hdfs encryption-zone get-file-encryption-info --path /securelake/data.csv
   ```

1. Liste de toutes les zones de chiffrement

   ```console
   azdata bdc hdfs encryption-zone list
   ```

1. Pour lister toutes les clés disponibles pour HDFS

   ```console
   azdata bdc hdfs key list
   ```

1. Afin de créer une clé personnalisée pour le chiffrement HDFS. Les tailles possibles sont 128, 192 et 256. La valeur par défaut est 256.

   ```console
   azdata hdfs key create --name key1 --size 256
   ```

## <a name="next-steps"></a>Étapes suivantes

Utiliser azdata avec les clusters Big Data, consultez [Présentation des [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]](big-data-cluster-overview.md).

Utiliser azdata avec les [services de données dotés d’Azure Arc](/azure/azure-arc/data/)
