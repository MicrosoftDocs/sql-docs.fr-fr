---
title: Fusionner des données à l’aide de la transformation d’union totale | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- merging datasets [Integration Services]
- merging inputs [Integration Services]
- combining datasets
- Union All transformation
- datasets [Integration Services], merging
ms.assetid: 78304403-a81c-4101-b87e-ec80ddfdac98
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 18ae2128e0f0bcbec5f3fb78a63778f44f260bf5
ms.sourcegitcommit: c8e1553ff3fdf295e8dc6ce30d1c454d6fde8088
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/22/2020
ms.locfileid: "86915236"
---
# <a name="merge-data-by-using-the-union-all-transformation"></a>Fusionner des données à l'aide de la transformation d'union totale

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  Pour pouvoir ajouter et configurer une transformation d'union totale, le package doit inclure au moins une tâche de flux de données et deux sources de données.  
  
 La transformation d'union totale combine plusieurs entrées. La première entrée qui est connectée à la transformation est l'entrée de référence ; les entrées connectées par la suite sont les entrées secondaires. La sortie contient les colonnes de l'entrée de référence.  
  
### <a name="to-combine-inputs-in-a-data-flow"></a>Pour connecter des entrées dans un flux de données  
  
1.  Dans [!INCLUDE[ssBIDevStudioFull](../../../includes/ssbidevstudiofull-md.md)], double-cliquez sur le package dans l’Explorateur de solutions pour ouvrir le package dans le concepteur [!INCLUDE[ssIS](../../../includes/ssis-md.md)] , puis cliquez sur l’onglet **Flux de données** .  
  
2.  Dans la **Boîte à outils**, faites glisser la transformation d’union totale vers l’aire de conception de l’onglet **Flux de données** .  
  
3.  Connectez la transformation d'union totale au flux de données en faisant glisser le connecteur à partir de la source de données ou d'une transformation précédente vers la transformation d'union totale.  
  
4.  Double-cliquez sur la transformation d'union totale.  
  
5.  Dans **l’Éditeur de transformation d’union totale**, mappez une colonne d’une entrée à une colonne de la liste **Nom de colonne de sortie** en cliquant sur une ligne, puis en sélectionnant une colonne dans la liste d’entrée. Sélectionnez **\<ignore>** dans la liste d’entrée pour ignorer le mappage de la colonne.  
  
    > [!NOTE]  
    >  Vous ne pouvez mapper deux colonnes que si leurs métadonnées correspondent.  
  
    > [!NOTE]  
    >  Les colonnes d'une entrée secondaire qui ne sont pas mappées à des colonnes de référence sont définies sur des valeurs null dans la sortie.  
  
6.  Si vous le souhaitez, modifiez le nom des colonnes dans la colonne **Nom de colonne de sortie** .  
  
7.  Répétez les étapes 5 et 6 pour chaque colonne de chaque entrée.  
  
8.  Cliquez sur **OK**.  
  
9. Pour enregistrer le package mis à jour, cliquez sur **Enregistrer les éléments sélectionnés** dans le menu **Fichier** .  
  
## <a name="see-also"></a>Voir aussi  
 [Transformation d'union totale](../../../integration-services/data-flow/transformations/union-all-transformation.md)   
 [Transformations Integration Services](../../../integration-services/data-flow/transformations/integration-services-transformations.md)   
 [Chemins Integration Services](../../../integration-services/data-flow/integration-services-paths.md)   
 [tâche de flux de données](../../../integration-services/control-flow/data-flow-task.md)  
  
  
