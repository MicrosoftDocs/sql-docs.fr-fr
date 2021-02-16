---
title: 'Assistant Groupe de disponibilité : Page Validation'
description: Cette rubrique décrit les options disponibles dans la page Validation de l’Assistant Groupe de disponibilité Always On.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
f1_keywords:
- sql13.swb.addreplicawizard.validation.f1
- sql13.swb.adddatabasewizard.validation.f1
- sql13.swb.newagwizard.validation.f1
helpviewer_keywords:
- ', listeners'
ms.assetid: c8971556-240c-491a-bc86-9cc72f71a3dd
author: cawrites
ms.author: chadam
ms.openlocfilehash: b4d05d10c54806b7ffeec607ffc1b50e27bfe5a8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100355092"
---
# <a name="validation-page-always-on-availability-group-wizards"></a>Page Validation (Assistants Groupe de disponibilité Always On)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
    
  Cette rubrique d’aide décrit les options de la page **Validation** . Cette rubrique s'applique à l' [!INCLUDE[ssAoNewAgWiz](../../../includes/ssaonewagwiz-md.md)], à l' [!INCLUDE[ssAoAddRepWiz](../../../includes/ssaoaddrepwiz-md.md)]et à l' [!INCLUDE[ssAoAddDbWiz](../../../includes/ssaoadddbwiz-md.md)] de [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)]. Utilisez cette page pour confirmer que votre environnement prend en charge tous les choix de configuration que vous avez apportés dans les pages précédentes de l'Assistant.  
  
##  <a name="validation-page-options"></a><a name="PageOptions"></a> Options de la page Validation  
 **Résultats de la validation du groupe de disponibilité.**  
 Cette grille affiche les résultats de chaque étape de validation finalisée. Les colonnes de la grille sont les suivantes :  
  
 **Nom**  
 Affiche une expression qui décrit une étape spécifique.  
  
 **Résultat**  
 Affiche l'un des textes de lien hypertexte suivants. Pour plus d'informations sur le résultat d'une étape de validation donnée, cliquez sur le lien hypertexte.  
  
|Résultats|Description|  
|------------|-----------------|  
|**Error**|Indique que l'étape de validation a échoué. Cliquez sur le lien pour afficher le message d'erreur.|  
|**Ignoré**|Indique que l'étape de validation a été ignorée parce qu'elle n'était pas requise par vos sélections. Cliquez sur le lien pour afficher la raison pour laquelle une étape a été ignorée.|  
|**Success**|Indique que l'étape de validation s'est terminée avec succès|  
|**Avertissement**|Indique un problème potentiel avec la configuration du groupe de disponibilité.  Cliquez sur le lien pour afficher le message d'avertissement.|  
  
 **Réexécuter la validation**  
 Cliquez pour répéter les étapes de validation si vous apportez une modification à l'extérieur de l'Assistant en réponse à une erreur de validation.  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Tâches associées  
  
-   [Utiliser la boîte de dialogue Nouveau groupe de disponibilité &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-new-availability-group-dialog-box-sql-server-management-studio.md)  
  
-   [Utiliser l’Assistant Ajouter un réplica au groupe de disponibilité &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-add-replica-to-availability-group-wizard-sql-server-management-studio.md)  
  
-   [Utiliser l’Assistant Ajouter une base de données au groupe de disponibilité &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/availability-group-add-database-to-group-wizard.md)  
  
  
## <a name="see-also"></a>Voir aussi  
 [Vue d’ensemble des groupes de disponibilité Always On (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)  
  
  
