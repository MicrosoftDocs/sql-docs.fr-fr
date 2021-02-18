---
title: Données de démonstration Taxis de New York pour les didacticiels
description: Créez une base de données contenant les exemples de données des taxis de la ville de New York. Ce jeu de données est utilisé dans les didacticiels R et Python pour SQL Server Machine Learning Services.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 10/31/2018
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||>=azuresqldb-mi-current'
ms.openlocfilehash: 0a62131735fb5612af44f67cdbb03359563019f3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347344"
---
# <a name="nyc-taxi-demo-data-for-sql-server-python-and-r-tutorials"></a>Données de démonstration Taxis de New York pour les didacticiels SQL Server Python et R
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

Cet article explique comment configurer une base de données exemple qui se compose de données publiques issues de la [New York City Taxi and Limousine Commission](http://www.nyc.gov/html/tlc/html/about/trip_record_data.shtml). Ces données sont utilisées dans plusieurs didacticiels R et Python pour l’analytique en base de données sur SQL Server. Pour accélérer l’exécution de l’exemple de code, nous avons créé un échantillon représentatif de 1 % des données. Sur votre système, le fichier de sauvegarde de la base de données fait légèrement plus de 90 Mo, ce qui donne 1,7 million de lignes dans la table de données principale.

Pour effectuer cet exercice, vous devez avoir [SQL Server Management Studio](../../ssms/download-sql-server-management-studio-ssms.md?view=sql-server-2017&preserve-view=true) ou un autre outil capable de restaurer un fichier de sauvegarde de base de données et d’exécuter des requêtes T-SQL.

Voici les tutoriels et les démarrages rapides qui utilisant ce jeu de données :

+ [Découvrir l’analytique en base de données à l’aide de R dans SQL Server](r-taxi-classification-introduction.md)
+ [Découvrir l’analytique en base de données à l’aide de Python dans SQL Server](python-taxi-classification-introduction.md)

## <a name="download-files"></a>Télécharger les fichiers

La base de données exemple est un fichier SQL Server 2016 BAK hébergé par Microsoft. Vous pouvez le restaurer sur SQL Server 2016 et les versions ultérieures. Le téléchargement des fichiers démarre immédiatement lorsque vous cliquez sur le lien. 

La taille du fichier est d’environ 90 Mo.

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
>[!NOTE]
>Pour restaurer l’exemple de base de données sur [Clusters Big Data SQL Server](../../big-data-cluster/big-data-cluster-overview.md), téléchargez [NYCTaxi_Sample.bak](https://sqlmldoccontent.blob.core.windows.net/sqlml/NYCTaxi_Sample.bak) et suivez les instructions [Restauration d’une base de données dans l’instance maître Clusters Big Data SQL Server](../../big-data-cluster/data-ingestion-restore-database.md).
::: moniker-end

::: moniker range=">=azuresqldb-mi-current"
>[!NOTE]
>Pour restaurer l’exemple de base de données sur [Machine Learning Services dans Azure SQL Managed Instance](/azure/azure-sql/managed-instance/machine-learning-services-overview), suivez les instructions de la section [Démarrage rapide : Restaurer une base de données dans Azure SQL Managed Instance](/azure/azure-sql/managed-instance/restore-sample-database-quickstart) à l’aide du fichier .bak de la base de données de démonstration des services de taxi de New York : [https://sqlmldoccontent.blob.core.windows.net/sqlml/NYCTaxi_Sample.bak](https://sqlmldoccontent.blob.core.windows.net/sqlml/NYCTaxi_Sample.bak).
::: moniker-end

1. Cliquez sur [NYCTaxi_Sample.bak](https://sqlmldoccontent.blob.core.windows.net/sqlml/NYCTaxi_Sample.bak) pour télécharger le fichier de sauvegarde de la base de données.

2. Copiez le fichier dans C:\Program files\Microsoft SQL Server\MSSQL-instance-name\MSSQL\Backup folder.

3. Dans Management Studio, cliquez avec le bouton droit sur **Databases** (Bases de données), puis sélectionnez **Restore Files and File Groups** (Restaurer les fichiers et groupes de fichiers).

4. Entrez *NYCTaxi_Sample* comme nom de la base de données.

5. Cliquez sur **From device** (À partir de l’appareil), puis ouvrez la page de sélection du fichier pour sélectionner le fichier de sauvegarde. Cliquez sur **Add** (Ajouter) pour sélectionner NYCTaxi_Sample.bak.

6. Cochez la case **Restore** (Restaurer), puis cliquez sur **OK** pour restaurer la base de données.

## <a name="review-database-objects"></a>Vérifier les objets de base de données
   
Vérifiez que les objets de base de données existent sur l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à l’aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Vous devez voir la base de données, les tables, les fonctions et les procédures stockées.
  
   ![rsql_devtut_BrowseTables](media/rsql-devtut-browsetables.png "rsql_devtut_BrowseTables")

### <a name="objects-in-nyctaxi_sample-database"></a>Objets dans la base de données NYCTaxi_Sample

Le tableau suivant récapitule les objets créés dans la base de données de démonstration Taxi de NYC.

|**Nom de l'objet**|**Type d'objet**|**Description**|
|----------|------------------------|---------------|
|**NYCTaxi_Sample** | database | Crée une base de données et deux tables :<br /><br />Table dbo.nyctaxi_sample : contient le jeu de données principal Taxis de New York. Un index cluster columnstore est ajouté à la table pour améliorer les performances du stockage et des requêtes. L’échantillon de 1 % du jeu de données Taxis de New York sera inséré dans cette table.<br /><br />Table dbo.nyc_taxi_models : permet de rendre persistant le modèle d’analytique avancée entraîné.|
|**fnCalculateDistance** |fonction scalaire | Calcule la distance directe entre les lieux de prise en charge et de dépose. Cette fonction est utilisée pour [Créer des caractéristiques de données](r-taxi-classification-create-features.md), [Entraîner et enregistrer un modèle](r-taxi-classification-train-model.md) et [Rendre le modèle R opérationnel](r-taxi-classification-deploy-model.md).|
|**fnEngineerFeatures** |fonction table | Crée de nouvelles caractéristiques de données pour l’apprentissage du modèle. Cette fonction est utilisée pour [Créer des caractéristiques de données](r-taxi-classification-create-features.md) et [Rendre le modèle R opérationnel](r-taxi-classification-deploy-model.md).|


Les procédures stockées sont créées à l’aide de scripts R et Python disponibles dans différents didacticiels. Le tableau suivant récapitule les procédures stockées que vous pouvez éventuellement ajouter à la base de données de démonstration Taxis de New York quand vous exécutez un script de différentes leçons.

|**Procédure stockée**|**Langage**|**Description**|
|-------------------------|------------|---------------|
|**RxPlotHistogram** |R | Appelle la fonction RevoScaleR rxHistogram pour tracer l’histogramme d’une variable, puis retourne le tracé sous forme d’objet binaire. Cette procédure stockée est utilisée pour [Explorer et visualiser les données](r-taxi-classification-explore-data.md).|
|**RPlotRHist** |R| Crée un graphique à l’aide de la fonction Hist, puis enregistre la sortie dans un fichier PDF local. Cette procédure stockée est utilisée pour [Explorer et visualiser les données](r-taxi-classification-explore-data.md).|
|**RxTrainLogitModel**  |R| Effectue l’apprentissage d’un modèle de régression logistique en appelant un package R. Le modèle prédit la valeur de la colonne tipped et est formé à l’aide d’un échantillon de 70 % des données sélectionné de façon aléatoire. La sortie de la procédure stockée représente le modèle formé, qui est enregistré dans la table nyc_taxi_models. Cette procédure stockée est utilisée pour [Entraîner et enregistrer un modèle](r-taxi-classification-train-model.md).|
|**RxPredictBatchOutput**  |R | Appelle le modèle entraîné pour créer des prédictions à l’aide du modèle. La procédure stockée accepte une requête comme paramètre d’entrée et retourne une colonne de valeurs numériques qui contient les scores pour les lignes d’entrée. Cette procédure stockée est utilisée pour [Prédire les résultats potentiels](r-taxi-classification-deploy-model.md).|
|**RxPredictSingleRow**  |R| Appelle le modèle entraîné pour créer des prédictions à l’aide du modèle. Cette procédure stockée accepte une nouvelle observation comme entrée, avec des valeurs de caractéristiques passées comme paramètres inline, et retourne une valeur qui prédit l’issue de la nouvelle observation. Cette procédure stockée est utilisée pour [Prédire les résultats potentiels](r-taxi-classification-deploy-model.md).|

## <a name="query-the-data"></a>Interroger les données

En guise d’étape de validation, exécutez une requête pour confirmer que les données ont été chargées.

1. Dans l’Explorateur d’objets, sous Bases de données, cliquez avec le bouton droit sur la base de données **NYCTaxi_Sample**, puis démarrez une nouvelle requête.

2. Exécutez des requêtes simples :

    ```sql
    SELECT TOP(10) * FROM dbo.nyctaxi_sample;
    SELECT COUNT(*) FROM dbo.nyctaxi_sample;
    ```
La base de données contient 1,7 million de lignes.

3. Dans la base de données se trouve une table **nyctaxi_sample** qui contient le jeu de données. Pour optimiser cette table de données pour les calculs basés sur les jeux, un [index columnstore](../../relational-databases/indexes/columnstore-indexes-overview.md) a été ajouté. Exécutez cette instruction pour générer un résumé rapide sur la table.

    ```sql
    SELECT DISTINCT [passenger_count]
        , ROUND (SUM ([fare_amount]),0) as TotalFares
        , ROUND (AVG ([fare_amount]),0) as AvgFares
    FROM [dbo].[nyctaxi_sample]
    GROUP BY [passenger_count]
    ORDER BY  AvgFares DESC
    ````
Les résultats doivent être similaires à ceux affichés dans la capture d’écran suivante.

  ![Informations de résumé de la table](media/nyctaxidatatablesummary.png "Résultats de la requête")

## <a name="next-steps"></a>Étapes suivantes

L’échantillon de données Taxis de New York est désormais disponible pour vos travaux pratiques.

+ [Découvrir l’analytique en base de données à l’aide de R dans SQL Server](r-taxi-classification-introduction.md)
+ [Découvrir l’analytique en base de données à l’aide de Python dans SQL Server](python-taxi-classification-introduction.md)