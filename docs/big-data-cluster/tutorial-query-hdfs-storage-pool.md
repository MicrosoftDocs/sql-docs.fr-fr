---
title: 'Interroger les données HDFS : pool de stockage'
titleSuffix: SQL Server Big Data Clusters
description: Ce tutoriel montre comment interroger les données HDFS dans un cluster Big Data SQL Server 2019. Vous créez une table externe sur les données dans le pool de stockage, puis exécutez une requête.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mihaelab
ms.metadata: seo-lt-2019
ms.date: 12/13/2019
ms.topic: tutorial
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: aa376549bf3d6430ab5967e193069d35650be76b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100043534"
---
# <a name="tutorial-query-hdfs-in-a-sql-server-big-data-cluster"></a>Tutoriel : Interroger HDFS dans un cluster Big Data SQL Server

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Ce tutoriel montre comment interroger des données HDFS dans un [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)].

Dans ce tutoriel, vous allez apprendre à :

> [!div class="checklist"]
> * Créer une table externe pointant vers des données HDFS dans un cluster Big Data.
> * Associer ces données à des données à valeur élevée dans l’instance maître.

> [!TIP]
> Si vous préférez, vous pouvez télécharger et exécuter un script pour les commandes de ce tutoriel. Pour obtenir des instructions, consultez les [exemples de virtualisation de données](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/sql-big-data-cluster/data-virtualization) sur GitHub.

Cette vidéo de 7 minutes vous guide tout au long de l’interrogation des données HDFS dans un cluster Big Data :

> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/Query-HDFS-data-inside-SQL-Server-big-data-cluster/player?WT.mc_id=dataexposed-c9-niner]

## <a name="prerequisites"></a><a id="prereqs"></a> Conditions préalables

- [Outils Big Data](deploy-big-data-tools.md)
   - **kubectl**
   - **Azure Data Studio**
   - **Extension SQL Server 2019**
- [Charger des exemples de données dans votre cluster Big Data](tutorial-load-sample-data.md)

## <a name="create-an-external-table-to-hdfs"></a>Créer une table externe pour HDFS

Le pool de stockage contient des données de parcours web dans un fichier CSV stocké dans HDFS. Procédez comme suit pour définir une table externe qui peut accéder aux données dans ce fichier.

1. Dans Azure Data Studio, connectez-vous à l’instance maître SQL Server de votre cluster Big Data. Pour plus d’informations, consultez [Se connecter à l’instance maître SQL Server](connect-to-big-data-cluster.md#master).

1. Double-cliquez sur la connexion dans la fenêtre **Serveurs** pour afficher le tableau de bord de serveur de l’instance maître SQL Server. Sélectionnez **Nouvelle requête**.

   ![Requête d’instance maître SQL Server](./media/tutorial-query-hdfs-storage-pool/sql-server-master-instance-query.png)

1. Exécutez la commande Transact-SQL suivante pour remplacer le contexte par celui de la base de données **Sales** dans l’instance maître.

   ```sql
   USE Sales
   GO
   ```

1. Définissez le format du fichier CSV à lire à partir de HDFS. Appuyez sur la touche F5 pour exécuter l’instruction.

   ```sql
   CREATE EXTERNAL FILE FORMAT csv_file
   WITH (
       FORMAT_TYPE = DELIMITEDTEXT,
       FORMAT_OPTIONS(
           FIELD_TERMINATOR = ',',
           STRING_DELIMITER = '"',
           FIRST_ROW = 2,
           USE_TYPE_DEFAULT = TRUE)
   );
   ```

1. Créez une source de données externe pour le pool de stockage, si elle n’existe pas déjà.

   ```sql
   IF NOT EXISTS(SELECT * FROM sys.external_data_sources WHERE name = 'SqlStoragePool')
   BEGIN
     CREATE EXTERNAL DATA SOURCE SqlStoragePool
     WITH (LOCATION = 'sqlhdfs://controller-svc/default');
   END
   ```

1. Créez une table externe capable de lire les `/clickstream_data` à partir du pool de stockage. **SqlStoragePool** est accessible à partir de l’instance maître d’un cluster Big Data.

   ```sql
   CREATE EXTERNAL TABLE [web_clickstreams_hdfs]
   ("wcs_click_date_sk" BIGINT , "wcs_click_time_sk" BIGINT , "wcs_sales_sk" BIGINT , "wcs_item_sk" BIGINT , "wcs_web_page_sk" BIGINT , "wcs_user_sk" BIGINT)
   WITH
   (
       DATA_SOURCE = SqlStoragePool,
       LOCATION = '/clickstream_data',
       FILE_FORMAT = csv_file
   );
   GO
   ```

## <a name="query-the-data"></a>Interroger les données

Exécutez la requête suivante pour associer les données HDFS de la table externe `web_clickstream_hdfs` aux données relationnelles de la base de données `Sales` locale.

```sql
SELECT  
    wcs_user_sk,
    SUM( CASE WHEN i_category = 'Books' THEN 1 ELSE 0 END) AS book_category_clicks,
    SUM( CASE WHEN i_category_id = 1 THEN 1 ELSE 0 END) AS [Home & Kitchen],
    SUM( CASE WHEN i_category_id = 2 THEN 1 ELSE 0 END) AS [Music],
    SUM( CASE WHEN i_category_id = 3 THEN 1 ELSE 0 END) AS [Books],
    SUM( CASE WHEN i_category_id = 4 THEN 1 ELSE 0 END) AS [Clothing & Accessories],
    SUM( CASE WHEN i_category_id = 5 THEN 1 ELSE 0 END) AS [Electronics],
    SUM( CASE WHEN i_category_id = 6 THEN 1 ELSE 0 END) AS [Tools & Home Improvement],
    SUM( CASE WHEN i_category_id = 7 THEN 1 ELSE 0 END) AS [Toys & Games],
    SUM( CASE WHEN i_category_id = 8 THEN 1 ELSE 0 END) AS [Movies & TV],
    SUM( CASE WHEN i_category_id = 9 THEN 1 ELSE 0 END) AS [Sports & Outdoors]
  FROM [dbo].[web_clickstreams_hdfs]
  INNER JOIN item it ON (wcs_item_sk = i_item_sk
                        AND wcs_user_sk IS NOT NULL)
GROUP BY  wcs_user_sk;
GO
```

## <a name="clean-up"></a>Nettoyer

Utilisez la commande suivante pour supprimer la table externe utilisée dans ce tutoriel.

```sql
DROP EXTERNAL TABLE [dbo].[web_clickstreams_hdfs];
GO
```

## <a name="next-steps"></a>Étapes suivantes

Passez à l’article suivant pour découvrir comment interroger Oracle à partir d’un cluster Big Data.
> [!div class="nextstepaction"]
> [Interroger des données externes dans Oracle](tutorial-query-oracle.md)
