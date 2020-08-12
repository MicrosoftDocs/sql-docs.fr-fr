---
title: Ajouter un paramètre à valeurs multiples sur un rapport | Microsoft Docs
description: Découvrez comment ajouter à un rapport un paramètre qui permet à l’utilisateur de sélectionner plus d’une valeur pour le paramètre dans le Générateur de rapports.
ms.date: 03/07/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-design
ms.topic: conceptual
ms.assetid: 12ad0e77-4c28-4bbb-ab11-473ae89ec9f1
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: fc464c7ffeb6f166a8c4c39c70dc4001468f3568
ms.sourcegitcommit: e572f1642f588b8c4c75bc9ea6adf4ccd48a353b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/15/2020
ms.locfileid: "84779425"
---
# <a name="add-a-multi-value-parameter-to-a-report"></a>Ajouter un paramètre à valeurs multiples sur un rapport
  Il est possible d'ajouter à un rapport un paramètre qui permet à l'utilisateur de sélectionner plus d'une valeur pour le paramètre.  
  
 Il est possible de passer un paramètre à valeurs multiples dans le rapport, dans son URL. Pour obtenir un exemple d’URL contenant un paramètre à valeurs multiples, consultez [Passer un paramètre de rapport dans une URL](../../reporting-services/pass-a-report-parameter-within-a-url.md).  
  
 Pour plus d’informations sur la façon de passer un paramètre à valeurs multiples dans une procédure stockée, consultez [Working With Multi-Select Parameters for SSRS Reports](https://go.microsoft.com/fwlink/?LinkId=321529) sur le site mssqltips.com.  
  
## <a name="to-add-a-multi-value-parameter"></a>Pour ajouter un paramètre à valeurs multiples  
  
1.  Dans le Générateur de rapports, ouvrez le rapport auquel vous souhaitez ajouter le paramètre à valeurs multiples.  
  
2.  Cliquez avec le bouton droit sur le dataset du rapport, puis cliquez sur **Propriétés du dataset**.  
  
3.  Ajoutez une variable à la requête du dataset en modifiant le texte de la requête dans la zone **Requête** ou en ajoutant un filtre à l’aide du concepteur de requêtes. Pour plus d’informations, consultez [Générer une requête dans le Concepteur de requêtes relationnelles &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-data/build-a-query-in-the-relational-query-designer-report-builder-and-ssrs.md).  
  
    ```  
    WHERE  
      Production.ProductInventory.ProductID IN (@ProductID)  
    ```  
  
    > [!IMPORTANT]  
    > *  Le texte de la requête ne doit pas inclure l'instruction DECLARE pour la variable de requête.  
    > *  Le texte de la variable de requête doit inclure l’opérateur **IN** , comme indiqué dans l’exemple ci-dessus.  
    > *  Veillez à inclure les parenthèses autour de la variable, comme indiqué ci-dessus. Sinon, le rapport échoue et l’erreur « La variable scalaire doit être déclarée » s’affiche.  
  
    Un paramètre de dataset pour un dataset incorporé ou partagé est créé automatiquement pour la variable de la requête. Un paramètre de rapport est créé automatiquement pour le paramètre du dataset.  
  
4.  Dans le volet **Données du rapport** , développez le nœud **Paramètres** , cliquez avec le bouton droit sur le paramètre de rapport créé automatiquement pour le paramètre de dataset, puis cliquez sur **Propriétés du paramètre**.  
  
5.  Pour autoriser un utilisateur à sélectionner plusieurs valeurs pour le paramètre, sélectionnez **Autoriser les valeurs multiples** l’onglet **Général** .  
  
6.  (Facultatif) Sous l’onglet **Valeurs disponibles** , spécifiez la liste des valeurs disponibles visibles par l’utilisateur.  
  
     Une liste de valeurs disponibles limite les choix qu'un utilisateur peut faire aux valeurs valides pour le paramètre. Le haut de liste commence par une fonctionnalité **Sélectionner tout** afin que l’utilisateur puisse sélectionner ou effacer toutes les valeurs d’un simple clic. Si vous choisissez d'obtenir les valeurs disponibles pour le paramètre de rapport à partir d'une requête de dataset, assurez-vous de sélectionner un dataset qui ne contient pas la variable de requête associée au même paramètre de rapport.  
  
     Pour plus d’informations, consultez [Ajouter, modifier ou supprimer les valeurs disponibles d’un paramètre de rapport &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/add-change-or-delete-available-values-for-a-report-parameter.md).  

## <a name="see-also"></a>Voir aussi  
 [Ajouter des paramètres en cascade à un rapport &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/add-cascading-parameters-to-a-report-report-builder-and-ssrs.md)   
 [Ajouter, modifier ou supprimer un paramètre de rapport &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/add-change-or-delete-a-report-parameter-report-builder-and-ssrs.md)  
  
  
