---
title: 'Rapports mobiles SQL Server : procédure pas à pas de bout en bout'
description: Découvrez comment créer des rapports mobiles dans l’Éditeur de rapports mobiles SQL Server, enregistrer des rapports sur le portail web Reporting Services et afficher des rapports dans les applications mobiles Power BI.
ms.date: 12/07/2018
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: mobile-reports
ms.custom: seodec18
ms.topic: conceptual
ms.assetid: e198575e-b154-4342-b944-2bf19ec49bfd
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: df09e4168790a4607de19348f50e76a4fbd0f383
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596541"
---
# <a name="sql-server-mobile-reports-end-to-end-walk-through"></a>Rapports mobiles SQL Server : procédure pas à pas de bout en bout
Étapes permettant de créer des rapports mobiles pour n’importe quelle taille d’écran avec [!INCLUDE[PRODUCT_NAME](../../includes/ss-mobilereptpub-long.md)] sur le portail web [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)] et de les afficher dans les applications mobiles Power BI.

Créez des rapports mobiles sur une aire de conception avec des lignes et des colonnes de grille réglables et des éléments de rapport mobile flexibles. Connectez-vous à diverses sources de données locales ou chargez des classeurs Excel pour créer des rapports mobiles. Enregistrez ensuite vos rapports sur un portail web [!INCLUDE[PRODUCT_NAME](../../includes/ssrsnoversion.md)] et affichez-les dans un navigateur ou dans les applications mobiles Power BI.  
  
Cet article vous guide tout au long des procédures suivantes :   
  
- Créer un jeu de données et une source de données partagés sur le portail web [!INCLUDE[PRODUCT_NAME](../../includes/ssrsnoversion.md)] , avec la base de données AdventureWorks comme exemple de source de données.  
- Créer un rapport mobile Reporting Services dans l’ [!INCLUDE[PRODUCT_NAME](../../includes/ss-mobilereptpub-short.md)].  
- Publier le rapport mobile sur le portail web [!INCLUDE[PRODUCT_NAME](../../includes/ssrsnoversion.md)] .  
- Afficher le rapport mobile dans l’application mobile Power BI.  
  
## <a name="before-we-start"></a>Avant de commencer  
Pour suivre la procédure, vous avez besoin de ces produits :  
  
* Pour créer des sources de données et des indicateurs de performance clés, et pour publier des jeux de données et des rapports mobiles, vous devez accéder à un [serveur de rapports Reporting Services en mode natif](../install-windows/install-reporting-services-native-mode-report-server.md).  
* Pour créer des jeux de données partagés, [installez le Générateur de rapports](../install-windows/install-report-builder.md).  
* Pour créer des rapports mobiles, [installez l’Éditeur de rapports mobiles SQL Server](../reporting-services-features-and-tasks-ssrs.md).  
* [Exemples de bases de données AdventureWorks](https://github.com/Microsoft/sql-server-samples/releases).  
*  OU : exemple de base de données World Wide Importers, disponible à partir de la page [Exemples Microsoft SQL Server](../../samples/sql-samples-where-are.md).
* Pour afficher le résultat 
  *   [Inscrivez-vous au service Power BI](https://go.microsoft.com/fwlink/?LinkID=513879) et
  *  [Téléchargez l’application mobile Power BI](/power-bi/consumer/mobile/mobile-apps-for-mobile-devices) sur votre appareil mobile : iOS, téléphone Android ou appareil Windows 10.  

  
## <a name="create-a-shared-data-source"></a>Créer une source de données partagée  
  
Vous pouvez créer une source de données partagée pour vos rapports mobiles à partir de n’importe quelle source de données prise en charge par Reporting Services. Consultez la [liste des sources de données prises en charge](../report-data/data-sources-supported-by-reporting-services-ssrs.md).  
  
1. À partir de votre portail web [!INCLUDE[PRODUCT_NAME](../../includes/ssrsnoversion.md)] , cliquez sur **Nouveau** > **Source de données**.  
  
   ![PBI_SSMRP_NewMenu](../../reporting-services/mobile-reports/media/pbi-ssmrp-newmenu.png)  
3. Entrez vos informations de source de données > **OK**.  
  
    Par défaut, les sources de données ne sont pas affichées sur le portail.    
   
5. Pour afficher les sources de données, cliquez sur **Affichage** > **Source de données**.  
  
   ![PBI_SSMRP_DisplayDataSources](../../reporting-services/mobile-reports/media/pbi-ssmrp-displaydatasources.png)  
   
6. La source de données est maintenant visible sur le portail [!INCLUDE[PRODUCT_NAME](../../includes/ssrsnoversion.md)] .  
  
   ![PBI_SSMRP_PortlDataSource](../../reporting-services/mobile-reports/media/pbi-ssmrp-portldatasource.png)  
  
En savoir plus sur [les sources de données partagées dans Reporting Services](../report-data/create-modify-and-delete-shared-data-sources-ssrs.md).  
   
## <a name=""></a><a name="shared-dataset">Créer un jeu de données partagé</a>  
  
Utilisez un outil client [!INCLUDE[PRODUCT_NAME](../../includes/ssrsnoversion.md)] existant, tel que le Concepteur de rapports dans [!INCLUDE[ssBIDevStudioFull_md](../../includes/ssbidevstudiofull-md.md)], pour créer le jeu de données partagé.  Cette procédure pas à pas utilise [!INCLUDE[PRODUCT_NAME](../../includes/ssrbnoversion.md)]. [Installez le Générateur de rapports](../install-windows/install-report-builder.md)ou lancez-le à partir de votre portail web. Vous allez créer trois jeux de données : un pour la valeur de l’indicateur de performance clé, un pour la tendance de l’indicateur de performance clé et un autre avec davantage de champs pour le rapport mobile Reporting Services.     
  
1. À partir de votre portail web [!INCLUDE[PRODUCT_NAME](../../includes/ssrsnoversion.md)] , cliquez sur **Nouveau** > **Rapport paginé** pour démarrer [!INCLUDE[PRODUCT_NAME](../../includes/ssrbnoversion.md)].  
  
   ![PBI_SSMRP_NewMenu](../../reporting-services/mobile-reports/media/pbi-ssmrp-newmenu.png)   
2. Cliquez sur **Nouveau jeu de données**.  
  
   ![PBI_SSMRP_RBNewDataset](../../reporting-services/mobile-reports/media/pbi-ssmrp-rbnewdataset.png)  
   
3. Cliquez sur **Sélectionner d’autres sources de données**.  
   
4. Dans le champ Nom, tapez le nom du serveur où vous avez enregistré votre source de données au format suivant :   
   
   Nom : https://*localhost*/ReportServer  
   Éléments de type : Sources de données (*.rsds)  
   
5. Cliquez sur **Ouvrir** et accédez à la source de données que vous avez créée sur ce serveur.  
   
6. Sélectionnez votre source de données et cliquez à nouveau sur **Ouvrir** .    
  
7. Concevez votre jeu de données dans [!INCLUDE[PRODUCT_NAME](../../includes/ssrbnoversion.md)].  
  
   ![PBI_SSMRP_RB_QueryDesignr600](../../reporting-services/mobile-reports/media/pbi-ssmrp-rb-querydesignr600.png)  
   
8. Une fois terminé, enregistrez le jeu de données sur le serveur de rapports [!INCLUDE[PRODUCT_NAME](../../includes/ssrs.md)] .    
   
Vous pouvez maintenant utiliser le jeu de données comme base pour vos indicateurs de performance clés et vos rapports mobiles.  Vous pouvez créer plusieurs jeux de données sur la même source de données. Vous pouvez aussi créer plusieurs indicateurs de performance clés et rapports mobiles à partir de ces jeux de données partagés.   
  
## <a name=""></a><a name="create-KPI">Créer un indicateur de performance clé</a>  
Vous créez les indicateurs de performance clés directement sur le portail web [!INCLUDE[PRODUCT_NAME](../../includes/ssrsnoversion.md)] .    
  
1. En haut à droite du portail web, cliquez sur **Nouveau** > **Nouveau KPI**.   
  
   ![PBI_SSMRP_NewMenu](../../reporting-services/mobile-reports/media/pbi-ssmrp-newmenu.png)  
      
   Dans l’écran de création d’indicateur de performance clé, vous pouvez entrer manuellement des valeurs ou utiliser un jeu de données partagé.    
2. Dans **Valeur** , remplacez **Définir manuellement** par **Champ de jeu de données**.  
   
   ![PBI_SSMRP_KPI_DatasetField](../../reporting-services/mobile-reports/media/pbi-ssmrp-kpi-datasetfield.png)  
   
3. Cliquez sur le bouton de sélection ( **...** ) dans la zone **Sélectionner un champ de jeu de données** et sélectionnez un jeu de données de l’étape précédente.  
   
   ![PBI_SSMRP_KPIPickDataset](../../reporting-services/mobile-reports/media/pbi-ssmrp-kpipickdataset.png)  
   
4. Choisissez le champ dans le jeu de données.    
   
   ![PBI_SSMRP_KPIPickField](../../reporting-services/mobile-reports/media/pbi-ssmrp-kpipickfield.png)  
     
5. Choisissez l’agrégation souhaitée. Les indicateurs de performance clés ne pouvant afficher qu’un seul nombre, le champ est agrégé pour afficher ce nombre.

   ![Capture d’écran de la section Choisir le champ dans AWSalesYTD, montrant la section Agrégation moyenne.](../../reporting-services/mobile-reports/media/reporting-services-kpi-pick-aggregation.png)

6. Cliquez sur **OK**.

7. Dans la zone **Jeu de tendances** , cliquez sur **Tendance de jeu de données**.  
  
6. Dans la zone **Sélectionner une tendance de jeu de données** , cliquez sur le bouton de sélection ( **...** ).  
   
7. Sélectionnez un champ et cliquez sur **OK**.  

   ![PBI_SSMRP_KPIPickTrend](../../reporting-services/mobile-reports/media/pbi-ssmrp-kpipicktrend.png)  
  
8. Donnez un nom à votre indicateur de performance clé et choisissez un type de visualisation, puis cliquez sur **Créer**.   
  
   L’indicateur de performance clé s’affiche sur le portail web [!INCLUDE[PRODUCT_NAME](../../includes/ssrsnoversion.md)] .  
   
    ![PBI_SSMRP_NewKPI](../../reporting-services/mobile-reports/media/pbi-ssmrp-newkpi.png)  
    
## <a name=""></a><a name="create-mobile-report">Créer un rapport mobile Reporting Services</a>  
   
Pour créer un rapport mobile Reporting Services, [installez l’Éditeur de rapports mobiles SQL Server](../reporting-services-features-and-tasks-ssrs.md)ou lancez-le à partir du portail web [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)] . 

Quand vous ouvrez [!INCLUDE[PRODUCT_NAME](../../includes/ss-mobilereptpub-short.md)]pour la première fois, une zone vide s’affiche. Vous pouvez y créer votre rapport mobile. Vous pouvez commencer par créer les visuels ou commencer par vos données. Si vous créez d’abord les visuels, [!INCLUDE[PRODUCT_NAME](../../includes/ss-mobilereptpub-short.md)] génère automatiquement des données simulées liées au rapport, qui changent de manière dynamique à mesure que vous modifiez vos sélections de visuels. Essayez vous-même.   
  
## <a name="start-with-the-visuals"></a>Commencer avec les visuels  
  
1. À partir de votre portail web [!INCLUDE[PRODUCT_NAME](../../includes/ssrsnoversion.md)] , cliquez sur **Nouveau** > **Rapport mobile** pour démarrer [!INCLUDE[PRODUCT_NAME](../../includes/ss-mobilereptpub-short.md)].  
  
   ![PBI_SSMRP_NewMenu](../../reporting-services/mobile-reports/media/pbi-ssmrp-newmenu.png)

   [!INCLUDE[PRODUCT_NAME](../../includes/ss-mobilereptpub-short.md)] ouvre la grille de disposition principale.  
  
2. Sous l’onglet **Disposition** , faites défiler jusqu’à la section Graphiques.  
  
   ![PBI_SSMRP_LayoutTabCharts2](../../reporting-services/mobile-reports/media/pbi-ssmrp-layouttabcharts2.png)  
  
2. Faites glisser **l’Arborescence** vers la grille, puis faites glisser l’angle inférieur droit pour qu’elle mesure trois colonnes sur trois lignes.  
  
   ![PBI_SSMRP_TreeMap](../../reporting-services/mobile-reports/media/pbi-ssmrp-treemap.png)  
  
3. Vous pouvez voir ses propriétés de visuel en bas. Vous devrez peut-être faire défiler latéralement pour les voir toutes.   
  
   ![PBI_SSMRP_TreeMapVisProps](../../reporting-services/mobile-reports/media/pbi-ssmrp-treemapvisprops.png)  
  
4. Le visuel d’arborescence étant sélectionné, cliquez sur l’onglet **Données** en haut à gauche.   
  
   Les champs et valeurs simulés générés par [!INCLUDE[PRODUCT_NAME](../../includes/ss-mobilereptpub-short.md)] sont maintenant visibles. Vous pouvez voir ce que représentent la taille et la couleur dans l’arborescence.  
  
   ![PBI_SSMRP_TreeMapDataProps](../../reporting-services/mobile-reports/media/pbi-ssmrp-treemapdataprops.png)  
  
6. Cliquez sur l’onglet **Disposition**.  
  
7. Cliquez sur l’onglet Options ![PBI_SSMRP_Cog](../../reporting-services/mobile-reports/media/pbi-ssmrp-cog.png) en haut à droite de l’arborescence pour afficher le menu qu’il contient.   
  
   ![PBI_SSMRP_OptionsWheel](../../reporting-services/mobile-reports/media/pbi-ssmrp-optionswheel.png)  
  
8. Cliquez sur la flèche au centre de la roulette pour la fermer.  
  
## <a name="add-your-own-data"></a>Ajouter vos propres données  
  
1. Basculez vers l’onglet **Données** .    
   
2. Pour ajouter vos propres données, cliquez sur **Ajouter des données** en haut à droite, puis accédez à vos données.    
  
3. Vous pouvez utiliser les données d’un classeur Excel local, mais dans ce cas c’est à partir du jeu de données partagé sur votre portail web [!INCLUDE[PRODUCT_NAME](../../includes/ssrsnoversion.md)] . Un message « Serveur ajouté » s’affiche.  
  
4. Sélectionnez le serveur, puis sélectionnez le jeu de données que vous avez créé.  
   
3. Sous l’onglet **Données** , dans le volet **Propriétés des données** , remplacez **La taille représente**, **La couleur représente** et d’autres propriétés par des champs de vos propres données. 
   
   *  Les champs **La taille représente**, **La couleur représente** et **Valeur centrale personnalisée** doivent contenir des valeurs numériques. 
   *  **Regrouper par** étant une catégorie, c’est un champ de texte.
   
   ![Capture d’écran de la section Propriétés des données.](../../reporting-services/mobile-reports/media/ssrs-mobile-report-data-properties.png)
   
6. Sélectionnez **Aperçu** pour voir l’arborescence mise à jour avec vos données.  

## <a name="add-a-gauge-to-your-mobile-report"></a>Ajouter une jauge à votre rapport mobile

Nous allons ajouter une jauge pour comparer les ventes de cette année à celles de l’année dernière, à l’aide du même jeu de données.

1. Sous l’onglet Disposition, faites glisser un demi-anneau vers la zone de dessin. Placez-le sous l’arborescence et faites glisser le coin inférieur droit pour qu’il mesure trois cases de large sur deux cases de haut. 

2. Là encore, nous commençons avec des données simulées. 

   Notez que dans **Propriétés visuelles**, par défaut **Les valeurs élevées sont préférables** et **Étiquette de l’écart** est un **Pourcentage de la cible**. Vous pouvez modifier la valeur **Arrêts de plage** par défaut, mais pour l’instant elle convient très bien.

   ![Capture d’écran de la section Plages définies dans les propriétés visuelles en anneau du rapport mobile.](../../reporting-services/mobile-reports/media/ssrs-mobile-report-donut-visual-properties.png)
   
3. Sous l’onglet **Données** , sélectionnez la table contenant vos données et sélectionnez le champ **Valeur principale** et celui auquel vous souhaitez le comparer dans **Valeur de comparaison**.

4. Vous pouvez choisir différentes agrégations pour parvenir à un nombre pour **Valeur principale** et un autre pour **Valeur de comparaison**. Par défaut, il s’agit d’une somme.

   ![Capture d’écran des options ou de la valeur de comparaison.](../../reporting-services/mobile-reports/media/ssrs-mobile-report-donut-sum.png)

5. Sélectionnez **Aperçu** pour obtenir un aperçu. 

   ![Capture d’écran de l’aperçu en anneau du rapport mobile.](../../reporting-services/mobile-reports/media/ssrs-mobile-report-donut-preview.png)

## <a name="add-a-selection-list-as-a-filter"></a>Ajouter une liste de sélection comme filtre

Les listes de sélection jouent le rôle de filtres dans Power BI et Excel. Nous pouvons en ajouter une pour filtrer les autres visuels dans le rapport mobile.

1. Sous l’onglet **Disposition** , faites glisser une liste de sélection à droite de l’arborescence et faites glisser le coin inférieur droit pour qu’elle fasse deux cases de large et toute la hauteur de la zone de dessin (cinq cases). 

   ![Capture d’écran de la liste de sélection du rapport mobile.](../../reporting-services/mobile-reports/media/ssrs-mobile-report-selection-list.png)

2. Sous l’onglet **Données** , **Propriétés des données**, affectez à **Clés** et **Étiquettes** un champ dans vos données que vous souhaitez filtrer.

   ![Capture d’écran de la section Propriétés de données de la liste de sélection du rapport mobile.](../../reporting-services/mobile-reports/media/ssrs-mobile-report-selection-list-data-properties.png)
   
## <a name="create-a-mobile-report-for-phones"></a>Créer un rapport mobile pour les téléphones  
  
Maintenant que vous avez créé des visuels sur la présentation principale, vous pouvez créer un rapport mobile avec une présentation spécialement optimisée pour vos utilisateurs de téléphone.    
  
1. Dans le coin supérieur droit, cliquez sur l’icône de zone de dessin > **Téléphone**.  
  
2. Sous l’onglet Disposition sous **Instances de contrôles** sont affichés les deux graphiques que vous avez créés.   
  
3. Faites glisser l’arborescence sur la zone de dessin de téléphone, et faites en sorte qu’elle mesure quatre colonnes sur trois lignes.  
  

## <a name="save-your-mobile-report"></a>Enregistrer votre rapport mobile  
Vous pouvez enregistrer votre rapport localement ou sur un portail web [!INCLUDE[PRODUCT_NAME](../../includes/ssrsnoversion.md)] . Si vous l’enregistrez localement, [!INCLUDE[PRODUCT_NAME](../../includes/ss-mobilereptpub-short.md)] l’enregistre avec des données en mémoire cache. Vous pouvez donc l’ouvrir et continuer à travailler dessus. En revanche, vous ne pouvez pas l’afficher sur un appareil mobile.   
  
1. Cliquez sur l’icône d’enregistrement en haut à gauche.   
   
2. Pour le partager avec d’autres personnes et l’afficher sur un appareil mobile, cliquez sur **Enregistrer sur le serveur**.  
  
3. Sur le serveur, accédez au dossier où vous souhaitez enregistrer votre rapport mobile.  
  
4. Cliquez sur **Choisir un dossier** > **Enregistrer**.  
  
   Un message confirme que le rapport a été enregistré.  
    
   Après l’enregistrement, vous pouvez revenir au portail où est visible la miniature de votre rapport mobile.   
    
5. Appuyez sur la miniature pour afficher le rapport sur le portail web.  
  
## <a name="view-your-report-on-the-web-portal"></a>Afficher votre rapport sur le portail web

  
## <a name="view-your-report-on-a-mobile-device"></a>Afficher votre rapport sur un appareil mobile   
  
Pour afficher votre rapport [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)] , vous devez d’abord :

*  [Vous inscrire au service Power BI](https://go.microsoft.com/fwlink/?LinkID=513879), si vous n’avez pas encore de compte.
*  [Télécharger l’application mobile Power BI](https://powerbi.microsoft.com/documentation/powerbi-power-bi-apps-for-mobile-devices/) sur votre appareil mobile.  

### <a name="view-your-mobile-report"></a>Afficher votre rapport mobile
  
1.  Ouvrez et connectez-vous à l’application Power BI sur votre appareil mobile.  
    
2.  Pour afficher vos rapports mobiles Reporting Services et vos indicateurs de performance clés, appuyez sur **Reporting Services**.  
![PBI_iPad_GetStartedSm](../../reporting-services/mobile-reports/media/pbi-ipad-getstartedsm.png)  
  
3. Appuyez sur l’icône d’options ![PBI_iPad_OptionsIcon](../../reporting-services/mobile-reports/media/pbi-ipad-optionsicon.png) en haut à gauche, puis appuyez sur **Se connecter au serveur**.  
  
   ![PBI_iPad_SSMRP_ConnectCrop](../../reporting-services/mobile-reports/media/pbi-ipad-ssmrp-connectcrop.png)  
  
4. Nommez le serveur et renseignez l’adresse du serveur ainsi que votre adresse e-mail et votre mot de passe, au format suivant :  
  
   ![PBI_iPad_SSMRP_ConnectContoso](../../reporting-services/mobile-reports/media/pbi-ipad-ssmrp-connectcontoso.png)   
  
5.  Le serveur est maintenant visible dans la barre de navigation gauche.  
  
    ![PBI_iPad_SSMRP_LeftNavBiggr](../../reporting-services/mobile-reports/media/pbi-ipad-ssmrp-leftnavbiggr.png)  
      
> [!TIP]
> Appuyez sur l’icône d’options ![PBI_iPad_OptionsIcon](../../reporting-services/mobile-reports/media/pbi-ipad-optionsicon.png) à tout moment pour basculer entre vos rapports mobiles Reporting Services sur le portail web Reporting Services et vos tableaux de bord dans le service Power BI.  

  
## <a name="view-kpis-and-mobile-reports-in-the-power-bi-app"></a>Afficher des indicateurs de performance clés et des rapports mobiles dans l’application Power BI  
  
Appuyez sur l’onglet **Indicateurs de performance clés** ou **Rapports mobiles** .   
  
![PBI_iPad_SSMRP_Portal](../../reporting-services/mobile-reports/media/pbi-ipad-ssmrp-portal.png)  
  
- Appuyez sur un indicateur de performance clé pour le voir en mode focus.  
  
    ![PBI_iPad_SSMRP_Tile](../../reporting-services/mobile-reports/media/pbi-ipad-ssmrp-tile.png)  
  
- Appuyez sur un rapport mobile pour l’ouvrir et interagir avec lui dans l’application Power BI.  
  
Les indicateurs de performance clés et les rapports mobiles sont affichés dans les mêmes dossiers que ceux où ils sont stockés sur le portail web Reporting Services.   
  
## <a name="see-also"></a>Voir aussi  
 
-  Afficher des [rapports mobiles et des indicateurs de performance clés du serveur de rapports local dans l’application mobile Power BI](/power-bi/consumer/mobile/mobile-app-ssrs-kpis-mobile-on-premises-reports) pour les appareils iOS et Android
-  Afficher des [rapports mobiles et des indicateurs de performance clés du serveur de rapports local dans l’application mobile Power BI pour les appareils Windows 10](https://powerbi.microsoft.com/documentation/powerbi-mobile-win10-kpis-mobile-reports/)    
  
