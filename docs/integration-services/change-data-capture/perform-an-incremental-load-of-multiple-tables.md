---
description: Exécuter un chargement incrémentiel de plusieurs table
title: Exécuter un chargement incrémentiel de plusieurs tables | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- incremental load [Integration Services],multiple tables
ms.assetid: 39252dd5-09c3-46f9-a17b-15208cfd336d
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 229809b060a9a0d3103acdb9a562a3dc639b91e8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338486"
---
# <a name="perform-an-incremental-load-of-multiple-tables"></a>Exécuter un chargement incrémentiel de plusieurs table

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Dans la rubrique [Amélioration des chargements incrémentiels avec la capture de données modifiées](../../integration-services/change-data-capture/change-data-capture-ssis.md), le diagramme illustre un package de base qui effectue un chargement incrémentiel sur une seule table. Toutefois, il est plus fréquent de devoir effectuer un chargement incrémentiel de plusieurs tables.  
  
 Dans le cadre d'un chargement incrémentiel de plusieurs tables, certaines étapes doivent être effectuées une seule fois pour toutes les tables, alors que d'autres doivent être répétées pour chaque table source. Plusieurs options s’offrent à vous pour implémenter ces étapes dans [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]:  
  
-   Utiliser un package parent et des packages enfants.  
  
-   Utiliser plusieurs tâches de flux de données dans un package unique.  
  
## <a name="loading-multiple-tables-by-using-a-parent-package-and-multiple-child-packages"></a>Chargement de plusieurs tables à l'aide d'un package parent et de plusieurs packages enfants  
 Vous pouvez utiliser un package parent pour effectuer les étapes qui ne doivent être réalisées qu'une seule fois. Les packages enfants effectuent quant à eux les étapes devant être réalisées pour chaque table source.  
  
#### <a name="to-create-a-parent-package-that-performs-those-steps-that-only-have-to-be-done-once"></a>Pour créer un package parent qui effectue les étapes qui ne doivent être réalisées qu'une seule fois  
  
1.  Créez un package parent.  
  
2.  Dans le flux de contrôle, utilisez une tâche d'exécution SQL ou des expressions [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] pour calculer les points de terminaison.  
  
     Pour obtenir un exemple montrant comment calculer les points de terminaison, consultez [Spécifier un intervalle de données modifiées](../../integration-services/change-data-capture/specify-an-interval-of-change-data.md).  
  
3.  Si nécessaire, utilisez un conteneur de boucles For pour différer l'exécution jusqu'à ce que les données modifiées pour la période sélectionnée soient prêtes.  
  
     Pour obtenir un exemple de ce type de conteneur For Loop, consultez [Déterminer si les données modifiées sont prêtes](../../integration-services/change-data-capture/determine-whether-the-change-data-is-ready.md).  
  
4.  Utilisez plusieurs tâches d'exécution SQL pour exécuter des packages enfants pour chaque table à charger. Passez les points de terminaison calculés dans le package parent à chaque package enfant en utilisant des configurations Variable de package parent.  
  
     Pour plus d’informations, consultez [Tâche d’exécution de package](../../integration-services/control-flow/execute-package-task.md) et [Utiliser les valeurs des variables et des paramètres dans un package enfant](../../integration-services/packages/legacy-package-deployment-ssis.md#child).  
  
#### <a name="to-create-child-packages-to-perform-those-steps-that-have-to-be-done-for-each-source-table"></a>Pour créer des packages enfants afin d'effectuer les étapes devant être réalisées pour chaque table source  
  
1.  Pour chaque table source, créez un package enfant.  
  
2.  Dans le flux de contrôle, utilisez une tâche de script ou une tâche d'exécution SQL pour assembler l'instruction SQL qui sera utilisée pour rechercher les modifications.  
  
     Pour obtenir un exemple montrant comment assembler la requête, consultez [Préparer la recherche des données modifiées](../../integration-services/change-data-capture/prepare-to-query-for-the-change-data.md).  
  
3.  Utilisez une tâche de flux de données unique dans chaque package enfant pour charger les données modifiées et les appliquer à la destination. Configurez le flux de données comme décrit dans les étapes suivantes :  
  
    1.  Dans le flux de données, utilisez un composant source pour interroger les tables de modifications à propos des modifications qui se situent dans les points de terminaison sélectionnés.  
  
         Pour obtenir un exemple montrant comment interroger les tables de modifications, consultez [Récupérer et comprendre les données modifiées](../../integration-services/change-data-capture/retrieve-and-understand-the-change-data.md).  
  
    2.  Utilisez une transformation de fractionnement conditionnel pour diriger les insertions, les mises à jour et les suppressions vers les différentes sorties pour un traitement approprié.  
  
         Pour obtenir un exemple montrant comment configurer cette transformation pour diriger la sortie, consultez [Traiter les insertions, les mises à jour et les suppressions](../../integration-services/change-data-capture/process-inserts-updates-and-deletes.md).  
  
    3.  Utilisez un composant de destination pour appliquer les insertions à la destination. Utilisez des transformations de commande OLE DB avec des instructions UPDATE et DELETE paramétrables pour appliquer les mises à jour et les suppressions à la destination.  
  
         Pour obtenir un exemple montrant comment utiliser cette transformation pour appliquer les mises à jour et les suppressions, consultez [Appliquer des modifications à la destination](../../integration-services/change-data-capture/apply-the-changes-to-the-destination.md).  
  
## <a name="loading-multiple-tables-by-using-multiple-data-flow-tasks-in-a-single-package"></a>Chargement de plusieurs tables à l'aide de plusieurs tâches de flux de données dans un package unique  
 Une autre méthode consiste à utiliser un package unique qui contient une tâche de flux de données séparée pour chaque table source à charger.  
  
#### <a name="to-load-multiple-tables-by-using-multiple-data-flow-tasks-in-a-single-package"></a>Pour charger plusieurs tables à l'aide de plusieurs tâches de flux de données dans un package unique  
  
1.  Créez un package unique.  
  
2.  Dans le flux de contrôle, utilisez une tâche d’exécution SQL ou des expressions [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] pour calculer les points de terminaison.  
  
     Pour obtenir un exemple montrant comment calculer les points de terminaison, consultez [Spécifier un intervalle de données modifiées](../../integration-services/change-data-capture/specify-an-interval-of-change-data.md).  
  
3.  Si nécessaire, utilisez un conteneur de boucles For pour différer l'exécution jusqu'à ce que les données modifiées pour l'intervalle sélectionné soient prêtes.  
  
     Pour obtenir un exemple de ce type de conteneur For Loop, consultez [Déterminer si les données modifiées sont prêtes](../../integration-services/change-data-capture/determine-whether-the-change-data-is-ready.md).  
  
4.  Utilisez une tâche de script ou une tâche d'exécution SQL pour assembler l'instruction SQL qui sera utilisée pour rechercher les modifications.  
  
     Pour obtenir un exemple montrant comment assembler la requête, consultez [Préparer la recherche des données modifiées](../../integration-services/change-data-capture/prepare-to-query-for-the-change-data.md).  
  
5.  Utilisez plusieurs tâches de flux de données pour charger les données modifiées à partir de chaque table source et les appliquer à la destination. Configurez chaque tâche de flux de données comme décrit dans les étapes suivantes.  
  
    1.  Dans chaque flux de données, utilisez un composant source pour interroger les tables de modifications à propos des modifications qui se situent dans les points de terminaison sélectionnés.  
  
         Pour obtenir un exemple montrant comment interroger les tables de modifications, consultez [Récupérer et comprendre les données modifiées](../../integration-services/change-data-capture/retrieve-and-understand-the-change-data.md).  
  
    2.  Utilisez une transformation de fractionnement conditionnel pour diriger les insertions, les mises à jour et les suppressions vers les différentes sorties pour un traitement approprié.  
  
         Pour obtenir un exemple montrant comment configurer cette transformation pour diriger la sortie, consultez [Traiter les insertions, les mises à jour et les suppressions](../../integration-services/change-data-capture/process-inserts-updates-and-deletes.md).  
  
    3.  Utilisez un composant de destination pour appliquer les insertions à la destination. Utilisez des transformations de commande OLE DB avec des instructions UPDATE et DELETE paramétrables pour appliquer les mises à jour et les suppressions à la destination.  
  
         Pour obtenir un exemple montrant comment utiliser cette transformation pour appliquer les mises à jour et les suppressions, consultez [Appliquer des modifications à la destination](../../integration-services/change-data-capture/apply-the-changes-to-the-destination.md).  
  
  
