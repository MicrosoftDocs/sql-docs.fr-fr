---
title: Page Sélectionner les bases de données (Assistants Nouveau groupe de disponibilité et Ajout de base de données)
description: Décrit la page « Sélectionner les bases de données » des Assistants Nouveau groupe de disponibilité et Ajout de base de données disponibles dans l’interface graphique utilisateur de SQL Server Management Studio.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
f1_keywords:
- sql13.swb.adddatabasewizard.selectdatabases.f1
- sql13.swb.newagwizard.selectdatabases.f1
ms.assetid: 929c5e15-d087-438d-b1f2-aa97c5f8bff8
author: cawrites
ms.author: chadam
ms.openlocfilehash: a25b2d75c6ee143f005677a250d59b4c0df32a22
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352651"
---
# <a name="select-databases-page-new-availability-group-wizard-and-add-database-wizard"></a>Page Sélectionner les bases de données (Assistant Nouveau groupe de disponibilité et Assistant Ajouter une base de données)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Cette rubrique d'aide décrit les options de la page **Spécifier les bases de données** . Cette rubrique s'applique à l' [!INCLUDE[ssAoNewAgWiz](../../../includes/ssaonewagwiz-md.md)] et à l' [!INCLUDE[ssAoAddDbWiz](../../../includes/ssaoadddbwiz-md.md)] de [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)].  
  
##  <a name="select-databases-options"></a><a name="PageOptions"></a> Options Sélectionner les bases de données  
 La grille **Bases de données utilisateur sur cette instance de SQL Server** répertorie chaque base de données utilisateur locale. Les colonnes sont les suivantes :  
  
 **Nom**  
 Affiche le nom d'une base de données utilisateur locale.  

 **Taille**  
 Affiche la taille de la base de données, si cette information est connue de l'Assistant.  
  
 **État**  
 Affiche un lien hypertexte dont le texte indique si une base de données particulière réunit les conditions préalables requises en vue de l'ajout à un groupe de disponibilité. Si l'état est «**Répond aux conditions requises**», vous pouvez ajouter la base de données au groupe de disponibilité. Si une base de données ne réunit pas l'ensemble des conditions préalables requises, le lien hypertexte **État** fournit une brève explication de la raison pour laquelle la base de données n'est pas éligible. Pour plus d'informations, cliquez sur le lien hypertexte.  
  
 Vous pouvez laisser l'Assistant sur la page **Sélectionner une base de données** lorsque vous agissez sur une base de données afin qu'elle remplisse une condition préalable. Lorsque vous revenez à la page **Sélectionner les bases de données** , cliquez sur **Actualiser** pour mettre à jour la grille.  
  
 **Mot de passe**  
 Si la base de données contient une clé principale de base de données, entrez le mot de passe de la clé principale de base de données.  
  
 **Actualiser**  
 Cliquez pour actualiser la grille. Cette action est utile lorsque vous agissez sur une base de données afin qu'elle remplisse une condition préalable.  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Tâches associées  
  
-   [Utiliser la boîte de dialogue Nouveau groupe de disponibilité &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-new-availability-group-dialog-box-sql-server-management-studio.md)  
  
-   [Utiliser l’Assistant Ajouter une base de données au groupe de disponibilité &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/availability-group-add-database-to-group-wizard.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Vue d’ensemble des groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Conditions préalables requises, restrictions et recommandations pour les groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)  
  
  
