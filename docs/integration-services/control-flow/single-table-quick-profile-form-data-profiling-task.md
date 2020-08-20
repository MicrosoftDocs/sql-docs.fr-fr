---
description: Formulaire de profil rapide de table simple (tâche de profilage des données)
title: Formulaire de profil rapide de table simple (tâche de profilage des données) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.dataprofilingtask.quickprofile.f1
helpviewer_keywords:
- Data Profiling Task Editor
ms.assetid: d2fac9ce-730e-474e-961a-69406b633778
author: chugugrace
ms.author: chugu
ms.openlocfilehash: b974db6df0f18a8063201f9e83ec905b87267301
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88495957"
---
# <a name="single-table-quick-profile-form-data-profiling-task"></a>Formulaire de profil rapide de table simple (tâche de profilage des données)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Utilisez le **Formulaire de profil rapide de table simple** pour configurer rapidement la tâche de profilage des données afin de profiler une table ou une vue unique à l'aide des paramètres par défaut.  
  
 Pour plus d’informations sur l’utilisation de la tâche de profilage des données, consultez [Configuration de la tâche de profilage des données](../../integration-services/control-flow/setup-of-the-data-profiling-task.md). Pour plus d’informations sur l’utilisation de la visionneuse du profil des données pour analyser le résultat de la tâche de profilage des données, consultez [Visionneuse du profil des données](../../integration-services/control-flow/data-profile-viewer.md).  
  
## <a name="options"></a>Options  
 **Connection**  
 Sélectionnez un gestionnaire de connexions [!INCLUDE[vstecado](../../includes/vstecado-md.md)] existant qui utilise le fournisseur de données .NET pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (SqlClient) afin d’établir la connexion à la base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] contenant la table ou la vue à profiler.  
  
 **Table ou vue**  
 Sélectionnez une table ou une vue dans la base de données à laquelle le gestionnaire de connexions sélectionné se connecte.  
  
 **Calcul**  
 Sélectionnez les profils à calculer.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**Profil de ratio de colonne Null**|Calculez un profil de ratio de colonne Null à l'aide des paramètres par défaut pour toutes les colonnes applicables dans la table ou la vue sélectionnée.<br /><br /> Ce profil signale le pourcentage de valeurs NULL dans la colonne sélectionnée. Ce profil peut vous aider à identifier des problèmes dans vos données, tels qu'un ratio élevé inattendu de valeurs Null dans une colonne. Pour plus d’informations sur les paramètres de ce profil, consultez [Options Demande de profil de ratio de colonne Null &#40;tâche de profilage des données&#41;](../../integration-services/control-flow/column-null-ratio-profile-request-options-data-profiling-task.md).|  
|**Profil de statistiques de colonnes**|Calculez un profil de statistiques de colonnes à l'aide des paramètres par défaut pour toutes les colonnes applicables dans la table ou la vue sélectionnée.<br /><br /> Ce profil répertorie des statistiques, telles que l'écart minimal, maximal, moyen et type pour les colonnes numériques, ainsi que l'écart minimal et maximal pour les colonnes **datetime** . Ce profil peut vous aider à identifier des problèmes dans vos données, tels que des dates non valides. Pour plus d’informations sur les paramètres de ce profil, consultez [Options Demande de profil de statistiques de colonnes &#40;tâche de profilage des données&#41;](../../integration-services/control-flow/column-statistics-profile-request-options-data-profiling-task.md).|  
|**Profil de distribution de valeurs de colonne**|Calculez un profil de distribution de valeurs de colonne à l'aide des paramètres par défaut pour toutes les colonnes applicables dans la table ou la vue sélectionnée.<br /><br /> Ce profil permet de préciser toutes les valeurs distinctes dans la colonne sélectionnée, ainsi que le pourcentage de lignes dans la table que représente chaque valeur. Ce profil peut également signaler les valeurs qui représentent beaucoup plus qu'un pourcentage de lignes spécifié dans la table. Il peut vous aider à identifier des problèmes dans vos données, tels qu'un nombre incorrect de valeurs distinctes dans une colonne. Pour plus d’informations sur ce profil, consultez [Options Demande de profil de distribution de valeurs de colonne &#40;tâche de profilage des données&#41;](../../integration-services/control-flow/column-value-distribution-profile-request-options-data-profiling-task.md).|  
|**Profil de distribution de longueurs de colonne**|Calculez un profil de distribution de longueurs de colonne à l'aide des paramètres par défaut pour toutes les colonnes applicables dans la table ou la vue sélectionnée.<br /><br /> Ce profil signale toutes les longueurs distinctes des valeurs de chaîne dans la colonne sélectionnée, ainsi que le pourcentage de lignes dans la table que chaque longueur représente. Ce profil peut vous aider à identifier des problèmes dans vos données, tels que des valeurs non valides. Pour plus d’informations sur les paramètres de ce profil, consultez [Options Demande de profil de distribution de longueurs de colonne &#40;tâche de profilage des données&#41;](../../integration-services/control-flow/column-length-distribution-profile-request-options-data-profiling-task.md).|  
|**Profil de modèle de colonne**|Calculez un profil de modèle de colonne à l'aide des paramètres par défaut pour toutes les colonnes applicables dans la table ou la vue sélectionnée.<br /><br /> Ce profil signale un jeu d'expressions régulières qui couvrent les valeurs dans une colonne de chaîne. Ce profil peut vous aider à identifier des problèmes dans vos données, tels que des chaînes non valides. Il peut également suggérer des expressions régulières susceptibles d'être utilisées à l'avenir pour la validation de nouvelles valeurs. Pour plus d’informations sur les paramètres de ce profil, consultez [Options Demande de profil de modèle de colonne &#40;tâche de profilage des données&#41;](../../integration-services/control-flow/column-pattern-profile-request-options-data-profiling-task.md).|  
|**Profil de clé candidate**|Calculez un profil de clé candidate pour les combinaisons de colonnes qui incluent jusqu'au nombre de colonnes spécifié dans **pour au plus N clés de colonne**.<br /><br /> Ce profil signale si une colonne ou un ensemble de colonnes est adapté en tant que clé pour la table sélectionnée. Ce profil peut également vous aider à identifier des problèmes dans vos données, tels que des valeurs dupliquées dans une colonne clé potentielle. Pour plus d’informations sur les paramètres de ce profil, consultez [Options Demande de profil de clé candidate &#40;tâche de profilage des données&#41;](../../integration-services/control-flow/candidate-key-profile-request-options-data-profiling-task.md).|  
|**pour au plus N clés de colonne**|Sélectionnez le nombre maximal de colonnes à tester dans les combinaisons possibles comme clé de la table ou de la vue. La valeur par défaut est 1. La valeur maximale est 1000. Par exemple, la valeur 3 teste les combinaisons de clé d'une colonne, de deux colonnes et de trois colonnes.|  
|**Profil de dépendance fonctionnelle**|Calculez un profil de dépendance fonctionnelle pour les combinaisons de colonnes déterminantes qui incluent jusqu'au nombre de colonnes spécifié dans **pour au moins N colonnes comme colonnes déterminantes**.<br /><br /> Ce profil indique le degré de dépendance entre les valeurs d'une colonne (colonne dépendante) et celles d'une autre colonne ou d'un ensemble de colonnes (colonne déterminante). Ce profil peut également vous aider à identifier des problèmes dans vos données, tels que des valeurs non valides. Pour plus d’informations sur les paramètres de ce profil, consultez [Options Demande de profil de dépendance fonctionnelle &#40;tâche de profilage des données&#41;](../../integration-services/control-flow/functional-dependency-profile-request-options-data-profiling-task.md).|  
|**pour au moins N colonnes comme colonnes déterminantes**|Sélectionnez le nombre maximal de colonnes à tester dans les combinaisons possibles comme colonnes déterminantes. La valeur par défaut est 1. La valeur maximale est 1000. Par exemple, la valeur 2 teste les combinaisons dans lesquelles les combinaisons d'une colonne ou de deux colonnes sont les colonnes déterminantes d'une autre colonne (dépendante).|  
  
> [!NOTE]  
>   Le type de profil d'inclusion de valeur n'est pas disponible à partir du **formulaire de profil rapide de table simple**.  
  
## <a name="see-also"></a>Voir aussi  
 [Éditeur de tâche de profilage de données &#40;page Général&#41;](../../integration-services/control-flow/data-profiling-task-editor-general-page.md)   
 [Éditeur de tâche de profilage de données &#40;page Demandes de profil&#41;](../../integration-services/control-flow/data-profiling-task-editor-profile-requests-page.md)  
  
  
