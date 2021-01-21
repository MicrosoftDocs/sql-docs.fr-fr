---
title: Ajouter des paramètres en cascade à un rapport (Générateur de rapports) | Microsoft Docs
description: Découvrez comment utiliser les paramètres en cascade dans vos rapports dans le Générateur de rapports pour gérer de grandes quantités de données de rapport.
ms.date: 08/17/2018
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-design
ms.topic: conceptual
ms.assetid: 3a22eec3-57a7-478e-b6fc-102a9dbe0591
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 77a96c10d94bb27d52f8527e6822ff036e4a214e
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596761"
---
# <a name="add-cascading-parameters-to-a-report-report-builder-and-ssrs"></a>Ajouter des paramètres en cascade à un rapport (Générateur de rapports et SSRS)
  Les paramètres en cascade permettent de gérer d'importantes quantités de données de rapport. Vous pouvez définir un ensemble de paramètres associés, de telle sorte que la liste des valeurs d'un paramètre dépende de la valeur choisie dans un autre paramètre. Par exemple, le premier paramètre est indépendant et peut présenter une liste de catégories de produits. Lorsque l'utilisateur sélectionne une catégorie, le deuxième paramètre dépend de la valeur du premier paramètre. Ses valeurs sont mises à jour avec une liste de sous-catégories au sein de la catégorie choisie. Lorsque l'utilisateur affiche le rapport, les valeurs des paramètres de catégorie et de sous-catégorie permettent de filtrer les données du rapport.  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
 Pour créer des paramètres en cascade, commencez par définir la requête de dataset et incluez un paramètre de requête pour chaque paramètre en cascade dont vous avez besoin. Vous devez également créer un dataset distinct pour chaque paramètre en cascade pour fournir les valeurs disponibles. Pour plus d’informations, consultez [Ajouter, modifier ou supprimer les valeurs disponibles d’un paramètre de rapport &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/add-change-or-delete-available-values-for-a-report-parameter.md).  
  
 L'ordre des paramètres en cascade est important, car la requête de dataset d'un paramètre situé plus bas dans la liste comporte une référence à chaque paramètre situé plus haut dans la liste. Au moment de l'exécution, l'ordre des paramètres dans le volet des données de rapport détermine l'ordre d'apparition des requêtes de paramètre dans le rapport et, par conséquent, l'ordre dans lequel un utilisateur choisit chaque valeur de paramètre consécutive.  
  
 Pour plus d'informations sur la création de paramètres en cascade avec plusieurs valeurs et notamment la fonctionnalité Sélectionner tout, consultez [Comment avoir un paramètre Tout sélectionner en cascade à valeurs multiples](/archive/blogs/psssql/)(en anglais).  
  
## <a name="to-create-the-main-dataset-with-a-query-that-includes-multiple-related-parameters"></a>Pour créer le dataset principal avec une requête incluant plusieurs paramètres associés  
  
1.  Dans le volet Données du rapport, cliquez avec le bouton droit sur une source de données, puis cliquez sur **Ajouter un dataset**.  
  
2.  Dans **Nom**, tapez le nom du dataset.  
  
3.  Dans **Source de données**, sélectionnez le nom de la source de données ou cliquez sur **Nouvelle** pour en créer une.  
  
4.  Dans **Type de requête**, choisissez le type de requête de la source de données sélectionnée. Cette rubrique part du principe que le type de requête **Texte** est utilisé.  
  
5.  Dans **Requête**, tapez la requête à utiliser pour récupérer des données pour ce rapport. La requête doit être constituée des éléments suivants :  
  
    1.  La liste des champs de sources de données. Par exemple, dans une instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] , l'instruction SELECT spécifie la liste des noms de colonnes de bases de données à partir d'une table ou d'une vue donnée.  
  
    2.  Un paramètre de requête par paramètre en cascade. Un paramètre de requête limite les données extraites de la source de données en spécifiant certaines valeurs à inclure dans la requête ou à exclure de la requête. En règle générale, les paramètres de requête se déclenchent dans une clause de restriction dans la requête. Par exemple, dans une instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] SELECT, les paramètres de requête se déclenchent dans la clause WHERE.  
  
6.  Cliquez sur **Exécuter** ( **!** ). Une fois que vous avez inclus les paramètres de requête et exécuté la requête, les paramètres de rapport qui correspondent aux paramètres de requête sont automatiquement créés.  
  
    > [!NOTE]  
    >  L'ordre des paramètres de requête lors de la première exécution d'une requête détermine leur ordre de création dans le rapport. Pour modifier l’ordre, consultez [Modifier l’ordre d’un paramètre de rapport &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/change-the-order-of-a-report-parameter-report-builder-and-ssrs.md).  
  
7.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
 Vous allez ensuite créer un dataset qui fournit les valeurs du paramètre indépendant.  
  
## <a name="to-create-a-dataset-to-provide-values-for-an-independent-parameter"></a>Pour créer un dataset en vue de fournir les valeurs d'un paramètre indépendant  
  
1.  Dans le volet Données du rapport, cliquez avec le bouton droit sur une source de données, puis cliquez sur **Ajouter un dataset**.  
  
2.  Dans **Nom**, tapez le nom du dataset.  
  
3.  Dans **Source de données**, vérifiez que le nom correspond au nom de la source de données que vous avez choisie au cours de l'étape 1.  
  
4.  Dans **Type de requête**, choisissez le type de requête de la source de données sélectionnée. Cette rubrique part du principe que le type de requête **Texte** est utilisé.  
  
5.  Dans **Requête**, tapez la requête à utiliser pour récupérer des valeurs pour ce paramètre. En règle générale, les requêtes des paramètres indépendants ne contiennent pas de paramètres de requête. Par exemple, pour créer une requête pour un paramètre qui fournit toutes les valeurs de catégorie, vous pouvez utiliser une instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] semblable à celle-ci :  
  
    ```  
    SELECT DISTINCT <column name> FROM <table>  
    ```  
  
     La commande SELECT DISTINCT supprime les valeurs dupliquées du jeu de résultats, de telle sorte que vous puissiez obtenir chaque valeur unique de la colonne spécifiée dans la table spécifiée.  
  
     Cliquez sur **Exécuter** ( **!** ). Le jeu de résultats indique les valeurs qui sont disponibles pour ce premier paramètre.  
  
6.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
 Vous allez ensuite définir les propriétés du premier paramètre en vue d'utiliser ce dataset pour remplir ses valeurs disponibles au moment de l'exécution.  
  
## <a name="to-set-available-values-for-a-report-parameter"></a>Pour définir les valeurs disponibles d'un paramètre de rapport  
  
1.  Dans le dossier Paramètres du volet Données du rapport, cliquez avec le bouton droit sur le premier paramètre, puis cliquez sur **Propriétés du paramètre**.  
  
2.  Dans **Nom**, vérifiez que le nom du paramètre est correct.  
  
3.  Cliquez sur **Valeurs disponibles**.  
  
4.  Cliquez sur **Obtenir les valeurs à partir d'une requête**. Trois champs apparaissent.  
  
5.  Dans **Dataset**, dans la liste déroulante, cliquez sur le nom du dataset que vous avez créé lors de la procédure précédente.  
  
6.  Dans le champ **Valeur** , cliquez sur le nom du champ qui fournit la valeur de paramètre.  
  
7.  Dans le champ **Étiquette** , cliquez sur le nom du champ qui fournit l'étiquette de paramètre.  
  
8.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
 Vous allez ensuite créer un dataset qui fournit les valeurs d'un paramètre dépendant.  
  
## <a name="to-create-a-dataset-to-provide-values-for-a-dependent-parameter"></a>Pour créer un dataset en vue de fournir les valeurs d'un paramètre dépendant  
  
1.  Dans le volet Données du rapport, cliquez avec le bouton droit sur une source de données, puis cliquez sur **Ajouter un dataset**.  
  
2.  Dans **Nom**, tapez le nom du dataset.  
  
3.  Dans **Source de données**, vérifiez que le nom correspond au nom de la source de données que vous avez choisie au cours de l'étape 1.  
  
4.  Dans **Type de requête**, choisissez le type de requête de la source de données sélectionnée. Cette rubrique part du principe que le type de requête **Texte** est utilisé.  
  
5.  Dans **Requête**, tapez la requête à utiliser pour récupérer des valeurs pour ce paramètre. En règle générale, les requêtes pour les paramètres dépendants comportent des paramètres de requête pour chaque paramètre dont ce paramètre dépend. Par exemple, pour créer une requête pour un paramètre qui fournit toutes les valeurs de sous-catégorie (paramètre dépendant) d’une catégorie (paramètre indépendant), vous pouvez utiliser une instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] semblable à celle-ci :  
  
    ```  
    SELECT DISTINCT Subcategory FROM <table>   
    WHERE (Category = @Category)  
    ```  
  
     Dans la clause WHERE, Category correspond au nom d’un champ de \<table> et @Category à un paramètre de requête. Cette instruction génère la liste des sous-catégories de la catégorie spécifiée dans @Category. Au moment de l'exécution, cette valeur est remplie avec la valeur que l'utilisateur choisit pour le paramètre de rapport qui porte le même nom.  
  
6.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
 Vous allez ensuite définir les propriétés du deuxième paramètre en vue d'utiliser ce dataset pour remplir ses valeurs disponibles au moment de l'exécution.  
  
## <a name="to-set-available-values-for-the-second-parameter"></a>Pour définir les valeurs disponibles pour le deuxième paramètre  
  
1.  Dans le dossier Paramètres du volet Données du rapport, cliquez avec le bouton droit sur le premier paramètre, puis cliquez sur **Propriétés du paramètre**.  
  
2.  Dans **Nom**, vérifiez que le nom du paramètre est correct.  
  
3.  Cliquez sur **Valeurs disponibles**.  
  
4.  Cliquez sur **Obtenir les valeurs à partir d'une requête**.  
  
5.  Dans **Dataset**, dans la liste déroulante, cliquez sur le nom du dataset que vous avez créé lors de la procédure précédente.  
  
6.  Dans le champ **Valeur** , cliquez sur le nom du champ qui fournit la valeur de paramètre.  
  
7.  Dans le champ **Étiquette** , cliquez sur le nom du champ qui fournit l'étiquette de paramètre.  
  
8.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
## <a name="to-test-the-cascading-parameters"></a>Pour tester les paramètres en cascade  
  
1.  Cliquez sur **Exécuter**.  
  
2.  Dans la liste déroulante du premier paramètre indépendant, choisissez une valeur.  
  
     Le processeur de rapports exécute la requête de dataset du paramètre suivant et lui transmet la valeur que vous avez choisie pour le premier paramètre. La liste déroulante du deuxième paramètre est remplie avec les valeurs disponibles qui reposent sur la première valeur de paramètre.  
  
3.  Dans la liste déroulante du deuxième paramètre dépendant, choisissez une valeur.  
  
     Le rapport ne s'exécute pas automatiquement une fois que vous avez choisi le dernier paramètre afin que vous puissiez modifier votre choix.  
  
4.  Cliquez sur **Afficher le rapport**. Le rapport actualise l'affichage en fonction des paramètres que vous avez choisis.  
  
## <a name="see-also"></a>Voir aussi  
 [Ajouter, modifier ou supprimer un paramètre de rapport &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/add-change-or-delete-a-report-parameter-report-builder-and-ssrs.md)   
 [Paramètres de rapport &#40;Générateur de rapports et Concepteur de rapports&#41;](../../reporting-services/report-design/report-parameters-report-builder-and-report-designer.md)   
 [Tutoriel : Ajouter un paramètre à votre rapport &#40;Générateur de rapports&#41;](../../reporting-services/tutorial-add-a-parameter-to-your-report-report-builder.md)   
 [Didacticiels du Générateur de rapports](../../reporting-services/report-builder-tutorials.md)   
 [Ajouter des filtres de datasets, des filtres de régions de données et des filtres de groupes &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/add-dataset-filters-data-region-filters-and-group-filters.md)   
 [Datasets incorporés dans le rapport et datasets partagés &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-data/report-embedded-datasets-and-shared-datasets-report-builder-and-ssrs.md)  
  
