---
description: Traiter les insertions, les mises à jour et les suppressions
title: Traiter les insertions, les mises à jour et les suppressions | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- incremental load [Integration Services],processing data
ms.assetid: 13a84d21-2623-4efe-b442-4125a7a2d690
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 2f4fddbdaa770c79008d4c4c1cd481a0c6c6eeb2
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88457666"
---
# <a name="process-inserts-updates-and-deletes"></a>Traiter les insertions, les mises à jour et les suppressions

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Dans le flux de données d'un package Integration Services qui effectue un chargement incrémentiel des données modifiées, la deuxième tâche consiste à séparer les insertions, les mises à jour et les suppressions. Ensuite, vous pouvez utiliser des commandes appropriées pour les appliquer à la destination.  
  
> [!NOTE]  
>  La première tâche pour concevoir le flux de données d'un package qui effectue un chargement incrémentiel des données modifiées consiste à configurer le composant source qui exécute la requête qui récupère les données modifiées. Pour plus d’informations sur ce composant, consultez [Récupérer et comprendre les données modifiées](../../integration-services/change-data-capture/retrieve-and-understand-the-change-data.md). Pour obtenir une description du processus global de création d’un package qui effectue un chargement incrémentiel des données modifiées, consultez [Capture des données modifiées &#40;SSIS&#41;](../../integration-services/change-data-capture/change-data-capture-ssis.md).  
  
## <a name="associating-friendly-values-to-separate-inserts-updates-and-deletes"></a>Association de valeurs conviviales pour séparer des insertions, des mises à jour et des suppressions  
 Dans l’exemple de requête qui récupère les données modifiées, la fonction **cdc.fn_cdc_get_net_changes_<capture_instance>** retourne uniquement la colonne de métadonnées nommée **__$operation**. Cette colonne de métadonnées contient une valeur ordinale qui indique l'opération ayant entraîné la modification.  
  
> [!NOTE]  
>  Pour plus d’informations sur la requête qui utilise la fonction **cdc.fn_cdc_get_net_changes_<capture_instance>**, consultez [Créer la fonction de récupération des données modifiées](../../integration-services/change-data-capture/create-the-function-to-retrieve-the-change-data.md).  
  
 La mise en correspondance d'une valeur ordinale à son opération associée n'est pas aussi facile que d'utiliser un mnémonique de l'opération. Par exemple, 'D' peut facilement représenter une opération de suppression (Delete) et 'I' une opération d'insertion. L’exemple de requête créé dans la rubrique [Création de la fonction de récupération des données modifiées](../../integration-services/change-data-capture/create-the-function-to-retrieve-the-change-data.md)effectue cette conversion d’une valeur ordinale en valeur de chaîne conviviale qui est retournée dans une nouvelle colonne. Le segment de code suivant illustre cette conversion :  
  
```sql
select   
    ...  
    case __$operation  
        when 1 then 'D'  
        when 2 then 'I'  
        when 4 then 'U'  
        else null  
     end as CDC_OPERATION  
```  
  
## <a name="configuring-a-conditional-split-transformation-to-direct-inserts-updates-and-deletes"></a>Configuration d'une transformation de fractionnement conditionnel pour diriger des insertions, des mises à jour et des suppressions  
 Pour diriger des lignes de données modifiées vers l'une des trois sorties, la transformation de fractionnement conditionnel est idéale. La transformation vérifie simplement la valeur de la colonne **CDC_OPERATION** dans chaque ligne et détermine si cette modification est une insertion, une mise à jour ou une suppression.  
  
> [!NOTE]  
>  La colonne CDC_OPERATION contient une valeur de chaîne conviviale dérivée de la valeur numérique dans la colonne **__$operation** .  
  
#### <a name="to-split-inserts-updates-and-deletes-for-processing-by-using-a-conditional-split-transformation"></a>Pour fractionner des insertions, des mises à jour et des suppressions à des fins de traitement à l'aide d'une transformation de fractionnement conditionnel  
  
1.  Sous l’onglet **Flux de données** , ajoutez une transformation de fractionnement conditionnel.  
  
2.  Connectez la sortie de la source OLE DB à la transformation de fractionnement conditionnel.  
  
3.  Dans **Éditeur de transformation de fractionnement conditionnel**, dans le volet inférieur de l’éditeur, entrez les trois lignes suivantes pour désigner les trois sorties.  
  
    1.  Entrez une ligne avec la condition `CDC_OPERATION == "I"` pour diriger des lignes insérées vers la sortie pour les insertions.  
  
    2.  Entrez une ligne avec la condition `CDC_OPERATION == "U"` pour diriger des lignes mises à jours vers la sortie pour des mises à jours.  
  
    3.  Entrez une ligne avec la condition `CDC_OPERATION == "D"` pour diriger des lignes supprimées vers la sortie pour des suppressions.  
  
## <a name="next-step"></a>étape suivante  
 Après avoir fractionné les lignes à des fins de traitement, l'étape suivante consiste à appliquer les modifications à la destination.  
  
 **Rubrique suivante :** [Appliquer les modifications à la destination](../../integration-services/change-data-capture/apply-the-changes-to-the-destination.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Conditional Split Transformation](../../integration-services/data-flow/transformations/conditional-split-transformation.md)   
 [Fractionner un dataset à l'aide de la transformation de fractionnement conditionnel](../../integration-services/data-flow/transformations/split-a-dataset-by-using-the-conditional-split-transformation.md)  
  
  
