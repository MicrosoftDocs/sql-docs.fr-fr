---
title: 'Didacticiel : introduction aux expressions | Microsoft Docs'
description: Découvrez comment utiliser des expressions avec des fonctions et des opérateurs courants pour créer des rapports paginés Reporting Services puissants et flexibles.
ms.date: 09/16/2016
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: reporting-services
ms.topic: conceptual
ms.assetid: 2d05ef4c-5f91-48b2-8795-f0a201a0b3cc
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 938ac8fc2e006bc74aab59955374f1b012b66b68
ms.sourcegitcommit: fc24f7ecc155d97e789676fffe55e45840fcb088
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/21/2021
ms.locfileid: "98620276"
---
# <a name="tutorial-introducing-expressions"></a>Tutoriel : Introduction aux expressions
Dans ce didacticiel [!INCLUDE[ssRBnoversion_md](../includes/ssrbnoversion.md)] , vous utilisez des expressions avec des opérateurs et des fonctions communes pour créer des rapports paginés [!INCLUDE[ssRSnoversion_md](../includes/ssrsnoversion-md.md)] puissants et flexibles. 

Vous allez écrire des expressions qui concatènent des valeurs de noms, qui recherchent des valeurs dans un autre dataset, qui affichent différentes couleurs en fonction des valeurs de champ, etc.  
  
Le rapport est un rapport à bandes avec des alternances de lignes en couleur et de lignes blanches. Le rapport inclut un paramètre de sélection de couleur pour les lignes non blanches.  
  
Cette illustration montre un rapport similaire à celui que vous allez créer.  
  
![Capture d’écran d’un rapport du générateur de rapports qui utilise des expressions.](../reporting-services/media/report-builder-expression-tutorial-in-browser.png) 
  
Durée estimée pour effectuer ce tutoriel : 30 minutes.  
  
## <a name="requirements"></a>Spécifications  
Pour plus d’informations sur les spécifications, consultez [Éléments requis pour les didacticiels &#40;Générateur de rapports&#41;](../reporting-services/prerequisites-for-tutorials-report-builder.md).  
  
## <a name="1-create-a-table-report-and-dataset-from-the-table-or-matrix-wizard"></a><a name="Setup"></a>1. Créer un rapport de tableau et un dataset à partir de l'Assistant Tableau ou matrice  
Dans cette section, vous allez créer un rapport de tableau, une source de données et un dataset. Lorsque vous créez le tableau, n'incluez que quelques champs. Après avoir terminé l'Assistant, vous ajouterez manuellement des colonnes. L’Assistant facilite la disposition du tableau.  
  
> [!NOTE]  
> Dans ce didacticiel, la requête contient les valeurs des données : elle n’a donc pas besoin d’une source de données externe. Cela rend la requête assez longue. Dans un environnement métier, une requête ne contient pas les données. Ceci est nécessaire à des fins de formation uniquement.  
  
### <a name="to-create-a-table-report"></a>Pour créer un rapport tabulaire  
  
1.  [Démarrez le Générateur de rapports](../reporting-services/report-builder/start-report-builder.md) à partir de votre ordinateur, du portail web de [!INCLUDE[ssRSnoversion_md](../includes/ssrsnoversion-md.md)] ou du mode intégré SharePoint.  
  
    La boîte de dialogue **Nouveau rapport ou dataset** s’ouvre.  
  
    Si vous ne voyez pas la boîte de dialogue **Nouveau rapport ou dataset**, dans le menu **Fichier**, choisissez **Nouveau**.  
  
2.  Dans le volet gauche, assurez-vous que **Nouveau rapport** est sélectionné.  
  
3.  Dans le volet droit, cliquez sur **Assistant Tableau ou matrice**.  
  
4.  Dans la page **Choisir un dataset** , cliquez sur **Créer un dataset** > **Suivant**.  
  
6.  Dans la page **Choisir une connexion à une source de données** , sélectionnez une source de données de type **SQL Server**. Sélectionnez une source de données dans la liste ou naviguez jusqu'au serveur de rapports pour en sélectionner une.  

    > [!NOTE]  
    > La source de données que vous choisissez n’a pas d’importance tant que vous disposez des autorisations appropriées. Vous n'allez pas récupérer de données à partir de la source de données. Pour plus d’informations, consultez [Autres manières d’obtenir une connexion de données &#40;Générateur de rapports&#41;](../reporting-services/alternative-ways-to-get-a-data-connection-report-builder.md).  
  
7.  Cliquez sur **Suivant**.  
  
8.  Dans la page **Créer une requête** , cliquez sur **Modifier en tant que texte**.  
  
9. Collez la requête suivante dans le volet de requête :  
  
    ```  
    SELECT 'Lauren' AS FirstName,'Johnson' AS LastName, 'American Samoa' AS StateProvince, 1 AS CountryRegionID,'Female' AS Gender, CAST(9996.60 AS money) AS YTDPurchase, CAST('2015-6-10' AS date) AS LastPurchase  
    UNION SELECT'Warren' AS FirstName, 'Pal' AS LastName, 'New South Wales' AS StateProvince, 2 AS CountryRegionID, 'Male' AS Gender, CAST(5747.25 AS money) AS YTDPurchase, CAST('2015-7-3' AS date) AS LastPurchase  
    UNION SELECT 'Fernando' AS FirstName, 'Ross' AS LastName, 'Alberta' AS StateProvince, 3 AS CountryRegionID, 'Male' AS Gender, CAST(9248.15 AS money) AS YTDPurchase, CAST('2015-10-17' AS date) AS LastPurchase  
    UNION SELECT 'Rob' AS FirstName, 'Caron' AS LastName, 'Northwest Territories' AS StateProvince, 3 AS CountryRegionID, 'Male' AS Gender, CAST(742.50 AS money) AS YTDPurchase, CAST('2015-4-29' AS date) AS LastPurchase  
    UNION SELECT 'James' AS FirstName, 'Bailey' AS LastName, 'British Columbia' AS StateProvince, 3 AS CountryRegionID, 'Male' AS Gender, CAST(1147.50 AS money) AS YTDPurchase, CAST('2015-6-15' AS date) AS LastPurchase  
    UNION SELECT  'Bridget' AS FirstName, 'She' AS LastName, 'Hamburg' AS StateProvince, 4 AS CountryRegionID, 'Female' AS Gender, CAST(7497.30 AS money) AS YTDPurchase, CAST('2015-5-10' AS date) AS LastPurchase  
    UNION SELECT 'Alexander' AS FirstName, 'Martin' AS LastName, 'Saxony' AS StateProvince, 4 AS CountryRegionID, 'Male' AS Gender, CAST(2997.60 AS money) AS YTDPurchase, CAST('2015-11-19' AS date) AS LastPurchase  
    UNION SELECT 'Yolanda' AS FirstName, 'Sharma' AS LastName ,'Micronesia' AS StateProvince, 5 AS CountryRegionID, 'Female' AS Gender, CAST(3247.95 AS money) AS YTDPurchase, CAST('2015-8-23' AS date) AS LastPurchase  
    UNION SELECT 'Marc' AS FirstName, 'Zimmerman' AS LastName, 'Moselle' AS StateProvince, 6 AS CountryRegionID, 'Male' AS Gender, CAST(1200.00 AS money) AS YTDPurchase, CAST('2015-11-16' AS date) AS LastPurchase  
    UNION SELECT 'Katherine' AS FirstName, 'Abel' AS LastName, 'Moselle' AS StateProvince, 6 AS CountryRegionID, 'Female' AS Gender, CAST(2025.00 AS money) AS YTDPurchase, CAST('2015-12-1' AS date) AS LastPurchase  
    UNION SELECT 'Nicolas' as FirstName, 'Anand' AS LastName, 'Seine (Paris)' AS StateProvince, 6 AS CountryRegionID, 'Male' AS Gender, CAST(1425.00 AS money) AS YTDPurchase, CAST('2015-12-11' AS date) AS LastPurchase  
    UNION SELECT 'James' AS FirstName, 'Peters' AS LastName, 'England' AS StateProvince, 12 AS CountryRegionID, 'Male' AS Gender, CAST(887.50 AS money) AS YTDPurchase, CAST('2015-8-15' AS date) AS LastPurchase  
    UNION SELECT 'Alison' AS FirstName, 'Nath' AS LastName, 'Alaska' AS StateProvince, 7 AS CountryRegionID, 'Female' AS Gender, CAST(607.50 AS money) AS YTDPurchase, CAST('2015-10-13' AS date) AS LastPurchase  
    UNION SELECT 'Grace' AS FirstName, 'Patterson' AS LastName, 'Kansas' AS StateProvince, 7 AS CountryRegionID, 'Female' AS Gender, CAST(1215.00 AS money) AS YTDPurchase, CAST('2015-10-18' AS date) AS LastPurchase  
    UNION SELECT 'Bobby' AS FirstName, 'Sanchez' AS LastName, 'North Dakota' AS StateProvince, 7 AS CountryRegionID, 'Female' AS Gender, CAST(6191.00 AS money) AS YTDPurchase, CAST('2015-9-17' AS date) AS LastPurchase  
    UNION SELECT 'Charles' AS FirstName, 'Reed' AS LastName, 'Nebraska' AS StateProvince, 7 AS CountryRegionID, 'Male' AS Gender, CAST(8772.00 AS money) AS YTDPurchase, CAST('2015-8-27' AS date) AS LastPurchase  
    UNION SELECT 'Orlando' AS FirstName, 'Romeo' AS LastName, 'Texas' AS StateProvince, 7 AS CountryRegionID, 'Male' AS Gender, CAST(8578.00 AS money) AS YTDPurchase, CAST('2015-7-29' AS date) AS LastPurchase  
    UNION SELECT 'Cynthia' AS FirstName, 'Randall' AS LastName, 'Utah' AS StateProvince, 7 AS CountryRegionID, 'Female' AS Gender, CAST(7218.10 AS money) AS YTDPurchase, CAST('2015-1-11' AS date) AS LastPurchase  
    UNION SELECT 'Rebecca' AS FirstName, 'Roberts' AS LastName, 'Washington' AS StateProvince, 7 AS CountryRegionID, 'Female' AS Gender, CAST(8357.80 AS money) AS YTDPurchase, CAST('2015-10-28' AS date) AS LastPurchase  
    UNION SELECT 'Cristian' AS FirstName, 'Petulescu' AS LastName, 'Wisconsin' AS StateProvince, 7 AS CountryRegionID, 'Male' AS Gender, CAST(3470.00 AS money) AS YTDPurchase, CAST('2015-11-30' AS date) AS LastPurchase  
    UNION SELECT 'Cynthia' AS FirstName, 'Randall' AS LastName, 'Utah' AS StateProvince, 7 AS CountryRegionID, 'Female' AS Gender, CAST(7218.10 AS money) AS YTDPurchase, CAST('2015-1-11' AS date) AS LastPurchase  
    UNION SELECT 'Rebecca' AS FirstName, 'Roberts' AS LastName, 'Washington' AS StateProvince, 7 AS CountryRegionID, 'Female' AS Gender, CAST(8357.80 AS money) AS YTDPurchase, CAST('2015-10-28' AS date) AS LastPurchase  
    UNION SELECT 'Cristian' AS FirstName, 'Petulescu' AS LastName, 'Wisconsin' AS StateProvince, 7 AS CountryRegionID, 'Male' AS Gender, CAST(3470.00 AS money) AS YTDPurchase, CAST('2015-11-30' AS date) AS LastPurchase  
    ```  

  
10. Dans la barre d’outils du Concepteur de requêtes, cliquez sur **Exécuter** ( **!** ). Le jeu de résultats affiche 23 lignes de données avec les colonnes suivantes : FirstName, LastName, StateProvince, CountryRegionID, Gender, YTDPurchase et LastPurchase.  

    ![Capture d’écran de l’étape Créer une requête de l’Assistant Nouveau tableau ou nouvelle matrice.](../reporting-services/media/report-builder-expression-tutorial-query-as-text.png)
  
11. Cliquez sur **Suivant**.  
  
12. Dans la page **Organiser les champs** , faites glisser les champs suivants, dans l’ordre spécifié, de la liste **Champs disponibles** vers la liste **Valeurs** .  
  
    -   StateProvince   
    -   CountryRegionID  
    -   LastPurchase  
    -   YTDPurchase  
  
    Étant donné que les champs CountryRegionID et YTDPurchase contiennent des données numériques, l’agrégat SUM est appliqué par défaut. Cependant, vous ne voulez pas que ces données soient des sommes.  
   
13. Dans la liste **Valeurs** , cliquez avec le bouton droit sur **CountryRegionID** , puis décochez la case **Sum** .  
  
    L'agrégat Sum n'est plus appliqué à CountryRegionID.  
  
14. Dans la liste **Valeurs** , cliquez avec le bouton droit sur **YTDPurchase** et cliquez sur l’option **Sum** .  
  
    L'agrégat Sum n'est plus appliqué à YTDPurchase.  
    
    ![Capture d’écran de la liste Valeurs montrant l’option Sum prête à être effacée.](../reporting-services/media/report-builder-expression-not-sum.png)
  
15. Cliquez sur **Suivant**.  
  
16. Dans la page **Choisir la disposition** , conservez tous les paramètres par défaut, puis cliquez sur **Suivant**.  

    ![Capture d’écran de l’étape Choisir la disposition de l’Assistant Nouveau tableau ou nouvelle matrice.](../reporting-services/media/report-builder-expression-tutorial-choose-layout.png)
  
17. Cliquez sur **Terminer**.  
  
## <a name="2-update-default-names-of-the-data-source-and-dataset"></a><a name="UpdateNames"></a>2. Mettre à jour les noms par défaut de la source de données et du dataset  
  
### <a name="to-update-the-default-name-of-the-data-source"></a>Pour mettre à jour le nom par défaut de la source de données  
  
1.  Dans le volet Données du rapport, développez le dossier **Sources de données** .  
  
2.  Cliquez avec le bouton droit sur **DataSource1** et cliquez sur **Propriétés de la source de données**.  
  
3.  Dans la zone **Nom** , tapez **ExpressionsDataSource**.  
  
4.  [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
### <a name="to-update-the-default-name-of-the-dataset"></a>Pour mettre à jour le nom par défaut du dataset  
  
1.  Dans le volet Données du rapport, développez le dossier **Datasets** .  
  
2.  Cliquez avec le bouton de droite sur **Jeu de données1** et cliquez sur **Propriétés du jeu de données**.  

    ![Capture d’écran montrant comment accéder à Propriétés du dataset dans le générateur de rapports.](../reporting-services/media/report-builder-expression-tutorial-rename-dataset.png)
  
3.  Dans la zone **Nom** , tapez **Expressions**.  
  
4.  [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
## <a name="3-display-first-initial-and-last-name"></a><a name="Concatenate"></a>3. Afficher la première initiale et le nom de famille  
Dans cette section, vous allez utiliser la fonction **Left** et l’opérateur **Concaténer** (**&**) dans une expression dont la valeur est un nom qui comprend une initiale et un nom. Vous pouvez générer l’expression pas à pas ou avancer dans la procédure et copier/coller l’expression à partir du didacticiel dans la boîte de dialogue **Expression** .   
  
1.  Cliquez avec le bouton droit sur la colonne **StateProvince** , pointez sur **Insérer une colonne** et cliquez sur **Gauche**.  
  
    Une nouvelle colonne est ajoutée à gauche de la colonne **StateProvince** . 
    
    ![Capture d’écran montrant comment insérer une colonne à gauche d’une colonne dans un rapport.](../reporting-services/media/report-builder-expression-tutorial-insert-column.png) 
  
2.  Cliquez sur l’en-tête de la nouvelle colonne, puis tapez **Name**.  
  
3.  Cliquez avec le bouton droit sur la cellule de données pour la colonne **Name** et cliquez sur **Expression**.  

    ![Capture d’écran montrant comment insérer une expression dans un rapport.](../reporting-services/media/report-builder-expression-tutorial-insert-expression.png)
  
4.  Dans la boîte de dialogue **Expression** , développez **Fonctions communes**, puis cliquez sur **Texte**.  
  
5.  Dans la liste **Élément** , double-cliquez sur **Left**.  
  
    La fonction **Left** est ajoutée à l’expression.  
    
    ![Capture d’écran montrant comment ajouter une fonction Left à une expression.](../reporting-services/media/report-builder-expression-tutorial-left-function.png)
  
6.  Dans la liste **Catégorie** , cliquez sur **Champs (Expressions)** .  
  
7.  Dans la liste **Valeurs** , double-cliquez sur **FirstName**.  
  
8.  Tapez **, 1)**  
  
    Cette expression extrait un caractère de la valeur **FirstName** , en partant de la gauche.  
  
9. Tapez **&". "&**  

    Un point et un espace sont ajoutés après l’expression.
  
10. Dans la liste **Valeurs** , double-cliquez sur **LastName**.  
  
    L’expression complétée est la suivante : `=Left(Fields!FirstName.Value, 1) &". "& Fields!LastName.Value`  
    
    ![Capture d’écran montrant comment ajouter une valeur LastName à une expression.](../reporting-services/media/report-builder-expression-tutorial-complete-name-expression.png)
  
11. [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
12. Cliquez sur **Exécuter** pour afficher un aperçu du rapport.  

## <a name="optional-format-the-date-and-currency-columns-and-header-row"></a><a name="DateFormat"></a>(facultatif) Mettre en forme les colonnes de date et de devise, et la ligne d’en-tête  
Dans cette section, vous allez mettre en forme la colonne **Last Purchase** qui contient des dates et la colonne YTDPurchase qui contient des devises. Vous allez également mettre en forme la ligne d’en-tête.  
  
### <a name="to-format-the-date-column"></a>Pour formater la colonne de date  
  
1.  Cliquez sur **Conception** pour repasser en mode Conception.  
  
2.  Sélectionnez la cellule de données dans la colonne **Last Purchase**, puis sous l’onglet **Accueil** > section **Nombre**, sélectionnez **Date**.  

    ![Capture d’écran montrant comment définir la colonne Last Purchase sur Date.](../reporting-services/media/report-builder-expression-tutorial-date-format.png)
  
3.  Ensuite, dans la section **Nombre** , cliquez sur la flèche en regard de **Styles des espaces réservés** , puis sélectionnez **Valeurs d’aperçu**. 

    ![Capture d’écran montrant l’option Exemples de valeurs dans le générateur de rapports.](../reporting-services/media/report-builder-expression-tutorial-sample-values.png)

    Vous pouvez maintenant voir un exemple de la mise en forme que vous avez sélectionnée. 
  
### <a name="to-format-the-currency-column"></a>Pour mettre en forme des valeurs monétaires

- Sélectionnez la cellule de données dans la colonne **YTDPurchase** , puis dans la section **Nombre** , sélectionnez **Symbole monétaire**.
 
### <a name="to-format-the-column-headers"></a>Pour mettre en forme des en-têtes de colonnes

1. Sélectionnez la ligne des en-têtes de colonnes.

2. Sous l’onglet **Accueil** > section **Paragraphe**, sélectionnez **Left**. 

    ![Capture d’écran montrant comment mettre en forme des en-têtes dans le générateur de rapports.](../reporting-services/media/report-builder-expression-tutorial-format-headings.png)

3. Cliquez sur **Exécuter** pour afficher un aperçu du rapport. 

Voici le rapport avec les dates, les devises et les en-têtes de colonnes mis en forme.

![Capture d’écran montrant l’aperçu du rapport mis en forme.](../reporting-services/media/report-builder-expression-tutorial-preview-formatted.png)

  
## <a name="4-use-color-to-display-gender"></a><a name="Gender"></a>4. Utiliser des couleurs pour afficher le sexe  
Dans cette section, vous allez ajouter des couleurs pour afficher le sexe d’une personne. Vous allez ajouter une colonne pour afficher la couleur, puis déterminer la couleur qui apparaît dans la colonne en fonction de la valeur du champ Sexe.  
  
Pour conserver la couleur que vous avez appliquée dans cette cellule de table lorsque vous transformez le rapport en rapport à bandes, ajoutez un rectangle, puis ajoutez la couleur d’arrière-plan au rectangle.  
    
 
### <a name="to-add-an-mf-column"></a>Pour ajouter une colonne M/F  
  
1.  Cliquez avec le bouton droit sur la colonne **Name** , pointez sur **Insérer une colonne**, puis cliquez sur **Gauche**.  
  
    Une nouvelle colonne est ajoutée à gauche de la colonne **Name** .  
  
2.  Cliquez sur l’en-tête de la nouvelle colonne, puis tapez **M/F**.  
  
### <a name="to-add-a-rectangle"></a>Pour ajouter un rectangle  
  
1.   Sous l’onglet **Insérer** , cliquez sur **Rectangle** , puis cliquez sur la cellule de données de la colonne **M/F** .  
  
     Un rectangle est ajouté à la cellule.  
     
     ![Capture d’écran montrant comment insérer un rectangle.](../reporting-services/media/report-builder-expression-tutorial-insert-rectangle.png)
  
2. Faites glisser le séparateur de colonne entre **M/F** et **Name** pour rendre la colonne **M/F** plus étroite.

    ![Capture d’écran montrant comment rendre une colonne plus étroite.](../reporting-services/media/report-builder-expression-tutorial-narrow-column.png)
  
### <a name="to-use-color-to-show-gender"></a>Pour utiliser des couleurs pour afficher le sexe  
  
1.  Cliquez avec le bouton droit sur le rectangle dans la cellule de données de la colonne **M/F** , puis cliquez sur **Propriétés du rectangle**.  
  
2.  Dans la boîte de dialogue **Propriétés du rectangle**, sous l’onglet **Remplissage**, cliquez sur le bouton d’expression **fx** situé en regard de **Couleur de remplissage**.  
  
3.  Dans la boîte de dialogue **Expression** , développez **Fonctions communes** et cliquez sur **Flux de programme**.  
  
4.  Dans la liste **Élément** , double-cliquez sur **Switch**.  
  
5.  Dans la liste **Catégorie** , cliquez sur **Champs (Expressions)** .  
  
6.  Dans la liste **Valeurs** , double-cliquez sur **Gender**.  
  
7.  Tapez **= « Masculin »,** (y compris la virgule).

8. Dans la liste **Catégorie** , cliquez sur **Constantes**, et dans la zone **Valeurs** , cliquez sur **Bleuet**.

    ![Capture d’écran montrant comment utiliser une couleur pour indiquer le sexe.](../reporting-services/media/report-builder-expression-tutorial-color-expression-cornflower-blue.png)

9. Ajoutez une virgule après « Bleuet ». 
  
5.  Dans la liste **Catégorie** , cliquez sur **Champs (Expressions)** , puis, dans la liste **Valeurs** , double-cliquez de nouveau sur **Gender** .  
  
7.  Tapez **= « Féminin »,** (y compris la virgule). 

8. Dans la liste **Catégorie** , cliquez sur **Constantes**, et dans la zone **Valeurs** , cliquez sur **Tomate**.

13. Ajoutez une parenthèse fermante **)** après « Tomate ». 
  
    L’expression complétée est la suivante : `=Switch(Fields!Gender.Value ="Male", "CornflowerBlue",Fields!Gender.Value ="Female","Tomato")`  
    
    ![Capture d’écran montrant l’expression complète dans la boîte de dialogue Expression.](../reporting-services/media/report-builder-expression-tutorial-color-expression-complete.png)
  
12. Cliquez sur **OK**, puis de nouveau sur **OK** pour fermer la boîte de dialogue **Propriétés du rectangle** .  
  
14. Cliquez sur **Exécuter** pour afficher un aperçu du rapport.  

    ![Capture d’écran montrant l’aperçu avec la colonne M/F.](../reporting-services/media/report-builder-expression-tutorial-preview-m-f-column.png)

### <a name="to-format-the-color-rectangles"></a>Pour mettre en forme les rectangles de couleur

1. Cliquez sur **Conception** pour repasser en mode Conception.  

16. Sélectionnez le rectangle dans la colonne **M/F** . Dans le volet Propriétés, dans la section Bordure, définissez ces propriétés :

    - BorderColor = White
    - BorderStyle = Solid
    - BorderWidth = 5pt
    
    ![Capture d’écran montrant comment mettre en forme les rectangles de couleur dans la colonne M/F.](../reporting-services/media/report-builder-expression-tutorial-format-m-f-column.png)

18. Cliquez sur **Exécuter** pour afficher un nouvel aperçu du rapport. Cette fois-ci, les blocs de couleur sont entourés d’un espace blanc.

    ![Capture d’écran montrant l’aperçu avec les rectangles mis en forme dans la colonne M/F.](../reporting-services/media/report-builder-expression-tutorial-preview-formatted-m-f-column.png)  
  
## <a name="5-look-up-the-countryregion-name"></a><a name="Lookup"></a>5. Rechercher un nom de CountryRegion  
Dans cette section, vous allez créer le dataset CountryRegion et utiliser la fonction **Lookup** pour afficher le nom d’un pays/d’une région au lieu de l’identifiant de pays/région.  
  
### <a name="to-create-the-countryregion-dataset"></a>Pour créer le dataset CountryRegion  
  
1.  Cliquez sur **Conception** pour repasser en mode Conception.  
  
2.  Dans le volet des données de rapport, cliquez sur **Nouveau** , puis sur **Dataset**.  
  
3.  Dans **Propriétés du jeu de données**, cliquez sur **Utiliser un jeu de données incorporé dans mon rapport**.  
  
4.  Dans la liste **Source de données** , sélectionnez ExpressionsDataSource.  
  
5.  Dans la zone **Nom** , tapez **CountryRegion**.  
  
6.  Vérifiez que le type de requête **Texte** est sélectionné et cliquez sur **Concepteur de requêtes**.  
  
7.  Cliquez sur **Modifier en tant que texte**.  
  
8.  Copiez et collez la requête suivante dans le volet de requête :  
  
    ```  
    SELECT 1 AS ID, 'American Samoa' AS CountryRegion  
    UNION SELECT 2 AS CountryRegionID, 'Australia' AS CountryRegion  
    UNION SELECT 3 AS ID, 'Canada' AS CountryRegion  
    UNION SELECT 4 AS ID, 'Germany' AS CountryRegion  
    UNION SELECT 5 AS ID, 'Micronesia' AS CountryRegion  
    UNION SELECT 6 AS ID, 'France' AS CountryRegion  
    UNION SELECT 7 AS ID, 'United States' AS CountryRegion  
    UNION SELECT 8 AS ID, 'Brazil' AS CountryRegion  
    UNION SELECT 9 AS ID, 'Mexico' AS CountryRegion  
    UNION SELECT 10 AS ID, 'Japan' AS CountryRegion  
    UNION SELECT 10 AS ID, 'Australia' AS CountryRegion  
    UNION SELECT 12 AS ID, 'United Kingdom' AS CountryRegion  
    ```  
  
9. Cliquez sur **Exécuter** ( **!** ) pour exécuter la requête.  
  
    Les résultats de la requête sont les identifiants et les noms des pays/régions.  
  
10. [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
11. Recliquez sur **OK** pour fermer la boîte de dialogue **Propriétés du dataset** .  

     À présent, vous avez un deuxième dataset dans la colonne **Report Data** .
  
### <a name="to-look-up-values-in-the-countryregion-dataset"></a>Pour rechercher des valeurs dans le dataset CountryRegion  
  
1.  Cliquez sur l’en-tête de la colonne **Country Region ID** et supprimez le texte : **ID**, afin de lire **Country Region**.  
  
2.  Cliquez avec le bouton droit sur la cellule de données pour la colonne **Country Region** et cliquez sur **Expression**.  
  
3.  Supprimez l'expression sauf le signe (=) de début.  
  
    L’expression restante est : `=`  
  
4.  Dans la boîte de dialogue **Expression** , développez **Fonctions communes** et cliquez sur **Divers**. Ensuite, dans la liste **Élément** , double-cliquez sur **Lookup**.  
  
6.  Dans la liste **Catégorie** , cliquez sur **Champs (Expressions)** , puis dans la liste **Valeurs** , double-cliquez sur **CountryRegionID**.  
  
8.  Placez le curseur immédiatement après `CountryRegionID.Value`, puis tapez **,Fields!ID.value, Fields!CountryRegion.value, "CountryRegion")**  
  
    Expression complétée : `=Lookup(Fields!CountryRegionID.Value,Fields!ID.value, Fields!CountryRegion.value, "CountryRegion")`  
  
    La syntaxe de la fonction **Lookup** spécifie une recherche entre CountryRegionID dans le dataset Expressions, et ID dans le dataset CountryRegion qui retourne la valeur CountryRegion, également dans le dataset CountryRegion.  
  
10. [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
11. Cliquez sur **Exécuter** pour afficher un aperçu du rapport.  
  
## <a name="6-count-days-since-last-purchase"></a><a name="Count"></a>6. Compter les jours depuis le dernier achat  
Dans cette section, vous allez ajouter une colonne, puis utiliser la fonction **Now** ou la variable globale intégrée `ExecutionTime` pour calculer le nombre de jours écoulés depuis les derniers achats d’un client.  
  
### <a name="to-add-the-days-ago-column"></a>Pour ajouter la colonne Days Ago  
  
1.  Cliquez sur **Conception** pour repasser en mode Conception.  
  
2.  Cliquez avec le bouton droit sur la colonne **Last Purchase** , pointez sur **Insérer une colonne** et cliquez sur **Droite**.  
  
    Une nouvelle colonne est ajoutée à droite de la colonne **Last Purchase** .  
  
3.  Dans l’en-tête de colonne, tapez **Days Ago**.  
  
4.  Cliquez avec le bouton droit sur la cellule de données pour la colonne **Days Ago** et cliquez sur **Expression**.  
  
5.  Dans la boîte de dialogue **Expression**, développez **Fonctions communes**, puis cliquez sur **Date & heure**.  
  
6.  Dans la liste **Élément** , double-cliquez sur **DateDiff**.  
  
7.  Immédiatement après `DateDiff(`, tapez **"d",** (y compris les guillemets "" et la virgule). 
  
9. Dans la liste **Catégorie** , cliquez sur **Champs (Expressions)** , puis dans la liste **Valeurs** , double-cliquez sur **LastPurchase**.  
  
11. Immédiatement après `Fields!LastPurchase.Value`, tapez **,** (virgule). 
  
13. Dans la liste **Catégorie**, cliquez de nouveau sur **Date et heure**, puis dans la liste **Élément**, double-cliquez sur **Now**.  
  
    > [!WARNING]  
    > Dans les rapports de production, vous ne devez pas utiliser la fonction **Now** dans les expressions évaluées plusieurs fois pendant la génération du rapport (par exemple, dans les lignes de détails d’un rapport). La valeur de **Now** change de ligne en ligne et les différentes valeurs affectent les résultats de l’évaluation des expressions, ce qui entraîne des résultats légèrement incohérents. Utilisez à la place la variable globale `ExecutionTime` fournie par [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] .  
  
15. Supprimez la parenthèse de gauche après `Now(`, puis tapez une parenthèse fermante **)** .  
  
    L’expression complétée est la suivante : `=DateDiff("d", Fields!LastPurchase.Value, Now)`  
    
    ![Capture d’écran montrant l’expression complète pour la date depuis le dernier achat.](../reporting-services/media/report-builder-expression-tutorial-date-since-last-purchase.png)
  
17. [!INCLUDE[clickOK](../includes/clickok-md.md)]  

11. Cliquez sur **Exécuter** pour afficher un aperçu du rapport.  
  
## <a name="7-use-an-indicator-to-show-sales-comparison"></a><a name="Indicator"></a>7. Utiliser un indicateur pour afficher la comparaison des ventes  
Dans cette section, vous allez ajouter une nouvelle colonne et utiliser un indicateur pour afficher si les achats de l’année en cours à ce jour (YTD) d’une personne sont au-dessus ou en-dessous de la moyenne des achats YTD. La fonction **Round** supprime les décimales des valeurs.  
  
La configuration de l’indicateur et de ses états s’effectue en plusieurs étapes. Si vous le souhaitez, vous pouvez avancer dans la procédure « Pour configurer l’indicateur » et copier-coller les expressions complétées à partir de ce tutoriel dans la boîte de dialogue **Expression**.  
  
### <a name="to-add-the--or---avg-sales-column"></a>Pour ajouter la colonne + or - AVG Sales  
  
1.  Cliquez avec le bouton droit sur la colonne **YTD Purchase** , pointez sur **Insérer une colonne** et cliquez sur **Droite**.  
  
    Une nouvelle colonne est ajoutée à droite de la colonne **YTD Purchase** .  
  
2.  Cliquez sur l’en-tête de la colonne, puis tapez **+ or - AVG Sales**.  
  
### <a name="to-add-an-indicator"></a>Pour ajouter un indicateur  
  
1.  Sous l’onglet **Insérer** , cliquez sur **Indicateur** et cliquez sur la cellule de données de la colonne **+ or - AVG Sales** .  
  
    La boîte de dialogue **Sélectionner un type d’indicateur** s’ouvre.  
  
2.  Dans le groupe **Directionnel** des jeux d’icônes, cliquez sur le jeu des trois flèches grises.  

    ![Capture d’écran montrant comment ajouter un indicateur.](../reporting-services/media/report-builder-expression-tutorial-select-indicator.png)
  
3.  [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
### <a name="to-configure-the-indicator"></a>Pour configurer l'indicateur  
  
1.  Cliquez avec le bouton droit sur l’indicateur, cliquez sur **Propriétés de l’indicateur**, puis sur **Valeur et états**.  
  
2.  Cliquez sur le bouton d’expression **fx** situé à côté de la zone de texte **Valeur** .  
  
3.  Dans la boîte de dialogue **Expression** , développez **Fonctions communes**, puis cliquez sur **Math**.  
  
4.  Dans la liste **Élément** , double-cliquez sur **Round**.  
  
5.  Dans la liste **Catégorie** , cliquez sur **Champs (Expressions)** , puis dans la liste **Valeurs** , double-cliquez sur **YTDPurchase**.  
  
7.  Immédiatement après `Fields!YTDPurchase.Value`, tapez  **-** (signe moins). 
  
9. Développez à nouveau **Fonctions communes** , cliquez sur **Agrégat**, puis dans la liste **Élément** , double-cliquez sur **Avg**.  
  
11. Dans la liste **Catégorie** , cliquez sur **Champs (Expressions)** , puis dans la liste **Valeurs** , double-cliquez sur **YTDPurchase**.  
  
13. Immédiatement après `Fields!YTDPurchase.Value`, tapez **, "Expressions"))** .  
  
    L’expression complétée est la suivante : `=Round(Fields!YTDPurchase.Value - Avg(Fields!YTDPurchase.Value, "Expressions"))`  
  
15. [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
16. Dans la zone **Unité de mesure des états** , sélectionnez **Numérique**.  
  
17. Dans la ligne contenant la flèche pointant vers le bas, cliquez sur le bouton **fx** situé à droite de la zone de texte pour la valeur **Démarrer** .  

    ![Capture d’écran montrant comment sélectionner le bouton fx en regard de la zone de texte Démarrer.](../reporting-services/media/report-builder-expression-tutorial-indicator-start.png)
  
18. Dans la boîte de dialogue **Expression** , développez **Fonctions communes**, puis cliquez sur **Math**.  
  
19. Dans la liste **Élément** , double-cliquez sur **Round**.  
  
20. Dans la liste **Catégorie** , cliquez sur **Champs (Expressions)** , puis dans la liste **Valeurs** , double-cliquez sur **YTDPurchase**.  
  
22. Immédiatement après `Fields!YTDPurchase.Value`, tapez  **-** (signe moins). 
  
24. Développez à nouveau **Fonctions communes** , cliquez sur **Agrégat**, puis dans la liste **Élément** , double-cliquez sur **Avg**.  
  
26. Dans la liste **Catégorie** , cliquez sur **Champs (Expressions)** , puis dans la liste **Valeurs** , double-cliquez sur **YTDPurchase**.  
  
28. Immédiatement après `Fields!YTDPurchase.Value`, tapez **, "Expressions")) < 0**  
  
    Expression complétée : `=Round(Fields!YTDPurchase.Value - Avg(Fields!YTDPurchase.Value, "Expressions")) < 0`  
  
30. [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
31. Dans la zone de texte de la valeur **Fin** , tapez **0**  
  
32. Cliquez sur la ligne contenant la flèche pointant à l’horizontale et cliquez sur **Supprimer**.  

    ![Capture d’écran montrant comment supprimer un indicateur.](../reporting-services/media/report-builder-expression-tutorial-delete-indicator-state.png)
    
    À présent, vous n’avez plus que deux flèches, vers le haut et vers le bas.
  
33. Dans la ligne contenant la flèche pointant vers le haut, dans la zone **Démarrer** , tapez **0**  
  
34. Cliquez sur le bouton **fx** situé à droite de la zone de texte pour la valeur **Fin** .  
  
35. Dans la boîte de dialogue **Expression** , supprimez **100** , puis créez l’expression : `=Round(Fields!YTDPurchase.Value - Avg(Fields!YTDPurchase.Value, "Expressions")) >0`  
  
36. [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
37. Cliquez sur **OK** à nouveau pour fermer la boîte de dialogue **Propriétés de l’indicateur** .  
  
38. Cliquez sur **Exécuter** pour afficher un aperçu du rapport.  

    ![Capture d’écran montrant l’aperçu avec la colonne + ou - AVG Sales comprenant tous les nouveaux indicateurs.](../reporting-services/media/report-builder-expression-tutorial-preview-indicator.png)
  
## <a name="8-make-a-banded-report"></a><a name="GreenBar"></a>8. Créer un rapport à bandes  
Créez un paramètre pour que les lecteurs du rapport puissent spécifier la couleur à appliquer aux lignes alternées dans le rapport, pour en faire un rapport à bandes.  
  
### <a name="to-add-a-parameter"></a>Pour ajouter un paramètre  
  
1.  Cliquez sur **Conception** pour repasser en mode Conception.  
  
2.  Dans le volet **Données du rapport** , cliquez avec le bouton droit sur **Paramètres** et cliquez sur **Ajouter un paramètre**.  

    ![Capture d’écran montrant comment ajouter un paramètre.](../reporting-services/media/report-builder-expression-tutorial-add-parameter.png)
  
    La boîte de dialogue **Propriétés du paramètre de rapport** s'ouvre.  
  
3.  Dans **Invite**, tapez **Choisir une couleur**.  
  
4.  Dans la zone **Nom**, tapez **RowColor**.  
  
5.  Dans la page **Valeurs disponibles** , sélectionnez **Spécifier les valeurs**.  
  
7.  Cliquez sur **Add**.  
  
8.  Dans la zone **Étiquette** , tapez **Yellow**.  
  
9. Dans la zone **Valeur** , tapez **Yellow**.  
  
10. Cliquez sur **Add**.  
  
11. Dans la zone **Étiquette** , tapez **Green**.  
  
12. Dans la zone **Valeur** , tapez **PaleGreen**.  
  
13. Cliquez sur **Add**.  
  
14. Dans la zone **Étiquette** , tapez **Blue**.  
  
15. Dans la zone **Valeur** , tapez **LightBlue**.  
  
16. Cliquez sur **Add**.  
  
17. Dans la zone **Étiquette** , tapez **Pink**.  
  
18. Dans la zone **Valeur** , tapez **Pink**.  

    ![Capture d’écran de la boîte de dialogue Propriétés du paramètre de rapport montrant l’étape Choisissez les valeurs disponibles pour ce paramètre.](../reporting-services/media/report-builder-expression-tutorial-parameter-available.png)
  
19. [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
### <a name="to-apply-alternating-colors-to-detail-rows"></a>Pour appliquer des couleurs alternées aux lignes de détails  
  
1.   Sélectionnez toutes les cellules de la ligne de données, à l’exception de la cellule de la colonne **M/F** , qui a sa propre couleur d’arrière-plan.  

     ![Capture d’écran montrant les cellules sélectionnées dans une ligne de données.](../reporting-services/media/report-builder-expression-tutorial-select-banded.png)
  
4.  Dans le volet Propriétés, cliquez sur **BackgroundColor**. 

     Si le volet Propriétés n’est pas ouvert, sous l’onglet **Affichage** , cochez la case **Propriétés** .  
  
    Si les propriétés sont classées par catégorie dans le volet Propriétés, **BackgroundColor** est classé sous la catégorie **Divers** .  
  
5.  Cliquez sur la flèche pointant vers le bas, puis sur **Expression**.  

    ![Capture d’écran de la zone Propriétés montrant comment associer une expression à un BackgroundColor.](../reporting-services/media/report-builder-expression-tutorial-banded-color-property.png)
  
6.  Dans la boîte de dialogue **Expression** , développez **Fonctions communes** puis cliquez sur **Flux de programme**.  
  
7.  Dans la liste **Élément** , double-cliquez sur **IIf**.  
  
8.  Sous **Fonctions communes**, cliquez sur **Divers**, puis dans la liste **Élément** , double-cliquez sur **RowNumber**.  

9. Immédiatement après **RowNumber (** tapez **Nothing) MOD 2,**
  
8. Cliquez sur **Paramètres** et dans la liste **Valeurs** , double-cliquez sur **RowColor**.  
  
22. Immédiatement après `Parameters!RowColor.Value`, tapez **, « White »)**  
  
    L’expression complétée est la suivante : `=IIF(RowNumber(Nothing) MOD 2, Parameters!RowColor.Value, "White")`  
    
    ![Capture d’écran montrant l’expression complète de bandes de couleur.](../reporting-services/media/report-builder-expression-tutorial-banded-color-expressn.png)
  
24. [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
### <a name="run-the-report"></a>Exécuter le rapport  
  
1.  Sous l’onglet **Accueil** , cliquez sur **Exécuter**.  

    Vous ne voyez pas le rapport tant que vous n’avez pas choisi une couleur pour les bandes non blanches.
  
3.  Dans la liste **Choisir une couleur** , sélectionnez une couleur pour les bandes non blanches du rapport.  
    
    ![Capture d’écran montrant comment choisir une couleur pour les bandes non blanches.](../reporting-services/media/report-builder-expression-tutorial-select-color.png)
  
4.  Cliquez sur **Afficher le rapport**.  
  
    Le rapport est généré et les lignes alternées sont de la couleur d'arrière-plan que vous avez choisie. 
    
    ![Capture d’écran montrant l’aperçu composé de lignes avec des couleurs alternées.](../reporting-services/media/report-builder-expression-tutorial-preview-banded.png) 
  
## <a name="optional-add-a-report-title"></a><a name="Title"></a>(facultatif) Ajouter un titre au rapport  
Ajoutez un titre au rapport.  
  
### <a name="to-add-a-report-title"></a>Pour ajouter un titre de rapport  
  
1.  Dans l'aire de conception, cliquez sur **Cliquez pour ajouter un titre**.  
  
2.  Tapez **Sales Comparison Summary**, puis sélectionnez le texte.  
  
3.  Sous l’onglet **Accueil** , dans la zone **Police** , définissez :

    -  Taille = 18
    -  Couleur = Gris
    -  Gras
  
4.  Sous l’onglet **Accueil** , cliquez sur **Exécuter**.  
  
3.  Sélectionnez une couleur pour les bandes non blanches du rapport, puis cliquez sur **Afficher le rapport**.  
  
## <a name="optional-save-the-report"></a><a name="Save"></a>(facultatif) Enregistrer le rapport  
Vous pouvez enregistrer les rapports sur un serveur de rapports, dans une bibliothèque SharePoint ou sur l'ordinateur. Pour plus d’informations, consultez [Enregistrement des rapports &#40;Générateur de rapports&#41;](../reporting-services/report-builder/saving-reports-report-builder.md).  
  
Dans ce didacticiel, vous allez enregistrer le rapport sur un serveur de rapports. Si vous n'avez pas accès à un serveur de rapports, enregistrez le rapport sur votre ordinateur.  
  
### <a name="to-save-the-report-to-a-report-server"></a>Pour enregistrer le rapport sur un serveur de rapports  
  
1.  Dans le menu **Fichier**, cliquez sur **Enregistrer sous**.  
  
2.  Cliquez sur **Sites et serveurs récents**.  
  
3.  Sélectionnez ou tapez le nom du serveur de rapports sur lequel vous êtes autorisé à enregistrer des rapports.  
  
    Le message « Connexion au serveur de rapports » s'affiche. Une fois la connexion établie, le contenu du dossier de rapports spécifié par l’administrateur du serveur de rapports s’affiche comme emplacement par défaut des rapports.  
  
4.  Attribuez un nom à votre rapport, puis cliquez sur **Enregistrer**.  
  
Le rapport est enregistré sur le serveur de rapports. Le nom du serveur de rapports auquel vous êtes connecté est indiqué dans la barre d'état située au bas de la fenêtre.

À présent, les lecteurs de votre rapport peuvent afficher votre rapport dans le portail web [!INCLUDE[ssRSnoversion_md](../includes/ssrsnoversion-md.md)] .

![Capture d’écran du nouveau rapport terminé avec chaque expression visible.](../reporting-services/media/report-builder-expression-tutorial-final-in-browser.png)

   
## <a name="see-also"></a>Voir aussi  
[Expressions &#40;Générateur de rapports et SSRS&#41;](../reporting-services/report-design/expressions-report-builder-and-ssrs.md)  
[Exemples d’expressions &#40;Générateur de rapports et SSRS&#41;](../reporting-services/report-design/expression-examples-report-builder-and-ssrs.md)  
[Indicateurs &#40;Générateur de rapports et SSRS&#41;](../reporting-services/report-design/indicators-report-builder-and-ssrs.md)  
[Images, zones de texte, rectangles et lignes &#40;Générateur de rapports et SSRS&#41;](../reporting-services/report-design/images-text-boxes-rectangles-and-lines-report-builder-and-ssrs.md)  
[Tables &#40;Générateur de rapports et SSRS&#41;](../reporting-services/report-design/tables-report-builder-and-ssrs.md)  
[Datasets de rapport &#40;SSRS&#41;](../reporting-services/report-data/report-datasets-ssrs.md)  
  
  
  

