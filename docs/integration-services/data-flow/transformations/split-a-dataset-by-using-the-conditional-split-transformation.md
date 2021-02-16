---
description: Fractionner un dataset à l'aide de la transformation de fractionnement conditionnel
title: Fractionner un dataset à l’aide de la transformation de fractionnement conditionnel | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- Conditional Split transformation
- splitting dataset
- datasets [Integration Services], splitting
ms.assetid: 23b3e84f-9296-4dc9-81c0-c7f06ae3f1ff
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 2deebc6e6c730be90264bf7f0f3586f56b3d1546
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350989"
---
# <a name="split-a-dataset-by-using-the-conditional-split-transformation"></a>Fractionner un dataset à l'aide de la transformation de fractionnement conditionnel

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  Pour pouvoir ajouter et configurer une transformation de fractionnement conditionnel, le package doit inclure au moins une tâche de flux de données et une source.  
  
### <a name="to-conditionally-split-a-dataset"></a>Pour fractionner un dataset de manière conditionnelle  
  
1.  Dans [!INCLUDE[ssBIDevStudioFull](../../../includes/ssbidevstudiofull-md.md)], ouvrez le projet [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] contenant le package souhaité.  
  
2.  Dans l'Explorateur de solutions, double-cliquez sur le package pour l'ouvrir.  
  
3.  Cliquez sur l’onglet **Flux de données** puis, dans la **Boîte à outils**, faites glisser la transformation de fractionnement conditionnel sur l’aire de conception.  
  
4.  Connectez la transformation de fractionnement conditionnel au flux de données en faisant glisser le connecteur à partir de la source de données ou de la transformation précédente vers la transformation de fractionnement conditionnel.  
  
5.  Double-cliquez sur la transformation de fractionnement conditionnel.  
  
6.  Dans **l’Éditeur de transformation de fractionnement conditionnel**, générez les expressions à utiliser comme conditions en faisant glisser des variables, des colonnes, des fonctions et des opérateurs dans la colonne **Condition** de la grille. Vous pouvez également taper l’expression dans la colonne **Condition** .  
  
    > [!NOTE]  
    >  Une variable ou une colonne peut être utilisée dans plusieurs expressions.  
  
    > [!NOTE]  
    >  Si l'expression n'est pas valide, son texte est mis en surbrillance et une info-bulle dans la colonne décrit les erreurs.  
  
7.  Si vous le souhaitez, modifiez les valeurs de la colonne **Nom de la sortie** . Les noms par défaut sont Case 1, Case 2, etc.  
  
8.  Pour modifier la séquence dans laquelle les conditions sont évaluées, cliquez sur les flèches haut ou bas.  
  
    > [!NOTE]  
    >  Placez les conditions qui ont le plus de chances d'être rencontrées en haut de la liste.  
  
9. Si vous le souhaitez, modifiez le nom de la sortie par défaut des lignes de données qui ne correspondent à aucune condition.  
  
10. Pour configurer l’affichage des erreurs, cliquez sur **Configurer l’affichage des erreurs**. Pour plus d’informations, consultez [Débogage d’un flux de données](../../../integration-services/troubleshooting/debugging-data-flow.md).  
  
11. Cliquez sur **OK**.  
  
12. Pour enregistrer le package mis à jour, cliquez sur **Enregistrer les éléments sélectionnés** dans le menu **Fichier** .  
  
## <a name="see-also"></a>Voir aussi  
 [Conditional Split Transformation](../../../integration-services/data-flow/transformations/conditional-split-transformation.md)   
 [Transformations Integration Services](../../../integration-services/data-flow/transformations/integration-services-transformations.md)   
 [Chemins Integration Services](../../../integration-services/data-flow/integration-services-paths.md)   
 [Types de données d’Integration Services](../../../integration-services/data-flow/integration-services-data-types.md)   
 [Tâche de flux de données](../../../integration-services/control-flow/data-flow-task.md)   
 [Expressions Integration Services &#40;SSIS&#41;](../../../integration-services/expressions/integration-services-ssis-expressions.md)  
  
  
