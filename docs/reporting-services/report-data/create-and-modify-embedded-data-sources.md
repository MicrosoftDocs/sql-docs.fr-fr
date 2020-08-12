---
title: Créer et modifier des sources de données incorporées | Microsoft Docs
description: Découvrez comment créer et modifier des sources de données incorporées qui sont définies dans une définition de rapport et utilisées uniquement par ce rapport.
ms.date: 03/01/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-data
ms.topic: conceptual
ms.assetid: 1c38c2e8-7a29-4f79-a4a3-85ed2b13723b
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 0e5f9aed11beacf056916e44e6f2df391444eae0
ms.sourcegitcommit: 6be9a0ff0717f412ece7f8ede07ef01f66ea2061
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85808449"
---
# <a name="create-and-modify-embedded-data-sources"></a>Créer et modifier des sources de données incorporées
  Une source de données incorporée est définie dans une définition de rapport et utilisée uniquement par ce rapport.  
  
## <a name="to-create-an-embedded-data-source-in-report-designer"></a>Pour créer une source de données incorporée dans le Concepteur de rapports  
  
1.  Dans la barre d'outils du volet des données de rapport, cliquez sur **Nouveau** , puis sur **Source de données**. La boîte de dialogue **Propriétés de la source de données** s'ouvre.  
  
    > [!NOTE]  
    >  Si le volet des données de rapport n’est pas visible, cliquez sur l’option **Données du rapport** du menu **Affichage** .  
  
2.  Dans la zone de texte **Nom** , tapez un nom pour la source de données ou acceptez la valeur par défaut. Le nom de la source de données est utilisé en interne dans le rapport. Par souci de clarté, il est recommandé que le nom de la source de données contienne le nom de la base de données spécifiée dans la chaîne de connexion.  
  
3.  Vérifiez que l’option **Connexion incorporée** est sélectionnée et procédez comme suit.  
  
    1.  Dans la liste déroulante **Type** , sélectionnez un type de source de données ; par exemple, **Microsoft SQL Server** ou **OLE DB**.  
  
    2.  Spécifiez une chaîne de connexion à l'aide de l'une des méthodes suivantes :  
  
        -   Tapez directement la chaîne de connexion dans la zone de texte **Chaîne de connexion** . Pour obtenir une liste d’exemples de chaînes de connexion, consultez [Créer des chaînes de connexion de données - Générateur de rapports et SSRS](../../reporting-services/report-data/data-connections-data-sources-and-connection-strings-report-builder-and-ssrs.md).  
  
        -   Cliquez sur le bouton d’expression (**fx)** pour créer une expression qui prend la valeur d’une chaîne de connexion. Dans la boîte de dialogue **Expression** , tapez l'expression dans le volet Expression. [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
        -   Cliquez sur **Modifier** pour ouvrir la boîte de dialogue **Propriétés de connexion** correspondant au type de source de données choisi à l'étape 2.  
  
             Renseignez les champs de la boîte de dialogue **Propriétés de connexion** comme il convient pour le type de source de données. Les propriétés de connexion incluent le type de la source de données, son nom, ainsi que les informations d'identification à utiliser. Une fois que vous avez spécifié les valeurs dans cette boîte de dialogue, cliquez sur **Tester la connexion** pour vérifier que la source de données est disponible et que les informations d'identification indiquées sont correctes. Pour plus d’informations sur les types de source de données spécifiques, consultez les rubriques dans [Ajouter des données à partir de sources de données externes &#40;SSRS&#41;](../../reporting-services/report-data/add-data-from-external-data-sources-ssrs.md).  
  
    3.  Cliquez sur **Informations d'identification**.  
  
         Spécifiez les informations d'identification à utiliser pour cette source de données. Le propriétaire de la source de données choisit le type d'informations d'identification pris en charge.  
  
4.  La nouvelle source de données incorporée apparaît dans le volet des données de rapport.  
  
## <a name="to-create-an-embedded-data-source-in-report-builder"></a>Pour créer une source de données incorporée dans le Générateur de rapports  
  
1.  Dans la barre d'outils du volet Données du rapport, cliquez sur **Nouveau**, puis sur **Source de données**. La boîte de dialogue **Propriétés de la source de données** s'ouvre.  
  
2.  Dans la zone de texte **Nom** , tapez un nom pour la source de données ou acceptez la valeur par défaut.  
  
3.  Vérifiez que l'option **Utiliser une connexion incorporée dans mon rapport** est sélectionnée.  
  
    1.  Dans la liste déroulante **Sélectionner un type de connexion** , sélectionnez un type de source de données ; par exemple, **Microsoft SQL Server** ou **OLE DB**.  
  
    2.  Spécifiez une chaîne de connexion en utilisant l'une des méthodes suivantes :  
  
        -   Tapez directement la chaîne de connexion dans la zone de texte **Chaîne de connexion** . Pour obtenir une liste d’exemples de chaînes de connexion, consultez [Créer des chaînes de connexion de données - Générateur de rapports et SSRS](data-connections-data-sources-and-connection-strings-report-builder-and-ssrs.md).  
  
        -   Cliquez sur le bouton d’expression (**fx)** pour créer une expression qui prend la valeur d’une chaîne de connexion. Dans la boîte de dialogue **Expression** , tapez l'expression dans le volet Expression. [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
        -   Cliquez sur **Générer** pour ouvrir la boîte de dialogue **Propriétés de connexion** correspondant au type de source de données choisi à l'étape 2.  
  
             Renseignez les champs de la boîte de dialogue **Propriétés de connexion** comme il convient pour le type de source de données. Les propriétés de connexion incluent le type de la source de données, son nom, ainsi que les informations d'identification à utiliser. Une fois que vous avez spécifié les valeurs dans cette boîte de dialogue, cliquez sur **Tester la connexion** pour vérifier que la source de données est disponible et que les informations d'identification indiquées sont correctes.  
  
4.  Cliquez sur **Informations d'identification**.  
  
     Spécifiez les informations d'identification à utiliser pour cette source de données. Le propriétaire de la source de données choisit le type d'informations d'identification pris en charge. Pour plus d’informations, consultez [Spécifier des informations d’identification et de connexion pour les sources de données de rapport](specify-credential-and-connection-information-for-report-data-sources.md).  
  
5.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
     La source de données apparaît dans le volet des données de rapport.  
  
## <a name="see-also"></a>Voir aussi  
 [Datasets incorporés dans le rapport et datasets partagés &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-data/report-embedded-datasets-and-shared-datasets-report-builder-and-ssrs.md)   
 [Spécifier des informations d'identification et de connexion pour les sources de données de rapports](specify-credential-and-connection-information-for-report-data-sources.md)  
  
  
