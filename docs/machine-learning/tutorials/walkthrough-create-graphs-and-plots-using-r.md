---
title: 'Didacticiel R : Créer des graphes et des tracés'
description: Découvrez les techniques de génération de tracés et de cartes à l’aide du langage R avec des données SQL Server. Créez un histogramme simple, puis développez un tracé de carte plus complexe.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 06/13/2019
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: edf65095f245e5185b9eeadbd1d8b6c8e87e49cd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272950"
---
# <a name="create-graphs-and-plots-using-sql-and-r-walkthrough"></a>Créer des graphiques et des tracés à l’aide de SQL et R (procédure pas à pas)
[!INCLUDE [SQL Server 2016](../../includes/applies-to-version/sqlserver2016.md)]

Dans cette partie de la procédure pas à pas, vous allez apprendre des techniques de génération de tracés et de cartes en utilisant R avec des données SQL Server. Vous allez d’abord créer un histogramme simple, puis développer un tracé de carte plus complexe.

## <a name="prerequisites"></a>Prérequis

Cette étape suppose que vous utilisez une session R ouverte basée sur les étapes précédentes de cette procédure pas à pas. Elle utilise les chaînes de connexion et les objets de source de données créés dans ces étapes. Les outils et les packages suivants sont utilisés pour exécuter le script.

+ RGUI.exe pour exécuter des commandes R
+ Management Studio pour exécuter T-SQL
+ googMap
+ package ggmap
+ package mapproj

## <a name="create-a-histogram"></a>Créer un histogramme

1. Générez le premier tracé à l’aide de la fonction [rxHistogram](/r-server/r-reference/revoscaler/rxdatasource) .  La fonction rxHistogram fournit une fonctionnalité similaire à celle des packages R open source, mais peut s’exécuter dans un contexte d’exécution à distance.

    ```R
    # Plot fare amount on SQL Server and return the plot
    start.time <- proc.time()
    rxHistogram(~fare_amount, data = inDataSource, title = "Fare Amount Histogram")
    used.time <- proc.time() - start.time
    print(paste("It takes CPU Time=", round(used.time[1]+used.time[2],2), " seconds, Elapsed Time=", round(used.time[3],2), " seconds to generate plot.", sep=""))
    ```

2. L’image est retourné dans l’appareil graphique R pour votre environnement de développement.  Par exemple, dans RStudio, cliquez sur la fenêtre **Plot** (Tracer).  Dans [!INCLUDE[rsql_rtvs](../../includes/rsql-rtvs-md.md)], une fenêtre graphique distincte est ouverte.

    ![utilisation de rxHistogram pour le tracé des prix](media/rsql-e2e-rxhistogramresult.png "utilisation de rxHistogram pour le tracé des prix")

    > [!NOTE]
    > Votre graphique est-il différent ?
    >  
    > Cela est dû au fait que _inDataSource_ utilise uniquement les 1 000 premières lignes. L’ordonnancement des lignes à l’aide de TOP est non déterministe en l’absence de clause ORDER BY ; les données et le graphique obtenu peuvent donc varier.
    > Cette image a été générée à l’aide de 10 000 lignes de données environ. Nous vous recommandons de faire des essais avec différents nombres de lignes pour obtenir différents graphiques, et de noter le temps nécessaire pour retourner les résultats dans votre environnement.

## <a name="create-a-map-plot"></a>Créer un tracé de carte

En règle générale, les serveurs de base de données bloquent l’accès à Internet. Cela peut être gênant quand vous utilisez des packages R qui doivent télécharger des cartes ou d’autres images pour générer des tracés. Il existe néanmoins une solution de contournement utile lorsque vous développez vos propres applications. En fait, vous générez la représentation sous forme de carte sur le client, puis superposez sur la carte les points qui sont stockés comme attributs dans la table SQL Server.

1. Définissez la fonction qui crée l’objet de tracé R. La fonction personnalisée *mapPlot* crée un nuage de points qui utilise les lieux de prise en charge des passagers pour tracer le nombre de trajets ayant démarré à chaque emplacement. Elle utilise les packages **ggplot2** et **ggmap**, qui doivent déjà être [installés et chargés](walkthrough-data-science-end-to-end-walkthrough.md#add-packages).

    ```R
    mapPlot <- function(inDataSource, googMap){
        library(ggmap)
        library(mapproj)
        ds <- rxImport(inDataSource)
        p <- ggmap(googMap)+
        geom_point(aes(x = pickup_longitude, y =pickup_latitude ), data=ds, alpha =.5,
    color="darkred", size = 1.5)
        return(list(myplot=p))
    }
    ```

    + La fonction *mapPlot* prend deux arguments : un objet de données existant, que vous avez défini précédemment à l’aide de RxSqlServerData, et la représentation sous forme de carte transmise à partir du client.
    + Dans la ligne qui commence par la variable *ds*, rxImport est utilisé pour charger dans la mémoire les données issues de la source de données créée précédemment, *inDataSource*. (Cette source de données contient uniquement 1000 lignes ; si vous souhaitez créer une carte avec plus de points de données, vous pouvez substituer une autre source de données.)
    + Chaque fois que vous utilisez des fonctions R open source, les données doivent être chargées dans des trames de données dans la mémoire locale. Toutefois, en appelant la fonction [rxImport](/r-server/r-reference/revoscaler/rximport), vous pouvez procéder à l’exécution dans la mémoire du contexte de calcul distant.

2. Basculez sur le contexte de calcul local et chargez les bibliothèques nécessaires pour créer les cartes.

    ```R
    rxSetComputeContext("local")
    library(ggmap)
    library(mapproj)
    gc <- geocode("Times Square", source = "google")
    googMap <- get_googlemap(center = as.numeric(gc), zoom = 12, maptype = 'roadmap', color = 'color');
    ```

    + La variable `gc` stocke un jeu de coordonnées pour Times Square, NY.

    + La ligne commençant par `googmap` génère une carte avec les coordonnées spécifiées au centre.

3. Basculez vers le contexte de calcul SQL Server et restituez les résultats en incluant dans un wrapper la fonction de tracé dans [rxExec](/r-server/r-reference/revoscaler/rxexec), comme illustré ici. La fonction rxExec fait partie du package **RevoScaleR** et prend en charge l’exécution de fonctions R arbitraires dans un contexte de calcul distant.

    ```R
    rxSetComputeContext(sqlcc)
    myplots <- rxExec(mapPlot, inDataSource, googMap, timesToRun = 1)
    plot(myplots[[1]][["myplot"]]);
    ````

    + Les données de carte dans `googMap` sont transmises en tant qu’argument à la fonction *mapPlot* exécutée à distance. Comme les cartes ont été générées dans votre environnement local, elles doivent être transmises à la fonction pour créer le tracé dans le contexte de SQL Server.

    + Lorsque la ligne qui commence par `plot` s’exécute, les données restituées sont sérialisées dans l’environnement R local afin que vous puissiez les afficher dans votre client R.

    > [!NOTE]
    > Si vous utilisez SQL Server sur une machine virtuelle Azure, vous pouvez recevoir une erreur à ce stade. Une erreur se produit lorsque la règle de pare-feu par défaut dans Azure bloque l’accès réseau par le code R. Pour plus d’informations sur la résolution de cette erreur, consultez [Installer SQL Server Machine Learning Services avec R et Python sur une machine virtuelle Azure](../install/sql-machine-learning-azure-virtual-machine.md).

4. L’image suivante montre le tracé de sortie. Les emplacements de montée en taxi sont ajoutés à la carte sous forme de points rouges. Votre image peut paraître différente, en fonction du nombre d’emplacements dans la source de données que vous avez utilisée.

    ![tracés des courses de taxi à l’aide d’une fonction R personnalisée](media/rsql-e2e-mapplot.png "tracés des courses de taxi à l’aide d’une fonction R personnalisée")

## <a name="next-steps"></a>Étapes suivantes

> [!div class="nextstepaction"]
> [Créer des caractéristiques de données à l’aide de R et SQL](walkthrough-create-data-features.md)