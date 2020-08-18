---
description: Groupes d'attributs (Master Data Services)
title: Groupes d'attributs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- attribute groups [Master Data Services]
- attribute groups [Master Data Services], about attribute groups
ms.assetid: 648b3d0b-e15a-45f9-8292-3a54a072e62c
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: e6ef092efb1133d5067f9f3959765017f695acc5
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88413435"
---
# <a name="attribute-groups-master-data-services"></a>Groupes d'attributs (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Dans [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)], les groupes d’attributs permettent d’organiser les attributs dans une entité. Lorsqu'une entité comporte un grand nombre d'attributs, les groupes d'attributs améliorent la manière dont elle est affichée dans l'application Web [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] .  
  
## <a name="how-attribute-groups-change-the-display"></a>Comment les groupes d'attributs modifient l'affichage  
 Les groupes d’attributs sont affichés sous forme d’onglets au-dessus de la grille dans le domaine fonctionnel **Explorateur[!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] de **.  
  
 Lorsqu’une entité a un grand nombre d’attributs et vous la consultez dans une grille dans l’ **Explorateur**, vous devez faire défiler le curseur à droite pour consulter tous les attributs. Pour éviter ce défilement, vous pouvez créer des groupes d'attributs.  
  
-   Les groupes d'attributs incluent toujours les attributs Name et Code.  
  
-   Chaque attribut d'une entité peut appartenir à un ou plusieurs groupes d'attributs.  
  
-   Tous les attributs sont inclus automatiquement sous l’onglet **Tous les attributs** dans **l’Explorateur**.  
  
-   Il n’existe aucun moyen de masquer l’onglet **Tous les attributs** .  
  
 Les groupes d’attributs sont administrés dans la zone fonctionnelle **Administration de système** de [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)].  
  
## <a name="show-or-hide-attribute-groups"></a>Afficher ou masquer les groupes d'attributs  
 Lorsque vous créez un groupe d'attributs, il est automatiquement masqué pour tous les utilisateurs, à l'exception de son créateur. Pour plus d’informations sur le fait de rendre visible le groupe, consultez [Rendre un groupe d’attributs visible pour les utilisateurs &#40;Master Data Services&#41;](../master-data-services/make-an-attribute-group-visible-to-users-master-data-services.md).  
  
 Si vous souhaitez masquer un attribut spécifique dans un groupe, vous pouvez lui affecter une autorisation **Refuser** . Pour plus d’informations, consultez [Autorisations de feuille &#40;Master Data Services&#41;](../master-data-services/leaf-permissions-master-data-services.md).  
  
## <a name="related-tasks"></a>Tâches associées  
  
|Description de la tâche|Rubrique|  
|----------------------|-----------|  
|Créer un groupe d'attributs et y ajouter des attributs.|[Créer un groupe d’attributs &#40;Master Data Services&#41;](../master-data-services/create-an-attribute-group-master-data-services.md)|  
|Rendre un groupe d'attributs visible pour les utilisateurs.|[Rendre un groupe d’attributs visible pour les utilisateurs &#40;Master Data Services&#41;](../master-data-services/make-an-attribute-group-visible-to-users-master-data-services.md)|  
|Modifier le nom d'un groupe d'attributs existant.|[Modifier le nom d’un groupe d’attributs &#40;Master Data Services&#41;](../master-data-services/change-an-attribute-group-name-master-data-services.md)|  
|Supprimer un groupe d'attributs existant.|[Supprimer un groupe d’attributs &#40;Master Data Services&#41;](../master-data-services/delete-an-attribute-group-master-data-services.md)|  
  
## <a name="related-content"></a>Contenu associé  
  
-   [Attributs &#40;Master Data Services&#41;](../master-data-services/attributes-master-data-services.md)  
  
  
