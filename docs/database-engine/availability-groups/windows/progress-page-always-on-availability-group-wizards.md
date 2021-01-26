---
title: Page Progression (Assistants de groupe de disponibilité Always On)
description: Description des options disponibles dans la « page Progression » de « l’Assistant Groupe de disponibilité Always On » dans SQL Server Management Studio.
ms.custom: seodec18
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
f1_keywords:
- sql13.swb.failoverwizard.progress.f1
- sql13.swb.adddatabasewizard.progress.f1
- sql13.swb.addreplicawizard.progress.f1
- sql13.swb.newagwizard.progress.f1
ms.assetid: bd3b0306-8384-4120-a1c9-03825f0ae26a
author: cawrites
ms.author: chadam
ms.openlocfilehash: a331651b92e4781a51de8e03dcfec55797d8723f
ms.sourcegitcommit: 2f3f5920e0b7a84135c6553db6388faf8e0abe67
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/26/2021
ms.locfileid: "98783480"
---
# <a name="progress-page-always-on-availability-group-wizards"></a>Page Progression (Assistants de groupe de disponibilité Always On)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  Utilisez cette boîte de dialogue pour afficher la progression d'un Assistant [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] que vous exécutez dans [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)]. La barre de progression indique la progression relative des étapes que l'Assistant effectue.  
  
## <a name="ui-element-list"></a>Liste d’éléments UI  
 **En savoir plus**  
 Cliquez sur la flèche bas pour afficher une grille de progression qui répertorie toutes les étapes effectuées, dans l'ordre, suivies de l'opération en cours actuelle. Cette grille comporte les colonnes suivantes :  
  
 **Nom**  
 Affiche une expression qui décrit une étape spécifique.  
  
 **État**  
 Indique le résultat des étapes terminées et le pourcentage d'exécution de l'étape active, comme suit :  
  
|Résultats|Description|  
|------------|-----------------|  
|**Error**|Indique que l'opération pour cette étape a rencontré une erreur. Cliquez sur le lien pour afficher une boîte de dialogue de message qui décrit l'erreur.|  
|**En cours (** *pourcentage-effectué* **)**|Indique que l'opération a maintenant lieu et estime le pourcentage de réalisation de cette étape.|  
|**Success**|Indique que l'opération pour cette étape s'est terminée avec succès.|  
  
 **Moins de détails**  
 Cliquez pour masquer la grille de progression.  
  
 **Annuler**  
 Cliquez pour ignorer les opérations restantes, puis quittez l'Assistant.  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Tâches associées  
  
-   [Utiliser la boîte de dialogue Nouveau groupe de disponibilité &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-new-availability-group-dialog-box-sql-server-management-studio.md)  
  
-   [Utiliser l’Assistant Ajouter un réplica au groupe de disponibilité &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-add-replica-to-availability-group-wizard-sql-server-management-studio.md)  
  
-   [Utiliser l’Assistant Ajouter une base de données au groupe de disponibilité &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/availability-group-add-database-to-group-wizard.md)  
  
-   [Utiliser l’Assistant Basculer le groupe de disponibilité &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-fail-over-availability-group-wizard-sql-server-management-studio.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Vue d’ensemble des groupes de disponibilité Always On (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)  
  
  
