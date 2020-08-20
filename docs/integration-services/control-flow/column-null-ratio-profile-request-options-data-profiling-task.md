---
description: Options Demande de profil de ratio de colonne Null (tâche de profilage des données)
title: Options Demande de profil de ratio de colonne Null (tâche de profilage des données) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- Data Profiling Task Editor
ms.assetid: 157ef8e4-fd23-4f81-8194-eebf74e9fd86
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 7f25cb279d26355461f622587aa56f9e0ef4ac1e
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88496015"
---
# <a name="column-null-ratio-profile-request-options-data-profiling-task"></a>Options Demande de profil de ratio de colonne Null (tâche de profilage des données)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Utilisez le volet **Propriétés de la demande** de la page **Demandes de profil** pour définir les options de la **Demande de profil de ratio de colonne Null** sélectionnée dans le volet Demandes. Un profil de ratio de colonne Null signale le pourcentage de valeurs Null dans la colonne sélectionnée. Ce profil peut vous aider à identifier des problèmes dans vos données, tels qu'un ratio élevé inattendu de valeurs Null dans une colonne. Par exemple, un profil de ratio de colonne Null peut profiler une colonne Code postal et découvrir un pourcentage beaucoup trop élevé de codes postaux manquants.  
  
> [!NOTE]  
>  Les options décrites dans cette rubrique apparaissent sur la page **Demandes de profil** de l' **Éditeur de tâche de profilage de données**. Pour plus d’informations sur cette page de l’éditeur, consultez [Éditeur de tâche de profilage de données &#40;page Demandes de profil&#41;](../../integration-services/control-flow/data-profiling-task-editor-profile-requests-page.md).  
  
 Pour plus d’informations sur l’utilisation de la tâche de profilage des données, consultez [Configuration de la tâche de profilage des données](../../integration-services/control-flow/setup-of-the-data-profiling-task.md). Pour plus d’informations sur l’utilisation de la visionneuse du profil des données pour analyser le résultat de la tâche de profilage des données, consultez [Visionneuse du profil des données](../../integration-services/control-flow/data-profile-viewer.md).  
  
## <a name="request-properties-options"></a>Options Propriétés de la demande  
 Pour une **Demande de profil de ratio de colonne Null**, le volet **Propriétés de la demande** affiche les groupes d'options suivants :  
  
-   **Données**, qui incluent les options **TableOrView** et **Column**  
  
-   **Général**  
  
### <a name="data-options"></a>Options de données  
 **ConnectionManager**  
 Sélectionnez le gestionnaire de connexions [!INCLUDE[vstecado](../../includes/vstecado-md.md)] existant qui utilise le fournisseur de données .NET pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (SqlClient) pour se connecter à la base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui contient la table ou la vue à profiler.  
  
 **TableOrView**  
 Sélectionnez la table ou la vue existante qui contient la colonne à profiler.  
  
 Pour plus d'informations, consultez la section « Options TableorView » dans cette rubrique.  
  
 **Colonne**  
 Sélectionnez la colonne existante à profiler. Sélectionnez **(\*)** pour profiler toutes les colonnes.  
  
 Pour plus d'informations, consultez la section « Options de colonne » dans cette rubrique.  
  
#### <a name="tableorview-options"></a>Options TableOrView  
 **Schéma**  
 Spécifie le schéma auquel la table sélectionnée appartient. Cette option est en lecture seule.  
  
 **Table**  
 Affiche le nom de la table sélectionnée. Cette option est en lecture seule.  
  
#### <a name="column-options"></a>Options de colonne  
 **IsWildCard**  
 Indique si le caractère générique **(\*)** a été sélectionné. Cette option a la valeur **True** si vous avez sélectionné **(\*)** pour générer le profil de toutes les colonnes. Sa valeur est **False** si vous avez sélectionné une colonne spécifique dont le profil doit être généré. Cette option est en lecture seule.  
  
 **ColumnName**  
 Affiche le nom de la colonne sélectionnée. Cette option est vide si vous avez sélectionné **(\*)** pour générer le profil de toutes les colonnes. Cette option est en lecture seule.  
  
 **StringCompareOptions**  
 Cette option ne s'applique pas au Profil de ratio de colonne Null.  
  
### <a name="general-options"></a>Options générales  
 **RequestID**  
 Tapez un nom descriptif pour identifier cette demande de profil. En règle générale, il n'est pas nécessaire de modifier la valeur générée automatiquement.  
  
## <a name="see-also"></a>Voir aussi  
 [Éditeur de tâche de profilage de données &#40;page Général&#41;](../../integration-services/control-flow/data-profiling-task-editor-general-page.md)   
 [Formulaire de profil rapide de table simple &#40;tâche de profilage des données&#41;](../../integration-services/control-flow/single-table-quick-profile-form-data-profiling-task.md)  
  
  
