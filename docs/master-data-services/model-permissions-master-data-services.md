---
description: Autorisations de modèle (Master Data Services)
title: Autorisations de modèle
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- models [Master Data Services], permissions
- permissions [Master Data Services], models
ms.assetid: 210f440b-2cc1-4c49-94b1-3a97e2af7bc3
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 029a3f624dea71567f34a26cdd7093cc96be36d3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340329"
---
# <a name="model-permissions-master-data-services"></a>Autorisations de modèle (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Les autorisations de modèle s'appliquent à l'ensemble des entités, attributs, groupes d'attributs, hiérarchies dérivées, hiérarchies explicites et collections qui existent dans le modèle. Les autorisations affectées au modèle peuvent être remplacées pour tout objet individuel.  
  
> [!NOTE]  
>  Si l'utilisateur est administrateur de modèle, le modèle est affiché dans toutes les zones fonctionnelles de l'interface utilisateur. Sinon, le modèle est affiché uniquement dans la zone fonctionnelle **Explorateur** . Pour plus d’informations, consultez [administrateurs &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md).  
  
|Autorisation|Description|  
|----------------|-----------------|  
|**Lire**|L’utilisateur peut lire des membres, des attributs, des appartenances hiérarchiques ou des appartenances aux collections.|  
|**Créer**|L’utilisateur peut créer des membres et affecter des valeurs d’attribut lors de la création.|  
|**Mettre à jour**|L’utilisateur peut mettre à jour des membres, des attributs, des appartenances hiérarchiques ou des appartenances aux collections.|  
|**Supprimer**|L’utilisateur peut supprimer des membres.|  
|**Deny**|Tous les accès au modèle sont refusés.|  
|**Administrateur**|Autorisation d’administrateur sur le modèle. L’autorisation d’administrateur est disponible uniquement au niveau du modèle.|  
  
 Vous pouvez aussi combiner les autorisations d’accès pour la lecture, la création, la mise à jour et la suppression. Lorsque les autorisations de création, de mise à jour et de suppression sont attribuées, l’autorisation d’accès en lecture est attribuée automatiquement.  
  
## <a name="see-also"></a>Voir aussi  
 [Affecter des autorisations d’objet de modèle &#40;Master Data Services&#41;](../master-data-services/assign-model-object-permissions-master-data-services.md)   
 [Autorisations d’objet de modèle &#40;Master Data Services&#41;](../master-data-services/model-object-permissions-master-data-services.md)   
 [Autorisations d’entité &#40;Master Data Services&#41;](../master-data-services/entity-permissions-master-data-services.md)   
 [Autorisations de collection &#40;Master Data Services&#41;](../master-data-services/collection-permissions-master-data-services.md)  
  
  
