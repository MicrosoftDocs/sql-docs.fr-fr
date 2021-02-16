---
description: Dériver les valeurs de colonnes avec la transformation de colonne dérivée
title: Dériver les valeurs de colonnes avec la transformation de colonne dérivée | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- columns [Integration Services]
- derived columns
- columns [Integration Services], values
- Derived Column transformation
ms.assetid: 28b07746-fc6f-42b2-b741-9de6fac3f29c
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 06ecff11e6fc32ad59e6457ec2404292a9c22955
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353721"
---
# <a name="derive-column-values-with-the-derived-column-transformation"></a>Dériver les valeurs de colonnes avec la transformation de colonne dérivée

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  Pour pouvoir ajouter et configurer une transformation de colonne dérivée, le package doit inclure au moins une tâche de flux de données et une source.  
  
 La transformation Colonne dérivée utilise des expressions pour mettre à jour les valeurs de colonnes existantes ou pour ajouter des valeurs à de nouvelles colonnes. Quand vous décidez d’ajouter des valeurs à de nouvelles colonnes, la boîte de dialogue **Éditeur de transformation de colonne dérivée** évalue l’expression et définit les métadonnées des colonnes en conséquence. Par exemple, si une expression concatène deux colonnes, chacune possédant le type de données DT_WSTR et une longueur de 50, avec un espace entre les deux valeurs de colonnes, la nouvelle colonne possède le type de données DT_WSTR et une longueur de 101. Vous pouvez mettre à jour le type de données des nouvelles colonnes. La seule nécessité est que le type de données soit compatible avec les données insérées. Par exemple, la boîte de dialogue **Éditeur de transformation de colonne dérivée** génère une erreur de validation quand vous affectez une valeur de date à une colonne contenant un type de données entier. En fonction du type de données sélectionné, vous pouvez spécifier la longueur, la précision, l'échelle et la page de codes de la colonne.  
  
### <a name="to-derive-column-values"></a>Pour dériver les valeurs de colonnes  
  
1.  Dans [!INCLUDE[ssBIDevStudioFull](../../../includes/ssbidevstudiofull-md.md)], ouvrez le projet [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] contenant le package souhaité.  
  
2.  Dans l'Explorateur de solutions, double-cliquez sur le package pour l'ouvrir.  
  
3.  Cliquez sur l’onglet **Flux de données** puis, dans la **Boîte à outils**, faites glisser la transformation de colonne dérivée sur l’aire de conception.  
  
4.  Connectez la transformation de colonne dérivée au flux de données en faisant glisser le connecteur à partir de la source ou de la transformation précédente vers la transformation de colonne dérivée.  
  
5.  Double-cliquez sur la transformation de colonne dérivée.  
  
6.  Dans la boîte de dialogue **Éditeur de transformation de colonne dérivée** , générez les expressions à utiliser comme conditions en faisant glisser des variables, des colonnes, des fonctions et des opérateurs dans la colonne **Expression** de la grille. Vous pouvez également taper l’expression dans la colonne **Expression** .  
  
    > [!NOTE]  
    >  Si l'expression n'est pas valide, son texte est mis en surbrillance et une info-bulle dans la colonne décrit les erreurs.  
  
7.  Dans la liste **Colonne dérivée**, sélectionnez **\<add as new column>** pour écrire le résultat d’évaluation de l’expression dans une nouvelle colonne ou sélectionnez une colonne existante à mettre à jour avec le résultat de l’évaluation.  
  
     Si vous choisissez d’utiliser une nouvelle colonne, la boîte de dialogue **Éditeur de transformation de colonne dérivée** évalue l’expression et affecte un type de données à la colonne en fonction du type de données, de la longueur, de la précision, de l’échelle et de la page de codes.  
  
8.  Si vous utilisez une nouvelle colonne, sélectionnez un type de données dans la liste **Type de données** . En fonction du type de données sélectionné et si vous le souhaitez, mettez à jour les valeurs des colonnes **Longueur**, **Précision**, **Échelle** et **Page de codes** . Les métadonnées des colonnes existantes ne peuvent pas être modifiées.  
  
9. Si vous le souhaitez, modifiez les valeurs de la colonne **Nom de la colonne dérivée** .  
  
10. Pour configurer la sortie d’erreur, cliquez sur **Configurer la sortie d’erreur**. Pour plus d’informations, consultez [Débogage d’un flux de données](../../../integration-services/troubleshooting/debugging-data-flow.md).  
  
11. Cliquez sur **OK**.  
  
12. Pour enregistrer le package mis à jour, cliquez sur **Enregistrer les éléments sélectionnés** dans le menu **Fichier** .  
  
## <a name="see-also"></a>Voir aussi  
 [Derived Column Transformation](../../../integration-services/data-flow/transformations/derived-column-transformation.md)   
 [Types de données d’Integration Services](../../../integration-services/data-flow/integration-services-data-types.md)   
 [Transformations Integration Services](../../../integration-services/data-flow/transformations/integration-services-transformations.md)   
 [Chemins Integration Services](../../../integration-services/data-flow/integration-services-paths.md)   
 [Tâche de flux de données](../../../integration-services/control-flow/data-flow-task.md)   
 [Expressions Integration Services &#40;SSIS&#41;](../../../integration-services/expressions/integration-services-ssis-expressions.md)  
  
  
