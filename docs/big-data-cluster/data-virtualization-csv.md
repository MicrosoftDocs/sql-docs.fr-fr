---
title: Virtualiser des données CSV à partir d’un pool de stockage
subtitle: Clusters Big Data SQL Server
description: Étapes détaillant la création d’une table externe pour la virtualisation d’un fichier CSV dans un cluster Big Data
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mikeray
ms.date: 04/24/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: polybase
monikerRange: '>= sql-server-ver15'
ms.metadata: seo-lt-2019
ms.openlocfilehash: 7b78e44ee36e9320688a8a6815fbb267f1663d3d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100038669"
---
# <a name="virtualize-csv-data-from-storage-pool-big-data-clusters"></a>Virtualiser des données CSV à partir d’un pool de stockage (clusters Big Data)

Les clusters Big Data SQL Server peuvent virtualiser des données à partir de fichiers CSV dans HDFS. Ce processus permet aux données de rester à leur emplacement d’origine, mais il est possible de les interroger à partir d’une instance de SQL Server au même titre qu’une table. Cette fonctionnalité utilise des connecteurs Polybase et minimise le recours aux processus ETL. Pour plus d’informations sur la virtualisation des données, consultez [Qu’est-ce que PolyBase ?](../relational-databases/polybase/polybase-guide.md)

## <a name="prerequisites"></a>Prérequis

- [Cluster Big Data déployé](deployment-guidance.md)
- [Azure Data Studio](../azure-data-studio/download-azure-data-studio.md)

## <a name="select-or-upload-a-csv-file-for-data-virtualization"></a>Sélectionner ou charger un fichier CSV pour la virtualisation des données 

Dans Azure Data Studio (ADS), [connectez-vous à l’instance maître de SQL Server](connect-to-big-data-cluster.md#master) de votre cluster Big Data. Une fois connecté, développez les éléments HDFS dans l’Explorateur d’objets pour localiser le ou les fichiers CSV dont vous souhaitez virtualiser les données. 

Pour les besoins de ce tutoriel, nous allons créer un répertoire nommé **Data**.

1. Cliquez avec le bouton droit sur le menu contextuel du répertoire racine HDFS.
2. Cliquez sur **Nouveau répertoire**.
3. Nommez le nouveau répertoire *Data*.

Chargez l’exemple de données. Pour une procédure pas à pas simple, vous pouvez utiliser un exemple de fichier de données CSV. Cet article utilise les données sur les causes des retards aériens du [ministère américain des Transports (USDOT)](https://www.transtats.bts.gov/OT_Delay/OT_DelayCause1.asp?pn=1). Téléchargez les données brutes et extrayez-les sur votre ordinateur. Nommez le fichier *airline_delay_causes.csv*.

Pour charger l’exemple de fichier une fois celui-ci extrait :

1. Dans Azure Data Studio, *cliquez avec le bouton droit* sur le nouveau répertoire que vous avez créé. 
2. Cliquez sur **Charger des fichiers**.

![Exemple de fichier CSV dans HDFS](media/data-virtualization/100-csv-sample-file-hdfs.png)

Azure Data Studio charge les fichiers dans HDFS sur le cluster Big Data.

## <a name="create-the-storage-pool-external-data-source-in-your-target-database"></a>Créer la source de données externe du pool de stockage dans votre base de données cible

La source de données externe du pool de stockage n’est pas créée par défaut dans une base de données au sein de votre cluster Big Data. Avant de pouvoir créer la table externe, créez la source de données externe **SqlStoragePool** par défaut dans votre base de données cible à l’aide de la requête Transact-SQL suivante. N’oubliez pas de remplacer d’abord le contexte de la requête par votre base de données cible.

```sql
-- Create the default storage pool source for SQL Big Data Cluster
IF NOT EXISTS(SELECT * FROM sys.external_data_sources WHERE name = 'SqlStoragePool')
    CREATE EXTERNAL DATA SOURCE SqlStoragePool
    WITH (LOCATION = 'sqlhdfs://controller-svc/default');
```

## <a name="create-the-external-table"></a>Créer la table externe

Dans ADS, cliquez avec le bouton droit sur le fichier CSV, puis sélectionnez **Créer une table externe à partir d’un fichier CSV** dans le menu contextuel. Vous pouvez également créer des tables externes à partir de fichiers CSV d’un répertoire dans HDFS si les fichiers en question respectent le même schéma. Cela permet la virtualisation des données au niveau des répertoires, sans nécessiter le traitement de chaque fichier ou l’obtention d’un jeu de résultats joints à partir des données combinées. Azure Data Studio vous guide tout au long des étapes de création de la table externe.

Spécifiez la base de données, la source de données, un nom de table, le schéma et le nom du format de fichier externe de la table.

Cliquez sur **Suivant**.

## <a name="preview-data"></a>Aperçu des données

Azure Data Studio fournit un aperçu des données importées.

![Capture d’écran montrant la fenêtre Créer une table externe à partir d’un fichier CSV avec un aperçu des données importées.](media/data-virtualization/130-csv-preview-data.png)

Une fois terminé, cliquez sur **Suivant** pour continuer.

## <a name="modify-columns"></a>Modifier les colonnes

Dans la fenêtre suivante, vous pouvez modifier les colonnes de la table externe que vous avez l’intention de créer. Vous pouvez modifier le nom de la colonne, modifier le type des données et autoriser les lignes pouvant accepter la valeur Null. 

![Capture d’écran de la fenêtre Créer une table externe à partir d’un fichier CSV montrant l’Étape 3 : Modifier les colonnes.](media/data-virtualization/140-csv-modify-columns.png)

Après avoir vérifié les colonnes de destination, cliquez sur **Suivant**.

## <a name="summary"></a>Résumé

Cette étape fournit un récapitulatif de vos sélections. Elle fournit le nom du serveur SQL Server, le nom de la base de données, le nom de la table, le schéma de la table et les informations de la table externe. Dans cette étape, vous pouvez générer un script ou créer une table. **Générer un script** crée un script en T-SQL pour créer la source de données externe. **Créer la table** crée la source de données externe.

![Écran Récapitulatif](media/data-virtualization/150-csv-virtualize-data-summary.png)

Si vous cliquez sur **Créer la table**, SQL Server crée la table externe dans la base de données de destination.

Si vous cliquez sur **Générer un script**, Azure Data Studio crée la requête T-SQL pour créer la table externe.

Une fois la table créée, vous pouvez l’interroger directement en utilisant T-SQL à partir de l’instance de SQL Server.

## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations sur SQL Server Big Data Cluster et les scénarios associés, consultez [Qu’est-ce que SQL Server Big Data Cluster ?](big-data-cluster-overview.md).
