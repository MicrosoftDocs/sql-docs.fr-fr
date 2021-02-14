---
description: Autorisations d'objet de modèle (Master Data Services)
title: Autorisations d'objet de modèle
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- permissions [Master Data Services], model objects
- models [Master Data Services], object permissions
ms.assetid: fab6335b-4cae-47de-ae7c-6c4743e0680f
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: fc4372748ee9556293eee5c074afc2f404134849
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340323"
---
# <a name="model-object-permissions-master-data-services"></a>Autorisations d'objet de modèle (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Les autorisations d'objet modèle sont obligatoires. Elles déterminent les attributs auxquels un utilisateur peut accéder dans la zone fonctionnelle **Explorateur** de l'interface utilisateur.  
  
 Par exemple, si vous affectez une autorisation **Mettre à jour** sur l'entité Product, l'utilisateur peut mettre à jour tous les attributs de l'entité Product. Si vous affectez une autorisation **Mettre à jour** à un seul attribut, l'utilisateur peut mettre à jour cet attribut uniquement.  
  
 Pour déterminer la sécurité affectée sur chaque valeur d'attribut individuelle, les autorisations d'objet modèle sont associées aux autorisations des membres de la hiérarchie, qui déterminent les membres auxquels un utilisateur peut accéder.  
  
 Pour octroyer à un utilisateur l’accès à une zone fonctionnelle autre que l’ **Explorateur**, l’utilisateur doit être administrateur de modèle, ce qui implique également l’attribution d’autorisations d’administrateur concernant l’objet modèle. Pour plus d’informations, consultez [administrateurs &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md).  
  
 Les autorisations d’objet de modèle sont affectées dans l' [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] interface utilisateur, dans la zone fonctionnelle **autorisations d’accès** sous l’onglet **modèles** . Sous cet onglet, le modèle est représenté sous la forme d’une arborescence. Lorsque vous affectez une autorisation à un objet dans l'arborescence, tous les objets suivants héritent de cette autorisation. Vous pouvez remplacer cet héritage en affectant l'autorisation à des objets individuels.  
  
 Vous pouvez attribuer une combinaison d’autorisations Read, Create, Update et Delete ou Deny aux objets de modèle. Si vous n'affectez aucune autorisation sous l'onglet **Modèles** , l'utilisateur ne peut pas afficher les modèles ou données dans [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)].  
  
## <a name="best-practice"></a>Bonne pratique  
 En général vous devez attribuer l’autorisation **ALL** à l’objet de modèle, puis attribuer explicitement l’autorisation aux objets en dessous.  
  
## <a name="external-resources"></a>Ressources externes  
 Billet de blog, [Améliorations de sécurité](/archive/blogs/e7/improvements-to-autoplay), sur msdn.com.  
  
## <a name="see-also"></a>Voir aussi  
 [Affecter des autorisations d’objet de modèle &#40;Master Data Services&#41;](../master-data-services/assign-model-object-permissions-master-data-services.md)   
 [Autorisations de modèle &#40;Master Data Services&#41;](../master-data-services/model-permissions-master-data-services.md)   
 [Autorisations de zone fonctionnelle &#40;Master Data Services&#41;](../master-data-services/functional-area-permissions-master-data-services.md)   
 [Autorisations des membres de la hiérarchie &#40;Master Data Services&#41;](../master-data-services/hierarchy-member-permissions-master-data-services.md)   
 [Mode de détermination des autorisations &#40;Master Data Services&#41;](../master-data-services/how-permissions-are-determined-master-data-services.md)  
  
