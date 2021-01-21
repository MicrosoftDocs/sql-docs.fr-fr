---
title: Collection de champs de dataset (Générateur de rapports) | Microsoft Docs
description: Découvrez la collection de champs de jeu de données. Les champs de dataset représentent les données d'une connexion de données. Un champ peut représenter des données numériques ou non numériques.
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-data
ms.topic: conceptual
ms.assetid: b3884576-1f7e-4d40-bb7d-168312333bb3
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 6ad6853715963c9a29ee4db2e73199fecd50801a
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/20/2021
ms.locfileid: "98597321"
---
# <a name="dataset-fields-collection-report-builder-and-ssrs"></a>Collection de champs de dataset (Générateur de rapports et SSRS)
  Les champs de dataset représentent les données d'une connexion de données. Un champ peut représenter des données numériques ou non numériques. À titre d'exemples, citons des chiffres d'affaires, des totaux de ventes, des noms de client, des identificateurs de base de données, des URL, des images, des données spatiales et des adresses de messagerie. Sur l'aire de conception, les champs s'affichent sous la forme d'expressions dans les éléments de rapport tels que les zones de texte, les tables et les graphiques.  
  
 Un rapport comporte trois types de champs et les affiche dans le volet des données de rapport : champs de dataset, champs calculés de dataset et champs prédéfinis.  
  
-   **Champs de dataset.** Métadonnées qui représentent la collection des champs retournés lorsque la requête de dataset s'exécute sur la source de données.  
  
-   **Champs calculés de dataset.** Champs supplémentaires que vous créez pour le dataset. Chaque champ calculé est créé en évaluant une expression que vous définissez.  
  
-   **Champs prédéfinis.** Métadonnées qui représentent une collection de champs fournie par le Générateur de rapports qui indique des informations sur le rapport, telles que le nom du rapport ou l'heure de son traitement. Pour plus d’informations, consultez [Références à des champs Globals et Users prédéfinis &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/built-in-collections-built-in-globals-and-users-references-report-builder.md).  
  
 Les noms de champ de dataset sont enregistrés dans la définition de dataset du rapport. Pour plus d’informations, consultez [Datasets incorporés dans le rapport et datasets partagés &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-data/report-embedded-datasets-and-shared-datasets-report-builder-and-ssrs.md).  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
##  <a name="dataset-fields-and-queries"></a><a name="Fields"></a> Champs de dataset et requêtes  
 Les champs de dataset sont spécifiés par la commande de requête de dataset et par tout champ calculé que vous définissez. La collection de champs que vous voyez dans votre rapport dépend du type de dataset dont vous disposez :  
  
-   **Dataset partagé.** La collection de champs correspond à la liste des champs pour la requête dans la définition du dataset partagé au moment où vous avez ajouté directement le dataset partagé à votre rapport, ou lorsque vous avez ajouté une partie de rapport incluant le dataset partagé. La collection de champs locale ne change pas lorsque la définition du dataset partagé est modifiée sur le serveur de rapports. Pour mettre à jour la collection de champs locale, vous devez actualiser la liste pour le dataset partagé local.  
  
-   **Dataset incorporé.** La collection de champs correspond à la liste de champs retournée suite à l'exécution de la requête actuelle sur la source de données.  
  
 Pour plus d’informations, consultez [Ajouter, modifier ou actualiser des champs dans le volet des données de rapport &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-data/add-edit-refresh-fields-in-the-report-data-pane-report-builder-and-ssrs.md)  
  
  
### <a name="calculated-fields"></a>Champs calculés  
 Vous spécifiez un champ calculé manuellement en créant une expression. Les champs calculés peuvent être utilisés pour créer de nouvelles valeurs qui n'existent pas dans la source de données. Un champ calculé peut, par exemple, représenter une nouvelle valeur, un ordre de tri personnalisé pour un jeu de valeurs de champ ou un champ existant converti en type de données différent.  
  
 Les champs calculés sont définis localement pour un rapport et ne peuvent pas être enregistrés dans le cadre d'un dataset partagé.  
  
 Pour plus d’informations, consultez [Ajouter, modifier ou actualiser des champs dans le volet des données de rapport &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-data/add-edit-refresh-fields-in-the-report-data-pane-report-builder-and-ssrs.md).  
  
  
### <a name="entities-and-entity-fields"></a>Entités et champs d'entité  
 Si vous utilisez une source de données de modèle de rapport, spécifiez les entités et les champs d'entité en tant que données de rapport. Dans le concepteur de requêtes d'un modèle de rapport, vous pouvez explorer et sélectionner interactivement des entités associées et choisir les champs que vous souhaitez inclure dans votre dataset de rapport. Une fois la requête créée, vous pouvez consulter l'ensemble d'identificateurs d'entité et de champs d'entité dans le volet des données de rapport. Les identificateurs d'entité sont générés automatiquement par le modèle de rapport et ne sont généralement pas visibles par l'utilisateur final.  
  
### <a name="using-extended-field-properties"></a>Utilisation des propriétés de champ étendues  
 Les sources de données qui prennent en charge des requêtes multidimensionnelles, comme [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], prennent en charge les propriétés de champ sur les champs. Les propriétés de champ apparaissent dans le jeu de résultats d'une requête, mais ne sont pas visibles dans le volet **Données du rapport** . Elles sont néanmoins disponibles pour les utiliser dans votre rapport. Pour vous référer à la propriété d'un champ, faites glisser le champ dans le rapport et remplacez la propriété par défaut **Value** par le nom de champ de la propriété souhaitée. Dans un cube [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] par exemple, vous pouvez définir des formats pour les valeurs dans les cellules de cube. La valeur mise en forme est disponible à l'aide de la propriété de champ **FormattedValue**. Pour utiliser directement la valeur au lieu d'utiliser une valeur, puis de définir la propriété de format de la zone de texte, faites glisser le champ vers la zone de texte et remplacez l'expression par défaut `=Fields!FieldName.Value` par `=Fields!FieldName.FormattedValue`.  
  
> [!NOTE]
>  Certaines propriétés **Field** ne peuvent pas être utilisées pour toutes les sources de données. Les propriétés **Value** et **IsMissing** sont définies pour toutes les sources de données. D’autres propriétés prédéfinies (comme **Key**, **UniqueName** et **ParentUniqueName** pour les sources de données multidimensionnelles) sont prises en charge uniquement si la source de données les fournit. Certains fournisseurs de données prennent en charge les propriétés personnalisées. Pour plus d’informations, consultez les rubriques spécifiques relatives aux propriétés de champ étendues correspondant à votre type de source de données dans [Datasets incorporés dans le rapport et datasets partagés &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-data/report-embedded-datasets-and-shared-datasets-report-builder-and-ssrs.md). Pour obtenir un exemple relatif à une source de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)][!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], consultez [Propriétés de champ étendues pour une base de données Analysis Services &#40;SSRS&#41;](../../reporting-services/report-data/extended-field-properties-for-an-analysis-services-database-ssrs.md).  
  
  
##  <a name="understanding-default-expressions-for-fields"></a><a name="Defaults"></a> Présentation des expressions par défaut pour les champs  
 Une zone de texte peut correspondre à un élément de rapport de zone de texte dans le corps du rapport ou il peut s'agir d'une zone de texte dans une cellule d'une région de données de tableau matriciel. Lorsque vous liez un champ à une zone de texte, l'emplacement de celle-ci détermine l'expression par défaut pour la référence du champ. Dans le corps du rapport, une expression de valeur de zone de texte doit spécifier un agrégat et un dataset. S'il n'existe qu'un seul dataset dans le rapport, cette expression par défaut est créée à votre place. Pour un champ qui représente une valeur numérique, la fonction d'agrégation par défaut est Sum. Pour un champ qui représente une valeur non numérique, l'agrégat par défaut est First.  
  
 Dans une région de données de tableau matriciel, l'expression de champ par défaut dépend des appartenances aux lignes et aux groupes de la zone de texte à laquelle vous ajoutez le champ. L'expression de champ pour le champ Sales, lorsqu'il est ajouté à une zone de texte dans la ligne de détails d'une table, est `[Sales]`. Si vous ajoutez ce même champ à une zone de texte dans un en-tête de groupe, l'expression par défaut est `(Sum[Sales])`, car l'en-tête de groupe affiche des valeurs résumées pour le groupe et non des valeurs de détails. Lors de l'exécution du rapport, le processeur de rapports évalue chaque expression et substitue le résultat dans le rapport.  
  
 Pour plus d’informations sur les expressions, consultez [Expressions &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/expressions-report-builder-and-ssrs.md).  
  
  
##  <a name="field-data-types"></a><a name="DataTypes"></a> Types de données de champ  
 Lorsque vous créez un dataset, il se peut que les types de données des champs sur la source de données ne correspondent pas exactement aux types de données utilisés dans un rapport. Les types de données peuvent passer par une ou deux couches de mappage. L'extension pour le traitement des données ou le fournisseur de données peuvent mapper les types de données de la source de données en types de données CLR (Common Language Runtime). Les types de données retournés par les extensions pour le traitement des données sont mappés à un sous-ensemble de types de données CLR (Common Language Runtime) à partir du [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)].  
  
 Dans la source de données, les données sont stockées dans des types de données pris en charge par la source de données. Par exemple, les données d’une base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doivent correspondre à l’un des types de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pris en charge, tels que **nvarchar** ou **datetime**. Lorsque vous récupérez des données à partir de la source de données, celles-ci sont transmises par l'intermédiaire d'une extension pour le traitement des données ou d'un fournisseur de données associé au type de source de données. Selon l'extension pour le traitement des données utilisée, les données peuvent être converties à partir des types de données utilisés par la source de données en types de données pris en charge par l'extension pour le traitement des données. [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] utilise des types de données pris en charge par le CLR (Common Language Runtime) installé avec [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]. Le fournisseur de données mappe chaque colonne contenue dans le jeu de résultats du type de données natif en un type de données CLR (Common Language Runtime) [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] .  
  
 À chaque étape, les données sont représentées par les types de données comme décrit dans la liste suivante :  
  
-   **Source de données** Il s'agit des types de données pris en charge par la version du type de source de données à laquelle vous vous connectez.  
  
     Par exemple, les types de données classiques pour une source de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] incluent **int**, **datetime** et **varchar**. [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] a introduit la prise en charge des types de données **date**, **time**, **datetimetz** et **datetime2**. Pour plus d’informations, consultez [Types de données (Transact-SQL)](/previous-versions/sql/sql-server-2008/ms187752(v=sql.100)).  
  
-   **Fournisseur de données ou extension pour le traitement des données** Il s'agit des types de données pris en charge par la version du fournisseur de données ou de l'extension pour le traitement des données que vous sélectionnez lorsque vous vous connectez à la source de données. Les fournisseurs de données se basant sur le [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] utilisent des types de données pris en charge par le CLR. Pour plus d’informations sur les types de données des fournisseurs de données [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] , consultez [Mappages de types de données (ADO.NET)](/dotnet/framework/data/adonet/data-type-mappings-in-ado-net) et [Utilisation des types de base](/dotnet/standard/base-types/common-type-system) sur MSDN.  
  
     Les types de données classiques pris en charge par le [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] sont **Int32** et **String**, par exemple. Les dates et heures de calendrier sont prises en charge par la structure **DateTime** . [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 2.0 Service Pack 1 a introduit la prise en charge de la structure **DateTimeOffset** pour les dates avec un décalage de fuseau horaire.  
  
    > [!NOTE]  
    >  Le serveur de rapports utilise les fournisseurs de données qui sont installés et configurés sur le serveur de rapports. Les clients de création de rapports en mode Aperçu utilisent les extensions pour le traitement des données installées et configurées sur l'ordinateur client. Vous devez tester votre rapport à la fois dans l'environnement de client de rapports et de serveur de rapports.  
  
-   **Processeur de rapports** Les types de données sont basés sur la version du CLR installée lorsque vous avez installé [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)].  
  
     Par exemple, les types de données que le processeur de rapports utilise pour les nouveaux types de date et d'heure introduits dans [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] sont indiqués dans le tableau ci-dessous :  
  
    |Type de données SQL|Type de données CLR|Description|  
    |-------------------|-------------------|-----------------|  
    |**Date**|**DateTime**|Date uniquement|  
    |**Time**|**TimeSpan**|Heure uniquement|  
    |**DateTimeTZ**|**DateTimeOffset**|Date et heure avec décalage de fuseau horaire|  
    |**DateTime2**|**DateTime**|Date et heure avec fractions de milliseconde|  
  
 Pour plus d’informations sur les types de bases de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , consultez [Types de données (moteur de base de données)](/previous-versions/sql/sql-server-2008/ms187752(v=sql.100)) et [Types de données et fonctions de date et d’heure (Transact-SQL)](/previous-versions/sql/sql-server-2008/ms186724(v=sql.100)).  
  
 Pour plus d’informations sur l’ajout de références dans un champ de dataset à partir d’une expression, consultez [Types de données dans les expressions &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/data-types-in-expressions-report-builder-and-ssrs.md).  
  
  
##  <a name="detecting-missing-fields-at-run-time"></a><a name="MissingFields"></a> Détection de champs manquants lors de l'exécution  
 Lorsque le rapport est traité, le jeu de résultats pour un dataset peut ne pas contenir de valeurs pour toutes les colonnes spécifiées, car les colonnes n'existent plus dans la source de données. Vous pouvez utiliser la propriété de champ IsMissing pour détecter si des valeurs ont été retournées pour un champ au moment de l’exécution. Pour plus d’informations, consultez [Référence à une collection de champs de dataset &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/built-in-collections-dataset-fields-collection-references-report-builder.md).  
  
  
## <a name="see-also"></a>Voir aussi  
 [Boîte de dialogue Propriétés du dataset, Champs &#40;Générateur de rapports&#41;]()   
 [Parties de rapports et datasets dans le Générateur de rapports](../../reporting-services/report-data/report-parts-and-datasets-in-report-builder.md)   
 [Datasets incorporés dans le rapport et datasets partagés &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-data/report-embedded-datasets-and-shared-datasets-report-builder-and-ssrs.md)  
  
