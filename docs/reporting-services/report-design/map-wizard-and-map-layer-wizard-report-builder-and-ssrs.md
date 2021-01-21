---
title: Assistant Carte et Assistant Couche (Générateur de rapports) | Microsoft Docs
description: Découvrez comment automatiser la création d’une carte, l’ajout d’une couche ou la modification des options de couche à l’aide de l’Assistant Carte ou de l’Assistant Couche du Générateur de rapports.
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-design
ms.topic: reference
f1_keywords:
- sql13.rtp.rptdesigner.mapandlayerwizard.f1
- "10542"
- MICROSOFT.REPORTDESIGNER.MAPLAYER.NAME
ms.assetid: 48cbe18b-1290-4107-8a1c-ec6acd71f73b
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: b307966577cfd30197963263597aee7a9d06e018
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/20/2021
ms.locfileid: "98597335"
---
# <a name="map-wizard-and-map-layer-wizard-report-builder-and-ssrs"></a>Assistant Carte et Assistant Couche (Générateur de rapports et SSRS)
 Dans les rapports paginés [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)] , l’Assistant Carte et l’Assistant Couche automatisent la tâche de création d’une carte, d’ajout d’une couche ou de modification des options de couche sur une couche existante.  
  
 Avant d’ajouter une carte à un rapport ou une couche à une carte, recueillez les informations suivantes :  
  
-   **Source de données spatiales.** Emplacement ou connexion à une source qui fournit des données spatiales, par exemple le nom d’une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et d’une base de données contenant des données spatiales, ou celui d’un fichier de forme ESRI (Environmental Systems Research Institute)  
  
-   **Spatial data.** Champ de la source de données spatiales contenant des jeux de coordonnées qui spécifient des emplacements.  
  
-   **Données analytiques.** Données analytiques utilisées pour modifier l'affichage du carte, par exemple, les chiffres de ventes annuels des magasins.  
  
-   **Champs de correspondance.** Champs de correspondance définissant la relation entre les données spatiales et les données analytiques, par exemple le nom d'une région et d'une ville pour identifier chaque ville de façon unique.  
  
 Les sections suivantes présentent des informations sur les options que vous spécifiez dans les Assistants Carte et Couche.  
  
-   À l'ouverture du Générateur de rapports, cliquez sur l'icône de l'Assistant **Carte** au milieu de l'aire de conception.  
  
-   Sous l'onglet **Insérer** , cliquez sur **Carte**, puis sur **Assistant Carte**.  
  
 Pour ouvrir l'Assistant Couche, procédez comme suit :  
  
-   Cliquez sur la carte pour afficher le volet Carte et, dans la barre d'outils, cliquez sur le bouton **Assistant Nouvelle couche** .  
  
 Cliquez sur le titre de la page de l'Assistant pour consulter l'aide correspondante. Les pages qui s'affichent varient en fonction de vos choix pour le type de carte, la source de données spatiales et la source de données analytiques.  
  
1.  [Choisir une source de données spatiales](#SpatialDataSource). Les données spatiales peuvent provenir de la bibliothèque de cartes, d'un fichier de forme ESRI ou des données spatiales d'une base de données relationnelle [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
    -   [Que sont les données spatiales ?](#SpatialData)  
  
    -   [Qu'est-ce que la bibliothèque de cartes ?](#MapGallery)  
  
    -   [Qu'est-ce qu'un fichier de forme ESRI ?](#Shapefile)  
  
    -   [Où puis-je obtenir des fichiers de forme ESRI ?](#GetShapefiles)  
  
    -   [Qu'est-ce qu'une requête spatiale SQL Server ?](#SqlServerSpatial)  
  
2.  [Choisir des options de vue cartographique et de données spatiales](#MapView). Définissez la vue cartographique, la résolution de la carte, spécifiez s'il faut incorporer des données spatiales dans le rapport et s'il faut inclure un arrière-plan de mosaïques Bing [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
    -   [Qu'est-ce que la vue cartographique ou fenêtre d'affichage ?](#Viewport)  
  
    -   [Qu'est-ce que la résolution ou l'optimisation de la carte ?](#Resolution)  
  
    -   [Quel est l'effet de l'incorporation de données spatiales ?](#Embed)  
  
    -   [Qu'est-ce qu'un arrière-plan de mosaïque Bing ?](#Tiles)  
  
3.  [Choisir la visualisation de la carte](#Visualization). Choisissez le type de carte à créer.  
  
    -   [Quelle est la différence entre une carte simple, une carte à bulles et une carte analytique ?](#MapType)  
  
    -   Choisir la visualisation de la carte : Polygones  
  
    -   Choisir la visualisation de la carte : Lignes  
  
    -   Choisir la visualisation de la carte : Points  
  
4.  Choisir une connexion à une source de données. Choisissez la visualisation de la carte : Points. Choisissez une connexion à une source de données ou créez-en une à une source de données externe qui contient des données analytiques à afficher sur la carte.  
  
5.  Créer une requête. Générez une requête qui spécifie les données analytiques.  
  
6.  [Choisir le dataset analytique](#AnalyticalData). Spécifiez une source de données pour les données analytiques.  
  
    -   [Quelle est la différence entre les données spatiales et les données analytiques ?](#Diff)  
  
7.  [Spécifiez les champs de correspondance pour les données spatiales et les données analytiques](#SpecifyMatchFields). Générez une relation entre les données spatiales et les données analytiques afin que l'apparence des éléments cartographiques puisse varier en fonction des données.  
  
    -   [Qu'est-ce qu'un champ de correspondance ?](#MatchFields)  
  
8.  [Choisir un thème de couleurs et une visualisation des données](#ThemeandVisualization). Pour spécifier comment visualiser vos données sur l'arrière-plan de la carte, spécifiez le thème de la carte, les champs à visualiser et les éléments à varier : couleur, taille ou type de marqueur.  
  
    -   [Quelle est la fonction du thème ?](#Theme)  
  
    -   [À quoi servent les légendes et les échelles dans l'aperçu de la carte ?](#Legends)  
  
    -   [Qu'est-ce que des règles ?](#Rules)  
  
 Après avoir ajouté une carte ou une couche et affiché un aperçu du rapport, vous pouvez modifier les options de carte et de couche que vous avez définies dans les Assistants. Pour plus d’informations, consultez [Personnaliser des données et l’affichage d’une carte ou d’une couche &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/customize-the-data-and-display-of-a-map-or-map-layer-report-builder-and-ssrs.md).  
  
 Pour plus d’informations sur les cartes, consultez [Cartes &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/maps-report-builder-and-ssrs.md). Pour savoir pas à pas comment ajouter une carte à un rapport, voir [Tutoriel : Rapport cartographique &#40;Générateur de rapports&#41;](../../reporting-services/tutorial-map-report-report-builder.md).  
  
##  <a name="choose-a-source-of-spatial-data"></a><a name="SpatialDataSource"></a> Choisir une source de données spatiales  
 Dans cette page, spécifiez la source de données spatiales et les données spatiales à inclure. Les données spatiales peuvent provenir de la bibliothèque de cartes, d'un fichier de forme ESRI ou d'une requête de dataset spécifiant des données spatiales [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] d'une base de données [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] ou version ultérieure.  
  
 Vous pouvez utiliser la même source ou une source différente de données spatiales pour chaque couche, mais vous devez spécifier la source chaque fois que vous ajoutez une couche. Lorsque les données spatiales proviennent de la bibliothèque de cartes ou d'un fichier de forme ESRI, la source de données spatiales n'est pas un élément distinct du rapport. Elle n'apparaît pas dans le volet des données de rapport.  
  
###  <a name="what-is-spatial-data"></a><a name="SpatialData"></a> Que sont les données spatiales ?  
 Les données spatiales contiennent des coordonnées qui définissent des éléments géographiques ou géométriques. Dans une carte, les données spatiales définissent des *éléments cartographiques*: des polygones qui définissent des zones ou des formes, des lignes qui définissent des itinéraires ou des chemins d'accès et des points qui définissent des marqueurs ou des punaises. Les données spatiales sont stockées au format binaire dans la source de données et spécifiées comme jeux de coordonnées. Par exemple, un point a deux coordonnées X et Y (X Y), une ligne deux jeux de coordonnées ((X1 Y1), (X2 Y2)), un polygone quatre jeux de coordonnées ou plus, le premier et le dernier jeu de coordonnées étant identiques ((X1 Y1), (X1 Y1) (X3 Y3) (X2 Y2)).  
  
 Pour plus d'informations, consultez la documentation pour le type de données spatiales que vous utilisez.  
  
###  <a name="what-is-the-map-gallery"></a><a name="MapGallery"></a> What is the map gallery?  
 La bibliothèque de cartes contient des cartes de rapports stockés dans le dossier Bibliothèque de cartes pour l'environnement de création de rapports. Les cartes de la bibliothèque fournissent un point de départ pratique pour ajouter rapidement une carte à votre rapport. Les cartes prédéfinies de la bibliothèque sont fournies par un fournisseur de cartes.  
  
> [!NOTE]  
>  Cette fonctionnalité de cartographie [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] utilise des données de fichiers de forme TIGER/Line gracieusement fournis par Bureau de recensement ([https://www.census.gov/](https://www.census.gov/)). Les fichiers de forme TIGER/Line sont un extrait d'informations géographiques et cartographiques sélectionnées de la base de données MAF/TIGER du Bureau de recensement. Les fichiers de forme TIGER/Line sont mis à disposition gratuitement par le Bureau de recensement des États-Unis. Pour obtenir plus d’informations sur les fichiers de forme TIGER/Line allez à [Fichiers de forme TIGER/Line et documentation technique des fichiers TIGER/Line](https://www.census.gov/programs-surveys/geography/technical-documentation/complete-technical-documentation/tiger-geo-line.html). Les informations de frontières dans les fichiers de forme TIGER/Line sont fournies à des fins de collecte et de tabulation de données statistiques uniquement ; leur description et leur désignation pour des objectifs statistiques ne constituent pas une détermination d'autorité juridictionnelle ou de droits de propriété et les informations ne constituent pas des descriptions juridiquement valables. Census TIGER et TIGER/Line sont des marques déposées du Bureau de recensement des États-Unis.  
  
 Pour étendre la bibliothèque de cartes, vous pouvez ajouter ou supprimer des rapports du répertoire de la bibliothèque de cartes et ajouter des dossiers pour organiser les cartes. Pour plus d’informations, consultez [Cartes &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/maps-report-builder-and-ssrs.md).  
  
###  <a name="what-is-an-esri-shapefile"></a><a name="Shapefile"></a> What is an ESRI shapefile?  
 Un fichier de forme ESRI est un ensemble de fichiers contenant des données conformes au format de données spatiales de l'ESRI . Le jeu de fichiers inclut en général le fichier *\<filename>* .shp qui contient les données spatiales, ainsi qu’un fichier de support, *\<filename>* .dbf.  
  
 Lorsque vous spécifiez un fichier de forme en tant que source de données spatiales et que ce fichier se trouve sur votre ordinateur local, les données spatiales sont incorporées automatiquement dans le rapport. Pour utiliser dynamiquement des données spatiales d'un fichier ESRI, vous devez effectuer les opérations suivantes :  
  
 Dans le Générateur de rapports, téléchargez le fichier .shp et le fichier .dbf dans le même dossier sur un serveur de rapports, puis créez un lien vers le fichier .shp comme source de données spatiales.  
  
 Dans le Concepteur de rapports dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], ajoutez le fichier .shp et le fichier .dbf au projet de rapport, puis spécifiez le nom du fichier .shp comme source de données spatiales.  
  
###  <a name="where-can-i-get-esri-shapefiles"></a><a name="GetShapefiles"></a> Où puis-je obtenir des fichiers de forme ESRI ?  
 Les fichiers de forme ESRI sont disponibles sur le Web. Pour plus d’informations, consultez [Rechercher des fichiers de forme ESRI pour une carte](https://go.microsoft.com/fwlink/?linkid=178814).  
  
###  <a name="what-is-a-sql-server-spatial-query"></a><a name="SqlServerSpatial"></a> Qu'est-ce qu'une requête spatiale SQL Server ?  
 Une requête spatiale [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est une requête de dataset qui spécifie des données de type SQLGeometry ou SQLGeography d'une base de données relationnelle [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
> [!NOTE]  
>  Lorsque vous définissez une source de données dans l'Assistant, vous remarquerez que la page Créer une requête contient différents concepteurs de requêtes, selon le type de la source de données à laquelle vous vous connectez. Pour plus d’informations, consultez [Outils de création de requêtes &#40;SSRS&#41;](../report-data/query-design-tools-ssrs.md).  
  
 Lorsque vous exécutez la requête dans le concepteur de requêtes, le jeu de résultats affiche une colonne avec des données spatiales affichées sous forme de texte. Par exemple, une ligne peut contenir des données spatiales qui sont un point unique et la ligne suivante peut contenir des données spatiales qui définissent un jeu de points. Chaque ligne devient un élément cartographique. Vous pouvez varier l'affichage de chaque élément cartographique en tant qu'unité indivisible.  
  
 Pour plus d’informations, consultez [Types de données spatiales](../../relational-databases/spatial/spatial-data-types-overview.md).  
  
##  <a name="choose-spatial-data-and-map-view-options"></a><a name="MapView"></a> Choisir des options de vue cartographique et de données spatiales  
 Dans cette page, vous pouvez définir les options suivantes :  
  
-   Définissez le centre d'affichage et le niveau de zoom des données spatiales que vous avez sélectionnées dans la page précédente de l'Assistant. La vue que vous définissez s'applique à l'ensemble de la carte.  
  
-   Définissez la résolution de la carte.  
  
-   Spécifiez si vous souhaitez incorporer les données spatiales dans le rapport.  
  
-   Pour les données incorporées, spécifiez s'il faut inclure toutes les données ou uniquement les données dans l'affichage en cours.  
  
-   Spécifiez si vous souhaitez inclure un arrière-plan de mosaïques Bing [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
###  <a name="what-is-the-map-view-or-viewport"></a><a name="Viewport"></a> Qu'est-ce que la vue cartographique ou fenêtre d'affichage ?  
 Le point de vue de la carte définit la zone de carte à afficher pour toutes les couches de votre rapport.  
  
 Par défaut, l'échelle de couleurs et l'échelle des distances s'affichent à l'intérieur de la fenêtre d'affichage et la légende de la carte s'affiche à l'extérieur de la fenêtre d'affichage. Vous pouvez modifier ces options pour la fenêtre d'affichage après avoir terminé l'Assistant.  
  
###  <a name="what-is-map-resolution-and-optimization"></a><a name="Resolution"></a> Qu'est-ce que la résolution et l'optimisation de la carte ?  
 Lorsque vous modifiez la résolution pour des données spatiales représentant des lignes ou des polygones, vous spécifiez le niveau de détail de la carte. Par exemple, pour les vues aériennes d'une région, avez-vous besoin d'un degré de granularité allant jusqu'à cent mètres de la surface, ou une résolution d'un kilomètre est-elle suffisante ?  
  
 Lorsque les données spatiales sont incorporées dans le rapport, une résolution plus élevée augmente le nombre d'éléments nécessaires pour faire figurer les détails à cette résolution. Lorsque les données spatiales ne sont pas incorporées dans le rapport, une résolution plus élevée augmente la durée requise par le processeur de rapports pour calculer les lignes pour la carte à cette résolution chaque fois que vous affichez le rapport.  
  
 Si vous réduisez la résolution, le processeur de rapports applique un algorithme pour réduire le nombre de points nécessaires pour faire figurer les éléments cartographiques. Plus la résolution est basse, moins le processeur nécessitera de données pour afficher les éléments cartographiques, ce qui peut entraîner de meilleures performances à l'affichage.  
  
 Lorsque vous ajustez le curseur, les données d'aperçu dans le volet Assistant sont mises à jour pour vous donner une indication de l'effet de la modification. Après avoir ajouté la carte au rapport, vous pouvez ajuster cette valeur en modifiant les options de point de vue de la carte.  
  
###  <a name="what-does-embedding-spatial-data-do"></a><a name="Embed"></a> Quel est l'effet de l'incorporation de données spatiales ?  
 Lorsque vous incorporez des éléments cartographiques ou des mosaïques Bing dans un rapport, les données spatiales sont stockées dans la définition de rapport.  
  
 Un rapport avec une carte peut utiliser des données spatiales ou des mosaïques Bing récupérées dynamiquement soit lors du traitement du rapport, soit au moment de la conception, puis incorporées dans la définition de rapport. Les éléments cartographiques incorporés peuvent augmenter considérablement la taille de la définition de rapport, mais ils réduisent le temps nécessaire pour afficher la carte dans le rapport. Les éléments cartographiques dynamiques réduisent la taille de la définition de rapport, mais ils augmentent le temps nécessaire pour traiter et afficher la carte.  
  
 Une conception de carte efficace requiert l'évaluation des avantages et inconvénients des données de cartes statiques ou dynamiques et la recherche d'un équilibre approprié aux circonstances. En général, un volume plus important de données signifie que la définition de rapport et le rapport compilé requièrent un volume de stockage plus important sur le serveur de rapports et une durée de traitement plus longue. Il est toujours recommandé de rogner les données spatiales, ainsi que de limiter les autres données de rapport, pour inclure uniquement les données nécessaires pour ce rapport.  
  
###  <a name="what-is-a-bing-map-tile-background"></a><a name="Tiles"></a> Qu'est-ce qu'un arrière-plan de mosaïques Bing ?  
 Pour ajouter un arrière-plan d'image géographique à votre carte, sélectionnez l'option d'arrière-plan de mosaïques Bing. Le processeur de rapports télécharge des mosaïques à partir des services Web Bing Maps pour la zone de carte et la résolution spécifiées dans cette page de l'Assistant. Vous pouvez spécifier un des types de mosaïques suivants :  
  
-   **Route.** Affichez un style de carte routière avec arrière-plan blanc.  
  
-   **Aérien.** Affichez une vue aérienne uniquement. Aucun texte n'est affiché dans ce mode.  
  
-   **Caractère hybride.** Affichez la combinaison des vues **Route** et **Aérien** .  
  
 Pour plus d'informations sur les mosaïques, consultez le [système de mosaïques Bing](/bingmaps/articles/bing-maps-tile-system). Pour plus d'informations sur l'utilisation de mosaïques Bing dans votre rapport, consultez [Conditions supplémentaires d'utilisation](https://go.microsoft.com/fwlink/?LinkId=151371).  
  
 Pour afficher un arrière-plan de mosaïques en mode Création, vous devez disposer d'un accès Internet. Pour afficher l'arrière-plan de mosaïques dans l'aperçu d'un rapport sur un serveur de rapports, le serveur de rapports doit être configuré pour prendre en charge les mosaïques Bing. Pour plus d’informations, voir [Résoudre les problèmes des rapports : Rapports cartographiques &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/troubleshoot-reports-map-reports-report-builder-and-ssrs.md) et [Planifier un rapport cartographique](../../reporting-services/report-design/plan-a-map-report-report-builder-and-ssrs.md).  
  
 Pour plus d’informations sur d’autres manières de personnaliser une couche de mosaïques, consultez [Ajouter, modifier ou supprimer une carte ou une couche &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/add-change-or-delete-a-map-or-map-layer-report-builder-and-ssrs.md).  
  
##  <a name="choose-map-visualization"></a><a name="Visualization"></a> Choisir la visualisation de la carte  
 Dans cette page, choisissez le type de carte ou de couche à ajouter à votre rapport. La première fois vous exécutez l'Assistant, vous ajoutez la carte et la première couche au rapport. Une carte peut contenir plusieurs couches. Chaque couche affiche un type spécifique de données spatiales : polygones, lignes ou points.  
  
 Le type de carte que vous choisissez dépend de l'objectif de la carte ainsi que des données disponibles.  
  
###  <a name="what-is-the-difference-among-a-basic-map-a-bubble-map-and-an-analytical-map"></a><a name="MapType"></a> Quelle est la différence entre une carte simple, une carte à bulles et une carte analytique ?  
 Une **carte simple** affiche uniquement des emplacements. Vous pouvez faire varier les couleurs des zones de la carte par ton, mais la couleur ne représente pas de valeurs de données analytiques.  
  
 Une **carte à bulles** transmet la valeur relative d'un agrégat unique de données analytiques en tant que taille de bulle, par exemple le chiffre de ventes des magasins. Vous pouvez créer des cartes à bulles pour des polygones ou des points. Pour les polygones, définissez les propriétés du point central du polygone ; pour les points, définissez les propriétés des marqueurs.  
  
 Une **carte analytique** transmet la valeur relative d'un ou plusieurs agrégats de données analytiques pour chaque élément cartographique. Par exemple, les chiffres de ventes des magasins comme taille de marqueur, la plage de bénéfices pour les catégories de produit comme couleur de marqueur et les meilleurs produits comme type de marqueur.  
  
 Pour plus d’informations, consultez [Planifier un rapport cartographique &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/plan-a-map-report-report-builder-and-ssrs.md).  
  
##  <a name="choose-the-analytical-dataset"></a><a name="AnalyticalData"></a> Choisir le dataset analytique  
 Dans cette page, spécifiez où obtenir les données analytiques qui seront affichées sur cette couche.  
  
 Pour afficher vos données de rapport ou toutes autres données analytiques sur l'arrière-plan de la carte, vous devez spécifier où sont les données et comment elles sont reliées aux données spatiales. Vos données peuvent provenir d'un dataset de rapport existant ou d'un nouveau dataset pour lequel vous générez une requête. Les données analytiques existantes peuvent être incluses dans le fichier de forme ESRI qui contient les données spatiales.  
  
###  <a name="what-is-the-difference-between-spatial-data-and-analytical-data"></a><a name="Diff"></a> Quelle est la différence entre les données spatiales et les données analytiques ?  
 Les données spatiales consistent en des jeux de coordonnées spécifiant des points, des lignes et des polygones. Les éléments cartographiques sont basés sur les données spatiales.  
  
 Les données analytiques sont des données numériques ou catégoriques que vous souhaitez utiliser pour modifier l'apparence de la carte. Elles peuvent provenir d'un dataset du rapport ou être incluses avec les données spatiales d'une carte de la bibliothèque de cartes ou d'un fichier de forme ESRI.  
  
##  <a name="specify-the-match-fields"></a><a name="SpecifyMatchFields"></a> Spécifier les champs de correspondance  
 Dans cette page, créez une relation entre les données spatiales et les données analytiques.  
  
###  <a name="what-are-match-fields"></a><a name="MatchFields"></a> Que sont les champs de correspondance ?  
 Les champs de correspondance permettent au processeur de rapports de créer une relation entre les données analytiques et les données spatiales. Les champs de correspondance spécifient des valeurs uniques dans les données analytiques. Par exemple, le nom du magasin peut ne pas être unique dans les données et vous pourriez donc spécifier à la fois la ville et le nom du magasin.  
  
##  <a name="choose-color-theme-and-data-visualization"></a><a name="ThemeandVisualization"></a> Choisir un thème de couleurs et une visualisation des données  
 Dans cette page, spécifiez comment visualiser vos données sur l'arrière-plan de la carte, spécifiez le thème de la carte, les champs à visualiser et les éléments à varier : couleur, taille et/ou type de marqueur.  
  
###  <a name="what-does-the-theme-do"></a><a name="Theme"></a> Quelle est la fonction du thème ?  
 Le thème que vous choisissez définit des valeurs par défaut pour la couleur, la bordure et la police. Vous pouvez modifier ces options après avoir terminé l'Assistant.  
  
###  <a name="what-are-the-legends-and-scales-in-map-preview-for"></a><a name="Legends"></a> À quoi servent les légendes et les échelles dans l'aperçu de la carte ?  
 Les légendes aident l'utilisateur à interpréter les données affichées sur une carte. Une carte fournit une plage de couleurs, une échelle des distances et une légende.  
  
-   **Plage de couleurs.** La plage de couleurs affiche une barre de couleur avec une échelle qui fournit des indications sur les intervalles de données déterminés par le processeur de rapports en fonction des règles que vous spécifiez pour la couche.  
  
-   **Échelle des distances.** L'échelle des distances fournit des indications sur les unités de distance utilisées dans la carte. Les unités de distance sont déterminées automatiquement en fonction de la projection cartographique et du niveau de zoom.  
  
-   **Légende.** La légende fournit des indications permettant d'interpréter les couleurs, les tailles et les types de marqueur sur une carte. Par défaut, toutes les règles pour toutes les couches affichent des intervalles de données dans la première légende. Vous pouvez personnaliser cette légende et ajouter des légendes après avoir ajouté la carte au rapport.  
  
###  <a name="what-are-rules"></a><a name="Rules"></a> Qu'est-ce que des règles ?  
 Les règles sont des calculs utilisés par le processeur de rapports pour diviser des données analytiques en plages. Vous pouvez spécifier des règles différentes pour chaque couche. Le type de règles vous pouvez spécifier dépend du type de données spatiales sur la couche :  
  
-   **Polygones.** Vous pouvez spécifier des règles de couleur.  
  
-   **Points centraux des polygones.** Vous pouvez spécifier des règles de couleur, de taille et de type de marqueur.  
  
-   **Lignes.** Vous pouvez spécifier des règles de couleur et de largeur.  
  
-   **Points.** Vous pouvez spécifier des règles de couleur, de taille et de type de marqueur.  
  
 Le processeur de rapports applique les règles que vous définissez et détermine automatiquement la liste d'éléments à afficher dans une légende. Par défaut, les résultats de toutes les règles pour toutes les couches sont affichés dans la première légende. Vous pouvez modifier ce paramètre après avoir terminé l'Assistant. Pour plus d’informations, consultez [Modifier l’affichage des polygones, des lignes et des points à l’aide de règles et de données analytiques &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/vary-polygon-line-and-point-display-by-rules-and-analytical-data.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Résoudre les problèmes liés aux rapports : rapports cartographiques &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/troubleshoot-reports-map-reports-report-builder-and-ssrs.md)   
 [Planifier un rapport cartographique &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/plan-a-map-report-report-builder-and-ssrs.md)   
 [Cartes &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/maps-report-builder-and-ssrs.md)  
  
