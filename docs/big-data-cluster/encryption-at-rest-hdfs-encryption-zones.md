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
ms.openlocfilehash: bfcc348595a645eca3b1150ae39e7c777f4e192a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100046759"
---
# <a name="sql-server-big-data-clusters-hdfs-encryption-zones-usage-guide"></a>Guide d’utilisation des zones de chiffrement HDFS des Clusters Big Data SQL Server

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Ce guide décrit comment utiliser les fonctionnalités de chiffrement au repos des Clusters Big Data SQL Server pour chiffrer des dossiers HDFS à l’aide des zones de chiffrement.

Notez qu’il existe déjà une zone de chiffrement par défaut montée sur __```/securelake```__ prêt à être utilisée. Elle a été créée avec une clé 256 bits générée par le système et nommée __securelakekey__. Cette clé peut être utilisée pour créer des zones de chiffrement supplémentaires.

## <a name="prerequisites"></a><a id="prereqs"></a> Conditions préalables

- [Cluster Big Data SQL Server CU8+](release-notes-big-data-cluster.md) avec l’intégration Active Directory.
- Utilisateur avec des privilèges d'administrateur.

## <a name="login-into-the-name-node"></a>Se connecter au nœud de nom

Utilisez [les instructions de connexion Active Directory](active-directory-connect.md) pour effectuer la connexion au cluster. Connectez-vous au namenode (nmnode-0-0) pour émettre des commandes de clés et de zones de chiffrement.

   ```console
   kubectl exec -it -c hadoop -n <cluster_namespace> nmnode-0-0 -- /bin/bash
   ```

## <a name="create-an-encryption-zone-using-the-provided-system-managed-key"></a>Créer une zone de chiffrement à l’aide de la clé managée par le système fournie

1. Créer un dossier HDFS

   ```console
   hdfs dfs -mkdir -p /user/zone/folder
   ```

1. Émettez la commande de création de zone de chiffrement pour chiffrer le dossier à l’aide de la clé __securelakekey__.

   ```console
   hdfs crypto -createZone -keyName securelakekey -path /user/zone/folder
   ```

## <a name="create-a-custom-new-key-and-encryption-zone"></a>Créer une nouvelle clé et une zone de chiffrement personnalisées

1. Utilisez le modèle suivant pour créer une clé de 256 bits.

   ```console
   kinit hdfs
   hadoop key create mydatalakekey -size 256
   ```

1. Créez et chiffrez un nouveau chemin d’accès HDFS à l’aide de la clé utilisateur.

   ```console
   hdfs dfs -mkdir -p /user/mydatalake
   hdfs crypto -createZone -keyName mydatalakekey -path /user/mydatalake
   ```
