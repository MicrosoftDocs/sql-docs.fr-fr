---
description: Sécuriser les éléments de source de données partagée
title: Sécuriser les éléments de source de données partagée | Microsoft Docs
ms.date: 03/01/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- shared data sources [Reporting Services]
- data sources [Reporting Services], shared
- security [Reporting Services], data sources
ms.assetid: 7299e498-0a1a-4821-a22a-5199bb773ce0
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: cf25b5dd7eac661f5ee77e2328a322601b0d3c8d
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88472641"
---
# <a name="secure-shared-data-source-items"></a>Sécuriser les éléments de source de données partagée
  Vous pouvez définir la sécurité sur un élément de source de données partagée afin d'en activer ou d'en désactiver l'accès.  
  
 Un utilisateur qui dispose d’un accès minimal à une source de données partagée (par exemple, l’accès accordé par le rôle **Explorateur** ) peut visualiser la liste des rapports qui utilisent l’élément, à condition qu’il soit autorisé à afficher les rapports eux-mêmes.  
  
 Un utilisateur qui dispose d’un accès supplémentaire (tel que celui conféré par le rôle **Gestionnaire de contenu** ) peut consulter et définir les propriétés de la source de données partagée.  
  
 Pour définir la sécurité, vous créez une attribution de rôle qui spécifie quel compte d'utilisateur ou de groupe dispose de l'accès à la source de données partagée. Les utilisateurs qui ont accès à un élément de source de données partagée peuvent changer son nom, sa description, sa chaîne de connexion ou ses informations d'identification.  
  
## <a name="tasks-and-access-to-items"></a>Tâches et accès aux éléments  
 Lorsque vous créez des attributions de rôles, utilisez un rôle qui possède ces tâches pour affecter des autorisations appropriées aux utilisateurs et aux groupes.  
  
|Sélectionnez cette tâche|Pour autoriser les utilisateurs à effectuer les opérations suivantes|  
|----------------------|---------------------------------|  
|Afficher les sources de données|Afficher les éléments de source de données dans l'arborescence des dossiers. Sans cette tâche, l'élément n'est pas visible pour les utilisateurs et ils peuvent ignorer que la source de données est disponible.|  
|Gérer les sources de données|Afficher les propriétés qui spécifient le nom, la description et les informations de connexion. Cette tâche est également utilisée pour afficher un élément de source de données partagée dans la hiérarchie de dossiers. Si vous choisissez cette tâche, vous pouvez omettre la tâche « Afficher les sources de données ».|  
|Définir la sécurité au niveau des éléments|Créer et modifier des attributions de rôles qui contrôlent l'accès à la source de données partagée. Cette tâche doit être utilisée avec la tâche « Afficher les sources de données » ou avec la tâche « Gérer les sources de données ». Si tel n'est pas le cas, elle est sans effet puisque l'utilisateur ne peut pas sélectionner l'élément.|  
  
## <a name="see-also"></a>Voir aussi  
 [Gérer des sources de données de rapports](../../reporting-services/report-data/manage-report-data-sources.md)   
 [Dossiers sécurisés](../../reporting-services/security/secure-folders.md)   
 [Sécuriser des rapports et des ressources](../../reporting-services/security/secure-reports-and-resources.md)   
 [Octroi d'autorisations sur un serveur de rapports en mode natif](../../reporting-services/security/granting-permissions-on-a-native-mode-report-server.md)   
 [Stocker des informations d’identification dans une source de données Reporting Services](../../reporting-services/report-data/store-credentials-in-a-reporting-services-data-source.md)  
  
  
