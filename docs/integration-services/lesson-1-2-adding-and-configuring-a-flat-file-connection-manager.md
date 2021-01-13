---
description: 'Leçon 1-2 : Ajouter et configurer un gestionnaire de connexions de fichiers plats'
title: 'Étape 2 : Ajouter et configurer un gestionnaire de connexions de fichiers plats | Microsoft Docs'
ms.custom: ''
ms.date: 01/03/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: tutorial
ms.assetid: 9a77dd32-d8c2-4961-ad37-2a971f9d6043
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 57bf9bd3863e5486c890088324641a5100581a02
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099222"
---
# <a name="lesson-1-2-add-and-configure-a-flat-file-connection-manager"></a>Leçon 1-2 : Ajouter et configurer un gestionnaire de connexions de fichiers plats

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]



Dans cette tâche, vous ajoutez un gestionnaire de connexions de fichiers plats au package que vous venez de créer. Un gestionnaire de connexions de fichiers plats permet à un package d'extraire des données d'un fichier plat. Grâce à ce Gestionnaire, vous pouvez spécifier le nom et l'emplacement du fichier, les paramètres régionaux et la page de codes et enfin, le format du fichier, y compris les séparateurs de colonnes, à appliquer lorsque le package extrait les données du fichier plat. Par ailleurs, vous pouvez spécifier manuellement le type de données pour les colonnes individuelles ou utiliser la boîte de dialogue **Suggérer les types de colonnes** pour mapper automatiquement les colonnes de données extraites aux types de données [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] .  
  
Vous devez créer un gestionnaire de connexions de fichiers plats pour chaque format de fichier que vous utilisez. Étant donné que ce tutoriel extrait des données de plusieurs fichiers plats qui ont tous le même format de données, vous devez uniquement ajouter et configurer un gestionnaire de connexions de fichiers plats pour l’exemple de package.  
  
Dans cette leçon, vous configurez les propriétés suivantes dans votre gestionnaire de connexions de fichiers plats :  
  
-   **Noms des colonnes :** étant donné que le fichier plat ne contient pas de noms de colonnes, le gestionnaire de connexions de fichiers plats crée des noms de colonnes par défaut. Ces noms par défaut ne servent pas à identifier ce que représente chaque colonne. Remplacez les noms par défaut par ceux qui correspondent à la table de faits dans laquelle les données du fichier plat doivent être chargées.  
  
-   **Mappages des données :** les mappages des types de données que vous spécifiez pour le gestionnaire de connexions de fichiers plats sont utilisés par tous les composants des sources de données de fichiers plats qui référencent ce gestionnaire de connexions. Vous pouvez choisir de mapper les types de données manuellement en utilisant le gestionnaire de connexions de fichiers plats ou bien d’utiliser la boîte de dialogue **Suggérer les types de colonnes** . Dans cette tâche, vous visualisez les mappages suggérés dans la boîte de dialogue **Suggérer les types de colonnes**, puis vous créez manuellement les mappages nécessaires dans la boîte de dialogue **Éditeur du gestionnaire de connexions de fichiers plats**.  
  
> [!NOTE]
> Le gestionnaire de connexions de fichiers plats fournit des informations de paramètres régionaux sur le fichier de données. Si votre ordinateur n’est pas configuré pour l’utilisation des paramètres régionaux **Anglais (États-Unis)**, vous devez définir des propriétés supplémentaires dans la boîte de dialogue **Éditeur du gestionnaire de connexions de fichiers plats**.  
  
## <a name="add-a-flat-file-connection-manager-to-the-ssis-package"></a>Ajouter un gestionnaire de connexions de fichiers plats au package SSIS  
  
1.  Dans le volet **Explorateur de solutions**, cliquez avec le bouton droit sur **Gestionnaires de connexions**, puis sélectionnez **Nouveau gestionnaire de connexions**.
1. Dans la boîte de dialogue **Ajout d’un gestionnaire de connexions SSIS**, sélectionnez **FLATFILE**, puis **Ajouter**.
  
2.  Dans la boîte de dialogue **Éditeur du gestionnaire de connexions de fichiers plats**, pour **Nom du gestionnaire de connexions**, entrez **Sample Flat File Source Data**.  
  
3.  Sélectionnez **Parcourir**.  
  
4.  Dans la boîte de dialogue **Ouvrir**, accédez au fichier **SampleCurrencyData.txt** sur votre ordinateur.  
  
5.  Décochez la case **Noms de colonne dans la première ligne de données**.  
  
### <a name="set-locale-sensitive-properties"></a>Définir les propriétés des paramètres régionaux  
  
1.  Dans la boîte de dialogue **Éditeur du gestionnaire de connexions de fichiers plats**, sélectionnez **Général**.  
  
2.  Définissez **Paramètres régionaux** sur **Anglais (États-Unis)** et **Page de codes** sur **1252**.  
  
### <a name="rename-columns-in-the-flat-file-connection-manager"></a>Renommer les colonnes dans le gestionnaire de connexions de fichiers plats  
  
1.  Dans la boîte de dialogue **Éditeur du gestionnaire de connexions de fichiers plats**, sélectionnez **Avancé**.  
  
2.  Dans le volet des propriétés, apportez les modifications suivantes :  
  
    -   Changez la propriété de nom de **Colonne 0** en **AverageRate**.  
  
    -   Changez la propriété de nom de **Colonne 1** en **CurrencyID**.  
  
    -   Changez la propriété de nom de **Colonne 2** en **CurrencyDate**.  
  
    -   Changez la propriété de nom de **Colonne 3** en **EndOfDayRate**.  
  
### <a name="remap-column-data-types"></a>Remapper les types de données des colonnes  
  
Par défaut, ces quatre colonnes ont initialement le type de données chaîne [DT_STR] avec **OutputColumnWidth** égal à 50.  

1.  Dans la boîte de dialogue **Éditeur du gestionnaire de connexions de fichiers plats**, sélectionnez **Suggérer les types**.  
  
    [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] suggère automatiquement des types de données appropriés en se basant sur les 200 premières lignes de données. Vous pouvez aussi modifier les options de suggestion et augmenter ou réduire l'échantillon de données, spécifier le type de données par défaut pour les entiers ou les données booléennes ou bien ajouter des espaces pour séparer les colonnes de type chaîne.  
  
    Pour l’instant, n’apportez aucune modification aux options de la boîte de dialogue **Suggérer les types de colonnes** et sélectionnez **OK** pour que [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] suggère des types de données pour les colonnes. Cette action vous ramène au volet **Avancé** de la boîte de dialogue **Éditeur du gestionnaire de connexions de fichiers plats**, où vous pouvez afficher les types de données pour les colonnes suggérés par [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)]. Ou bien, si vous cliquez sur **Annuler**, aucune suggestion n’est faite pour les métadonnées des colonnes, et le type de données chaîne par défaut (DT_STR) est utilisé.  
  
    Dans ce didacticiel, [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] suggère les types de données montrées dans la deuxième colonne de la table ci-dessous pour les données issues du fichier SampleCurrencyData.txt. La quatrième colonne fournit les types de données requis pour les colonnes dans la destination, qui sont définies dans une étape ultérieure.  
  
    |Colonne de fichier plat|Type suggéré|Colonne de destination|Type de destination|  
    |--------------------|------------------|----------------------|--------------------|  
    |AverageRate|float [DT_R4]|FactCurrencyRate.AverageRate|float|  
    |CurrencyID|string [DT_STR]|DimCurrency.CurrencyAlternateKey|nchar(3)|  
    |CurrencyDate|date [DT_DATE]|DimDate.FullDateAlternateKey|Date|  
    |EndOfDayRate|float [DT_R4]|FactCurrencyRate.EndOfDayRate|float|  
  
    Le type de données suggéré pour la colonne **CurrencyID** n’est pas compatible avec le type de données du champ de la table de destination. Étant donné que le type de données de `DimCurrency.CurrencyAlternateKey` est nchar(3), le type de la colonne **CurrencyID**, chaîne [DT_STR], doit être remplacé par chaîne Unicode [DT_WSTR]. De plus, le champ `DimDate.FullDateAlternateKey` étant défini avec le type de données date, le type pour **CurrencyDate** doit être changé de [DT_Date] en date de base de données [DT_DBDATE].  
  
2.  Dans la liste, sélectionnez la colonne **CurrencyID** et, dans le volet des propriétés, changez le type de données de la colonne **CurrencyID** de chaîne [DT_STR] en chaîne Unicode [DT_WSTR].  
  
3.  Dans le volet des propriétés, changez le type de données de la colonne **CurrencyDate** de date [DT_DATE] en date de base de données [DT_DBDATE].  
  
4.  Sélectionnez **OK**.  
  
## <a name="go-to-next-task"></a>Passer à la tâche suivante
[Étape 3 : Ajouter et configurer un gestionnaire de connexions OLE DB](../integration-services/lesson-1-3-adding-and-configuring-an-ole-db-connection-manager.md)  
  
## <a name="see-also"></a>Voir aussi  
[Gestionnaire de connexions de fichiers plats](../integration-services/connection-manager/flat-file-connection-manager.md)  
[Types de données d’Integration Services](../integration-services/data-flow/integration-services-data-types.md)  
  
  
  
