---
title: 'Assistant Ajouter un réplica : Page Entrer les mots de passe pour les groupes de disponibilité'
description: Description des propriétés disponibles dans la page « Entrer les mots de passe » de l’Assistant « Ajouter un réplica » dans SQL Server Management Studio.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: end-user-help
f1_keywords:
- sql13.swb.addreplicawizard.enterpasswords.f1
ms.assetid: e69207a0-c5c4-44e4-ae9a-4afbb67251d1
author: cawrites
ms.author: chadam
ms.openlocfilehash: c9d1cfdb0437972ef5f1115c0e9ba6e667038932
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/25/2021
ms.locfileid: "98765541"
---
# <a name="enter-passwords-page-add-replica-wizard-for-always-on-availability-groups"></a>Page Entrer les mots de passe (Assistant Ajouter un réplica) pour les groupes de disponibilités Always On
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Cette rubrique d’aide décrit les options de la page **Entrer les mots de passe** . Cette rubrique s’applique à [!INCLUDE[ssAoAddRepWiz](../../../includes/ssaoaddrepwiz-md.md)] de [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)].  
  
 Si les réplicas que vous avez sélectionnés sur la page **Spécifier les réplicas** contiennent des bases de données comportant une clé principale de base de données, la page Entrer les mots de passe s’affiche.  
  
## <a name="enter-passwords-options"></a>Options de la page Entrer les mots de passe  
 La grille **Bases de données utilisateur sur cette instance de SQL Server** répertorie chaque base de données utilisateur locale. Les colonnes sont les suivantes :  
  
 **Nom**  
 Affiche le nom d'une base de données utilisateur locale.  
  
 **Taille**  
 Affiche la taille de la base de données, si cette information est connue de l'Assistant.  
  
 **État**  
 Indique **Mot de passe requis** pour les bases de données comportant une clé principale de base de données. Après avoir entré les mots de passe correspondant aux clés principales de base de données dans la colonne **Mots de passe** , cliquez sur **Actualiser**. Si les mots de passe entrés sont corrects, la colonne **État** indique **Mot de passe entré**.  
  
 Si la base de données n’a pas de clé principale de base de données, la colonne **État** indique **Mot de passe non nécessaire**.  
  
 **Mot de passe**  
 Si la colonne **État** indique **Mot de passe requis**, entrez le mot de passe correspondant à la clé principale de base de données.  
  
 **Actualiser**  
 Cliquez pour actualiser la grille. Cette opération est utile après la saisie des mots de passe requis.  
  
## <a name="related-tasks"></a>Tâches associées  
  
-   [Utiliser l’Assistant Ajouter un réplica au groupe de disponibilité &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-add-replica-to-availability-group-wizard-sql-server-management-studio.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Conditions préalables requises, restrictions et recommandations pour les groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)  
  
  
