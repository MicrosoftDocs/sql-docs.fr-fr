---
description: Boîte de dialogue Importer des stratégies
title: Importer des stratégies, boîte de dialogue | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
f1_keywords:
- sql13.swb.dmf.importpolicy.f1
ms.assetid: 78ab5f6e-2f13-4788-937e-8892ef4e2345
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 8cd6f4223d3cf23d9ccd98f5b8e4f787872132f3
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88423693"
---
# <a name="import-policies-dialog-box"></a>Boîte de dialogue Importer des stratégies
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Utilisez cette boîte de dialogue pour importer une ou plusieurs stratégies (et leur condition référencée) enregistrées en tant que fichiers XML dans l'instance [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] active.  
  
## <a name="options"></a>Options  
 **Fichiers à importer**  
 Pour importer une stratégie à partir d’un fichier XML, tapez le nom et le chemin du fichier ou utilisez le bouton d’exploration (**...**).  
  
 **Remplacer les doublons avec les éléments importés**  
 Sélectionnez cette option pour remplacer toute stratégie ou condition existante du même nom si elle existe déjà sur cette instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)] . Une condition avec une stratégie dépendante ne peut pas être remplacée, à moins que la stratégie dépendante ne soit également remplacée. Si cette option n'est pas sélectionnée, une condition existante qui utilise la même expression de condition ne provoquera pas d'erreur.  
  
 **État de la stratégie**  
 Sélectionnez l'état que vous souhaitez attribuer à la stratégie importée :  
  
-   **Conserver l'état de la stratégie lors de l'importation**  
  
-   **Activer toutes les stratégies lors de l'importation**  
  
-   **Désactiver toutes les stratégies lors de l'importation**  
  
## <a name="see-also"></a>Voir aussi  
 [Administrer des serveurs à l'aide de la Gestion basée sur des stratégies](../../relational-databases/policy-based-management/administer-servers-by-using-policy-based-management.md)   
 [Importer une stratégie de gestion basée sur des stratégies](../../relational-databases/policy-based-management/import-a-policy-based-management-policy.md)   
 [Exporter une stratégie de gestion basée sur des stratégies](../../relational-databases/policy-based-management/export-a-policy-based-management-policy.md)  
  
  
